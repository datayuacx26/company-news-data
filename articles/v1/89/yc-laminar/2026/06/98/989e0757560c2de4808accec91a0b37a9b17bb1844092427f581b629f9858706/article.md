---
schema_version: "1.0.0"
document_id: "989e0757560c2de4808accec91a0b37a9b17bb1844092427f581b629f9858706"
company_key: "yc-laminar"
company: "Laminar"
source_id: "yc-laminar-news-import-4ade4fae8778"
canonical_url: "https://laminar.sh/article/langfuse-alternatives-2026"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-22T01:53:16.367078+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:1b3b0e27db363926d7d9398b5dcb926794cb3fa3ce902c0d7beb54d4c23edb14"
---

# Langfuse Alternatives 2026: 7 Top Picks for Agent Observability

Langfuse is a good open-source LLM observability tool. It was built for single LLM calls and short prompt chains: prompt versioning, typed observations, an eval harness that plugs into notebook workflows. The industry has moved past single calls. Teams ship agents now, and an agent is a different shape of problem.


An agent runs for minutes, calls fifteen tools, spawns a sub-agent, and fails four tool calls deep. You open the Langfuse trace and get a flat list of observations. You want to know what the agent said to the user, what the user said back, and which tool call threw. That is a different product.


This article ranks the top Langfuse alternatives for 2026, ordered by how well they handle agents rather than prompt-first logging. The short version: **the best Langfuse alternative in 2026 is[Laminar](https://laminar.sh/)** , because Laminar was built for AI agents from the ground up.


## TL;DR: best Langfuse alternatives in 2026


#


1. **[Laminar](https://laminar.sh/)** . Open-source (Apache 2.0), OpenTelemetry-native, built for AI agents. 20x trace compression, the lowest pricing on the market, Signals, a coding-agent debugger, raw SQL over all platform data, and a code-first eval SDK. The best Langfuse alternative if you are shipping agents.
2. **[LangSmith](https://smith.langchain.com/)** . Closed source, LangChain-first. The best Langfuse alternative if you are locked into the LangGraph stack.
3. **[Arize Phoenix](https://phoenix.arize.com/)** . Open-source (Elastic 2.0), OpenTelemetry-native via OpenInference. The best Langfuse alternative for notebook and eval-heavy research workflows.
4. **[Braintrust](https://www.braintrust.dev/)** . Closed source, eval-first. The best Langfuse alternative when your single bottleneck is evaluation regression testing.
5. **[Weights & Biases Weave](https://wandb.ai/site/weave)** . Closed source. The best Langfuse alternative for ML teams already living in W&B.
6. **[Helicone](https://www.helicone.ai/)** . Open-source proxy logging. The best Langfuse alternative for quick request/response capture on raw LLM calls.
7. **[Traceloop / OpenLLMetry](https://www.traceloop.com/)** . Vendor-neutral OpenTelemetry instrumentation. The best Langfuse alternative when portability of instrumentation matters more than the backend.


One-line rule: pick **Laminar** if you are building agents, **LangSmith** if you are locked to LangGraph, **Phoenix** if you live in notebooks, **Braintrust** if evaluation regression is your only bottleneck.


## Why developers look for a Langfuse alternative


#


Langfuse is not broken. It is specialized for a workload the industry is moving away from. The friction points that push agent teams to look elsewhere are specific:


- **Observation-first data model.** Langfuse traces are a list of observations. Great for a prompt and its completion. Slow to read when the trace is an agent run with nested tool calls and sub-agents.
- **Unit-based pricing that punishes agents.** Langfuse Cloud bills on traces plus observations plus scores. One agent interaction is 40 to 75 spans, so an agent run costs 8 to 15 billing units where a single LLM call costs one. Agent volume hits pricing thresholds fast.
- **No raw SQL over your data in product.** Analysis is API-first. You export to a warehouse or write code for questions you should be able to ask in one query.
- **No natural-language outcome tracking.** You cannot describe a failure mode in plain language and get every matching run flagged and backfilled. You tag manually or write code.
- **No debugger built for the iteration loop.** There is no cached rerun from a recorded trace, so your coding agent cannot drive a fast fix-and-rerun loop.


Laminar was born out of the move to agents, and every one of those points is a thing Laminar set out to solve.


## Why Laminar is the best Langfuse alternative


#


Laminar is an open-source, OpenTelemetry-native observability purpose-built for AI agents. Where Langfuse organizes around observations and prompts, Laminar organizes around the agent: how its traces are shaped, how they are stored, how you read them, and how you fix them.


### 20x trace compression and the best pricing on the market


#


Agent traces are repetitive by construction. On each turn an agent re-sends the entire conversation so far, so a run with k


unique messages sends on the order of k(k+1)/2


messages in total. Laminar hashes every message, stores each unique message once per trace, and reconstructs the full trace byte-for-byte at query time. The result is **20x storage reduction on average, and up to 50x on the longest agent runs** . Full detail in[how Laminar compresses agent traces by 20x](https://laminar.sh/blog/laminar-20x-agent-trace-compression) .


Compression only works because the storage layer understands the structure of agent traces. That understanding is why Laminar has the best pricing on the market for agents. Pricing is by data volume, not by trace or observation count and not by seat:


- **Free** : 1GB, 7-day retention, 1 project, 1 seat.
- **Hobby** : $30/month, 3GB included then $2/GB, 30-day retention, unlimited projects and seats.
- **Pro** : $150/month, 10GB included then $1.50/GB, 6-month retention, unlimited projects and seats.
- **Enterprise** : custom limits and retention, on-premise deployment, SOC 2 Type II and HIPAA options.


Because Laminar stores the compressed trace, a gigabyte of Laminar quota holds far more agent traffic than a gigabyte priced against raw payload, and the data-volume model stays predictable as traces grow. See the full[pricing page](https://laminar.sh/pricing) .


### Signals: track agent failures in plain language


#


Agent traces are large, and large traces are hard for a human to read and debug at scale.[Signals](https://laminar.sh/docs/signals/introduction) are Laminar's answer, built for exactly that problem. A Signal is a plain-language instruction paired with a JSON output schema. It reads every trace and writes a structured event whenever it sees what you described.


You write "agent stuck in a loop." Laminar analyzes every trace, catches this error and[notifies you in slack](https://laminar.sh/docs/signals/alerts) . Signal events are also[automatically clustered](https://laminar.sh/docs/signals/clusters) to give you high level understanding of your agent behavior. This is the primitive Langfuse does not have: you name a failure mode once, and Laminar finds every run that hit it, past and future, without re-tagging anything.


### Debugger: let your coding agent drive the fix-and-rerun loop


#


Building an agent is a loop: run it, read what it did, change something, run it again. Laminar's[debugger](https://laminar.sh/docs/debugger/introduction) is that loop, built for a coding agent to run. Claude Code, Codex, or Cursor runs your agent with LMNR_DEBUG=true


, reads the resulting trace in Laminar to see what worked and what didn't, edits your code, and reruns, all driven through the Laminar CLI.


The rerun is the point. Laminar serves cached responses up to the call your coding agent is testing and runs only the change and everything after it live. A run that takes minutes end to end becomes a turn your coding agent can take dozens of times in the time one live run would cost. The failing call is often three-quarters of the way through a trace; caching the prefix is what makes iterating on it cheap.


### Raw SQL over all platform data, from the CLI, MCP, or API


#


Agent traces are complex and raise questions that only a query can answer: how many runs called tool X more than five times and then errored, which model version regressed on latency last Tuesday, what every failed checkout had in common. Laminar exposes all platform data, traces, spans, signal events, evaluations, through a[SQL interface](https://laminar.sh/docs/platform/sql-editor) .


You reach that SQL three ways, so whoever is asking the question (you or your coding agent) can ask it where they are:


- The **SQL editor** in the Laminar UI.
- The **[CLI](https://laminar.sh/docs/platform/cli)** : lmnr-cli sql query


from your terminal or a coding agent's shell.
- The **[MCP server](https://laminar.sh/docs/platform/mcp)** : your coding agent queries your trace data directly while it works.
- The **SQL API** for programmatic access from your own code.


No warehouse export, no API pagination loop. The question and the answer live in the same place as the traces.


### Evals: a code-first, barebones SDK


#


Laminar's[evaluations](https://laminar.sh/docs/evaluations/introduction) follow a code-first philosophy: a small, unopinionated SDK that makes versatile evals easy to write, because agent behavior is too varied for a rigid eval form. You define datapoints, an executor function that produces an output, and one or more evaluator functions that score it. Laminar runs them in parallel, traces every call, and stores the scores so you can[compare runs](https://laminar.sh/docs/evaluations/comparing-runs) over time.


### Open-source and easy to self-host


#


Laminar is Apache 2.0 and ships a production-ready Helm chart. Clone, apply, and you are running, with core features on the OSS image, including the SQL editor, and the debugger. No enterprise sales call to self-host, no feature held back for the paid tier.


### Where Laminar is not the right pick


#


- Your entire workflow is versioning and testing prompts on single LLM calls. Langfuse still fits that better.
- You have no tool use, no sub-agents, and no multi-step runs. A single-call logging tool is enough.


## 2. LangSmith


#


**License:** Closed source. **Deployment:** Cloud, hybrid, self-hosted (Enterprise only).


If your stack is LangChain or LangGraph, LangSmith fits like a glove. One environment variable and runs are traced.[LangGraph Studio](https://smith.langchain.com/) is the best agent IDE available for that stack: visualize the graph, set breakpoints, modify state mid-run, resume from a checkpoint.


**Strengths:**


- LangGraph Studio (a real agent IDE, not just a viewer).
- Managed deployment with checkpointing and memory.
- OpenTelemetry support added in 2026.


**Weaknesses:**


- Closed source. Self-hosting is Enterprise-only.
- Seat-based pricing ($39/seat/month on Plus) gets expensive with larger teams.
- Tightest fit is still LangChain. Teams on other frameworks get less.


**Pricing:** Developer free with 5k base traces/month. Plus $39/seat/month plus $0.50 per 1k base traces. Extended-retention traces cost $2.50 per 1k.


## 3. Arize Phoenix


#


**License:** Elastic License 2.0. **Deployment:** Self-host (pip install), Arize AX managed option.


Phoenix is the open-source side of Arize. It uses[OpenInference](https://github.com/Arize-ai/openinference) , a widely adopted set of OTel semantic conventions for LLM spans.


**Strengths:**


- OpenTelemetry-native with OpenInference. Instrument once, send anywhere.
- Strong evaluation harness (Phoenix Evals).
- Notebook-friendly; runs in Colab or locally.


**Weaknesses:**


- Trace UX is span-tree-first. No transcript view.
- Less purpose-built for agents than Laminar.
- Commercial Arize AX has a different cost curve. Plan ahead if you need to graduate.


**Pricing:** Phoenix is free. Arize AX pricing is custom.


## 4. Braintrust


#


**License:** Closed source. **Deployment:** Cloud, on-prem for Enterprise.


Braintrust is eval-first. Tracing exists to feed the eval loop, not to stand alone.


**Strengths:**


- Mature scorers, comparisons, regression detection.
- Clean prompt playground tied to eval sets.
- Strong if your bottleneck is "did this change break behavior X."


**Weaknesses:**


- Not a debugger. You will not be faster at finding what broke in production.
- Lighter agent-specific UX.
- Closed source.


**Pricing:** Free tier available. Pro scales with usage. Enterprise custom.


## 5. Weights & Biases Weave


#


**License:** Closed source. **Deployment:** Cloud, on-prem for Enterprise.


Weave plugs tracing into the existing W&B console. If your ML team already lives there, it is the path of least friction.


**Strengths:**


- Native W&B integration.
- Strong eval framework with scorers and comparisons.
- Good for teams evaluating models and agents on the same platform.


**Weaknesses:**


- Trace UX borrowed from ML experiment tracking. Not agent-first.
- Weak on realtime trace viewing during long runs.
- Closed source.


**Pricing:** Free tier with limited storage. Paid plans scale with volume and seats.


## 6. Helicone


#


**License:** Apache 2.0. **Deployment:** Cloud, self-host.


Helicone is a proxy that sits in front of the LLM provider and logs every request. Simplest integration of any tool in this list: change a base URL.


**Strengths:**


- Zero-code proxy integration.
- Caching, rate-limit, and retry built into the proxy.
- Cheap to get started.


**Weaknesses:**


- Request/response focused, not span-based. Multi-step agents are stitched after the fact.
- No transcript view, no Signals, no debugger.
- Proxy model adds a hop to every LLM call.


**Pricing:** Free tier. Paid plans scale with request volume.


## 7. Traceloop / OpenLLMetry


#


**License:** Apache 2.0 (OpenLLMetry SDK). **Deployment:** Cloud backend, vendor-neutral SDK.


Traceloop's value is the[OpenLLMetry](https://www.traceloop.com/openllmetry) SDK: vendor-neutral OpenTelemetry instrumentation for LLMs. Traceloop's own backend is one place the traces can go. Most backends in this list (Laminar, Langfuse, Phoenix, LangSmith) can also ingest OpenLLMetry spans, which makes it the safest instrumentation choice for teams that want portability.


**Strengths:**


- OTel-native. Works with any compatible backend.
- Active open-source community.


**Weaknesses:**


- The backend UX is less agent-specific than Laminar or LangSmith.
- Primary value is the SDK, not the product.


## Head-to-head: where each Langfuse alternative wins


#


Criterion Winner Why


Built for agents **Laminar** 20x trace compression, Signals, coding-agent debugger, raw SQL over all data.


Trace storage cost **Laminar** 20x average compression of agent traces, billed by data volume, not span count.


Pricing for agents **Laminar** Data-volume pricing with no per-span units and no seat fees.


LangGraph integration **LangSmith** LangGraph Studio is the best agent IDE for that stack.


Open-source self-host **Laminar** Apache 2.0, Helm chart, all features on the OSS image. Langfuse (MIT) is a close second.


OpenTelemetry support **Laminar / Phoenix** Both OTel-native from day one.


Evaluation harness **Laminar / Braintrust** Code-first eval SDK (Laminar) and purpose-built regression scorers (Braintrust).


Vendor-neutral instrumentation **OpenLLMetry / OpenInference** Instrument once, switch backends later.


## Pricing comparison for 2026


#


Platform Free tier Paid entry Enterprise / self-host


Laminar 1GB, 7-day retention $30/mo Hobby (3GB), $150/mo Pro (10GB, 6-month retention) Custom, on-premise. Self-host free via Helm chart


Langfuse 50k observations, 30-day retention $29/mo Core, $199/mo Pro $2,499/mo Enterprise, self-host all features


LangSmith 5k base traces $39/seat/mo + $0.50 per 1k traces Enterprise self-host


Phoenix Free open-source Arize AX (custom) Arize AX / self-host


Braintrust Free tier Pro scales with usage Custom, on-prem


Weave Limited storage Scales with volume and seats On-prem for Enterprise


Helicone Free tier Scales with requests Self-host


Laminar's data-volume pricing tracks compressed payload size, and 20x trace compression means each gigabyte holds far more agent traffic. Unit-based pricing (Langfuse) counts traces plus observations plus scores, so an agent run with 40 to 75 spans burns 8 to 15 units against a single call's one. Seat-based pricing (LangSmith) scales with team size independent of usage.


## Open-source scorecard


#


Matters if you self-host, run in air-gapped environments, or want to own the trace data.


Platform License Self-host All features on self-host


Laminar Apache 2.0 Yes, Helm chart, one command Yes


Langfuse MIT Yes Yes


Phoenix Elastic 2.0 Yes Yes


Helicone Apache 2.0 Yes Yes


OpenLLMetry SDK Apache 2.0 N/A (SDK) N/A


LangSmith Closed Enterprise only N/A


Braintrust Closed Enterprise only N/A


Weave Closed On-prem Enterprise N/A


## How to pick a Langfuse alternative in 5 minutes


#


Answer these in order. Stop at the first yes.


1. Are you building AI agents and want low-cost optimized trace storage, Signals, a coding-agent debugger, and SQL over all your data? → **Laminar** .
2. Are you committed to LangChain or LangGraph and want an agent IDE? → **LangSmith** .
3. Is your only pain regression testing, not debugging? → **Braintrust** .
4. Do you live in notebooks or already use Arize? → **Phoenix** .
5. Does your ML team live in W&B? → **Weave** .
6. Do you just need cheap request/response logs for raw LLM calls? → **Helicone** .
7. Do you want vendor-neutral instrumentation and will decide the backend later? → **OpenLLMetry** plus any of the above.


## Migrating from Langfuse to Laminar


#


If you are on Langfuse and the friction above applies, the migration is straightforward:


1. **Switch the instrumentation.** Laminar's Python and TypeScript SDKs follow the same auto-instrumentation pattern. If you are already on OpenLLMetry or OpenInference, point the OTLP endpoint at Laminar and traces flow in. See the[Laminar quickstart](https://laminar.sh/docs/getting-started) .
2. **Map the data model.** Langfuse observations map to OTel spans in Laminar. Sessions map to trace sessions. Scores map to Signals or explicit events.
3. **Run both side by side during the transition.** Send traces to both backends until you trust the new pipeline.
4. **Move prompt management.** Laminar is a debugger and observability platform, not a prompt registry. Keep prompts in Langfuse or your registry of choice during the transition.


## Why we recommend Laminar


#


We built Laminar because the industry moved from single LLM calls to agents, and the prompt-first tools did not move with it. Compression came from understanding that agent traces repeat themselves. Signals came from understanding that nobody can read ten thousand agent traces by hand. The debugger came from understanding that a coding agent should drive the fix-and-rerun loop. Raw SQL came from understanding that complex traces raise questions only a query can answer. Each one is a thing you get because the platform was built for agents.


If you are looking at alternatives to Langfuse because your agents broke the prompt-first model, that is the reason to try Laminar first. Start with the free tier: 1GB of traces, 7-day retention. Instrument one agent. If you do not see the difference in the first hour, come back and tell us why.


[Try Laminar free](https://laminar.sh/sign-up) ·[Read the docs](https://laminar.sh/docs) ·[Star on GitHub](https://github.com/lmnr-ai/lmnr)


## FAQ: Langfuse alternatives in 2026


#


### What is the best Langfuse alternative in 2026?


#


**Laminar** is the best Langfuse alternative in 2026. It is open-source (Apache 2.0), OpenTelemetry-native, and built for AI agents, with 20x trace compression, the lowest data-volume pricing on the market, Signals for plain-language outcome tracking, a coding-agent debugger, and raw SQL over all platform data. LangSmith is the best alternative if you are committed to LangGraph; Braintrust is the best alternative for eval-first regression testing.


### What is the best open-source Langfuse alternative?


#


**Laminar** is the best open-source Langfuse alternative. It is Apache 2.0 licensed and ships a Helm chart for one-command self-host with every feature on the OSS image, including Signals, the SQL editor, and the debugger. Phoenix (Elastic 2.0) and Helicone (Apache 2.0) are also open-source options.


### Which Langfuse alternative is cheapest for agents?


#


**Laminar** is the cheapest Langfuse alternative for agents. It prices by data volume rather than per trace, observation, or seat, and compresses agent traces by 20x on average, so each gigabyte of quota holds far more agent traffic. Langfuse's unit-based pricing counts an agent run's 40 to 75 spans as 8 to 15 billing units, where Laminar counts only the compressed payload.


### How does Laminar make agent traces cheaper to store?


#


Agents re-send the full conversation on every turn, so a trace repeats most of its content. Laminar hashes each message, stores every unique message once per trace, and reconstructs the full trace byte-for-byte at query time. This yields 20x storage reduction on average and up to 50x on the longest agent runs, which is why Laminar's data-volume pricing is the lowest for agent workloads.


### Can I query my agent traces with SQL in Laminar?


#


Yes. Laminar exposes all platform data, traces, spans, signal events, and evaluations, through SQL. You can run queries from the SQL editor in the UI, the lmnr-cli sql query


command, the MCP server (so a coding agent can query your data directly), or the SQL API. No warehouse export is needed.


### Is Laminar a drop-in replacement for Langfuse?


#


Close, but not identical. Laminar ingests OpenTelemetry, so if you already use OpenLLMetry or OpenInference you can point the exporter at Laminar without re-instrumenting. Langfuse observations map cleanly to OTel spans. Prompt management is the one area where Laminar does not overlap with Langfuse; treat that workflow separately.


### What is the difference between Langfuse and Laminar?


#


Langfuse is optimized for prompt versioning, evaluation, and structured observation logging on single LLM calls or short chains. Laminar is built for AI agents: 20x trace compression and data-volume pricing, Signals for outcome tracking across history, a coding-agent debugger with cached reruns, raw SQL over all platform data, and a code-first eval SDK. Full comparison:[Laminar vs Langfuse](https://laminar.sh/blog/2026-02-13-laminar-vs-langfuse-why-we-prefer-laminar) .


### What is agent observability?


#


Agent observability is the practice of capturing and debugging the full execution of an AI agent, including every LLM call, tool call, retrieval, and sub-agent invocation. It differs from classical LLM observability because agent runs are long, non-deterministic, and deeply nested. Agent-specific tooling renders the run as a transcript, tracks outcomes in plain language, and lets a coding agent rerun the agent from a cached point. See our[ranked list of the top agent observability platforms](https://laminar.sh/article/2026-04-23-top-6-agent-observability-platforms) for the full field.


### How much does a Langfuse alternative cost?


#


Pricing varies by model. Laminar: data-volume pricing, free 1GB with 7-day retention, Hobby $30/month for 3GB, Pro $150/month for 10GB with 6-month retention. LangSmith: seats plus traces, $39/seat/month plus $0.50 per 1k base traces. Phoenix: free open-source, Arize AX custom. Braintrust: free tier plus usage-based Pro. Weave: scales with volume and seats. Helicone: free tier plus request-based plans. For agents with large traces, Laminar's data-volume pricing with 20x compression is the most predictable. Self-hosting Laminar is free.


*Last updated: June 2026. Verify features and pricing against each vendor's current documentation before committing.*
