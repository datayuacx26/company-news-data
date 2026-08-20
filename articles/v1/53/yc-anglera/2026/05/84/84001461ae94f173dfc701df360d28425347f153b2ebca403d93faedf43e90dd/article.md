---
schema_version: "1.0.0"
document_id: "84001461ae94f173dfc701df360d28425347f153b2ebca403d93faedf43e90dd"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/oracle-commerce-product-json-ld"
published_at: "2026-05-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:f15027445e0f69f21d0ea0062634e6c0678eeef2b2647fa457d09ee067f55f98"
---

# Adding Product JSON-LD on Oracle Commerce — and keeping it in sync

Oracle Commerce (the storefront platform Oracle sells as Oracle CX Commerce / Oracle Commerce Cloud, and its on-prem ATG-based predecessor) ships a built-in structured data generator, but retailers running custom widgets, headless storefronts, or older templates often need to hand-build or extend it. This guide covers both paths — using the native feature and writing your own Product[JSON-LD](https://www.anglera.com/glossary/json-ld) — plus the field mapping and sync issues that actually cause Rich Results Test failures in production.


## Two ways to add Product JSON-LD on Oracle Commerce


**Path 1 — use the built-in structured data feature.** Oracle's own documentation (Using Oracle CX Commerce guide, Manage SEO chapter, "Customize structured data" topic) confirms Commerce automatically generates JSON-LD for the homepage, product pages, and collection pages, covering` Product` ,` BreadcrumbList` ,` ItemList` ,` Review` ,` WebSite` , and` Organization` types out of the box. Commerce lets you switch off the default markup for a given page type and substitute your own script template that reads from Commerce's page variables — useful when you need product-type-specific properties (apparel size/color variants, B2B pack quantities) the default template doesn't cover.


**Path 2 — add it yourself in a widget template.** Oracle Commerce Cloud storefronts are built from widgets (a` widget.json` , a` display.template` HTML fragment, and JavaScript under` js/` ), rendered client-side with Knockout.js data-binding (` data-bind="text: $data.displayName"` and similar). If you're customizing the default` productDetails` -family widget or building a headless/PWA front end against the Commerce REST APIs, you inject a JSON-LD script block into the same template that renders the visible page, populated from the identical product/SKU JSON the page already fetched — not a second, separately-maintained copy:


```text
<script type="application/ld+json">
{
"@context": "https://schema.org",
"@type": "Product"
}
</script>


```


Either path lands in the same place: a JSON-LD script tag in the document that describes the product shown on the page.


## Which fields matter, and where they live in your catalog


Oracle Commerce's data model separates the parent **Product** repository item (marketing content,` displayName` , category placement) from child **SKU** items (` catRefId` , price, stock, size/color variant properties). Map that to schema.org like this:


- **name** — the Product's` displayName` .
- **sku** — the SKU's` catRefId` (Oracle's catalog reference ID; this is your variant/SKU identifier, not the parent` productId` ).
- **brand** — a` Brand` object with` name` . Base Oracle Commerce doesn't ship a first-class "brand" property on Product or SKU; most catalogs add it as a custom property — on the on-prem platform, via the catalog repository definition; in CX Commerce, via the Admin API's product-properties/item-type endpoints — after which it appears as an editable field on the product's details page, and gets populated from the PIM feed.
- **[gtin](https://www.anglera.com/glossary/gtin-global-trade-item-number)** — also not native. It has to be added as a custom SKU-level property (map to` gtin13` /` gtin14` depending on your barcode format, or the unified` gtin` property) and populated from UPC/EAN data upstream — this is exactly the kind of identifier field that's easy to leave blank at catalog load time.
- **offers** — an` Offer` (or` AggregateOffer` for multi-SKU products) built from the SKU's` listPrice` /active price,` priceCurrency` ,` availability` (map from Commerce's stock status), and the canonical product URL.
- **aggregateRating** —` ratingValue` and` reviewCount` , sourced from whatever review system is integrated (Oracle Commerce doesn't include a native reviews engine; this is typically Bazaarvoice, PowerReviews, or a similar add-on's aggregate feed). Omit the property entirely on pages with zero reviews — don't emit a rating of 0.


