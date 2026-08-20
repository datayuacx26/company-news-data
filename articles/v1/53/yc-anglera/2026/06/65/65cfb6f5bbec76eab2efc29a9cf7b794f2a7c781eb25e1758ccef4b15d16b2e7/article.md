---
schema_version: "1.0.0"
document_id: "65cfb6f5bbec76eab2efc29a9cf7b794f2a7c781eb25e1758ccef4b15d16b2e7"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/enriched-data-to-page-checklist"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:c9d4fe184496370aeabaaa07351fc473d08f4832946032f5f4602b6e5dc1c776"
---

# From enriched data to the page: a technical-SEO checklist for any PDP

Getting a product page enriched — full attributes, specs, use-cases, identifiers — is only half the job. If that data never makes it into the actual HTML response, buyers and AI agents never see it. This checklist walks through the technical layer that carries enriched data from wherever it lives (PIM, commerce platform, metafield) onto a page that both humans and crawlers can parse, in the order to check it.


## 1. Rendering: get content into the HTML response


Google still processes JavaScript through a two-wave system: it crawls the raw HTML first, then queues a second pass through headless Chromium to render client-side content, sometimes hours or days later. Server-side rendering, static generation, or hydration remain Google's recommended patterns over pure client-side rendering, mainly because they make the first HTML response meaningful on its own ([Understand JavaScript SEO Basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ).


The bigger issue in 2026 is that most AI crawlers don't execute JavaScript at all.[GPTBot](https://www.anglera.com/glossary/gptbot) ,[ClaudeBot](https://www.anglera.com/glossary/claudebot) , Claude-User, Claude-SearchBot, and[PerplexityBot](https://www.anglera.com/glossary/perplexitybot) fetch raw HTML and generally do not run a rendering pass. If your enriched attributes, specs, or use-case copy only appear after a client-side fetch call resolves, these agents see an empty shell. Practical checklist:


- Enriched attributes, specs, and descriptions should be present in the initial server response, not fetched client-side after load.
- If you must ship a client-rendered app, add server-side rendering or static pre-rendering for product routes specifically — this is the one route type where it matters most.
- Verify with` view-source:` or` curl` (see validation section below) rather than trusting what you see in DevTools, which always shows the post-render DOM.


## 2. Structured data: JSON-LD Product markup


Google's Product structured data requires` name` plus at least one of` offers` ,` review` , or` aggregateRating` ; once one of those three is present, the rest (image, description, brand,` sku` ,` mpn` ,` gtin` ) become recommended rather than required ([Product snippet structured data](https://developers.google.com/search/docs/appearance/structured-data/product-snippet) ). A minimal but solid block looks like this:


```text
{
"@context": "https://schema.org",
"@type": "Product",
"name": "Example Widget 400",
"sku": "WID-400-BLK",
"gtin13": "0012345678905",
"mpn": "W400-BLK",
"brand": {
"@type": "Brand",
"name": "Example Brand"
},
"description": "Enriched product description with specs and use-case detail.",
"image": [
"https://example.com/images/widget-400-1x1.jpg",
"https://example.com/images/widget-400-4x3.jpg",
"https://example.com/images/widget-400-16x9.jpg"
],
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "129.00",
"availability": "https://schema.org/InStock",
"url": "https://example.com/products/widget-400-black"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.6",
"reviewCount": "212"
}
}


```


