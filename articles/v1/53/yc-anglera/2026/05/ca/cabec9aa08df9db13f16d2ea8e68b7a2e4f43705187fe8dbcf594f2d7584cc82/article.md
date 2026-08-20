---
schema_version: "1.0.0"
document_id: "cabec9aa08df9db13f16d2ea8e68b7a2e4f43705187fe8dbcf594f2d7584cc82"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/adobe-commerce-technical-seo-checklist"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:2bf37daae40f9e5c9b3b4ba4333672d74062d67609c8dfd104660d7a42dc2e2b"
---

# The technical SEO checklist for Adobe Commerce product pages

Enriched product data only helps buyers and AI shopping agents if it actually reaches the rendered page in a form both can parse. Adobe Commerce gives you most of the plumbing for that out of the box, but the defaults are conservative and a few settings are easy to get backwards on a large catalog. This checklist walks through the technical SEO surface of an Adobe Commerce[product detail page](https://www.anglera.com/glossary/pdp-product-detail-page) (PDP), section by section, with the exact admin paths as of the current Commerce Admin documentation.


## Rendering: what's actually in the first response


Before anything else, check what a crawler gets without executing JavaScript. On the default Luma theme, PDPs are server-rendered PHP/Knockout templates, so the price, description, attributes, and any server-injected metadata are present in the raw HTML. If you're on PWA Studio (still supported, though Adobe's active investment has shifted elsewhere) or on Adobe Commerce Storefront — the newer, Edge Delivery Services-based frontend built on the` aem-boilerplate-commerce` reference project — confirm which parts of the page are actually server-rendered: a PWA Studio storefront's app shell renders server-side via its UPWARD server, but a meaningful share of PDP content — including anything wired up via client-side GraphQL calls — can hydrate after the initial response. AI agents and many crawlers fetch raw HTML and don't wait for hydration, so title, meta tags, canonical, and[JSON-LD](https://www.anglera.com/glossary/json-ld) all need to be present in the initial document, not injected only after client-side rendering.


## Structured data: don't assume it's already there


This is the field most teams get wrong on Adobe Commerce. The default Luma theme does not ship full` Product` /` Offer` JSON-LD — it only includes schema.org **microdata** for` AggregateRating` , rendered from the reviews summary template, and only when the product has at least one review. If you need` Product` ,` Offer` , availability, price, and` BreadcrumbList` markup (which is what actually drives Google rich results and gives AI agents a clean structured summary), you have to add it via a custom template, a structured-data extension, or, if you're on Adobe Commerce Storefront, the official PDP drop-in component and PDP Metadata Generator tooling that Adobe documents for that architecture.


Whatever you use, render it server-side as JSON-LD in` <head>` , and never populate` aggregateRating` with placeholder or zero values — Google Merchant Center and Bing both treat fabricated ratings as a policy violation.


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"sku": "PUMP-4400-SS",
"name": "4400 Series Stainless Centrifugal Pump",
"description": "1.5 HP, 316 stainless housing, NPT 2-inch inlet/outlet.",
"image": ["https://cdn.example.com/media/catalog/product/p/u/pump-4400.jpg"],
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "2149.00",
"availability": "https://schema.org/InStock",
"url": "https://www.example.com/4400-series-stainless-centrifugal-pump.html"
}
}


