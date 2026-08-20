---
schema_version: "1.0.0"
document_id: "9d1db64a632834329a39d9e11f1a5e4ad7f76327e81a7dc9de79fc054317fb29"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/commercetools-product-json-ld"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:bf8cd8d22be02fd7aa8e2db07e0c95897ad34c931599066c42833006f2bd6a9a"
---

# Adding Product JSON-LD on commercetools — and keeping it in sync

commercetools is headless: it stores products, prices, and reviews, but nothing about it renders HTML or emits structured data for you. That part lives in your storefront (Next.js, Nuxt, commercetools Frontend, or a custom build), and it's easy to get subtly wrong — a stale price in the[JSON-LD](https://www.anglera.com/glossary/json-ld) , a` brand` attribute that doesn't exist on half your product types, an` aggregateRating` that never updates. Below is a field-by-field mapping from the commercetools data model to a valid Product JSON-LD block, plus the sync mechanics that keep it honest.


## Where this code lives


There's no commercetools "SEO module" that outputs JSON-LD — you build it in the layer that renders the PDP, from the same API response that renders the visible page. In practice that means:


- **Next.js / React storefronts** : build the JSON-LD object in the same server component (or` getServerSideProps` /route handler) that fetches the product for the page, and inject it via a` <script type="application/ld+json">` tag rendered server-side — never a client-only` useEffect` that patches it in after hydration, since crawlers that don't execute JS (and many AI agents) will only see the pre-hydration HTML.
- **commercetools Frontend (formerly Frontastic)** : add the script tag in the PDP page template/tastic that already has the mapped` Product` data model available; don't call a second, separate API for structured data.
- **Any other stack** : same rule — one data fetch, two outputs (visible markup, JSON-LD), not two fetches that can drift out of sync with each other.


## Mapping commercetools fields to schema.org Product


