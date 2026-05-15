---
type: report
created: 2026-05-09
updated: 2026-05-09
---

# Initial Vault Audit

Conducted: 2026-05-09
Scope: Full vault reorganization from unstructured to wiki layout.

## Pre-reorganization structure

### Top-level folders found

| Folder | Contents | Classification |
|--------|----------|---------------|
| `Company/` | funding_pipeline.md, Demo Sprint/sprint_plan.md, Notes/yc_strategy_and_odds.md, Notes/Example Notes.md | Working notes / business |
| `Demo/` | demo_script.md, Iterative Pipeline Diagram.md, Multi-Model IG Workflow.md, pipeline_flowchart.svg | Working notes / demo materials |
| `Paper/` | decisions.md, methods/integrated_gradients.md, methods/modal_cost_estimate.md | Working notes / paper methodology |
| `Papers/` | Targets/*.md (19 papers + overview), ML/.gitkeep | Raw sources (paper summaries) |
| `Pipeline/` | .gitkeep only | Empty placeholder |

### Files moved to raw/

All pre-existing content was preserved and moved to `raw/`:

- `Papers/Targets/*.md` (19 research paper notes) → `raw/papers/`
- `Papers/Targets/targets_overview_v1.md` → `raw/notes/targets_overview_v1.md`
- `Company/funding_pipeline.md` → `raw/notes/funding_pipeline.md`
- `Company/Notes/yc_strategy_and_odds.md` → `raw/notes/yc_strategy_and_odds.md`
- `Company/Notes/Example Notes.md` → `raw/notes/example-notes.md`
- `Company/Demo Sprint/sprint_plan.md` → `raw/notes/sprint_plan.md`
- `Demo/demo_script.md` → `raw/notes/demo_script.md`
- `Demo/Iterative Pipeline Diagram.md` → `raw/notes/iterative-pipeline-diagram.md`
- `Demo/Multi-Model IG Workflow.md` → `raw/notes/multi-model-ig-workflow.md`
- `Demo/pipeline_flowchart.svg` → `raw/assets/pipeline_flowchart.svg`
- `Paper/decisions.md` → `raw/notes/paper-decisions.md`
- `Paper/methods/integrated_gradients.md` → `raw/notes/integrated_gradients.md`
- `Paper/methods/modal_cost_estimate.md` → `raw/notes/modal_cost_estimate.md`

## Files that look like raw sources (good for ingestion)

The `raw/papers/` folder contains 19 research paper summaries — these are strong ingestion candidates:

- Papers about *Plutella xylostella* (PxnAChRa6, PxOA2B1, PxRdl, PxEcR, PxRyR, PxNav, PxAChE1)
- Papers about *Spodoptera frugiperda* (SfInR, SfNav, SfRdl)
- Papers about *Bemisia tabaci* (BtCHS2, BtnAChRb1, BtAChE1)
- Additional papers: Chen, Guo, Ibrahim, Jiang, Jindra, Li, Lv, Mao, Shu, Wang, Zhao, Zhu

## Files that look like working notes or synthesis

- `raw/notes/integrated_gradients.md` — methodology note, could become a concept page
- `raw/notes/multi-model-ig-workflow.md` — workflow description, relevant to the IG analysis project
- `raw/notes/iterative-pipeline-diagram.md` — pipeline description
- `raw/notes/paper-decisions.md` — decisions log, relevant context for the paper project
- `raw/notes/modal_cost_estimate.md` — cost estimate, useful for project planning

## Possible duplicates

None identified yet. Targets overview (`raw/notes/targets_overview_v1.md`) overlaps with individual paper notes in `raw/papers/` — these complement rather than duplicate each other.

## Notes that should become concept pages

The following topics appear frequently and should have concept pages in `wiki/concepts/`:

- `nicotinic-acetylcholine-receptor` (nAChR) — central target in multiple papers
- `ryanodine-receptor` (RyR) — another key target
- `integrated-gradients` — core analysis method for the paper
- `boltz-2` — primary scoring model
- `boltzgen` — primary generation model
- `biopesticide-peptide-design` — the overarching project concept
- `attention-analysis` — paper methodology

## Suggested next steps

1. Ingest one paper from `raw/papers/` as a test (e.g., a Plutella nAChR paper — most central to the project).
2. Create concept pages for nAChR and integrated-gradients, since they are referenced across many sources.
3. Ingest `raw/notes/targets_overview_v1.md` to seed the targets landscape.
4. Gradually ingest remaining papers, linking concept pages as you go.
5. Create a project page for the paper and one for the discovery pipeline.
6. Lint after every 5–10 ingests to catch inconsistencies early.

## No files were deleted

All pre-existing content was preserved.
