---
schema_version: "1.0.0"
document_id: "b43b96855cb46e0a8f842e45ed38eff05f6aaa21994a7126a57aed99c4388057"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/salesforce-commerce-cloud-technical-seo-checklist"
published_at: "2026-06-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:08.422826+00:00"
content_hash: "sha256:106bd577ccf37e27f6abaccb41737f23ede460484537533ab3c5254b97eebd70"
---

# The technical SEO checklist for Salesforce Commerce Cloud product pages

Salesforce Commerce Cloud ships real SEO infrastructure: Page Meta Tag Rules, URL rules, sitemaps, canonical tags — but none of it is "set and forget." Most of it is rule-based and configuration-driven, so a product page can look perfect to a shopper and still be unreadable to a crawler or AI agent if one setting is off. This checklist walks through what matters on an SFCC[product detail page](https://www.anglera.com/glossary/pdp-product-detail-page) (PDP), roughly in the order a technical reviewer would check it.


## Rendering: confirm what's actually in the response, not the browser


What matters is whether the meaningful content (name, price, description, attributes, availability) exists in the HTML the server returns, before any JavaScript runs. This differs by architecture:


- **SFRA (Storefront Reference Architecture)** : ISML templates render server-side on request, so a PDP's content is present in the initial HTML by default. The checklist item here is really about *what* the template outputs, not whether it renders at all.
- **PWA Kit / Composable Storefront** : The Retail React App runs through an SSR render function in Managed Runtime, then hydrates client-side. The first response is real HTML, but content fetched or computed only after hydration (late-loading reviews, personalized pricing) won't be in it and can be invisible to non-JS clients. ([Rendering — Composable Storefront](https://developer.salesforce.com/docs/commerce/pwa-kit-managed-runtime/guide/rendering.html) )


Either way, verify with a raw HTTP fetch, not a browser tab. See "How to validate" below.


## Structured data: Product schema, sourced from the same data that drives the page


B2C Commerce offers a native structured-data capability for product pages that emits Product-type schema.org markup (name, image, description, brand, and offer details like price, currency, and availability) based on the same catalog data used to render the page. Salesforce's own release notes have carried this capability under a "(Pilot)" label, so treat it as opt-in: confirm it's enabled, and don't assume reviews/ratings or variant-level offers are covered without checking current documentation. ([Structured Data for Commerce Product Meta Tags](https://help.salesforce.com/s/articleView?id=sf.comm_seo_structured_data.htm&language=en_US&type=5) )


If you're hand-rolling[JSON-LD](https://www.anglera.com/glossary/json-ld) instead (common on older SFRA sites), keep it minimal and accurate. Don't emit` aggregateRating` or` offers` your data can't back up:


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "Product Name",
"sku": "SKU-12345",
"image": ["https://cdn.example.com/images/product-12345.jpg"],
"brand": { "@type": "Brand", "name": "Brand Name" },
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "49.99",
"availability": "https://schema.org/InStock",
"url": "https://www.example.com/product-12345.html"
}
}


```


## Titles and meta descriptions: rule-based, not hand-typed per page


For catalogs with thousands of SKUs, you don't write a title tag and meta description per product. You write **Page Meta Tag Rules** once, scoped to Product (or Category, or Content), using dynamic variables that resolve at render time. Rules live under Merchant Tools → SEO → Page Meta Tag Rules, and a typical title rule looks like:


```text
${Product.pageTitle} | ${Site.displayName}


```


with a description rule such as:


```text
${Product.pageDescription} — shop now at ${Site.displayName}


