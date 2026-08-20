---
schema_version: "1.0.0"
document_id: "e11f9d40e21e80f06ab2fc6b7113cbeb876ffc766864e2bce79a21e365f503a1"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/procurement-software-contract-manufacturers"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-24T10:12:39.972373+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:37d6b2165f0759f1f1fe6ea6af6b1da1794cbb22e20f0ff0d5b8d7d8c1724e70"
---

# Procurement Software for Contract Manufacturers: Why Generic Tools Don't Fit

Procurement software for contract manufacturers has to handle a reality that most procurement platforms ignore: you're buying parts on someone else's behalf, against someone else's bill of materials, often from a supplier list someone else approved. Coupa, SAP Ariba, NetSuite, and most of the AI procurement startups assume your company owns the BOM, owns the AVL, and owns the inventory. Contract manufacturers don't. That single mismatch is why CM (contract manufacturing) procurement teams keep buying enterprise tools and then quietly running the actual work in Excel.


This is a guide for the procurement and ops leaders at EMS providers, machine shops, injection molders, sheet metal fabs, and PCBA houses who've sat through the demo, asked "but what about consigned parts?" and watched the AE pivot to a different slide.


## A Tuesday Morning at a 200-Person EMS Shop


The buyer opens the new program packet from Customer A. 58-line BOM, control panel build, 800 units, first article in six weeks. Inside the packet:


-


12 lines are tagged "AVL-restricted." Specific approved manufacturers, specific part numbers, no substitutes. Customer A's quality team has already qualified those vendors.


-


6 lines are CSPs (customer-supplied parts). Customer A ships them to the dock, the EMS holds them as consigned inventory, BOM cost is $0 on the customer-facing quote. They still get received, kitted, and tracked through WIP.


-


40 lines are open. Source them however, but pass through with a 12% margin (Customer B is 18%, Customer C is cost-plus-fixed-fee).


-


One line has an NRE (non-recurring engineering) tooling charge amortized across the first 5,000 units, then drops off.


-


The whole program is flow-down ITAR, so every supplier touching it has to be on the buyer's ITAR-cleared list (a subset of the general AVL).


The buyer opens SAP Ariba. Or NetSuite. Or whatever's on the desktop. None of those concepts (CSP, per-customer AVL, margin pass-through, NRE amortization, ITAR flow-down) exist as first-class objects. So the buyer opens a spreadsheet. The spreadsheet is named` CustA_ProgramX_Quote_v7_FINAL_actualfinal.xlsx` . That's where contract manufacturing procurement actually lives.


We've talked to enough CM teams to know this isn't an edge case. It's the job.


## What Actually Makes Contract Manufacturing Procurement Different?


CM procurement is its own discipline. Here's what generic tools weren't designed for.


**Customer-supplied parts (consigned inventory).** The end customer ships parts to your dock. You hold them, kit them, consume them, but you never own them and they never hit your PO log. Every standard procurement platform models inventory as "stuff you bought." CSPs break that from line one. The buyer still tracks expected ship dates, receives against a customer reference instead of a PO, and reconciles shortages back to the customer (not the supplier). When a CSP line goes short, your build line stops, and your contract probably says you're not liable.


**Per-program AVLs.** Your customer-approved supplier list is not your supplier list. It's a per-program subset of your suppliers, plus suppliers your customer added that you've never bought from. Customer A approves Murata for capacitors. Customer B approves Murata, Yageo, and Samsung. Customer C requires a specific Murata part number from a specific distributor with specific lot traceability. Same physical part. Three different AVL rules. Most platforms treat AVL as one global table.


**Per-program quality gates.** AS9100 (the aerospace quality management standard) requires First Article Inspection, special process certifications, and supplier monitoring at a level ISO 9001 doesn't. IATF 16949 (automotive) requires PPAP (Production Part Approval Process) submissions. ISO 13485 (medical devices) layers in design controls and supplier qualification records. The same EMS shop might run all three on different lines. Every supplier qualification record has to be tagged to the programs it's valid for. Generic procurement software has one "supplier qualified: yes/no" checkbox.


**ITAR and EAR controls.** If you're on a defense program, every supplier touching the BOM has to be ITAR-registered and the data flow has to stay inside controlled channels. EAR (Export Administration Regulations) adds another layer for dual-use items. For a CM running mixed commercial and defense work, this is the gating control on whether a buyer can even send an RFQ to a supplier. Most platforms treat compliance flags as an afterthought.


