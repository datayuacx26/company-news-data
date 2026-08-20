---
schema_version: "1.0.0"
document_id: "67ce196342809041f42485a7fc2d728face98f0cc0e69b04d8e91488b17008f7"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2026-02-18-from-always-on-to-on-demand-scaling-kafka-sinks-with-keda/"
published_at: "2026-02-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:19:43.789522+00:00"
content_hash: "sha256:ca6fcc2fd83f88949e1fd0d4385dd783afd8e1b879bdb3bf5c7c10b46b651489"
---

# From Always-On to On-Demand: Scaling Kafka Sinks with KEDA

## Introduction / Context


Kafka sits at the heart of how we move data between systems at trivago. Many teams publish changes to Kafka, and downstream services consume those changes to keep user-facing features up to date—things like accommodation reviews, highlights, and other derived attributes.


To make that possible at low latency, we run a fleet of Kafka consumers we call **sinks** . Each sink takes events from Kafka, applies the necessary business logic (filtering, normalization, policy checks, etc.), and writes the result into a **service-local database** as a materialized view.


This pattern works well from a data and reliability perspective, but it comes with an operational cost. We run **50+ sinks** on **Kubernetes** across **three regions (US, EU, ASIA)** . Topic activity isn’t constant; most changes arrive in bursts, often triggered by nightly imports or cronjobs. The rest of the day, many sinks have nothing to do—yet they still run, poll Kafka, emit metrics, keep connections open, and consume cluster resources.


We wanted a setup where sinks are available when there’s work, but effectively “free” when there isn’t. In this post, I’ll walk through how we used **KEDA** (Kubernetes Event-driven Autoscaling) to scale Kafka sinks **down to zero** and back up based on **consumer lag** , how we rolled it out safely, and the edge cases we hit along the way.


## Background: Current Data Flow


At trivago, Kafka is the main transport layer we use to move data between systems. Many of our services need fast, local access to shared datasets (for example, accommodation reviews, highlights, and other derived attributes). Instead of calling another service on every request, we keep **service-local materialized views** in a local database, so reads are low-latency and independent from upstream systems.


To keep those local databases up to date, we use **sink services** (often just called “sinks”). A sink is a consumer application that reads events from one or more Kafka topics and turns them into a query-friendly representation in the service’s database. Importantly, this is not a simple “Kafka → DB copy”: sinks typically apply business rules and policies, filter/validate records, enrich or normalize fields, and sometimes join/aggregate data before writing it. The output is a materialized view that matches the service’s needs (usually written via idempotent upserts).


A large part of the data that lands in Kafka comes from databases via **Debezium** . Debezium is a Change Data Capture (CDC) platform that monitors database transaction logs and streams every row-level change (inserts, updates, deletes) into Kafka topics in real time. Combined with **Kafka log compaction** , each topic retains only the latest value for every unique key—effectively providing a continuously updated snapshot of the current database state rather than an endless event history.


This setup is ideal for the materialized view pattern: a sink can bootstrap its local database by consuming the compacted topic from the beginning, and then continue applying incremental changes as they arrive. The result is a reliable, eventually consistent replica that can be rebuilt from Kafka at any time, without needing direct access to the source database.


To make this concrete, let’s follow one example end-to-end: **accommodation reviews** .


- The data team (or an upstream system) produces review updates—new reviews, edits, or deletions.
- These changes are published to a Kafka topic.
- A sink subscribes to that topic, consumes events, applies transformations, and writes into a local database.
- Our backend APIs and website read from that database, so updated reviews can be served to users with low latency.


In short: **Kafka carries the change events, sinks materialize them into query-friendly storage, and the platform serves user-facing features from that materialized data.**


## The Problem: Idle Sinks Burning Resources


Before diving into the solution, let’s clarify the problem we’re trying to solve.


Our Kafka sinks are mostly waiting for new messages, meaning they’re idle most of the time. On average, *each sink* consumes around **1 CPU core and 1 GB of memory** . Even if a single sink is “small”, running **50+ sinks** —and running them in **three regions (US, EU, ASIA)** —quickly adds up to a significant amount of wasted cluster capacity.


Idle sinks don’t just waste compute resources. They also:


- **Increase Kubernetes control-plane churn.** Even “idle” pods still generate regular activity like readiness/liveness probes, endpoint updates, metrics scraping, and frequent status updates. At scale (50+ sinks × 3 regions), this adds noise and background load to the cluster control plane.
- Increase baseline **network traffic** and **monitoring/logging** load (metrics scraping, log shipping, tracing, etc.).
- Add steady pressure on **Kafka brokers** , since consumers still poll frequently to check for new messages.


## Why Traditional Autoscaling Wasn’t Enough


