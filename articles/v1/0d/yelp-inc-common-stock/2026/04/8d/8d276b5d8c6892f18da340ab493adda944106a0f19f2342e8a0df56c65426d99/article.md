---
schema_version: "1.0.0"
document_id: "8d276b5d8c6892f18da340ab493adda944106a0f19f2342e8a0df56c65426d99"
company_key: "yelp-inc-common-stock"
company: "Yelp Inc."
source_id: "yelp-inc-common-stock-rss-c1f3492370c5"
canonical_url: "https://engineeringblog.yelp.com/2026/04/zero-downtime-upgrade-yelp-cassandra-upgrade-story.html"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:18:26.702617+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:0a2877fafab669ac6229290dc97083f6f566bbcc7310050e89b5022e319f64c4"
---

# Zero downtime Upgrade: Yelp’s Cassandra 4.x Upgrade Story

# Zero downtime Upgrade: Yelp’s Cassandra 4.x Upgrade Story


- Mark Surnin and Muhammad Junaid Muzammil, Software Engineer


- Apr 7, 2026


The Database Reliability Engineering team at Yelp seamlessly upgraded more than a thousand Cassandra nodes with zero downtime. This post takes you behind the scenes of our upgrade strategy, from planning sessions to flawless rollouts.


# Background


## Motivation


