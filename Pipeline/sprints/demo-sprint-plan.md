# Demo Sprint Plan

**Goal:** `python demo.py --target pxryr --n 5` → ranked peptide candidates with per-residue heatmaps + sequence rendering
**Target:** PxRyR (PDB: 5Y9V) — crystal structure, insect-specific pockets, strongest target
**Time budget:** ~6 hours

---

## Architecture

### Data flow

```
Target PDB (PxRyR 5Y9V)
        ↓
    BoltzGen
        ↓
  ~10 peptide sequences (FASTA)
        ↓
    Affinity scoring (Boltz-2, Chai-1, ESM-2)
        ↓
  Top 3 candidates
        ↓
  Per-residue scoring (see below)
        ↓
  Stacked heatmap figure + sequence rendering
```

### Per-residue scoring sources

```
peptide sequence + target PDB
        │
        ├── Boltz-2 AffinityModule ──→ IG (L,)           affinity, structure+chemistry
        ├── Chai-1 AffinityModule ───→ IG (L,)           affinity, structure+chemistry  
        ├── ESM-2 affinity head ─────→ IG (L,)           affinity, sequence only
        │
        ├── Proteolytic cleavage ────→ (L,)              gut/surface protease risk
        ├── Amphipathicity ──────────→ (L,)              membrane interaction potential
        ├── Aggregation propensity ──→ (L,)              formulation risk
        ├── Solubility contribution ─→ (L,)              aqueous solubility
        ├── Selectivity risk ────────→ (L,) binary       mammalian ortholog conservation
        │
        └── Hydrophobicity, charge ──→ (L,)              baseline physicochemistry
```

Every module returns a `(L,) float array`. The visualizer stacks them as tracks — same interface regardless of source.

### What each track tells you

| Track                     | Model         | Biological meaning                                                           |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| Boltz-2 IG                | Boltz-2       | Which residues drive predicted affinity                                      |
| Chai-1 IG                 | Chai-1        | Independent structure+chemistry affinity signal                              |
| ESM-2 IG                  | ESM-2         | Pure sequence affinity signal                                                |
| Cross-model agreement     | Derived       | Residues all three models agree on — highest confidence                      |
| Proteolytic cleavage risk | Rule-based    | Will this position get digested before reaching target                       |
| Amphipathicity            | Rule-based    | Membrane interaction — relevant for gut wall crossing                        |
| Aggregation propensity    | Rule-based    | Formulation risk — will peptide clump in solution                            |
| Solubility contribution   | Rule-based    | Per-residue aqueous solubility contribution                                  |
| Selectivity risk          | Manual lookup | Is this position conserved with mammalian RyR — binding here = toxicity risk |
| Hydrophobicity + charge   | Lookup table  | Baseline physicochemistry                                                    |

### File structure

```
demo/
├── demo.py              # entrypoint: python demo.py --target pxryr --n 5
├── generate.py          # BoltzGen wrapper → FASTA
├── score.py             # Boltz-2, Chai-1, ESM-2 affinity scoring + IG
├── properties.py        # all biopesticide-specific per-residue tracks
├── visualize.py         # stacked heatmaps + sequence rendering
└── results/
    ├── candidates.fasta
    ├── summary.csv
    └── figures/
        └── <peptide_id>.png
```

---

## Sprint Phases

### Phase 1 — Environment (45 min)
- [ ] `pip install boltz chai_lab fair-esm`
- [ ] Download PxRyR: `wget https://files.rcsb.org/download/5Y9V.pdb`
- [ ] Confirm one affinity forward pass on each of: Boltz-2, Chai-1, ESM-2
- [ ] **Gate:** if any model fails to install after 20 min, cut it and move on — don't lose the night to one dependency

### Phase 2 — Generation (45 min)
- [ ] Write `generate.py` — BoltzGen wrapper, takes target PDB + N, returns peptide sequences
- [ ] Run against 5Y9V, get 5–10 candidates out
- [ ] Save to `results/candidates.fasta`

### Phase 3 — Affinity scoring + ranking (30 min)
- [ ] Write scalar affinity scoring in `score.py` for all three models
- [ ] Rank candidates by Boltz-2 score (primary)
- [ ] Note where Chai-1 / ESM-2 agree or disagree
- [ ] Take top 3 into IG
- [ ] Save to `results/summary.csv`

