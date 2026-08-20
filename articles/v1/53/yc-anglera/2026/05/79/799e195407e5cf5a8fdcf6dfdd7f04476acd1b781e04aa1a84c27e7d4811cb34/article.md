---
schema_version: "1.0.0"
document_id: "799e195407e5cf5a8fdcf6dfdd7f04476acd1b781e04aa1a84c27e7d4811cb34"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/optimizely-agent-readable"
published_at: "2026-05-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:57.660175+00:00"
content_hash: "sha256:f54c66fd7ca50fc509666a2e774bd0eb2c97cdbb19088a8f20836062bff0617c"
---

# Making your Optimizely Configured Commerce catalog agent-readable (AEO)

Most Configured Commerce distributor catalogs were built to satisfy a buyer clicking through a category tree, not an AI agent trying to answer "does this fit a 3/4-inch NPT fitting" in one pass. Configured Commerce already ships the plumbing for both: structured attributes, a native Product structured-data toggle, and (on Spire) server-side rendering. The work is turning those mechanisms on, filling them in, and pointing them at real data instead of placeholder copy. Below is the implementation path, plus what an agent can and cannot pull off a typical distributor PDP today.


Distributor catalogs are attribute-heavy — thread size, voltage, material, pressure rating, compatible models — and buyers (human or agent) usually arrive with a spec in hand, not a brand name. If that spec lives only in a PDF cut sheet or an unstructured paragraph, an LLM-based shopping agent has to guess at it. Getting attributes into Configured Commerce's structured fields, and those fields onto the rendered page, turns "guess" into "cite."


## Step 1: Get the data into structured fields, not prose


Configured Commerce separates three content mechanisms, and agent-readability depends on using the right one for each kind of fact:


