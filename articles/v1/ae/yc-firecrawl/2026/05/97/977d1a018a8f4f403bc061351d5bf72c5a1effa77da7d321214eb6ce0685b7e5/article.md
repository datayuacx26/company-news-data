---
schema_version: "1.0.0"
document_id: "977d1a018a8f4f403bc061351d5bf72c5a1effa77da7d321214eb6ce0685b7e5"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/firecrawl-vercel-marketplace"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T22:12:43.834196+00:00"
content_hash: "sha256:0fa0ed1fbd54b59f89e3e1884b29603ae923fca5c40e8d7ad3704cdbaba45ec3"
---

# Firecrawl is now live on the Vercel Marketplace

AI apps and agents built on Vercel often need web data: a page to scrape, a search to run, a dynamic site to interact with. Until now, that meant a separate account, separate billing, and manually copying API keys. **Firecrawl is now a native integration on the Vercel Marketplace.**


## What the Vercel + Firecrawl integration sets up


The Vercel Marketplace integration handles Firecrawl account provisioning, API key management, and billing automatically. Here is what happens when you install:


- Provisions a Firecrawl team and API key automatically
- Injects` FIRECRAWL_API_KEY` directly into your Vercel project's environment variables (no copy-paste)
- Adds Firecrawl to your Vercel invoice, so there is no separate vendor or payment method to manage
- Gives you SSO into the Firecrawl dashboard from Vercel, with no separate login


Plan changes, API key rotation, and uninstall all work through the Marketplace the same way.


## How to get started


1. Go to the[Vercel Marketplace](https://vercel.com/marketplace/firecrawl) and find Firecrawl.
2. Pick a plan: Hobby, Starter, Growth, or Scale.
3. Select the Vercel project to attach it to.
4. Done.` FIRECRAWL_API_KEY` is live in your project environment.


## What can you build with Firecrawl on Vercel?


Firecrawl gives your Vercel app three endpoints for pulling data from the web. It covers AI agents that browse the web, RAG pipelines that stay fresh, and SaaS tools that extract structured data:


- ` search` — search the web and get full page content from results in a single call
- ` scrape` — convert any URL into LLM-ready Markdown, structured JSON, or screenshots
- ` interact` — scrape a page, then click, scroll, or fill forms to reach dynamic content


Here is what teams typically build:


Use case Endpoint Description


RAG pipelines with fresh web data` scrape` Pull docs, changelogs, or any public page into LLM-ready Markdown on a schedule.[RAG pipelines](https://www.firecrawl.dev/blog/10-ai-projects-with-firecrawl) stay current without manual re-ingestion.


Research and deep research` search` Returns complete page content per result. A[deep research agent](https://www.firecrawl.dev/blog/open-deep-research-explainer) can search a topic, retrieve full articles, and synthesize across sources in one pass.[Stanford's AI Playground](https://www.firecrawl.dev/blog/customer-story-stanford) processes 800 sources daily this way for real-time LLM grounding.


Real-time product and news monitoring` search` Runs on each request so your app always reflects current data. Surface product launches, competitor updates, or breaking news as they happen.


Data extraction for AI-native SaaS` scrape` +` interact`` scrape` returns structured JSON you can validate with Zod or Pydantic.` interact` handles clicks and scrolls for dynamically loaded content.


## Try it today


Firecrawl is live on the Vercel Marketplace now. Install takes under a minute and your API key lands in your project environment automatically.


[Install on Vercel Marketplace](https://vercel.com/marketplace/firecrawl) ·[Read the docs](https://docs.firecrawl.dev/quickstarts/vercel-marketplace)