```


` request.pageMetaData` carries the resolved values into` htmlHead.isml` (or the equivalent PWA Kit head component), which writes the tags into the response. Keep titles roughly 50–60 characters and descriptions roughly 150–165 characters so search engines don't truncate them, and add a fallback rule for products missing a custom` pageTitle` /` pageDescription` so no PDP ships blank. ([Create Page Meta Tag Rules for B2C Commerce](https://help.salesforce.com/s/articleView?id=cc.b2c_creating_page_meta_tag_rules.htm&language=en_US&type=5) )


## Canonicals: one URL per product, watch multi-category assignment


SFCC generates canonical tags for standard PDPs automatically, so most teams don't need to build them. The catch is what the canonical points *to* : it's typically built from the product's primary category path. Assign a product to multiple categories (a toaster under "Kitchenware" and "Gifts") and change the primary one, and the canonical, and the URL search engines associate with the page, can shift too. Per-page or filter canonical overrides aren't exposed in Business Manager; they require custom development. Check hostname aliases (Merchant Tools → SEO → Aliases) too, so the same site isn't reachable, and indexable, on more than one domain. For PWA Kit sites, the alias must match the storefront's live domain exactly. ([Create Canonical URL Tags for B2C Commerce](https://help.salesforce.com/s/articleView?id=cc.b2c_creating_canonical_url_tags.htm&language=en_US&type=5) )


## Images and alt text: annotate at the catalog level, not the template


Image alt text and title text are catalog data in SFCC, not template strings. Each image assignment (by view type or variation) can carry an explicit, localizable alt description, or you can define a **default annotation rule** — a pattern built from marker attributes (color, material, angle) — so newly imported images inherit sensible alt text instead of shipping blank. This matters for two audiences: shoppers using screen readers or image search, and AI agents that treat alt text as a cheap signal when they skip running a vision model on every image.


## Internal linking: let the category structure do the work


Breadcrumbs on an SFRA or PWA Kit PDP are generated from the product's category path, the same path that feeds the canonical URL, so correct primary category assignment pays off twice: right canonical, and a breadcrumb trail that hands link equity down from category pages. Beyond breadcrumbs, make sure related-product and cross-sell components (Page Designer or SFRA includes) render as real, crawlable anchor tags, not JS-only click handlers. Keep component nesting on Page Designer pages to five levels or fewer, too; deep nesting slows rendering and can bury links.


## Performance: images and caching, not just code


Two levers move the needle most on SFCC PDPs. First, the built-in Dynamic Imaging Service (DIS) serves resized, cached images through Salesforce's CDN: use it instead of raw originals, and set explicit` width` /` height` (or` aspect-ratio` ) on image tags so the browser reserves space before load, keeping CLS down. Second, cache deliberately: keep static PDP content in the page cache, and move volatile pieces like live inventory or personalized pricing into remote includes so they don't force the whole page to bypass cache. On PWA Kit, Managed Runtime's CDN caches the SSR output itself, so a cache-friendly page structure benefits both the crawl and every human visit after it.


## Crawlability: robots.txt, sitemaps, and URL rules


Three Business Manager areas gate whether crawlers ever reach your PDPs at scale:


- **URL Rules** (Merchant Tools → Site → SEO & Discoverability → URL Rules) generate the clean, human-readable URLs PDPs live at; enable lowercasing to avoid case-duplicate URLs.
- **Sitemaps** (Merchant Tools → SEO → Sitemaps) produce a` sitemap_index.xml` referencing child files, respecting the standard 50,000-URL / 10MB caps per file; schedule generation to run *after* staging-to-production replication, or it will miss same-day changes. ([Improve SEO with a Sitemap — Composable Storefront](https://developer.salesforce.com/docs/commerce/b2c-commerce/guide/create-a-sitemap.html) )
- **Robots** (Merchant Tools → SEO → Robots, or the equivalent file in PWA Kit) should reference the sitemap index with a` Sitemap:` directive.


## How to validate


- **View-source vs. rendered DOM** : compare` curl -A "Mozilla/5.0 (compatible; Googlebot/2.1)" https://www.example.com/product-12345.html` against the browser's Elements panel post-hydration. Title, meta description, canonical, and Product JSON-LD should all be present in the raw response.
- **Structured data** : run the PDP URL through[Google's Rich Results Test](https://search.google.com/test/rich-results) and confirm Product markup parses with no missing required fields.
- **Meta tags** : use Business Manager's meta tag preview before publishing, then spot-check PDPs across categories.
- **Canonicals and sitemap** : confirm the canonical tag matches the URL you'd want indexed, and that the same URL appears in` sitemap_index.xml` .
- **robots.txt** : fetch` /robots.txt` directly and confirm it isn't disallowing` /product` or catalog paths, and that it points to the correct sitemap URL.


**Verified as of July 2026** against current Salesforce Help and Commerce Cloud Developer Center documentation. Menu paths, meta tag syntax, and structured-data field coverage can shift between releases, so reconfirm against your instance's release notes before rolling changes to production.


None of this works if the underlying product data is thin: a Page Meta Tag Rule can't invent a` pageDescription` that was never written, and a Product schema block is only as useful as the attributes behind it. Anglera continuously enriches that catalog data (attributes, specs, use-cases, identifiers) directly in the PIM or commerce platform feeding these templates, so every rule, canonical, and structured-data field has something substantive to render.
