---
schema_version: "1.0.0"
document_id: "d5f4f19f68ecc93673c9872f58509405696ff341ac110e68a03cf0cb4aaf74c7"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/skincare-attributes"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:43.834196+00:00"
content_hash: "sha256:851bdf8b25298072d60a095e4c44c846af503977b7e993f6ccec2ece3e46a473"
---

# Building an attribute schema for Skincare that shoppers and AI can actually use

Skincare is the category where "just add more attributes" actually means something specific. A serum isn't just a serum: it's a skin type, a concern, a list of actives at particular concentrations, a texture, and a set of exclusions a shopper is actively filtering for. When those fields are blank, the product doesn't rank lower in search — it disappears from the facet entirely, and it never gets mentioned when someone asks an AI assistant what to buy.


## The attributes that actually matter in skincare


Generic PDP templates give you brand, size, and price. Skincare shoppers filter on none of those first. They filter on:


- **Skin type** : dry, oily, combination, normal, sensitive
- **Skin concern** : acne, hyperpigmentation/dark spots, fine lines and wrinkles, redness, dullness, uneven texture, large pores, dehydration
- **Key active ingredients and concentration** : niacinamide (2%, 5%, 10%), hyaluronic acid, vitamin C (L-ascorbic acid vs. derivatives), retinol/retinaldehyde, bakuchiol, peptides, salicylic acid, azelaic acid, glycolic acid
- **Format/texture** : serum, gel-cream, lotion, oil, essence, ampoule
- **Exclusion flags** : fragrance-free, alcohol-free, oil-free, silicone-free, sulfate-free, paraben-free, non-comedogenic
- **Claims and certifications** : cruelty-free, vegan, dermatologist-tested, hypoallergenic, reef-safe (for SPF)
- **Usage context** : AM/PM, pregnancy-safe, suitable for sensitive/reactive skin, layering order


An academic study of ingredient-based product recommendation identified five core skin types, eleven skin concerns, and seventeen additional preference attributes (fragrance-free, cruelty-free, non-comedogenic, and similar exclusion flags) as the working vocabulary shoppers and recommendation systems actually use — see the[Beauty Beyond Words paper on explainable beauty recommendations](https://arxiv.org/html/2409.13628v1/) . That's the real shape of a skincare schema. Anything narrower and you're filtering on brand and price alone, which is not how anyone shops for a serum.


## Why a missing attribute is worse than a missing photo


A blank` skin_type` field doesn't get skipped in faceted search — the product gets excluded the moment a shopper clicks "sensitive skin" in the sidebar. Same with concern, same with "fragrance-free." Faceted navigation is a series of AND filters; a product with no value in a facet field fails every query that uses it, even if the product would have been a great match.


AI shopping assistants have the same failure mode, just less visible. ChatGPT, Gemini, and Perplexity lean on structured product data — schema.org` Product` /` Offer` markup, merchant feeds — to decide what to surface and how to describe it. Reporting on ChatGPT's shopping behavior found that a majority of cited product pages carry structured data, and that schema.org markup is treated as a baseline threshold: without it, an assistant either skips the source or uses it "fragmentarily," per[analysis from iPullRank on how OpenAI's product feed works](https://ipullrank.com/ecommerce-chatgpt-product-feeds) . Google's own guidance for merchant listing structured data lists the fields it expects on a product page, and skin type, concern, and active ingredient aren't part of the generic required set — brands have to add them explicitly, or an AI assistant summarizing "best serum for dry skin" has nothing to point to. See[Google's structured data guidance for products](https://developers.google.com/search/docs/appearance/structured-data/product) .


Ask an AI to recommend a niacinamide serum for oily, acne-prone skin under $30, and it will compose an answer from whichever products actually carry` active_ingredient` ,` concentration` ,` skin_type` , and` concern` values in machine-readable form. A serum with a beautiful product description and no structured attributes is invisible to that query, no matter how good the formula is.


## Before and after: a facial serum


Here's a raw retailer feed for a niacinamide serum versus what an enriched attribute set looks like.


Attribute Raw feed Enriched


Title "Brightening Serum 30ml" "Niacinamide 10% Brightening Serum, 30ml"


Skin type (blank) Oily, Combination, Normal


Skin concern (blank) Dullness, Dark Spots, Large Pores


Active ingredient (blank) Niacinamide 10%, Zinc PCA 1%


Format (blank) Serum


Fragrance (blank) Fragrance-free


Comedogenic rating (blank) Non-comedogenic


Usage (blank) AM/PM, apply before moisturizer


Claims "Cruelty-free" (in body copy only) Cruelty-free, Vegan (structured fields)


pH (blank) 5.5–6.0


The raw version reads fine to a human scanning the PDP. It fails every faceted filter and gives an AI assistant almost nothing to match against a shopper's stated skin type or concern. The enriched version turns that same product into ten filterable, quotable data points — and the "cruelty-free" claim moves from unstructured body copy, which AI systems weight less, into an actual attribute field.


## Structuring the schema so it holds up


A workable skincare schema separates fields into tiers instead of one flat attribute list:


1. **Core identity** : product name, format, size, brand
2. **Shopper-facing filters** : skin type, concern, active ingredient(s) with concentration
3. **Exclusion/claims flags** : fragrance-free, non-comedogenic, cruelty-free, vegan, pregnancy-safe
4. **Regulatory/compliance** : full INCI ingredient list, pH where relevant, SPF rating and broad-spectrum status for daytime products


Each tier needs a[controlled vocabulary](https://www.anglera.com/glossary/controlled-vocabulary) — "oily" and "oily skin" should resolve to the same facet value, not fragment into two dead-end filters. That normalization work is usually where catalogs actually fall apart: attributes exist somewhere in a spec sheet or a marketing brief, but they never make it into the structured field a facet or an AI feed reads from.


That gap between "the fact exists" and "the fact is in a queryable field" is exactly what Anglera closes. Your PIM stores the data; Anglera continuously scores product data completeness against schemas like this one, flags where skin type, concern, or active-ingredient fields are missing or inconsistent, and gap-fills them from existing content, brand specs, and ingredient lists — without requiring a rip-and-replace of whatever commerce stack or PIM you already run.
