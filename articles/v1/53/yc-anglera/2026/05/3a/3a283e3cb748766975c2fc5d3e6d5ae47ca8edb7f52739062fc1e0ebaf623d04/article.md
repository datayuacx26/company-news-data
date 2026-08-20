---
schema_version: "1.0.0"
document_id: "3a283e3cb748766975c2fc5d3e6d5ae47ca8edb7f52739062fc1e0ebaf623d04"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/data-hub-silver-layer-product-attributes"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:b16334ae29e52dbede3967a2f908bd53b0954d3a81327e6ef636a81375a5324c"
---

# Product attributes in the lakehouse: cleaning the silver layer for real

Most data teams can recite the medallion pattern in their sleep: bronze holds raw data exactly as it landed, silver cleans and conforms it, gold aggregates it into something a dashboard or model can consume.[Databricks describes silver as the layer for "data cleansing, deduplication, and normalization," with schema enforcement and null handling done before anything moves upstream](https://docs.databricks.com/aws/en/lakehouse/medallion) . It is a good pattern. It is also, for product data, incomplete in a way most pipelines never notice until a forecast blows up.


Here is the gap. "Clean" in a typical silver layer means: cast the types, drop the nulls, dedupe on primary key, standardize date formats, enforce a schema. That is real work and it matters. But none of it touches the actual content of a product attribute field. A silver table can be perfectly deduped, perfectly typed, and still contain a` material` column that reads` 100% cott` ,` COTTON` ,` cotton (organic)` , and` Cotton/Poly Blend` for four rows that should all roll up into one planning bucket. Type-casting does not fix that. Deduplication does not fix that. The field is technically clean and semantically garbage.


## Why this matters more for planning than for BI


A sales dashboard can survive some sloppiness in attribute values. A human looks at the chart, mentally normalizes "cott" to "cotton," and moves on. A forecasting model or an assortment-planning rollup cannot do that. It aggregates literally. If four material spellings exist in silver, gold either treats them as four different categories (fragmenting the sample size a category-level model needs) or someone downstream writes a` CASE WHEN` statement to patch it in the BI layer, which is exactly the kind of shadow logic that model governance is supposed to prevent.


This is the same failure mode data quality practitioners have flagged for years under the "garbage in, garbage out" banner:[automation doesn't correct errors, it amplifies them](https://parseur.com/blog/gigo) , and forecasting is automation.[Clean data has always been described as vital to demand forecasting accuracy](https://www.unioncrate.com/resources/importance-of-clean-data-for-demand-planning-accuracy) because outdated or incomplete inputs produce unreliable outputs — but "clean" in that context has to mean clean values, not just clean schema. A demand plan is an aggregation. Attributes are the dimensions it aggregates along. If the dimension values are inconsistent, every rollup built on them inherits the inconsistency silently, with no error thrown anywhere in the pipeline.


## What silver-layer cleaning actually requires for attributes


Standard silver-layer transformations (per the Databricks reference architecture) are cleanse, validate, deduplicate, and lightly enrich. For[product attributes](https://www.anglera.com/glossary/product-attributes) , each of those steps needs a different definition than the one written for transactional or event data:


Standard silver step What it does for order/event data What it has to do for product attributes


Cleanse Trim whitespace, fix encoding Extract structured values out of free text (tech pack notes, spec PDFs, legacy ERP free-text fields)


Normalize Standardize date/currency formats Map every raw value to a controlled pick list (` cott` ,` 100% cotton` ,` ctn` all become` Cotton` )


Validate Check foreign keys, non-null constraints Check the value against the source of truth — does the imagery actually show a zipper closure if the field says "zip"?


Deduplicate Collapse duplicate transaction rows Resolve conflicting attribute values across source systems and flag rather than silently overwrite


That middle-right cell is the one most teams skip entirely, because it requires actually looking at the tech pack, the spec sheet, or the product photo, not just the row. It is extraction and normalization against a[controlled vocabulary](https://www.anglera.com/glossary/controlled-vocabulary) , and increasingly it means validating a text field against imagery — confirming a stated attribute matches what a photo or document actually shows, and flagging the mismatch instead of trusting whichever source loaded last.


## Why "just add validation rules" undersells the problem


It is tempting to think dbt tests or a Great Expectations suite closes this gap. They do not, because they validate structure, not content. A` not_null` test passes on` material = "cott"` just as happily as it passes on` material = "Cotton"` . A regex check can catch obviously malformed strings, but it cannot tell you that "water resistant" and "waterproof" are being used interchangeably by two different suppliers to mean different IP ratings, or that a sole material field was copy-pasted from last season's near-identical style. Those are semantic errors, and semantic errors are exactly what corrupt like-item matching, substitution logic, and any model feature built on attribute rollups.


This is also why entity resolution and MDM approaches, which are well understood for matching customer records or supplier IDs to a canonical key, don't fully solve it either. Assigning a canonical product key does not tell you whether the attributes attached to that key are correct. You can have a perfectly resolved, deduplicated product master where half the color values are still wrong, because nobody checked them against anything.


## What this actually looks like as a silver-layer step


Treat attribute enrichment as its own conformance stage, sitting inside silver, before anything reaches gold:


- Extract candidate values from unstructured sources feeding bronze: tech packs, BOMs, spec sheets, product imagery, even review text where customers describe a fit or material issue the catalog got wrong.
- Normalize every extracted value against a governed pick list per attribute, not a free-text field, so "cott," "ctn," and "100% cotton" become one value before a model ever sees them.
- Validate contested values against a second source (imagery against spec sheet, spec sheet against tech pack) and flag conflicts for review rather than silently picking one. A quality score per attribute, not just a filled/empty flag, is what lets downstream consumers decide how much to trust a value.
- Backfill gaps at the attribute level across the whole catalog in one pass, rather than waiting for the next PLM refresh cycle, so a newly required planning dimension (say, a closure type or a fabric-weight band) doesn't sit null for two quarters.


None of this replaces schema enforcement or deduplication. It runs alongside them, because a perfectly typed, perfectly deduplicated field with the wrong value in it is still the wrong value, and it will still be wrong every time it gets aggregated.


This is the layer Anglera is built for. Your PIM, MDM, or lakehouse stores the record; Anglera does the extraction, normalization, and validation work that turns a raw attribute field into something a forecast can actually trust, working from whatever source system or flat export you already have, without replacing the pipeline underneath it. Planning tools are only as good as the item data feeding them. Fixing that data where it lives, in the layer built to hold "clean," is the step most medallion pipelines still skip.
