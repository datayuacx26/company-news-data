---
schema_version: "1.0.0"
document_id: "6ddf3c2e136310592d585c1d1b29b46c4fbc66c35e4bdda4699621f08c6620c2"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/cdc-replication-search/"
published_at: "2025-11-04T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:f997783a832fcfba4436cecac0aed545500aebd2b523bde2301d5ad128d66bae"
---

# Replication redefined: How we built a low-latency, multi-tenant data replication platform

Sanketh Balakrishna


Andrew Zhang


At Datadog, we operate thousands of services that rely on consistent, low-latency data access. Moving data between diverse systems—quickly and reliably—is essential, but complex. Each application may have its own requirements for data freshness, consistency, and latency, making ad hoc solutions brittle and hard to scale as the company grows.


Traditional approaches to data replication—manual pipelines, point-to-point integrations, or custom scripts—quickly become unmanageable as the number of connections and data sources multiply. The complexity compounds further when you factor in the need for observability, error handling, and operational resilience across diverse environments.


To address these challenges, we set out to build a managed data replication platform: a unified system designed to deliver highly reliable, highly scalable, and flexible data movement across Datadog. Our goal was to abstract away the operational overhead for engineering teams, provide robust monitoring and alerting, and ensure that data flows could adapt to new use cases—without requiring teams to rearchitect the underlying infrastructure or, when necessary, overhaul it entirely to support evolving product needs.


In this post, we’ll walk through how we built this platform, the key architectural decisions we made, and how you can apply them to your own multi-tenant data replication systems.


## Laying the foundation: Scaling search


