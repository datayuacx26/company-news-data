---
schema_version: "1.0.0"
document_id: "746dee09ff5d2e12342656c6773e3446adbc546cbb41b9344f1be2a956473726"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/adobe-commerce-product-json-ld"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:5f547b42b09922968b6c8e69fc2c34f03382bbf25ca0ce70e2a18fe24e94ead8"
---

# Adding Product JSON-LD on Adobe Commerce — and keeping it in sync

Adobe Commerce runs two very different storefront architectures today, and the[JSON-LD](https://www.anglera.com/glossary/json-ld) story is different on each: the traditional PaaS stack (Luma or Hyvä on top of Magento Open Source / Adobe Commerce core) ships mostly Microdata and needs a small custom module to emit JSON-LD, while the newer Adobe Commerce Storefront on Edge Delivery Services has an official metadata-generation path built around the Catalog Service GraphQL API. This guide covers both, plus the fields that actually move Rich Results eligibility and the sync issues that quietly break markup after launch.


## Which architecture are you on


Before touching code, confirm which one applies:


- **PaaS / Luma / Hyvä** — a` catalog_product_view.xml` layout, PHP blocks, and` .phtml` templates render the[product detail page](https://www.anglera.com/glossary/pdp-product-detail-page) server-side. Default themes emit schema.org Microdata (` itemtype` ,` itemprop` attributes), not JSON-LD, so JSON-LD has to be added.
- **Adobe Commerce Storefront (Edge Delivery Services / the Commerce boilerplate)** — pages are built from Catalog Service data and pre-rendered; Adobe documents a metadata-generation script and a PDP Metadata Generator tool that produce` WebSite` ,` Product` ,` AggregateRating` ,` Rating` , and` BreadcrumbList` JSON-LD directly, with output validated by lowercase-URL rules for canonical paths.


The rest of this guide focuses on the PaaS path, since that's where most retailers and distributors still need custom work, with a note on the EDS path where it differs.


## Fields that matter, and where they come from in Magento


Google's Product structured-data guidance requires` name` , plus at least one of` offers` ,` review` , or` aggregateRating` to qualify for merchant listing or product-snippet treatment. In an Adobe Commerce catalog, map them like this:


- **name** — the product's` name` attribute.
- **brand** — Adobe Commerce has no dedicated Brand entity out of the box. Most catalogs use a custom dropdown/swatch attribute (commonly` manufacturer` or a custom` brand` code); render its label as` brand.name` .
- **sku** — the product's native` sku` field.
- **[gtin](https://www.anglera.com/glossary/gtin-global-trade-item-number)** — also not native. Create a text attribute (e.g.,` gtin` ) under Stores → Attributes → Product, assign it to the relevant attribute set, and populate UPC/EAN/ISBN values there. Use the generic` gtin` property rather than` gtin12` /` gtin13` unless you're reliably distinguishing formats.
- **offers** — build from` Offer` :` price` from the product's price info (not a raw attribute — see the sync section below),` priceCurrency` from the current store's currency code,` availability` from stock/salable-quantity status, and` url` from the canonical product URL.
- **aggregateRating** — Magento's` Magento_Review` module aggregates ratings into a summary (a percentage,` rating_summary` , plus` reviews_count` ) via` Magento\\Review\\Model\\Review\\Summary` . Convert the percentage to a 1–5 scale (` rating_summary / 100 * 5` ) for` ratingValue` .


## Adding the JSON-LD block (PaaS / Luma / Hyvä)


Adobe's Frontend Developer Guide documents adding blocks to page containers with` referenceContainer` in layout XML — the same mechanism used to inject any custom markup into` head.additional` or a content container. Create a small module with three files.


Layout XML (` app/code/VendorName/ProductJsonLd/view/frontend/layout/catalog_product_view.xml` ):


```text
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
<body>
<referenceContainer name="head.additional">
<block class="VendorName\ProductJsonLd\Block\Product\JsonLd"
name="product.jsonld"
template="VendorName_ProductJsonLd::product/jsonld.phtml"
cacheable="true" />
</referenceContainer>
</body>
</page>


```


Block (` Block/Product/JsonLd.php` ) reads the current product from the registry, pulls the rating summary, and returns a JSON string. Pull price from the indexed price info rather than a raw attribute, so it matches what the storefront actually shows:


```text
$amount = $product->getPriceInfo()->getPrice('final_price')->getAmount()->getValue();


```


Template (` view/frontend/templates/product/jsonld.phtml` ):


```text
<script type="application/ld+json">
<?= /* @noEscape */ $block->getJsonLd() ?>
</script>


```


Example rendered output for a single simple product:


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "12mm Titanium Hex Bolt, Grade 5",
"sku": "TIT-HEX-12-G5",
"gtin": "00812345678905",
"brand": {
"@type": "Brand",
"name": "Ironclad Fasteners"
},
"image": [
"https://www.example.com/media/catalog/product/t/i/tit-hex-12-g5_1.jpg"
],
"description": "Grade 5 titanium hex bolt, 12mm x 40mm, DIN 933 thread.",
"offers": {
"@type": "Offer",
"url": "https://www.example.com/12mm-titanium-hex-bolt-grade-5.html",
"priceCurrency": "USD",
"price": "4.85",
"availability": "https://schema.org/InStock",
"itemCondition": "https://schema.org/NewCondition"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.6",
"reviewCount": "38"
}
}


```


For configurable products with distinct child SKUs, either emit one` Product` per matching variant using` ProductGroup` /` isVariantOf` , or (simpler, and generally sufficient for search) emit the parent's data with an` AggregateOffer` covering the price range across children.


## Adding JSON-LD on Adobe Commerce Storefront (Edge Delivery Services)


If you're on the EDS-based storefront, don't hand-roll this: Adobe's documented path is the metadata-generation script that reads product data from the Catalog Service GraphQL API and writes both meta tags and JSON-LD at build/publish time, with support for bulk metadata upload. Because EDS pages are largely pre-rendered, JSON-LD lands directly in the HTML response rather than being injected client-side — which matters for crawlers and AI agents that don't execute JavaScript.


## Keeping it in sync


This is where JSON-LD quietly drifts from the visible page:


- **Full-page cache.** If the JSON-LD block is cached as part of the full page but price or stock is rendered through an ESI/cache hole (as Magento's price block often is), a price change can update the visible page while the cached JSON-LD still shows the old value. Give the JSON-LD block real cache tags tied to the product entity (via` getIdentities()` ), or render it through the same cache-hole mechanism as price.
- **Price source.** Pull price from the product's` PriceInfo` (indexed) rather than a raw` price` attribute — Adobe Commerce computes final price through catalog rule and tier-price indexers, and reading the raw attribute will disagree with what shoppers see whenever a rule or special price is active.
- **Stock and MSI.** With Multi-Source Inventory, "in stock" is a function of salable quantity per source/website, not a single stock flag — resolve availability the same way the add-to-cart button does, or the two will disagree during partial stockouts.
- **Store view scope.** Currency, price, and even brand labels can vary by store view; generate JSON-LD per store view context, not from the default scope.
- **Review aggregation lag.** Rating summaries are recalculated by an indexer/cron, not instantly on review submission — expect a short lag between a new review and the JSON-LD reflecting it, and make sure that indexer is actually running on schedule.
- **EDS publish cadence.** Because EDS content is pre-rendered, a price or stock change is only reflected in JSON-LD after the page republishes — align publish/refresh frequency with how often your fast-moving fields (price, availability) actually change.


## How to validate


- **View source, not just inspect element.** Run` curl -s https://www.example.com/your-product.html | grep -A 40 'application/ld+json'` (or use "View Page Source" in the browser) to confirm the JSON-LD is present in the raw HTML response, not injected only after JavaScript runs.
- **Rich Results Test.** Paste the live URL into Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm the` Product` type is detected and see which rich-result eligibility (merchant listing vs. product snippet) it qualifies for.
- **Schema Markup Validator.** Cross-check with[validator.schema.org](https://validator.schema.org/) for strict schema.org conformance independent of Google's eligibility rules.
- **Spot-check against the DOM.** Compare the JSON-LD's price, availability, and rating values against what actually renders on the page for the same store view and currency — this is the check that catches cache and indexer drift.


Verified as of July 2026 against Adobe's Commerce Frontend Development guide and Adobe Commerce Storefront (Edge Delivery Services) SEO documentation, and Google's Search Central Product structured-data guidance; menu paths and attribute names can vary by Adobe Commerce version and installed extensions, so confirm against your own instance before shipping.


None of this JSON-LD is worth much if the underlying attributes — brand, gtin, accurate specs, use-case descriptions — are thin or missing in the PIM to begin with. Anglera enriches that product data continuously across attributes, identifiers, and specs, feeding whichever PIM or commerce platform you already run, so the block above has genuinely rich, current data to render rather than a handful of populated fields surrounded by nulls.
