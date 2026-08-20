---
schema_version: "1.0.0"
document_id: "354f03777f6f59968d83964cb5e70279beecf89c0663c4831b73211588231a78"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/attribute-trust-hierarchy-conflicts"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:e4b276c5606e2802dd9eaafbfd2732134a98ddf6e75fb1d42b3218e81b3ace40"
---

# When the image says rubber and the copy says leather: resolving attribute conflicts

Pull the tech pack, the product copy, and a photo of the same SKU side by side and you will find disagreements more often than you'd expect. The copy says "genuine leather upper." The BOM lists a leather-and-synthetic composite. The hero image shows an obvious rubberized coating on the toe cap. All three describe the same style number, and most catalogs have no mechanism for deciding which one is right.


What actually happens at most retailers and brands is simpler and worse: whichever value got typed in last wins. A merchandiser updates PDP copy for SEO and overwrites the material field with a friendlier word. A vendor resubmits a corrected BOM, and the enrichment team never re-checks the copy against it. Nobody flags the mismatch, because nothing is looking for it.


## Why this is a planning problem, not just a content problem


E-commerce teams notice material conflicts when a shopper complains that a "leather" boot arrived with a synthetic panel, or when a filter search misses items because the field doesn't match the copy. That's a real cost, but it's the small one.


The bigger cost shows up where nobody is reading the PDP at all. Demand forecasts, assortment plans, and like-item comparisons run on the attribute field, not the description. If material is wrong for even a meaningful share of SKUs, every rollup by material silently absorbs that error: sell-through by fabrication, markdown cadence by material category, vendor quality signals, even the reference set used to forecast a new style by matching it to "similar" past items.[Poor data quality is frequently cited as the root cause of forecast error](https://www.onepint.ai/insights/what-causes-forecast-errors-in-demand-planning) , and misclassified attributes are a common way that shows up. As[one supply-chain analysis put it](https://itsupplychain.com/when-the-forecast-lies-how-data-quality-failures-break-supply-chain-decisions/) , a forecasting problem is often a data quality problem in a different costume — no model architecture fixes a material field that was never true.


## Silent conflicts are worse than visible gaps


A blank material field is annoying. It shows up in a completeness report, someone gets assigned to fill it, and the catalog is honest about what it doesn't know in the meantime.


A wrong material field is worse, because it looks finished. It passes every completeness check and feeds a forecast, a filter, and a vendor scorecard with total confidence, and nothing about the pipeline signals the value might be false. You only find out when a rollup looks strange, and by then the bad value has already been read by every downstream system that trusted it.


This is the case for treating conflict detection as its own step, separate from ordinary field completion. A missing attribute is a known unknown. A conflicting attribute, resolved by accident, is an unknown unknown wearing a checkmark.


## Detecting the conflict before resolving it


Conflict detection means cross-referencing the same attribute across every source that mentions it, for every SKU, on an ongoing basis rather than at initial load:


- **Copy vs. imagery.** Extract material, color, and pattern signals from product photography and compare them against what the copy claims. "Genuine leather" against a visibly synthetic texture is a flag, not a silent overwrite in either direction.
- **Copy vs. BOM or tech pack.** Bill-of-materials documents are the most granular and least polished source, built for production rather than marketing, and they often list composite materials that get simplified in customer-facing copy.
- **BOM vs. imagery.** Even structured documents go stale. A BOM revision shipped after the product photography can describe a change the image never reflects.
- **Legacy ERP fields vs. everything else.** Old category or material codes often persist in the system of record long after the product changed.


None of this requires a human to eyeball every SKU. It requires running the comparison at catalog scale, continuously, and keeping a record of where each value came from.


## Resolving conflicts with a trust hierarchy, not a coin flip


Detection tells you where sources disagree. Resolution requires deciding, in advance, which source wins for which attribute — a rule set defined once and revisited when it stops making sense.[Master data management](https://www.anglera.com/glossary/master-data-management) calls this a survivorship rule, and the discipline applies just as well to[product attributes](https://www.anglera.com/glossary/product-attributes) as to customer records:[survivorship rules should be built on trust, recency, and usage context for each attribute](https://profisee.com/blog/mdm-survivorship/) , not a single blanket policy for the whole record.


A reasonable default hierarchy for physical product attributes, most specific and production-grounded source first:


Attribute Typical trust order Why


Material composition BOM/tech pack over imagery over legacy copy BOM is the production document; copy is often simplified for marketing


Color Imagery over copy over legacy code Color names drift ("navy" vs. "midnight blue"); pixels don't


Dimensions Spec sheet over imagery over copy Copy rounds; spec sheets carry tolerances


Category/type Current PIM taxonomy over legacy ERP code Legacy codes lag taxonomy changes by years


The specific order matters less than having one, applied at the attribute level rather than the record level — a product can trust the BOM for material and the current PIM for category without those decisions conflicting with each other.


## Auto-resolve the confident cases, route the rest


Not every conflict deserves the same handling. A useful split:


**Auto-resolve** when the trust hierarchy gives a clear answer and confidence in the winning source is high — the BOM explicitly states "100% cowhide leather," the image confirms a leather grain texture, and the only outlier is stale marketing copy. Update the attribute, log the source and the override, and move on without a human in the loop.


**Route to review** when sources genuinely conflict at comparable confidence, or the trust hierarchy doesn't clearly apply — the BOM lists a blend without percentages, the image is ambiguous, and no source is decisively more current than another. These go to a person with the conflicting evidence attached, not a blank field and a guess.


This should stay conservative about what counts as confident. A forecast built on an attribute auto-resolved wrong is worse than one built on an attribute flagged as pending, because the flagged version doesn't pretend to be certain.


## What this requires operationally


None of this needs a system replacement. It's a layer that reads whatever imagery, tech packs, BOMs, and copy already sit in a PIM, ERP, or flat file export, cross-references them continuously, applies the trust hierarchy, and writes back the resolved value with a confidence score and a source citation. Conflicts that clear the bar get auto-resolved; the rest surface in a queue instead of getting buried in a field that looks complete.


The output is an attribute layer that a demand forecast or a like-item model can actually trust, because every value carries a record of where it came from and how sure the system is that it's right. That distinction, between populated and verified, is the difference between a catalog that looks done and one that's usable by the systems making buying and allocation decisions downstream.


Anglera runs this conflict detection as an ongoing layer on top of whatever PIM, ERP, or flat file already holds the catalog — extracting attribute values from imagery, documents, and copy, flagging where they disagree, applying a trust hierarchy your team defines once, and routing only the genuinely ambiguous cases for review. Your PIM still stores the data; Anglera keeps it honest about what it actually knows.
