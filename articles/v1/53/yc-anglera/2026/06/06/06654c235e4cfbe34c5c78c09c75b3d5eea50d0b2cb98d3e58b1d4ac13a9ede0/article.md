---
schema_version: "1.0.0"
document_id: "06654c235e4cfbe34c5c78c09c75b3d5eea50d0b2cb98d3e58b1d4ac13a9ede0"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/product-schema-fields-that-matter"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:0e5de462094889b4f300cd80a21d62f76765830b817ee11ffa2cd9913d39e0a0"
---

# Product structured data: the schema.org fields that matter for AI and rich results

Structured data is the one artifact on a product page that both Googlebot and AI crawlers parse the same way: a typed, unambiguous statement of what the product is, what it costs, and whether it's in stock. Most teams already have a` Product`[JSON-LD](https://www.anglera.com/glossary/json-ld) block somewhere in their template. Fewer have checked it against what Google actually requires versus what's merely nice to have — and that gap is usually where rich results silently fail to show up.


## Two paths, one script tag


Google Search Central splits product markup into two eligibility tracks that share the same` Product` type:[Product snippets](https://developers.google.com/search/docs/appearance/structured-data/product-snippet) , for pages where the product itself isn't directly purchasable (editorial reviews, informational pages), and[Merchant listings](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing) , for pages where a shopper can actually buy the item. Merchant listings support more of the fields buyers care about — shipping cost, return policy, apparel sizing — and Google's own guidance notes that pages meeting the merchant-listing requirements are automatically eligible for product-snippet treatment too, so distributors and retailers should generally build to the merchant-listing spec rather than the lighter one.


Both tracks read the same JSON-LD, so there's no reason to maintain two markup strategies. The distinction is about which fields you populate, not which schema you choose.


## The fields that actually gate eligibility


For a **product snippet** , Google requires` name` plus at least one of` offers` ,` review` , or` aggregateRating` — you only need one of the three, though supplying more strengthens the result. For a **merchant listing** , the hard requirements are` name` ,` image` , and a nested` offers` with a price and currency. Everything else —` description` ,` sku` ,` gtin` (or` gtin8` /` gtin12` /` gtin13` /` gtin14` /` isbn` ),` mpn` , and` brand` — is "recommended," which in practice means: skip it and you're technically valid but you lose eligibility for the richer visual treatments (price history, size/variant pickers, review stars) and you give an[AI crawler](https://www.anglera.com/glossary/ai-crawler) less to work with when it's trying to disambiguate your SKU from a near-identical competitor listing.


A minimal but real merchant-listing block looks like this:


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "Bosch GBH 18V-26 Cordless Rotary Hammer",
"image": [
"https://example.com/images/gbh18v26-1x1.jpg",
"https://example.com/images/gbh18v26-4x3.jpg",
"https://example.com/images/gbh18v26-16x9.jpg"
],
"description": "18V SDS-plus cordless rotary hammer, brushless motor, 2.6 J impact energy, bare tool.",
"sku": "GBH18V26-BARE",
"mpn": "0611917000",
"gtin13": "3165140886541",
"brand": {
"@type": "Brand",
"name": "Bosch"
},
"offers": {
"@type": "Offer",
"url": "https://example.com/products/gbh18v26-bare",
"priceCurrency": "USD",
"price": 249.00,
"priceValidUntil": "2026-12-31",
"itemCondition": "https://schema.org/NewCondition",
"availability": "https://schema.org/InStock",
"seller": {
"@type": "Organization",
"name": "Example Distribution Co."
}
}
}


