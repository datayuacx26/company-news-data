---
schema_version: "1.0.0"
document_id: "4f1974b7667842d6cf92479bd5bb41c88c46a5b62b7f67745f80c9a0bd31e8f5"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/procurement-automation-sap"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-24T10:12:39.972373+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:56432eec9c086d08f960cfc52b7220b8d238e66bea1a5b7beeeba8ee7d1d7c84"
---

# SAP Procurement Automation: What MM Covers and What Still Runs on Email

Monday morning, 7:45am. A buyer runs ME2N, filters for open PO lines, exports to Excel, and starts writing emails. "Hi, just following up on PO 4500123876, can you confirm the ship date?" Forty times. She'll do it again Thursday.


Her company spent seven figures on SAP. The purchase orders are immaculate. The follow-up is artisanal.


That's the state of SAP procurement automation at most manufacturers we talk to, and it's worth being precise about why. SAP automates a lot, just not the part that eats your buyers' days.


## The Part SAP Already Automates Well


Credit where due. Inside the four walls of the system, SAP MM is genuinely automated.


MRP turns demand into purchase requisitions without anyone touching them. If your source lists and purchasing info records are maintained, those requisitions convert straight into POs, right vendor, agreed price, no buyer involved. Release strategies route the big ones to approvers by value, plant, or purchasing group. Output determination fires the PO off to the vendor the moment it's released. And on the back end, invoice verification matches invoice against PO and goods receipt, blocking payment on discrepancies.


If your master data is clean (a real "if"), a replenishment PO for a known part can go from requirement to transmitted order with zero human touches. That's not marketing. It works, and it's been working since the R/3 days.


So when an SAP shop says "our procurement is automated," they're telling the truth about one half of the process. The transactional half. The half that lives inside SAP.


Then the PO leaves the building.


## What Happens After You Hit Save on ME21N?


The PO arrives in a supplier's inbox. From this moment on, SAP is blind, and everything that follows is manual.


Did the supplier even receive and accept the order? SAP has a real mechanism for this: confirmation control keys. Set the right key on a PO item and SAP expects an order acknowledgment (AB) and later a shipping notification (LA), and MRP will plan against confirmed dates instead of hoped-for dates. It's a well-designed feature.


It's also empty at most companies. Unless the supplier sends EDI (an ORDRSP message), every confirmation arrives as an email or a PDF, and a human has to open the PO and type the confirmed date into the confirmations tab. With hundreds of open lines, nobody does. So the confirmation framework sits there, perfectly designed and unused, and MD04 keeps planning against dates no supplier ever agreed to.


That's the detail that gives away the whole game. SAP's planning logic is only as truthful as the last buyer who typed in a delivery date. Garbage in, MRP exception messages out. Buyers learn to ignore the "reschedule" exceptions because they know the underlying dates are stale, which quietly defeats the point of running MRP at all.


