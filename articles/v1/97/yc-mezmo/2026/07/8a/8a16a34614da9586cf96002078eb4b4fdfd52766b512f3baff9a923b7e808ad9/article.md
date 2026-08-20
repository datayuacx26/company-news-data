---
schema_version: "1.0.0"
document_id: "8a16a34614da9586cf96002078eb4b4fdfd52766b512f3baff9a923b7e808ad9"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/why-sre-agents-need-orchestration-not-just-more-tools"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:3c1c19afdab81c1fabd5bbd5413d81e793c55761bd50e4bc9020ea1ba30488db"
---

# Why SRE agents need orchestration, not just more tools

Single agents are a useful starting point for SRE workflows. They are not where the architecture should end.


The first version is simple enough: connect an LLM to a few tools, give it a system prompt, and point it at your infrastructure. It can summarize an alert, pull logs, answer questions, and draft a useful next step.


Then the workflow gets real.


You add GitHub for runbooks, Kubernetes for cluster state, PagerDuty for incident context, Prometheus for metrics, and Mezmo for telemetry. In one AURA SRE orchestration setup, that exposed 146 tools.


That is not a weird edge case. It is what real incident response looks like once the agent has access to the same systems an SRE uses.


At that point, the problem is not just tool count. The problem is responsibility.


One agent is now expected to investigate incidents, retrieve context, reason over telemetry, decide what matters, and update documentation inside the same task boundary. That is where single-agent SRE workflows start to break down.


AURA orchestration mode addresses this by routing production SRE workflows across scoped workers, each with its own tools, context, and responsibility.


## What breaks with a single agent at scale


The second problem is context confusion.


When every tool is available, the agent has to decide which ones are relevant to the current task. Ask it to update a line in a runbook and it might decide to redo the full investigation first: pull logs, correlate signals, and re-examine the system state. That is not a model failure. It is what happens when task boundaries are not enforced at the architecture level.


The third problem is task boundary collapse.


Investigation and documentation are different jobs. Investigation requires pulling large amounts of data. Documentation requires pulling almost none. Mixing them in one context window introduces noise, spends tokens on the wrong work, and produces worse output on both ends.


A single agent that does everything eventually becomes hard to reason about for the same reason any overloaded system does: unclear responsibilities, too much shared context, and no clean boundary for failure.


## How multi-agent orchestration works in[AURA](https://www.mezmo.com/aura)


Orchestration mode introduces a coordinator agent.


The coordinator receives the task, reasons about what is needed, and routes the work to the right worker. Each worker is scoped to a specific job and a specific set of tools. Workers can be scoped to have access to only the MCPs and tools that they need.


That separation matters operationally. It reduces tool-selection noise, limits blast radius, and makes each worker's behavior easier to reason about.


The coordinator does not need to know how to pull logs or write a GitHub PR. It needs to know which worker to call. Each worker does not need to understand the full investigation context. It needs to do its job well with a focused set of inputs.


In practice, the coordinator handles the multi-step planning that would otherwise overwhelm a single agent's context. It can run workers sequentially or in parallel, pass findings between them, and synthesize a result.


For incident response, that might look like:


1. Receive the page or alert.
2. Retrieve the relevant runbook.
3. Investigate the incident.
4. Decide whether an update is needed.
5. Draft the change for review.


Those are separate workers, each doing one thing, coordinated by a layer that holds the overall task state.


The goal is not to hide the workflow behind code. It is to make the agent topology explicit and reviewable.


The configuration lives in TOML. Here is a simplified version of what our SRE orchestration setup looks like:


‍


The investigator gets the Mezmo and Kubernetes MCPs. The runbook engineer gets GitHub. Neither sees the other's tools. The coordinator sees the original task and the workers' outputs.


If your workflow involves GitHub, you need a GitHub personal access token for the GitHub MCP. The hosted version handles the rest. You do not need to run a local MCP server.


## What this enables in practice


The[runbook use case](https://www.mezmo.com/blog/the-runbook-problem-how-aura-documents-what-teams-dont-have-time-to-write) is one example.


In one internal use case, we had about 40 missing runbooks. The pattern was familiar: incidents were investigated, resolved, and then everyone moved on. Not because the team lacked discipline, but because documentation has high effort, low immediate payoff, and no hard deadline.


The orchestrator changes the cost of that work.


An investigator worker can handle the discovery. A runbook engineer can draft the missing documentation or open a PR with a proposed update. The engineer still reviews and merges. The difference is that the most repetitive part of the work is no longer sitting entirely with the person who just finished the incident.


But the runbook workflow is not the reason orchestration mode exists. It is one example of a broader pattern.


Production SRE workflows are inherently multi-step and multi-context. Incident response is not one task. It is a sequence of different tasks: understand what is happening, identify root cause, find prior context, decide on action, execute safely, and document what changed.


Those steps require different tools, different context, and different levels of permission. Collapsing them into a single agent context is the wrong abstraction for production work.


Other workflows that benefit from the same architecture include:


**Deployment validation** A coordinator routes a deployment event to workers checking metrics drift, error rate changes, and dependent service health in parallel, then synthesizes a pass/fail recommendation with evidence.


**Drift detection** A worker monitors infrastructure state against expected topology. When it detects drift, it hands off to an investigation worker, which can hand off to a remediation worker with human approval.


**Postmortem generation** After incident resolution, an orchestrator sequences the work: pull the incident timeline, retrieve relevant runbooks and prior incidents, draft a structured postmortem, and open a PR for engineer review.


Each of these workflows spans multiple context domains, multiple tool sets, and multiple decisions. Orchestration mode provides the coordination layer that keeps that work structured.


## Why this is different from a general-purpose agent framework


General-purpose agent frameworks can support multi-agent workflows. The difference is where they start.


Most frameworks give you primitives: agents, tools, handoffs, memory, and execution loops. That can be powerful, but production SRE teams still have to decide how to partition tools, constrain workers, manage context, handle provider differences, preserve auditability, and keep workflows inspectable under incident conditions.


AURA starts from those production SRE constraints.


The coordinator/worker model, MCP assignments, worker isolation, retry behavior, turn depth limits, schema sanitization, streaming behavior, and context controls are part of the runtime and configuration model. You define the workflow. AURA handles the orchestration substrate underneath it.


The practical difference is that teams can start from an inspectable SRE workflow configuration instead of building the orchestration layer from scratch.


## What agentic SRE looks like from here


Orchestration mode is what moves agentic SRE beyond a chatbot pattern.


[A useful SRE agent](https://www.mezmo.com/platform/agentic-sre) cannot just answer questions. It has to coordinate work across telemetry, infrastructure, incidents, code, runbooks, and change history. It has to know when to investigate, when to retrieve context, when to write, when to stop, and when to ask for human review.


That is the difference between an assistant and an operating model.


In the near term, agents act like first officers. They do the investigative legwork and produce evidence-backed outputs for engineers to review. The engineer still decides what to trust, what to merge, and what to execute.


Over time, as teams define guardrails and repeatable workflows, agents can take on more execution for well-understood, low-risk patterns. That does not mean handing production over to a black box. It means using policy, approval, auditability, and scoped tools to move more operational work into repeatable systems.


Orchestration is the layer that makes that progression possible.


## Getting started


AURA is Apache 2.0, and available here:[https://github.com/mezmo/aura](https://github.com/mezmo/aura)


To try orchestration mode, you need:


- AURA running locally or in a container
- MCP connections to the tools you want workers to use
- A TOML config defining the coordinator and workers
- Optional: GitHub MCP access if your workflow involves runbooks or PRs
