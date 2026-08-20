---
schema_version: "1.0.0"
document_id: "fe02ff14dce2299b8e96ca5aa8c09150ed4fcc928e46d199be30d631a9c8d7a2"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/machining/maximum-material-condition/"
published_at: "2026-07-16T03:00:15+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:27b763daefda757e38aa3b603bae43de9c0432184185048c5baee0df134a3fa8"
---

# Maximum Material Condition (MMC): Definition, Tolerance Control, and Function

## How Is MMC Applied to Holes and Shafts?


MMC is applied to holes and shafts through the methods listed below.


- **MMC for Holes** : A hole toleranced at 12.0 mm ±0.2 mm reaches MMC at 11.8 mm, representing its smallest allowable diameter. The smallest diameter leaves the least available space for a mating shaft, establishing the tightest internal boundary at worst-case conditions.
- **MMC for Shafts or Pins** : A shaft toleranced at 11.5 mm ±0.2 mm reaches MMC at 11.7 mm, representing its largest allowable diameter. The largest diameter occupies the most space within a mating hole, defining the tightest external boundary at worst-case conditions.
- **Functional Fit Considerations** : The fit category depends on the relationship between the shaft MMC and the hole MMC. A shaft MMC of 11.7 mm against a hole MMC of 11.8 mm produces a 0.1 mm clearance fit, confirming assembly feasibility at worst-case conditions.
- **Clearance and Interference Relationships** : A shaft MMC exceeding the hole MMC produces an interference fit, requiring press or thermal assembly methods. A shaft MMC below the hole MMC produces a clearance fit, allowing free assembly without mechanical force.


### Why Is MMC Important for Mating Parts?


MMC is important for mating parts because it ensures mating components assemble correctly under worst-case dimensional conditions. Each mating pair carries a combination of size tolerances that accumulate across the feature boundaries of each component. MMC defines the tightest size boundary each feature reaches, giving engineers a measurable worst-case reference for evaluating assembly feasibility before production begins. A shaft at MMC of 11.7 mm mating with a hole at MMC of 11.8 mm confirms a minimum clearance of 0.1 mm, verifying that assembly remains physically possible at the tightest allowable boundaries.


Dimensional variation across production batches increases the risk of interference fits when mating parts fall near their MMC boundaries simultaneously. Engineers apply MMC references during tolerance stack-up analysis to quantify cumulative variation from multiple features across an assembly. A tolerance stack exceeding the available clearance from the hole MMC to the shaft MMC signals a design adjustment before manufacturing begins, preventing costly rework and assembly failures in precision production environments.


#### Can MMC Prevent Assembly Interference?


Yes, MMC can prevent assembly interference. Engineers use MMC values during tolerance analysis to verify that the shaft MMC never exceeds the hole MMC, maintaining a positive clearance at the tightest allowable boundaries. A shaft MMC of 11.7 mm against a hole MMC of 11.8 mm confirms a 0.1 mm minimum clearance, preventing interference at worst-case dimensional conditions. Assemblies with multiple mating features accumulate tolerance stack-ups that narrow the available clearance from each feature boundary, increasing interference risk across the full assembly. MMC analysis quantifies the cumulative effect of size variation across each mating pair, allowing design teams to adjust tolerances before production begins. Identifying interference risks at the design stage through MMC analysis eliminates costly rework and assembly failures in precision manufacturing environments.


## What GD&T Symbols Are Associated With MMC?


The GD&T symbols that are associated with MMC are listed below.


- **MMC Modifier Symbol (Ⓜ)** : The Ⓜ symbol appears inside a feature control frame to indicate that the stated geometric tolerance applies at the maximum material condition of the feature. The symbol activates bonus tolerance, allowing the geometric tolerance to increase as the feature departs from MMC toward LMC.
- **Position Tolerance Controls** : Position tolerance appears in the feature control frame alongside the Ⓜ symbol to control the location of a feature relative to a datum reference frame. A position tolerance of 0.2 mm at MMC on a hole of 10.0 mm ±0.15 mm permits a total position tolerance of 0.5 mm when the hole reaches LMC at 10.15 mm.
- **Datums Referenced at MMC** : Datums referenced at MMC account for the shift allowable when the datum feature departs from its MMC boundary. A datum shaft at MMC of 12.2 mm allows datum shift as its diameter decreases toward LMC, providing additional tolerance for the entire pattern of controlled features.
- **Virtual Condition Concepts** : Virtual condition is the worst-case boundary generated by combining the MMC size limit with the stated geometric tolerance. A shaft with an MMC of 15.0 mm and a straightness tolerance of 0.3 mm produces a virtual condition of 15.3 mm, defining the absolute outer boundary that the feature never exceeds.


