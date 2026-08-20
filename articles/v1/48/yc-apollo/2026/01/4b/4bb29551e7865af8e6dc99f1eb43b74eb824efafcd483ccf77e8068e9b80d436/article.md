---
schema_version: "1.0.0"
document_id: "4bb29551e7865af8e6dc99f1eb43b74eb824efafcd483ccf77e8068e9b80d436"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/transforming-capital-ones-data-marketplace-with-embedded-governance-and-a-product-mindset"
published_at: "2026-01-13T13:34:19+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:450e91d96c9d5c26e0c3e2bc1ff316488154a78645bd83cf2aacfc7f181f108c"
---

# Transforming Capital One’s Data Marketplace with Embedded Governance and a Product Mindset

## A recap from GraphQL Summit 2025 on how Capital One applied a product mindset to build an enterprise data marketplace powered by Apollo


Large organizations with complex data ecosystems often face a familiar challenge. As systems evolve over time, data becomes harder to find, harder to trust, and harder to use. Teams build APIs independently, integrations multiply, and identifying the authoritative source of data can take significant effort.


This was the broader context behind Capital One’s journey.


*“All data consumers want to do is find data and build experiences for customers,”* explained Susan Price, Product Manager at Capital One. *“And all producers want to do is publish their data so consumers will find and use it. Without a holistic solution, organizations may spend a lot of time looking for data, struggle with inconsistent integrations, and validate data quality on their own.”*


While Capital One already had a governed data marketplace in place, they saw the opportunity to build on that foundation to improve speed, reuse, and modern orchestration, creating a more discoverable platform that could serve as a single source of truth across the enterprise.


**The Federated GraphQL Approach: Orchestration at the Center**


Capital One started with a concrete pain point: an internal platform needed to orchestrate data from multiple microservices while avoiding confusion over which systems were authoritative, cached, or redundant. The team saw an opportunity to solve not just this single use case, but a recurring pattern every platform across the company would face.


To address it, they brought in GraphQL and[Apollo Federation](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/federation) to scale across teams and compose many services into one cohesive graph.


*“We brought in GraphQL as a querying framework to handle the orchestration,”* said Gaurav Singh, Engineering Leader at Capital One. *“With Apollo Federation, we built a central framework that applied our company standards and took care of the heavy lifting, so our teams could focus on business functionality instead of figuring out where the data was coming from.”*


To prove out their approach, the team routed a subset of their production traffic to their new supergraph. Performance dashboards confirmed low latency, high throughput, and zero disruption to live workloads. The evidence-based rollout built confidence across the organization.


With[Apollo Router](https://www.apollographql.com/docs/graphos/routing) as the foundation, the team standardized APIs, improved discoverability, and gave teams a transparent way to publish, manage, and govern their data. The marketplace now revolves around five core pillars: **Publish, Manage, Find, Use, and Govern.**


- **Publish** and **Manage** serve data producers, ensuring data is registered, versioned, and traceable.
- **Find** and **Use** serve data consumers, enabling quick discovery of authoritative datasets and clear documentation.
- **Govern** spans all of them, embedding quality checks, compliance, and observability into every step.


*“From the beginning, observability was paramount,”* said Jake Yesbeck, Engineering Manager at Capital One. *“We wanted to build a glass house so we could see all the water going through the pipes and all the wiring at once. That visibility let us spot issues before they reached our customers and gave everyone confidence in how the system worked.”*


## **Velocity Through Structure**


What began as a solution for one platform evolved into Capital One’s enterprise standard for data sharing and orchestration. As teams saw the platform solving real problems, adoption grew organically, and the system scaled right alongside it. In recent months, schema changes proposed against their subgraphs quadrupled, and supergraph deployments tripled, yet the system continued to accelerate rather than slow down.


The team’s investment in embedded governance and automation produced measurable efficiency gains. Organizations building similar data marketplaces have the potential to see benefits as impactful as these example gains:


- **50% reduction** in schema composition, proposal, and deployment steps
- **50% reduction** in communication tools needed (chats, documents, tracking systems)
- **67% reduction** in people required in the approval loop
- **300% increase** in deployment velocity


*“When you spend the time building structure and paving clear paths, you don’t slow people down,”* said Jake Yesbeck, Engineering Manager at Capital One. *“You actually get more speed because everyone knows the safe path to take.”*


## **Iterating on Real Producer and Consumer Feedback**


Capital One’s product approach meant continuously listening to their data community. They learned that schema generation tooling had become overloaded with unnecessary custom directives, causing customer-facing errors. They learned that producers wanted visibility into when changes would reach QA and production, who was accountable for next steps, and whether reviews were happening within SLAs.


Acting on that feedback, the team expanded authorized approvers, introduced schema observability dashboards, and built a “fast track” workflow for high-priority schema changes.


By embedding these improvements directly into workflows, the team maintained compliance while enabling speed at scale.


## **From Data Chaos to Strategic Asset**


What started as a solution for one platform’s orchestration problem became Capital One’s standard approach to data access across the enterprise. More platforms joined organically because it solved real pain points in transparent, measurable ways.


The architectural lesson is clear: when you standardize data and make it accessible, predictable, intuitive, and observable, you transform a marketplace from just another source into the source of truth.


Capital One’s implementation proves that governance does not have to slow you down. When embedded into workflows rather than bolted on afterward, it becomes the mechanism for sustainable acceleration.


“You can only achieve scalable, reliable data access when you treat graph as a platform and embed governance in the workflows,” Price concludes. “Your job is to make the right thing the easy thing.”


### **Lessons for Every Enterprise**


Capital One’s journey offers a practical blueprint:


- **Start with a high-value dataset.** Apply a product lens to its lifecycle from creation to consumption.
- **Talk to producers and consumers.** Identify friction points and bake governance into solutions.
- **Build for evolution.** Governance and structure should enable change, not resist it.


By treating the graph as a long-term product rather than a one-time project, Capital One turned what used to be a bottleneck into a built-in advantage. This approach reflects a broader industry shift where governance is no longer a separate process layered on top of data systems but an integral part of how modern enterprises deliver reliability, compliance, and speed at scale


With Apollo, Capital One has established a foundation for continued growth. Teams can now build new data products and AI-powered applications on top of a trusted, observable graph that connects the entire enterprise.


Watch the full Capital One session from GraphQL Summit 2025 to hear Susan Price, Gaurav Singh, and Jake Yesbeck share their journey in their own words.


Written by


Valeria Gomez


[Read more by Valeria Gomez](https://www.apollographql.com/blog/author/vgomez)
