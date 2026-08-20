---
schema_version: "1.0.0"
document_id: "0cea8da98fc8dfc7d0273daceaa41bf4f28286336284fb700217cdb7358625bc"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/scaling-database-availability-without-disruption/"
published_at: "2026-07-21T13:37:09+00:00"
first_seen_at: "2026-07-22T13:37:02.744795+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:0ee7682855bf5a8e3e17141af5bf6cd619266fca81457b65daa3dc4241931a71"
---

# Scaling Database Availability Without Disruptio

# Scaling Database Availability Without Disruption


## A database should not have to be rebuilt because an application became more successful.


Jul 21, 2026


•


6 min read


•


[Micah Bhakti, Director of Product Management](https://www.singlestore.com/blog/author/micah-bhakti/)


When a high availability database is created, an administrator decides whether to run in a single Availability Zone to minimize infrastructure cost or across multiple Availability Zones to tolerate an AZ failure. That decision becomes part of the deployment architecture and, in many database systems, changing it later is surprisingly disruptive.


Applications rarely remain static long enough for that assumption to hold.


Development systems become production systems. Internal services become customer-facing APIs. AI applications that initially tolerate maintenance windows become continuously available because they are now embedded inside business workflows. At the same time, some workloads move in the opposite direction. Disaster recovery environments, seasonal applications, and analytics systems may no longer justify the infrastructure cost of cross-AZ redundancy.


Infrastructure should evolve with those requirements.


With SingleStore's Online AZ Migration, customers can reconfigure a running deployment between single-AZ and multi-AZ operation without taking applications offline or migrating databases. The capability is useful in its own right, but more importantly it illustrates an architectural advantage of distributed SQL. The operation is possible because the database is built around independently managed partitions rather than a single machine that owns the entire dataset.


## **Availability in Traditional Databases**


Databases such as Amazon RDS for MySQL and PostgreSQL fundamentally revolve around a primary database instance, with one DB instance serving as the primary server.


The primary owns every page of the database, executes every write, and serves as the authoritative copy of the data. High availability and automatic failover are achieved by maintaining one or more standby servers that continuously replicate changes from the primary.


This architecture works well, but it makes availability topology part of the database itself. A single-AZ deployment consists of one primary server. A Multi-AZ deployment introduces additional infrastructure, replication channels, synchronization logic, failover coordination, and monitoring. Although cloud providers automate much of this operational complexity, the database still moves between two fundamentally different deployment models.


Changing that architecture later typically requires provisioning a new database instance or additional infrastructure, synchronizing the complete dataset, validating replication, coordinating maintenance, and accepting at least some operational disruption while the topology changes.


The difficulty is not an implementation detail, it is a consequence of the database having exactly one owner.


## **Distributed SQL Changes What "The Database" Means**


SingleStore distributes ownership across an entire cluster.


Instead of one machine containing every table, each table is divided into many independent partitions. Every partition has one or more replicas that are distributed across leaf nodes throughout the cluster.


Applications continue to see a single SQL database.


They connect through aggregators, submit ordinary SQL queries, and receive ordinary SQL results. Underneath, however, the query planner decomposes work into partition-level operations that execute in parallel across many machines before the results are combined.


The important distinction is that no individual server owns the database. Servers own partitions. The database exists as the collection of those partitions.


Once ownership moves from servers to partitions, infrastructure becomes dramatically more flexible. Capacity expansion, node replacement, rebalancing, hardware maintenance, and availability changes all become different versions of the same operation: moving partitions while the database remains online.


## **Partition Distribution and Leaf Fanout Failover**


Every partition inside SingleStore has one or more replicas. Those replicas are intentionally distributed according to the configured availability policy. In a single-AZ deployment, replicas may remain inside one Availability Zone while still protecting against machine failures.


In a multi-AZ deployment, replicas are distributed across multiple Availability Zones so that losing an entire AZ still leaves every partition available somewhere else in the cluster.


This architecture also changes how automatic failover is handled. Traditional databases generally fail over entire database servers. SingleStore fails over individual partitions.


If a leaf node becomes unavailable, the distributed query engine simply redirects work to healthy replicas for the affected partitions. The remainder of the query plan continues executing normally across the rest of the cluster. SingleStore refers to this capability as **leaf fanout failover** .


Rather than waiting for ownership of an entire database server to move elsewhere, the execution engine fans query execution across whichever replicas remain available. Recovery occurs at partition granularity instead of server granularity, allowing failures to be isolated to a much smaller portion of the system.


## **Online AZ Migration**


Online AZ Migration applies exactly the same architectural principles.


Suppose a workload begins life as a cost-optimized single-AZ deployment. Months later, it becomes business critical and must survive the loss of an Availability Zone. With traditional architectures, this usually means introducing a different replication topology.


With SingleStore, the logical database remains exactly where it is. The cluster first provisions additional leaf nodes inside the target Availability Zones. Once the additional capacity is available, the database begins redistributing partitions throughout the expanded cluster.


New replicas are created online, existing partitions are relocated, and placement policies are updated until every partition satisfies the desired redundancy configuration.


Throughout the process:


-


Applications continue using the same endpoints.


-


Existing connections remain valid.


-


Queries continue executing.


-


Transactions continue committing.


-


Data remains available.


The distributed query planner automatically routes requests to the appropriate partition replica as ownership changes during redistribution.


When migration completes, the workload now operates as a multi-AZ deployment. The reverse operation is equally valuable.


Organizations frequently discover that development, test, analytics, or disaster recovery environments no longer justify multi-AZ infrastructure costs. Those deployments can be returned to single-AZ operation using the same online redistribution process, eliminating unnecessary infrastructure without requiring another migration project.


Availability becomes an operational policy instead of a permanent architectural commitment.


## **Why Enterprise Customers Care**


Large organizations rarely operate databases whose requirements remain constant.


Applications become more important over time. Service-level objectives become stricter. Compliance standards change. Customer adoption increases. Geographic expansion introduces new resiliency requirements.


The traditional response has been migration. Migration projects are expensive not because copying data is inherently difficult, but because every surrounding activity introduces operational risk. Maintenance windows must be negotiated. Replication must be validated. Rollback procedures must be tested. Application teams must coordinate deployment schedules. Infrastructure teams must provision new environments while production systems continue operating.


For business-critical systems, avoiding the migration often becomes more valuable than accelerating it. Distributed SQL changes the equation because changing availability no longer requires changing the database architecture itself.


The database continues operating as the same logical system. Only the placement of partitions changes.


For continuously available SaaS platforms, financial systems, customer-facing APIs, and AI applications that cannot tolerate planned downtime, that distinction removes an entire class of operational work that has historically accompanied infrastructure evolution.


## **More Than a Feature**


Online AZ Migration is best understood as an architectural benefit rather than an isolated capability.


When data is partitioned, replicated, and independently managed across a distributed SQL cluster, the database becomes substantially easier to evolve. Infrastructure can expand. Nodes can be replaced. Hardware can be refreshed. Capacity can be rebalanced. Availability policies can change.


Each operation follows the same underlying principle: redistribute partitions while the database continues serving requests.


That is fundamentally different from architectures where an individual server owns the database.


As enterprise systems become larger and remain in production for longer periods of time, the ability to adapt infrastructure without interrupting applications becomes increasingly important. Availability is no longer something chosen once during deployment. It becomes another property of a distributed system that can evolve as business requirements evolve.


## On this page


- Availability in Traditional Databases
- Distributed SQL Changes What "The Database" Means
- Partition Distribution and Leaf Fanout Failover
- Online AZ Migration
- Why Enterprise Customers Care
- More Than a Feature


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fscaling-database-availability-without-disruption%2F)


[Product](https://www.singlestore.com/blog/category/product/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Security Engineering, Not Just Security Features Product](https://www.singlestore.com/blog/cloud-database-security-engineering-sdlc/)


[Blog Enterprise AI Needs Real-Time Data. The Industry Is Finally Catching Up. Product](https://www.singlestore.com/blog/enterprise-ai-needs-real-time-data-the-industry-is-finally-catching-up/)


[Blog Why Real-Time Analytics Is the Next Battleground for Revenue Operations Platforms Product](https://www.singlestore.com/blog/why-real-time-analytics-is-the-next-battleground-for-revenue-operations-platforms/)


[Blog Build and Deploy an App Prototype with an AI Agent using MCP in an Afternoon Product](https://www.singlestore.com/blog/build-and-deploy-an-app-prototype-with-mcp-and-ai-agent/)


[Blog Retrieval-Augmented Generation (RAG) for Real-World Machine Learning Engineering](https://www.singlestore.com/blog/retrieval-augmented-generation-rag-for-real-world-machine-learning/)


[Blog Data Intelligence Is the Missing Piece in Your AI Strategy Product](https://www.singlestore.com/blog/data-intelligence-is-the-missing-piece-in-your-ai-strategy/)