Traditional autoscaling doesn’t help much here. **CPU- and memory-based scaling isn’t a good signal for sink workloads** : processing messages often doesn’t cause a strong or consistent increase in CPU or memory usage. Many sinks spend most of their time blocked on I/O (polling Kafka, waiting on database writes, waiting on network), so resource usage can look “flat” even when they are actually falling behind.


This creates a few concrete problems:


- **CPU stays low while lag grows**
A consumer can be “busy” in the sense that it has a lot of messages to process, but CPU won’t necessarily spike—especially if the bottleneck is external (database latency, network throughput, rate limits, downstream services). The outcome is that HPA doesn’t scale when it should, and Kafka lag can grow silently.
- **Memory isn’t a backlog indicator**
For most sinks, memory usage is relatively steady (JVM heap, caches, connection pools). It doesn’t correlate with “how much work is waiting in Kafka”. So memory-based scaling either does nothing or forces you into thresholds that are unrelated to actual demand.
- **The signal arrives too late (or never)**
Even when CPU does increase, it may happen only after the service is already overloaded (e.g., retries, slow DB writes, longer processing times). At that point, scaling up is reactive and can’t prevent the backlog from building up.
- **Burst traffic makes manual replica settings expensive**
Most topic activity comes in bursts (nightly imports/cronjobs). To handle those bursts quickly, we typically configure more replicas or higher minimum replica counts. But outside of those windows, those replicas just sit idle—consuming resources 24/7.


In short, CPU/memory autoscaling answers the question: *“Is this pod using resources?”* —but for Kafka sinks, we really need to answer: *“Is there work waiting to be processed?”* The most direct signal for that is **Kafka consumer lag** , not CPU or memory.


## Event-Driven Scaling with KEDA


CPU/memory autoscaling works great when resource usage is a good proxy for “how busy” an application is. For our sinks, it isn’t. A sink can be doing real work (polling, waiting on Kafka, writing to the database) without ever showing a meaningful CPU spike—and during long idle periods, it still costs us capacity just by staying alive. We needed a way to scale based on the only signal that actually matters: **is there data waiting to be processed?**


That’s what led us to **KEDA** . Instead of asking “how much CPU is this pod using?”, KEDA lets Kubernetes ask “how many events are waiting right now?”. Depending on the system, that “event” can be queue depth, a Prometheus metric, or—in our case— **Kafka consumer lag** . When lag appears, scale up; when lag is gone, scale down.


Under the hood, KEDA does this through small adapters called **scalers** . Each scaler knows how to query a specific external system (Kafka, Prometheus, cloud queues, etc.) and turn what it finds into an autoscaling signal that Kubernetes can act on. The feature that really mattered for us is that KEDA can go one step further than standard HPA and **scale a Deployment all the way down to zero** . For sinks that are idle most of the time, that’s the difference between “always paying a baseline cost” and “only paying when there’s actual work.”


