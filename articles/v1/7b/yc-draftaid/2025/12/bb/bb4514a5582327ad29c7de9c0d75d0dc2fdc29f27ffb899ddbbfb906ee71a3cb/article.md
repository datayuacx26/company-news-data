---
schema_version: "1.0.0"
document_id: "bb4514a5582327ad29c7de9c0d75d0dc2fdc29f27ffb899ddbbfb906ee71a3cb"
company_key: "yc-draftaid"
company: "Draftaid"
source_id: "yc-draftaid-news-import-10f3d93d5772"
canonical_url: "https://draftaid.io/blog/weld-symbols-drawings/"
published_at: "2025-12-23T10:58:50+00:00"
first_seen_at: "2026-07-27T01:55:41.495149+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:9f12c1ec558cebc60cbb5c1a44d11418942a045300c13f5fed0de7cc879e9eb7"
---

# Using Weld Symbols on Drawings: A Practical Guide

Weld symbols are used on engineering drawings to specify the type, size, location, and extent of welds without relying on written notes. They provide a compact way to communicate fabrication requirements that would otherwise be unclear or open to interpretation.


In practice, weld symbols sit at the intersection of design intent and fabrication reality. A correctly designed joint can still fail to meet requirements if the drawing does not communicate how it should be welded. This often results in rework, delays, or disputes between design, fabrication, and inspection.


Weld symbols provide joint-specific instructions that fabricators and inspectors reference at the point of execution, while general notes provide broader context. They must be unambiguous, consistent, and aligned with the applicable standard. Small differences in placement or notation can change how a weld is executed.


This guide explains how weld symbols are used on engineering drawings, how to interpret their components, and how to apply them clearly in practice.


## What a Weld Symbol Represents on a Drawing


A weld symbol represents an instruction, not a process description. Its purpose is to define what the finished weld must achieve at a specific location on the part or assembly. How that weld is produced is usually left to the fabricator unless the drawing explicitly states otherwise.


