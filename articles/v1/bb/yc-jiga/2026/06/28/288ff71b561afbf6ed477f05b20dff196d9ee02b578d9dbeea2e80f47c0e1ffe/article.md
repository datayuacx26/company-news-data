---
schema_version: "1.0.0"
document_id: "288ff71b561afbf6ed477f05b20dff196d9ee02b578d9dbeea2e80f47c0e1ffe"
company_key: "yc-jiga"
company: "Jiga"
source_id: "yc-jiga-news-import-07676ebb04d8"
canonical_url: "https://jiga.io/articles/engineering-change-orders/"
published_at: "2026-06-15T14:33:49+00:00"
first_seen_at: "2026-07-25T10:15:55.219560+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:c5e3f2d24faafd7bd543284ec14ff0d30609ea00b9fadb740299faad1627b0e1"
---

# Engineering change orders: What they are and how to roll them through your supply chain

Engineering changes are inevitable in handling real-world manufacturing, for correction, optimization, cost reduction, component substitutions, and a range of other causes. The change authorization and control process varies in detail between companies and industries from the rigidity of aircraft component certifications to the simpler handling of consumer products.


However, the process typically seeks to operate open-channels for review, authorization, and execution to ensure outcomes are controlled as required.


Designs evolve, tolerances require adjustment, suppliers discontinue materials/parts, customers revise requirements, and field failures expose weaknesses that were not visible during prototyping. The question is not whether an engineering change order (ECO) will occur, but whether the change will be implemented in a controlled way that keeps manufacturing aligned with engineering intent.


Poorly controlled changes are a too-common cause of production discrepancies in contract manufacturing. A supplier builds the wrong revision, work-in-progress is scrapped without agreement, inspection criteria do not match the updated drawing, or inventory remains mixed across multiple revisions. In many cases the problem is not the engineering change itself, but the communication and rigor enacting it.


This guide explains what an engineering change order is, how the ECR–ECO–ECN workflow functions, what information an ECO must contain, how to communicate engineering changes to suppliers, and how to prevent revision-control problems from propagating through production.


## Key takeaways


- An ECO is the formal authorization to modify a product, drawing, BOM, specification, or manufacturing process.


- The standard workflow is ECR (identify the issue, request chang), then ECO (authorization of the change), then ECN (communicate implementation).


- Effect and non-product implications must always be defined clearly, including whether the change applies immediately, after inventory depletion, or from a specific serial or PO number onward.


- Most ECO failures originate from late (or absent) supplier notification, unresolved WIP disposition, unclear revision control, or missing delta first article inspection after critical changes.


Engineering change requests arise for a huge range of reasons - material changes, dimension/tolerance adjustments, process changes, part/supplier substitutions, design changes and more. The critical factors in an Engineering Change control process are specification, validation, circulation and complete record keeping. All departments must sign off on any change before implementation, even when their perceived relevance is marginal. Perception is not always fact!


## What is an engineering change order (ECO)


An engineering change order (ECO) is the formal controlled document that authorizes a modification to:


- a product,


- component,


- engineering drawing,


- bill of materials (BOM),


- specification,


- or manufacturing process.


The ECO is the authorization stage in the engineering change process. It converts an approved engineering change


*request* into an actionable instruction with:


- a revision number,


- implementation requirements,


- effectivity,


- approvals,


- and traceable revision history.


An ECO is not:


- a redline markup,


- an email,


- a Teams message,


- a verbal supplier instruction,


- or a CAD comment thread.


Those may support the process, but the ECO is the controlled record that defines:


- exactly the change,


- why it is required,


- when it must take effect,


- and what happens to existing inventory and work-in-progress


Without formal ECO control, suppliers frequently end up building:


- mixed revisions,


- outdated geometry,


- incorrect BOM configurations,


- or assemblies where the drawing and production reality diverge silently over time


This is particularly hazardous in regulated industries such as aerospace, medical, automotive, and any production environment requiring traceability.


A typical Engineering Change Request provides reviewers with full details of relevant part number(s), nature and purpose of change, indications of effect if not accepted and the opportunity to feed-back to revise the change, if there are issues observed.


## ECR vs ECO vs ECN: How they differ


The terms ECR, ECO, and ECN are often used interchangeably, but they represent three sequential stages of the change-management workflow.


Organizations that conflate them typically encounter one of two problems:


- changes begin production before approval,


- or suppliers never receive a formal implementation notification


### Engineering change request (ECR)


The engineering change


*request* identifies a potential need for change.


An ECR may originate from:


- engineering,


- manufacturing,


