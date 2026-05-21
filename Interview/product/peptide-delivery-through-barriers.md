# Peptide Delivery Through Cell Walls and Cuticles

The hard delivery question is not "can peptides cross every biological barrier?" They usually cannot, at least not without help. The right question is: **where does the peptide need to act for the first product, and what barrier actually matters?**

For Agros, the first wedge is Botrytis on high-value specialty crops. That is important because Botrytis is a foliar pathogen: it spends part of its infection cycle on the plant surface, germinates on leaves/fruit/flowers, and penetrates locally. We do not need systemic plant delivery for the first product. We need the peptide to stay on the surface long enough and reach the pathogen at the infection site.

---

## The barriers

| Barrier | Where it matters | Why it is hard | Agros strategy |
|---------|------------------|----------------|----------------|
| Plant cuticle | Foliar sprays on grapes, strawberries, tomatoes, ornamentals | Waxy, hydrophobic, sheds water, limits penetration | Optimize leaf retention, wetting, and local release; do not require full systemic uptake at first |
| Fungal cell wall | Botrytis hyphae/spores | Chitin/glucan wall blocks large or highly charged molecules | Start with extracellular or surface-accessible targets; use cationic/amphipathic motifs or carriers if internal access is required |
| Fungal membrane | After crossing or disrupting the wall | Selectivity risk if peptide is broadly membrane-active | Design target-specific binding first; screen for mammalian/plant toxicity early |
| Insect cuticle | Contact insecticides | Hydrophobic multilayer barrier; peptide penetration is difficult | Prefer oral/gut route for insect programs unless formulation data supports contact delivery |
| Insect gut lining | Feeding insects | Proteases, pH, peritrophic matrix, epithelial uptake | Encapsulation, protease stabilization, gut-binding motifs, target selection for exposed gut receptors |
| Plant vascular system | Soil/systemic disease targets | Uptake and transport through plant tissue is very hard for peptides | Avoid as first wedge; revisit for seed treatment/drip programs after foliar proof |

---

## Botrytis: why delivery is tractable first

Botrytis is the best first target because the product can be a **topical foliar spray**, not a systemic therapeutic.

What this means:
- The peptide can be applied directly to the tissue where infection starts.
- The required delivery distance is short: spray deposit to leaf/fruit surface to germinating spore or hyphal tip.
- The first product can act during high-risk infection windows rather than maintaining season-long systemic concentration.
- Greenhouse leaf-disc assays can quickly test whether formulated peptide retains activity on real plant surfaces.

The first success condition is not "peptide crosses the entire plant cuticle." It is:

> Enough active peptide remains near the Botrytis infection site long enough to suppress germination, hyphal growth, or virulence-factor activity.

---

## Delivery modes we can use

### 1. Surface-local delivery

This is the first commercial path.

Use case:
- Botrytis gray mold
- Powdery mildew / other surface-associated fungi
- Flower, fruit, and leaf infections where disease starts externally

Tools:
- Sticker-spreader adjuvants to keep peptide on the leaf or fruit
- Surfactants to improve wetting across waxy cuticles
- Humectants to keep the peptide hydrated after spraying
- Encapsulation for slow release at the surface
- Application timing around disease-risk windows

Why it works for Agros:
- It avoids the hardest version of the delivery problem: systemic plant uptake.
- It matches how growers already apply fungicides.
- It is compatible with greenhouse and field trial protocols.

### 2. Fungal-wall interaction

If the target is extracellular, secreted, or surface-accessible, the peptide may not need to cross the fungal wall. If the target is inside the fungus, wall passage becomes a major constraint.

Possible strategies:
- **Target exposed biology first:** prioritize secreted virulence factors, cell-wall-associated enzymes, or membrane-accessible proteins.
- **Cationic/amphipathic design:** many antifungal peptides interact with fungal walls and membranes through charge and hydrophobic patterning.
- **Cell-penetrating motifs:** add short penetration motifs only at low-attribution positions so binding residues are preserved.
- **Carrier formulation:** use nanoparticles, liposomes, or clay/polymer carriers that improve local uptake or contact with fungal structures.
- **Enzyme-sensitive release:** formulate the peptide so fungal enzymes or local pH release active peptide near the pathogen.

What to avoid:
- Do not claim we can reliably deliver arbitrary peptides into fungal cytoplasm today.
- Do not choose an intracellular target as the first commercial wedge unless the assay shows uptake.

