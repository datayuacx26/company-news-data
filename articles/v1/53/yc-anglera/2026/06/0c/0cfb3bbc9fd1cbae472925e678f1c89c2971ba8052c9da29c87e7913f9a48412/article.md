---
schema_version: "1.0.0"
document_id: "0cfb3bbc9fd1cbae472925e678f1c89c2971ba8052c9da29c87e7913f9a48412"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/optimizely-technical-seo-checklist"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:5ae95ed1b90926f236ed494dc07523cd20d626e2c59520204a8dd4f32d3847f9"
---

# The technical SEO checklist for Optimizely Configured Commerce product pages

Configured Commerce (Spire) product pages are a React application first and an SEO surface second — which means most of the things that make a PDP "unreadable" to Google or an AI shopping agent are rendering and configuration issues, not content issues. This checklist walks through the platform-specific settings that determine whether a distributor's product data actually reaches a crawler, in the order we'd check them during an implementation review.


## Rendering: does the crawler see what the buyer sees?


Spire is a client-side React/Redux storefront by default, so if server-side rendering (SSR) isn't enabled, crawlers and most AI retrieval agents (which typically don't execute JavaScript the way a full browser does) may see an empty shell instead of your product data. Configured Commerce ships SSR support for Spire, and it's off by default for regular users — historically it applied only to requests matching known crawler user-agents.


Since release 5.2.2411, there's a separate setting, "Enable Server-Side Rendering for All User Agents" (Admin Console, under Administration, then Settings), that renders SSR for every visitor rather than crawlers only — the option Optimizely recommends if you're chasing Core Web Vitals as well as crawlability, since it removes the render-blocking gap between first paint and hydration for everyone, not just bots.


Two implementation notes that matter for distributors with custom PDP widgets:


- Only crawler-facing templates strictly need SSR — product detail, category/product list, brand, and content pages. Authenticated areas (cart, account, saved lists) are optional and often better left client-rendered if they depend on slow pricing/inventory APIs.
- React's` useEffect` and` useLayoutEffect` don't run during SSR — they only fire after client hydration. If a custom widget loads SEO-relevant data (specs, price, availability copy) inside a` useEffect` , that data will be missing from the server-rendered HTML a crawler sees. Move that logic into the component's initial render path (or a class component's` UNSAFE_componentWillMount` , per Optimizely's own guidance) if it needs to be crawlable.


## Structured data: turn on what's built in, then extend


Configured Commerce ships several[JSON-LD](https://www.anglera.com/glossary/json-ld) structured data types out of the box, toggled under Admin Console, Administration, Settings, Site Configurations (Classic CMS toggles this per-page instead, via "Enable Structured Page Data" on the[Product Detail Page](https://www.anglera.com/glossary/pdp-product-detail-page) ). Available types include:


- Product structured data — pulled from Product Details and written into the page head of the Spire PDP.
- Breadcrumb structured data — generated from catalog navigation on category/PDP pages.
- Sitelinks search box and Organization structured data — generated from Search settings and Website Details, written to the homepage.


These are off by default, so confirm they're actually enabled before assuming buyers (or AI agents parsing` Product` /` Offer` schema) are getting anything. The out-of-the-box` Product` markup already covers name, SKU, brand, and manufacturer number, but it won't necessarily include every field an AI shopping agent wants —[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) and` AggregateRating` /review data in particular are common gaps, since Optimizely's own documentation notes these aren't populated by default — so plan a short partner engagement to extend the JSON-LD template if those fields matter for your catalog:


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "20V MAX Cordless 18GA Flooring Stapler",
"sku": "DCN660B",
"gtin12": "885911234567",
"brand": { "@type": "Brand", "name": "DEWALT" },
"description": "…",
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "249.00",
"availability": "https://schema.org/InStock"
}
}


```


## Titles and meta descriptions


Page Title, Meta Description, and Meta Keywords are per-product fields, set at Admin Console, Catalog, Products, Edit, then the Content tab, under Meta Data — or bulk-loaded via Product Import or your ERP feed, which is the practical path for distributors with tens of thousands of SKUs. Because these fields are typically blank until populated, an unmapped ERP export usually means Spire falls back to a generic template (often just the product name), so audit a sample of live PDPs rather than assuming the field is filled.


## Canonicals: get this right before you touch anything else


Distributor catalogs almost always have the same SKU living under multiple categories, so Configured Commerce canonicalizes product URLs by default: whichever category path a shopper actually navigated through, the canonical link tag in the page head points to a standardized` /Product/{name}` -style URL, stripping the category segment. Three settings control the behavior, all in Site Configurations:


- **Product Canonical Root Path** — the first URL segment for canonical product links (default` Product` ; can be blank).
- **Microsite Canonical Products** — whether microsite/multi-site products get their own canonical URLs or point back to the root-site product.
- **Canonical Link for Products In Sitemap** — whether the sitemap lists canonical product URLs or the category-specific path.


If you run multiple storefronts (microsites) off one catalog, get the microsite setting deliberately reviewed — the default behavior of pointing microsite products at the root domain is usually correct for avoiding duplicate content, but it also means a microsite-only product needs its own canonical, or it won't get indexed at all.


## Images and alt text


Product images are managed at Admin Console, Catalog, Products, Edit, then the Images tab, with required Small/Medium/Large variants (Medium is used as fallback if Small or Large aren't set). The base admin UI doesn't expose a dedicated alt-text field for product images, so confirm with your implementation partner what the PDP image widget in your specific Spire theme uses as the image's alt attribute — commonly the product name or short description — and check it's non-empty and specific rather than a generic placeholder, since this is one of the more common gaps distributors hit during an SEO audit.


## Internal linking and crawlability


Sitemap and robots visibility are managed in two places: Websites, then the SEO tab (per-page visibility, plus a "Hide all products from search engines" toggle worth double-checking is off), and Administration, Settings, Site Configurations for site-wide defaults. Once a sitemap is generated, Configured Commerce references it in` robots.txt` automatically and can ping it to Bing via the IndexNow API key setting. The` robots.txt` file itself is a content-tree page editable from Admin Console, View Website, Content Tree.


For internal linking, related/cross-sell/up-sell product associations (set per-product in the Admin Console) become the crawl paths between PDPs and category pages — thin or missing associations are a common reason large distributor catalogs have orphaned SKUs that never surface in either search or AI retrieval.


## How to validate


- **View-source vs. rendered DOM** : run` curl -A "Googlebot" https://yoursite.com/Product/your-sku` and diff it against the browser DOM (Chrome DevTools Elements panel, or the rendered "View Page Source" via Inspect). If the curl output is missing price, specs, or the canonical tag that the browser shows, SSR isn't covering that template or that crawler user-agent.
- Confirm JSON-LD with Google's[Rich Results Test](https://search.google.com/test/rich-results) on a live PDP URL, and check the` Product` type resolves with the fields you expect.
- Spot-check the canonical link tag on the same SKU reached via two different category paths — it should be identical both times.
- Fetch` /robots.txt` and your sitemap URL directly to confirm products aren't excluded and the sitemap isn't stale.


**Verified as of July 2026** against Optimizely's Configured Commerce developer documentation and support help center; SSR, canonicalization, and structured-data settings are plan/version-dependent, so confirm exact toggle names against your instance's release notes before an audit.


None of this checklist matters if the underlying product data is thin — you can serve a perfectly-rendered page with nothing in it. Anglera runs alongside your PIM to keep the attributes, specs, and use-case content that feed these titles, JSON-LD fields, and alt text continuously enriched, so the rendering work above has something substantive to put on the page.
