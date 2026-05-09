# Agros Research Vault

This vault is structured as an LLM-maintained research wiki for the Agros biopesticide discovery platform.

## How to use this vault

### Add a source

Put the original file in `raw/`.

Examples:
- Articles go in `raw/articles/`
- Papers go in `raw/papers/`
- Lecture notes go in `raw/notes/`
- Transcripts go in `raw/transcripts/`
- Images and attachments go in `raw/assets/`

Then ask an LLM agent:
> Ingest `raw/...` into the wiki. Create a source page, update relevant concept/project/issue pages, update `wiki/index.md`, and append to `wiki/log.md`.

### Ask a question

Ask:
> Use the wiki to answer: [question]. Read `wiki/index.md` first, then relevant pages. If the answer is reusable, save it in `wiki/questions/`.

### Lint the wiki

Ask:
> Lint the wiki for contradictions, stale claims, missing provenance, duplicate pages, orphan pages, and missing concept pages. Save the report in `wiki/reports/`.

## Rule

Raw sources are never edited. The generated wiki is allowed to evolve.

## Files that still need manual sorting

The following files were moved to `raw/notes/` during vault initialization and may need manual review to determine their final home:

- `raw/notes/funding_pipeline.md`, `raw/notes/yc_strategy_and_odds.md`, `raw/notes/example-notes.md`, `raw/notes/sprint_plan.md` — Business notes
- `raw/notes/demo_script.md`, `raw/notes/iterative-pipeline-diagram.md`, `raw/notes/multi-model-ig-workflow.md` — Demo materials
- `raw/notes/paper-decisions.md`, `raw/notes/integrated_gradients.md`, `raw/notes/modal_cost_estimate.md` — Paper working notes
- `raw/notes/targets_overview_v1.md` — Overview of ag targets
- `raw/papers/` — Individual research paper summaries (Zhao, Zhu, Wang, etc.)
