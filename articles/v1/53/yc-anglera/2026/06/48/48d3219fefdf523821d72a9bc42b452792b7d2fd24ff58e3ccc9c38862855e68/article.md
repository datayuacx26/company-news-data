---
schema_version: "1.0.0"
document_id: "48d3219fefdf523821d72a9bc42b452792b7d2fd24ff58e3ccc9c38862855e68"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/bigcommerce-agent-readable"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:704e165a8ee73ab31f70c9785466aad962eb91f87e49a18f8effc8131778806e"
---

# Making your BigCommerce catalog agent-readable (AEO)

Once your product data is enriched — attributes, specs, use-cases, identifiers filled in — the remaining question is mechanical: does it reach the HTML a buyer's browser or an AI shopping agent actually receives? On BigCommerce that comes down to three things: where the data lives (custom fields vs. metafields), whether it renders server-side, and whether it's also expressed as machine-readable Product markup.


## Where structured attributes actually live in BigCommerce


BigCommerce gives you two native places to store attribute data beyond core catalog fields, and agents treat them very differently.


**Custom fields** are simple name/value pairs attached to a product, added from the product's Custom Fields section in the admin or the[Custom Fields REST endpoint](https://developer.bigcommerce.com/docs/rest-catalog/products/custom-fields) . BigCommerce caps these at 200 fields per product and 250 characters per name or value. Cornerstone-based themes render them as a visible name/value list on the product page by default, so anything stored here is agent-visible with zero template work.


