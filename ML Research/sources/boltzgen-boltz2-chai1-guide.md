# How BoltzGen, Boltz-2, Chai-1, and Protenix-v1 Work

**Type:** ML Research — deep technical guide
**Date:** 2026-05-19
**Relevance:** Core models in the Agros discovery pipeline and research paper

---

## TL;DR

Four models, four jobs:

- **BoltzGen** — *generates* candidate peptide sequences + their predicted bound structures simultaneously, using a diffusion process over joint sequence-structure space. You give it a target PDB and a design spec; it samples a population of binders.
- **Boltz-2** — *scores* those candidates by predicting both the 3D structure of the complex and a binding affinity proxy (Kd/IC50 scale). Its new affinity module sits on top of a shared structural trunk and is the primary ranking signal in the pipeline.
- **Chai-1** — *independently validates* structure predictions. Same PairFormer + diffusion paradigm as Boltz-2, but no affinity module. Useful as a second-opinion on complex geometry and interface confidence.
- **Protenix-v1** — ByteDance's fully open-source AF3 reimplementation (Apache 2.0). Matches AF3 accuracy, supports RNA MSA + multimer templates, and is the only realistic fine-tuning candidate post-funding. No affinity module — structural oracle only.

All four descend from the same architectural lineage: **AlphaFold3's PairFormer + diffusion** framework. Understanding that foundation is the key to understanding all four models.

|                          | BoltzGen               | Boltz-2                    | Chai-1                | Protenix-v1              |
| ------------------------ | ---------------------- | -------------------------- | --------------------- | ------------------------ |
| **Job**                  | Generate binders       | Score structure + affinity | Score structure only  | Score structure only     |
| **Output**               | Sequence + complex PDB | Structure + affinity proxy | Structure + pLDDT/PAE | Structure + pLDDT/PAE    |
| **Affinity prediction**  | No                     | Yes                        | No                    | No                       |
| **MSA required**         | No                     | No                         | Optional (helps)      | Optional (helps)         |
| **Open source**          | Yes (MIT)              | Yes (MIT)                  | Yes (non-commercial)  | Yes (Apache 2.0)         |
| **Training code open**   | No                     | No                         | No                    | Yes                      |
| **Primary use in Agros** | Candidate generation   | Ranking                    | Cross-validation      | Watch list / fine-tuning |

---

## Part 1: The Shared Foundation — PairFormer and Diffusion

Before going into each model, you need to understand the two core components they all share: the **PairFormer trunk** and the **diffusion structure module**. Everything else is built on top of these.

### 1.1 Two levels of representation

These models think about molecules at two simultaneous resolutions:

**Token level.** A token is the coarse unit:
- One amino acid = one token (for proteins)
- One nucleic acid base = one token (for DNA/RNA)
- One heavy atom = one token (for small molecules / ligands)

**Atom level.** All-atom resolution — every hydrogen-bonding nitrogen, every carbonyl oxygen. This is needed for accurate docking geometry but is expensive to compute everywhere.

The trunk operates primarily at token level. The diffusion module operates at atom level. Bridging them — expanding token representations back out to atoms, then averaging atoms back to tokens — is handled by dedicated encoder/decoder layers at the boundaries.

### 1.2 The PairFormer trunk

The trunk's job is to build a rich representation of every *pair* of tokens in the input — a full $$N \times N$$ matrix where each cell $$(i, j)$$ holds a learned vector representing the relationship between residue $$i$$ and residue $$j$$. Alongside this, each token also has its own *single* representation capturing local chemical context.

This pair matrix is the central data structure. Everything — attention, structure prediction, affinity estimation — reads from and writes to it.

The trunk is a stack of **PairFormer blocks**, each containing:
1. Triangle multiplicative update (outgoing)
2. Triangle multiplicative update (incoming)
3. Triangle self-attention (row-wise)
4. Triangle self-attention (column-wise)
5. Single-sequence transition (feedforward on token representations, using pair as bias)

Chai-1 uses **48 PairFormer blocks**. Boltz-1/2 uses a similar count (not officially disclosed, inherited from AlphaFold3 which uses 48). Each block has independent trainable parameters — no weight sharing.

**Chai-1 embedding dimensions (the only model to publish these):**
- Pair representation: **128 channels** per (i,j) cell
- Single representation: **384 channels** per token

