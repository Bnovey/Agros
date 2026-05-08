# Agros Pipeline Demo Script

**Duration:** 60 seconds  
**Visual:** Pipeline flowchart graphic

---

## Script

**[0-15s: Generation Step]**
"Our pipeline starts with target-specific generation using BoltzGen. The model generates thousands of protein candidates using structure-based sampling and diversity optimization to explore the design space systematically. 

**[15-30s: Scoring Step]**  
"Next, we score and filter every candidate for binding affinity, soluability, stability, and other agriculture specific characteristics. With affinity being the main goal, we use Boltz-2 to predicts binding affinity, AutoDock Vina provides physics-based validation, and GNINA adds deep learning insights. We rank the top 10% for detailed analysis—filtering thousands down to the most promising leads."

**[30-45s: Analysis Step]**
"Here's where it gets interesting—integrated gradients analysis. We extract per-residue attribution maps from Chai-2, Boltz-2, and ESM-2. This heatmap shows which amino acid positions are critical for binding. Notice positions 3 and 7 are highlighted across all models—these are our 'lock' residues for optimization."

**[45-60s: Optimization & Iteration]**
"Finally, constraint-based optimization. We lock the high-attribution residues and sample new combinations, capturing epistatic interactions while avoiding over-constraining. The process iterates—each cycle refines our understanding and generates better candidates. This closed-loop approach is how we compress years of discovery into weeks."

---

## Visual Cues

- **Generation box:** Highlight BoltzGen model, show 1000+ candidates
- **Scoring box:** Show three model icons (Boltz-2, AutoDock Vina, GNINA)
- **Analysis box:** Focus on integrated gradients heatmap, highlight positions 3 & 7
- **Optimization box:** Show constraint sampling, point to iteration arrow
- **Heatmap:** Zoom in on "LOCK" positions during analysis section

## Key Messages

- **Speed:** Years → weeks compression
- **Rigor:** Multiple model validation
- **Intelligence:** Gradient-guided optimization
- **Iteration:** Continuous improvement loop