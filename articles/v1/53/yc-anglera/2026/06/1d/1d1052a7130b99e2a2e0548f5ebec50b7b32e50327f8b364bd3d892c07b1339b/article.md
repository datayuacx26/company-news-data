---
schema_version: "1.0.0"
document_id: "1d1052a7130b99e2a2e0548f5ebec50b7b32e50327f8b364bd3d892c07b1339b"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/salsify-structured-data-sync"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:f8c7f61062573e89a67f03a2c49f6b44ed6c809d7372d3985568fbfa9ff99c81"
---

# Keeping structured data in sync from Salsify to the page

Salsify is very good at being the single source of truth for product content — attributes, digital assets, copy, compliance data. But "correct in Salsify" and "correct on the page" are two different claims, and the gap between them is where AI agents (and Googlebot) actually get burned: they read the rendered HTML, not your PIM. This guide covers the mechanics of getting Salsify data onto the page — as visible content and as` Product`[JSON-LD](https://www.anglera.com/glossary/json-ld) — and keeping both in sync as records change.


## Where the sync actually breaks


Most teams already have Salsify pushing data somewhere: a syndication channel, a scheduled export, or a direct integration into a commerce platform. The sync problem shows up in three places downstream of that:


1. **Stale builds.** A statically generated or cached PDP doesn't rebuild when the Salsify record changes, so the export happened but the page didn't move.
2. **Partial mapping.** The template only renders a subset of what was exported (e.g., marketing copy but not[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) , weight, or compliance attributes), so JSON-LD and visible content drift apart even when both are technically "from Salsify."
3. **Client-side-only rendering.** The JSON-LD or attribute table is injected by JavaScript after page load, which most crawlers and many agent fetchers never execute, so what left Salsify never actually reaches the reader.


Fixing each of these is a matter of picking the right Salsify mechanism and wiring it to a rebuild or revalidation trigger, not a rip-and-replace of your stack.


## Getting data out of Salsify: export vs. webhook vs. channel


Salsify gives you three mechanisms with different latency and shape, and most implementations end up needing two of them.


**Export Run API** — a pull-based, on-demand snapshot. You start a run against` POST /api/orgs/:org_id/export_runs` with an` entity_type` (` product` ,` digital_asset` ,` attribute` ,` attribute_value` , or` all` ), a` format` (` json` ,` jsonl` ,` csv` , or` xlsx` — note only JSON/JSONL are valid when exporting` all` entity types together), and optionally a` filter` and a` properties` list to scope which fields come back. This is the right tool for a nightly full-catalog sync or a one-off backfill, and it's where you'd pull GTIN, weight, dimensions, and other structured attributes in bulk for a JSON-LD field-mapping job.


**Product Change Webhooks** — event-driven, near-real-time. You configure a monitored product list inside Salsify (by smart list or saved filter) and Salsify POSTs a payload in the product CRUD API JSON format whenever a product on that list is added, changed, or removed, with a` trigger_type` of` add` ,` change` , or` remove` . Two behaviors matter for a sync design: deliveries are retried up to 15 times over 48 hours with exponential backoff if your endpoint is down, and there's no cross-product ordering guarantee (same-product updates arrive in order; different products don't). Design your webhook handler to be idempotent and to key off the payload's own timestamp, not arrival order.


**Channels** — Salsify's syndication layer for pushing mapped, scheduled feeds (CSV, Excel, XML, or via connector) directly into a destination like Shopify or BigCommerce, mapping Salsify attributes and categories to that platform's product, variant, and metafield fields. If your PDP is served by the commerce platform itself, a channel is often the simplest path — Salsify becomes the writer of the platform's product record, and your template reads from the platform's own data layer.


## Mapping attributes to` Product` JSON-LD


Whichever mechanism delivers the data, the mapping step is the same: decide which Salsify attributes become which schema.org` Product` properties, and keep that mapping in one place (a config file or transform function) rather than duplicated across templates.


