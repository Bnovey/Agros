# Tech Stack — Decisions and Rationale

Organized by pipeline layer. For each tool: what it does, why we chose it over alternatives, and what it means for the business.

---

## Layer 1: Peptide Generation

### BoltzGen
**What it does:** Generates novel peptide sequences conditioned on a target protein structure. Uses flow matching in structure space — it designs toward a binding pocket, not at random.

**Why we chose it:**
- Designed specifically for target-conditioned generation — the model is optimized for "given this binding site, generate peptides likely to bind it," which is exactly our task
- Shares an ecosystem with Boltz-2 (same lab group, MIT). The two models are designed to work together — BoltzGen proposes, Boltz-2 scores. No translation layer needed.
- MIT license: we can fine-tune, modify, and use commercially without restriction

**Alternatives we considered:**
- *RFDiffusion (Baker lab):* Strong for protein backbone generation but not optimized for small peptide-protein binding. Better for larger protein design tasks.
- *ProtGPT2 / ESM-IF:* Language-model-based sequence generators. Generate in sequence space, not structure space — miss 3D binding geometry. Useful for diversity, not targeted binding.
- *AfDesign:* AlphaFold2-based hallucination. Clever but slower, less suited to peptide-specific generation, and AF2 doesn't natively score binding affinity.

**Business implication:** BoltzGen + Boltz-2 as an integrated ecosystem is not widely deployed for ag targets — we're early movers using this combination in a domain where the data is sparse. First-mover advantage compounds as we generate proprietary training data.

---

## Layer 2: Structure Prediction + Binding Affinity Scoring

### Boltz-2
**What it does:** Predicts the 3D structure of peptide-protein complexes and outputs a binding affinity score. This is the primary ranking signal for candidate prioritization.

**Why we chose it:**
- State-of-the-art binding affinity prediction for peptide-protein complexes, with binding affinity outputs built in (not just structure)
- MIT license — critical. AlphaFold3 is the main alternative and it is strictly better on benchmarks, but Google restricts commercial use and API access. We can't build a company on a model we don't control.
- Active development: the Boltz team is releasing updates; we're invested in an improving tool, not a frozen one
- Fine-tunable with Hugging Face PEFT — we can adapt it to ag-specific targets as we generate wet lab data

**Alternatives we considered:**
- *AlphaFold3 (Google):* Best-in-class, but closed API with no commercial use. Not viable for a company.
- *Chai-1 (Chai Discovery):* Promising, similar architecture to Boltz-2, open weights. We monitor it as a potential secondary scorer or future replacement.
- *RoseTTAFold / RF-Diffusion:* Good for structure prediction, but binding affinity scoring is weaker than Boltz-2.
- *AutoDock Vina (physics-based):* We use this as a validation baseline (see Layer 4), but physics-based docking is 100-1000× slower than ML inference and much less sensitive for peptides.

**Business implication:** Our decision to run on Boltz-2 rather than AlphaFold3 is specifically because we need a model we can fine-tune on our own wet lab data. The fine-tuned Boltz-2 — trained on our proprietary peptide-activity pairs for ag targets — becomes an asset no competitor can replicate.

---

## Layer 3: Sequence-Level Embeddings

### ESM-2 (Meta, facebookresearch/esm)
**What it does:** Protein language model that converts amino acid sequences into high-dimensional embeddings capturing evolutionary and structural information. Used for sequence clustering, similarity search, and as input features for downstream models.

**Why we chose it:**
- Mature, extensively benchmarked, and widely cited — our results using ESM-2 embeddings are directly comparable to published literature
- Efficient inference on CPU for embedding-only tasks — doesn't require GPU for every candidate
- MIT license
- Specifically good for peptides: ESM-2 is trained across the full protein sequence space, including short peptides

**Alternatives we considered:**
- *ESM-3:* Newer, more powerful (generates structure + sequence + function simultaneously). We may migrate to ESM-3 as it matures, but it's heavier and less stable for production inference currently.
- *ProtTrans / prot_bert:* Earlier protein language models. ESM-2 is strictly better on most benchmarks.
- *One-hot encoding:* No information about evolutionary context. Discarded immediately.

---

## Layer 4: Interpretability (Core Differentiator)

### Integrated Gradients (IG) on Boltz-2
**What it does:** Computes per-residue attribution scores — for each amino acid in a candidate peptide, how much does it contribute to the predicted binding affinity? Produces a heatmap showing which residues drive binding.

**Why this approach:**
- Integrated Gradients is theoretically grounded (Sundararajan et al., 2017) and satisfies axioms (completeness, sensitivity) that simpler gradient methods don't
- Produces residue-level scores that are directly interpretable: "residue 4 (His) contributes 38% of the binding signal" is actionable
- Applicable to any differentiable model — works directly on Boltz-2 without model modification beyond exposing the gradient path

