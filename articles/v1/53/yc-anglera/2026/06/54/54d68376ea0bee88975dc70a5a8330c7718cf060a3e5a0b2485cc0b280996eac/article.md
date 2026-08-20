---
schema_version: "1.0.0"
document_id: "54d68376ea0bee88975dc70a5a8330c7718cf060a3e5a0b2485cc0b280996eac"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/woocommerce-ssr-rendering"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:a4f67ab7aca0adf5818b2371d786aa7bd7473d963f0f5d4899a6ace1d6834332"
---

# Server-side rendering on WooCommerce: making product data visible to Google and AI

WooCommerce renders classic product pages server-side by default, which is good news for SEO and AI visibility. But headless storefronts, page builders, and certain block or plugin setups can quietly move product data into client-side JavaScript, where it never reaches the initial HTML response. This guide covers how WooCommerce's rendering actually works, where things go wrong, and how to check.


## How a standard WooCommerce product page renders


A classic (non-headless) WooCommerce store is built on WordPress's PHP template hierarchy. For a single product, WordPress looks for a slug-specific template such as` single-product-example-widget.php` , then` single-product.php` (WooCommerce's default, found in` plugins/woocommerce/templates/` ), then falls back to` single.php` or` singular.php` . Whichever template resolves, WooCommerce's template functions (` woocommerce_template_single_title` ,` woocommerce_template_single_price` ,` woocommerce_show_product_images` ,` woocommerce_template_single_excerpt` , and so on) run on the server and write title, price, gallery, short description, attributes, and stock status directly into the HTML PHP sends to the browser. The full product description renders the same way, output via` woocommerce_output_product_data_tabs` in the tabs beneath the summary. There's no client-side fetch required to see a product's name or price — it's baked into` view-source` . This is the[template hierarchy WooCommerce's own developer docs describe](https://developer.woocommerce.com/docs/theming/theme-development/template-structure/) , and it's the reason WooCommerce has historically been a reasonably safe platform for SEO: the default rendering path is server-side.


WooCommerce also writes` Product` structured data ([JSON-LD](https://www.anglera.com/glossary/json-ld) ) into the page automatically, without a theme or plugin needed. The` WC_Structured_Data` class hooks its` generate_product_data()` method into the same` woocommerce_single_product_summary` action that renders the product summary, and outputs a JSON-LD script tag (type` application/ld+json` ) containing` name` ,` image` ,` description` ,` sku` ,` offers` (with` price` ,` priceCurrency` ,` availability` ), and seller/organization data, per the[WooCommerce structured-data documentation](https://github.com/woocommerce/woocommerce/wiki/Structured-data-for-products) . The same class separately writes breadcrumb and website-level markup via the` woocommerce_breadcrumb` and` woocommerce_before_main_content` actions. Like the rest of the page, this JSON-LD is rendered server-side and present in the raw HTML — you don't need JavaScript to see it, and you can extend or override it with the` woocommerce_structured_data_product` filter.


## Where client-side rendering creeps in


That default is easy to lose. Watch for these patterns, which are common in real WooCommerce stores:


- **Headless / decoupled storefronts.** Stores built with the WooCommerce Store API or WPGraphQL/WooGraphQL feeding a React, Vue, or plain SPA frontend often render entirely client-side unless the frontend framework is explicitly configured for server-side rendering (SSR) or static generation (SSG). A Next.js storefront using SSR or SSG will output full HTML per request or at build time; the same storefront running as a client-only single-page app will ship a near-empty HTML shell and build the product page in the browser after a data fetch. From the outside, both "look" identical to a shopper — the difference only shows up in the raw response a crawler sees.
- **Page builders and dynamic widgets.** Elementor, Divi, and similar builders sometimes render "dynamic" product data — related products, upsells, variation swatches, tabbed descriptions — via AJAX calls that fire after the initial page load rather than at PHP render time.
- **Variation data fetched on demand.** WooCommerce's built-in variable-product handling embeds all variation data (per-variation price, SKU, description, image) as a JSON blob in a` data-product_variations` attribute on the form, which JavaScript reads to update the display when a shopper picks options — this data is already in the HTML. The anti-pattern is a plugin or custom build that instead fetches variation-specific data via a live AJAX call, meaning only the parent product is in the source HTML and the specific variant a customer (or an AI agent) lands on isn't.
- **"Load more" / infinite scroll on the gallery or specs tab.** Content that renders only on scroll or click, backed by a fetch call, isn't in the initial HTML at all.


None of this breaks the shopping experience for humans in a browser. It matters because Google — and AI crawlers, most of which do not execute JavaScript at all — read the page differently than a browser does.


## Why this matters for crawlers and AI agents


Google's own guidance describes rendering as a two-wave process: Googlebot fetches raw HTML immediately and can index it right away, then a second, JavaScript-executing rendering pass happens later, with a delay that can range from seconds to days or weeks depending on rendering-queue load, per[Google Search Central's JavaScript SEO documentation](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) . Content only visible after JavaScript execution is in Wave 2 — delayed, and dependent on Google being able to execute your JS correctly at all. Most non-Google AI crawlers and agent fetchers (used for answer engines, shopping assistants, and LLM-based research tools) skip JavaScript execution entirely and only ever see Wave 1's raw HTML. If a product's price, availability,[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) , or specs only exist after a client-side fetch, those systems never see them, full stop.


## How to validate


Check the difference between what's sent and what's rendered directly:


1. **View-source vs. rendered DOM.** Open the product page, use "View Page Source" (raw HTML, no JS executed), and separately open DevTools → Elements (the live, JS-modified DOM). If price, SKU, description, or availability appear in Elements but not in View Source, that data is being added client-side.
2. **Curl the page.** Run a plain HTTP request with no JS engine and grep for the fields you care about:


```text
curl -s https://example.com/product/example-widget/ | grep -i "application/ld+json" -A 20
curl -s https://example.com/product/example-widget/ | grep -i "product_variations"


```


If your product name, price, and JSON-LD block show up in that output, they're server-rendered. If they don't, they're arriving via JavaScript.


1. **Google's Rich Results Test.** Paste the product URL into the[Rich Results Test](https://search.google.com/test/rich-results) to confirm your` Product` JSON-LD parses correctly and see exactly what Google's renderer extracts.
2. **Disable JavaScript.** In Chrome DevTools, Command Menu → "Disable JavaScript," then reload. Whatever's missing from the page at that point is invisible to most non-Google crawlers.


## Verified as of July 2026


Template hierarchy,` WC_Structured_Data` , and the` data-product_variations` mechanism reflect current WooCommerce core behavior; headless/SSR behavior depends on the specific frontend framework and its configuration, so confirm your store's actual rendering setup against the checks above rather than assuming based on platform alone.


Getting product data into server-rendered HTML only pays off if the data itself is worth serving — complete specs, accurate identifiers, real use-case detail, not a thin description and a price. That's the piece Anglera handles: it continuously enriches[product attributes](https://www.anglera.com/glossary/product-attributes) , specs, and identifiers in the systems you already use, so whichever rendering approach your WooCommerce store takes, there's substantive data to put on the page. Your PIM stores the data; Anglera does the work of keeping it accurate and complete.
