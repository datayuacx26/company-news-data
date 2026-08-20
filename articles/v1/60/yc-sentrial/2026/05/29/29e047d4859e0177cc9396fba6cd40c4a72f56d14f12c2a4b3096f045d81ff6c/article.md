---
schema_version: "1.0.0"
document_id: "29e047d4859e0177cc9396fba6cd40c4a72f56d14f12c2a4b3096f045d81ff6c"
company_key: "yc-sentrial"
company: "Sentrial"
source_id: "yc-sentrial-news-import-e72e91747750"
canonical_url: "https://www.sentrial.com/blog/ai-for-observability-your-agent-isnt-crashing-its-lying"
published_at: "2026-05-27T06:48:37.987+00:00"
first_seen_at: "2026-07-25T23:34:43.014509+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:740a9eed258fe4a24a39723b4d27c32b65f704e0d760d3dd734ea717ca030909"
---

# Your Dashboards Are Green and Your AI Is Still Failing Users

Most engineering teams shipping AI agents hit the same wall eventually. Their dashboards are green, error rates are flat, and their users are quietly getting wrong answers. AI for observability is the practice of collecting and analyzing telemetry signals, prompts, responses, tool calls, retrieval steps, agent decision branches, that reveal whether an AI system is behaving correctly, not just whether it's running. That distinction sounds small until you're three weeks into a silent regression you never saw coming.


If you want a deeper look at LLM-specific failure modes, we've covered that in detail in[LLM Observability Explained: Silent Failures and What to Track](https://www.sentrial.com/blog/llm-observability-silent-failures-nobody-warns-you-about) . This article focuses on the broader agent-behavior layer, which is harder to instrument and almost universally under-monitored.


## "Observability" for AI Means Something Different Than You Think


The word "observability" comes from control theory. It means you can infer a system's internal state from its external outputs. For traditional software, the internal state that matters is whether the service is functioning, and logs, metrics, and traces are enough to answer that.


For AI agents, the internal state that matters most is whether the answer was correct. And logs, metrics, and traces can't answer that question.


This creates a taxonomy problem most teams never fully resolve. There are three distinct layers of observability for AI systems:


1. 1.


**System observability:** latency, error rates, token usage, inference cost, uptime. Table stakes. Traditional APM tools handle these well.
2. 2.


**LLM and RAG quality observability:** prompt and response pairs, retrieval relevance, groundedness scores, hallucination rates, context window utilization.
3. 3.


**Agent-behavior observability:** tool execution sequences and timing, decision branch traces, multi-step reasoning chains, retry patterns, and user frustration proxies like abandonment and repeated queries.


Most teams instrument layer one. Some instrument layer two. Almost nobody systematically monitors layer three, which is where the majority of production failures actually live.