Pull this from the[Product Projection](https://docs.commercetools.com/api/projects/productProjections) you already fetch for the PDP (not the raw` Product` resource, which carries both staged and current data — see the sync section below).


schema.org field commercetools source


` name`` masterVariant` /product` name` , a` LocalizedString` — read the same locale key you're rendering on the page


` description` product` description` (` LocalizedString` )


` sku`` variant.sku` (unique per[ProductVariant](https://docs.commercetools.com/learning-model-your-product-catalog/product-modeling/products) )


` gtin` ,` gtin8/12/13/14` ,` mpn` commercetools has no native GTIN/MPN field — these are custom text[Attributes](https://docs.commercetools.com/learning-model-your-product-catalog/product-modeling/product-types) on the Product Type (commonly named` gtin13` ,` ean` , or` mpn` ); read them by attribute name from` variant.attributes`


` brand` also not a built-in field — modeled as a custom text/enum attribute (e.g.,` brand` ) or a reference attribute pointing at a Category/custom object used for the brand's own name and logo


` image`` variant.images\[\].url`


` offers.price` /` priceCurrency` the selected embedded` Price.value.centAmount` /` currencyCode` — commercetools returns multiple scoped prices per variant (by currency, country, customer group, channel); resolve the same price-selection context (currency/country/customer group) server-side that you used to render the visible price


` offers.availability` derived from Inventory (` InventoryEntry.availableQuantity` ) or Standalone Prices/channel stock, depending on how you've modeled fulfillment — map` > 0` to` https://schema.org/InStock` ,` 0` to` OutOfStock`


` aggregateRating.ratingValue` /` reviewCount` commercetools' native[Reviews](https://docs.commercetools.com/api/projects/reviews) resource plus the product's` reviewRatingStatistics.averageRating` and` reviewRatingStatistics.count` fields — no separate reviews platform required if you're already using this resource


Two things worth calling out because they trip teams up:


- **[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) and brand are attributes, not fields** — merchant-configurable per Product Type. If a category was set up without a` gtin13` attribute, or with` EAN` instead, read by the attribute's actual key (check the Product Type definition, don't assume a name) and fail gracefully rather than emitting an empty` gtin` .
- **` reviewRatingStatistics.averageRating`** is already rounded (to five decimal places) and pairs with` ratingsDistribution` buckets keyed by whatever rating values are actually in use (commonly 0–5 stars) — you don't need to compute the average yourself from raw` Review.rating` values, which use a wider −100..100 internal scale so the same field can represent stars, percentages, or a like/dislike.
- commercetools' **Product-level Attributes** (defined once and shared across all variants, as distinct from the long-standing **variant-level** Attributes) shipped as public beta in June 2025 and reached general availability in December 2025. On this model, GTIN/brand may live in the product-level` attributes` array under` masterData.current` rather than in each variant's own` attributes` — check which model each Product Type uses. Don't confuse this HTTP API` attributes` field with the GraphQL-only` attributesRaw` raw-value accessor, which is a separate thing.


## A real JSON-LD example


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "Trailhead 3L Rain Shell",
"description": "Waterproof, breathable 3-layer shell built for alpine weather.",
"sku": "TH-RS-3L-BLK-M",
"gtin13": "0194900123456",
"brand": {
"@type": "Brand",
"name": "Trailhead"
},
"image": [
"https://cdn.example.com/products/th-rs-3l-black/hero.jpg",
"https://cdn.example.com/products/th-rs-3l-black/back.jpg"
],
"offers": {
"@type": "Offer",
"url": "https://www.example.com/products/trailhead-3l-rain-shell/black/m",
"priceCurrency": "USD",
"price": "189.00",
"priceValidUntil": "2026-12-31",
"itemCondition": "https://schema.org/NewCondition",
"availability": "https://schema.org/InStock",
"seller": {
"@type": "Organization",
"name": "Example Outfitters"
}
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.6",
"reviewCount": "212"
}
}


```


This is the merchant-listing shape Google looks for (` offers` with` price` /` priceCurrency` /` availability` ); only include` aggregateRating` when the product actually has qualifying reviews behind it — don't emit a static or default rating.


## Keeping it in sync with the visible page


This is the part that actually matters day to day, because a page that "has JSON-LD" but disagrees with what a shopper (or an AI shopping agent) sees on the page is worse than no JSON-LD at all — Google's guidance is explicit that structured data must reflect the visible page content, and mismatches between the two can cost you eligibility.


- **One fetch, one context.** Generate the JSON-LD from the exact same Product Projection call — same locale, same currency/country/customer-group price selection — that renders the visible price and copy. Don't run a second code path (e.g., a cached SEO job) that computes the JSON-LD independently; that's how price and stock drift apart.
- **Current, not staged.** Product Projections can be fetched as` staged` (draft, merchandiser-only) or` current` (published). Build customer-facing JSON-LD from` current` only, even in preview environments.
- **Invalidate together.** commercetools emits change[Messages/Subscriptions](https://docs.commercetools.com/api/projects/messages) (` ProductPublished` , price changes, inventory changes). Wire PDP cache/ISR invalidation to these so the visible price and the JSON-LD price expire and re-render together, not one before the other.
- **Attribute drift.** Because GTIN/brand are custom attributes, a Product Type edit (renaming an attribute, adding a locale) can silently break the mapping. Log, rather than emit blank or stale values, when an expected attribute key is missing.
- **Reviews are live data.** If you pull` reviewRatingStatistics` at request time, make sure it comes from the same read model as any "4.6 stars (212 reviews)" text on the page — not a nightly batch job on a different schedule.


## How to validate


- **View-source vs. rendered DOM** : run` curl -s https://www.example.com/products/your-product | grep -A 40 'application/ld+json'` to confirm the JSON-LD is present in the raw HTML response, not only after client-side JS runs. Compare that output to what you see in the browser's rendered DOM (Inspect Element) — they should match exactly.
- **Rich Results Test** : paste the live URL into[Google's Rich Results Test](https://search.google.com/test/rich-results) and confirm it detects the` Product` type with no errors. Note that this tool checks markup validity, not whether the values match the visible page — that check is on you (and on Google's manual/algorithmic review after the fact).
- **Spot-check price and stock** : pull the same product in two currencies/countries if you sell internationally, and confirm the JSON-LD price context matches the page for each, not just your default market.


**Verified as of July 2026** against commercetools' HTTP API docs (Product Projections, Product Types, Reviews) and Google's Product structured data guidelines. commercetools ships new API features on a rolling basis (e.g., Product-level Attributes went public beta → GA between June and December 2025) — recheck field names and beta/GA status against current docs before shipping.


None of this markup is worth much if the underlying attributes are thin — a` gtin` field with no value, a` brand` no one filled in, or specs that stop at "color" and "size." That's the data-enrichment problem Anglera is built to solve continuously in the background, so your Product Types actually have the GTINs, brand names, and use-case attributes to put in the fields above. The PIM (or commercetools itself) stores the data; Anglera does the work of keeping it complete, so this page-side JSON-LD has something real to render.
