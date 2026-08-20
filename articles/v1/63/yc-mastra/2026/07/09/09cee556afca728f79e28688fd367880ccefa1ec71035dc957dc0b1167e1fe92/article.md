---
schema_version: "1.0.0"
document_id: "09cee556afca728f79e28688fd367880ccefa1ec71035dc957dc0b1167e1fe92"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/announcing-mastra-factory"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T16:27:30.844834+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:ce6a79cf896f0a594839b342666070675e7128a719bc6445359c4eddeb14c7b2"
---

# Announcing Mastra Factory

Software production is becoming a hierarchy of loops. Engineers move “down” into lower-level loops to debug reliability. They move “up” to design better processes. The highest-value human work is deciding which loops to create.


So today we’re launching[Mastra Factory](https://factory.mastra.ai/) , a system for agents to take software from issue into production.


Your browser does not support the video tag.


In Mastra Factory, specialized agents triage issues, write and validate code, release changes, update documentation, and monitor production. Mastra provides the typed workflows, memory, scheduling, tools, and observability needed to keep the process governed and auditable.


Mastra Factory connects to GitHub, Linear, and Slack. An issue comes in, you open the factory UI, click start, and a coding agent begins working inside the system.


Mastra started as an agent framework. Then we shipped long-running agents. We became a harness framework. And now Mastra is also a factory framework.


## What is a software factory?


Zooming out, there are roughly speaking four approaches to software factories:


1. **Goal-driven** : One supervisor agent, with subagents, oriented towards a` /goal`
2. **Staged automation** : Explicit stage gates around each part of the software development lifecycle with changes measurable and reversible
3. **Learning infrastructure** : Org memory and prod outcomes improve each future run
4. **Human workflow redesign** : Human processes are retooled around agent collaboration


Our concept of a software factory leans towards (2) and (3). We built explicit gates around intake, triage, planning, building, review, and completion, with the option to implement full` /goal` mode via subagents, durable background agents, and shared state.


## Our experience so far


As we’ve adopted the Factory, we’ve seen a number of improvements in how we work together. We were previously seeing bottlenecks around issue intake and PR review. We built skills helping us understand incoming PRs and issues, and a review agent giving us context on the codebase so reviews happened faster with more confidence. During reviews, notification signals keep us in the flow with less idle time.


We like working together. But before the Factory, we were getting lonely. We were pairing more with agents than each other. With the Factory, the shared cloud dashboard let us work together more, not less.


## Get started


Install Factory with:


Terminal


```text
npm   create   factory
```
