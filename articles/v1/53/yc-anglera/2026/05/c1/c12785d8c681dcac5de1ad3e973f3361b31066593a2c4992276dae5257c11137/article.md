---
schema_version: "1.0.0"
document_id: "c12785d8c681dcac5de1ad3e973f3361b31066593a2c4992276dae5257c11137"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/salsify-agent-readable"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:680fd8bccf6b9f62b81c4835b8fb92a89db351a4201c5a0ae62aa68e8ae524f9"
---

# Making Salsify-managed catalogs agent-readable

Salsify is good at what it's built for: centralizing product content, enforcing governance, and pushing it out to channels through syndication and connector apps. But "synced to Shopify" and "readable by an AI shopping agent" are not the same thing — an agent parsing a PDP is looking for machine-verifiable facts (identifiers, structured attributes, availability), not just well-formatted HTML. This guide covers the handoff: how to make sure what leaves Salsify arrives on the page as attributes and identifiers an agent can actually use, and how to render that into valid Product structured data.


## Where Salsify data lands on the page


Salsify records reach a storefront through one of three paths, and each has a different failure mode for agent-readability:


- **Native commerce connectors** (Shopify, BigCommerce): Salsify's apps map product properties and digital assets into the platform's product, variant, and metafield structure via that platform's own admin API — Shopify's connector runs on its GraphQL Admin API; BigCommerce's is mainly REST-based. An attribute that isn't mapped to a real field or metafield never reaches the theme layer — it just sits in Salsify.
- **Templated syndication exports** : a retailer- or channel-specific spreadsheet/feed template that Salsify populates from its data model. This is the classic PIM export path, but it's built for retailer ingestion forms, not your own PDP.
- **Direct API pull** : your storefront (headless or custom) reads product and digital-asset records straight from the Products API and renders them itself. This gives the most control over what a crawler or agent sees in the rendered DOM, but nothing is enforced for you — a missing[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) in Salsify is a missing GTIN on the page.


In all three cases, the fix is the same: get the attribute complete and correctly typed in Salsify first, confirm the mapping actually reaches the target field, then render it as both visible content and structured data.


## Complete the structured attributes before they leave Salsify


