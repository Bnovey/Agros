# Target Overview — Pesticide-Resistant Insects
*For BoltzGen peptide binder design. 20 targets across Plutella xylostella (Px), Spodoptera frugiperda (Sf), Bemisia tabaci (Bt).*

---

## Priority Key
- **Structural data:** PDB = crystal/cryo-EM structure exists; AF = AlphaFold model only
- **Selectivity:** ✓ = no mammalian ortholog or clearly divergent binding site; ~ = ortholog exists but insect-specific residues known
- **Druggability:** accessibility of a discrete binding pocket or PPI interface for a peptide binder

---

## Category 1 — Nicotinic Acetylcholine Receptors (nAChR)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 1 | nAChR α6 (Pxα6) | *P. xylostella* | AF | ~ | Spinosad target; resistance mutations cluster at extracellular agonist-binding interface — ideal site for competing peptide |
| 2 | nAChR α6 (Sfα6) | *S. frugiperda* | AF | ~ | CRISPR knockout confers >300× spinosad resistance; extracellular ECD is accessible |
| 3 | nAChR β1 (BTβ1) | *B. tabaci* | AF | ~ | Dual A58T+R79E mutations cause cross-resistance to all neonicotinoids; orthosteric site accessible |

**Notes for design:** No insect nAChR crystal structure for these specific subunits. Use mammalian nAChR ECD structures (PDB: 2QC1, 7QLH) as homology templates. Target the extracellular agonist-binding domain (AChBP-like interface between α and β subunits). Resistance mutations are a useful guide to the binding site geometry.

---

## Category 2 — Ryanodine Receptor (RyR)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 4 | PxRyR | *P. xylostella* | **PDB: 5Y9V** | ✓ | Crystal structure available; two insect-specific pockets not found in mammalian RyR identified crystallographically |
| 5 | SfRyR | *S. frugiperda* | Cryo-EM 2024 | ✓ | Cryo-EM structure with bound diamides now available (Jiang 2024 Nature Comms); resistance mutations (I4734M/K) localized in transmembrane gating region |

**Notes for design:** PxRyR is the strongest target in this list — only Px-specific PDB entry available. Jiang 2024 provides the diamide-bound cryo-EM structure, which directly shows the binding pocket geometry. Design peptides to occlude or allosterically disrupt the channel-gating domain. Insect-selective pockets make this intrinsically safe.

---

## Category 3 — Voltage-Gated Sodium Channel (Nav / para)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 6 | Nav/para (PxNav) | *P. xylostella* | AF | ~ | Pyrethroid and SCB target; multiple resistance mutations mapped in transmembrane domains; extracellular loops accessible |
| 7 | Nav/para (SfNav) | *S. frugiperda* | AF | ~ | T929I, L932F, L1014F kdr mutations mapped; extracellular domain loops present peptide-binding surface |

**Notes for design:** Nav channels are well-validated insecticide targets but have mammalian orthologs. Focus peptide design on insect-specific extracellular loop regions (Domain II-IV) or use resistance-site-adjacent surfaces that differ from mammalian channels. AF models are available; pyrethroid-bound insect Nav cryo-EM structures can be used as templates.

---

## Category 4 — GABA-Gated Chloride Channel (Rdl)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 8 | PxRdl1 / PxRdl2 | *P. xylostella* | AF | ~ | Px uniquely expresses two Rdl paralogs with distinct fipronil sensitivity (40× difference); two-gene system creates selectivity opportunity |
| 9 | SfRdl1 | *S. frugiperda* | AF | ~ | Highly sensitive to fluralaner/fipronil (IC50 ~5 nM); extracellular GABA-binding domain and pore-lining TM segments are druggable |

**Notes for design:** No insect Rdl crystal structure. Use GABA-A receptor cryo-EM (PDB: 6HUG) as template. Target the extracellular agonist-binding domain (GABA site) or the transmembrane pore entrance (picrotoxin/fipronil site). The dual-paralog system in Px is a unique differentiator.

---

## Category 5 — Acetylcholinesterase (AChE1)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 10 | PxAChE1 (ace1) | *P. xylostella* | AF + homology | ~ | Primary catalytic AChE; active-site gorge and peripheral anionic site accessible; resistance mutations (A298S, G324A) structurally characterized |
| 11 | BtAChE1 | *B. tabaci* | AF + homology | ~ | Unique Y390N and F392W mutations in active-site backdoor; insect-specific structural divergence from vertebrate AChE |

