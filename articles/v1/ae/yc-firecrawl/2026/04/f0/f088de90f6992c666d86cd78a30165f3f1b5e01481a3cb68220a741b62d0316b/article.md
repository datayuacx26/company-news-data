---
schema_version: "1.0.0"
document_id: "f088de90f6992c666d86cd78a30165f3f1b5e01481a3cb68220a741b62d0316b"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/lockdown-mode-launch"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:a669c0f115dbccdef6138d9739ceb722bb2884150e41a705b5f287b004e893aa"
---

# Lockdown Mode: /scrape Without Touching the Web

**Today we're shipping Lockdown Mode: Firecrawl's new cache-only scrape mode for security-sensitive workloads.** It's a single flag on` /scrape` that serves results exclusively from Firecrawl's existing index - the request never leaves Firecrawl and nothing is retained. Available everywhere` /scrape` is: the API, every SDK, the CLI (` --lockdown` ), and the MCP server.


## The problem


Every live scrape is an outbound request to a third-party origin. For most workloads that's fine - but when an LLM agent decides which URLs to scrape, a prompt injection can turn that outbound request into a data exfiltration channel: sensitive context smuggled out in a path, query string, or header to a server the attacker controls. The same outbound request is also a problem for regulated teams who need every external call logged, approved, or simply prevented.


Lockdown Mode solves this with a single flag that serves results from Firecrawl's existing cache and guarantees the request never leaves Firecrawl.


## What is Lockdown Mode?


Lockdown Mode is a cache-only scrape mode for the` /scrape` endpoint. Pass` lockdown: true` and Firecrawl serves the result exclusively from its existing index - no connection to the target URL, no robots.txt fetch, no search-index write, no audio transforms. Every outbound path is gated at the engine layer.


```text
firecrawl   scrape   https://example.com   --lockdown
```


If the URL is in the cache, you get the result. If it isn't, the request returns a` SCRAPE_LOCKDOWN_CACHE_MISS` error rather than falling back to a live scrape. This is by design. When the URL isn't cached, you know immediately - no silent fallback to a live scrape.


It's particularly useful for regulated-industry teams, LLM agents that need guardrails, and any workflow where the URL itself is sensitive data.


## How Lockdown Mode protects sensitive scrape jobs


### No outbound request


All external operations are disabled at the engine layer on the same flag: HTTP engines, robots.txt fetching, search index writes, audio transforms. There's one auditable enforcement point, not a checklist of settings.


For regulated environments where outbound requests need approval or logging, this is the difference between an auditable workflow and a manual review process.


### Zero data retention by default


Every lockdown request carries ZDR semantics automatically. The URL is never persisted, the response is never stored, and the scrape job is cleaned up immediately after delivery.


The standard ZDR pricing uplift is waived. Compliance and security don't cost extra.


### One flag for every surface


` lockdown: true` works the same way across every surface Firecrawl supports:


- **API** -` POST /scrape` with` { "lockdown": true }`
- **Python, Node, Go, Rust, Java, .NET, Ruby, PHP, Elixir SDKs**
- **CLI** -` firecrawl scrape --lockdown`
- **MCP server** - same flag, same behavior


No version requirements, no additional configuration. If you can call` /scrape` , you can use Lockdown Mode.


## Use cases


- **Regulated-industry scraping** : Healthcare, finance, legal, and government teams scraping documentation, regulatory pages, or partner sites where every outbound request needs audit or approval.
- **Agent guardrails** : Pin LLM agents to an already-indexed corpus so untrusted user input can't trigger outbound requests to arbitrary origins.
- **Sensitive-URL workflows** : Scrape URLs that themselves leak intent - competitor pages, internal hostnames, identifiers embedded in paths - without those URLs ever crossing the network.
- **Deterministic replay** : Serve a stable, indexed snapshot of pages back to downstream pipelines without re-hitting origins or paying live-scrape costs.


## A few things to know


- **Cache miss returns an error, not a fresh scrape.** To serve a URL via Lockdown Mode, scrape it normally once first to seed the cache.
- **Not for fresh or time-sensitive data.** Lockdown returns cached results up to 2 years old. News feeds, live prices, and dashboards may return stale content.
- **Conflicting options are silently ignored.**` actions` ,` waitFor` , custom headers, proxy configuration, and` changeTracking` are dropped when lockdown is active - lockdown wins at the engine layer.
- **` /scrape` only.** Lockdown Mode is not currently supported on` /crawl` ,` /map` , or` /search` .


## Try it today


Lockdown Mode is live for all Firecrawl users.


Lock down your most sensitive scrape jobs with` lockdown: true` .


[Get started with Lockdown Mode](https://www.firecrawl.dev/) ·[Read the docs](https://docs.firecrawl.dev/features/lockdown)
