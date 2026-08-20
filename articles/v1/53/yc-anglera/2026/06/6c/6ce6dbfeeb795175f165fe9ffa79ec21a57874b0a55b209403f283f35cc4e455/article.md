---
schema_version: "1.0.0"
document_id: "6ce6dbfeeb795175f165fe9ffa79ec21a57874b0a55b209403f283f35cc4e455"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/commercetools-data-to-page"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:e2c2216fc23b16e30b2e94d4986a4f799ab204c45e5d03d494689afa6d4193c8"
---

# Getting enriched product data onto commercetools product pages

commercetools is API-first: there is no built-in theme layer, so an enriched attribute only becomes visible once your storefront explicitly fetches and renders it. This guide traces one attribute end to end — from where it's defined in the data model, through the Product Projections or GraphQL API, into a storefront component, and finally into HTML a shopper (and a crawler) can actually read.


## Where the attribute actually lives


In commercetools, every Product is an instance of a Product Type, and the Product Type is what defines the schema of custom Attributes available on that product — things like` material` ,` care_instructions` , or` warranty_years` . Attributes can be scoped at the Product level (shared across all variants) or the Variant level (e.g.,` color` ,` size` ), and each has an` attributeConstraint` such as` SameForAll` ,` Unique` , or` CombinationUnique` . This structure is documented in the[Product Types HTTP API reference](https://docs.commercetools.com/api/projects/productTypes) .


Products also maintain two parallel representations: **staged** (the draft your team or an integration is editing) and **current** (the published version shown to shoppers). A product can have` hasStagedChanges: true` while its live storefront page still shows the old value — a common reason an enriched attribute looks "missing" even though it saved correctly. See the[Product catalog overview](https://docs.commercetools.com/api/product-catalog-overview) for the staged/current model.


## Step 1: Define or confirm the attribute definition


If the attribute doesn't exist yet on the relevant Product Type, add it in Merchant Center under **Settings > Product types and attributes > \[your product type\] > Add attribute** , setting the attribute identifier, label, level (Product or Variant), type, and whether it's` Searchable` . The same change can be made via the API using an` addAttributeDefinition` update action:


```text
{
"version": 4,
"actions": [
{
"action": "addAttributeDefinition",
"attribute": {
"type": { "name": "text" },
"name": "care_instructions",
"label": { "en": "Care instructions" },
"isSearchable": false,
"attributeConstraint": "SameForAll",
"inputHint": "MultiLine",
"isRequired": false,
"level": "Product"
}
}
]
}


```


Once defined, every product of that type accepts a value for` care_instructions` , whether the value is written by hand, pushed from a PIM, or written by an enrichment pipeline via the Products API.


## Step 2: Read the value back through the API


commercetools ships two read paths storefronts commonly use for product detail pages: the **Product Projections API** (strongly consistent, ideal for a single PDP fetch) and the **GraphQL API** (flexible field selection, good for composing a page query in one round trip). Both are documented under[Product Projections](https://docs.commercetools.com/api/projects/productProjections) and the[GraphQL API reference](https://docs.commercetools.com/api/graphql) .


REST:


```text
curl --get "https://api.{region}.commercetools.com/{projectKey}/product-projections/{id}" \
--header "Authorization: Bearer ${BEARER_TOKEN}"


```


Because` care_instructions` was defined at the **Product** level (not the Variant level), it comes back in a top-level` attributes` array on the projection, separate from the variant-specific data in` masterVariant` :


```text
{
"id": "080feded-4f74-4d31-9309-f7ef6b7f1279",
"attributes": [
{ "name": "care_instructions", "value": "Machine wash cold, tumble dry low." }
],
"masterVariant": {
"id": 1,
"attributes": []
},
"published": true,
"hasStagedChanges": false
}


```


If the attribute were defined at the Variant level instead (like` color` or` size` ), it would show up inside` masterVariant.attributes` (or the relevant entry in` variants` ) rather than at the top.


GraphQL, using` attributesRaw` to get name/value pairs regardless of type — query it at the top level for Product-level attributes and again inside` variants` for Variant-level ones:


```text
query {
product(id: "080feded-4f74-4d31-9309-f7ef6b7f1279") {
masterData {
current {
attributesRaw {
name
value
}
variants {
sku
attributesRaw {
name
value
}
}
}
}
}
}


```


A few details worth checking every time: pass` staged=false` (or omit it) so you're reading the published value, not a draft; use` localeProjection` if the attribute is a` ltext` (localized text) type, since the value will come back as a locale map rather than a plain string; and if your storefront calls the Product Projection **Search** endpoint (` /product-projections/search` ) rather than a direct GET by ID, be aware Product-level attributes are not returned there — fetch by ID, or use the newer Product Search endpoint, instead.


## Step 3: Bind the attribute to the template


commercetools doesn't dictate a rendering layer, so how the attribute reaches HTML depends on your storefront setup:


- **commercetools Frontend (Studio-based)** : a component's` schema.json` declares a data-bound field, a Node.js data-source extension fetches the product (typically via the Product Projections or GraphQL API), and the resulting payload is passed to the React component as props — for example, a field mapped from` data.product.dataSource` — which the component then renders into JSX/HTML. See[Creating a Frontend component with a data source](https://docs.commercetools.com/frontend-development/creating-frontend-component-with-a-data-source) .
- **Custom storefront (Next.js, or any framework)** : the PDP route's data-fetching function calls the Product Projections or GraphQL endpoint directly, finds the attribute by` name` in the appropriate` attributes` array (product-level or variant-level), and renders it into the template.


A minimal PDP render, once the attribute is in hand:


```text
// care_instructions is a Product-level attribute, so it's on product.attributes.
// Variant-level attributes (color, size) would instead be read from
// product.masterVariant.attributes.
const careInstructions = product.attributes.find(
(attr) => attr.name === "care_instructions"
)?.value;


export function ProductSpecs({ product }) {
return (
<section>
<h2>Care instructions</h2>
<p>{careInstructions ?? "Not specified"}</p>
</section>
);
}


```


If you also want the value legible to AI shopping agents and search crawlers, add it to a[JSON-LD](https://www.anglera.com/glossary/json-ld)` Product` block in the page head or a script tag near the PDP content, alongside the visible copy — not instead of it:


```text
<script type="application/ld+json">
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "Example Jacket",
"additionalProperty": [
{
"@type": "PropertyValue",
"name": "Care instructions",
"value": "Machine wash cold, tumble dry low."
}
]
}
</script>


```


## How to validate


1. **Confirm the value is published, not staged.** Query the Product Projections API without` staged=true` ; if the field is empty there but present when` staged=true` , the change hasn't been published yet.
2. **View-source vs. rendered DOM.** Right-click → View Page Source shows what's in the initial HTML payload; if your storefront renders client-side, the attribute may be present in the DOM (inspect via DevTools) but absent from view-source — meaning crawlers that don't execute JavaScript won't see it. Server-rendered or statically generated PDPs should show the value in both.
3. **Check the raw API response** with the` curl` command above to isolate whether the gap is in the data (attribute missing or unpublished) or in the template (attribute present in the API but never mapped to a component/prop).
4. **Validate structured data** with Google's[Rich Results Test](https://search.google.com/test/rich-results) if you added JSON-LD, to confirm it parses and the property is picked up.


## Verified as of July 2026


Endpoint paths, the` attributesRaw` GraphQL field, and the Merchant Center menu path above reflect commercetools' current documentation as of July 2026. Product-level attributes (the` level: "Product"` option shown in Step 1) reached general availability in December 2025, after a public beta that began in mid-2025 — a newer part of the model, and the reason the top-level` attributes` array versus` masterVariant.attributes` distinction above is easy to get wrong in older tutorials or AI-generated code. commercetools ships frequent API releases, so re-check the[HTTP API release notes](https://docs.commercetools.com/api/releases) if a field behaves unexpectedly.


None of this changes how the attribute gets good in the first place. Anglera plugs into commercetools (or whichever PIM sits in front of it) and keeps attributes like` care_instructions` populated, current, and consistent across your catalog — so the mapping work above has accurate, complete data to render, rather than a blank field with nothing to show.
