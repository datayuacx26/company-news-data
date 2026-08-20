---
schema_version: "1.0.0"
document_id: "f431e038169061851b158d986dcff9f293324bbf8db17d94b0da49dfd04b0817"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/salesforce-commerce-cloud-ssr-rendering"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:59.555603+00:00"
content_hash: "sha256:601600cd83bb434bbf7857ba381444b993f0fa6a6cc1aea8f9426db3a0632cc0"
---

# Server-side rendering on Salesforce Commerce Cloud: making product data visible to Google and AI

Salesforce Commerce Cloud (B2C Commerce) gives you two different rendering paths for a[product detail page](https://www.anglera.com/glossary/pdp-product-detail-page) , and they behave very differently in front of a crawler. One serves complete product data in the first response; the other can serve an empty shell until JavaScript runs. Knowing which one you're on, and confirming it, is the difference between a PDP that Google and AI agents can actually read and one they silently skip.


## Two rendering paths on the same platform


**SFRA (Storefront Reference Architecture)** is the traditional, monolithic storefront: a controller (` Product-Show` ) builds a ViewModel from the B2C Commerce script API, converts it into a plain JSON object, and hands it to an ISML template via` res.render()` . ISML is a server-side tag language, similar in spirit to JSP, that's compiled to HTML on Salesforce's servers before the response ever leaves the data center. In SFRA, the price, name, description, variation attributes, and availability that appear on a PDP are baked into the HTML by the time it hits the wire. Client-side JavaScript in SFRA (jQuery-based add-to-cart, swatches, carousels) only handles interaction after the fact — it isn't what puts the product data on the page ([SFRA architecture docs](https://developer.salesforce.com/docs/commerce/sfra/guide/b2c-sfra-features-and-comps.html) ).


**Composable Storefront (PWA Kit + Managed Runtime)** is Salesforce's React-based headless option and the one receiving most current investment. It's isomorphic: the same React components render once on the server (via Managed Runtime, Salesforce's serverless SSR hosting) and then "hydrate" in the browser, at which point rendering duties hand off to the client ([PWA Kit rendering guide](https://developer.salesforce.com/docs/commerce/pwa-kit-managed-runtime/guide/rendering.html) ). Salesforce's own debugging guide is explicit that this matters for crawling: "seeing the server-side rendered version of the page helps troubleshoot issues not only with server-side rendering, but also with SEO since search engines crawl this version of the page" ([PWA Kit debugging guide](https://developer.salesforce.com/docs/commerce/pwa-kit-managed-runtime/guide/debugging.html) ). Props from API calls are serialized into the initial HTML specifically so the client doesn't have to re-fetch them — which also means, done correctly, that data is present in the raw response.


The risk isn't "React is bad for SEO." It's that PWA Kit can be deployed in ways that break the SSR guarantee: a route component that fetches product data only in a client-side effect instead of the server-side data-fetching strategy, a build that ships as a static SPA bundle outside Managed Runtime, or a hybrid rollout (SFRA for some routes, PWA Kit for others, per Salesforce's[hybrid implementation guidance](https://developer.salesforce.com/docs/commerce/pwa-kit-managed-runtime/guide/hybrid-implementation.html) ) where the PDP route quietly ends up on the client-only side of that split. Any of these leaves the initial HTML with a loading skeleton and no[product attributes](https://www.anglera.com/glossary/product-attributes) .


## Why this matters beyond Googlebot


Google runs a two-wave process: it indexes the raw HTML immediately, then queues the page for a headless Chromium render and re-indexes with whatever JavaScript produced ([Google's JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ). That second wave has a real but variable delay, and it can fail silently if the render times out or the client-side fetch depends on a cookie, region header, or auth token the renderer doesn't have. Most AI crawlers — the agents summarizing or citing your PDP for a shopping answer — don't render JavaScript at all; they read the first response. If your product name, price, and specs only appear after hydration, an SSR-only crawler sees nothing to cite.


## Getting product data into the response


**On SFRA** , this is mostly a discipline problem, not a technology problem: keep the ISML template rendering directly from the ViewModel/` pdict` , and don't move core product fields (price, availability, key specs) into a component that's populated by a client-side AJAX call after page load — a pattern some teams introduce for "dynamic pricing" widgets. If you inject Product schema, do it server-side in the same template pass, not via a client-side script tag added after load:


```text
<isscript>
var jsonLdObj = {
"@context": "https://schema.org/",
"@type": "Product",
"name": product.productName,
"sku": product.id,
"image": product.images.large[0].url.toString(),
"offers": {
"@type": "Offer",
"priceCurrency": product.price.sales.currency,
"price": product.price.sales.value,
"availability": product.available
? "https://schema.org/InStock"
: "https://schema.org/OutOfStock"
}
};
</isscript>
<script type="application/ld+json"><isprint value="${JSON.stringify(jsonLdObj)}" encoding="off"/></script>


```


That` encoding="off"` on the` <isprint>` tag is not optional decoration — ISML HTML-encodes expression output by default (it's the platform's XSS protection), which will silently mangle the quotes and braces in your[JSON-LD](https://www.anglera.com/glossary/json-ld) if you drop a bare` ${JSON.stringify(...)}` straight into the template instead.


**On Composable Storefront** , use the framework's server-side data-fetching strategy (the route's` getProps` /loader pattern, not a` useEffect` fetch) so product data is resolved before the server render completes and gets serialized into the initial payload along with the rest of the component props. If a PDP route is intentionally client-rendered for personalization reasons, make sure the non-personalized core fields — name, SKU, price, description, key specs — still ship in the SSR pass, and layer personalization on top rather than gating the whole product block behind it.


## How to validate


Don't trust what your browser shows you — DevTools has already run the JavaScript. Compare the raw response to the rendered DOM instead:


```text
# Raw server response — this is what non-JS crawlers see
curl -s -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" \
https://www.example.com/product/12345.html | grep -i "application/ld+json" -A 20


```


- **View-source vs rendered DOM** :` view-source:` on the PDP shows the raw HTML; comparing it against what you see in the regular DevTools Elements panel (post-hydration) will surface anything that only appears after JS runs.
- **Google URL Inspection Tool** (Search Console): shows the rendered HTML from Google's own Web Rendering Service, plus the raw HTTP response — the authoritative check for what Googlebot actually indexed.
- **Rich Results Test** : fetches and renders the URL through the same rendering service and validates any Product/Offer JSON-LD it finds; use URL mode, not code-paste mode, so you're testing the live rendering path.
- **curl with no user agent tricks needed for structured data** : since JSON-LD should be server-rendered either way, a plain` curl` of the page should return the full JSON-LD` script` block. If it doesn't, but the DOM shows it, your schema is being injected client-side.


## Verified as of July 2026


SFRA and Composable Storefront (PWA Kit + Managed Runtime) details reflect Salesforce's current developer documentation as of July 2026; confirm behavior against your specific SFRA version or PWA Kit release, since rendering and caching defaults have shifted across releases.


Rendering discipline only pays off if there's rich, structured product data to render in the first place — attributes, use-cases, identifiers, comparison specs, the details that turn a bare PDP into something worth indexing. That's the data side of this problem, and it's what Anglera is built to keep current continuously, sitting on top of your existing PIM or commerce platform rather than replacing it, so your SFRA or PWA Kit templates always have complete data to server-render.
