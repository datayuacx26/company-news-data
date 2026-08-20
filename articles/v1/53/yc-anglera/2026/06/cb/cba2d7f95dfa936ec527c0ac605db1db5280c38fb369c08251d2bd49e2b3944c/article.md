---
schema_version: "1.0.0"
document_id: "cba2d7f95dfa936ec527c0ac605db1db5280c38fb369c08251d2bd49e2b3944c"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/beauty-attributes"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:593e96ad494419d1c69a5c9718994075946bb475d72175bbca6ccd3958f35ec6"
---

# Why beauty products go invisible: the attribute gaps that filter you out

A lipstick with a gorgeous photo and a one-line description still won't show up when a shopper filters by "matte" or asks ChatGPT for "a berry lipstick that won't dry out my lips." Beauty is one of the most attribute-dense categories in retail, and one of the sloppiest about structuring that data. Products that exist, are in stock, and would satisfy the shopper perfectly never surface, because the fields a filter or an AI agent needs are blank, buried in a paragraph, or spelled three different ways across the catalog.


## Why beauty breaks faceted search more than other categories


Faceted search and AI shopping agents work the same way under the hood: they read discrete attribute-value pairs, not prose. A shopper who clicks "matte" + "berry" + "long-wearing" in a sidebar, or types "a matte berry lipstick that lasts through dinner" into an AI assistant, is querying fields like` finish` ,` shade_family` , and` wear_time` . If those fields don't exist on the product record, the product is invisible to that query, even though the marketing copy might say "beautifully matte, deep berry hue, 8-hour wear" in a sentence.


Beauty compounds this problem because it carries more meaningful facets per SKU than almost any other vertical. A single foundation can reasonably have 15-20 shade variants, each needing its own undertone and depth data, plus category-level attributes for finish, coverage, skin type suitability, and formulation. Retailers are advised to consolidate and standardize attributes by category rather than dump every possible field on the shopper, which is really an admission that most catalogs haven't done the standardization work yet, per[BigCommerce's guide to ecommerce faceted search](https://www.bigcommerce.com/articles/ecommerce/faceted-search/) .


## The attributes that actually matter in beauty


Not every field is worth the effort. These consistently drive filter and AI-match behavior across color cosmetics, skincare, and haircare:


Attribute Why it matters Example values


Shade / shade family Core filter for makeup; groups individual shade names into buckets shoppers actually search Berry, nude, coral, deep red


Undertone Distinguishes cool/warm/neutral within a shade family; critical for foundation and concealer matching Cool, warm, neutral, olive


Finish Second most-used makeup filter after shade Matte, satin, dewy, shimmer, glossy


Coverage level Foundation/concealer/BB cream differentiator Sheer, medium, full


Formulation / texture Skincare and some makeup; affects layering and application Gel, cream, oil, balm, water-based, powder


Skin type suitability Drives skincare and foundation matching Oily, dry, combination, sensitive, all


Ingredient flags Increasingly a hard filter, not a nice-to-have Fragrance-free, paraben-free, non-comedogenic, alcohol-free


Ethical/sourcing claims Distinct from ingredient flags; needs its own field, not a footnote Vegan, cruelty-free, reef-safe


Active ingredients (INCI + common name) What shoppers and AI agents actually search skincare by Niacinamide (Vitamin B3), retinol, hyaluronic acid


Wear time / longevity Common qualitative filter, especially in color cosmetics 8-hour, transfer-resistant, waterproof


Application method Affects both filtering and how-to content Stick, liquid, cream, wand, brush-on


Ingredient data deserves special mention because it behaves differently from the rest. Shoppers and AI agents search by both the clinical INCI name and the plain-English name, and a three-layer structure, INCI name, common name, and the benefit it addresses, is what lets an ingredient show up whether someone asks for "niacinamide" or "something for redness," per[Alhena AI's breakdown of INCI data structuring for AI engines](https://alhena.ai/blog/inci-beauty-ingredient-data-ai-engines/) . That mapping has to live in crawlable text on the page, not inside a PDF or an ingredient-list image, or it doesn't exist as far as an AI shopping agent is concerned.


## The lipstick, before and after


Here's a real pattern: a lipstick feed record that has a title, a price, and marketing copy, but no queryable attributes behind it.


**Before (raw feed):**


Field Value


title Velvet Matte Lipstick


description A rich, long-wearing matte lipstick in a stunning berry shade that glides on smooth and stays put through dinner and drinks.


price $24.00


color Berry


gtin 0123456789012


That description reads fine to a human. But a facet filter for "matte," a shopper searching "long-wear lipstick," and an AI agent asked to recommend "a berry lipstick that won't feather" all fail against this record, because none of those concepts exist as fields. "Matte" and "long-wearing" are trapped in a sentence.


**After (enriched):**


Attribute Value


shade_name Midnight Berry


shade_family Berry


undertone Cool


finish Matte


formulation Cream-to-matte, non-drying


wear_time 8-hour, transfer-resistant


application_method Bullet / direct-application stick


ingredient_flags Fragrance-free, paraben-free, vegan


skin_benefit Hydrating base, non-feathering


Now the exact same product answers a facet click on "matte" + "berry," a search for "vegan long-wear lipstick," and an AI prompt like "ask an AI to recommend a matte berry lipstick that won't dry out my lips," because "cream-to-matte, non-drying" and "hydrating base" are sitting in structured fields the agent can actually read, not paraphrased in ad copy.


## Where this data actually needs to live


Google Merchant Center's own color guidance illustrates the trap: it expects one color value per variant and has no native concept of "shade family" or "finish" at all, per[Google's product data specification](https://support.google.com/merchants/answer/6324487?hl=en) . Retailers who only fill in what the feed spec demands end up with a` color` field and nothing else, enough to pass validation but not enough to win a facet click or an AI match. The attributes that actually drive discovery, finish, undertone, ingredient flags, wear time, mostly live in custom fields or nowhere at all.


That's a data-modeling problem, not a copywriting problem, and it's the gap between "the copy is good" and "the fields exist." A PIM can hold these fields once someone defines the taxonomy and fills every SKU consistently; most catalogs stall at the taxonomy step because it means auditing thousands of SKUs by hand.


Anglera plugs into whatever PIM or catalog a retailer already runs and handles that filling and standardizing continuously: scoring every beauty SKU against a shade, finish, formulation, and ingredient taxonomy, flagging the gaps, and enriching missing fields so shade variants, finish claims, and INCI ingredient names show up as structured data instead of prose. Your PIM stores the shade name; Anglera makes sure "matte," "berry," and "fragrance-free" are fields it can actually filter and answer on.
