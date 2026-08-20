---
schema_version: "1.0.0"
document_id: "65b30af0599fe29f6af0c812d2236852b0bc8c2f34c7033fbb9fca443ac8e81d"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/claude-code-codex-web-data-layer-anakin-mcp"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-21T06:45:54.754455+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:3cd1445b6e37be03bb9953e07fe16f525e08ae8ec7615fa6071ce550f333d9cd"
---

# Anakin MCP - How to give Claude Code and Codex a real web data layer

**TL;DR:**


- Claude Code and Codex have built-in web tools, but they're limited: they return links and titles rather than real content, struggle with JavaScript-heavy sites, and aren't available on all platforms.
- Anakin's remote MCP connector fills that gap with three capabilities: search that returns actual content snippets, full-page scraping including SPAs and anti-bot sites, and Wire - a catalog of pre-built actions for pulling structured data from sites that never shipped an API.
- One URL to connect, works in every MCP client.


## The problem with your AI harness's web tools


Most agentic harnesses - Claude Code, Codex, Cursor - need live data and web access. The built-in tools inside those harnesses don't fully cover this.


Claude Code ships with two web tools: WebSearch and WebFetch.


### WebSearch


WebSearch runs through Anthropic's server-side infrastructure. That's also why it's unavailable on Bedrock and Vertex - if you're on either platform, the tool isn't exposed at all.


When WebSearch returns results, you get titles and URLs. Not page content. The agent has to infer what's on those pages before it can reason over them.


### WebFetch


WebFetch checks whether a domain is safe to fetch, retrieves the page, and summarizes it using a secondary Haiku 4.5 model scoped to your query. That keeps context overhead manageable - full pages can run 10-100 KB, and pushing them into the main model is expensive.


The tradeoff: you get one page at a time, you get a model summary rather than the original content, and anything behind a JavaScript render or anti-bot layer fails silently.


### How Codex handles it


Codex controls web access through a top-level` web_search` config key with three modes:


- ` cached` - serves results from OpenAI's web index (default)
- ` live` - fetches from the live web; same as running` --search`
- ` disabled` - turns the tool off


` cached` is fine for well-indexed topics. For anything time-sensitive or niche, you'll want` live` - but that's a configuration decision your team has to make, not something the agent manages for you.


## What Anakin's MCP connector adds


Native web tools are designed to look things up. Anakin's remote MCP connects to your agent and turns websites into clean, structured data.


### Connect by URL - no install, no key-pasting


```text
claude mcp add --transport http anakin https://mcp.anakin.io/mcp
```


One command registers the remote endpoint. Anakin handles OAuth at login - no API keys in config files.


### It does things native tools can't


Native search returns links. Native fetch returns one summarized page. The Anakin connector returns clean structured data - either human-readable markdown or AI-structured JSON - from any URL you point at it.


### It reaches the hard web


Native fetch fails on JavaScript-heavy sites and anything behind anti-bot defenses. Anakin handles browser rendering, proxy routing, DataDome bypass, and authenticated sessions in its own infrastructure. Your agent doesn't manage any of that.


### One endpoint, every client


The same URL works in Claude Code, Claude Desktop, Cursor, Cline, Windsurf, Zed, VS Code, and the Anthropic API directly. Set it up once.


## The three tools your agent gets


### 1. Search


Built-in search returns titles and links, then fetches pages one at a time. Anakin's` search` tool returns the URL, title, and a content snippet in a single call - your agent can reason over actual text immediately without chaining additional requests.


```text
# Request
curl -X POST https://api.anakin.io/v1/search \
-H 'X-API-Key: your_api_key' \
-H 'Content-Type: application/json' \
-d '{"prompt": "EU AI Act compliance deadlines 2025", "limit": 5}'


# Response
{
"id": "63385e99-3ef5-4667-84a7-e7b398ec8e06",
"results": [
{
"url": "https://example.com/article",
"title": "EU AI Act Key Deadlines",
"snippet": "The EU AI Act enters into force in stages, with the first obligations applying from August 2025...",
"date": "2025-01-15"
}
]
}
```


The agent gets ranked results with the text already attached. One call, no follow-up fetches needed.


### 2. Scrape / Crawl


Native fetch returns one summarized page and fails silently on anything JavaScript-heavy. Anakin's` scrape` tool returns the rendered page as clean markdown. Pass` useBrowser: true` and it spins up a headless browser for SPAs.


