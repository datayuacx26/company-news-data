---
schema_version: "1.0.0"
document_id: "b956d4153a19bdf600dbd1a018e47c1d631b5dfc7e2edd893926b5baf743e323"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/shopify-data-to-page"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:adbdc6518e270096a1a9d2730757ff98fc407b8655af347c7585890b3df541f6"
---

# Getting enriched product data onto Shopify product pages

Enriching a product record is only half the job — that data has to land on the actual page before a shopper or an AI shopping agent can read it. On Shopify, "the actual page" almost always means a metafield feeding a theme section through Liquid, and increasingly a[JSON-LD](https://www.anglera.com/glossary/json-ld) block that machine readers parse directly. This guide walks through that path end to end: where the data lives, how it's wired to the template, and how to confirm it actually shipped in the HTML response rather than just the admin preview.


## Where the enriched data lives


Shopify's native place for structured[product attributes](https://www.anglera.com/glossary/product-attributes) that don't fit the built-in fields (title, price, variants) is a **metafield** . Metafields are created as **metafield definitions** in Shopify admin under Settings, then the custom-data area — Shopify has been labeling this section "Metafields and metaobjects" in some admin accounts and "Custom data" in others, so use whichever name your store's Settings menu shows — scoped to a resource type (Products, Variants, Collections, and so on). A definition sets a name, a namespace and key pair (for example` custom.material_composition` ), a type — single line text, rich text, list, number, JSON, and others — and optional validations. Once the definition exists, values get populated either by hand on each product's admin page, in bulk via CSV/Matrixify-style import, or programmatically through the Admin GraphQL API's` metafieldsSet` mutation, which is how most enrichment pipelines write data back at scale ([Shopify Help Center: metafields](https://help.shopify.com/en/manual/custom-data/metafields) ,[Creating custom metafield definitions](https://help.shopify.com/en/manual/custom-data/metafields/metafield-definitions/creating-custom-metafield-definitions) ).


A metafield with a value sitting on a product record does nothing for the storefront by itself. It has to be connected to a template.


## Binding the metafield to the theme


If the theme is built on Online Store 2.0 conventions (JSON templates plus sections and blocks — the default since 2021 and what every current Shopify theme, including Dawn, uses), there are two ways to make the connection:


**No-code path — Dynamic Sources.** In the theme editor, open the Product template, select a section or block that exposes a connectable setting (Dawn's "Product information" section supports several), click the "connect dynamic source" icon next to that setting, and pick the metafield from the list. The theme editor writes the reference into the section's settings JSON; no Liquid is touched. This only works for settings types the theme has explicitly marked as eligible for dynamic sources, so a merchant is limited to whatever the theme author exposed ([Dynamic data sources](https://shopify.dev/docs/storefronts/themes/architecture/settings/dynamic-sources) ).


**Code path — direct Liquid.** For anything a theme doesn't expose as a dynamic source — or when you want control over markup, wrapping elements, or conditional logic — edit the section file directly (Online Store, then Themes, then Edit code) and reference the metafield object:


```text
{% comment %} inside sections/main-product.liquid or a custom snippet {% endcomment %}
{%- if product.metafields.custom.material_composition.value != blank -%}
<div class="product-attribute product-attribute--material">
<span class="product-attribute__label">Material</span>
<span class="product-attribute__value">
{{ product.metafields.custom.material_composition.value }}
</span>
</div>
{%- endif -%}


```


The` .value` accessor is required — omitting it returns the metafield object, not its content. Rich text and list-type metafields need a rendering filter rather than a plain double-brace output, since their` .value` is structured JSON. Shopify's` metafield_tag` filter handles this automatically, choosing the right HTML wrapper per type — a div for rich_text_field, a ul list for list.single_line_text_field, an anchor element for reference types, and so on:


```text
{{ product.metafields.custom.care_instructions | metafield_tag }}


```


That single line, for a` rich_text_field` metafield, renders as a real div element carrying the class` metafield-rich_text_field` , containing formatted paragraph, list, and bold-text markup — not a JSON blob ([metafield_tag filter](https://shopify.dev/docs/api/liquid/filters/metafield_tag) ,[metafield Liquid object](https://shopify.dev/docs/api/liquid/objects/metafield) ).


## Making it visible to AI agents, not just shoppers


Buyers read the visible page. AI shopping agents and crawlers more often parse the JSON-LD script block (type` application/ld+json` ), which is a second, parallel rendering of the same underlying data. Every Online Store 2.0 theme, including Dawn, already emits a baseline` Product` (or` ProductGroup` , for products with variants) schema via Shopify's built-in filter:


```text
<script type="application/ld+json">
{{ product | structured_data }}
</script>


```


This filter covers standard fields — name, brand, image, offers, price, availability — but it does not know about your custom metafields, so an enriched attribute like material composition or a certification code won't appear there automatically ([structured_data filter](https://shopify.dev/docs/api/liquid/filters/structured_data) ). To surface enriched attributes to machine readers, the reliable pattern is a second, custom-authored JSON-LD block that adds them as` additionalProperty` entries, which is the schema.org-sanctioned way to attach arbitrary named attributes to a` Product` :


```text
<script type="application/ld+json">
{
"@context": "https://schema.org/",
"@type": "Product",
"name": {{ product.title | json }},
"sku": {{ product.selected_or_first_available_variant.sku | json }},
"additionalProperty": [
{%- if product.metafields.custom.material_composition.value != blank %}
{
"@type": "PropertyValue",
"name": "Material composition",
"value": {{ product.metafields.custom.material_composition.value | json }}
}
{%- endif %}
]
}
</script>


```


Using the` json` filter (not raw output) on each value is what keeps quotes and special characters from breaking the JSON. Whether you extend the theme's existing structured_data block or add an adjacent one, keep the source of truth in the metafield — the JSON-LD should read from the same value that renders on the page, never a separately hand-typed copy.


## How to validate


Confirm the data actually reached the delivered HTML, not just the Shopify preview or the DOM after client-side scripts run:


- **View-source vs. rendered DOM.** Liquid renders server-side, so a correctly wired metafield should appear in "View Page Source" (` Cmd+Option+U` /` Ctrl+U` ), which shows the raw response — the same thing a non-JS crawler or AI agent sees. If an attribute only shows up in the browser's Inspector/Elements panel (the live DOM) but not in view-source, it's being added by JavaScript after load and many bots won't see it.
- **curl the page directly** , which mimics a bot request with no JS execution:


```text
curl -s https://yourstore.com/products/your-handle | grep -o '<script type="application/ld+json">.*</script>'
curl -s https://yourstore.com/products/your-handle | grep -o 'metafield-rich_text_field.\{0,200\}'


```


- **Validate the JSON-LD** with Google's[Rich Results Test](https://search.google.com/test/rich-results) or the[Schema Markup Validator](https://validator.schema.org/) , both of which will flag malformed JSON (a common cause: forgetting the` json` filter on a value with quotes or line breaks).
- **Check theme scope.** Dynamic sources and the` metafield_tag` /` structured_data` filters only work on Online Store 2.0 themes; a legacy Vintage theme requires hand-written Liquid throughout and won't expose the theme-editor connection UI.


Verified as of July 2026 against Shopify's current admin (Settings, then the "Metafields and metaobjects" / "Custom data" section — naming varies by store), the Liquid metafield object and` metafield_tag` /` structured_data` filters, and the Dawn reference theme. Field names, filter behavior, and menu paths are current for Online Store 2.0 themes; confirm against your specific theme's documentation if it was built before that architecture shifted in 2021.


None of this rendering plumbing has anything to render without a metafield that already holds a real, verified value — which is the part Anglera is built for. Anglera continuously enriches product attributes, specs, and use-case data and writes them back into your existing Shopify metafields (or your PIM, if that's the system of record), so the moment a theme section or JSON-LD block is wired up to point at` custom.material_composition` or its equivalent, there's substantive, current data behind it rather than a blank field. It plugs into the store you already have; nothing about your metafield structure or theme needs to change.
