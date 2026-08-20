---
schema_version: "1.0.0"
document_id: "36241780f0dd28db1cfc4a874fcb262bab1b2092a9887979fd38c80929ef687c"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/ai-agent-observability"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:57:46.286490+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:15cba76788027bd696d74fa8c15cf5bfa5f47d63667c6ee40c3a983b014a2162"
---

# AI agent observability: a complete guide for production teams

Your AI agents return 200 OK even when they produce wrong answers, burn through token budgets, or silently drift off-task. Traditional application monitoring cannot catch these failures because it was never designed for non-deterministic, tool-calling systems.


According to the[State of AI Agent Security report from Gravitee](https://www.gravitee.io/state-of-ai-agent-security) , 88% of organizations running AI agents reported some form of incident, yet confirmed incident rates actually dropped between December 2025 and April 2026, even as agent fleets doubled in size. The researchers attribute this gap to underreporting and detection failure, not improved security: agents are failing more often than teams can see. AI agent observability gives you the visibility to detect, diagnose, and fix problems that metrics dashboards alone will never surface.


This guide covers what AI agent observability is, why it matters for production workloads, how to instrument your agents, and which practices separate teams shipping reliable agents from teams firefighting in the dark.


## What is AI agent observability?


When you deploy an agent, you need to understand not just whether it responded, but why it chose a particular path and whether the result was correct. AI agent observability is the practice of collecting and analyzing telemetry from every stage of an agent's execution: the prompts it receives, the reasoning steps it takes, the tools it calls, and the outputs it returns. It extends traditional observability concepts (metrics, logs, traces) with AI-specific signals like token consumption, model selection, eval scores, and decision-path context.


Where traditional monitoring asks "is the system up?", agent observability asks "why did this agent choose that tool, and was the answer correct?"


### How it differs from traditional software observability


Your existing observability stack tracks request latency, error rates, CPU utilization, and throughput. Those signals assume deterministic code paths. An HTTP handler that returns the same response for the same input is straightforward to monitor with dashboards and alerts.


Agents break that assumption. The same user query can produce different tool-call sequences, different intermediate reasoning, and different final outputs on consecutive runs. You need telemetry that captures not just the response, but the full decision trace leading to it.


Traditional APM also lacks vocabulary for agent-specific data: token counts, model identifiers, prompt versions, eval results, and context-window utilization. Without these signals, you cannot answer the questions that matter most in production.


### Why agents introduce unique observability challenges


You face three categories of challenge that do not exist in conventional software. First, agents are non-deterministic. Two identical inputs can yield different tool-call chains and outputs, making it harder to write pass/fail assertions. Second, agents accumulate context over multi-turn interactions, and that context directly affects downstream decision-making processes. A subtle error early in a conversation can compound across turns.


Third, agents compose multiple external systems: large language models, vector databases, external APIs into a single execution. Failures can originate anywhere in that chain. Without end-to-end traces that span every hop, root cause analysis becomes guesswork.


## Why AI agent observability matters


Running agents without observability is viable during prototyping, but the risks escalate the moment agents touch customers, revenue, or regulated data. Here is what goes wrong when you skip it.


### Risks of running unobservable agents in production


Unobservable agents fail silently. A customer-facing agent can hallucinate incorrect information for hours before anyone notices because your HTTP error rate stays flat. Token costs can spike overnight when an agent enters a retry loop, and without token-level telemetry you only discover the problem on your next invoice.


The problem extends to agentic AI systems running in production without human oversight. Governance becomes impossible when you cannot answer basic questions about what your agents decided, why they decided it, and whether those decisions were correct.


### Trust, compliance, and accountability requirements


If your agent operates in a regulated domain (healthcare, finance, legal), you need an audit trail showing every input, reasoning step, and output. Compliance teams will ask how a specific decision was reached. Without traces and logs that capture the full decision path, you cannot answer.


Even outside regulated industries, your users and stakeholders need to trust agent outputs. When an agent makes a visible mistake, the first question is always "why did it do that?" Observability gives you the data to answer quickly and fix the root cause rather than guessing.


## How AI agent observability works


At a high level, your observability pipeline has three stages: data collection during agent execution, ingestion into a storage and query layer, and analysis that drives actions like alerts, debugging, or model changes.


### Data collected during an agent run


Every time you trigger an agent execution, it generates multiple categories of telemetry. Traces capture the end-to-end flow from user input through tool calls and LLM invocations to final output, structured as a tree of spans. Logs record discrete events: which tool was called, what parameters were passed, what the tool returned, and any errors encountered.


Performance metrics quantify behavior across runs: latency per span, total tokens consumed (input and output), cost per request, error rates, and model identifiers. Eval results attach quality scores to individual outputs, measuring dimensions like factual accuracy, task completion, and safety.


### Collecting and ingesting observability data


You have two main instrumentation approaches. The first is framework-native instrumentation, where your agent framework emits telemetry automatically. The second is explicit instrumentation, where you add tracing calls at key points in your code.


Most production setups use OpenTelemetry (OTel) as the wire format. OTel provides a vendor-neutral standard for traces and spans, meaning your telemetry can flow into any compatible backend. Platforms like Arize and Langfuse implement OTel's semantic conventions for AI, adding GenAI-specific attributes for prompts, token counts, and model metadata.


### Analyzing and acting on that data


Once telemetry lands in your backend, you use it in three ways. Dashboards surface aggregate trends: average latency, token cost per user session, error-rate spikes. Trace explorers let you drill into individual executions to see exactly what went wrong. Alerts notify you when metrics cross thresholds you define, like cost per trace exceeding a dollar or eval scores dropping below a minimum.


The most valuable analysis is often qualitative. Your team reviews individual traces, reads the prompts and outputs, and classifies failures by type. That classification drives targeted improvements to prompts, tool design, or model selection.


## The core pillars of observability for AI agents


Your agent observability practice rests on four pillars. Each captures a different dimension of agent behavior, and you need all four to get a complete picture. The following sections break down what each pillar measures and why it matters for your production deployments.


### Traces and spans across tool calls and LLM steps


When you inspect a trace, you see one complete agent execution represented as a tree of spans. Each span covers a discrete unit of work: an LLM call, a tool invocation, a retrieval query, or a post-processing step. Spans carry timing data, input/output payloads, status codes, and metadata like the model used.


Traces answer the structural question: what did the agent do, in what order, and how long did each step take? In a well-instrumented system, you can open a trace and immediately see whether a slow response came from a sluggish API call, a large prompt, or an unnecessary retry loop.


### Metrics: latency, token usage, cost, and error rates


Metrics give you the aggregate view. Track these at minimum:


-


**Latency per span and per trace:** identifies bottlenecks in tool calls or model inference


-


**Token usage (input and output):** the primary cost driver for most agent workloads


-


**Cost per trace:** calculated from token usage and model pricing, essential for unit economics


-


**Error rates by type:** distinguish LLM errors, tool failures, and timeout errors


-


**Model distribution:** which models handle which requests, useful when you route across providers


### Logs and structured event capture


Your logs provide the fine-grained narrative that metrics cannot capture. Every tool call, every prompt sent to the model, every intermediate result returned by a retrieval step should be logged as structured events with timestamps and trace IDs so you can correlate them with spans.


Structured logs are especially valuable for debugging edge cases. A latency p99 spike might look alarming in a dashboard, but the log for that specific trace might reveal a one-off network timeout rather than a systemic problem.


### Evals and output quality scoring


Evals are what make agent observability fundamentally different from traditional APM. You need a systematic way to measure whether your agent outputs are correct, complete, safe, and aligned with user intent. Online evaluations run against live production traffic and catch distribution shifts; offline evaluations run on fixed datasets before each deploy and catch regressions.


The following table summarizes the most common eval types and when to apply them:


**Eval type** **Description** **When to run**


LLM-as-judge A second model scores the agent's output against a rubric Online (sampled production traffic) and offline (pre-deploy regression suites)


Tool-call accuracy Checks whether the agent called the right tools with the right parameters Offline against curated test cases; online for anomaly detection


Task completion Measures whether the agent finished the job the user asked for Both online and offline; ties directly to business KPIs


Multi-turn coherence Evaluates whether the agent maintained context and stayed on task across a conversation Offline against multi-turn test scenarios; online for long-running sessions


If your team has reached production maturity, run both online and offline evals for the tightest feedback loop.


## Observability in multi-agent systems


Most production architectures eventually grow beyond a single agent. When multiple agents coordinate on a task, as in LangChain or LangGraph-based orchestration, or AutoGen-style workflows, observability becomes both harder and more important.


### Tracing across agent handoffs and orchestration layers


In your multi-agent system, one agent might plan the task, hand subtasks to specialist agents, and aggregate their results. Your traces need to span the full graph: from the supervisor agent through every subagent and back.


This requires propagating trace context across agent boundaries. If your supervisor agent calls a subagent as a tool, the subagent's spans should appear as children of the supervisor's span. Without that linkage, you see isolated traces per agent but cannot reconstruct the end-to-end flow.


### Attributing failures in complex agent graphs


When you trace a multi-agent workflow that produces a bad output, the failure might originate in any node of the graph. A subagent might return a subtly wrong intermediate result that the supervisor agent incorporates without question. Your traces need to capture inputs and outputs at every handoff point so you can pinpoint where the error entered the pipeline.


Context sharing between subagents also matters here. If subagents work in isolation without seeing each other's context, they can produce conflicting intermediate products. Observability helps you detect these conflicts by comparing subagent outputs within a single trace.


## AI agent observability best practices


Not every practice needs to land on day one. Start with the ones that give you the highest signal for your current stage, and layer on complexity as your deployment matures.


### Evaluate agents continuously in development and production


Your agents should face evaluation before every deploy and continuously after. Offline evals run against a fixed dataset and catch regressions. Online evals sample live traffic and catch real-world issues that synthetic data misses.


Build your eval dataset in layers. Start with hand-curated examples that force clear thinking about what "good" looks like. Add synthetic examples for coverage. Once you have production traffic, mine it for high-signal test cases that reflect real user behavior.


### Integrate evaluations into CI/CD pipelines


Treat eval scores like test results. If a code change drops accuracy below your threshold, the build fails. This prevents regressions from reaching production and makes quality a first-class engineering constraint rather than a manual review step.


Run evals on every pull request that touches agent logic, prompt templates, or tool definitions. Compare results against the previous baseline and require explicit approval for any score decrease.


### Scan for vulnerabilities and prompt-injection risks before release


Your agents are exposed to untrusted input: user messages, uploaded documents, web content. Prompt injection attacks embed malicious instructions in that content, attempting to override your agent's system prompt.


Before deploying, test your agents against adversarial inputs. Simulate prompt injection, jailbreak attempts, and PII extraction requests. Add input guardrails that intercept and sanitize messages before they reach the model, and output guardrails that screen generated responses for data leakage or policy violations.


### Monitor agents in production with tracing, evaluations, and alerts


Effective AI agent monitoring combines three layers. Real-time metrics dashboards track latency, error rates, token costs, and throughput. Continuous evals sample live traffic and score outputs on quality and safety dimensions. Alerts fire when any metric crosses a threshold you define.


Set alerts for cost anomalies (sudden spikes in token usage), quality degradation (eval scores dropping), and operational failures (elevated error rates on tool calls). The goal is to detect problems in minutes, not days.


### Benchmark model selection with data-driven leaderboards


Select models based on measured performance against your specific tasks, not generic benchmarks. Run your eval suite against candidate models, compare quality, latency, and cost, and choose based on evidence.


Revisit this regularly. Model providers release updates, new models become available, and your workload evolves. A model that was optimal six months ago may no longer be the best choice. Normalize your benchmarks across providers before drawing conclusions.


## Use cases for AI agent observability


Your observability data serves multiple purposes beyond debugging. Each use case extracts different value from the same underlying telemetry.


### Monitoring service health and performance


You need real-time visibility into whether your agents are responsive and functional. Track request volume, latency distributions, error rates, and availability. Set SLOs for response time and accuracy, then measure against them continuously.


### Managing service quality and cost


You balance quality against cost on every request. Observability lets you identify which queries consume the most tokens, which models deliver the best quality-to-cost ratio, and where you can route to cheaper models without sacrificing output quality.


Token-level telemetry is critical here. If a particular tool returns verbose results that inflate your context window, you can add compression or filtering at that step to reduce downstream token consumption.


### Enabling end-to-end tracing and debugging


When a user reports a bad output, you need to reconstruct exactly what happened. End-to-end tracing lets you follow the request from initial input through every tool call, LLM invocation, and post-processing step to the final output.


This capability turns debugging from a guessing game into a systematic investigation. You open the trace, identify the span where the error occurred, inspect the inputs and outputs, and know exactly what to fix.


### Example: observability in action for a multi-step agent


Consider a customer support agent that handles refund requests. It receives a user message, classifies the intent, queries an order database, checks refund eligibility rules, drafts a response, and sends it. Each step is a span in the trace.


If the agent incorrectly denies a valid refund, you open the trace and walk through the spans. You might discover that the order database tool returned stale data, or that the eligibility-check prompt did not account for a recent policy change. Without the trace, you would only know the output was wrong, not why.


## Challenges in implementing AI agent observability


Every team building an observability practice hits real obstacles. Knowing them upfront helps you plan around them.


### Non-determinism and emergent agent behavior


Your agents produce different outputs for the same input across runs. This makes it harder to define expected behavior, write regression tests, and distinguish genuine failures from acceptable variation.


Evals with rubric-based scoring (rather than exact-match assertions) help, but they introduce their own subjectivity. Emergent behavior is even harder to predict. An agent with access to many tools might discover unexpected tool-call sequences that work but were never anticipated during development.


### High cardinality of tool calls and context windows


Your agents can call dozens of tools across a single execution, each with unique parameters and return values. Storing and querying this high-cardinality data at scale requires thoughtful schema design and storage choices.


Context windows compound the problem. As conversations grow longer, the volume of telemetry per trace increases. You need retention policies that balance debuggability against storage cost.


### Tooling fragmentation across model providers


You likely use multiple model providers, each with different API formats, error codes, and telemetry conventions. Normalizing this data into a consistent format requires an abstraction layer. OpenTelemetry and its semantic conventions help on the tracing side, but you still need to standardize model-specific metadata like token counts and finish reasons.


## Mastra's built-in observability for TypeScript agents


If you're building agents in TypeScript,[Mastra](https://mastra.ai/) ships observability out of the box through[Mastra](https://mastra.ai/) Studio. The decision to make it a core primitive, not an optional integration, came directly from user feedback. When Mastra analyzed support calls and user interviews ranked by number of accounts affected, the number one complaint was observability: no clean cost or token visibility, no way to tell which agent step was burning budget, no per-customer cost breakdown.


Teams described their existing setups in pointed terms: Langfuse was "a mess," OTel combined with Sentry was "duct tape," and neither gave them per-customer cost data. Companies including ControlUp, Cobli, WKS, Plancraft, Palette, and Cedar all raised this as a blocker. Mastra's response was to ship Studio observability with per-trace customer IDs and cost tracking as first-class features. Every agent run, workflow step, tool call, and model interaction now produces a span automatically, and when each span ends Mastra extracts duration, token counts, and cost estimates without any instrumentation code on your part.


Logs correlate to traces automatically: every logger call within a traced context is tagged with the current trace and span IDs, so you can navigate from a log entry to the exact trace that produced it. In production, traces export to any OTel-compatible backend by adding an exporter to your observability config. The built-in evals framework runs scorers asynchronously alongside agents, integrates into CI/CD pipelines so a prompt change that degrades accuracy fails the build before reaching production, and covers hundreds of models across dozens of providers through one routing interface.


[Build your first observable TypeScript agent on Mastra.](https://mastra.ai/)


## Context engineering and its role in agent observability


Your agent's performance depends heavily on what goes into its context window. Context engineering, the practice of assembling, pruning, and versioning the information an agent sees before each LLM call, has a direct relationship with observability.


### Why prompt and context changes must be versioned and traced


Treat prompts and context-assembly logic like code: version-controlled and tied to specific agent runs. When an agent's output quality changes, you need to determine whether the cause was a prompt edit, a change in retrieved context, a model update, or something else entirely.


If your traces capture the exact prompt and context sent to the model on every call, you can diff across runs and isolate the variable that caused a regression. Without this, you are debugging blind.


Context failures tend to fall into distinct patterns. Poisoning happens when errors persist in context and get referenced repeatedly across turns. Distraction occurs when the model receives so much context that it ignores its training. Confusion shows up when irrelevant context degrades output quality, while clash emerges from conflicting information within the same window.


Finally, rot describes the quality degradation that appears beyond roughly 100k tokens, a taxonomy explored in depth in[Principles of Building AI Agents](https://mastra.ai/books/principles-of-building-ai-agents) . Each failure mode produces a different observability signature that you can learn to recognize in your traces.


### Linking context inputs to evaluation outcomes


Your evals become far more actionable when you can correlate them with the specific context the agent received. If an eval score drops, you want to know whether the agent saw the right information. Did the retrieval step return relevant documents? Was the context window near capacity? Did a previous tool call inject noisy data?


By linking eval results to context metadata in your traces, you build a feedback loop. Low eval scores on traces with large context windows might indicate context rot. Low scores on traces with sparse context might indicate missing information. Each pattern points to a different fix.


## How to get started with AI agent observability


You do not need a dedicated observability platform on day one. Start with what your framework provides, add structure incrementally, and invest in tooling as your deployment scales.


### Instrumentation approach for existing agent code


The fastest way to get traces into production is to use a framework that instruments everything automatically. In Mastra, observability is configured once in your project setup and then every agent run, tool call, and workflow step generates spans, correlated logs, and cost metrics without any additional code.


If you're working outside Mastra, or want to understand what Mastra is doing under the hood, the comparison below shows the manual OpenTelemetry equivalent. Both approaches produce OTel-compatible traces; the difference is the amount of boilerplate you own.


**Mastra (configure once, instrument automatically)**


```text
import   {   Mastra   }   from   "  @mastra/core/mastra  "  ;
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {
Observability,
MastraStorageExporter,
MastraPlatformExporter,
SensitiveDataFilter,
}   from   "  @mastra/observability  "  ;
import   {   LibSQLStore   }   from   "  @mastra/libsql  "  ;
import   {   DuckDBStore   }   from   "  @mastra/duckdb  "  ;
import   {   MastraCompositeStore   }   from   "  @mastra/core/storage  "  ;
const   supportAgent   =   new   Agent  ({
id:   "  support-agent  "  ,
instructions:   "  Handle support requests.  "  ,
model:   "  openai/gpt-4o  "  ,
tools: { searchDatabase },
});
export   const   mastra   =   new   Mastra  ({
agents: { supportAgent },
storage:   new   MastraCompositeStore  ({
id:   "  composite-storage  "  ,
default:   new   LibSQLStore  ({
url:   "  file:./mastra.db  "  ,
}),
domains: {
observability:   await   new   DuckDBStore  ().  getStore  (  "  observability  "  ),
},
}),
observability:   new   Observability  ({
configs: {
default: {
serviceName:   "  my-agent-app  "  ,
exporters: [  new   MastraStorageExporter  (),   new   MastraPlatformExporter  ()],
spanOutputProcessors: [  new   SensitiveDataFilter  ()],
},
},
}),
});
// Done. Every LLM call, tool invocation,
// and workflow step is traced. Token counts
// and cost are extracted per span. Logs
// are correlated to traces automatically.
```


**Manual OTel (boilerplate you own per call)**


```text
import   {   trace   }   from   "  @opentelemetry/api  "  ;
const   tracer   =   trace.  getTracer  (  "  agent-observability  "  );
async   function   runAgent  (input  :   string  )   {
return   tracer.  startActiveSpan  (  "  agent.run  "  ,   async   (span)   =>   {
span.  setAttribute  (  "  agent.input  "  ,   input);
const   toolResult   =   await   tracer.  startActiveSpan  (
"  tool.searchDatabase  "  ,
async   (toolSpan)   =>   {
const   result   =   await   searchDatabase  (input);
toolSpan.  setAttribute  (  "  tool.result_count  "  ,   result.  length  );
toolSpan.  end  ();
return   result;
},
);
const   response   =   await   tracer.  startActiveSpan  (
"  llm.generate  "  ,
async   (llmSpan)   =>   {
const   completion   =   await   generateResponse  (input,   toolResult);
llmSpan.  setAttribute  (  "  llm.model  "  ,   "  gpt-4o  "  );
llmSpan.  setAttribute  (
"  llm.tokens.input  "  ,
completion.usage.prompt_tokens,
);
llmSpan.  end  ();
return   completion;
},
);
span.  end  ();
return   response;
});
}
```


The manual approach requires you to wrap every LLM call and tool invocation in spans, set token attributes by hand, and repeat this pattern across your entire codebase. With Mastra, you configure observability once at the framework level and all of that instrumentation happens automatically across every agent, tool, and workflow step. You can still export to any OTel-compatible backend by adding an exporter to the exporters array.


### Choosing the right agent observability tools


Your stack needs three components: a trace backend that supports high-cardinality data, a dashboard layer for metrics and alerts, and an eval runner that can score outputs at deploy time and in production. The market for AI agent observability tools includes both fully managed platforms and open-source self-hosted stacks.


The key selection criteria are:


**Criterion** **Why it matters** **What to look for**


OTel compatibility Avoids vendor lock-in and lets you swap backends without re-instrumenting Native OTLP ingestion, no proprietary SDKs required


AI-specific attributes Standard APM backends lack fields for token counts, model IDs, and prompt metadata First-class support for GenAI semantic conventions


Eval integration Attaching eval scores to traces lets you query quality alongside latency and cost Built-in eval scoring or API hooks for external eval runners


Cost at scale Agent traces are large and frequent, so storage pricing matters Transparent per-GB or per-span pricing; configurable retention policies


### Building an iterative eval and monitoring loop


Your observability practice is not a one-time setup. It is an iterative loop that tightens over time.


1.


**Instrument your agents** and collect traces from production traffic


2.


**Define your initial eval rubrics** and run them against sampled production data


3.


**Classify failures by type** and cross-reference them against business metrics


4.


**Prioritize the failure modes** that impact your north-star metric most


5.


**Experiment with fixes** (prompt changes, model swaps, tool redesigns) using your eval dataset


6.


**Validate improvements** against past production data before deploying


7.


**Repeat** , expanding your eval dataset with new production examples each cycle


The faster you move through this loop, the more reliable your agents get. Each cycle sharpens your evals, tightens your alerts, and gives you higher confidence in every deployment.


## Wrapping up


AI agent observability closes the gap between "the agent responded" and "the agent responded correctly." When you instrument traces, track token-level metrics, and run evals against live traffic, you move from reactive firefighting to systematic improvement. If you're building in TypeScript,[Mastra Studio](https://mastra.ai/studio) gives you traces, correlated logs, and per-trace cost data out of the box, so you can start with real visibility on day one rather than assembling a monitoring stack from scratch.
