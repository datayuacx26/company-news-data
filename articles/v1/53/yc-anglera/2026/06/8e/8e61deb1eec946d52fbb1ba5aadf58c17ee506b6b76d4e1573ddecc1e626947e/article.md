---
schema_version: "1.0.0"
document_id: "8e61deb1eec946d52fbb1ba5aadf58c17ee506b6b76d4e1573ddecc1e626947e"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/elastic-path-data-to-page"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:30988b5a89b34d8b473266550275517d51fdf1b25624739e61a43c9adb806908"
---

# Getting enriched product data onto Elastic Path product pages

Elastic Path is headless: there is no theme layer or Liquid-style template that auto-renders a new field the moment you add it. Every attribute you want on a product page has to travel a specific path — stored in Product Experience Manager (PXM), published into a catalog, fetched by your storefront through the Shopper Catalog API, and explicitly mapped into a component. This guide walks that path end to end for a single enriched attribute, with the exact API shapes and a validation step so you can confirm it actually made it onto the page rather than just into the admin.


## Where the attribute lives: PXM, and which mechanism to use


Elastic Path gives you two ways to attach non-core data to a product in PXM, and picking the right one matters for how (and whether) the data shows up downstream.


**Custom attributes** (` shopper_attributes` and` admin_attributes` ) are schema-less key-value pairs stored directly on the product resource.` shopper_attributes` are customer-facing and merge into the published catalog;` admin_attributes` are internal-only and never leave the PXM API. This is the mechanism built for exactly the kind of continuous, high-volume enrichment a PIM or an enrichment pipeline produces — no template setup required, filterable with` eq` /` like` /` in` on dot-notation paths, and importable via CSV using` shopper_attributes.` prefixed column headers.


**Extension templates** (Commerce Manager → Merchandise → Products → Templates/Attributes) are the older, more structured route: you create a Template, then create typed Attributes (Boolean, Float, Integer, Date, String) and assign each one to exactly one template. These are better suited to a small, curated set of merchandiser-managed fields with strict typing than to bulk, machine-generated enrichment.


For a specification like a water-resistance rating pushed from a PIM or enrichment tool,` shopper_attributes` is almost always the right call. Setting it via the PXM Products API is a partial update — you only send the keys you're changing:


```text
curl -X PUT "https://useast.api.elasticpath.com/pcm/products/<product_id>" \
-H "Authorization: Bearer <admin_access_token>" \
-H "Content-Type: application/json" \
-d '{
"data": {
"type": "product",
"id": "<product_id>",
"attributes": {
"shopper_attributes": {
"water_resistance_rating": "IPX7",
"material": "recycled-aluminum"
}
}
}
}'


```


Attribute names are capped at 64 characters (alphanumeric, underscore, hyphen only) and values must be strings up to 512 characters — worth knowing before you pipe in a long enrichment paragraph expecting it to fit in one attribute.


## Publishing the attribute into a catalog


An attribute existing on a product in PXM does not mean a shopper (or a crawler) can see it. Three conditions have to be true before it reaches the storefront:


1. The product's status is` live` , not` draft` .
2. The product is associated with at least one hierarchy node.
3. A catalog references that hierarchy, and the catalog has been published (a "catalog release" has been created since the change).


If a product and its price book both define overlapping` shopper_attributes` , the catalog release merges them, with the price book's values taking precedence — worth checking if you maintain regional price books, since a stale price-book value can silently override the product's enriched value in one locale and not another. Edit an attribute after the fact and nothing changes on the storefront until you trigger a new catalog release.


## Fetching it in the storefront


Once published, the attribute is available through the public-facing Shopper Catalog API,` GET /catalog/products/:product_id` , which is what any headless frontend — Elastic Path's own Next.js–based Composable Frontend, or a custom app — actually calls at render time:


```text
curl "https://useast.api.elasticpath.com/catalog/products/<product_id>?include=main_image,files" \
-H "Authorization: Bearer <storefront_access_token>"


```


```text
{
"data": {
"id": "b2c3d4e5-...",
"type": "product",
"attributes": {
"name": "Trailhead 40L Pack",
"sku": "TH-PACK-40L",
"shopper_attributes": {
"water_resistance_rating": "IPX7",
"material": "recycled-aluminum"
}
}
}
}


```


