---
schema_version: "1.0.0"
document_id: "5383a21f3ffd0f4c868f5aecf99f78b83f65e1c8e29cb6628c80aac0f9d64d4f"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/machining/true-position-tolerance/"
published_at: "2026-08-13T03:00:11+00:00"
first_seen_at: "2026-08-13T17:41:42.989942+00:00"
fetched_at: "2026-08-13T17:41:52.800977+00:00"
content_hash: "sha256:5a7f515c7a9f5100669358881e081aa16a90296020585c3090f8474fa56d1130"
---

# True Position Tolerance: Definition, Calculation, and GD&T Applications

True position tolerance is a Geometric Dimensioning and Tolerancing (GD&T) control used to specify the allowable variation in the location of a feature relative to its theoretically exact position. True position tolerance applies a circular or cylindrical tolerance zone around the theoretical center point, defining the maximum permissible deviation from a coordinate, datum-referenced position. Position tolerance ranks among the most used geometric controls for holes, pins, slots, and patterns of features across mechanical assemblies and machined components. Engineers apply **true position** to ensure proper assembly, interchangeability, and alignment between mating components across high-volume and precision manufacturing programs.


Industrial applications of true position span CNC machining, aerospace assemblies, automotive components, precision manufacturing, fixtures, and inspection systems. CNC-machined parts rely on position tolerance to control hole patterns for bolt circles, dowel pin locations, and mating fastener arrays across production runs. Aerospace and automotive assemblies apply position tolerance to control critical mating interfaces where multiple components must align within tight dimensional limits. Coordinate measuring machines (CMM) and functional gauges verify position tolerance compliance during inspection, confirming that manufactured features fall within the specified tolerance zone before parts proceed to final assembly.


## What Is True Position Tolerance?


True position tolerance is a GD&T control that defines the permissible deviation of a feature's location from its theoretically exact position using a tolerance zone. The tolerance zone forms a circle or cylinder centered on the theoretically exact position, with the diameter value specifying the maximum allowable location deviation. Datums establish the reference frame from which the theoretically exact position is calculated, anchoring the tolerance zone to specific reference surfaces or axes on the part.


Position tolerance controls feature location more effectively than conventional coordinate tolerancing, since coordinate tolerancing creates a rectangular zone that permits greater deviation along the diagonal axis than along the X or Y axis alone. The circular tolerance zone of true position allows 57% more usable tolerance area compared to an equivalent square coordinate tolerance zone. The larger usable area reduces unnecessary part rejection during inspection, improving manufacturing yield across hole patterns, pin locations, and fastener arrays.


### Why Is Feature Location Important in Engineering Design?


Feature location is important in engineering design because accurate placement ensures proper assembly, alignment, load distribution, and interchangeability between mating parts. Misaligned holes, pins, or slots prevent fasteners from engaging correctly, causing assembly delays, increased rework, and potential structural failure under operational load. Precise feature location distributes mechanical stress evenly across mating interfaces, preventing localized stress concentration that leads to premature fatigue cracking or component failure. Interchangeability between mass-produced parts depends directly on consistent feature location, allowing components from different production batches to assemble without custom fitting or selective matching. Aerospace, automotive, and precision manufacturing sectors rely on accurate feature location to maintain dimensional consistency across thousands of identical parts, supporting reliable mass production and reduced quality control costs.


#### Does True Position affect Elongation at Break?


No, true position does not affect elongation at break. True position is a GD&T control that governs the locational accuracy of a feature (hole, pin, or slot) relative to its theoretical exact position on a rigid or semi-rigid component. Elongation at break measures a material's mechanical deformation capacity before fracture under tensile load, a property determined entirely by molecular composition, crosslink density, and material processing conditions. The two concepts apply to separate engineering domains, where true position governs dimensional and geometric accuracy, while material properties govern mechanical performance independently of feature location. A part machined with precise true position tolerance carries the same[Elongation at Break](https://www.xometry.com/resources/materials/elongation-at-break/) value as an identical part with poor positional accuracy.


## How Does True Position Tolerance Work?


