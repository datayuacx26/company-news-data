---
schema_version: "1.0.0"
document_id: "137ea495130962a5d54dc5e1f2361b878f3cd1925060f7394ab348360a0f1b89"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/rate-limit-aware-agent-orchestration"
published_at: "2026-07-22T09:00:00+00:00"
first_seen_at: "2026-07-23T11:18:23.733865+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:265ed6eed85208b10509ed035be6c5fd9129edce07a1e1fbebbb2abd19749d02"
---

# The Case for Rate Limit-Aware Agent Orchestration

In many agent orchestration systems, the agent assembles the context required for an action at runtime by calling individual tools. This approach can work well in demonstrations and relatively simple workflows, but it often becomes fragile in production as the number of tools, dependencies, latency constraints, and failure modes increases.


One of the first reliability mechanisms teams commonly add is automatic retry handling for transient tool failures, including provider rate limits such as HTTP 429 responses. These retries typically require bounded exponential backoff, jitter, and support for provider-supplied retry guidance.


The problem is that retries are often implemented independently within each agent instance. That works reasonably well when a single instance is making requests, but production systems may have many agents calling the same provider and consuming a shared rate-limit quota. Without centralized coordination, each instance has only a partial view of the total request volume. Multiple agents may exceed the shared limit and then retry at roughly the same time, increasing contention and prolonging the rate-limit condition.


## **The Retry Decorator Tracks the Budget at the Wrong Level**


Any budget for a provider's rate limit should be shared among all session instances that are accessing that rate limit. The retry decorator tracks that budget locally, as if it belonged to a single agent instance, but the budget itself belongs to the org, account, or app making the calls, not to any one instance.


Rather, the system needs to consider four components of an effective, rate-limit-aware agent system.


- Shared rate-limit pool
- Backpressure
- Prioritization
- Graceful degradation


The shared pool is the foundation of the system. Backpressure, prioritization, and graceful degradation are policies defined on top of that shared pool.


## **Who Owns The Quota**


The rate limit for any provider is defined in terms of some accounting boundary for that provider. The quota is typically not defined in terms of the agent session.


That boundary is the org, account, or app, not the individual agent session. Each instance of an agent session that makes tool calls consumes the rate limit for that same quota, whatever it's counting against.


A rate-limit counter kept inside a single agent instance is therefore reasoning about the wrong object: it has no way to see what the other instances have already used. To account for the quota shared across all instances, the coordination boundary for rate limits must sit at the same level (the org, account, or app), and not within a single agent session.


## **Why Independent Backoff Synchronizes The Herd**


The second instinct for creating a robust retry system for a 429 response is to introduce jitter.


