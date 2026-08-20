---
schema_version: "1.0.0"
document_id: "e8ce211501b1a0bb92e63882b20df7e9a588404acdd4ede084bcbf5fa9355c51"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/bill-of-materials"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-08-09T23:40:56.159161+00:00"
fetched_at: "2026-08-09T23:40:57.548506+00:00"
content_hash: "sha256:d26aaa4e38e80b2b5c88f876564c4787e17d69fa17abfbfb8444bb362bda6a9a"
---

# Bill of Materials: What a BOM Really Tracks (and Why It's Always Slightly Wrong)

A bill of materials, or BOM, is the structured list of every component, sub-assembly, raw material, and consumable that goes into making a finished product. It includes part numbers, descriptions, quantities, units of measure, revision levels, and the relationship between parents and children in the assembly tree. ERP, MRP, and PLM systems all read from it.


That's the clean definition. Now the messy truth: at most companies, there are two BOMs. The engineering BOM (eBOM) lives in the CAD or PLM system and represents how the product is designed. The manufacturing BOM (mBOM) lives in the ERP and represents how the product is actually built, with consumables, packaging, and process-specific items added in. The two should match. They almost never fully do. Reconciling them is a job.


## What a Working BOM Includes


A real BOM, the kind procurement and production actually use, tracks more than just parts.


Part number and revision. Description. Quantity per assembly. Unit of measure (each, kg, ft, L, m). Reference designator if it's an electronic component (R12, C5, U3). Approved manufacturer and approved manufacturer part number (often multiple, with a preferred ranking). Make-or-buy flag. Lead time. Cost rollup. Effective dates and revision history. Where-used data showing every parent assembly that consumes this part.


The where-used field is the one most teams underweight. When a supplier discontinues a part or a tariff hits a country of origin, the buyer needs to know every product affected, and they need to know in five minutes, not two days. A BOM without good where-used data turns a sourcing decision into a forensic investigation.


## eBOM vs mBOM vs sBOM: Why the Same Product Has Three Lists


The naming gets confusing. Worth being precise.


The engineering BOM (eBOM) is the design output. It comes out of SolidWorks, Creo, NX, or whatever CAD system the engineers use, and it's owned by engineering. It lists what the product is designed to be made of.


The manufacturing BOM (mBOM) is the build output. It includes everything the eBOM has, plus consumables (loctite, solder paste, lubricants), packaging materials, and sometimes labor as a phantom part. It's structured by how the product is assembled on the floor, which doesn't always match how it's designed in CAD.


The service BOM (sBOM) is the spares list. It includes the subset of parts that a service tech might replace in the field, along with kits and serviceable assemblies. Aftermarket teams own it.


In a world with one PLM system everyone uses, these would all sync. In the real world, the eBOM lives in PLM (Windchill, Teamcenter, Arena), the mBOM lives in the ERP (SAP, NetSuite, Epicor, Oracle, JDE), and they get out of sync every time engineering pushes a revision and the ERP integration lags by 48 hours. The buyer then orders against a stale mBOM and finds out two weeks later that revision C is no longer the active rev.


## The BOM in Procurement's Day


Buyers care about three things in a BOM: what to source, in what quantity, and against which revision.


A typical Tuesday in procurement looks like this. The MRP run kicks out a list of parts to order based on demand. Each part is tied to a BOM line item. The buyer pulls the approved manufacturer list, picks a supplier, and cuts a PO. If the BOM has good cost rollup data, the buyer can spot a part that's drifted 11% above its target standard cost and flag it before issuing the PO. If the BOM doesn't, the variance shows up at month-end close and finance asks why.


The deeper issue is that procurement's BOM-driven decisions are only as good as the BOM. Three things go wrong over and over.


A part on the BOM has been EOL'd by the manufacturer six months ago and nobody updated the AML. The buyer sends the PO. The distributor responds saying the part is no longer available, here's the new revision, please confirm.


The BOM lists "any approved manufacturer" but the engineer who wrote it actually only validated one. The buyer sources from a different approved manufacturer, the parts arrive, and quality fails them in inspection because the spec was tighter than what the BOM said.


The BOM has "ECO pending" against a line item but doesn't say what's changing. The buyer either guesses or has to chase engineering. By the time the answer comes back, lead time has eaten into the build schedule.


These aren't BOM bugs. They're BOM hygiene problems, and they show up in every company we talk to.


## Single-Level vs Multi-Level BOM


A single-level BOM lists only the immediate children of the parent. One level deep. Useful for a buyer ordering a specific assembly's components. Not useful for understanding total demand on a raw material that's consumed across three sub-assemblies.


A multi-level BOM (or "indented BOM") shows the full tree. Parent, sub-assemblies, sub-sub-assemblies, all the way down to raw materials. This is what MRP runs against. It's how the system figures out that you need 4,200 of part X across three different products this quarter, not 1,400 like the single-level view suggests.


If a procurement team is making sourcing decisions off single-level BOMs, they're missing volume aggregation. That's negotiating power left on the table at every supplier conversation.


## Industry-Specific BOM Requirements That Trip Teams Up


Aerospace requires lot and serial traceability down to the raw material heat number, per AS9100. The BOM has to support batch-level tracking. If you're running aerospace work in a generic ERP without lot-tracking, you have a problem.


Automotive PPAP requires the BOM submitted at part submission warrant to match production exactly, with every change re-submitted. A late ECO that doesn't get re-submitted is a non-conformance.


Medical devices under FDA 21 CFR Part 820 require design history file (DHF) traceability that ties the BOM to verification and validation records. The BOM is part of the device master record (DMR) and is audited.


Electronics with RoHS or REACH compliance need substance data per part, which most BOMs don't carry natively. The BOM has to be enriched with material declarations.


These aren't optional fields. They're regulatory. Companies running these workflows in spreadsheets are one audit away from a finding.


## The BOM Hygiene Take


Most BOM problems are not technology problems. They're process problems. Engineering pushes a revision. Procurement doesn't see it until the next day's MRP. Quality flags a substitution. The AML doesn't get updated. Tooling moves to a new supplier. The BOM still references the old one. Each individual gap is small. Compounded across 6,000 line items and four years, the BOM is 12% wrong and nobody knows which 12%.


Spot checks help. Pick 20 random BOM lines a month and verify the AML, the cost, the lead time, and the revision against reality. The teams that do this find issues every single time. The teams that don't think their BOM is fine until they hit a sourcing crisis and discover it isn't.


## Where Lumari Helps With BOM-Driven Procurement


Lumari sits next to the ERP and the email inbox. When a supplier emails about a part discontinuation, an obsolescence notice, a revision change, or a price adjustment, Lumari reads the message, ties it to the affected BOM lines and POs, flags the impact, and writes the supplier response back into the system of record. So when a manufacturer EOLs a part, the procurement team sees the where-used impact immediately, not three weeks later when MRP fails.


If your BOM-driven procurement is held together by email and good intentions,[see how Lumari keeps the supplier voice synced to the BOM](https://lumari.ai/) .
