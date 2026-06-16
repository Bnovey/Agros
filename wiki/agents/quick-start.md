# Quick Start: Target Discovery Workflow

> Use this guide to quickly orient an agent or team member to the target discovery process for pesticides / biopesticides.

---

## 1. I have a pest organism in mind. What do I do?

**Step 1:** Create an organism profile using the template:
```
Templates/organism-profile-template.md
```

**Step 2:** Run through the gut-check checklist in the template. If it passes, proceed to Step 3.

**Step 3:** Save the completed profile to:
```
Paper/targets/organism-profiles/<organism-name>.md
```

**Step 4:** Log the decision in `Paper/decisions.md` and `Paper/log.md`.

---

## 2. I have an organism and want to find its molecular targets. What do I do?

**Step 1:** Consult the target class mapping table in [[target-discovery-workflow#22-target-class-selection]].

**Step 2:** For each candidate target class, search for:
- Genome annotation (is the gene present?)
- RNAi / CRISPR evidence (is it essential?)
- Structural data (PDB, AlphaFold)
- Small-molecule or peptide inhibitor precedent

**Step 3:** Create a target protein profile for each candidate using the template in [[target-discovery-workflow#templates--checklists]].

**Step 4:** Score each target using the prioritization matrix. Targets scoring ≥ 15 proceed to validation.

**Step 5:** Save profiles to:
```
Paper/targets/target-profiles/<target-name>.md
```

---

## 3. I have a target protein and want to validate it for design. What do I do?

**Step 1:** Run the Phase 4 Design Readiness Checklist:
- Structural readiness (PDB / AF / homology model)
- Assay readiness (expression, binding assay, functional assay)
- Computational readiness (pocket detection, druggability)

**Step 2:** If any checklist item fails, flag for additional work and return to Phase 2 or 3.

**Step 3:** If all pass, assemble the handoff package and pass to the design / ML team.

---

## 4. Agent Task Cheat Sheet

| If you are a... | Your job is to... | Use this file |
|---|---|---|
| **Researcher** | Find literature, populate profiles | [[target-discovery-workflow]] Phase 1–2 |
| **Analyst** | Score, rank, assess competition | [[target-discovery-workflow]] Phase 3 |
| **Validator** | Check structural & assay readiness | [[target-discovery-workflow]] Phase 4 |
| **Design Liaison** | Assemble handoff, brief design team | [[target-discovery-workflow]] Phase 4.4 |

---

## 5. Key Files & Links

| File | Purpose |
|---|---|
| `wiki/agents/target-discovery-workflow.md` | Full workflow documentation |
| `wiki/agents/quick-start.md` | This file — quick orientation |
| `Templates/organism-profile-template.md` | Template for new organism profiles |
| `Paper/targets/targets-overview.md` | Existing target list (Agros pipeline) |
| `Paper/targets/candidate-targets-pam-marrone-2026-05-22.md` | Recent target research (fungal & insect) |
| `Paper/decisions.md` | Decision log (append only) |
| `Paper/log.md` | Chronological work log |

---

## 6. Common Pitfalls

- **Don't skip the economics.** A target can be perfect biologically but irrelevant commercially. Always check yield loss data.
- **Don't assume selectivity.** Always sequence-align to vertebrate orthologs. Document divergent residues explicitly.
- **Don't ignore resistance.** A target with many known resistance mutations is still valuable — it confirms the binding site — but design must account for escape.
- **Don't reinvent the wheel.** Before creating a new profile, check `Paper/targets/` for existing work on that organism or target.
- **Log everything.** This is a collaborative workflow. Other agents and team members need to know what decisions were made and why.

---

*Last updated: 2026-06-15*