```


` priceCurrency` must be a three-letter ISO 4217 code,` availability` and` itemCondition` must be full schema.org URLs (not bare strings like` "InStock"` ), and` price` must be a positive number, not a range or a string like` "Call for price"` . If you sell the same item in multiple currencies or regions, each variant needs its own canonical URL and its own` Offer` — don't try to stuff multiple currencies into one offer.


## The fields that do the heaviest lifting for AI


Google and LLM-based shopping agents both lean hardest on the identifier fields —` gtin` ,` mpn` ,` sku` , and` brand` — because they're the only ones that let a machine match your listing to the same product elsewhere on the web with confidence.` additionalProperty` (a repeatable` PropertyValue` array) is where structured attributes and specs belong — voltage, material, dimensions, certifications — and it's the field most PDPs leave empty even when the same data exists in a spec table two inches below the fold. If it's only in a rendered HTML table and not in` additionalProperty` , an AI agent has to infer structure from prose, which it does inconsistently.


For products that come in sizes, colors, or configurations, use[ProductGroup](https://developers.google.com/search/docs/appearance/structured-data/product-variants) with` isVariantOf` on each child` Product` , plus` variesBy` listing which property differs (color, size). Treating every variant as an unrelated standalone` Product` is a common mistake that fragments reviews and price history across SKUs that should be read as one item.


` aggregateRating` and` review` matter for both surfaces, but only if they're real: reviewer names must resolve to an actual` Person` or` Organization` , not a coupon headline, and Google explicitly disqualifies markup that doesn't match what's visibly rendered on the page.


## Shipping and returns: the fields buyers actually ask AI about


` offers.shippingDetails` (` OfferShippingDetails` ) and` offers.hasMerchantReturnPolicy` (` MerchantReturnPolicy` ) are recommended, not required, but they answer exactly the questions a shopping agent is likely to ask on a buyer's behalf — "does it ship free, and can I return it?"` MerchantReturnPolicy` can be declared once at the` Organization` level if your policy is uniform, or overridden per offer for exceptions (final-sale items, freight-only SKUs). Google's[return policy documentation](https://developers.google.com/search/docs/appearance/structured-data/return-policy) also allows a simpler` merchantReturnLink` instead of full structured terms — use the structured version if your policy varies by category, since it's the only way to express` returnPolicyCategory` and` merchantReturnDays` distinctly per product line.


## Common mistakes worth checking for


- **Markup that doesn't match the rendered page.** Price, availability, and name in the JSON-LD must agree with what a shopper actually sees — Google treats mismatches as a spam-policy violation, not a minor bug.
- **Client-side-only rendering.** If the` Product` block is injected by JavaScript after initial load, confirm Googlebot's rendered HTML actually contains it — don't assume; check.
- **One` Product` block per template, reused across variants.** Each purchasable SKU needs its own` Offer` (and its own canonical URL) even if the page visually shows a shared parent.
- **String availability values.**` "in stock"` isn't valid; it has to be the full URL` https://schema.org/InStock` .
- **Category or listing pages marked up as` Product` .** Merchant listings are restricted to single-product pages.


## How to validate


Check three things, in order:


1. **View-source vs. rendered DOM.**` curl -s https://example.com/products/sku | grep -A 40 'application/ld+json'` shows what's in the raw HTML. If your JSON-LD only appears in the browser-rendered DOM (inspect via dev tools) and not in view-source, confirm your rendering approach serves it server-side, or that it survives Googlebot's rendering pass.
2. **Google's Rich Results Test.** Paste the live URL into the[Rich Results Test](https://search.google.com/test/rich-results) — it flags missing required fields distinctly from missing recommended ones, and it's the authoritative signal for eligibility, not third-party linters.
3. **Search Console's Merchant Listings report.** Once live, watch this report for field-level errors and warnings across the whole catalog rather than checking pages one at a time.


Verified as of July 2026 against Google Search Central's Product, product-snippet, merchant-listing, product-variants, and return-policy documentation, and the current schema.org Product/Offer vocabulary; Google has updated this documentation multiple times in the last two years, so recheck field requirements before a large re-platform.


Getting these fields right assumes the underlying data — accurate GTINs, complete spec attributes, current pricing and availability — actually exists somewhere upstream, which for most catalogs it doesn't at full coverage. Anglera continuously enriches that layer (identifiers, structured attributes, use-case detail) in whatever PIM or commerce platform already holds your catalog, so the JSON-LD your template emits has real, complete values to point at instead of blanks.
