---
schema_version: "1.0.0"
document_id: "fbeda13b802d1bad1354c8ccd5eac589bf6514dcfb2fc9e38a080e21110555c6"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/ending-on-call-nightmares-architectural-lessons-from-distributed-caching-with-apache-ignite-78eec9123d31"
published_at: "2026-07-06T10:16:36+00:00"
first_seen_at: "2026-07-20T23:17:33.321656+00:00"
fetched_at: "2026-08-20T03:52:32.830617+00:00"
content_hash: "sha256:95f84fa4cb1540c300c39d5ad28bf45f5820e91d3611e15f4ae40f78a73b612b"
---

# Ending On-Call Nightmares: Architectural Lessons from Distributed Caching with Apache Ignite

*Written By:*[Shreyas Bhandare](https://www.linkedin.com/in/sbhandare/)


### **The Nightmare**


Picture this: It’s Friday evening, you’re on-call, and your entire in-memory Apache Ignite cluster just went down. A simple data streamer that consumes data from Kafka and puts it into Ignite crashed. Why did a single data streamer take down the entire cluster? Apache Ignite 2.15 introduced a complicated bug that broke existing security contexts.


This happened to us.


To give some background, we built a mission-critical caching layer on Apache Ignite to replace legacy cache services. The cluster stores billions of off-heap records, amounting to a couple of hundred gigabytes and ingests a high volume of real-time updates streamed from Kafka. Through Ignite’s Continuous Query, about 500 upstream hosts always receive fresh data.


We had a clean, separated architecture:


- **Ignite Cluster:** Kubernetes StatefulSets running on server nodes that host our caching layer.
- **Streamers:** Kubernetes Deployments running our Kafka consumers. Streamers are a part of the Ignite cluster as thick clients.


This worked beautifully until we migrated to Ignite 2.15. After the upgrade, even a routine streamer restart — due to deployments, pod evictions or GKE upgrades — could trigger node crashes, causing server pods to enter an error loop with the exception:


*JVM will be halted immediately due to the failure: \[failureCtx=FailureContext \[type=SYSTEM_WORKER_TERMINATION, err=java.lang.IllegalStateException: Failed to find security context for subject with given ID\]*


### An Operational Breaking Point


What started as an occasional on-call scare quickly became an operational hazard:


- **Random cluster deaths:** Routine streamer deployments turned into Russian roulette, and any restart could take down server nodes.
- **Security context exceptions:** Server pods repeatedly fell into the same error loop, requiring remediation steps to restore stability.
- **Outages** : Each incident required cluster recovery, which at times was extensive.
- **Deployment paralysis:** Releases required careful coordination due to deployment instability.
- **Data loss:** When enough nodes failed, upstream data freshness was temporarily impacted.
- **Operational overhead:** We had to maintain two Ignite clusters in each region as a fallback during deployments. We implemented additional redundancy measures to support safe deployments.


One may wonder… was upgrading to version 2.15 really necessary?


In our case, the other extensive bug fixes, new features and enhancements in 2.15 clearly outweighed the benefits of staying on 2.14. And, as with any other engineering team at DV, we performed thorough due diligence, running comprehensive stress and chaos tests across all environments before proceeding with the upgrade.


As we investigated further, we realized we weren’t the only ones fighting this fire.


### Understanding the Systemic Issue


Apache Ignite’s[IGNITE-21737](https://issues.apache.org/jira/browse/IGNITE-21737) documents this exact issue: thick clients with security enabled can cause server nodes to become unresponsive during client disconnections. The security context validation gets stuck in loops, essentially freezing the entire server node. It was a systemic issue affecting anyone using thick clients with security enabled in Ignite 2.15+.


At this point, the next logical question was: **how do we fix it?**


### Architectural Shift


The primary reason for using Thick Client Streamers was their low overhead. The original solution leveraged Apache Ignite’s built-in *IgniteDataStreamer* and *StreamAdapter* , where the *addMessage()* method handled populating the Ignite cache. Since this option was provided out of the box by Ignite, we just had to implement our Kafka consumers around it, keeping the overall design simple.


Before committing to an architectural turnaround, we tried every reasonable alternative. We experimented with quick bug fixes, adjusted our security plugin, added defensive guards around client disconnects and explored custom failure handlers. None of it addressed the real problem, because the failure pattern didn’t originate in our streamer logic or infrastructure. No amount of defensive coding could prevent it.


And so, it became clear: **as long as our streamers connected as thick clients, the cluster would remain unstable.**


The only reliable solution was to remove this dependency altogether, shifting the streamers to thin clients, which avoided the problematic server-side security context handling entirely. This change also gave us several additional advantages:


But moving to thin clients wasn’t just a configuration flip. It fundamentally changed how our streamers interacted with Ignite, introducing a new set of engineering challenges.


### Rebuilding from the Ground Up


Things got a little complicated when we decided to migrate the data streamers to thin clients. The existing utility classes weren’t compatible out of the box, which meant we had to build a significant amount of custom integration logic to achieve the same functionality.


We also had to rebuild resilience from scratch. Because thin clients don’t participate in cluster events, we could no longer depend on Ignite’s internal signals to detect failures or restart streamers. Instead, we developed our own health-check probes that continuously monitored client connectivity and consumer thread health, triggering safe, automated restarts whenever a disconnection occurred.


With these challenges laid out, we moved on to designing and implementing the new streaming pipeline.


### The Technical Journey


There were three steps to implement the whole migration process.


#### Step One: Building the Thin Client Streamer logic


We developed a new streamer class that leveraged Ignite’s thin client APIs to connect to our StatefulSets. The key innovation was partition-based buffering: instead of processing all records at once, we grouped incoming Kafka messages by partition and flushed them in controlled batches. This strategy helped prevent cluster overload while maintaining high throughput.


We also plugged in Ignite’s partition awareness feature, which was supported out of the box. This allowed thin clients to route data requests directly to the server nodes that own the relevant data partitions, resulting in faster writes.


A multi-threaded model allowed us to consume from multiple Kafka partitions in parallel. Each thread maintained its own *IgniteClient* instance to efficiently write data to the cache using *putAll()* and *removeAll()* .


We validated that the behavior of *putAll()* and *removeAll()* closely matched our original *IgniteDataStreamer* implementation, meaning that during a full data load, the thick-client streamer and thin-client streamer performed exactly the same. To continuously monitor performance, we added Prometheus summary metrics to track throughput and latency over time. On full data load, the thick-client streamer and thin-client streamer performed exactly the same.


To ensure reliability, we also built in safeguards for edge cases, including retry mechanisms, rollback handling for failed Ignite writes, and recovery logic for Kafka offset commit failures.


#### Step Two: Health Monitoring Infrastructure


We built our own monitoring, and being on Kubernetes helped us build an easy and workable solution:


- Check if Kafka Consumer Threads are healthy
- Check if IgniteClients are healthy
- If any of the above fail, restart the data streamer after a delay


#### Step Three: Moving Actual Datasets


This was the moment of truth: switching our real datasets to use the new thin-client streamer. The beauty of our approach was that it required only configuration changes — no code changes for individual datasets, just Helm chart updates and property file modifications. We could migrate datasets one by one, testing each one before moving to the next.


### What Made This Work


This approach completely sidestepped the security context issue. Since thin clients don’t create the same security context dependencies as thick clients, the Ignite server nodes don’t get stuck in error loops. When we restart a streamer deployment, it’s just a regular client disconnect/reconnect, meaning no impact on server nodes. Recall the earlier separated-architecture diagram. Moving to thin clients allowed us to decouple the system even further:


### An Absolute Operational Win


After we went live, the results were positive and immediate. Earlier, we used to spend two to six hours per incident, sometimes multiple incidents per month. It went down to zero.


There were significant operational improvements as well. We could deploy streamers and server nodes with 100% confidence. GKE upgrades no longer need careful scheduling. The person on-call would spend time improving monitoring and alerts rather than firefighting the Ignite cluster outages. Thanks to the new health check probes, we eliminated the need to call SRE support for any manual intervention.


### Looking Back from the Other Side


We found that Ignite security contexts can fail in severe and unexpected ways, and that connection type plays a critical role in overall cluster stability. We also discovered that thin clients are far more capable than we had previously assumed — something we had never fully explored in the past.


Ultimately, the most effective solution wasn’t a patch or waiting for a new Ignite release, but an architectural shift that addressed the root cause at the connection layer. Today, this architecture powers DV’s low-latency, high-throughput platform, processing hundreds of billions of pre-bid requests daily across multiple DSPs while consistently meeting strict SLAs. It has transformed our streaming pipeline into a resilient and stable foundation we can fully rely on.


---


[Ending On-Call Nightmares: Architectural Lessons from Distributed Caching with Apache Ignite](https://medium.com/doubleverify-engineering/ending-on-call-nightmares-architectural-lessons-from-distributed-caching-with-apache-ignite-78eec9123d31) was originally published in[DoubleVerify Engineering](https://medium.com/doubleverify-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
