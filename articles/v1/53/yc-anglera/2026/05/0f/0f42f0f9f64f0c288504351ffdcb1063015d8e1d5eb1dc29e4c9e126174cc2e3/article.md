---
schema_version: "1.0.0"
document_id: "0f42f0f9f64f0c288504351ffdcb1063015d8e1d5eb1dc29e4c9e126174cc2e3"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/salesforce-commerce-cloud-product-json-ld"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:88010c69755f111f0abb51160c1b20a004f29301dd7b097682813fe5eb99301f"
---

# Adding Product JSON-LD on Salesforce Commerce Cloud — and keeping it in sync

Salesforce B2C Commerce Cloud (SFCC) storefronts built on the Storefront Reference Architecture (SFRA) aren't starting from zero here. The base cartridge already computes a basic schema.org Product object for every PDP, built by a helper script and threaded through the product controller into the page's render context. The catch is that the out-of-the-box version is thin: it reuses the internal product ID for both` sku` and` mpn` , wraps brand as a generic` Thing` instead of a proper` Brand` , skips` gtin` and` aggregateRating` entirely, and only distinguishes preorder from in-stock/out-of-stock. This guide covers what's already there, how to check whether your storefront prints it, and how to extend it with the` dw.catalog.Product` API that already powers the rest of the PDP, instead of building a second, disconnected[JSON-LD](https://www.anglera.com/glossary/json-ld) pipeline.


## What SFRA already builds, and where it lives


