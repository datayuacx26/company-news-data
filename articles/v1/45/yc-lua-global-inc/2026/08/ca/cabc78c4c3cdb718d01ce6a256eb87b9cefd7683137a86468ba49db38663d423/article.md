---
schema_version: "1.0.0"
document_id: "cabc78c4c3cdb718d01ce6a256eb87b9cefd7683137a86468ba49db38663d423"
company_key: "yc-lua-global-inc"
company: "Lua Global Inc"
source_id: "yc-lua-global-inc-rss-87d3f9cc68e5"
canonical_url: "https://blog.heylua.ai/the-qwen-3-6-and-3-7-model-families-are-now-in-lua/"
published_at: "2026-08-03T09:58:12+00:00"
first_seen_at: "2026-08-03T10:47:24.655448+00:00"
fetched_at: "2026-08-03T11:29:23.074207+00:00"
content_hash: "sha256:d487aa589e3aff3065fc4cb972ee268f0ba00038b873078603ac20385822764f"
---

# The Qwen 3.6 and 3.7 model families are now in Lua

We've partnered with Alibaba Cloud to add the full Qwen lineup to the Lua model library: Qwen 3.7 Max and Plus, Qwen 3.6 Plus and Flash, and the open-weight 27B and 35B-A3B variants.


### **Why we keep growing the library**


Lua has a simple position on models: your agents should run on the best model for each job, whoever makes it. We don't have a house model to protect, so we never have to steer your work toward one. When a new family raises the bar on capability or price, our job is to get it into the library fast.


And an agent team is a team. The agent planning a category reset and the agent triaging ten thousand tickets behind it are doing different jobs. Staffing both with the same model means overpaying on one end and underdelivering on the other.


### **Why Qwen**


Qwen is one of the strongest model families in the world right now: frontier-level reasoning, coding and agent performance, the leading open-weight lineup, and price-performance the rest of the market is still chasing. It's also trained across more than a hundred languages, which matters to us. Our agents work in markets where the customer's first language usually isn't English, and a model that handles Swahili or Arabic properly beats one that fakes it.


**Here's how we'd staff each model on a typical agent team.**


Qwen 3.7 Max. The deep-reasoning model. Planning, complex multi-step decisions, synthesis across hundred-page documents. The model you hand your hardest problems.


Qwen 3.7 Plus and 3.6 Plus. The workhorses. Drafting, extraction, standard agent conversations. Most of a team's day-to-day runs well here at a fraction of Max cost.


Qwen 3.6 Flash. Built for volume. Classification, routing, short summaries. When an agent handles thousands of items an hour, Flash keeps quality high and the cost near zero.


Qwen 3.6 27B and 35B-A3B. Open-weight variants for teams that want tighter control over the capability-cost tradeoff, including a mixture-of-experts option that runs light.


Qwen 3.6 Max (preview). The 3.6 flagship, available now in preview. Select it like any other model in agent settings; it will graduate from preview as Alibaba Cloud finalises the release.


### One team, several models


This is where the library pays off. Take a support operation: Qwen 3.7 Max reads the escalations and works out what actually went wrong. Qwen 3.6 Flash triages the thousand tickets an hour arriving behind it. A Plus-tier agent drafts the replies and passes anything unusual up the chain. One team, three models, each on the job it's best at.


You set defaults per agent in settings, or let Lua's routing pick the model per task. Right model, right job, and you never overpay for either.


The Qwen family is available in all workspaces today. Sign-up for the waitlist[here](https://heylua.ai/waitlist/?ref=blog.heylua.ai) .
