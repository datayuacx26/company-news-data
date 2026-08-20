---
schema_version: "1.0.0"
document_id: "b7c4185a43baa41b4ac66725a5f23d63d85b491b3cfdf1951364b7df1abed38d"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/web-search-subagents-claude-code"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:54800f7bb35508ed4356283740f2c29944d1d5acb2f680ff6beb7569f31036ae"
---

# Subagents and web search in Claude Code

Ollama now supports subagents and web search in Claude Code. No MCP servers or API keys required.


## Get started


```text
ollama launch claude --model minimax-m2.5:cloud


```


It works with any model on Ollama’s cloud.


## Subagents


Subagents can run tasks in parallel, such as file search, code exploration, and research, each in their own context.


Longer coding sessions stay productive. Side tasks don’t fill the context with noise.


Some models will naturally trigger subagents when needed (minimax-m2.5, glm-5, kimi-k2.5), but you can force triggering subagents by telling the model to “use/spawn/create subagents”


Example prompts:


```text
> spawn subagents to explore the auth flow, payment integration, and notification system


> audit security issues, find performance bottlenecks, and check accessibility in parallel with subagents


> create subagents to map the database queries, trace the API routes, and catalog error handling patterns


```


## Web search


Ollama’s[web search](https://ollama.com/blog/web-search) is now built into the Anthropic compatibility layer. When a model needs current information, Ollama handles the search and returns results directly without any additional configuration.


Subagents can leverage web search to research topics in parallel and come back with actionable results.


```text
> research the postgres 18 release notes, audit our queries for deprecated patterns, and create migration tasks


> create 3 research agents to research how our top 3 competitors price their API tiers, compare against our current pricing, and draft recommendations


> study how top open source projects handle their release process, review our CI/CD pipeline, and draft improvements


```


## Recommended cloud models


- ` minimax-m2.5:cloud`
- ` glm-5:cloud`
- ` kimi-k2.5:cloud`


## Learn more


- [ollama launch](https://ollama.com/blog/launch) for more integrations
- [Claude Code with Ollama](https://ollama.com/blog/claude) for basic setup
- [Web search API](https://ollama.com/blog/web-search) for standalone usage
