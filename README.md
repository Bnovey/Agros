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

Interview/        — YC interview prep (market, competition, Q&A, team, traction)
```

## Artifact norms

Every workflow produces a note before it's considered done:

| Workflow | Artifact | Location |
|---|---|---|
| Paper read | Link, abstract, takeaways | `Papers/<subfolder>/` |
| Pipeline run | Manifest: target, seed, model hash, results summary | `Pipeline/<run_id>.md` |
| Paper decision | Entry: what, why, who | `Paper/decisions.md` |
| Meeting / sync | Notes with action items | `Company/meetings/<date>.md` |

## File naming convention

All files in `raw/papers/` follow this pattern:

```
LastName_Year_SpeciesTarget_topic.ext
```

- **LastName** — first author's last name, title-case
- **Year** — 4-digit publication year
- **SpeciesTarget** — species prefix + gene/protein ID (e.g. `PxnAChRa6`, `SfRdl`, `BtCHS2`)
  - `Px` = *Plutella xylostella*, `Sf` = *Spodoptera frugiperda*, `Bt` = *Bemisia tabaci*, `Sl` = *Spodoptera litura*
- **topic** — 1–3 word description
- `.md` for notes, `.pdf` for original papers — same stem when both exist

## Rule

Raw sources are never edited. The generated wiki is allowed to evolve.
