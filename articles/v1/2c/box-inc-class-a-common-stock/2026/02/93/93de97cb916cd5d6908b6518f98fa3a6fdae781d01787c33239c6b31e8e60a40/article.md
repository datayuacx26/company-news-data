---
schema_version: "1.0.0"
document_id: "93de97cb916cd5d6908b6518f98fa3a6fdae781d01787c33239c6b31e8e60a40"
company_key: "box-inc-class-a-common-stock"
company: "Box Inc."
source_id: "box-inc-class-a-common-stock-rss-6b6ba587c738"
canonical_url: "https://medium.com/box-tech-blog/from-service-metrics-to-user-reality-building-meaningful-user-availability-at-box-8d3cc31190e1"
published_at: "2026-02-24T21:49:32+00:00"
first_seen_at: "2026-07-20T04:36:04.074136+00:00"
fetched_at: "2026-07-28T22:19:05.004303+00:00"
content_hash: "sha256:cc4b600d0b82c91edfd7152eaea5ffdbb0bc981bda9f3797ea35e001b6650997"
---

# From Service Metrics to User Reality: Building Meaningful User Availability at Box

# From Service Metrics to User Reality: Building Meaningful User Availability at Box


[Anuraag Shah](https://medium.com/@anuraagshah?source=post_page---byline--8d3cc31190e1---------------------------------------)


14 min read


·


Feb 24, 2026


--


## Introduction


At Box, one of our core values is to *blow our customers’ minds* — and high availability is table stakes for doing that in enterprise SaaS. But “high availability” measured how? We measured availability using metrics that answered the question our systems could easily answer: “What percentage of requests succeeded?” The harder question — “What fraction of users were able complete their intended actions?” — went largely unmeasured.


We measured availability using Critical Service Availability (CSA), a framework that calculated “critical minutes lost” when traffic throughput dropped below baseline. CSA was designed to answer a critical business question: when something goes wrong, how much impact did it cause? By measuring throughput degradation and translating it into minutes lost against a quarterly budget, CSA gave leadership a consistent, quantifiable signal they could act on.


But as Box’s feature set expanded and matured, and interactive workflows became central to how customers used the product, we needed to shift our reliability lens — and we started observing patterns that monitoring wasn’t designed to catch:


- **Latency degradations without throughput drop:** HTTP 200s with multi-second response times kept throughput charts stable, but users were abandoning sessions.
- **User-clustered failures:** Issues affecting a small percentage of requests but a large percentage of users — edge cases in specific workflows — fell below throughput thresholds while causing real pain.
- **Interactive traffic masked by automation:** High-volume integrations, bots, and sync jobs dominated the request-volume signal, making it harder to see problems affecting typical interactive users.


These weren’t flaws in our reliability program — they were new patterns emerging from how the product had grown. We needed a complementary approach: one that measured availability from the user’s perspective.


## A New Perspective: What Does “Available” Really Mean?


Consider: a service that’s technically accessible 99.99% of the time but is slow to load, has frequent errors, or fails to complete user workflows isn’t truly “available” in any meaningful sense. Our users don’t care about server health — they care about whether they can successfully upload their files, collaborate with their team, or access their content when they need it. This realization led us to explore meaningful User Availability (UA), inspired by[Google’s Meaningful Availability white paper](https://research.google/pubs/meaningful-availability/) . Instead of measuring what our systems were doing, we decided to measure what our users were actually experiencing.


## Defining User Availability


**UA answers one operational question: “In this minute, what fraction of active users had a good experience?”**


The formula is deceptively simple:


> UA = 100 × (1 − Impacted Users / Active Users)


Where, for each one-minute window:


- **Active User** : A distinct user who made at least one qualifying request during the minute
- **Impacted User** : An active user who experienced at least one “impacting” event — a server error, timeout, or response slow enough to be user-breaking


The key design principle is **one-user-one-vote** : each user counts exactly once per minute, regardless of request volume. A power user generating 1,000 API calls has the same weight as someone making a single upload. UA is intentionally stricter than request-based metrics. It doesn’t average away user pain — if one request out of twenty fails for a user, that user’s minute was bad. It surfaces latency as impact — a slow-but-successful response still degrades experience. And it exposes problems that request metrics hide — an issue affecting 2% of requests might impact 20% of users if those failures cluster on specific user patterns.


## Defining Impact: The Hard Part


Deciding what counts as “impact” is where theory meets reality. We needed rules that were:


- **Defensible:** based on observable signals, not intuition
- **Consistent:** applied uniformly across services
- **Tunable:** adjustable as we learned more


## Error/exception Impact


Press enter or click to view image in full size


Press enter or click to view image in full size


## Latency Impact


Errors alone miss “slow but successful” — often the bulk of user pain. We establish an[Apdex](https://en.wikipedia.org/wiki/Apdex) **-inspired model** to classify latency:


Press enter or click to view image in full size


Press enter or click to view image in full size


**How we set T (the latency threshold):**


We use two methods to determine what constitutes an acceptable latency threshold:


1. **Conservative global baseline:** Start with T = 2s (so Frustrated > 8s) as a defensible default
2. **Percentile-derived per-workflow thresholds:** Analyze historical p99 and p99.9 latency distributions during stable periods to establish what “normal slow” looks like for each critical workflow


This hybrid approach grounds impact in actual observed behavior (percentiles) while maintaining a consistent, interpretable threshold definition (Apdex 4×T multiplier). Over time, we evolved from a single global T to workflow-specific thresholds — uploads tolerate more latency than API calls.


## From Prototype to Production: Building the Aggregation Pipeline


Press enter or click to view image in full size


Press enter or click to view image in full size


Building UA wasn’t a waterfall process — it was an iterative loop. The diagram above captures our workflow, split into two phases: a **development loop** on the left and the **production system** on the right.


In the development loop, **Query Updates** are changes to how UA is computed — adjusting thresholds, adding signals, fixing edge cases. Those changes go through **Validate Query Changes** (unit tests, schema checks), then **Visualize** to inspect the output. If the signal doesn’t match reality, iterate. Once validated, the logic gets promoted into the **3-level aggregation UA Query** — our production architecture, optimized for scale — which powers **Realtime** alerting and **ETL** for batch reporting.


## Validation: Does the Signal Match Reality?


We ran this development loop hundreds of times before UA was production-ready. We prototyped with manual BigQuery queries against tier-1 services — no infrastructure to provision, fast iteration, immediate access to real production data.


The validation questions: Does the data exist? Is the math computable at scale? Does the signal correlate with user pain?


That last question was the real test. We backtested UA against historical incidents — outages we knew had caused user pain, support escalations, customer complaints. Would UA have detected them? How early? With what specificity?


Backtesting revealed both strengths and blind spots. UA caught “slow but successful” incidents that throughput-based metrics missed entirely — latency degradations that generated support tickets but never triggered alerts. It also exposed where our initial thresholds were either aggressive (false positives during normal load) or lenient (missing real degradations). Each failure sent us back through the loop until UA reliably detected the incidents we knew mattered.


## The Three-Level Aggregation Query


The core computation transforms raw request events into per-minute UA through three level nested aggregation levels:


Press enter or click to view image in full size


Press enter or click to view image in full size


The key design choices: **any-impact semantics** (one bad request ruins the user’s minute), **one-user-one-vote** (a power user counts the same as a casual user), and **separated error types** (responders need to know if they’re chasing errors vs latency).


## Demonstrative Example: How the Three Levels Work


Consider a single minute (minuteX) with the following activity:


**Setup:**


- **5 users** interact exclusively with Feature1 (10 requests each → 50 requests)
- **4 users** interact exclusively with Feature2 (10 requests each → 40 requests)
- **1 common user** interacts with both features (5 requests to Feature1, 5 requests to Feature2)


**Totals:**


- Feature1: 6 users (5 exclusive + 1 common), 55 requests (50 + 5)
- Feature2: 5 users (4 exclusive + 1 common), 45 requests (40 + 5)
- Combined: 10 distinct users, 100 requests


Press enter or click to view image in full size


Press enter or click to view image in full size


Press enter or click to view image in full size


Press enter or click to view image in full size


**The punchline:** Request availability is 99%. UA is 90%. Same data, different lens. If this were a real incident affecting 10% of users, request-based alerting might not fire — but UA would.


## Real Production Data Output Example:


Here’s actual output from our production UA pipeline:


Press enter or click to view image in full size


Press enter or click to view image in full size


**Reading a single row (2026–01–14 23:03:00 UTC):**


```text
Users:     418,434 active  →  574 impacted  →  UA = 99.86%  Requests:  4,532,751 total →  958 errors   →  Request Availability = 99.98%
```


The gap tells the story: request availability says 99.98% (nearly perfect), but UA says 99.86% (574 users had a bad minute). Both numbers are correct — they’re answering different questions.


**Breaking down the 574 impacted users:**


- ` userCodeError` : 16 users hit 5xx errors
- ` userLatencyError` : 563 users hit latency thresholds
- ` commonUserError` : 5 users hit both


Latency is the dominant source of user pain — **35x more users** were impacted by slowness than by errors. Traditional 5xx monitoring would show green dashboards while 563 users waited through frustrating delays. Those requests eventually succeeded, so they don’t register as failures in request metrics. But the users were impacted, and UA surfaces that.


## Transforming UA into a True Interactive User Signal


Traffic is power-law distributed: a few users, integrations, and automations can generate huge volume. Even with per-user aggregation, we don’t want UA dominated by “users” that are really machines (service accounts, bots, sync jobs, retry storms). We address this through **interactive-eligibility filtering** :


**Interactive-only eligibility (keep automation from masquerading as users)**


User-weighting helps, but doesn’t prevent machine actors — from skewing the signal. We filter non-human traffic through a small set of explicit, versioned eligibility rules:


- **Rate gate:** Exclude identities exceeding X requests/minute. Legitimate human interaction rarely sustains this rate; most traffic above this threshold is automation.
- **Identity gate:** Exclude known service accounts, test tokens, and requests without reliable user attribution.
- **Client gate:** Exclude machine-to-machine patterns identified via user-agent or registered application ID.
- **Endpoint gate:** Optionally exclude endpoints that are primarily background activity (health checks, sync operations) rather than user-visible workflows.


**The goal isn’t perfect classification — it’s a stable, human-representative signal.** We accept some false positives (humans occasionally filtered) and false negatives (bots occasionally counted) in exchange for operational simplicity.


We monitor two key health indicators:


- **Coverage:** What fraction of traffic/users pass eligibility filters? Unexpected drops suggest over-filtering of human users.
- **Incident correlation:** Do known outages that affected humans show up in UA? If not, our filters may be hiding real problems.


We review coverage quarterly and investigate any significant drift and adjust thresholds when we find blind spots to ensure UA remains representative of actual user experience.


## Key architecture decision records


Before implementing UA at scale, we made several foundational architectural choices. Each involved tradeoffs — we document them here as decision records to explain the rationale and constraints that shaped the system.


## Decision 1: Server-Side Events Over Client Telemetry


**Trade-off:** Client-side telemetry is the gold standard for user experience — it captures the “last mile” of user experience (network latency to the client, rendering time) that servers never see. But client instrumentation required coordinating changes across multiple teams, platforms, and release cycles — a multi-quarter effort before we’d have usable data.


**Our choice:** Use server-side request logs as the primary signal source. This gave us immediate coverage across all services without cross-team coordination, at the cost of missing “last mile” experience.


**What we’d do differently:** Start client instrumentation earlier, in parallel with server-side UA. We now have user experience visibility gaps that will take quarters to close.


## Decision 2: Per-Minute Granularity


**Trade-off:** Finer granularity (sub-minute) enables faster detection but increases computational cost and alert noise. Coarser granularity (5+ minutes) reduces cost but delays incident detection unacceptably.


**Our choice:** One-minute windows. Fast enough for operational alerting, coarse enough to be stable, aligned with how responders think (“problems started around 10:42”).


**What we learned:** One minute is near-optimal for incident detection but too noisy for executive reporting. We aggregate to hourly/daily for leadership dashboards.


## Decision 3: Dual Pipeline Architecture


We run two parallel pipelines because “fast” and “accurate” have different requirements:


**Real-time pipeline:** A custom service queries the data warehouse every minute, computing near-real-time UA for alerting. Accepts some inaccuracy from late-arriving data in exchange for speed.


**Batch pipeline:** Scheduled jobs reprocess with full data completeness for historical reporting. Produces the authoritative record.


**Why both?** Real-time UA told us an incident was happening; batch UA told us the true impact for postmortems and SLA reporting. Attempting to serve both needs from one pipeline would require compromises that degraded both use cases.


Press enter or click to view image in full size


Press enter or click to view image in full size


## What Didn’t Work


- **Our initial latency thresholds needed iteration** . We started with a uniform “frustrated” threshold across all workflows based on industry guidance. It took three months of tuning — reducing false positives from legitimately slow bulk operations and catching false negatives from latency regressions in fast-path APIs — before we arrived at workflow-specific thresholds. Lesson: prototype with real incident data before committing to threshold values.
- **We underestimated pipeline reliability requirements.** UA depends on data pipelines that must themselves be highly available. Early on, we had incidents where data lag caused UA to report phantom degradations, sending responders on wild goose chases. We eventually invested ~30% of engineering effort on pipeline monitoring, alerting, and graceful degradation — far more than initially planned.
- **Cross-team alignment required as much investment as the technical work** . Getting service teams to adopt UA-based alerting meant building shared understanding and trust. Teams naturally had questions about a new metric computed by pipelines outside their ownership. We invested time in explaining the methodology, walking through incident backtests together, and incorporating their feedback. Successful adoption depends on partnership, not just code.
- **Our eligibility filters had blind spots.** We encountered edge cases where legitimate enterprise users were filtered out by rate gates (power users doing bulk operations) and cases where sophisticated bots passed all filters. Rather than pursuing perfect classification, we focused on achieving representative accuracy with monitoring in place to catch drift over time.


## Operational Lessons


- **Build confidence signals into every dashboard.** Every UA chart should answer: “Can I trust this data right now?” We display data freshness, pipeline health indicators, and known issues inline. Stale or unreliable UA is worse than no UA — it erodes trust in the metric.
- **Plan for graceful degradation.** A reliability metric that fails during incidents defeats its purpose. We built resilience into UA: rolling query windows capture late-arriving data, backpressure delays processing rather than dropping events, and batch pipelines provide fallback when real-time fails. Stale but accurate UA is better than no UA during an incident.
- **Version your rules like code.** Impact definitions, eligibility filters, and latency thresholds are configuration that changes over time. We treat them as code: version-controlled, reviewed, and auditable.
- **Monitor the monitor.** We track query execution latency, metric emission success rates, and data completeness. UA that fails silently is a reliability liability.


## Results and Impact


Since deploying UA:


- **Faster incident detection** : With UA, we were able to improve our median incident (experience degradation) detection time to under 5mins from 10mins
- **Improved customer trust:** UA provided rich, multi-dimensional customer segmentation that strengthened incident response. By analyzing UA across customer behaviors, automation patterns, and severity levels, teams quickly isolated impact sources, enabled proactive outreach, and engaged affected customers during incidents with clear, data-driven context and remediation timelines — reducing escalations and building trust.
- **Early warning system** : Anomaly detection caught latency-induced degradations before they reached incident severity, enabling intervention before customer impact spread.
- **Data-driven performance investment** : UA analysis identified that 40–50% of tail latency could be addressed through cache-backed APIs and batched calls. Acting on this insight, targeted optimizations reduced p99 latency from 4s to under 800ms for key workflows, with UA data continuing to inform which optimizations deliver the greatest user impact.
- **Organizational alignment around user experience:** UA became the primary health indicator across Box features, embedded into change control policy. Significant UA degradation now gates deployments and triggers incident review. This shifted the conversation from “did something break?” to “how many users are affected?” — giving leadership a single metric that reflects actual customer experience.


## Call to Action: How to Start Your Own User Availability Journey


If you’re thinking about adopting a user-centric reliability metric at your organization, the good news is you don’t need a perfect platform to start. You need a clear definition, consistent instrumentation, and a willingness to iterate.


- **Start with 1–2 critical workflows, not “all traffic.”** Build trust in the methodology before expanding scope. All-traffic UA is appealing but masks signal quality issues.
- **Write down your contracts explicitly.** For each workflow: what makes a user “active”? What makes them “impacted”? What’s excluded and why? Ambiguity in definitions creates confusion during incidents.
- **Backtest against known incidents.** Before going operational, run UA against historical data for incidents you know caused user pain. If UA doesn’t detect them, your definitions are wrong.
- **Run in shadow mode before alerting.** Parallel-run UA alongside existing monitoring for several weeks. Investigate every discrepancy. Tune thresholds based on false positive/negative analysis.
- **Budget heavily for operational tooling.** The aggregation logic is maybe 20% of the work. Pipeline reliability, monitoring, dashboards, and runbooks are the other 80%.


## What We’d Do Next


- **Client-side integration:** Server-side UA misses rendering failures, network issues, and UI bugs. Integrating client telemetry will give us true end-to-end visibility.
- **Broader coverage:** We’re currently tracking tier-1 workflows. Expanding to long-tail features requires investment in automated onboarding.
- **Latency-driven optimization:** UA has surfaced which tail-latency hotspots impact the most users. Next step is using this data to prioritize performance investments — converting reactive “fix the slow thing” into proactive “improve the experiences that matter most.”


## Conclusion


The shift from “what percentage of requests succeeded?” to “what fraction of users had a meaningfully bad minute?” sounds like a small reframing. In practice, it required rethinking our data pipelines, impact definitions, and organizational processes.


The technical implementation — three-level aggregation, per-user-per-minute windowing, eligibility filtering — is relatively straightforward. The hard parts were: defining impact thresholds that balanced sensitivity with stability, building pipelines reliable enough to trust for paging, and socializing a new metric across teams accustomed to request-based thinking.


UA isn’t a silver bullet. It has blind spots (users who give up before making requests, client-side failures we don’t observe), operational overhead (pipelines to maintain, filters to tune), and complexity (more configuration than simple request availability). But it answers the question that actually matters: “Are our users having a good experience?”


That question turns out to be worth the complexity.


## References and acknowledgments


Our approach builds on foundational research in user-centric availability measurement:


1. **Google’s “Meaningful Availability” (2016)** : Hauer et al.’s work on counting users rather than requests was the primary inspiration for UA. Their insight that “availability is not about request success rates; it’s about user success rates” fundamentally shaped our design philosophy. \[Read the paper\]([https://research.google/pubs/meaningful-availability/](https://research.google/pubs/meaningful-availability/)[)](https://research.google/pubs/pub45416/))
2. **Site Reliability Engineering literature** : Concepts from Google’s SRE books — particularly error budgets, SLO-based alerting, and the distinction between SLI/SLO/SLA — informed how we operationalized UA for incident response and executive reporting.
3. **Apdex (Application Performance Index)** : The Apdex standard’s approach to categorizing response times (Satisfied/Tolerating/Frustrated) provided a proven framework for translating latency into user experience impact.[https://en.wikipedia.org/wiki/Apdex](https://en.wikipedia.org/wiki/Apdex)
4. **Mentors:** I would like to acknowledge this work to the mentorship of[Tapas Kumar Mohapatra](https://www.linkedin.com/in/tapasmohapatra/) and[Sergio Aguilar](https://www.linkedin.com/in/saguilarg/) . They consistently asked the hard questions, challenged my assumptions, and pushed me to go further — enabling me to take this work beyond what I could have done alone.
