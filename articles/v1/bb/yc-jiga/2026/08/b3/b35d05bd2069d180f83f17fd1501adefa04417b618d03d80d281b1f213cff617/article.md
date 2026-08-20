---
schema_version: "1.0.0"
document_id: "b35d05bd2069d180f83f17fd1501adefa04417b618d03d80d281b1f213cff617"
company_key: "yc-jiga"
company: "Jiga"
source_id: "yc-jiga-news-import-07676ebb04d8"
canonical_url: "https://jiga.io/articles/stainless-steel-passivation/"
published_at: "2026-08-17T16:21:38+00:00"
first_seen_at: "2026-08-20T01:58:45.018415+00:00"
fetched_at: "2026-08-20T01:58:46.131739+00:00"
content_hash: "sha256:c3cb00e966829d1eec07b7b8780a3406fdfb9f15bc6cab78c77d5bce224d3a5a"
---

# Passivation on the drawing: How to specify stainless steel passivation so parts actually pass

A stainless steel component can meet every dimensional tolerance, surface finish requirement, and material specification on your drawing, yet still fail corrosion testing.


The reason is not the alloy itself, but how its surface was treated after machining.


Many production drawings simply state:


**PASSIVATE PER ASTM A967**


While technically valid, that instruction is far from complete, leaving critical decisions open to supplier interpretation. The drawing satisfies the standard, but it doesn’t necessarily satisfy the engineering intent.


Which passivation method should be used? (nitric or citric acid based)


Which acceptance test demonstrates compliance?


What documentation should accompany the finished parts?


Those questions matter because different suppliers can legitimately produce different outcomes, while still asserting compliance with