Elastic Path's Composable Frontend starter wraps this same call in the` @epcc-sdk/sdks-shopper` package's` getByContextProduct` function, which the reference storefront uses to fetch a product server-side, detect its type (` standard` ,` parent` ,` child` ,` bundle` ) from` meta.product_types` , and hand the response to a display component.


## Rendering it on the page


In a Next.js-based storefront (the pattern Composable Frontend ships), the attribute becomes a server component that reads` shopper_attributes` off the fetched product and maps it into markup:


```text
import { getByContextProduct } from "@epcc-sdk/sdks-shopper"


export async function ProductSpecs({ productId }: { productId: string }) {
const { data: product } = await getByContextProduct({
path: { product_id: productId },
})


const specs = product?.attributes?.shopper_attributes ?? {}
const rows = Object.entries(specs)


if (rows.length === 0) return null


return (
<dl className="grid grid-cols-2 gap-x-4 gap-y-1 text-sm">
{rows.map(([label, value]) => (
<div key={label} className="contents">
<dt className="font-medium capitalize">{label.replace(/_/g, " ")}</dt>
<dd>{String(value)}</dd>
</div>
))}
</dl>
)
}


```


Because this runs as a React Server Component, the markup is generated on the server and shipped in the initial HTML — not bolted on after hydration. That matters for AI crawlers and answer engines that don't execute JavaScript: what's in view-source is what they see.


It's worth doing the same attribute twice: once as visible copy, once as structured data, since many AI agents and rich-result parsers prefer` schema.org` markup over prose:


```text
const additionalProperty = Object.entries(specs).map(([name, value]) => ({
"@type": "PropertyValue",
name,
value,
}))


const productJsonLd = {
"@context": "https://schema.org",
"@type": "Product",
name: product?.attributes?.name,
sku: product?.attributes?.sku,
additionalProperty,
}


```


```text
<script
type="application/ld+json"
dangerouslySetInnerHTML={{ __html: JSON.stringify(productJsonLd) }}
/>


```


## How to validate


Don't trust the browser's Inspect Element panel by itself — it shows the rendered DOM after JavaScript has run, which can mask a component that only populates client-side.


1. **View-source vs. rendered DOM.** Load the live PDP, then compare` curl -s https://yourstore.com/products/trailhead-40l-pack | grep -i "water_resistance\\|IPX7"` (raw HTML, no JS) against what you see in DevTools → Elements. If the attribute is in the DOM but absent from the curl output, it's client-only and invisible to most crawlers — check whether the component is accidentally marked` "use client"` in Next.js.
2. **Confirm the API response first.** Before debugging the frontend, curl the Shopper Catalog API directly (the request shown above) and verify` shopper_attributes` is present and populated. If it's missing there, the problem is upstream — the catalog hasn't been republished, or the attribute landed in` admin_attributes` by mistake.
3. **Validate the structured data.** Run the page through Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm the[JSON-LD](https://www.anglera.com/glossary/json-ld) parses and the` additionalProperty` array is populated, not just the base` Product` fields.


## Verified as of July 2026


Field names, endpoint paths, and the custom-attributes vs. extension-templates distinction reflect Elastic Path's current Composable Commerce (SaaS) documentation as of July 2026. Elastic Path also maintains a separate, older Self-Managed Commerce product line with a different Cortex-based API; if you're on that platform, the storefront-fetch and rendering mechanics differ even though the PXM concepts are similar. Confirm which product line your store runs on before following the code above literally.


Anglera doesn't touch any of this page-rendering plumbing — it's additive on the data side, continuously enriching attributes like the one above and pushing them into whichever field your PIM or Elastic Path store expects,` shopper_attributes` included. Your PIM stores the data; Anglera does the work of keeping it complete and current, so the templates and components you build following the steps above have something rich to render on day one and on day one thousand.


Sources:[Custom Attributes — Elastic Path Documentation](https://developer.elasticpath.com/guides/key-concepts/product-experience-manager/custom-attributes/) ,[Product Templates Attributes in Commerce Manager](https://developer.elasticpath.com/docs/commerce-manager/product-experience-manager/extending-products/templates) ,[Get a Product — Shopper Catalog API](https://developer.elasticpath.com/docs/api/pxm/catalog/get-by-context-product) ,[elasticpath/composable-frontend — all-product-types example](https://github.com/elasticpath/composable-frontend/tree/main/examples/all-product-types)
