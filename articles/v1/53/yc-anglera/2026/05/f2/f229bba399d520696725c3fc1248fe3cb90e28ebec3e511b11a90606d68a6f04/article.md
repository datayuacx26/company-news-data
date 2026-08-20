---
schema_version: "1.0.0"
document_id: "f229bba399d520696725c3fc1248fe3cb90e28ebec3e511b11a90606d68a6f04"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/bigcommerce-ssr-rendering"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:cf1cf0874154ff5488c6090db5451ec49473b6ab472ce81294ddbefebd044390"
---

# Server-side rendering on BigCommerce: making product data visible to Google and AI

BigCommerce ships two different rendering models depending on which storefront you run: the built-in Stencil theme engine, and the newer Catalyst headless storefront. Both can put full product data into the HTML a crawler receives on first request — but both can also be configured or customized in ways that push key data into a client-side JavaScript pass instead. This guide covers how each model actually renders a product page, where the common blind spots are, and how to confirm your enriched product data is landing in the response, not just in the browser after the fact.


## Two rendering models, one underlying question


**Stencil** (the default BigCommerce storefront framework, used by Cornerstone and most themes in the Theme Marketplace) is server-rendered by design. Templates are written in Handlebars, and in production "Handlebars statements run on the server side, generating HTML received by the shopper's browser" — the theme engine resolves your product objects (title, price, SKU, description, images, custom fields) into markup before the response ever leaves BigCommerce's servers ([Stencil docs](https://docs.bigcommerce.com/developer/docs/storefront/stencil/getting-started/about-stencil) ). That means a plain HTTP fetch of a Stencil product URL — no JavaScript execution required — already contains the core product content in the DOM.


