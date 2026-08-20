---
schema_version: "1.0.0"
document_id: "141f25230c92e56369b1dc6e70b9463fbaf0bb56ff2ad45617113038c5e4a9e0"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/approved-supplier-list"
published_at: "2026-05-23T00:00:00+00:00"
first_seen_at: "2026-08-09T23:40:56.159161+00:00"
fetched_at: "2026-08-09T23:40:57.548506+00:00"
content_hash: "sha256:5f2f62049432c1b2e136a2544bc091be665ef9665bc17540f611cccd49604a17"
---

# Approved Supplier List: How to Build One, Keep It Current, and Actually Use It

> **TL;DR:** An approved supplier list (ASL) is the master record of which suppliers your team is authorized to buy from, scoped by part number, commodity, or spend threshold. The point is to enforce consistency: same supplier, same terms, same quality, every time. Most ASLs fail because they're maintained as a static spreadsheet that goes stale within months. The fix is treating the ASL as a living workflow, not a document.


There's a 142-row spreadsheet sitting on a procurement manager's OneDrive right now called "ASL_Master_2024_v7.xlsx." It hasn't been touched since July. Three of the suppliers on it went out of business. Two were acquired. Eleven have new addresses. Four have pricing that's 18 months out of date. The buyer pulling parts off this list still treats it as gospel.


That's the approved supplier list at most companies. A document everyone refers to and nobody owns. This post is about what an ASL actually is, why almost every team's version is broken, and what the working version looks like.


## What an Approved Supplier List Is, Concretely


An approved supplier list is a controlled record that says, for a given part, commodity, or service, here are the suppliers procurement has qualified to provide it. Buyers must source from this list unless they go through a deviation process. The list scopes who you can buy from, often at the part-number level for direct materials and the commodity level for indirect.


In ISO 9001-certified shops it's a quality requirement. Section 8.4 of ISO 9001:2015 says you have to evaluate, select, monitor, and re-evaluate external providers, and you have to keep records. The ASL is the artifact that proves you're doing it. In aerospace under AS9100, it's also tied to AS9120 distributor controls and the customer flow-down requirements that come with each PO. In automotive under IATF 16949, your ASL is what your customer auditors ask to see.


For non-regulated companies the ASL is still operational. It's how you stop a buyer from issuing a PO to a supplier nobody has vetted, or from re-shopping a strategic part that's locked into a long-term agreement, or from accidentally double-sourcing in a way that violates a vendor managed inventory contract.


