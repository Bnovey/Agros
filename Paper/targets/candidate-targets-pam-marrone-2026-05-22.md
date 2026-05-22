# Candidate Target Research — Post Pam Marrone Call

**Date:** 2026-05-22
**Trigger:** Call with Pam Marrone (pam@invasivespeciescorporation.com) — she flagged three high-potential target areas. See [[Company/meetings/2026-05-22-pam-marrone]].
**Note:** Pam explicitly said do NOT lead with Botrytis cinerea — crowded market, growers discover it post-infection.

---

## 1. Fusarium — Myosin-I (FgMyo1)

### Pathogen
- *Fusarium graminearum* (best structural data; head blight in cereals)
- *Fusarium oxysporum* f. sp. *cepae* / *lycopersici* / *vasinfectum* — the commercial targets (onion, tomato, cotton)

### Molecular Target: Class I Myosin (FgMyo1)

| Property | Detail |
| :--- | :--- |
| Function | Motor protein for hyphal tip growth, endocytosis, cytoskeletal organization. Required for infection establishment. |
| PDB structure | **6UI4** — 2.65 Å, phenamacril-bound *F. graminearum* myosin-I (available now, use for Boltz-2) |
| Validated inhibitor | Phenamacril (JS399-19) — registered fungicide in China; binds allosteric pocket at S217 in 50 kDa cleft |
| Mutagenesis data | Extensive resistance mutation panel published; binding-site residues fully characterized |
| Mammalian selectivity | High — the selectivity-conferring residue is unique to susceptible Fusarium spp.; confirmed little/no inhibition of human myosin-1c |
| Existing peptide inhibitor | None — Agros would be first |
| Gap in *F. oxysporum* | V151A/S418T substitutions reduce phenamacril sensitivity → opportunity to design peptides that address the oxysporum gap |

### Market / Urgency

- FBR causes up to 50% yield loss in onion fields; 30–40% post-harvest storage loss
- *F. oxysporum* infects >1,000 plant species across 586 genera
- Current biologicals (*Trichoderma*, *Bacillus*) are not targeted inhibitors — this gap is exactly what Pam called "underserved"

### Fit Assessment

**Excellent.** Crystal structure available (6UI4), rich mutagenesis data for attention-map validation, no existing peptide competitor, clear selectivity, Pam's explicit market endorsement. **Recommend replacing Botrytis as primary fungal target in the paper.**

Risk: *F. oxysporum* has slightly different binding-site residues vs. *F. graminearum*. Would need to model or obtain the Fo myosin structure for commercial targets.

---

## 2. Downy Mildew (Grape) — Cellulose Synthase (PvCesA3)

### Pathogen
**Plasmopara viticola** — oomycete (not true fungus). Causal agent of grapevine downy mildew.

### Molecular Target: Cellulose Synthase (CesA3)

| Property | Detail |
| :--- | :--- |
| Function | Essential oomycete cell wall biosynthesis enzyme. CAA fungicide class (mandipropamid) validated against this target. |
| PDB structure | None deposited. AlphaFold model available; use with caution for Boltz-2 scoring. |
| Validated inhibitor | Mandipropamid (Revus, Syngenta) — registered commercial fungicide |
| Mutagenesis data | G1105S or G1105V confers full CAA resistance — binding site well-characterized |
| Mammalian selectivity | High — oomycete CesA has no mammalian ortholog |
| Existing peptide inhibitor | **NoPv1** (RLTAQCRL-NH₂, 8 residues) against PvCesA2 — inhibits sporulation at 100 μM, 60% efficacy in greenhouse. *Scientific Reports* 2020, follow-up *IJMS* 2024. Cross-inhibits *Phytophthora infestans* (CesA 97% identity). |

### Market / Urgency

- *P. viticola* can destroy 40–90% of a vineyard in a single season
- European growers apply 8–9 sprays/season for downy mildew alone; resistance to CAAs spreading
- Organic viticulture is growing fast; copper-based solutions are under regulatory pressure in the EU
- Direct market comp: no peptide product registered or in pipeline

### Fit Assessment

**Moderate-to-strong.** NoPv1 is a compelling proof-of-concept to cite. Mutagenesis data is solid for paper validation. Main risk: no experimental crystal structure means AlphaFold-only scoring, which may limit Boltz-2 reliability. Good second target — include in paper, develop commercial case with more structural data.

---

## 3. Sucking & Chewing Insects — Kinin Receptor (GPCR)

