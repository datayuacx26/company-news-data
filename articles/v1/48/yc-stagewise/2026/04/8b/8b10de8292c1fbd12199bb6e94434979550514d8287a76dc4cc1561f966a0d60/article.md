---
schema_version: "1.0.0"
document_id: "8b10de8292c1fbd12199bb6e94434979550514d8287a76dc4cc1561f966a0d60"
company_key: "yc-stagewise"
company: "stagewise"
source_id: "yc-stagewise-news-import-0e327623c986"
canonical_url: "https://stagewise.io/news/release-week-april-6-12"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-22T14:44:14.475585+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:c847a2e72ff02473cd69100e6573de4b240cb63383b0d70c481669081f0b9e18"
---

# Release Week: April 6–12 · stagewise Newsroom

## What Shipped Last Week


### ` /preview` for fast mockups and experiments


The most useful addition this week is **` /preview`** . You can ask the agent to generate a small app that shows a mockup, pulls in data from the page you’re looking at, or tests an idea before you touch the real codebase. It’s a quick way to explore directions in context instead of describing them abstractly.


These previews are basically *throwaway mini-websites* the agent builds on the fly. You can iterate on them, use them to decide what should become a real change, and discard them when they’ve done their job.


### Avoiding accidental agent deletion


We also fixed a sharp edge in the product: deleting an agent used to happen immediately. That was too easy to trigger by mistake, especially when the cost is losing the conversation with it.


Now there’s a **confirmation dialog** before deletion. Small change, obvious in hindsight, and much better.


### Performance improvements across the app


A lot of the work this week went into **responsiveness** . We’ve been tightening up the UI so the app feels quicker when you move around, interact with agents, and work through longer sessions.


This kind of work usually doesn’t make for dramatic release notes. It does make the product feel less sticky and less fragile, which matters more.


### Windows path and tooling fixes


We also fixed a set of Windows issues in the agent tooling. Some Windows users were running into path resolution problems in the sandbox and related tools, which could break tasks that depended on correct file handling.


Those fixes should make Windows workflows *much more reliable* , especially when the agent needs to move across files, tools, and generated outputs.


## What the Week Adds Up To


This was a week of making stagewise easier to trust: quicker ways to try ideas, fewer destructive mistakes, a faster UI, and fewer platform-specific failures.