### 1.3 Triangular attention — why it's called that

Regular self-attention lets each position attend to every other position independently. That's fine for language, but protein structure has a hard geometric constraint: if residue A is 5Å from residue B, and residue B is 5Å from residue C, then A and C must be within 10Å of each other (triangle inequality). Standard attention doesn't know this.

Triangular attention enforces geometric consistency by having the pair representation for edge $$(i, j)$$ be updated using *two other edges that share a vertex* with it.

**Row-wise (starting node) attention:** For the pair $$(i, j)$$, attend over all pairs $$(i, k)$$ — edges sharing the source node $$i$$. This asks: "given that I know how $$i$$ relates to $$k$$, and how $$i$$ relates to $$j$$, can I infer how $$j$$ and $$k$$ relate?"

**Column-wise (ending node) attention:** For the pair $$(i, j)$$, attend over all pairs $$(k, j)$$ — edges sharing the destination node $$j$$. Mirror image of the above.

**Triangle multiplicative update:** A cheaper, non-attention version of the same idea. For the pair $$(i, j)$$:

$$z_{ij} \leftarrow z_{ij} + \text{LayerNorm}\left(\sum_k (W_a \cdot z_{ik}) \odot (W_b \cdot z_{kj})\right)$$

Each entry in the 128-channel pair vector for edge $$(i, k)$$ is multiplied elementwise with the corresponding entry for edge $$(k, j)$$, then summed over all intermediate tokens $$k$$. This sweeps over all possible "third atoms" and propagates geometric constraints. Two variants: outgoing ($$i, k$$ and $$k, j$$) and incoming ($$k, i$$ and $$j, k$$).

This is the computationally expensive part — it's $$O(N^3)$$ in the naive form ($$N \times N$$ pairs, each updated by $$N$$ intermediates). In practice, the trifast kernel (used in Boltz-2) and sequence-local approximations (used in Boltz-1) make it tractable.

### 1.4 The diffusion structure module

Once the trunk has built the pair and single representations, the structure module uses them to predict 3D atomic coordinates.

The approach is **score-based diffusion** (specifically the EDM / DDPM framework): you start with random noise — atoms placed at random positions — and iteratively denoise toward a physically plausible structure, conditioned on the pair representations.

**Forward process (training):** Add Gaussian noise to real atomic coordinates at noise level $$t$$:

$$\mathbf{x}_t = \mathbf{x}_0 + t \cdot \boldsymbol{\epsilon}, \quad \boldsymbol{\epsilon} \sim \mathcal{N}(0, I)$$

**Reverse process (inference):** A neural denoiser $$D_\theta(\mathbf{x}_t, t; z)$$ predicts the clean structure from the noisy one, conditioned on trunk features $$z$$:

$$D_\theta(\mathbf{x}, t; z) = \frac{\sigma^2_{\text{data}}}{\sigma^2_{\text{data}} + t^2} \mathbf{x} + \frac{t \cdot \sigma_{\text{data}}}{\sqrt{\sigma^2_{\text{data}} + t^2}} F_\theta(\mathbf{x}, t; z)$$

The model is run for multiple denoising steps (Boltz-1 uses **200 steps**, with performance plateauing at 50), progressively refining the structure. **Recycling** (running the trunk again on the predicted structure, then denoising again) further improves quality — Boltz-1 uses **10 recycling rounds**, plateauing at 3.

The structure module in Boltz-1/BoltzGen has a layered design:
- **3 atom-level layers** (sequence-local attention on individual atoms; 32 atoms attend to the 128 nearest in sequence space)
- **24 token-level layers** (full attention over tokens, using pair representation as attention bias)
- **3 atom-level layers** again (decode back to per-atom coordinates)

---

## Part 2: BoltzGen — Deep Dive

### 2.1 The core idea: unifying generation and folding

Every previous peptide design workflow had the same structure: generate a sequence → fold it to get structure → score the structure → reject or keep → repeat. Generation and structure are separate models that don't talk to each other.

BoltzGen eliminates this separation. It runs a *single diffusion process over the joint space of sequence and 3D coordinates simultaneously*. The model learns to sample (sequence, structure) pairs together, with structural reasoning informing sequence generation at every denoising step.

