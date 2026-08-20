---
schema_version: "1.0.0"
document_id: "7f83c0051170bb7f53b7a2cdf16041ff76eef767ae4e5c62ffa94bc936afd41f"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/introducing-workshop/"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-22T10:48:02.200992+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:cd9763a91d745df87fa3593e403a9ae4c2bfc36644ba54aaa647b5e34b929344"
---

# Introducing Raindrop Workshop

Today we're launching Raindrop Workshop: an open-source, free, local debugger for AI agents. It streams every span from your agent to a browser UI with zero latency, and it exposes the same traces to Claude Code over MCP so your coding agent can read them, write evals, and fix what's broken.


One command to install:


bash


```text
curl -fsSL https://raindrop.sh/install | bash
```


## Debugging an agent locally is miserable


Debugging an AI agent is miserable. Failures hide three levels deep in nested spans. You're either printing terminal output or going to a SaaS dashboard that's thirty seconds behind. Either way you end up reading thousands of spans by hand, guessing what broke, and hand-writing evals.


## Two parts: a local UI and an MCP


Workshop has two surfaces.


**The local UI is a live trace viewer.** Every span from your agent streams to your browser as it happens with 0 latency: LLM calls, tool calls, reasoning etc.


**The MCP turns Claude Code into the engineer who debugs the agent.** The same traces that stream to your browser are exposed to your coding agent over MCP, so Claude Code can read the spans, write evals from the trace, and fix the code until the agent works.


## Code-aware evals


Claude can read the traces and Claude can write evals.


Instead of writing evals by hand Workshop lets Claude just generate evals from real runs. You're testing against actual failures instead of approximating them. (For a broader look at where code-aware evals fit into the rest of the AI evaluation stack, see our 2026 guide:


[How to Eval AI Agents](https://www.howtoeval.com/) .)


Replay is the other half. Wiring your harness up to a hosted trace platform is cumbersome. With Workshop the traces are right there, so any LLM call can be rerun with a different prompt, a different model, or a different tool implementation, in your own environment.


## From local to production


Workshop is the local half of the same system that powers production debugging in Raindrop. The traces you capture locally use the same SDKs, the same schemas, and the same primitives as the ones in production. When you're ready to ship, your agent is already instrumented.


## Get started


bash


```text
curl -fsSL https://raindrop.sh/install | bash
```


-


**GitHub:**


[github.com/raindrop-ai/workshop](https://github.com/raindrop-ai/workshop)


-


**Docs:**


[raindrop.ai/docs/workshop](https://raindrop.ai/docs/workshop)
