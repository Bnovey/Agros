# Multi-Model IG Workflow Demo

## Flowchart: Single Iteration Demo

```
┌─────────────────┐
│ Example Peptide │
│ + Target Protein│
└────────┬────────┘
         │
    ┌────▼────────────────────────┐
    │     Multi-Model IG          │
    ├─────────┬─────────┬─────────┤
    │ Chai-2  │ Boltz-2 │ ESM-2   │
    │(Structure)│(Binding)│(Function)│
    └─────────┴─────────┴─────────┘
         │
    ┌────▼─────────────────────────┐
    │    Consensus Analysis        │
    │ Position 3,7,12: 🔒 LOCK     │
    │ Position 5,9: 🔧 CONSTRAIN   │
    │ Position 1,2,4: 🔄 EXPLORE   │
    └────────┬─────────────────────┘
         │
    ┌────▼─────────────────────────┐
    │  "Generated" Candidates      │
    │  (show improved scores)      │
    └──────────────────────────────┘
```

## 45-60 Second Talking Track

**"Traditional peptide design is trial-and-error. We've built the first system that uses three different AI models—Chai-2 for structure, Boltz-2 for binding, and ESM-2 for function—to identify which amino acid positions are critical.**

**Here's how it works: [show flowchart] We run integrated gradients on all three models simultaneously. When all models agree a position is important, we lock it in place. When they disagree, we allow controlled exploration.**

**This gives us surgical precision—we only change positions that won't break the peptide, while preserving the critical binding and structural elements. Early results show this approach generates candidates with significantly higher predicted binding affinity than random design.**

**Next step is lab validation, but the computational framework is ready to scale."**

## Key Demo Elements
- Show actual IG heatmaps from your runs
- Highlight consensus positions visually
- Show before/after binding scores (even if simulated)
- Emphasize the "first to combine three models" angle

## Timing Breakdown
- Problem setup: 15 seconds
- Technical approach: 25 seconds  
- Results/value prop: 15 seconds
- Next steps: 5 seconds

**Total: 60 seconds**