**Quote pass-through and margin rules.** When you quote a build back to the end customer, every line item has a markup that's per program, per customer, sometimes per part class. Some customers demand open-book pricing where they see the supplier quote. Others get a single price. Your tool has to support both, and your records have to keep the original supplier quote separate from what the customer saw.


**NRE and tooling amortization.** A new injection mold runs $40K. A test fixture for a PCBA program runs $15K. Tooling cost gets amortized into the unit price over a contractual volume, and if the program ends early, there's a true-up clause. None of that is a normal PO. It's a hybrid of capital tracking, unit cost accounting, and contract terms.


**Customer flow-down terms.** Your customer's contract with their end customer (a prime contractor, an OEM, a defense agency) cascades down to your suppliers. Counterfeit parts clauses, conflict minerals, REACH/RoHS, cyber requirements. The buyer flows those terms into every supplier PO on the program, and documents that they did.


That's the daily reality. Now look at what's on the market.


## Why Don't the Big Procurement Platforms Handle This?


Most of them were built for a different buyer. The architecture assumptions show.


**SAP Ariba** is built around catalog-based indirect spend at large enterprises buying their own stuff. The Ariba Network has 5 million suppliers, but the data model assumes a single buying entity, a single AVL, and inventory the buyer owns. There's no concept of "this BOM belongs to Customer A and the quality requirements come from their flow-down." You can configure custom fields for everything, but you're paying a Big 4 implementation partner $500K to do it, and it still won't model CSP cleanly. We've talked to EMS shops that bought Ariba for the AP automation and then ran procurement in Excel anyway.


**Coupa** has the same problem in a different wrapper. Strong on spend analytics and contracts. Built for a CFO who wants to see indirect spend by category at a services firm. If your procurement pain is per-program AVL enforcement, Coupa wasn't designed to help. One CM procurement manager described it to us as "a finance reporting tool that happens to issue POs."


**NetSuite procurement** is fine if you're a small OEM running your own product line. For a CM, the BOM model assumes the BOM is yours. Multi-customer programs work as a creative workaround at best. The per-customer pricing logic, the consigned inventory handling, the program-level cost rollups: all bolt-ons or third-party scripts. ([NetSuite has its own procurement gaps any team hits eventually.](https://lumari.ai/blog/procurement-automation-netsuite) )


**Plex and Epicor** are closer because they're MES/ERP systems built for manufacturers. Plex (now part of Rockwell) has real shop-floor depth and handles consigned inventory in a way Ariba never will. Epicor's manufacturing modules have program-level cost tracking that's at least possible to configure. Both are ERPs first, procurement tools second. The supplier-facing execution layer (RFQs, quote extraction, PO follow-up, expediting) is thin in both.


**Arena PLM and Propel PLM** handle the BOM and revision side well, especially for hardware companies and contract manufacturers running on customer BOMs. But PLM isn't procurement. It tells you what to buy and at what revision. It doesn't run the buy.


**Aegis FactoryLogix** is strong on the manufacturing execution side for EMS, including some BOM intelligence. Procurement is again a side function.


**SourceDay** is a genuinely good PO lifecycle tool. We've recommended it to teams that needed delivery date tracking and ERP sync. It's not built for CM-specific complexity though. The CSP workflow, the per-program AVL, the quote pass-through logic: not its job and not on the roadmap.


**AI procurement startups** (us included). Most of the new wave (Lumari, Lightsource, Didero, Keelvar) are excellent at specific slices: BOM-level sourcing, supplier execution over email, complex bidding, quote extraction. None of us have built a CM-native platform that handles consigned inventory, per-program AVL gates, and customer flow-down terms as first-class concepts. We're saying that out loud because the alternative is selling you a workflow that doesn't fit.


The bigger pattern: procurement software, generic or AI-native, was built for OEMs. CMs are underserved because they're harder to build for and the buyers are smaller.


## Quick Comparison Table


Platform


CSP Support


Per-Program AVL


Quality Gates (AS9100, IATF, ISO 13485)


Quote Pass-Through


Supplier Execution


SAP Ariba


Workaround


Workaround


Possible with config


No native


Portal-based


Coupa


Workaround


Workaround


Limited


No native


Portal-based