[Marc Brooker's analysis](https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/) on the AWS Architecture Blog showed that capped exponential backoff, where the wait interval grows up to a maximum value, still leaves clusters of calls: clients that fail together retry together, round after round. His simulation modeled contending database writers rather than rate-limited API clients, but the dynamic is identical for agent instances that all receive a 429 at the same moment. This is the correct response to the problem, but it only goes halfway to fixing it.


Introducing jitter to the retry backoff schedule will decorrelate those clusters. However, jitter only addresses timing. The retry system also needs a cap on how many tool instances can make a request within a given time window, and that cap can only exist on top of a shared rate-limit pool.


## **How Backpressure Slows The Producer Before Rejection**


A shared rate limit pool helps determine how many requests can be made before the provider returns a 429 response. Backpressure uses that information beforehand: it stops requests before they can trigger a 429, rather than reacting to one after the fact.


Backpressure in the context of[reactive streams](https://www.reactive-streams.org/) is a signal that travels upstream from a resource-constrained consumer to a fast producer, slowing the producer to a rate the consumer can absorb. If the provider exposes an API endpoint that returns the remaining rate limit, that value represents the consumer's capacity. The agent system’s loop is the producer. Therefore, implement backpressure by having the orchestrator tell the agent instance to stop making tool calls when the shared budget is low.


A rejected request and its retry will consume additional rate limit from the pool. A request that has not been made because the producer has been “throttled” doesn’t impact the rate limit pool. Backpressure changes the agent’s approach to rate limits from exception to rule.


## **Why The Shared Pool Is The Hard Part**


This is where the implementation of most rate limit-aware agents falls apart.


In the scenario where there is no shared rate-limit pool, *N* instances of an agent, each enforcing a limit of *L* , will allow the following number of requests against a quota sized for one:


*N × L requests*


Everything beyond the real limit is the overrun. To fix it, you can create a centralized token-bucket store. Stripe built its[rate limiters](https://stripe.com/blog/rate-limiters) on Redis, a common choice for distributed systems that implement rate limits at scale.


The operation that consumes a token from the pool must be atomic so that two separate agent instances do not both read the remaining tokens and each consume one.


[Figma](https://www.figma.com/blog/an-alternative-approach-to-rate-limiting/) has published its findings on how a read-then-write operation on the remaining token count introduces a race condition between two processes that check the count at the same time. The standard fix, the one Stripe implements with Lua scripts in Redis, places the check, the refill, and the consumption of a token in a single atomic call, so an instance cannot consume a token that has already been taken by another. Figma itself sidestepped the race with a different algorithm, sliding window counters, whose Redis operations are atomic on their own.


In this design, agent instances reserve small batches of tokens from the pool ahead of execution rather than taking one token per call. Every reservation needs a time-to-live (TTL) tied to it so that if the agent instance crashes before it can exhaust its tokens, the unused tokens return to the shared pool. Otherwise, the shared pool will understate the number of requests that can actually be made within the time period.


## **Per-Provider Limiters Have to Match Provider Quotas**


A shared pool with backpressure can handle a single provider with a rate limit. But each provider defines its quota differently, so one shared model won't fit all three.


Salesforce enforces a daily request limit with a concurrency cap. HubSpot enforces a 10-second request burst on top of a daily limit. Jira enforces an hourly points-based quota, where different endpoints cost different point values, plus per-second burst limits on individual endpoints.


Provider Accounting Boundary Shape of the Limit


**Salesforce** Org Daily API request limit, with a system protection limit after exhaustion.


**HubSpot** Private app and account Per-10-second burst plus daily ceiling.


**Jira Cloud** App across tenants Points-based hourly quota plus burst behavior.


Because the accounting boundary and the shape of the limit both vary by provider, a single shared budget for "the" rate limit won't work. The system needs a per-provider rate-limit model instead.


That model should be driven by each provider's own limit signals, as those are the source of truth. The 429 response from Jira Cloud includes headers for the remaining quota and when it resets. HubSpot returns a 10-second remaining count and a daily remaining count on most responses. Salesforce never signals quota exhaustion with a 429 at all: it reports usage via the **` Sforce-Limit-Info`** header on every REST response, via a dedicated /limits endpoint, and enforces limits with a 403 once its system protection limit kicks in. The rate-limit model should read these signals to define its per-provider limits, and, where available, from successful responses. By the time a 429 arrives, backpressure has already failed.


## **Why Calls Need Different Priorities**


Even with a well-tuned per-provider pool of tokens, there’s still one more thing to consider: what happens when there’s a demand for calls that exceeds the budget set for each provider?


Since all calls are treated equally by the system, some incorrect calls are discarded. For instance, an agent’s background enrichment poll call and the customer-facing record write call have vastly different levels of importance. Yet the current system treats them the same.


Prioritizing calls can allow the critical-path write call to proceed while the background poll call waits for its turn.


This matters more for agents than for other API clients because agent workloads are dominated by polling. Zapier measured this for integration workloads years ago:[98.5 percent of polls returned nothing new](https://resthooks.org/) across a 30-million-poll sample, and agent loops that poll for task status inherit the same pattern. A status poll can be deduplicated and served from cache across many agents. The customer-facing write cannot wait. When demand exceeds the budget, the write is the call that must win.


## **How Graceful Degradation Handles Incidents**


The prioritization system will be in place for normal operations. Yet there will also be a system in place that handles incidents that occur regardless of the rate-limiting system.


The rate-limiting system will control how quickly requests from APIs with a high tolerance for latency are processed. Yet graceful degradation will also handle instances in which one of the providers is throttled by the external API, regardless of the rate limiting in place.


For agents, the system will focus on shedding non-essential calls so that agents do not waste time on information that can be served from cache or a lower-priority call path.


Borrowing the criticality levels from the Google SRE Book, the calls can be classified as` CRITICAL` for customer-facing writes;` SHEDDABLE_PLUS` for reads needed to provide context for those writes; and` SHEDDABLE` for background enrichment polls that can be dropped when one of the providers begins to be throttled. Either we, as developers, decide which calls will be sheddable, or the external APIs make that decision by rejecting all calls equally. I'd rather make that decision myself.


## **The Cascade These Layers Block**


Each of these systems prevents a certain type of failure. If any of them are removed, the system will be vulnerable to failure in response to the external API provider’s slowdown.


Here’s how the failures will occur in the system:


- One of the application's API providers slows down rather than rejecting requests from agents.
- The session times out while requests are being sent to external APIs.
- The system retries the calls to the external APIs.
- The shared API token quota gets emptied as a result of the retry calls.
- The calls to the external APIs fail and get rejected.
- The agent that initiated the failed calls becomes a source of congestion for the external APIs.


[The Google SRE Book](https://sre.google/sre-book/handling-overload/) explains this challenge, stating that if the backend spends significant resources processing requests that will ultimately fail, the retries will overload the backend already experiencing issues.


The shared pool prevents sessions from collectively overrunning a quota that none of them can see. Backpressure prevents an agent from making calls to external APIs that will ultimately be rejected. Prioritization ensures that background calls do not starve the critical path of calls to external APIs. Graceful degradation prevents the system from making non-essential calls to external APIs that could overload it. Remove any of these systems or controls, and the APIs will fail in the same way.


## **Do This Next**


- **Build a shared token bucket with an atomic check-and-decrement operation in a centralized store.** Without atomicity, two sessions will race to use up the last allocated API token. Add a time-to-live period to every API token reservation to account for crashed sessions that might otherwise hold onto the allocated tokens.
- **Make the shared token bucket per-provider, using each provider's own limit signals.** The global rate limit is wrong for at least two of the three APIs that I use. Read the` Retry-After` and` remaining-quota` headers where the provider sends them, read` Sforce-Limit-Info` and the` /limits` endpoint for Salesforce, and feed those values into each limiter's budget and refill rate.
- **Add backpressure control before you add retry logic.** When the orchestrator encounters a low budget, do not emit any new tool calls.
- **Tag every call with a criticality level so that critical-path calls get their share of available API tokens.** A background poll should never starve a customer-facing write.
- **Decide the degraded path of calls ahead of time.** This is part of the graceful degradation system. Some calls will be more important than others, and some will be sheddable under load.


A decorator that wraps individual tool calls will improve the politeness of the individual clients using the tools. Yet production systems will require the coordination of the shared API budgets. Fifty clients will share one budget for calls to an external API. None of them can see how many calls are left in the shared budget. That coordination has to live at the orchestration layer rather than inside the individual tool call wrappers. Build it there, and the failures described above won't happen in the first place.
