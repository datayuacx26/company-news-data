---
schema_version: "1.0.0"
document_id: "c0033eee20b702aa167b300366c93f2ca27d96aac1aecd935460728f324a5b80"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/unilog-ssr-rendering"
published_at: "2026-06-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:411d0a6d68e60250ce829e3c49151466293a505a7ec28eb9695f4c4d7cdfe07c"
---

# Server-side rendering on Unilog: making product data visible to Google and AI

Distributors running Unilog's CX1/CIMM2 platform usually get the base product template server-rendered, but real-world sites layer in search widgets, personalization overlays, and custom theme scripts that can quietly push attribute data into a client-side render pass. The distinction matters more than it used to: search engines eventually run JavaScript, but the major AI crawlers currently don't render it at all, so whatever isn't in the first HTML response may never reach either audience. Here's how to check what your Unilog instance is actually shipping, and how to fix it if the answer is "not enough."


## How Unilog product pages typically render


CX1/CIMM2 is built on a traditional server-rendered architecture: product, category, and brand pages resolve through numeric-ID routes (for example` /2871737/product/mcguckin-adt5000nav-sm` or` /52247781/brand/elkhart-products/` ), which is a routing convention typical of server-templated .NET applications rather than a client-side single-page app. In the default setup, the PIM-managed title, description, specifications, and images that live in CIMM2's product content module get compiled into the HTML template on the server before the response goes out — no browser JavaScript execution required to see them.


Where this gets muddied is in per-implementation customization, which is common on Unilog because most distributor sites run custom themes and bolt-on modules:


- **Search and faceted navigation widgets.** Unilog's own materials describe using on-site search and facets to "auto-generate a massive number of SEO-impactful dynamic pages" for category and brand landing pages. If that facet/search layer is a third-party JavaScript widget (Unilog integrates with providers like HawkSearch) rendering results client-side after the page loads, those generated pages can look empty to anything that doesn't execute JS.
- **Personalization and pricing overlays.** Customer-specific pricing and inventory pulled from an ERP or POS system is often fetched via an AJAX call after initial page load, which is appropriate for account-gated pricing — but if the underlying spec/attribute block is bundled into that same deferred call instead of the base template, it disappears along with the pricing.
- **Custom theme scripts.** Agencies and internal teams frequently inject descriptions, spec tables, or badges via tag-manager scripts or theme-layer JavaScript for design reasons. Convenient to build, invisible to non-rendering crawlers.


Because rendering behavior depends on your specific theme, template overrides, and installed widgets, don't assume based on the platform name alone — verify your own instance using the steps below.


## Why this matters for crawlers and AI agents differently


Google documents a three-phase pipeline for JavaScript-heavy pages: crawl, render, index. Googlebot fetches the URL, queues it for rendering, and only when Chromium executes the JavaScript does the rendered HTML get parsed and indexed — and Google is explicit that "server-side or pre-rendering is still a great idea because it makes your website faster for users and crawlers, and not all bots can run JavaScript" ([Google Search Central, JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ). Rendering is also queued separately from crawling, so JS-dependent content can be discovered and indexed on a real delay relative to your server response.


The gap is starker for AI agents. Independent traffic analysis published by Vercel found that none of the major AI crawlers — OpenAI's[GPTBot](https://www.anglera.com/glossary/gptbot) and[OAI-SearchBot](https://www.anglera.com/glossary/oai-searchbot) , Anthropic's[ClaudeBot](https://www.anglera.com/glossary/claudebot) , Meta-ExternalAgent, ByteDance's Bytespider, and[PerplexityBot](https://www.anglera.com/glossary/perplexitybot) — currently render JavaScript; they fetch and parse raw HTML only, occasionally downloading a JS file without executing it ([Vercel, "The rise of the AI crawler"](https://vercel.com/blog/the-rise-of-the-ai-crawler) ). Practically: if your spec table, materials, dimensions, or compliance attributes only exist after a client-side render pass, Googlebot will eventually see them (with a lag), but ChatGPT, Claude, and Perplexity answering a buyer's sourcing question will not — they see whatever arrived in the initial response and nothing more.


## What "good" looks like on a Unilog page


The base, non-personalized product content — title, long description, attributes/specs, category and brand context, images with real` src` URLs and alt text, and identifiers (MPN, UPC/[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) ,[manufacturer part number](https://www.anglera.com/glossary/mpn-manufacturer-part-number) ) — should all be present in the raw HTML the server returns, before any script runs. It's fine, and often correct, for volatile data like live inventory count or account-specific net pricing to arrive via a follow-up call; that data changes per buyer and per second, and isn't the kind of thing you want cached in a crawl anyway. The failure mode to avoid is bundling static attribute content into that same deferred call for convenience.


It's also worth adding` Product` structured data server-side, since Unilog's default templates don't guarantee schema markup out of the box:


```text
{
"@context": "https://schema.org",
"@type": "Product",
"name": "Elkhart 3/4 in. Copper Pressure Fitting",
"sku": "EP-10142334",
"mpn": "10142334",
"description": "Wrot copper pressure coupling for potable water and HVAC piping systems.",
"brand": { "@type": "Brand", "name": "Elkhart Products" },
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"availability": "https://schema.org/InStock"
}
}


```


## How to validate


Compare the raw server response against the fully rendered DOM to find the gap:


```text
# 1. What the server actually sends, no JS executed
curl -s -A "Mozilla/5.0 (compatible; GPTBot/1.0; +https://openai.com/gptbot)" \
https://yourdomain.com/12345/product/example-sku | grep -i "specifications\|description"


# 2. Compare against Googlebot's own view via Search Console
# GSC > URL Inspection > "View Crawled Page" > Screenshot / HTML tabs


```


Then check view-source (` view-source:https://yourdomain.com/...` , which shows the unexecuted HTML) against the browser's rendered DOM in DevTools (Elements panel, which shows the post-JS state). If a spec table or description appears in DevTools but not in view-source or the curl output, that content is client-rendered only. Run the page through Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm both the rendered HTML and any` Product` schema are actually detected, and re-check after any theme or search-widget change — a redesign or new merchandising widget can silently move content from server to client rendering.


**Verified as of July 2026:** platform-specific behaviors (default template rendering, search widget integrations, theme customization options) are based on Unilog's public product materials and case-study sites; because Unilog doesn't publish a formal rendering specification and behavior varies by theme and installed modules, treat the checks above — not the platform name — as the source of truth for your own site.


None of this replaces the harder problem: having complete, accurate attributes, specs, and identifiers to put on the page in the first place. That's the side Anglera works on — continuously enriching product data in your PIM or feed so the fields above (specs, use-cases, identifiers) actually exist and stay current, without requiring a rip-and-replace of Unilog. Once that data is enriched, the rendering checks in this guide are how you confirm it's actually reaching buyers and AI agents, not just sitting in the PIM.