```text
# Request
curl -X POST https://api.anakin.io/v1/url-scraper \
-H 'X-API-Key: your_api_key' \
-H 'Content-Type: application/json' \
-d '{"url": "https://example.com/pricing"}'


# Response (202 - job queued)
{"jobId": "job_abc123xyz", "status": "pending"}


# Poll for result
curl https://api.anakin.io/v1/url-scraper/job_abc123xyz \
-H 'X-API-Key: your_api_key'


# Response (200 - completed)
{
"id": "job_abc123xyz",
"status": "completed",
"markdown": "# Pricing\n\nStarter plan: ...",
"html": "<html>...</html>"
}
```


Once` status` moves to` completed` , you get back` markdown` and` html` fields for the full rendered page.


The scraper is async by design - it polls internally so your agent doesn't have to manage retry logic.


The` crawl` tool works the same way across multiple pages, with` includePatterns` and` excludePatterns` to scope what gets crawled.


### 3. Wire Catalog


Wire is a catalog of pre-built actions for sites that never shipped an API. Instead of scraping HTML and guessing at structure, your agent calls a Wire action built for that specific site and gets back clean, typed JSON.


```text
wire_discover     → find actions for a site or intent
wire_catalog      → browse the full catalog (800+ sites, 4,000+ actions)
wire_read_action  → run a read action and get structured results
```


Example prompt:


*Use a Wire action to get the top products in Walmart's electronics category and list their prices.*


The agent calls a Wire action built for Walmart and gets back structured results - no HTML parsing, no selector maintenance.


## Benchmark: how Anakin's scraper stacks up


We ran a benchmark of Anakin's scraper against Firecrawl, ZenRows, Scraper API, ScrapingBee, and Tavily across 24 URLs - static pages, JavaScript-heavy SPAs, Cloudflare-protected sites, Akamai-protected e-commerce listings, and news pages.


Anakin led on success rate. It's not the fastest - the async model trades raw latency for reliability, and that tradeoff is intentional. The tool is built specifically for Cloudflare-protected and JavaScript-heavy pages where synchronous scrapers fail.


Worth noting: this is a self-published benchmark, not an independent one. You can inspect the methodology and run it yourself:


[github.com/Anakin-Inc/scraper-benchmark](https://github.com/Anakin-Inc/scraper-benchmark)


## Setup


### Claude Code


**Add the server:**


```text
claude mcp add --transport http Anakin https://mcp.anakin.io/mcp
```


**Authenticate:**


Run` /mcp` inside a Claude Code session. Select Anakin and complete the OAuth flow in the browser.


**Verify:**


Run` /mcp` again to confirm the connection. You should see:` scrape` ,` crawl` ,` map` ,` search` ,` agentic_search` , plus the Wire set:` wire_catalog` ,` wire_discover` ,` wire_identities` ,` wire_read_action` .


### Codex


```text
codex mcp add anakin --url https://mcp.anakin.io/mcp
codex mcp login anakin
codex mcp list
```


Restart Codex after adding the server so the new session picks up the tools.


### Cursor


Add to` ~/.cursor/mcp.json` (global) or` .cursor/mcp.json` (project-level):


```text
{
"mcpServers": {
"anakin": {
"url": "https://mcp.anakin.io/mcp"
}
}
}
```


Restart Cursor fully. In **Settings - Tools & MCP** , Anakin will show "Needs authentication." Click **Connect** , sign in at anakin.io, and the OAuth token is stored. You'll see` scrape` ,` crawl` ,` search` ,` wire_catalog` , and the rest.


### Claude Desktop


Open **Settings - Connectors - Add custom connector** . Paste` https://mcp.anakin.io/mcp` as the server URL. Leave the OAuth fields blank - Anakin handles auth automatically when you click **Add** .


Sign in with Google or email and approve the consent screen. In any chat, click + at the lower-left, choose Connectors, and toggle Anakin on. The tools are available for that conversation.


## Get started


The web wasn't built for agents. Native web search is a solid first step, but when your agent needs actual page content, sites that require a real browser, or structured data from a site without an API - it hits a wall.


Wire is how you fix that.[Get started at anakin.io/products/wire](https://anakin.io/products/wire) - 300 free credits, no card required. Already have an account? Check the[Wire catalog](https://openwire.sh/#catalog) to see if your site is already covered.


---


[Back to blog](https://anakin.io/blog)