True position tolerance works by establishing a theoretical exact location for a feature, then defining a tolerance zone around that location within which the actual feature must fall. Theoretically exact dimensions (basic dimensions) define the perfect, untoleranced coordinate location of a feature, derived from datum references on the part. A feature control frame specifies the position symbol, tolerance zone diameter, and applicable datum references in a fixed sequence, governing how the tolerance applies to the feature. Datum references establish the coordinate origin and orientation from which the theoretically exact position is calculated, anchoring the tolerance zone to specific reference surfaces or axes. The tolerance zone forms a circle or cylinder centered on the theoretically exact position, with the diameter value defining the maximum allowable deviation in any direction. Positional variation evaluation compares the actual feature center against the theoretical center, confirming compliance when the deviation falls within the specified tolerance zone diameter during inspection.


### What Is the Theoretically Exact Position of a Feature?


The theoretically exact position of a feature is the perfect, untoleranced coordinate location defined by basic dimensions before any tolerance applies. Basic dimensions establish the exact intended location of a feature relative to specified datum references, expressed in a rectangular box on engineering drawings to distinguish them from toleranced dimensions. The theoretically exact position functions as the center point around which the position tolerance zone forms, anchoring the allowable deviation to a fixed coordinate origin. Datum references define the coordinate system orientation from which basic dimensions are calculated, ensuring the theoretically exact position remains consistent across multiple features on a single part. Manufacturing and inspection processes evaluate actual feature locations against the theoretical center, determining compliance based on whether the measured deviation falls within the specified position tolerance zone.


#### Does True Position Use Basic Dimensions?


Yes, true position uses basic dimensions. Basic dimensions define the theoretically exact location of a feature relative to specified datum references, serving as the untoleranced coordinate origin around which the position tolerance zone centers. Engineering drawings express basic dimensions inside a rectangular box, distinguishing them from standard toleranced dimensions that carry plus or minus deviation limits. The position tolerance value, rather than the basic dimension itself, defines the allowable deviation from the theoretically exact location during manufacturing and inspection. Coordinate measuring machines and functional gauges reference the basic dimension values to establish the theoretical center point, then verify whether the actual feature location falls within the specified position tolerance zone diameter.


## What Is a Position Tolerance Zone?


A position tolerance zone is the allowable region within which the feature axis, center plane, or center point exists relative to its theoretically exact position. The zone forms a circle for two-dimensional features (holes viewed in a flat plane) or a cylinder for three-dimensional features (holes evaluated through their full depth), centered directly on the basic dimension coordinate. The diameter value specified in the feature control frame defines the boundary of the zone, establishing the boundary within which the feature axis or center point must lie, where the maximum permissible radial deviation from the theoretical center is half the specified diameter value.


Manufacturing and inspection processes confirm compliance when the actual feature axis or center point falls entirely within the specified tolerance zone boundary. Coordinate measuring machines calculate the deviation between the actual feature location and the theoretical exact position, comparing the resulting value against the zone diameter to determine acceptance. Features exceeding the tolerance zone boundary fail inspection, requiring rework, scrap disposition, or engineering review before proceeding to final assembly.


### How Does a Cylindrical Tolerance Zone Work?


A cylindrical tolerance zone works by establishing a three-dimensional cylindrical boundary centered on the true position, within which the actual feature axis must fall along its entire depth. Hole and pin locations rely on cylindrical tolerance zones rather than flat circular zones, since the features extend through material thickness and require positional control at each depth increment. The cylinder diameter, specified in the feature control frame, defines the maximum permissible deviation of the feature axis from the theoretically exact position at any point along the cylinder length. Coordinate measuring machines evaluate the feature axis by sampling multiple points along the hole or pin depth, calculating the actual axis orientation and comparing it against the cylindrical boundary. A feature axis that remains within the cylindrical zone across its full length passes inspection, while any deviation beyond the boundary at any depth point results in a positional tolerance violation.


#### Is a Position Tolerance Zone Different From Cylindricity Tolerance?


Yes, a position tolerance zone is different from cylindricity tolerance. A position tolerance zone controls the location of a feature axis or center point relative to a theoretically exact position established through datum references and basic dimensions. Cylindricity tolerance controls the form of a cylindrical surface itself, limiting variation in roundness, straightness, and taper along the feature without reference to any datum or theoretical location. Position tolerance requires datum references to establish the coordinate origin for the theoretically exact position, while cylindricity tolerance applies independently of any datum reference. A hole can maintain a perfectly round and straight bore that satisfies **Cylindricity Tolerance** , yet still violate position tolerance if the bore axis sits outside its specified location relative to the part's datum structure.


## How Is True Position Specified on Engineering Drawings?


