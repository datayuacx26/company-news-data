---
schema_version: "1.0.0"
document_id: "941d76bfa498cad8be20f40ab446176bf3f0a0368035706acd3daf7cd4ad4027"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/machining/datum-in-gd-t/"
published_at: "2026-08-13T03:00:11+00:00"
first_seen_at: "2026-08-13T17:41:42.989942+00:00"
fetched_at: "2026-08-13T17:41:52.800977+00:00"
content_hash: "sha256:ac159ee5946b3dadc71e90044eb2b4f4ae608051b4bcbc026a2cf4b8aa1ce2ce"
---

# Datum in GD&T: Definition, Features, and Types in Engineering Drawings

### Why Are Datum Features Important for Inspection?


Datum features are important for inspection because the measurable physical surfaces are required to position and orient manufactured parts accurately and repeatably before any geometric measurement begins. CMM inspection programs probe datum features first to establish the part's coordinate frame. All subsequent position, orientation, and profile measurements are reported within that aligned frame. Selecting an unstable or functionally irrelevant datum feature introduces systematic error into every measurement taken on the part.


Stable datum features reduce probe variation during CMM alignment, gauge setup, and fixture loading. Functional datum features keep inspection results connected to the real assembly contact. Clear datum feature selection improves repeatability across first article inspection, in-process checks, and final quality verification.


#### Can Holes and Cylinders Serve as Datum Features?


Yes, holes and cylinders serve as datum features in GD&T. A cylindrical feature establishes a datum axis rather than a datum plane, referencing the central axis of the cylinder as the geometric origin. Datum axes from bores and locating holes are referenced for runout, perpendicularity, and concentricity controls. In aerospace and automotive assemblies, shaft bores and precision locating holes frequently serve as primary datum features for rotational components. A cylindrical datum feature requires controlled size and roundness to create a reliable derived axis. Fixture pins, mandrels, and CMM probing routines simulate the datum axis from the physical cylinder. Accurate datum axes reduce rotational misalignment during assembly, machining, and final inspection.


## What Are the Main Types of Datums in GD&T?


The main types of datums in GD&T create the ordered reference structure used for part setup, measurement, inspection, and assembly control. Primary, secondary, and tertiary datums work in sequence to remove movement from a manufactured part. The datum order defines which feature establishes stability first, which feature controls alignment next, and which feature fixes the final direction.


The main types of datums in GD&T are listed below.


- **Primary Datum:** The primary datum is the first reference in a datum system. It establishes the main seating plane (constraining 3 degrees of freedom) or primary axis (constraining 4 degrees of freedom) during setup.
- **Secondary Datum:** The secondary datum is the second reference in a datum system. It works after the primary datum and constrains 2 additional degrees of freedom for alignment control.
- **Tertiary Datum:** The tertiary datum is the final reference in a datum system. It constrains the last remaining degree of freedom and completes the Datum Reference Frame.


### 1. Primary Datum


A primary datum is the first datum reference used to establish the datum reference frame during GD&T inspection. The primary datum has the highest precedence in the feature control frame. A planar primary datum usually contacts a datum simulator at 3 points, then controls one translation plus 2 rotations in a 3-plane setup. A datum reference frame works as the coordinate system that defines tolerance zones, inspection origin, part orientation, and part location.


The purpose of a primary datum is to create the most stable starting reference from a functional part surface or feature. A flat mounting face, machined base, large bearing face, hole axis, or center plane becomes a primary datum when the feature controls assembly alignment. The primary datum reduces inspection variation by fixing the part before secondary references enter the setup. The selected feature needs enough surface quality, repeatable contact, and clear drawing identification. Xometry defines a datum as a theoretically ideal line, surface, or feature used as a reference to define tolerance limits, which supports accurate GD&T interpretation.


### 2. Secondary Datum