**Notes for design:** Use *Torpedo californica* AChE (PDB: 1ACJ) as homology template. Target peripheral anionic site (PAS) entrance for allosteric peptide binders — this avoids the highly conserved catalytic triad while still disrupting activity. Bt has unique backdoor mutations that differentiate it from vertebrate AChE.

---

## Category 6 — Octopamine Receptors (insect-selective GPCRs)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 12 | PxOA2B1 / PxOctβ1 | *P. xylostella* | AF | ✓ | β-adrenergic-like; no vertebrate ortholog; EC50 ~49.5 nM; 7TM extracellular loops accessible; antagonists already validated |
| 13 | Octopamine/NPY GPCRs | *B. tabaci* | AF | ✓ | 35 neuropeptide receptors genome-wide; octopamine GPCRs present with no vertebrate counterparts; monoterpenes exploit this site |

**Notes for design:** Octopamine receptors are the most insect-selective targets on this list — no vertebrate ortholog exists. Use β2AR (PDB: 2RH1) as GPCR template for homology modeling. Target orthosteric binding pocket in extracellular loops 2/3 (EL2/EL3) and the vestibule entrance. Note: PxOctβ3 (Zhu 2022) is already documented separately.

---

## Category 7 — Juvenile Hormone Receptor (Met)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 14 | Met / Methoprene-tolerant (SfMet) | *S. frugiperda* | AF + Dm crystal | ✓ | bHLH-PAS transcription factor; PAS-B ligand-binding domain has defined hydrophobic pocket; disruption blocks metamorphosis |

**Notes for design:** Crystal structure of *Drosophila* Met PAS-B available (PNAS 2011). SfMet AlphaFold model available. Design cyclic/stapled peptides that mimic JH III binding or block the Met-Taiman PPI interface. Insect-selective target — JH pathway is absent in vertebrates.

---

## Category 8 — Ecdysone Receptor (EcR)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 15 | PxEcR / PxUSP | *P. xylostella* | AF + 1R0O | ✓ | Nuclear receptor; LBD binds ponasterone A (Kd ~3 nM); EcR/USP dimer interface is a PPI target for stapled peptides |

**Notes for design:** EcR-USP DBD structure available (PDB: 1R0O). Design peptides that compete with ecdysone binding in the LBD pocket or disrupt the EcR/USP heterodimerization interface. Invertebrate-selective — vertebrate equivalents (RXR/RAR) are structurally divergent in the binding pocket.

---

## Category 9 — Chitin Synthase (CHS)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 16 | PxCHS1 | *P. xylostella* | AF | ✓ | Absent in vertebrates/plants; cuticle-expressed; RNAi knockdown lethal; AlphaFold-assisted inhibitor design published 2025 |
| 17 | BtCHS2 | *B. tabaci* | AF | ✓ | Midgut-specific (peritrophic matrix); RNAi achieves ~80% nymph mortality at 48h; no vertebrate ortholog |

**Notes for design:** CHS is the most inherently selective target — no vertebrate homolog. AF model used for drug design in Li 2025 (JAFC) — use that structural model directly. Target the glycosyltransferase active site pocket. BtCHS2 is particularly attractive due to gut-specific expression (natural delivery route for oral peptides).

---

## Category 10 — V-ATPase Subunits

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 18 | V-ATPase subunit E (PxvATPaseE) | *P. xylostella* | AF + 7SIF | ~ | Midgut pH homeostasis; RNAi causes dose-dependent larval mortality; V1 domain presents insect-specific PPI interfaces |

**Notes for design:** Mammalian V-ATPase cryo-EM (PDB: 7SIF) provides structural template. Target the V1 domain subunit-interface rather than the catalytic site — insect V-ATPase assembly has diverged from the mammalian proton pump at inter-subunit contacts.

---

## Category 11 — Neuropeptide Receptors

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 19 | sNPF receptor (SfsNPFR) | *S. frugiperda* | AF | ✓ | Regulates feeding, JH III synthesis, larval development; sNPF analog (III-2) shows insecticidal activity (LC50 18.7 mg/L); no direct mammalian ortholog |

