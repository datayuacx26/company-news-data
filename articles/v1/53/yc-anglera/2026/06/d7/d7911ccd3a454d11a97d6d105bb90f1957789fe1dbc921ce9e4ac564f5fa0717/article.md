---
schema_version: "1.0.0"
document_id: "d7911ccd3a454d11a97d6d105bb90f1957789fe1dbc921ce9e4ac564f5fa0717"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/adobe-commerce-agent-readable"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:b889e8c347a3decb0135db192914b7ae157dbb9751e587b7be2c7919091f5ab5"
---

# Making your Adobe Commerce catalog agent-readable (AEO)

Once your product data is enriched, the remaining question is mechanical but consequential: does that data actually land in the HTML an AI shopping agent or crawler receives, or only in a UI that requires a browser to construct? Adobe Commerce gives you two very different starting points depending on which storefront architecture you're running, and both need deliberate work to become fully agent-readable. This guide walks through structured attributes, Product[JSON-LD](https://www.anglera.com/glossary/json-ld) , server rendering, and direct buyer-question copy, with a checklist for what an agent can and can't currently pull off your pages.


## Two storefront architectures, two different problems


Adobe Commerce merchants are generally on one of two rendering models, and the fix looks different for each:


- **Classic (Luma/PHP storefront)** : pages render server-side via PHTML templates and layout XML. HTML is complete on arrival. The default Luma theme ships basic schema.org markup, but it's incomplete — missing` offers.availability` ,` aggregateRating` ,` gtin` /` mpn` , and` brand` in many themes.
- **Adobe Commerce Storefront (Edge Delivery Services boilerplate + Catalog Service GraphQL, "drop-in" components)** : product data is fetched and rendered largely client-side by JavaScript. Without prerendering, a crawler or agent that doesn't execute JavaScript sees a near-empty shell.


Check which one you're on before applying fixes below — the attribute work is identical, but the rendering fix differs.


## 1. Complete structured attributes in the catalog


Everything downstream — JSON-LD, spec tables, buyer answers — depends on attributes actually existing as discrete catalog fields rather than being buried inside a paragraph of description copy.


In the Admin, attributes live under **Stores > Attributes > Product** , where you set a **Default Label** , a **Catalog Input Type for Store Owner** (text, dropdown, multiselect, price, yes/no, and so on), and whether the value should appear as a **Products list column** or filterable option. Attributes are then grouped into **attribute sets** ( **Stores > Attributes > Attribute Set** ) so that, for example, all products in a "Work Boots" set carry` sole_material` ,` safety_rating` , and` waterproof_rating` as first-class fields rather than free text.


Practical guidance:


- One attribute per fact (e.g.,` material` ,` voltage` ,` certification` ) — not one long "Specifications" text block. A text block is unreadable to structured extraction; a set of typed attributes is queryable and mappable to schema.org properties.
- Use dropdown/multiselect for controlled vocabularies (finish, size, certification) so values are consistent across the catalog — inconsistent free text ("Stainless," "SS," "stainless-steel") fragments what should be one filterable value.
- Assign attributes to attribute sets deliberately. An attribute that exists but isn't in the product's attribute set won't show up in the admin form or, by default, in exports/GraphQL responses for that product.


Reference:[Adobe Commerce — Create and delete product attributes](https://experienceleague.adobe.com/en/docs/commerce-admin/catalog/product-attributes/create/attribute-product-create) ,[Attribute sets](https://experienceleague.adobe.com/en/docs/commerce-admin/catalog/product-attributes/create/attribute-sets) .


## 2. Product JSON-LD: what's there by default, what to add


Adobe's own documentation states that "by default, structured data markup that is based on the schema.org standard is added to your store's product template," but on the classic Luma theme this is often microdata (` itemprop` attributes scattered through the template) rather than a single complete JSON-LD block, and it's frequently missing` offers` ,` availability` ,` aggregateRating` ,` sku` ,` gtin` /` mpn` , and` brand` . For catalog data that AI agents and Google's Product structured data both key off, you want one consolidated JSON-LD block with those fields populated from the attributes above.


For a classic/Luma storefront, the supported extension point is layout XML plus a template block — add a block via` catalog_product_view.xml` (in your theme, e.g.` app/design/frontend/<Vendor>/<theme>/Magento_Catalog/layout/catalog_product_view.xml` ) that renders a` .phtml` template outputting the JSON-LD` <script type="application/ld+json">` tag, populated from the product model's attributes:


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "12V Cordless Impact Driver",
"sku": "IMP-12V-01",
"gtin13": "0012345678905",
"mpn": "IMP-12V-01",
"brand": { "@type": "Brand", "name": "Acme Tools" },
"description": "Compact 12V impact driver rated for 1,500 in-lb of torque.",
"additionalProperty": [
{ "@type": "PropertyValue", "name": "Voltage", "value": "12V" },
{ "@type": "PropertyValue", "name": "Torque", "value": "1500 in-lb" },
{ "@type": "PropertyValue", "name": "Chuck size", "value": "1/4 in hex" }
],
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "89.00",
"availability": "https://schema.org/InStock",
"url": "https://www.example.com/impact-driver-12v.html"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.6",
"reviewCount": "128"
}
}