A secondary datum is the second datum reference used after the primary datum constrains the first part's motions. The secondary datum establishes the next orientation or location reference in the datum reference frame. A planar secondary datum usually contacts a datum simulator at 2 points in a 3-plane setup. The secondary datum then controls 2 remaining degrees of freedom after the primary datum has fixed the first 3 degrees.


The purpose of a secondary datum is to align the part to a second functional feature that reflects assembly behavior. A side face, keyway, hole pattern, slot, or machined wall works as a secondary datum when the feature guides placement relative to the primary datum. The secondary datum must remain perpendicular or otherwise basically related to the primary datum according to the drawing requirement. The secondary datum improves repeatable measurement of hole position, profile, parallelism, perpendicularity, and angularity. Lower precedence datum features control remaining degrees of freedom not already controlled by higher precedence datum features, which keeps inspection order consistent.


### 3. Tertiary Datum


A tertiary datum is the third datum reference used after the primary datum plus secondary datum establish most of the datum reference frame. The tertiary datum normally completes part immobilization in a 3-plane setup. A planar tertiary datum usually contacts a datum simulator at one point. The tertiary datum controls the last remaining degree of freedom after the first 2 datum references constrain the other 5 motions.


The purpose of a tertiary datum is to remove final shifts or clocking during inspection. A small side face, end face, slot edge, pin hole, or stop surface becomes a tertiary datum when the feature completes assembly location. The tertiary datum has lower precedence than the primary datum plus the secondary datum. The tertiary datum must not override the higher-order references. The feature mainly refines the final location, which supports repeatable checks of position tolerances, surface profile tolerances, and runout tolerances. GD&T references commonly place tertiary datum letters after the secondary datum inside the feature control frame, and then measurement fixes the tertiary datum last.


## What Is a Datum Reference Frame (DRF)?


A datum reference frame (DRF) is a three-dimensional coordinate system established from primary, secondary, and tertiary datums to control part geometry during design, manufacturing, and inspection. A DRF gives a part a fixed origin, orientation, and location reference, so geometric tolerances have a stable basis. A datum feature is a real part surface or feature, while a datum is the theoretically exact plane, axis, line, or point derived from the feature. Xometry defines a datum as an ideal reference used to define tolerance limits, which fits the role of DRF in GD&T inspection.


A DRF controls geometry by locking necessary degrees of freedom. A planar primary datum commonly controls 3 degrees of freedom, a planar secondary datum controls 2 more, and a planar tertiary datum controls the final degree of freedom. The datum order matters because higher precedence features establish the inspection setup first. Lower precedence references refine remaining motion after the earlier datum references constrain the part. The DRF keeps measurements consistent across hole location, surface profile, perpendicularity, parallelism, and runout. FARO describes the DRF as the common coordinate system of tolerance zones, which makes inspection results reliable.


### How Does a Datum Reference Frame Control Part Orientation?


A datum reference frame (DRF) controls part location and orientation by constraining all 6 degrees of freedom, which include 3 translational movements (X, Y, Z) and 3 rotational movements (pitch, yaw, roll). The DRF establishes a fixed three-plane structure that locks a part into a defined spatial position. Each plane in the DRF corresponds to a datum feature, labeled sequentially as A, B, and C in order of priority. The primary datum (A) removes three degrees of freedom by establishing full contact with a flat surface. The secondary datum (B) removes two additional degrees of freedom, and the tertiary datum (C) removes the final one. The constrained positioning ensures that every measurement, inspection, and assembly operation references the same fixed orientation. Dimensional deviations are measured from the same origin point across all production runs, maintaining consistency from part to part. Standardized orientation through the DRF directly reduces misalignment errors during assembly, preventing fit failures in mating components. The three-plane hierarchy follows ASME Y14.5 standards, which define how datum features interact with the part geometry. In inspection environments, the DRF aligns with the coordinate system of the measuring equipment, ensuring that reported values reflect actual geometric deviations rather than positioning inconsistencies.


#### Is a Datum Reference Frame Similar to a Coordinate System?


