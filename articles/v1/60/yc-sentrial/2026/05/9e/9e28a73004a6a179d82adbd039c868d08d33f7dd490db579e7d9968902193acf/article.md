---
schema_version: "1.0.0"
document_id: "9e28a73004a6a179d82adbd039c868d08d33f7dd490db579e7d9968902193acf"
company_key: "yc-sentrial"
company: "Sentrial"
source_id: "yc-sentrial-news-import-e72e91747750"
canonical_url: "https://www.sentrial.com/blog/best-practices-for-agentic-ai-observability-in-production"
published_at: "2026-05-31T17:37:47.669+00:00"
first_seen_at: "2026-07-25T23:34:43.014509+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:e8bc6b89074960838e58c1ca6d2ddf0d059f0b7ff6b5e0a0c28d1be98ec33922"
---

# Your Agent Looks Fine. Here's Why That's the Problem.

Before Sentrial existed, we kept hitting the same thing: an agent would be running fine by every metric anyone was watching, and users would be quietly getting wrong answers. No exceptions, no error codes, response times totally normal. That gap between "the system looks healthy" and "users are getting correct answers" is what agentic AI observability is actually about, and closing it is harder than most teams expect.


## Most Teams Aren't Watching the Right Things


Traditional observability has one core question: did the service stay up? Agentic observability asks something different entirely, did the agent reach the right goal, through the right steps, at an acceptable cost? Those aren't refinements of the same question. They need different instrumentation, different evaluation methods, and different alerting logic.


