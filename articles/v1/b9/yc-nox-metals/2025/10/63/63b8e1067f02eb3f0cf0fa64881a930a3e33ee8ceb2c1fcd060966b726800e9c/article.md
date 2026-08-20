---
schema_version: "1.0.0"
document_id: "63b8e1067f02eb3f0cf0fa64881a930a3e33ee8ceb2c1fcd060966b726800e9c"
company_key: "yc-nox-metals"
company: "Nox Metals"
source_id: "yc-nox-metals-news-import-4d0abe59faf1"
canonical_url: "https://noxmetals.co/blog/stress-corrosion-cracking-7xxx-aluminum"
published_at: "2025-10-13T00:00:00+00:00"
first_seen_at: "2026-07-24T06:33:38.984322+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:538717d799661e357a41c45c289b5b3b9f953bb92f504464f42984ed63a0707f"
---

# Stress Corrosion Cracking in 7xxx Aluminum: Mechanisms, Testing, and Alloy Selection

## The Electrochemical Mechanism


During the artificial aging step of 7xxx alloy heat treatment, MgZn2 intermetallic particles (eta phase) precipitate preferentially along grain boundaries. The zone immediately adjacent to the grain boundary is depleted in solute - this precipitate-free zone (PFZ) is anodic relative to the grain interior. In the presence of moisture, chlorides, or other electrochemically active species, a galvanic couple forms between the anodic PFZ and the cathodic grain interior. Under sustained tensile stress across the grain boundary, this galvanic attack concentrates at the crack tip, lowering the fracture toughness of the material locally and enabling crack propagation at applied stresses far below the bulk yield strength. The process is self-sustaining: once initiated, the stress concentration at the crack tip accelerates both the corrosion attack and the mechanical driving force for crack growth.


## Why Short-Transverse Orientation Is Most Vulnerable


Aluminum plate is produced by rolling, which elongates grains into a pancake-like morphology: long in the rolling direction (L), shorter in the long-transverse direction (LT), and very thin in the short-transverse direction (ST). A tensile stress applied in the ST direction crosses the maximum number of grain boundaries per unit length. Since grain boundaries are the locus of the anodic PFZ and the path of crack propagation, the ST orientation has the highest crack growth rate and the lowest SCC threshold stress. The L and LT orientations are substantially more resistant because fewer grain boundaries are crossed per unit crack advance. This geometry-dependent susceptibility is why the ST direction is the primary concern in structural design: bolted joints, press fits, and thermal gradients can all introduce ST-direction tensile stress in plate components.


## T6 vs T73 vs T7351: The Precipitation State and SCC Resistance


The temper has a larger effect on SCC resistance than any other variable in alloy processing. At T6 peak aging, MgZn2 is fine, coherent, and densely distributed along grain boundaries - this produces maximum strength and minimum SCC resistance. At T73 overaging, MgZn2 coarsens, loses coherency, and redistributes - the anodic contrast between the PFZ and grain interior is reduced, and SCC resistance improves dramatically at a cost of 10 to 15% in tensile strength. T7351 is an intermediate overaging condition between T6 and T73, providing a compromise between strength and SCC resistance. T7451 (used on 7050) reaches the T73-equivalent corrosion resistance level with better through-section property uniformity than 7075 achieves at any temper, because 7050's lower quench rate sensitivity allows the interior of thick plate to reach a comparable precipitation state to the surface.


## ASTM Testing Standards


SCC resistance in aluminum alloys is evaluated using several ASTM standards. ASTM G44 describes the alternate immersion test: specimens are cycled between immersion in 3.5% NaCl solution and air exposure, with test durations of 20 to 90 days at specified stress levels. ASTM G47 describes the stress corrosion test in high-humidity environment. ASTM G64 provides a classification system: Class A = no SCC failures at 75% of yield strength in ST orientation; Class B = no failures at 50%; Class C = no failures at 25%; Class D = failures below 25% yield. 7075-T651 in ST orientation is typically Class C or D depending on lot and processing. 7050-T7451 achieves Class A or B. 7075-T7351 typically achieves Class B. These ratings directly inform design allowables for fatigue and sustained load applications.


Alloy / Temper ASTM G64 ST Rating Typical Application


7075-T651 C to D Machined parts, no ST sustained load, no corrosive service


7075-T7351 B Parts with moderate ST load in corrosive environments


7050-T7451 A to B Thick structural plate, SCC-critical aerospace structure


7050-T7651 B Maximum strength with improved SCC vs 7075-T651


6061-T651 A Weldable applications; SCC not a design concern at normal stress levels


## Practical Design Rules for 7xxx Procurement


-


Specify T651 for 7075 plate used in machined components with no sustained ST-direction tensile stress and no corrosive service environment
-


Specify T7351 for 7075 plate when the finished part will carry sustained load in the ST direction or operate in humidity, saltwater, or chloride-containing environments
-


Specify 7050-T7451 (AMS 4050) for sections above 3 inches thick or when SCC resistance in the ST direction is a formal design requirement
-


Protective coating (anodize plus primer, or conversion coat plus primer) dramatically reduces SCC risk by excluding the electrolyte - a coated T651 part in a well-sealed structure is far less vulnerable than an uncoated T7351 part in direct moisture exposure
-


Never stress 7075-T651 in sustained tension across the short-transverse grain direction without a protective coating and explicit design analysis
-


Require mill certs to explicitly state temper designation - accepting T6 plate when T651 was specified introduces unknown residual stress into the part
