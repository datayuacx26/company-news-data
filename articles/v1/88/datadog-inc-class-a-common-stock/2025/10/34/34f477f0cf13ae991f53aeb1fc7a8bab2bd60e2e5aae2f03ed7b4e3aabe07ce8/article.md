---
schema_version: "1.0.0"
document_id: "34f477f0cf13ae991f53aeb1fc7a8bab2bd60e2e5aae2f03ed7b4e3aabe07ce8"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/rethinking-reliability/"
published_at: "2025-10-15T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:b28d3ec7f566e4eb3da527f23041ec368eb9a01ce53bf96cd99e73fe8381f74e"
---

# Failure is inevitable: Learning from a large outage, and building for reliability in depth at Datadog

Laura de Vesine


Rob Thomas


Maciej Kowalewski


In March 2023, Datadog experienced a rare, widespread[incident](https://www.datadoghq.com/blog/2023-03-08-multiregion-infrastructure-connectivity-issue/) that left large parts of our infrastructure only partially functional, but from a customer’s perspective, our platform looked completely down. This **square-wave failure pattern** —up, then instantly down—revealed critical limitations in how our systems degraded and our ability to serve our customers. Since then, we’ve rethought our approach to failure across our products and infrastructure.


In this post, we explain what we learned from the incident, how we responded, and what it takes to build for graceful degradation at scale.


## The problem: What our March 2023 incident taught us


Revisiting[our March 2023 incident](https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-platform-level-impact/) , the immediate cause was an unsupervised global update, which caused a restart interaction that removed connectivity to approximately **50–60%** of our Kubernetes nodes in production. While that level of platform loss would have a significant impact on any system, the effect on the Datadog platform was a nearly complete loss of user-facing functionality. Our web interface mostly self-recovered quickly, but logs, metrics, alerting, traces, and various other products and features critical to our customers’ operations were fully unavailable. Pages loaded, but displayed no data.


## Why classical root-cause analysis wasn’t enough


Classical incident analysis usually traces back to the **precipitating event** —often called **root cause** —that caused a system to become unstable, and focuses on remediating that cause. In this case, we could easily identify our global update mechanism for critical security patches as the precipitating event for this outage. We also identified that this mechanism was a legacy system. Since its original implementation, we had built lifecycle automation for all nodes that could apply changes, including for critical CVEs, across our entire fleet using a “normal,” regularly exercised update mechanism. We disabled the fleet-wide CVE pull as soon as we identified it as the trigger for the incident, with no loss of security or reliability.


The challenge with this kind of causal analysis is that there are an infinite number of events that might destabilize a system and lead to a significant outage. In this case, the trigger was an automated update, but across the industry, major incidents have been caused by everything from certificate expiry to daylight saving time bugs, leap day handling, system-wide cascading overload, configuration pushes, and beyond. To truly build a more resilient and reliable Datadog, it became clear that we could not rely on fixing the precipitating event alone. Nor is it feasible to prevent every possible perturbation of reality that causes an incident.


## Why this incident hit our users so hard


Instead of focusing solely on the precipitating event, we turned to a more pragmatic engineering question: **Why was this incident so bad?**


While the impact to our systems was significant, it was not total. At peak, approximately **40–50%** of our Kubernetes nodes were still running and connecting. But from a user’s perspective, the failure was binary. The platform appeared completely down.


Historically, we had optimized service design to guarantee **data correctness** , often in ways that prioritized **full stop** over showing **almost correct** data. Under normal operating conditions, this is the right choice. Say you are using a metric like` aws.ec2.host_ok` , which is tagged with the host ID and records a value of` 1` if the host is up and running, with a monitor that will fire if the sum (total number of hosts running) falls below a threshold. A minor delay in processing this metric could make it appear as though only 50% of your hosts are actually running, resulting in spurious alerts. To avoid this, our system would wait to report query values until tags for the metric had been fully processed, ensuring accuracy over partial visibility.


This bias to accuracy works well under normal circumstances, with systems fully up and running. But during a large-scale outage, it produces a **square-wave failure pattern: we cannot report on any data, because some data is missing** . This problem can be compounded by several factors: queuing systems that process in order, which can cause results to stall behind a single stuck issue and prevent real-time data from being shown immediately with recovery; retry logic that overloads downstream systems; and system designs that force specific metrics to be processed on particular nodes.


Overall, we identified a pattern in our systems design that needed to change: we had built with the assumption that the only way to handle failure was to prevent it entirely—or to stop everything—rather than finding ways to degrade gracefully and continue delivering value to customers, even under extreme conditions.


## Why graceful degradation became a priority


Reliability has always been a priority for Datadog. We know our customers depend on us for their business, and we take that responsibility extremely seriously. But prioritizing reliability had led us to build **never-fail** architectures—systems in which components and services had to be fully functional to serve customer use cases. We built for reliability largely through redundancy, designing systems so that those components never go down in the first place.


This incident introduced a shift in mindset. Instead of only trying to prevent failure at every level of the system, we had to accept that no action on our part—no matter how heroic—could prevent all failures. We had to invest in not only preventing failures, but also in **failing better** when they inevitably occur.


Failing better means continuing to serve our customers’ priorities even when parts of the system are failing. For most of our products, that means:


-


Data is never lost, even if it’s late.


-


Real-time data takes priority, and we avoid spending scarce resources on processing stale data.


-


Whenever possible, we serve partial-but-accurate results instead of nothing at all.


## How we approached the solution


With this new mindset in hand, we began making changes product by product to enable graceful degradation under unavoidable failures. Mechanically, we approached this as a company-wide program, with contributions from many individual product teams. While **graceful degradation** is a broadly applicable principle, how to implement it for any given product depends heavily on its specific customer use cases and its internal architecture. Still, common themes emerged across teams as we worked toward this goal.


### Preventing data loss with persistent intake storage


During the original outage, we lost a limited but non-zero amount of customer data in unrecoverable ways. Based on our priorities outlined above, addressing the causes of this data loss was critical. **Our analysis found that one major factor was a lack of disk-based persistence at the very beginning of some processing pipelines.**


This gap led to data loss in two ways. First, where data was only in memory or a local disk, losing the node meant that not-yet-replicated data was lost. While our pipelines typically write to replicated data stores early in processing, some did so *after* acknowledging receipt. This approach allowed us to provide low-latency responses at intake. This meant that some data existed only on the local node and wasn’t eligible for agent retries. When those were lost, so was the data.


Second, our post-intake replicated data stores were often unable to accept writes following the catastrophic node loss. That meant even intake nodes that stayed online could lose data as memory and local disk buffers overflowed as the incident went on.


A **never-fail** approach to solving this would have relied on somehow keeping the replicated data stores able to accept data—an impossible expectation, since catastrophic failure modes are not limited to just node loss. Instead, we worked under the assumption that these data stores **would** fail, and we built substantially more robust persistent disk storage to persist data as part of intake, no matter what goes wrong. **This allows us to replay data no matter what kind of failure occurs.**


### Making live data available faster


When we examined why this outage prevented us from serving live monitoring data, a few themes emerged. First, many of our systems were built to process all data indiscriminately, in the order it was received. This approach makes sense if you are building a system that you assume can never fail. It is simple to reason about and offers strong correctness guarantees.


We’ve since made changes that allow some services to **skip forward** over a processing backlog and catch up to live data as quickly as possible during recovery. We also recognized that not all telemetry data is equal—for example, some powers high-urgency monitors—and we’ve built (and continue to build) internal QoS mechanisms to prioritize processing more important data first.


Second, we found that in many cases, systems’ automated recovery behavior—such as processing backlogs, retries, and so on—actually made recovery slower. We addressed this by **updating retry logic in affected systems** . Retries now go to a different backend to avoid persistent failure with limited downstream failure scopes, use strong backoff mechanisms to reduce overloading, and fall back to dead letter queues sooner to avoid halting processing on single “bad” packets. We also introduced mechanisms to **throttle backlog recovery** , ensuring that old data does not preempt live data processing.


Finally, we found that scarce compute resources were not always directed to the most important services for live customer data. We resolved this by introducing **prioritization at the infrastructure and compute level** : we added a` PriorityClass` mechanism for our Kubernetes workloads, implemented a faster and more responsive autoscaler, and we conducted a global audit of job priorities.


### Removing architectural bottlenecks and technical debt


In some cases, the reasons for our **square-wave failure** were tightly linked to service architecture, accumulated technical debt, and historical caching decisions. For example, our metrics processing pipeline was originally designed to de-duplicate tags using a large, shared Cassandra cluster as a durable cache.


Over time, the need for this shared cache had diminished as services in the pipeline shifted toward relying on local caches for tag lookup. While there were plans to eventually deprecate this shared cache cluster, it was still on the critical path for processing metrics at the time of this incident. Unfortunately, a large Cassandra cluster is slow to rebuild after losing many nodes. This caused a significant delay in recovering service for customers.


After this incident, we prioritized removing this cache pathway to **reduce the number of non-local critical dependencies for serving real-time metrics queries** . In other cases, we identified services that could fall back to replicas or local caches and **serve slightly stale data** during backend failures, but weren’t yet configured to do so.


### Scaling recovery and building for shared fate


We also observed several cases where recovery was[delayed by the sheer scale of systems to recover](https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-platform-level-recovery/) , circular dependencies in our recovery tooling, and generally slow service startup times.


To address recovery at scale, we introduced additional horizontal sharding along with improvements to shared fate and data locality. For example, we moved from a single site-wide Vault instance for certificate issuance to cluster-local Vault instances with additional fallback options. While running and syncing these instances is more complex operationally, it dramatically improves our ability to begin recovery quickly—even in catastrophic scenarios— by **eliminating single bottleneck services** that were already near their scaling limits.


For our internal tooling (tools to recover our tools), we analyzed circular dependencies and shared-fate risks across systems like our build infrastructure, database recovery tooling, and Kafka deployment automation. We added **manual break-glass mechanisms** wherever a circular dependency could prevent recovery of our systems.


When looking at slow service startup, we found two common causes. The first was a **scarcity of compute resources** , which we addressed using Kubernetes priority mechanisms. The second was **services loading large, processing-intensive caches into memory at startup** . We shortened lookback windows for these caches and adjusted their data format to not require startup processing.


### Maintaining control of the system


The changes we’ve made are designed to allow the system to gracefully adapt to disruption on its own, without human intervention. In some cases, we achieved that by designing the system such that failure modes naturally cause degradation that makes the most sense to the user. In others, we relied on detecting and responding to failures—for example, through automated circuit breaking or failovers. While those mechanisms **shouldn’t require human action to trigger** , we also **allow operators to override the behavior** in case of false negatives or false positives.


The control plane built for this purpose is also designed to degrade gracefully in case of failures. We’ve designed layers of break glass procedures that allow us to continue controlling system behaviors—perhaps with a degraded experience for operators, or requiring experts to propagate changes semi-manually. **All elements of the system, even its internal control plane, should be designed to work as well as possible even under failure.**


### Replaying chaos at scale


As we built and rolled out these solutions, we used chaos testing to validate our improvements. Test plans include explicit hypotheses about how services should behave under degraded conditions, with a focus on the degradation condition itself (for example, not enough capacity to serve all traffic) rather than the specific mechanism that causes it.


These scenarios enable ongoing, fully automated chaos testing—confirming that our systems are now significantly better at serving customers, even under major failure scenarios.


## What we’ve learned


Designing our systems around inevitable failure—while still preventing failures wherever possible—represented a significant shift for many of our designs. Because the changes that enable graceful degradation often require rethinking fundamental design assumptions, we’ve taken a deliberate, careful approach. We certainly don’t want to cause new issues while trying to prevent future ones!


While the particulars of how to design for graceful degradation will always depend on individual services, systems, and products, we’ve identified some useful patterns and techniques:


-


**Always start with what’s important to the end user.** Prioritizing real user needs through the end-to-end experience is at the heart of any graceful degradation story.


-


**Persist data early.** If data durability is critical, persist data as early as possible in the processing pipeline.


-


**Avoid global control systems that influence many services.** They often become new, complex failure modes.


-


**Data and query prioritization (without building a complex control plane) is key to fast recovery.** This includes strategies like processing data out of order, de-prioritizing backlogs, and so on.


-


**Check retries and caching carefully.** They’re useful, but have sharp edges and “footguns” that can make major incidents much worse if not carefully examined.


-


**Reduce technical debt dependencies.** Added complexity can be a ticking time bomb.


-


**Invest in break-glass tooling.** While smart engineers can often break circular dependencies in the moment, tooling is a better strategy.


-


**Fix your tools, not just your systems.** Tools to fix your tools are as important as tools to run your systems.


As we’ve made these changes to our systems, we’ve already seen that incidents are typically shorter and less disruptive. Live monitoring data and alerts, in particular, recover faster as part of system restoration. While the rare and unpredictable nature of incidents means that aggregate measures can give only an incomplete story, we see promising trends over the past two years. Notably:


We have a 30% decline in the number of significant incidents affecting customers’ monitors.


Our median time to mitigate customer impact for incidents in our logs product is down 10%; at the 95th percentile, that time is down almost 50%.


Incidents in our metrics product have much more limited blast radius— *most* metrics incidents affect only a limited number of metrics we process, usually less than 10%.


## Building for failure—on purpose


No system can prevent every failure, but we can design for better outcomes when failure inevitably happens. The March 2023 incident reminded us that incident failures and user experience don’t always align, and that recovering quickly isn’t just about restoring systems—it’s about restoring the right systems in the right order.


By prioritizing graceful degradation, Datadog is building an even more resilient, customer-focused infrastructure.


Interested in solving problems like these at scale?[We’re hiring!](https://careers.datadoghq.com/all-jobs/?s=SRE&parent_department_Engineering%5B0%5D=Engineering&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025--&gh_src=hm4uekgj1us)


-
-
-