This matters because it allows the model to avoid sequences that would fold into a structure that doesn't fit the binding pocket — it can "see" the steric consequences of its sequence choices while still designing the sequence.

### 2.2 Residue identity as geometry — a key innovation

Normally, a generative model represents "which amino acid is at position 5" as a discrete categorical variable (a one-hot vector over 20 amino acids). BoltzGen does something unusual: it encodes residue identity *geometrically*, using the placement of virtual atoms.

Each designable residue gets a 14-atom representation. The residue type is signaled by where those 14 atoms are placed — specifically, which backbone atoms virtual atoms are stacked on top of. Examples:
- **Proline**: 7 virtual atoms placed on the backbone oxygen
- **Threonine**: 3 atoms on the nitrogen + 4 atoms on the oxygen

This means the diffusion process generates atom positions rather than discrete amino acid labels. The residue identity *emerges* from the geometry. The benefit: the model's loss function operates on continuous 3D coordinates (easier to train by diffusion) rather than discrete tokens (harder to diffuse over).

### 2.3 Diffusion process details

The noise schedule follows the EDM framework. The forward process is:

$$d\mathbf{X}_t = \sqrt{2}\, t\, d\mathbf{B}_t$$

The noise distribution samples $$\sigma_{\text{data}} \cdot \exp(-1.2 + 1.5 \cdot \mathcal{N}(0,1))$$, concentrated at intermediate noise levels.

**Training loss** is a time-weighted combination of three terms:

$$\mathcal{L}_\theta = \mathbb{E}\left[w_t \cdot \left(\mathcal{L}_{\text{MSE}} + \mathcal{L}_{\text{bond}} + \mathcal{L}_{\text{smooth-lDDT}}\right)\right]$$

Where $$w_t = \frac{t^2 + \sigma^2_{\text{data}}}{t \cdot \sigma^2_{\text{data}}}$$.

- **$$\mathcal{L}_{\text{MSE}}$$**: Mean squared error between predicted and true atom positions (main structure loss)
- **$$\mathcal{L}_{\text{bond}}$$**: Penalty on predicted bond lengths that deviate from chemistry
- **$$\mathcal{L}_{\text{smooth-lDDT}}$$**: Differentiable approximation to lDDT (local distance difference test) — penalizes predictions that get local neighborhood distances wrong

### 2.4 Generation sampling — the dilated schedule

At inference, BoltzGen uses a **probability flow ODE** sampler:

$$\mathbf{x}'_t = -\frac{\mathbf{x} - \mu_t(\mathbf{x}_t)}{t} \frac{dt}{dt}$$

with configurable noise scale $$\beta$$ (diversity) and step scale $$\alpha$$ (convergence). High $$\beta$$ → more diverse but less precise candidates. Low $$\beta$$ → more reproducible, potentially lower diversity.

A key trick for peptide design is the **dilated schedule**: the interval $$\tau \in [0.6, 0.8]$$ of the diffusion timeline is stretched by a factor $$\lambda = 8/3$$ across the total 300 function evaluations. Why? This is the range where residue types crystallize in the denoising process — the 14-atom geometric encoding "decides" which amino acid is at each position during this window. Spending more sampling steps here gives the model more resolution over the hardest design decision (sequence choice), without wasting compute on the early (pure noise) or late (fine-tuning geometry) stages.

### 2.5 The design specification language

BoltzGen accepts a structured specification that conditions the diffusion process. The constraints are encoded as:
- **Expanded token-level features** injected into atom-level input representations
- **Attention biases** applied to the pair representation across the trunk and diffusion module

The four constraint types:

| Constraint | How encoded | Use case |
|---|---|---|
| **Covalent bonds** | Pairwise bond matrix | Disulfide bridges, cyclic peptides |
| **Structure groups** | Fixed pairwise distances between atoms | Constrain a known motif |
| **Binding sites** | Binary residue-level mask (contact/no-contact) | Focus design on a specific pocket |
| **Secondary structure** | Per-residue label (helix/sheet/coil) | Force an α-helical binder |

For our pipeline: specify the nAChR binding pocket residues as the binding site constraint, set peptide length to 8–20, and let BoltzGen design the sequence.

### 2.6 Training tasks and data

BoltzGen is jointly trained on four tasks in each batch, which provides multi-task regularization and prevents overfitting to any single design paradigm:

