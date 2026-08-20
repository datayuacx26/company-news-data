---
schema_version: "1.0.0"
document_id: "e2bceecce6b52bd024b20d14024d1dfbdda84ae0d28a2d7841fd69e854e43d3d"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/reliable-log-delivery/"
published_at: "2025-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:500fd5e9706dee7bf40e742eed7764d0760a2ce8710e9c355ddc808716da2375"
---

# How we built reliable log delivery to thousands of unpredictable endpoints

Gabriel Reid


Package delivery services like UPS, FedEx, and the postal service have a tough job. They contend with a never-ending stream of packages to be delivered, with expectations of prompt and reliable delivery. In building the[Datadog Log Forwarding](https://www.datadoghq.com/blog/route-logs-with-datadog-log-forwarding/) feature, we had to contend with similar challenges.


Running a package delivery service initially sounds relatively simple, but consider details like:


-


How do you use resources efficiently to get packages delivered?


-


How do you avoid wasting time revisiting an address when no one is there to receive the package?


-


What do you do if a package has a bad or non-existent address?


-


How do you make sure packages actually get delivered?


While building the Log Forwarding feature in Datadog, we realized that—like delivering physical packages—moving logs from one system to another sounds simple at first. But at Datadog’s scale, with thousands of tenants and endpoints that can be slow, flaky, or unavailable, the problem gets a lot more complicated. Exploring these challenges gave us a new appreciation for package delivery workers—they’re solving a lot of the same problems we were.


In this post, we’ll walk through how we designed and built Datadog’s Log Forwarding functionality to provide low latency, high throughput, and reliable multi-tenant log delivery—even when forwarding to thousands of unpredictable external endpoints.


## What is Log Forwarding?


Datadog Log Forwarding acts as a digital delivery service for logs, sending processed and enriched logs that have been ingested into Datadog in the form of schemaless JSON records. Log Forwarding enables our customers to send logs to virtually any external destination, including ElasticSearch, Splunk, or generic HTTP endpoints that can handle POST requests containing JSON.


### How logs are handled internally in Datadog


If you visit a package distribution center, you’ll see packages zooming around on multiple conveyor belts. Logs are treated in a similar way within Datadog, being transported around on[Kafka](https://www.youtube.com/watch?v=J7RRJ1iBeAg) topics in an orderly fashion.


You can read more in-depth details on how we transport data on Kafka in[Husky: Exactly-once ingestion and multi-tenancy at scale](https://www.datadoghq.com/blog/engineering/husky-deep-dive/) , but for the purposes of this post, the most important thing to know is that each Kafka partition—the equivalent of a single conveyor belt—functions in a first-in, first-out (FIFO) fashion. Logs come out of a Kafka partition in the same order they went in. Additionally, data for diverse destinations is distributed over multiple Kafka partitions, which means we must eventually also regroup the data for a given destination back together.


Just as having a single conveyor belt per destination in a distribution service is both simple and completely infeasible, the same is true of having a single Kafka partition per logs destination. This means that, as we deliver logs to destinations, we need to deal with the fact that not all logs going to the same destination are already grouped together.


## Challenges of reliably sending logs to multiple external endpoints


In general, the concept of forwarding logs (packages, in our analogy) to an external HTTP destination endpoint (a delivery address) is relatively straightforward. Assuming the logs are already in the appropriate format, you can send a simple HTTP POST request to the endpoint to forward the log data. The fact that the forwarded data is being read from one or more Kafka partitions does not immediately complicate things. Logs can be read from Kafka in order and then sent to the external endpoint.


However, the complexity increases once you dig into various failure scenarios and details, such as:


-


What happens if the destination endpoint is temporarily unavailable?


-


How is a sufficiently high level of throughput maintained along with efficient use of resources, particularly if we’re making a call to an external endpoint for every log to be delivered?


-


How do you ensure that you don’t DDOS a customer’s endpoint?


Package delivery services deal with many of these same challenges and have developed highly efficient ways of handling them. For example, packages going to the same address are “buffered” in a distribution center, and then delivered in batches so that the delivery driver drops off multiple packages at one time. When nobody is home to receive a package, the packages are brought back to a distribution center so that delivery can be retried later. It seems obvious that a delivery service wouldn’t insist on delivering packages in FIFO order, and this is where applying real-world lessons to the constraints of software also becomes more complex.


Implementing the equivalent logic in software requires several non-obvious decisions, partly due to Kafka’s strictly FIFO behavior. For example, should you wait until each forwarding request has returned before reading more data from Kafka, or optimistically continue to forward data and risk dropping data or sending duplicates? Coordination between forwarding data and waiting for successful delivery becomes even more complex when handling data from multiple tenants and forwarding to many different destination endpoints concurrently.


A key challenge is that the destination endpoints are entirely outside Datadog’s control. In the same way that a delivery service has no idea whether someone will be home to receive a package at a destination address, we can’t predict when forwarding endpoints will become temporarily unstable—or even unavailable—for hours or days at a time.


Given that losing customer data is unacceptable, duplicates must be avoided, and data in Kafka is ordered per partition, even a single unavailable forwarding endpoint can block progress. A consequence of the strict ordering of data in Kafka is that you can only acknowledge handling of items in a topic — **committing offsets** , in Kafka terminology—in the same order they were written to the topic. This means that if you block reading data from Kafka until you’re able to successfully forward the previously read data, then all data is delayed until the unhealthy endpoint is available again. The alternative—continuing to read and acknowledge data from Kafka without confirming successful delivery—is a direct path to data loss. The following diagram shows how a single unavailable destination can block all logs in the same Kafka partition—even those bound for other endpoints.


We already had a lot of experience with a similar problem in our[Log Archives](https://docs.datadoghq.com/logs/log_configuration/archives/) feature, although archiving logs could rely on much more reliable delivery endpoints provided by cloud provider object storage systems, as well as far lower expectations in terms of latency. We were also able to use this past experience to anticipate and avoid potential pitfalls with the new system we were building.


## Limitations of just using a huge number of Kafka topics


In this situation, what we really needed was the ability to group data by target destination. One way to achieve this would be to have one or more Kafka partitions per destination, and then simply route data to the appropriate partition. This would allow processing each destination’s data independently, which would mean that a temporarily slow or unavailable endpoint would not impact forwarding to other destinations. This approach would be similar to a package distribution center having one dedicated conveyor belt for each address it was going to deliver to.


However, taking this approach brings up another issue: **hot partitions** . Just as having one conveyor belt per address would mean that many conveyor belts would be empty, while others would be too small for their destination’s packages, having one dedicated partition is more than enough for some forwarding destinations, and not enough for others. On top of that, there would be the non-negligible cost and operational overhead to having one or more Kafka topics and partitions per customer-defined destination endpoint.


## Building the equivalent of an unlimited number of Kafka topics


What we ended up building—and the subject of this post—is **a system that offers the same isolation advantages of a Kafka topic per destination endpoint, without the drawbacks of worrying about the operational overhead of such a system** . The system also exhibits improved recovery of live data after a destination endpoint has been temporarily unavailable, which would not even be possible using per-endpoint Kafka topics. Continuing the package delivery analogy, we’ve built something similar to how package delivery services work—with a large storage area with packages grouped by destination, allowing undeliverable ones to wait without blocking packages ready for delivery.


To do this, we leveraged cloud object storage as a highly expandable distribution center to store data for multiple destinations in single large files, with the data already grouped by destination. This allows us to provide the abstraction of partitions of data per destination endpoint, with much lower overhead than maintaining multiple Kafka partitions at that level of granularity.


## The components of a massively scalable delivery system


### Groups of packages going to the same address: slice files


The first piece of this puzzle is the concept of **slice files** . The analogous concept in the package delivery world would be a section of a distribution center where packages are grouped by their destination address. In Log Forwarding, slice files are binary files that contain multiple groups—slices—of log records destined for a given endpoint, stored as opaque blobs of data in the format to be used for forwarding them. An important detail in the layout of a slice file is that **each slice is individually compressed within the file** . This allows us to address and read individual slices within a larger slice file when reading from cloud storage using range reads, meaning that we only need to download and decompress the exact slice that we’re interested in at any point in time.


A slice file also contains a header that summarizes the data contained within it, which allows rebuilding the metadata store if needed, as shown in the following diagram:


### Package intake: stager service


A **stager service** is the metaphorical equivalent of a distribution center’s intake process, where unsorted incoming packages are stored together with other packages headed to the same destination. The stager reads structured JSON logs from a set of Kafka partitions, filters the logs to retain the ones destined for one or more destination endpoints, and builds a compressed slice file. Every few seconds, a full slice file is built based on the in-memory state, uploaded to a staging area in cloud object storage, and registered in a metadata store, which is explained in the next section.


After all of this, the current Kafka positions are committed, acknowledging their successful handling, although in the meantime a new slice file is already being written to. After this point, the logs in the slice file won’t flow through another Kafka topic before they are forwarded, and just as with the intake of a package at a distribution center, this is where the FIFO handling of items ends.


### Keeping track of the location of all the packages: metadata store


As mentioned previously, there is a **metadata store** in which slice files are registered. A package delivery service has exactly the same thing, in the form of a database that tracks the location and state of all in-motion packages. Each slice within a slice file is individually registered, making it possible to get a listing of all slices for a given destination, including the slice file in cloud object storage where they’re located, and the byte offsets in that file.


### Coordinating which packages go where: planner service


Package delivery services coordinate the planning of delivering sorted packages, and log forwarding is the same in this regard. At this point in the process, slice files contain the data to be forwarded in cloud object storage, with each slice either containing enough data for a single forwarding payload, or needing to be combined with slices from other slice files to comprise a forwarding payload.


A leader-elected **planner service** constantly reads the metadata store to find slices that are eligible for forwarding, and constructs **tasks** of data to be forwarded. For each set of slices that are to be routed to a given endpoint, the planner constructs one or more forwarding tasks, depending on the size of the slices and the batching settings for the destination. It registers the tasks in a task queuing system, which is also used for many other tasks within Datadog. Note that these tasks contain only references to the slices to be forwarded to destination endpoints, so they’re lightweight.


The planner is aware of the buffering and batch size settings for each destination, and so is able to make specific decisions about the total number of records that must be available in a registered slice file before it plans a forwarding task for a given destination. We intentionally keep the planner simple and isolated, focused only on handling slices as they become available. This design lets us delegate complex tasks—such as retrying failures—to a downstream service: the shipper.


### Delivery vans bringing packages to their destination: shipper service


The **shipper service** is the delivery driver of our system. At this point, we’ve got tasks in the task queue, with each task pointing to one or more slices in one or more slice files to be forwarded. The individual slices are still in cloud object storage.


The shipper service then dequeues forwarding tasks, downloads the specific slices from within slice files hosted in cloud storage by using range read requests, performs final formatting and payload compression, and sends it to the endpoint.


Assuming that shipping the data to the destination endpoint is successful, which is indeed what happens in the majority of cases, the task is acknowledged and the slice is marked as forwarded in the metadata store.


If shipping to the destination endpoint is not successful—for example, because the endpoint is temporarily unavailable—the shipping task is rescheduled using exponential backoff.


## Putting it all together


The following diagram shows the conceptual flow of data as described above. Due to splitting up the various tasks within the system into services that focus on a specific facet of the whole process, the system has proven to be horizontally scalable and capable of processing Datadog’s full log traffic—which is a **huge quantity of data measured in gigabytes per second** —even though only a fraction of that traffic needs to be forwarded externally.


The actual interaction between the various services is shown in the following diagram:


This system is able to handle consistent low-latency forwarding, while at the same time it does not get held up by unreliable single forwarding destinations that occasionally encounter high failure rates. The following graph shows a snapshot of an incident involving a single unreliable destination for external forwarding. The red bars count failed calls to the destination that return 500 errors.


## Smooth recovery when a destination is unavailable


The system as described can forward data according to a globally defined batch size within seconds, while also indefinitely holding on to data for the subset of unavailable destination endpoints until they become available. This fully isolates the forwarding performance for each destination endpoint.


Another important case to consider is what happens when a previously unavailable destination endpoint becomes available and there is a sizable backlog of data to be forwarded to it. As an example, consider an Elasticsearch cluster that **receives data at a rate of 5,000 records per second** . If it is temporarily offline for two hours, there will be a **backlog of 36 million records** to be ingested.


In the package delivery world, the equivalent would be if you were to come back from a two-week vacation after forgetting to cancel a daily delivery of fresh fruit and vegetables. You’d be much more interested in first receiving today’s fresh raspberries and blueberries than over-ripe rotting bananas that have been sitting in a distribution center after multiple failed delivery attempts. To push the limits on this analogy, you probably also wouldn’t want to deal with all of your packages from the past two weeks at the same time.


Datadog has the capacity to send that data all at once, but that could overload the customer’s Elasticsearch cluster, leading to further unavailability. For that reason, an[additive increase, multiplicative decrease](https://en.wikipedia.org/wiki/Additive_increase/multiplicative_decrease) adaptive concurrency control system is used when forwarding the data. The rate of execution of forwarding tasks for that destination is automatically tuned to what the destination is able to handle without increasing error rate or latency, as shown in the following graph:


Another advantage of the independently queued forwarding tasks that have been requeued with exponential backoff is that **more recent data will be implicitly favored** . Log data is generally most useful in the first minutes after logs have been emitted, so we focus on forwarding **live logs** while also progressively backfilling data after a downtime. Note that this **restore live logs first** behavior is an important advantage of this design that we wouldn’t have with a per-destination Kafka partition.


All of this automatic recovery happens independently for each forwarding destination, with one destination’s recovery not impacting live log forwarding of all other healthy destinations.


## Final delivery: A scalable system with failure isolation


This project’s delivery had a lot of moving parts (pun intended). It presented a significant departure from our typical way of processing log data within Datadog, where we usually use Kafka to transport data from one service to another.


**Reliability and scaling tests of the full system made it clear that we made good choices with our design.** And nobody likes a noisy neighbor, so we can forward logs both to reliable endpoints and also to unreliable endpoints, with the poorly performing endpoints not affecting the reliable endpoint forwarding at all.


Fully separating the concerns of high-throughput processing, per-tenant locality, and task scheduling was a lot of work, but it proved to be well worth it. The design of this system has proven successful enough that we’re now migrating the[Log Archives](https://docs.datadoghq.com/logs/log_configuration/archives/) delivery system to take advantage of the increased reliability and cost-efficiency provided by this design.


If this type of work intrigues you, consider applying to work for our engineering team![We’re hiring!](https://careers.datadoghq.com/all-jobs/?s=logs)


-
-
-
