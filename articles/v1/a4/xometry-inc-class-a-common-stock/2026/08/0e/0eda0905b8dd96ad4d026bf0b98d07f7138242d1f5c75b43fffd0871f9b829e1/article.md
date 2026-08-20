---
schema_version: "1.0.0"
document_id: "0eda0905b8dd96ad4d026bf0b98d07f7138242d1f5c75b43fffd0871f9b829e1"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/machining/cylindricity/"
published_at: "2026-08-13T03:00:11+00:00"
first_seen_at: "2026-08-13T17:41:42.989942+00:00"
fetched_at: "2026-08-13T17:41:52.800977+00:00"
content_hash: "sha256:e7db0bdcabd9cceccb23d22a3e066b62dd304ba12006b1887f6f42cc9ca756b4"
---

# Cylindricity: Definition, Measurement, and GD&T Applications

Cylindricity is a form tolerance that controls the shape of a cylindrical feature by combining circularity and straightness requirements into a single tolerance zone. Cylindricity ensures an entire cylindrical surface remains uniformly round and straight along its full length, applying without reference to any datum feature. The control evaluates roundness, straightness, and taper simultaneously, capturing surface irregularities that form controls miss when assessed independently. Engineers apply **cylindricity** to improve fit, sealing performance, rotational accuracy, and wear characteristics in cylindrical components across precision manufacturing programs.


Industrial applications of cylindricity span shafts, bearings, hydraulic cylinders, precision machined components, aerospace systems, and automotive assemblies. Shafts and bearing journals rely on cylindricity control to maintain consistent rotational accuracy and minimize vibration during high-speed operation. Hydraulic cylinders apply cylindricity to ensure proper seal contact along the bore surface, preventing fluid leakage and pressure loss during operation. Aerospace and automotive assemblies depend on cylindricity to control wear distribution and maintain dimensional stability across rotating and sliding mating interfaces throughout the component's service life.


## What Is Cylindricity?


Cylindricity is a GD&T form control that specifies how closely a cylindrical surface must conform to a perfect cylinder throughout its entire length and circumference. The tolerance zone forms two concentric perfect cylinders, with the actual surface required to fall entirely between the inner and outer boundary at cross-section and along the full feature length. Cylindricity applies without reference to any datum feature, evaluating the surface independently against its own derived axis rather than an external coordinate system.


Unlike size tolerances, cylindricity controls the geometric form of the complete cylindrical surface, capturing roundness, straightness, and taper deviations simultaneously within a single tolerance value. A shaft can satisfy its diameter size tolerance while still containing localized bulges, tapers, or out-of-round sections that violate a specific cylindricity callout, since size tolerances allow form variations within the limits of size unless refined by a tighter form control. Cylindricity inspection requires comprehensive surface data collection across multiple cross-sections and axial positions, confirming the entire surface remains within the specified tolerance zone boundary.


### Why Is Cylindrical Form Important in Engineering Applications?