**Why not attention maps:**
- Attention weights don't have a clean relationship to model outputs — high attention ≠ high contribution to binding prediction
- We're using attention maps for the research paper (per-residue attention analysis of Boltz-2 on ag vs. pharma targets), but IG is the right tool for the discovery pipeline
- The paper and the pipeline use different interpretability methods intentionally: attention for comparative analysis, IG for actionable residue attribution

**Implementation note from CLAUDE.md:**
> Boltz-2 attention extraction requires modifying internal forward methods, not just hooks. We've read the architecture. IG gradients must target the binding affinity output — not embeddings or confidence scores. Tensor shapes and residue indexing are logged alongside saved attributions to prevent off-by-one bugs.

**Business implication:** This layer is the moat. Bindwell screens broadly and blindly. We know which residues matter before we synthesize. That means our second round is always smarter than our first, compounding across every discovery campaign.

---

## Layer 5: Sequence Redesign

### ProteinMPNN (Baker lab, MIT license)
**What it does:** Inverse folding — given a backbone structure, generates sequences likely to fold into it while optimizing for other properties. Used as a redesign pass on top-ranked BoltzGen candidates to improve stability while preserving predicted binding geometry.

**Why we chose it:**
- Battle-tested on the largest published inverse folding benchmark
- Fast inference — redesigns thousands of sequences in minutes on CPU
- MIT license
- Integrates directly with our PyTorch pipeline

**Alternatives we considered:**
- *LigandMPNN:* Baker lab successor to ProteinMPNN, explicitly conditioned on small molecules and ligands. We monitor it and may migrate — it's better suited to our binding-conditioned redesign task. Currently less stable in production.
- *ESM-IF:* ESM-based inverse folding. Comparable performance but less community tooling around it.

---

## Layer 6: Physics-Based Validation Baselines

### AutoDock Vina + GNINA
**What it does:** Physics-based molecular docking. Independently predicts binding pose and affinity using force field calculations, not ML. Used for paper benchmarking and as a sanity check on Boltz-2 predictions.

**Why we use both ML and physics:**
- Orthogonal failure modes: ML models fail when the target is out of distribution; physics-based models fail when the binding pocket has unusual geometry. Candidates that score well on both have higher confidence.
- Required for the paper: benchmarking Boltz-2 against physics-based baselines is scientifically necessary to claim superiority

**Why Vina specifically:**
- Free, open source, standard in academic literature — our benchmarks are directly comparable to published work
- GNINA extends Vina with an ML scoring layer (CNN) on top of the physics engine — better performance than Vina alone for protein-ligand systems
- No licensing cost, no API dependency

**Alternatives considered:**
- *Glide (Schrödinger):* Industry gold standard. $30K+/year license. Not viable at our stage.
- *GOLD (CCDC):* Academic license available but complex. Vina/GNINA is sufficient for our benchmarking needs.

---

## Layer 7: Candidate Filters

### NetSolP
**What it does:** Predicts solubility of peptide sequences in aqueous solution. Filters out candidates that are likely to aggregate or precipitate before synthesis.

**Why it's in the pipeline:**
- Synthesizing an insoluble peptide wastes $100–300 and an assay slot. NetSolP screens this out computationally for essentially zero cost.
- Specifically trained on peptides — general small-molecule solubility models don't transfer well to peptides

**Why NetSolP over alternatives:**
- Published benchmark specifically on peptide solubility (not small molecules) — it's the right tool for the job
- Easy Python integration — one forward pass per sequence

### RDKit
**What it does:** Molecular validation — confirms SMILES parsability, computes basic molecular properties, screens for structural alerts.

**Why it's in the pipeline:**
- Standard filter for anything wrong at the chemical representation level before sending to more expensive downstream steps
- Catches BoltzGen output artifacts (malformed sequences, invalid residues) early
- RDKit is the industry standard; no justification needed

### OpenMM
**What it does:** Molecular dynamics (MD) simulation for stability validation. Simulates top candidates in explicit solvent to check that predicted structures are thermodynamically stable.

**Why OpenMM specifically:**
- Python-native — other MD packages (GROMACS, AMBER, NAMD) require subprocess calls and complex input file management. OpenMM runs entirely in Python, integrates cleanly with our pipeline.
- GPU-accelerated with CUDA
- MIT license
- Used only on top candidates (post-synthesis shortlist) — not on the full candidate set, which would be prohibitively expensive

**Alternatives considered:**
- *GROMACS:* More commonly used in academic MD, higher performance for large systems. The subprocess interface makes it harder to integrate. OpenMM is sufficient for peptide-scale simulations.

---

## Layer 8: ML Infrastructure

### Hugging Face PEFT (LoRA fine-tuning of Boltz-2)
**What it does:** Parameter-efficient fine-tuning — adapts Boltz-2 to ag-specific targets by training only a small set of adapter parameters (LoRA) rather than the full model. This lets us specialize the model on our proprietary wet lab data without overfitting or requiring massive compute.

