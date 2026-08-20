---
schema_version: "1.0.0"
document_id: "52964339b91f0e3a0a58eeb8a5d76b2672fd3806b7e56920fa1cd6feadb04882"
company_key: "yc-cofactr"
company: "Cofactr"
source_id: "yc-cofactr-news-import-06c16db4e4eb"
canonical_url: "https://www.cofactr.com/articles/what-your-cmems-should-be-able-to-answer-about-parts-compliance-and-quality"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-27T08:34:13.149498+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:36e8c57a3a28bed280a54f705ef29888a2e409d594c58070015be478687a6269"
---

# What Your CM/EMS Should Be Able to Answer About Parts Compliance and Quality

##


When you're building hardware for aerospace, defense, or other high-reliability applications, your contract manufacturer (CM) or electronics manufacturing services (EMS) provider isn't just assembling boards — they're operating inside your quality boundary. Their materials handling, traceability systems, and environmental controls are your controls. A gap in their processes is a gap in yours, and in this industry, those gaps show up as failed audits, customer escapes, and program risk.


The problem is that most CMs have learned to say the right words. "We follow J-STD-033." "We have full traceability." "Our facility is climate-controlled." What they often can't do is show you the records, walk you through the audit trail, or tell you how fast they can produce a report when your customer asks for it.


The following questions are designed to move past the talking points. Ask them before you award a program, and revisit them during your first program review. The answers will tell you exactly how mature your CM's compliance infrastructure really is.


## 1. How are date codes, lot codes, and Certificates of Conformance tracked — and how fast can you pull them per build?


This is the table-stakes traceability question, and it has two parts that CMs often answer differently.


The first part — how are they tracked — should yield a specific answer: what system captures the data (ERP, MES, paper log, or hybrid), at what point in the receiving and kitting workflow it's recorded, and how it's tied to specific work orders and build lots. If the answer is vague ("we record it in our system"), push harder. Where in the system? Can you see it? Is it linked to the serialized unit or only to the lot?


The second part — how fast can you get a report — is where most CMs stumble. If your customer asks for date codes and CoCs for every component on a specific serial number, your CM should be able to pull that in hours, not days, and in a structured format you can actually use (not a stack of PDFs from a filing cabinet). Ask them to show you an example report. Ask what happens if a CoC wasn't captured at receiving — what controls prevent that part from making it into a build?


The right answer includes a hard block or at least a hard flag in their workflow: a part without a captured CoC doesn't get kitted. If there's no such control, you're depending on operator discipline, and that's not a quality system.


## 2. Are parts handled per J-STD-033? What does your audit trail look like?


J-STD-033 governs the handling, packaging, shipping, and use of moisture-sensitive devices (MSDs) — a category that covers a large and growing share of modern components. A CM claiming compliance needs to demonstrate more than an MSD policy document.


Specifically, you want to know:


- How are MSDs classified and labeled at receiving? Is the moisture sensitivity level (MSL) pulled from the manufacturer's datasheet and applied to the part record, or is it assumed?
- How is floor life tracked? When a bag is opened, what system logs the time? If a bag is left open over a shift change or a weekend, what catches it?
- What are the bake procedures for parts that have exceeded floor life or have unknown exposure history? Who authorizes a bake, what equipment is used, and how is the bake documented?
- What's the dry storage environment — desiccated cabinets or a dry room — and what monitoring exists to verify humidity levels are maintained?


The audit trail question is key. If they can't show you a record that ties a specific bag opening event to a time-stamped floor life clock, and a corresponding closure or bake record, their J-STD-033 compliance is procedural, not operational. That distinction matters when you're facing a field failure investigation.


## 3. Is component storage maintained at 25°C ± 10°C? How is that monitored and recorded?


This is the basic storage environment requirement that appears across IPC standards, most component manufacturer storage guidelines, and virtually every aerospace supply chain quality requirement. It sounds simple. It isn't always.


Ask your CM how temperature is monitored across their storage areas — not just the clean room or the SMT floor, but the receiving dock, the kit staging area, and the long-term inventory shelves. Ask whether monitoring is continuous or spot-checked, and whether there's a data logging system that records temperature against time with enough resolution to catch a compressor failure or a door left open overnight.


Then ask what happens when an excursion is detected. Is there a written corrective action procedure? Is there a threshold at which affected inventory is quarantined pending review? Do they track whether any specific lot was in storage during a recorded excursion?


A mature CM will have data loggers throughout the facility, automated alerts tied to threshold violations, and a procedure for tracing affected inventory. If they're checking a wall thermometer once a day and writing it in a logbook, that's not a quality system for aerospace-grade components.