```


## Titles and meta descriptions


Each product's SEO fields live on the product edit page under **Catalog > Products > \[product\] > Search Engine Optimization** :` URL Key` ,` Meta Title` ,` Meta Keywords` , and` Meta Description` , all store-view scoped. Adobe's guidance is a meta title under ~70 characters and a meta description in the 150–160 character range (255 max). Left blank, Commerce auto-generates these from templates (` {{name}}` for title,` {{name}} {{description}}` for description) — fine as a fallback, but on a distributor catalog with thousands of near-identical SKUs, auto-generated titles collapse into duplicates fast, so plan to populate these fields with attribute-driven values (size, model, material) rather than relying on the template alone.


## Canonical tags


Under **Stores > Settings > Configuration > Catalog > Catalog > Search Engine Optimization** , set **Use Canonical Link Meta Tag for Products** to Yes. This matters because Adobe Commerce can generate more than one working URL to the same product (direct URL-key path and category-prefixed path), and without canonicalization search engines may index and split authority across both. Adobe's own documentation recommends enabling canonical tags for both products and categories as a baseline.


## Images and alt text


Product images are managed under **Catalog > Products > \[product\] > Images and Videos** . Each image has a store-view-scoped **Alt Text** field plus role assignments (Base, Small, Thumbnail, Swatch) — the base image role is what renders on the PDP itself. Write alt text that describes the product concretely (material, variant, angle), not the filename or a repeated brand phrase; this is both an accessibility requirement and one of the few image-level text signals a crawler or AI agent gets when it can't render the image.


## Internal linking


Related Products, Up-sells, and Cross-sells are standard Commerce blocks and render as plain crawlable` <a>` links by default — make sure a custom theme hasn't wrapped them in JavaScript-only carousels that never land in the initial HTML. Category breadcrumbs on the PDP are another internal-linking signal search engines use to understand catalog hierarchy; confirm the breadcrumb trail reflects your actual category structure, since it also feeds the` BreadcrumbList` schema if you're emitting one.


## Faceted navigation and crawl budget


Layered navigation is the highest-risk area for a large B2B catalog: each filterable attribute set on **Use in Layered Navigation** in the attribute configuration multiplies into combinatorial URLs. Keep that setting off for attributes that aren't a genuine entry point for search, make sure filtered category URLs canonicalize back to the base category (Luma does this natively; custom themes sometimes break it), and use **Content > Design > Configuration > \[website\] > Search Engine Robots** to add` Disallow` rules for filter query parameters in` robots.txt` .


```text
User-agent: *
Disallow: /*?*color=
Disallow: /*?*price=


```


## Performance


Adobe strongly recommends Varnish over the file or database Full Page Cache backend in production; Commerce uses Edge Side Includes (ESI) to keep personalized blocks (cart, pricing tiers) out of the cached page shell. A fast, cached PDP response matters for Core Web Vitals (LCP in particular) and for crawl efficiency — slow responses are one of the two levers Google names for crawl budget, alongside content quality.


## Crawlability


Generate and submit your XML sitemap under **Marketing > SEO & Search > Site Map** , writing it to a path under` pub/media` if you're on Adobe Commerce Cloud infrastructure. Double check store-level indexing isn't blocked — Commerce Cloud has a separate "Hide from search engines" / indexing toggle in the Cloud Console that's easy to leave on for a staging clone and forget on production.


## How to validate


- **View-source vs. rendered DOM** : compare` curl -s https://yoursite.com/product-url.html` against the browser DevTools "Elements" tab. If title, meta description, canonical, or JSON-LD appear in DevTools but not in the curl output, they're client-rendered only and invisible to most crawlers and AI agents.
- **curl for headers and tags** :` curl -sI` for the canonical response, and` curl -s ... | grep -i 'canonical\\|<title\\|application/ld+json'` to confirm the raw markup.
- **Google Rich Results Test** : validate JSON-LD renders and passes with no errors — one invalid annotation on the page can invalidate all structured data for that URL.
- **robots.txt and sitemap.xml** : fetch both directly (` /robots.txt` ,` /media/sitemap.xml` ) to confirm they're live and not accidentally blocking product paths.


Verified as of July 2026 against Adobe Commerce's Experience League admin documentation; menu paths and field names may shift slightly across Commerce versions, so confirm against your instance's admin before rolling changes to production.


None of this checklist matters if the underlying product data is thin — a clean canonical tag around an empty description just ships an empty description faster. Anglera enriches the attributes, specs, and structured facts that feed these fields and JSON-LD blocks directly from your PIM or commerce platform, continuously, so the page-side work above has genuinely rich content to render rather than boilerplate.
