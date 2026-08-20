---
schema_version: "1.0.0"
document_id: "cecbbe04bd1b84d95c4e7a9f139c708a09897c4ab766b5cfe00ac69c2a95fd98"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/firecrawl-search-openrouter"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:fc730a36dbf2b888f36ea1a6812bd0809021c88cda9e75a412a27d1e9fa38dd6"
---

# Firecrawl /search is now available on OpenRouter

**Firecrawl is now a web search engine on OpenRouter.** When your model reaches for the web, it gets the full page, rendered and converted to clean markdown, not a results list. Unlike most web search integrations, which hand your model a page of snippets to guess from, Firecrawl fetches the actual source pages live.


\[ OpenRouter credit offer \]


The launch offer has expired. Enable Firecrawl /search and get


10,000 free credits


- no separate Firecrawl account needed.


[Enable Firecrawl on OpenRouter](https://openrouter.ai/docs/guides/features/plugins/web-search#firecrawl)


## What is Firecrawl /search on OpenRouter?


When you set[Firecrawl](https://firecrawl.dev/) as your Web Search engine in OpenRouter's Plugin settings, any model you run that uses web search gets Firecrawl's full extraction pipeline behind it. Firecrawl fetches the actual pages, renders JavaScript, strips nav, footers, ads, and cookie banners, and passes clean markdown to the model as context.


The model sees the *real* *page.* That means it can reason over full articles, documentation, pricing pages, or research papers, not just the fragments a typical search index surfaces.


No separate Firecrawl account is required. OpenRouter auto-provisions one linked to your email. The launch offer that included a free Hobby plan and 100,000 credits has expired; new accounts get **10,000 free credits** .


## What you get with Firecrawl on OpenRouter


### Full context, not snippets


Standard search integrations pass a results page to the model. Firecrawl fetches the actual source pages - renders JavaScript, strips boilerplate, and converts content to clean markdown. The model receives the content of the pages themselves, not a list of titles and descriptions to guess from.


That makes a real difference for agentic workflows. An agent doing market research, financial analysis, or competitive intel can pull the actual article or report mid-conversation and reason over it directly - no parsing step, no cleanup, no second tool in the chain.


### No setup beyond a single toggle


There are two paths to enabling it:


1. In OpenRouter's BYOK settings, open the Web Search tab and accept the Firecrawl Terms of Service.
2. Or go to Plugin settings and set Firecrawl as your Web Search engine.


Either way, that's the entire setup. No API key to copy, no reformatting results, no extra code to wire into your prompts.


### Works with every model on OpenRouter


Firecrawl works with any model available on OpenRouter: GPT, Claude, Gemini, Llama, and open-source models. The web search layer stays consistent regardless of which model is active, which matters when you're running evals or A/B testing models. If the output changes when you swap models, you know it's the model - not the search stack.


## How the pipeline works


1. The model decides a web search is needed and issues a query
2. OpenRouter routes the query to Firecrawl
3. Firecrawl fetches the live pages, renders JavaScript, and strips boilerplate
4. Clean markdown is passed back to the model as context
5. The model answers using actual page content


## Try it today


Firecrawl /search is live on OpenRouter now. Two paths to enable it:


- [OpenRouter BYOK settings](https://openrouter.ai/settings/byok) → Web Search tab → accept Firecrawl Terms of Service
- [OpenRouter Plugin settings](https://openrouter.ai/docs/guides/features/plugins/web-search#firecrawl) → set Firecrawl as your Web Search engine


New sign-ups get 10,000 free credits. No Firecrawl account needed, OpenRouter provisions it automatically.


[Get started with Firecrawl search](https://openrouter.ai/docs/guides/features/plugins/web-search#firecrawl) ·[Read the docs](https://docs.firecrawl.dev/introduction)
