---
schema_version: "1.0.0"
document_id: "fe1f32ac0a1c9e222864251a5b7088f86160426591e7d77921fc4c24584cb304"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/the-ml-feature-pipeline-that-got-slower-and-no-one-noticed-8e90c224eae3"
published_at: "2026-05-07T16:01:01+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-08-20T03:40:21.870140+00:00"
content_hash: "sha256:022ac625680af744fe77eb538bf55c232f6f856c0040d0119ead34200e8edae9"
---

# The ML Feature Pipeline That Got Slower and No One Noticed

[David Kawashima](https://medium.com/whatnot-engineering/linkedin.com/in/david-kawashima-b60585104/?skipRedirect=true) **,**[Miguel Fernandez-Montes Cuberta](https://medium.com/whatnot-engineering/linkedin.com/in/miguelfmc/?skipRedirect=true) **,**[Ford Bohrmann](https://medium.com/whatnot-engineering/linkedin.com/in/fordbohrmann/?skipRedirect=true) **| Discovery**


A bad PR went out just as our hourly feature pipeline was about to run. The pipeline was responsible for recomputing fresh user behavior signals used in livestream ranking on the home feed. These features capture recent user intent: watches, bids, orders aggregated across users, sellers, and categories. A missed feature update means that the system stops responding to user signals immediately.


This PR release caused one of our feature groups to suddenly produce zero rows … **** but nothing broke!


Even though a critical feature group for the model stopped updating, the online feature store continued serving the **last-known-good features** , buffered by a 2-day TTL and nothing broke explicitly. Our anomaly monitors went off immediately, flagging the drop in row counts. The team identified the bad change and reverted it before the freshness gap grew large enough to matter.


No corrupted data reached production. The system absorbed the failure and degraded safely. This graceful degradation didn’t happen by accident; it was the result of hardened processes from years of lived experience, seeing the way these systems can degrade in obvious or subtle ways. In this post, we share some of those lessons learned.


### The system: hourly feature pipeline


Whatnot is a highly dynamic marketplace where buyers’ preferences change at a moment’s notice. A buyer might research a laptop, join a football-card break a few hours later, and buy chocolate before logging off for the night. Daily feature updates capture long-term preferences but miss intra-day shifts.


To keep up with these rapidly changing buyer interests (in 2025) we moved from *daily to hourly* updates for features that power our ML feed ranking models. Fresher features are a critical component for capturing the live nature of the Whatnot marketplace, where the inventory available to purchase changes in real time. Moving from daily to hourly updates was a meaningful improvement. However, this update also came with a catch: the more frequently we update features, the **less time we have to catch and correct issues** before they impact users.


This is especially critical for new users. Their “aha moment” depends on how quickly recommendations adapt to their latest actions to help find the right livestream, with the right item at the right time.


Feature Training & Serving Pipeline


Going hourly exposed failure modes that daily pipelines could mostly hide.


Our hourly pipeline for livestream ranking runs in three stages: raw engagement events (watches, bids, orders) are ingested and transformed via DBT into user-seller and user-category feature aggregations (~30 min total), then pushed into our feature store that uses Snowflake for training offline and Redis for real-time ranking (~5 min). Each stage has dedicated alerts for failures, runtime regressions, stale ingestion, and feature distribution anomalies, providing end-to-end observability across the ~37-minute pipeline. Hourly features have a TTL of 2 days in the online store to provide a buffer and preserve non-zero, stale values in case of >24 hours of ingestion failures.


On paper, this is simple. In practice, it spans multiple teams and a third-party system, with tight timing constraints. Every step can fail, and at an hourly cadence, we need to catch issues quickly.


### **What production actually threw at us**


#### **The bad PR: a dramatic setup, a deliberately boring ending**


The failure mode described above, with a bad PR stalling feature updates, happened early in the rollout of hourly updates. We pushed a change that caused one of our feature groups to return zero rows. Our row-count anomaly alert fired within the hour. We found the bad change, reverted it, and by the time we’d done a full postmortem, production systems barely noticed. The online store kept serving last-known-good values the whole time, buffered by the 2-day TTL meant to handle cases like this.


A boring ending, which was the point. We’d designed for this class of failure, and when it finally showed up, the design held. A clear success story!


#### **False alarms masked real issues.**


As we expanded to new countries and categories, however, our feature distribution alerts started firing frequently, even when nothing was actually broken. Once we sliced aggregate alerts by country and category, normal marketplace volatility started looking like pipeline failure.


Some false positives are acceptable to avoid missing real issues. The problem was that we treated all alerts the same. Over time, the alerts started firing constantly, weren’t actionable enough, and on-calls started ignoring these alerts. The monitoring system we’d built faded into the background, and real issues weren’t being triaged.


The fix was to be more opinionated about what each alert meant. We split alerts into two tiers:


- **High-confidence failure signals:** e.g., zero-row counts. Binary, unambiguous, and almost always bad. These block ingestion and page on-call immediately.
- **Low-confidence anomalies:** distribution shifts with many benign causes. These go to a Slack channel for business-hours review, with runbooks for common patterns, and for quick diagnosis when needed.


Once we made this distinction, alerts became actionable again. When they fired, on-call teams trusted them—and responded accordingly.


#### **The system that got slower and no one noticed**


A few weeks in, something felt off. Hourly features weren’t updating as often as they should. The core table job had quietly shifted to running every two hours with no errors, no failures, no alerts. By every metric we were tracking, the pipeline looked “healthy.” It was just running half as often as intended.


At the same time, a slower trend was emerging. As Whatnot grew with more users, more categories, and more data, the pipeline got heavier. Feature models increased from ~2 to ~5 minutes. Feature store ingestion grew from ~5 to ~10. Again, no alerts, just gradual erosion of the buffer that kept us within the hourly window.


These were the same failure modes expressed differently: one abrupt, one gradual. Neither triggered alerts because nothing was technically “broken.”


For systems where cadence and execution time matter, the absence of failure is a weak signal. We need to explicitly monitor what matters: time between updates, end-to-end latency, and runtime trends. Otherwise, degradation goes unnoticed.


### **How we monitor and ensure reliability today**


We’ve seen what production threw at us and realized there are many different ways that pipelines could break, from distribution skews to corrupt data models to ingestion failures, so we broke down issues into 3 tiers based on estimated GMV impact. We determined these thresholds based on evaluating the performance of a model trained with accurate hourly features but served using inaccurate **** hourly features, and came up with the following degradation tiers:


- **Tier 0:** Features missing or corrupted → ~ -2% GMV
- **Tier 1:** Delay >24h → ~ -1% GMV
- **Tier 2:** Delay <24h → degraded experience


Tier 0 or 1 issues constitute production incidents and trigger on-call immediately, while Tier 2 issues trigger alerts to be handled during business hours.


Incident Classification for hourly features


We also defined the following service level objectives (SLOs):


- 99.9% success for core tables + feature models + ingestion
- End-to-end runtime < 60 minutes (37 min at launch)


But what actually protected us in practice wasn’t any single monitor, it was **defense in depth** . We built layered coverage across the entire pipeline, with each layer designed to catch a different class of failure:


- Core data model failure detection and runtime monitoring
- Feature-level distribution checks and anomaly detection
- Ingestion health checks in Feature store
- Monitoring of features as actually served in production


Each layer assumes the previous one can fail. Online serving metrics act as the backstop for anything that slips through upstream, and TTLs provide a final safety net for anything that escapes monitoring entirely.


### Ready and reliable


This layered approach is what makes operating fresh features viable in production. Freshness is exciting, but reliability is the real constraint, and now that we’ve made hourly features robust, we’re in a position to push even further. Our next step is moving to within-session and sub-minute updates, because when you’re the leading livestream shopping platform, every second counts.


If building systems at this scale sounds interesting to you in live commerce, real-time infrastructure, andthe intersection of community and technology ,[we’re hiring](https://www.whatnot.com/careers) !


---


[The ML Feature Pipeline That Got Slower and No One Noticed](https://medium.com/whatnot-engineering/the-ml-feature-pipeline-that-got-slower-and-no-one-noticed-8e90c224eae3) was originally published in[Whatnot Engineering](https://medium.com/whatnot-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
