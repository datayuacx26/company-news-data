---
schema_version: "1.0.0"
document_id: "8a6080cf37ae9d809bffc94817581cfca2bb1781e5dcdd14f18ced1efc7421ad"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/ssr-vs-csr-prerender-agent-readable"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:43.834196+00:00"
content_hash: "sha256:5822938946197bd6a38bbe8ce4edea2ddea384f104919b17f13fc689e25f1e8b"
---

# SSR vs CSR vs pre-rendering: which makes your PDP agent-readable

Rich product data only helps buyers and AI agents if it lands in the HTML a crawler receives. This guide walks through the three rendering models your product page can use, which ones put your enriched attributes, specs, and identifiers into the document a bot can read without running JavaScript, and how to check what you're actually shipping today.


## The three models, plainly


**Client-side rendering (CSR):** the server sends a near-empty HTML shell (usually a single empty root container element) plus a JavaScript bundle. The browser downloads the bundle, executes it, fetches data from an API, and builds the DOM in place. Nothing meaningful exists in the HTML response itself — it's all assembled after the fact, in the client.


**Server-side rendering (SSR):** the server executes your application code on every request and returns fully-formed HTML, with the actual product title, price, and description already in the markup. The client then "hydrates" that HTML — attaching event listeners and JavaScript so the page becomes interactive — but the content was already there before hydration ran.


**Pre-rendering / static generation (SSG):** the same idea as SSR, except the HTML is built once, at deploy or build time (or on a schedule/webhook), and served as a static file from a CDN. It's cheaper and faster than SSR because nothing runs per-request, but content can lag behind reality until the next build, so it suits pages that don't change every few minutes.


Frameworks blur these lines on purpose. Next.js's own docs group SSG (built once, reused per request) and SSR (built per request) under one umbrella term, "pre-rendering," and let you mix both page by page ([Next.js rendering docs](https://nextjs.org/docs/pages/building-your-application/rendering) ); its current App Router goes further, letting a static shell and per-request dynamic pieces ship in the same response. The framework label matters less than one question: **is the product data already in the HTML the server sends, before any JavaScript runs?**


## What Google actually sees