### 3. Plant cuticle penetration

For the first Botrytis product, full cuticle penetration is not required. Still, cuticle interaction matters because the peptide must stay where it is sprayed.

Formulation levers:
- Nonionic surfactants for wetting
- Oils or wax-compatible adjuvants for retention on hydrophobic surfaces
- Sticker polymers for rainfastness
- Encapsulation that adheres to cuticular waxes
- Droplet-size tuning for coverage on berries, grape clusters, flowers, and leaf undersides

Validation:
- Spray leaf or fruit discs
- Simulate drying and humidity cycles
- Wash with controlled water volume
- Inoculate with Botrytis
- Measure lesion suppression versus naked peptide

### 4. Oral delivery for insect targets

For Lepidopteran or whitefly programs, the likely first path is oral delivery rather than contact penetration through the insect cuticle.

Why:
- Insect cuticle penetration is difficult for peptides.
- Feeding pests naturally ingest treated plant material.
- Gut epithelium, gut receptors, and digestive physiology give clearer biological access.

Tools:
- Encapsulation to protect the peptide from gut proteases
- pH-triggered release tuned to insect gut conditions
- Gut-binding motifs to increase retention near epithelial targets
- D-amino acid substitutions or cyclization at non-binding positions
- Feeding assays before contact-toxicity assays

YC phrasing:

> "For insect targets, we would not start by forcing peptides through the cuticle. We would design for oral exposure through feeding, where the pest naturally brings the peptide to the gut."

---

## How IG helps delivery engineering

Delivery modifications can destroy activity if they hit the binding residues. IG gives us a map of which residues are binding-critical.

That lets us:
- Keep high-attribution residues fixed.
- Add penetration or adhesion motifs at low-attribution positions.
- Cyclize or cap termini when termini are not part of the pharmacophore.
- Tune charge and hydrophobicity without randomly disrupting binding.
- Compare modified variants back through Boltz-2 / Chai / CAMP before synthesis.

This is the platform answer: delivery is not separate from design. Once we know the pharmacophore, we can engineer the delivery scaffold around it.

---

## Experimental plan

### Before field trials

| Question | Experiment | Pass signal |
|----------|------------|-------------|
| Does naked peptide work in solution? | In vitro Botrytis growth inhibition / MIC / EC50 | Low µM activity or strong fixed-dose inhibition |
| Does it work on plant tissue? | Leaf-disc or fruit-disc Botrytis assay | Lesion suppression vs. untreated and formulation-only controls |
| Does formulation improve delivery? | Compare naked peptide vs. sticker/spreader vs. encapsulated peptide | Higher retained activity after drying/washoff |
| Does it need fungal uptake? | Fluorescently labeled peptide microscopy or LC-MS uptake assay | Signal localized to spore/hypha/wall or enough extracellular efficacy |
| Is it selective? | Plant phytotoxicity, hemolysis, beneficial microbe screen | No toxicity at multiples of active dose |

### After first hit

1. Label the peptide and observe where it goes on leaf tissue and Botrytis hyphae.
2. Test naked peptide, capped peptide, and formulated peptide side by side.
3. Run rainfastness and simulated sunlight challenges.
4. Advance only if formulated peptide beats naked peptide on plant tissue.

---

## What not to overclaim

Do not say:
- "Our peptides pass through plant cell walls."
- "We can deliver to any intracellular target."
- "Cell-penetrating peptides solve delivery."
- "Cuticle penetration is solved."

Say instead:

> "We picked Botrytis first because it avoids the hardest delivery problem. It is a foliar pathogen, so our first product can work as a topical spray at the infection site. For intracellular or insect targets, delivery becomes target-specific: oral gut delivery for feeding insects, formulation carriers for fungal uptake, and sequence modifications only at residues IG says are not binding-critical."

---

## Interview answer

> "We are not assuming arbitrary peptide uptake through plant cell walls. That is exactly why Botrytis is the first wedge: it is a foliar pathogen, so the first product can act locally on the leaf, fruit, or flower surface during infection. If the target requires fungal entry, we can add delivery motifs, encapsulation, or charge/hydrophobicity changes at low-IG residues, but we would validate that with leaf-disc assays and labeled-peptide uptake before field trials. For insect programs, we would bias toward oral delivery through feeding rather than trying to force peptides through the cuticle."