## 4. Can you support GEIA-STD-0003 Level 1 handling and storage? How do you verify it?


GEIA-STD-0003 sets humidity controls for long-term electronic component storage — Level 1 requires ≤10% relative humidity, which is significantly more demanding than standard J-STD-033 dry storage. This matters for long-cycle aerospace and defense programs where components may be stored for years before use, and for programs operating under RCC 319-25 or similar government specifications that explicitly reference it.


Not every CM can support this, and that's a legitimate answer — but it needs to be a direct one, not a pivot to "we have controlled storage." Level 1 compliance requires purpose-built dry rooms or nitrogen-purged cabinets with continuous humidity monitoring, not desiccated cabinets that might get close on a good day.


If your program requires it, you need to see the facility, verify the monitoring equipment is calibrated and data-logged, and confirm there's a documented procedure for breach response and affected inventory disposition. You also want to know whether their Level 1 storage is segregated from general inventory and how parts are transferred in and out without compromising the controlled environment.


## 5. How is component serialization tracked and managed?


For programs that require serialized component tracking — which increasingly includes any build destined for a major aerospace prime or defense integrator — ask your CM how serialization is handled end-to-end.


This means: How is a serial number assigned? Is it scanned at placement, at test, at final inspection, or some combination? How is the serialized unit record linked back to the lot, date code, CoC, and supplier for each component installed in it? If a specific serial number needs to be traced after delivery, how long does that query take and what does the output look like?


Some CMs handle serialization in their MES at the board level but have no component-level serialization — they can tell you what lot a component came from, but not which specific unit from that lot was installed in a specific serial number. Whether that's sufficient depends on your customer's requirements. Know what you need before you ask the question, so you can evaluate their answer accurately.


Also ask how serialization records are retained and for how long. For defense programs, you may have contractual requirements for record retention measured in decades, not years. Your CM's document retention policy needs to align.


## 6. How is my inventory kept separate from other customers?


Inventory segregation is a basic requirement that gets answered with varying levels of rigor. "We use dedicated shelving" is not the same as a controlled, labeled inventory location with receiving procedures that prevent co-mingling and a system that enforces physical location as a required field.


Ask whether your inventory has dedicated physical storage locations — not just labeled but locked, caged, or access-controlled. Ask how parts are labeled at receiving and whether the customer ownership field is required in their ERP. Ask what happens if a part is pulled from your inventory and issued to the wrong work order — is that catchable before placement, or only discoverable in a reconciliation?


For programs with government-furnished material (GFM) or controlled inventory, the bar is higher. You'll want to know whether your CM has experience managing GFM under FAR clauses and what their property management procedures look like.


## 7. Can you handle and store parts per MIL-PRF-81705 Type I VBB antistatic packaging? How is compliance verified?


MIL-PRF-81705 Type I specifies vapor barrier bags (VBBs) with defined moisture vapor transmission rate (MVTR) limits, electrostatic discharge (ESD) protection properties, and low-outgassing/low-contaminant material requirements. Programs under defense contracts with explicit packaging specifications — particularly for long-term storage or field deployment — may require it.


This is a different question from "do you do ESD packaging," and the difference matters. Standard antistatic bags used for commercial electronics may not meet the MVTR requirements of MIL-PRF-81705 Type I. Many CMs that claim VBB capability are working with commercial equivalents that are close but not qualified.


Ask whether they can source qualified MIL-PRF-81705 Type I material, or whether they require you to provide it. Ask about their desiccant specification — the standard requires low-contaminant desiccant, and some common commercial desiccants contain volatile compounds that can degrade sensitive components over long storage cycles. Ask how they handle VBB replacement cycles: when a bag is opened for inspection or rework, what's the procedure for resealing, desiccant replacement, and documentation?


If your program requires it, you want records showing bag lot numbers, desiccant lot numbers, seal dates, and any bag openings with corresponding replacement records. If your CM can't describe that workflow, they haven't actually implemented it.


## What the Answers Tell You


A CM that can answer these questions specifically — naming systems, describing procedures, walking you through what a report looks like — is a CM that has built their quality infrastructure intentionally. A CM that gives you policy language and promises to get back to you with specifics is telling you something important about how those systems will perform when you actually need them.


The right time to discover your CM's traceability system can't produce a per-unit date code report in four hours is before your prime contractor asks for one, not after. These questions are how you find out before it matters.


##


*Cofactr is an AI-powered electronics procurement and supply chain platform built for aerospace, defense, and robotics manufacturers. Our managed supply chain services are designed to meet the traceability, environmental, and handling requirements your programs actually require — with the audit trails to prove it.*
