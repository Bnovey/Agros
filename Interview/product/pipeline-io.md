# Pipeline — Inputs and Outputs at Each Stage

The full discovery loop, stage by stage. Every stage has a defined input format, a defined output format, and a clear artifact written to disk before the next stage begins.

---

## Stage 0: Target Preparation

**Input:**
- Raw PDB file of target protein (e.g., Botrytis cinerea β-glucan synthase, Lepidopteran nAChR) — from RCSB PDB or homology model
- Literature: published mutagenesis data identifying known binding residues (used for IG validation later)

**Processing:**
- Clean PDB (remove water, ligands, alternate conformations)
- Define binding site coordinates (center + radius, or residue list)
- Extract FASTA sequence of the target

**Output:**
- `target.pdb` — cleaned structure
- `target.fasta` — target sequence
- `binding_site.json` — binding pocket definition (residue indices, coordinates)
- `target_manifest.md` — run manifest: PDB source, hash, mutagenesis literature used, timestamp

**Why this matters:** Everything downstream is conditioned on this target representation. A wrong binding site definition produces plausible-looking but useless candidates. The manifest is the reproducibility record.

---

## Stage 1: Candidate Generation (BoltzGen)

**Input:**
- `target.pdb` — cleaned target structure
- `binding_site.json` — binding pocket definition
- Generation parameters: number of candidates (N), temperature, sequence length range, any amino acid constraints (e.g., exclude cysteines for stability)
- Random seed (pinned for reproducibility)

**Processing:**
- BoltzGen runs flow matching in structure space, conditioned on the target binding pocket
- Generates N peptide sequences with predicted backbone conformations targeting the defined pocket

**Output:**
- `candidates.fasta` — N peptide sequences (amino acid strings, 10–30 residues)
- `candidates_structures/` — N predicted backbone structures (one `.pdb` per candidate)
- `generation_manifest.json` — model hash, seed, generation parameters, timestamp

**Typical N:** 100–500 candidates per run; we score all, synthesize the top 10–20.

---

## Stage 2: Binding Affinity Scoring (Boltz-2)

**Input:**
- `candidates.fasta` — all N generated peptide sequences
- `target.pdb` — cleaned target structure

**Processing:**
- For each candidate: Boltz-2 predicts the full peptide-protein complex structure and outputs a binding affinity score
- Also produces pLDDT (per-residue confidence) and PAE (predicted aligned error) as quality metrics

**Output (per candidate):**
- `complex_{id}.cif` — predicted peptide-protein complex structure (mmCIF format)
- `scores.json` — binding affinity scores for all N candidates (ΔG estimate or relative affinity rank), pLDDT, PAE
- Candidates sorted descending by binding affinity score

**Key number:** Boltz-2 inference on an A100 — approximately 30–60 seconds per candidate. 100 candidates ≈ 1–2 hours, costing ~$5–20 total on Modal.

---

## Stage 3: Interpretability — Integrated Gradients (IG)

**Input:**
- Top K candidates by Boltz-2 score (e.g., top 50 from Stage 2)
- `complex_{id}.cif` — complex structures for each candidate
- Boltz-2 model weights (frozen)

**Processing:**
- For each candidate: run IG on the Boltz-2 forward pass, attributing the binding affinity prediction back to each residue in the peptide
- IG requires modifying Boltz-2's internal forward methods (not just hooks) to expose the gradient path to the binding affinity output
- Run IG across all K candidates and aggregate: which residue positions consistently receive high attribution? (IG convergence analysis)

**Output (per candidate):**
- `ig_scores_{id}.npy` — per-residue attribution vector (length = peptide length), float32
- `ig_heatmap_{id}.png` — visualization of attribution scores along the sequence

**Output (aggregate, for convergence analysis):**
- `ig_convergence.npy` — attribution scores stacked across all K candidates (shape: K × max_length)
- `ig_convergence_heatmap.png` — shows which positions consistently drive binding across candidates — the key visualization for the demo and the paper
- `ig_manifest.json` — model hash, residue indexing convention, tensor shapes, timestamp (off-by-one indexing errors here invalidate the paper)