[ASTM A967](https://store.astm.org/a0967-01.html) and client requirements.


For engineers working in aerospace, defence, robotics, medical devices and other high-reliability industries, passivation is not simply a finishing process. It is a tightly controlled manufacturing operation that should be specified, verified and documented with the same level of discipline as heat treatment, plating, or anodising.


This guide focuses on the practical engineering questions that generic


*what is passivation?* articles typically fail to answer:


- What passivation actually delivers.


- When to specify nitric or citric methods.


- How to write an unambiguous drawing callout.


- Which ASTM A967 verification tests to require.


- What documentation should accompany production parts.


Just as importantly, it explains who should own ensuring passivation is completed correctly and documented throughout the supply chain. That accountability is often the difference between a component that passes incoming inspection and one that creates delays, rework or audit findings. Please review


[Jiga services and support](https://jiga.io/articles/metal-plating-techniques/) in passivation.


This valve actuation shaft typifies the need for passivation in stainless steel components exposed to harsh environments. In its as-machined state it will have surface contaminants and uneven Chromium dioxide film formation that can enable pitting corrosion, staining, and media contamination


## What passivation actually does (and what it doesn't)


Despite being specified on countless engineering drawings, passivation remains poorly understood.


Some engineers describe it as applying a protective coating. It doesn’t.


Others think it simply cleans stainless steel with acid. That is only part of the process.


Passivation is a controlled chemical treatment that removes free Iron and other surface contaminants introduced during machining, grinding, polishing, handling, or fabrication of stainless steel (


[303, 394, 316, 17-4 PH](https://jiga.io/articles/cnc-machining-steel) etc). Once these contaminants are removed, Chromium already present within the alloy preferentially oxidises to form an extremely thin chromium-rich passive film only a few nanometres thick.


This passive layer provides the


[corrosion resistance](https://jiga.io/articles/cnc-machining-materials) that stainless steel is known for.


Importantly, passivation does not change the dimensions of the part, deposit a coating or alter the alloy itself. Instead, it restores the surface chemistry so the material can perform as intended.


This is particularly important after operations such as:


- CNC machining


- Grinding


- Bead blasting


- Wire brushing


- Vibratory finishing


- Assembly using carbon steel fixtures or tooling


Each process can leave microscopic iron contamination on the surface. Left untreated, these particles may corrode long before the stainless steel underneath.


This illustration micrograph of a pre-and-post passivation surface shows the near-zero dimensional change that passivation imposes, and the nature of the cleanup process that regulates and optimizes the part surfaces .


### Passivation is not pickling


These two terms are often used interchangeably, but they describe different processes with different objectives.


Passivation Pickling


Removes free iron contamination Removes heat tint and oxide scale


Preserves the existing surface finish Chemically removes a thin surface layer


Uses nitric or citric chemistry Typically uses nitric and hydrofluoric acids


Improves corrosion resistance Restores heavily oxidised or welded surfaces


A welded fabrication often requires pickling before passivation, because welding creates oxide scale that passivation alone cannot remove.


Conversely, a precision-machined valve stem or actuator shaft usually requires only passivation. Specifying pickling unnecessarily can affect appearance, increase cost and alter the finished surface.


The correct choice depends on how the component was manufactured, not simply on the material grade.


**CTA:** Secondary finishing requirements should be reviewed alongside manufacturability, not added after machining begins. Defining these requirements early helps prevent supplier interpretation and unnecessary production delays.


### Why engineers specify passivation


Passivation is specified because corrosion failures rarely remain cosmetic. In many engineered systems, localised corrosion can compromise sealing surfaces, contaminate process media, initiate fatigue cracks or shorten component service life. The cost of preventing these failures is typically insignificant compared with the cost of repairing or replacing finished equipment.


The importance of passivation varies with the application, but it is commonly specified wherever stainless steel components operate in aggressive environments or where long-term reliability is critical.


#### Valve stems and process equipment


Valve stems, shafts and sealing components frequently operate in humid, chemically aggressive or marine environments. Free iron contamination left after machining can become initiation sites for pitting corrosion, particularly around sealing diameters, threads and bearing surfaces. Even minor corrosion can increase operating torque, accelerate seal wear or create leakage paths in pressure-retaining equipment.


#### Medical devices and surgical instruments


Implants, surgical instruments and medical fluid-handling components demand exceptionally clean, corrosion-resistant surfaces. Surface contamination can compromise biocompatibility, create corrosion sites during repeated sterilisation cycles and increase the risk of particle generation..


#### Hydraulic actuators and aerospace mechanisms


Aircraft actuators, landing gear components, hydraulic cylinders and precision mechanical linkages often rely on stainless steel shafts operating under cyclic loading. Corrosion pits act as stress concentrators that can significantly reduce fatigue life.


#### Semiconductor and high-purity fluid systems


Semiconductor manufacturing equipment frequently uses precision-machined stainless steel fittings, valve bodies and gas-handling components that carry ultra-high-purity process gases and chemicals.


#### Marine equipment


Marine hardware experiences continuous exposure to chloride-rich environments where localised pitting corrosion can develop rapidly. Shafts, fasteners, actuators and instrumentation components benefit from properly passivated surfaces.


#### Precision sealing surfaces


Many machined stainless steel components incorporate O-ring grooves, valve seats, sealing tapers or precision bearing journals. Passivation removes embedded free iron that may otherwise become isolated corrosion sites.


Ultimately, engineers do not specify passivation simply because a drawing standard recommends it. They specify it because they understand how surface contamination can become a failure mechanism.


This illustrates the stages in preparation and passivation applied to the shaft in Fig1


### Nitric vs. citric passivation: Choosing the right method


Selecting the correct passivation process is an engineering decision rather than simply a finishing-house preference. ASTM A967 recognises multiple nitric and citric passivation methods because different stainless steel grades, service environments and customer specifications require different treatments. The appropriate choice depends on the material, governing specification, intended application and programme requirements.


### Nitric passivation


Nitric acid passivation has been the industry standard for many decades, and remains widely specified in aerospace, defence and other legacy manufacturing programmes. ASTM A967 defines four standard nitric methods together with an open-ended Method 5 for qualified user-developed processes.


Nitric Method 1 incorporates sodium dichromate, improving performance for certain alloys but introducing significant environmental and disposal challenges because of its hexavalent chromium content.


### Citric passivation


Citric passivation has become increasingly popular because it provides excellent corrosion performance while significantly reducing environmental impact. ASTM A967 defines three standard citric methods together with provisions for qualified user-developed processes. Concentrations range from 4 to 10 percent, and treatment cycles are shorter than comparable nitric processes.


Citric chemistry eliminates nitrogen oxide emissions and dichromate additives, simplifying waste treatment while maintaining excellent corrosion resistance on many common stainless steel grades.


### Comparing nitric and citric passivation


Selection Criteria Nitric Passivation Citric Passivation


Process chemistry Nitric acid solution Citric acid solution


Environmental impact High Low


Availability May be limited in some regions because of environmental permitting and waste-handling requirements. Widely available through many commercial finishing suppliers.


Typical industries Aerospace, defence, oil & gas, legacy manufacturing programmes. Medical devices, food processing, semiconductor equipment, pharmaceutical manufacturing.


Ferritic stainless steels Well established for many ferritic grades. Suitable where qualified by customer specification.


Martensitic stainless steels Extensive production history and strong acceptance for grades such as 410, 420 and 440C. Increasingly used where customer requirements permit.


Waste treatment Complex. Acid neutralisation, emission control and regulated waste disposal challenges. Simpler waste treatment with lower environmental burden.


Legacy qualification Long history of qualification within aerospace and defence programmes. Increasing adoption in new programmes where environmental objectives and sustainability influence choice.


Typical engineering choice Often selected when AMS 2700 requirements apply. Often selected for new designs, for lower environmental impact, easy availability are priorities.


### Selecting the correct method


Engineering practice typically considers both the material and the governing specification when selecting a passivation process.


Stainless Steel Grade Typical Engineering Approach


304, 304L, 316, 316L Nitric Method 2 or 3, or approved citric methods


409 Commonly Nitric Method 1 or 4


410, 420, 440C Frequently Nitric Method 1 or 4, unless otherwise specified


Aerospace components Commonly governed by AMS 2700 or customer-specific specifications


These recommendations should be treated as engineering guidance rather than mandatory requirements. Customer specifications, contractual obligations and programme-specific standards always take precedence.


For aerospace applications, drawings frequently reference AMS 2700 alongside or instead of ASTM A967. Always verify the current revision before releasing production documentation.


In practice, the engineering decision is rarely based on corrosion performance alone. Existing customer specifications, supplier capability, environmental compliance, process availability and legacy programme qualification often have an equal influence on whether nitric or citric passivation is selected.


### Avoid leaving the method open to interpretation


A


[drawing callout](https://jiga.io/articles/part-tolerances-for-cnc) stating:


**PASSIVATE PER ASTM A967**


still leaves important questions unanswered.


Different suppliers may legitimately choose different nitric or citric methods, processing temperatures and acceptance tests.


A more complete specification removes that ambiguity.


For example:


**PASSIVATE IN ACCORDANCE WITH ASTM A967, CITRIC METHOD 2. VERIFY USING HIGH HUMIDITY TEST. PROVIDE CERTIFICATE OF CONFORMANCE IDENTIFYING METHOD USED AND ACCEPTANCE TEST PERFORMED.**


That single instruction significantly improves consistency across multiple suppliers.


### A simple decision framework


Before releasing a production drawing, confirm:


1. Stainless steel grade.


2. Customer or industry requirements.


3. Governing specification.


4. Required passivation method.


5. Acceptance test.


6. Documentation requirements.


This transforms passivation from a generic finishing note into a controlled engineering requirement.


### What to put on the print


Passivation problems rarely begin at the finishing shop.


More often, they begin with the callout on the component drawing.


An incomplete drawing note allows the machine shop, finishing house or supplier to make assumptions. Those assumptions may all comply with ASTM A967, but they may not produce the required outcome.


Like material grade, heat treatment or


[surface finish](https://jiga.io/articles/cnc-precision-machining) , passivation should be specified clearly enough that every qualified supplier interprets the requirement in the same way.


Every drawing should specify:


- Standard


- Method


- Verification


- Documentation


### Good versus poor callouts


Ambiguous Recommended


Passivate per ASTM A967 Passivate per ASTM A967, Citric Method 2


Supplier selects verification High Humidity Test required


Documentation not specified Certificate of Conformance required


Method left to interpretation Engineering defines the required process


Incomplete and complete drawing callouts for the stainless steel shaft in Fig 1


### Don't forget the acceptance test


Specifying the chemical treatment alone is not enough.


The drawing must also identify how successful passivation will be verified.


ASTM A967 provides several recognised verification methods, because various applications require applicable levels of confidence. Leaving the verification method unspecified means suppliers may legitimately choose different tests, while still claiming compliance.


For safety-critical or regulated industries, defining the acceptance test on the drawing removes another source of variation.


## Proving it worked: ASTM A967 acceptance testing


Passivation is only valuable if it can be demonstrated to have worked.


ASTM A967 requires completed parts to satisfy recognised verification methods rather than assuming that successful immersion automatically produces a corrosion-resistant surface.


Verification should be viewed as part of the manufacturing process rather than a separate quality activity.


### Start with visual inspection


Every passivated component should first be inspected visually.


The surface should be clean, uniform and free from visible damage.


Typical signs of an unsuccessful process include:


- Etching


- Frosting


- Pitting


- Staining


- Surface discolouration and non-uniformity


These defects may indicate contamination before treatment, incorrect chemistry or poor process control.


### ASTM A967 verification tests


ASTM A967 includes seven recognised verification methods.


The engineer selects the test appropriate for the application, while the finishing supplier performs the verification test specified.


Verification Method Primary Purpose


Water immersion General corrosion screening


High humidity Passive layer verification in humid environments


Salt spray Accelerated corrosion resistance


Copper sulfate Detection of free iron contamination


Potassium ferricyanide-nitric acid Sensitive detection of residual iron


Ferroxyl Identification of embedded iron contamination


Qualified user-developed method Customer-approved alternative procedure


Not all applications require the same level of testing.


For example, a semiconductor valve component or aerospace actuator justifies more rigorous verification than a decorative architectural fitting.


The objective is selecting a verification method appropriate to the service environment.


### Surface preparation still matters


Passivation cannot compensate for poor cleaning.


ASTM A967 references ASTM A380 for cleaning and descaling before passivation. Components should be substantially free of:


- Oil/Grease


- Cutting fluids


- Rust


- Scale


- Other contaminants


If contamination remains on the surface, the passive oxide layer cannot form consistently.


Cleaning is therefore part of the passivation process, not simply preparation for it.


### Rinsing is part of quality control


Following chemical treatment, effective rinsing removes residual chemicals and prevents staining.


ASTM A967 recommends rinse water containing less than 200 ppm total dissolved solids. The standard does not require a separate neutralisation step after passivation.


Although engineers rarely specify rinse-water quality on drawings, experienced finishing suppliers recognise that rinsing is essential to achieving repeatable results.


### Verification should be traceable


Verification has little value if it is not documented, so it cannot be assessed/reviewed later.


Each production batch should remain traceable to:


- Material certification


- Passivation specification


- Passivation method


- Verification test


- Production lot


- Certificate of Conformance


Without this information, investigating corrosion failures or responding to customer audits becomes considerably more difficult.


For regulated industries, documentation is often as important as the corrosion test itself.


**CTA** A qualified supplier should deliver more than compliant parts. They should also provide complete traceability showing exactly how critical secondary processes such as passivation were performed and verified.


### Practical acceptance checklist


Before approving passivated production parts, confirm the supplier has provided:


- Governing specification


- Passivation method


- Acceptance test performed


- Certificate of Conformance


- Material certification where required


- Batch traceability


- Additional inspection records specified by contract


These records help demonstrate that passivation has been completed correctly and consistently, rather than expecting conformance to be assumed.


### The part the drawing doesn't cover: Documentation and accountability


An engineering drawing defines what must be manufactured.


It rarely defines who is responsible for ensuring every secondary process has been completed correctly.


In practice, a single component may pass through several organisations before delivery. One supplier machines the part. Another performs passivation. A third may carry out inspection or testing, before the component is packaged and shipped.


Every handoff creates an opportunity for specifications, inspection records or traceability to become disconnected from the finished product.


Successful manufacturers treat passivation as part of the quality system, rather than simply another finishing operation.


### What documentation should accompany passivated parts?


Documentation requirements vary by customer and industry, but production components should typically be supplied with:


Document Purpose


Certificate of Conformance (CoC) Confirms passivation was completed to the specified standard


Material certificate Verifies the stainless steel grade


Passivation process record Identifies the governing specification and method used


Acceptance test record Demonstrates compliance with the specified verification method


First Article Inspection (FAI), where required Confirms the manufacturing process has been validated


PPAP documentation, where applicable Demonstrates production capability and process control


The Certificate of Conformance is particularly important.


Rather than simply stating that passivation was performed, it should identify:


- the governing specification,


- the passivation method,


- the production batch,


- the verification method,


- and the processing date.


These details provide the traceability required for customer audits, warranty investigations and long-term quality records.


### Why traceability matters


Consider two production batches of identical valve stems, delivered six months apart.


- Both use 316 stainless steel


- Both reference ASTM A967


- One performs flawlessly


- The other develops corrosion during environmental testing.


Without traceable records, determining the precise cause becomes very challenging.


- Was a different passivation method used?


- Did another finishing supplier process the second batch?


- Was a different verification test performed?


- Did documentation simply fail to follow the parts?


Complete records allow engineers to answer those questions quickly, limiting both investigation time and production risk.


### Integrating Passivation into FAI and PPAP


Passivation should never occur outside the control of quality documentation.


For aerospace programmes, it commonly supports First Article Inspection (FAI).


For automotive and many industrial applications, it may also form part of Production Part Approval Process (PPAP) documentation.


In both cases, the objective is identical: To demonstrate that production is controlled, repeatable and fully traceable.


Missing passivation records can delay qualification just as readily as missing material certificates or dimensional inspection reports.


### One point of accountability


Many corrosion issues are not directly caused by poor passivation.


- They are caused by unclear ownership.


- Engineering assumes purchasing specified the correct process.


- Purchasing assumes the finishing supplier selected the appropriate method.


- Quality assumes engineering reviewed the documentation.


- Meanwhile, nobody verifies that every requirement has actually been completed.


A supplier of record closes that gap.


Instead of coordinating multiple independent suppliers, one accountable organisation ensures that:


- manufacturing follows the approved drawing,


- passivation matches the specified method,


- verification testing satisfies the drawing requirements,


- documentation is complete,


- and every production batch remains fully traceable.


This approach reduces supplier disputes, simplifies audits and gives engineering teams greater confidence that the delivered component matches both the drawing and the supporting documentation.


**CTA**


Jiga acts as the supplier of record for custom manufacturing, coordinating machining, secondary finishing, inspection and documentation through a single accountable workflow. That reduces supplier management effort while improving traceability and quality consistency.


## Best practice checklist


Before releasing your next production drawing, confirm that you have:


- Identified the correct stainless steel grade.


- Determine whether passivation or pickling is required.


- Specified the governing standard (ASTM A967 or customer-required equivalent).


- Defined the required passivation method.


- Specified the acceptance test.


- Requested the necessary documentation.


- Verified any customer-specific aerospace or defence requirements.


- Ensured purchasing instructions match the engineering drawing.


Completing these steps adds only a few lines to the drawing, yet significantly reduces production ambiguity and improves repeatability.


## Conclusion


Passivation is often treated as a simple note at the bottom of an engineering drawing.


In reality, it is a controlled manufacturing process that deserves the same level of attention as material selection, heat treatment and surface finish.


A vague instruction (Passivate per ASTM A967) leaves important decisions to supplier interpretation. A better specification identifies the governing standard, the required method, the verification test and the documentation expected with every shipment.


Just as importantly, someone must own ensuring those requirements are carried through production. This is central to the


[Jiga supplier of record approach](https://jiga.io/cnc-machining)


For engineering teams sourcing precision stainless steel components, that responsibility extends beyond selecting a qualified machine shop. It includes ensuring every secondary process is completed correctly, verified objectively and documented thoroughly.


When specification, verification and accountability are managed together, effectively specified passivation transforms a loosely defined finishing operation into a controlled engineering process, with defined chemistry, verifications, traceability and accountability. That is what ensures stainless steel parts consistently pass inspection, corrosion testing and customer audits.


## Frequently Asked Questions


Does passivation apply a protective coating?


No. Passivation does not deposit a coating or increase part thickness. It removes free iron contamination from the surface, allowing a chromium-rich passive oxide layer to form naturally and improve corrosion resistance.


What is the difference between passivation and pickling?


Passivation removes free iron contamination while preserving the existing surface finish. Pickling removes heat tint and oxide scale using a more aggressive chemical process. Welded fabrications often require pickling before passivation, whereas precision machined components commonly require passivation alone.


Should I specify nitric or citric passivation?


The correct choice depends on the stainless steel grade, customer requirements and governing specification. Neither process is universally better. ASTM A967 provides multiple approved methods, while many aerospace programmes reference AMS 2700.


What should an ASTM A967 drawing callout include?


A complete callout should specify:


- the governing standard,


- the required passivation method,


- the acceptance test,


- and any documentation requirements such as a Certificate of Conformance.


Providing this information reduces supplier interpretation and improves production consistency.


What documentation should accompany passivated components?


Production parts should normally be supplied with a Certificate of Conformance identifying the governing specification and passivation method. Depending on programme requirements, this may also include material certificates, acceptance test records, FAI documentation and PPAP records to maintain full traceability.