[Apache Cassandra](https://cassandra.apache.org/_/index.html) is a distributed wide-column NoSQL datastore and is used widely at Yelp for storing both primary and derived data. Yelp orchestrates Cassandra clusters on Kubernetes with the help of operators, as explained in[our operator overview post](https://engineeringblog.yelp.com/2020/11/orchestrating-cassandra-on-kubernetes-with-operators.html) . Upgrading from Cassandra 3.11 to 4.1 offered several observability and reliability improvements, in addition to performance gains.


- Based on public benchmarks, we expected to see faster node restarts, up to 34% faster streaming operations, and 21-60% lower p99 latency.
- The new[guardrails framework](https://cassandra.apache.org/_/blog/Apache-Cassandra-4.1-Features-Guardrails-Framework.html) allows setting warning and error thresholds on a number of tunable parameters, such as the number of partition keys in a multi-partition query, the maximum collection size, the maximum number of scanned tombstones, and more.
- The denylisting partitions feature is useful for blocking traffic to specific partitions that are too large or cause noisy neighbor issues.
- Cassandra 4.1 allows us to upgrade from Java 8 to Java 11.
- Cassandra 3.11 could only read SSL keys and certificates from file-based artifacts. The upgrade enables hot reloading of certificates and supports industry-standard mechanisms for loading them via key management solutions ([CEP-9](https://cwiki.apache.org/confluence/display/CASSANDRA/CEP-9%3A+Make+SSLContext+creation+pluggable) ).
- Although incremental repairs were available with Cassandra 3.11, a known bug significantly limited their usability ([CASSANDRA-9143](https://issues.apache.org/jira/browse/CASSANDRA-9143) ). This issue has been fixed in 4.1.
- Additional logging mechanisms, such as full query logging and audit trails, are available in the newer version.
- Cassandra 3.11 is end-of-life, and upgrading to 4.1 would unblock the upgrade to Cassandra 5, which introduces features like ACID transactions and vector search.


## Components


As a part of the project, we had to make every homegrown component that interacts with Cassandra compatible with 4.1 including our ad-hoc CQL query tool, schema change service, backup and restore tool, etc. In particular, we’d like to highlight a few components that involved significant work to support the new version:


- **[Stargate](https://stargate.io/)** : The open-source proxy to Cassandra that’s token-aware and provides low-latency access. It generates a GraphQL schema for every keyspace based on its CQL schema.
- **Cassandra Source Connector** : A system that streams data from Cassandra into Yelp’s data pipeline, which is an abstraction on top of Kafka. Details around the architecture of Cassandra Source Connector are explained[in our blog series](https://engineeringblog.yelp.com/2019/12/cassandra-source-connector-part-1.html) . The CSource connector consists of two components:


- [Change Data Capture (CDC)](https://cassandra.apache.org/doc/4.1/cassandra/operating/cdc.html) Publisher
- DataPipeline Materializer


- **Cassandra Sink Connector** : A system that publishes data from Yelp’s data pipeline into Cassandra.
- **Spark Cassandra Connector** : An interface that enables direct reading and writing of data between Spark jobs and Cassandra. Additional information on our specific usage can be found in[this article](https://engineeringblog.yelp.com/2024/09/boosting-ml-pipeline-efficiency-direct-cassandra-ingestion-from-spark.html) .
- **Pushplan Automation** : A system that allows making Cassandra schema changes in a declarative way.


# Upgrade Assessment


Before starting the upgrade, we went through a preparation phase where we defined our goals and tested the feasibility of the project.


## 1. Verifying Public Benchmarks


Performance benchmarks are highly dependent on the environment in which they are carried out; such as the data model, queries, cluster resources and more. So, it was essential to validate the expected performance improvements in our own operating environment.


We spun up Cassandra 3.11 and 4.1 clusters with identical resources, configured traffic profiles to resemble our production workloads, and benchmarked the Cassandra clusters. We observed a 4% improvement in 99th percentile latencies and nearly 11% improvement in mean latency. At the same time, we saw more than 11% improvement in request throughput with the new version. These results aligned with the performance benefits claimed in a Datastax whitepaper. However, a few surprises awaited us in the next stages of the upgrade.


## 2. Avoid Hard Blocking


One of our core principles was to avoid hard-blocking ourselves during the upgrade. Even though the benchmark results were promising, the real test would come once the use cases were upgraded in production. We had to support both Cassandra 3.11 and 4.1 until all clusters were fully upgraded. From a code management perspective, we achieved this by publishing version-specific Cassandra images from dedicated Git branches. The appropriate Cassandra image was selected at bootstrap time via version-specific environment variables.


While this approach required some additional effort to ensure that 3.11 changes were also shipped to the 4.1 system, the ability to deploy changes independently to either Cassandra version outweighed the overhead. Additionally, during the upgrade window, we expected critical fixes for Cassandra 3.11 to be rare.


## 3. Seamless Upgrade


Another principle that we prioritized was ensuring that the upgrade did not negatively impact production traffic or require any client code changes. In our experience, adding a dependency on our customers increases project complexity, potentially leading to a long tail of migrations. The upgrade was performed independently of client teams. To ensure a seamless process, we carefully planned the upgrade steps, tested staged upgrade and rollback procedures, and notified relevant teams during their cluster upgrades.


## 4. Production Qualification Criteria


We developed the following production qualification criteria:


- The performance of the Cassandra platform should not degrade after upgrade, as measured by latency, throughput, uptime, resource utilization, and corresponding SLOs.
- There should be no functional regressions after the upgrade, such as breaking API changes, bugs, etc.
- Security posture must remain intact during and after the upgrade.
- Comprehensive deployment and rollback plans must be in place.
- Sufficient observability must be available to track the progress of upgrades.
- All components interacting with Cassandra must remain fully operational, reliable, and functionally correct.


## 5. Automate As Much As Possible


Ensure the entire upgrade process is automated with checkpoint support to reduce human error and inconsistencies. The checkpoint support is especially helpful in the early stages of the upgrade, when confidence in the process is still building. We implemented it as a script that executes various kubectl and CLI commands, creates pull requests, and performs other workflow steps. The script can run in auto-proceed mode or pause for confirmation from an engineer after each step, which was particularly useful when upgrading critical clusters.


# Implementation Journey


## Upgrade strategy


The[official Cassandra upgrade guide](https://docs.datastax.com/en/luna-cassandra/guides/upgrade/overview.html) recommends upgrading clusters to 4.1 via a rolling restart, rather than creating a new data center (DC) on 3.11, upgrading its nodes, and redirecting the traffic to the new DC. We had considered the separate DC method due to benefits such as right-sizing EBS volumes, standardizing the DC-specific configurations, and enabling easier rollback by pointing the traffic to the old DC.


However, this would have involved streaming all data to the new DC and could have taken weeks to complete the upgrade. Additionally, we would have had to account for eventual consistency due to downgrading from the[EACH_QUORUM consistency level](https://docs.datastax.com/en/cassandra-oss/3.x/cassandra/dml/dmlConfigConsistency.html) . Running twice the number of nodes per DC would also have significantly increased costs. As a result, we opted for an in-place upgrade to reduce time and cost.


## Compatibility Changes


As part of the project, we evaluated the compatibility of different components in the Cassandra ecosystem with the 4.1 version. While many components required no or minimal changes, upgrading the Stargate proxy service and the Cassandra Source connector was more involved.


### Cassandra


Since our Cassandra fleet runs on Kubernetes without static IPs for pods, each Cassandra pod receives a new IP address upon restart. This caused issues where initial gossip communication would fail when both the Cassandra version and IP address changed simultaneously ([CASSANDRA-19244](https://issues.apache.org/jira/browse/CASSANDRA-19244) ). To circumvent this, we leveraged the Kubernetes init containers to first start the Cassandra node on a new pod with a different IP with the older 3.11 version, allowing the node to gossip with its new IP address before proceeding with the version upgrade. This approach was explained[in detail](https://www.youtube.com/watch?v=nTmwmd4fcGI) at KubeCon 2025.


### Stargate


The Cassandra 3.11-compatible Stargate proxy was unable to pull the schema from Cassandra 4.1 nodes due to the behavior of[Cassandra’s MigrationCoordinator](https://github.com/apache/cassandra/blob/efba69dbe119b5badff494243b7e2c5f560ce5b5/src/java/org/apache/cassandra/schema/MigrationCoordinator.java#L372-L374) . Ultimately, we opted for version-specific Stargate instances, each relying on the corresponding version of the Cassandra persistence layer. During this process, we ensured that the seed list of the proxy always pointed to a Cassandra node running the matching major version. To prevent any breaking API changes between the major versions, we expanded our acceptance test coverage across all services.


### Cassandra Source Connector


One of the key characteristics of the Cassandra Source Connector is its ability to read from Cassandra CDC commit logs. Due to several architectural changes in Cassandra 4.1, the existing connector was not forward-compatible. Notable changes include:


1. With Cassandra 4, CDC commit logs are now created as soon as mutations happen on a CDC-enabled table ([CASSANDRA-12148](https://issues.apache.org/jira/browse/CASSANDRA-12148) ).
2. Instead of maintaining a Schema Change Listener for handling schema updates using a Cassandra driver, we switched to actively detecting schema changes as commit logs are processed. While not strictly required, this approach simplified the CDC Publisher.
3. Major refactoring of Cassandra codebase in 4.1.


While implementing these changes, we ensured that the Cassandra Source Connector remained backward-compatible with version 3.11. Of the two sub-components, the DataPipeline Materializer was first made compatible with both versions and was shipped across all environments prior to kickstarting the upgrade process. The CDC Publisher was upgraded in tandem with the Cassandra nodes upgrades.


## Overall Upgrade Process


The overall upgrade process can be divided into three stages, all of which are automated. The actual upgrade is carried out during the flight stage, one DC at a time, in sequence.


### Pre-flight


The pre-flight stage prepares the Cassandra cluster for the upgrade and involves the following steps:


- Communicating with relevant stakeholders.
- Ensuring Cassandra schema versions are fully in agreement across the cluster.
- Disabling schema changes for the cluster during the upgrade.
- Verifying that a full backup exists for the cluster.
- Ensuring that anti-entropy repairs (processes that synchronize data across all nodes in a Cassandra cluster to maintain consistency) for the cluster remain paused during the upgrade.


### Flight


- The upgrade process began with a cluster running Cassandra 3.11.


- The first step involved upgrading one Cassandra node to version 4.1. Since the CDC Publisher process runs on the same Cassandra node as a separate container, it also gets updated.


- Once a node is upgraded, we introduce the 4.1-compatible Stargate proxy. Services could seamlessly communicate with either of the two Stargate instances, which were advertised under the same namespace in the service mesh. We continuously monitor p99 latency and errors per keyspace for 3.11 and 4.1 instances to make sure there are no regressions.


- We then upgraded the rest of the Cassandra nodes, except for the last one, which was kept on 3.11 so that the 3.11-compatible Stargate instances can still pull schema at startup.


- Next, we stopped the 3.11-compatible Stargate instances, allowing us to upgrade the last Cassandra node.


- Finally, we upgraded the last Cassandra node to complete the flight process.


### Post-flight


The post-flight operation involved:


- Re-enabling anti-entropy repairs on the cluster.
- Allowing user-initiated schema changes on the cluster.
- Sending out communications about the completion of the upgrade.


# Lessons Learned


As with any complex project dealing with many components, we encountered a few surprises during the upgrade.


## Performance Impact


Although the initial benchmarks on our proxy service showed an overall improvement in latency, some specific use cases such as range queries and multi-partition queries were found to be slower. After extensive debugging, we identified the performance regression as being introduced by Stargate 2.x. Downgrading to version 1.x resolved the issue. Our detailed observability dashboards helped us detect this early in our non-production environments. In some cases, we also observed elevated latency while the Cassandra cluster contained a mix of 3.11 and 4.1 nodes. This was transient and resolved once all nodes were upgraded.


This experience highlighted the importance of a comprehensive benchmarking suite to detect regressions in specific queries or data models.


## Schema Disagreement


On some Cassandra clusters with CDC enabled, we observed schema disagreement after all the nodes in the cluster were upgraded. While the root cause of this issue is not fully understood, we found that making dummy schema changes from multiple nodes after the upgrade led to gradual schema convergence. This approach served as an effective remediation.


# Wins


## Improved Performance


We observed a significant performance boost, with up to a 58% reduction in p99 latencies on key Cassandra clusters at Yelp.


## Faster and More Stable Restarts


The non-disruptive seed list reload feature ([CASSANDRA-14190](https://issues.apache.org/jira/browse/CASSANDRA-14190) ) enabled us to have a consistent and smaller seed list within each Cassandra cluster. This resulted in faster gossip convergence on topology changes and significantly improved node restart times.


## Seamless Upgrade


The principles established at the beginning of the project ensured the upgrade went smoothly and with no noticeable impact on customers. There were no client code changes required, and no downtime or incidents observed during the entire upgrade process.


# Acknowledgements


This post is dedicated to Mark Surnin. His professionalism, insight, and steadfast support were instrumental in the successful completion of the Cassandra cluster upgrade. We are grateful for his contributions and honor his memory.


[Tweet](https://twitter.com/share)


### Become an Engineer at Yelp


We work on a lot of cool projects at Yelp. If you're interested, apply!


[View Job](https://www.yelp.careers/us/en/c/engineering-jobs?lever-source=engineering_blog)


[Back to blog](https://engineeringblog.yelp.com/)