Why this matters: agents make sequential decisions, call external tools, and accumulate errors silently across steps in ways that never produce an HTTP 500. When tool calls and branching behavior vary across runs, just capturing a trace and checking the end state stops being enough. As[Arize notes](https://arize.com/blog/best-ai-observability-tools-for-autonomous-agents-in-2026/) , agentic systems fail in ways that look like success, incorrect but well-formed outputs, unnecessary tool calls, actions that are syntactically valid but semantically wrong.[Coralogix makes the same point](https://coralogix.com/ai-blog/agentic-ai-observability/) more directly: a correct agent run and an incorrect one can produce traces that look identical, and traditional APM cannot tell them apart on the dashboards your on-call engineer is watching.


The absence of a crash is not evidence of correct behavior. That's the whole argument for why agentic observability is its own discipline, not a feature you bolt onto your existing stack.


For more on this, our article[Most AI Agent Failures Never Trigger an Alert. Here's Why.](https://www.sentrial.com/blog/llm-observability-silent-failures-nobody-warns-you-about) covers the full stack context.


## Legacy Monitoring Is Blind to What Agents Actually Do


Traditional APM tools, Datadog, New Relic, and their peers, instrument infrastructure. They track CPU, memory, request latency, and error rates. An agent run that returns a hallucinated answer with a 200 OK response is completely invisible to those tools. The[Arize field analysis](https://arize.com/blog/common-ai-agent-failures/) describes it clearly: your existing observability stack looks at agent actions and reports "Success" because the HTTP status code was 200, even as agents hallucinate parameters, burn cash in invisible loops, or execute valid operations that produce wrong results.


From our own analysis across 12 million logs, around 22% of issues were explicit tool call failures, something that actually made the agent stop. The remaining 78% were silent: hallucinations as the top category, user frustration second, agent forgetfulness or laziness third. The majority of agent failures don't stop the run. The user just gets a wrong or unhelpful answer and leaves.


This costs real money. One of the most costly silent failures we encountered involved a Series B finance startup using agents to automate account receivables. Their agent ingested vendor PDFs, extracted data, and generated quotes. In the first two weeks after launch, everything looked fine, different PDFs produced different quotes at roughly the right price points. What was actually happening: the agent had broken PDF ingestion and was hallucinating quote prices based on RFP context and whatever other data was available, not the actual document contents. The run succeeded end-to-end from every surface-level metric. Legacy monitoring gave the team zero signal.


[InsightFinder's analysis](https://insightfinder.com/blog/why-llms-hallucinate-detection/) captures the mechanism well: failures in retrieval and reasoning systems are often silent. Latency stays acceptable, error rates stay low, and the system appears healthy while producing wrong outputs. This is the failure category that legacy monitoring structurally cannot catch, and it's where most production agent failures actually live.


Our article[Your Dashboards Are Green and Your AI Is Still Failing Users](https://www.sentrial.com/blog/ai-for-observability-your-agent-isnt-crashing-its-lying) covers the taxonomy of silent agent failures in depth.


## Here Are the Four Signal Types You Actually Need


Adapting the MELT framework to agents produces four signal categories that teams need to collect and correlate.


**Traces and spans** cover the full session journey from start to end, with each decision step represented as a span. For multi-agent workflows, every agent in the chain needs to share a single execution ID so that debugging a failure doesn't require manually reconstructing which agent did what and when.


**Logs and events** capture LLM calls and tool invocations including inputs, outputs, latency per call, and failure codes. This is where the agent-specific fields that generic OpenTelemetry instrumentation often misses live: intermediate state transitions, tool call timing, and full result payloads, not just whether the call succeeded.


**Infrastructure metrics** cover latency, token counts, and cost per step. Token usage and cost at step granularity matter for agents in a way they don't for traditional services: a runaway reasoning loop may complete successfully from a process perspective while consuming 50x the expected token budget.


**Quality and evaluation metrics** are the category most teams underinstrument. This includes hallucination rate, tool failure rate, goal completion rate, and safety flags. These can't be derived from the first three categories alone, they require evaluation logic running against the semantic content of agent outputs, not just their structural properties.


The[OpenTelemetry Semantic Conventions for Generative AI](https://opentelemetry.io/blog/2024/otel-generative-ai/) address the first three categories through traces, metrics, and events, and the emerging[agentic conventions on the OTel GitHub](https://github.com/open-telemetry/semantic-conventions/issues/2664) are starting to standardize attributes for tasks, actions, agents, teams, artifacts, and memory in complex AI workflows. OTel is the right instrumentation backbone, it provides the vendor-neutral wire format, and frameworks like LangGraph and LangChain emit OTel-compatible traces natively.


The fourth category, quality metrics, is where an evaluation layer has to sit on top of the telemetry layer. We use OpenTelemetry for initial logging, then run our own analysis on top to classify and cluster problems across runs. Capturing traces is necessary but not enough. The logged data has to drive classification of what actually went wrong, not just record that something happened.


One example of why this matters: a fintech company using agents for GL code processing couldn't track mismatched codes with start-state or end-state checks, because even the end states had hundreds of valid variations. When the GL code wasn't in the system at all, the agent might not output a product with the code, a valid-looking output that was semantically wrong. Standard telemetry didn't catch it. A custom classifier on the semantic content of intermediate steps did.


## Sampling Strategies Built for Infrastructure Will Burn You Here


This is the gap we see teams hit after they think they've solved instrumentation. Sampling strategies that work well for infrastructure monitoring are dangerous for agents, and the reason is statistical.


Rare but critical failure modes, goal abandonment on a specific intent pattern, jailbreak attempts, tool misuse on edge-case inputs, occur in the tail of the distribution. Any reasonable sampling rate will drop most of them. If you sample 10% of production runs and hallucinations occur in 2% of sessions, you're classifying roughly 0.2% of traffic. That's not enough volume to detect a regression, fire a reliable rate alert, or distinguish a new failure pattern from statistical noise.


[OpenObserve's analysis of sampling in tracing](https://openobserve.ai/blog/head-and-tail-based-sampling/) puts the math plainly: if errors represent 0.1% of traffic and your sampling rate is 5%, you're keeping only 0.005% of your error traces. For a high-volume agent system, that can mean roughly one error trace every several minutes, far too sparse for meaningful pattern detection.[Logz.io makes the complementary point](https://logz.io/learn/sampling-in-distributed-tracing-guide/) : rare events that matter most for incident flagging may not be sampled at all with arbitrary sampling decisions.


The operational consequence is that behavioral regressions surface days later when enough volume has accumulated to survive the sample, rather than immediately when the failure rate crosses a threshold. That's not a data preference issue, it's a reliability decision with a direct impact on how quickly teams can detect and respond to production failures.


Classifying every interaction means a regression that pushes hallucination rate from 2% to 4% is visible in hours, not days. Most teams expect to use an LLM-as-judge system or sample-based evals and classify a percentage of their logs. In practice, that means missing exactly the issues that matter most once an agent is in production at scale.


## The Full Loop: Trace to Fix, Without the Gaps


Every other explainer on agentic observability covers the instrumentation side and stops. The operational question, how collected telemetry turns into a detected failure, a fired alert, and a closed fix, gets almost no attention. This is where the work actually happens in production.


The path has four stages, and each depends on the previous one.


**Stage 1: Capture.** Instrument with OpenTelemetry or a framework-native integration (LangChain, LangGraph) to collect full session traces with consistent execution IDs across every step and every agent in the workflow. This is the foundation. Nothing downstream works without it.


**Stage 2: Evaluate.** Run automated evaluations on every interaction, not a sample. Built-in classifiers cover common failure modes: hallucinations, bad tool calls, agent forgetfulness, jailbreak attempts. Custom classifiers cover domain-specific failures that no off-the-shelf classifier anticipates. The key is that custom classifier creation shouldn't require a machine learning project. Checking three or four example logs to instantiate a fine-tuned classifier in under a minute is the right interaction model, otherwise teams will only monitor what the tool vendor predefined, which won't cover the failure modes specific to their agent's domain.


**Stage 3: Alert.** Fire real-time alerts on error spikes and behavioral anomalies with enough context to act on immediately, failure classification, contributing step, agent version, and a direct link to the failing session. Latency dashboards are not alerting. An alert that tells you the hallucination rate spiked 3x in the last hour on your account resolution agent, with the specific session that triggered the spike, is alerting. According to[Dynatrace's 2026 agentic AI report](https://dynatrace.com/news/blog/agentic-ai-report-new-observability-strategy/) , 44% of organizations still rely on manual methods to monitor agent interactions, which makes this the most commonly skipped stage and the most consequential to get right.


**Stage 4: Debug.** Replay or fork from any intermediate step in the failing run. Scrolling through raw logs to reconstruct what the agent decided at step seven of a twelve-step session isn't debugging, it's archaeology.[Braintrust's analysis of agent debugging](https://braintrust.dev/articles/best-ai-agent-debugging-tools-2026) describes the right model: load the failing trace and re-run against the exact production inputs, tool calls, intermediate steps, and model configuration that triggered the failure. A[replayable run](https://medium.com/@ThinkingLoop/replayable-agent-runs-the-debugging-trick-that-ships-f5460ebf390a) lets you re-execute step-by-step using the same inputs, tool responses, and intermediate state, so you can reproduce what happened rather than infer it.


Evaluations without traces have no context for why a failure happened. Alerts without evaluations are latency dashboards with no semantic signal. Debugging without replay requires manually reconstructing agent state from logs, which doesn't scale beyond a few sessions per week.


We built Sentrial to cover all four stages in one platform rather than requiring teams to instrument with one tool, run evals in a separate use, configure custom alerting webhooks, and maintain a separate replay environment. The finance company with the broken PDF ingestion we described earlier, that failure went undetected because their team couldn't monitor, scan, and run remediation across millions of logs a month. The volume alone makes manual inspection impossible, which is why the loop from trace to fix has to be automated end-to-end.


## Passing Evals Is Not the Same Thing as Having Observability


most teams treat agentic observability as an evaluation problem. They write evals in staging, run the benchmark, hit a passing score, and ship to production assuming the signal carries over. It doesn't.


Evals on a fixed test set measure a snapshot of performance against a distribution you designed. Production observability measures a stream of inputs you didn't design, from users with intents your test set never represented. A model that scores 94% on your eval suite can simultaneously be hallucinating on 8% of production sessions, and you won't know until a user complains, or until you're monitoring production directly.[LayerLens captures this precisely](https://layerlens.ai/blog/llm-evaluation-framework-for-production) : static benchmark scores don't predict production reliability under distribution shift.


The finance startup with the broken PDF ingestion is a good example and the PDF ingestion failure. The agent produced outputs that looked fine. They didn't have production monitoring on every step because, and this is the real constraint, they couldn't do that manually for millions of logs a month. Their evals weren't wrong; they were just measuring the wrong distribution. Without production-level monitoring, that failure would not have been caught, as they said themselves, "for a century."


[Braintrust's framing](https://braintrust.dev/articles/what-is-llm-monitoring) is useful here: online evaluation catches issues that offline testing can't anticipate, including novel user queries, distribution shifts, and gradual model drift. Agent workflows multiply monitoring complexity because a single user request can trigger chains of tool calls, and each chain represents a new failure surface that static evals don't cover.


Evals are a pre-flight check. Observability is the flight recorder. Teams need both, in sequence, covering different phases of the deployment lifecycle. Our article[Your Evals Are Passing While Your Agent Is Failing Users](https://www.sentrial.com/blog/ai-evals-what-they-are-and-what-they-miss) covers what evals miss without the observability framing.


Most prompt testing compounds this problem. Teams run A/B tests that look clean on their eval metrics but don't measure what actually changes in production, frustration rates, refusal rates, accuracy on the long tail of user intents. Observability has to extend into prompt experimentation as well, not just baseline monitoring. Our article on[Prompt A/B Testing That Actually Catches Silent Production Failures](https://www.sentrial.com/blog/prompt-ab-testing-that-catches-silent-failures) covers that extension specifically.


## How to Get Started Without Overcomplicating It


**Step 1: Instrument with consistent session IDs.** Use OpenTelemetry or a framework-native integration (LangChain, LangGraph) to capture session-level traces. The single most important data hygiene decision is ensuring every step in a multi-step run, and every agent in a multi-agent workflow, shares a consistent execution ID. Without it, debugging requires manually reconstructing execution chains from disconnected log entries.


**Step 2: Define your failure modes before you need them.** What counts as a hallucination for your specific agent? What is a tool failure? What does goal abandonment look like in your domain? These definitions need to exist before you start classifying production data, not after an incident forces the question.[Research on agent behavioral drift](https://arxiv.org/pdf/2601.04170) confirms that LLM-based agents can exhibit progressive deviation from design specifications without explicit parameter changes, catching that requires knowing what "on-spec" looks like.


**Step 3: Run evaluations on production data, not just your staging eval set.** The staging eval set tells you whether the agent works on inputs you anticipated. Production data tells you whether it works on inputs users actually send.


**Step 4: Alert on eval-derived metrics, not just latency.** Set thresholds on hallucination rate, tool failure rate, and goal completion rate. An alert that fires when hallucination rate crosses 3% is useful. An alert that fires when p99 latency exceeds 2 seconds tells you nothing about whether users got correct answers.


For teams who want to assemble this from open-source components: OpenTelemetry covers instrumentation, the OpenLLMetry and OpenInference semantic conventions extend OTel for GenAI spans, and tools like DeepEval or Arize Phoenix cover offline evaluations. The gap those components leave is the integration work, connecting instrumentation output to evaluation logic, evaluation output to alerting, and alerting to a debugging interface with replay capability. That integration is not trivial at production scale.


Sentrial covers the full loop in one platform. Instrumentation is five lines of code via OpenTelemetry, LangChain, or LangGraph. Built-in classifiers for hallucinations, bad tool calls, agent forgetfulness, and jailbreaking start working immediately. Custom classifiers for domain-specific failure modes, the mismatched GL codes, the broken PDF ingestion, whatever your agent's specific failure surface is, deploy from three or four example logs in under a minute. Real-time Slack alerts include failure classification and a direct link to the failing session. Replay from any intermediate step replaces log archaeology with reproducible debugging.


Once baseline observability is running, the next layer is prompt experimentation: measuring whether a prompt change actually improves production failure rates rather than just staging eval scores. Our article on[Prompt A/B Testing](https://www.sentrial.com/blog/prompt-ab-testing-that-catches-silent-failures) covers that layer in detail.


we're already seeing agents set up their own monitoring. API endpoint traffic at Sentrial now exceeds dashboard activity, and a meaningful portion of that is agents, using their own tooling to sign up, configure, and instrument Sentrial via MCP install. As multi-agent orchestration grows more complex and context windows extend further, retrofitting observability after a production incident becomes more expensive than instrumenting from the start. The right time to build this loop is before the first production failure, not after.


---


## FAQ


**What is agentic AI observability?**


Agentic AI observability is end-to-end visibility into what a multi-step AI agent did, whether it produced correct outputs, and exactly where it failed, spanning session-level traces, automated quality evaluations, and actionable alerts. It differs from traditional observability in that it monitors semantic correctness and goal completion, not just uptime and latency.


**What telemetry data does agentic AI observability collect?**


The four signal categories are: traces and spans covering every decision step in a session; logs and events capturing LLM calls and tool invocations with full input/output payloads; infrastructure metrics including latency, token counts, and cost per step; and quality evaluation metrics such as hallucination rate, tool failure rate, and goal completion rate. The fourth category is what generic OpenTelemetry instrumentation alone doesn't cover.


**How is agentic AI observability different from traditional observability?**


Traditional APM monitors infrastructure, CPU, memory, request latency, error codes. It can't detect a hallucinated answer that returns 200 OK, a goal abandonment that produces a polite apology, or token waste from a reasoning loop that completes without error. From our analysis of 12 million logs, 78% of agent failures are silent, no exception, no error code, just a wrong or unhelpful answer that the user receives without the system registering a failure.


**What is OpenTelemetry's role in agentic observability?**


OpenTelemetry provides the vendor-neutral instrumentation layer and wire format for capturing agent traces. The[OTel Semantic Conventions for Generative AI](https://opentelemetry.io/blog/2024/otel-generative-ai/) standardize how LLM calls, tool invocations, and agent spans are represented, and emerging[agentic conventions](https://github.com/open-telemetry/semantic-conventions/issues/2664) extend those standards to multi-agent workflows. Frameworks like LangChain and LangGraph emit OTel-compatible traces natively. OTel covers the instrumentation and trace capture layer; evaluation, alerting, and debugging logic run on top of it.


**What should teams monitor for agent quality?**


The core quality metrics are hallucination rate, tool failure rate, goal completion rate, agent forgetfulness (losing earlier context in long sessions), and safety flags for jailbreak attempts or out-of-scope outputs. Beyond those built-in categories, teams should define custom failure modes specific to their domain, these are the failure patterns no generic classifier will catch, and they tend to be the most expensive ones in production. Defining them before a production incident, rather than after, is the single highest-leverage step a team can take when setting up agentic observability.
