---
schema_version: "1.0.0"
document_id: "03d6a0e0aa2069526a1bc015671d22d106501f282cc194259682eaaeb24afe05"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/unilog-product-json-ld"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:b1c52bd014842f813f0020937f70a51351f2f7d42f83939b50135841fcbcbe8b"
---

# Adding Product JSON-LD on Unilog — and keeping it in sync

Unilog's CX1 CIMM2 platform already gives distributors a real product record — part number, manufacturer, category, pricing, inventory — pulled together from your PIM and ERP feeds. Getting that record onto the[product detail page](https://www.anglera.com/glossary/pdp-product-detail-page) (PDP) as machine-readable schema.org Product[JSON-LD](https://www.anglera.com/glossary/json-ld) is a separate, smaller job, but it's the one that determines whether Google's rich results and AI shopping/procurement agents can actually parse what you're selling instead of guessing from page text. Here's how to add it on CIMM2 and, more importantly, how to keep it from drifting away from what buyers see.


## Where the JSON-LD block lives on a CIMM2 page


CX1 CIMM2 renders PDPs from a template layer, not a page-by-page editor — one "item detail" template controls every SKU, populated at request time from your item, pricing, and inventory services. That's good news for structured data: you add the JSON-LD script block once, in the shared item template (or a CMS content component included on it), and every product on the site inherits it automatically.


In practice this template edit is usually done by your internal dev team if you have template/theme access on your CX1 instance, or by Unilog's professional services team or an implementation partner if your site is on a fully managed build — CIMM2 is a hosted, multi-tenant platform, so exactly which admin screen exposes template/HTML editing (and whether it's self-service or requires a service ticket) depends on your CX1 version and support tier. Confirm the current path with your Unilog account team before assuming a menu location; don't guess at it.


Whichever route you take, the JSON-LD fields should be bound to the same merge fields/attributes the visible page already uses for title, part number, manufacturer, price, and stock status — not re-typed by hand. That single decision is what prevents the sync problem covered below.


## The fields that actually matter for a distributor SKU


For a distributor catalog, a handful of properties do most of the work for[product rich results](https://developers.google.com/search/docs/appearance/structured-data/product-snippet) and for giving an AI agent something unambiguous to read:


- **` name`** : the item's display title, pulled from the same PIM description field the PDP's main heading renders.
- **` brand`** : a nested` Brand` object with the manufacturer name — usually your PIM's "Manufacturer" or "Brand" attribute, not a free-text field on the item.
- **` sku`** : your internal part/item number. It only needs to be unique within your catalog.
- **` gtin`** (or the older` gtin8` /` gtin12` /` gtin13` /` gtin14` variants, chosen by digit length): map it if your PIM carries a UPC/EAN/[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) for the item. A large share of industrial, plumbing, and electrical SKUs are private-labeled or don't carry a retail barcode at all — when there's no GTIN, use` mpn` (the manufacturer's part number) instead rather than leaving both blank or fabricating a value. Schema.org's[gtin property reference](https://schema.org/gtin) treats` gtin` as the property that generalizes the older length-specific variants, so use the most specific length you actually have; Google's own merchant guidance is consistent on this point — never guess an identifier or borrow one from a similar SKU, leave it out instead.
- **` offers`** : nested` price` ,` priceCurrency` ,` availability` , and` url` . This is the block most likely to go stale because it's driven by live pricing/inventory, not static catalog content — see the sync section below for the B2B-specific wrinkle.
- **` aggregateRating`** : only include this if you have genuine` ratingValue` /` reviewCount` data behind the item, from an actual reviews feature or integrated ratings provider. Google's structured data policies prohibit marking up fake or misleading reviews, and the numbers in your markup need to match what's visible on the page; most industrial and PVF distributor catalogs simply don't have reviews at the SKU level, and omitting the property entirely is the right call rather than inventing one.


## A real example


```text
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "3/4 in. Brass Ball Valve, Threaded, 600 WOG",
"sku": "BV-075-BR-T",
"mpn": "S-585-70-6",
"gtin13": "0785612345678",
"brand": {
"@type": "Brand",
"name": "Apollo Valves"
},
"offers": {
"@type": "Offer",
"url": "https://www.example-distributor.com/product/BV-075-BR-T",
"priceCurrency": "USD",
"price": "18.42",
"availability": "https://schema.org/InStock",
"itemCondition": "https://schema.org/NewCondition"
}
}


```


Note there's no` aggregateRating` in this example — that's deliberate, not an oversight, for a SKU with no real review data behind it.


## Keeping the JSON-LD in sync with the visible page


The most common way this breaks isn't a missing field, it's drift between the schema and the page a buyer actually sees:


- **Hardcoded snippets.** If someone pastes a static JSON-LD block with a literal price into the item template "to get it working," it will be correct on day one and wrong the next time price or stock changes in the ERP feed — because the visible price re-renders from the live pricing service and the hardcoded schema doesn't. Bind` offers.price` and` offers.availability` to the same service calls that render the buy box, full stop.
- **Gated/net pricing.** Many CIMM2 sites show "Login for pricing" to guest users and only reveal contract/net pricing after authentication. Don't publish a guest-visible` offers.price` that doesn't match what an unauthenticated buyer sees on the page — if the real price is behind login, either omit` price` for those SKUs or publish an accurate list/MSRP value and be consistent about which one appears in both places.` availability` should still reflect real inventory regardless of login state.
- **Discontinued or zero-stock items.** If the PIM marks an item discontinued or out of stock,` availability` needs to flip to` OutOfStock` or` Discontinued` in the same update cycle as the visible stock badge, not on a separate, slower catalog sync.


## How to validate


- **View-source vs. rendered DOM** : fetch the page without executing JavaScript (curl or "view page source") and confirm the full JSON-LD script block is present there. If it only shows up in the browser's inspected DOM and not in the raw HTML, it's being injected client-side and search crawlers and most AI agents won't reliably see it.
- **` curl -s https://yoursite.com/product/SKU | grep -A 30 'application/ld+json'`** is a fast way to pull the block and diff it by eye against the visible price and stock status on the same page.
- Run the URL through Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm the markup parses and is eligible for product rich results, and spot-check a sample of SKUs (in stock, out of stock, gated pricing) rather than just one.


Verified as of July 2026 against Google's Product structured data and product-snippet documentation and schema.org's` gtin` property reference. Unilog CX1 CIMM2 doesn't publish its template-editing menu paths for public reference, so confirm the current admin location with your Unilog account team or implementation partner before scheduling the work.


None of this matters if the underlying attributes — GTIN, brand, manufacturer part numbers, normalized specs — aren't populated and current in the PIM to begin with. That's the half of the problem Anglera handles: it enriches product data continuously inside the PIM or commerce platform you already run, so the JSON-LD template above always has a complete record to map from instead of blank fields to paper over.
