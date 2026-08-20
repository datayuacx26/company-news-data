---
schema_version: "1.0.0"
document_id: "865119caa6b640c0eef15decd676a24a92f4b97f830dcca421cd9c2fd2dd93e2"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/blacksmith-outage-on-july-21-2026"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:4aad1d16fb4d6fa8a976c1938fa7642aff7728448ee1bf56cb5841c4c0014b8e"
---

# Blacksmith outage on July 21, 2026

On July 21, 2026, between approximately 10:20 AM and 3:55 PM ET, our control plane experienced significant service degradation. Customer jobs were not picked up by Blacksmith for several hours during this outage, completely blocking development workflows. Additionally, Actions cache operations, monitors, and sticky disk requests failed or timed out at elevated rates. As a result, job executions were noticeably slower across the board.


Control plane latencies recovered by 3:55 PM ET and the remaining job backlogs drained over the following hours, with service in most regions back to normal queueing latencies by approximately 5:25 PM ET (our eu-central region recovered at around 7:00 PM ET). Jobs that failed to dispatch during the incident were re-dispatched by 7:19 PM ET.


We apologize for the severe impact this had on our customers. Companies rely on Blacksmith to run their most critical workflows, and for most of a working day we did not deliver that. This post explains exactly what happened, why it took as long as it did to resolve, and what we are changing to prevent it from happening again.


## What happened


Our control plane has a dependency on Redis in the hotpath that processes and queue customer jobs. The CPU load on that Redis instance had been ramping up over the past few weeks, but due to inadequate alerting we were not aware of the shrinking headroom. At around 10:20 AM ET, GitHub delivered an erroneous burst of out-of-order webhook events, mostly duplicate deliveries of webhooks we had already processed. That burst set off the following chain of events:


1. The burst caused a spike in HTTP requests processing those deliveries. Each of these requests pushed the already strained Redis host past CPU saturation.
2. With Redis saturated, every Redis operation slowed dramatically. Our servers handle requests with a fixed pool of workers, and each request holds its worker while it waits on Redis, so workers were now tied up several times longer than normal.
3. The worker pool filled faster than it could drain. Incoming requests, including the cache and sticky disk requests that customer GitHub Actions jobs depend on, queued behind the backlog and timed out or were rejected.
4. Without caches and sticky disks, customer jobs ran slower. The increased job durations gradually saturated our compute capacity, and a large backlog of unstarted jobs accumulated.


Redis CPU load over 30 days


HTTP worker pool saturation during the incident


‍


## Why resolution took as long as it did


Two additional problems amplified the impact and extended the time to recovery.


**Our own scale-up overloaded the database connection pool.** Early in the incident, we prematurely scaled up the HTTP worker pool to serve the growing request backlog. Every additional worker holds database connections, and the expanded fleet pushed us into our database's connection limit. Requests that were already slow waiting on Redis now also waited on database connections, and our own mitigation had added a second saturated system to diagnose. The database was now the loudest signal, and it pulled the investigation toward Postgres for several hours before Redis saturation was confirmed as the root cause. Once we understood the interaction, we scaled back down, which relieved the database but meant we could not add capacity to absorb the backlog.


**Our health checks were served by the same saturated worker pool.** Health check requests waited in line behind the backlog like every other request, and timed out. Our load balancer read those timeouts as instance failures and terminated instances that were actually healthy. For roughly two hours this recycled capacity as fast as we could add it, until we pointed the load balancer at a status endpoint unaffected by the backlog.


Postgres connection limits were hit during the incident


## How we resolved it


- **Rebalanced Redis across multiple instances** to distribute load. Once traffic was spread across instances, with metrics moved to a dedicated instance, the core Redis instance's CPU load was halved. Latencies soon recovered and the system began draining its backlog.
- **Redirected the load balancer to a separate status endpoint** for validating service health. This endpoint was unaffected by the ongoing load, which allowed the services to stay up despite the contention from the large volume of incoming requests.


## Timeline (ET)


Incident Timeline — Blacksmith


## What we're doing to prevent this in the future


### What we've already done


- Control plane Redis traffic is now load-balanced across multiple instances. This resolved the CPU saturation that was at the root of the degradation.


### What we plan to do


To address the immediate gaps, we are going to:


- Redesign backend health checks so they respond independently of the request-serving worker pool and cannot remove healthy capacity during a backlog.
- Add alerting on Redis CPU headroom and database connection utilization, and define explicit headroom targets for our control plane's core dependencies.


Looking ahead, Blacksmith's usage has grown substantially this year, and this incident is a reminder that capacity assumptions that held a few months ago no longer do. Since a set of capacity-related incidents in May, we have been re-architecting core subsystems across networking, storage, and job assignment to scale ahead of demand rather than react to it. This incident showed our control plane needs the same treatment. We are expediting that work and dedicating additional engineering to capacity headroom, monitoring, and alerting across every subsystem, so that erosion like the Redis CPU trend that set up this incident is caught and fixed before customers ever see it.


### Fixing how we communicate during incidents


Beyond the technical failures, our communication during this incident was not acceptable. This was a multi-hour outage, and customers went long stretches without a status update. The updates we did post did not clearly state what was impacted, what we were doing, or when to expect the next update.


We are instituting a formal incident response plan to fix this: every incident gets a dedicated communications owner, status page updates at least every 30 minutes, and every update states the current customer impact and the time of our next update. These commitments hold whether or not we have new information to share.


We apologize for the disruption. If you experienced impact that was not covered above, please contact our support team. We're happy to go deeper on the details.
