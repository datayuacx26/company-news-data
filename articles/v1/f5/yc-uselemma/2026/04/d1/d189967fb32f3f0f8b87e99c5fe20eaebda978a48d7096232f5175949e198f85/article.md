---
schema_version: "1.0.0"
document_id: "d189967fb32f3f0f8b87e99c5fe20eaebda978a48d7096232f5175949e198f85"
company_key: "yc-uselemma"
company: "Lemma"
source_id: "yc-uselemma-news-import-dd56f4936f86"
canonical_url: "https://www.uselemma.ai/blog/introducing-lemma"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-22T02:10:55.235409+00:00"
fetched_at: "2026-07-28T22:15:39.870488+00:00"
content_hash: "sha256:d0589ff303e0eb007763ac563bd4c76966dc50dd86375b16fab89d581d3d483e"
---

# Introducing Lemma

## AI agents fail silently


They don’t crash, throw visible errors, or trigger alerts. Instead, they confidently give incorrect responses, misinterpret user intent, or execute workflows that technically succeed but produce the wrong outcome.


To your system, everything looks fine. To your users, it’s broken.


This is a fundamental shift from traditional software. In deterministic systems, failures are visible: a request fails, an exception is thrown, a service goes down. With AI agents, failures are **semantic** :


- A customer support agent confidently cites the wrong refund policy
- A financial audit agent generates an outdated report
- A CRM agent calls an integration with a hallucinated user name


Observability breaks down in this world. It tells you latency, errors, and system health, but not whether the agent actually did its job. Once agents are in production, teams hit the same wall: failures don’t show up in logs, and issues are buried deep within thousands of verbose agent traces.


The only options today are to:


1. manually dig through millions of traces,
2. rely on flaky evals that don’t reflect real-world usage,
3. use generic LLM-as-a-judge monitors that don’t capture what’s actually going wrong


Without a system to surface failure modes specific to your agent, teams only find out when users complain. We've experienced this firsthand while improving AI agents at Tandem and Chipstack.


## The continuous feedback loop


To address this, we need a new layer in the stack.


Not observability. Not just evaluation.


Agents need an **adaptation layer** , one that **teaches them to learn from their own mistakes** .


AI software needs AI-native infrastructure. Infrastructure that can intelligently analyze the actions of agents, identify failure modes, and adapt to new inputs and edge cases without human intervention.


At Lemma, we’re building the core scaffolding for this adaptation layer, closing the loop by:


**Surfacing failure patterns** by automatically grouping traces, user feedback, and interaction logs into recurring patterns, so you can quickly see what’s happening and prioritize what matters most without digging through them one by one.


**Diagnosing issues** by analyzing traces, patterns, and surrounding context to identify the root cause behind each failure, understanding not just what went wrong, but why it happened.


**Proposing improvements** by producing concrete improvements to prompts, logic, or workflows, so issues can be resolved without manual debugging.


**Expanding evaluations** by turning real-world failures into new metrics, so agents continuously learn and improve from production usage.


This shifts teams from reactive to proactive, surfacing and resolving failures before users are impacted. Mean time to detect and fix issues drops dramatically, turning hours of debugging into immediate insight. Engineering time moves from chasing edge cases to building core products, while agents become more reliable over time. The result is better systems, better user experience, and software that improves the more it’s used.


## The next decade of agents


Foundation models are improving rapidly. Benchmarks and evals continue to climb, but real-world impact hasn’t kept up. In practice, AI systems are held back by flawed implementations, weak observability, and high maintenance overhead. Offline evals optimize for controlled scenarios, but real failures happen in production as messy, unpredictable, user-driven behavior.


This gap shows up as model unpredictability: agents perform well in some cases and fail in others. As they move into critical workflows and longer-running tasks, these failures become the bottleneck. The problem isn’t capability, it’s adaptability, every failure still requires humans to inspect traces, diagnose issues, patch prompts, and redeploy.


The next phase of AI is not just better models, its continual learning systems. Agents that learn directly from production, adapt to new scenarios automatically, and become more reliable over time.


We’re already working with teams at the frontier of agentic systems to improve their reliability. If you’re running AI in production and feeling this pain, we’d love to talk.
