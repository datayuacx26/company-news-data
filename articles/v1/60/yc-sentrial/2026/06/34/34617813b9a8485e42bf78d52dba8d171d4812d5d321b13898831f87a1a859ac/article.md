---
schema_version: "1.0.0"
document_id: "34617813b9a8485e42bf78d52dba8d171d4812d5d321b13898831f87a1a859ac"
company_key: "yc-sentrial"
company: "Sentrial"
source_id: "yc-sentrial-news-import-e72e91747750"
canonical_url: "https://www.sentrial.com/blog/langfuse-review-2026-features-gaps-honest-verdict"
published_at: "2026-06-12T07:19:57.292+00:00"
first_seen_at: "2026-07-25T23:34:43.014509+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:7d92e56b641b8f64a72382fcd9b6823bbcde18567970c4bd78abbe27e0703883"
---

# Langfuse Is Good at Tracing. Here's Where It Stops.

Most teams that switch away from Langfuse don't do it because tracing was bad. They do it because tracing was fine, and then production agents started failing in ways tracing couldn't catch. We've looked at 12 million logs across production agent deployments, and 78% of the failures that actually hurt teams never showed up in a log at all. No error, no latency spike, nothing an engineer would notice scrolling past. That's the gap this review is about.


## Quick Verdict: What Langfuse Is Good For