The list usually carries, per supplier: legal name and DBA, qualification scope (which parts or commodities they're approved for), qualification status (active, restricted, probation, suspended, terminated), date of last qualification, performance scorecard or grade, certifications on file (ISO, AS9100, ITAR, CMR, REACH, RoHS), insurance and W-9 dates, primary commercial contact, and any approval notes. In an ERP, this lives in the vendor master with custom fields or in a parallel system that the vendor master gets reconciled against.


## Why Most ASLs Are Wrong by Friday


The reason most ASLs decay isn't laziness. It's that the list is a snapshot of a moving target, and almost nobody has a process to keep the snapshot current.


Suppliers churn faster than people think. We've seen lists where 12% of suppliers had moved, been acquired, or shut down inside 18 months. Nobody on the procurement team caught it. The buyer found out when an email bounced.


Qualifications expire. ISO certificates have to be renewed every three years. Insurance is annual. W-9s should be refreshed every two to three years. If your ASL doesn't track expiration dates, you've got suppliers on the active list whose certifications lapsed last year. That's a finding waiting to happen if you're audited.


Performance data goes uncaptured. The whole point of monitoring a supplier is to feed that signal back into the ASL: drop the supplier whose OTD is at 67%, escalate the one whose PPAP keeps getting rejected, expand the one carrying you on tariff exposures. Most teams collect the data, then never connect it back to the list. The supplier stays "approved" because nobody made the change.


Buyers stop using it. This is the worst failure mode. The buyer needs a part by Friday, the ASL only has one supplier listed for that part, that supplier said no, and the buyer issues a PO to a new supplier without going through qualification. The PO ships. The new supplier becomes a de facto approved supplier. The list grows shadow entries that aren't on the list at all.


We've seen one company where the official ASL had 280 suppliers and the actual AP run showed 412. The 132-supplier delta was the buyers' real working set. The official ASL was theater.


## The Difference Between an ASL and a Vendor Master


People mix these up. They're not the same.


The vendor master in your ERP is the list of every supplier you can transact with: addresses, payment terms, banking, tax info. It's a financial and operational record. AP needs it. Procurement uses it. Suppliers can be in the vendor master without being approved to supply specific parts.


The ASL is the procurement-and-quality-controlled subset that says which suppliers are authorized to provide what. Every approved supplier is in the vendor master. Not every supplier in the vendor master is on the ASL.


When the two diverge silently, you end up paying suppliers who were never qualified, or refusing to pay suppliers who were qualified but somehow got dropped from the master. The cleanest setups reconcile the two on a cadence and use a single field on the vendor master to flag ASL status, with the qualification details in a procurement system or shared workbook tied back by vendor ID.


## What Has to Be on the List


Every ASL should be able to answer four questions for any supplier on it:


What are they approved to provide? At what part-number, commodity, or service-category level? Specificity matters. "Approved for fasteners" means nothing when one buyer reads it as "all fasteners" and another reads it as "the half-dozen SKUs we tested two years ago."


What's their current qualification status? Active, restricted to specific spend thresholds, on probation pending corrective action, suspended pending requalification, or terminated. The status drives buyer behavior, so it has to be unambiguous.


When does their qualification expire? ISO recertification, insurance renewal, W-9 refresh, financial review. Each has its own clock. Without expiration tracking, you get the slow-motion compliance failure where everyone assumed someone else was watching.


What's their recent performance? OTD, quality acceptance rate, response rate on RFQs, NCR count per quarter. The list shouldn't only carry status. It should carry signal.


A list that captures these four answers at a glance is usable. A list that only has names and addresses is a phone book.


## Building an ASL from Nothing


If you're starting fresh or rebuilding from a known-bad list, this is the order that actually works.


Start with last 18 months of spend. Pull every supplier who received a PO in that window. Sort by spend, descending. The top suppliers carrying 80% of your dollars are where qualification effort goes first. The long tail can be handled in waves.


Document what each supplier provides. Match POs to part numbers and commodity codes. This is where shadow entries surface: suppliers paid regularly with no clear scope. Either qualify them properly or stop using them.


Pull existing certifications. Get ISO certs, insurance COIs, W-9s, and any industry-specific paperwork (AS9100, IATF, NIST 800-171, etc.) on file with expiration dates loaded.


Score on actual performance, not aspiration. OTD and quality acceptance are easy if you've been tracking POs. Don't grade on a curve. A supplier delivering at 78% OTD doesn't get an A because they're a long-term partner.


Define your tiers. Some companies use three (preferred, approved, restricted), some use four or five. Whatever the structure, it has to translate into clear buyer behavior. "Preferred" should mean "default choice." "Restricted" should mean "needs approval above $X."


Set the cadence. Monthly performance refresh, quarterly recertification check, annual full review. Put the cadence on someone's calendar with a name attached, not "the procurement team" generally.


The cadence is the part that fails most often. It only works if a specific person owns the calendar entry and reports out.


## Keeping It Current Without Burning a Headcount


Maintenance is where most ASLs die quietly. It's nobody's full-time job, so it becomes nobody's job.


A few patterns we've seen work:


Tie qualification expirations to automated reminders, ideally pulled into the same workflow tool the procurement team already lives in. If the buyer has to remember to check ISO recertification dates, they won't. If a system pings them 60 days out, they will.


Run a monthly ASL exception report. New suppliers paid through AP without an ASL entry. Active suppliers with no PO in the last 12 months (probably should be retired). Suppliers whose certifications expire in the next 90 days. Suppliers whose OTD or quality dropped below threshold last month. The report drives the meeting. The meeting drives the updates.


Make adding a supplier intentional. The fastest way to corrupt an ASL is to let buyers add suppliers ad hoc. Force a request, a quick qualification, and an approval. Make the path of least resistance "use someone already on the list."


Make dropping a supplier also intentional. Going cold on a supplier without a formal status change is how you end up with 280 listed and 412 paid. If a supplier is being phased out, mark them as "restricted" or "terminated" with a date and a reason. Then the buyer can't accidentally re-engage them six months later.


Reconcile the ASL with the vendor master quarterly. Find the deltas. Decide what to do about each one. This is the cleanup pass that prevents drift from compounding.


## A Take Most Teams Don't Want to Hear


Single sourcing isn't the goal of a good ASL.


The instinct, especially in a small procurement team, is to keep the list short. Fewer suppliers, easier to manage. Pick a winner per category and put the runner-up on a shelf.


That's a fragility trap. The ASL exists to give procurement choices, not to lock in a single answer. A category with one approved supplier and no qualified backup is a category waiting for a supply incident. We've seen this play out: a sole-source supplier had a fire, was offline for six weeks, and the company spent two months getting a backup qualified under emergency conditions because no second source had been kept warm.


Healthy ASLs have at least two qualified suppliers per critical category, with one designated as primary and the second receiving small-volume placements to keep them current. Yes, this means more administrative overhead. It also means you have a Plan B that you can execute in 48 hours instead of six weeks.


The argument against second-sourcing is usually about pricing leverage and consolidation. Both are real. Neither matters when your line is down because your sole source went silent.


## How Lumari Helps Companies Keep the ASL Honest


The reason ASLs decay is that supplier reality lives in email and the ASL lives in a spreadsheet, and the two never talk. New supplier responses, certification updates, address changes, performance signals, all of it shows up in the inbox and almost none of it makes it back to the list.


Lumari watches the supplier inbox, parses what suppliers actually send (acknowledgments, ASNs, certifications, capacity updates, financial changes), and pushes the relevant data back to the systems where procurement runs the ASL: ERP vendor master, Excel, Notion, or whatever you're using. It chases for missing certifications before they expire, flags suppliers who've gone quiet for too long, and surfaces shadow suppliers showing up in AP without an ASL entry.


If you've got a stale supplier list and a buyer team that doesn't have time to clean it up,[book a Lumari demo](https://lumari.ai/contact-us) and we'll show you what's actually moving through your supplier inbox versus what your list says.


## FAQs About Approved Supplier Lists


**What is the difference between an AVL and an ASL?** An AVL (approved vendor list) and an ASL (approved supplier list) are usually the same thing, with "vendor" and "supplier" used interchangeably depending on industry. Some companies use AVL for the broader pool and ASL for the qualified subset, but most use them as synonyms.


**Is an ASL required by ISO 9001?** ISO 9001:2015 doesn't mandate the term "approved supplier list" specifically, but Section 8.4 requires you to evaluate, select, monitor, and re-evaluate external providers and to maintain records. An ASL is the standard artifact for satisfying that requirement.


**How often should the ASL be reviewed?** Monthly for performance and exceptions, quarterly for certification renewals and reconciliation against the vendor master, and annually for a full review of every active supplier.


**Who owns the ASL?** Procurement owns the operational maintenance. Quality owns the qualification criteria and audit trail. The two have to coordinate, especially in regulated industries. A common failure pattern is procurement adding suppliers without quality sign-off and quality flagging suppliers without procurement updating the buyer-facing list.


**What happens when a buyer needs to use a supplier not on the ASL?** The right answer is a documented deviation process: the buyer requests an exception, procurement and quality review and either approve or deny, and if approved, the supplier gets fast-tracked through qualification rather than added permanently as a one-off. The wrong answer (and the common one) is the buyer just issuing a PO and dealing with it later.


**What's the difference between an ASL and a preferred supplier list?** A preferred supplier list is a subset of the ASL, usually the top-tier suppliers a buyer should default to. The ASL is "you're allowed to buy from these." The preferred list is "you should buy from these unless there's a reason not to."