1. **Folding** — No designable residues; pure structure prediction. Keeps the model honest about structural reasoning.
2. **Binder design** — One chain is fully designable, the rest are fixed targets. The main task.
3. **Motif scaffolding** — Some residues in the binder are fixed (a pharmacophore); the model builds around them.
4. **Unconditional design** — Everything is designable; no target.

Training data: experimental PDB structures + AlphaFold2 AFDB self-distillation + Boltz-1 predictions. Crop sizes up to 768 residues for folding, 512 for generative tasks.

### 2.7 Post-generation pipeline

After BoltzGen samples N candidates, a six-stage pipeline scores and filters them:

1. Generate via diffusion (BoltzGen)
2. Optional inverse folding with **BoltzIF** (a 6-layer encoder that redesigns the sequence given a fixed backbone — used to improve sequence-structure consistency)
3. Refold with **Boltz-2** and compute RMSD vs. BoltzGen's predicted structure (candidates with high RMSD disagree with Boltz-2 → deprioritize)
4. Boltz-2 affinity prediction
5. Physics-based metrics: H-bond count, salt bridges, buried surface area, NetSolP solubility
6. **Quality-diversity selection** using worst-rank aggregation — rank each candidate by each metric, take the worst rank across all metrics, maximize that (selects candidates that are good on *all* criteria, not just one). Diversity term penalizes structural/sequence redundancy:

$$\text{score}(x, A) = 1 - w_{\text{struct}} \cdot \text{TM-score}(x, A) - w_{\text{seq}} \cdot \text{seq-align}(x, A)$$

where $$A$$ is the already-selected set.

---

## Part 3: Boltz-2 — Deep Dive

### 3.1 What Boltz-2 adds over Boltz-1

Boltz-1 is a structure predictor. Boltz-2 keeps that capability and adds three things:

1. **An affinity module** — the first open model to approach FEP-level binding affinity prediction
2. **Steering at inference time** — method conditioning, template conditioning, contact/pocket constraints
3. **Ensemble supervision** — trained on MD trajectory ensembles, not just static crystal structures, so it handles flexible/disordered regions better

### 3.2 The trunk (inherited from Boltz-1 / AlphaFold3)

The trunk is a 48-layer PairFormer stack operating on paired token representations (see Part 1). Boltz-2 adds two engineering optimizations:

- **bfloat16 mixed precision** — reduces memory footprint, allows larger batches
- **Trifast kernel** — a fused GPU kernel for triangle attention that reduces the $$O(N^3)$$ cost. Standard triangle attention requires materializing $$N^3$$ intermediate values; trifast avoids this with kernel fusion.

**Crop size**: up to **768 tokens** during training, matching AlphaFold3. This is the maximum complex size the model can handle in one pass — complexes larger than 768 residues must be cropped.

The trunk's final output is:
- A pair representation $$\mathbf{Z} \in \mathbb{R}^{N \times N \times 128}$$ (or similar dimension) — used by the affinity module
- A single representation $$\mathbf{S} \in \mathbb{R}^{N \times d}$$ — used by confidence and B-factor heads

### 3.3 The denoising module with steering

The structure diffusion module (same architecture as Boltz-1) is augmented with three conditioning modes:

**Method conditioning.** Crystal structures, cryo-EM structures, and NMR structures represent fundamentally different conformational ensembles — a drug bound to a rigid crystal lattice sits differently than the same drug in solution. Method conditioning lets the user specify the experimental context, shifting the model's predictions toward the appropriate conformational ensemble.

**Template conditioning.** If a homologous structure is known, it can be fed as a structural template with full multimeric support. The template is featurized and added to the trunk input as additional pair features.

**Contact/pocket conditioning.** Distance constraints can be specified between atoms — "residue 150 and residue 2 of the peptide must be within 5Å." These are added as hard or soft biases to the pair representation before diffusion. This is especially useful for peptide design when the binding pose is partially known.

**Boltz-steering** (physics-based potentials applied at inference): during the reverse diffusion process, Boltz-2x applies energy-based steering — gradients from a physical energy function (clash penalties, bond geometry) are added to each denoising step. This is analogous to classifier guidance in image diffusion: the model's denoising direction is nudged toward physically plausible conformations without retraining.

