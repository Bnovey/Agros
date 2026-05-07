# Paper Decision Log

---

## 2026-05-06 — Per-residue affinity analysis method: integrated gradients

**What:** Use integrated gradients (IG) to generate per-residue binding affinity heatmaps from Boltz-2's AffinityModule output.

**Why:** IG is more principled than vanilla gradient saliency (avoids local linearization artifacts), requires no model modification, and produces a directly interpretable per-residue scalar that maps to "contribution to binding affinity." Output is publishable and validatable against in silico alanine scanning and/or experimental mutagenesis data.

Alternatives considered:
- Attention maps — tell you where the model looks, not what drives the affinity score. Not directly interpretable as affinity contribution.
- Vanilla gradient saliency — fast but noisy; local linearization misses non-linear contributions.
- In silico alanine scanning — most biologically interpretable but slower (N forward passes). Use as validation, not primary method.

**Who:** Bowman
