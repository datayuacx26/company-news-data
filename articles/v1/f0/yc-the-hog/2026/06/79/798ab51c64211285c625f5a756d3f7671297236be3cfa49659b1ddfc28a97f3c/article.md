---
schema_version: "1.0.0"
document_id: "798ab51c64211285c625f5a756d3f7671297236be3cfa49659b1ddfc28a97f3c"
company_key: "yc-the-hog"
company: "The Hog"
source_id: "yc-the-hog-news-import-764edb381fb6"
canonical_url: "https://thehog.ai/blog/structured-web-context-for-agents"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-22T16:24:44.963526+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:fd1bf14161da45de789e2145de98677913bc762040b0d8a3ae1c4431fba7ebca"
---

# Structured web context for AI agents

Agents got smarter, but they still can't really *see* the live web. Drop a raw HTML page into a context window and you've spent thousands of tokens on navigation chrome, cookie banners, and markup before the model reaches a single useful fact. The fix isn't a bigger context window — it's **better context** .


## The shape of good context


Good agent context has three properties. It's **fresh** (minutes old, not months), **structured** (typed fields, not prose to re-parse), and **scoped** (only what the task needs). The Hog is built around delivering exactly that.


Here's how a few common signals map to where they come from:


Field Source Freshness


Company Open web search Real-time


Headcount Enrichment Daily


Tech stack Page scrape + inference On request


News Monitored sources Streaming


## A worked example


Say your agent qualifies inbound leads. Instead of handing it a homepage, hand it a structured record:


```text
const   company   =   await   hog.enrich.  company  ({ domain:   "example.com"   });


if   (company.headcount   >   50   &&   company.hiring) {
await   routeToSales  (company);
}
```


The agent reasons over clean fields —` headcount` ,` hiring` ,` funding` — and never touches a line of HTML.


## Keep the loop tight


A few principles we've found hold up in production:


1. **Fetch narrowly.** Ask for the fields the task needs, nothing more.
2. **Cache aggressively.** Most context is reusable across runs within a session.
3. **Monitor, don't poll.** Subscribe to changes instead of re-scraping on a timer.


> The best agent context is the smallest set of fresh, structured facts that lets the model make the next decision.


That's the bar we hold every endpoint to.
