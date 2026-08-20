---
schema_version: "1.0.0"
document_id: "d50bb435fddb8d6017d41cd4a263a8f08db3d08bde2d528fffca3b23bc04310d"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/attribute-consolidation-splintered-schema"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:49c2a394cd6d28b9374f3c7aa3da0b72e226c2ee36df777755cd004bf666a0f9"
---

# Too many attributes, too little signal: consolidating a splintered schema

Pull the attribute list for almost any catalog that has survived three or four years of category launches, and you will find` color` ,` color_desc` ,` primary_color` , and` colour` sitting side by side, each populated for a different slice of SKUs. Nobody set out to build that. It happened one migration, one category launch, and one well-meaning analyst at a time. The failure mode gets less attention than missing data, but it does just as much damage to a forecast: the signal isn't absent, it's scattered across five weak columns instead of concentrated in one strong one.


## The quiet cost of a splintered schema


Missing attributes are easy to spot. A blank field shows up in a completeness report and someone gets assigned to fill it. Splintered attributes are harder to catch because, from a distance, the data looks fine. Every SKU has a color value somewhere. The problem only surfaces when someone tries to roll up sell-through by color across the whole assortment and discovers that a third of the catalog is tagged in a field the report doesn't query.


Here's the mechanism worth sitting with: a demand forecast, an assortment plan, or a like-item substitution model all work by aggregating history along dimensions, color, material, fit, pack size, whatever the category calls for. If the values that describe one true concept are split across` color` ,` primary_color` , and a free-text` notes` field where someone typed "mostly black w/ white trim," the model sees three thin, inconsistent signals instead of one strong one. Sample sizes per bucket shrink, variance goes up, and the attribute effectively stops predicting anything. The data isn't wrong, exactly. It's diluted past the point of usefulness.


Terminology drift alone can break the joins that forecasting depends on. SAS has flagged this with a blunt example: a demand model that treats "Head and Shoulders" and "Head&Shoulders" as two different products, silently splitting that item's history in half before the analysis ever starts (see[SAS's writeup on data quality in demand forecasting](https://blogs.sas.com/content/hiddeninsights/2022/03/29/data-quality-in-demand-forecasting/) ). A near-duplicate attribute column does the same thing to a rollup: the aggregation logic simply never looks in the second place the value is hiding.


## How schemas end up this way


Nobody designs a splintered schema on purpose. It accretes: a new category's buyer imports a spreadsheet with its own naming convention, so` finish` becomes` surface_finish` just for that category. A vendor feed arrives pre-mapped to the vendor's own field names, and rather than reconcile it, someone adds it as a parallel attribute to hit a launch date. A field gets renamed for a new system, but the old name is never retired, so both exist, one live and one fossilized. A free-text notes field becomes an informal second attribute because the governed pick list didn't have the right value yet, so people typed around it instead of requesting an addition.


Each decision was locally reasonable. The aggregate result is a schema where the same concept has three or four names, none of them complete, and a planning team that has to know all four before it can trust a rollup.


## Auditing for overlap


The audit is mostly pattern-matching against the schema itself, before you touch a single record.


Start by listing every attribute name across every category and clustering them by string similarity and by co-occurrence: fields almost never populated on the same SKU are often the same concept, split by category or by era. Then check population rates and value overlap. If` color_desc` and` primary_color` share 80 percent of their populated SKUs with the same value in both, they're very likely one attribute wearing two names. A field populated on a handful of SKUs, untouched by any enrichment or import process in a year, is a candidate for retirement regardless of whether it overlaps with anything.


It helps to score each attribute name on a few dimensions before deciding what to do with it:


Signal What it tells you Typical action


High population, one category only Legitimate category-specific attribute Keep, scope it explicitly


High population, overlaps another field's values Duplicate under a different name Merge into one survivor


Low population, no recent writes Dead field from a past migration Retire


Free-text field shadowing a governed attribute Workaround for a missing pick-list value Merge values in, expand the pick list, retire the free-text field


Populated inconsistently across regions or systems of record Naming or mapping drift across integrations Reconcile mapping, keep one canonical name


## Merge, retire, or keep


Once an attribute is flagged as a likely duplicate, the decision is less about the name and more about which values are trustworthy. Pick the survivor based on population rate, source reliability, and how recently it's been validated, not on which system happens to own it politically. Then map every other variant's values into the survivor's schema, including reconciling free-text entries into the governed pick list.


This is where extraction from source documents earns its keep. Values scattered across a legacy` notes` field, a vendor tech pack, or an old ERP export can be parsed and mapped to the governed list rather than manually retyped SKU by SKU, which is the part of consolidation that usually stalls a schema cleanup for months. Conflicting values between the surviving attribute and the ones being merged in should be flagged for review, not silently overwritten. A dead field untouched for a year can usually just be archived, not migrated.


## Backfilling the survivor consistently


Merging the schema only pays off if the survivor attribute is then filled out consistently across the SKUs that used to rely on the retired fields. That means the same source hierarchy, the same validation rules, and the same confidence scoring applied catalog-wide, not category by category. A backfill that leaves the merged attribute at 60 percent population in footwear and 95 percent in apparel just relocates the dilution problem instead of fixing it.


## Governing so it doesn't happen again


Consolidation without governance is a one-time cleanup that starts drifting again the day after it ships. The fix is process, not a tool: a documented attribute dictionary naming the one canonical field for each concept, an approval step before anyone adds a new attribute (especially one that sounds suspiciously like an existing one), and a recurring review of population rates so dead fields get flagged before they've sat ignored for two years. One PIM-implementation retrospective put it plainly: governance has to be a business process, not a feature that ships with the software, because "the PIM will fix it later" consistently fails as a plan (see[McFadyen Digital on product data governance](https://mcfadyen.com/articles/your-pim-wont-fix-your-product-data-thats-the-work-you-have-to-do-first) ). The same discipline applies whether the schema lives in a PIM, an MDM, or a flat file everyone edits by hand.


A splintered schema and a sparse one cause the same failure downstream: a planning system that can't tell one product from another along the dimension that actually predicts demand. Anglera doesn't replace whatever system of record holds your schema. It sits alongside it, extracting values from the source documents and images that fed each of those splintered fields in the first place, mapping them to one governed attribute, flagging what conflicts instead of guessing, and keeping the survivor populated as new SKUs and new categories arrive. Your PIM stores the field. Anglera does the work of making sure there's only one that matters.