**Why this matters:**
- Full fine-tuning of Boltz-2 requires updating hundreds of millions of parameters — expensive and prone to catastrophic forgetting
- LoRA (Low-Rank Adaptation) updates a small fraction of parameters while freezing the rest — achieves 80%+ of full fine-tuning performance at 10% of the compute cost
- As we generate wet lab data, each fine-tuning round makes the model better on our specific targets. This is the compounding effect that builds the moat.

**Alternatives considered:**
- *Full fine-tuning:* Too expensive for early-stage, and risks losing general protein knowledge that Boltz-2 is pre-trained on
- *Prompt tuning / prefix tuning:* Less effective for structural protein models than LoRA

### Weights & Biases (W&B) / MLflow — Experiment Tracking
**What it does:** Logs every experiment run: model version, seed, target, hyperparameters, loss curves, candidate scores, IG attributions. Creates a reproducible record of every discovery campaign.

**Why it's non-negotiable:**
- Without experiment tracking, we cannot reproduce results — which kills the paper and undermines IP claims
- W&B is the standard in ML research; every model we use has W&B integration built in

---

## Layer 9: Compute

### Modal (Serverless GPU — A100/H100)
**What it does:** Runs all GPU-intensive inference (BoltzGen generation, Boltz-2 scoring, IG gradient computation, OpenMM MD) on serverless A100/H100 GPUs. Pay per second of GPU use, no standing servers.

**Why Modal specifically:**
- **Bursty workload match:** We don't run inference continuously — we run design campaigns (intense GPU bursts) followed by wet lab waiting periods. Reserved instances would mean paying for idle GPU 80% of the time. Modal charges only for what we use.
- **Python-native:** Modal apps are Python functions decorated with `@app.function()`. No YAML, no Kubernetes, no DevOps overhead. Frederick can build and deploy a new inference pipeline in an afternoon.
- **Image pinning:** Modal images pin exact model checkpoint hashes and package versions. This is required for reproducibility — the same image runs the same computation six months later.
- **Cost:** ~$300–500 for the paper phase; $1–5K/month for design campaigns. This is viable pre-revenue.

**Alternatives considered:**
- *AWS EC2 reserved instances:* Pay whether you use them or not. Fine for always-on workloads, expensive for bursty research workloads.
- *Lambda Labs / RunPod / Vast.ai:* Cheaper per-hour than Modal but require more DevOps overhead and have no Python-native deployment model. The engineering time cost outweighs the price difference at our stage.
- *Google Colab Pro+:* Adequate for notebooks, not for production inference pipelines with reproducibility requirements.
- *AWS SageMaker:* Heavyweight, expensive, complex to configure. Overkill for our current scale.

**Business implication:** Modal's pay-per-use model means our compute cost scales linearly with the number of candidates we evaluate. At $5–20/candidate for scoring + IG, we can run 50 candidates for under $1K. This makes the discovery loop economically viable at our current funding level.

---

## Layer 10: Dev Infrastructure

### Python 3.11+
The entire scientific ecosystem — PyTorch, RDKit, NumPy, SciPy, Biopython — runs on Python. 3.11 provides meaningful performance improvements over 3.10 (~10–60% faster on compute-bound tasks). No real alternative given our model dependencies.

### GitHub + Git
Standard version control. Every experiment produces a manifest (target, seed, model hash, timestamp) committed to the repo — this is our paper's reproducibility guarantee and our IP timestamping mechanism.

### Docker / Modal image specs
All GPU workloads run inside pinned Docker images (managed by Modal). Pinning package versions and model checkpoint hashes means compute results from month 1 are reproducible in month 12. Required for the paper; also required for any serious IP claim ("we ran this model, on this data, and got this result").

### Ruff + Black + Pre-commit
Linting (Ruff) and formatting (Black) enforced via pre-commit hooks. In a two-person team, the only code review bottleneck that matters is correctness, not style — these tools remove style discussions entirely.

---

## Summary: Why This Stack Fits a 2-Person Team at Pre-Seed

| Constraint | How the stack addresses it |
|-----------|--------------------------|
| No standing DevOps | Modal handles all GPU infrastructure in Python |
| Small wet lab budget | NetSolP + RDKit filters eliminate bad candidates before synthesis |
| Need to reproduce results for the paper | W&B + Git manifests + Docker image pinning |
| Can't afford AlphaFold3 licensing | Boltz-2 (MIT) is comparable and fine-tunable |
| Need to move fast between model and wet lab | Python-native everything — no context switching to YAML/bash |
| IP moat requires model specialization | Hugging Face PEFT fine-tuning on proprietary wet lab data |

---

## What We'd Change With More Funding

- Add LigandMPNN (over ProteinMPNN) for better ligand-conditioned redesign
- Build a dedicated in-house wet lab or formal university lab partnership (removes dependency on contract labs)
- Expand to ESM-3 for sequence generation in parallel to BoltzGen
- Add active learning loop: wet lab results automatically trigger the next generation campaign without manual intervention