According to[Cleanlab's 2025 production survey](https://beam.ai/agentic-insights/silent-failure-at-scale-the-ai-risk-that-compounds-before-anyone-notices) , only 5% of AI agents that reach production have mature monitoring, with teams still focused on surface-level response quality rather than deeper reasoning and precision control. That number hasn't moved much, and it shows.


## Traditional APM Tools Were Built on an Assumption AI Breaks


Engineering teams default to Datadog, Dynatrace, and New Relic because that's what they know. Those tools are excellent at system observability, they catch crashes, latency spikes, and error rate anomalies. But they were built on a foundational assumption that[traditional monitoring tools still haven't outgrown](https://insightfinder.com/blog/ai-observability-vs-traditional-monitoring/) : that software operates in deterministic states. Working or broken. Up or down.


AI agents aren't deterministic. And the failures that matter most in production produce no error signal whatsoever.


Across 12 million logs we've analyzed at Sentrial, around 22% of issues were explicit tool call failures, something the agent ran into that made it stop. The remaining 78% were silent: hallucinations, user frustration, agent forgetfulness, lazy responses. The agent responded. Latency was fine. Cost was normal. The answer was wrong or useless, and the user left.


A hallucinating agent that confidently returns the wrong order total looks identical to a correctly-functioning one at the infrastructure layer. Your dashboard stays green while users are being misled. As[LangChain's observability team has written](https://www.langchain.com/articles/ai-observability) , AI applications fail in ways that latency dashboards and error rates simply don't capture, instead of crashing, models produce plausible but incorrect answers, and instead of throwing exceptions, agents follow incorrect reasoning paths even when every traditional metric looks healthy.


A regression in retrieval grounding or a drift in tool-calling behavior can silently degrade user experience for days before anyone notices, because nothing crashed.[Models left unchanged for six months or longer see error rates jump 35% on new data](https://beam.ai/agentic-insights/silent-failure-at-scale-the-ai-risk-that-compounds-before-anyone-notices) . For Series A and later teams shipping agents to real users, that kind of invisible drift is an existential product risk, not a monitoring inconvenience.


## The Three Signal Layers You Actually Need


Here's the concrete telemetry checklist that most observability guides gesture at but never structure clearly.


**Layer 1: System signals.** Latency percentiles, error rates, token usage, inference cost, uptime. Start here if you haven't already. These take hours to instrument, not days, and they give you the baseline every other layer depends on.


**Layer 2: LLM and RAG quality signals.** Prompt and response pairs, retrieval relevance, groundedness scores, hallucination rate, context window utilization, re-ranking decisions. RAG pipelines deserve their own attention here, retrieval step latency, chunk relevance, and what gets passed to the model all affect output quality in ways layer one metrics will never surface. We've kept this section brief because the LLM-specific deep-dive lives in our[LLM Observability article](https://www.sentrial.com/blog/llm-observability-silent-failures-nobody-warns-you-about) .


**Layer 3: Agent-behavior signals.** Tool execution sequences and timing, decision branch traces, multi-step reasoning chains, retry patterns, and user frustration proxies. This is the hardest layer to instrument, for a specific reason: unlike infrastructure metrics, agent-behavior signals don't have a standard schema. Tool calls vary by framework, LangChain, LlamaIndex, and custom agents log very different things by default. Decision branches aren't logged at all unless you instrument them explicitly. And "frustration" has to be inferred from interaction patterns rather than read from a metric.


[OpenTelemetry](https://opentelemetry.io/blog/2025/ai-agent-observability/) is becoming the standard for instrumentation across layers one and two. By establishing shared conventions, it lets AI agent frameworks report standardized metrics, traces, and logs across different observability solutions, that matters for portability. But OpenTelemetry handles tracing and telemetry collection. It doesn't classify behavioral quality at scale. Knowing a tool call happened in 340ms is not the same as knowing whether the tool returned a useful result.


For DevOps agents and other non-deterministic agent types, this gap gets wider fast. When tool calls and branching behavior vary across runs, trace-and-end-state-only approaches stop being sufficient quickly. We built Sentrial specifically to monitor the semantic and behavioral signals that appear even when a run doesn't follow the same path twice.


## AI Inside Observability Platforms Is a Different Problem


The phrase "AI for observability" carries a second meaning worth addressing: using AI techniques to improve observability of any system, not just AI systems. Both interpretations are valid. The second one is a more solved problem, and it's not what this article is about.


Traditional software stacks have benefited a lot from AI-assisted observability, anomaly detection on metric streams, log clustering and pattern recognition, automated root-cause analysis, natural language querying of telemetry data. Dynatrace, Datadog, and similar platforms use these techniques to help SREs find incidents faster. They work well for that use case.


The problem is that these same techniques hit a ceiling when applied to AI agent monitoring. Anomaly detection requires "normal" to be defined, but agent output quality isn't a numeric metric, it's a semantic judgment.[Traditional APM tracks requests, responses, and errors, but misses AI model behavioral shifts, like predictions skewing or losing confidence](https://insightfinder.com/blog/ai-observability-vs-traditional-monitoring/) . Log clustering by pattern tells you a set of responses look similar. It doesn't tell you whether any of them were factually correct.


This is why most teams trying to solve the agent monitoring problem with incumbent tools end up in one of two failure modes. The first is using evals, running a fixed test suite against production samples before deployment. This worked reasonably well when agents were simple chatbots. It becomes unreliable as agents grow to involve hundreds of tool calls, complex branching behavior, and multi-hour runs. The second is using an LLM-as-judge system, passing logs through a model with a scoring prompt.[This approach samples only a subset of traffic due to cost constraints](https://www.getmaxim.ai/articles/the-best-3-llm-evaluation-and-observability-platforms-in-2025-maxim-ai-langsmith-and-arize-ai/) , which means it structurally misses the long-tail failures that compound quietly in production.


The right tool for classifying AI agent behavior at scale isn't a general-purpose LLM. It's a model purpose-trained for that classification task, running against every log, not a sample.


## Three Things Teams Get Wrong About AI Observability


**Misconception 1: Traces are enough.**


Many teams instrument with OpenTelemetry, get detailed waterfall traces of every agent step, and think they have observability. What they have is execution visibility. A trace tells you the tool was called and returned in 340ms. It doesn't tell you the tool returned garbage, or that the agent used the result incorrectly in the next step.[Traditional monitoring confirms a request succeeded with a 200 OK and acceptable latency, but it cannot detect when an agent selects the wrong tool or gets trapped in a reasoning loop](https://www.langchain.com/articles/llm-monitoring-observability) . Traces are necessary. They're not sufficient.


**Misconception 2: LLM-as-judge evals give you production coverage.**


This one is less obvious and more dangerous. Teams run periodic eval suites using a judge LLM to score samples of production traffic, and it feels rigorous, there's a score, a threshold, the dashboard shows a pass rate. The structural flaw is threefold. First, sampling misses the long tail of rare failure modes. The edge case affecting 2% of conversations won't appear reliably in a 5% sample. Second, generic judge models aren't calibrated to your specific agent's expected behavior, they evaluate against general quality heuristics, not your product's actual success criteria. Third, batch evals don't catch regressions in real time.[Due to budget and time constraints, even careful human reviewers only cover small samples, and nuanced failures of tone, subtle hallucinations, and context misunderstanding get missed as a result](https://www.mlaidigital.com/blogs/llm-evaluation-frameworks-2025-vs-2026-what-matters-now-2026) . You can pass your eval suite on Tuesday and have a silent regression accumulating through the rest of the week.


**Misconception 3: More dashboards equals more observability.**


Collecting every available metric and surfacing it in a dashboard creates alert fatigue without improving detection of what matters. Observability isn't about metric volume. It's about reducing the time between "something is wrong" and "I know exactly what and why." That requires classification precision, the ability to say "this is a hallucination failure in this specific tool call" rather than "quality score dropped 0.3 points."


This is the pattern we designed Sentrial to break. Rather than adding generic metrics, we use post-trained models fine-tuned on each customer's own agent traffic to classify every log, not a sample, in real time. When something goes wrong, teams get a Slack alert that names the failure mode, points to the source-code location, and suggests a fix. The goal is zero distance between "something is wrong" and "here is what and where."


We cover how this compares to specific alternatives in our[Arize vs Sentrial comparison](https://www.sentrial.com/blog/arize-vs-sentrial-honest-comparison-2026) and[Braintrust vs Sentrial comparison](https://www.sentrial.com/blog/braintrust-vs-sentrial-honest-comparison-2026) if you want a direct tool-level breakdown.


## How to Actually Get Started With AI Observability


Don't try to instrument everything at once.


**Step 1: Get layer one in place.** If you're not already capturing latency, error rates, token usage, and inference cost, start there. Table stakes, takes hours with OpenTelemetry or your existing APM, and gives you the baseline everything else depends on.


**Step 2: Add trace-level visibility into your agent's execution path.** Most orchestration frameworks, LangChain, LangGraph, LlamaIndex, have built-in tracing hooks. Use them. You want to see tool calls, retrieval steps, and decision branches per run, not just aggregate metrics. Our[AI Agent Tracing Explained](https://www.sentrial.com/blog/ai-agent-tracing-explained-traces-evaluations-replay-and-debugging) article covers this layer in detail if you want the full breakdown.


**Step 3: Add quality classification.** This is where you move from "is it running" to "is it working." You have two realistic options.


The first is building your own eval pipeline with periodic sampling and an LLM judge. Cheap to start, reasonable for low-traffic agents. The limitations are real: you won't cover your full log volume, calibration against your specific agent behavior requires ongoing maintenance, and you won't catch regressions in real time. For agents with fewer than a few hundred daily conversations, this may be acceptable.


The second is using a dedicated AI observability platform. The landscape includes Arize, Langfuse, and Braintrust, which focus primarily on eval workflows and quality metrics. Sentrial focuses specifically on production agent behavior, classifying every log with per-customer fine-tuned models, providing replay and fork from any intermediate step for root-cause diagnosis, and sending real-time Slack alerts with source-code-level failure pinpointing. Integration is five lines of code via OpenTelemetry, LangChain, LangGraph, or a custom Python agent.


A practical note on data handling before you ship: decide what to log and what to redact before your first production deployment. Prompt and response logs contain user data. At minimum, redact PII at the collection layer and define retention windows. You need enough signal to classify failure modes without exposing sensitive content. This is a configuration decision, not an afterthought.


**The prioritization heuristic:**[Gartner projects that 40% of enterprise applications will be integrated with task-specific AI agents by 2026](https://www.gartner.com/en/newsroom/press-releases/2025-08-26-gartner-predicts-40-percent-of-enterprise-apps-will-feature-task-specific-ai-agents-by-2026-up-from-less-than-5-percent-in-2025) , up from less than 5% in 2025. That's a lot of new agents, and the monitoring burden grows with the count. If your agent is already handling more than a few hundred daily active conversations, sampling-based evals will miss real failure modes. Full-coverage classification becomes necessary before you'll see the silent regressions your users are already experiencing.


We've written a practical guide specifically for early-stage teams in[AI Agent Monitoring for Startups](https://www.sentrial.com/blog/ai-agent-monitoring-for-startups-2026-guide) if you want a more startup-specific framing of these tradeoffs.


---


## FAQ


**What is AI observability?**


AI observability is the practice of collecting and analyzing telemetry signals, prompts, responses, tool calls, retrieval steps, agent decision branches, that reveal whether an AI system is behaving correctly, not just whether it's running. It's distinct from infrastructure monitoring, which tracks uptime and latency. True AI observability covers three layers: system signals, LLM and RAG quality signals, and agent-behavior signals. Most teams only instrument the first layer, which means the majority of production failures, hallucinations, user frustration, agent forgetfulness, go undetected.


**What are the best AI observability tools?**


The right tool depends on which layer you're trying to cover. For system observability, traditional APM tools like Datadog and Dynatrace work well. For LLM and RAG quality signals, platforms like Arize, Langfuse, and Braintrust offer eval workflows and quality metrics. For production agent-behavior monitoring, classifying every log in real time, alerting on behavioral anomalies, and diagnosing failures at the source-code level, Sentrial is built specifically for that use case. We have detailed comparisons in our[Arize vs Sentrial](https://www.sentrial.com/blog/arize-vs-sentrial-honest-comparison-2026) and[Sentrial vs Langfuse](https://www.sentrial.com/blog/sentrial-vs-langfuse-honest-comparison-2026) articles.


**How do you use AI for observability?**


AI techniques improve observability pipelines in two ways: by helping monitor traditional software systems (anomaly detection on metric streams, log clustering, automated root-cause analysis), and by evaluating the quality of AI agent behavior in production. The second use case is harder. Output quality for an AI agent is a semantic judgment, not a numeric metric, so it requires classification models trained specifically for that task, not generic anomaly detection or LLM-as-judge scoring. The practical starting point is instrumenting your agent with OpenTelemetry for trace collection, then layering quality classification on top of the log data.


**What are the four pillars of observability?**


Traditional observability is defined by four signals: logs, metrics, traces, and events. These pillars were designed for deterministic software systems and remain relevant for the infrastructure layer of AI deployments. For AI agents specifically, a fifth pillar is required: behavioral quality classification, the ability to determine whether an agent's outputs were correct, not just whether the system produced them. Without that fifth pillar, you have execution visibility but not behavioral observability. Across 12 million logs we've analyzed, 78% of agent failures produced no error signal in the traditional four pillars. They showed up only as wrong or unhelpful answers.
