---
schema_version: "1.0.0"
document_id: "991dcd22128a6b4124b850d24e9ef82f3609b6d88cd9db377591c6aaf998b9d1"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/json-ld-vs-on-page-what-agents-read"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:267dba155b91ab8a223bc7138739ab5d8f5be5ccf2b792baa50f06e7978f7f89"
---

# JSON-LD vs microdata vs on-page text: what AI agents actually read

Product pages get read by three different consumers now: shoppers, traditional search crawlers, and AI agents that summarize or cite your page without ever showing a screenshot to a human. Each of the three markup approaches below —[JSON-LD](https://www.anglera.com/glossary/json-ld) , microdata/RDFa, and plain visible text — gets treated differently by each reader. Here's what actually gets parsed, what gets weighted, and how to keep all three consistent so nothing you mark up gets ignored or, worse, flagged.


## The three formats, briefly


**JSON-LD** is a block of JSON dropped into a script tag (` type="application/ld+json"` ), usually in the document head or right before the closing body tag. It describes the page's entities (a Product, its Offer, its Review aggregate) independently of the surrounding HTML.


```text
<script type="application/ld+json">
{
"@context": "https://schema.org",
"@type": "Product",
"name": "12mm Torque Wrench, 1/2-inch Drive",
"sku": "TW-4412-12",
"gtin12": "012345678905",
"brand": { "@type": "Brand", "name": "Acme Tools" },
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "89.99",
"availability": "https://schema.org/InStock"
}
}
</script>


```


**Microdata** (and its cousin RDFa) embeds the same vocabulary as attributes —` itemscope` ,` itemtype` ,` itemprop` — directly on the visible HTML elements:


```text
<div itemscope itemtype="https://schema.org/Product">
<h1 itemprop="name">12mm Torque Wrench, 1/2-inch Drive</h1>
<span itemprop="sku">TW-4412-12</span>
<div itemprop="offers" itemscope itemtype="https://schema.org/Offer">
<span itemprop="price" content="89.99">$89.99</span>
<meta itemprop="priceCurrency" content="USD">
</div>
</div>


```


**Visible on-page text** is just the rendered copy a shopper reads: the title, the bullet points, the spec table, the description paragraph. No markup required, but no machine-readable structure either.


## What Google's crawler parses and weights


Google explicitly supports all three formats — JSON-LD, microdata, and RDFa are "equally fine for Google, as long as the markup is valid" — but recommends JSON-LD because it's easiest to implement and maintain at scale and least prone to breaking when a template changes ([Google Search Central: Intro to structured data](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data) ). Structured data feeds Rich Results (star ratings, price, availability badges) and helps Google understand entities on the page, but it is a supplement to the visible text, not a replacement for it — Google still indexes and ranks primarily on rendered content.


The rule that matters most operationally: Google's structured-data policy states, "Don't mark up content that is not visible to readers of the page." If your JSON-LD Product entity describes a color, price, or availability state that isn't also present in the rendered HTML, that's a policy violation that can cost you rich-result eligibility or trigger a manual action ([Google Search Central: General Structured Data Guidelines](https://developers.google.com/search/docs/appearance/structured-data/sd-policies) ). Practically: JSON-LD is a mirror of the visible page, not an appendix to it.


## What AI agents actually read


This is where the three formats diverge sharply from the SEO case. AI crawlers and answer engines — OpenAI's[GPTBot](https://www.anglera.com/glossary/gptbot) and[OAI-SearchBot](https://www.anglera.com/glossary/oai-searchbot) , Perplexity's[PerplexityBot](https://www.anglera.com/glossary/perplexitybot) , and similar bots — are documented by their operators as fetching pages for indexing or training and can be allowed or disallowed independently via` robots.txt` ([OpenAI: Overview of OpenAI Crawlers](https://developers.openai.com/api/docs/bots) ). What's consistently reported by the SEO and web-infrastructure community operating against these bots is that they behave like lightweight HTTP fetchers, not full browsers: they request the URL, take whatever HTML comes back on that first response, and move on — they don't wait around to execute your JavaScript bundle, hydrate a single-page app, or run a second rendering pass the way Googlebot's Web Rendering Service does. If your JSON-LD or your product copy is injected client-side after page load, a large share of AI traffic never sees it.


That gives JSON-LD two jobs when an AI agent is the reader. First, it has to be present in the initial server response, not injected by client-side JavaScript. Second, because it's a clean, self-contained JSON object, it's the cheapest thing on the page for a model to extract a fact from — no need to walk a DOM tree, strip nav and footer boilerplate, or guess which element holds the price. Microdata and plain visible text both require more parsing work to separate signal (the actual spec) from noise (the surrounding template), which matters when a bot is budget- or time-constrained.


Visible text still carries weight of its own kind: it's the fallback and the corroboration. An agent that can't or doesn't parse your JSON-LD will fall back to reading the rendered text, and an agent that does read your JSON-LD will (implicitly or explicitly) sanity-check it against the surrounding copy. A page where the JSON-LD says "in stock" and the button says "sold out" is a page an agent has good reason to distrust.


## Where each format fits


- **New builds, headless storefronts, PIM-driven templates:** JSON-LD, generated server-side from the same data source that renders the visible page. This is the only approach that scales cleanly across thousands of SKUs without a developer hand-editing HTML attributes per product.
- **Legacy themes already using microdata or RDFa:** valid, still supported, no urgent need to rip out — but don't add new attribute-based markup to new templates, and don't let it drift out of sync with copy changes, since it's harder to audit than a single JSON block.
- **Anything rendered only in the browser:** move it server-side (or use static-site generation / SSR) if you want AI agents — or any non-JS-executing crawler — to see it at all.


## How to validate


- **View-source vs. rendered DOM:**` curl -s https://example.com/product/sku | grep -A5 'application/ld+json'` shows exactly what a non-JS-executing bot receives. Compare that to what you see in Chrome DevTools' Elements panel (the rendered DOM) — if the JSON-LD only appears in DevTools and not in the curl output, it's client-side injected and invisible to most AI crawlers.
- **Google's Rich Results Test** (search.google.com/test/rich-results) fetches and renders the page the way Googlebot does, then reports which rich-result types it detects — useful for catching syntax errors and missing required fields, though it won't tell you what a non-rendering AI bot sees.
- **Field-by-field parity check:** for every property in your JSON-LD (price, availability,[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) , name), confirm the same value is visible somewhere in the rendered page text. This is the single highest-leverage check for both Google's spam policy and AI-agent trust.


## Verified as of July 2026


Google's format support and content-matching policy are drawn from current Google Search Central documentation. AI-crawler behavior (no JavaScript execution, robots.txt–gated access) reflects OpenAI's published crawler overview plus consistent, widely corroborated reporting from the web-infrastructure community; treat specifics as subject to change and re-check each bot's own documentation before relying on them for a launch.


None of this works if the underlying data is thin — a clean JSON-LD block around a one-line description doesn't give an AI agent much to cite. Anglera enriches the[product attributes](https://www.anglera.com/glossary/product-attributes) , specs, and use-case detail that feed both the visible copy and the JSON-LD on a page, continuously, from whatever PIM or commerce platform you already run — so the page-side work above has something substantive to render in the first place.