### 3.4 The affinity module — architecture in detail

This is the novel contribution of Boltz-2 and the most important component for the Agros pipeline.

**Input:** The trunk's final pair representation, *specifically the protein-ligand and intra-ligand subgraph* — the $$n_{\text{protein}} \times n_{\text{ligand}}$$ and $$n_{\text{ligand}} \times n_{\text{ligand}}$$ slices of $$\mathbf{Z}$$. The protein-protein pairs are not used for affinity (they don't encode binding-relevant information for small molecule interactions).

**Processing:** A dedicated PairFormer stack (smaller than the trunk) processes these interaction pair representations. The triangle attention within this sub-PairFormer reasons specifically about protein-ligand geometric relationships — if residue A contacts the ligand and residue B contacts the ligand, what does that imply about A-B-ligand geometry?

**Aggregation:** The $$n_{\text{protein}} \times n_{\text{ligand}}$$ pair representations are pooled (aggregated) into a fixed-size vector representing the complex as a whole.

**Two heads:**

*Binary head (binding likelihood):*
A classifier trained with **focal loss** to address class imbalance (binders are rare relative to random sequences). Outputs $$p(\text{binder})$$.

*Regression head (affinity value):*
Trained with **Huber loss** on two target types simultaneously:
- Absolute affinity values (log µM scale)
- Pairwise intra-assay differences (compound A vs. compound B within the same assay)

Pairwise differences are weighted more heavily because they're more reliable — absolute IC50 values vary across labs and assay conditions, but *relative rankings within a single assay* are consistent. This is why Boltz-2's affinity scores are better for ranking than for absolute prediction.

**Training is decoupled from the trunk.** During affinity training, gradients are *detached* from the trunk — the trunk's weights are frozen and only the affinity module is updated. This prevents the affinity loss from distorting the structural representations.

**Training data:**

| Dataset | Size | Type |
|---|---|---|
| ChEMBL + BindingDB | 1.2M binders, ~2,000 targets | Continuous Kd/IC50/Ki |
| PubChem HTS screens | 200K binders + 1.8M decoys | Binary (active/inactive) |
| MD ensembles (MISATO, ATLAS, mdCATH) | Large | Structural conformations |

Data cleaning: PAINS filters applied (removes assay-interfering compounds), ligands >50 heavy atoms excluded, synthetic decoys generated with Tanimoto similarity <0.3 to known binders.

All affinity measurements (Kd, Ki, IC50, AC50, EC50, XC50) are standardized to the same log µM scale and treated as proxies for binding strength within their assay context. **Do not compare Boltz-2 affinity scores across different assay types as if they were the same quantity.**

### 3.5 B-factor prediction and ensemble supervision

Boltz-2 trains the trunk's final single representation to predict **B-factors** — crystallographic temperature factors that indicate per-residue flexibility (higher B-factor = more mobile atom).

Why does this matter? B-factors are a proxy for conformational dynamics. By supervising the trunk on B-factors from both experimental structures and MD ensembles (MISATO, ATLAS, mdCATH), the trunk learns which parts of a protein are rigid vs. flexible. This is critical for:
- Predicting binding poses of flexible peptides
- Capturing the conformational diversity of intrinsically disordered regions
- Avoiding over-confident predictions at mobile loops

**Ensemble supervision:** Rather than training on one structure per protein-ligand complex, Boltz-2 trains on *distributions of structures* from MD simulations. The training target is an aggregated distogram (histogram of pairwise distances across the ensemble) rather than a single distance. This reduces variance and teaches the model that a given sequence doesn't fold to one structure — it samples from a conformational ensemble.

### 3.6 Confidence module

Produces pLDDT (per local distance difference test, 0–100) and PAE (predicted aligned error, in Ångströms). Both are critical for filtering candidates:

- **pLDDT at the interface** < 70 → the model is uncertain about binding pose, discard
- **High PAE between peptide and target** → the model doesn't know where the peptide sits relative to the protein, discard

These are not structure quality metrics — they are **model uncertainty** metrics. A high pLDDT means the model is confident, not that the structure is correct.

---

## Part 4: Chai-1 — Deep Dive

### 4.1 What Chai-1 is and isn't