### How Is the MMC Modifier Used in Feature Control Frames?


The MMC modifier is used in feature control frames by placing the MMC symbol directly after the geometric tolerance value within the frame compartments. A feature control frame reads from left to right, starting with the geometric characteristic symbol, followed by the tolerance value, the MMC modifier, and the datum references. A positional tolerancing callout on a hole reads as position, 0.2 mm at MMC, datum A, and datum B, indicating that the 0.2 mm position tolerance applies at the MMC boundary of the hole.


The MMC modifier activates bonus tolerance calculation during inspection, linking the allowable geometric variation directly to the measured feature size. A hole at MMC of 9.8 mm carries the stated 0.2 mm position tolerance, while the same hole measured at 10.2 mm gains 0.4 mm of bonus tolerance, reaching a total position tolerance of 0.6 mm. Inspection teams read the feature control frame to determine the applicable tolerance at each measured size, reducing part rejection rates across precision manufacturing environments.


#### Does MMC Change Positional Tolerance Limits?


Yes, MMC changes positional tolerance limits. The stated position tolerance in a feature control frame applies strictly at the MMC boundary of the feature, and the limit increases as the feature departs from MMC toward LMC. A hole toleranced at 10.0 mm ±0.2 mm carries an MMC of 9.8 mm with a stated position tolerance of 0.1 mm. The position tolerance limit increases to 0.5 mm when the hole reaches LMC at 10.2 mm, gaining 0.4 mm of bonus tolerance across the full size departure range. Design teams apply MMC modifiers to positional tolerances when assembly fit remains acceptable across the full size range of the feature, allowing more parts to pass inspection without compromising functional performance.


## What Is Virtual Condition in MMC?


Virtual condition in MMC is the worst-case boundary generated by combining the MMC size limit of a feature with its stated geometric tolerance. The boundary illustrates the absolute limit within which a mating part must fit to guarantee assembly under worst-case dimensional conditions. For an external feature, virtual condition extends beyond the MMC size limit, while for an internal feature, it falls within the MMC size limit. Design teams use virtual conditions to define the space a feature occupies at its absolute worst-case combination of size and geometry.


Virtual condition differs from MMC because it accounts for size variation and geometric error simultaneously. A shaft with an MMC of 20.0 mm and a straightness tolerance of 0.4 mm produces a virtual condition of 20.4 mm, defining the outermost boundary that the shaft never exceeds. A mating hole must carry a virtual condition of at least 20.4 mm to guarantee clearance fit at worst-case conditions. Gauge designers use virtual condition boundaries to set the dimensions of functional gauges, ensuring that each accepted part assembles correctly with its mating component across high-volume production runs.


### How Does Virtual Condition Control Functional Assembly?


Virtual condition controls functional assembly by defining the maximum permissible boundary a mating part must clear to guarantee physical assembly under worst-case conditions. The boundary combines the MMC size limit with the stated geometric tolerance, producing a fixed envelope that accounts for size and form error simultaneously. A shaft virtual condition of 20.4 mm requires the mating hole to provide at least 20.4 mm of clearance at its worst-case boundary.


Gauge designers translate virtual condition boundaries directly into functional gauge dimensions, producing fixed-limit gauges that replicate worst-case mating part geometry. A GO gauge for a mating hole carries a diameter equal to the shaft virtual condition of 20.4 mm, rejecting any hole that fails to clear the gauge. Each part accepting the gauge confirms clearance at the absolute worst-case boundary, guaranteeing functional assembly across the full tolerance range of mating features.


#### Is Virtual Condition Important for Functional Gauging?


Yes, virtual condition is important for functional gauging. Functional gauges use virtual condition boundaries to replicate the worst-case mating part geometry, providing a direct pass or fail result during inspection. A GO gauge for a hole carries a pin diameter equal to the mating shaft virtual condition, rejecting any hole that fails to accept the gauge at its worst-case boundary. A shaft virtual condition of 15.3 mm sets the external diameter of the GO gauge pin, ensuring every accepted hole assembles with its mating shaft under worst-case dimensional conditions. Functional gauging based on virtual condition eliminates the need for complex coordinate measurements, reducing inspection time and operator error across high-volume production environments.


## What Is Bonus Tolerance in MMC?


