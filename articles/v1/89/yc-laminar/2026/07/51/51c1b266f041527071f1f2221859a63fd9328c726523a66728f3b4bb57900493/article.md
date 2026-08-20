---
schema_version: "1.0.0"
document_id: "51c1b266f041527071f1f2221859a63fd9328c726523a66728f3b4bb57900493"
company_key: "yc-laminar"
company: "Laminar"
source_id: "yc-laminar-news-import-4ade4fae8778"
canonical_url: "https://laminar.sh/article/top-6-agent-observability-platforms"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-22T01:53:16.367078+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:0523b4e80958e07888b690c852207bd0319360cc9aecc70f73f2d53a00e103e6"
---

# Top 6 Agent Observability Platforms (2026): A Developer's Ranking

Most AI observability tools were built for single LLM calls. A prompt goes in, a completion comes out, and the trace is two spans deep. That model breaks the first time you deploy an agent that calls fifteen tools, decides its own control flow, and re-sends its entire conversation on every turn.


Agent observability is a different problem. The trace is 2,000 spans. The failure happens four tool calls deep. You need to know what the agent said, what the user said back, and which sub-step threw, without reading every span in order. The industry has moved from single LLM calls to agents, and the tools that win are the ones built for agents from the ground up rather than retrofitted from an LLM logging product.


This article ranks the platforms that actually solve that problem in 2026. We rate them on six things: trace depth, agent-specific UX, replay and debugging workflows, OpenTelemetry support, self-hosting options, and pricing model.


## TL;DR: Top 6 Agent Observability Platforms (2026)


#


