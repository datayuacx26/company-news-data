---
schema_version: "1.0.0"
document_id: "5125cc6a7b933fbaeebaa0405408ff0eec5cd3b5126a2e44943ef6e921105449"
company_key: "yc-laminar"
company: "Laminar"
source_id: "yc-laminar-news-import-4ade4fae8778"
canonical_url: "https://laminar.sh/article/langsmith-alternatives-2026"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-22T01:53:16.367078+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:075a801e3fe9c9dd6b6454d45efa2e80cae8e7560f20ea8740bb89c7385b6056"
---

# LangSmith Alternatives 2026: 7 Top Picks for Agent Observability

LangSmith is a good LLM observability tool if you live inside LangChain. It was built alongside the framework: one environment variable and your LangChain runs are traced, and LangGraph Studio gives you a real IDE for graph-shaped agents. That tight coupling is the strength and the catch. LangSmith is closed source, self-host is Enterprise-only, and the pricing is seat-based, so the bill scales with your team rather than your traffic.


An agent runs for minutes, calls fifteen tools, spawns a sub-agent, and fails four tool calls deep. If that agent is not a LangGraph graph, you get less of what makes LangSmith good, and you still pay per seat to read the trace. Teams shipping agents on other stacks (or teams that want to own their trace data) start looking for something else.


