# Feature Attribution | Stanford CS224U Natural Language Understanding

**Type:** Lecture video (Spring 2021)
**Course:** Stanford CS224U — Natural Language Understanding
**Link:** https://www.youtube.com/watch?v=RFE6xdfJvag

## What it covers

CS224U lecture on feature attribution methods — techniques for explaining which input features (tokens, residues, positions) a model relies on to make predictions. Covers gradient-based methods (vanilla gradients, integrated gradients), attention-as-explanation, and their limitations.

Directly relevant to the paper's per-residue attention analysis of Boltz-2: the same conceptual framework applies when asking which residues the model attends to when scoring binding affinity on ag vs. pharma targets.

## Takeaways

- Gradient-based attribution (integrated gradients) is more faithful than raw attention weights for explaining model decisions
- Attention ≠ explanation — attention heads can spread weight without actually driving the prediction; IG pins attribution to gradient w.r.t. the output
- Completeness axiom: IG attributions sum to the output difference between input and baseline — useful sanity check for our per-residue profiles
- Baseline choice matters for IG: for sequences, a masked/zero-embedding baseline is standard
- Relevant if we want to move beyond attention maps toward gradient-based feature attribution in the paper
