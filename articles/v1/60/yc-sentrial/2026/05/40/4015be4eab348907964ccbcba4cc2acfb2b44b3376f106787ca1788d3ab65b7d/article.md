---
schema_version: "1.0.0"
document_id: "4015be4eab348907964ccbcba4cc2acfb2b44b3376f106787ca1788d3ab65b7d"
company_key: "yc-sentrial"
company: "Sentrial"
source_id: "yc-sentrial-news-import-e72e91747750"
canonical_url: "https://www.sentrial.com/blog/llm-monitoring-explained-traces-evals-alerts-and-debugging"
published_at: "2026-05-31T17:35:30.889+00:00"
first_seen_at: "2026-07-25T23:34:43.014509+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:11b547176232cf41ed82d33f16d1776a4ee61df39a4a08c26047d11da9cb3c99"
---

# LLM Monitoring Is Harder Than You Think, Here's Why

Most teams building LLM applications assume their existing monitoring stack will tell them when something goes wrong. It won't. We went through 12 million production logs at Sentrial and found that 78% of failures were completely silent, wrong answers, confused agents, hallucinated facts, all returning HTTP 200 while nobody noticed. That's the gap LLM monitoring exists to close.


## Traditional APM Has No Idea What a Wrong Answer Looks Like


LLM monitoring is the continuous process of capturing what an LLM application does in production, inputs, outputs, latency, token costs, tool calls, evaluating whether those outputs meet quality and safety thresholds, and triggering actionable alerts when they don't. It's worth separating this from LLM observability: observability is the property of a system that makes it possible to understand internal state from external outputs. Monitoring is the practice you build on top of that property. For a deeper look at the four-layer observability model, see our article on[LLM observability and silent failures](https://www.sentrial.com/blog/llm-observability-silent-failures-nobody-warns-you-about) .


The reason this distinction matters operationally: traditional APM watches for crashes and latency spikes. It has no concept of a wrong answer. An LLM application can return HTTP 200 on every request while hallucinating on a third of them, and nothing in your existing stack will fire.[Braintrust](https://www.braintrust.dev/articles/what-is-llm-monitoring) puts it plainly: traditional APM tools measure infrastructure health but cannot evaluate the quality of LLM output.


In our data, roughly 22% of failures were explicit tool call failures where something made the agent stop. The remaining 78% were silent regressions: hallucinations, user frustration, agent forgetfulness. The user got a wrong or useless answer and left. No alert fired.


## Three Things Make LLM Monitoring Categorically Different From What You Already Know


Three compounding difficulties make this a different problem from standard application monitoring.


The first is non-determinism.[Sendbird](https://sendbird.com/blog/ai-hallucination-detection) describes it directly: AI agents' outputs are inherently non-deterministic, so the same input may produce different outputs at different times. You can't write a test that asserts an exact string match and call it done.


The second is multi-step chain opacity. A failure in step 3 of a 7-step agentic run is invisible if you only log the final response.[Trantorr's analysis of agent failure modes](https://trantorinc.com/blog/ai-agent-failure-modes-what-goes-wrong-design-resilience) captures this well: an agent can complete a task returning a confident, well-formatted output while getting the answer completely wrong, or misunderstand an instruction on step two and silently propagate that error across twenty downstream steps. When tool calls and branching behavior vary across runs, as they do with DevOps agents and any long-horizon task, trace-and-end-state-only approaches stop being sufficient.


The third is mixed user intent. You can't write a regex for "the agent forgot what the user said three turns ago." Failure modes like instruction-following drift and goal abandonment require semantic classification, not pattern matching.


Underneath all three sits the silent failure problem. A bad LLM output returns HTTP 200.[Respan](https://www.respan.ai/blog/llm-failures-in-the-wild) notes that without a separate fact-checking layer or traces showing retrieval failures, there's no signal that anything went wrong.[TianPan](https://tianpan.co/blog/2026-03-11-llm-api-resilience-production) calls this silent quality degradation: HTTP success rates cannot detect it.


There's also a sampling risk most teams underestimate. A 10% traffic sample with a 0.3% failure rate means you're inspecting 0.03% of actual failures. Rare but severe failure modes, a jailbreak variant, a specific tool-call edge case, a failure that only triggers in step 5 or later of a long chain, appear at frequencies below typical sampling thresholds. You'll never see them until a user complains.


## You Need Three Signal Layers, and Most Teams Only Have One


Most production LLM monitoring requires three distinct signal layers, and the failure mode for each is different if you skip it.


**Layer 1: Operational signals.** Latency per step, token count and cost per step (not just per session), error rates, and tool call success and failure rates. The key word is per step. If you only attribute token costs at the session level, you can't identify which retrieval call or tool invocation is ballooning your spend. Operational signals are the easiest to collect and the most commonly instrumented, which is why they give teams false confidence.


**Layer 2: Quality signals.** Hallucination assertions, retrieval relevance, goal completion, and instruction-following fidelity across the full conversation. This is the layer most teams under-instrument. From our analysis, hallucinations are the top failure category, user frustration is second, and agent forgetfulness or laziness is third. None of these stop the run. They show up as incorrect or unhelpful answers that create silent regressions. Quality signals require automated evaluations running against every interaction, not spot checks.


What makes this layer tractable at scale is classifier flexibility. We can classify pretty much any metric. Our built-in classifiers cover bad tool calls, agent forgetfulness, and jailbreaking. But a finance company we work with needed a mismatched GL codes classifier. You might assume an end-state check could catch that, but even end states can have hundreds of variations, and if a GL code isn't in the system, the agent might not output a product with the code at all. Because agents can take many different paths based on input, a rigid start-or-end-state check won't catch behavioral variance across intermediate steps.


**Layer 3: Safety and risk signals.** Jailbreak detection, prompt injection, toxicity, and data leakage. These belong in the same assertion pipeline as quality signals. They shouldn't be a separate manual review workflow. At the volumes production agents operate at, manual review doesn't scale.


## Here's the Monitoring Workflow We Actually Use


Here's the implementation loop we use.


**Step 1: Instrument every intermediate step.** Emit spans for prompt construction, retrieval, each tool invocation, model call, and output parsing. Not just the final response.[OpenTelemetry](https://agenta.ai/blog/the-ai-engineer-s-guide-to-llm-observability-with-opentelemetry) is the free, open-source standard that provides a single, standardized way to handle this instrumentation layer.[Distributed tracing](https://www.getmaxim.ai/articles/tracing-ai-agent-failures-debugging-multi-step-tool-workflows/) records LLM requests, tool and API calls, retrieval operations, and multi-turn conversation flows as a connected trace graph, giving teams the structure they need to diagnose root causes.[Unified traces](https://www.truefoundry.com/blog/ai-agent-observability-tools) are what let teams identify which specific model call, tool invocation, or reasoning step caused a regression.


**Step 2: Run automated evaluations against every interaction.** Start with built-in classifiers for hallucinations, bad tool calls, goal abandonment, and user frustration. Add custom classifiers for domain-specific failure modes as you learn what actually breaks in your application. Classification needs to run at full log volume. A 10% sample isn't a monitoring strategy; it's a way to miss the failures that matter most.


**Step 3: Gate prompt changes with statistically rigorous A/B testing.** Most prompt testing is fake. Teams run evals that look clean or do manual A/B tests, but neither tells you what actually improves agent performance in production. Production A/B testing against live traffic, measuring what matters like frustration, refusals, and accuracy, is a different discipline. For a full treatment, see our article on[prompt A/B testing that catches silent failures](https://www.sentrial.com/blog/prompt-ab-testing-that-catches-silent-failures) .


**Step 4: Route alerts with code-level context.** Not "quality degraded" but: classifier, hallucination; trace ID, XYZ; step 4 of 7; source file, agent.py, line 112; suggested fix. An alert without that context sends your on-call engineer on a log spelunking expedition. The goal is zero digging required.


**Step 5: Debug by replaying or forking from the exact failing step.** Not by re-running the whole session from scratch. When a failure surfaces in step 5 of a 9-step chain, you should be able to fork from step 4, change the prompt or tool configuration, and replay forward. That's how monitoring becomes your fastest feedback loop.


At Sentrial, setup starts with five lines of code and uses OpenTelemetry for initial logging, then runs classification and clustering on top of those traces. The important part is that it's not just capturing traces, we use the logged data to drive classification and clustering of problems across runs, across the full log volume, without routing every log through an LLM judge.


## Three Beliefs That Keep Burning Teams in Production


**Misconception 1: "If evals pass in CI, the model is fine in production."** Eval datasets go stale.[VentureBeat](https://venturebeat.com/infrastructure/monitoring-llm-behavior-drift-retries-and-refusal-patterns) calls this eval rot: as user behavior evolves, static datasets become outdated. Production drift introduces new user intents, updated tools, and prompt version changes that create failure modes your eval set never anticipated. And most issues pop up in production precisely because you can't predict and account for them beforehand. Monitoring is what catches the delta between "passed evals last Tuesday" and "hallucinating today."


**Misconception 2: "Monitoring a 10% sample is good enough."** This is the mistake we see most often. A 10% sample with a 0.3% failure rate means you're effectively inspecting 0.03% of actual failures. Rare but severe failure modes, a jailbreak variant, a tool-call edge case, a failure that only triggers deep in a long chain, appear at frequencies below typical sampling rates. You won't catch them.[Earezki](https://earezki.com/ai-news/2026-03-12-we-built-a-service-that-catches-llm-drift-before-your-users-do/) documents a related version of this: developers assume frozen model versions remain static, but providers modify model behavior without notice, and the resulting drift stays undetected until user reports surface. Full-coverage classification isn't a premium feature. It's the minimum for reliable regression detection.


**Misconception 3: "Latency and error rate cover the risk."** An LLM application can have perfect operational metrics while producing wrong answers.[Langchain](https://www.langchain.com/articles/llm-monitoring-observability) states it plainly: an agent can have 99% uptime but still fail to follow user intent. Traditional monitoring confirms a request succeeded but can't detect whether the output meets quality thresholds. Quality signals aren't derivable from operational telemetry. They require a separate evaluation layer.


These three misconceptions share a common thread: teams underestimate the volume and variety of ways an agent can fail silently. That's exactly what drove the design of Sentrial. We classify every log, not a sample. Built-in classifiers for common failures ship out of the box; custom classifiers for domain-specific failure modes deploy in under a minute using three or four example logs. Session-level tracing makes replay from any intermediate step possible, so when something breaks, you can go directly to the step that broke it.


## Build This in Four Phases, Not All at Once


Treat this as a phased build, not an all-or-nothing implementation.


**Phase 1: Instrument.** Set up OpenTelemetry or a native SDK and capture inputs, outputs, latency, and token cost at the session level. This is the foundation. Nothing else in the stack works without it. If you're using LangChain or LangGraph, both have native integration paths that make span emission straightforward.


**Phase 2: Add automated quality assertions.** Start with built-in classifiers for the most common failures: hallucinations and tool call errors. As you run in production and learn what actually breaks in your specific application, add custom classifiers for your domain-specific failure modes. Custom classifier instantiation shouldn't require a machine learning team. The pattern we've found most effective is: check three or four representative failure logs, deploy a fine-tuned lightweight classifier, verify accuracy on the next batch of live traffic.


**Phase 3: Wire alerts to your incident workflow.** Connect Slack or PagerDuty, but make sure the alert payload includes the trace, the failing step, the classifier label, and a suggested fix. An alert that requires a manual log investigation before anyone can act is an alert that delays resolution.


**Phase 4: Close the loop.** Use production monitoring data to improve your eval dataset, validate prompt changes with statistically rigorous A/B testing in production, and replay from failing sessions during debugging. At this stage, monitoring isn't a cost center. It's your fastest feedback loop for improving the agent.


For teams that want phases 1 through 4 in a single platform,[Sentrial](https://www.sentrial.com/) integrates via OpenTelemetry, LangChain, LangGraph, and custom Python agents. Setup takes five lines of code. For teams comparing platforms before committing, our[LLM observability platform comparison](https://www.sentrial.com/blog/best-llm-observability-platforms-in-2026) covers the field in 2026.


## Frequently Asked Questions


**What is the difference between LLM monitoring and LLM observability?**


Observability is the property of a system that allows you to understand its internal state from external outputs. Monitoring is the practice you build on top of that property: collecting signals, evaluating them, and acting when something degrades. You need observability as a foundation, but observability alone doesn't alert you, classify failures, or route an engineer to the right line of code. The opening section of this article covers the distinction, and our[LLM observability explainer](https://www.sentrial.com/blog/llm-observability-silent-failures-nobody-warns-you-about) covers the four-layer architectural model in depth.


**What metrics should you monitor for LLM applications?**


The three-layer taxonomy in the "Signal Layers" section above gives the full picture. At a minimum: per-step latency and token cost (Layer 1), hallucination rate, goal completion, and instruction-following fidelity (Layer 2), and jailbreak and prompt injection detection (Layer 3). Most teams over-invest in Layer 1 and underinvest in Layer 2, which is where the majority of production failures actually live.


**How do you monitor hallucinations or answer quality in production?**


With automated evaluation assertions running against every interaction, not human review and not spot sampling. Human review doesn't scale to production log volumes. LLM-as-judge systems lose accuracy as agents become more complex and run longer chains. The approach that works at scale is a fine-tuned lightweight classifier that runs on every log. From our data, hallucinations are the top failure category we see, which means any monitoring stack that doesn't include a quality assertion layer is missing most of its signal.


**How can OpenTelemetry be used for LLM monitoring?**


OpenTelemetry provides the instrumentation layer: spans, events, and attributes that capture what happened at each step of an agent run. It's the right standard for this because it's open, portable, and widely supported. What it doesn't include is evaluation or alerting. You need a layer above OTel that classifies output quality, runs assertions, and routes actionable alerts. OTel gives you the raw trace data; a monitoring platform gives you the signal.


**What should an LLM monitoring stack include end-to-end?**


At minimum: session-level tracing with per-step instrumentation, automated quality and safety evaluations running at full log coverage, real-time alerting with trace context and code-level failure attribution, production A/B testing for prompt validation, and replay or fork capability for debugging from any intermediate step. Most tools cover one or two of these. The[LLM observability platform comparison](https://www.sentrial.com/blog/best-llm-observability-platforms-in-2026) maps the current landscape if you're evaluating options.