Yes, a datum reference frame is similar to a coordinate system because it defines origin, orientation, and location for part measurement. The two terms are related, but they are not identical. A coordinate system is a mathematical reference based on axes, origin, and spatial values. A datum reference frame is a GD&T reference system derived from datum features shown on an engineering drawing. The DRF converts the drawing requirement into an inspection setup through datum simulators, contact points, planes, axes, or center planes. The DRF gives measurement authority to selected part features, not to an abstract coordinate grid alone. Primary, secondary, and tertiary datums create a reference hierarchy that controls part movement during inspection. The primary datum establishes the first stable reference. The secondary datum refines orientation or location after primary contact. The tertiary datum removes the final remaining motion in a common 3-plane setup. A pure coordinate system assigns X, Y, and Z positions. A datum reference frame assigns physical inspection priority to features used in GD&T.


## What Symbols Are Used for Datums in GD&T?


The symbols that are used for Datums in GD&T are listed below.


- **Datum Feature Symbols:** A datum feature symbol consists of a capital letter enclosed in a square frame, connected to a filled or open triangle placed on the referenced surface or feature. The letter identifies the specific datum (A, B, or C) and links the surface to its role in the DRF. The triangle points directly to the surface line or extension line of the feature being designated.
- **Datum Reference Letters:** Datum reference letters appear inside feature control frames to specify which datums govern a particular tolerance. The letters are listed in compartments following the geometric tolerance value, ordered by priority from left to right. A tolerance controlled by datum A, then B, then C reads in that exact sequence within the control frame.
- **Feature Control Frames:** A feature control frame is a rectangular box divided into compartments that communicate the complete tolerance requirement for a feature. The frame includes the geometric characteristic symbol, tolerance value, material condition modifiers, and datum reference letters. The frame connects directly to the feature it controls through a leader line or direct attachment.
- **Datum Target Symbols:** A datum target symbol is a circle divided horizontally by a line, with the upper half identifying the datum letter and target number and the lower half containing target dimensions or size specifications when applicable. The symbol connects to the specific contact point, line, or area on the part through a leader line with an "X" marking the exact contact location on the drawing view.


### How Are Datum Symbols Applied on Engineering Drawings?


Datum feature symbols are applied on engineering drawings to identify specific surfaces, features, or axes that serve as origin references for inspection and part orientation. The filled or open triangle of the datum symbol attaches directly to the surface line of the referenced feature or to an extension line projected from that surface, depending on the drawing view. When a datum applies to a planar surface, the triangle touches the surface outline or its extension line at a right angle. The symbol attaches to the dimension line of the feature to indicate that the axis, not the surface, is the datum when a datum applies to a cylindrical feature (a bore or shaft). Capital letters (A, B, C) enclosed in square frames label each datum in order of measurement priority. The primary datum letter (A) identifies the surface that makes the most contact and constrains the most degrees of freedom. Letters B and C follow in descending constraint priority. The same letters are then referenced inside feature control frames elsewhere on the drawing, linking geometric tolerances back to the established datum structure. Datum symbols placed on section views or auxiliary views follow the same attachment rules, ensuring the reference remains unambiguous regardless of the drawing projection used. Consistent symbol placement directly supports repeatable inspection setups across different operators and measurement environments.


#### Do Datum Letters Represent Measurement Priority?


Yes, datum letters represent measurement priority in GD&T. The sequence of datum letters within a feature control frame directly communicates the order in which datums are applied during inspection and fixture setup. The first letter listed (primary datum) establishes the most constrained reference, typically a flat surface that removes three degrees of freedom. The second letter (secondary datum) removes two additional degrees of freedom by contacting a perpendicular surface or feature. The third letter (tertiary datum) removes the final degree of freedom by contacting a third mutually perpendicular surface. The priority sequence is not alphabetical by convention; it reflects functional importance to part performance and assembly. A designer assigns the primary datum letter to the surface most critical to the part's function, regardless of whether the letter is A, B, or C. Reversing the order of datum letters in the feature control frame changes the measurement reference hierarchy, producing different inspection results for the same part. Datum letter priority also governs how fixtures are designed for production and how CMM measurement routines are programmed.


