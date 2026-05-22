# Q: Peptide Field Stability

> "Peptides have notoriously bad field stability — UV degradation, protease cleavage, leaf surface washoff. Has any of your pipeline modeled that, or are you optimizing for an assay that doesn't predict the field?"

This is a real and sharp question. The honest answer is: our current pipeline optimizes for binding affinity and in vitro growth inhibition, not field stability. Here is how to answer it without being caught flat-footed.

---

## The honest state of the pipeline

Our current filters (NetSolP, RDKit, OpenMM) screen for:
- Aqueous solubility
- Structural liabilities (reactive groups, known degradation handles)
- Binding pocket stability under MD simulation

**What we are not yet modeling:**
- UV photodegradation half-life on a leaf surface
- Protease stability against plant/fungal/insect extracellular proteases
- Rainfastness or cuticle retention
- Thermal stability under field temperature fluctuations

This is a known gap. In vitro MIC data tells us the peptide kills the pathogen in a buffered aqueous environment. It does not tell us how much active peptide reaches the pathogen after 6 hours on a grape leaf in Napa in July.

---

## Why this isn't fatal at our stage — and how to say so

**1. This is a formulation problem, not a discovery problem.**

Every biopesticide — proteins, peptides, RNA, even Bt toxin — faces this challenge. The solution is formulation engineering: microencapsulation, UV protectants, adjuvants that improve cuticle adhesion, and sticker/spreader additives. These are standard tools that CROs and formulation partners (e.g., Adjuvant International, Nufarm, Brandt) apply after the active is identified. We are not expected to solve formulation at the discovery stage.

The precedent: Bt toxin (a protein ~130 kDa) is notoriously UV-labile — it degrades on a leaf surface within hours. The solution was microencapsulation and encapsulation into the plant genome (Bt crops). Our peptides at 10–30 residues are smaller and potentially more formulation-flexible, not less.

**2. Our in vitro assay is the right first gate, not the only gate.**

The YC batch goal is in vitro MIC data — proof that our peptides have the right mechanism and potency. Field stability is the next validation layer, not the first. The staged development path is:

```
In vitro MIC (now) → greenhouse spray trial (12 months) → field trial (24 months)
```

The greenhouse trial, done under controlled conditions with standardized application, is where we first encounter real-world degradation. That data informs formulation choices before the field trial.

**3. Peptide stability is designable.**

This is where our platform has a future advantage. IG attribution tells us which residues are critical for binding. Residues that are NOT critical can be freely modified for stability — including:
- **D-amino acid substitution** at protease-susceptible positions (proteases are stereospecific; D-amino acids are essentially invisible to most plant/fungal proteases)
- **N-methylation** of backbone amides to block protease cleavage
- **Cyclization** (head-to-tail or side-chain) to eliminate termini that are primary protease targets and improve UV resistance
- **PEGylation** of non-binding residues to extend half-life on the leaf surface

ProteinMPNN (Stage 4 of our pipeline) redesigns low-attribution positions — stability modifications fit directly into this step. We can add a stability filter in Stage 5 once we have a validated hit. The pipeline is designed to support this; we just haven't needed it yet.

**4. Commercial precedent: peptide biopesticides already work in the field.**

Plant Health Care's Obrona® (harpin peptide) is registered and sold commercially in the US. Vestaron's Spear® (GS-omega/kappa-Hxtx-Hv1a, a spider venom peptide) is registered for use on vegetables and ornamentals. These are peptides surviving real-world field application. The stability problem is solved at the formulation level — it's not a fundamental barrier to the class.

---

## What to add to the pipeline (near-term, before Series A)

To close this gap before investors or licensing partners push on it:

| Addition | What it adds | When |
|----------|-------------|------|
| Protease stability screen (in silico) | Flag cleavage sites using PeptideCutter or similar; score candidates on predicted protease resistance | Stage 5, after hit confirmation |
| Photostability literature lookup | Check if residue composition predicts UV sensitivity (Trp, Tyr are primary chromophores) | Stage 5 |
| Greenhouse spray trial protocol | Real-world stability data under controlled conditions; data for licensing package | Month 7–12 post-YC |
| Formulation partner engagement | Partner with a formulation CRO to test microencapsulation options for top 2–3 hits | Month 12+ |

---

## One-line answer for the interview

> "You're right that it's a gap in the current pipeline — we're optimizing for binding and in vitro activity first. Field stability is a formulation engineering problem, not a discovery problem, and it's the next layer of validation after we have confirmed hits. Our IG pipeline already identifies which residues are dispensable for binding, which is exactly where you make D-amino acid substitutions or cyclization modifications to improve protease resistance. The commercial precedents — Vestaron's spider peptide, Plant Health Care's harpin peptides — show this is solvable at the formulation stage."
