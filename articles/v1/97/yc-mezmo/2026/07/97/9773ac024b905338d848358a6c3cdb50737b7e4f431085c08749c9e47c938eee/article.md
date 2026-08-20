---
schema_version: "1.0.0"
document_id: "9773ac024b905338d848358a6c3cdb50737b7e4f431085c08749c9e47c938eee"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/when-your-agents-hallucinate-at-2-am-it-is-not-a-model-problem"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:f13a558fb6d50e4910ac5b8d1fa7b02f5bc5893590e985ab2a26bef43198e4b7"
---

# When your agents hallucinate at 2 AM, it is not a model problem

The first time an AI assistant suggests "restart the service" during a live incident and nobody on the bridge can tell whether that suggestion came from a current runbook, a stale wiki page, or thin air, you stop caring about model benchmarks. You start caring about what the agent actually knew, where that knowledge came from, and whether you can trust the chain of reasoning behind it.


‍


This is the failure mode SRE and platform teams are running into as LLMs and early agents enter incident response, change management, and on-call workflows. The agents are not the problem. The substrate they are reasoning over is.


‍


Recent Gartner® research puts a number on the broader agentic AI risk.[In Analyst Take: Why Context Engineering and Decision Intelligence Are Critical for Agentic AI Success](https://www.mezmo.com/resources/in-analyst-take-why-context-engineering-and-decision-intelligence-are-critical-for-agentic-ai-success) , Sr. Director Analyst Deepak Seth predicts “By 2027, over 40% of agentic AI projects will be canceled due to escalating costs, unclear business value or inadequate risk controls.” Our view is that observability teams are seeing the same underlying problem in production: agents are only as useful as the operational context they can reason over.


## The same telemetry problem, with new consequences


Walk a typical observability pipeline today. Logs and metrics arrive without consistent service ownership tags. Traces drop spans because instrumentation is inconsistent across services. Service catalogs and runbooks live in a wiki nobody trusts after the last reorg. SREs already work around all of this by holding context in their heads and in tribal Slack channels.


‍


Pipe that same telemetry into an LLM and the workarounds disappear. A mislabeled log, a missing OpenTelemetry resource attribute, a stale dependency edge in the service graph, all of it propagates through embeddings and reasoning layers and comes out as a confident but wrong recommendation.


‍


A concrete example. An agent watching error rates flags a spike on payments-api and recommends rolling back the most recent deploy. What it does not know is that the spike correlates with an upstream dependency that was deprecated last week, and that the on-call rotation for that dependency moved to a different team. The signal is in the telemetry. The context is not. Same data, wrong decision, and the fix is upstream of the model.


## Context engineering as a practice, not a slide


The discipline that closes this gap is what we and others have started calling context engineering: the deliberate design of telemetry, metadata, and feedback so that humans and machines can reason about what is happening in production. In practice it looks like:


‍


- Context schemas that carry service lineage, ownership, dependencies, and deploy state as first-class signals, not afterthoughts in a free-form label.
- Contextual pipelines that enrich, route, and shape telemetry at the edge before it lands in storage or an LLM, using OpenTelemetry semantic conventions as the common substrate.
- Distributed context graphs that stay in sync with the systems they describe, so an agent reasoning about payments-api knows what it depends on right now, not last quarter.


‍


This applies whether you have agents in production today or not. The teams doing this work are also the teams whose incident response feels less chaotic. Agents are just the consumer that makes the cost of bad context legible.


## Where to go next


Two reads worth your time:


‍


- Mezmo's recent O'Reilly report with cloud native and SRE practitioner David Beale, *Context Engineering for Observability* , covers the architecture in depth: active telemetry, contextual pipelines, and context graphs, with diagrams and a practical on-ramp for teams starting the work. Read it at[mezmo.com/oreilly](https://mezmo.com/oreilly) .
- Gartner *Analyst Take: Why Context Engineering and Decision Intelligence Are Critical for Agentic AI Success* by Deepak Seth covers the broader agentic AI angle and the decision intelligence pairing.[Contact us](https://www.mezmo.com/resources/in-analyst-take-why-context-engineering-and-decision-intelligence-are-critical-for-agentic-ai-success) to request access to the Gartner reprints.


‍


The work of designing the feedback systems that power both human and machine reasoning is what separates observability that scales with autonomy from observability that breaks under it. Better to engineer it deliberately than to discover the gap during your next incident.


‍


‍


*Gartner, Analyst Take: Why Context Engineering and Decision Intelligence Are Critical for Agentic AI Success, Deepak Seth, 2 April 2026.*


GARTNER is a trademark of Gartner, Inc. and/or its affiliates.


‍
