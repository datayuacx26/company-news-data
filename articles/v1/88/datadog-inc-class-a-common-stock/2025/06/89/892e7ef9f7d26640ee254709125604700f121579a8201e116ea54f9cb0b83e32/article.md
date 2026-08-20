---
schema_version: "1.0.0"
document_id: "892e7ef9f7d26640ee254709125604700f121579a8201e116ea54f9cb0b83e32"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/scaling-config-delivery-containers/"
published_at: "2025-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:9b821def144792eb410fdc465cfd52565e73bbd3ebb802e575bdd5d67d22ad73"
---

# How we scaled fast, reliable configuration distribution to thousands of workload containers

Gabriel Reid


When you think about engineering challenges faced in handling incoming log data in Datadog, one of the first things that comes to mind is the scale: quickly and reliably parsing and processing millions of logs per second over thousands of containers. By comparison, if you look at the configuration screens in the Datadog web interface for how incoming logs are to be parsed, they look relatively simple: probably just a boring old CRUD application, and hardly interesting enough to warrant an engineering blog post, right?


These two systems are intrinsically linked, however. To process millions of logs per second, the thousands of workload containers need to be aware of user-specified configuration and need to be able to react almost immediately to user-initiated configuration changes to log processing configurations. Given that logs are processed in a large multi-tenant distributed environment, reliably ensuring that all containers are aware of all configuration changes is far from simple.


Log processing configurations are only one of many examples in the Datadog backend where we need to quickly and reliably propagate users’ configurations at scale. In this post, we’ll talk about why this isn’t as simple as it initially sounds and the internal system that we came up with to handle the challenges of keeping user configuration updates lightning fast and reliable.


## Why fast, reliable configuration propagation is harder than it looks


