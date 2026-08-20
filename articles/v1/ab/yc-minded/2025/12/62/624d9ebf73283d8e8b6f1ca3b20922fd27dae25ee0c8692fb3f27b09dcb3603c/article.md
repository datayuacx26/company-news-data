---
schema_version: "1.0.0"
document_id: "624d9ebf73283d8e8b6f1ca3b20922fd27dae25ee0c8692fb3f27b09dcb3603c"
company_key: "yc-minded"
company: "Minded"
source_id: "yc-minded-news-import-42e7450e1631"
canonical_url: "https://www.minded.com/blog/agent-squad"
published_at: "2025-12-23T12:00:00+00:00"
first_seen_at: "2026-07-22T04:25:07.261888+00:00"
fetched_at: "2026-07-28T22:24:50.551214+00:00"
content_hash: "sha256:1051d2f7193dd3fbd5091e9d92ae9aa9620a9f4d84e41f0bf2ac9cd5c1173f30"
---

# Meet the Agent Squad

In the early days, software complexity meant databases, middleware, and APIs, problems tricky enough, but predictable once you grasped the rules. Today, complexity is different. Today, complexity means intelligent agents, weaving together countless interactions between people, businesses, and machines. At Minded, we've discovered firsthand that deploying AI isn't purely an engineering challenge. It's fundamentally about teamwork. It's the difference between cooking at home and running a restaurant. Both might produce food, but that's about as far as the similarity goes.


## Introducing the Agent Squad


An Agent Squad is a cross functional team, a mix of product managers, engineers, and domain experts that's designed for the Agent Development Lifecycle:


-


Design: Defines user interfaces, workflows, and key success metrics.


-


Build: Connects agents with data sources and develops prompts and data models.


-


Evaluate: Tests agents against real-world scenarios and measures performance.


-


Improve: Continuously enhances agents based on feedback from actual interactions.


Taking an example from our work with eToro to help with trader onboarding, their Agent Squad had agent engineers who translated KYC requirements into prompts and built connectors from eToro's proprietary data models into the agent's tools. And domain experts who reviewed over a thousand live cases. They worked together daily, tweaking flows, reviewing test results, and fixing edge cases, and continued collaborating even after launch, as the agent scaled to handle millions of interactions a year.


## The Key Roles of the Agent Squad


An Agent Squad isn't just a blend of different technical skills; it's also a balance of distinct responsibilities.


The Agent Product Manager figures out how users should talk to the agent and how to measure success. They also make sure the agent fits into existing dashboards, apps, or workflows, whether that's a back office system, a customer support interface, or a mobile app used daily by end users.


The Agent Engineer makes the agent useful. One part of the job is wiring it up to the right systems so it has the information it needs. The other is writing prompts that help the agent reason with that information in a way that actually works for users. Behind the scenes, this means working with a modern AI stack: LLMs, RAG pipelines, vector databases, and even Computer Use, to equip the agents with the tools and data to get the job done.


The Agent Expert is the one making sure the agent is actually ready to face the real world. Before launch, they run the agent through real scenarios, checking how it handles tricky edge cases and unexpected inputs. After the rollout, they review live interactions with users, flagging mistakes and identifying patterns. They also maintain the agent's brain, keeping its knowledge base fresh by updating workflows and rules as the world around it changes.


## Agent Development Through Real-World Examples


At Minded we learnt the hard way that Agent development doesn't start with grandiose diagrams, but with a single real world example: a complete, end-to-end task. Usually something that a humans already knows how to do. When we worked with Amex, we sat with travel agents as they created new bookings. They didn't just search flights by price or duration, they selected seats based on historical preference of travelers. Reproducing that meant wiring the agent not just to the flight database, but to the parts of the UI and history those agents relied on instinctively. That's where agent development really begins.


To take another real world example, JustEat's agent tried to refund a customer for an item that cost nothing. The domain expert explained it was part of a combo meal, not a standalone line item. The engineer then had to trace how pricing data flowed through the restaurant's menu system, eventually building logic that let the agent infer real value even when the original input said zero. That loop happened dozens of times before they were confident enough to let it run without oversight.


This mindset leads to a shift from Task-Based to Outcome-Based engineering. Historically, engineering teams tracked technical success through metrics like uptime or accuracy. However, at Minded, our Agent Squads focus on outcomes such as customer satisfaction, or task completion rate without a human review.


## Build What Matters


Building an Agent Squad isn't just organizational advice, it's a call to action. The future of software isn't just better code; it's better collaboration, tighter feedback loops, and aligning engineering practices with business realities.


If this resonates, you're invited to build your first agent with Minded. Start small, iterate fast, and transform complexity into simplicity. Because that's what great engineering has always been about.