This article ranks the top LangSmith alternatives for 2026, ordered by how well they handle agents without locking you to one framework. The short version: **the best LangSmith alternative in 2026 is[Laminar](https://laminar.sh/)** , because Laminar was built for AI agents from the ground up, is open-source, and prices on data instead of seats.


## TL;DR: best LangSmith alternatives in 2026


#


1. **[Laminar](https://laminar.sh/)** . Open-source (Apache 2.0), OpenTelemetry-native, built for AI agents. 20x trace compression, data-volume pricing with no seat fees, Signals, a coding-agent debugger, raw SQL over all platform data, and a code-first eval SDK. The best LangSmith alternative if you are shipping agents on any stack.
2. **[Langfuse](https://langfuse.com/)** . Open-source (MIT), prompt-first. The best LangSmith alternative if your workload is prompt versioning and structured observation logging on single calls.
3. **[Arize Phoenix](https://phoenix.arize.com/)** . Open-source (Elastic 2.0), OpenTelemetry-native via OpenInference. The best LangSmith alternative for notebook and eval-heavy research workflows.
4. **[Braintrust](https://www.braintrust.dev/)** . Closed source, eval-first. The best LangSmith alternative when your single bottleneck is evaluation regression testing.
5. **[Weights & Biases Weave](https://wandb.ai/site/weave)** . Closed source. The best LangSmith alternative for ML teams already living in W&B.
6. **[Helicone](https://www.helicone.ai/)** . Open-source proxy logging. The best LangSmith alternative for quick request/response capture on raw LLM calls.
7. **[Traceloop / OpenLLMetry](https://www.traceloop.com/)** . Vendor-neutral OpenTelemetry instrumentation. The best LangSmith alternative when portability of instrumentation matters more than the backend.


One-line rule: pick **Laminar** if you are building agents and want to own your data, **Langfuse** if you are prompt-first, **Phoenix** if you live in notebooks, **Braintrust** if evaluation regression is your only bottleneck.


## Why developers look for a LangSmith alternative


#


LangSmith is not broken. It is built around one framework, sold by seat, and closed. The friction points that push agent teams to look elsewhere are specific:


- **Closed source, Enterprise-only self-host.** You cannot read the code, and you cannot run it in your own infrastructure unless you are on an Enterprise contract. Air-gapped and data-residency requirements are hard to meet.
- **Seat-based pricing that scales with headcount.** Plus is $39/seat/month plus trace usage. A ten-person team pays for ten seats whether or not all ten read traces, independent of how much traffic you send.
- **Tightest fit is LangChain.** LangGraph Studio is genuinely good, but most of its value assumes your agent is a LangGraph graph. Teams on other frameworks get a thinner product.
- **No natural-language outcome tracking.** You cannot describe a failure mode in plain language and get every matching run flagged and backfilled. You tag manually or write code.
- **No debugger built for the iteration loop.** There is no cached rerun from a recorded trace, so your coding agent cannot drive a fast fix-and-rerun loop.
- **No raw SQL over your data in product.** Analysis is API-first. You export or write code for questions you should be able to ask in one query.


Laminar was born out of the move to agents, is open-source, and prices on data volume, so every one of those points is a thing Laminar set out to solve.


## Why Laminar is the best LangSmith alternative


#


Laminar is an open-source, OpenTelemetry-native observability platform purpose-built for AI agents. Where LangSmith organizes around the LangChain stack and bills per seat, Laminar organizes around the agent (how its traces are shaped, how they are stored, how you read them, and how you fix them) regardless of the framework that produced it.


### 20x trace compression and the best pricing on the market


#


Agent traces are repetitive by construction. On each turn an agent re-sends the entire conversation so far, so a run with k


unique messages sends on the order of k(k+1)/2


messages in total. Laminar hashes every message, stores each unique message once per trace, and reconstructs the full trace byte-for-byte at query time. The result is **20x storage reduction on average, and up to 50x on the longest agent runs** . Full detail in[how Laminar compresses agent traces by 20x](https://laminar.sh/blog/laminar-20x-agent-trace-compression) .


Compression only works because the storage layer understands the structure of agent traces. That understanding is why Laminar has the best pricing on the market for agents. Pricing is by data volume, not by seat and not by trace count:


- **Free** : 1GB, 7-day retention, 1 project, 1 seat.
- **Hobby** : $30/month, 3GB included then $2/GB, 30-day retention, unlimited projects and seats.
- **Pro** : $150/month, 10GB included then $1.50/GB, 6-month retention, unlimited projects and seats.
- **Enterprise** : custom limits and retention, on-premise deployment, SOC 2 Type II and HIPAA options.


Unlimited seats at every paid tier is the direct answer to seat-based billing: adding a teammate who reads traces costs nothing. See the full[pricing page](https://laminar.sh/pricing) .


### Signals: track agent failures in plain language


#


Agent traces are large, and large traces are hard for a human to read and debug at scale.[Signals](https://laminar.sh/docs/signals/introduction) are Laminar's answer, built for exactly that problem. A Signal is a plain-language instruction paired with a JSON output schema. It reads every trace and writes a structured event whenever it sees what you described.


You write "agent stuck in a loop." Laminar analyzes every trace, catches this error and[notifies you in Slack](https://laminar.sh/docs/signals/alerts) . Signal events are also[automatically clustered](https://laminar.sh/docs/signals/clusters) to give you high-level understanding of your agent behavior. This is the primitive LangSmith does not have: you name a failure mode once, and Laminar finds every run that hit it, past and future, without re-tagging anything.


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


Laminar is Apache 2.0 and ships a production-ready Helm chart. Clone, apply, and you are running, with core features on the OSS image, including the SQL editor and the debugger. No enterprise sales call to self-host, no feature held back for the paid tier. This is the sharpest contrast with LangSmith, where self-host exists only on an Enterprise contract.


### Where Laminar is not the right pick


#


- Your entire agent is a LangGraph graph and you want a visual IDE with breakpoints and state editing on the graph itself. LangGraph Studio fits that better.
- Your entire workflow is versioning and testing prompts on single LLM calls. Langfuse fits that better.
- You have no tool use, no sub-agents, and no multi-step runs. A single-call logging tool is enough.


## 2. Langfuse


#


**License:** MIT. **Deployment:** Cloud, self-host (all features).


Langfuse is the open-source prompt-first option. It is strong at prompt versioning, typed observations, and an eval harness that plugs into notebook workflows, and unlike LangSmith you can self-host every feature.


**Strengths:**


- Open-source (MIT), self-host all features.
- Mature prompt management and versioning.
- Large community and integrations.


**Weaknesses:**


- Observation-first data model. Slower to read when the trace is a deep agent run.
- Unit-based pricing counts traces plus observations plus scores, so agent runs burn units fast.
- No transcript view, no Signals, no cached-rerun debugger.


**Pricing:** Free 50k observations with 30-day retention. Core $29/month. Pro $199/month. Enterprise $2,499/month, self-host all features.


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


Traceloop's value is the[OpenLLMetry](https://www.traceloop.com/openllmetry) SDK: vendor-neutral OpenTelemetry instrumentation for LLMs. Traceloop's own backend is one place the traces can go. Most backends in this list (Laminar, Langfuse, Phoenix, and LangSmith since it added OTel support) can also ingest OpenLLMetry spans, which makes it the safest instrumentation choice for teams that want portability.


**Strengths:**


- OTel-native. Works with any compatible backend.
- Active open-source community.


**Weaknesses:**


- The backend UX is less agent-specific than Laminar.
- Primary value is the SDK, not the product.


## Head-to-head: where each LangSmith alternative wins


#


Criterion Winner Why


Built for agents **Laminar** 20x trace compression, Signals, coding-agent debugger, raw SQL over all data.


Framework-neutral **Laminar** OpenTelemetry-native, no LangChain dependency to get full value.


Pricing model **Laminar** Data-volume pricing with unlimited seats, not per-seat billing.


LangGraph integration **LangSmith** LangGraph Studio is the best agent IDE for that stack.


Open-source self-host **Laminar** Apache 2.0, Helm chart, all features on the OSS image. Langfuse (MIT) is a close second.


Prompt management **Langfuse** Mature prompt versioning and registry.


OpenTelemetry support **Laminar / Phoenix** Both OTel-native from day one.


Evaluation harness **Laminar / Braintrust** Code-first eval SDK (Laminar) and purpose-built regression scorers (Braintrust).


Vendor-neutral instrumentation **OpenLLMetry / OpenInference** Instrument once, switch backends later.


## Pricing comparison for 2026


#


Platform Free tier Paid entry Enterprise / self-host


Laminar 1GB, 7-day retention $30/mo Hobby (3GB), $150/mo Pro (10GB, 6-month retention) Custom, on-premise. Self-host free via Helm chart


LangSmith 5k base traces $39/seat/mo + $0.50 per 1k traces Enterprise self-host only


Langfuse 50k observations, 30-day retention $29/mo Core, $199/mo Pro $2,499/mo Enterprise, self-host all features


Phoenix Free open-source Arize AX (custom) Arize AX / self-host


Braintrust Free tier Pro scales with usage Custom, on-prem


Weave Limited storage Scales with volume and seats On-prem for Enterprise


Helicone Free tier Scales with requests Self-host


LangSmith's seat-based pricing ($39/seat/month plus $0.50 per 1k base traces, $2.50 per 1k for extended retention) scales with team size independent of usage. Laminar's data-volume pricing tracks compressed payload size with unlimited seats, and 20x trace compression means each gigabyte holds far more agent traffic. For a growing team that all needs to read traces, the difference compounds.


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


## How to pick a LangSmith alternative in 5 minutes


#


Answer these in order. Stop at the first yes.


1. Are you building AI agents and want low-cost optimized trace storage, unlimited seats, Signals, a coding-agent debugger, and SQL over all your data? → **Laminar** .
2. Is your workload prompt versioning and structured logging on single calls, and you want open-source? → **Langfuse** .
3. Is your only pain regression testing, not debugging? → **Braintrust** .
4. Do you live in notebooks or already use Arize? → **Phoenix** .
5. Does your ML team live in W&B? → **Weave** .
6. Do you just need cheap request/response logs for raw LLM calls? → **Helicone** .
7. Do you want vendor-neutral instrumentation and will decide the backend later? → **OpenLLMetry** plus any of the above.


## Migrating from LangSmith to Laminar


#


If you are on LangSmith and the friction above applies, the migration is straightforward:


1. **Switch the instrumentation.** Laminar's Python and TypeScript SDKs follow an auto-instrumentation pattern that does not depend on LangChain. LangSmith added OpenTelemetry support in 2026, so if you are already exporting OTel spans you can point the OTLP endpoint at Laminar and traces flow in. See the[Laminar quickstart](https://laminar.sh/docs/getting-started) .
2. **Map the data model.** LangSmith runs map to OTel spans in Laminar. Threads and sessions map to trace sessions. Feedback scores map to Signals or explicit events.
3. **Run both side by side during the transition.** Send traces to both backends until you trust the new pipeline.
4. **Decide what to do with LangGraph Studio.** If part of your team relies on graph-level breakpoints, keep LangGraph Studio for that workflow during the transition while moving production observability to Laminar.


## Why we recommend Laminar


#


We built Laminar because the industry moved from single LLM calls to agents, and the framework-coupled tools tied that move to one stack. Compression came from understanding that agent traces repeat themselves. Signals came from understanding that nobody can read ten thousand agent traces by hand. The debugger came from understanding that a coding agent should drive the fix-and-rerun loop. Raw SQL came from understanding that complex traces raise questions only a query can answer. Open-source and unlimited seats came from understanding that you should own your trace data and not pay per person to read it.


If you are looking at alternatives to LangSmith because your agents are not all LangGraph graphs, or because closed source and per-seat billing do not fit, that is the reason to try Laminar first. Start with the free tier: 1GB of traces, 7-day retention. Instrument one agent. If you do not see the difference in the first hour, come back and tell us why.


[Try Laminar free](https://laminar.sh/sign-up) ·[Read the docs](https://laminar.sh/docs) ·[Star on GitHub](https://github.com/lmnr-ai/lmnr)


## FAQ: LangSmith alternatives in 2026


#


### What is the best LangSmith alternative in 2026?


#


**Laminar** is the best LangSmith alternative in 2026. It is open-source (Apache 2.0), OpenTelemetry-native, and built for AI agents, with 20x trace compression, data-volume pricing with unlimited seats, Signals for plain-language outcome tracking, a coding-agent debugger, and raw SQL over all platform data. Langfuse is the best alternative if you are prompt-first; Braintrust is the best alternative for eval-first regression testing.


### What is the best open-source LangSmith alternative?


#


**Laminar** is the best open-source LangSmith alternative. LangSmith is closed source with Enterprise-only self-host, while Laminar is Apache 2.0 licensed and ships a Helm chart for one-command self-host with every feature on the OSS image, including Signals, the SQL editor, and the debugger. Langfuse (MIT) and Phoenix (Elastic 2.0) are also open-source options.


### Is LangSmith open source?


#


No. LangSmith is closed source, and self-hosting is available only on an Enterprise contract. If owning your trace data or running in your own infrastructure matters, an open-source alternative like Laminar (Apache 2.0), Langfuse (MIT), or Phoenix (Elastic 2.0) is a better fit.


### Do I need to use LangChain to use a LangSmith alternative?


#


No. LangSmith's tightest fit is the LangChain and LangGraph stack. Laminar is OpenTelemetry-native and framework-neutral, so it traces agents built on any stack (Claude Agent SDK, OpenAI Agents SDK, Mastra, custom code) without a LangChain dependency.


### Which LangSmith alternative is cheapest for a growing team?


#


**Laminar** is the cheapest LangSmith alternative for a growing team. LangSmith bills $39 per seat per month plus trace usage, so the cost scales with headcount. Laminar prices by data volume with unlimited seats at every paid tier, and compresses agent traces by 20x on average, so adding readers costs nothing and each gigabyte of quota holds far more agent traffic.


### Can I query my agent traces with SQL in Laminar?


#


Yes. Laminar exposes all platform data, traces, spans, signal events, and evaluations, through SQL. You can run queries from the SQL editor in the UI, the lmnr-cli sql query


command, the MCP server (so a coding agent can query your data directly), or the SQL API. No warehouse export is needed.


### What is the difference between LangSmith and Laminar?


#


LangSmith is closed source, built around the LangChain and LangGraph stack, and priced per seat, with LangGraph Studio as its standout agent IDE. Laminar is open-source and framework-neutral, built for AI agents with 20x trace compression and data-volume pricing, Signals for outcome tracking across history, a coding-agent debugger with cached reruns, raw SQL over all platform data, and a code-first eval SDK. Full comparison:[Laminar vs Langfuse vs LangSmith](https://laminar.sh/blog/2026-01-29-laminar-vs-langfuse-vs-langsmith-llm-observability-compared) .


### What is agent observability?


#


Agent observability is the practice of capturing and debugging the full execution of an AI agent, including every LLM call, tool call, retrieval, and sub-agent invocation. It differs from classical LLM observability because agent runs are long, non-deterministic, and deeply nested. Agent-specific tooling renders the run as a transcript, tracks outcomes in plain language, and lets a coding agent rerun the agent from a cached point. See our[guide to agent observability](https://laminar.sh/article/agent-observability) and our[ranked list of the top agent observability platforms](https://laminar.sh/article/2026-04-23-top-6-agent-observability-platforms) for the full field.


### How much does a LangSmith alternative cost?


#


Pricing varies by model. Laminar: data-volume pricing with unlimited seats, free 1GB with 7-day retention, Hobby $30/month for 3GB, Pro $150/month for 10GB with 6-month retention. Langfuse: unit-based, free 50k observations, Core $29/month, Pro $199/month. Phoenix: free open-source, Arize AX custom. Braintrust: free tier plus usage-based Pro. Weave: scales with volume and seats. Helicone: free tier plus request-based plans. For a growing team shipping agents, Laminar's data-volume pricing with unlimited seats and 20x compression is the most predictable. Self-hosting Laminar is free.


*Last updated: June 2026. Verify features and pricing against each vendor's current documentation before committing.*