Google's own documentation describes crawling and indexing as three sequential phases: crawl, render, index. Googlebot fetches the URL, and if it returns a 200 status and isn't blocked, the page is queued for rendering. A headless, evergreen Chromium instance (the Web Rendering Service) then executes the JavaScript, and Google indexes the *rendered* HTML — not the raw response ([JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ).


Two details matter for a PDP:


- **The rendering queue isn't instant.** Google's docs note a page "may stay on this queue for a few seconds, but it can take longer than that" before Chromium actually runs your JS. If your data depends on client-side fetches, there's a real-world delay between crawl and the point your content is visible to indexing — and a re-crawl may be needed if data changes in between.
- **Dynamic rendering (serving bots a separately pre-rendered version) is explicitly discouraged.** Google's own guidance calls it "a workaround and not a long-term solution," recommending SSR, static rendering, or hydration instead ([Dynamic rendering as a workaround](https://developers.google.com/search/docs/crawling-indexing/javascript/dynamic-rendering) ). Cloaking-adjacent bot-detection schemes add complexity and risk; a page that renders the same way for everyone is the safer, simpler target.


So Google *can* read CSR content, eventually, if the render succeeds and nothing times out. That's a bet, not a guarantee — worse still for a PDP with hundreds of SKUs needing re-render every time price or stock changes.


## What AI agents see — and it's stricter


This is where the gap widens. Independent analyses of large-scale crawl logs report that the major AI crawlers — OpenAI's[GPTBot](https://www.anglera.com/glossary/gptbot) /[OAI-SearchBot](https://www.anglera.com/glossary/oai-searchbot) , Anthropic's[ClaudeBot](https://www.anglera.com/glossary/claudebot) , and[PerplexityBot](https://www.anglera.com/glossary/perplexitybot) — do not execute JavaScript; they read the raw HTML response and move on. One frequently cited analysis of over 500 million GPTBot fetches found no evidence of JS execution, even though these bots do fetch` .js` files at meaningful rates ([Vercel: the rise of the AI crawler](https://vercel.com/blog/the-rise-of-the-ai-crawler) ). Google's Gemini is the exception, since it can reuse Googlebot's Web Rendering Service — but that's an outlier, not the norm.


Practically: if your title, price, specs, and availability only appear after a client-side fetch resolves, most AI answer engines and shopping agents never see them. CSR-only PDPs are functionally invisible to that traffic, however rich the underlying data.


## Choosing a strategy for your PDP


A decision framework that holds up across platforms:


1. **Core PDP content — title, price, availability, description, specs, images, reviews summary — belongs in the initial HTML.** That means SSR or pre-rendering, not CSR. Server-rendered templating engines produce full HTML per request by default; headless/composable storefronts need this decision made explicitly.
2. **Fast-changing, per-visitor data (a specific warehouse's live stock, personalized pricing, "12 people viewing this") is fine as CSR** — no crawler expects a bot-specific answer for it anyway.
3. **If your catalog changes constantly, prefer SSR or incremental static regeneration over pure build-time SSG.** A price change that takes an hour to propagate through a rebuild pipeline is worse than a few hundred milliseconds of server render time.
4. **Never rely on dynamic rendering (serving bots a separate pre-rendered copy) as the fix.** Fix the rendering path, not the bot detection.


Structured data ([JSON-LD](https://www.anglera.com/glossary/json-ld) for` Product` ) should follow the same rule: it needs to be in the HTML response, not injected by a client-side script after mount. Google's Product structured data documentation describes two related paths — product snippets and Merchant Center-linked "merchant listings" — and points to the Rich Results Test and Search Console to confirm eligibility ([Product structured data](https://developers.google.com/search/docs/appearance/structured-data/product) ). Both tools check the rendered/served HTML, not your source code, so a script tag added by client JS after the fact must already be present by the time they fetch the page.


```text
<script type="application/ld+json">
{
"@context": "https://schema.org/",
"@type": "Product",
"name": "Example Product Name",
"sku": "EX-1234",
"description": "Server-rendered description text.",
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "129.00",
"availability": "https://schema.org/InStock"
}
}
</script>


```


## How to validate


Don't trust a browser's Inspect Element panel — it shows the DOM *after* JavaScript ran. Check what arrives before any script executes:


```text
# Raw HTML exactly as a non-rendering crawler receives it
curl -s https://example.com/products/widget-1234 | less


# Look specifically for product data in that raw response
curl -s https://example.com/products/widget-1234 | grep -i "application/ld+json" -A 20


```


- **View-source vs rendered DOM:** open` view-source:https://example.com/products/...` (raw HTML) and compare it to the Elements panel (post-hydration DOM). If title, price, and JSON-LD appear only in the second, that content is CSR.
- **Google Search Console → URL Inspection → "View crawled page"** shows Google's own rendered HTML and a screenshot — confirmation of what Googlebot resolved after running JS, beyond curl.
- **Rich Results Test** validates whether` Product` structured data is present and well-formed, and surfaces it as missing if it's injected too late or blocked by consent/JS gating.
- Disable JavaScript (DevTools → Command Menu → "Disable JavaScript," reload) and confirm the PDP still shows a name, price, and description. If it's blank, that's the page an[AI crawler](https://www.anglera.com/glossary/ai-crawler) sees.


## Verified as of July 2026


Google's crawl/render/index model, its discouragement of dynamic rendering, and its Product structured data guidance are drawn from current Google Search Central documentation. AI-crawler JavaScript behavior (GPTBot, ClaudeBot, PerplexityBot) reflects third-party crawl-log analysis rather than an official spec, since none of those companies publish a formal rendering policy; treat it as directionally reliable and re-check periodically, since crawler behavior can change without notice.


None of this matters if there's nothing worth rendering. Anglera enriches the underlying product data — attributes, specs, use-cases, identifiers — continuously, in whatever PIM, commerce platform, or metafield store you already run, so the SSR or pre-rendered HTML your engineering team ships has rich, accurate content to put on the page.
