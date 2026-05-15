# Per-Residue Affinity Analysis via Integrated Gradients

## What it is

Integrated gradients (IG) is an attribution method that assigns each input feature a scalar score representing its contribution to a model output. For Boltz-2, the output is the affinity score from `AffinityModule`; the input features are the per-token embeddings from the trunk.

Formally, for input `x` and baseline `x'`:


$$ IG(x_i) = (x_i - x'_i) × ∫[α=0→1] ∂F(x' + α(x - x')) / ∂x_i  dα $$
```

In practice, the integral is approximated by summing gradients at M evenly-spaced steps along the interpolation path (M = 50–100 steps is standard):

```
$$ IG(x_i) ≈ (x_i - x'_i) × (1/M) × Σ_k ∂F(x' + (k/M)(x - x')) / ∂x_i $$
Each `x_i` is the embedding vector for token `i`. Taking the L2 norm across the embedding dimension gives a single scalar per residue — the affinity heatmap value.

## Why this over alternatives

| Method | Modification needed | Passes | Interpretability |
|--------|-------------------|--------|-----------------|
| Attention maps | Yes (modify forward) + disable kernels | 1 | Indirect — where model looks, not affinity driver |
| Vanilla saliency | None | 1 | Noisy; local only |
| **Integrated gradients** | **None** | **M (~50–100)** | **Direct affinity contribution per residue** |
| Alanine scanning | None | N+1 | Highest biological interpretability; use for validation |

IG satisfies two formal axioms vanilla saliency does not:
- **Sensitivity:** if changing a feature changes the output, it gets nonzero attribution
- **Implementation invariance:** attributions don't depend on network implementation details (fused kernels, etc.)

## Baseline choice

The baseline `x'` (the "absence of signal") matters. Options:

- **Zero embedding** — simplest; may not be a meaningful null for a transformer
- **Masked token embedding** — use Boltz-2's pad/mask token embedding; more semantically meaningful null
- **Gaussian noise average** — average IG over multiple random baselines; most robust but expensive

Start with zero embedding. If results are noisy or baseline-sensitive, switch to masked token.

## What the output represents

- Positive IG score at residue `i` → residue `i` contributes positively to the predicted affinity
- Negative IG score → residue disfavors binding (or is neutral)
- Magnitude → relative importance

Plot as:
1. Sequence logo (bar chart over residue index, colored by magnitude)
2. Structure coloring in PyMOL/ChimeraX (B-factor column ← IG score)

## Validation

Cross-validate IG heatmaps against:
1. **In silico alanine scanning** — run WT + each single-Ala mutant through Boltz-2, compute `ΔΔG_pred`. Expect high IG residues to have large `|ΔΔG|`.
2. **Published mutagenesis data** — where available for the target (e.g., PxRyR NTD mutagenesis, nAChR α6 binding interface data). This is the paper's ground truth.

## Implementation notes

- No Boltz-2 modification needed — standard `loss.backward()` after a forward pass through `AffinityModule`
- Works with `use_kernels=True` (fused kernels are fine since we only need gradients at the input, not intermediate attention weights)
- Run in `model.eval()` mode; disable dropout
- Extract gradients at the token embedding layer output (after `InputEmbedder`, before the trunk), not at the raw sequence input
- Token indexing: use `feats["token_pad_mask"]` to strip padding, `feats["mol_type"]` to separate receptor (0) vs. peptide (0) tokens — both are `mol_type=0` (protein), so use chain ID or token range to distinguish
- Memory: M forward+backward passes at N=500 tokens; each pass is ~same cost as inference. At M=50, expect ~50× inference time per complex. Budget accordingly on Modal.
- Save IG arrays as float16 + npz compression alongside the run manifest