True position specification on engineering drawings requires a defined set of standardized elements that together establish the location, reference frame, and acceptable deviation of a feature. Each element serves a specific function within the positional control system, from establishing the coordinate origin to defining the allowable tolerance zone size. Omitting any required element creates ambiguity during manufacturing and inspection, leading to inconsistent interpretation across production and quality control teams.


True positions are specified on engineering drawings in the ways listed below.


- **Feature Control Frame** : A rectangular frame containing the position symbol, tolerance zone value, and applicable datum references in a fixed sequence. The frame attaches directly to the toleranced feature through a leader line or extension line on the drawing.
- **Datum References** : Reference surfaces, axes, or points that establish the coordinate origin and orientation for the theoretically exact position. Datums appear in the feature control frame in order of precedence, controlling rotational and translational degrees of freedom.
- **Basic Dimensions** : Untoleranced coordinate values that define the theoretically exact location of a feature relative to specified datum references. Basic dimensions appear inside a rectangular box on the drawing, distinguishing them from standard toleranced dimensions.
- **Material Condition Modifiers** : Symbols (Maximum Material Condition or Least Material Condition) applied to the tolerance value or datum reference, allowing additional positional tolerance as the feature size departs from its specified limit. Material condition modifiers expand the usable tolerance zone under bonus tolerance principles defined in ASME Y14.5.
- **Tolerance Value Designation** : The numerical value specifying the diameter of the cylindrical or circular tolerance zone within which the feature axis or center point must fall. The tolerance value appears immediately after the position symbol inside the feature control frame, preceded by a diameter symbol for cylindrical zones.


### What Information Is Included in a Position Feature Control Frame?


The information included in a position feature control frame consists of the position symbol, tolerance value, datum references, and optional material condition modifiers, arranged in a fixed sequence within compartmentalized boxes. The position symbol (a circle with two centered crosshairs) appears first, identifying the geometric control type applied to the feature. The tolerance value follows, preceded by a diameter symbol when the zone applies cylindrically, specifying the maximum allowable deviation from the theoretical exact position. Datum references occupy the subsequent compartments of precedence, establishing the coordinate origin and orientation from which the theoretical exact position is calculated. Material condition modifiers (Maximum Material Condition or Least Material Condition) appear as optional symbols attached to the tolerance value or datum reference, permitting bonus tolerance as the feature size departs from its specified limit.


#### Do Datum References Affect Position Tolerance Evaluation?


Yes, datum references affect position tolerance evaluation. Datum references establish the coordinate origin and orientation from which the theoretically exact position is calculated, directly anchoring the position tolerance zone to specific reference surfaces, axes, or points on the part. The order of precedence assigned to datum references in the feature control frame controls which rotational and translational degrees of freedom each datum constrains during evaluation. Changing the datum reference sequence or substituting a different datum feature alters the coordinate system orientation, shifting the calculated theoretical exact position and the resulting tolerance zone location. Coordinate measuring machines establish the part's datum reference frame first, before calculating feature locations, since any deviation in datum setup propagates directly into the position tolerance evaluation results.


## What Are Material Condition Modifiers in Position Tolerancing?


Material condition modifiers in position tolerancing define how the allowable position tolerance value changes relative to the actual produced size of a toleranced feature. Each modifier establishes a different relationship between feature size variation and the resulting positional tolerance zone, allowing engineers to optimize tolerance allocation based on the functional requirement of the feature. ASME Y14.5 defines three material condition states that apply to size-related geometric tolerances, each carrying a distinct symbol within the feature control frame.


The material condition modifiers in position tolerancing are listed below.


- **Maximum Material Condition (MMC)** : The condition where a feature contains the maximum amount of material permitted by its size tolerance, represented by the largest pin diameter or smallest hole diameter. Position tolerance at MMC allows bonus tolerance as the feature departs from its maximum material size, expanding the usable tolerance zone toward the least material limit.
- **Least Material Condition (LMC)** : The condition where a feature contains the minimum amount of material permitted by its size tolerance, represented by the smallest pin diameter or largest hole diameter. Position tolerance at LMC allows bonus tolerance as the feature departs from its minimum material size, prioritizing wall thickness control over assembly clearance.
- **Regardless of Feature Size (RFS)** : The condition where the specified position tolerance applies at each produced feature size, without bonus tolerance regardless of size variation within the tolerance range. RFS constitutes the default material condition state when no modifier symbol appears in the feature control frame, applying the most restrictive tolerance interpretation.


### How Does MMC Affect True Position Tolerance?