- **[Attributes](https://support.optimizely.com/hc/en-us/articles/4413199911565-Manage-attributes)** (` Admin Console > Catalog > Attribute Types` ) are the closest thing to true structured data. Each attribute type has an` Include On Product` toggle to show it on the PDP,` Filterable` to expose it in facets, and` Comparable` for product comparison. Attribute types are assigned to categories first, then values are assigned to individual products — so a mis-attributed category silently produces products with no filterable specs.
- **Specifications** are the content-tab mechanism (a Content Editor writes rich text, a Content Approver publishes it) — good for a torque chart or install note, but it's HTML in a WYSIWYG field, not machine-parseable key/value data. Treat specs as supplementary explanation, not the primary source for a fact like voltage or[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) .
- **Product documents** (spec sheets, SDS, CAD files) attach files to the product, but the content inside them is invisible to structured data and, generally, to agents, unless you extract the key facts into attributes.


Rule of thumb: any fact a buyer would type into a search box or ask an agent (size, material, certification, compatible part number) belongs in an **attribute** , not a specification tab's paragraph text.


## Step 2: Turn on native Product structured data


Configured Commerce has a built-in[Product structured-data feature](https://support.optimizely.com/hc/en-us/articles/5638590918029-Schema-org-tags) that reads Product Details and emits[JSON-LD](https://www.anglera.com/glossary/json-ld) into the page` <head>` . On Spire, enable it under` Administration > Settings > Site Configurations` in the SEO section (off by default). On Classic CMS, open the[Product Detail Page](https://www.anglera.com/glossary/pdp-product-detail-page) in Content Administration and check` Enable Structured Page Data` . Optimizely has since added companion toggles for Breadcrumb, SiteLinksSearchBox, and Organization structured data in the same settings area — turn all four on.


The generated markup follows the standard` schema.org/Product` shape. A representative distributor PDP should render something like this:


```text
{
"@context": "https://schema.org",
"@type": "Product",
"name": "1/2 in. Brass Ball Valve, 600 WOG",
"sku": "BV-050-BR",
"mpn": "BV050BR",
"gtin12": "012345678905",
"brand": {
"@type": "Brand",
"name": "Acme Flow Controls"
},
"description": "Full-port brass ball valve rated to 600 WOG, NPT threaded, for potable water and compressed air lines.",
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "18.42",
"availability": "https://schema.org/InStock",
"url": "https://distributor.example.com/p/bv-050-br"
}
}


```


Two things distributors get wrong here. First,` price` and` availability` only reflect reality if your storefront resolves list pricing (rather than "log in to see price") for at least the unauthenticated view — otherwise the structured data contradicts the visible page, a mismatch both Google and AI crawlers treat as a trust signal against you. Second,` gtin` and` mpn` require those fields to actually be populated on the product record; a blank field just drops out of the markup silently, so a Product schema can ship without its most useful identifiers and no one notices.


## Step 3: Make sure the content is actually server-rendered


Spire is a server-side-rendered React storefront, and[Optimizely's SSR guidance](https://docs.developers.optimizely.com/configured-commerce/docs/server-side-rendering-ssr-guidelines-for-spire) is explicit that product detail, brand, and product/category list pages should render server-side because they're what crawlers depend on. Configured Commerce also lets you defer catalog, pricing, or inventory data from server to client rendering for performance — useful for Core Web Vitals, but a crawler or agent that doesn't fully execute JavaScript can see a PDP shell with price or stock missing if those settings are tuned too aggressively. If real-time ERP pricing is slow, defer inventory, but keep name, attributes, description, and identifiers server-rendered.


Classic CMS handles this differently: it has a separate SEO rendering path (` /Views/SeoCatalog/SeoProductDetail.cshtml` ) that serves crawlers a lightweight, server-rendered version of the page independent of the client-side theme. Optimizely's own guidance is blunt about the tradeoff: these SEO views work out of the box, but if you customize the client-side product page template, you have to customize the matching SEO view too — otherwise the two versions quietly drift apart, and crawlers keep seeing the old one.


## Step 4: Answer the buyer's question in the copy, not just the spec sheet


Structured data tells an agent *what* a product is; the description and specification tabs need to answer *whether it fits the use case* . For distributor SKUs, that means writing to the actual buyer question — "rated for outdoor use?", "compatible with \[common mating part\]?", "max pressure?" — as a direct sentence near the top of the description, not just a row in an attribute table. Agents summarizing a page tend to quote sentences, not reconstruct them from rows.


## What an AI agent can and cannot extract


**Can extract reliably** , once the steps above are done: product name, SKU/MPN/GTIN, brand, price and stock status (if server-rendered with an unauthenticated fallback), attributes marked` Include On Product` , breadcrumb path, and any sentence-form answer in the description or a published specification tab.


**Cannot extract, or extracts unreliably** : facts that exist only inside an attached PDF; attributes assigned at the category level but never given a value on the product; pricing gated entirely behind login; anything rendered only after client-side hydration when catalog/inventory SSR has been deferred; and specification content still sitting Draft/unpublished, since only published content reaches the rendered page.


## How to validate


- **View-source vs. rendered DOM** : load the PDP with JavaScript disabled (or` curl -s https://yourdomain.com/p/sku | less` ) and confirm the product name, attributes, price, and JSON-LD block are present in the raw HTML, not only in the hydrated client render.
- **Structured data** : run the page through[Google's Rich Results Test](https://search.google.com/test/rich-results) and the[Schema.org Validator](https://validator.schema.org/) to confirm the` Product` type parses and required fields (` name` ,` offers` ,` brand` ,` gtin` /` mpn` ) are present.
- **Spot-check attribute coverage** : pick a handful of PDPs per category and diff the attributes shown against the attribute type list for that category — gaps usually mean a value was never set on the product record.
- **SEO/Classic parity** : if you're on Classic CMS, confirm` SeoProductDetail.cshtml` shows the same content set as the live theme by comparing a rendered page to its SEO-crawler equivalent.


Verified as of July 2026 against current Optimizely Configured Commerce (Configured Commerce SDK / Spire and Classic CMS) documentation; menu paths and toggle names can shift slightly between releases and plan tiers, so confirm against your instance's Site Configurations before rolling out broadly.


None of this matters if the underlying attribute data is thin, which is the more common failure mode on distributor catalogs than a missing JSON-LD toggle. Anglera enriches product records — attributes, specs, identifiers, use-case language — continuously, and pushes them into whatever PIM or commerce platform already holds your catalog, Configured Commerce included, without a rip-and-replace. Once that data is complete, the steps above put it in front of buyers and their agents.