- quality,


- procurement,


- field service,


- suppliers,


- or customers.


Examples:


- a tolerance repeatedly causes scrap,


- a connector becomes obsolete,


- a weld fails during testing,


- a supplier suggests a lower-cost material,


- a field failure reveals fatigue cracking.


The ECR describes:


- the problem,


- proposed change,


- justification,


- and expected impact.


It does


**not** authorize implementation, it


*seeks* that authorization from all parties who may have an interest, or information that enhances/assures the nature of the change.


### Engineering change order (ECO)


The ECO is the formal


*authorization* document, validated by all interested parties as agreed and confirmed.


It defines:


- the approved change,


- affected documents,


- revision levels,


- implementation timing,


- inventory disposition,


- inspection requirements,


- and approvals.


The ECO is the control instruction that manufacturing and suppliers execute against, effecting the change.


### Engineering change notice (ECN)


The ECN is the communication mechanism used to distribute the approved change to:


- manufacturing,


- procurement,


- suppliers,


- quality,


- logistics,


- and service teams.


The ECN ensures every affected stakeholder knows:


- what changed,


- which revision is now valid,


- when the change becomes effective


- and what their responsibilities are in enacting the change


An organization may issue an ECO internally but still fail operationally if the ECN is delayed or unclear.


## What does an engineering change order include?


A complete ECO should function as a self-contained implementation package and authorization for all required actions. A supplier receiving it should understand:


- what changed,


- why it changed,


- when it takes effect,


- and what happens to existing material/stock.


Incomplete or unclear ECOs are one of the leading causes of supplier-side production errors.


### Identification: Part numbers, drawing revisions, and BOM


Every ECO must clearly identify:


- affected part numbers,


- assemblies,


- BOMs,


- CAD files,


- specifications,


- and drawing revisions.


Ambiguity here creates immediate risk. Affected assemblies should also be identified.


### Rason and justification


The ECO should explain why the change is occurring.


Examples:


- field failure reduction,


- assembly interference,


- cost reduction,


- supplier component obsolescence,


- manufacturability improvement,


- regulatory compliance.


This is key, because suppliers often need contextual understanding to implement the change operationally.


Not all engineering changes are equal in impact. This shows the effect of later changes in design/materials/process for a tooled part - die-cast, injection-molded, forged etc. Weighing the impact in terms of cost (and production schedule) of late specification/design changes can heavily affect decision making. The impact may be less for non-tooled component changes, but similar rules apply- change early to reduce $ and time impacts


### Change classification: Class I vs class II


Many organizations classify ECOs by severity.


#### Class I changes


Affect:


- fit,


- form,


- function,


- safety,


- interchangeability,


- compliance.


These usually require:


- formal approval,


- updated qualification,


- delta FAI,


- customer notification.


#### Class II changes


Minor administrative or cosmetic changes that do not affect function.


Examples:


- note clarification,


- typo correction,


- non-functional marking update


These may entail lower level permission processes and a more relaxed authorization.


### Impact assessment: Cost, inventory, and schedule


An effective ECO shares evaluation of:


- tooling impact,


- scrap exposure,


- inventory impact,


- supplier lead times,


- requalification cost,


- schedule disruption.


For example:


- changing anodizing thickness may require new qualification testing,


- tightening tolerances may increase machining cost,


- changing suppliers may require PPAP or FAI.


### Effectivity date and WIP disposition


Effectivity defines when the change becomes active.


Examples:


- immediate implementation,


- next PO only,


- after stock depletion,


- from a specified serial number onward.


WIP disposition must also be explicit:


- use as-is,


- rework,


- scrap,


- segregate,


- return to supplier.


Unresolved WIP disposition is one of the most common causes of mixed revisions in the field.


### Approvals and sign-off


Most ECO systems require cross-functional approvals and formal lodging/signatures.


Typical approvers:


- engineering,


- manufacturing,


- quality,


- supply chain,


- program management,


- customer,


- regulatory teams.


The approval structure reflects the risk level of the change, with increasing scrutiny and seniority of authorization as the criticality or commercial implications expand.


## What are the common reasons an ECO is issued?


Manufacturers issue Engineering Change Orders based on six primary triggers: correcting field quality failures, executing cost-reduction initiatives, substituting components or suppliers, satisfying new regulatory compliance, integrating design evolution testing, and updating manufacturing processes. These triggers command immediate documentation revisions to preserve product safety, minimize production expenses, and ensure operational validity.


### Quality and field failures


Examples can arise from


*any aspect* of a product or its manufacture:


- interference during assembly,


