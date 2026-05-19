# How BoltzGen, Boltz-2, and Chai-1 Work

**Type:** ML Research — technical guide
**Date:** 2026-05-19
**Relevance:** Core models in the Agros discovery pipeline and research paper

---

## Overview: What Each Model Does

| Model | Role | Output |
|---|---|---|
| **BoltzGen** | *Generator* — proposes new peptide sequences | Peptide sequences + predicted complex structures |
| **Boltz-2** | *Scorer* — evaluates structure quality and binding affinity | 3D structure + binding affinity (Kd proxy) |
| **Chai-1** | *Scorer (alternative)* — evaluates structure quality | 3D structure + confidence scores |

BoltzGen asks "what sequence should bind this target?" Boltz-2 and Chai-1 ask "how well does this sequence bind?" They are complementary: generation → scoring → filter → iterate.

---

## BoltzGen

### What it does

BoltzGen is an all-atom generative model for designing protein/peptide binders against a given target structure. Given a target (e.g. the nAChR binding pocket), it generates candidate binder sequences *and* their predicted complex structure in a single forward pass — generation and structure prediction are unified, not sequential.

It handles multiple binder modalities: linear peptides, disulfide-bonded cyclic peptides, nanobodies. Validated across 26 pharma/academic wet labs; achieves nanomolar-level binding in ~66% of difficult targets (novel, <30% sequence similarity to known binders).

### Core architecture

**Unified generation + folding.** Most prior approaches separate sequence generation from structure prediction (generate → fold → score → repeat). BoltzGen does both simultaneously using a geometry-based residue representation that captures structural interactions during generation itself. The model reasons about the binding interface *while* sampling the sequence.

**Flow matching / diffusion.** BoltzGen uses a flow-matching generative process over the joint space of sequence and 3D coordinates. Rather than predicting a single deterministic output, it samples from a distribution — run multiple times to get a diverse ensemble of candidates.

**Multi-task training.** Trained across diverse biomolecular design tasks simultaneously, enabling transfer learning — patterns learned from nanobody design transfer to peptide design.

**Design specification language.** Users provide constraints as a structured specification:
- Sequence length range
- Which residues are fixed vs. designable
- Binding site / pocket constraints (which target residues to contact)
- Covalent bond constraints (for cyclic/disulfide peptides)
- Structural flexibility parameters

This lets you say "design a 10–15 residue cyclic peptide that contacts residues 150–175 of this receptor" rather than hoping the model guesses the right pocket.

### How to run it (conceptually)

```
Input:  target PDB structure + design spec (length, pocket, modality)
→ BoltzGen samples N candidate sequences + their predicted complexes
Output: FASTA of candidate sequences + PDB of predicted bound structures
```

Use it as an **iterative design partner**, not a one-shot oracle. Start small (N=50–100), inspect the outputs, adjust constraints, resample.

---

## Boltz-2

### What it does

Boltz-2 is a structure prediction model that adds **binding affinity prediction** on top of Boltz-1's folding capabilities. It takes a protein-ligand or protein-peptide complex (sequences + optional template) and outputs:

1. Predicted 3D structure of the complex
2. A binding affinity score (proxy for Kd/IC50)
3. Confidence scores (pLDDT, PAE)

It is described as the first AI model to approach the performance of free-energy perturbation (FEP) methods on binding affinity — traditionally the gold standard but computationally expensive.

### Architecture: four modules

**1. Trunk**
The backbone. A PairFormer network that builds pairwise representations between every pair of residues/atoms. Uses triangular attention — operations that explicitly model three-body geometric relationships (if A contacts B and B contacts C, A and C are implicitly related). This is where most of the structural reasoning happens.

Internally represents each token (residue or atom) with:
- A *single* representation: per-token embedding capturing local chemical context
- A *pair* representation: $$N \times N$$ matrix capturing all pairwise relationships

**2. Denoising module**
Handles structure generation via diffusion over atomic coordinates. Given the trunk's pair representations, it iteratively denoises a noisy 3D configuration toward the predicted structure. Supports *steering*:
- Method conditioning (X-ray vs. cryo-EM vs. NMR — different experimental contexts produce different conformations)
- Template conditioning (use a known structure as a prior)
- Contact/pocket constraints (force specific contacts during prediction)

**3. Affinity module**
The key addition over Boltz-1. A separate PairFormer operating on the trunk's final protein-ligand and intra-ligand pair representations. Two prediction heads:

- **Binary head**: Is this a binder at all? (classification)
- **Regression head**: What is the binding affinity? (continuous output, log-scale in µM)

Separating affinity estimation from structure generation prevents the two objectives from fighting each other during training while still coupling them through shared trunk representations.

**Training data for the affinity module:**
- 1.2M binders from ChEMBL + BindingDB across ~2,000 protein targets (continuous Kd/IC50 values)
- 200K+ binders + 1.8M+ decoys from PubChem HTS screens (binary labels)
- Standardized to log µM scale — Kd, IC50, Ki, AC50, EC50 are all treated as proxies for binding strength within their respective assay contexts

**4. Confidence module**
Predicts reliability of the structure output: pLDDT (per-residue confidence, 0–100) and PAE (predicted aligned error — how confident the model is about the relative position of each pair of residues). Low pLDDT at the interface = unreliable binding prediction.

### What Kd and IC50 mean

$$K_d$$ (dissociation constant): equilibrium measure of how tightly a ligand binds. Lower = tighter. A binder with $$K_d = 1 \text{ nM}$$ is 1000× stronger than one with $$K_d = 1 \text{ µM}$$.

