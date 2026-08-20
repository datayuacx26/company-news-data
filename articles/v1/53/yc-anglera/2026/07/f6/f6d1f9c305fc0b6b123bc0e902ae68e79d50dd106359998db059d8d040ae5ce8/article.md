---
schema_version: "1.0.0"
document_id: "f6d1f9c305fc0b6b123bc0e902ae68e79d50dd106359998db059d8d040ae5ce8"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/erp-item-master-is-not-a-catalog-2026"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-08-06T19:14:01.668988+00:00"
fetched_at: "2026-08-06T19:14:09.920895+00:00"
content_hash: "sha256:8e31d5b338accb94f92a730dfce0c1b1fd880e73ddfdced005c29930e2b5e9b5"
---

# The Item Master Is Not a Catalog: Why Replatforming Won't Fix Your Product Data

When an ERP replacement lands on the roadmap, everything customer-facing tends to queue up behind it — catalog work, ecommerce, content — on the assumption that once the "system of record" is fixed, the rest will follow. That assumption is backwards, and it is an expensive place to be wrong. The[item master](https://www.anglera.com/glossary/item-master) inside an ERP is a transactional record — SKU, cost, GL code, vendor, bin location — not a catalog. It was built to make invoices balance and inventory reconcile, not to answer a buyer's question or feed a structured product page. The catalog and the ERP have different jobs and different clocks, and only one of them can afford to wait two years.


## The business case DSG wants, pointed the other direction


[Distribution Strategy Group has argued](https://distributionstrategy.com/2025/02/how-to-justify-an-erp-investment-get-specific/) that ERP investments need to be justified with specifics — named processes, named savings, a real business case instead of a vague modernization pitch. That discipline is correct, and distributors should hold their ERP vendors to it. But the same discipline has to run the other direction: what, specifically, is being deferred while the ERP consumes budget and attention? If the answer is "the catalog and digital storefront wait for the new system," that is itself a decision with a cost, and it deserves the same rigor as the ERP business case. Most operators never write that cost down. It shows up two years later as a stale product-detail page and a buyer who left for a competitor's site.


## The trough is longer than the estimate


[Distribution Strategy Group has also written](https://distributionstrategy.com/2023/01/why-your-new-erp-is-worse-than-your-old-one-for-now/) about the "trough of despair" that follows a go-live, when a stripped-down new system underperforms the old one for what the piece estimates at 12 to 18 months. The independent data suggests that estimate is optimistic. Panorama Consulting's 2025 ERP Report puts the average mid-size implementation at 17.4 months against a 12-month plan — 3.6 months over — and finds 68% of projects run longer than scheduled. Stack the trough on top of the overrun and a distributor is realistically looking at close to two years where IT bandwidth, executive attention, and budget are pinned to accounting reconciliation and inventory valuation. Nobody in that window is arguing for catalog investment. "Wait for the new system" becomes the default answer to every digital request, and the catalog decays on schedule.


## What an item master actually contains


Walk the field list on a typical ERP item master and the gap explains itself. Cost, list price, vendor part number,[unit of measure](https://www.anglera.com/glossary/uom-unit-of-measure) , bin location, GL classification, reorder point — every field answers an accounting or operations question. None of it answers what a buyer or a search engine needs: dimensioned specs, compliance documents, cross-references to superseded parts, application notes, images clean enough to zoom, a taxonomy that holds together across categories. ERPs are not bad at this by accident. Attachments live as unstructured blobs if they exist at all; there is no governed attribute schema because the system was never asked to have one. Expecting the item master to double as a catalog is expecting an invoice to double as a spec sheet.


## Our own numbers say the same thing


Anglera's[Top Distributors 2026 index](https://anglera.com/blog/top-distributors-2026) measures customer-facing digital performance across 200+ distributors using the[Digital Readiness Index](https://anglera.com/blog/top-distributors-2026/methodology) — four pillars, fourteen signals, all measured from each distributor's own live site. Cross-reference those scores against what's publicly known about each distributor's ERP posture — recent go-lives, RFPs, vendor press releases — and there is no clean line between ERP modernity and DRI score. Distributors mid-migration land across the full range. Some running ERPs that predate the smartphone score higher on catalog completeness and structured product data than peers on brand-new platforms. The variable that actually predicts a strong score isn't which generation of ERP a distributor runs. It's whether anyone owns product content as a standing function, independent of whatever system holds the transactional truth that quarter.


## What survives the replatform


Here is the part worth building a roadmap around: enrichment is one of the few digital investments that comes through a replatform intact. Clean, structured product data — attributes, taxonomy, assets, descriptions — maps into a new item master's fields the way a spreadsheet maps into a new spreadsheet. What doesn't survive is the layer built on top of the old schema: custom reports, workarounds, integrations wired to fields the new system renamed or dropped. That asymmetry is why treating PIM-layer content as decoupled from the ERP has become standard advice in its own right, not just an Anglera talking point —[PIM vendors describe the pattern the same way](https://pimberly.com/blog/pim-vs-erp/) : rich content sits outside the ERP specifically so an ERP transition doesn't take the catalog down with it. Anglera doesn't touch the ERP and isn't a PIM replacement — your PIM stores the data, Anglera does the work of getting it clean and complete before the ERP question is even settled.


## The buyer isn't waiting for your go-live


The stakes for the wait-and-see approach just went up. Gartner projects AI agents will[intermediate more than $15 trillion in B2B purchases by 2028](https://catalistgroup.co/blog/b2b-ai-sourcing-future/) , and those agents don't tolerate the gaps a human buyer used to shrug off. An inconsistent spec or a missing attribute used to be a minor inconvenience; to an agent doing structured comparison, it's a disqualifier. A two-year ERP window with the catalog frozen isn't a neutral pause — it's a two-year head start for every competitor who kept enriching product data while the ERP business case worked its way through committee.


The fix isn't refusing the ERP investment or rushing it. It's refusing to let its timeline set the catalog's. Distributors who decouple the two — running enrichment as a continuous function that starts from whatever flat file or feed exists today, live in weeks rather than waiting on a system migration measured in quarters — walk out of the ERP trough with a catalog that's better than the one they went in with, instead of one that's two years staler.