Let’s explain how[KEDA works](https://keda.sh/docs/2.19/concepts/) with an example.


Assume we have a worker` Deployment` that processes messages from a queue:


1.


**Define a ScaledObject**
You create a` ScaledObject` (conceptually similar to an HPA configuration) where you specify:


- the target workload (your Deployment)
- the scaler to use (e.g., queue depth / Kafka lag)
- scaling boundaries like **min/max replicas** (often` minReplicaCount: 0` )


2.


**KEDA polls the external system**
KEDA periodically queries the external system through the selected scaler to retrieve the current value (e.g., number of pending messages, Kafka consumer lag).


3.


**KEDA exposes metrics to Kubernetes**
KEDA converts that external value into Kubernetes autoscaling metrics.


4.


**Kubernetes scales the Deployment**
KEDA creates and manages an HPA for the target workload. When the metric crosses a threshold, Kubernetes scales the Deployment (e.g.,` 0 → 3 → 6` replicas).


5.


**Workload processes events**
More pods consume messages faster and reduce the backlog/lag.


**Note:** Kubernetes HPA alone does not support scaling from 1 to 0 replicas. KEDA enables scale-to-zero by managing the scaling behavior and HPA configuration around it.


## Before vs After


Before diving into YAML and tuning parameters, it helps to visualize what actually changed. The data pipeline itself stayed the same—Kafka is still in the middle and sinks still materialize data into local databases—but the **runtime behavior** of those sinks changed completely once we introduced KEDA.


In the “before” world, we had Kafka topics in the middle and a fleet of sink services continuously consuming from them and writing into a database to keep local materialized views up to date. The key detail is that the sinks were **always running** , even when nothing was happening on the topic.


Most of the day, they were simply **polling Kafka and waiting** —ready to process the next change, but otherwise doing no useful work while still consuming CPU, memory, and the usual per-pod overhead (network connections, metrics/logging, etc.).


With KEDA in place, the architecture stays the same—Kafka is still the transport layer and sinks still materialize data into the database—but there’s a new component making the key decision: **when should a sink actually be running?** Instead of keeping every consumer alive “just in case”, KEDA watches Kafka lag and only starts pods when there’s backlog to process. When the topic is quiet, and lag drops back to zero, it scales the sink back down again.


Here’s what the flow looks like now:


1. Data is published to Kafka (for example, via imports/cronjobs or CDC).
2. KEDA continuously checks the **consumer group lag** for the sink’s topic(s).
3. If lag rises above the activation threshold, KEDA scales the sink **from 0 to N replicas** .
4. The sink consumes and processes messages, writing the results into the database/materialized views.
5. Once lag is drained and stays low for the cooldown period, KEDA scales the sink **back to 0** .
6. Our website continues to read from the database as before—only now we pay for sink compute **only when there’s work** .


## Our Solution


We use **KEDA** with the **Kafka scaler** to scale our sinks based on **consumer group lag** . The following` ScaledObject` shows a simplified configuration:


```text
apiVersion  :   keda.sh/v1alpha1
kind  :   ScaledObject
metadata  :
name  :   sink-foo
spec  :
cooldownPeriod  :   300
fallback  :
failureThreshold  :   6
replicas  :   1
maxReplicaCount  :   1
minReplicaCount  :   0
pollingInterval  :   30
scaleTargetRef  :
name  :   sink-foo
triggers  :
-   metadata  :
activationLagThreshold  :   '0'
bootstrapServers  :   kafka.kafka:9092
consumerGroup  :   foo-consumer-group
lagThreshold  :   '100'
topic  :   hotel-topics
type  :   kafka
```


Let’s explain each configured parameter:


` cooldownPeriod: 300`


How long KEDA waits after the trigger becomes “inactive” before scaling down further (including back to 0).
This avoids scale “flapping” when lag oscillates around the threshold or when bursts arrive in short waves.


```text
fallback  :
failureThreshold  :   6
replicas  :   1
```


Fallback is a safety net: if KEDA cannot fetch metrics (Kafka unavailable, auth issues, networking problems, etc.) for` failureThreshold` consecutive checks, it scales the workload to the configured number of replicas. We use` replicas: 1,` so the sink keeps running “safely” rather than being stuck at 0 due to missing metrics.


` pollingInterval: 30`


How often (in seconds) KEDA queries Kafka to evaluate the trigger (lag).
Smaller values react faster but create more frequent polling.


` minReplicaCount: 0` enables **scale-to-zero** , which is the key to eliminating idle sink costs.


` maxReplicaCount` puts an upper bound on how much the sink can scale out.


For a Kafka consumer group, **each partition can be processed by at most one consumer instance at a time** . That means scaling a single sink deployment beyond the **number of partitions** usually won’t increase throughput—it will just create extra pods that sit idle, because Kafka can’t assign them any partitions.


A good rule of thumb is:


- **effective max replicas ≤ number of partitions** (for the topic(s) that consumer group reads).
- In practice, you may set it even lower if the sink is **database-bound** or you want to limit the downstream load.


```text
scaleTargetRef  :
name  :   sink-foo
```


Points KEDA at the Kubernetes workload to scale (here a` Deployment` named` sink-foo` ).


The main part is` Kafka Scaler` configurations:


```text
activationLagThreshold  :   '0'
```


This controls when scaling should “wake up” from zero.


- If lag is **0** , KEDA considers the trigger inactive and can scale down to 0.
- If lag becomes **> 0** , KEDA activates scaling and brings up at least one replica.


(You can also set this to a small number > 0 if you want to avoid waking up for tiny/short-lived lag spikes.)


```text
bootstrapServers  :   kafka.kafka:9092
consumerGroup  :   foo-consumer-group
topic  :   hotel-topics
```


These define *where* KEDA should look and *which* consumer lag should drive scaling.


**Important:** the sink deployment and KEDA must reference the **same consumer group** ; otherwise, the lag KEDA sees won’t match the actual workload.


```text
lagThreshold  :   '100'
```


Target value for the total lag (sum of all partition lags) to trigger scaling actions.


If the measured lag is **greater than 100** , KEDA considers the workload under-provisioned and **scales up** .


If the lag is **at or below 100** , it tends to **scale down** .


KEDA tries to keep lag around the threshold by setting desired replicas roughly as follows:


- **desired replicas ≈ totalLag / 100**
- Example:


- totalLag = **50** → ~0–1 replicas (often scales down)
- totalLag = **250** → ~3 replicas
- totalLag = **1200** → ~12 replicas (then capped by` maxReplicaCount` )


## What this gives us in practice


With this approach:


- sinks run at **0 replicas** when there is no work
- When messages arrive, and lag grows, KEDA scales the sink up
- The sink drains the lag
- Once lag returns to 0 and stays there for the cooldown period, KEDA scales back down to **0**


## Edge case: consumer group cleanup for very low-traffic topics


We discovered an operational edge case. A **consumer group** is the Kafka concept that represents a set of consumers sharing the work of reading a topic, and it’s also where Kafka tracks their progress via committed offsets. We have a policy that removes **inactive consumer groups from Kafka** (i.e., their stored group metadata/offsets in the cluster) if they haven’t been active for more than two weeks. Some topics receive updates very rarely (for example, once per month), so if a sink stays scaled to zero for too long, its consumer group may be removed—causing extra recovery work (and potentially slower catch-up) when messages finally arrive again.


To prevent that, we add a **Cron scaler** to periodically “wake up” the sink and keep the consumer group active:


```text
-   metadata  :
desiredReplicas  :   '1'
end  :   5 2 * * 0,6
start  :   0 2 * * 0,6
timezone  :   Europe/Berlin
type  :   cron
```


This scales the sink up to 1 replica for a short window once (or twice) per week.


**Note:** We deliberately generate different` start/end` windows per sink in our Helm chart so that not all sinks wake up at the same time, keeping the cluster impact minimal.


## Migration Path


We were initially skeptical about **scale-to-zero** . These sinks feed local materialized views, and if a sink doesn’t scale up quickly enough (or fails to start), we risk serving **stale data** on the platform—reviews, highlights, and other user-facing content.


To reduce that risk, we rolled KEDA out gradually:


1. **Start with a non-critical sink**
We picked one sink where a short delay would have minimal user impact.
2. **Enable KEDA in a single region (ASIA)**
We turned on KEDA-based scaling only in one Kubernetes region first to limit blast radius.
3. **Run it in production for a week**
We kept this setup for one full week to cover different traffic patterns and at least one nightly import cycle.
4. **Expand to all regions for that sink**
Once the behavior was stable, we enabled the same configuration in the **US and the EU** .
5. **Roll out to the rest of the sinks**
After proving the approach end-to-end, we applied it to the remaining sinks, typically in small batches rather than all at once.


After each step, we validated correctness and responsiveness by comparing:


- **Kafka-reported lag** and our existing consumer metrics, and
- **The metrics KEDA exposed to Kubernetes** (the signals actually driving scaling)


This gave us confidence that scaling decisions matched real backlog, and that scaling to zero wouldn’t compromise data freshness.


## Results


After introducing KEDA-based autoscaling (utilizing Kafka lag as the scaling signal), our sinks ceased consuming resources when there was no work to do.


- **Idle sinks now scale to zero.**
For most of the day, our Kafka topics don’t change, so most sinks now sit at **0 replicas** instead of running continuously and wasting CPU/memory just to poll. In terms of utilization, this reduced our average daily consumption from about **50 replica-hours per region** to roughly **1–2 replica-hours per region** .
- **Automatic “burst handling” without manual replica tuning.**
When nightly cronjobs/imports produce bursts of events, consumer lag increases, and KEDA scales the relevant sinks up automatically. Once the backlog is processed and lag returns to 0, KEDA scales them back down again.
- **Less baseline cluster load across regions.**
Because we run the same pattern across **US/EU/ASIA** and have **50+ sinks** , removing always-on replicas significantly reduces baseline consumption in each regional Kubernetes cluster (compute, networking, and observability overhead).
- **More reliable scaling signal.**
Scaling on Kafka lag matches the actual problem we care about (“how much work is waiting”), unlike CPU/memory signals, which were often flat during processing and therefore didn’t scale when needed.


## Conclusion


If we take one thing away from this solution, it’s this: **scale on work, not on resource usage** . For event-driven workloads—Kafka sinks, queue workers, Redis-backed jobs—the most reliable scaling signal is the **backlog** (consumer lag/queue depth), not CPU or memory.


Rolling this out taught us a few practical lessons.


First, **validate the signal before you scale the fleet** : comparing Kafka lag and our existing consumer metrics with what KEDA exposed to Kubernetes was what made scale-to-zero feel safe.


Second, the outcome depends heavily on a small set of knobs—` lagThreshold` ,` cooldownPeriod` , and` maxReplicaCount` —so we’d invest earlier in tuning those per sink type (and in setting sensible defaults in our Helm chart). And finally, proactively identify edge cases like **very low-traffic topics** and bake in a keepalive strategy (Cron scaler) from the start.


The net result is simple: **we shifted a large set of “always-on” deployments into true on-demand workloads** , and that translates directly into lower steady-state cost across our regions.