```text
{
"@context": "https://schema.org",
"@type": "Product",
"name": "{{ salsify:product_name }}",
"sku": "{{ salsify:sku }}",
"gtin": "{{ salsify:gtin }}",
"brand": {
"@type": "Brand",
"name": "{{ salsify:brand }}"
},
"description": "{{ salsify:long_description }}",
"image": ["{{ digital_asset:primary_image_url }}"],
"additionalProperty": [
{
"@type": "PropertyValue",
"name": "Material",
"value": "{{ salsify:material }}"
},
{
"@type": "PropertyValue",
"name": "Weight",
"value": "{{ salsify:weight }}"
}
],
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "{{ commerce_platform:price }}",
"availability": "https://schema.org/InStock"
}
}


```


Two details worth building into the mapping layer rather than fixing later:


- Use schema.org's unified` gtin` property, not the older` gtin8` /` gtin12` /` gtin13` /` gtin14` variants —` gtin` accepts any valid 8-, 12-, 13-, or 14-digit code and avoids mismatched-length errors.
- Attribute-level values that don't map to a first-class` Product` field (material, care instructions, certifications) belong in` additionalProperty` as` PropertyValue` pairs — don't drop them just because schema.org doesn't have a named slot.
- Price and availability usually live in the commerce platform, not Salsify, so the JSON-LD build step needs both sources. Keep price out of the Salsify-driven part of the template so a stock/price change doesn't require touching product-content code.


## Rendering: server, not just source


Google's own structured data guidance is explicit that JSON-LD must describe content that's actually present on the page, and in practice that means server-rendering both the visible attributes and the JSON-LD block rather than injecting them client-side. If your PDP is a JavaScript framework, generate the JSON-LD (and the human-readable spec table) at build time or on the server, not in a` useEffect` after hydration — most agent fetchers and some crawlers don't execute JavaScript, so client-injected markup is invisible to exactly the readers this guide is for.


## Closing the loop: trigger a rebuild, don't just trust the export


The step teams skip is wiring the webhook to an actual page update. A Salsify product-change webhook should do one of:


- Call your platform's on-demand revalidation (e.g., a Next.js ISR revalidate endpoint, or your CMS's publish webhook) for the specific PDP path affected.
- Write to the record your commerce platform reads (if using a Channel into Shopify/BigCommerce, the platform's own save event should trigger cache invalidation).
- Queue the changed SKU for the next incremental export/rebuild pass rather than waiting for the nightly full sync.


Without one of these, the Export Run API and webhooks tell you data changed — they don't make the page change.


## How to validate


- **View-source vs. rendered DOM** :` curl -s https://example.com/products/sku-123 | grep -A5 'application/ld+json'` shows exactly what a non-JS-executing fetcher receives. Compare it against what you see in browser DevTools' rendered DOM — a mismatch means something is client-injected.
- **Field-level diff** : pull the same SKU from Salsify (` GET` the product or an export filtered to that ID) and diff its attribute values against the JSON-LD` additionalProperty` array and visible spec table. Automate this as a scheduled check on a sample of SKUs, not just at launch.
- **Google's Rich Results Test** (search.google.com/test/rich-results) and the Schema Markup Validator (validator.schema.org) both parse the live URL — use them after any template change to confirm the JSON-LD still parses and matches an eligible type.
- **Freshness check** : compare the page's rendered` dateModified` (if you set one) or a last-synced value against Salsify's own last-updated timestamp for that record, to catch silent staleness.


**Verified as of July 2026** : Salsify API endpoint paths, webhook retry behavior, and export parameters are per Salsify's public developer documentation as of this writing; confirm current field names and any plan-gated features against your org's Salsify Developer Hub instance, since API and Channel availability can vary by contract.


This whole exercise — mapping, rendering, and validating — only pays off if what's sitting in Salsify is actually complete. Anglera plugs into Salsify as an additive layer, continuously enriching attributes, specs, and identifiers so the export or webhook you're syncing to the page has full, agent-ready content behind it, not gaps you're stuck templating around.


## Sources


- [Salsify Developer Hub — Start Export Run](https://developers.salsify.com/reference/start-export-run)
- [Salsify Developer Hub — Product Change Webhooks](https://developers.salsify.com/docs/webhooks-prod-change)
- [Google Search Central — General Structured Data Guidelines](https://developers.google.com/search/docs/appearance/structured-data/sd-policies)