Chai-1 is a structure prediction model — no affinity module. Its strength is breadth: it handles proteins, nucleic acids, small molecules, antibodies, and multimers in a single unified model, and it can take optional experimental constraints (crosslink MS, epitope maps) that directly guide the structure prediction. It does *not* predict binding affinity.

In the Agros pipeline, Chai-1 serves as an independent structural oracle — if both Boltz-2 and Chai-1 predict a similar complex geometry and both have high interface pLDDT, confidence in the prediction is substantially higher than if only one model agrees.

### 4.2 Architecture specifics

**Confirmed dimensions (published):**
- Pair representation: **128 channels**
- Single representation: **384 channels**
- PairFormer blocks: **48**, each with independent parameters

Same triangular attention + multiplicative update structure as described in Part 1.

**Language model embeddings.** Chai-1 uses a **3 billion parameter protein language model** to generate per-residue embeddings for each input sequence. These embeddings capture evolutionary and biochemical context from the training corpus (hundreds of millions of protein sequences). They are added to the single representation before the PairFormer trunk, giving the model a richer starting point than sequence one-hot encodings alone.

This is one of Chai-1's differentiating choices. Boltz-1 processes MSAs explicitly (aligning homologous sequences to extract co-evolutionary information). Chai-1 can use MSAs, but its language model embeddings provide a partial substitute — co-evolutionary information is distilled into the LM weights during pretraining.

### 4.3 MSA handling — optional, not required

This is Chai-1's most operationally important difference from Boltz-1/Boltz-2. **Chai-1 can run in single-sequence mode** with no MSA, relying on the 3B parameter LM embeddings alone. Quality degrades somewhat but remains competitive.

When MSAs are available, they are processed through an **MSA module** (similar to the Evoformer MSA stack from AlphaFold2) before being projected into the pair representation. The MSA provides direct co-evolutionary signals: if position 23 and position 89 always co-vary across homologs, they're likely in contact.

For peptides targeting insect proteins with few known homologs, MSAs may be sparse or unavailable. Chai-1 handles this more gracefully than Boltz-1.

### 4.4 Experimental constraint integration

Chai-1 can take *wet-lab data as direct model input*, not just as a post-hoc filter:

**Crosslink mass spectrometry (XL-MS):** XL-MS identifies pairs of residues that are close in space (within the crosslinker length, typically ~10–30Å). These distance constraints are encoded as additional pair features that bias the trunk's attention toward configurations consistent with the experimental crosslinks.

**Epitope mapping:** Known binding residues from antibody epitope assays are encoded as single-sequence features marking which residues are at the interface. The model learns to produce structures where those residues are accessible and at the surface.

**Pocket constraints:** Known binding pocket residues from mutagenesis or co-crystal structures can be specified and used to constrain docking poses.

When epitope data is incorporated, antibody-antigen prediction accuracy improves by **>10 percentage points** on DockQ (from ~60% to ~70%+). This is directly relevant to future Agros wet-lab integration — if we get mutagenesis data back from assays, it can be fed back into Chai-1 as epitope constraints to improve structural predictions.

### 4.5 Confidence scoring

Chai-1 produces:
- **pLDDT** per residue (same semantics as Boltz-2)
- **ipTM** (interface TM-score predicted) — specifically calibrated for the quality of the binding interface, not the whole complex. Chai-1 ranks its sampled structures by ipTM.

Multiple structures are sampled (stochastic diffusion) and ranked by ipTM — the top-ranked structure is returned. The diversity of sampled structures gives a rough sense of the energy landscape: if 5/5 samples agree on the interface geometry, the prediction is more reliable than if all 5 are different.

---

## Part 5: Protenix-v1 — Deep Dive

### 5.1 What it is

Protenix-v1 (PX-v1) is ByteDance's fully open-source reimplementation of the AF3 architecture, released February 2026. It is the first open model to match or exceed AF3 accuracy under identical constraints: same training data cutoff (September 30, 2021), same model scale (368M parameters), same inference budget.

The name means "Protein + X" — X covering DNA, RNA, small-molecule ligands, and ions. It handles all-atom 3D structure prediction for arbitrary multi-component complexes in a single model.

License: **Apache 2.0** for weights + training code + data pipeline — more permissive than Boltz-2 (MIT) and substantially more permissive than AlphaFold3 (non-commercial only). This is relevant for eventual commercialization.

