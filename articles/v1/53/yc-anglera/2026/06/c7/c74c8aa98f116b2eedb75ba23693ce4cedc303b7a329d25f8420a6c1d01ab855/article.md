---
schema_version: "1.0.0"
document_id: "c74c8aa98f116b2eedb75ba23693ce4cedc303b7a329d25f8420a6c1d01ab855"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/footwear-demand-forecasting"
published_at: "2026-06-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:90412706d23499c3e72f5ab5b713f907a9e7a22ce8ab5ca5202d30c918d45022"
---

# Demand forecasting in Footwear: the attribute layer your models are missing

A planner opens a new sneaker style with zero sales history and needs a launch forecast by Friday. The model reaches for the nearest attribute-similar items to borrow a demand curve, and comes up with a folder full of half-tagged records: upper material logged as "mesh/synth combo," cushioning tech left blank, closure type buried in a free-text description nobody standardized. The forecast still runs. It just runs on noise wearing the shape of data.


That is the quiet failure mode in footwear demand planning. Nobody debates whether attributes matter to a forecast — everyone nods at that in the planning meeting. The problem is that almost no one audits whether the attributes actually powering the model are clean enough to trust, and by the time a forecast is visibly wrong, the root cause is three systems removed from the person who gets asked to explain it.


## A forecast is an aggregation. Attributes are what it aggregates along.


Every demand forecast in footwear rolls up along some hierarchy: category to style to color to size. Planners then re-slice that same data by attribute — silhouette, closure, material, cushioning platform — to build assortment plans, allocate by climate zone, and set markdown cadence. Those attribute fields are the join keys. If "upper material" contains five different spellings of "knit" across five source systems, the rollup that's supposed to compare knit performance against leather performance is actually comparing three overlapping, mislabeled buckets. The model doesn't throw an error. It just produces a number that's wrong in a way nobody can trace.


This is also exactly the mechanism McKinsey points to when it pushes apparel and footwear teams toward more disciplined, data-driven size-curve and assortment analytics: the payoff shows up when the underlying product data is granular and consistent enough to segment on, not just when a bigger model gets bolted on ([McKinsey](https://www.mckinsey.com/industries/retail/our-insights/geek-meets-chic-four-actions-to-jump-start-advanced-analytics-in-apparel) ).


## Like-item matching is where thin attributes cost the most


Footwear runs on newness. Seasonal drops, collabs, and constant style churn mean a huge share of any given season's forecast is for SKUs with no sales history at all. The standard fix is like-item matching: find historically similar styles, borrow their demand curve, adjust for price and season. Vendors from o9 to smaller planning tools now build entire modules around this kind of size-curve and cluster analysis specifically because manual "pick a comparable style" judgment doesn't scale to modern assortment velocity ([o9 Solutions](https://o9solutions.com/articles/advanced-size-curve-analysis) ).


Like-item matching is only as good as the attributes it matches on. If two running shoes share a midsole compound and a drop height but the tech pack calls one "responsive foam" and the other "React cushioning" with no shared taxonomy behind either term, the matching engine treats them as unrelated. The new style either gets forecast against a generic category average — usually a flatter, less useful curve — or gets matched to something with a genuinely different sell-through pattern. Either way, the buy quantity coming out the other end is off before a single unit ships.


## Three attributes that quietly bend the curve


A few footwear-specific attributes do outsized damage when they're thin, inconsistent, or free-text:


**Upper material and construction.** Knit, mesh, full-grain leather, suede, synthetic overlay — these aren't cosmetic. They drive climate suitability, durability perception, and price tier, all of which shape seasonality. A style tagged only "textile" collapses distinctions a forecast needs to separate a breathable summer trainer from a lined fall silhouette.


**Cushioning and midsole platform.** Proprietary foam names change every season and rarely get mapped back to a consistent underlying category (EVA, TPU, foam-injection, air-based). Without that normalization, a forecast can't tell whether last year's strong sell-through was about the silhouette or the cushioning platform underneath it, which matters enormously when a brand reuses a cushioning tech across a new upper.


**Width and last shape.** This is the one planning teams underrate most. Return-rate research shows footwear returns run far higher than the rest of apparel, with one industry benchmark putting single-brand DTC returns around a median 19 percent and multi-brand retail returns higher still ([Eightx](https://eightx.co/blog/average-footwear-return-rate-benchmarks) ). The same research traces most of that back to fit, not preference, and specifically to width: reviews cited in that analysis found a majority of wearers are in shoes too narrow, not too long, even though most size charts and most PIM records only capture length. When width and last-shape data don't exist as structured attributes, a forecasting model can't distinguish "this style oversells because demand is high" from "this style oversells because everyone orders two widths and returns one" — and bracketing behavior gets booked as real demand.


## Where free text turns into a false signal


Attribute (as stored) What the model sees What it should see


"mesh/synth upper, breathable" one unstructured string upper_material: mesh; overlay_material: synthetic; breathability: high


"cushioning: proprietary foam" unmapped brand term midsole_type: EVA-injected; stack_height: 28mm


"true to size, some run narrow" a review snippet, unindexed width_profile: narrow-fit; fit_notes flagged for QA


None of this requires a new forecasting engine. It requires the attribute layer underneath the engine to be complete, consistently coded, and validated against real source signals, not left as whatever free text a merchandiser typed into a spec sheet three seasons ago.


## The planning cost of getting this wrong


Thin attributes don't just blur one forecast. They compound: a bad like-item match skews an initial buy, the resulting markdown or stockout gets logged as "actual demand," and that mislabeled outcome becomes training data for the next season's model. Footwear's combination of high newness ratio and unusually high, fit-driven return rates makes this compounding effect sharper than in most other apparel categories — every extra six months a style attribute sits mistagged is another cycle where the forecast is quietly learning the wrong lesson.


None of this is an argument for a smarter algorithm. It's an argument for cleaner inputs. Anglera sits on top of whatever PIM, ERP, or flat file already holds a footwear catalog and does the unglamorous work of extracting, normalizing, and validating the attributes underneath it — upper material, cushioning platform, width, closure, and the dozens of other fields that forecasting, allocation, and assortment tools depend on — flagging conflicts instead of silently guessing. The forecasting model stays whatever a planning team already runs. What changes is whether the dimensions it aggregates along are actually true.
