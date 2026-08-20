---
schema_version: "1.0.0"
document_id: "c194d9dc819482b1dad6f6a6327e64b99ed347ba10c31b6ea02408383d05d278"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/rollup-catalog-debt-diligence-2026"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-08-06T19:14:01.668988+00:00"
fetched_at: "2026-08-06T19:14:09.920895+00:00"
content_hash: "sha256:263716c2c883965eb728da69caf4b40364dc4faed976311e5cd16113049e23f4"
---

# The Roll-Up Liability Nobody Diligences: Catalog Debt

M&A diligence on a distributor prices EBITDA, branch density, and supplier overlap down to the decimal. It does not price what happens when two item masters, two taxonomies, and two duplicate SKU sets get pushed together and nobody owns the merge. We measured 29 PE-backed roll-ups for the Top Distributors 2026 index, and the pattern is visible in the data: the more a company grows by acquisition, the more likely its own catalog stops being something a buyer — or a customer, or a crawler — can actually see in one place.


## What diligence actually prices


[QXO's $2.25 billion purchase of Kodiak Building Partners](https://distributionstrategy.com/2026/04/qxo-completes-2-25-billion-kodiak-deal-expands-into-lumber-distribution/) , which closed April 1, came with a specific, quantified synergy claim: 16 of Kodiak's top 20 vendors already overlap with legacy Beacon, according to reporting on the deal from[HousingWire](https://www.housingwire.com/articles/qxo-kodiak-building-partners/) . That is exactly the kind of number diligence teams are built to produce — supplier overlap, quantified to the vendor, disclosed to investors. No equivalent number exists for catalog overlap. Nobody discloses what percentage of Kodiak's SKUs already have a match in Beacon's[item master](https://www.anglera.com/glossary/item-master) , because almost nobody measures it before close.


Distribution Strategy Group has covered both sides of this gap. Its["What Private Equity Firms Want in a Distributor"](https://distributionstrategy.com/2021/07/what-private-equity-firms-want-in-a-distributor-a-look-behind-the-curtain/) piece lays out the standard platform checklist — cash conversion, management depth, ERP systems that can absorb 50 to 200 percent growth, vendor relationships without excessive concentration. Product data doesn't appear on that list. And in["The Minefields of Building Companies through Acquisition,"](https://distributionstrategy.com/2020/11/the-minefields-of-building-companies-through-acquisition/) Bill Wade gets closer, naming "IT and accounting standardization" as one of ten integration failure points — different systems, different data quality, benefits that never materialize because nobody prioritized the merge. That's the right instinct, buried at number ten on a people-and-culture list, treated as an operational headache rather than something with a price tag before the deal closes.


## The measurable pattern


We didn't set out to prove this thesis. It fell out of scoring the roll-up archetype on the same[Digital Readiness Index](https://anglera.com/blog/top-distributors-2026/methodology) we apply to every distributor in the index — four pillars, fourteen signals, all measured from a company's own live site, no self-reported claims. Of the 29 PE-backed roll-ups we classified, only four carry a published score at all. Three came back with no public catalog to sample from whatsoever. Four more looked the same way until a second verification pass found live product pages hiding one owned banner deep — the corporate parent's own site is brochure and recruiting content, and the actual catalog lives on a regional brand nobody outside the industry has heard of.


US LBM is the clean example: the corporate site is spread across roughly 48 separate, locally branded building-materials sites, and the sampleable catalog we eventually found lives on one banner, Higginbotham Brothers. That's not a broken website. It's the direct, structural output of a strategy that keeps acquired brand names intact on purpose, because local reputation is worth more than a unified front end — a trade-off that's usually correct on the revenue side and invisible on the data side until someone tries to measure it.


The four roll-ups that did score landed at a median of 56 — Imperial Dade at 54, Veritiv at 61, SunSource at 52, Distribution Solutions Group at 58 — barely under the 58 median across the full 200-plus-distributor index. On four data points that's not proof the model is fine; it's proof the sample is too small to know yet. What it does show, inside those four, is where roll-ups specifically bleed points. Identifier discipline split the hardest: Imperial Dade carries GTINs on 80 percent of its sampled products, against zero for the other three, despite all four running a broadly comparable acquire-and-integrate playbook. SunSource showed a 24-point spread between its richest and thinnest product page on a single storefront — triple Imperial Dade's spread — meaning even a roll-up that consolidated onto one platform can still be treating its own legacy SKUs unevenly underneath.


## Catalog debt compounds like real debt


Every acquisition in a roll-up doesn't just add revenue and branches. It adds a taxonomy that classifies fasteners, or pipe fittings, or safety gear, differently from the acquirer's taxonomy. It adds attribute fields the parent's PIM doesn't have a column for. It adds duplicate SKUs under different part numbers for the same physical product, because the two companies never sold to overlapping customers before the deal made them siblings. None of that shows up on a balance sheet. All of it compounds, because the tenth acquisition inherits the reconciliation debt of the previous nine, unmerged, plus its own.


That's the frame trade press coverage of these deals is missing: technical due diligence firms already price remediation cost and timeline directly into software M&A valuations and earn-outs when the target's codebase is a mess. Distribution roll-ups deserve the same discipline applied to the item master — a line item with a number, not a footnote about "system integration" buried in the 10-K's risk factors. A rough version of that number isn't hard to build: SKU overlap rate, taxonomy delta, percentage of the target's catalog that would need re-mapping before it can sit next to the parent's — an integration engineer could quote that in a week, the same way they'd quote a codebase's remediation cost.


## The payoff is real when someone does the work


The counter-evidence sits inside our own four scored companies. Imperial Dade's 80 percent[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) coverage after 97 acquisitions since 2007 isn't an accident — somebody made identifier discipline a standing integration requirement, deal after deal, rather than a one-time cleanup project. That's the difference between a roll-up whose cross-sell synergy is a slide in the investor deck and one where a shared customer can actually search both legacy catalogs and get one clean result. The item master is the thing that makes cross-sell real instead of aspirational, and it's the thing almost nobody diligences before they buy it.


This is the exact gap Anglera sits in. Your PIM stores the merged catalog once you build one; we do the unglamorous work of reconciling SKUs, attributes, and taxonomies across acquired brands so that work doesn't wait for the next platform migration. For a roll-up already three or four deals into its thesis, that's not a rebuild — it's additive, and it can start from whatever flat files the last acquisition left behind.