Cylindrical form is important in engineering applications because accurate roundness and straightness directly affect assembly fit, bearing performance, sealing effectiveness, rotational balance, and load distribution. Shafts and bores with poor cylindrical form create uneven clearance distribution during assembly, causing binding, excessive play, or misalignment between mating components. Bearing journals depend on consistent cylindrical form to maintain uniform contact pressure across the rolling element surface, since localized form deviations accelerate wear and reduce bearing service life. Hydraulic and pneumatic cylinder bores rely on tight cylindrical form to maintain continuous seal contact along the full stroke length, preventing fluid or air leakage during operation. Rotating shafts with poor cylindrical form generate uneven mass distribution and vibration during high-speed operation, leading to premature fatigue failure and reduced rotational balance across precision-machined components in modern[Engineering Applications](https://www.xometry.com/applications-engineering-team/) .


#### Does Cylindricity Control the Entire Surface of a Cylinder?


Yes, cylindricity controls the entire surface of a cylinder. The tolerance zone forms two concentric perfect cylinders, requiring each point across the full circumference and axial length to fall between the inner and outer boundaries. Cylindricity evaluates roundness at each cross-section, straightness along each longitudinal line element, and taper across the full feature length, capturing form deviations that localized measurements miss. The control applies independently of any datum reference, assessing the surface against its own derived axis rather than an external coordinate frame. Comprehensive surface data collection across multiple cross-sections and axial positions confirms whether the complete cylindrical surface remains within the specified tolerance zone, rather than verifying isolated points or sections alone.


## How Does Cylindricity Work in GD&T?


Cylindricity works in GD&T by establishing two concentric perfect cylinders that define the tolerance zone boundary, requiring the actual surface to fall entirely between the inner and outer cylinder walls. The radial distance between the inner and outer cylinder equals the specified tolerance value, defining the maximum permissible variation in surface radius at any point across the feature. The tolerance zone applies without reference to any datum feature, evaluating the cylindrical surface against its own derived axis rather than an external coordinate system established through datum references.


Full-surface evaluation requires data collection across multiple cross-sections along the feature length and multiple points around each cross-sectional circumference, capturing roundness, straightness, and taper deviations simultaneously. Geometric form verification confirms compliance when each collected surface point falls within the tolerance zone boundary, with any single point exceeding the limit resulting in a cylindricity violation. Coordinate measuring machines and dedicated roundness measurement equipment evaluate dense point sampling, since accurate cylindricity verification depends on capturing sufficient surface data to detect localized form irregularities.


### What Is the Cylindricity Tolerance Zone?


The cylindricity tolerance zone is the space between two perfectly concentric cylinders within which the entire surface must lie across its full length and circumference. The outer cylinder defines the maximum allowable boundary, while the inner cylinder defines the minimum allowable boundary, with the radial distance between them equal to the specified tolerance value. The two cylinders share a common axis, derived directly from the actual feature surface rather than from any external datum reference. The actual cylindrical surface must fall entirely within the annular space at each cross-section and axial position along the feature length. A surface containing even a single point outside the tolerance zone boundary, whether bulging beyond the outer cylinder or recessing within the inner cylinder, results in a cylindricity violation regardless of how closely the remaining surface conforms.


#### Does Cylindricity Require Datum References?


No, cylindricity does not require datum references. Cylindricity evaluates a cylindrical surface against its own derived axis, established directly from the actual surface data collected during inspection rather than from any external coordinate reference. The tolerance zone forms two concentric perfect cylinders centered on the self-referencing axis, independent of any datum feature specified elsewhere on the part. Form tolerances (cylindricity, circularity, straightness, and flatness) apply without datum references under ASME Y14.5, since the controls evaluate the inherent shape of a feature rather than its location or orientation relative to other features. The absence of datum dependency distinguishes cylindricity from location and orientation controls (position, perpendicularity, and parallelism), which rely on **Datum References** to establish the coordinate frame against which deviation is calculated.


## What Surface Errors Does Cylindricity Control?


Cylindricity controls multiple surface error types simultaneously, capturing form deviations that geometric controls miss when assessed independently. Each error type portrays a distinct deviation pattern from a perfect cylindrical form, ranging from cross-sectional roundness errors to longitudinal shape distortions.


The surface errors that cylindricity control is listed below.


- **Circularity Deviations** : Out-of-round conditions at cross-sections, where the surface deviates from a perfect circle at a single axial position. Cylindricity captures circularity errors across each cross-section along the feature length, rather than at an isolated location.
- **Straightness Deviations** : Bending or curvature along the longitudinal axis of the cylindrical feature, causing the surface to deviate from a straight line element. Cylindricity evaluates straightness simultaneously with roundness, detecting bowed or curved shaft and bore conditions.
- **Barrel-Shaped Forms** : A convex form where the cylinder's middle section bulges outward relative to ends, creating a larger diameter at the center than at the extremities. Cylindricity detects barrel-shaped deviations through full-length surface sampling that isolated diameter checks do not reveal.
- **Tapered Forms** : A progressive diameter change along the feature length, where one end of the cylinder measures consistently larger or smaller than the opposite end. Cylindricity flags tapered conditions even when cross-sectional roundness measurements remain within acceptable limits.
- **Hourglass-Shaped Forms** : A concave form where the cylinder's middle section narrows relative to the ends, creating a smaller diameter at the center than at the extremities. Cylindricity detects hourglass deviations through the same full-surface evaluation method applied to barrel-shaped form errors.
- **Surface Waviness Effects** : Repetitive, wave-like undulations across the cylindrical surface, resulting from machining vibration, tool chatter, or improper cutting parameters. Cylindricity captures waviness through dense point sampling, distinguishing it from random surface roughness through wavelength and amplitude characteristics.


### How Does Cylindricity Control Circularity and Straightness Simultaneously?


Cylindricity controls circularity and straightness simultaneously by evaluating cross-sectional roundness and longitudinal straightness as a single combined form requirement within one tolerance zone. The tolerance zone forms two concentric perfect cylinders that span the entire feature length, requiring each cross-sectional circle along that length to fall within the radial boundary while maintaining axial alignment with the derived cylinder axis. A cross-section that satisfies perfect roundness still violates cylindricity if its center point shifts away from the common axis shared by adjacent cross-sections, since the shift introduces a straightness deviation. Conversely, a feature that maintains perfect axial straightness still violates cylindricity if any cross-section deviates from a true circle, since it introduces a circularity deviation. Full-surface data collection across multiple cross-sections and axial positions allows inspection equipment to detect deviation types within the same measurement pass, confirming compliance when roundness and straightness conditions are satisfied together across the complete surface.


#### Can a Cylinder Be Round but Fail Cylindricity Requirements?


Yes, a cylinder can be round but fail cylindricity requirements. Cylindricity evaluates roundness, straightness, and taper simultaneously, meaning a feature satisfying perfect circularity at each cross-section can still violate cylindricity through other form deviations. A shaft with perfectly round cross-sections along its entire length can still fail cylindricity if the feature bends, producing a curved axis rather than a straight one. A bore with consistently round cross-sections at each axial position can fail cylindricity if the diameter slowly increases or decreases along the length, creating a tapered, barrel-shaped, or hourglass-shaped form. Cylindricity inspection requires evaluating the complete surface across multiple cross-sections and axial positions, since circularity measurement alone at isolated locations cannot detect straightness, taper, or full-length form deviations that cause cylindricity violations.


## How Is Cylindricity Specified on Engineering Drawings?


Cylindricity specification requires a defined set of standardized elements that together establish the symbol, tolerance value, and surface application on Engineering Drawings. Each element serves a specific function within the form control system, from identifying the geometric control type to defining the allowable tolerance zone size. The absence of any required element creates ambiguity during manufacturing and inspection, leading to inconsistent interpretation across production and quality control teams.


The elements listed below cover the standardized components required to fully specify cylindricity.


- **Cylindricity Symbol** : A circle with two diagonal tangent lines forming the geometric characteristic symbol, appearing as the first compartment within the feature control frame. The symbol identifies the form control type applied to the toleranced cylindrical feature on[Engineering Drawings](https://www.xometry.com/resources/design-guides/technical-drawing-best-practices/) .
- **Feature Control Frame** : A rectangular frame containing the cylindricity symbol and tolerance value, without any datum reference compartments since cylindricity applies independently of datum features. The frame attaches directly to the toleranced cylindrical surface through a leader line or extension line.
- **Tolerance Value Designation** : The numerical value specifying the radial distance between the two concentric cylinders forming the tolerance zone boundary. The tolerance value appears immediately after the cylindricity symbol inside the feature control frame, without a diameter symbol since the value already constitutes a radial measurement.
- **Surface-Specific Application** : The feature control frame attaches directly to the cylindrical surface itself, rather than to a dimension line, distinguishing cylindricity from size-related tolerances. Proper placement ensures the control applies to the entire surface rather than a single diameter callout.


### How Is the Cylindricity Symbol Applied in GD&T?


The cylindricity symbol applies in GD&T by appearing as the first compartment within a feature control frame, attached directly to the controlled cylindrical feature through a leader line or extension line. The symbol, a circle with two diagonal tangent lines, identifies the form control type without requiring any datum reference compartments in the frame. The tolerance value follows the symbol within the same compartment sequence, specifying the radial distance between the two concentric cylinders forming the tolerance zone. The feature control frame attaches directly to the cylindrical surface itself, rather than to a dimension line, distinguishing cylindricity from size-related tolerances applied to a diameter callout. The direct surface attachment ensures the control applies to the entire cylindrical feature across its full length and circumference, rather than to a single cross-sectional measurement point.


#### Does Cylindricity Affect Feature Size Limits?


No, cylindricity does not affect feature size limits. Cylindricity controls the geometric form of a cylindrical surface, evaluating roundness, straightness, and taper independently of the feature's diameter size tolerance. Size tolerance establishes the upper and lower diameter limits within which the feature must measure, while cylindricity establishes a separate form requirement that the surface must satisfy regardless of where the actual size falls within the limits. A shaft can measure within its specified diameter size tolerance while still violating cylindricity through bulging, tapering, or out-of-round sections that the size tolerance alone does not constrain. ASME Y14.5 controls form through size limits under the default envelope principle (Rule 1), requiring the feature form to remain within the maximum material condition envelope while allowing form tolerances to further refine form independently of size.


## How Is Cylindricity Measured?


Cylindricity measurement requires equipment capable of capturing dense surface data across multiple cross-sections and axial positions, then evaluating the collected points against the derived cylindrical tolerance zone. Each measurement method offers a distinct balance of accuracy, speed, and cost, suiting different production volumes and form precision requirements. Comprehensive surface sampling underpins each measurement method, since cylindricity evaluation depends on detecting localized form irregularities across the complete feature surface.


Cylindricity is measured in the ways listed below.


- **Coordinate Measuring Machines (CMM)** : A CMM uses a probe to capture multiple points across the cylindrical surface at diverse cross-sections and axial positions. The software calculates the derived axis and tolerance zone boundary, delivering flexible measurement capability alongside other geometric tolerances.
- **Roundness Measuring Instruments** : A roundness instrument rotates the part on a precision spindle while a stylus traces the surface at fixed cross-sections. Combining data across multiple axial positions allows the instrument to evaluate straightness and taper alongside circularity.
- **Form Measuring Systems** : A dedicated form measuring system combines rotational and linear scanning axes to capture continuous surface data in a single automated measurement cycle. The systems deliver high-resolution form data, suiting precision components where segmented measurement lacks sufficient accuracy.
- **Precision Metrology Equipment** : Specialized optical or laser-based metrology equipment captures non-contact surface data across cylindrical features, suiting delicate or high-speed inspection applications. Captured data is processed through form-fitting algorithms to calculate cylindricity deviation against the specified tolerance value.


### How Does a CMM Evaluate Cylindricity?


A Coordinate Measuring Machine (CMM) evaluates cylindricity by collecting multiple measurement points across the cylindrical surface and comparing the points against a best-fit ideal cylinder calculated from the collected data. The probe samples points at numerous cross-sections along the feature length, with each cross-section capturing points around the circumference to detect roundness variation. Software algorithms process the complete point cloud to derive a best-fit cylindrical axis that minimizes the deviation across all collected surface points. The software then constructs two concentric cylinders around the derived axis, sized to encompass the maximum and minimum radial deviations found within the data set. The CMM reports the radial distance between the two boundary cylinders as the measured cylindricity value, confirming compliance when the value falls within the specified tolerance limit.


#### Are Roundness Testers Used for Cylindricity Inspection?


Yes, roundness testers are used for cylindricity inspection. A roundness tester rotates the part on a precision spindle while a stylus traces the surface at a fixed axial position, capturing circularity data at that specific cross-section. Repeating the measurement at multiple axial positions along the feature length builds a comprehensive data set covering roundness variation across the entire cylindrical surface. Combining the circularity data from each cross-section allows the tester's software to evaluate straightness and taper alongside roundness, deriving the complete cylindricity value. Roundness testers deliver high-precision form data suited for shafts, bearing journals, and precision machined components where detailed surface form analysis exceeds standard CMM probing resolution.


## What Factors Affect Cylindricity in Manufacturing?


Cylindricity deviations in manufactured parts result from a combination of process, equipment, and environmental factors that introduce form errors during machining. Each factor contributes a distinct deviation pattern, ranging from localized surface irregularities to full-length taper or barrel-shaped distortion. Identifying and controlling the factors reduces cylindricity violations, improving part acceptance rates across precision manufacturing programs.


The factors that affect cylindricity in manufacturing are listed below.


- **Machining Accuracy** : Cutting tool path, spindle rotation, and feed rate control determine the consistency of material removal across the surface. Inconsistent cutting parameters introduce roundness and straightness deviations that accumulate into cylindricity violations.
- **Tool Deflection** : Cutting forces cause the tool to deflect away from its intended path during deep cuts or long, slender features. Tool deflection produces tapered or barrel-shaped deviations that increase as unsupported tool length grows.
- **Workpiece Vibration** : Resonance between the cutting tool and workpiece generates repetitive surface waviness across the feature. Vibration-induced chatter marks create periodic form deviations that isolated cross-section checks cannot detect.
- **Thermal Distortion** : Heat generated during machining causes localized thermal expansion, altering dimensional accuracy as the part cools. Uneven heat distribution produces non-uniform contraction, resulting in taper or barrel-shaped form errors.
- **Fixture Alignment** : Improper workholding introduces clamping forces that distort the workpiece, especially in thin-walled components. Misaligned fixtures cause the feature to deviate from true cylindrical form once clamping forces release.
- **Machine Tool Wear** : Worn spindle bearings, guideways, or ball screws introduce positional inaccuracy during cutting. Accumulated wear over extended production runs slowly degrades cylindricity performance.


### How Does Tool Deflection Influence Cylindricity?


Tool deflection influences cylindricity by causing cutting tool movement away from its intended path under applied cutting forces, resulting in taper, bowing, or uneven material removal across the cylindrical surface. Radial cutting forces push the tool away from the workpiece centerline during turning operations, especially when machining long, slender shafts with insufficient rigidity or support. The deflection magnitude increases with cutting depth, feed rate, and unsupported tool or workpiece length, producing greater material removal inconsistency as the parameters increase. Uneven deflection along the feature length creates a tapered or barrel-shaped form, since the tool removes more or less material at different axial positions depending on the deflection magnitude at that point. Bowing occurs when deflection varies progressively along the feature length, causing the resulting surface to curve away from a straight axis and violate the straightness component of the cylindricity requirement.


#### Can Thermal Expansion Affect Cylindricity Measurements?


Yes, thermal expansion can affect cylindricity measurements. Heat generated during machining or inspection causes the workpiece material to expand, altering the measured dimensions until the part returns to a stable ambient temperature. Uneven heat distribution across the feature length produces non-uniform expansion, introducing apparent taper or barrel-shaped form deviations that do not reflect the part's true geometry once fully cooled. Temperature differences between the workpiece and the measuring equipment introduce measurement uncertainty, since the part and the instrument's reference surfaces expand or contract at different rates depending on material composition. Precision metrology environments control ambient temperature within tight limits, 20°C plus or minus 1°C, allowing parts to stabilize thermally before cylindricity measurement to ensure accurate and repeatable results.


## What Are the Applications of Cylindricity?


Cylindricity tolerance applies across industries where rotational accuracy, sealing performance, and consistent surface contact determine component function and service life. The control ensures cylindrical surfaces maintain uniform roundness and straightness, supporting reliable performance in rotating, sliding, and sealing applications. Component types ranging from bearing journals to aerospace assemblies depend on cylindricity to prevent premature wear, leakage, or rotational imbalance.


The applications of cylindricity are listed below.


- **Bearing Journals** : Cylindricity ensures uniform contact pressure across the rolling element surface, preventing localized wear and extending bearing service life. Consistent cylindrical form reduces vibration and maintains rotational accuracy during high-speed operation.
- **Hydraulic Cylinders** : Cylindricity maintains continuous seal contact along the bore surface throughout the full piston stroke length. Tight cylindrical form prevents fluid leakage and pressure loss during repeated operating cycles.
- **Rotating Shafts** : Cylindricity controls mass distribution and rotational balance, reducing vibration during high-speed rotation. Consistent form along the shaft length prevents premature fatigue failure at stress concentration points.
- **Engine Components** : Cylindricity in cylinder bores and piston pins ensures proper seal contact and consistent clearance throughout combustion cycles. Accurate cylindrical form reduces oil consumption, blow-by, and component wear over engine service life.
- **Medical Devices** : Cylindricity in surgical instruments and implant components ensures precise mating fit and consistent mechanical performance. Tight form control supports biocompatibility requirements and repeatable function across regulated medical device manufacturing.
- **Aerospace Precision Assemblies** : Cylindricity in shafts, pins, and rotating components maintains dimensional stability under extreme operating loads and temperature variation. Strict form control supports flight-critical system reliability across aerospace manufacturing programs.


### Why Is Cylindricity Important for Bearing Surfaces?


Cylindricity is important for bearing surfaces because proper cylindrical form promotes uniform load distribution, reduced wear, and smoother rotation across the rolling element contact area. A bearing journal with consistent roundness and straightness maintains even contact pressure across its full circumference and length, preventing localized stress concentration that accelerates surface fatigue. Out-of-round or tapered bearing surfaces create uneven load distribution, causing contact points to bear disproportionate stress while other areas remain underutilized during rotation. Uneven load distribution accelerates wear at high-stress contact points, leading to premature bearing failure, increased vibration, and reduced rotational accuracy over the component's service life. Bearing surfaces machined within tight cylindricity tolerance maintain smoother rotation with lower friction and reduced heat generation, extending operational life and improving mechanical efficiency across rotating machinery applications.


#### Are Hydraulic Cylinders Commonly Controlled by Cylindricity Requirements?


Yes, hydraulic cylinders are commonly controlled by cylindricity requirements. The cylinder bore surface requires tight cylindricity tolerance to maintain continuous seal contact along the full piston stroke length, preventing fluid leakage between the piston seal and bore wall. Out-of-round or tapered bore surfaces create inconsistent seal compression at different points along the stroke, leading to localized leakage paths and reduced hydraulic pressure efficiency. Barrel-shaped or hourglass-shaped bore deviations cause uneven seal wear, accelerating seal degradation and shortening the maintenance interval for cylinder rebuilding. Manufacturers commonly specify cylindricity tolerances ranging from 0.005 mm to 0.025 mm on precision hydraulic cylinder bores, depending on the operating pressure and sealing system requirements of the application.


## What Are Common Challenges in Achieving Cylindricity?


Common challenges in achieving cylindricity include machining limitations, measurement complexity, thermal effects, equipment capability constraints, and process variation across production runs. Machining limitations arise from tool deflection, spindle runout, and feed rate inconsistency, each introducing localized form deviations that accumulate into cylindricity violations across long or slender features. Measurement complexity presents a challenge, since accurate cylindricity verification requires dense surface data collection across multiple cross-sections and axial positions, demanding more sophisticated equipment and longer inspection cycles than simple diameter checks.


Thermal effects introduce additional difficulty, as heat generated during machining or measurement causes the workpiece to expand unevenly, producing apparent form deviations that do not reflect the part's true geometry once fully cooled. Equipment capability constraints limit achievable cylindricity tolerance, since older or lower-precision machine tools lack the spindle accuracy and guideway stability required for tight form control on demanding applications. Process variation across production runs, driven by tool wear, fixture inconsistency, and material property differences between batches, introduces unpredictable form deviation that complicates maintaining consistent cylindricity across high-volume manufacturing programs.