### Phase 4 — IG on all three models (1.5 hr)
- [ ] Write `compute_ig(model, feats, baseline="zero", m_steps=50) -> np.ndarray`
- [ ] Run against Boltz-2, Chai-1, ESM-2 for each top 3 candidate
- [ ] **Critical:** validate token indexing — print shapes + chain IDs, confirm peptide vs. receptor token split before running full batch
- [ ] Compute cross-model agreement track: mean of normalized IG vectors across all three models
- [ ] Save each as `.npz`: `(model, peptide_id) → (L,)`
- [ ] Sanity check: high-IG residues should cluster at the binding interface, not be randomly distributed

### Phase 5 — Biopesticide properties (1 hr)
All pure Python, no models, lookup tables only. Write in `properties.py`.

- [ ] **Proteolytic cleavage** — per-position cleavage risk across: trypsin (K/R), chymotrypsin (F/Y/W/L), elastase (A/V/G), insect gut serine proteases (broad, V8-like motifs). Sum across protease types → `(L,)` risk vector.
- [ ] **Amphipathicity** — sliding 7-residue window, hydrophobic moment assuming α-helix geometry (100° rotation per residue), assign to central residue → `(L,)`
- [ ] **Aggregation propensity** — flag windows of 5+ consecutive hydrophobic residues, score by hydrophobicity sum → `(L,)`
- [ ] **Solubility contribution** — positive score for charged/polar residues, negative for hydrophobic, penalty for exposed cysteines → `(L,)`
- [ ] **Selectivity risk** — lookup dict from `Papers/Targets/` notes: positions in PxRyR conserved with mammalian RyR flagged as high risk → binary `(L,)`
- [ ] **NetSolP** — run once per sequence, store as header stat (one number, not a track)
- [ ] **Hydrophobicity** — Kyte-Doolittle scale
- [ ] **Charge** — pH 7 charge per residue

### Phase 6 — Visualization (1 hr)
Write `visualize.py`. Stacked tracks per peptide, grouped by category:

**Header:** sequence ID, affinity rank, NetSolP score, cleavage site count

**Affinity tracks**
- Boltz-2 IG — bar chart, green (positive) / red (negative)
- Chai-1 IG — bar chart, same color scheme
- ESM-2 IG — bar chart, same color scheme
- Cross-model agreement — bar chart, darker = higher agreement

**Biopesticide viability tracks**
- Proteolytic cleavage risk — red spikes at danger positions
- Amphipathicity — blue/orange line
- Aggregation propensity — shaded regions
- Solubility contribution — green/red line
- Selectivity risk — binary red/green per position

**Physicochemistry tracks**
- Hydrophobicity + charge — dual axis line

**Sequence row**
- Each residue colored by cross-model IG agreement magnitude

Save as `results/figures/<peptide_id>.png`

### Phase 7 — Wire up (30 min)
- [ ] Write `demo.py` — calls all modules in order, one command runs everything
- [ ] Test: `python demo.py --target pxryr --n 5`
- [ ] Clean up figures so they look presentable

---

## Cut list (if time runs short)

Cut in this order — demo still works without them:

1. Chai-1 — if install is painful, Boltz-2 + ESM-2 still gives two independent models
2. Cross-model agreement track — drop it, just show three separate rows
3. Aggregation propensity — add post-demo
4. Selectivity risk lookup — skip if target notes aren't clean enough to parse quickly

**Minimum viable demo:** Boltz-2 IG + ESM-2 IG + proteolytic cleavage + amphipathicity + hydrophobicity/charge + sequence rendering

---

## Key risks

| Risk                              | Mitigation                                                                   |
| --------------------------------- | ---------------------------------------------------------------------------- |
| Boltz-2 / Chai-1 install fails    | Have Colab A100 notebook ready as fallback                                   |
| Token indexing wrong in IG        | Print shapes + chain IDs at every step before running batch                  |
| BoltzGen output format unexpected | Read the BoltzGen README before assuming output structure                    |
| IG scores look random             | Check `model.eval()`, dropout off; try masked-token baseline instead of zero |
| No GPU locally                    | Rent Colab A100 — ~$1–2 for the whole sprint                                 |

---

## Success criteria

`python demo.py --target pxryr --n 5` runs clean and outputs:
- `results/summary.csv` — 5 candidates ranked by affinity
- `results/figures/<id>.png` — one stacked heatmap per candidate

That's the demo.