Bonus tolerance in MMC is the additional geometric tolerance a feature gains when its actual size departs from its MMC boundary toward its LMC boundary. The additional tolerance accumulates in direct proportion to the size departure, adding an equal amount of geometric variation for every unit of size change from MMC. A hole at MMC carries the stated geometric tolerance, and every increment of size increase beyond MMC adds an equivalent increment of bonus tolerance to the original value. Bonus tolerance never applies at MMC and reaches its maximum value when the feature arrives at LMC.


A hole toleranced at 10.0 mm ±0.3 mm carries an MMC of 9.7 mm with a stated position tolerance of 0.2 mm. The hole gains 0.1 mm of bonus tolerance for every 0.1 mm departure from MMC toward LMC at 10.3 mm. At LMC, the total position tolerance reaches 0.8 mm, combining the original 0.2 mm with 0.6 mm of accumulated bonus tolerance. Design teams apply bonus tolerance to increase part acceptance rates without compromising the functional fit requirements of the assembly.


### How Is Bonus Tolerance Calculated?


Bonus tolerance is calculated as the difference between the actual measured feature size and the MMC size of the feature. The calculation produces a direct numerical value representing the additional geometric tolerance available at the measured size. A hole with an MMC of 9.8 mm measured at an actual size of 10.1 mm carries a bonus tolerance of 0.3 mm, derived from the difference between 10.1 mm and 9.8 mm. Adding the bonus tolerance to the stated position tolerance of 0.2 mm produces a total allowable position tolerance of 0.5 mm at that measured size.


The total position tolerance increases linearly with each increment of size departure from MMC. A hole reaching its LMC of 10.4 mm carries a bonus tolerance of 0.6 mm, combining with the stated 0.2 mm to produce a total position tolerance of 0.8 mm. Inspection teams perform the bonus tolerance calculation at each measured feature size to determine the applicable geometric limit, reducing unnecessary part rejection in precision manufacturing environments.


#### Can Bonus Tolerance Improve Manufacturing Flexibility?


Yes, bonus tolerance can improve manufacturing flexibility. A feature departing from MMC gains additional geometric tolerance, expanding the acceptable range of variation without compromising assembly fit. A hole toleranced at 10.0 mm ±0.3 mm with a stated position tolerance of 0.2 mm at MMC reaches a total position tolerance of 0.8 mm at LMC, accepting parts that otherwise fail a fixed tolerance inspection. The expanded tolerance range reduces scrap rates and rework costs across high-volume production runs, allowing manufacturers to maintain acceptable rejection rates without tightening machining parameters. Production teams apply MMC modifiers to positional tolerances when functional fit requirements remain satisfied across the full size departure range of the feature, giving machinists greater dimensional freedom during manufacturing.


## What Are the Applications of MMC in Manufacturing?


The applications of MMC in manufacturing are listed below.


- **Precision Machining** : MMC defines the worst-case size boundary for machined features, ensuring mating components meet functional fit requirements at the tightest allowable dimensions. A bored hole toleranced at 25.0 mm ±0.15 mm reaches MMC at 24.85 mm, setting the tightest boundary that inspectors verify during post-machining measurement.
- **Automotive Assemblies** : MMC governs the fit from shaft diameters to bearing bores in drivetrain and suspension assemblies, preventing interference at worst-case dimensional conditions. Engine crankshaft journals, toleranced at MMC, ensure consistent clearance fits with connecting rod bearings across high-volume production runs.
- **Aerospace Component Alignment** : MMC controls positional tolerances on fastener holes and structural interfaces, guaranteeing alignment under worst-case size conditions. A bolt hole pattern toleranced at MMC ensures each fastener clears its mating hole at the tightest allowable boundary across assembled aerospace structures.
- **Functional Gauge Inspection:** Functional gauges use MMC-based virtual condition boundaries to replicate worst-case mating part geometry, producing direct pass or fail inspection results. A GO gauge pin diameter set at the hole virtual condition of 15.3 mm rejects any mating hole failing to clear the worst-case boundary.
- **Computer Numerical Control (CNC) Manufacturing** : CNC machining centers apply MMC references during process planning to set cutting parameters that keep features within their MMC boundaries. A milled slot toleranced at MMC of 8.0 mm minimum width allows operators to adjust tool paths within the bonus tolerance range as the feature departs from MMC.
- **High-Volume Interchangeable Part Production** : MMC ensures dimensional consistency across large production batches, guaranteeing that any part from the batch assembles with any mating component. Interchangeable fastener assemblies, toleranced at MMC, maintain consistent clearance fits from 0.05 mm to 0.2 mm across production volumes exceeding 10,000 units.