Our technical journey began with a shared Postgres database powering key product pages at Datadog. (Read our blog post to learn[how we’re unwinding our shared monolithic database](https://www.datadoghq.com/blog/engineering/unwinding-shared-database/) .) A shared Postgres database was the right choice for a small footprint—its ease of use, ACID guarantees, and cost-effectiveness made it ideal for low to medium traffic. Initially, direct queries delivered snappy responses, with most page loads completing in under 100 ms.


As Datadog’s data volumes grew, we began to encounter well-known Postgres scaling limits: Query execution times that once took milliseconds began creeping into the seconds range, especially for complex joins and aggregations across multiple tables.


One concrete example was our[Metrics Summary](https://docs.datadoghq.com/metrics/summary/) page. For a certain org, we needed to join a table of 82,000 active metrics with 817,000 metric configurations for every query. At this scale, the p90 page latency approached 7 seconds, and every facet adjustment triggered another expensive query, resulting in I/O bottlenecks and higher CPU usage, especially during peak times.


We considered adjusting join order, adding multi-column indexes, and using query heuristics based on table size. Disk and index bloat led to slower inserts and updates,` VACUUM` and` ANALYZE` operations caused ongoing maintenance overhead, and memory pressure resulted in elevated I/O wait times. We tracked rising error rates and reduced throughput as the database struggled under concurrent load, impacting customer experience. Monitoring with Datadog APM confirmed that these queries began consuming a disproportionate share of system resources and time, particularly as normalized rows per query scaled upward.


These data points reflect industry-wide Postgres constraints: Complex analytical queries degrade sharply as table and index sizes grow; vertical scaling reaches diminishing returns; and operational tasks—such as backups, failover, and replication—become riskier at multi-terabyte scale. By the time our tables crossed the 50,000 metrics per org threshold for multiple orgs, there were sufficient warning signs—such as slow page loads, unreliable facet filters, and operational overhead.


Recognizing that real-time search and faceted filtering are fundamentally different workloads from OLTP, we re-architected our system. We rerouted search queries and aggregations to a dedicated search platform, with data dynamically denormalized during replication from Postgres, as shown below. This approach flattened relational structures for efficient document indexing and immediately eliminated search-related bottlenecks, slashing query latencies and restoring user experience.


By replicating PostgreSQL data into our search platform and enriching it via our pipeline, **we reduced page load times by up to 97%** (from ~30 s to ~1 s) and maintained replication lag around 500 ms.


This first project laid the technical and operational groundwork for the larger, automated, multi-tenant replication platform we would go on to build. By decoupling search from our primary database, we saw firsthand how targeted data replication can unlock scalability, improve reliability, and deliver a better user experience.


## Automating replication: Scaling provisioning with Temporal


As our platform grew, managing the complexity of provisioning replication pipelines—from Postgres to our search platform—became a significant challenge. We used technologies such as Debezium, Elasticsearch, and Kafka, but assembling all the components of the platform proved to be a non-trivial challenge.


Each pipeline required a series of precise, repeatable steps:


-


Enabling logical replication on Postgres by setting` wal_level` to logical


-


Creating and configuring Postgres users with the right permissions


-


Establishing replication objects, such as publishers and slots


-


Deploying Debezium instances to capture changes and stream data into Kafka


-


Creating Kafka topics and ensuring each Debezium instance was mapped correctly


-


Setting up heartbeat tables to address WAL (Write-Ahead Log) retention and monitoring


-


Configuring sink connectors to move data from Kafka into the search platform


When replicated across many pipelines and data centers, the operational load grew exponentially, as shown in the following diagram:


To address this, **we made automation a foundational principle** . Using[Temporal workflows](https://opensource.datadoghq.com/projects/temporal/) , we broke the provisioning process into modular, reliable tasks—then stitched them together into higher-level orchestrations. This made it easy for teams to create, manage, and experiment with new replication pipelines without getting bogged down in manual, error-prone steps, as shown below:


This automation accelerated our ability to scale and delivered compounding benefits: Developers could focus on innovation rather than repetitive tasks and our platform became more robust, consistent, and adaptable to new requirements.


## Reliability over consistency: The asynchronous replication setup


When designing our data replication platform, we faced a fundamental architectural choice: **synchronous versus asynchronous replication** .


**Synchronous replication** writes data to both the primary and replica systems at the same time, guaranteeing strong consistency—every write is acknowledged only after all replicas confirm receipt. This approach is ideal when real-time accuracy is critical, but it introduces significant latency and operational complexity, especially at scale and across distributed environments.


By contrast, **asynchronous replication** allows the primary system to acknowledge writes immediately, with data replicated to secondary systems afterward. This method is inherently more scalable and resilient in large-scale, high-throughput environments like Datadog’s—it decouples application performance from network latency and replica responsiveness. While asynchronous replication can introduce minor data lag during failures, it enables robust, always-on data movement across thousands of services without bottlenecking on consistency guarantees.


Given our priorities—favoring scalability over strict consistency— **we chose asynchronous replication as the foundation of our platform** . This decision let us handle massive data volumes, tolerate network partitions, and keep data pipelines resilient as infrastructure and workloads evolve.


To power this architecture, we rely on proven open source technologies like **Debezium** and **Kafka Connect** . Debezium enables change data capture (CDC) from a variety of databases, while Kafka Connect serves as the backbone for scalable, fault-tolerant data movement between systems. Together, they form the core of our managed replication platform, enabling flexible, reliable, and extensible data sharing across Datadog.


## Ensuring pipeline resilience through schema compatibility


One of the key challenges with asynchronous replication is dealing with constant schema evolution. Even with schema changes in the source datastore, our platform needs to ensure that change events can be reliably replicated downstream, as shown in the following:


As a platform team, we needed to build a scalable system since we can’t verify that replication continues to work after schema changes. To achieve this, we implemented a two-part solution: one for validating schema changes before they are applied, and another for propagating them throughout the pipeline.


For the first part, we developed an internal automated schema management validation system. This system analyzes schema migration SQL before it’s applied to the database to catch potentially risky changes. For example, we would want to block a schema change like` ALTER TABLE ... ALTER COLUMN ... SET NOT NULL` because not all messages in the pipeline are guaranteed to populate that column. If a consumer gets a message where the field was null, the replication could break. Our validation checks allow us to approve most changes without manual intervention. For breaking changes, we work directly with the team to coordinate a safe rollout.


The second part relies on a multi-tenant Kafka Schema Registry, integrated with our source and sink connectors. We’ve configured it for **backward compatibility** , which means new schemas must still allow older consumers to read data without errors. In practice, this limits schema changes to safe operations—like adding optional fields or removing existing ones.


When a schema migration happens, Debezium captures the updated schema, serializes the data into Avro format, and pushes both the data and schema update to the Kafka topic and the Schema Registry. This updated schema is compared with the one stored in the Schema Registry and either accepts or rejects the update based on the compatibility mode—in our case, backward. Since users can also build custom Kafka consumers to directly read the topics, maintaining schema compatibility is especially important—we want to ensure that all consumers, whether internal or external, continue to work without disruption.


## Empowering user-centric data pipelines


At Datadog, building infrastructure that supports a wide range of use cases is a core requirement. As more internal teams began ingesting and replicating data into downstream systems, we found that a one-size-fits-all pipeline didn’t scale well with growing requirements. Some teams needed filtered or denormalized data, others required custom enrichment logic, and several needed to manipulate the shape or structure of their data prior to storage.


To support a variety of requirements, we designed our pipeline to be modular, adaptable, and customizable—focusing on two key mechanisms: **Kafka Sink Connector transformations** and a **standardized enrichment API** .


Kafka Connect serves as the backbone of our data ingestion framework, as shown in the following diagram. To give teams control over how data flows from source to sink, we leaned on Kafka Connect’s single message transforms. These enabled a range of customizations—like dynamically changing topic names, transforming column types, generating composite primary keys by concatenating fields, and adding or dropping columns at the message level. This flexibility made it possible for teams to shape their data according to their specific needs without requiring changes at the source.


Where out-of-the-box connector customizations fell short, we maintained custom forks to introduce Datadog-specific logic and optimizations. This lets us support more advanced use cases and integrate deeply with internal systems.


For teams that needed additional derived fields or metadata appended to their records, we introduced a custom enrichment API. This service sits as a layer on top of our search platform, providing a standardized way to request enrichments during or after ingestion. By centralizing enrichment logic, we avoided duplicating it across individual pipelines. The result: teams could maintain consistency across their data products while reducing the complexity of their ingestion flows.


## From singular solution to a multi-tenant platform


What started as a single-purpose pipeline quickly evolved into a platform capable of supporting diverse, large-scale replication use cases across Datadog. We evolved from something that looks like this:


To this:


Our initial use case for data replication was simple: send updates from a SQL database to a downstream search platform. But as we built that first pipeline, we saw the opportunity to create something much bigger—a generalized multi-tenant platform designed to operate at Datadog’s scale.


Since then, we’ve unlocked a range of new CDC-power applications across the company:


-


**Postgres-to-Postgres replication** to[unwind our large shared database](https://www.datadoghq.com/blog/engineering/unwinding-shared-database/) and support reliable backups of Orgstore


-


**Postgres-to-**[Iceberg](https://nightlies.apache.org/flink/flink-cdc-docs-master/docs/connectors/pipeline-connectors/iceberg/) **pipelines** for scalable, event-driven analytics


-


**Cassandra replication** to expand sourcing beyond traditional SQL databases


-


**Cross-region Kafka replication** to improve data locality and resilience for[Datadog On-Call](https://www.datadoghq.com/blog/datadog-on-call/)


## Trade-offs and lessons learned


To reach this scale, we made several key architectural decisions that balanced performance, scalability, and operational excellence:


-


**Moved search workloads off Postgres** , eliminating join-heavy queries and reducing latency by up to 87%


-


**Chose asynchronous replication** to optimize for availability and throughput over strict consistency guarantees


-


**Automated pipeline provisioning with Temporal** , enabling reliable, modular workflows at scale


-


**Ensured schema compatibility** through validation checks and the multi-tenant Kafka Schema Registry


-


**Enabled pipeline customizations** with transformations and enrichment APIs to support multiple product teams’ requirements


These choices allowed us to grow a single-use replication flow into a robust, extensible, multi-tenant platform, and to do it without slowing down the teams who depend on it.


We’re continuing to push the boundaries of CDC by scaling throughput and supporting more sources and sinks across the platform. The data replication space is evolving rapidly—and we’re excited to be helping shape what’s next.


If this type of work intrigues you, consider applying to work for our engineering team.[We’re hiring!](https://careers.datadoghq.com/all-jobs/?s=platform%20infrastructure&parent_department_Engineering%5B0%5D=Engineering&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025-architecture-postgresql&gh_src=hm4uekgj1us)


-
-
-
