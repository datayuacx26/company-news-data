---
schema_version: "1.0.0"
document_id: "97d91d4b7be5df24bcf5387ca10b7abe56fe09d0e539b64713b9594b86f14dc5"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/orocommerce-data-to-page"
published_at: "2026-05-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:05bf42cc439e320d876f8c11734a98d4978ea5cf96b1b1168f626ae8faa9991f"
---

# Getting enriched product data onto OroCommerce product pages

Enriching a product record with specs, use-cases, and identifiers only pays off once that data actually renders on the storefront in a form buyers and AI shopping agents can parse. OroCommerce keeps product data and page rendering in two distinct layers —[product attributes](https://www.anglera.com/glossary/product-attributes) /families on the data side, and the layout + Twig block system on the presentation side — and a distributor has to configure both before an enriched field shows up in view-source. This guide walks through that binding for a real attribute, then covers how to validate it actually rendered.


## Where the enriched attribute lives


In OroCommerce, a "product attribute" is a typed custom field on the Product entity, created under **Products → Product Attributes** in the back-office. Creation is two steps: first you pick a field name (2-22 characters, alphanumeric and underscores) and a type — String, Integer, Decimal, Boolean, Date, File, Image, Select/Multi-Select, or a relation — then, on the follow-up screen, you set the label, description, and a set of storefront/back-office flags: **Show on View** , **Show on Form** , plus Searchable, Filterable, Sortable, and Exportable ([Manage Product Attributes](https://doc.oroinc.com/user/back-office/products/product-attributes/) ).


An attribute existing on the entity isn't enough to make it visible to shoppers. OroCommerce groups attributes into **Product Families** , and within each family the **Attributes** section organizes them into named **attribute groups** — the same groups that render as titled sections on the storefront PDP. To surface a new attribute, open **Products → Product Families** , edit the relevant family, and in the Attributes section start typing the attribute name to add it into an existing or new group ([Manage Product Attributes in Product Families](https://doc.oroinc.com/user/back-office/products/product-families/manage/) ). This is the step distributors most often miss after a bulk data load: the value can be sitting on every product record and still never reach the page because it was never assigned into a family's attribute group.


So a distributor pushing an enriched "Rated Voltage" spec, for example, would: create a Decimal or String attribute named` rated_voltage` , enable **Show on View** , add it to the relevant electrical-components family's "Electrical Specifications" group, and reindex. At that point Oro's default PDP template will pick it up automatically — no template change required for a standard text/number/select attribute.


## How it binds to the theme and renders as HTML


The storefront PDP is not a single flat template — it's a **layout** , assembled from layout blocks, and each block is rendered by a matching Twig block ([Templates (Twig)](https://doc.oroinc.com/frontend/storefront/templates/) ). For product attributes specifically, Oro generates a layout block per attribute group and iterates its attributes, giving each one a predictable, deterministic Twig block ID of the form:


```text
_product_view_attribute_group_<group_code>_attribute_<field_type>_<attribute_code>_widget


```


So the out-of-the-box renderer for a text SKU attribute is the block` _product_view_attribute_group_general_attribute_text_sku_widget` , and the same convention applies to your custom attribute once it's in a group. The layout engine resolves a Twig block by looking for the block-ID-specific name first, then the generic block-type name, then the parent type — which is why Oro can render hundreds of differently-named attributes with one shared` attribute_widget` -style template while still letting you override a single one by name.


For most attributes you never touch Twig at all: assigning the attribute to a group is the whole job, and Oro's generic attribute-group renderer outputs the label/value pair inside the section markup. You only write a layout update when you need non-default rendering — a unit suffix, an icon, a link, or a different DOM wrapper for that one field.


## A concrete override: adding a unit suffix


To render` rated_voltage` with a "V" suffix and a data attribute for downstream parsing, create a layout update in your theme — a YAML file placed under the product-view route's own folder (for example, Resources/views/layouts/your-theme/oro_product_frontend_product_view/layout_update.yml, since Oro's directory convention keys the first folder level to the theme name and the second to the route name) — that loads a custom block theme, then define the matching Twig block:


```text
layout:
actions:
- '@setBlockTheme':
themes: 'attributes.html.twig'


```


```text
{% block _product_view_attribute_group_specifications_attribute_decimal_rated_voltage_widget %}
{% set attr = layout_attr_defaults(attr, {'~class': ' attribute-value', 'data-spec': 'rated-voltage'}) %}
<div {{ block('block_attributes') }}>
{{ value }} V
</div>
{% endblock %}


```


Use` @add` ,` @move` , or` @remove` layout actions if you also need to reposition the block, and target specific product types or categories with layout-update` conditions` , since every product view page exposes` product_type` and` category_id` /` category_ids` in the layout context ([Customize Product View Page](https://doc.oroinc.com/bundles/commerce/ProductBundle/customize-products/customize-pdp/) ).


## Making the same value legible to AI agents


OroCommerce does not ship schema.org/[JSON-LD](https://www.anglera.com/glossary/json-ld) Product markup out of the box, so an AI shopping agent parsing rendered HTML only sees what's in the visible attribute-group markup unless you add structured data yourself. The straightforward approach is a small custom layout block, added via the same` @add` /` @addTree` pattern Oro documents for the WYSIWYG-content case, that outputs a JSON-LD script block pulling from the same product/attribute data already bound to the page — so specs like rated voltage, dimensions, and identifiers appear both as human-readable label/value rows and as machine-readable` additionalProperty` entries in one pass, without duplicating enrichment logic:


```text
{% block _product_view_structured_data_widget %}
<script type="application/ld+json">
{{ {
'@context': 'https://schema.org/',
'@type': 'Product',
'sku': product.sku,
'additionalProperty': [
{ '@type': 'PropertyValue', 'name': 'Rated Voltage', 'value': product.rated_voltage ~ ' V' }
]
}|json_encode|raw }}
</script>
{% endblock %}


```


## How to validate


- **View-source vs rendered DOM** : browser "View Page Source" shows the server-rendered Twig output before JS runs; if your attribute value is missing there but present in DevTools' Elements panel, something client-side (a widget, lazy block) is injecting it and it likely won't be visible to non-JS crawlers or simple agent fetchers.
- **Block Debug Info** : enable "Include Block Debug Info Into HTML" under **System → Configuration → Development Settings** , then inspect the rendered page for` data-layout-debug-block-id` and` data-layout-debug-block-template` attributes to confirm which Twig block and template actually rendered a given element ([Frontend Developer Tools](https://doc.oroinc.com/frontend/storefront/debugging/) ).
- **Twig Inspector** : with the Symfony profiler active, use the Twig Inspector toolbar to click any rendered element and jump straight to the template/block that produced it — faster than grepping theme overrides by hand.
- **curl** :` curl -s https://yourstore.example/product/123 | grep -i "rated_voltage\\|application/ld+json"` confirms the value and any JSON-LD block exist in the raw HTTP response, independent of the browser.
- **Google Rich Results Test** : once JSON-LD is in place, validate the markup at[Google's Rich Results Test](https://search.google.com/test/rich-results) to confirm it parses as a valid` Product` entity.
- After any attribute, family, or layout change, clear the application and layout cache and reindex the storefront search index so the storefront reflects the new configuration.


Verified as of July 2026 against current OroCommerce back-office and storefront-layout documentation; menu paths and Twig block-naming conventions can shift slightly between minor versions, so confirm against your instance's version-specific docs before shipping a layout override.


Anglera plugs into the PIM or commerce platform you already run and keeps attributes like rated voltage, dimensions, and use-case data continuously enriched and accurate at the source — so once an attribute is assigned to a OroCommerce attribute group, the values flowing into that layout block are already complete instead of sparse. Your PIM stores the data; Anglera does the work of keeping it worth rendering.