|  | Protenix-v1 | Boltz-2 | Chai-1 | AlphaFold3 |
|---|---|---|---|---|
| **Parameters** | 368M | Not disclosed | Not disclosed | ~600M est. |
| **Affinity prediction** | No | Yes | No | No |
| **License** | Apache 2.0 | MIT | Non-commercial | Non-commercial |
| **Training data cutoff** | Sept 30, 2021 | Sept 30, 2021 | 2023 est. | Sept 30, 2021 |
| **Open training code** | Yes | No | No | No |

### 5.2 Architecture

Protenix-v1 is a faithful PyTorch re-implementation of AF3's **PairFormer + diffusion** framework (see Part 1 for the shared foundation). The core pipeline is identical:

1. Input embedding — sequence, MSA, templates, ligand features → initial pair and single representations
2. PairFormer trunk — 48 blocks of triangle multiplicative updates + triangle attention → refined $$\mathbf{Z}$$ (pair) and $$\mathbf{S}$$ (single)
3. Diffusion structure module — iterative denoising over all-atom coordinates conditioned on $$\mathbf{Z}$$
4. Confidence module — per-residue pLDDT + PAE

Key additions over a vanilla AF3 reimplementation:

**RNA MSA support.** Protenix-v1 processes MSAs for RNA chains, not just proteins. AF3 effectively treats RNA as a single-sequence input; PX-v1 extracts co-evolutionary signals for RNA, improving RNA structure and protein-RNA complex accuracy.

**Protein template integration with full multimer support.** Templates (homologous structures) can be provided for any chain in a multimer. The template features are projected into the pair representation before the trunk runs — same mechanism as AF3, but extended to handle heteromeric complexes with templates on multiple chains simultaneously.

**Inference-time scaling.** Accuracy improves log-linearly with the number of sampled candidates, particularly on antibody-antigen and other flexible-interface targets. This is not a model-architecture property — it reflects that the diffusion process is stochastic, and sampling more candidates increases the probability of hitting a near-native structure. The implication: for high-stakes predictions, budget 10–25 samples rather than 1.

### 5.3 Training

Protenix-v1 is the first open model to release the full training pipeline, not just inference:

- **Data pipeline**: PDB processing, clustering, MSA generation (identical splits to AF3 to enable fair benchmarking)
- **Training code**: PyTorch, fully reproducible from checkpoint
- **Distillation data**: Uses AF2-predicted structures for low-homology targets (same approach as AF3)

Because the training code is open, it is the most feasible model to fine-tune on ag-specific binding data post-funding. Boltz-2's fine-tuning requires working backward from a closed training stack; PX-v1's is directly accessible.

### 5.4 The PXMeter benchmark

The team released **PXMeter v1.0.0** alongside the model — a manually curated evaluation suite with:
- 6,000+ complexes held out past the training cutoff
- Time-split variants (to test generalization to new structures, not just new sequences)
- Domain-specific subsets: antibody-antigen, protein-RNA, protein-small-molecule
- Unified metrics: complex lDDT, DockQ

This is now the most rigorous public benchmark for structure prediction models. If the Agros paper includes structural validation, PXMeter subsets are worth citing as evaluation standards.

### 5.5 Supporting ecosystem

| Tool | Purpose | Relevance to Agros |
|---|---|---|
| **PXDesign** | Binder design; 20–73% experimental hit rates reported | Potential alternative to BoltzGen for generation |
| **Protenix-Dock** | Classical ligand docking integrated with PX-v1 structures | Physics-based docking baseline for paper benchmarking |
| **Protenix-Mini** | Compressed lightweight variants | Could run on A10G vs A100 — worth testing for cost reduction |

### 5.6 Relevance to Agros

**Not currently in the pipeline.** Protenix-v1 has no affinity module, so it cannot replace Boltz-2 as the ranking signal. Its role, if added, would be the same as Chai-1: an independent structural oracle for cross-validation.

**Why it's worth watching:**
1. Apache 2.0 license makes it the only realistic candidate for fine-tuning on proprietary ag-specific assay data post-funding
2. Protenix-Mini variants may offer a cheaper second-opinion check than Chai-1 on A100
3. PXDesign experimental hit rates (20–73%) are competitive with BoltzGen — worth benchmarking head-to-head once wet-lab data exists