Maximum Material Condition (MMC) affects true position tolerance by allowing bonus tolerance to accumulate as the feature departs from its maximum material size limit. The position tolerance value specified in the feature control frame applies directly when the feature measures exactly at its maximum material condition, representing the tightest allowable tolerance zone. As the actual feature size departs from the maximum material limit toward the least material limit, the difference adds directly to the specified position tolerance, expanding the usable tolerance zone diameter. A hole produced larger than its MMC size gains additional positional tolerance equal to the size departure, since a larger hole provides greater clearance for mating fastener insertion regardless of slight positional deviation. The bonus tolerance principle under **MMC** reduces part rejection rates during inspection, since features departing from their tightest size limit receive proportionally greater positional tolerance allowance.


#### Can Position Tolerance Include Bonus Tolerance?


Yes, position tolerance can include bonus tolerance. Bonus tolerance applies when a material condition modifier (Maximum Material Condition or Least Material Condition) appears in the feature control frame alongside the position tolerance value. The bonus tolerance amount equals the difference between the feature's actual produced size and its specified material condition limit, adding directly to the stated position tolerance value. A hole toleranced at Maximum Material Condition gains bonus tolerance as its actual diameter increases beyond the maximum material size, expanding the usable tolerance zone toward the least material limit. Position tolerance specified as Regardless of Feature Size excludes bonus tolerance entirely, applying the stated tolerance value uniformly across the full size range of the feature.


## How Is True Position Measured and Inspected?


True position measurement and inspection require equipment capable of establishing the part's datum reference frame, then calculating the actual feature location against its theoretically exact position. Each inspection method offers a distinct balance of accuracy, speed, and cost, suiting different production volumes and tolerance precision requirements. Datum-based measurement techniques underpin each inspection method, since the position tolerance evaluation depends entirely on correctly establishing the coordinate origin before measuring feature deviation.


True position is measured and inspected in the ways listed below.


- **Coordinate Measuring Machines (CMMs)** : A CMM uses a probe to capture precise X, Y, and Z coordinates of a feature, then calculates the deviation between the actual feature center and its theoretical exact position. CMMs deliver measurement accuracy ranging from 1 to 5 micrometers, suiting low-volume and high-precision inspection requirements across aerospace and medical component manufacturing.
- **Functional Gauges** : A custom-fabricated tool that physically simulates the mating part geometry, verifying whether a feature falls within its position tolerance zone through direct go or no-go acceptance. Functional gauges suit high-volume production environments, delivering rapid pass or fail results without requiring detailed coordinate data collection.
- **Optical Inspection Systems** : An optical inspection system uses cameras, structured light, or laser scanning to capture feature geometry without physical contact, calculating positional deviation through image processing algorithms. Optical systems suit delicate, small-scale, or high-speed inspection applications where contact probing risks part damage or measurement delay.
- **Datum-Based Measurement Techniques** : Datum-based measurement establishes the part coordinate system from specified datum features before calculating any positional deviation, ensuring measurement results align with the drawing's reference frame. Incorrect datum setup propagates directly into position tolerance evaluation errors, regardless of the measurement equipment accuracy applied during inspection.


### How Does a CMM Evaluate True Position?


A Coordinate Measuring Machine (CMM) evaluates true position by capturing coordinate measurements of a feature and comparing the measurements against the datum reference frame and theoretically exact feature location. The CMM probe first contacts the specified datum features, establishing the coordinate origin and orientation from which all subsequent measurements are calculated. The probe then samples multiple points on the toleranced feature (a hole bore or pin surface), calculating the actual feature axis or center point through software algorithms. The software compares the calculated actual center against the theoretical exact position derived from basic dimensions, determining the positional deviation distance and direction. The CMM reports the deviation value against the specified tolerance zone diameter, confirming compliance when the actual feature center falls within the allowable zone boundary across the full inspected feature depth.


#### Are Functional Gauges Used for Position Verification?


Yes, functional gauges are used for position verification. A functional gauge is a custom-fabricated tool machined to replicate the mating part geometry at the worst-case material condition, physically simulating the assembly interface during inspection. The gauge incorporates pins or holes positioned at the theoretically exact location, sized to the maximum or minimum material condition limit specified in the feature control frame. A part passes inspection when the toleranced features accept the gauge pins or holes without interference, confirming the actual feature locations fall within the allowable position tolerance zone. Functional gauges suit high-volume production environments, delivering rapid go-or-no-go acceptance results without requiring detailed coordinate data collection or computational analysis.
