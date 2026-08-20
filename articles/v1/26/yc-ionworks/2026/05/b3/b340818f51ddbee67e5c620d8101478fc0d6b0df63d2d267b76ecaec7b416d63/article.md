---
schema_version: "1.0.0"
document_id: "b340818f51ddbee67e5c620d8101478fc0d6b0df63d2d267b76ecaec7b416d63"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/material-properties-vs-cell-metrics"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:30.651124+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:f8c707ba419f85dbf8e2e3eba5151496766130c68a52d1c1b34fa365e72165cb"
---

# Material properties aren't cell metrics

A new electrode material shows a reversible capacity of 2,500 mAh/g in a coin cell against a lithium-metal counter electrode. The cycling curves look clean. A press release goes out. Two years later, the same material is in a commercial pouch cell, and that cell delivers less energy per kilogram than the high-nickel NMC it was supposed to replace. The active material did exactly what the half-cell data promised. The cell did not.


This gap between material-level performance and cell-level performance is one of the most persistent sources of disappointment in battery R&D. It is also the place where most of the engineering value gets created or destroyed. A 2023 *Nature Communications* perspective by Ulissi makes the case[bluntly](https://www.nature.com/articles/s41467-023-35933-2) : the metrics that get a paper published (specific capacity at the electrode level, cycle life of a thin coupon, conductivity of a freshly pressed pellet) are not the metrics that get a cell qualified. Cao and Amine had argued the same point a few years earlier in *Nature Nanotechnology* ,[calling explicitly for the field to align the academic and industrial metrics](https://www.nature.com/articles/s41565-019-0371-8) used to report progress on new battery chemistries. Translating between them is a non-trivial engineering problem in its own right.


## **What a coin cell hides**


Lab-scale coin cells (the 2032 format and its relatives) are designed for screening, not for representativeness. The Ulissi perspective puts numbers to the gap. A typical lab coin cell uses several times the practical loading of electrolyte and a large excess of lithium metal as the counter electrode. The N/P ratio (the ratio of negative-to-positive capacity) is effectively infinite. Coating thicknesses are often a fraction of what a commercial cell uses. Porosities are higher than what you'd ever ship.


Every one of those choices masks a real-world constraint:


- **Excess electrolyte** removes electrolyte-limited capacity fade as a failure mode. The material can consume electrolyte indefinitely without the test showing it.
- **Excess lithium** removes capacity loss to side reactions like SEI growth or[lithium plating](https://ionworks.com/blog/lithium-plating) from the visible cycling signature. In a real cell, every lithium inventory loss is visible as capacity fade.
- **Thin coatings and high porosity** make ion transport easy. A 50 µm electrode is a different transport problem than a 5 µm one.
- **Small areas** keep the current per unit area low even at apparent high C-rates. Polarization that would dominate a commercial cell barely registers.


The Ulissi paper makes the point that a "fast-charging" material in a coin cell may have been tested at conditions that bear no resemblance to a commercial fast charge. The same is true for cycle life: thousands of cycles at low rate, low areal capacity, and unlimited lithium say very little about a 4 mAh/cm² electrode being cycled 1C/1C in a pack. Nölle and colleagues spelled this out in detail in *Materials Today* ,[walking through how the cell setup itself determines what conclusions can honestly be drawn](https://www.sciencedirect.com/science/article/abs/pii/S1369702119307230) from a given electrochemical measurement. Liu and colleagues made it quantitative in *Nature Energy* : getting a lithium-metal cell past 350 Wh/kg[requires controlling cathode loading, electrolyte amount, and Li foil thickness simultaneously](https://www.nature.com/articles/s41560-019-0338-x) , each of which is invisible in a typical half-cell test.


## **Where the energy goes**


The same paper shows the cost of moving from theoretical material performance to a useful battery in stark terms. For a current high-energy NCA cell with a silicon-containing graphite negative electrode, the theoretical specific energy of the active materials is roughly halved by the time you reach a usable pack. The graphite||LFP system loses a smaller fraction, but starts from a lower theoretical number. Schmuch and colleagues laid out[the same accounting for automotive cells in Nature Energy](https://www.nature.com/articles/s41560-018-0107-2) , tracing how the price-per-kWh and the energy density delivered at the pack are both shaped by choices that look like cell-engineering trivia at the material stage: binder fraction, electrode loading, format selection, and pack architecture.


The losses come from places that are invisible at the material level:


- **Reversible fraction.** The cell only uses the voltage window in which it cycles stably. High-Ni cathodes have to be charged below their full theoretical capacity to keep cycle life acceptable.
- **Inactive mass.** Current collectors, separator, electrolyte, tabs, can, and binder are typically 40–60% of cell mass.
- **Cell-to-pack overhead.** Modules, cooling, electronics, structure, and thermal-runaway mitigation. Fire-rated barriers between cells, venting paths, and compartmentalization that slows or stops propagation when one cell fails all add mass and volume, and the penalty scales with cell energy: more reactive cathodes need more aggressive containment, which gives back some of the gain a higher-energy cell promised.


A material that looks 30% better than incumbent on a per-gram basis may give you 10% at the cell and 3% at the pack. It may give you nothing at all if the gains came from a chemistry window that can't be cycled, or if realizing them requires a pack architecture Li-ion has never needed. Li-metal anodes are the obvious example: they depend on sustained uniform stack pressure across the cell face (typically a few MPa) to keep plating smooth and avoid dendrites, which means platens and clamping hardware that add mass and volume no Li-ion pack ever had to budget for. The decision about whether to develop it cannot be made from the material data alone.


## **Three examples**


**Silicon negative electrodes.** Pure silicon stores around 3,580 mAh/g of reversible capacity, roughly ten times graphite. In a half-cell, that capacity comes out cleanly. In a full cell, two things have to be managed. The first is volumetric expansion of roughly 280% on full lithiation, which fractures particles and the SEI and consumes lithium and electrolyte every cycle. The second is the N/P balancing problem: silicon's swing in stoichiometry forces tight balancing to keep the cell within its voltage limits. Eshetu and colleagues gave a thorough industrial-perspective review of[what it actually takes to build a working silicon-containing high-energy cell](https://www.nature.com/articles/s41467-021-25334-8) . Different programs land in different places. Silicon-oxide blended into graphite, high-silicon composites with engineered binders, and high-Si or pure-Si anodes with mechanical accommodation and prelithiation are all shipping in some form today. Each path represents a multi-year cell-engineering effort wrapped around the same active material. The material specs of pure silicon have been known for decades. The cell-level engineering is what took the time.


**High-nickel and high-capacity cathodes.** The center of gravity has moved from NMC811 to NMC9-series and to Li- and manganese-rich (LMR) chemistries, both of which push the same envelope harder: more capacity per gram, higher voltage windows, higher demands on the electrolyte and the anode. The reason commercial cells using these cathodes still charge to a lower upper cutoff than the active material can theoretically handle is not the cathode in isolation. It is the combination of cathode surface reactivity, electrolyte oxidation, and gas generation that makes cycle life unacceptable above a certain voltage window. Single-crystal morphologies, fluorinated additives, and surface coatings help, and each one pushes the cell-level window outward by a few tens of millivolts. The same logic applies to charge rate: the cathode can intercalate faster than the cell will let you charge, because the rate limit is set by graphite plating at the negative electrode under realistic loading, not by cathode kinetics.


**Solid electrolytes.** A solid electrolyte pellet can show ionic conductivity in the same range as a liquid electrolyte (>1 mS/cm at room temperature), and oxide and sulfide chemistries have both reached that bar. The pellet is also a few hundred microns of dense, isotropic material with controlled interfaces. A full solid-state cell stacks that electrolyte against a composite cathode, an interlayer, and a metallic or composite anode under stack pressure that has to be controlled to within a few hundred kPa across the cell area. The properties that matter (interfacial impedance, dendrite resistance, mechanical stability under cycling) depend on the entire stack, not on the pellet. By 2026 a half-dozen leading solid-state programs have pilot lines running, and the[Volta Foundation's 2025 Battery Report](https://volta.foundation/battery-report-2025) catalogues third-party-validated cells in the 350–400 Wh/kg range. The pellet-conductivity question is mostly settled. Janek and Zeier laid out why in *Nature Energy* : the[challenges in speeding up solid-state battery development](https://www.nature.com/articles/s41560-023-01208-9) sit primarily at the interfaces and the composite cathode, not at the bulk conductivity of the electrolyte itself. A material that wins on bulk conductivity can still lose on the metric that matters: usable current density at acceptable degradation.


## **The metrics that matter at the cell**


A battery program is judged on a small set of cell-level numbers: energy density (Wh/L and Wh/kg), charge time, C-rate capability, calendar and cycle life, and cost per kWh. Every other measurement (exchange current densities, diffusion coefficients, half-cell capacities) is interesting only to the extent it explains or predicts those numbers.


This sounds obvious. The reason it has to be said is that the practical workflow in materials development often runs the other direction. A new material gets characterized exhaustively at the material level, then someone tries to estimate what it would do in a cell. The estimate is usually a back-of-the-envelope scaling, and the actual cell-build six months later disagrees. The information loop closes very slowly.


The Ulissi paper argues, correctly, that the field would benefit from designing experiments around cell-level KPIs from the beginning: N/P ratio, areal capacity, electrolyte loading, coating thickness, porosity, and current density per unit area. Not as final numbers, but as the variables a screen needs to keep in mind.


## **Closing the loop with simulation**


The other thing that helps close the loop is a model that runs both ways. Physics-based cell models, like the DFN and SPMe families that underpin most of modern battery simulation, take the parameters a material scientist measures (open-circuit potentials, diffusion coefficients, exchange current densities, electrolyte properties) and turn them into the metrics a cell engineer reports (rate capability, charge time, energy retention, plating margin under a duty cycle). When the parameters change, the cell predictions change. When the cell predictions change, the design decision changes.


That direction, from material parameters to cell behavior, is the one that compresses development. Instead of waiting for the next pouch-cell build to find out whether a new electrolyte additive helps with fast-charge plating, you ask the model. Instead of guessing whether a silicon blend will hit your energy target without unacceptable swelling, you sweep the composition against a cell-level objective and see where the tradeoff is.


This is the gap we work on at Ionworks. The Simulation OS is built around the assumption that materials data is upstream of decisions made on energy density, charge time, and lifetime, and that the path between them runs through physics-based cell models that have to be parameterized, validated, and shared across a team.
