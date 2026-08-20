---
schema_version: "1.0.0"
document_id: "f7206ecde1ab2e5f8507b6b2d6b6890e59956d5aa5c462cf973465257f6f3546"
company_key: "yc-laminar"
company: "Laminar"
source_id: "yc-laminar-news-import-4ade4fae8778"
canonical_url: "https://laminar.sh/article/agent-observability"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-22T01:53:16.367078+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:6cda8e7c18f6be4a071b3b03b157a37ee7e632ffb2eb45bf243a2539e1d81db2"
---

# Agent Observability: Tracing, Debugging, and Improving AI Agents

An AI agent that worked fine in dev can fail in production for reasons you will never see from logs. The model chose the wrong tool. A sub-agent got a malformed input and retried four times. A retrieval step returned the wrong chunk and nothing downstream noticed. Agent observability is how you see that.


This article explains what agent observability actually means, what to instrument, how threads and multi-turn runs change the problem, how evaluations close the loop, and how[Laminar](https://laminar.sh/) covers the whole thing as an open-source, OpenTelemetry-native platform.


## What is agent observability?


#


Agent observability is step-by-step visibility into an agent's execution: every LLM call, every tool invocation, every retrieval step, every sub-agent handoff, and the reasoning the agent used to move between them. It's the layer that turns *"the agent failed"* into *"the agent called search_docs with an empty query because the planner's JSON output was truncated on turn four."*


Traditional software observability (logs, metrics, traces) is built around deterministic code paths. Identical inputs produce identical outputs, so a stack trace and a log line are usually enough to locate a bug. Agents break that assumption. Two runs with the same prompt can take different paths, call different tools, and produce different answers. The failure mode is often *drift* rather than *exception* , and drift doesn't show up in a log file.


Three things follow:


- You need structured traces, not text logs. A trace has parent-child relationships between spans, so you can see which LLM call triggered which tool call, and which sub-agent made which decision.
- You need the full input and output of every LLM and tool span preserved verbatim. Summarised or sampled payloads throw away the evidence you need to debug.
- You need the trace plus the cross-trace layer. A single trace tells you what one run did. Across ten thousand runs, the interesting questions are about rates and patterns, and no amount of single-trace inspection answers them.


## When you need agent observability


#


You need it the moment your agent is non-trivial. Non-trivial means any of:


- **Multi-step** : the agent makes more than one LLM call per user turn, or calls tools in sequence.
- **Multi-turn** : the agent carries state across turns, and a failure on turn six depends on context from turn one.
- **Multi-agent** : a planner delegates to sub-agents or specialist tools, and each sub-agent has its own LLM calls.
- **Long-running** : an agent run takes minutes, not seconds, and manually replaying a failure costs real time.


You also need it the moment you have users. The first time a customer tells you *"the agent gave me a weird answer yesterday around 3pm"* , you either have the trace for that run or you don't. There is no middle ground.


### When a single log line is enough


#


Not every LLM call needs full tracing. A one-shot classifier that takes a string, calls a model once, and returns a label is fine with an input/output log. The cost of adding observability to that is higher than the cost of debugging it with print


.


Draw the line at the point where a single call becomes a sequence. Once the run has more than two or three steps, you want spans.


## What to instrument


#


For an agent to be debuggable after the fact, five things need to end up in a trace.


**LLM calls.** Model name, input messages (system, user, assistant), output messages, token counts, cost, latency, and the full request/response JSON. Laminar's SDKs capture this automatically for OpenAI, Anthropic, Google, Bedrock, Cohere, Mistral, Groq, and the major agent frameworks. See the[integrations overview](https://laminar.sh/docs/tracing/integrations/overview) .


**Tool calls.** Tool name, arguments, return value, errors, latency. In Laminar, any function wrapped with @observe(span_type="TOOL")


(Python) or observe({ spanType: "TOOL" }, fn)


(TypeScript) renders as a tool row in the transcript view. See[span types](https://laminar.sh/docs/tracing/structure/span-types) .


**Retrieval steps.** Query, retrieved documents, relevance scores. These are usually tool calls in disguise. Instrument them the same way.


**Sub-agent boundaries.** When a planner calls a specialist, the specialist's full run (its own LLM calls and tool calls) should nest under the tool span that invoked it. This is the difference between a trace that reads like a conversation and a trace that reads like a flat list of calls.


**Metadata and context.** User ID, session ID, run ID, any feature flag or config value that changes behavior. These are the fields you'll filter on when you're debugging a specific cohort of runs. See[metadata](https://laminar.sh/docs/tracing/structure/metadata) .


### Auto-instrumentation vs manual spans


#


Most of the above is free once you call Laminar.initialize()


. The SDK patches the LLM and agent-framework libraries at import time, so every model call and every framework-level tool call lands as a span without further code changes.


You still add manual spans for two things: your own functions (business logic, custom retrievers, custom tools) and the top-level agent loop. Wrap them with @observe


. That gives you a named root span on every run, which is the anchor every other span in the trace hangs off.


## OpenTelemetry as the transport


#


Laminar is OpenTelemetry-native. Spans are OTLP spans, delivered over gRPC or HTTP. This has three concrete consequences.


**You are never locked in.** Every trace Laminar stores is a standard OpenTelemetry span. You can fan out to any OTel backend at the same time by configuring a second exporter. You can migrate away from Laminar without rewriting instrumentation.


**You can bring your existing instrumentation.** If you already have OpenLLMetry or any of the @opentelemetry/instrumentation-*


packages wired up, point them at Laminar's OTLP endpoint and the spans arrive in the UI without further configuration. See[OpenTelemetry in Laminar](https://laminar.sh/docs/tracing/otel) .


**You get the semconv payoff.** OpenTelemetry's GenAI semantic conventions define standard attribute names for LLM spans ( gen_ai.request.model


, gen_ai.usage.input_tokens


, gen_ai.prompt.*


, gen_ai.completion.*


). Laminar's SDKs emit them, and Laminar's UI and SQL schema read them. Any span that follows semconv, from any source, renders correctly.


## A span tree is not the answer. Transcript view is.


#


Here is the honest part. A long agent trace has 500 to 5,000 spans. Render those as a tree of rows and you have an outline, not a trace. Every row has equal visual weight: the outer agent span, its query span, the planning turn, every tool call, every nested sub-agent. You scroll the outline and try to reconstruct what the agent actually did.


Laminar's default trace view is not a tree. It's a[transcript](https://laminar.sh/docs/platform/viewing-traces) .


The transcript lays the run out top-to-bottom as a conversation. The agent's input is parsed out of the first system/user messages and rendered inline as an Input


block. Each LLM turn shows the assistant's message with a one-line preview. Each tool call shows the tool name and its first argument ( Read fizzbuzz.py


, search_docs("checkout flow")


). Each sub-agent is a single collapsible card with its own Input, Output preview, token badge, and duration. Click the card and it expands in place: the sub-agent's own LLM turns and tool calls appear inside it, without collapsing the rest of the trace.


Three things come out of this that a tree never gave you:


1. The first question a reader asks ( *what did we ask this agent to do?* ) has an answer on the first screen, not after click-hunting through an LLM span's messages\[0\]


.
2. Multi-agent runs stay readable. Ten sub-agents produce ten cards, not ten subtrees. You drill into the one you care about.
3. Inline previews let you scan a long trace without expanding anything. If the failure is on turn 47, the preview on row 47 often tells you.


Tree view is still one click away in the view-mode dropdown. You use it when you need to confirm parent/child nesting, inspect raw span attributes, or debug a custom integration.


## Threads, sessions, and why a single trace is not enough


#


Agents get interesting across turns. A user asks a question, the agent answers, the user follows up, and the failure is on the fourth follow-up because the agent lost track of state between turns two and three.


Laminar groups traces by[session ID](https://laminar.sh/docs/tracing/structure/sessions) . Every trace in a session shares the same session_id


metadata field. The sessions view renders them as a timeline of numbered trace cards (1/5, 2/5, ...) with auto-extracted Input and last-LLM-span Output previews on each card. You can read a conversation end-to-end without opening each trace individually, then drill into any single trace to see the full span detail.


For agents that accept multiple user messages on the same run (instead of one trace per turn), you can also continue an existing trace rather than starting a new one: see[continuing traces](https://laminar.sh/docs/tracing/structure/continuing-traces) .


The failure pattern this catches:


- **Context bleed.** A tool call on turn two returned a stale value and the agent cached it across turns.
- **Memory drift.** The planner's state summary dropped a constraint from turn one.
- **Cumulative failure.** Each turn is individually fine; the combination is wrong. You only see this by reading the session, not a single trace.


## The cross-trace layer: Signals


#


A trace answers *what happened on this run* . Across ten thousand runs, the questions are different: *how often did the agent ask the user for clarification? how many runs hit the retry cap? what fraction of sessions ended with the user giving up?*


You cannot answer these by reading traces. You could try to answer them by tagging every span with every interesting outcome, but that means deciding up front every question you'll ever want to ask, and re-deploying every time the question changes.


[Signals](https://laminar.sh/docs/signals/introduction) are Laminar's answer. A Signal is an instruction, written in plain language, paired with a JSON schema for the structured output. Laminar runs it against every matching trace and produces a stream of signal_events


: one row per match, linked back to the source trace, carrying the structured payload.


A worked example. You write a Signal:


> Detect when the agent asked the user for clarification. Extract: was_clarifying
>
>
> (bool), reason
>
>
> (string: what the agent was unclear about), resolved
>
>
> (bool: did the user provide what was needed).


Signals run in two modes, independently:


- **Triggers** run on new traces as they arrive. You use triggers for anything you want in real time: live error rates, live success counts, anything feeding[Alerts](https://laminar.sh/docs/signals/alerts) .
- **Jobs** run across a historical window. You use jobs to backfill a new Signal against last week's traffic, re-run a changed prompt, or investigate a specific slice of traces.


Both modes produce the same kind of event, queryable from the[signal_events table](https://laminar.sh/docs/platform/sql-editor) in SQL. Similar events get auto-grouped into[clusters](https://laminar.sh/docs/signals/clusters) so you can see the top ten failure modes at a glance.


Signals replace the "tag every span you might want to query" pattern. You write the question once, the data comes back for the last week and for every future run.


## Querying across traces with SQL


#


Past a certain volume of runs, you need SQL. Laminar stores spans, traces, and signal events in ClickHouse and exposes them as read-only SQL tables in the[SQL editor](https://laminar.sh/docs/platform/sql-editor) and as an API endpoint at /v1/sql/query


.


A few queries the editor makes cheap:


**Which model is costing you the most this week:**


```text
SELECT   model  ,     sum  (  total_cost  )     AS   cost  ,     count  (  *  )     AS   calls
FROM   spans
WHERE   span_type   =     'LLM'     AND   start_time   >     now  (  )     -     INTERVAL     7     DAY
GROUP     BY   model
ORDER     BY   cost   DESC


```


**Error rate by span name over the last day:**


```text
SELECT   name  ,
countIf  (  status     =     'error'  )     AS     errors  ,
count  (  *  )     AS   total  ,
round  (  errors     /   total   *     100  ,     2  )     AS   error_rate
FROM   spans
WHERE   start_time   >     now  (  )     -     INTERVAL     1     DAY
GROUP     BY   name
HAVING   total   >     10
ORDER     BY   error_rate   DESC


```


**Sessions where a specific Signal fired more than three times (customer friction):**


```text
SELECT   trace_id  ,     count  (  *  )     AS   clarifications
FROM   signal_events
WHERE   signal_id   =     '...'     AND     timestamp     >     now  (  )     -     INTERVAL     7     DAY
GROUP     BY   trace_id
HAVING   clarifications   >     3


```


Export any result to a dataset and it becomes an evaluation set. That's the production-to-eval loop.


## From traces to evaluations: the improvement loop


#


Tracing tells you what broke. Evaluations tell you whether your fix makes things better or worse in aggregate.


[Laminar evaluations](https://laminar.sh/docs/evaluations/introduction) are the offline testing layer. You define a list of datapoints (input, optional target, optional metadata), an executor (the function being tested), and one or more evaluators (functions that score the output). Laminar runs them in parallel, traces every call, stores the scores, and compares runs against each other by group.


The production-to-eval loop, in practice:


1. A Signal fires on a class of failure in prod (for example, "agent looped on the same tool without progress").
2. You open the clustered events, pick the representative traces, and export them to a[dataset](https://laminar.sh/docs/datasets/introduction) from the SQL editor or the trace list.
3. You write an evaluation that runs your current agent against that dataset and scores whatever the failure mode was.
4. You run the evaluation once as a baseline, change the prompt or model or tool, run it again, and[compare the two runs side by side](https://laminar.sh/docs/evaluations/comparing-runs) .
5. The fix either improves the score or it doesn't. If it does, ship it. If it doesn't, the trace on the regressed datapoint tells you why.


Evaluations run from code (Python or TypeScript) or from the CLI ( lmnr eval


, npx lmnr eval


). The CLI picks up every matching file in an evals/


directory so the whole suite runs in CI against every pull request.


## The debugger: rerun from any point in a trace


#


When the failure is in a long-running agent, the slow part of debugging is reproducing it. An agent that takes four minutes to reach the broken step burns four minutes every time you want to test a fix. Change the prompt, rerun, wait four minutes, observe, repeat.


Laminar's[debugger](https://laminar.sh/docs/platform/debugger) attacks this directly. You mark an entrypoint with rolloutEntrypoint: true


, start a session with npx lmnr-cli dev path/to/entry.ts


, and you can now:


- Set a checkpoint on any span in an existing trace and rerun the agent from that point. Earlier steps are replayed from cache, so the rerun starts from the checkpoint state.
- Override the system prompt in the UI and rerun without redeploying.
- Inspect the new trace in the same page as the old one.


The turnaround on a prompt tweak drops from "minutes per iteration" to "as long as the remaining steps take."


## Browser-agent observability


#


Agents that drive a browser (Browser Use, Stagehand, Playwright-based agents) add a layer most tracing backends don't handle: the DOM state at each step. Laminar captures browser sessions alongside the trace. The trace carries the LLM calls and tool calls as usual; the session recording plays back the actual browser window, synced to the trace timeline via a playhead. See[browser agent observability](https://laminar.sh/docs/tracing/browser-agent-observability) .


When a browser agent clicks the wrong button, you need both views: the trace tells you which LLM output generated the click decision, the recording tells you what the page looked like when the decision was made.


## Self-hosting vs cloud


#


Laminar is[open source](https://github.com/lmnr-ai/lmnr) (Apache 2.0). The same binary runs in Laminar Cloud and in a self-hosted Helm deployment. Self-hosting matters when your traces carry data that can't leave your network: PII, regulated healthcare data, financial records.


The self-hosted chart supports a hybrid data plane (data stays in your VPC; control plane can be cloud-hosted), full air-gap deployments, and standard OTLP ingestion. See[hosting options](https://laminar.sh/docs/hosting-options) .


## Getting started


#


If you're using a supported framework (Vercel AI SDK, Claude Agent SDK, OpenAI Agents SDK, LangGraph, Pydantic AI, Mastra, Browser Use, Stagehand, or a raw OpenAI/Anthropic client), tracing is two lines:


```text
from   lmnr   import   Laminar  ,   observe


Laminar  .  initialize  (  )


@observe  (  )
async     def     run_agent  (  prompt  :     str  )  :
# your agent code
.  .  .


```


```text
import     {   Laminar  ,   observe   }     from     "@lmnr-ai/lmnr"  ;


Laminar  .  initialize  (  )  ;


export     const   runAgent   =     observe  (  {   name  :     "runAgent"     }  ,     async     (  prompt  )     =>     {
// your agent code
}  )  ;


```


Everything else (transcript view, SQL access, Signals, evaluations, the debugger) reads from those spans.


## What agent observability buys you, in one sentence


#


The short version: agent observability turns *"the agent is broken, we don't know why"* into *"the agent is broken on this class of input, the fix is in this step, the eval confirms the fix."* That's the loop Laminar is built for, end to end.


Next:[read a trace in transcript view](https://laminar.sh/docs/platform/viewing-traces) ,[write your first Signal](https://laminar.sh/docs/signals/quickstart) , and[wire up evaluations in CI](https://laminar.sh/docs/evaluations/quickstart) .
