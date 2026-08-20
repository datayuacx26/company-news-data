---
schema_version: "1.0.0"
document_id: "5997e8dc327c928349e13b1f672fd98ff29bb91edbd90af1cf33c2ac61bf700b"
company_key: "yc-quicknode"
company: "Quicknode"
source_id: "yc-quicknode-news-import-d0b2f3935987"
canonical_url: "https://www.quicknode.com/blog/go-filters-and-ai-filter-agent-for-streams"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T14:56:00.405612+00:00"
fetched_at: "2026-08-11T14:56:01.660454+00:00"
content_hash: "sha256:1b572202a538edee04ac1c2f09298d86ac60eab1ad51afffe4e7391bf5f611d5"
---

# Go Filters and the AI Filter Agent for Quicknode Streams | Quicknode

#


Write Streams Filters in Go. Or Don't Write Them at All


[Quicknode](https://www.quicknode.com/blog/author/quicknode)


August 10, 2026 — 3 min read


Quicknode Streams filters can now be written in Go, and Quicknode's AI Filter Agent turns a plain-language prompt into an optimized filter. Both are live in the filter editor today.


### **Faster filters, especially on fast chains**


Go filters run in-process, 30% faster, especially on high-throughput chains like Solana and Robinhood.


The gain comes from the runtime. Running a filter inside an isolated VM adds parsing and execution overhead to every invocation. Go filters skip the VM, so that overhead is gone, and at high block rates the saving compounds. The heavier the filter, the bigger the difference. The gap is widest on compute-intensive logic and large block payloads.


### **Filter your stream without writing code**


Streams filters are getting easier to configure and deploy, based on user feedback. Quicknode's AI Filter Agent writes the filter for you.


Open the filter editor and describe what you want in plain language:


*"filter all transactions and token transfers that involve addresses in my 'wallets' list"*


The agent returns an optimized, annotated filter using the correct fields for your selected network and dataset. It has context on the filter API, per-chain payload schemas, Key-Value Store capabilities and best practices, and a library of real-world filter templates.


Read every line. Adjust anything you want.


### **Migrating from JavaScript**


If you're filtering with JavaScript and want to migrate to Go for faster filtering, use the "Migrate to a Go filter" option. AI converts your current logic to Go automatically.


Review the generated code, confirm the logic and return values match your JavaScript filter, then publish.


### **Getting started**


1. Open the Streams filter editor and select Go as the filter language.


2. Describe the filter you want, or migrate an existing one.


3. Review the output and publish.


The docs at[quicknode.com/docs/streams/filters](http://quicknode.com/docs/streams/filters) now carry tabbed Go and JavaScript examples.


### **Streams already running in production**


pay3 replaced wallet addresses with a single shareable link for stablecoin payments. Streams watches 7,000+ registered addresses across Ethereum, BNB Smart Chain, Solana, and Tron, and fires a webhook roughly two seconds after a payment confirms onchain. One pipeline, four chains, no separate tooling per chain.
[Read the pay3 case study](https://www.quicknode.com/case-studies/pay3-real-time-multi-chain-payments-on-streams)


Defimon monitors EVM chains for exploits and alerts protocol teams the moment one lands. Filtering by address, contract, and event type before data reaches their pipeline, with watchlists in Key-Value Store lists instead of in memory, took block-to-detection latency from 2 seconds on their previous polling setup to under 0.5 seconds. They went from 3 chains to 8, from 3-4 engineers to 1, and cut infrastructure costs by $48,000 a year.


[Read the Defimon case study](https://www.quicknode.com/case-studies/defimon-real-time-defi-security-with-streams)


Both are address and event-list workloads, the same shape as the 'wallets' example above. The AI Filter Agent writes that first draft for you now, and Go runs it 30% faster.


### About Quicknode


Founded in 2017, Quicknode deploys institutional-grade blockchain infrastructure for developers and enterprises. With 99.99% uptime and support for 80+ chains, teams build and scale onchain applications without compromise.


[Start building today](https://www.quicknode.com/)