### Pest Species (primary)
- *Myzus persicae* (green peach aphid) — model species, best published neuropeptide data
- *Bemisia tabaci* (silverleaf whitefly) — tomato, cotton, vegetables
- Lepidoptera (caterpillars) — neonicotinoid resistance driving acute need

### Molecular Target: Insect Kinin Receptor (Class A GPCR)

| Property | Detail |
| :--- | :--- |
| Function | GPCR regulating osmoregulation, hindgut contraction, diuresis, muscle contraction, digestive enzyme secretion |
| PDB structure | None for insect kinin GPCR. Human NPY Y1 (PDB 5ZBQ, 5ZBH) usable as homology model template. |
| Endogenous ligand motif | Phe-X₁-X₂-Trp-Gly-NH₂ — arthropod/invertebrate-specific; no mammalian ortholog |
| Mutagenesis/SAR data | Published: kinin analogues L02 and R01 (2025, *JAFC*) against *M. persicae* kinin GPCR; Ser2 critical pharmacophore |
| Mammalian selectivity | High — insect kinin circuit is arthropod-specific; human NK receptor ligands confirmed inactive |
| Existing commercial product | None registered |

### Market / Urgency

- Aphids alone cause ~$1.5B/year in US losses (feeding + virus transmission)
- Whitefly-transmitted viruses: billions in losses globally
- Neonicotinoid resistance crisis is the primary commercial driver

### Competitive Risk: Solasta Bio

- Platform: insect neuropeptide analogues ("micro-peptides")
- Targets: aphids, whitefly, lepidoptera — exactly this space
- Regulatory: EPA "biochemical-like" classification secured March 2025
- IP: 14 granted patents (8 US, 6 EU)
- Funding: $14M Series A closed December 2024
- Timeline: market entry 2027–2028
- Key differentiation gap: Solasta uses natural sequence analogue optimization; Agros uses generative AI design against any specified structure. Platform angle is the differentiator.

### Fit Assessment

**Strong mechanistically; competitive risk from Solasta.** Include in paper as "insect target" to demonstrate AI platform vs. biology-first discovery. Commercial strategy should identify a specific receptor subtype or pest where Solasta is not patented. Do not pitch "neuropeptide biopesticide" as a novel concept — pitch "AI-designed peptides that outperform what biology alone can find."

---

## Summary

| Target | Pathogen | PDB | Mutagenesis | Peptide Precedent | Competition | Platform Fit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| FgMyo1 | *F. graminearum / oxysporum* | **6UI4 (excellent)** | Excellent | None yet | Weak (*Trichoderma*, no peptides) | **Excellent** |
| PvCesA3 | *P. viticola* | AlphaFold only | Good (G1105S/V) | NoPv1 (CesA2, 100 μM) | Moderate (copper, elicitors) | Moderate-strong |
| Kinin GPCR | *M. persicae*, whitefly, lepidoptera | AlphaFold (NPY homology) | Good (SAR data) | Kinin analogues L02/R01 | **Solasta Bio (direct)** | Strong mechanistically |

**Recommended paper target set:**
1. FgMyo1 (*F. graminearum*) — replace Botrytis as primary fungal ag target
2. *P. viticola* CesA3 — secondary ag fungal/oomycete target
3. Kinin GPCR (*M. persicae*) — insect target, differentiate on AI platform vs. Solasta

---

## Action Items (from this research)

- [ ] Log decision to replace Botrytis with Fusarium myosin-I in `Paper/decisions.md`
- [ ] Download PDB 6UI4 to `data/targets/`
- [ ] Obtain or generate AlphaFold model for *F. oxysporum* myosin (Fo myosin-I)
- [ ] Evaluate AlphaFold quality for PvCesA3 before committing to downy mildew target
- [ ] Identify which Solasta Bio patents cover kinin receptor subtypes — define non-overlapping space
- [ ] Ask Pam to clarify "Salasta" reference — likely Solasta Bio (confirm)

---

## Sources

- PDB 6UI4: phenamacril-bound *F. graminearum* myosin-I (2.65 Å). PMC7100991.
- Phenamacril resistance mutations: PMC7469408.
- NoPv1 peptide: PMC7567880 (*Sci Reports* 2020); mechanism follow-up PMC11121460 (*IJMS* 2024).
- PvCesA3 resistance (G1105S/V): PubMed 20226261.
- Kinin analogues L02/R01 vs. *M. persicae*: PMC12512177 (*JAFC* 2025).
- Insect neuropeptide review: Springer 10.1186/s40659-025-00651-0.
- Solasta Bio Series A + EPA classification: BioSpace (Dec 2024), Scottish Financial Review (Mar 2025).