**Notes for design:** Class A GPCR; design peptide mimics of sNPF that act as antagonists at the extracellular binding site. The short neuropeptide F signaling axis is insect-specific, making this intrinsically safe.

---

## Category 12 — Insulin Receptor (InR)

| # | Target | Species | Structure | Selectivity | Why it's a good binder target |
|---|--------|---------|-----------|-------------|-------------------------------|
| 20 | Insulin receptor (SfInR) | *S. frugiperda* | AF + 2DTG | ~ | IIS/TOR controls fecundity and ovarian development; RNAi reduces fecundity dramatically; extracellular L1-CR-L2 module is solvent-exposed with insect-specific sequences in cysteine-rich region |

**Notes for design:** Human InR ectodomain (PDB: 2DTG, 3LOH) provides template but insect InR L2 and fibronectin domains show meaningful divergence — selectivity surface for peptide design. Target the L1 domain binding interface (insulin-binding site) using insect-divergent residues as specificity handles.

---

## Summary Table

| # | Target | Gene | Species | Structure | Category |
|---|--------|------|---------|-----------|----------|
| 1 | nAChR α6 | Pxα6 | *P. xylostella* | AF | nAChR |
| 2 | nAChR α6 | Sfα6 | *S. frugiperda* | AF | nAChR |
| 3 | nAChR β1 | BTβ1 | *B. tabaci* | AF | nAChR |
| 4 | Ryanodine receptor | PxRyR | *P. xylostella* | **PDB: 5Y9V** | RyR |
| 5 | Ryanodine receptor | SfRyR | *S. frugiperda* | Cryo-EM 2024 | RyR |
| 6 | Voltage-gated Na channel | PxNav | *P. xylostella* | AF | Nav |
| 7 | Voltage-gated Na channel | SfNav | *S. frugiperda* | AF | Nav |
| 8 | GABA-Cl channel | PxRdl1/2 | *P. xylostella* | AF | Rdl |
| 9 | GABA-Cl channel | SfRdl1 | *S. frugiperda* | AF | Rdl |
| 10 | Acetylcholinesterase 1 | PxAChE1 | *P. xylostella* | AF + homology | AChE |
| 11 | Acetylcholinesterase 1 | BtAChE1 | *B. tabaci* | AF + homology | AChE |
| 12 | Octopamine receptor β1 | PxOA2B1 | *P. xylostella* | AF | OAR |
| 13 | Octopamine/NP GPCRs | BtGPCRs | *B. tabaci* | AF | OAR |
| 14 | JH receptor (Met) | SfMet | *S. frugiperda* | AF + Dm crystal | Met |
| 15 | Ecdysone receptor | PxEcR/PxUSP | *P. xylostella* | AF + 1R0O | EcR |
| 16 | Chitin synthase 1 | PxCHS1 | *P. xylostella* | AF | CHS |
| 17 | Chitin synthase 2 | BtCHS2 | *B. tabaci* | AF | CHS |
| 18 | V-ATPase subunit E | PxvATPaseE | *P. xylostella* | AF + 7SIF | V-ATPase |
| 19 | sNPF receptor | SfsNPFR | *S. frugiperda* | AF | NP-GPCR |
| 20 | Insulin receptor | SfInR | *S. frugiperda* | AF + 2DTG | InR |

---

## Recommendations for First BoltzGen Campaign

**Best immediate targets (structural data + clear binding site):**
1. **PxRyR** (Target 4) — only Px-specific PDB entry; insect-specific pockets identified; diamide-bound cryo-EM now available
2. **PxOctβ1/PxOA2B1** (Target 12) — no vertebrate ortholog, pharmacologically validated, AF model available
3. **PxnAChRα6** (Target 1) — spinosad resistance mutations define the binding interface geometry

**Most insect-selective (lowest safety risk):**
Octopamine receptors (12, 13), chitin synthases (16, 17), sNPFR (19), Met (14), EcR (15)

**Best validated by resistance genetics (ground truth for attention maps):**
Targets 1, 2, 3, 4, 5, 6, 10, 11 — all have field-resistance mutations that localize the binding site

---
*v1 — 2026-05-06*
