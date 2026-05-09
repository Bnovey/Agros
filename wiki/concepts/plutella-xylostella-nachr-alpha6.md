---
type: concept
created: 2026-05-09
updated: 2026-05-09
status: active
tags: [nAChR, ion-channel, target, Plutella-xylostella, spinosad, peptide-design]
sources:
  - baxter-2010-pxnachra6-spinosad-resistance
---

# Plutella xylostella nAChR α6 subunit (Pxα6)

## Definition

The nicotinic acetylcholine receptor α6 subunit from *Plutella xylostella* (diamondback moth), gene symbol **Pxα6**. It is a subunit of a pentameric ligand-gated ion channel (Cys-loop receptor superfamily) expressed in the insect nervous system. The functional receptor is assembled from multiple subunit types (typically α and β); the α6 subunit contributes to the agonist-binding interface located in the extracellular N-terminal domain (ECD).

## Why it matters

Pxα6 is the primary agronomic target for the Agros discovery pipeline:

- It is the **confirmed sole molecular target of spinosad**, a commercially important biopesticide — establishing that compounds acting on this receptor achieve effective insect control in the field.
- The agonist-binding interface is in the **extracellular N-terminal domain (ECD)**, which is solvent-exposed and accessible to peptide binders without requiring membrane penetration.
- The receptor is **selective to insects**: human α6 subunits differ sufficiently in sequence and pharmacology that selectivity is achievable.
- Spinosad resistance is caused by exon skipping (loss of functional receptor), not by binding-site mutations — meaning the **wild-type binding site geometry is intact** in susceptible (and most field) populations and is a valid design target.
- *P. xylostella* is a globally important agricultural pest with demonstrated resistance to most conventional insecticide classes, making new biopesticide modalities highly valuable.

## Key claims

- Pxα6 is the confirmed sole target of spinosad in *P. xylostella* — established by genetic mapping and *Xenopus* oocyte functional assays. (Source: [[baxter-2010-pxnachra6-spinosad-resistance]])
- Field-evolved spinosad resistance proceeds via exon skipping, not binding-site mutation; the WT binding site is intact. (Source: [[baxter-2010-pxnachra6-spinosad-resistance]])
- The agonist-binding interface is located in the ECD — well-accessible for peptide binders. (Source: [[baxter-2010-pxnachra6-spinosad-resistance]])
- Resistance is geographically widespread (multiple continents), confirming global agronomic relevance. (Source: [[baxter-2010-pxnachra6-spinosad-resistance]])

## Evidence

- From [[baxter-2010-pxnachra6-spinosad-resistance]]: Genetic mapping + *Xenopus* oocyte expression confirmed Pxα6 as the causal spinosad target; mis-spliced variants confer resistance by producing non-functional subunits.

## Related concepts

- [[nicotinic-acetylcholine-receptor]]
- [[spinosad]]
- [[extracellular-domain-binding]]

## Open questions

- What structural data (crystal structure, cryo-EM, or AlphaFold model) exists for the Pxα6 ECD? Is there sufficient quality for Boltz-2 scoring and docking baseline runs?
- Which specific residues in the Pxα6 ECD contact spinosad or acetylcholine? Is there published mutagenesis data to validate attention maps against?
- What is the sequence identity between Pxα6 and human α6 (CHRNA6) at the ECD binding interface — and does it provide a sufficient selectivity window?
- Are there additional α subunits (α1, α3) known to co-assemble with α6 in *P. xylostella* that should be modeled as part of the receptor complex?

## Contradictions or caveats

- No structural data for native *P. xylostella* α6 is currently noted in the vault; structural modeling will depend on homology or AlphaFold predictions, which may not resolve binding-site details at the resolution needed.
- The ECD accessibility claim assumes the peptide binder need not cross the membrane; this should be re-evaluated if the peptide is intended for systemic delivery (gut absorption, hemolymph exposure).
- Pxα6 alone may not be sufficient to reconstitute a spinosad-sensitive receptor in heterologous systems — co-subunit requirements are not fully specified in the raw note.
