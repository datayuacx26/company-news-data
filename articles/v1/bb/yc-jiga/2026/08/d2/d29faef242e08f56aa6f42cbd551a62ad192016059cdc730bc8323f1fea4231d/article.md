---
schema_version: "1.0.0"
document_id: "d29faef242e08f56aa6f42cbd551a62ad192016059cdc730bc8323f1fea4231d"
company_key: "yc-jiga"
company: "Jiga"
source_id: "yc-jiga-news-import-07676ebb04d8"
canonical_url: "https://jiga.io/articles/knurling-for-production-parts/"
published_at: "2026-08-18T15:22:26+00:00"
first_seen_at: "2026-08-20T01:58:45.018415+00:00"
fetched_at: "2026-08-20T01:58:46.131739+00:00"
content_hash: "sha256:42ef5c7be80aeaefa9766ded9d863179b7b5a056b46b1e163fc0df847a660af1"
---

# Knurling for production parts: Form vs. cut, blank diameters, and how to specify it right

A knurl is one of the simplest features you can add to a machined part. It is also one of the easiest to get wrong.


A production drawing that simply notes “knurl OD” or specifies a generic diamond pattern leaves several critical manufacturing and[tolerance decisions](https://jiga.io/articles/part-tolerances-for-cnc) open to interpretation. Should the feature be form knurled or cut? Has the blank diameter been adjusted so the pattern tracks correctly? Will the finished diameter remain within tolerance after the material is displaced?


Prototype quantities often tolerate these ambiguities. Production volumes rarely do.


They become critical when machining thousands of shafts, thumbscrews, knobs or threaded inserts where every component must look identical, fit consistently and remain within dimensional tolerance.


Most articles on knurling stop after explaining that it improves grip and showing the common pattern types. Production engineers need different information. They need to understand which process produces the most consistent results, how blank diameter affects tracking, what causes double-tracked or “drunken” knurls, and how to communicate these requirements clearly on an engineering drawing.


This guide focuses on those production decisions. It explains the trade-offs between form and cut knurling, the role of diametral pitch, how to calculate an appropriate blank diameter and how to specify knurls in accordance with[ANSI/ASME B94.6](https://www.asme.org/codes-standards/find-codes-standards/b94-6-knurling) so suppliers can manufacture them consistently at production volume.


## What knurling is


Knurling is a secondary machining operation that creates a regular pattern of ridges on the outside diameter of a cylindrical component. Depending on the application, the pattern may improve grip, increase friction, provide anti-rotation features for press-fit components, improve retention in overmoulded plastic assemblies or simply create a controlled decorative finish.


Unlike conventional turning, form-knurling does not remove material to generate its geometry, where cut-knurling does. In the most common process, specially profiled wheels deform or displace the material to produce a repeatable surface pattern.


For production engineers, however, the important question is not what a knurl looks like.


It is how that pattern is produced, because the manufacturing method influences dimensional accuracy, surface quality, material behaviour and production repeatability.


For production engineers, the critical issue is not the pattern, but how the pattern is generated.


## Form vs. cut knurling: The decision that drives everything


The single most important decision when specifying a knurl is selecting the manufacturing process.


Although both methods can produce visually similar patterns, they generate the geometry in fundamentally different ways and behave very differently in production.


Choosing the wrong process can result in oversized parts, distorted thin-wall components, torn stainless steel surfaces or inconsistent tracking across production batches.


### Form knurling


Form knurling, sometimes called roll knurling or bump knurling, produces the pattern by plastically deforming the workpiece. Hardened knurl wheels are pressed into the rotating component, displacing material upward into the familiar ridges without removing metal. Because the material is displaced rather than cut, the surface layer tends to work-harden slightly, which is worth noting during process planning even though it isn’t a reason to choose form knurling on its own.


Because material is displaced rather than cut, the finished outside diameter becomes larger than the original turned diameter.


Form knurling displaces material upward and outward from the original surface as hardened wheels are pressed into the rotating part, so the finished OD grows beyond the pre-knurl diameter. Alt text: Cross-section diagram of a form-knurl wheel pressing into round bar stock, showing the toothed wheel profile and the raised ridges it forms as material is displaced upward from the original diameter.


This growth is often overlooked during design.


If the blank diameter is not reduced before knurling, the finished component may exceed its specified tolerance even though the knurl itself appears perfect.


Advantages of form knurling include:


- Fast production cycle times.


- Excellent repeatability on suitable materials.


- No chips generated.


- Strong, well-defined surface pattern.


However, significant radial forces are generated during forming. Thin-wall components, long unsupported shafts and less rigid parts may deflect during machining.


Material selection also becomes important. Ductile alloys such as aluminum, brass and low-carbon steel generally form clean, consistent patterns.


This shows the form knurl process, where the toothed wheels are clamped to impress their shape into the bar, displacing material to form the tooth profile. This straight knurl is the simplest profile, where a mirrored helix knurl would result from two mirror image wheels whose patterns overlap.


### Cut knurling


Cut knurling creates the same visual pattern by removing material rather than displacing it.


Instead of forcing material upward, specially designed cutting wheels machine the profile into the workpiece.


Because material is removed, the finished diameter remains much more predictable.


The cutting forces are also significantly lower, making cut knurling well suited to:


- Thin-wall components.


- Long, slender shafts.


- Tight diameter tolerances.


- Harder stainless steels.


- Difficult-to-form alloys.


Cut knurling generally requires more machining time than form knurling, but the improved dimensional control often outweighs the additional cycle time for precision components.


It is particularly valuable where the knurled diameter interfaces with bearings, seals or mating components that cannot tolerate dimensional growth.


The slowest and most precise cut-knurling method uses a broaching approach, where a single-point cutter passes over the surface, cutting each groove individually and indexing the part to present the next cut. This shows a straight, axial knurl, but a 4-axis CNC machining center or a fully capable CNC lathe can cut more complex helical, diamond (mirrored helical) or spline-based patterns using the same method. Alt text: Diagram of a single-point broaching tool cutting individual axial grooves into a rotating cylindrical workpiece, indexing between each cut.


The more widely used and faster technique for cut-knurling uses rotating cutters of varied tooth pitch and attack angle to cut multiple features in a single pass. This technique is most commonly applied using a CNC or manual lathe. Alt text: Diagram of a multi-tooth rotary cutting wheel engaging a rotating workpiece to cut several knurl grooves simultaneously.


### Choosing the right process


There is no single,


*correct* answer, it’s the result of an informed


[DFM process](https://jiga.io/cnc-machining/cnc-machining-dfm)


Instead, evaluate the feature using five engineering targets/references.


Consideration Form Knurling Cut Knurling


Dimensional accuracy Moderate Excellent


OD growth Yes Minimal


Thin-wall components Less suitable Preferred


Hard or work-hardening materials Less suitable Preferred


High-volume cycle time Excellent Moderate


As a general rule:


Choose form knurling when production speed and cosmetic appearance matter most and the material is ductile.


Choose cut knurling when dimensional accuracy or component rigidity outweighs cycle time.


Material behavior is one of the biggest swing factors in that decision, covered in full later in this guide.


**CTA:** Manufacturing reviews should evaluate knurled features before production begins. Selecting the correct process early helps prevent unnecessary scrap, tolerance issues and supplier rework.


## Why material matters


Material behaviour influences knurl quality as much as tool selection.


Ductile alloys such as aluminium, brass and mild steel readily flow into the knurl pattern during form knurling, producing crisp, well-defined ridges.


Stainless steels behave differently.


Grades such as 303 and 304 tend to work-harden during machining. If excessive pressure or incorrect feeds are used during form knurling, the material may tear instead of flowing cleanly into the pattern.


For these alloys, many manufacturers prefer cut knurling together with slower spindle speeds and greater tool engagement to produce cleaner results.


The same principle applies to tougher engineering alloys. As material strength increases, cut knurling generally provides greater process stability and more predictable dimensional control. Titanium alloys, however, are rarely form knurled because high forming forces and limited ductility tend to accelerate tool wear and degrade surface finish.


## Designing knurls for manufacture


Choices must be made in selecting knurl type, depth, etc.


Design Consideration Manufacturing Guidance Why It Matters


Choose the right knurling method Use form knurling for ductile materials and general-purpose parts. Use cut knurling for hard materials, thin walls and tight tolerances. Form knurling displaces material and increases OD, while cut knurling maintains better dimensional control.


Allow for OD growth Turn the blank undersize before form knurling. Specify the finished knurled diameter where it is functionally important. Form knurling plastically displaces material, increasing the outside diameter.


Select an appropriate blank diameter Calculate the starting diameter to match the chosen diametral pitch. Avoid arbitrary stock sizes. Incorrect blank diameters cause double-tracking ("drunken" knurls), poor appearance and scrap.


Provide adequate wall thickness Thin-walled components generally benefit from cut knurling or a coarser pattern with reduced forming pressure. Excessive forming loads can distort or collapse thin sections.


Use reliefs and runouts Add relief grooves or short unknurled sections adjacent to shoulders and threads. Gives the tool room to enter and exit cleanly without damaging adjacent features.


Match pattern to function Diamond patterns maximize grip, straight knurls suit press fits and linear retention, while diagonal patterns are often selected for appearance or directional grip. The pattern affects grip, insertion force and load distribution.


Consider assembly requirements For overmolded or press-fit parts, specify a knurl that provides mechanical retention without creating excessive insertion force. Knurls often serve as a retention feature rather than simply improving hand grip.


Account for material behavior Aluminum, brass and mild steel form cleanly. Stainless steels and titanium frequently require slower feeds, higher forming loads or cut knurling. Work-hardening alloys are more prone to tearing, poor fill and rapid tool wear.


Specify only functional length Knurl only the area needed for grip or retention. Reduces cycle time, tooling wear and unnecessary deformation.


Dimension the feature clearly Specify pattern, pitch (DP), length and finished diameter in accordance with ASME B94.6. Removes ambiguity and improves repeatability across suppliers.


Good knurl design balances function with manufacturability. Selecting the correct knurling method, blank diameter and material-specific process allows suppliers to produce consistent,


[repeatable features](https://jiga.io/articles/cnc-precision-machining) at production volumes.


## Patterns and pitch: More than just appearance


Once the manufacturing method has been selected, the next decision is the knurl pattern itself.


Most engineers recognize straight, diagonal and diamond knurls. Fewer consider how pattern selection affects manufacturability, tracking and interchangeability between suppliers.


For production components, pattern geometry should be specified with the same care as thread form or surface finish.


### Straight knurls


Straight knurls consist of parallel ridges running along the axis of the component.


They are commonly used where rotational grip is less important than linear engagement or alignment.


Typical applications include:


- Sliding adjustment collars


- Press-fit inserts


- Alignment features


- Decorative rings


Because only one set of teeth is generated, straight knurls generally produce lower forming forces than diamond patterns.


### Diagonal knurls


Diagonal knurls use a single helical pattern, usually at a standard helix angle.


They are less common on production parts but are occasionally selected where directional grip or specific assembly characteristics are required.


Since only one helix is formed, they also generate lower deformation forces than a full diamond knurl.


### Diamond Knurls


Diamond knurls are by far the most common production specification.


Two opposing diagonal patterns intersect to produce the familiar cross-hatched texture found on:


- Thumbscrews


- Hand knobs


- Tool handles


- Instrument controls


- Adjustment wheels


- Overmolded inserts


Diamond knurls provide excellent grip regardless of rotation direction while producing a visually consistent finish.


Most production drawings specifying a grip surface will call for a diamond pattern.


Patterns and pitches of knurl in common use. Custom patterns exist well beyond these standards, for particular applications and aesthetics. Alt text: Reference diagram comparing straight, diagonal, and diamond knurl patterns side by side, showing tooth orientation for each.


## Diametral pitch: The number that really matters


Pattern selection is only part of the specification.


The spacing between those teeth determines whether the finished knurl tracks correctly.


ANSI/ASME B94.6 standardizes the diametral pitch (DP) system used for production knurling. Rather than specifying teeth per inch directly, the standard defines the relationship between the workpiece diameter and the number of teeth produced.


Common standard pitches include:


Diametral Pitch Typical Use


64 DP Coarse knurls (generally avoided)


96 DP General production standard


128 DP Medium-fine features


160 DP Fine precision components


Among these, 96 DP has become the practical default for many production applications because it balances grip, appearance and manufacturability. The standard itself recommends avoiding 64 DP where possible.


### DP versus TPI


Engineers occasionally encounter specifications expressed in teeth per inch (TPI) rather than diametral pitch.


Although related, they are not interchangeable systems.


Typical equivalents include:


DP Straight TPI 30° Diagonal TPI


96 30.8 35.6


128 41.1 47.4


160 51.2 59.1


Using the ASME B94.6 diametral pitch designation removes ambiguity and provides greater consistency across tooling suppliers.


## Getting the blank diameter right


This is the section almost every online knurling guide skips.


Yet it is often the difference between a clean production knurl and a rejected batch of components.


Unlike turning or grinding, form knurling depends on the relationship between the workpiece diameter and the pitch of the knurl wheel.


If the blank diameter is incorrect, the teeth cannot register consistently around the circumference.


The result is a pattern that appears to wander, overlap or drift as the tool advances.


Machinists commonly refer to this as a double-tracked or drunken knurl. The problem is rarely caused by poor tooling. It usually begins with the blank diameter relationship to the feature size/pitch.


### The basic relationship


For diametral-pitch knurls, the relationship is straightforward:


**Blank Diameter = Number of Teeth ÷ Diametral Pitch**


Production shops often keep standard blank diameter tables for commonly used DP values rather than calculating every feature individually. This attention ensures the circumference contains a whole number of knurl teeth, allowing the pattern to repeat cleanly without interference.


In practice, standard DP knurls are designed to track correctly on common fractional diameters.


For example:


A 96 DP knurl applied to 0.500 in stock produces 48 teeth around the circumference.


Because the circumference matches the tooth spacing exactly, each revolution follows the previous one without creating overlapping tracks.


### Worked Example


Suppose a component requires a diamond knurl on a nominal ½-inch shaft.


Rather than selecting an arbitrary blank diameter, begin with the intended knurl pitch.


**Specification**


- Pattern: Diamond


- Pitch: 96 DP


- Nominal stock: 0.500 in


A 96 DP tool generates 48 teeth around the circumference of a ½-inch blank.


That integer relationship allows the knurl wheels to track cleanly from the beginning of the operation through to completion.


If the blank diameter is changed without considering this relationship, tooth engagement gradually shifts.


The pattern no longer registers correctly.


### What Causes Double-Tracked Knurls?


A properly tracked knurl produces crisp, evenly spaced diamonds. A mismatched blank diameter produces something very different: instead of each tooth following the previous impression, the knurl wheels begin cutting a second path between the first, and the defect becomes increasingly obvious over longer knurled sections.


The result is:


- Split ridges


- Irregular diamonds


- Blurred intersections


- Poor visual appearance


- Reduced grip consistency


Although experienced machinists can sometimes compensate by adjusting blank diameter or tooling selection, preventing the issue at the design stage is considerably more reliable. Cut depth adjustments change ridge height, not tooth count, so they do not correct a diametral mismatch once tracking has already gone wrong.


### Design before you machine


Blank diameter is not simply a machining setup variable.


It is a design input.


Selecting the appropriate diameter before releasing the drawing helps ensure:


- repeatable production,


- consistent appearance,


- reduced scrap,


- longer tool life,


- and interchangeability across qualified suppliers.


It also removes unnecessary trial-and-error from production, particularly when components are manufactured by multiple suppliers using different machines.


**CTA:** Design-for-manufacturing reviews should verify knurl method, diametral pitch and blank diameter before production begins. Correcting these features in CAD is significantly less expensive than correcting them after machining.


## Material behavior: Why some alloys knurl better than others


Hardness, ductility and resistance to deformation determine how a knurling tool performs, regardless of the material grade stamped on the certification sheet. Those material properties largely determine whether a knurl forms cleanly or produces torn ridges, excessive burrs or dimensional variation.


For production work, material selection should influence the knurling method just as much as the pattern itself.


### Ductile materials


Materials that deform readily generally produce the cleanest form-knurled surfaces.


Typical examples include:


- Aluminum alloys


- Brass


- Copper


- Low-carbon steels


Because these materials plastically deform with relatively low forming forces, the displaced metal fills the knurl profile uniformly, producing crisp ridges with minimal surface tearing.


For high-volume production, these materials are ideal candidates for form knurling.


### Stainless steel


Stainless steels require more careful consideration.


Grades such as 303 and 304 are well known for work hardening.


As the knurl wheels deform the surface, the material rapidly becomes harder. If excessive pressure is applied or the tool hesitates during engagement, the surface may tear rather than flow into the profile.


The result can include:


- Ragged ridge peaks


- Surface tearing


- Inconsistent diamond geometry


- Reduced cosmetic quality


- Increased tool wear


For these materials, many manufacturers favor cut knurling, particularly where dimensional accuracy and surface quality are critical. Slower spindle speeds and a more positive tool engagement also help maintain consistent results.


### Component rigidity matters too


Material is only part of the equation. A solid thumbwheel behaves very differently from a thin-wall tube, and because form knurling applies substantial radial force, long or lightly supported components may deflect during machining.


Typical examples include:


- Thin-wall sleeves


- Long actuator shafts


- Hollow handles


- Precision instrument components


Even when the material itself forms well, deflection can distort the finished geometry.


Where rigidity is limited, cut knurling often provides better dimensional control because cutting forces are significantly lower.


A basic reference for material and process choices to get the best results. Alt text: Decision-reference chart matching common materials (aluminum, brass, mild steel, stainless, titanium) to recommended knurling process and process notes.


## How to specify a knurl on an engineering drawing


A surprisingly large number of production drawings specify knurls using only a leader note such as:


**DIAMOND KNURL**


That description identifies the appearance, but little else.


It tells the supplier nothing about pitch, manufacturing method or dimensional requirements.


If different suppliers use different tooling, the resulting parts may all satisfy the drawing while looking noticeably different.


Like threads, gears or splines, knurls should be defined using recognized standards.


For diametral-pitch knurls, that standard is ANSI/ASME B94.6.


### What should a drawing specify?


A complete production callout should include:


- Pattern (straight, diagonal or diamond)


- Diametral pitch


- Knurled length


- Diameter before knurling, where critical


- Finished diameter, where critical


- Applicable standard


For example:


**DIAMOND KNURL, 96 DP, PER ASME B94.6. BLANK Ø0.500 IN (48 TEETH). FINISHED Ø0.510–0.515 IN.**


That single note communicates considerably more manufacturing intent than a simple “diamond knurl,” and it uses the same 96 DP / 0.500 in blank relationship worked through earlier in this guide, so the teeth count is verified rather than assumed.


It also reduces supplier interpretation and improves repeatability when multiple manufacturing sources are involved.


Drawing details needed to fully define a knurl. Alt text: Annotated engineering drawing callout showing pattern type, diametral pitch, applicable standard, blank diameter, and finished diameter range for a knurled feature.


### Specify functional requirements, not just appearance


The drawing should communicate why the knurl exists.


For example:


- Grip surface


- Press-fit retention


- Plastic overmold retention


- Anti-rotation feature


- Cosmetic finish


Knowing the functional requirement helps determine whether dimensional accuracy or grip performance should receive priority.


A cosmetic thumbwheel may tolerate small dimensional variation.


A knurled insert destined for overmolding into an engineering polymer may rely on precise ridge geometry to achieve consistent pull-out strength.


### Include finished dimensions where they matter


One of the most common production errors occurs when the drawing specifies only the pre-machined diameter.


For form knurling, this is rarely sufficient because the outside diameter increases during material displacement.


Where the knurled diameter interfaces with another component, specify the finished dimension or an acceptable range after knurling.


This removes uncertainty for both the machinist and the inspector.


## Common failure modes in production


Most knurling problems are predictable.


They are usually caused by design decisions rather than machining capability.


The most common production issues include:


Failure Typical Cause


Double-tracked ("drunken") pattern Incorrect blank diameter


Oversized finished diameter Blank not turned undersize before form knurling


Torn ridge profile Work-hardening material or excessive forming pressure


Flattened diamonds Worn tooling or insufficient penetration


Component distortion Excessive forming force on thin-wall or unsupported parts


Inconsistent appearance between suppliers Incomplete drawing specification


Premature tool wear Incorrect material or excessive forming pressure


Notice that very few of these failures originate with the knurl wheel itself.


Most begin with design intent, process selection or incomplete documentation.


Addressing those factors during design-for-manufacturing review is considerably less expensive than discovering them during incoming inspection.


## Getting knurled parts made right


Successful knurling depends on much more than selecting a pattern.


The machining process, material behaviour, blank diameter, tooling and drawing specification all influence the final result.


When several suppliers are involved, maintaining consistency becomes even more challenging.


One supplier may optimise for cycle time.


Another may prioritise cosmetic appearance.


A third may substitute different tooling while remaining technically compliant with the drawing.


This is where a supplier of record provides value.


Rather than simply sourcing machined components, the supplier of record ensures that:


- the selected knurling method matches the application,


- blank diameter is appropriate for the specified pitch,


- production follows the engineering drawing,


- inspection confirms dimensional compliance,


- and finished components remain consistent across production batches.


That single point of accountability reduces engineering effort while improving quality and repeatability.


**CTA**


Jiga works as a supplier of record for precision machined components, coordinating qualified manufacturers from our extensive and actively curated network, while ensuring critical features such as knurls, threads, secondary finishing and inspection remain consistent from prototype through production.


## Conclusion


Knurling may appear to be a simple machining operation, but successful production depends on a series of engineering decisions made long before the first component reaches the lathe.


Selecting between form and cut knurling determines how material behaves, how dimensions change and how consistently parts can be manufactured at volume.


Choosing the correct diametral pitch and blank diameter ensures the pattern tracks cleanly instead of producing costly double-tracked defects.


Finally, a complete ASME B94.6 drawing callout removes unnecessary supplier interpretation and improves consistency across production runs.


For production engineers, a knurl should be judged as a feature that performs its intended function, stays within tolerance, and can be manufactured repeatedly by qualified suppliers, not merely as a textured surface.


When design intent, manufacturing method and supplier accountability are aligned, knurling becomes another predictable, controlled element of a high-quality production process.


Like threads or bearing fits, knurls should be treated as engineered features rather than cosmetic textures. Correct process selection and specification make them predictable manufacturing features rather than variable workshop operations.


## Frequently Asked Questions


What is the difference between form and cut knurling?


Form knurling displaces material to create the pattern, increasing the finished outside diameter. Cut knurling removes material, producing lower cutting forces and better dimensional control. Cut knurling is often preferred for tight tolerances, thin-wall components and work-hardening materials.


Why is blank diameter important for knurling?


The blank diameter determines whether the knurl teeth track correctly around the circumference. An incorrect diameter can produce a double-tracked or “drunken” pattern, resulting in poor appearance and reduced production quality.


What is the most common diametral pitch?


ANSI/ASME B94.6 defines several standard pitches, including 64, 96, 128 and 160 DP. For many production applications, 96 DP is the preferred general-purpose choice because it provides a good balance of grip, appearance and manufacturability.


How should a knurl be specified on a drawing?


A complete drawing callout should identify the pattern, diametral pitch, applicable standard, knurled length and any critical dimensions before and after knurling. Where dimensional accuracy is important, specify the required finished diameter rather than relying solely on the blank diameter.


When should cut knurling be used instead of form knurling?


Cut knurling is generally preferred for harder materials, work-hardening stainless steels, thin-wall components, long unsupported shafts and applications where dimensional accuracy is more important than production speed.