Salsify records are JSON objects made of properties; each property has a declared type — string, number, boolean, date, enumerated (a value drawn from a referential/[controlled vocabulary](https://www.anglera.com/glossary/controlled-vocabulary) ), rich_text, html, link, or digital_asset (a link to a managed media object) — plus system properties like` salsify:id` ,` salsify:parent_id` , and` salsify:digital_assets` . The type matters more than most teams treat it: "material" or "color" stored as free-text` rich_text` is much harder to render as a clean` additionalProperty` or spec row than the same value stored as an` enumerated` property tied to a referential, which guarantees one consistent code instead of ten spellings of "stainless steel."


Two things worth auditing in your Salsify org before you touch templates:


1. **Coverage, not just presence.** A property can exist on the attribute set and still be null on much of the catalog. Use Salsify's completeness/readiness reporting, or a saved view filtered on the properties your PDP template renders, to find gaps before they show up as blank spec rows on the live page.
2. **Parent/child inheritance.** In organizations with parent/child relationships enabled, a child record can inherit property values from its parent (tracked via` salsify:parent_id` ), with precedence rules deciding which value surfaces. If your export or connector's change-detection only watches the child record's own update signal, a bulk edit to a shared parent attribute (a safety certification, a care instruction) can propagate slowly, or not at all, to variants that inherit it. Confirm with your Salsify admin exactly what your sync job watches, not just what it exports.


## Get real identifiers into the record


Product structured data — and most agent/shopping-assistant matching logic — leans on identifiers, not on product names. At minimum, each Salsify product record that maps to a sellable page needs:


- A **GTIN/UPC/EAN** in a dedicated, correctly typed field (not embedded in a description string). Salsify validates and normalizes GTIN-family identifiers, and its[GDSN](https://www.anglera.com/glossary/gdsn-global-data-synchronization-network) Data Pool integration can generate and validate GTINs against GS1/market requirements if your brand doesn't already have them assigned.
- An **MPN** ([manufacturer part number](https://www.anglera.com/glossary/mpn-manufacturer-part-number) ) where a GTIN doesn't exist or doesn't apply (e.g., B2B or made-to-order lines).
- A stable **SKU** , distinct from` salsify:id` (which is your internal record key and not meant to be customer- or agent-facing).
- A **brand** property, ideally standardized so "Acme Inc.", "ACME", and "Acme" don't all appear across the catalog.


If you sell through retailers as well as your own site, Salsify's Universal Properties mapping (currently strongest for grocery/CPG retailers such as Kroger) can keep these identifiers consistent across channel-specific export templates — but that only helps the retailer feed. Your own PDP still needs the identifier rendered into the page, which brings us to structured data.


## Render Product JSON-LD from the Salsify fields


Once the attributes and identifiers are complete in Salsify, map them to` schema.org/Product` at template render time. Google splits this into two markup types with different required fields: **Product snippets** (informational pages) require only` name` plus one of` review` ,` aggregateRating` , or` offers` ; **Merchant listings** (pages where the item can be bought) require` name` ,` image` , and an` offers` object with` price` greater than zero and a valid ISO 4217` priceCurrency` . Treat most brand PDPs syndicated through Salsify as merchant listings.


A template pulling from Salsify-backed fields should emit something like this:


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "{{ product.name }}",
"description": "{{ product.long_description }}",
"sku": "{{ product.sku }}",
"gtin13": "{{ product.gtin }}",
"mpn": "{{ product.mpn }}",
"brand": {
"@type": "Brand",
"name": "{{ product.brand }}"
},
"image": [
"{{ product.primary_image_url }}"
],
"additionalProperty": [
{
"@type": "PropertyValue",
"name": "Material",
"value": "{{ product.material }}"
},
{
"@type": "PropertyValue",
"name": "Certification",
"value": "{{ product.certification }}"
}
],
"offers": {
"@type": "Offer",
"url": "{{ canonical_url }}",
"priceCurrency": "USD",
"price": "{{ product.price }}",
"availability": "https://schema.org/InStock",
"itemCondition": "https://schema.org/NewCondition"
}
}


```


Two mapping details matter more than they look:


- ` additionalProperty` is where your Salsify structured attributes (materials, dimensions, certifications, compatibility) should land — not buried in a prose description. This is the block an agent extracts most reliably, and it's a direct, mechanical mapping from enumerated/typed Salsify properties.
- For parent/child variant families, use` hasVariant` on a` ProductGroup` (or` isVariantOf` on each child` Product` ) so the variant's own GTIN/SKU/price stays distinct from the inherited parent attributes — otherwise agents can't tell which specific variant a price or identifier belongs to.


Where this[JSON-LD](https://www.anglera.com/glossary/json-ld) gets injected depends on the platform: in a modern (Online Store 2.0) Shopify theme it belongs in a Liquid snippet rendered from the product section (e.g.,` main-product.liquid` ), not pasted into` theme.liquid` , since that file loads on every page; in a headless storefront it's emitted server-side alongside the rendered HTML, not injected client-side after hydration, since not every crawler or agent executes JavaScript.


## How to validate


- **View-source vs. rendered DOM** : run` curl -s https://yourdomain.com/products/example | grep -A 40 'application/ld+json'` to confirm the JSON-LD is in the raw HTML response, not only after client-side JavaScript runs. If it's missing from` curl` output but visible in dev tools, it's client-side injected and many agents won't see it.
- **Rich Results Test** : paste the live URL into Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm the markup parses and see which fields Google flags as missing.
- **Search Console** : once indexed, the Merchant listings report surfaces field-level warnings (missing` gtin` , invalid` priceCurrency` ) site-wide, faster than checking pages one at a time.
- **Spot-check a variant family** : pull one parent and two child records via the Products API and confirm each variant's rendered JSON-LD has distinct` sku` /` gtin` /` price` , not three copies of the parent's.


Verified as of July 2026 against Salsify's public developer documentation and Google Search Central's structured data guidelines; both are subject to change, and some Salsify capabilities (e.g., Universal Properties channel coverage) are rolling out incrementally by retailer.


None of this replaces the work of actually having complete, accurate attributes to put in these fields — that's the harder problem, and it's the one Anglera is built for. Anglera continuously enriches product data (attributes, specs, use-cases, identifiers) directly in the systems you already run, including PIMs like Salsify, so the templates and JSON-LD mappings above have real, complete data to render instead of gaps. Your PIM stores the data; Anglera does the work of keeping it accurate and complete.
