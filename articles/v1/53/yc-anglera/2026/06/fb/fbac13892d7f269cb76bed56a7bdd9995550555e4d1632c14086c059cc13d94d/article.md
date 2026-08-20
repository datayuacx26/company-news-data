---
schema_version: "1.0.0"
document_id: "fbac13892d7f269cb76bed56a7bdd9995550555e4d1632c14086c059cc13d94d"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/validate-agent-readable-product-data"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:7cb4c0f6b37319d58d633aefd525d60174035353041da6b294ca23a4c5d39480"
---

# How to validate that your product data is agent-readable (tools, curl, view-source)

Enriching a PDP with specs, use-cases, and identifiers doesn't help anyone if the page never delivers that data to the thing reading it. Search crawlers, LLM answer engines, and shopping agents don't see your page the way you do in a browser tab — some never run JavaScript at all, and even the ones that do apply their own rendering budget and timing rules. This guide walks through the concrete checks you can run in the next twenty minutes to confirm your product facts actually reach a crawler or agent, not just a human with a fully-loaded browser.


## Why this isn't optional


A page can look perfect in Chrome and still be functionally blank to a crawler if the content is injected client-side after an API call finishes. Google is explicit that it processes JavaScript pages in distinct crawl, render, and index phases, and that if content isn't present in the rendered HTML, it won't be indexed — the same logic applies to any agent that fetches a URL without executing your bundle ([Google Search Central, JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ). Most non-Google AI crawlers are even more conservative about rendering, so the gap between "what a human sees" and "what a bot receives" tends to be wider, not narrower, once you leave Googlebot.


## Step 1: See what the server actually sends


Before touching a browser, ask curl to fetch the page with no JavaScript engine at all — this is the closest approximation to what a lightweight crawler receives on the first pass.


```text
curl -sL -A "Mozilla/5.0 (compatible; validation-check/1.0)" https://example.com/products/widget-100 -o raw.html
grep -o "<script type=\"application/ld+json\">" raw.html


```


If your product's[JSON-LD](https://www.anglera.com/glossary/json-ld) , price, availability, and identifiers ([GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) /MPN/SKU) show up in` raw.html` , they're present in the initial response — good news for any agent that doesn't render JavaScript. If the body element is mostly empty divs and script tags, your content is client-side rendered and you're depending entirely on the crawler executing JavaScript, which many AI agents do not do.


It's also worth pointing curl at real crawler user-agent strings to rule out user-agent-based cloaking or bot-blocking rules that silently serve a different (often thinner) response:


```text
curl -sL -A "Mozilla/5.0 AppleWebKit/537.36 (KHTML, like Gecko); compatible; GPTBot/1.3; +https://openai.com/gptbot" https://example.com/products/widget-100 -o gptbot.html
diff raw.html gptbot.html


```


OpenAI publishes exact user-agent strings for its three crawlers —` GPTBot` (training),` OAI-SearchBot` (ChatGPT search citations), and` ChatGPT-User` (on-demand fetches when a user asks ChatGPT to read a page) — and documents that each can be allowed or blocked independently in` robots.txt` ([OpenAI, Overview of OpenAI crawlers](https://developers.openai.com/api/docs/bots) ). Confirm you're not accidentally disallowing the crawler you want:


```text
User-agent: OAI-SearchBot
Allow: /products/


User-agent: GPTBot
Allow: /products/


```


## Step 2: Compare view-source with the rendered DOM


view-source (` view-source:https://example.com/...` in Chrome, or` curl` ) shows the raw HTML your server returned, before any JavaScript runs. The rendered DOM — what you get from "Inspect" → Elements, or from a headless-browser fetch — shows the page after your framework has hydrated it. For a JavaScript-heavy PDP these can differ substantially, and Google's own guidance is direct on this point: the initial HTML often doesn't contain the real content, and Google needs to execute JavaScript before it can see what the page actually renders ([Google Search Central, JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ).


Practical check: open view-source on a product page and search (Cmd/Ctrl+F) for a distinctive spec value, like a model number or a materials callout. If it's not in view-source but is in the rendered DOM, that fact only reaches crawlers and agents willing to render JavaScript — and many aren't.


## Step 3: Validate the structured data itself


Once you know your JSON-LD is present in the HTML a crawler receives, validate that it's syntactically correct and complete:


- **Rich Results Test** (` search.google.com/test/rich-results` ) parses your structured data, renders the page's JavaScript the way Googlebot does, and reports exactly which rich result types (Product, Review, FAQ, etc.) the page qualifies for, along with missing required fields ([Google Search Central, Rich Results Test](https://support.google.com/webmasters/answer/7445569) ). Note that this checks structural validity, not whether your JSON-LD matches what's visibly on the page — a stale price in your markup will pass even if it doesn't match the displayed price.
- **Schema Markup Validator** (` validator.schema.org` ) is the Schema.org project's own validator, built from what was previously Google's Structured Data Testing Tool. It extracts and checks JSON-LD, Microdata, and RDFa from a URL or pasted code, which is useful when you want to validate markup independent of any single search engine's rich-result eligibility rules ([Schema.org, Markup Validator](https://validator.schema.org/) ).


For a Product node, confirm the required and strongly recommended fields are present and populated with real values, not placeholders:


```text
{
"@context": "https://schema.org",
"@type": "Product",
"name": "Widget 100",
"sku": "WID-100",
"gtin13": "0012345678905",
"brand": { "@type": "Brand", "name": "Acme" },
"offers": {
"@type": "Offer",
"price": "49.99",
"priceCurrency": "USD",
"availability": "https://schema.org/InStock"
}
}


```


Google's own product structured data guide covers the fields required for merchant listing and product snippet eligibility, and recommends supplementing on-page markup with a Merchant Center feed where relevant for extra confidence in the data ([Google Search Central, Product structured data](https://developers.google.com/search/docs/appearance/structured-data/product) ).


## Step 4: Cross-check with Search Console


The URL Inspection tool's "View Crawled Page" shows the rendered HTML Google actually indexed for that URL — not your view-source HTML. Search that rendered HTML (Ctrl/Cmd+F) for the product facts you care about; if they're missing there, they're missing from Google's index regardless of what a human sees in the browser ([Search Console Help, URL Inspection tool](https://support.google.com/webmasters/answer/9012289) ). This step catches rendering-timeout issues that curl and view-source alone won't reveal, since Search Console actually executes your JavaScript with a Googlebot-configured renderer.


## How to validate


1. Run` curl -sL` against the product URL and check the raw response for JSON-LD and key spec text — no JavaScript involved.
2. Repeat the curl request with a crawler user-agent string ([GPTBot](https://www.anglera.com/glossary/gptbot) ,[OAI-SearchBot](https://www.anglera.com/glossary/oai-searchbot) , ChatGPT-User, and any of your own known bots) to rule out UA-based blocking or cloaking.
3. Open view-source on the product URL and search for a distinctive spec value; compare against the same search in the rendered DOM (browser Inspect panel).
4. Run the URL through the[Rich Results Test](https://search.google.com/test/rich-results) and the[Schema Markup Validator](https://validator.schema.org/) — resolve every error, and treat warnings as a checklist for richer eligibility.
5. In Search Console, use URL Inspection → "View Crawled Page" to confirm the product facts survived Google's own rendering pass.
6. Spot-check` robots.txt` for each[AI crawler](https://www.anglera.com/glossary/ai-crawler) you want to reach; a blanket` Disallow: /` under a wildcard user-agent will silently exclude all of them.


**Verified as of July 2026:** tool URLs, user-agent strings, and Search Console workflow reflect current published documentation; Google and OpenAI periodically revise crawler versions and rendering behavior, so re-check the linked docs if something here stops matching what you observe.


None of this replaces having accurate, complete product data to put on the page in the first place — validation only tells you whether what you already have made it through. That's the problem Anglera is built to solve upstream: it continuously enriches attributes, specs, and identifiers in your PIM or commerce platform so there's a complete Product record to render, then leaves the page-side implementation — including everything above — to your existing stack.
