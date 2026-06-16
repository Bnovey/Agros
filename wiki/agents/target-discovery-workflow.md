# Agent Workflow: Target Organism & Target Protein Discovery

**Purpose:** Systematic discovery and validation pipeline for identifying high-value agricultural pest organisms and their corresponding molecular targets (proteins/receptors) for pesticide / biopesticide development.

**Scope:** Insect pests (lepidoptera, sucking insects, whitefly) and fungal pathogens (Fusarium, oomycetes, Botrytis) — matching Agros' current pipeline focus.

**Date created:** 2026-06-15
**Status:** active

---

## Table of Contents

1. [Workflow Overview](#workflow-overview)
2. [Phase 1: Target Organism Identification](#phase-1-target-organism-identification)
3. [Phase 2: Target Protein/Receptor Identification](#phase-2-target-proteinreceptor-identification)
4. [Phase 3: Target Validation & Prioritization](#phase-3-target-validation--prioritization)
5. [Phase 4: Design Readiness Assessment](#phase-4-design-readiness-assessment)
6. [Agent Instructions by Role](#agent-instructions-by-role)
7. [Templates & Checklists](#templates--checklists)
8. [See Also](#see-also)

---

## Workflow Overview

```
Phase 1: Identify Target Organism
    |
    v
Phase 2: Identify Molecular Targets
    |
    v
Phase 3: Validate & Prioritize
    |
    v
Phase 4: Assess Design Readiness
    |
    v
Handoff to Design Pipeline (BoltzGen, AlphaFold, etc.)
```

Each phase is designed to be executed by one or more agents (see [Agent Instructions by Role](#agent-instructions-by-role)). Decisions at each phase are logged to `Paper/decisions.md`.

---

## Phase 1: Target Organism Identification

### 1.1 Market & Economic Impact Assessment

| Question | Data Source | Decision Threshold |
|---|---|---|
| What is the annual yield loss USD? | USDA, FAO, peer-reviewed crop loss studies | > $500M/year for crop category |
| What is the geographic spread? | IRAC/FRAC resistance maps, regional pest surveys | Multi-region or spreading |
| Is resistance to existing products documented? | IRAC MoA working group reports, FRAC | Resistance to >2 MoA classes |
| Regulatory pressure on existing products? | EPA, EFSA, national regulator databases neonicotinoid bans | Banned or restricted in major markets |
| Unmet need for biological or peptide alternative? | Grower surveys, investor diligence, company calls | Confirmed gap; no peptide competitor |

### 1.2 Organism Selection Criteria

**Must-haves (dealbreakers):**
- [ ] Genome sequenced (or closely related species with high-quality assembly)
- [ ] Functional genetics validated (RNAi, CRISPR, or classical genetics show target is essential)
- [ ] Clear economic impact document
- [ ] No trivial biological control alternative exists
- [ ] Feasible delivery route for the product type (e.g., oral for gut targets, contact for cuticle targets)

**Must-not-haves (exclude):**
- [ ] Natural enemy / beneficial overlap (kills pollinators, parasitoids, etc.)
- [ ] Entirely soluble in existing chemistry (no unmet need)
- [ ] Infeasible biology (e.g., target organism cannot be cultured, no viable assay)

### 1.3 Organism Triage Decision Tree

```
Economic Impact > $500M/year?
    ├── No → Deprioritize / exclude
    └── Yes → Genome available?
            ├── No → Flag for sequencing / use close relative
            └── Yes → Resistance to existing products?
                    ├── No → Is there still unmet need?
                    │           ├── Yes → Proceed
                    │           └── No → Deprioritize
                    └── Yes → Proceed to Phase 2
```

### 1.4 Deliverables

- `Paper/targets/organism-profiles/<organism-name>.md`
- Summary table of top 3–5 organisms with ranked scores
- Log entry in `Paper/log.md`

---

## Phase 2: Target Protein/Receptor Identification

### 2.1 Target Class Selection

Map organism to known target classes based on literature and prior success:

| Target Class | Examples | Key Reference |
|---|---|---|
| Ion channels (ligand-gated) | nAChR, GABA-R, RyR | [[targets-overview]] |
| Ion channels (voltage-gated) | Nav, Cav | [[targets-overview]] |
| Enzymes (cuticle / cell wall) | CHS, AChE, CES | [[candidate-targets-pam-marrone-2026-05-22]] |
| Receptors / GPCRs | Octopamine, kinin, sNPF | [[candidate-targets-pam-marrone-2026-05-22]] |
| Motor / cytoskeletal | Myosin-I | [[candidate-targets-pam-marrone-2026-05-22]] |
| Transcription factors | Met, EcR/USP | [[targets-overview]] |
| Hormone / neuropeptide receptors | Insulin receptor, JH receptor | [[targets-overview]] |
| V-ATPase / ion pumps | V-ATPase subunits | [[targets-overview]] |

### 2.2 Target Discovery Checklist

For each candidate target protein:

#### Essentiality
- [ ] RNAi / knockout / mutation confers lethality or severe fitness defect
- [ ] Ortholog in target organism shares > 60% identity to well-studied model (if model exists)
- [ ] No known compensatory pathway that rescues the phenotype

#### Selectivity & Safety
- [ ] No vertebrate ortholog (ideal) OR
- [ ] Key binding-site residues diverge significantly from vertebrate ortholog (document divergence)
- [ ] No expressed in beneficial organisms at risk (e.g., bees, parasitoids)

#### Druggability
- [ ] Discrete binding pocket or protein-protein interface identified
- [ ] Structure available (PDB, cryo-EM, AlphaFold with AF-confidence > 70)
- [ ] If no structure: plausible homology model exists with template > 30% identity

#### Validation
- [ ] Small-molecule or peptide inhibitor exists (even if weak — proves druggability)
- [ ] Resistance mutations mapped to specific residues (ground truth for binding site)
- [ ] Published SAR or mutagenesis data available

### 2.3 Target Prioritization Matrix

Score each target 1–5 on each axis. Total score is the sum.

| Axis | 1 (weak) | 5 (strong) |
|---|---|---|
| **Essentiality** | RNAi shows mild phenotype | Knockout is lethal; no rescue |
| **Selectivity** | Vertebrate ortholog identical | No vertebrate ortholog exists |
| **Druggability** | No structure, no pocket predicted | Crystal structure + validated binding site |
| **Validation** | No inhibitor known | Multiple inhibitors with SAR; resistance mutations mapped |
| **Market fit** | Niche pest; limited acreage | Major pest; multi-billion dollar crop |

**Min score to proceed to Phase 3:** 15 / 25

### 2.4 Target Protein Triage Decision Tree

```
Is the target essential? (RNAi/CRISPR lethal or severe)
    ├── No → Exclude
    └── Yes → Is it selective (no mammalian ortholog OR divergent binding site)?
            ├── No → Flag for safety risk; only proceed if strong other signals
            └── Yes → Structure or high-confidence model available?
                    ├── No → Flag for structural biology investment needed
                    └── Yes → Proceed to Phase 3
```

### 2.5 Deliverables

- `Paper/targets/target-profiles/<target-name>.md` per target
- Summary table of all candidate targets with scores
- Cross-reference to existing Agros targets (e.g., [[targets-overview]]) to avoid duplication
- Log entry in `Paper/log.md`

---

## Phase 3: Target Validation & Prioritization

### 3.1 Literature Deep-Dive

Agent must read and summarize:
1. Most recent review on target class (last 5 years)
2. All structural papers (PDB entries, cryo-EM, NMR)
3. Key mutagenesis / resistance papers
4. Any peptide or biologic inhibitor papers
5. Competitive intelligence (patent landscape, competitor programs)

### 3.2 Resistance & Escape Risk Assessment

| Risk Factor | Low Risk | High Risk |
|---|---|---|
| Target copies in genome | Single copy | Multiple paralogs (compensation risk) |
| Known resistance mechanisms | None | Multiple MoA-crossing mutations documented |
| Binding site conservation | Highly variable across species | Perfectly conserved across all pest species |
| Target function | Redundant / non-essential pathway | Single critical pathway |

### 3.3 Competitive Landscape Scan

- Search Espacenet / USPTO / Google Patents for: target name + "peptide" + "pesticide"
- Check competitor pipelines (e.g., Solasta Bio for neuropeptides, Biome Makers for microbiome)
- Document in target profile: `competitive_threat.md` section

### 3.4 Final Prioritization

Rank targets into tiers:

| Tier | Criteria | Action |
|---|---|---|
| **Tier 1** | Score > 20, no serious competition, structure available | Immediate design pipeline entry |
| **Tier 2** | Score 15–20, minor gaps (e.g., AF only, limited SAR) | Proceed with caveats; invest in missing data |
| **Tier 3** | Score < 15 or major safety/competition concerns | Hold for future; revisit if landscape changes |

### 3.5 Deliverables

- Validated target profile with full literature review
- Prioritization table with Tier 1/2/3 ranking
- Risk register per target
- Update to `Paper/decisions.md` if major strategic decisions made

---

## Phase 4: Design Readiness Assessment

### 4.1 Structural Readiness

Before handing to design team, confirm:

- [ ] 3D structure exists (PDB / EMDB / high-confidence AlphaFold)
- [ ] Binding pocket / interface coordinates are known or can be inferred
- [ ] Key residues for selectivity are identified (for mammalian divergence)
- [ ] Any necessary cofactors / ions / ligands are documented

### 4.2 Assay Readiness

- [ ] Recombinant protein expression protocol exists (or can be adapted from close ortholog)
- [ ] Binding assay validated (e.g., SPR, BLI, FP, thermal shift)
- [ ] Functional assay exists (e.g., electrophysiology for ion channels, enzymatic for enzymes)
- [ ] Cell-based or organism-based assay available for hit validation

### 4.3 Computational Readiness

- [ ] AlphaFold model quality is acceptable (pLDDT > 70 for binding region)
- [ ] Homology model can be built if no direct structure
- [ ] Pocket detection / druggability scoring completed (e.g., Fpocket, SiteMap)
- [ ] Molecular dynamics or docking feasibility assessed

### 4.4 Handoff Package

Package for design team:
1. Target profile markdown (all phases above)
2. FASTA sequence of target protein
3. Structural files (.pdb, .cif) or AlphaFold model
4. Binding pocket definition (residue list, coordinates, or reference ligand)
5. Key selectivity residues (for filtering designs)
6. Assay details & protocols
7. Literature PDFs (key 3–5 papers)
8. Decision log entry

### 4.5 Decision Gate

```
All Phase 4 checklists complete?
    ├── No → Return to Phase 2 or 3 to fill gaps
    └── Yes → Hand off to Design Pipeline (BoltzGen, etc.)
```

---

## Agent Instructions by Role

### Agent: Researcher

**Trigger:** New target organism or target class to investigate.

**Tasks:**
1. Search PubMed, Google Scholar, and preprint servers for recent literature on the target organism / protein.
2. Extract: key papers, structural data, mutagenesis data, inhibitor data, economic impact.
3. Populate the target profile template with findings.
4. Flag any contradictions or uncertainties.

**Output:** Completed target profile (Phase 1–3 content).

**See:** [[../source-template]] for general source formatting.

---

### Agent: Analyst

**Trigger:** Target profiles ready for scoring and prioritization.

**Tasks:**
1. Score each target using the Phase 2 matrix.
2. Assess competitive landscape (patents, competitors).
3. Assess resistance / escape risk.
4. Produce tiered prioritization table.
5. Log decisions and rationale.

**Output:** Prioritization report with Tier 1/2/3 rankings.

---

### Agent: Validator

**Trigger:** Tier 1 or Tier 2 target selected for detailed validation.

**Tasks:**
1. Verify structural data quality (PDB resolution, AF confidence, binding region coverage).
2. Verify assay availability and protocols.
3. Confirm selectivity claim with sequence alignment to vertebrate orthologs.
4. Identify any gaps that would block design pipeline entry.

**Output:** Phase 4 design-readiness report.

---

### Agent: Design Liaison

**Trigger:** Target passes Phase 4.

**Tasks:**
1. Assemble handoff package (FASTA, structures, pocket definitions, assays).
2. Write one-page brief for design team (target, why it matters, key design constraints).
3. Schedule kickoff with design / ML team.

**Output:** Complete handoff package + design brief.

---

## Templates & Checklists

### Organism Profile Template

See: `Templates/organism-profile-template.md` (create if not exists).

### Target Protein Profile Template

Create one markdown file per target using this structure:

```markdown
---
type: target-protein
organism: "Scientific name"
gene: "Gene symbol"
protein_name: "Full protein name"
target_class: "e.g., Ion channel / Enzyme / GPCR"
structure_status: "PDB | Cryo-EM | AlphaFold | Homology model | None"
selectivity: "High | Medium | Low (with rationale)"
priority_tier: "1 | 2 | 3"
score: 20
status: "active | on-hold | deprioritized"
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
sources: []
---

# Protein Name (Organism)

## Essentiality

- Evidence for essentiality:
- Knockout / RNAi phenotype:

## Selectivity & Safety

- Vertebrate ortholog:
- Key divergent residues:
- Risk to beneficials:

## Druggability

- Binding pocket / interface:
- Structural data:
- Known inhibitors:

## Validation

- Resistance mutations:
- SAR data:
- Peptide / biologic precedent:

## Competitive Landscape

- Patents:
- Competitor programs:

## Design Readiness

- Structure file:
- Pocket definition:
- Assay protocol:
- Handoff date:

## Notes & Open Questions

- 
```

### Decision Log Entry

Each major decision (organism selected, target advanced, target deprioritized) must be logged to `Paper/decisions.md`:

```markdown
## YYYY-MM-DD — Decision: [Short description]

**Context:** [Brief background]
**Options considered:** [List alternatives]
**Decision:** [What was chosen]
**Rationale:** [Why]
**Risks:** [What could go wrong]
**Owner:** [Who made the call]
```

---

## See Also

- [[targets-overview]] — existing target list for lepidoptera & sucking insects
- [[candidate-targets-pam-marrone-2026-05-22]] — fungal & insect targets from Pam Marrone call
- `Paper/decisions.md` — decision log
- `Paper/log.md` — chronological log of all target work

---

*Workflow version: 1.0*
