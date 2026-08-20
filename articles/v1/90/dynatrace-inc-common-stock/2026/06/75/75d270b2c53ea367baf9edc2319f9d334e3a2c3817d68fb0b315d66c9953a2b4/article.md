---
schema_version: "1.0.0"
document_id: "75d270b2c53ea367baf9edc2319f9d334e3a2c3817d68fb0b315d66c9953a2b4"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/evaluate-llm-and-agent-quality-in-dynatrace-ai-observability/"
published_at: "2026-06-11T19:44:47+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:48af02c0aa96c6d41115a55c132ce4b427db470016bdee835d789b57d685e64e"
---

# Evaluate LLM and agent quality in Dynatrace AI Observability with dt-evals

AI applications fail in ways that differ from traditional software. They can return responses quickly, with no errors, and still deliver answers that are inaccurate, ungrounded, unsafe, or unusable. That's why AI quality can't be treated as a side project.


For AI systems, reliability is defined by response quality, factual grounding, data security, and usability — and those signals need to live alongside the same observability data teams already trust to monitor performance and availability.


When evaluation scores are isolated in notebooks, spreadsheets, standalone tools, or CI logs, they’re hard to operationalize. By bringing AI quality metrics into[Dynatrace AI Observability](https://www.dynatrace.com/solutions/ai-observability/) —next to latency, cost, errors, traces, and user behavior—teams can connect poor responses and hallucinations directly to the prompts, models, retrieval contexts, tool calls, services, and traces that produced them.


## What is dt-evals?


[dt-evals](https://github.com/dynatrace-oss/dt-evals/) is an open source CLI for evaluating LLM and agent quality from real GenAI traces, agentic interactions. Teams can run online evaluations against live or recent interactions, score outputs with an LLM judge, and send structured results back to Dynatrace AI Observability so quality becomes visible, queryable, trendable, and actionable.


A minor prompt edit, model change, or retrieval update to an AI application can improve one behavior while quietly breaking another. The challenge to tracking down where and why these systems break is that evaluation results are often maintained outside the operational workflow, making it difficult to connect a low score to the exact trace, prompt, model version, retrieval context, tool call, or service that produced the unwanted behavior.


Dynatrace AI Observability closes this loop. With[dt-evals](https://github.com/dynatrace-oss/dt-evals) and the AI Observability[Evaluation Preview](https://docs.dynatrace.com/docs/shortlink/preview-program#evaluation-and-llm-as-a-judge-support) teams can pull recent` gen_ai.*` spans, score real interactions with an LLM judge, and write structured evaluation results back as business events. These scores can be viewed with the originating trace, queried for custom analysis, trended in dashboards, and used to trigger alerts or workflow-driven remediation.


A failing faithfulness score is no longer just a number in a report. It’s now an operational signal.


## What are LLM evaluations?


An LLM evaluation system scores an AI response against a range of quality and safety dimensions. Common examples include whether the answer is relevant to the question, faithful to the provided context, free of hallucinations, safe for users, complete enough to be useful, and resistant to prompt-injection attempts.


LLM evaluations are typically applied in two modes:


**Offline evaluations** run before release against a fixed test set or curated trace dataset. These are used to compare a proposed prompt, model, retriever, or agent-tool change against a known baseline before shipping. For example,` replay 500 representative support questions in CI and block the release if faithfulness drops below the configured threshold.`


**Online evaluations** run after deployment against sampled production or user traffic. Use online evaluations to detect regressions caused by live inputs, changing retrieval results, tool behavior, traffic mix, or model drift. For example,` evaluate 10% of support-agent traces from the last hour and alert the team if hallucination failures exceed the configured window.`


With dt-evals, you can run evaluations from the command line, use them in CI/CD, or schedule them to detect quality regressions autonomously after deployment as a post-processing quality gate for your AI agents and LLM output.


Figure 1. AI Evaluation & Agentic App Performance dashboard showing dt-evals results in Dynatrace AI Observability


## Run evaluations from the command line


[dt-evals](https://github.com/dynatrace-oss/dt-evals) is an open source evaluation toolkit for teams that want to bring their own data, judge provider, and evaluation logic while keeping traces, scores, dashboards, and alerts connected.


Install the CLI:


` npm install -g @dynatrace-oss/dt-evals`


Or run it directly with npx:


` npx @dynatrace-oss/dt-evals <command>`


A typical first run has three steps:


1. Configure your environment and judge provider (Bring Your Own AI API key):


` dt-evals configure`


1. Verify your local setup and connection:


` dt-evals doctor`


1. Run evaluations on recent GenAI traces:


` dt-evals run --since 1h --sample 10`


This command evaluates traces from the last hour and samples 10% of them. In other words, dt-evals evaluates roughly one out of every ten matching traces, including the prompt and completion messages associated with each selected trace.


During configuration, you provide the connection to your Dynatrace environment and the credentials for the LLM judge provider you want to use. dt-evals does not require teams to send evaluations through a fixed provider. You bring your own judge credentials and control where evaluation execution happens.


For CI/CD use cases, run in CI mode:


` dt-evals run --since 6h –ci`


In CI mode, dt-evals emits machine-readable output and can fail the pipeline when a configured threshold is breached. This makes quality checks part of the same delivery process used for prompt changes, model upgrades, retrieval updates, and agent releases.


Video 1. dt-evals in action


## Bring your own LLM judge provider


The “LLM-as-judge” evaluation approach involves using an AI model to score another model or agent response. The LLM judge needs to come from an AI provider your team trusts and has approved for the type of data being evaluated.


dt-evals supports common LLM judge AI models and inference providers, including OpenAI, Anthropic, Google/Vertex/Gemini, AWS Bedrock, and Azure OpenAI. Depending on the package and configuration path you use. Teams provide their own credentials, choose the judge model, and can tune execution settings such as thresholds and concurrency.


This matters for both governance and cost control. Teams can decide which LLM provider is assigned to evaluate which traffic, how many judge calls run in parallel, and where evaluation results are stored.


## Which quality and safety dimensions are evaluated by dt-evals?


dt-evals supports built-in LLM-as-Judge evaluators for a range of quality and safety dimensions, including:


- **Relevance:** Does the response answer the user’s question?
- **Faithfulness:** Is the response supported by the provided context?
- **Hallucination:** Does the response invent facts that are not present in the available context?
- **Answer completeness:** Does the response fully address the user’s request?
- **Context relevance:** Is the retrieved or supplied context useful for answering the question?
- **Factual accuracy:** Does the response match an expected or known-correct answer?
- **Summarization quality:** Does the summary preserve the important information?
- **Conciseness:** Is the response direct with no unnecessary detail?
- **Fluency:** Is the response clear and readable?
- **Toxicity:** Does the response contain harmful or abusive content?
- **Bias:** Does the response show unfair or inappropriate bias?
- **PII leakage:** Does the response expose sensitive personal information?
- **Prompt injection:** Did the input or response show signs of instruction manipulation?
- **User frustration:** Does the interaction suggest the user is blocked or dissatisfied?
- **Drift:** Are scores changing meaningfully compared with prior behavior?


A Retrieval Augmented Generation (RAG) application might focus on faithfulness, hallucination, context relevance, and answer completeness. A customer-facing support agent might focus on relevance, fluency, bias, toxicity, and prompt-injection risk. An internal assistant might add custom checks for tone, policy compliance, or whether the answer includes required next steps.


### Add custom evaluations


Built-in metrics are useful, but most production AI systems also need checks that are specific to the business, domain, or workflow.


Custom evaluations let teams define their own judge prompts, scoring rules, labels, and thresholds. For example, a support team can create a custom evaluator that checks whether an answer includes a required troubleshooting step before recommending escalation. A financial services team can check whether responses include the required disclaimers. A platform team can check whether an agent uses the correct tool before answering.


The critical point is that custom evaluators run through the same pipeline as built-in evaluators. They can produce the same structured results, appear alongside other scores, and be used in dashboards, alerts, and release checks.


A typical configuration defines the target service, judge provider, sampling strategy, enabled metrics, and thresholds:


```text
schemaVersion: 1
name: support-agent-prod


dynatrace:
environmentUrl: https://your-env.apps.dynatrace.com
platformToken: dt0s16.xxxxx


judge:
provider: openai
model: gpt-5.5


scope:
service: support-agent
since: 1h
sampling:
strategy: random
percent: 10


metrics:
enabled:
- faithfulness
- hallucination
- relevance
- drift


alerts:
thresholds:
faithfulness: 0.7
relevance: 0.7


```


## Evaluation results in the AI Observability app


Evaluation results appear directly in the AI Observability app, so teams don’t have to jump between a trace view, an eval report, and a separate dashboard to understand what happened.


In the **Prompts** view, teams can filter for prompts with evaluation scores and inspect row-level verdicts. Score badges such as relevance, fluency, bias, faithfulness, or toxicity make response quality easy to scan without opening every trace.


This is useful when triaging a regression. Instead of starting with a generic failure count, teams can quickly see which prompts failed, which evaluator failed them, and whether the issue is isolated or widespread.


Figure 2: AI Observability App Prompts stream with evaluation results


From an individual prompt or trace, the Evaluations tab shows run-level details, including the evaluation name, score, provider, judge model, method, and supporting metadata.


That detail matters because a failed score is only useful if teams can explain it. Engineers and evaluation owners can move from a low score to the exact prompt, response, trace, model, evaluator, and rationale that produced it.


Figure 3: Prompt detail view with evaluation results and trace context in Dynatrace AI Observability


## Query, trend, and alert on evaluation scores


Because dt-evals writes results back as structured events, evaluation scores can be analyzed with the rest of your telemetry.


Teams can ask questions such as:


- Which evaluator has the lowest average score?
- Which services are producing the most failed evaluations?
- Did quality drop after a model or prompt change?
- Are hallucinations increasing over time?
- Is quality improving at the cost of latency or token usage?


For example, to get average score by evaluator you could write this query:


```text
fetch bizevents
| filter event.type == "gen_ai.evaluation.result"
| summarize avg_score = avg(gen_ai.evaluation.score.value),
by: { gen_ai.evaluation.name }
| sort avg_score asc


```


Figure 4: Querying failed evaluations by service and evaluator in Dynatrace AI Observability


Failed evaluations by service and metric can be determined with this query:


```text
fetch bizevents
| filter event.type == "gen_ai.evaluation.result"
| filter gen_ai.evaluation.score.label == "fail"
| summarize failures = count(),
by: { dt.service.name, gen_ai.evaluation.name }
| sort failures desc


```


Figure 5: Average evaluation scores by evaluator in Dynatrace AI Observability


Trending is where evaluation data becomes more useful than a point-in-time report. A single failed score can show an issue. A trend can show whether quality is drifting slowly, whether a release caused a sudden drop, or whether a fix actually improved behavior over time.


On the **AI Evaluation & LLM App Performance** dashboard, teams can track trends in quality score, pass rate, failed evaluations, drift detections, evaluator health, run cadence, and pass/fail volume over time.


Video 2: AI Evaluation & LLM App Performance dashboard


## How to turn quality regressions into alerts


Evaluation results can also drive alerts. For example, a support agent team may want to notify the AI team when hallucinations appear in production, or when faithfulness drops for more than a few minutes.


```text
name: support-agent-prod


alerts:
notifications:
- name: hallucination-detected
metric: hallucination
condition: count > 0
window: 5m
channel:
type: slack
connection: ai-observability-slack
channel: "#ai-alerts"


- name: faithfulness-regression
metric: faithfulness
condition: fail_rate > 10%
window: 15m
channel:
type: email
connection: ai-team-email
to: [ai-team@example.com]


```


Deploy the alerts with:


` dt-evals alerts list ./support-agent-prod.yaml`
` dt-evals alerts apply ./support-agent-prod.yaml`


Once applied, Dynatrace runs these checks continuously as Workflows. If hallucinations appear in the last five minutes, the team gets a Slack alert. If more than 10% of faithfulness checks fail over 15 minutes, the AI team receives an email. This turns LLM quality from something teams inspect manually into something Dynatrace can monitor and route automatically.


For continuous alerting, evaluation runs need to happen continuously or on a schedule. You can run dt-evals in CI for release checks[(see our example here)](https://github.com/dynatrace-oss/dt-evals/actions/workflows/evals-pydantic-ai-music-agent.yml) , run it manually during investigation, or deploy a scheduled runner for ongoing production evaluation. An alert is only as fresh as the evaluation results it carries.


Once configured, quality signals can be routed to the teams that need to act. If hallucinations appear in the last five minutes, the team can receive a Slack alert. If more than 10% of faithfulness checks fail over 15 minutes, the AI team can receive an email. This turns LLM quality from something teams inspect manually into something they can monitor and route automatically.


## Close the loop in the AI software delivery lifecycle


Evaluation gates are most useful when they meet developers where they already work. Because dt-evals writes evaluation results back into the observability data layer, those results are not limited to dashboards or post-release reviews. They can be queried, inspected, and acted on from development workflows, CI/CD pipelines, and agentic coding environments.


For example, a team can run dt-evals after any change to a prompt, model, retriever, or agent tool, and then use[dtctl](https://github.com/dynatrace-oss/dtctl) (Dynatrace CLI tool for AI Agents) to query the resulting evaluation data, inspect related traces, review dashboards, or validate whether a release threshold was met. In an AI-assisted workflow, tools such as Claude Code, Cursor, GitHub Copilot, or an internal agent harness can leverage MCP or CLI access to bring that same observability context into the developer’s daily workflow.


That closes the loop of the AI software delivery lifecycle: teams can evaluate behavior, control rollout decisions, remediate regressions, and feed production learning back into the next development cycle. Quality signals are no longer in a separate report; they’ve become a part of how AI software is built, shipped, and operated.


## Bring evaluations into the release process


Evaluation support is not just for inspection after something breaks. It can also help prevent regressions before they reach users.


Figure 6: Overview of a typical release workflow for an AI app with dt-evals


This makes AI quality part of the release process. Teams can gate changes based on relevance, faithfulness, hallucination risk, prompt-injection risk, toxicity, or custom metrics, rather than relying solely on latency and error rate.


What makes this meaningful is that it’s the same pipeline teams already run. AI quality gates sit alongside the latency, error rate, and SLO gates teams have been using for years. There’s no second CI system, no second platform to learn, no second dashboard to monitor. Quality becomes one more dimension of the release decision, gated the same way performance is gated, by the same platform, in the same pipeline.


## Coming next


The current experience makes evaluation results visible and actionable inside Dynatrace[AI Observability](https://www.dynatrace.com/solutions/ai-observability/) . Next, the focus is on making evaluation workflows easier to run at scale and easier to compare across changes.


Planned improvements include targeted and bulk trace evaluations, custom evaluation libraries, evaluator versioning and lineage, baseline comparisons, experiment views, native quality gates, and deeper visibility into online evaluations.


These capabilities will help teams compare prompt and model variants, understand quality versus cost and latency tradeoffs, and detect sustained quality regressions before they affect more users.


## Start today


To get started,[check out the Git repository](https://github.com/dynatrace-oss/dt-evals) . You’ll need:


- Node.js 20 or later
- A Dynatrace environment with GenAI spans and the AI Observability app installed


- Credentials for the judge provider you want to use
- A service, trace sample, or CI workflow you want to evaluate


Then, install the CLI:


` npm install -g @dynatrace-oss/dt-evals`


Configure your service and judge provider:


` dt-evals configure`


Run your first evaluation:


` dt-evals run --since 1h --sample 10`


With Dynatrace AI Observability and dt-evals, teams can bring LLM and agent evaluations into the operational loop, where they can trace behavior, score outputs, trend results, alert on regressions, and gate releases before silent failures reach production.