Langfuse is a strong[OpenTelemetry-native tracing and prompt management platform](https://langfuse.com/integrations/native/opentelemetry) . If your primary need right now is visibility into what your agent did (full session traces, token costs, latency, prompt versioning), it's a fine first choice, especially early on when you're just getting instrumentation in place.


Where it stops is the production operations loop. Langfuse doesn't classify every run by default, doesn't fire real-time Slack alerts tied to evaluated failure modes, and doesn't tell you which source-code step caused a bad trace. It shows you the trace. What to do with it is on you.


We score Langfuse against four things production AI agent teams need: tracing completeness, eval coverage of real failure modes, alerting depth, and debugging speed. We use Sentrial as the comparison point throughout. We're not the only alternative, but we built specifically around the gaps this review covers.


### Quick Comparison


Capability Langfuse Sentrial


Session-level tracing (inputs, outputs, latency, token costs) Yes Yes


OpenTelemetry-native Yes Yes


Framework-agnostic (LangChain, LangGraph, custom Python) Yes Yes


Self-hosting / data residency Yes No


Free tier Yes No


Eval approach LLM-as-a-judge, manual config Built-in classifiers + custom classifiers


Full log coverage (every run, not a sample) Sampling-based Every interaction


Built-in classifiers (hallucination, tool failures, forgetfulness) No, manual setup required Yes, out of the box


Custom classifier deployment Manual eval pipeline 3-4 example logs, under a minute


Real-time Slack alerts on evaluation failures Not yet (on roadmap) Yes


Source-code failure pinpointing No Yes


Fix suggestions / 1-click PR No Yes


Trace replay and fork from intermediate step No Yes


Prompt A/B testing with statistical rigor No, versioning only Yes


## What Langfuse Gets Right


Tracing is where Langfuse is strongest. It's[OpenTelemetry-native and accepts traces on the OTLP endpoint](https://langfuse.com/integrations/native/opentelemetry) , so it works across frameworks without needing dedicated SDKs. It[integrates with OpenAI, LangChain, LangGraph, LiteLLM, and custom Python](https://langfuse.com/docs/observability/get-started) , capturing session-level traces with inputs, outputs, latency, and token costs at every step. For multi-step agents with branching execution paths, that visibility is useful.


Prompt management is where Langfuse pulls ahead of most of this space. Langfuse supports[prompt versioning, labels, and linking prompt versions directly to trace outcomes](https://langfuse.com/docs/prompt-management/features/webhooks-slack-integrations) . You can see what happened to downstream behavior when you changed a prompt. Most tools tell you the change deployed; Langfuse shows you how traces looked before and after. That feedback loop is more mature than most tools in this space.


Evals cover the basics. Langfuse[supports LLM-as-a-judge and code evaluators, running both online on production traces and offline on datasets](https://langfuse.com/docs/evaluation/overview) . Unlike general-purpose APM tools, it[provides features specific to AI engineering: LLM-as-a-Judge evaluation, prompt management, experiments, and datasets](https://langfuse.com/docs/observability/overview) . If you already have eval logic written, Langfuse gives you a sane place to run it against traces.


Self-hosting is the other genuine strength. For enterprises with compliance requirements that prevent sending production data to a third-party cloud, Langfuse's self-hosted option settles the question. We don't offer self-hosting, so this is a category where Langfuse has coverage we don't.


## The Production Gaps Are Structural, Not on a Roadmap


Back to that 12 million log dataset: roughly 22% of issues were explicit tool call failures, the kind that make an agent surface an error. The other 78% were silent. Hallucinations were the top category, followed by user frustration signals and agent forgetfulness. Traditional observability showing you logs and latency graphs won't catch any of that. The question is whether Langfuse's eval layer closes the gap.


**Hallucination.** Langfuse can run LLM-as-a-judge scoring against a trace to detect hallucinated outputs, but you have to build and configure the eval logic yourself, and[production agents hallucinate when they assume data exists that doesn't, requiring logging raw tool calls before execution](https://medium.com/@hackastak/debugging-ai-agent-hallucinations-a-checklist-from-production-f0d3317fda5c) to catch it. Langfuse gives you the raw log. Building the classifier is on your team.


**Bad tool calls.**[Tool misuse is the most common agent-specific failure mode in production, a single malformed argument silently corrupts every subsequent step](https://latitude.so/blog/ai-agent-failure-detection-guide) . Langfuse traces tool calls, so you can see they happened. It has no opinion on whether the arguments were right.


**Agent forgetfulness.** When an agent drops context across a multi-turn session and starts contradicting its earlier outputs, you need behavioral comparison across steps in the same session. Langfuse's session traces give you the raw material and nothing else.


**Goal abandonment.** An agent that quietly stops pursuing its objective without surfacing an error is one of the harder failure modes to catch. Langfuse doesn't ship a built-in classifier for it. Sentrial does.


The sampling gap makes all of this worse. Langfuse's[evaluation rules define targets, filters, and sampling rates](https://langfuse.com/docs/evaluation/evaluation-methods/llm-as-a-judge) . By design, not every run gets evaluated. For high-frequency obvious failures, sampling is a fair tradeoff. For the failure modes above, which often affect a small percentage of sessions and show no surface error, sampling means they stay invisible. A jailbreak hitting 0.3% of sessions will almost certainly not show up in a sampled eval pipeline. We classify every interaction because that's the only way to catch low-frequency, high-impact failures before they compound.


Alerting is where the operational gap gets concrete. Langfuse[recently launched Slack integration for prompt version notifications](https://langfuse.com/changelog/2025-07-30-slack-prompt-notifications) , but[alerting for evaluation assertion failures remains on the roadmap](https://langfuse.com/changelog/2025-07-30-slack-prompt-notifications) .[Community requests for score-based Slack alerts are acknowledged as high priority but require infrastructure work](https://github.com/orgs/langfuse/discussions/1512) . In practice, Langfuse can tell your team a prompt version changed. It can't fire an alert when hallucination rate spikes in production, tied to the specific trace and code step that caused it.


Then there's speed. Langfuse requires teams to build and maintain their own eval logic for each failure mode they want to catch. We let teams define any failure mode, check three or four example logs, and deploy a fine-tuned classifier in under a minute. At scale, that difference is weeks of engineering time.


## After You Find a Bad Trace, the Tools Diverge


Both tools deliver solid session-level tracing. Langfuse logs[every step of a multi-step conversation or agentic workflow with latency and token costs attached](https://langfuse.com/docs/observability/get-started) , and its OTel-native approach means you can pipe in traces from almost any framework without framework-specific instrumentation overhead.


The divergence is what happens after you find a bad trace.


Langfuse shows you the trace. Our trace replay shows an execution graph: which step fed which, where the bad state entered, and how it spread. You can replay from any intermediate step and fork the run to test a fix instead of reproducing the failure by hand.


There's one setup gotcha with the OTel integration. Langfuse[requires trace-level attribute propagation to all spans for accurate filtering and aggregation](https://langfuse.com/integrations/native/opentelemetry) , which can require platform engineering work to keep agent traces cleanly separated from unrelated OTel traffic in a shared infrastructure environment.[Langfuse gives you the LLM-specific view, prompt content, completions, token usage, and latency per agent](https://dev.to/alexiskroberson/opentelemetry-for-llm-applications-a-practical-guide-with-launchdarkly-and-langfuse-1a3a) , but the filtering and routing to make that view actionable in a complex deployment is configuration work.


We also wire the trace into the fix: a failing run carries its execution context into GitHub, where it becomes a diff, a suggested patch, or an open pull request.


**Winner on tracing and debugging:** Langfuse for teams that want framework-agnostic tracing with minimal setup. Sentrial for teams that need replay, fork, and fix suggestions as part of the debugging loop.


## Built-in Failure Detection vs. Building It Yourself


Langfuse's eval mechanics work.[LLM-as-a-judge and code evaluators run both online on production traces and offline on datasets](https://langfuse.com/docs/evaluation/overview) . If your team has existing eval logic and wants to connect it to production traces, Langfuse is a reasonable integration surface.[Evaluation rules define targets, filters, and sampling rates for production online evaluation](https://langfuse.com/docs/evaluation/evaluation-methods/llm-as-a-judge) .


The gap is coverage speed and what ships by default. Langfuse ships no built-in classifiers for hallucinations, bad tool calls, agent forgetfulness, or goal abandonment. Every failure mode you want to catch requires you to build the eval logic, test it, and maintain it as your agent evolves. We ship built-in classifiers for all four, plus jailbreak detection, plus the ability to instantiate custom classifiers from example logs in under a minute.


The accuracy argument matters too. LLM-as-a-judge systems, the mechanism underlying most custom eval pipelines in Langfuse, struggle with agents that run for extended periods with hundreds of tool calls. The longer a session runs, the more the judge misses. We use fine-tuned classifiers rather than a general LLM judge, which is how we maintain accuracy across long multi-step agent sessions.


One Fortune 1000 customer using Sentrial to monitor production agents across supply chain, HR, and marketing workflows saw their error rate drop from 20% to under 10% in a single week. The agent itself didn't change. They could finally see what was failing and where.


[Early mistakes in agent execution cascade through subsequent decisions, making error propagation more critical than variety of failure modes detected](https://galileo.ai/blog/agent-failure-modes-guide) . Catching the first bad tool call before it corrupts five downstream steps requires both full coverage and low-latency detection.


**Winner on evaluations and failure detection:** Sentrial for production failure detection coverage and classifier deployment speed. Langfuse for teams with existing eval pipelines they want to connect to traces, or for offline dataset evaluation during development.


## Alerting That Pages You vs. Alerting That Notifies You


This is the gap that costs you at 2 AM.


Langfuse's Slack integration[is currently scoped to prompt version notifications](https://langfuse.com/docs/prompt-management/features/webhooks-slack-integrations) . It's not tied to evaluated assertion failures. When hallucination rate spikes or tool failures climb past a threshold you've set, nothing pages your team.[Score-based alerting for evaluation failures is acknowledged on the roadmap but requires infrastructure work](https://github.com/orgs/langfuse/discussions/1512) . Until it ships, the operational path is: something looks wrong in your dashboard, you investigate manually.


We fire real-time Slack alerts on error spikes and behavioral anomalies with source-code-level failure pinpointing. The alert names the code step behind the spike. We've watched teams spend months reviewing sessions by hand and stitching together telemetry from multiple systems to answer questions an alert should have answered in a minute.


Here's a practical rubric for evaluating any observability tool's alerting: it should trigger on evaluator failures (not just crashes), deduplicate, link to the specific failing trace or step, and name the code change or prompt version that introduced the regression. Langfuse currently does none of these. We do all four.


On prompt A/B testing: Langfuse supports prompt versioning and labels, which enables manual before-and-after comparison. It doesn't provide[statistical significance testing for production A/B experiments](https://langfuse.com/docs/prompt-management/features/webhooks-slack-integrations) . Versioning tells you Prompt B produced different outputs than Prompt A. It doesn't tell you whether B performed better across representative production traffic with statistical confidence. Our A/B testing answers the second question.


**Winner on alerting and A/B testing:** Sentrial for production teams that need behavioral alerting and statistically rigorous prompt experiments. Langfuse for teams whose alerting needs are covered by infrastructure-level error monitoring and whose prompt comparison workflow is manual.


## Which One Should You Actually Pick?


**Choose Langfuse if:**


- •


Your primary need is framework-agnostic OTel tracing with solid prompt versioning
- •


Your eval workflows are primarily offline, run against datasets during development
- •


You have compliance or data residency requirements that require self-hosting
- •


You're early-stage and want a free tier to instrument your agent before you have production traffic to analyze


**Choose Sentrial if:**


- •


You're running production agents and need the full loop: tracing, automated failure classification on every run, real-time behavioral alerts, and source-code-level debugging
- •


Sampled eval pipelines miss rare failures; we classify everything
- •


You need custom classifiers deployed fast without building eval infrastructure from scratch
- •


You're a Series A or later startup or an enterprise with millions of agent interactions monthly that your team can't manually review


**If you're torn:** If your team's primary gap is tracing visibility and you already maintain an eval pipeline, Langfuse covers a lot of ground at low cost. If your primary gap is "we don't know what's failing and we need to find out fast," our end-to-end stack is the faster path to root cause. One of the most common patterns we see is teams graduating from Langfuse when production traffic scales past what manual log review and sampled evals can realistically cover.


If you're evaluating the broader space before deciding, our[LLM observability platforms comparison for 2026](https://www.sentrial.com/blog/best-llm-observability-platforms-in-2026) and our[head-to-head Sentrial vs Langfuse breakdown](https://www.sentrial.com/blog/sentrial-vs-langfuse-honest-comparison-2026) go deeper on specific tradeoffs. For what production agent monitoring requires beyond tracing, see our[agentic AI observability guide](https://www.sentrial.com/blog/best-practices-for-agentic-ai-observability-in-production) .


---


## FAQ


**Is Langfuse good for production AI agent observability and tracing?**


Langfuse is a strong choice for tracing and prompt management, especially for teams that want framework-agnostic instrumentation via OpenTelemetry. It captures session-level traces with inputs, outputs, latency, and token costs, and integrates with LangChain, LangGraph, and custom Python. Where it falls short in production is behavioral failure detection: it doesn't ship built-in classifiers for hallucination, bad tool calls, or goal abandonment, and its evaluation layer depends on sampling rather than classifying every run. For teams that need to catch silent failures at scale, tracing alone isn't sufficient.


**Does Langfuse support LLM-as-a-judge evals, online and offline?**


Langfuse supports both LLM-as-a-judge and code-based evaluators, and can run them online against production traces or offline against datasets. The mechanics work. The practical limitation is that online production eval uses sampling: evaluation rules define targets, filters, and rates rather than covering every run. Each failure mode you want to detect also requires manual eval configuration; Langfuse doesn't ship pre-built classifiers for common agent failure types.


**What are common limitations or criticisms of Langfuse in production?**


The most consistent patterns we see from teams that switch: Langfuse shows you logs but doesn't give you much to work with on top of them. Teams end up building eval logic themselves for each failure mode they care about, which is significant ongoing maintenance. Sampling means low-frequency failures stay invisible. And the alerting pathway is currently scoped to prompt version events, not evaluated behavioral failures, so production spikes require manual investigation rather than automated notification. Teams with light production traffic and offline eval workflows hit these limits less often.


**Does Langfuse work with OpenTelemetry and is it framework-agnostic?**


Yes. Langfuse is OpenTelemetry-native and accepts traces on the OTLP endpoint, which makes it framework-agnostic by design. It supports OpenAI, LangChain, LangGraph, LiteLLM, and custom Python instrumentation. One practical note: keeping LLM agent traces cleanly separated from unrelated OTel traffic in a shared infrastructure environment requires trace-level attribute propagation configuration, which can involve platform engineering work before the filtered views are fully actionable.


**Can Langfuse send Slack alerts and how deep is its alerting support?**


Langfuse has Slack integration, but it's currently scoped to prompt version change notifications rather than production evaluation failures. Score-based alerting, triggered when a hallucination rate or tool failure rate crosses a threshold, is on the roadmap but requires infrastructure work and isn't yet available. For teams that need to be paged when behavioral anomalies spike in production, Langfuse's current alerting depth isn't sufficient without building supplementary infrastructure around it.