**Catalyst** , BigCommerce's Next.js-based composable storefront, is also SSR by default for product and category routes: dynamic pages are rendered at request time against the GraphQL Storefront API, with caching layered on top rather than replacing the render. BigCommerce's own guidance is that stores with small, infrequently changing catalogs can lean on static generation, while larger or fast-changing catalogs should combine dynamic SSR with webhook-driven cache invalidation ([Catalyst overview](https://docs.bigcommerce.com/developer/docs/storefront/catalyst/overview) ). Either way, the intent is that product, price, and structured data are resolved on the server before the page reaches the client — but Catalyst is a fully open codebase you deploy yourself, so it is entirely possible to write a component that fetches product data client-side (for example, inside a` "use client"` component with a` useEffect` fetch) and accidentally move data out of the initial HTML.


## Where client-only rendering actually creeps in


Neither platform is "client-rendered" by default, but both have well-known spots where content only appears after the browser runs JavaScript:


- **Stencil AJAX option/price updates.** When a shopper changes a variant (size, color), Stencil themes commonly call the storefront's product API client-side (via` stencil-utils` , e.g.` utils.api.product.getById(...)` ) to refresh price, SKU, and availability without a full page reload. That's good UX, but it means the *variant-specific* price/SKU a crawler sees is whatever was rendered for the default variant in the initial HTML — not whatever a shopper clicks into. If your enriched attributes (materials, capacity, compliance specs) only get written into the DOM through this same AJAX path, they never reach the server response at all.
- **Third-party review and Q&A widgets.** Cornerstone server-renders a full` Product`[JSON-LD](https://www.anglera.com/glossary/json-ld) block by default (a` components/products/schema` template partial), including` aggregateRating` /` review` populated from BigCommerce's *native* review data when reviews exist. The gap appears when a store swaps in a third-party review app: those typically render stars, counts, and Q&A through their own client-side script and their own separate schema, which may never feed into Cornerstone's built-in JSON-LD. If your rating data depends on that widget's JavaScript, it can show up in the rendered DOM but not the raw HTML — invisible to a crawler that doesn't execute JS.
- **Custom Catalyst components that fetch on the client.** Anything wrapped in` "use client"` that calls the Storefront API in a` useEffect` (rather than being passed down as server-rendered props via` generateMetadata` or a server component) renders an empty shell on first paint and fills in afterward.
- **Customized themes are thinner than the default.** Stock Cornerstone JSON-LD covers name, SKU/MPN/[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) , brand, description, image, and` offers` — but stores frequently fork Cornerstone or install page-builder apps that replace or strip that partial, and enriched attributes were never in its scope (they live in the visible markup, not the schema block, unless someone adds them). Don't assume schema or enriched attributes are complete just because *some* structured data shows up in view-source.


## Why this matters beyond classic SEO


Google's own guidance describes JavaScript-heavy pages going through three separate phases — crawling, rendering, then indexing — not all at once. Googlebot first fetches the raw HTML; only later, once resources allow, "a headless Chromium renders the page and executes the JavaScript," and Google uses that rendered output (not the raw HTML) to index the page ([Google Search Central, JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ). That rendering step can sit in a queue and lag well behind the initial crawl. Google recommends server-side or pre-rendering partly because "not all bots can run JavaScript" — a list that includes most AI answer engines and shopping agents crawling your catalog today. If your[product attributes](https://www.anglera.com/glossary/product-attributes) , price, and identifiers only materialize client-side, an LLM-based crawler or shopping agent is far more likely to see a blank shell than a fully populated PDP.


## What should be in the server-rendered response


For a BigCommerce product page, the non-negotiable list is: product name, canonical URL, primary price and currency, availability, SKU/GTIN/MPN, description, primary image, and` Product` JSON-LD (including` offers` and, if you have real review data,` aggregateRating` ). All of this should be resolvable from data the server already has when the page is requested — none of it should depend on a follow-up client-side call. In Stencil, that means keeping these fields inside the Handlebars templates themselves — the product content lives in` templates/components/products/product-view.html` , and the` Product` JSON-LD is a separate partial (` templates/components/products/schema.html` ) included on the product page — not injected via a script tag that runs after the DOM content has loaded. In Catalyst, that means resolving these fields inside the server component or route handler and using` generateMetadata` plus a literal JSON-LD script element populated from the same server-side GraphQL result — not a client component that fetches after mount, for example:


```text
export async function generateMetadata({ params }: { params: { slug: string } }) {
const product = await getProduct(params.slug);
return { title: product.name, description: product.metaDescription };
}


// In the server component body:
// <script type="application/ld+json">
//   {JSON.stringify(productJsonLd)}
// </script>


```


## How to validate


Check the raw response the way a crawler does, before comparing it to what a browser shows:


```text
curl -s https://yourstore.example.com/product-slug/ | grep -i "application/ld+json" -A 20


```


```text
curl -s https://yourstore.example.com/product-slug/ | grep -io "\"price\"[^,]*"


```


Then compare that raw HTML to the rendered DOM:


1. In Chrome DevTools, open the page, then **View Page Source** (` Ctrl+U` /` Cmd+Option+U` ) — this is the pre-JavaScript HTML, equivalent to what` curl` returns.
2. Separately, open **Elements** in DevTools (the live/rendered DOM) and search for the same price, SKU, or JSON-LD block.
3. If a field exists in Elements but not in View Source, it was added by client-side JavaScript and may be invisible to non-JS crawlers.
4. Run the URL through Google's[Rich Results Test](https://search.google.com/test/rich-results) — it shows both the rendered result and flags missing/invalid` Product` schema fields.
5. For AI-agent-specific checks, fetch the URL with a plain HTTP client (no headless browser) and confirm the same attributes are present — that's closer to how many current AI shopping and answer agents actually retrieve pages.


**Verified as of July 2026:** rendering behavior described here reflects current Stencil and Catalyst documentation; always re-check theme-specific templates and any review/Q&A apps installed on your store, since those can override default behavior per theme version.


Getting the enriched attributes into the server-rendered template only matters if the attributes exist and stay current in the first place — that's the harder, ongoing problem. Anglera continuously enriches product data (attributes, specs, use-cases, identifiers) in your PIM or BigCommerce catalog, so whichever rendering path you use — Stencil template or Catalyst server component — has complete, current data to put on the page rather than gaps to paper over.
