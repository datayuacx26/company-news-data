---
schema_version: "1.0.0"
document_id: "ffa131b76a7d908881dfa3e39d6a73ac214e57aec811093ee4e147c41fcc6478"
company_key: "yc-mindsdb"
company: "MindsDB"
source_id: "yc-mindsdb-news-import-209cdb193474"
canonical_url: "https://mindshub.ai/blog/mindsdb-is-now-mindshub"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-22T04:26:30.664731+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:7c8eded5a2899769c7a71419606e00ad62522f59380a5b2e5a2b30733bbe9688"
---

# MindsDB is now MindsHub: same Minds, new shape

Today, MindsDB becomes **MindsHub** .


Same company. Same team. Same investors. Same open-source roots. What changes is the shape of the product - and the name finally says out loud what we’ve actually become: **a home for open-source AI agents.**


> “Same Minds. New shape.” – Jorge Torres, Co-founder & CEO


## The short version


If you only read one section, read this one:


- **MindsHub is the new product surface. MindsDB is the parent identity.** The company, the people, and the mission are unchanged.
- **Nothing breaks.** Your existing MindsDB workloads keep running. The open-source MindsDB engine and the Anton project continue. Every` mindsdb.com` URL redirects to` mindshub.ai` . No migration required.
- **What’s new** is a platform for running open agents in production - one-click deploy, a 24/7 cloud runtime, model routing, pluggable memory, and a credentials vault - free to start.


If that’s all you needed,[try MindsHub](https://mindshub.ai/) . If you want the story behind the name, read on.


## Where this started: bring the AI to the data


We started MindsDB in 2018, in Berkeley, California. Adam Carrigan and I had watched team after team try to put machine learning into production and grind to a halt on the same wall: the model lived in one place and the data lived in another.


The standard pipeline of the era went like this. Extract the data from the system that owned it. Clean it somewhere else. Model it in a third place. Then thread the predictions back into the application that actually needed them. Every step was a copy, a handoff, and a thing to keep in sync. Most projects died in the plumbing long before they delivered a result.


We took the inverse position: **don’t ask the data to come to the AI - bring the AI to the data.** Put the model where the data already is - inside PostgreSQL, MySQL, MongoDB, Snowflake, the systems teams already trusted - and let people use the tool they already knew, SQL, to train and query it. No new cluster to learn, no fragile export job, no second copy of sensitive data to govern.


The name came from the same idea. We were fans of Iain M. Banks’ *Culture* novels, where the “Minds” are AIs that work alongside people - partners that augment human judgment rather than replace it. A “Mind” you could query like a database. **MindsDB.**


That bet held up. What began as an open-source project grew into the most widely adopted open platform for AI analytics and data-workflow automation - built, from day one, for the people who already understand their data: SQL authors, data engineers, and analysts, not just ML specialists.


## How the work changed under our feet


The mission never moved. The technology under it moved a great deal.


- **2018–2022 - In-database ML.** Bring models to the data and train them with SQL. Forecasting, classification, anomaly detection - declared as queries, run where the data lived.
- **2023–2024 - Beyond tables.** Large language models arrived, and “data” stopped meaning rows and columns. We extended the same connect-and-query idea to documents, vectors, and unstructured text: knowledge bases, semantic search, and RAG you could build with SQL instead of a stack of glue code. We re-architected the API layer around the[Model Context Protocol](https://mindshub.ai/blog/mindsdb-now-supports-model-context-protocol-the-unified-ai-data-hub-your-enterprise-needs) , turning MindsDB into a universal adapter between AI and any backend.
- **2025–2026 - Agents.** The center of gravity shifted again, this time to agents that don’t just answer but *act* - planning work, running code, calling tools, and following through. We[shipped Anton](https://mindshub.ai/blog/introducing-anton-what-business-intelligence-is-supposed-to-be) , an open-source AI coworker for analytics, and watched a whole ecosystem of open agents take off around it.


By early 2026 the most valuable thing we did was no longer “machine learning inside a database.” It was giving open agents a reliable place to connect to data, reach for tools, and run real work. The product had quietly become something its name no longer described.


## Why “DB” stopped fitting


A name is a promise about what a product is. “DB” promised a database. That was the right promise in 2018, and it became the wrong one.


People who met us through the name expected a database and were surprised to find an agent platform. People who needed an agent platform never looked, because the name told them we were a database. The label was working against the product.


“Hub” tells the truth. A hub is where things connect: open agents on one side; your data, tools, and models on the other; and the operational layer in between that makes the connection actually run. That’s what we are now. So that’s what we’re called.


To be clear about what the rename is *not* : it isn’t a new company, a change of ownership, or a pivot away from open source. Same company, same team, same investors. MindsDB is the parent identity; **MindsHub is the product.**


## What MindsHub is


MindsHub is a home for open-source AI agents - the place to run them in production without standing up the infrastructure yourself.


Open agents are powerful and, often, awkward to operate. Many are command-line tools that assume a developer’s laptop. They need API keys wired up, data connected, memory persisted, and something watching them when they run long. MindsHub supplies that operational layer:


- **One-click deploy** - go from repo to a running agent in seconds.
- **A cloud runtime** - agents run 24/7, not just while your laptop is open.
- **Model routing** - match the right LLM to each task, across Anthropic, OpenAI, Google, and open models, with no lock-in.
- **Pluggable memory** - give agents persistent state instead of starting cold every session.
- **A web UI** - a usable interface for agents that otherwise live only in the terminal.
- **A credentials vault** - connect data and tools once; keep secrets encrypted and out of prompts.


The agent lineup is open source and growing - **OpenClaw** , the generalist open agent; **Anton** , the open-source AI coworker for analytics; with **Hermes** , **NanoClaw** , and **OpenHuman** rolling out. It is **free to start** with no subscription and no minimum commitment, because “open” should also mean affordable to actually run.


## What this means for you


Whoever you are today, the change is designed to cost you nothing.


If you’re a… What changes


**MindsDB user with workloads in production** Nothing. They keep running. No migration.


**Open-source user of the engine** The Minds query engine stays open and maintained at[github.com/mindsdb/engine](https://github.com/mindsdb/engine) .


**Visitor with old links or bookmarks** Every` mindsdb.com` URL redirects to its` mindshub.ai` equivalent.


**New to us** Start at[mindshub.ai](https://mindshub.ai/) - deploy an open agent and hand it real work.


The engine that started it all - semantic, federated queries across 200+ data sources - is still here. It’s now one of the things agents stand on, rather than the whole story.


## Seven years in, by the numbers


The shape changed because the work did. The foundation it changed *on* is real:


- **500K+** deployments
- **200+** data sources
- **38K+** GitHub stars
- **$50M+** raised, from Benchmark, Mayfield, Y Combinator, NVIDIA, and others


## Same Minds, new shape


We set out in 2018 to democratize AI - to put real capability in the hands of the people who already understand their data, without making them become someone else. That hasn’t changed and won’t.


What’s changed is where that capability lives. In 2018 it was a model inside your database. In 2026 it’s an open agent that can connect to your systems and do the work. The bet is the same one we made on day one: bring the AI to where the work already happens - and keep it open.


Same Minds. New shape. Come see what we built -[try MindsHub](https://mindshub.ai/) , or read[how MindsHub relates to MindsDB](https://mindshub.ai/mindshub-vs-mindsdb) .
