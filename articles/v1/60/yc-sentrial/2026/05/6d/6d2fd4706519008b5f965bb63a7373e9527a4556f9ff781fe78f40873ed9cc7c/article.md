---
schema_version: "1.0.0"
document_id: "6d2fd4706519008b5f965bb63a7373e9527a4556f9ff781fe78f40873ed9cc7c"
company_key: "yc-sentrial"
company: "Sentrial"
source_id: "yc-sentrial-news-import-e72e91747750"
canonical_url: "https://www.sentrial.com/blog/langfuse-pricing-why-your-bill-wont-match-your-estimate"
published_at: "2026-05-29T17:53:44.916+00:00"
first_seen_at: "2026-07-25T23:34:43.014509+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:11ee4f92252dbda9cb833471380af144cd33c055603931ee12a972494a04b1e1"
---

# Langfuse Pricing in 2026: What Agent Teams Actually Pay

Most teams evaluating Langfuse start by comparing plan tiers and monthly prices. That's the wrong place to start. The real question is whether your unit consumption is predictable once agents hit real traffic, and whether your retention window is long enough to actually debug the failures that matter. For both questions, the answer is usually worse than teams expect going in.


## The Plan Breakdown Is Simpler Than the Feature Matrices Make It Look


Hobby works for solo prototyping and pre-production experimentation. Core handles early-production teams running light, single-step LLM traffic on a tight budget. Pro is the right default for any team running multi-step agents in production who need 90-day forensic retention. Self-hosting flips the math entirely for organizations with hard data residency requirements or volume high enough that cloud TCO becomes painful.


The thing almost every pricing guide skips: Langfuse bills on units, not conversations. For agentic workloads with many spans, tool calls, and evaluation artifacts per run, unit consumption scales much faster than your conversation count suggests. We'll work through the math below.


## How We Looked at This


We evaluated Langfuse across five criteria: published plan pricing accuracy, unit-billing mechanics and how well they can be forecasted, data retention windows relative to realistic incident detection timelines, operational limits including rate limits and ingestion throughput, and self-host total cost of ownership beyond the licensing fee.


The lens we applied throughout is agent-specific. We evaluated Langfuse specifically for teams running multi-step AI agents where each run generates dozens of spans, tool calls, and evaluation scores, because that's exactly where unit consumption forecasting breaks down and where the gap between plan cost and actual cost shows up.


Primary sources were Langfuse's official pricing page, billing documentation, and unit definition documentation. Third-party content was used for corroboration only. One honest limitation: Enterprise pricing is custom and we don't have a signed contract to verify, the ballpark figures here come from published guidance only.


## What Each Tier Actually Gets You


### Hobby, Free


**Best for:** Solo developers, pre-production experimentation, learning the platform.


Hobby gives you 50,000 units per month at no cost. Data retention is limited to 30 days, and there are no governance features, no SSO, RBAC, SCIM, or audit logs. Rate limits apply, which is fine for low-volume experimentation but will surface quickly if you start load testing anything resembling a real agent workload.


**Honest warning:** The 30-day retention window sounds generous until your agent regresses in week five and you have no trace history to replay. Hobby is a prototyping tier, not a production tier.


### Core, ~$29/month


**Best for:** Early-production teams with simple LLM calls, modest traffic, and strict budget constraints.


Core includes approximately 100,000 units per month, with overages billed at around $8 per additional 100,000 units. Data retention extends to 30 days. You get basic collaboration features but no enterprise governance add-ons.


**Honest warning:** The overage rate is manageable for simple LLM call patterns, but a multi-step agent with 15 spans per run will burn through 100,000 units in roughly 6,500 runs, less than a week of modest production traffic for many teams. And 30-day retention is a real constraint: incidents that started 35 days ago are simply gone.


### Pro, ~$199/month


**Best for:** Production AI agent teams who need 90-day forensic retention, higher throughput limits, and volume overage discounts.


Pro raises the included unit allotment substantially, extends retention to 90 days, reduces overage rates relative to Core, and adds team collaboration features. This is the tier where Langfuse becomes a credible production tool rather than a monitoring supplement.


**Honest warning:** Pro solves the retention cliff that Core creates, but it doesn't change what Langfuse is built to surface, logs and traces. If your agents are failing through hallucinations, user frustration, or forgetfulness rather than thrown errors, 90 days of trace history still won't tell you that. You'll see the run completed. You won't see that the output was wrong.