And the[follow-up work itself](https://lumari.ai/blog/stop-chasing-suppliers-ai-po-follow-ups) ? The chasing, the "any update on this?" emails, the escalation when a supplier goes dark on a line-down part? There is no transaction code for that. It's Outlook, a personal spreadsheet, and the buyer's memory.


## Can SAP Send RFQs? Technically, Yes. Nobody Uses It.


SAP has an RFQ flow: create the RFQ (ME41 in ECC, or the S/4HANA equivalents), send it to vendors, maintain their quotations (ME47), run a price comparison (ME49). On paper, a complete competitive bidding process.


In practice, ME47 means a buyer reading each supplier's emailed quote and manually keying every line into SAP before ME49 can compare anything. There's no real handling for the engineering drawings that go out with the RFQ, no tracking of who opened or responded, no chasing of the three vendors who went quiet. So buyers skip the whole thing. They send RFQs from Outlook, collect PDF quotes, and[build the comparison in Excel](https://lumari.ai/blog/supplier-quote-comparison) . The RFQ transactions sit unused next to the confirmation tabs.


We've talked to SAP shops where exactly one person knew ME41 existed, and she didn't use it either.


## Isn't This What SAP Business Network Is For?


SAP's official answer to the supplier communication gap is the SAP Business Network (the Ariba network), plus Ariba Sourcing for RFQs. Get your suppliers onto the network and confirmations, ship notices, and invoices flow in electronically.


For your largest suppliers, this can work. They have IT teams, they do enough volume with you to justify the setup, and they may already be on the network for other customers.


Now think about your supplier number 47, the 12-person machine shop that makes your brackets. They're not joining a network, setting up an account, and learning a portal to confirm a $1,800 PO. Suppliers have grumbled about network fees and enrollment friction for years, and the small shops respond the rational way: they ignore it and keep replying to email. Portal-based supplier enablement consistently stalls on the long tail, and in direct materials procurement, the long tail is where your custom parts and your delivery risk actually live.


So even SAP shops that bought the full Ariba stack end up with a two-tier reality: structured data from a handful of big vendors, and email from everyone else. The email tier is the one that breaks.


## The Real Architecture: SAP as Record, Something Else as Action


The conclusion we keep arriving at, and it applies to[NetSuite](https://lumari.ai/blog/procurement-automation-netsuite) and every other ERP just as much as SAP: the ERP is the system of record, and it's good at that. What's missing is a system of action, the layer that does the supplier-facing work and keeps the record honest.


Concretely, for an SAP shop, that layer needs to do four things:


1.


Send RFQs with drawings and specs from email, track who responded, and chase who didn't.


2.


Read whatever comes back (PDF quote, Excel, pricing typed inline) and normalize it into a comparison without a buyer re-keying it.


3.


Watch every open PO: request the acknowledgment, confirm the ship date as it approaches, detect the "we're running two weeks behind" email.


4.


Write the results back to SAP, so confirmed dates land in the PO confirmations tab and MD04 plans against reality.


This is what Lumari does on top of SAP. It works over plain email, so supplier 47 never changes anything about how they work. When that machine shop replies "ship date moved to 7/18, sorry," Lumari matches it to the right PO line, updates SAP through the integration, and flags the buyer with the original email attached. The buyer makes the judgment call. The typing, chasing, and reconciling stops being her job.


The Monday ME2N export ritual disappears, because the follow-ups already went out and the answers are already in SAP.


## FAQ


**Can SAP automatically follow up with suppliers on open POs?**


No. SAP can send the initial PO via output determination and can generate reminder and expediting messages for overdue confirmations, but the reminder logic depends on confirmation data that's rarely maintained, and it can't read or act on the supplier's reply. Real follow-up (context-aware emails, escalation when a supplier goes quiet) happens outside SAP.


**Can SAP read a supplier's email or PDF quote?**


No. SAP consumes structured input: manual entry, EDI/IDocs, or API calls. An emailed PDF quote or an inline "we can do $4.20 at 8 weeks" reply is invisible to it until a person or an outside system extracts the data and enters it.


**What is confirmation control in SAP purchasing?**


Confirmation control keys tell SAP which supplier confirmations to expect for a PO item (order acknowledgment, shipping notification) and let MRP plan against confirmed dates instead of the original PO date. The mechanism is solid; the constraint is that without EDI, every confirmation must be typed in manually, so at most companies the data simply isn't there.


**Do we need Ariba to automate procurement on SAP?**


No. Ariba addresses the gap with a network-and-portal model, which works for large connected suppliers and stalls on small ones. An email-native layer covers the entire supply base, including the shops that will never join a network, and writes back to SAP either way.


Your SAP system already knows how to plan, approve, and pay. What it's never known is what your suppliers said this morning.[Lumari](https://lumari.ai/) closes that loop on top of SAP: it runs the RFQs, reads the replies, chases the silence, and keeps your confirmed dates real, so MD04 finally means something again.