Many details of how incoming observability data is handled in Datadog, whether for log parsing,[Sensitive Data Scanner](https://www.datadoghq.com/product/sensitive-data-scanner/) configuration, or[daily logs storage quotas](https://docs.datadoghq.com/logs/log_configuration/indexes#set-daily-quota) , are configurable by users on a per-tenant—or per-customer—level. Internally, we refer to these types of per-tenant configuration data as **context data** .


It’s important to note that Datadog handles changes to settings—such as log processing configurations—by applying them right away. For example, if you update a log parsing rule in the user interface and then look at your live streaming logs in[Live Tail](https://docs.datadoghq.com/logs/explorer/live_tail/) , you’ll see that the new parsing configuration is applied almost immediately. This low-latency configuration change propagation is an important aspect of how Datadog works across many products that are powered by the same underlying system used in the[Log Management](https://www.datadoghq.com/product/log-management/) product. Per-tenant context data is used by many backend services, distributed over thousands of containers that are dealing with incoming data from thousands of different tenants.


**The expectation of low-latency context data update propagation is coupled with high reliability expectations.** Context data is essential for Datadog services to handle customer data, so the system that provides the configuration to downstream services needs to be rock solid. At Datadog’s scale, anything can break at any time, so, in designing a system like this, we need to embrace this potential for failure and design around it.


## Why straightforward solutions fall short at Datadog’s scale


At this point, you may be thinking that this still sounds like a pretty simple problem to solve. It’s just a bunch of tenant-specific configuration settings stored in a database—how difficult could that be? As we’ll dive into here, it turns out that **it becomes complex when you’re dealing with thousands of containers and a large volume of customer-specific context data** .


It seems pretty obvious that customer log-parsing configurations need to be stored in a durable data store—like a relational database. However, this still leaves the challenge of delivering the log-parsing configuration to the workload containers that are actually performing the processing work on incoming logs.


## Why fetching context data on demand won’t work


To understand the challenges of doing this reliably, it’s useful to first consider the simplest approach, and why it isn’t sufficient for our needs.


The simplest approach possible for making sure containers have the most up-to-date context data would be to always load the context data needed to process a tenant’s logs every time we receive a log for that tenant. This would provide immediate propagation of context data changes, but would not be feasible at Datadog’s scale. We parse hundreds of thousands of logs per second in near-real-time for larger tenants, meaning even a single instance of a processing workload container would need to make thousands of reads per second on the context database, and this number would then be further multiplied by the number of workload instances that we have. **We would need to have a huge number of highly performant read replicas of our context database to make this approach work.**


A typical solution to this kind of problem of massive reads would be to cache per-tenant context data within workload containers. For example, instead of reading the latest log parsing context data for every log we encountered for a given tenant, we would only reread from the context database every N seconds. While this would greatly reduce the total number of database reads being done for high-throughput tenants, the total number of database reads would still be extremely high due to the large number of workload instances and the high cardinality of context data.


On top of that, caching in this fashion means that we would introduce a mean context update delay that is half of the cache invalidation interval, meaning that any attempts to ease the load on our context database would directly translate into a worse experience for our users. **The longer you cache data, the longer it takes for user-initiated changes to show up.**


## Context loading v1: Direct database connections and cache invalidation via Kafka


In solving the challenges listed above around managing read rate and having low latency in propagating updates, we arrived at the system that we used successfully for a number of years. This system still relied on each workload container loading tenant-scoped context data on demand from a central context database, and then caching the per-tenant context data indefinitely. A Kafka topic was used to distribute cache invalidation messages that triggered a workload container to reload a single context entry for a customer after a user-initiated configuration change. The general flow was:


1.


A user updates their log processing configuration.


2.


The central context database stores the tenant’s configuration.


3.


Kafka broadcasts an invalidation notification after the database write to indicate that the context has changed for a tenant.


4.


All workload containers receive the invalidation message and trigger a reload of that tenant’s context data from the context database.


The following diagram shows the v1 context loading architecture.


## Why we had to rethink our architecture


This approach generally worked well and provided a good balance of doing the smallest number of reads possible from the database, while also having low latency for updates being propagated. However, as Datadog grew, the limitations in terms of reliability and resilience became evident.


For one, this approach still depended on each workload instance being able to access the context database whenever an update was performed. As the number of workload instances grew along with Datadog, this presented a bigger and bigger load on the central context database. Through internal game days and a couple of painful incidents, **we confirmed that anything that impacted the context database could have a direct downstream effect on the workload containers** , causing context updates to no longer be propagated, or even making it impossible for new workload containers to start because they couldn’t read the initial context data from the context database.


Additionally, there were a number of edge cases where delivering notifications with Kafka was not 100 percent reliable. For example, sometimes notifications were not sent when a process was terminated after writing to the context database but before successfully sending a notification. Such cases are particularly problematic because if an invalidation notification was never received, a workload instance could potentially continue using its cached data for hours, days, or even longer. Due to the read load that time-based cache invalidation could generate on the context database, there was no time limit on cache invalidation.


We also found that **we had unintentionally built an excellent system for executing a distributed denial-of-service attack on our own context database** . Particularly at startup of workload containers—for example, when rolling out a new version of a workload, which happens multiple times per day—instances would start up with a cold cache and have to quickly load all context data for all tenants for which they were processing log data. This placed a huge load on the context database, both in terms of the number of reads and the number of connections to the data store.


As we approached the limits of what we could handle by vertically scaling our data store, we knew it was time to take a more drastic approach.


### First step: Introducing a gRPC context service


An initial step that we took to reduce the total number of connections to the central context database was to put a[gRPC](https://grpc.io/) -based service between workload instances and the central context database. Introducing this service abstraction over our context database—which we had been lacking all of this time—provided a **better separation of concerns between the workload instances and the context database** . Additionally, this **greatly reduced the total number of connections to the context database** , which was our most pressing issue at the time. As illustrated below, this change started to push us in the right direction, but we still had work to do.


To solve our problems once and for all, we needed to fully redesign how context data was propagated, in addition to rolling it out on a production system that was processing millions of logs per second.


## Context loading v2: Publishing contexts


To approach this, we first took a hard look at the actual shape of the context data we were dealing with.


We found that context data is relatively:


-


**Small** (relative to Datadog scale), with the total sum of all context data for all customers being measured in megabytes.


-


**Slow-moving** , in that updates to context data are generally made by human actions in the UI. This means that the number of updates is measured in the thousands per day, as opposed to thousands per millisecond.


In terms of constraints for the new system, it had to:


-


**Be highly reliable** and able to withstand a full outage of our context database without immediate effect on our ability to operate or even scale up our processing workload containers


-


**Propagate context updates within seconds** so that user-initiated changes would take effect nearly immediately


By examining the specific shape of the context data, **we saw that we could easily fit all relevant context data within each workload container** , meaning we could actually deploy a mini context database replica inside each running workload instance. This would provide the extremely fast bulk access to context data needed at workload startup time, while also making each workload instance totally independent of the central context database.


That just left the context update path, but we also saw that update traffic was low enough that we could have a dedicated low-traffic distribution path just for propagating updates. This essentially turned into a **relatively small but highly replicated database** , which was only possible because of the small size and low write rate.


The system we ended up with is shown in the following diagram:


The **context-publisher** architecture features two data paths that work together to provide high-reliability and low-latency updates. The first data path is a **batch publish** path, and the second path is an **individual update publish** path:


-


**Batch publish path** : The context-publisher service takes a snapshot of all tenants’ context data of a given type—for example, “logs processing contexts”—and writes it to a[RocksDB](https://rocksdb.org/) file, and publishes it in cloud storage in the form of a **context blob** .


-


**Individual update publish path** : The context-publisher listens to change notification messages like in the original system, and then writes a single full context entry for a given tenant to Kafka whenever it receives a change notification.


Each workload instance maintains its own mutable full copy of the relevant parts of the context’s RocksDB database. A context blob is downloaded from object storage at startup time of a workload instance, and the successful download is required for the workload instance to pass health checks and start dealing with actual traffic.


The workload instance then reads and applies updates to its local RocksDB context database as individual updates are published. Additionally, a new RocksDB context blob is downloaded to replace the current local copy whenever a new context blob is available—approximately every 10 minutes.


This essentially provides each workload instance with its own read replica of the context database, which it can use as much as it wants without affecting the central context database.


### Putting the smallest service in charge


User-specified context data—such as log processing configurations—for our workloads is extremely important. Saying that most of **our workloads can’t do anything useful without the context data that is provided by the context-publisher service** is not an exaggeration. A common approach when dealing with highly critical data like this is to put a lot of effort into fortifying and overprovisioning a critical service like context-publisher, but we decided instead to take an alternative approach and put the smallest service in charge. As shown in the preceding diagrams, there are no external services that can communicate directly with the context-publisher. The context-publisher is entirely in charge of how it handles its own work, meaning that it can never get overloaded by external demand. The context-publisher is tiny in comparison to the workloads that it controls; concretely, its **CPU and memory usage are less than 1 percent of that of the workloads that depend on it** .


A common and difficult problem in distributed systems is dealing with failure. A reasonable first instinct for a highly critical system is to have some kind of fallback option for when the core system fails—for example, allowing services to read directly from the database if the context-publisher system were to fail.


**Such a bimodal system is generally a bad idea in practice, however.** If something goes wrong with the regular path, then the system reverts to a mostly untested mode, which is likely to fail or even make the existing situation worse. For example, if we had a fallback that allowed workload instances to read directly from the central context database in the event of a failure of context-publisher, then this would be a generally untested path—because context-publisher doesn’t typically fail—and could potentially make a bad situation much worse by overloading the context database.


For the context-publisher, we decided to have two complementary behaviors that offer a fallback option for each other, but both are part of the normal flow, so they are exercised as part of normal operation.


The static download of a context database is performed every 10 minutes, so even if the update is completely unavailable, this would only cause an increase in update latency, but it wouldn’t break the system entirely. **In the absolute worst case, this system exhibits static stability** , in that any given workload instance can continue to operate indefinitely with the last-downloaded context blob.


The update flow itself offers a fallback to the static distribution method being unavailable, in that updates continue to be applied indefinitely even if a new static database copy can’t be downloaded for whatever reason—for example, due to cloud object storage being degraded.


## Using context-publisher for more use cases


As alluded to previously, **applying this solution for loading and propagating log processing configurations is just a single instance of a more broadly applicable solution for context data handling** . There are many customer-facing and internal-context data use cases in which the context-publisher architecture is put to work. An example of a non-customer-facing use case is when a logs index hits its daily quota; the command to temporarily stop storing data for the index is just another piece of context data that is propagated to the workloads responsible for writing log data to storage.


## Lessons learned and what’s next for context distribution


This approach for granular context propagation has proven to be successful internally at Datadog, **providing sub-millisecond local loading latency of context data across tens of thousands of workload containers of varying types** . Although this project initially started with a somewhat limited scope, its adoption as a general method of context data handling has been increasing steadily since its inception.


Previously, even a momentary blip in context databases could have significant impact, preventing workload containers from starting up. Although it’s obviously not a desirable situation to have any kind of database outage, the system substantially mitigates the impact of such an outage while still providing the low-latency configuration updates that users expect.


Looking back, **one of our biggest lessons learned from this project was the importance of closely analyzing the real shape and properties of the data** that we’re dealing with. This understanding allowed us to uncover nonconventional options available to us, which could help us take advantage of the small and relatively slow-moving nature of our context data.


Looking forward, **we’re continuing to expand the context-loading system to support new use cases** by further generalizing the way in which it can source context data. Our goal is to support an even wider variety of context data types as we continue building more robust and scalable solutions to solve our technical challenges.


If this type of work intrigues you, consider applying to work for our engineering team.[We’re hiring!](https://careers.datadoghq.com/all-jobs/?s=event%20platform)


-
-
-