### Enterprise, ~$2,499/month or custom


**Best for:** Large organizations with compliance requirements, multi-team governance needs, or enterprise SLA requirements.


Enterprise adds SSO, RBAC, SCIM provisioning, audit logs, dedicated support, and data retention extending to three years. Pricing is negotiated; the $2,499/month figure appears in published guidance as a starting point, not a fixed rate.


**Honest warning:** Enterprise pricing is opaque by design. If governance features are the primary driver, confirm exactly which add-ons, SSO, SCIM, audit logs, are bundled versus separately priced before signing. Some teams discover in contract negotiations that features they assumed were included require additional line items.


## Unit Billing Is Where Agent Teams Get Surprised


A Langfuse unit is not a conversation and not a request. Units are counted at the observation level: each trace, span, generation, score, and event logged to the platform counts toward your monthly total. A single agent run that produces a top-level trace, 15 intermediate spans, 3 tool call observations, and 2 evaluation scores produces approximately 21 units, not 1.


That's what causes teams to underestimate consumption by an order of magnitude.[Glassbrain.dev](https://glassbrain.dev/blog/langfuse-pricing) notes that a complex agent with tool calls, retrieval, reranking, and multi-step reasoning can easily generate ten to thirty observations per user request.[OneUptime](https://oneuptime.com/blog/post/2026-03-07-ai-agents-breaking-observability-budget/view) puts it more precisely: an agent that reasons through five steps generates 40 to 75 spans per user interaction, multiplying unit consumption 8 to 15 times compared to traditional API calls.


Here's what that looks like at three scales:


Monthly Agent Runs Spans Per Run Eval Scores Per Run Units Per Run Total Units Estimated Tier


1,000 10 2 12 12,000 Hobby


10,000 15 3 18 180,000 Core + overage


100,000 15 3 18 1,800,000 Pro (verify overage)


At 10,000 runs per month with 15 spans each, you're consuming 180,000 units, 80,000 over Core's included allotment, generating roughly $6.40/month in overages at the $8/100k rate. That's not catastrophic. But if your spans per run are closer to 30 (retrieval, reranking, chain-of-thought steps), you're at 360,000 units, and the math changes fast.