$$IC_{50}$$ (half-maximal inhibitory concentration): functional measure — the concentration needed to inhibit 50% of a target's activity. Depends on assay conditions, so less absolute than Kd but more directly actionable for biopesticide efficacy.

Boltz-2 predicts a *proxy* for these values, not a calibrated absolute measurement. Use it for **ranking candidates relative to each other**, not as an absolute prediction of Kd.

### Key improvements over Boltz-1

- Affinity module (new)
- Ensemble supervision — trained on MD ensembles (MISATO, ATLAS, mdCATH), not just static crystal structures → better handles flexible/disordered regions
- B-factor prediction — per-residue flexibility supervision, capturing local dynamics
- Better antibody-antigen and challenging complex prediction

---

## Chai-1

### What it does

Chai-1 is a multi-modal structure prediction model from Chai Discovery (2024). It predicts atomic-resolution 3D structures of proteins, antibodies, multimers, nucleic acids, and protein-ligand complexes from sequence and SMILES inputs. It does *not* predict binding affinity — it is a structure oracle.

Its main role in the Agros pipeline is as an **independent scoring signal** to cross-validate Boltz-2 structure predictions and provide a second confidence estimate.

### Architecture

**Input encoding.** Takes biomolecular sequences (and SMILES for small molecules) and encodes them into numerical representations. Optionally incorporates:
- MSAs (multiple sequence alignments) — evolutionary patterns from homologs improve prediction quality
- Language model embeddings — per-residue contextual embeddings from protein LMs
- Experimental constraints — cross-linking mass spectrometry data, epitope maps, known contact residues

**Transformer trunk.** Self-attention over all residues — every position can attend to every other position, capturing long-range interactions across the complex. Modular design: input features can be selectively enabled/disabled depending on what data is available.

**Diffusion head.** Iteratively denoises 3D coordinates from noise to a predicted structure, conditioned on the trunk representations.

**Key distinction from Boltz-2:** Chai-1 does not have an affinity module. It is strictly a structure predictor. Where Boltz-2 outputs a binding affinity score alongside the structure, Chai-1 outputs a structure with confidence scores only.

### When to use Chai-1 vs. Boltz-2

| Situation | Use |
|---|---|
| Primary affinity scoring for candidate ranking | Boltz-2 |
| Independent structure verification | Chai-1 (compare pLDDT, check if structures agree) |
| Antibody-antigen complex prediction | Either — compare both |
| You have MSA data available | Chai-1 (uses MSA natively; stronger with it) |
| Speed-critical pipeline | Boltz-2 (generally faster on GPU) |

---

## How They Fit Together in the Agros Pipeline

```
Target structure (PDB)
        │
        ▼
  ┌─────────────┐
  │  BoltzGen   │  ← design spec (length, pocket, modality)
  └─────────────┘
        │ N candidate sequences + predicted complexes
        ▼
  ┌─────────────────────┐
  │  Boltz-2 scoring    │  ← re-score each candidate
  │  - affinity (proxy) │
  │  - structure        │
  │  - confidence       │
  └─────────────────────┘
        │
        ├─── filter by pLDDT at interface (>70)
        ├─── filter by affinity rank (top 10–20%)
        ▼
  ┌─────────────────────┐
  │  Chai-1 (optional)  │  ← independent structure check on survivors
  └─────────────────────┘
        │
        ▼
  ┌─────────────────────┐
  │  Downstream filters │  ← NetSolP, RDKit, OpenMM
  │  (solubility, ADMET)│
  └─────────────────────┘
        │
        ▼
   Lead candidates → wet-lab assay
```

### Critical implementation notes

- **Affinity scores from Boltz-2 are relative, not absolute.** Use them to rank candidates within a run, not to compare across different targets or different run configurations.
- **Never fill in Boltz-2/Chai-1 scores with synthetic data.** If the model fails, fix the integration. The pipeline has no value if scores are fabricated.
- **Attention extraction for the paper** (Boltz-2): the per-residue attention maps live in the trunk's pair representation stack. Extracting them may require modifying Boltz-2's internal forward methods — hooks alone may not capture triangle attention intermediate states. Read the architecture source before assuming standard hook-based extraction works.
- **Seed everything** before running BoltzGen — it samples stochastically and results must be reproducible.
- **Pin model checkpoints by hash**, not by name — BoltzGen and Boltz-2 are actively developed and weights change between releases.

---

## Sources

- [BoltzGen paper — bioRxiv](https://www.biorxiv.org/content/10.1101/2025.11.20.689494v1.full)
- [BoltzGen — PMC full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC12697729/)
- [Behind the Model: Building BoltzGen with Hannes Stärk — Benchling](https://www.benchling.com/blog/behind-the-model-building-boltzgen-with-hannes-staerk)
- [BoltzGen GitHub](https://github.com/HannesStark/boltzgen)
- [Boltz-2 paper — bioRxiv](https://www.biorxiv.org/content/10.1101/2025.06.14.659707v1.full.pdf)
- [Boltz-2 — PMC full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC12262699/)
- [Boltz-2 NVIDIA NIM model card](https://build.nvidia.com/mit/boltz2/modelcard)
- [Chai-1 paper — bioRxiv](https://www.biorxiv.org/content/10.1101/2024.10.10.615955v1.full)
- [Chai-1 GitHub](https://github.com/chaidiscovery/chai-lab)