```


A dedicated, version-controlled module that only emits this JSON-LD (rather than a large third-party SEO suite) keeps the mapping between attribute code and schema property explicit and easy to audit. Third-party options such as the Outer Edge structured data module or Apptrian's rich snippets extension exist for teams that don't want to maintain this in-house.


For the newer Adobe Commerce Storefront (EDS boilerplate), the boilerplate ships a metadata generator (` tools/pdp-metadata/` ) that pulls product data from the Catalog Service GraphQL API and produces meta tags plus JSON-LD, and the PDP drop-in component includes a JSON-LD reference implementation you adapt to your attribute set. Validate the same` Product` /` Offer` /` AggregateRating` fields regardless of which path generates them.


## 3. Make sure the content is actually server-rendered


JSON-LD and structured attributes are wasted if the HTML an agent fetches doesn't contain them. On classic Luma, this is largely solved — layout-XML-driven PHTML output is present in the initial server response.


On the EDS-based Adobe Commerce Storefront, product content and the JSON-LD generated by the metadata tooling need to reach the crawler without a JavaScript execution step. Adobe's answer here is the[AEM Commerce Prerender framework](https://github.com/adobe-rnd/aem-commerce-prerender) : it fetches products from the Catalog Service ahead of any request, detects changes, generates complete HTML (with meta tags and structured data embedded), and publishes it via the Bring Your Own Markup (BYOM) API so pages are served pre-rendered rather than assembled client-side. If you're on this architecture and haven't deployed prerendering, treat it as required infrastructure for AI/search visibility, not an optimization.


## What an AI agent can and cannot extract


**Can extract** (when set up as above): SKU, price, currency, availability, brand,[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) /MPN, rating/review counts, and any attribute exposed via` additionalProperty` in server-delivered JSON-LD; visible spec-table text and short descriptions present in the initial HTML; breadcrumb and category context from` BreadcrumbList` markup.


**Cannot extract** : attributes that exist in the PIM/catalog but aren't assigned to the product's attribute set or mapped into JSON-LD; spec content trapped in an image, PDF spec sheet, or downloadable-only asset; anything injected purely by client-side JavaScript on an unprerendered EDS storefront; accordion/tab content that's populated on user interaction rather than present in server HTML; free-text specifications embedded inside a single description block with no discrete field to key off.


## How to validate


- **View-source vs. rendered DOM** : compare` curl -s https://yourstore.com/product-url.html` against what a headless browser renders. On Luma they should be nearly identical; on an unprerendered EDS storefront they will diverge sharply — that gap is what agents miss.
- **Grep for structured data** :` curl -s <url> | grep -A2 'application/ld+json'` confirms the JSON-LD block ships in the initial response.
- **Rich Results Test** : run the product URL through Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm the` Product` schema parses cleanly — a single invalid field can invalidate all annotations on the page.
- **EDS-specific** : check the Storefront Prerender Management UI to confirm the product's prerendered status and last-published timestamp.


Verified as of July 2026 against Adobe's Experience League and Commerce Storefront developer documentation; Adobe Commerce Storefront tooling (drop-ins, prerender framework, metadata generator) is actively evolving, so confirm exact file paths and script names against your installed boilerplate version.


None of this works if the underlying attributes are thin — a complete JSON-LD template with empty` additionalProperty` arrays doesn't help anyone. Anglera enriches the attribute, spec, and use-case data sitting in your Adobe Commerce catalog on an ongoing basis, so the structured fields this page-side work depends on are actually populated, current, and worth rendering.
