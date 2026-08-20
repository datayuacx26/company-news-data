---
schema_version: "1.0.0"
document_id: "344ba777b1487f0b33be5248f7e7d97fe8d2b60252d8af78c911c86ff9a9a178"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/give-ai-agents-markdown-they-actually-want/"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:9456da81498329ae119f7a9d3484d42bd8bfee2b32d1406601debb1269119a22"
---

# Give AI Agents the Markdown They Actually Want

AI crawlers are going to ask for your pages whether you're ready for them or not. Today they get HTML, same as any browser, and they spend CPU cycles stripping your nav and footer to find the article underneath. This post walks through a small Fastly Compute service that meets them in the middle: normal requests still get your site, while agents get a clean Markdown version of the same content.


We can easily accomplish this with about 200 lines of JavaScript, which you can find in the[repo here](https://github.com/fastly/html-2-md) . You can skim the pipeline section to see the shape, or clone and deploy if you want to get there faster.


## Why This Matters


Our own[Security Research report](https://learn.fastly.com/Security-Threat-Insights-Report) found that bots account for 49% of requests. The vast majority is unwanted traffic, and verified AI is only a sliver of what's left, but that sliver carries outsized business impact. A single hit from GPTBot, PerplexityBot, or ChatGPT-User isn't one user. It's every real user who'll eventually see your content through a large language model instead of on your site. Getting that experience right is worth a little engineering.


The problem with serving those crawlers HTML: they don't want it. LLM training pipelines and retrieval systems operate on text. So when a crawler pulls your product documentation and needs to turn it into answers, HTML is overhead for them. It has to be parsed, stripped of boilerplate, de-noised of tracking pixels and menu chrome, and flattened into plain text. Some of that cleanup is lossy, especially tables, code blocks, and footnotes, which often show up mangled in downstream summaries.


Markdown sidesteps most of that, it's what those existing pipelines already speak natively. And it's small, a typical article compresses to 20-30% of its HTML size, which means less bandwidth and fewer tokens burned on your structure instead of your ideas.


The catch is that rewriting everything to serve Markdown at origin isn't realistic for most teams, and you don't want to anyway. Browsers still need the HTML. What you want is a transform that runs on the request path, doesn't slow things down, and caches well so you're not paying for the same work twice.


## What We're Building


A small JavaScript service on[Fastly Compute](https://www.fastly.com/products/edge-compute) that sits in front of your origin and does three things based on who's asking:


-


A normal browser request gets HTML, passed through origin untouched.


-


An AI crawler user-agent (we detect 17 of them by default) or a request with` Accept: text/markdown` gets a Markdown version of the same page.


-


An explicit` /md/<path>` request always returns Markdown. Useful for debugging, internal tooling, and content teams who want to spot-check what crawlers see.


Here's what the output looks like for a request to` /md/blog/rate-limits` :


Copied!


```text
---
title: "Rate limits — API docs"
description: "How rate limits work, per-tier quotas, and the headers to inspect."
author: "Platform team"
date: "2026-03-02T00:00:00Z"
url: "https://example.com/docs/rate-limits"
source: "https://your-site.edgecompute.app/md/blog/rate-limits"
---


# Rate limits


Every API key is subject to a request budget per minute and per day...


## Quotas by tier


| Tier | Requests / min | Requests / day |
| --- | --- | --- |
| Free | 60 | 10,000 |
| Pro | 600 | 500,000 |
| Enterprise | Custom | Custom |
```


Clean headings, a real Markdown table, YAML frontmatter a downstream pipeline can parse without heuristics. Nav, footer, related-articles, newsletter prompts, inline scripts, are all stripped away.


## The Stack


Four pieces do all the work:


-


[Fastly Compute](https://docs.fastly.com/en/guides/about-compute) runs the whole thing as WebAssembly, close to the user. We use the JavaScript SDK (` @fastly/js-compute` ).


-


[linkedom](https://github.com/WebReflection/linkedom) parses the origin HTML into a DOM. It's a lightweight, standards-adjacent implementation that compiles cleanly to WASM, unlike jsdom, which pulls in a lot of Node-specific machinery.


-


[Defuddle](https://github.com/kepano/defuddle) extracts the main content. It's a newer extractor from the Obsidian Web Clipper team, purpose-built for agent-facing Markdown. It handles site-specific quirks (per-site extractors for known publications), standardizes code blocks and footnotes into consistent HTML, and falls back to heuristic scoring when it has to.


-


[Turndown](https://github.com/mixmark-io/turndown) walks the extracted DOM and emits Markdown. We add the GFM plugin for tables and strikethrough, plus one small custom rule to handle a linkedom quirk (more on that below).


Plus` fastly:cache` 's SimpleCache for edge caching, no other dependencies.


## The Conversion Pipeline


Everything that turns HTML into Markdown lives in one file,` src/converter.js` :


Copied!


```text
import Defuddle from 'defuddle';
import { parseHTML } from 'linkedom';
import TurndownService from 'turndown';
import { gfm } from '@joplin/turndown-plugin-gfm';


const turndown = new TurndownService({
headingStyle: 'atx',
codeBlockStyle: 'fenced',
bulletListMarker: '-',
});
turndown.use(gfm);


export function htmlToMarkdown(html, sourceUrl) {
const { document } = parseHTML(html);


const result = new Defuddle(document, { url: sourceUrl }).parse();
const articleDoc = parseHTML(result?.content || '').document;
const markdown = turndown.turndown(articleDoc.documentElement).trim();


if (!markdown) {
throw new Error('Could not extract readable content from page');
}


const frontmatter = buildFrontmatter(result, document, sourceUrl);
return `${frontmatter}\n\n${markdown}\n`;
}
```


The pipeline is linear: parse with linkedom, hand the` Document` to Defuddle, let Defuddle do its extraction and standardization, then re-parse its HTML output through linkedom one more time so Turndown has a real DOM node to walk. That second parse feels redundant, but it matters and we'll get to why in a moment.


The` buildFrontmatter` helper pulls title, description, author, and published date from Defuddle's metadata, falling back to standard` <meta>` tags when Defuddle doesn't have them. We also emit the canonical URL, so whatever consumes this Markdown can point back to the original page.


### The DOM-node-not-string gotcha


If you read Defuddle's docs, you'll notice a` markdown: true` option that looks like it should do everything Turndown does for us. It does in Node, but it doesn't in Compute.


The reason: Defuddle's built-in Markdown step calls` turndownService.turndown(htmlString)` . Turndown, given a string, parses it internally by calling` document.implementation.createHTMLDocument` . The Compute JS runtime is SpiderMonkey with linkedom providing the DOM, and linkedom doesn't expose` document.implementation` . Turndown throws, Defuddle swallows the throw, and you get a fallback message like "Partial conversion completed with errors" with the raw HTML appended.


Handing Turndown a DOM node sidesteps that parser entirely. It walks the tree we give it. That's why the second` parseHTML` call is there.


### The Table Rule


One more linkedom quirk:` HTMLTableElement.rows` isn't populated. The GFM plugin's table rule checks` node.rows\[0\]` to decide whether to convert the table or skip it, and since` rows` is undefined, every table becomes flattened text.


The fix is a small custom rule registered after GFM:


Copied!


```text
turndown.addRule('linkedom-table', {
filter: (node) => node.nodeName === 'TABLE',
replacement: (_content, node) => {
const rows = Array.from(node.querySelectorAll('tr'));
if (!rows.length) return '';
const cells = (tr) =>
Array.from(tr.querySelectorAll('th, td')).map((c) =>
c.textContent.replace(/\s+/g, ' ').trim().replace(/\|/g, '\\|'),
);
const header = cells(rows[0]);
const body = rows.slice(1).map(cells);
const sep = header.map(() => '---');
const fmt = (row) => `| ${row.join(' | ')} |`;
return `\n\n${[fmt(header), fmt(sep), ...body.map(fmt)].join('\n')}\n\n`;
},
});
```


` querySelectorAll('tr')` works where` .rows` doesn't. Since our custom rule is registered last, Turndown picks it over the GFM default. A few extra lines that save any page with a table.


## Routing and content negotiation


The Compute fetch handler lives in` src/index.js` . The whole routing layer is about 50 lines:


Copied!


```text
async function handleRequest(event) {
const req = event.request;
const url = new URL(req.url);


if (url.pathname === '/health') return jsonResponse({ status: 'ok' });
if (url.pathname === '/__html-2-md__') return landingResponse();


if (url.pathname.startsWith('/md/') || url.pathname === '/md') {
const originPath = url.pathname.replace(/^\/md/, '') || '/';
return await convertAndRespond(req, url, originPath);
}


const ua = req.headers.get('User-Agent') || '';
const accept = req.headers.get('Accept') || '';


if (isAiCrawler(ua) || wantsMarkdown(accept)) {
return await convertAndRespond(req, url, url.pathname);
}


return fetch(req, { backend: 'origin' });
}
```


Four decision points, in order. Health and debug routes are served locally. A` /md/<path>` prefix forces Markdown regardless of headers. After that, we look at the request: if it's from a known AI crawler or explicitly asks for Markdown, we convert. Otherwise, a straight pass-through to origin.


The crawler detection is a small list in` src/agents.js` , 17 user-agent patterns covering the mainstream ones: GPTBot, ChatGPT-User, ClaudeBot, anthropic-ai, PerplexityBot, GoogleOther, cohere-ai, and so on. It's a case-insensitive substring match. Agents evolve, so treat the list as a starting point and prune or extend based on what actually shows up in your logs.


### Caching


Markdown conversion takes a few hundred milliseconds on a cold request, most of it in Defuddle's scoring. That's fine for the first crawler hit, painful for the hundredth. SimpleCache turns it into a one-liner:


Copied!


```text
const cacheKey = `html-2-md:${originUrl.pathname}${originUrl.search}`;
const cached = SimpleCache.get(cacheKey);


if (cached) {
body = await cached.text();
} else {
body = await fetchAndConvert(originUrl, url);
SimpleCache.set(cacheKey, body, CACHE_TTL); // 5 minutes
}
```


Five minutes is a reasonable default for most content sites, just tune it to how often you publish. The cache is per-POP, so you'll see a cold conversion per region on first request, then cached responses after.


We also set` Vary: Accept, User-Agent` on the response. Any downstream caches (yours, the crawler's) will respect the same content negotiation we do.


## Testing Locally


The converter is a pure function, HTML in, Markdown out. That makes it trivial to test with plain Node, no Compute runtime required:


Copied!


```text
import { test } from 'node:test';
import assert from 'node:assert/strict';
import { htmlToMarkdown } from '../src/converter.js';


test('docs page: preserves tables and nested lists', async () => {
const html = await readFile('test/fixtures/docs-page.html', 'utf8');
const md = htmlToMarkdown(html, 'https://example.com/docs/rate-limits');


assert.match(md, /# Rate limits/);
assert.match(md, /\|\s*Tier\s*\|/);  // markdown table header
assert.match(md, /\|\s*Free\s*\|\s*60\s*\|/);
});
```


Drop a handful of representative fixtures into` test/fixtures/` (a blog post, a docs page with tables, a news article with boilerplate), and assert on the properties you care about. Our companion repo ships with three.` npm test` runs in about 200ms, which means you can iterate on extraction quirks without rebuilding WASM.


For the full edge pipeline,` fastly compute serve` boots Viceroy (Fastly's local Compute emulator) on` 127.0.0.1:7676` :


Copied!


```text
curl -s "http://127.0.0.1:7676/" -H "Accept: text/markdown" | head -30
curl -s "http://127.0.0.1:7676/" -H "User-Agent: GPTBot/1.0" | head -30
curl -s "http://127.0.0.1:7676/md/blog/my-post" | head -30
curl -sI "http://127.0.0.1:7676/"   # confirm HTML pass-through
```


Point` \[local_server.backends.origin\]` in` fastly.toml` at whatever origin you want to proxy, and you've got a working end-to-end loop.


## Deploying


Same two commands as any other Compute service:


Copied!


```text
npm run build        # compile to bin/main.wasm
fastly compute deploy
```


First run prompts you to create a service and configure your production origin backend. After that, you've got a Compute endpoint that'll respond at` <service>.edgecompute.app` . Point a custom domain at it, or front it with your existing Fastly service as a shielding config, whichever fits your topology.


## What's actually happening on the wire


For a request from GPTBot to` /blog/my-post` :


1.


Compute gets the request. User-Agent matches` GPTBot` → route to conversion path.


2.


Check SimpleCache for` html-2-md:/blog/my-post` . Miss.


3.


Fetch HTML from origin (the` origin` backend declared in` fastly.toml` ).


4.


Parse with linkedom → run Defuddle → re-parse → Turndown → frontmatter.


5.


Store in SimpleCache with 5-minute TTL. Return.


6.


Response:` Content-Type: text/markdown` ;` charset=utf-8` ,` Vary: Accept` ,` User-Agent` ,` X-Markdown-Tokens: <estimate>` .


For a regular browser hitting the same URL at the same time, step 2 is skipped entirely. They get HTML straight from origin, same as always.


## Where to Take it From Here


A few directions worth considering once it's running:


**Token counting:** Our heuristic (` length / 4` ) is a rough approximation of GPT-style tokenization. If you care about accurate accounting, swap in a real tokenizer. There are WASM-compatible tiktoken builds that work in Compute.


**Link rewriting:** The current output preserves relative URLs from origin, which means a crawler has to resolve them against the request URL. You can rewrite relative links to absolute inside the Defuddle result before Turndown runs it.


**Per-site extractors:** Defuddle supports custom extractors for sites with unusual structure. If you're proxying a specific publication or docs site, writing a one-off extractor produces much cleaner output than the generic heuristics.


**Streaming:** For very long articles, the current implementation buffers the whole body before emitting the response. Streaming the conversion would reduce TTFB. It's more complex (Defuddle wants the full document to score) but feasible by chunking on section boundaries.


**Rate limiting by agent:** If you want to serve GPTBot but throttle a noisier bot, pair this service with our[Edge Rate Limiting](https://docs.fastly.com/products/edge-rate-limiting) offering.


## Wrapping up


Serving Markdown to AI agents is one of those small efforts that can have an outsized impact. It respects the agent’s workload, but also *your* bandwidth (and ultimately your bottom line). Compute is a good fit for it because the work is close to the request, cacheable, and measured in milliseconds. What you want is a transform that runs on the request path, doesn't slow things down, and caches well so you're not paying for the same work twice.


Feel free to clone the service[here](https://github.com/fastly/html-2-md) . If you build something interesting on top of this (a token counter, a custom extractor, a link rewriter), we'd like to hear about it.
