# Product — How It Works

## Core Loop (explain in 3 steps)

1. **Generate candidates** — BoltzGen proposes peptide sequences optimized for binding to Botrytis β-glucan synthase (antifungal) or Lepidopteran nAChR/RyR (insecticidal/nematicidal). Scored by Boltz-2 for binding affinity.
2. **Understand why** — Integrated Gradients (IG) analysis maps which amino acid residues drive the predicted binding. This gives mechanistic insight competitors don't have: we know the 3–5 positions that matter before we synthesize.
3. **Wet lab validation** — Top candidates synthesized at $2–4/amino acid (ProteoGenix / AAPPTEC), tested in vitro (Botrytis growth inhibition assay, MIC / EC50). Results feed back to improve the model.

**Key point:** The interpretability layer is what makes the loop tight. Failed experiments aren't just failures — they're data about which residues the model got wrong. Each round is faster and more targeted.

---

## Technical Stack

### Models
- **BoltzGen** — peptide generation (MIT, Hannes Stark lab). Generates novel sequences against specified targets.
- **Boltz-2** — structure prediction + binding affinity scoring (MIT, jwohlwend/boltz). Scores each candidate.
- **ESM-2** — protein language model embeddings (Meta, facebookresearch/esm). Used for sequence-level features.
- **ProteinMPNN** — inverse folding for sequence redesign.
- **AutoDock Vina + GNINA** — physics-based docking baselines (used for paper benchmarking / validation).

### Interpretability Layer (key differentiator)
- **Integrated Gradients (IG)** on Boltz-2 — extracts per-residue attribution scores that show which amino acids are driving the binding affinity prediction
- IG convergence analysis: run across 1,000+ candidates, the attribution scores should converge to a consistent binding region — this validates that the model has learned real biology
- Validation approach: compare IG-predicted binding residues against known mutagenesis data from literature for the target

### Compute
- **Modal (serverless GPU, A100/H100)** — all inference runs through Modal apps. Cost: $5–20 per candidate for scoring + IG. Bursty design campaigns are pay-per-use, no standing servers.

### Supporting filters
- **NetSolP** — solubility prediction (rules out insoluble candidates before synthesis)
- **RDKit** — molecular validation
- **OpenMM** — MD simulation for stability checks

---

## Targets

| Target | Disease / Pest | Crop impact | Why we chose it |
|--------|---------------|-------------|----------------|
| **Botrytis cinerea β-glucan synthase** | Gray mold (antifungal) | >$10B/yr crop losses; affects grapes, strawberries, tomatoes, ornamentals | High resistance to all synthetic FRAC classes; large published mutagenesis dataset for IG validation |
| **Lepidopteran nAChR** | Fall armyworm, diamondback moth (insecticidal) | Major row crop and vegetable pests globally | Well-validated target; nicotinic receptors are proven pesticide targets |
| **Lepidopteran RyR** | Same pest group | Same | Ryanodine receptor is the target of modern insecticides (diamides) — peptide alternatives possible |

---

## Differentiation vs. Pure Screening

| | Agros | Screening-only (e.g., Bindwell) |
|--|-------|---------------------|
| Candidates per round | 10–50 (targeted) | Millions (brute force) |
| Insight per experiment | High — residue-level IG attribution | Low — pass/fail per compound |
| Model improvement from failure | Yes — IG tells us what the model got wrong | No — just removed from candidate list |
| Synthesis cost per round | ~$7,000–20,000 | High (millions of compounds) |
| Time to mechanistic understanding | 1–2 rounds | Never |

---

## What We've Built

- [x] BoltzGen + Boltz-2 pipeline running on Modal infrastructure
- [x] IG analysis extracting per-residue attributions from Boltz-2 forward passes
- [x] ESM-2 embeddings integrated
- [x] NetSolP + RDKit filters in pipeline
- [ ] IG convergence analysis on 1,000+ candidates against Botrytis target: [status]
- [ ] Validation vs. known Botrytis mutagenesis data: [status]
- [ ] First peptide candidates synthesized: [status]
- [ ] In vitro Botrytis growth inhibition assays: [status]
- [ ] Demo video: [status]

---

## Open Technical Risks (be ready for these)

**"Does Boltz-2 accurately predict binding for agricultural targets?"**
> Boltz-2 was trained on PDB structures, most of which are human/pharma targets. For agricultural targets like Botrytis β-glucan synthase, we're operating slightly out-of-distribution. Mitigant: we validate IG predictions against published mutagenesis data, and we use docking baselines (Vina, GNINA) as independent sanity checks.

**"Will IG analysis work on Boltz-2's architecture?"**
> Boltz-2 attention extraction requires modifying internal forward methods — not just hooks. We've read the architecture code. IG gradients target the binding affinity output, not embeddings. We verify tensor shapes and residue indexing to avoid off-by-one bugs that would invalidate the analysis.

**"What's your hit rate?"**
> Literature benchmark for de novo antimicrobial peptides: 10–30% show activity at MIC ≤ 32 µg/mL. Our interpretability-guided approach should be at the high end — we're not selecting randomly. We'll know more after our first in vitro round.
