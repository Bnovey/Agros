# Iterative Peptide Design Pipeline

## Full Pipeline Flowchart

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           ITERATIVE PEPTIDE DESIGN                          │
└─────────────────────────────────────────────────────────────────────────────┘

   ┌─────────────────┐
   │  🧬 CREATION    │
   │                 │
   │   BoltzGen      │
   │ Generate 1000+  │
   │  Candidates     │
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │  📊 SCORING     │
   │                 │
   │ Boltz-2 (Bind)  │
   │ Vina (Physics)  │
   │ GNINA (ML)      │
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │  🎯 TOP LEADS   │
   │                 │
   │ Filter to top   │
   │ 10-50 by score  │
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │  🧠 IG ANALYSIS │
   │                 │
   │ Chai-2 (Struct) │
   │ Boltz-2 (Bind)  │
   │ ESM-2 (Function)│
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │ 🔒 CONSENSUS    │
   │                 │
   │ Lock: 3,7,12    │
   │ Constrain: 5,9  │
   │ Explore: 1,2,4  │
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │ 🔄 ITERATE      │
   │                 │
   │ Generate new    │
   │ candidates with │
   │ locked positions│
   └────────┬────────┘
            │
            │ ⟲ Next Generation
            └──────────────────────────────────┐
                                              │
                                              ▼
                                    ┌─────────────────┐
                                    │ 📈 CONVERGENCE  │
                                    │                 │
                                    │ Score plateau → │
                                    │ Lab validation  │
                                    └─────────────────┘
```

## Key Metrics Per Iteration

| Generation | Candidates | Top Score | Locked Positions | Improvement |
|------------|-----------|-----------|------------------|-------------|
| Gen 0      | 1000      | -8.2 kcal | None            | Baseline    |
| Gen 1      | 1000      | -9.1 kcal | 3,7,12          | +10.9%      |
| Gen 2      | 1000      | -9.4 kcal | 3,7,12,5        | +14.6%      |
| Gen 3      | 1000      | -9.6 kcal | 3,7,12,5,9      | +17.1%      |

## 90-Second Demo Script

**[0-15s] Problem Setup**
*"Current peptide design takes 2-5 years of trial and error. We've built an AI system that reduces this to weeks through intelligent iteration."*

**[15-45s] Technical Innovation** 
*"Here's our secret: [point to diagram] Instead of random generation, we use integrated gradients from three different AI models to identify which amino acid positions are actually critical for binding."*

*"When Chai-2, Boltz-2, and ESM-2 all agree a position is important—like positions 3, 7, and 12 here—we lock it permanently. When they disagree, we allow controlled exploration."*

**[45-75s] Results**
*"Each iteration gets better: we started at -8.2 kcal binding affinity, and by generation 3 we're at -9.6 kcal—that's a 17% improvement with surgical precision."*

*"Instead of blindly changing everything, we're only optimizing positions that won't break the peptide."*

**[75-90s] Value Proposition**
*"This is the first system to combine structural, binding, and functional AI models for peptide design. We're ready for lab validation and scaling to new agricultural targets."*

## Visual Elements for Demo

### Iteration Heatmap
```
Position:  1  2  3  4  5  6  7  8  9 10 11 12
Gen 0:    🟡🟡🔴🟡🟡🟡🔴🟡🟡🟡🟡🔴   (-8.2 kcal)
Gen 1:    🟡🟡🔒🟡🟡🟡🔒🟡🟡🟡🟡🔒   (-9.1 kcal)
Gen 2:    🟡🟡🔒🟡🔒🟡🔒🟡🟡🟡🟡🔒   (-9.4 kcal) 
Gen 3:    🟡🟡🔒🟡🔒🟡🔒🟡🔒🟡🟡🔒   (-9.6 kcal)

🔴 High importance (to be locked)
🔒 Locked position  
🟡 Mutable position
```

### Model Consensus Visualization
```
      Chai-2  Boltz-2  ESM-2  → Decision
Pos 3:  ████    ████    ███   → 🔒 LOCK
Pos 5:  ███     ██      ███   → 🔧 CONSTRAIN  
Pos 1:  ██      █       ██    → 🔄 EXPLORE
```

## Implementation Notes

- **Generation size**: 1000 candidates per iteration optimal for demo timing
- **Scoring cutoff**: Top 10% advance to IG analysis  
- **Consensus threshold**: 2/3 models agree = lock position
- **Convergence**: Score plateau &lt; 2% improvement over 2 iterations
- **Demo dataset**: Lepidopteran nAChR (well-validated target)

## File Outputs for Demo
- `generation_N_scores.json` - All candidate scores
- `generation_N_ig_consensus.json` - Position-level IG analysis  
- `generation_N_locked_positions.json` - Current constraints
- `pipeline_metrics.json` - Iteration-over-iteration improvement