On an[assembly drawing](https://draftaid.io/blog/assembly-drawings/) , the weld symbol links the geometry of the joint to the required weld characteristics. It tells the fabricator **where the weld is needed, what type of weld is required, and any constraints on size, length, or finish.** When interpreted correctly, it removes the need for assumptions.


This distinction is important because weld symbols are sometimes misunderstood as shorthand for welding methods. In most cases, weld symbols are used to specify weld geometry and placement, not the welding process itself.


That said, the optional tail of the weld symbol is sometimes used to reference a welding process, procedure, or standard. Whether this information is placed in the symbol tail, general notes, or a separate specification depends on company practice and the level of control required.


If the welding method matters to performance, quality, or compliance, it is typically specified separately in drawing notes, a welding procedure specification (WPS), or a referenced standard. If no method is specified, it is generally acceptable to leave process selection to the fabricator, provided the finished weld meets the defined requirements.


From a design perspective, weld symbols are part of the overall functional definition of a component. They affect strength, fatigue performance, distortion, cost, and inspectability. Poorly specified welds can introduce unnecessary manufacturing complexity or compromise performance.


From a fabrication and inspection perspective, weld symbols are read alongside dimensions, tolerances, and notes. If the symbol is unclear or conflicts with other information on the drawing, the risk of incorrect execution increases. This is why consistency and standard compliance matter.


A well-applied weld symbol allows all parties to interpret the drawing in the same way. It supports clear communication between design, fabrication, and inspection without requiring additional clarification.


## Standards Governing Weld Symbols


Weld symbols are not free-form notation. Their meaning is defined by formal standards, and those standards must be applied consistently on a drawing. Using weld symbols without reference to a recognized standard creates ambiguity, especially when drawings are shared across suppliers, regions, or inspection bodies.


Most confusion around weld symbols in practice comes from mixing conventions from different standards or assuming they are interchangeable. They are not.


### Common Standards


Several standards define weld symbol notation, but two account for the majority of[engineering drawings](https://draftaid.io/blog/engineering-drawings/) used in industry.


#### AWS A2.4


[AWS A2.4](https://pubs.aws.org/p/1999/a242020-standard-symbols-for-welding-brazing-and-nondestructive-examination) is the primary standard used in North America. It defines the symbols, placement rules, and supplementary markings commonly seen on drawings produced to AWS or ASME practices.


#### ISO 2553


[ISO 2553](https://www.iso.org/obp/ui/en/#iso:std:iso:2553:ed-5:v2:en) is widely used in Europe and many other regions. While it serves the same purpose as AWS A2.4, the notation system differs in several important ways, including symbol placement and reference line conventions.


#### ASME drawing standards


ASME Y14 drawing standards reference AWS weld symbols rather than defining a separate system. In practice, drawings produced to ASME conventions still rely on AWS A2.4 for weld symbol interpretation.


### Key Differences Between AWS and ISO notation


Although AWS and ISO symbols describe the same physical welds, the way information is presented on the drawing is not the same. The most significant difference is how the symbol is placed relative to the reference line.


Under AWS conventions, weld symbols placed below the solid reference line indicate that the weld is on the arrow side of the joint, while a symbol placed above the line indicates that the weld is on the other side of the joint.


The ISO standard defines two systems - system A and system B. The former being more widely adopted uses a dual line convention (solid with dashed line below), while the latter uses a single solid line.


The location of dimensions, finish symbols, and supplementary information can also differ. A symbol that looks familiar at first glance may convey a different instruction if interpreted under the wrong standard. Because of this, a drawing should never mix AWS and ISO weld symbols. Even small inconsistencies can lead to incorrect fabrication or inspection assumptions.


Identical joint shown once using AWS notation and once using ISO notation


### Why Standard Selection Matters in Practice


The weld symbol standard used on a drawing affects more than symbol appearance. It influences how fabricators interpret intent, how inspectors verify compliance, and how disputes are resolved if questions arise. In multi-supplier or international environments, assumptions about weld symbol standards are a common source of errors. A drawing produced using ISO notation may be misread by a shop accustomed to AWS conventions, even if the weld type itself is familiar.


Best practice is to:


- Select a single weld symbol standard for the drawing set
- State that standard clearly in the drawing notes or title block
- Apply it consistently across all views and details


This upfront clarity reduces interpretation risk and supports smoother fabrication and inspection.


## Anatomy of a weld symbol


A weld symbol is made up of several elements that work together to convey the required weld. Each element has a defined meaning under the applicable standard, and changing its position can change how the symbol is interpreted. Understanding the role of each part is essential for both specifying and reading welds correctly on an engineering drawing.


### Reference Line and Arrow


The reference line is the horizontal line that carries the weld symbol and any associated dimensions or supplementary information. It acts as the baseline from which the weld instruction is read.


The arrow connects the reference line to the joint location on the drawing. Its purpose is to identify which joint the weld symbol applies to, particularly when multiple joints are present in the same view.


In many cases, the arrow also plays a role in defining weld location. For joints where the two sides are not symmetric, the arrow indicates which side of the joint is being referenced. This distinction becomes important when specifying arrow-side and other-side welds.


Typical weld symbol notation


### Arrow-Side vs Other-Side Welds


For joints with two distinct sides, weld symbols can specify whether the weld is to be placed on the arrow side of the joint or the opposite side.


Under common conventions, a weld symbol placed on one side of the reference line indicates a weld on the arrow side of the joint, while placement on the opposite side indicates a weld on the other side. The exact interpretation depends on the standard being used.


This mechanism allows a single symbol to clearly define weld placement without additional notes. However, it also introduces the potential for misinterpretation if the reference line orientation or arrow direction is unclear.


On complex assemblies, it is good practice to review arrow direction and symbol placement carefully to ensure the intended weld location is obvious from the drawing alone.


asymmetric joint showing arrow-side versus other-side weld placement (AWS Standard)


### Weld Symbol Placement on the Reference Line


The weld symbol itself is placed on the reference line to indicate the[type of weld](https://www.chap2.com/blog/guide-to-weld-types/) required, such as a fillet or groove weld. Its position relative to the reference line carries meaning and should not be treated as purely graphical.


Dimensions related to the weld, such as size or length, are placed in defined positions around the symbol. These positions are standardized to avoid ambiguity and to allow the symbol to be read consistently. Supplementary symbols, such as those indicating contour or finishing requirements, are also attached to the reference line. Their placement signals how the finished weld surface should be treated after welding.


Weld symbol showing weld type, dimension size and contour mark


### Reading a Complete Weld Symbol


A complete weld symbol should be read as a single instruction rather than as separate pieces of information. The reference line, arrow, weld symbol, and any dimensions together define the required weld. When reviewing a drawing, it is useful to trace the symbol in a consistent order. Start at the arrow to identify the joint, then read the symbol and dimensions on the reference line, and finally note any supplementary markings.


This approach helps avoid overlooking details that may affect fabrication or inspection, particularly on drawings with multiple weld symbols in close proximity.


## Common Weld Symbols


Engineering drawings tend to rely on a small number of basic weld symbol types used repeatedly across many applications. Understanding how these symbols are intended to be applied is more important than memorising every possible variation.


The sections below focus on the weld symbols most commonly encountered on general mechanical and structural drawings, with emphasis on how they are specified rather than how they are welded.


### Fillet Welds


Fillet welds are the most frequently specified weld type on engineering drawings. They are commonly used to join parts at right angles, such as plates, brackets, and stiffeners. A fillet weld symbol defines the presence and location of the weld, along with its required size where specified. The size dimension typically refers to the leg length of the fillet, depending on the applicable standard and drawing convention.


In practice, fillet weld symbols are often over-specified. Designers sometimes apply large, continuous fillet welds by default, even when smaller or intermittent welds would be sufficient. When using fillet welds, it is important to consider whether the weld size is functionally critical or whether the fabricator can select an appropriate size based on standard practice and material thickness.


Fillet weld symbol applied to a T-joint between two perpendicular plates


### Groove Welds


Groove weld symbols are used when weld penetration through the joint thickness is required. These symbols define the joint preparation rather than the welding process itself. Common groove weld symbols include square, V, bevel, and U configurations. Each indicates the shape of the joint preparation required before welding. Additional dimensions may be used to specify groove angle, root opening, or depth of preparation where needed.


A common issue on drawings is specifying a groove weld symbol without providing sufficient information to define the joint fully. If critical dimensions are omitted, the fabricator may need to make assumptions or request clarification. Groove weld symbols are often used in load-bearing joints where full strength or fatigue performance is required. In these cases, clarity in symbol definition directly affects structural integrity and inspection outcomes.


Single-V groove weld symbol applied to a butt joint


### Plug and Slot Welds


Plug and slot welds are typically used to join overlapping parts where access for fillet or groove welds is limited. They are commonly specified on sheet metal or plate assemblies.The weld symbol defines the shape, size, and spacing of the plugs or slots, along with their placement relative to the joint. These details are critical because plug and slot welds rely on geometry to achieve the required load transfer.


In practice, these symbols are frequently misread or under-dimensioned. Omitting slot length, width, or spacing can lead to inconsistent fabrication results, especially when multiple welds are involved. When specifying plug or slot welds, it is good practice to ensure the symbol provides enough information for repeatable execution across all instances on the drawing.


Slot weld symbol applied to overlapping plates with dimensions shown (slot width, length and pitch)


### Selecting the Appropriate Weld


The choice of weld type should reflect the functional requirements of the joint. Factors such as load path, accessibility, material thickness, and distortion sensitivity all influence which weld type is appropriate.


Clear selection and specification of weld symbols helps ensure that the drawing communicates intent without forcing unnecessary constraints on fabrication. When in doubt, reviewing the joint from both a structural and manufacturing perspective can help avoid over-complication.


## Dimensions and Supplementary Information


Weld symbols rarely stand alone. In most cases, additional dimensions or supplementary symbols are required to fully define how the weld should be applied. These details are critical to achieving consistent results across fabrication and inspection.


Misplaced or incomplete supplementary information is a common cause of drawing ambiguity, even when the base weld symbol itself is correct.


### Weld Size, Length and Pitch


Weld size defines the required throat or leg dimension of the weld, depending on the weld type. Where size is critical to strength or performance, it should be explicitly specified rather than assumed.


For fillet welds, the size dimension is typically placed to the left of the weld symbol. This convention allows the symbol to be read quickly without confusion. Omitting size information usually implies that a weld of sufficient size to meet standard practice is acceptable, but this assumption should be used carefully.


When welds are not continuous along the joint, length and pitch are used to define intermittent welds. The length specifies how long each weld segment should be, while the pitch defines the center-to-center spacing between segments.


Intermittent weld notation is frequently misunderstood. If either length or pitch is unclear, the resulting weld pattern may not match the design intent. Clear placement of both values avoids unnecessary clarification during fabrication.


Intermittent fillet weld symbol showing size, length and pitch notation


### Continuous vs Intermittent Welds


In practice, continuous welds are typically specified by default, even when they are not essential for strength. In some cases, continuous welds might actually compromise function:


During a past project where a frame was designed for a high-pressure syringe pump, a continuous fillet weld was initially specified to fix a large flat bar mounting plate to two RHS uprights. The plate supported a gearbox and motor assembly, coupled to a ball screw actuator.


During a design review, the fabricators recommended switching to intermittent fillet welds instead. Their concern was that continuous welding would introduce distortion in the mounting plate, potentially compromising ball screw and pump alignment. Using stitch welds provided sufficient strength while reducing heat input and preserving flatness.


This kind of feedback highlights why weld symbols should be specified with both structural intent and fabrication effects in mind, rather than defaulting to continuous welds where they are not required.


### Weld-All-Around and Field Weld Symbols


The weld-all-around symbol indicates that a weld is required continuously around the entire perimeter of a joint. It is typically used where sealing or uniform strength is required. Because this symbol has significant fabrication implications, it should be applied deliberately. Using a weld-all-around symbol where it is not functionally necessary can substantially increase welding time and distortion risk.


Field weld symbols indicate that a weld is to be performed at the installation site rather than in the shop. They are generally used where site welding provides a practical advantage over full shop fabrication. Field weld symbols are often applied when final assembly depends on site conditions rather than fixed geometry. Structural frames installed on uneven or tolerance-critical foundations are a typical example. While major members may be shop welded, final attachment plates or stiffeners are sometimes welded on site once alignment and level are confirmed.


Weld-all-around symbol applied to cylindrical part in an assembly


### Contour and Finish Symbols


Contour symbols specify the desired shape of the finished weld surface, such as flush, convex, or concave. These symbols are often paired with finish symbols that define how the contour is achieved. Finish symbols indicate the method used to produce the final surface, such as grinding or machining. They do not imply a quality level on their own, but they do affect surface condition and appearance.


These symbols are particularly important where weld profile affects fit, fatigue performance, or aesthetic requirements. They should be used when the weld surface interacts with mating parts or functional surfaces. Overuse of finish symbols can increase fabrication time unnecessarily. As with other weld details, they should be specified only where they serve a clear purpose.


Fillet weld symbol with flush contour and grinding finishing method


## Common Mistakes


Many issues with welded components can be traced back to how weld symbols are specified on engineering and fabrication drawings. The symbols themselves may be correct, but small inconsistencies or omissions can lead to incorrect assumptions during fabrication or inspection.


- Not enough supporting information. For example, a fillet weld symbol may be shown correctly, but without a defined weld size or length where those parameters are functionally important. In these cases, different fabricators may apply different assumptions, leading to variation in the finished part.
- Unclear use of the reference line. Under some standards, weld information placed on the reference line opposite the arrow side carries a different meaning. Confusion between a single horizontal line and two parallel reference lines, as used in certain notation systems, can result in welds being placed on the wrong side of a joint.
- Mixing standards on the same drawing. Symbols derived from ISO 2553 may appear alongside notation expected under American Welding Society conventions, even when the title block references only one system. This creates ambiguity, particularly for suppliers who work across multiple regions.
- Incomplete joint details (especially important with groove welds). Specifying a V groove weld, square groove weld, bevel groove weld, or U groove weld without clearly defining groove angle, root opening, or preparation depth can force the fabricator to make assumptions. Where complete joint penetration is required, this should be communicated clearly rather than implied.
- Insufficient dimensional control (especially common with plug welds, slot welds, seam welds, and spot welds). Omitting plug diameter, slot length, spacing, or location makes consistent execution difficult, especially when multiple welds are involved.
- Over-specification. Applying continuous welds, extensive finish symbols, or field weld requirements where they are not functionally necessary increases cost and fabrication effort without improving performance. Each weld symbol should serve a clear purpose tied to strength, fit, sealing, or inspection.


## Conclusion


Weld symbols are a compact but powerful way to communicate welding requirements on engineering drawings. When applied correctly, they define weld location, geometry, and critical constraints without relying on lengthy notes.


Most problems with weld symbols arise not from the symbols themselves, but from inconsistent standards, incomplete information, or over-specification. Understanding how weld symbols are read in practice helps avoid these issues.


By selecting the appropriate standard, applying symbols consistently, and specifying only what is functionally required, engineers can produce drawings that are clearer, more robust, and easier to manufacture and inspect.