**Metafields** are the higher-capacity option — namespace/key/value triples supporting up to 65,535 characters, a better fit for enriched content than the 250-character custom field limit allows. The catch: a metafield is only visible through the[GraphQL Storefront API](https://developer.bigcommerce.com/docs/storefront/graphql) , and therefore usable on the storefront at all, if its permission is set to` read_and_sf_access` or` write_and_sf_access` . A metafield created with default (app-only) permissions can hold perfectly enriched data that never reaches the page, for a human or an agent — worth auditing directly, since it's the most common reason enrichment work quietly disappears.


Custom fields are the pragmatic default for anything short. Reach for metafields when an attribute needs more room, or structured JSON values.


## Getting enriched data server-rendered, not just stored


Storing the data correctly is necessary but not sufficient — it also has to render in the HTML the page sends. Stencil themes handle this well: every top-level template, including` product.html` , can declare a GraphQL query in its front-matter block. BigCommerce executes that query server-side before the page renders, exposing the result to Handlebars under the` gql` key — no client-side fetch required. That's the mechanism you want for metafield content, since (unlike custom fields) it isn't pulled into the template automatically.


A front-matter query for spec metafields looks roughly like this (consult BigCommerce's[Front Matter Reference](https://developer.bigcommerce.com/docs/storefront/stencil/themes/context/frontmatter) for exact syntax):


```text
---
graphql:
queries:
productSpecs: |
query ProductSpecs($productId: Int!) {
site {
product(entityId: $productId) {
metafields(namespace: "specs", first: 20) {
edges {
node {
key
value
}
}
}
}
}
}
---


```


Then in the template body:


```text
{{#each gql.productSpecs.data.site.product.metafields.edges}}
<tr>
<td>{{this.node.key}}</td>
<td>{{this.node.value}}</td>
</tr>
{{/each}}


```


Because this runs server-side, the spec table is present in the raw HTML response — the same payload a non-JS-executing crawler or agent fetches. Attribute data injected only by a client-side widget doesn't have that property: if the agent doesn't execute JavaScript, the content simply doesn't exist as far as it's concerned. "Does this render server-side" is the question that matters most for agent readability.


BigCommerce's local development tooling also supports appending` ?debug=context` to a storefront URL, dumping the full page context as JSON — useful for confirming a metafield or custom field made it through before troubleshooting the template itself.


## Product JSON-LD: what's there by default, and what's missing


Cornerstone and most Stencil themes ship with basic` Product` structured data, but it's often a partial set — name, image, and a price/offer, without brand,[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) /MPN, or aggregate rating. Per Google's[Product structured data guidelines](https://developers.google.com/search/docs/appearance/structured-data/product) , the required minimum is` name` plus at least one of` review` ,` aggregateRating` , or` offers` — the complete, agent-useful version includes identifiers and availability too:


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "18-Gauge Cordless Brad Nailer",
"sku": "BN18-450",
"mpn": "450-CBN",
"gtin13": "0761023456789",
"brand": { "@type": "Brand", "name": "Anglera Tools" },
"description": "Cordless 18-gauge brad nailer rated for 450 shots per charge; compatible with 5/8\" to 2\" fasteners.",
"image": ["https://example.com/images/bn18-450-1.jpg"],
"offers": {
"@type": "Offer",
"url": "https://example.com/18-gauge-brad-nailer/",
"priceCurrency": "USD",
"price": 189.00,
"availability": "https://schema.org/InStock",
"itemCondition": "https://schema.org/NewCondition"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": 4.6,
"reviewCount": 112
}
}


```


Two practical notes: check for duplication before adding a script — BigCommerce apps (schema plugins, review widgets, some SEO apps) often inject their own` Product` block, and two conflicting[JSON-LD](https://www.anglera.com/glossary/json-ld) payloads on one page can cause a crawler to distrust or ignore both. And structured data must describe what's actually visible on the page — don't mark up a price, rating, or spec value that isn't also in the rendered content buyers see.


If your catalog has size/color/configuration variants, use` ProductGroup` rather than repeating near-duplicate` Product` blocks: mark the parent with` variesBy` (e.g.` https://schema.org/size` ) and a` productGroupID` , then give each variant its own` sku` /` gtin` and` inProductGroupWithID` back to the parent, per Google's[product variant guidance](https://developers.google.com/search/docs/appearance/structured-data/product-variants) . Distributors with large option sets benefit most — without it, an agent can't tell twelve SKUs are one product in twelve sizes rather than twelve unrelated items.


## Answering the buyer's actual question


JSON-LD tells an agent what a product is; visible page content tells it whether the product fits the buyer's need. Make sure real answers — compatibility, minimum order quantity, certifications — live as server-rendered text (a spec table, an FAQ section, a comparison note), not inside a PDF spec sheet, a product image, or an accordion whose content only appears after a click with no fallback markup. An agent parsing HTML can read a rendered spec table; it generally cannot open a linked PDF or run OCR on a photo.


## What an agent can and cannot extract


**Can extract** , if wired up correctly:


- Name, identifiers (SKU/GTIN/MPN), brand, price, availability, and rating from JSON-LD.
- Custom field name/value pairs, rendered by default on most Stencil themes.
- Metafield content, but only when its permission includes storefront access *and* a front-matter query pulls it into the template.
- Variant distinctions (size, color, configuration) marked up with` ProductGroup` .


**Cannot extract** , even with rich data in your PIM:


- Metafields left at default (non-storefront) permission, invisible to the Storefront API regardless of how well-enriched the data is.
- Anything injected purely by client-side JavaScript, for agents that don't execute JS.
- Data that never left the PIM — enrichment never pushed into a BigCommerce field.
- Content trapped in a linked PDF, image, or video with no text equivalent on the page.
- Duplicate or conflicting JSON-LD blocks from a theme and an app both writing` Product` schema.


## How to validate


- **View-source vs. rendered DOM** : use your browser's View Page Source command, or run` curl -s https://yourstore.com/your-product/ | grep -A 40 'application/ld+json'` , to see what a non-JS crawler receives. If content appears in the browser's Inspector but not in view-source or curl, it was added client-side and isn't reliably agent-readable.
- **Rich Results Test** : run the live product URL through Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm the` Product` /` Offer` /` ProductGroup` markup parses and flags any missing fields.
- **Context check** : on a Stencil dev/preview URL, append` ?debug=context` to confirm a custom field or metafield is present in the page context before troubleshooting the template.
- **Permission check** : for any metafield you expect on the storefront, confirm its permission is` read_and_sf_access` or` write_and_sf_access` — the most common silent failure point.


Verified as of July 2026 against BigCommerce's Stencil, GraphQL Storefront API, and Custom Fields documentation, and Google's Search Central guidelines. Menu paths and front-matter syntax can vary by theme version — confirm against your store's admin and theme docs.


This whole exercise assumes the attribute is already sitting somewhere in BigCommerce, correctly filled in — that's the part Anglera handles continuously, pulling specs, use-cases, and identifiers from spec sheets and manufacturer sites so the field or metafield is never empty. Your PIM (or BigCommerce itself) stores the data; Anglera does the enrichment work upstream, so there's something complete to render once it reaches the page.