**For the paper:** Protenix-v1 / AF3 parity is relevant context for situating Boltz-2's affinity module as the differentiating capability. The paper's IG analysis runs on Boltz-2 specifically because it has the affinity head — Protenix-v1 and Chai-1 are structural models only and cannot produce the affinity-attributed IG profiles the paper is built around.

---

## Part 6: How They Fit Together in the Agros Pipeline

```
Target PDB (nAChR, RyR, etc.)
           │
           ▼
   ┌──────────────────────────┐
   │        BoltzGen          │
   │  - Design spec: pocket,  │
   │    length, modality      │
   │  - Sample N=100–1000     │
   │    (seq, structure) pairs│
   └──────────────────────────┘
           │ candidates
           ▼
   ┌──────────────────────────┐
   │ BoltzIF (optional)       │  ← Redesign sequence on BoltzGen backbone
   └──────────────────────────┘
           │
           ▼
   ┌──────────────────────────┐
   │       Boltz-2            │
   │  - Refold + RMSD check   │  ← Discard if RMSD > threshold (BoltzGen
   │  - pLDDT / PAE filter    │    and Boltz-2 disagree on structure)
   │  - Affinity ranking      │
   └──────────────────────────┘
           │ top 10–20%
           ▼
   ┌──────────────────────────┐
   │       Chai-1             │  ← Independent structure check
   │  - Second pLDDT/ipTM     │    Filter if Chai-1 interface confidence
   │  - Agreement with Boltz-2│    is low or structure disagrees
   └──────────────────────────┘
           │ survivors
           ▼
   ┌──────────────────────────┐
   │   Downstream filters     │
   │  NetSolP, RDKit, OpenMM  │
   └──────────────────────────┘
           │
           ▼
        Lead candidates → wet-lab
```

### Critical implementation notes for Agros

**Attention extraction for the paper.** The trunk's pair representation $$\mathbf{Z}$$ is what you want for per-residue attention analysis. Specifically: the triangle attention layers produce attention weights $$\alpha_{ij}^{(l)}$$ at each layer $$l$$ — these are the "attention maps" that show which residue pairs the model focuses on when reasoning about structure. However, extracting these from Boltz-2 requires modifying the internal forward method of the PairFormer stack — `register_forward_hook` on standard PyTorch modules will likely miss the fused trifast kernel outputs. Read the Boltz-2 source code before assuming hooks work.

**Affinity scores are relative, not absolute.** Boltz-2's affinity head outputs a log µM proxy trained on heterogeneous assay data. A score of -8 does not mean $$K_d = 10^{-8}$$ M. Use scores only to rank candidates within a single run against the same target.

**Never fill in scores.** If Boltz-2 fails on a candidate (OOM, convergence failure, etc.), fix the integration or discard that candidate. Do not substitute random or average scores. The entire pipeline value depends on scores being real model outputs.

**Seed everything.** BoltzGen is stochastic — the same input spec with a different seed gives different candidates. Fix seeds before any design campaign and log them in the run manifest.

**Pin checkpoints by hash.** Both BoltzGen and Boltz-2 are under active development. Model weights from one release will produce different scores than weights from another release, breaking reproducibility. Store the SHA256 of the checkpoint file alongside every result.

---

## Sources

- [BoltzGen paper — PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12697729/)
- [BoltzGen paper — bioRxiv](https://www.biorxiv.org/content/10.1101/2025.11.20.689494v1.full)
- [Behind the Model: Hannes Stärk — Benchling](https://www.benchling.com/blog/behind-the-model-building-boltzgen-with-hannes-staerk)
- [BoltzGen GitHub](https://github.com/HannesStark/boltzgen)
- [Boltz-2 — PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12262699/)
- [Boltz-1 — PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11601547/)
- [Chai-1 technical report](https://chaiassets.com/chai-1/paper/technical_report_v1.pdf)
- [Chai-1 GitHub](https://github.com/chaidiscovery/chai-lab)
- [Triangular self-attention explained](https://medium.com/@ding.zhongqiang/triangular-self-attention-with-alphafold3-51e6d9c8575b)
- [AF3 / Boltz-1 / Chai-1 comparison](https://medium.com/@punta.indratomo/comparison-of-alphafold-3-boltz-1-and-chai-1-9bc818f4efa0)