1. **[Laminar](https://laminar.sh/)** . Open-source, OpenTelemetry-native, built for AI agents from the ground up. 20x trace compression, the lowest pricing on the market, Signals, a coding-agent-driven debugger, SQL over all platform data, and a code-first eval SDK. Best pick for anyone shipping agents to production.
2. **[Langfuse](https://langfuse.com/)** . Strong open-source option (MIT). Best for prompt-centric workflows and dataset management. Trace model is solid but not agent-first.
3. **[LangSmith](https://smith.langchain.com/)** . Tight integration with LangChain and LangGraph. LangGraph Studio is a real advantage if you live in that ecosystem. Closed source. Self-host is Enterprise-only.
4. **[Arize Phoenix](https://phoenix.arize.com/)** . OpenTelemetry-native, open-source, OpenInference semantic conventions. Good for evaluation-heavy teams already on Arize.
5. **[Weights & Biases Weave](https://wandb.ai/site/weave)** . Fits teams already on W&B. Decent trace view, strong eval harness.
6. **[Braintrust](https://www.braintrust.dev/)** . Eval-first platform with tracing bolted on. Strong for teams whose primary bottleneck is regression testing.


If you only read one sentence: pick Laminar if you are shipping and debugging agents, Langfuse if you are iterating on prompts, LangSmith if you are committed to LangGraph, and Phoenix if your team is already on Arize.


## What "agent observability" actually means


#


A normal LLM observability tool logs prompts, completions, tokens, and latency. That is enough when your app is a single chain.


Agent observability has to handle four things that break simpler tools:


- **Long traces.** A research agent can produce thousands of spans across LLM calls, tool calls, sub-agent invocations, and retries, with the full conversation re-sent on every turn.
- **Non-deterministic control flow.** The agent decides which tool to call next. The trace shape is different every run.
- **Nested causality.** A failure at span 1,800 might be caused by a bad retrieval at span 42. You need to follow the chain, not just read linearly.
- **Session continuity.** Agents resume. A single "task" spans multiple process invocations. The trace model has to stitch them together.


Every tool below claims to support this. Some do.


## 1. Laminar


#


**Category:** Open-source agent observability and debugging platform. **License:** Apache 2.0. **Deployment:** Cloud, or self-hosted in minutes via the official Helm chart. **Repo:**[github.com/lmnr-ai/lmnr](https://github.com/lmnr-ai/lmnr) .


### Why Laminar is top of this list


#


Laminar was built from the start for AI agents, not retrofitted from an LLM logging tool. Every choice in it follows from understanding the shape of an agent trace, and the feature set tells one story: shipping agents means Laminar. Here it is in the order that matters.


**20x trace compression, and the lowest pricing on the market.** Agents re-send the full conversation on every turn, so a 30-turn run that has k unique messages carries on the order of k(k+1)/2 messages across its spans, the same context copied over and over. Generic tools store every copy. Laminar hashes each message, stores every unique message once per trace, and reconstructs the full trace byte-for-byte at query time: an average 20x reduction in storage, up to 50x on the longest runs ([full write-up](https://laminar.sh/article/laminar-20x-agent-trace-compression) ). That is the foundation of the pricing. Because Laminar stores a fraction of the bytes, it can bill on[data volume](https://laminar.sh/pricing) and still come in below everyone else.


**Signals: read ten thousand traces without reading them.**[Signals](https://laminar.sh/docs/signals/introduction) turn a plain-language instruction plus a JSON schema into a structured event on every trace it matches. You write "agent looped on the same tool without making progress." Laminar extracts it, backfills across history, and fires on every new trace. The result is a stream of events you can query, cluster, and alert on. One trace you can skim; ten thousand you cannot, and Signals are how you answer questions across all of them.


**A debugger your coding agent drives.** Building an agent is a loop: run it, read what it did, change something, run it again. Laminar's[debugger](https://laminar.sh/docs/debugger/introduction) is that loop, built so Claude Code, Cursor, or Codex runs it through the Laminar CLI. Start your agent with LMNR_DEBUG=true


and the run is traced into a session; the coding agent reads the trace, edits your code, and reruns, with each rerun served from cache up to the point it is testing so the turn is fast and cheap. The call you are fixing is often three-quarters of the way through a multi-minute run; caching the prefix means the agent can take that turn dozens of times in the span it would take to run live once.


**SQL over all platform data.** Agent traces raise questions a dashboard was never going to answer. Laminar gives you raw[SQL](https://laminar.sh/docs/platform/sql-editor) over traces, spans, signal events, evaluations, and metadata, reachable wherever you or your coding agent work: the in-app SQL editor, lmnr-cli sql query


, the[MCP server](https://laminar.sh/docs/platform/mcp) , and the SQL API. "How many runs called tool X more than five times and then errored" is one query.


**A code-first eval SDK.** Laminar's[evals](https://laminar.sh/docs/evaluations/introduction) follow a code-first, barebones-SDK philosophy: a dataset, an executor function (your agent or a piece of it), and evaluator functions that score the output, written in plain Python or TypeScript and run with python my_eval.py


, tsx my-eval.ts


, or lmnr eval


. Because the SDK is thin, you can evaluate any part of an agent without contorting it into a prompt-and-scorer shape.


On top of these, the[transcript view](https://laminar.sh/docs/platform/viewing-traces) is the default way to read a trace: what the agent said, what the user said back, and what each tool call did, rendered as a conversation, with the span tree one click away. Browser-agent session replay syncs the agent's DOM state to spans, so you can see what the agent saw when it made a decision.


### Ecosystem fit


#


Native SDKs for Python and TypeScript. Auto-instrumentation for LangChain, LangGraph, CrewAI, AutoGen, Claude Agent SDK, Browser Use, OpenAI Agents SDK, Vercel AI SDK, and raw OpenAI / Anthropic clients. Because it is OTel-native, any OpenInference or OpenLLMetry instrumentation also works.


### Self-host story


#


Laminar is genuinely easy to self-host. The repo ships a production-ready Helm chart: clone, apply, and you are running. No enterprise sales call, no proprietary operator, no "contact us for self-host." All features ship on the OSS image, including Signals, the SQL editor, the debugger, and evals. That is unusual in this category.


### Pricing


#


Data-volume pricing with no seat fees and no per-span unit counting. Free: 1GB/month, 7-day retention. Hobby: $30/month for 3GB then $2/GB, 30-day retention. Pro: $150/month for 10GB then $1.50/GB, 6-month retention, unlimited seats and projects. Enterprise is custom. Self-hosting is free. Because Laminar compresses agent traces ~20x before storing them, that data allowance holds far more real traffic than the raw number suggests.


### Where Laminar is not the right pick


#


- You only log single LLM calls and do not have nested tool use. Simpler tools will do.
- Your entire workflow is prompt versioning and you do not run agents. Langfuse and LangSmith are more specialized there.


## 2. Langfuse


#


**Category:** Open-source LLM observability and prompt management. **License:** MIT. **Deployment:** Cloud, self-host. **Repo:**[github.com/langfuse/langfuse](https://github.com/langfuse/langfuse) .


Langfuse has one of the most explicit data models in the space: traces, observations, sessions, scores. Observations are typed (generations, spans, events), which makes complex flows tractable if you structure your instrumentation correctly.


**Strengths:**


- First-class prompt management with versioning.
- Mature evaluation harness and dataset workflows.
- Full OTLP ingestion endpoint; works as a generic OTel backend.
- Free, MIT-licensed self-host with all core features.
- Agent graph view (beta) for LangGraph-style workflows.


**Weaknesses relative to agent debugging:**


- Trace UX is built around observations, not agent conversations. Reading a 2,000-span agent run is slower than in Laminar.
- No built-in SQL editor. Analysis is API-first.
- No natural-language signal extraction across history, no trace compression.
- Agent graph view is still beta.


**Pricing:** Cloud Hobby is free with 50k observations. Core is $29/month. Pro is $199/month. Enterprise is $2,499/month. Usage is counted in billable units ( traces + observations + scores


), so agents with many small spans can add up fast. See the[full Langfuse comparison](https://laminar.sh/article/langfuse-alternatives-2026) .


## 3. LangSmith


#


**Category:** LLM and agent observability from LangChain. **License:** Closed source. **Deployment:** Cloud, hybrid, self-hosted (Enterprise only).


If your stack is LangChain or LangGraph, LangSmith will give you the tightest out-of-the-box integration. One environment variable and every run is traced.


**Strengths:**


- **LangGraph Studio.** A real agent IDE. Visualize agent graphs, set breakpoints, modify state mid-trajectory, resume from a checkpoint. Nothing else in this list has a comparable purpose-built agent UI.
- **LangSmith Deployment.** Managed agent infrastructure with checkpointing, memory, and scaling.
- Full OpenTelemetry support (as of March 2026).
- Rich real-time dashboards, alerting, conversation clustering.


**Weaknesses:**


- Closed source. Self-hosting is an Enterprise-only add-on.
- Seat-based pricing ($39/seat/month on Plus) adds up for larger teams.
- Tightest fit is still LangChain and LangGraph. Teams on other frameworks get less.
- Trace retention tiering (14 days base, 400 days extended) complicates pricing.


**Pricing:** Developer plan is free with 5k base traces/month. Plus is $39/seat/month. Base traces cost $0.50 per 1k; extended traces (400-day retention) cost $2.50 per 1k.


## 4. Arize Phoenix


#


**Category:** Open-source LLM tracing and evaluation, built on OpenTelemetry. **License:** Elastic License 2.0. **Deployment:** Self-host (pip install), Arize AX managed option.


Phoenix is the open-source side of Arize. It uses[OpenInference](https://github.com/Arize-ai/openinference) , a set of OTel semantic conventions for LLMs that is widely adopted.


**Strengths:**


- OpenTelemetry-native with OpenInference conventions. Instrument once, send anywhere.
- Strong evaluation harness (Phoenix Evals).
- Good for notebook-first workflows; runs locally, spins up in Colab.
- Tight integration with Arize AX if you need production monitoring at enterprise scale.


**Weaknesses:**


- Trace UX is span-tree-first. Not built around agent conversations.
- Elastic License 2.0 is not OSI-approved open source.
- Commercial Arize AX has a different cost curve from open-source Phoenix. Plan ahead if you need to graduate.


**Pricing:** Phoenix open-source is free. Arize AX pricing is custom. Full comparison:[Arize Phoenix alternatives 2026](https://laminar.sh/article/arize-phoenix-alternatives-2026) .


## 5. Weights & Biases Weave


#


**Category:** LLM tracing, evaluation, and experiment tracking. **License:** Closed source. **Deployment:** Cloud, on-prem for enterprise.


If your ML team is already on W&B, Weave fits in the same console. Trace LLM calls, run evals, compare experiments.


**Strengths:**


- Native integration with existing W&B workflows.
- Strong eval framework with scorers and comparisons.
- Good for teams that evaluate models and agents on the same platform.


**Weaknesses:**


- Less agent-first than Laminar or LangSmith. Trace UX is borrowed from ML experiment tracking.
- Weak on realtime trace viewing during long agent runs.
- Closed source.


**Pricing:** Free tier with limited storage. Paid plans scale with trace volume and seats.


## 6. Braintrust


#


**Category:** LLM evaluation platform with tracing. **License:** Closed source. **Deployment:** Cloud, on-prem for enterprise.


Braintrust is eval-first. Tracing exists to feed the eval loop, not to stand alone.


**Strengths:**


- Mature experiment harness: structured scorers, comparisons, regression detection.
- Strong for teams whose primary bottleneck is "did our change break behavior X."
- Clean prompt playground that ties into eval sets.


**Weaknesses:**


- Not a debugger. You will not be faster at finding what broke in production.
- Lighter agent-specific UX.
- Closed source, with a proprietary storage layer.


**Pricing:** Free tier available. Pro is $249/month; Enterprise is custom. Full comparison:[Braintrust alternatives 2026](https://laminar.sh/article/braintrust-alternatives-2026) .


## Head-to-head: who wins each criterion


#


Criterion Winner Why


Agent-specific UX **Laminar** Transcript view, 20x compression, Signals, coding-agent debugger, browser-agent replay.


Trace storage efficiency **Laminar** 20x average compression on agent traces, up to 50x on the longest runs.


LangGraph integration **LangSmith** LangGraph Studio is genuinely the best agent IDE available today.


Open-source self-host **Laminar** Apache 2.0, Helm chart, all features on the OSS image. Langfuse (MIT) is a close second.


OpenTelemetry support **Laminar / Phoenix (tie)** Both OTel-native from day one. Phoenix uses OpenInference conventions.


Prompt management **Langfuse** Mature versioning, caching, and team workflows.


Eval SDK **Laminar / Braintrust (tie)** Code-first, versatile evals on both; Braintrust adds CI scorer sweeps.


Pricing predictability **Laminar** Data-volume pricing on compressed traces, no seat or per-span fees.


## How to pick in under 5 minutes


#


Answer these in order. Stop at the first yes.


1. Are you committed to LangGraph and want an agent IDE? → **LangSmith** .
2. Are you shipping and debugging AI agents and want a transcript view, 20x compression, Signals, SQL, and a coding-agent debugger? → **Laminar** .
3. Is your primary pain prompt versioning, not agent debugging? → **Langfuse** (OSS) or **Braintrust** (commercial).
4. Do you need OpenInference and already run Arize for ML observability? → **Phoenix** .
5. Is your team already on W&B? → **Weave** .


## Open-source scorecard


#


Matters if you self-host, run in air-gapped environments, or want to own your data.


Platform OSS license Self-host


Laminar Apache 2.0 Yes, Helm chart, one command, all features


Langfuse MIT Yes, all features


Phoenix Elastic 2.0 Yes


LangSmith Closed Enterprise only


Weave Closed On-prem for enterprise


Braintrust Closed On-prem for enterprise


## OpenTelemetry scorecard


#


Matters if you already have an OTel pipeline or do not want to marry a specific vendor.


- **Native OTel from day one:** Laminar, Phoenix.
- **Full OTLP endpoint:** Langfuse, LangSmith (as of March 2026).
- **Works via OpenLLMetry / OpenInference:** most of the above, with varying fidelity.


If vendor neutrality matters, instrument once with[OpenLLMetry](https://www.traceloop.com/openllmetry) or[OpenInference](https://github.com/Arize-ai/openinference) and switch backends later without re-instrumenting.


## Why we still recommend Laminar


#


We built Laminar because none of the existing tools solved our own problem: debugging a 30-minute browser agent that failed at minute 18, with no idea which of 2,000 spans to look at first, and paying to store the same conversation re-sent on every one of those spans.


The transcript view was the first thing we built, then 20x compression so storing agent traffic stopped being the expensive part, then Signals because the failure mode you care about today is not the one your dashboards captured a month ago, then the debugger so the coding agent writing your agent could run the fix loop itself. Every one of those came from understanding agents, which is the thing LLM-first tools were not built around.


If you are shipping agents to production, these primitives are your day-to-day reality. Other tools can do pieces of this. None put them together in one product. That is the bias, and we think it is the right one.


Start with the free tier: 1GB of traces, 7-day retention. Instrument one agent. If you do not see the difference in the first hour, come back and tell us why.


[Try Laminar free](https://laminar.sh/sign-up) ·[Read the docs](https://laminar.sh/docs) ·[Star on GitHub](https://github.com/lmnr-ai/lmnr)


## FAQ


#


### What is agent observability?


#


Agent observability is the practice of capturing, inspecting, and debugging the full execution of an AI agent, including every LLM call, tool call, retrieval, and sub-agent invocation. It differs from classical LLM observability because agent runs are long, non-deterministic, deeply nested, and re-send their full conversation on every turn. Good agent observability gives you a readable transcript of what the agent did, compresses the repeated context, tracks the outcomes that matter across every run, and lets a coding agent re-run the agent from any point.


### What is the best open-source agent observability platform in 2026?


#


Laminar (Apache 2.0) is the best open-source agent observability platform for agents in production. It ships a Helm chart for one-command self-host, and every feature is on the OSS image, including Signals, the SQL editor, the debugger, and evals. Langfuse (MIT) is the best pick if prompt management is your core workflow rather than agent debugging. Phoenix (Elastic 2.0) is strong for teams already on Arize or using OpenInference.


### Do I need a dedicated agent observability tool, or is my APM enough?


#


APM tools like Datadog and New Relic can ingest OpenTelemetry spans, but they are built for service-level metrics, not conversational traces. They do not render agent runs as conversations, do not compress the re-sent context, do not support natural-language signal extraction over LLM content, and do not support a coding-agent debugger. If your agent is more than one LLM call deep, a purpose-built tool saves hours.


### Is LangSmith better than Laminar?


#


LangSmith is the better pick if you are committed to LangChain or LangGraph and want LangGraph Studio. Laminar is the better pick for everyone else: it is open-source, OpenTelemetry-native, framework-agnostic, and built for AI agents from the ground up, with 20x trace compression, Signals, SQL over all platform data, and a coding-agent debugger.


### How does Laminar compare to Langfuse?


#


Laminar is optimized for agents: transcript view, 20x trace compression, Signals, a coding-agent debugger, SQL over all platform data, and a code-first eval SDK. Langfuse is optimized for prompt management: versioned prompts, typed observations, an eval harness. Both are open-source. Pick Laminar if you are shipping and debugging agents; pick Langfuse if your workflow centers on prompt iteration. See our full[Laminar vs Langfuse comparison](https://laminar.sh/blog/2026-02-13-laminar-vs-langfuse-why-we-prefer-laminar) .


### Can I send OpenTelemetry traces to any of these platforms?


#


Laminar, Langfuse, LangSmith, and Phoenix all accept OpenTelemetry traces natively or via OTLP. Weave and Braintrust have partial OTel support. If vendor neutrality matters, instrument with OpenLLMetry or OpenInference and you can switch backends without re-instrumenting.


### What does agent observability cost?


#


Pricing models vary. Laminar charges by data volume with no per-seat fees (Free: 1GB at 7-day retention; Hobby: $30/month for 3GB; Pro: $150/month for 10GB at 6-month retention; self-host is free) and compresses agent traces ~20x before storing them. Langfuse charges by billable units (traces + observations + scores). LangSmith charges per seat plus per trace. For agents with large traces, data-volume pricing on compressed data is usually the most predictable and the lowest cost.


*Last updated: July 2026. Verify features and pricing against each vendor's current documentation before committing.*
