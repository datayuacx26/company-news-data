---
schema_version: "1.0.0"
document_id: "9149c0615c3e786731cd2062f240e36c63bf360bd5490b627b954d0b40d2d350"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/rendering-pitfalls-hide-product-data"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:428a7a2d260781bcd73cd1de4ac28dec77e4ec9f101048eb11a7c7789daf74bd"
---

# Rendering pitfalls that hide product data from crawlers and agents

Enriching a product page is only half the job. If the specs, use-cases, and identifiers you've populated never make it into the HTML a crawler or AI agent receives, none of that work gets read. Googlebot renders JavaScript before indexing, but most AI crawlers and answer engines fetch raw HTML and stop there — so a page that "looks fine" in a browser can be functionally empty to the systems deciding what gets cited or ranked. Below are five common ways product data goes missing between the database and the DOM, how to catch each one, and how to fix it without a platform rewrite.


## Why this matters more for AI agents than for Google


Googlebot processes pages in three phases — crawl, render, index — using an evergreen headless Chromium, so it eventually executes your JavaScript ([Google's JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) ). AI crawlers generally don't get that far. Independent traffic analyses of[GPTBot](https://www.anglera.com/glossary/gptbot) and[ClaudeBot](https://www.anglera.com/glossary/claudebot) consistently find them issuing plain HTTP requests and parsing whatever HTML comes back, without executing scripts. Anthropic separately documents ClaudeBot as one of three distinct crawlers it operates, alongside Claude-User and Claude-SearchBot ([Claude Help Center: does Anthropic crawl the web](https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler) ). A rendering gap that costs a Google impression today can cost an AI Overview or a chat citation entirely — there's no rendering step to close the gap later.


## Pitfall 1: Client-only rendering (empty initial HTML)


**The problem.** In a fully client-side-rendered (CSR) app, the server returns a near-empty HTML shell, and the product title, price, specs, and description only appear after JavaScript bundles download and execute. Any crawler that doesn't run JS sees nothing but the shell.


**How to detect it.**


```text
curl -s https://example.com/products/widget-100 | grep -i "widget-100"


```


If the product name, SKU, or price aren't in that raw response, they only exist post-render. You can also compare view-source against the rendered DOM in DevTools — a large gap between the two is the signature of CSR.


**The fix.** Move to server-side rendering (SSR), static generation, or hybrid rendering so the initial HTML already contains title, price, availability, key specs, and identifiers ([GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) /MPN), with JavaScript layered on top for interactivity. Google now recommends SSR, static rendering, or hydration over client-only rendering, and treats "dynamic rendering" (serving crawlers a separate pre-rendered copy) as a deprecated workaround — it adds a second code path and does nothing for non-rendering AI crawlers ([Google: dynamic rendering as a workaround](https://developers.google.com/search/docs/crawling-indexing/javascript/dynamic-rendering) ).


## Pitfall 2: Lazy-loaded specs and images that never fire for crawlers


**The problem.** Lazy loading helps page speed, but common implementations trigger on scroll events. Googlebot doesn't scroll like a user — it simulates a tall viewport and never fires scroll events — so scroll-triggered lazy loading can leave specs, tables, or images unloaded in the rendered DOM. Non-rendering AI crawlers never trigger any of it, since they don't execute the loading script at all.


**How to detect it.** In Chrome DevTools, throttle or disable JavaScript and reload, or use Search Console's URL Inspection Tool to view the rendered HTML/screenshot Google actually captured — content missing from that rendered snapshot is content Google never indexed either.


**The fix.** Use the native` loading="lazy"` attribute for images and iframes where possible, since it's parsed as HTML rather than JS-triggered, and reserve` IntersectionObserver` -based loading for elements needing custom behavior — firing on visibility, never on click or scroll ([Google: fix lazy-loaded content](https://developers.google.com/search/docs/crawling-indexing/javascript/lazy-loading) ). Anything crawlers must see regardless of viewport — spec tables, structured data, identifiers — shouldn't be lazy-loaded at all.


## Pitfall 3: Content locked behind JS-driven tabs and accordions


**The problem.** Google has long said that content already in the HTML at load time (e.g., a "Specifications" panel with` display:none` ) is crawled and indexed at full weight, even collapsed by CSS. The real risk is different: specs or use-case content fetched via a separate AJAX call only when a user clicks a tab. That content isn't in the DOM at all until the click fires — invisible to Googlebot's non-interactive crawl and to every non-rendering[AI crawler](https://www.anglera.com/glossary/ai-crawler) .


**How to detect it.** View-source or` curl` the page and search for the spec text. Present in a collapsed panel is fine. Only injected after a click handler fires an XHR/fetch call is missing.


**The fix.** Render all tab and accordion panel content into the initial HTML and use CSS, not conditional JS fetches, to show and hide it. This keeps progressive-disclosure UX while guaranteeing the content ships on first response — a practice Google's guidance confirms is safe for indexing.


## Pitfall 4: Blocked resources that break rendering entirely


**The problem.** A` robots.txt` that disallows` /js/` ,` /assets/` , or a bundler's chunked script paths prevents Googlebot from fetching files it needs to render the page. Google is explicit that it won't render JavaScript from blocked files or pages, so a disallowed CSS or JS path can leave Googlebot indexing a broken, content-thin version, even though a human visitor sees the full thing.


**How to detect it.** Check` robots.txt` for disallowed script/style paths, and use the URL Inspection Tool's rendered screenshot in Search Console — a visibly broken layout there is a strong signal that a required resource is blocked.


**The fix.** Any resource required to render page content or layout should not be disallowed, even if you intentionally block other paths ([Google: how Google interprets robots.txt](https://developers.google.com/search/docs/crawling-indexing/robots/robots_txt) ). It's a one-line fix that's easy to overlook after a bundler or CDN path change.


## Pitfall 5: Slow hydration that times out the render


**The problem.** Even with SSR, server-rendered markup isn't fully "live" until client-side hydration attaches event handlers and, in some setups, re-renders parts of the DOM. If hydration is slow — large JS bundles, waterfalled data fetches, heavy component trees — a crawler's rendering budget can expire before hydration-dependent content (price toggles, variant selectors, specs populated post-hydration) finishes. Google's Web Rendering Service also doesn't persist local storage, session storage, or cookies between page loads, so product data gated behind client-side state may never render for a crawler.


**How to detect it.** Run Lighthouse or PageSpeed Insights for Time to Interactive and Total Blocking Time, and compare the URL Inspection Tool's rendered HTML against production.


**The fix.** Reduce what depends on hydration to render at all — ship specs, identifiers, and pricing directly in server-rendered markup rather than a post-hydration fetch. Where interactivity is still needed, adopt partial or progressive hydration, or an islands-style architecture, so static product data is present immediately while only interactive widgets wait on JS.


## How to validate


- **View-source vs. rendered DOM** : a plain` curl` request (or "View Page Source") shows what non-rendering crawlers get. Compare it against the DevTools "Elements" panel to see what only appears after JS runs. Re-running` curl` with a specific crawler's user-agent string can sanity-check the exact bytes that bot receives.
- **Search Console URL Inspection Tool** : shows Google's actual rendered HTML, a screenshot, console errors, and blocked resources — the single best source of truth for what Googlebot saw.
- **Rich Results Test** : confirms structured data ([JSON-LD](https://www.anglera.com/glossary/json-ld) ) is present and parseable in the rendered output, useful when JSON-LD is injected via JavaScript instead of emitted server-side.
- **robots.txt check** : confirm no script, style, or data-fetch endpoint the page depends on is disallowed.


Verified as of July 2026 against current Google Search Central documentation and Anthropic's published crawler behavior; consult your specific platform's rendering mode (SSR/SSG/ISR/CSR) documentation, since exact defaults vary by framework and version.


None of this fixes what's missing from the data itself — a well-rendered page still needs specs, use-cases, and identifiers to put in that HTML. That's the side Anglera handles: it continuously enriches[product attributes](https://www.anglera.com/glossary/product-attributes) in your PIM or commerce platform, so once rendering is sound, there's rich, structured content ready to ship in the first server response instead of a partial record waiting on a client-side fetch.