## What Are Datum Targets?


Datum targets are specific, defined contact points, lines, or areas designated on a part drawing to establish datum planes on irregular, cast, forged, or flexible parts where full surface contact is impractical or unreliable. Rather than referencing an entire surface as a datum, datum targets define precise locations where tooling, fixtures, or gauges physically contact the part. A datum target point is a single contact location represented by an "X" on the drawing. A datum target line is a controlled linear contact, specified with a length value. A datum target area is a defined contact zone with a specific shape and size, typically circular or rectangular, marked with crosshatch lines on the drawing. Three datum target points on the primary datum plane, two points on the secondary plane, and one point on the tertiary plane establish a stable 3-2-1 locating scheme. The 3-2-1 locating principle removes all six degrees of freedom from the part during inspection or machining. Datum targets are governed by ASME Y14.5 and appear on drawings using the datum target symbol, which is a circle split horizontally with the target identification in the upper half.


### Why Are Datum Targets Used Instead of Full Surfaces?


Datum targets are used instead of full surfaces when a part's geometry does not provide a stable, flat, or consistent contact area for establishing a datum reference. Cast parts, forged components, and machined blanks frequently have surface irregularities (warping, draft angles, and non-planar faces) that make full surface contact unreliable and non-repeatable. A full surface contact datum on an irregular part shifts the measurement origin based on where the high points of the surface happen to land, introducing variability from part to part. Datum targets eliminate the variability by defining exact contact locations that remain fixed regardless of surface condition. The controlled contact points also reduce the influence of surface roughness, parting lines, and flash on casting or forging datum establishment. Repeatability is directly measurable when datum targets are used, as each fixture or CMM setup contacts the same geometric locations on every part. For parts with tight tolerances, a shift of even 0.05 mm in the datum contact location translates directly into measurement error. Datum targets standardize the contact geometry, ensuring that inspection results reflect true part deviation rather than setup inconsistency.


#### Can Flexible Parts Require Special Datum Targeting?


Yes, flexible parts require special datum targeting due to their tendency to deform under their own weight or under contact forces during inspection. Sheet metal components, rubber seals, plastic panels, and thin-walled structures change shape when placed in an unsupported condition, causing standard datum references to produce unrepeatable results. ASME Y14.5 addresses flexible part inspection through the designation of a restrained condition, where the part is fixtured in a specified configuration that simulates the assembled state before measurements are taken. Datum targets for flexible parts specify not only the contact location but the applied force or fixture constraint required to hold the part in the restrained position. Contact areas for flexible parts are kept small and precisely located to prevent localized deformation at the datum contact zone itself. In aerospace sheet metal and automotive body panel applications, datum target areas are sized to distribute contact load without distorting the reference surface. The restrained condition note appears on the drawing alongside the datum target symbols to inform inspection personnel of the required setup.


## What Are the Applications of Datums in Manufacturing?


The applications of datums in manufacturing are listed below.