- weld cracking,


- coating delamination,


- connector failure,


- fatigue cracking.


- user experience failures


- exposure of performance issues


- material degradation


These changes are often urgent, because defective products may already exist in the field and timely correction can be market-influential


### Cost reduction / value engineering


Examples:


- relaxing unnecessarily tight tolerances,


- changing material grades,


- reducing machining time,


- simplifying assembly.


A


[tolerance change](https://jiga.io/articles/part-tolerances-for-cnc/) from ±0.01 mm to ±0.05 mm may dramatically reduce cost without affecting function. An allowance for increasing recycled material content from 5% to 25% can greatly reduce cost, without functional consequence.


### Supplier or component aubstitution


Supplier/source changes are a regular feature of ongoing mass production, and examples can arise from:


- discontinued raw material,


- unavailable component,


- alternate supplier qualification,


- sub-tier process change.


These often require qualification review, FAI, and evaluation of sample builds to confirm compliance.


### New regulatory or customer requirements


Examples:


- RoHS compliance,


- REACH updates,


- medical implant standards material updates


- aerospace specification revisions,


- customer-driven geometry changes.


### Design evolution


Prototype and EVT/DVT testing frequently expose required changes before production release. Equally, ongoing design maintenance can result in improvement opportunities that were previously out of reach.


Examples:


- rib reinforcement,


- geometry changes,


- tolerance adjustment,


- thermal redesign


### Manufacturing process change


Examples:


- downgrading machining methods/equipment requirements,


- new tooling, cavity count increases, or post-repair tooling


- alternate plating vendor,


- new welding process, such as revision from MIG to laser welding


Even when the geometry remains unchanged, a process change


*itself* may require ECO review and delta FAI.


The shift left approach to design originated with software development, but it has become a useful tool in product manufacture. This emphasizes the need for early testing, early failures, rapid correction in order to reduce the impact of design revisions and bring them into the early development stage. An incidental benefit is that most development teams run a very simple engineering change process control at these early stages, as the influence outside the design team is null and no outside review is required.


## The engineering change order process step by step


The ECO process is a critical, tightly controlled, and gated workflow. Skipping stages is a common cause of production misalignment.


A sequence of tasks result from an Engineering Change Request that is accepted and proceeds. These apply to all areas of operation and ensure every aspect is under full control and is in compliance with the associated quality manual.


### 1. Identify the need (ECR raised)


The issue is documented formally, often by a ‘champion’ who will act casual usher for the entire change process.


Inputs may include:


- NCRs,


- supplier feedback,


- field returns,


- cost analysis,


- testing failures,


- customer requests.


### 2. Investigate and draft the ECO


Engineering, or the change champion/driver evaluates the change as a validation step that builds confidence in affected parties/departments who must authorize their aspects:


- technical impact,


- manufacturability,


- qualification requirements,


- inventory impact,


- and supply chain implications.


Updated:


- drawings,


- BOMs,


- specifications,


- CAD files


are prepared, to support the change process and ensure version control is maintained.


### 3. Review and authorize (CCB)


The change control board (CCB) reviews:


- technical validity,


- cost,


- schedule,


- risk,


- and implementation strategy


Approvals are captured formally.


### 4. Notify and implement (ECN issued)


The ECN distributes the change to:


- suppliers,


- procurement,


- manufacturing,


- quality,


- inventory control.


The supplier must confirm:


- receipt,


- understanding,


- and implementation timing.


### 5. Verify in production (Delta FAI where required)


Critical changes should be verified in pre-production sample builds that are evaluated as ‘new’ product.


This may include:


- delta FAI,


- PPAP,


- dimensional verification,


- material testing,


- functional testing.


Skipping this stage frequently allows incorrect revisions into production unnoticed.


## Common ECO pitfalls and how to avoid them


Engineering Change Order failures typically occur during supplier handoff rather than internal engineering design. To eliminate these operational risks, manufacturing teams must execute immediate supplier notifications, dictate explicit inventory dispositions, define exact effectivity metrics, and enforce delta First Article Inspections.


### Late notification to suppliers already in production


A supplier may already have:


- raw materials prepared or in stock,


- parts machined,


- tooling loaded,


- or assemblies partially built.


Late notification creates:


- scrap,


- mixed revisions,


- delivery delays,


- disputes.


Suppliers should be informed immediately after approval, and in critical they should be consulted in the ECR stage, to reduce surprises.


### Unresolved WIP disposition


One of the most damaging failures is ambiguity around partially completed inventory, and the danger of creating uncontrolled versions with unresolved (or un-evaluated) failure risks.


Questions must be answered explicitly:


- scrap?


- rework?


- use as-is?


- segregate?


Without clear disposition, mixed inventory becomes highly likely, and potentially commercially hazardous.


### Unclear or missing effectivity


Effectivity must never be implied.


Statements like:


- “use latest revision”


- “implement ASAP”


are operationally dangerous.


Instead specify:


- exact serial range,


- PO applicability,


- stock exhaustion policy,


- implementation deadline.


### No delta FAI after a class I change


Critical feature changes frequently require verification – at component, sub assembly, AND product level.


Examples:


- hole relocation,


- tolerance tightening,


- material substitution,


- coating change,


- supplier process change.


Without delta FAI, the organization may incorrectly assume implementation succeeded, with subsequent field failures the potential outcome.


## ECO best practices for engineering and supply chain teams


Document and version control are imperative, and must be closely managed to reduce risk.


### Use a single source of truth for drawing revisions


Suppliers should never rely on:


- email attachments,


- desktop copies,


- uncontrolled PDFs.


Centralized controlled revision systems reduce confusion and consequent risk of failure/waste/schedule bloat.


### Loop in manufacturing and quality before the ECO is drafted


Many ECO problems originate because:


- manufacturing,


- suppliers,


- or quality teams


were not consulted early enough. Operating on a no-surprises basis is by far the safest approach.


Supplier input should be actively encouraged/sought, as it often identifies:


- tooling risks,


- unrealistic tolerances,


- lead time impacts,


- or inspection challenges.


### Tie every ECO to cost and schedule impact


Even “small” changes may:


- increase machining time,


- require new tooling,


- affect plating,


- delay qualification,


- or create scrap exposure.


Operational visibility


*matters* .


## How Jiga makes ECOs easier to execute with contract manufacturers


Most organizations already understand their own internal mechanics of ECOs. The failure point is usually the contract manufacturing interface:


- revision communication,


- implementation timing,


- WIP handling,


- and verification.


These are fundamentally communication and traceability problems.


Jiga addresses these through:


- direct client/supplier communication –


*supported* by Jiga team members who do not stand-between,


- centralized drawing and revision attachment,


- traceable part records,


- integrated inspection workflow requirements baked-in to the supply chain,


- and supplier visibility during implementation.


Instead of changes being buried inside email chains, the supplier works against the current controlled documentation attached directly to the manufacturing workflow.


This becomes especially important during:


- [prototype-to-production transitions](https://jiga.io/articles/prototype-to-mass-production/) ,


- supplier substitutions,


- tolerance changes,


- or process changes requiring verification.


Pre-shipment inspection also helps reduce one of the most common ECO failures:


- incorrect revisions shipping, delivering disruptive surprises.


The operational advantage is not merely faster communication, but maintaining alignment between:


- the engineering definition,


- supplier implementation,


- and inspected delivered hardware.


## Conclusion


An engineering change order is not


*simply* obsessive administrative bureaucracy – it is the mechanism that keeps manufacturing synchronized with design intent.


The change process matters most when the change itself appears minor, to the originator – but carries unobserved complexities that other responsible parties can see. Small undocumented changes accumulate into major production discrepancies over time.


Define the change clearly, classify it precisely, specify effectivity


*explicitly* ,


[communicate with suppliers early](https://jiga.io/articles/direct-supplier-communication/) , resolve WIP disposition formally, and verify implementation through inspection. Sequence consistency is what separates controlled production from revision chaos.


## Frequently Asked Questions


What does effectivity mean on an engineering change order?


Effectivity defines when the ECO becomes active and which products or production lots it applies to. It may specify:


- immediate implementation,


- next purchase order,


- serial number ranges,


- or implementation after inventory depletion.


How do you handle work-in-progress when an ECO is issued mid-production?


The ECO should explicitly define WIP disposition:


- use as-is,


- rework,


- scrap,


- segregate,


- or return to supplier.


Leaving WIP status ambiguous is one of the fastest ways to create mixed revisions in the field.


Does a supplier need to perform a first article inspection after an engineering change?


Often yes – especially after Class I changes affecting:


- fit,


- form,


- function,


- materials,


- tolerances,


- or manufacturing process.


A delta FAI verifies that the changed features were implemented correctly.


Can you use Jiga to manage engineering changes with contract manufacturers?


Yes. Jiga supports revision-controlled supplier communication by attaching current drawings, revisions, specifications, and inspection workflows directly to the manufacturing record, helping reduce miscommunication and revision-control failures during ECO implementation.
