# Protecting Peptides Against the Elements

Peptides are attractive crop-protection actives because they can be specific, biodegradable, and designable. The tradeoff is field stability: sunlight, rain, heat, leaf-surface chemistry, and proteases can destroy or remove the active before it reaches the pathogen.

The Agros answer is not "our peptides magically ignore the field." The answer is: we separate **active discovery** from **field protection**, then use formulation and sequence engineering to make the lead peptide survive long enough to work.

---

## The field threats

| Threat | What can go wrong | How we protect against it |
|--------|-------------------|---------------------------|
| UV / sunlight | Aromatic residues and exposed backbones can photodegrade on leaf surfaces | UV blockers, encapsulation, evening application, residue-level redesign where binding does not depend on UV-sensitive residues |
| Rain / irrigation | Active washes off before infection window | Sticker-spreader adjuvants, wax/cuticle-binding formulation, microencapsulation, spray timing around disease-risk windows |
| Proteases | Plant, fungal, and environmental proteases cleave the peptide | D-amino acid substitution, N-terminal acetylation, C-terminal amidation, cyclization, N-methylation, protease-site redesign |
| Heat / pH | Peptide unfolds, aggregates, or hydrolyzes during storage or field exposure | Buffering, lyophilized concentrate, stabilizing excipients, cold-chain only if absolutely needed |
| Leaf-surface retention | Active dries, crystallizes, or fails to stay near the infection site | Humectants, surfactants, encapsulation, formulation tuned to grape/strawberry/tomato leaf surfaces |
| Non-target exposure | Broadly membrane-active peptides can harm beneficial organisms | Selectivity screens, dose control, target-specific design, formulation that limits environmental spread |

---

## The core strategy

### 1. First prove biological activity

The first gate is still in vitro potency: does the peptide inhibit Botrytis growth or the target enzyme at a commercially plausible concentration?

This is the right first question because formulation cannot rescue an inactive molecule. A weak peptide with great field stability is still not a pesticide.

**Near-term success metric:** MIC / EC50 against Botrytis in the low µM range, plus a clean selectivity screen.

### 2. Protect the molecule with formulation

Most field-stability problems are formulation problems. The active ingredient does not need to survive naked on a leaf for days. It needs to survive long enough, at the right place, during the infection-risk window.

Likely formulation levers:
- **Microencapsulation:** polymer, lipid, or clay-based particles that shield the peptide from UV and slow release onto the leaf surface.
- **Sticker-spreader adjuvants:** improve leaf adhesion and reduce washoff after rain or irrigation.
- **UV protectants:** inert formulation components that absorb or scatter UV before it reaches the peptide.
- **Humectants:** keep the active hydrated long enough to diffuse into the pathogen microenvironment.
- **Controlled-release carriers:** release peptide over hours to days instead of all at once.

**YC phrasing:** "We do not need the peptide to be indestructible. We need enough active peptide present during the disease window to beat the pathogen."

### 3. Engineer stability without losing binding

This is where Agros has a platform advantage. Integrated Gradients tells us which residues drive binding. We protect those residues and modify the rest.

Residue-level protection strategies:
- **D-amino acids at non-binding positions:** most proteases recognize L-amino acid backbones, so D substitutions can sharply reduce cleavage.
- **Terminal capping:** N-terminal acetylation and C-terminal amidation reduce exopeptidase attack.
- **Cyclization:** head-to-tail or side-chain cyclization removes vulnerable termini and can improve thermal/protease stability.
- **N-methylation:** blocks backbone protease access at selected positions.
- **Aromatic residue management:** if Trp/Tyr/Phe are not binding-critical, replace or shield them to reduce photodegradation risk.
- **Charge tuning:** improve leaf retention, solubility, and membrane selectivity without touching the pharmacophore.

The key is that we are not blindly modifying the peptide. We use IG to separate "binding-critical" residues from "stability-engineering" residues.

---

## How this fits into the pipeline

### Current pipeline

1. Generate peptide candidates against the target.
2. Score binding and structure.
3. Use IG to identify binding-critical residues.
4. Filter for solubility and obvious liabilities.
5. Synthesize and run in vitro assays.

### Next stability layer

After the first validated hit, add:

| Stability gate | Test | Decision |
|----------------|------|----------|
| Protease challenge | Incubate peptide with plant/fungal protease mix; LC-MS or activity readout over time | Modify vulnerable non-binding residues |
| UV exposure | Expose dried peptide film or formulated spray deposit to simulated sunlight | Add UV protectant or redesign sensitive residues |
| Rainfastness | Spray leaf discs, simulate rainfall, measure remaining activity | Add sticker-spreader or encapsulation |
| Leaf-surface half-life | Apply to grape/strawberry/tomato leaves; measure active over 0, 2, 6, 24 hours | Choose formulation and application timing |
| Greenhouse efficacy | Spray treated plants, inoculate with Botrytis, measure lesion suppression | Advance to field trial or reformulate |

**Important:** these are not all required before YC. They are the post-hit development plan that turns an active peptide into a field product.

---

## Why this is credible

Peptide and protein crop-protection products already face these same constraints. The field does not require impossible stability; it requires a formulation that delivers an effective dose during a defined window.

Agros has three advantages:

1. **Short peptides are engineerable.** A 10-30 residue peptide can be capped, cyclized, substituted, or encapsulated faster than a large protein can be re-engineered.
2. **The target use case is foliar.** Botrytis lives on above-ground plant tissue, so we can start with topical spray delivery instead of solving systemic plant uptake.
3. **IG preserves the pharmacophore.** We can improve stability at low-attribution positions while preserving the residues that drive binding.

---

## What not to overclaim

Do not say:
- "Our peptides are field-stable already."
- "Biodegradable means no formulation problem."
- "D-amino acids solve everything."
- "We can model all field conditions before testing."

Say instead:

> "Our current pipeline proves target binding and in vitro activity first. Field stability is the next engineering layer: formulation plus residue-level stabilization. The reason Agros is well-suited to this is that IG tells us which residues are essential for binding, so we know where we can safely modify the peptide for UV, protease, and rainfastness without destroying activity."

---

## Interview answer

> "Peptides do face UV, protease, and washoff problems in the field. We handle that in two layers. First, formulation: encapsulation, sticker-spreader adjuvants, UV protectants, and controlled release. Second, sequence engineering: once IG tells us which residues drive binding, we can modify non-critical positions with D-amino acids, terminal caps, cyclization, or N-methylation to improve stability. Our first milestone is in vitro activity; the next milestone is showing that a formulated lead maintains activity on leaves under greenhouse conditions."

