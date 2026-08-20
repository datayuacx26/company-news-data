---
schema_version: "1.0.0"
document_id: "6d7d0b0246207044742600b5bdee79da129ef7af859e46733f65565beb4c1076"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/ml-demand-planning-data-foundation"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:87ca1ff094f08cbff95c060e01dfa9b8ce87e39685a4379a08bff320d8fa2257"
---

# AI demand planning is coming. Your attribute layer decides if it works.

Every major planning vendor now has an AI forecasting pitch. Toolio, Blue Yonder, o9, Anaplan, Impact Analytics — the roadmap decks all look similar: gradient-boosted models, external signal ingestion, agentic recommendations that flag exceptions instead of just charting a demand curve.[Gartner projects that 70 percent of large organizations will adopt AI-based supply chain forecasting by 2030](https://www.gartner.com/en/newsroom/press-releases/2025-09-16-gartner-predicts-70-percent-of-large-orgs-will-adopt-ai-based-supply-chain-forecasting-to-predict-future-demand-by-2030) , and McKinsey has found that AI-driven forecasting can cut errors 20 to 50 percent versus traditional statistical methods, with knock-on reductions in stockouts of up to 65 percent. Those are real numbers, from real deployments.


They are also, quietly, the ceiling for a specific kind of retailer or brand — the one whose item master is clean, consistent, and rich enough for a model to actually use. For everyone else, the same algorithm produces a forecast that is confidently wrong.


## The model isn't the bottleneck anymore


This is the part planning vendors don't lead with, because it isn't their problem to fix. Random forests, gradient boosting, even the newer foundation-model approaches to time series are mature technology at this point. The gap between a good demand forecast and a mediocre one increasingly comes down to what the model is allowed to see, not how the model is built.


A forecast is not really a prediction about a single SKU. It's an aggregation — sales history rolled up by category, by fabric, by silhouette, by price band, by pack size, by whatever dimension the business plans against. Every one of those rollups depends on an attribute being populated correctly on every row. If "sleeve length" is free text on a chunk of an apparel brand's catalog and a controlled value on the rest, the model either drops that feature or learns from a corrupted version of it. If "material" says "COTTON," "100% Cotton," and "ctn" for the same fiber content across three merchandisers' data entry habits, the model sees three categories where there should be one, and the size of each becomes too thin to learn anything from.


This is the classic feature-engineering problem in machine learning, and demand planning is not exempt from it.[Practitioner analysis of production forecasting systems](https://appifyintelligence.com/blog/forecasting-demand-ai-2026) has found that once classical models get comprehensive feature engineering — point-of-sale joined to promotions, weather, foot traffic, and competitor signals — they perform comparably to newer foundation-model approaches. The delta shows up in how well the features are built, not which algorithm consumes them. Garbage attributes in, garbage forecast out, no matter how modern the model.


## Where attribute rot actually breaks a forecast


It rarely fails loudly. A forecasting engine doesn't throw an error when it gets a null value or an inconsistent category — it just produces a number, and the number is wrong in ways that are hard to trace back to a root cause. A few concrete failure modes:


- **New-item cold start.** A model forecasting a new SKU leans on attribute similarity to existing SKUs with sale history — same category, same fabric, same price point. If the new item's attributes are missing or mis-tagged at launch, the model has no comparable to anchor to, and it either defaults to a category average or guesses badly.
- **Like-item substitution.** Planners routinely borrow demand curves from a discontinued or similar item to seed a new one. That match is only as good as the attributes used to find it. A missing or wrong attribute silently pulls in the wrong analog.
- **Cannibalization and halo effects.** Detecting that a new color variant steals share from an existing one, or that a bundle lifts an unrelated SKU, requires attributes precise enough to group "the same product" correctly across variants. Loose or inconsistent variant tagging blurs the boundary the model needs to see.
- **Assortment rollups.** Every plan built at category or subcategory level inherits whatever mapping error exists in the[product taxonomy](https://www.anglera.com/glossary/product-taxonomy) underneath it. One mis-tagged attribute doesn't just skew one SKU's forecast — it skews the aggregate everything else is measured against.


None of these show up as a broken dashboard. They show up as a forecast that's meaningfully off, in a way nobody can explain, until someone manually audits the item master months later.


## What a training-ready attribute layer looks like


Being "training-ready" isn't about having more data. It's about having attributes that are complete, consistent, and validated — not just present in a field somewhere.


Signal Not training-ready Training-ready


Material/fiber content Free text, inconsistent casing, abbreviations Controlled vocabulary, normalized values


New-item attributes Backfilled weeks after launch, if at all Populated before the item ships


Variant relationships Inferred loosely from SKU naming Explicit parent-child and attribute lineage


Conflicting source values Silently overwritten by the last system to write Flagged, reconciled, source-tracked


Attribute coverage Spot-checked on flagship SKUs only Consistent depth across the full catalog


Getting there usually means treating attribute enrichment as infrastructure that sits between raw source systems and every downstream consumer — planning tools included — rather than a one-time cleanup project. In modern data architecture terms, this is the same logic behind a medallion lakehouse: raw data lands, gets refined and validated, then gets served to whatever consumes it downstream.


A word of caution here, because this space attracts overpromising: AI planning tools are not going to intuit demand for a product they can't describe. They're pattern-matching systems, and the patterns they find are bounded by the features they're fed. No model available today reliably predicts demand for a genuinely novel item with thin or absent attribute data, and treating any vendor's roadmap as a substitute for attribute discipline is a mistake retailers will pay for later.


Anglera's job is the layer underneath that decision, not the forecasting layer itself. Your PIM, ERP, or planning system stores the data — Anglera extracts, normalizes, validates, and gap-fills the attributes underneath it from the source documents, images, and specs that already exist, so that whichever forecasting engine a team adopts is working from a catalog it can actually learn from. The vendors racing to ship AI forecasting are betting on models. The retailers who win with those models will be the ones who spent the last year making sure their attributes were worth training on.
