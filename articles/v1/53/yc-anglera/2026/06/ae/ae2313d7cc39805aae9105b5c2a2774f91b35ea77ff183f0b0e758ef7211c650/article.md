---
schema_version: "1.0.0"
document_id: "ae2313d7cc39805aae9105b5c2a2774f91b35ea77ff183f0b0e758ef7211c650"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/orocommerce-ssr-rendering"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:844078e463187a5fbc65c85741504f2b47e48084fa0d40336a5e78ffed393390"
---

# Server-side rendering on OroCommerce: making product data visible to Google and AI

OroCommerce ships with a server-rendered storefront out of the box, which is good news for distributors worried about crawlability. But two common changes — a headless/PWA frontend bolted on for a custom buyer experience, and price or availability blocks that are cached and swapped client-side — can quietly pull product data out of the HTML a crawler or AI agent actually receives. This guide covers how OroCommerce renders product pages today, where the gaps show up, and how to check your own storefront in five minutes.


## How OroCommerce renders product pages by default


OroCommerce's storefront is built on the Oro Layout component, which composes a page from layout blocks and renders them through Twig — the same server-side templating approach used across the platform's back-office and storefront. According to Oro's own frontend documentation, the storefront "is not using the SPA approach," and pages are assembled from layout blocks mapped to Twig block themes at request time. JavaScript (built on a Chaplin/Backbone module system) handles interactivity — accordions, sliders, quantity steppers, the mini-cart — not the initial delivery of product content.


A product view page is itself a layout with named containers, including` product_view_main_container` ,` product_view_description_container` , and` product_specification_container` , each populated by data providers that pull product name, SKU, attributes, and description directly into the server response. In the unmodified storefront, this means a` curl` request and a browser's rendered DOM should return substantially the same product content, because there's no client-side data fetch standing between the response and the page.


Two features are relevant to what's actually in that server HTML:


- **SEO meta fields.** The OroSEOBundle extends the Product entity with Meta Title, Meta Description, and Meta Keywords fields (stored as localized values per locale) and writes them into the page's title tag and meta description tag at render time.
- **Schema.org microdata.** OroCommerce embeds Schema.org Microdata (` itemprop` attributes in the HTML, not[JSON-LD](https://www.anglera.com/glossary/json-ld) ) into the product template. Per-website settings under System, then Websites, then your website, then Commerce, Guests, SEO control which field feeds the Schema.org description (` Used Product Description Field` , choosing between the long description, SEO meta description, or short description) and whether to suppress microdata on products without an assigned price (` Disable Product Microdata Without Price` ) — useful if guest/anonymous price lists aren't configured for a given catalog.


## Where product data can end up client-only


The risk isn't the default storefront — it's what gets layered on top of it.


**Headless and PWA frontends.** OroCommerce's REST/GraphQL Web API is commonly used to power a decoupled React or Next.js storefront (Oro's own extension directory lists headless-storefront options, and this pattern shows up in agency-built PWAs). If that frontend fetches product data client-side after an empty (or app-shell) HTML response — rather than using SSR or static generation — crawlers and AI agents that don't execute JavaScript will see a near-blank page regardless of how rich the underlying catalog is. This is worth auditing explicitly if your team, or an implementation partner, has replaced the native Twig storefront with a separate frontend application.


**Render caching with live price substitution.** OroCommerce's storefront render-cache layer lets you cache an entire product block "forever" (` cache: true` ) while carving out sub-blocks — most often price — that are excluded from the cache and refreshed on each request or via a follow-up call (` maxAge: 0` on the price sub-block, or an anonymous-only condition such as` if: '=!context\["is_logged_in"\]'` ). This is normally implemented as a fragment re-render at request time, not a pure client-side AJAX call, so it typically still lands in the HTML response before it's sent — but if a custom theme or extension implements the price refresh as a browser-side fetch instead (common when price lists are customer- or contract-specific and teams want to avoid caching authenticated pricing), the number a crawler sees in view-source can differ from, or be entirely absent compared to, what a logged-in buyer sees. Confirm which pattern your theme uses before assuming price is server-rendered for guests.


**Custom product widgets.** Configurable-product variant selectors, spec-sheet accordions, and comparison widgets are frequently built as Underscore.js templates rendered by JS components after page load. If attribute or variant data is fetched via AJAX into one of these widgets rather than passed into the initial layout context, that data won't appear in the raw HTML even though it's visible on screen.


## Getting enriched product data onto the page


If your PIM or Anglera-enriched attributes (use-cases, compatibility, expanded specs, identifiers) live in custom[product attributes](https://www.anglera.com/glossary/product-attributes) , the practical goal is making sure those attributes are wired into the product view layout as server-rendered blocks, not JS-fetched widgets:


- Confirm the attribute is added to the relevant attribute group and is set to show on the storefront product page (Attribute Family / Product Attributes configuration in the back-office).
- If a custom layout update is needed, add the attribute's data provider output into a Twig block inside` product_specification_container` (or a custom container) rather than a JS template — this is what determines whether it survives to the server response.
- For the description that feeds Schema.org microdata, make sure the enriched long-form description is the one selected in` Used Product Description Field` , not a thin default.


## How to validate


Check what a non-JavaScript client actually receives, then compare it to the rendered page:


```text
# Raw server response — this is what most crawlers and AI agents see
curl -s -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" \
https://your-store.example.com/product/12345 | grep -i "itemprop\|meta name=\"description\""


```


- **View-source vs. rendered DOM** : In Chrome, compare` view-source:` on the product URL against the DOM in DevTools' Elements panel after the page finishes loading. If the price, description, or key specs appear in Elements but not in view-source, that content is being injected client-side.
- **Microdata check** : Search the curl output (or view-source) for` itemtype="https://schema.org/Product"` and nested` itemprop` attributes for` name` ,` description` ,` sku` ,` offers` , and` price` . If a product has no price and` Disable Product Microdata Without Price` is enabled, expect the Product microdata block to be absent by design — verify that's intentional for that catalog.
- **Google's Rich Results Test** : Run the live product URL through[Google's Rich Results Test](https://search.google.com/test/rich-results) to confirm the Product markup parses and which properties (price, availability, ratings) it detects.
- **Headless/PWA setups** : If a decoupled frontend is in play, repeat the curl check against the storefront domain (not the OroCommerce API endpoint) to confirm the framework is doing SSR/SSG and not shipping an empty shell.


## Verified as of July 2026


Details above reflect current OroCommerce documentation on the Oro Layout/Twig rendering model, SEOBundle meta fields, Schema.org microdata settings, and storefront render caching. Field names and menu paths can shift slightly between OroCommerce versions and community/enterprise editions — confirm against your installed version's docs before scripting a layout override.


Once the page side is rendering correctly, the harder problem is having enough good data to put there. Anglera plugs into whatever PIM or commerce platform already stores your product data — OroCommerce included — and continuously enriches attributes, specs, and use-cases so those server-rendered blocks have something substantive to show, without requiring a re-platform.