- **CNC Machining Alignment:** Datums define the origin point and orientation of the workpiece coordinate system within a CNC machine. The machine's work offset is registered to the datum features, ensuring that every cutting operation begins from the same spatial reference. Misalignment from the datum by 0.01 mm translates directly into dimensional error relative to the datum features.
- **Coordinate Measuring Machine (CMM) Inspection:** CMM inspection programs reference datum features to establish the measurement coordinate system before probing any part features. The CMM aligns its probe path to the DRF, ensuring that all reported deviations are measured from the correct origin. Datum-aligned CMM inspection eliminates operator-dependent variation in part placement.
- **Assembly Positioning:** Datums identify the reference surfaces and features that align mating components during assembly. Assembly fixtures locate parts against their datum features to ensure that holes, faces, and profiles align within the specified tolerance. Consistent datum-based assembly reduces rework rates and improves first-pass yield.
- **Fixture Design:** Fixtures are designed to contact a part at its designated datum features, replicating the DRF in a physical holding device. The 3-2-1 locating principle guides fixture pin and rest placement to match primary, secondary, and tertiary datum assignments. Fixture accuracy directly determines the repeatability of all machining or inspection operations performed while the part is fixtured.
- **Tolerance Verification:** Geometric tolerances stated in feature control frames are verified by measuring feature deviations relative to the DRF established by the referenced datums. Without a defined datum reference, geometric tolerances (perpendicularity, position, and angularity) have no fixed origin from which deviation is calculated. Datum-referenced tolerance verification ensures that pass/fail judgments are consistent across all inspection setups.
- **Precision Aerospace Manufacturing:** Aerospace components require datum structures that maintain positional tolerances as tight as ±0.025 mm across complex multi-feature assemblies. Primary datum surfaces on aerospace structural parts are machined to flatness values of 0.01 mm to 0.05 mm to ensure DRF stability. Datum continuity from machining through final assembly inspection is a formal requirement in aerospace quality systems (AS9100).


### Why are Datums Important in CNC Machining?


Datums are fundamental to machining work because each cutting operation needs a fixed spatial reference to calculate tool paths. The workpiece coordinate system, or WCS, registers to part datum features. The setup establishes the origin point used by the machine controller during axis movement. The machine repeats the same cutting geometry across a production run after the datum feature location and the WCS origin are set. Dimensional accuracy depends on precise datum feature contact during fixture setup. A datum surface deviation of 0.02 mm in the fixture transfers the same 0.02 mm error into features machined from the datum. Repeatable datum registration supports multi-operation sequencing, where a part is refixed from one datum setup to another without accumulating positional error. Datum references on an engineering drawing show the CNC programmer the surfaces that define the WCS origin. Datum selection becomes a design decision that affects production accuracy. Operations (milling, turning, and EDM) rely on datum consistency to hold tolerances from ±0.005 mm to ±0.125 mm, depending on process, material, and[CNC machining](https://www.xometry.com/resources/machining/what-is-cnc-machining) .


#### Are Datums Essential for CMM Inspection?


Yes, datums are essential for CMM inspection because the measurement coordinate system of the CMM must align with the DRF defined on the engineering drawing before any feature measurement is valid. The CMM probes the datum features of the part first, constructing the datum planes mathematically from the collected contact points. The measurement software aligns the part's coordinate system to the DRF, so that all subsequent feature measurements are reported relative to the correct datum origin. Without datum alignment, the CMM reports feature positions relative to an arbitrary machine coordinate origin, producing values that cannot be compared to drawing tolerances. A misaligned datum setup on a CMM shifts every position measurement by the same error vector, causing all features to appear out of tolerance or all within tolerance incorrectly. CMM inspection programs reference datum letters from the feature control frame to determine which datum planes to construct and in what priority sequence. Datum-to-CMM alignment accuracy directly governs the measurement uncertainty of the inspection process, with datum registration errors contributing 20% to 40% of total measurement uncertainty in typical precision inspection setups. The[CMM inspection](https://www.xometry.com/resources/industry-design-tips/inspection-options/) process for complex aerospace and automotive components routinely references 3 to 6 datum features to fully constrain the part's measurement coordinate system.


#### Disclaimer


*The content appearing on this webpage is for informational purposes only. Xometry makes no representation or warranty of any kind, be it expressed or implied, as to the accuracy, completeness, or validity of the information. Any performance parameters, geometric tolerances, specific design features, quality and types of materials, or processes should not be inferred to represent what will be delivered by third-party suppliers or manufacturers through Xometry’s network. Buyers seeking quotes for parts are responsible for defining the specific requirements for those parts. Please refer to our[terms and conditions](https://www.xometry.com/legal/) for more information.*