NetSuite Procurement


Workaround


Workaround


Limited


Limited


Manual


Plex / Epicor


Native consignment


Configurable


Strong (Plex)


Limited


Thin


Arena / Propel PLM


N/A (PLM)


Strong on BOM


Strong


N/A


N/A


Aegis FactoryLogix


Strong


Partial


Strong


Limited


Thin


SourceDay


No


No


Limited


No


Strong (PO lifecycle)


Lumari


Tracks but not core


Honors via supplier list


Documents qualification


Yes (margin rules at quote layer)


Strong (email-native)


This is the part of the post where most blogs would tell you Lumari solves everything. We don't. Read the table again. The honest answer is that there's no single CM-native platform that does it all, and any procurement leader at a CM should plan to combine tools.


## What Should CMs Actually Look For?


Here's an evaluation framework that fits the work.


**Start with the BOM model.** Can the tool handle a BOM that belongs to your customer, not you? Can it tag lines as customer-supplied vs. open-sourced vs. AVL-restricted? Can it carry the revision the customer is on, not the one your engineering team thinks is current? If the answer is "you can configure custom fields," that's a no.


**Then look at AVL handling.** Is there a concept of "this supplier is approved for these customers/programs"? Can the platform stop a buyer from sending an RFQ to a supplier who's not on the customer's AVL? Can it document the approval chain (who approved the supplier, when, against what quality record)? See[supplier qualification done right](https://lumari.ai/blog/supplier-onboarding) for the longer version.


**Check the quote workflow end to end.** Most quotes come in as PDFs and Excel files from machine shops that haven't changed their template since 2014. The tool needs to read those, normalize them, and let the buyer build a customer-facing quote with the right margin per program. The supplier quote and the customer quote are different documents. The tool should treat them that way.


**Test the consigned inventory path.** Walk a CSP through receiving, kitting, and consumption. If the tool can't handle inventory that isn't yours, the procurement team will keep tracking CSPs in Excel forever.


**Audit the compliance overlay.** AS9100 FAI. PPAP for automotive. ITAR clearance gates. Counterfeit parts documentation. These aren't optional. The tool either supports them or it doesn't, and the workaround is usually a separate document system nobody updates.


**Then ask about supplier execution.** Once the PO is out, who's chasing the supplier? Who's reading the reply that says "we need to slip by 10 days"? Who's updating the ERP with the new date? This is the work that eats most of a CM buyer's day, and it's the work that platforms designed for the CFO's dashboard don't touch. ([We've written about this gap before.](https://lumari.ai/blog/supplier-performance-scorecards) )


**And finally, run the supplier test.** Pick 10 of your active suppliers at random, not your top 10 by spend. The ones who reply to email and don't have a sales engineer. Will the tool work with them? Or are you going to spend a year asking them to log into a portal?


If a vendor won't let you run a 60-90 day pilot with your real BOMs and real suppliers, walk away. This applies to us too.


## Where Does Lumari Fit in a CM Stack?


Honest answer: we're the supplier-side execution layer.


We handle the operational work that eats your buyers' days. PO follow-ups over email, quote extraction from PDFs, supplier confirmations, delivery date tracking, RFQ chasing. Your suppliers don't log into anything. They reply to email the way they always have, and Lumari reads the replies, normalizes the data, and keeps your ERP current. For CMs running on Plex, Epicor, NetSuite, or a homegrown system, that execution layer is usually the biggest gap.


What we don't do: we're not a per-program AVL manager. We're not a PLM. We don't model CSP as a first-class object. We don't do quality gate documentation for AS9100 or PPAP. If your criteria is "one platform to run a multi-program CM end to end," nobody on the market does that and you should keep your standards high.


The teams we work with combine an ERP or MES (Plex, Epicor, NetSuite) for the system of record, a PLM (Arena, Propel) for BOM control, and[Lumari](https://lumari.ai/) for the daily supplier execution and quote work that turns buyers from email-chasers back into procurement professionals. A single suite that promises everything usually doesn't.


If you're a CM evaluating procurement software and you've been burned by demos that don't survive contact with a real BOM, we'd rather show you what we do and don't fit than pretend we're the whole answer.[Here's how we think about procurement tooling for manufacturers](https://lumari.ai/blog/best-ai-procurement-tools) . Run a pilot on one of your actual customer programs.