[Pydantic's observability pricing comparison](https://pydantic.dev/articles/ai-observability-pricing-comparison) confirms that a 5 million span workload requires roughly 6 million units, which can consume a plan's included allotment in under a day of moderate traffic. It also notes that a 10-span trace with one evaluation score costs 12 billable units, consistent with the per-run math above.


Instrumentation decisions matter a lot here.[Checkthat.ai](https://checkthat.ai/brands/langfuse/pricing) reports that unit consumption varies 3 to 5 times based on instrumentation choices alone, logging every intermediate step costs much more than logging only the top-level trace, with 50 to 90 percent cost reductions possible through optimized instrumentation. The tradeoff is visibility: the spans you don't log are the spans you can't replay when something goes wrong.


One additional wrinkle for teams using reasoning models like OpenAI o1: token and cost tracking can't always be inferred when usage fields aren't ingested. The cost attribution data you're relying on to correlate trace cost with agent behavior may be incomplete, which undermines the business case for granular per-run cost tracking on those models specifically.


The practical recommendation: before choosing a tier, estimate your spans per run by logging 100 test runs against a staging environment and measuring actual unit consumption. Don't plan on conversation count.


## Self-Hosting Is Free in Licensing, Not in Practice


Langfuse's open source version is genuinely free to self-host. No licensing fee, and Langfuse's documentation doesn't claim meaningful feature limitations relative to the cloud offering. For teams with hard data residency requirements, this is a real option worth evaluating seriously.


What most analyses miss is what "free" actually costs to operate. Infrastructure for a containerized Langfuse deployment, database, object storage, compute, and networking for modest agent workloads, runs $200 to $800 monthly according to[TrueFoundry's analysis of self-hosted observability stacks](https://www.truefoundry.com/blog/litellm-pricing-guide) . That's before engineering time.


Engineering time is the larger cost.[SitePoint's 2026 self-hosted LLM cost analysis](https://www.sitepoint.com/self-hosted-llm-costs-2026/) estimates that for a production system, allocating 20 to 30 percent of a senior engineer's time translates to roughly $3,000 to $6,000 per month in staffing cost. At Series A salaries, even 0.25 FTE of ongoing ops work exceeds Pro's annual cost in the first quarter.


Self-hosting makes sense when data residency is a hard blocker that legal or compliance won't negotiate around, when you have existing infrastructure team bandwidth that isn't fully utilized, or when you're at volume where cloud costs clearly exceed self-hosted TCO. It doesn't make sense when your team is already stretched on core product work and monitoring uptime becomes your problem. Langfuse ships frequently, and the upgrade cycle is yours to own. An unpatched version in production is not the same risk profile as a vendor-maintained SaaS.


Self-hosting is a legitimate choice for the right organization. It's not a cost hack for teams trying to avoid paying for Pro.


## How Langfuse Stacks Up Against the Alternatives


Tool Starting Price Billing Model Data Retention Silent Failure Detection Self-Host Option Best For


Langfuse Hobby Free Unit-based 30 days No Yes (OSS) Solo prototyping


Langfuse Core ~$29/mo Unit-based + overage 30 days No Yes (OSS) Early production, simple LLM calls


Langfuse Pro ~$199/mo Unit-based + overage 90 days No Yes (OSS) Production agents needing forensic retention


LangSmith Free tier; paid from ~$39/mo Trace-based Varies by plan Limited (rule-based) No Teams deep in LangChain ecosystem


Arize Phoenix Free OSS; cloud pricing varies Event-based Varies by plan Partial (drift detection) Yes (OSS) ML + LLM teams needing unified eval


Sentrial Usage-based; contact for current pricing Usage-based Configurable Yes (post-trained classifiers, full coverage) No Production agent teams where silent failures are the primary risk


The column that matters most for agent teams is silent failure detection.[LLMs hallucinate in 3 to 27 percent of responses depending on use case](https://suprmind.ai/hub/insights/ai-hallucination-statistics-research-report-2026/) , and every one of those responses returns a clean HTTP 200 to standard monitoring. Trace retention tells you what the agent did. It doesn't tell you whether what it did was correct.


## Sentrial: Built for Teams Whose Agents Fail Without Crashing


Langfuse tells you what your agent did. We tell you when what your agent did was wrong, hallucinations, user frustration, silent forgetfulness, even when no error was thrown and no alert fired.


**Best for:** Engineering teams running production AI agents where silent failures are the primary risk, not latency or uptime.


**Key features:**


- •


Post-trained classifiers fine-tuned on your specific agent traffic, not generic LLM-as-judge evaluation, which in our experience produces lower accuracy as agent complexity grows
- •


Full log coverage: we classify every interaction, not a sample, because[sampling is the wrong tradeoff when 78% of the failures you're trying to catch are silent](https://www.sentrial.com/blog/ai-for-observability-your-agent-isnt-crashing-its-lying)
- •


Custom classifier instantiation in under a minute: check 3 to 4 example logs, deploy a fine-tuned classifier for whatever failure mode your team needs to track, jailbreaks, bad tool calls, agent forgetfulness, domain-specific regressions
- •


Replay and fork from any intermediate step in an agent run for precise root cause isolation
- •


Real-time Slack alerts with source-code-level failure pinpointing and fix suggestions


Across 12 million logs we've analyzed, around 22 percent of agent issues were explicit tool call failures, the kind that stop the run and produce a visible error. The remaining 78 percent were silent: hallucinations first, user frustration second, agent forgetfulness or laziness third. The majority of agent failures don't produce a clean error or timeout. The user gets a wrong or unhelpful answer and leaves.


Setup takes five lines of instrumentation code via OpenTelemetry, LangChain, LangGraph, or custom Python agents. We use OpenTelemetry for initial log capture, then run post-trained classification and clustering on top of that data, so you get traces and behavioral intelligence from the same integration.


**Pricing:** Usage-based.[See current pricing at sentrial.com](https://www.sentrial.com/) .


**Pros:**


- •


Only platform that classifies every log rather than sampling
- •


Custom classifier deployment in minutes, not weeks
- •


Catches the silent failure categories that trace tools structurally miss
- •


Source-code-level debugging with suggested fixes reduces mean time to resolution significantly


**Cons:**


- •


Purpose-built for agent failure detection, not general-purpose LLM tracing. If you need prompt management, dataset versioning, or experiment tracking, you'll likely run Sentrial alongside Langfuse rather than replacing it
- •


Not the right choice if your primary workflow is pre-production eval benchmarking rather than production monitoring
- •


Younger platform than Langfuse; ecosystem integrations are growing but narrower


The use case where Langfuse and Sentrial coexist naturally: Langfuse for trace storage, prompt management, and experiment tracking; Sentrial for production behavioral monitoring and silent failure detection. Several of our customers, including those who came from Langfuse, run both. Langfuse was popular early in LLM reliability, but as agents scaled with tool calls and multi-step reasoning, teams needed semantic analysis on top of traces that trace storage alone couldn't provide.


## How to Pick a Tier Based on What You're Actually Running


**Prototyping or pre-production:** Start on Hobby. No cost, no commitment, and the 50,000 unit limit is generous for anything not receiving real user traffic. The 30-day retention window doesn't matter yet.


**Early production with simple LLM calls and a tight budget:** Core at $29/month is the right call if your per-run span count is low (under 10 observations per run) and you don't need to debug incidents older than 30 days. Monitor your unit consumption weekly for the first month.


**Production agents with multi-step workflows:** Pro is the correct default. The 90-day retention window is the meaningful upgrade, not the higher unit allotment. If an agent regression takes three weeks to surface in user feedback (which is common for subtle hallucinations), you need six weeks of data minimum to establish a before/after baseline. Core's 30-day window makes that forensics impossible.


**Enterprise governance requirements:** Enterprise tier or self-host, depending on whether data residency or compliance governance features are the primary driver.


**Silent failure detection as a primary concern:** This is where the Langfuse tier choice becomes secondary.[An agent can have 99 percent uptime but still fail to follow user intent](https://www.langchain.com/articles/llm-monitoring-observability) ; traditional monitoring shows healthy latency and low error rates while users report the agent confidently provided incorrect information. If that's the failure mode you're trying to catch, the relevant question isn't which Langfuse plan to buy, it's whether trace storage alone is sufficient monitoring for your production environment.


The core insight here: **the cheapest tier is the wrong optimization if your retention window is shorter than your regression detection lag.** If a silent failure started 45 days ago and you're on Core with 30-day retention, you have no data to debug it. Pick the lowest tier whose retention window covers your typical incident detection lag, then ask whether trace data is sufficient to catch the failures that actually affect your users.


For a deeper look at the broader LLM observability landscape, our[best LLM observability platforms comparison](https://www.sentrial.com/blog/best-llm-observability-platforms-in-2026) covers the full evaluation in detail.


## FAQ


**What are the alternatives to Langfuse?**


The main alternatives are LangSmith, Arize Phoenix, and Sentrial. LangSmith is the natural choice for teams already deep in the LangChain ecosystem and offers tight integration with LangGraph. Arize Phoenix is strong for teams that need unified ML and LLM evaluation, particularly drift detection. Sentrial is purpose-built for production agent failure detection, specifically the silent failures (hallucinations, user frustration, forgetfulness) that trace tools don't surface. If you want a full comparison, our[Sentrial vs. Langfuse breakdown](https://www.sentrial.com/blog/sentrial-vs-langfuse-honest-comparison-2026) covers the functional gaps directly.


**What are the key features of Langfuse?**


Langfuse's core strengths are session-level trace capture and visualization, prompt management and versioning, dataset management for offline evaluation, and an SDK that integrates with most major LLM frameworks. It gives you a structured view of what your agent did at each step, inputs, outputs, latency, and token cost per observation. Where it's limited for production agent teams is on the analysis layer: it stores and displays traces well, but doesn't semantically classify whether those traces represent successful agent behavior. See the unit billing section above for what you're paying for, and the comparison table for where the gaps sit.


**Is LangSmith free and open source?**


LangSmith has a free tier but is not open source. The free tier includes a limited number of traces per month and is useful for development and light testing. LangGraph, the agent orchestration framework from the same team, is open source. The distinction matters if you're evaluating options on licensing grounds: LangSmith cloud is proprietary, and there's no self-hosted LangSmith equivalent the way there is for Langfuse.


**How do I get a Langfuse secret key?**


After creating a Langfuse account, work through to your project settings and select the API Keys section. You'll generate a public key and secret key pair there. The secret key is shown once at creation time, store it immediately in your secrets manager or environment variables. If you lose it, you'll need to rotate and generate a new pair. For self-hosted deployments, the same key management UI is available in your self-hosted instance; key storage security is then your own responsibility rather than Langfuse's.