On the` Product-Show` route (in` app_storefront_base/cartridge/controllers/Product.js` , or your cartridge's override of it), the base implementation calls a product page helper that returns, among other things, a` schemaData` object — built by` getProductSchema()` in` scripts/helpers/structuredDataHelper.js` — and passes it straight into the template's render context alongside` product` ,` breadcrumbs` , and` canonicalUrl` .


Whether that object actually shows up as a` <script type="application/ld+json">` block on the rendered page depends on your version and template customizations — some implementations print it, some don't wire the print statement in at all. So the first real step isn't writing code; it's checking view-source on a live PDP for an existing` application/ld+json` block and comparing its fields against what you actually want shoppers and AI crawlers to see.


If it's missing, thin, or wrong, the fix is to extend the existing helper rather than reinvent the controller wiring:


1. In a custom cartridge positioned ahead of` app_storefront_base` on your site's cartridge path, create a file at the same path:` cartridge/scripts/helpers/structuredDataHelper.js` .
2. Inside it, use` module.superModule` to pull in the base implementation, call it to get the thin schema, then enrich the result with fields the base helper doesn't populate.
3. Re-export the enriched function alongside the rest of the base module's exports so nothing else that depends on this helper (for example, category/listing-page schema) breaks.
4. If your PDP template doesn't already print` schemaData` as JSON-LD, add a small ISML partial that does` JSON.stringify()` on it inside a script tag, included from` productDetail.isml` .


```text
// cartridge/scripts/helpers/structuredDataHelper.js (override — same path, in a
// cartridge that sits ahead of app_storefront_base on the cartridge path)
var base = module.superModule;
var ProductMgr = require('dw/catalog/ProductMgr');


var AVAILABILITY_MAP = {
IN_STOCK: 'https://schema.org/InStock',
BACKORDER: 'https://schema.org/BackOrder',
PREORDER: 'https://schema.org/PreOrder',
NOT_AVAILABLE: 'https://schema.org/OutOfStock'
};


function getProductSchema(product) {
var schema = base.getProductSchema(product);
var apiProduct = ProductMgr.getProduct(product.id);


if (!apiProduct) {
return schema;
}


// The base helper reuses the product ID for both sku and mpn; replace mpn
// with the manufacturer's own part number when the catalog actually has one.
var manufacturerSKU = apiProduct.getManufacturerSKU();
if (manufacturerSKU) {
schema.mpn = manufacturerSKU;
}


// Base helper types brand as a generic Thing; Google's guidelines expect Brand.
if (apiProduct.getBrand()) {
schema.brand = { '@type': 'Brand', name: apiProduct.getBrand() };
}


var upc = apiProduct.getUPC();
var ean = apiProduct.getEAN();
if (upc) schema.gtin12 = upc;
if (ean) schema.gtin13 = ean;


var availabilityModel = apiProduct.getAvailabilityModel();
if (availabilityModel) {
var status = availabilityModel.getAvailabilityStatus();
schema.offers.availability = AVAILABILITY_MAP[status] || schema.offers.availability;
}


return schema;
}


module.exports = base;
module.exports.getProductSchema = getProductSchema;


```


Reading straight off the catalog API here, instead of hand-copying values already sitting in the page's view model, is what keeps this from drifting out of sync later.


## Which fields matter, and where they come from in SFCC


Schema.org field SFCC source Notes


` name` /` description` Base helper (from the product view model) Already populated out-of-the-box; usually fine as-is.


` sku` Base helper (` product.id` ) Fine to leave — it's the B2C Commerce product ID, your retailer-assigned SKU.


` mpn`` Product.getManufacturerSKU()` Base helper defaults this to the same value as` sku` ; override with the real manufacturer part number when your PIM carries one.


` gtin12` /` gtin13`` Product.getUPC()` (12-digit UPC) and` Product.getEAN()` (13-digit EAN) Not populated by the base helper. Emit only the property matching the digit length you have; Google also accepts the generic` gtin` property.


` brand`` Product.getBrand()` Base helper wraps this as a generic` Thing` ; override to` Brand` with a nested` name` , which is what Google expects.


` offers.price` /` priceCurrency`` Product.getPriceModel().getPrice()` (a` dw.value.Money` , with` .available` ,` .currencyCode` ,` .decimalValue` ) Use the price for the customer group/promotion actually displayed, not list price, if they differ.


` offers.availability`` Product.getAvailabilityModel().getAvailabilityStatus()` Base helper only distinguishes preorder from in-stock/out-of-stock. Map all four SFCC constants (` AVAILABILITY_STATUS_IN_STOCK` ,` _BACKORDER` ,` _PREORDER` ,` _NOT_AVAILABLE` ) to` schema.org`` ItemAvailability` values.


` aggregateRating` /` review` Your reviews integration (Bazaarvoice, PowerReviews, Yotpo, etc.), not a native SFCC attribute or the base helper B2C Commerce doesn't store ratings on` Product` . Pull` ratingValue` and` reviewCount` from whichever service renders the visible star widget so the two never disagree.


Only one of` offers` ,` review` , or` aggregateRating` is strictly required for Google's Product structured data to be eligible for rich results, but Google explicitly recommends including all three when you have them, plus` description` ,` sku` ,` gtin` , and` brand` , per its[merchant listing structured data guidelines](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing) .


## A real JSON-LD example


Output after the override above runs, via whichever partial prints` schemaData` on your PDP template:


```text
<script type="application/ld+json">
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "Merino Wool Crew Socks - 3 Pack",
"sku": "701643328912M",
"gtin12": "190123456789",
"mpn": "MWS-CREW-3PK-M",
"brand": {
"@type": "Brand",
"name": "Alpine Trail"
},
"image": ["https://www.example.com/dw/image/v2/AAAA_PRD/on/demandware.static/-/Sites-master-catalog/default/dw12345/images/large/701643328912M.jpg"],
"description": "Cushioned merino wool crew socks built for all-day comfort on the trail.",
"offers": {
"@type": "Offer",
"url": "https://www.example.com/s/Example/merino-wool-crew-socks/701643328912M.html",
"priceCurrency": "USD",
"price": "24.00",
"availability": "https://schema.org/InStock"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.6",
"reviewCount": "212"
}
}
</script>


```


## Keeping the JSON-LD in sync with the visible page


The most common failure mode isn't a missing field — it's drift: the markup says "in stock" while the page shows a backorder message, or the JSON-LD price doesn't match an active promotion. A few practices prevent this:


- **Extend the base helper instead of building a parallel one.** Your override runs inside the same` schemaData` pipeline the controller already populates, so it inherits the same page-cache and CDN invalidation behavior as the rest of the PDP, instead of becoming a second code path someone forgets to update.
- **Read prices and availability from the API at render time** , not from values already serialized elsewhere on the page, so a price-book or inventory refresh updates both the visible price and the JSON-LD together.
- **Reuse the reviews widget's own data source** for` aggregateRating` rather than a separately-synced copy, so a stale cache on one can't disagree with the other.
- **Guard against nulls.** Variant-heavy PDPs mean` getUPC()` ,` getBrand()` , or` getEAN()` can return` null` at the master level. Strip undefined keys before serializing rather than emitting` "gtin12": null` , which some validators flag.


## How to validate


- **Check what's already there first.** View-source (or` curl` ) a live PDP and look for an existing` application/ld+json` block before assuming you're starting from nothing — SFRA's base helper may already be emitting a thin version.
- **View-source vs. rendered DOM** : since this markup is added server-side in ISML,` curl` and "View Page Source" should match the rendered DOM exactly. If they differ, something client-side is mutating or duplicating it — check for a second copy added by a tag manager or a reviews widget.
- **` curl` spot-check** :


```text
curl -s "https://www.example.com/s/Example/merino-wool-crew-socks/701643328912M.html" \
| grep -A 30 'application/ld+json'


```


- **Google's Rich Results Test** : paste the URL or the raw code into[search.google.com/test/rich-results](https://search.google.com/test/rich-results) to confirm the Product type is detected and see which properties are read as required vs. recommended.
- **Schema.org validator** :[validator.schema.org](https://validator.schema.org/) is useful for a stricter, vendor-neutral check of the JSON-LD syntax itself, separate from Google's rich-result eligibility rules.
- Spot-check PDPs across categories (a plain product, a variant master, an out-of-stock item) — these are where null fields or availability mapping bugs tend to show up first.


Verified as of July 2026 against Salesforce's[SFRA developer guide](https://developer.salesforce.com/docs/commerce/sfra/guide/b2c-sfra-features-and-comps.html) , the[B2C Commerce Script API reference](https://salesforcecommercecloud.github.io/b2c-dev-doc/docs/current/scriptapi/html/api/class_dw_catalog_Product.html) , and Google's[merchant listing structured data documentation](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing) . Note: Salesforce's separate "structured data for meta tags" pilot applies to B2B Commerce's LWR-based storefronts, not B2C Commerce/SFRA, so extending the base helper above remains the standard path for SFCC.


None of this works if the underlying fields are thin — a` brand` that's blank, a` gtin` no one ever populated, a description that's one generic sentence copied across a thousand SKUs. That's the half of the problem Anglera is built for: it enriches[product attributes](https://www.anglera.com/glossary/product-attributes) , identifiers, and use-case detail directly in your PIM or product catalog on an ongoing basis, so the fields this guide maps into JSON-LD (` brand` ,` gtin` ,` mpn` ,` description` ) are actually populated and current. Anglera plugs into your existing PIM or commerce platform rather than replacing it — your PIM still stores the data, Anglera keeps it enriched, and the override above is what puts it in front of buyers and AI agents alike.
