# Agros Research Vault

AI-designed peptide biopesticides. Two-person team (Berkeley undergrads). Applying to YC W26.

## Vault structure

```
Company/
  funding/        — funding programs, applications, timelines
  strategy/       — YC odds, competitive analysis
  demos/          — demo scripts and materials

Paper/
  decisions.md    — paper decision log (what, why, who)
  targets/        — target selection notes
  methods/        — IG, scoring, pipeline methods
  concepts/       — synthesized concept pages
  sources/        — synthesized source pages

Papers/
  targets/        — ag target literature (Plutella, Spodoptera, Bemisia)
  ML/             — ML/methods literature

Pipeline/
  sprints/        — sprint plans
  <run_id>.md     — pipeline run manifests (target, seed, model hash, results)

Templates/        — note templates for papers, concepts, projects, questions
```

## Artifact norms

Every workflow produces a note before it's considered done:

| Workflow | Artifact | Location |
|---|---|---|
| Paper read | Link, abstract, takeaways | `Papers/<subfolder>/` |
| Pipeline run | Manifest: target, seed, model hash, results summary | `Pipeline/<run_id>.md` |
| Paper decision | Entry: what, why, who | `Paper/decisions.md` |
| Meeting / sync | Notes with action items | `Company/meetings/<date>.md` |