One rule matters more than any field name: Google's structured data guidelines state markup must reflect what's genuinely visible on the page, not content hidden behind toggles, absent from the DOM, or fabricated to win a rich result ([General Structured Data Guidelines](https://developers.google.com/search/docs/appearance/structured-data/sd-policies) ). If your enrichment pipeline populates a spec table that's rendered,[JSON-LD](https://www.anglera.com/glossary/json-ld) can mirror it. If a field only exists in the feed and never on the page, leave it out of the markup.


For products sold in multiple sizes, colors, or configurations, use the` isVariantOf` /` ProductGroup` pattern rather than duplicating one` Product` block per variant on a single URL — variants need their own URLs or the group pattern to stay compliant.


## 3. Identifiers: GTIN, MPN, brand, SKU


Identifiers are what let a crawler or shopping agent match your PDP to the same product elsewhere, and they matter for both structured data and any Merchant Center feed you run alongside it. Google's guidance: provide[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) together with MPN and brand where products have them, because missing or incorrect identifiers limit matching and visibility ([About unique product identifiers](https://support.google.com/merchants/answer/160161) ). For genuinely unbranded, custom, or pre-GTIN products, set` identifier_exists` to` false` in the feed rather than leaving fields blank or inventing values — a fabricated GTIN is worse than an honest "no identifier."


Keep identifier fields consistent across three places: the PIM/source of truth, the on-page JSON-LD, and the feed (Merchant Center, marketplace, or affiliate feed). Mismatches between these are a common cause of "incorrect identifier" flags.


## 4. Attributes and specs on the page itself


Structured data supplements the page; it doesn't replace a rendered spec table. Enriched attributes (dimensions, materials, compatibility, certifications) should appear as real text in the DOM — a definition list, table, or labeled grid — not only inside JSON-LD or an alt attribute. For attribute-heavy categories,` additionalProperty` (` PropertyValue` pairs) can carry structured attribute data in JSON-LD, but only alongside a visible on-page version.


## 5. Media


- Serve product images at multiple aspect ratios (Google recommends including images in the 1:1, 4:3, and 16:9 range within the` image` array) at reasonable resolution.
- Write real, descriptive` alt` text per image, not the product name repeated verbatim.
- Avoid` loading="lazy"` on the primary hero image — it can delay Largest Contentful Paint measurement and, in edge cases, prevent the image from appearing in a lightweight-fetch snapshot.


## 6. Meta and on-page tags


- Unique title tag and meta description per product; don't template them so tightly that every variant reads identically.
- Self-referencing canonical tag defined in the original HTML, not injected only by JavaScript.
- Open Graph tags (` og:title` ,` og:description` ,` og:image` ) for link previews and citation cards; these are also read by many AI agents when summarizing a page.
- If you run localized or multi-currency PDPs, use distinct URLs per locale/currency with correct` hreflang` , matching Google's guidance that Product markup should not mix currencies on one URL.


## 7. Performance


Core Web Vitals thresholds — Largest Contentful Paint under 2.5s, Interaction to Next Paint under 200ms, Cumulative Layout Shift under 0.1 — are still the bar Google references for good page experience ([Understanding Core Web Vitals](https://developers.google.com/search/docs/appearance/core-web-vitals) ). For PDPs specifically, the usual offenders are render-blocking third-party scripts (reviews widgets, chat, personalization) loaded above the fold, and layout shift caused by images or price blocks that load without reserved space. Reserve height for images and dynamic price/rating blocks with explicit width/height or aspect-ratio CSS.


## 8. AI crawler access


Rendering and markup only matter if agents are allowed to fetch the page. Check robots.txt against the specific tokens each provider uses — blocking one does not block the others. Anthropic runs three independent bots:` ClaudeBot` (training),` Claude-SearchBot` (search indexing), and` Claude-User` (fetches a page when a user asks Claude about it directly); all three honor robots.txt but need separate directives ([Anthropic crawler documentation](https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler) ). If your PDPs are meant to be citable in AI answers, confirm you aren't accidentally disallowing` Claude-User` ,` Claude-SearchBot` ,` OAI-SearchBot` , or` PerplexityBot` while trying to block training crawlers.


## How to validate


- **View-source vs. rendered DOM** : open` view-source:https://yourdomain.com/product/slug` and confirm the enriched description, attributes, and JSON-LD are present in the raw response — not just visible in DevTools' Elements panel, which shows the post-hydration DOM.
- **Curl check** :` curl -A "Googlebot" https://yourdomain.com/product/slug` (or no user agent at all) and grep for a known attribute string or` "@type": "Product"` to confirm it's server-rendered.
- **Rich Results Test** : run the live URL through[Google's Rich Results Test](https://search.google.com/test/rich-results) to confirm Product eligibility and catch missing required fields.
- **Search Console** : check the Product Snippets report periodically for identifier or markup errors surfacing at scale, not just on one URL.
- **robots.txt spot-check** : fetch` /robots.txt` and confirm the crawler tokens you intend to allow aren't disallowed by a broader rule higher in the file.


Verified as of July 2026 against Google Search Central, Google Merchant Center Help, and Anthropic's published crawler documentation; check these sources directly before implementation, since structured data requirements and crawler behavior do shift between releases.


None of this checklist matters if there's nothing enriched to render. Anglera keeps the underlying data — attributes, specs, use-cases, identifiers — continuously enriched inside whatever PIM or commerce platform you already run, so the page-side work above has accurate, complete content to put in front of buyers and agents in the first place.
