---
schema_version: "1.0.0"
document_id: "47d7549d4181b8f715e505f84e7d19994302e218bb2807de59df6377e8f9e1b3"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/why-we-open-sourced-aura-infrastructure-for-production-ai"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:c1186d5f8ad76f7d60ced85257ab9c9d68f49a33066b0335a133fe8d67bbab54"
---

# Why we open-sourced AURA: Infrastructure for production AI

*By Henry Andrews, Engineering Manager at Mezmo*


‍


Over the last year, I’ve talked to dozens of SRE teams about AI. The excitement is real, but conversations hit a wall when we get to production reality. How does an agent manage complex context without losing the plot? How does it avoid hallucinating relationships between signals? Who owns the orchestration logic that ties it all together?


We realized the bottleneck wasn’t model intelligence. It was the lack of a reliable logic layer between the data and the model. That is why we built, and open-sourced,[AURA](https://www.mezmo.com/aura) .


‍


**The Orchestration Layer for Context**


‍


AURA stands for AI Universal Rigging Agent. We describe it as a[System of Context](https://www.mezmo.com/aura) because it provides the configuration layer needed to turn a raw LLM into a grounded operational tool.


A System of Context holds the relationships, rules, and workflows your team relies on during an incident—so agents aren’t guessing, and humans can trust what they see.


In practice, that means four things:


‍


- **Dynamic Context:** Context isn’t a blind, one-time prompt. It has to be intelligently assembled, synthesized, and propagated from step to step as an investigation unfolds.
- **Open** : You should be able to connect new tools, use the models you choose, and apply your governance without waiting on a vendor roadmap.
- **Transparent:** You need to see what the agent used, why it took an action, and what evidence it relied on—so you can audit and improve behavior over time.
- **Engineered:** Especially for telemetry, raw volume isn’t helpful. Deduplication, clustering, enrichment, and routing turn noisy streams into predictable inputs for agents.


‍


In a real investigation, context is not a static snapshot. It is an evolving timeline of alerts, runbooks, logs, and metrics. While most frameworks treat this like a simple chat session, an investigation requires coordinated, multi-step execution. AURA provides the rigging to connect your models to the systems you[already use via MCP](https://www.mezmo.com/learn-observability/agentic-ai-what-is-model-context-protocol-agent2agent-and-how-does-this-impact-automation) , gracefully propagating context so tool integration remains stable and consistent as incidents evolve.


‍


**Why Open Source: Ownership over Renting**


The logic layer that connects your data to your agents should be open source and built on open standards, not a proprietary service you rent. SRE teams operate critical systems. If AI is going to participate in those systems, the orchestration layer must be inspectable and adaptable. We chose Rust because production reliability matters, and we chose open source so teams can audit behavior and extend workflows themselves. We rely on open standards like MCP for tool connectivity and TOML for configuration so the brain of your workflows stays under your control.


‍


**Built for Production**


AURA focuses on the engineering hurdles that usually kill AI projects when they move out of the lab and into the cluster:


- Interoperability: It sanitizes tool schemas so function calls remain consistent across providers like OpenAI, Anthropic, or Gemini.
- Grounding: Vector stores are first-class. You can attach runbooks and knowledge bases with clear context prefixes so the agent has grounded reference material.
- Observability: Structured tracing hooks emit correlation context designed for OTel integration. You can connect agent decisions to your existing tracing infrastructure to see exactly why a specific path was taken.
- Stability: It runs as a stateless service, built in Rust for memory safety and the predictable execution required for incident response.


‍


**Moving Beyond the Novelty**


We are still early in what Production AI will look like for[SRE teams](https://www.mezmo.com/site-reliability-engineers) . But one thing is already clear: models alone are not enough. To move from experimentation to reliability, we need infrastructure that manages context deliberately and transparently.


‍


AURA is our attempt to build that foundation in the open, shaped by real operational pain. It is designed to make AI a dependable participant in your workflow, not a novelty layered on top.


It's time to move past scripts and start building infrastructure.


‍
AURA is available here:[https://github.com/mezmo/aura](https://github.com/mezmo/aura)
