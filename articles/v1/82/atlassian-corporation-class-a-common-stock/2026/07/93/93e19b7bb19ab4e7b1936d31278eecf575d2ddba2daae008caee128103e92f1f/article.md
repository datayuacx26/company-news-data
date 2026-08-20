---
schema_version: "1.0.0"
document_id: "93e19b7bb19ab4e7b1936d31278eecf575d2ddba2daae008caee128103e92f1f"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/rovo/long-horizon-whats-changed"
published_at: "2026-07-28T17:12:23+00:00"
first_seen_at: "2026-08-06T00:58:11.949434+00:00"
fetched_at: "2026-08-06T00:58:14.178329+00:00"
content_hash: "sha256:820ba36f958641b2b0662215558f05f32a13718cb1e108635ba5f42c938aa917"
---

# Meet the new Rovo Chat: One prompt, multiple steps, zero hand-holding

**The new Long Horizon reasoning engine replaces multi-agent routing with a single reasoning loop — and the results speak for themselves.**


---


**TL;DR:** Rovo Chat’s new Long Horizon reasoning engine replaces fragmented multi-agent handoffs with one model that keeps full context across the **Teamwork Graph** – including Jira, Confluence, Slack, and other tools. It can plan, call tools, reflect, and iterate through complex requests to deliver more complete, trustworthy answers — with quality gains in testing and visible reasoning traces that make progress easier to follow.


For the first generation of Rovo Chat, we built what made sense at the time: a multi-agent routing system. One agent for Jira, one for Confluence, one for Slack. Each an expert in its domain. It was clean and efficient.


The problem emerged in real usage. When you asked Rovo a question that touched multiple products — *“Find the bugs Jenny logged last month and remind me of the context from Slack”* — the system had to hand off work between agents. Each handoff meant context was lost or incomplete. The answer was slower. It felt fragmented.


This architecture was a workaround for the earlier-generation LLMs that struggled with large contexts and long list of tools. Newer frontier models are just much more capable of handling these in addition to doing planning and reflection.


So we asked ourselves: what if we didn’t hand off at all?


## Enter Project Long Horizon: one model, full context, iterative reasoning


Long Horizon is Rovo Chat’s new reasoning engine. Instead of routing your request to different specialists, a single model now holds the full context of your conversation and every tool interaction. It thinks, acts, observes, reflects, and decides what to do next. All in one loop. All without losing context.


Here’s how it works in practice:


1. **Think** — The model considers what you’re asking and what it might need to find the answer.
2. **Call tools** — It selects the right tool (Jira search, Slack lookup, Confluence query) to gather information.
3. **Observe** — It sees what the tool returned and evaluates if it answers your question.
4. **Reflect** — It decides: do I have enough to answer, or do I need more information?
5. **Repeat or respond** — If it needs more, it loops. If it has what it needs, it returns your answer.


The model can iterate up to 150 times per query if needed.


Long Horizon uses adaptive reasoning effort to match the depth of thinking to the complexity of each query. For straightforward lookups — *“What’s the status of PROJ-123?”* — the model applies minimal reasoning overhead and responds quickly. For multi-step research tasks — “ *Compare our sprint velocity across the last three quarters and identify trends”* — it engages deeper reasoning to plan its approach, evaluate intermediate results, and synthesize a thorough answer. For most questions, it finds the answer in 3–8 iterations.


This improvement does come with a tradeoff. As queries get complex, the engine takes a bit longer vs previous architecture to get the final answer. However, from feedback, there was clear consensus that users strongly prefer a trustworthy, higher quality answer even if it meant waiting for a few seconds longer. To better handle this latency, we now expose reasoning traces in UI so you can see how the system is approaching your task as it makes progress and where it stumbles and recovers, driving transparency and reducing perceived latency.


## The numbers: what improved


In our offline evaluations and online A/B tests, Long Horizon showed statistically significant gains in user-facing quality while keeping the latency trade-off visible and measurable.


- **8.5% improvement in offline answer quality** — This reflects improved accuracy on a curated set of hard, multi-tool queries.
- **23% improvement on Confluence evaluations** — The model was significantly better at variety of Confluence tasks such as find page, retrieve content, create/edit.
- **37% reduction in perceived latency** — You see progress in real-time as Rovo thinks and gathers information, instead of silence while agents hand off behind the scenes.


That Confluence jump is worth pausing on. It reflects how much better Rovo can now navigate complex documentation when it holds full context across every search, every page read, every cross-reference.


## What this enables: new use cases


Long Horizon isn’t just faster answers to old questions. It opens entirely new workflows that weren’t reliably possible before.


**Agentic tool use.** Rovo can now create a Jira ticket from a Slack message, update a Confluence page based on context from multiple sources, or trigger multi-step workflows that span products. You say, *“Create a ticket for this bug and link it to the Q3 roadmap epic,”* and Rovo does both in one pass. Reliably.


**Long-running background tasks.** Some questions take time — summarizing a day’s worth of Slack activity, preparing a weekly status report from live project data, or tracing a production incident across deployment logs, PRs, and design docs. Rovo can now persist through that work, gathering from multiple sources and returning a complete answer instead of partial results.


**Deeper cross-product reasoning.** When you ask Rovo something that touches Jira, Confluence, Slack, and third-party tools, it now has the cognitive continuity to connect those dots. Context from one product informs what it looks for in the next. No more stitched-together composites.


## Better foundations across the board


Long Horizon also brought upgrades to the underlying stack: newer foundation models (now running Claude Opus 4.x class models, with the ability to swap out the latest and greatest reasoning models), improved connectors across[Teamwork Graph](http://teamworkgraph.com/) to Jira, Confluence, and Slack, and better interoperability with third-party tools via MCP (Model Context Protocol). As your team connects more of its stack, Long Horizon’s cross-product reasoning extends to those surfaces automatically.


## Go deeper: the engineering story


If you’re an engineer wanting to understand the full architecture — the iteration strategy, prompt caching, progressive tool discovery, and how we measured quality — we’ve published a companion technical deep dive:[Long Horizon: How Atlassian Build a Reasoning Engine for Complex AI Tasks](https://www.atlassian.com/blog/how-we-build/rovo-long-horizon-reasoning-engine) .


## The bottom line


The bottleneck to getting work done isn’t speed. It’s coordination. Long Horizon reduces friction in that coordination loop — one model holding context across all your tools, fewer dropped threads, more complete answers, more work shipped.


Long Horizon is live today for all Rovo customers. Open Rovo Chat in any Atlassian product and ask it something that spans multiple sources. You’ll see the reasoning steps as the model works, and you’ll see the difference in the answer.


[Try Rovo Chat →](https://www.atlassian.com/software/rovo)