**What the convergence heatmap shows:** If the model has learned real biology, attribution scores should concentrate at the same 3–5 residue positions across all candidates — corresponding to the pharmacophore that matches the binding pocket. This is the validation signal.

**Validation step (before any synthesis):** Compare IG-predicted binding residues against known mutagenesis data from Stage 0 literature. If the positions don't match, the model is wrong about mechanism even if it produces high binding scores. This check happens here — not after wet lab.

---

## Stage 4: Sequence Redesign (ProteinMPNN) — Optional

**Input:**
- Backbone structures of top candidates (from Stage 1 BoltzGen output or Boltz-2 complex)
- Residue positions to fix: IG-identified high-attribution positions are locked (don't change the residues that matter)
- Positions to redesign: low-attribution positions are free to vary for stability/solubility

**Processing:**
- ProteinMPNN generates alternative sequences that thread onto the same backbone while optimizing for stability
- Only redesigns positions IG identified as unimportant to binding — preserves the pharmacophore

**Output:**
- `redesigned_candidates.fasta` — alternative sequences for each backbone, typically 5–20 sequences per backbone
- These are fed back into Stage 2 (Boltz-2 scoring) for a second evaluation pass

**When we use this:** If top candidates from Stage 2/3 have poor solubility or stability predictions but high binding scores — redesign the scaffold while preserving the binding residues.

---

## Stage 5: Candidate Filters

Three filters run in sequence on the shortlisted candidates (post-IG, typically top 20–50).

### 5a: Solubility Filter (NetSolP)

**Input:** `candidates.fasta` — peptide sequences for shortlisted candidates

**Output:** `solubility_scores.json` — probability of solubility (0–1) for each candidate

**Threshold:** Candidates below 0.5 solubility probability are flagged; below 0.3 are dropped. Insoluble peptides cannot be assayed.

### 5b: Structural Validation (RDKit)

**Input:** Peptide SMILES strings (generated from sequences) or sequence directly

**Output:** `rdkit_flags.json` — molecular weight, charge at pH 7, SMILES validity, structural alert flags (reactive groups, known liabilities)

**Threshold:** Candidates with structural alerts are flagged for manual review before synthesis.

### 5c: Stability Simulation (OpenMM) — Top candidates only

**Input:**
- `complex_{id}.cif` — peptide-protein complex for top 5–10 candidates
- Force field parameters (AMBER ff14SB for protein, GAFF for peptide)

**Processing:** Short MD simulation (10–50 ns) in explicit solvent. Checks if the complex stays together or falls apart.

**Output:**
- `md_trajectory_{id}.dcd` — trajectory file
- `stability_metrics_{id}.json` — RMSD over time, binding pocket RMSD, peptide backbone RMSD
- Candidates with RMSD > threshold are flagged as unstable

**When we run OpenMM:** Only on the final 5–10 candidates before synthesis — it's the most expensive filter ($10–50 per simulation). Never on the full candidate set.

---

## Stage 6: Candidate Ranking + Synthesis Selection

**Input:**
- `scores.json` — Boltz-2 binding affinity scores
- `ig_scores_{id}.npy` — IG attribution scores (mechanism confidence)
- `solubility_scores.json` — NetSolP outputs
- `rdkit_flags.json` — structural flags
- `stability_metrics_{id}.json` — OpenMM RMSD

**Processing:** Composite ranking across all signals. Binding affinity is the primary signal; IG convergence confidence is secondary; filters are go/no-go.

**Output:**
- `synthesis_list.csv` — final ranked list of 10–20 candidates for synthesis: sequence, Boltz-2 score, IG confidence, solubility score, synthesis cost estimate, CRO order details
- `round_{N}_manifest.md` — full run manifest: target, model hash, seed, number of candidates scored, filters applied, final selection rationale, timestamp

**This manifest is committed to GitHub before synthesis begins.** It timestamps our IP claim on each candidate sequence.

---

## Stage 7: Wet Lab Validation (Off-Pipeline)

**Input:** Synthesized peptides (physical) — ordered from contract CRO (ProteoGenix, AAPPTEC, or similar) based on `synthesis_list.csv`

**Assay:** In vitro Botrytis cinerea growth inhibition (MIC / EC50 determination)
- Spore germination assay or mycelial growth inhibition assay
- Serial dilution (typically 0.5–128 µg/mL range)
- Read-out: OD600 or colony count at 48–72 hours

**Output:**
- `assay_results_{round}.csv` — peptide ID, MIC (µg/mL), EC50 (µg/mL), % growth inhibition at fixed concentration, any toxicity observations
- `assay_manifest.md` — assay protocol version, organism strain, positive/negative controls used, date, lab/operator

**What counts as a hit:** MIC ≤ 32 µg/mL (active), MIC ≤ 8 µg/mL (potent). Literature benchmark for antifungal peptides.

---

## Stage 8: Feedback Loop (PEFT Fine-tuning of Boltz-2)

**Input:**
- `assay_results_{round}.csv` — experimental MIC/EC50 values for this round's candidates
- `scores.json` — Boltz-2 predicted binding affinities for the same candidates
- Boltz-2 base model weights

**Processing:**
- Pair each candidate's Boltz-2 prediction with its experimental activity
- Fine-tune Boltz-2 using Hugging Face PEFT (LoRA) to minimize prediction error on this target-specific data
- Only adapter weights are updated — base model knowledge is preserved

**Output:**
- `boltz2_lora_{target}_{round}.pt` — LoRA adapter weights (small, ~10–100 MB vs. full model ~1 GB+)
- `finetuning_manifest.json` — training data hash, base model hash, LoRA rank, learning rate, epochs, val loss, timestamp
- Updated model checkpoint used in the next generation round (Stage 1)

**Why this is the moat:** After round N, we have a Boltz-2 model that has been calibrated against N rounds of real Botrytis β-glucan synthase binding data. No competitor has this data. The model gets better at predicting our target specifically — and only we can run round N+1 with the round N-calibrated model.

---

## End-to-End Data Flow Summary

```
Target PDB + mutagenesis literature
        ↓  [Stage 0: Target Prep]
target.pdb + binding_site.json
        ↓  [Stage 1: BoltzGen]
N candidate sequences + backbone structures (FASTA + PDB)
        ↓  [Stage 2: Boltz-2 scoring]
N binding affinity scores + complex structures (JSON + mmCIF)
        ↓  [Stage 3: IG analysis on top K]
Per-residue attribution vectors + convergence heatmap (NPY + PNG)
        ↓  [Stage 4: ProteinMPNN redesign, optional]
Redesigned sequences for low-attribution positions (FASTA)
        ↓  [Stage 5: NetSolP + RDKit + OpenMM filters]
Filtered, flagged candidate list (JSON + CSV)
        ↓  [Stage 6: Composite ranking]
synthesis_list.csv (10–20 candidates) + round manifest (MD)
        ↓  [Stage 7: Wet lab — off pipeline]
assay_results.csv (MIC / EC50 per candidate)
        ↓  [Stage 8: PEFT fine-tuning]
Updated LoRA adapter weights for Boltz-2 on this target
        ↓  [Loop back to Stage 1 with better model]
```

---

## Key Invariants (from CLAUDE.md — do not violate)

- All model outputs must come from real model inference — never synthetic/fake scores
- IG gradients must target binding affinity outputs, not embeddings or confidence scores
- Every stage writes a manifest before proceeding to the next
- Tensor shapes and residue indexing are logged with every IG save — off-by-one bugs here invalidate the paper
- Seeds are pinned at the top of every script (numpy, torch, python random)
- Model checkpoints are pinned by hash, not name
