---
schema_version: "1.0.0"
document_id: "5a50f013f5fb114f7f9a2deccf81c780890d41a8c69252bf86f5f9eaa2da7d2e"
company_key: "yc-wild-moose"
company: "Wild Moose"
source_id: "yc-wild-moose-news-import-a47316f7ff4d"
canonical_url: "https://www.wildmoose.ai/post/human-centric-observability"
published_at: "2024-11-23T00:00:00+00:00"
first_seen_at: "2026-07-26T05:31:33.003390+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:ce746dbb26d42bfa388e51c7993134385bcd49058500c42706543380ae4a3fb5"
---

# From Data-Centric to Human-Centric Observability: A Vision for the Future

Observability tools today are essential, but they weren’t designed with humans in mind. The systems we rely on to diagnose and resolve production issues were built with a focus on indexing and storing data, not on helping engineers navigate incidents efficiently. The result? A convoluted and often frustrating experience, where the tools intended to make our lives easier are sometimes more of a barrier than a solution.


## Legacy Roots in Modern Observability


Take logging, for example. Many logging platforms today still rely on Lucene queries—a technology built in 1999 as a full-text indexing engine. While Lucene may be great for indexing text, it’s far from optimized for handling the complex, fast-paced, and human-centric needs of today’s production environments. The same can be said for many other observability tools, which were built for storing and indexing data rather than empowering engineers to resolve issues efficiently.


Additionally, the explosion of observability tools has created its own set of challenges. In 2024, the average company uses no fewer than 15 observability products to monitor their systems. This has led to increased complexity, as engineers juggle multiple interfaces, tools, and data formats. Some of the bigger players in the observability space are pushing for consolidation—integrating many features under one umbrella. But this often comes at the cost of vendor lock-in, which prevents organizations from choosing the best tool for each job.


Lock-in also means paying sky-high prices. Some vendors are infamous for costing more than the cloud infrastructure they monitor! While OpenTelemetry (OTel) is a fantastic step toward breaking down some of these barriers, it only addresses data ingestion. The real question is: what about consumption?


## The Consumption Gap in Observability


OpenTelemetry has made it easier to avoid re-instrumenting code and replacing agents, but it leaves a significant gap when it comes to the consumption of that data. Once a company is invested in a particular observability tool, switching becomes a massive ordeal—not just in terms of setting up new queries, dashboards, and alerts, but in retraining engineers who are accustomed to working with a specific interface and workflow.


This is where many organizations find themselves stuck. Despite the promise of open standards, they’re either trapped by large incumbents or find themselves in a perpetual transition phase, maintaining multiple tools for essentially the same purpose. It’s an expensive and inefficient way to work.


## The Power of Decoupling


The good news? There’s a way out—and it lies in one of engineers’ favorite concepts: decoupling.


Today’s observability tools conflate the task of ingesting, indexing, and storing data with that of consuming it during production work. These are fundamentally different problems. The key to unlocking a more efficient, human-centric approach to observability is to decouple them.


An example of this decoupling can be found in Grafana, which separates visualizations from the data sources. But even Grafana is largely used within its own ecosystem for logs, metrics, and traces, and falls short of a true decoupling that prioritizes production work.


## A New Abstraction Layer for Observability


What we need is an abstraction layer that provides a unified interface across all types of observability data—logs, metrics, traces, and beyond—regardless of where that data is stored. This layer should integrate seamlessly not only with traditional observability data, but also with the myriad other data sources engineers rely on to handle production issues. This could include analytics, internal APIs, cluster information, and more.


But the abstraction layer needs to go further. To truly transform production work, it should also integrate the organization’s knowledge base—runbooks, tickets, known issues, change history, and past incidents and alerts. The reality is that solving production problems isn’t just about looking at logs; it’s about accessing the full context of the system and understanding its history.


## Enter Gen AI: The Human-Centric Future


What’s missing in today’s observability landscape is a tool designed with production work—and the humans handling it—as the primary concern. We need an abstraction layer that decouples data ingestion from consumption and integrates the entire organizational knowledge base into the workflow.


With the help of Generative AI, this abstraction layer could finally give developers the ability to interact with production in a way that makes sense for the human handling the issue, unhindered by legacy concerns rooted in data-centric designs from over two decades ago.


Generative AI’s ability to process and synthesize vast amounts of unstructured data—such as historical logs, chat conversations, and post-mortems—turns this abstraction layer into something more than just a data tool. By using AI to analyze real-time data alongside this contextual information, engineers can receive actionable insights in seconds. Instead of navigating through multiple interfaces and performing tedious data searches, AI can automate the analysis, identify patterns, and suggest actionable next steps.


Incorporating generative AI into this framework means that engineers don’t just get a technical view of an issue—they gain a **human-first experience** , tailored to the specific problem they’re facing, based on the collective knowledge of the entire organization.


## A Human-First Approach to Observability


This is the next frontier of observability: one where engineers are empowered to focus on solving problems, not wrangling tools. And it’s a future we can start building today.


By shifting the conversation from data-centric to human-centric observability, we can create a more intuitive, powerful experience for engineers on the front lines of production. The tools are evolving—now it’s time for the philosophy behind them to evolve as well.
