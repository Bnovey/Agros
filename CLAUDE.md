# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## Project overview

**AI-designed peptide biopesticides.** Two-person team (Berkeley undergrads: bioengineering + data science, CS + math). Applied to YC W26.

We're building a platform to design novel peptides for biopesticide use by combining state-of-the-art protein generation and evaluation models (BoltzGen, Boltz-2) with a lab-in-the-loop validation system. Goal: replace synthetic chemical pesticides. Wet-lab assay results feed back into the models, creating a flywheel that improves efficacy and model performance — discovering new biopesticides in weeks instead of years.

The repo currently contains two parallel workstreams:

1. **Discovery pipeline** — generate peptide candidates against agricultural targets, score them, filter to leads.
2. **Research paper** — per-residue attention analysis of Boltz-2 applied to nonhuman (agricultural) targets vs. human/pharma baselines. Targeting bioRxiv preprint + mid-tier journal (Bioinformatics, PLOS Comp Bio, or NeurIPS AI for Science workshop).

## Tech stack

**AI models**
- BoltzGen — peptide generation (MIT, github.com/HannesStark/boltzgen)
- Boltz-2 — structure + binding affinity (MIT, github.com/jwohlwend/boltz)
- ESM-2 — protein embeddings (MIT, facebookresearch/esm)
- ProteinMPNN — inverse folding for sequence redesign
- AutoDock Vina + GNINA — physics-based docking baselines (for paper benchmarking)

**ML infra**
- PyTorch — inference
- Hugging Face PEFT — parameter-efficient fine-tuning of Boltz-2 on ag-specific data
- Weights & Biases or MLflow — experiment tracking

**Scientific computing**
- RDKit — molecular validation
- OpenMM — MD simulation for stability
- NetSolP — solubility prediction

**Compute**
- Modal — serverless GPU (A100/H100). Bursty design campaigns, pay-per-use.
- Budget: ~$300–500 for the paper phase; $1–5K/month for ongoing design campaigns.

**Dev**
- Python 3.11+
- GitHub for version control
- Docker / Modal image specs for reproducibility

## Repository layout (target structure)

```
.
├── src/
│   ├── generation/      # BoltzGen wrappers, candidate generation
│   ├── scoring/         # Boltz-2, Vina, GNINA scoring pipelines
│   ├── attention/       # Attention extraction hooks for Boltz-2 (paper)
│   ├── analysis/        # Per-residue analysis, comparative stats
│   ├── filters/         # NetSolP, RDKit, OpenMM downstream filters
│   └── utils/           # Modal config, I/O, common helpers
├── data/
│   ├── targets/         # PDB structures for ag + pharma targets
│   ├── benchmarks/      # DBAASP, APD3 known peptides
│   └── results/         # Generated candidates, scores, attention tensors
├── notebooks/           # Exploratory analysis only — not for production code
├── scripts/             # CLI entrypoints for pipeline stages
├── manuscript/          # Paper draft, figures
└── tests/
```

## Key targets

**Agricultural (paper + product):**
- Lepidopteran nicotinic acetylcholine receptor (nAChR) — *Plutella xylostella* preferred, well-validated
- Lepidopteran ryanodine receptor (RyR)
- *Botrytis cinerea* β-glucan synthase (fungal)

**Pharma baselines (paper only, for comparison):**
- Pick 2–3 with strong published binding/mutagenesis data

Targets are picked specifically for the existence of published mutagenesis or binding data — without ground truth, attention-map analysis has nothing to validate against.

## Working norms

**Code style**
- Type hints on all function signatures
- Docstrings: short summary + args/returns. NumPy or Google style, pick one and stick with it
- Black for formatting, ruff for linting
- Prefer pure functions; isolate I/O at boundaries

**Modal-specific**
- All GPU work goes through Modal apps in `src/<module>/modal_app.py`
- Pin model versions in Modal images — do not let Boltz-2 / BoltzGen versions drift between runs
- Cold starts are 10–30s for large models; batch work to amortize
- Set billing alerts; check `modal app logs` regularly

**Attention extraction (paper-critical)**
- Boltz-2 attention extraction may require modifying internal forward methods, not just hooks. Read the architecture code before assuming hooks work.
- Save attention tensors per layer per head — they're large; use float16 + compression
- Always log tensor shapes and the input residue indexing alongside saved attention; off-by-one bugs here invalidate the paper

**Reproducibility**
- Seed everything (numpy, torch, python random) at the top of every script
- Pin all model checkpoints by hash, not just by name
- Every experiment writes a manifest: code commit, model hash, target hash, seed, timestamp

**Git**
- Branches: `feat/...`, `fix/...`, `paper/...`, `exp/...` (experiments are throwaway)
- Don't commit data, model weights, or attention tensors. Use Git LFS only for figures.
- Don't commit Modal API tokens, W&B keys, or credentials of any kind

## Commands

```bash
# Setup
pip install -e ".[dev]"
pre-commit install

# Lint/format/test
ruff check src/ tests/
black src/ tests/
pytest tests/ -v

# Run a generation campaign on Modal
modal run scripts/generate.py --target plutella_nachr --n 1000

# Score candidates
modal run scripts/score.py --input data/results/<run_id>/candidates.fasta

# Extract attention for a single complex
modal run scripts/extract_attention.py --target plutella_nachr --peptide <seq>

# Build paper figures
python scripts/build_figures.py --run <run_id>
```

(Adjust paths to whatever the repo actually has — these are the intended entrypoints.)

## Closed-loop norms

Every workflow produces a readable artifact before it's considered done. Nothing lives only in someone's head.

| Workflow | Artifact | Location |
|---|---|---|
| Paper read | Note with link, abstract, takeaways | `Papers/<subfolder>/` |
| Pipeline run | Manifest with target, seed, model hash, results summary | `Pipeline/<run_id>.md` |
| Paper decision | Entry in decision log (what, why, who) | `Paper/decisions.md` |
| Meeting / sync | Short notes with action items | `Company/meetings/<date>.md` |

**Claude should prompt for the artifact if a task completes without one.** For example: if a pipeline run finishes, ask "want me to write the manifest?" before closing out.

## What Claude should do by default

- **Default to small, focused changes.** This is a research codebase being built by two part-time founders. Big rewrites cost time we don't have.
- **Read before writing.** Before modifying generation, scoring, or attention extraction code, read the upstream Boltz / BoltzGen source — these libraries change and our wrappers must match.
- **Ask before scoping up.** If a task could plausibly be done in 50 lines or 500, do the 50 first and check in.
- **Check Modal cost implications** for any change that runs on GPU. Note expected cost in PR descriptions.
- **Treat attention extraction as paper-critical infrastructure.** Test it carefully. Off-by-one residue indexing or silently dropped attention layers will sink the paper.
- **Don't add dependencies casually.** Each new package is a future Modal image rebuild. Justify additions.

## What Claude should NOT do

- Don't re-architect the pipeline without being asked.
- Don't introduce new ML models beyond the listed stack without discussing tradeoffs first.
- Don't write speculative code for wet-lab integration — that's post-funding work, not in scope yet.
- Don't optimize prematurely. Correctness first, then profile, then optimize.
- Don't paraphrase or restate paper claims without checking the underlying analysis — the manuscript and code must agree.

## Open questions / context the team is still resolving

- Final target selection for the paper (2 ag + 2–3 pharma)
- Whether to validate attention maps against known mutagenesis data (strongly preferred — needed for publishability)
- Active learning loop design for the post-funding wet-lab phase
- Patent strategy: what to file before bioRxiv preprint goes up

If a task touches one of these and the answer isn't obvious from context, ask before assuming.
