---
schema_version: "1.0.0"
document_id: "be59b02f227c6f2a82f234ac6ab6d32ca1e58e1d43304b0fd05b60317f1a1b0d"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/demand-planning-attribute-schema"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:d342354496858a3be39e38d5c9b3cc1438df276baa3d0e335bddb40cc3f7b736"
---

# The attribute schema demand planners actually need (it's not the e-comm facet list)

A planner pulls a rollup by color family to spot the trend before the buy meeting closes. Half the SKUs come back under "Multi," a third show whatever string the vendor typed into a free-text field last season, and the rest are split across "Navy," "Deep Navy," and "Ink" as if they were three different colors. The chart is technically correct and practically useless. Nobody did anything wrong, exactly. The catalog just wasn't built for this question.


That's the quiet mismatch behind a lot of bad forecasts: the attribute schema retailers have is the one built for the website, and the schema demand planning needs is a different animal wearing the same clothes.


## Facets and planning attributes overlap, but they're not the same job


E-commerce facets exist to help a shopper narrow a list fast. They're optimized for click-through: broad enough to feel intuitive, forgiving enough to tolerate some overlap, and free to change every season if it makes the site feel fresher. A facet that returns 40 products when a shopper expects 12 is a minor annoyance. Nobody re-runs last year's numbers against it.


A planning attribute exists to be aggregated, compared across periods, and fed into a model. It has to partition the assortment cleanly, hold its meaning across seasons, and never silently drop or double-count a SKU. A facet that's "close enough" is fine for merchandising a site. An attribute that's close enough will corrupt every rollup built on top of it, because a forecast is nothing more than an aggregation, and the attribute is the dimension it aggregates along. Get the dimension wrong and the number underneath it is wrong too, no matter how good the forecasting model is.


Retail taxonomy work already has language for the underlying problem. In[product taxonomy](https://www.anglera.com/glossary/product-taxonomy) design,[mutual exclusivity](https://www.earley.com/insights/product-taxonomy-mutual-exclusivity-one-product-one-place) means every item has exactly one home in the structure — not because that's tidy, but because "[data governance](https://www.anglera.com/glossary/data-governance) " and "partner integration" both depend on a SKU never showing up in two buckets that get summed separately. Facets are allowed to overlap. Planning categories can't.


## What "planning-grade" actually requires


Four properties separate a planning attribute from a facet, and a schema missing any one of them will produce numbers a planner has learned not to trust.


**Discrete and mutually exclusive.** Every value in a planning pick list has to partition the line with no ambiguity and no gaps — one SKU, one bucket, every time. "Multi" as an escape hatch for anything with two colors isn't a value, it's a hole in the taxonomy. If 8 percent of the assortment lands in the miscellaneous bucket, 8 percent of every rollup by that dimension is wrong by construction.


**Stable across seasons.** A facet can be renamed to chase a seasonal trend word. A planning attribute can't, or the year-over-year comparison breaks the moment the label changes even though the underlying product didn't. If "Athleisure" becomes "Performance Casual" between fall and spring resets, the trend line for that segment doesn't dip and recover, it just vanishes and reappears under a different name, and whoever's building the buy has to manually reconcile two years of history by hand.


**Granularity chosen for the analysis, not the shelf.** This is where teams most often pick one option when they need both. A single "color" field with 400 unique values is too fine to spot a trend; a single "color family" field with 12 values is too coarse to actually place a reorder. Planners need color family for the trend read and specific shade for the buy — as a hierarchy, not a replacement of one for the other.[Retail hierarchy guidance](https://www.toolio.com/post/optimizing-product-hierarchy-and-attributes-for-effective-retail-decision-making) makes the same point about attributes generally: detail matters for some decisions, an aggregated view matters for others, and a mature schema carries both levels rather than forcing a choice.


**Coverage of dimensions the storefront never shows.** Construction method, component and material composition, factory or sourcing region, lead time tier, compliance or duty classification — none of these appear as a customer facet, and all of them drive real planning and buying decisions: which styles can flex quickly on reorder, which share a supply constraint, which carry a landed-cost risk the model needs to know about. A schema copied straight from the PDP facet list simply doesn't have a field for any of it.


## Two views, one source of truth


Field E-comm facet (what shoppers see) Planning attribute (what planners need)


Color "Multi," broad marketing names, can shift by season Specific shade code plus a fixed color-family layer above it


Material One tag, often just the dominant fiber Full composition with percentages, construction method


Fit Marketing labels ("Relaxed," "Athletic") Stable fit code mapped consistently across styles and years


Category SEO-friendly, can be re-labeled for trend language Fixed hierarchy node, unchanged season to season


Sourcing Not shown Factory, region, lead-time tier


The fix isn't to replace the facet list with the planning list, or vice versa. It's to design them as two views over one governed attribute layer: the facet is a curated, human-friendly projection for the shopper; the planning attribute is the underlying, MECE, seasonally stable value that the facet gets derived from. When color family and shade both live on the product record, the facet can show "Navy" while the planning rollup still has the shade-level detail sitting one layer down, ready when someone needs it.


Building that layer means going back to the sources that actually carry this information — tech packs, BOMs, spec sheets, imagery, even reviews — because most of it was never captured as clean structured data in the first place. It was written into a PDF, buried in a legacy ERP free-text field, or never recorded at all.


## Building the planning view alongside the digital one


A workable approach starts small. Pick the three or four attributes that actually drive the current planning cycle — for most lines, that's category, color family plus shade, material, and one internal dimension like construction or sourcing region. Define each as a closed pick list, not a free-text field. Map every existing value, including the legacy junk, into that list once, and lock the list for the season. Then let the customer-facing facet be generated from it, not maintained in parallel by a different team with different incentives.


None of this requires ripping out the PIM or the planning tool already in place. Your PIM stores the data; the work is in extracting the real values from the source documents and images, normalizing them into a schema built for aggregation, and keeping it validated as new product comes in — so the rollup a planner pulls on Monday reflects the assortment that actually exists, not whatever happened to get typed into a field last season.