### Why Is MMC Important in Precision Machining?


MMC is important in precision machining because it supports tight assembly control by defining the worst-case size boundary each feature reaches within its tolerance range. Precision-machined components (bearing housings, spindle shafts, hydraulic valve bores) require consistent dimensional boundaries to guarantee functional fit across mating assemblies. A bearing bore toleranced at 50.0 mm ±0.025 mm reaches MMC at 49.975 mm, setting the tightest internal boundary that inspectors verify during post-machining measurement. MMC allows efficient manufacturing tolerances by activating bonus tolerance as features depart from their MMC boundaries. A shaft toleranced at 25.0 mm ±0.1 mm with a position tolerance of 0.05 mm at MMC gains additional geometric freedom as its diameter decreases toward LMC, reducing scrap rates without compromising assembly fit across high-volume production environments.


#### Are Functional Gauges Designed Around MMC Principles?


Yes, functional gauges are designed around MMC principles. A functional gauge replicates the worst-case mating part geometry by setting its gauge dimensions equal to the virtual condition boundary of the controlled feature. A GO gauge pin for a mating hole carries a diameter equal to the hole virtual condition, rejecting any hole that fails to clear the worst-case boundary. A shaft with an MMC of 12.0 mm and a straightness tolerance of 0.2 mm produces a virtual condition of 12.2 mm, setting the exact GO gauge ring diameter inspectors use during verification. Functional gauges designed around MMC principles reduce inspection time and operator error by replacing complex coordinate measurements with direct pass or fail results across high-volume production environments.


## How Does MMC Compare to RFS?


Maximum Material Condition (MMC) and Regardless of Feature Size (RFS) are two separate tolerance modifier conditions applied within GD&T to control geometric variation across mating features. MMC links the stated geometric tolerance directly to the feature size, activating bonus tolerance as the feature departs from its MMC boundary toward LMC. RFS applies the stated geometric tolerance at each actual feature size without any bonus tolerance, maintaining a fixed geometric limit regardless of where the feature falls within its size range. MMC suits applications where assembly fit governs the design requirement, while RFS suits applications where geometric accuracy remains critical at each produced size.


A hole toleranced at 10.0 mm ±0.2 mm with a position tolerance of 0.1 mm at MMC gains bonus tolerance as its diameter increases beyond 9.8 mm. The same hole under RFS maintains a fixed position tolerance of 0.1 mm at every measured size from 9.8 mm to 10.2 mm. MMC increases part acceptance rates in high-volume production, while RFS maintains strict geometric control in precision applications (medical devices, optical instruments, aerospace guidance systems) where functional performance depends on consistent geometric accuracy at every produced size.


### What Is the Difference Between Maximum Material Condition and Least Material Condition?


Maximum Material Condition and Least Material Condition depict opposite size boundaries within the allowable tolerance range of a feature. MMC defines the state where a feature carries the greatest amount of material, while LMC defines the state where a feature carries the least amount of material. A shaft toleranced at 15.0 mm ±0.2 mm reaches MMC at 15.2 mm and LMC at 14.8 mm, with the full size range spanning 0.4 mm from boundary to boundary. External features reach MMC at their largest diameter and LMC at their smallest diameter, while internal features follow the opposite relationship. A hole toleranced at 20.0 mm ±0.25 mm reaches MMC at 19.75 mm and LMC at 20.25 mm. Bonus tolerance accumulates as a feature departs from MMC toward LMC, reaching its maximum value at the **Least Material Condition** boundary of the feature.


#### Is RFS the Default GD&T Condition?


Yes, the Regardless of Feature Size (RFS) is the default GD&T condition. The American Society of Mechanical Engineers (ASME) Y14.5 standard establishes RFS as the default modifier for all geometric tolerances when no material condition symbol appears in the feature control frame. A position tolerance of 0.15 mm stated without an MMC or LMC modifier applies at each actual feature size within the tolerance range, maintaining a fixed geometric limit regardless of the measured size. A hole toleranced at 12.0 mm ±0.2 mm with a position tolerance of 0.15 mm under RFS carries the same 0.15 mm limit at each size from 11.8 mm to 12.2 mm. Design teams specify RFS in applications where geometric accuracy must remain consistent at each produced size, independent of the actual feature size measured during inspection.