## A working example


```text
{
"@context": "https://schema.org",
"@type": "Product",
"name": "Milwaukee M18 FUEL 1/2 in. Hammer Drill/Driver Kit",
"image": [
"https://www.example-distributor.com/media/catalog/product/m18-fuel-hammer-drill.jpg"
],
"description": "Brushless hammer drill/driver kit with 2 REDLITHIUM batteries, charger, and case.",
"sku": "SKU-2704-22CT",
"gtin13": "0045242336289",
"brand": {
"@type": "Brand",
"name": "Milwaukee"
},
"offers": {
"@type": "Offer",
"url": "https://www.example-distributor.com/tools/m18-fuel-hammer-drill-2704-22ct",
"priceCurrency": "USD",
"price": "199.00",
"availability": "https://schema.org/InStock",
"itemCondition": "https://schema.org/NewCondition"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.7",
"reviewCount": "312"
}
}


```


For distributors with contract/tiered pricing where public "price" isn't meaningful, omit` offers.price` and use` offers.availability` alone, or represent price ranges with` AggregateOffer` (` lowPrice` /` highPrice` ) rather than publishing a number that doesn't match what a logged-in buyer actually pays.


## Keeping the JSON-LD in sync with the visible page


This is where most Oracle Commerce structured data quietly breaks:


- **Client-side rendering + SEO snapshots.** Oracle Commerce Cloud storefronts render primarily client-side (Knockout.js), and Commerce generates static HTML "SEO snapshots" of crawlable pages for bots, refreshed roughly every 24 hours or on publish. If your JSON-LD is injected only by client-side JavaScript after page load, confirm it's actually captured in the snapshot pipeline — otherwise the copy served to crawlers can lag price and stock changes by up to a day. Triggering a republish after a catalog price change, rather than waiting for the nightly refresh, closes that gap.
- **One data source, not two.** The most common drift is a custom JSON-LD template hard-coded with a price or availability string instead of reading the same` catRefId` /` listPrice` /stock-status variables the visible price widget uses. If a merchandiser changes price in the Commerce admin catalog and the visible page updates but the script tag doesn't, you have two sources of truth. Bind both to the same catalog fetch.
- **Variant products.** For configurable products (color/size), make sure the JSON-LD reflects the *specific SKU* the shopper is currently viewing (or use` ProductGroup` /` AggregateOffer` for the parent view), not always the first child SKU in the catalog.
- **Custom properties survive republishing.** Brand and GTIN are usually added as custom repository properties — confirm your catalog import/PIM sync actually writes to those properties on every update, not just on initial SKU creation.


## How to validate


- **View-source vs. rendered DOM** : because Oracle Commerce Cloud pages are client-rendered,` view-source:` may not show the JSON-LD if it's injected purely client-side — check the *rendered* DOM (browser DevTools → Elements panel, or` document.querySelectorAll('script\[type="application/ld+json"\]')` in the console) to confirm what actually reaches the DOM.
- **curl the URL with a bot user agent** to see what the SEO snapshot pipeline serves to crawlers specifically, since that may differ from what a logged-in browser session renders.
- **Run the page through Google's[Rich Results Test](https://search.google.com/test/rich-results)** and, in Search Console, the **URL Inspection tool's "View Crawled Page"** option, which shows the last-indexed rendered version — the one that matters for indexing (distinct from "View Tested Page," which reflects a fresh live test).
- **Watch the Structured Data report in Google Search Console** over the following days for` Product` errors/warnings (missing` price` , missing` availability` ) across the catalog, not just one page.


**Verified as of July 2026** against Oracle's current CX Commerce documentation for structured data, SEO snapshots, and widget development; on-prem/ATG-based Oracle Commerce Platform installs use the same schema.org fields but implement the script tag in JSP templates rather than Knockout widgets, so file locations and menu paths above are specific to the CX Commerce (SaaS) admin.


None of this works if` brand` and` gtin` sit blank on half your SKUs, which is the normal state of a distributor catalog fed by inconsistent supplier data. Anglera enriches those attributes continuously against your PIM or Oracle Commerce catalog directly, so whichever path above you choose to render the JSON-LD, the fields it needs are already populated rather than empty.
