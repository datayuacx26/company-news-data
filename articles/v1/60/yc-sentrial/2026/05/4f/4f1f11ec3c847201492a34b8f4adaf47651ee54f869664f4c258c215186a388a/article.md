---
schema_version: "1.0.0"
document_id: "4f1f11ec3c847201492a34b8f4adaf47651ee54f869664f4c258c215186a388a"
company_key: "yc-sentrial"
company: "Sentrial"
source_id: "yc-sentrial-news-import-e72e91747750"
canonical_url: "https://www.sentrial.com/blog/datadog-pricing-the-bill-shock-nobody-explains"
published_at: "2026-05-26T05:07:26.303+00:00"
first_seen_at: "2026-07-25T23:34:43.014509+00:00"
fetched_at: "2026-07-28T22:12:43.834196+00:00"
content_hash: "sha256:065f0c26e567667f5f231ebaa1513bbe1a6a7126d4724a4b735b92a32fdbc8c6"
---

# Datadog's AI Agent Pricing Is More Complicated Than It Looks

Most teams don't get surprised by Datadog pricing because the rates are hidden. They get surprised because several meters run at the same time, and AI agent workloads activate combinations that infrastructure teams have never had to think about. Understanding what you'll actually pay means understanding which dimensions your workload touches and how they compound when they all fire at once.


## Datadog's Billing Model Isn't Unpredictable, You're Just Modeling One Dimension at a Time


Datadog bills independently across hosts, APM spans, log ingestion (per GB), indexed log events (per million), retention windows, and add-on products like LLM Observability. No single line item is the problem. The problem is that real workloads activate several of these simultaneously, and the mental model most teams use, budgeting one product at a time and summing them up, misses how the meters stack.


Datadog's pricing pages are organized by product, not by workload type. That structure makes sense for Datadog, but it means an engineering team estimating cost is almost always modeling one dimension at a time. In practice the compounding is non-linear, especially at scale.


