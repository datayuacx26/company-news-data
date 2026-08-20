---
schema_version: "1.0.0"
document_id: "168009105d7ff4604ea7139d2a0203560ddd64b25b902d3ce9fd644d26212ba5"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/product-attributes-demand-forecasting"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:08.422826+00:00"
content_hash: "sha256:3ff46c2b69b60e5a4baf43d372426711ac56e922bd7e6c71f8616e08030ddeb9"
---

# Your demand forecast is only as good as your attribute data

Ask a demand planner how confident they are in this quarter's forecast and they'll cite a MAPE or a bias number. Ask them how confident they are in the attribute data the forecast is built on, and the answer gets vaguer fast. That gap is the subject of this piece, because it's usually the real gap between a forecast that's directionally right and one that's wrong in a way nobody notices until the PO is already placed.


## A forecast is a rollup, not a number


Every demand forecast is an aggregation. Sell-through for "women's dresses" is a sum of sell-through for individual SKUs, sliced along whatever dimensions the planning system can see: category, price band, season, and, in the better systems,[product attributes](https://www.anglera.com/glossary/product-attributes) like sleeve length, fabric weight, heel height, or pack size. The forecast isn't smart because the algorithm is smart. It's useful because the dimensions it aggregates along are real, clean, and consistent across every SKU in the rollup.


That's the part that quietly fails. If "sleeve length" is populated for 40% of dresses and free-text ("3/4," "3/4 sleeve," "threequarter") for the rest, you don't have a sleeve-length dimension. You have a column that looks like one in a dashboard demo and behaves like noise in production. The planner sees a forecast by sleeve length; the model sees a coin flip.


## "Our data is fine" usually means "our hierarchy is fine"


Most retailers and brands have a workable merchandise hierarchy: department, class, subclass are usually solid, because buying and finance depend on them and someone owns that structure. Attribution below that line is a different story. Color, material, fit, closure type, sole type, and dozens of category-specific specs get entered by whoever touched the product last, at whatever level of care that day allowed. A[2025 review of new-product demand forecasting in fashion](https://onlinelibrary.wiley.com/doi/full/10.1002/for.3192) notes that forecasting accuracy for new items depends heavily on the completeness and consistency of the attribute data feeding the model, not just the modeling technique itself.


So when a planning leader says "our data is fine," they usually mean the hierarchy is fine. Attribution is thin, inconsistent, and rarely audited, because nobody downstream complains loudly enough about a missing spec field the way finance complains about a missing revenue number.


## Where planning teams actually touch attributes


Three places, mostly.


**Rollups.** Demand by color, by fabric, by fit, by pack configuration. This is how a planner spots that mid-rise is outperforming high-rise three weeks into a season, or that a fastener change quietly killed a category. If the attribute is empty or inconsistent, that cut simply doesn't exist. It doesn't show an error; it shows a flat, uninformative line.


**Cohort and assortment building.** Planners group items into comparable sets to plan against each other, not against the whole catalog. A cohort of "puffer jackets, mid-weight fill, drop shoulder" is only as good as the attributes used to build it. Build it on a bad hierarchy alone and you're comparing a $60 vest to a $400 down parka because both sit in "outerwear."


**Like-item matching for new products.** This is the sharpest edge case, because it's where attribute data quality shows up as a dollar number fast. New SKUs have no sales history, so planning systems find a "like item" or a cluster of similar items and borrow their demand curve as a starting forecast. That match runs entirely on attributes. Patent filings around retail planning software describe exactly this mechanism: identifying items with matching attribute values and treating the number of non-matching attributes as a distance metric for how much to trust the analog. If two products should match on fabric weight, cut, and closure but the fabric weight field is blank on one and "poly" versus "polyester" on the other, the system either fails to find a match or finds the wrong one, and the new item launches against a demand curve that has nothing to do with it.


It's worth being precise about what attribute-based forecasting can and can't do here. Some retail platforms report meaningful accuracy gains from attribute-similarity forecasting for new items, but academic work on the problem, including a widely cited[arXiv study on forecasting demand for new fashion items](https://ar5iv.labs.arxiv.org/html/1907.01960) , found that naive attribute clustering alone doesn't reliably explain sell-through: items launched at the same time clustered together regardless of attributes, and the strongest models combined attribute data with merchandising context like discounting and visibility. The takeaway isn't that attributes don't matter. It's that they're necessary input, not a magic key, and they have to be clean structured fields, not free text, before any model can use them as a feature at all.


## What breaks quietly, and what a fix looks like


The failure mode is rarely a crash. It's a silent substitution: missing values get treated as zero or as "not applicable," near-duplicate free-text values get bucketed separately when they should be one value, and conflicting values across ERP, PIM, and spec sheet get resolved by whichever system loaded last.


Attribute state What planning sees What it should see


Sleeve length: blank on 60% of SKUs Flat, uninformative rollup by sleeve length Full rollup across all SKUs


Fabric: "poly," "polyester," "100% polyester" Three separate demand cohorts One clean cohort


Heel height: free-text "approx 3in" Excluded from numeric filtering entirely Numeric field usable in a model


Fit: conflicting between ERP and tech pack Silently overwritten by most recent load Flagged for review, source retained


One[industry analysis of product attribute strategy](https://www.jestais.com/the-strategic-value-of-product-attributes-in-modern-retail/) puts it plainly: attributes only deliver value when the data is complete, standardized, and governed, and recommends tracking something like an attribute completeness score by category rather than assuming attribution is fine because the hierarchy is.


A planning-grade attribute layer treats every attribute like a first-class dimension, not an e-commerce filter that's nice to have. That means extracting values from the sources that actually contain them (tech packs, BOMs, imagery, spec sheets, even reviews), validating them against a[controlled vocabulary](https://www.anglera.com/glossary/controlled-vocabulary) , flagging conflicts across systems instead of silently overwriting them, and back-filling gaps across the full catalog history so a new attribute isn't just available going forward.


That's the layer Anglera builds. Your PIM, ERP, or planning system stays the system of record; Anglera continuously extracts, normalizes, and quality-scores the attribute data underneath it, so the rollups, cohorts, and like-item matches your planning team runs every week are built on dimensions that are actually real.