AI agent workloads make this worse. A single LLM call generates traces for the API request, token streaming, embedding lookups, vector database queries, and response parsing,[8 to 15 spans per request compared to 2 to 3 for a typical API endpoint](https://oneuptime.com/blog/post/2026-03-07-ai-agents-breaking-observability-budget/view) . Agent loops compound this further, producing 40 to 75 spans per user interaction. That volume hits APM ingestion, indexed spans, and log ingestion at the same time.[Adding AI workload monitoring to existing Datadog setups has increased observability bills by 40 to 200%, depending on volume and instrumentation depth](https://oneuptime.com/blog/post/2026-04-01-ai-workload-observability-cost-crisis/view) .


If you're evaluating alternatives alongside this decision, our[Datadog Alternatives for AI Agent Monitoring](https://www.sentrial.com/blog/datadog-alternatives-that-catch-agent-failures) article covers the tool landscape. This piece focuses on understanding what you're being charged for before you commit to anything.


## Six Meters Your Workload Probably Activates


Before you can estimate a bill, you need to map which meters your workload actually touches. Here are the six primary dimensions, each billed independently.


**1. Infrastructure hosts.** Billed per host per month. The rate varies by plan (Pro vs Enterprise), and Datadog counts containers differently from traditional VMs. This is typically the baseline for any Datadog deployment.


**2. APM spans (ingested and indexed).** APM is often billed per host, but span ingestion and indexing are separate meters.[Indexed spans cost $1.70 per million spans per month with 15-day retention, and $2.50 per million spans per month at 30-day retention](https://last9.io/blog/datadog-pricing-all-your-questions-answered/) . Ingested spans that aren't indexed are cheaper, but cheaper doesn't mean free, volume still accumulates.


**3. Log ingestion.** Charged per GB regardless of whether logs are indexed.[Datadog charges approximately $0.10 per GB to ingest logs](https://www.parseable.com/blog/datadog-log-management-cost) . This meter runs whether or not you ever query those logs.


**4. Indexed log events.** Charged per million events after ingestion.[Indexing logs with 15-day retention costs approximately $1.70 per GB per month](https://www.parseable.com/blog/datadog-log-management-cost) . This is the meter teams most often underestimate when moving from infrastructure to AI workloads, because AI agents produce structured logs at a much higher event rate than servers do.


**5. Retention windows.** Retention isn't a flat feature, it multiplies your indexed log and indexed span costs. Moving from 15-day to 30-day retention roughly doubles your indexing bill. For AI workloads where debugging often requires tracing back several days to find when a model behavior changed, the temptation to extend retention is real and expensive.


**6. LLM Observability.** Datadog's LLM Observability add-on bills per LLM span. This meter is newer relative to traditional APM, and it interacts with APM span billing in ways that aren't always obvious. Teams that instrument LLM calls through both APM and the LLM Observability add-on can find themselves paying for the same interaction twice across different meters.


Three additional meters that frequently get missed: custom metrics (where high-cardinality agent metadata causes cardinality explosions), indexed spans as distinct from ingested spans (teams sometimes assume indexing is included in ingestion pricing), and log rehydration from archive (pulling logs back from cold storage carries its own per-GB charge that doesn't appear in initial pricing estimates).


[Datadog's pricing layers multiple independent charges, hosts, log ingestion, log indexing, span indexing, metrics, and retention premiums, in a way that compounds aggressively as infrastructure scales](https://cubeapm.com/pricing-calculator/datadog/) .


## Here's What the Bill Actually Looks Like for AI Agent Traffic


The scenario modeling you'll find elsewhere covers infrastructure workloads. Here's what it looks like specifically for AI agent workloads across three volume tiers.


For this math, assume: 40 spans per user session (a conservative midpoint for an agent loop), 2 MB of log data per session, 15-day retention, and all logs indexed.


**Small: 10,000 agent sessions per month**


- •


Spans generated: 400,000 (0.4M)
- •


Indexed span cost at 15-day retention: roughly $0.68
- •


Log volume: 20 GB ingested ($2.00), 20 GB indexed (approximately $34.00)
- •


Estimated observability-layer cost before host and base fees: approximately $37 per month


At this volume the bill is manageable, and the indexed log cost already dominates. The span cost is low because you're under the million-span threshold.


**Medium: 100,000 agent sessions per month**


- •


Spans generated: 4,000,000 (4M)
- •


Indexed span cost at 15-day retention: $6.80
- •


Log volume: 200 GB ingested ($20.00), 200 GB indexed (approximately $340.00)
- •


Estimated observability-layer cost: approximately $367 per month


At this tier, the indexed log line becomes the dominant cost. If the team extends retention to 30 days to support debugging, the indexed log cost roughly doubles to $680, pushing total observability-layer cost past $700 per month, before host fees, base plan costs, or LLM Observability add-on billing.


**Large: 1,000,000 agent sessions per month**


- •


Spans generated: 40,000,000 (40M)
- •


Indexed span cost at 15-day retention: $68.00
- •


Log volume: 2,000 GB ingested ($200.00), 2,000 GB indexed (approximately $3,400.00)
- •


Estimated observability-layer cost: approximately $3,668 per month at 15-day retention; over $7,000 at 30-day


At this scale, teams face a hard choice: sample aggressively to control the indexed log bill, or pay for full coverage. We'll get to why that choice isn't as neutral as it sounds.


**Estimation checklist before you commit:**


1. 1.


Count agent sessions per day (not monthly, daily variance matters for forecasting spikes)
2. 2.


Estimate spans per session by instrumenting a representative sample
3. 3.


Estimate log volume per session in MB by capturing one week of actual output
4. 4.


Decide on retention window (15 vs 30 day) based on your actual debugging workflow
5. 5.


Identify which Datadog products are already active in your account, APM, Infrastructure, custom metrics, and add those base costs before modeling the AI layer


## Sampling Sounds Like a Cost Lever. For AI Agents, It's a Blind Spot.


Here's the assumption most teams carry into this conversation: sampling is a neutral cost-optimization lever. For infrastructure monitoring, that's largely true. A server crash generates a status code, an exception trace, a timeout, signals that survive sampling because they're discrete events that repeat and cluster visibly.


AI agent failures don't work that way. Across 12 million logs we've analyzed at Sentrial, roughly 22% of issues were explicit tool call failures, the kind that generate a clean error or timeout. The remaining 78% were silent regressions: hallucinations, user frustration signals, agent forgetfulness or laziness. The user gets a wrong or useless answer and leaves. No error thrown. No alert fired. Nothing in your APM dashboard.


This holds up in external research too.[78% of AI failures are invisible, something went wrong but the user gave no overt indication there was a problem; invisible failures cluster into eight archetypes where 91% involve interactional dynamics](https://arxiv.org/pdf/2603.15423) .[AI agents fail silently by returning incomplete answers, freezing on slow APIs, or burning tokens calling the same tool repeatedly; the agent appears to work but the output is wrong, late, or expensive](https://dev.to/aws/why-ai-agents-fail-3-failure-modes-that-cost-you-tokens-and-time-1flb) .


Full log coverage at Datadog's indexing rates gets expensive fast at scale, as the scenario math above shows. Sampling cuts that cost. But for AI agents, the failure signal is almost always semantic and behavioral, it's in the content of a response, not in a status code, so sampled coverage systematically discards logs from exactly the category of interaction you most need to examine.


One example: a Series B finance company used an agent to automate vendor quoting from PDFs. The agent appeared to work, it returned quotes, the numbers varied by vendor, nothing crashed. What we found was that the agent wasn't actually ingesting the PDF. It was hallucinating quote values based on context from the RFP and other customer data, not the document itself. That failure lived entirely in the semantic content of the output. A sampled log pipeline had a high probability of never capturing the specific interactions that would have revealed the pattern.


[AI workloads generate non-deterministic failures that traditional sampling cannot capture; full-fidelity telemetry is now a requirement, not a luxury, for monitoring AI agents](https://tfir.io/ai-observability-sampling-groundcover/) .


This is where Sentrial's design logic differs from Datadog's. We built Sentrial to classify every interaction using post-trained models fine-tuned on each customer's actual agent traffic, not a sample, not a generic eval use. The goal is catching the 78% of failures that don't generate errors, which requires seeing all the logs, not a statistically representative slice. To be clear about where Datadog still excels: for infrastructure correlation, incident management, and unified visibility across your full stack, it remains a strong platform. The mismatch is specific, its billing model creates a financial incentive to sample exactly the telemetry that AI agent failure detection requires in full.


For a deeper look at why traditional observability tools miss this failure category, our article on[LLM Observability: Silent Failures Nobody Warns You About](https://www.sentrial.com/blog/llm-observability-silent-failures-nobody-warns-you-about) covers the underlying mechanics.


## Run a Structured Pilot Before You're Locked Into Volume Pricing


The cleanest way to avoid a billing surprise is to profile your actual workload before you're committed to volume-based pricing.


**Step 1: Instrument a representative slice.** Pick one agent type and one week of real traffic. Don't model from assumptions, capture actual GB ingested per day, actual indexed event counts, and actual spans per LLM call. AI agent span counts vary significantly by agent design; the only way to know your number is to measure it.


**Step 2: Identify your retention requirement.** Ask your engineering team how far back they typically need to trace a behavioral regression. If the honest answer is two to three weeks, 15-day retention will leave you pulling from archive (with rehydration costs) regularly. If the answer is a few days, 15-day is probably fine.


**Step 3: Add the base costs last.** Host fees and base plan costs are the most visible numbers in Datadog's pricing, so they're usually what teams budget first. That's backwards. For AI workloads, the indexed log and span costs are what scale with usage. Model those first, then add the fixed-ish base.


**Step 4: Make the sampling decision explicitly.** Don't let sampling happen by default because of cost pressure. Make a deliberate choice about what percentage of logs you're indexing and what failure category you're accepting as invisible.


On the annual vs on-demand question: on-demand pricing protects against overcommitting before you've profiled your actual volume. Annual discounts are real, but AI agent traffic can spike unpredictably, a new product launch, a viral moment, a change in agent design that multiplies span counts, and overage rates above committed volume can erode the annual discount quickly. We'd suggest profiling for at least two months before committing to an annual volume contract.


If your primary goal is catching silent AI agent failures and you need full log coverage to do it, Sentrial's session-based pricing is designed to be forecastable without modeling host count, cardinality, or indexing tiers, and one Fortune 1000 customer running supply chain, HR, and marketing agents cut their agent error rate from 20% to under 10% in a single week once they had full-coverage visibility. You can see current rates at[Datadog's official pricing page](https://www.datadoghq.com/pricing/) and use the estimation checklist above to pressure-test your assumptions before signing.


For teams evaluating the broader tool landscape alongside this decision, our[AI Agent Monitoring for Startups](https://www.sentrial.com/blog/ai-agent-monitoring-for-startups-2026-guide) guide covers the tradeoffs across monitoring approaches, and our[Best LLM Observability Platforms in 2026](https://www.sentrial.com/blog/best-llm-observability-platforms-in-2026) article ranks options by debugging depth.


---


## Frequently Asked Questions


**How do retention windows affect Datadog pricing for logs?**


Retention windows multiply your indexed log cost directly. At list price, indexing logs with 15-day retention costs approximately $1.70 per GB per month. Extending to 30-day retention roughly doubles that indexed log cost. For AI agent workloads, where debugging a behavioral regression often requires tracing back two or three weeks, this creates a real trade-off: either pay for longer retention or accept that you'll need to rehydrate logs from archive storage, which carries its own per-GB charge that doesn't appear in most initial cost estimates.


**What is the difference between log ingestion and indexed logs, and why does it matter for pricing?**


Ingestion and indexing are two separate meters that bill independently. Datadog charges approximately $0.10 per GB to ingest logs regardless of what happens to them next. Indexing, which makes logs searchable and queryable, costs approximately $1.70 per GB per month at 15-day retention. A log that's ingested but not indexed costs the ingestion rate only. The practical implication: teams that route all logs through ingestion but index only a fraction are still paying the ingestion fee on every byte, plus the indexing fee on the subset they query. The decision about what to index is also the decision about what failure evidence you can actually investigate.


**How do you calculate Datadog log costs from your usage, for example, GB per day to monthly events?**


Start with your actual GB per day from a representative week, then multiply by 30 for a monthly estimate. To convert GB to approximate event counts, a useful rule of thumb for structured JSON logs is roughly 500 to 1,000 events per MB depending on average record size, but measure your own logs, AI agent outputs vary widely. Once you have monthly GB and approximate event counts, apply the ingestion rate ($0.10/GB) and the indexing rate ($1.70/GB at 15-day retention) separately, then add the span costs using the indexed span rate ($1.70/million at 15-day retention). The scenario math in the "Here's What the Bill Actually Looks Like for AI Agent Traffic" section above shows this calculation worked through three volume tiers.


**What is a cost-effective alternative to Datadog for AI agent monitoring?**


The right answer depends on what you need. Datadog is strongest when you need unified infrastructure and AI visibility in one platform and have the volume forecasting confidence to commit to annual pricing. If your primary need is catching silent AI agent failures, hallucinations, user frustration, agent forgetfulness, wrong answers that don't throw errors, purpose-built platforms like[Sentrial](https://www.sentrial.com/) are designed specifically for that problem, with session-based pricing that scales predictably without modeling host count, cardinality tiers, or indexing windows. Our[Datadog Alternatives for AI Agent Monitoring](https://www.sentrial.com/blog/datadog-alternatives-that-catch-agent-failures) article covers the full comparison.
