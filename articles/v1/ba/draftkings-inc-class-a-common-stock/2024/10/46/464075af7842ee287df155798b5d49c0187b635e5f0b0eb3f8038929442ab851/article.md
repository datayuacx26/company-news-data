---
schema_version: "1.0.0"
document_id: "464075af7842ee287df155798b5d49c0187b635e5f0b0eb3f8038929442ab851"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/migrate-hundreds-of-microservices-to-the-cloud-with-zero-downtime-part-2-b15ad75a0729"
published_at: "2024-10-02T06:20:32+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:77c179200aa19b7295e683891e40fadb83b923d37f6d1cdcfef13c4c4f573813"
---

# Migrate hundreds of microservices to the cloud with zero downtime — Part 2

# Migrate hundreds of microservices to the cloud with zero downtime — Part 2


[Valerii Golovko](https://medium.com/@nevalikne?source=post_page---byline--b15ad75a0729---------------------------------------)


7 min read


·


Oct 2, 2024


--


Press enter or click to view image in full size


## Pipeline service migration


## Inter-service communication via Kafka


In the described system, most of the services are pipelines interconnected via Kafka, so it is essential to establish and maintain communication between services during migration process. As the system should be up and running 24/7, there are certain ways to achieve zero-downtime when changing its topology.


While moving certain parts of the system from On-Prem to the Cloud, the Migrating Service exists in two states:


- Service is in the process of migration.
- Before fully enabling the target Service in the Cloud and shutting it down in the On-Prem, there should be sufficient ways for its verification.
- Service has already moved to the Cloud.
Input/Output topics mirroring:
- As a prerequisite for service migration, all input topics produced by not yet migrated services should already be available in the Cloud.
- In some rare occasions, once a service is migrated, its output should be migrated back to On-Prem.
- In the describing system, there are cases when certain output topics should be mirrored to other various Datacenters. This requires a set of inputs/outputs mirroring strategies during migration process, which is out of the scope of this article.


### Kafka topics mirroring


As mentioned above, mirroring inputs/outputs is necessary in order to support different service states during the migration process.


Term mirroring here implies real-time replication of all messages from source to target Kafka topic with minimal latency.


Source and Target topics could be in different Kafka clusters, and in the scope of this article, that’s exactly the setup which supposed to be supported.


There are various ways in order to support topic mirroring, including ready solutions like[Kafka Connect](https://docs.confluent.io/platform/current/connect/index.html#kafka-connect) +[Mirror Maker 2.0](https://cwiki.apache.org/confluence/display/KAFKA/KIP-382%3A+MirrorMaker+2.0) or a custom one.


For simplicity, a component responsible for topic mirroring will be called **MirrorMaker** downstream. The diagram below highlights this concept:


Press enter or click to view image in full size


MirrorMaker component is deployed to the environment where the target Kafka cluster resides. It connects to the source Kafka cluster, consumes the topic, and publishes all the messages into the target topic.


It is important to note that this solution comes at a price:


- Additional network latencies between the source and target Kafka clusters consisting of:
- Consumer latency between MirrorMaker and source topic
- Producer latency between MirrorMaker and target topic
- It becomes a business-critical component during the transition phase, as any disruptions in MirrorMaker work lead to major downtimes. Therefore choosing a proper MirrorMaker solution should consider appropriate resiliency strategy and monitoring.


This should always be taken into account when a mirroring step going to be introduced in the pipeline.


### Double mirroring


In certain scenarios, there could be a need to support double mirroring of a single topic from the source Kafka Cluster into the target and vice verca.


Such a scenario could be needed when there is more than one publisher on the same topic, together with consumers in both clusters.


Press enter or click to view image in full size


In such cases, MirrorMaker is deployed in both Source and Target clusters. However, with the double mirroring setup, it’s important to make sure that there are no duplicated messages in the source and target topics.


To achieve that, MirrorMaker should support publishing a custom header as part of every message which could contain the origin cluster name. Once another MirrorMaker consumes a message with such a header, it knows its origin and won’t mirror it if the origin matches the destination cluster.


As a result, in the Cloud version of the topic, MirrorMaker mirrors only messages from the On-Prem Service Publisher, and in the On-Prem topic version, MirrorMaker mirrors only messages from the Cloud Service Publisher.


## Service migration process


Let’s highlight the most common scenario when a pipeline service has only input/output as Kafka topics.


However, different services may have certain limitations and various dependencies, which could significantly impact migration strategy.


### **Current state of the service**


A typical pipeline service has certain input and output topics. If it’s not the very first migrating service, based on strategies discussed earlier, its output should be already mirroring the target Kafka cluster in the Cloud.


Press enter or click to view image in full size


### **Testing phase**


Migrating service can be deployed to the target cluster with a “Testing” configuration when it consumes production input, however produces its output into “Testing” topics created in advance. This opens the following opportunities:


- Validate current production service output vs testing one, produced by the service with “Testing” configuration.
- Validate that all input dependencies are consumed correctly and that the service operates according to expectations in the target environment without production impact.
- The actual rollout would be just a switch configuration to production one + shutting down service in the previous cluster.
- This could be achieved with either zero or minimal downtime, depending on the service’s output characteristics (which will be described later).


The testing phase steps are:


1. Start mirroring all the input of a migrating service.
2. Deploy the service to the target environment with configuration to produce into “Test” topics (created in advance).
- After this step, the Prod Service instance in the On-Prem produces Prod output topics, and the deployed Test Service in the Cloud produces the same but “test” topics.
3. Start verification over the service.
- Verify operational characteristics of the Test Service.
- Compare Prod vs Test topics’ content.


Press enter or click to view image in full size


### Rollout phase


Once verification is done, the Cloud Service configuration could be changed to production one and start publishing into the Prod topics. Service in On-Prem can be stopped.


During the rollout phase, it is important to decide how to handle the service’s output. If it’s acceptable to have duplicated messages in output topics for a short period of time, the deployed service in the Cloud can be switched to start producing into production topics and then service in the On-Prem can be stopped. Such rollout strategy has zero downtime. Usually it’s applicable when output topics are[compacted](https://docs.confluent.io/kafka/design/log_compaction.html) .


The rollout phase steps in such a scenario are:


1. Switch the Cloud Service’s configuration to “Production” in order to start publish to the production topics
- This change causes publishing duplicated messages into output topics until On-Prem Service is not stopped
2. Stop On-Prem Service
3. Stop mirroring of output topics On-Prem → Cloud
4. Start service verification process


Press enter or click to view image in full size


Duplicated messages in output topics may be unacceptable in some cases. Even more, missing messages could be an issue when the Service’s output is a stream of events that must be ordered.


In Sports business area it could be a set of events that happened on the field, like goals, red cards, touchdowns, etc. Therefore, missing or duplicating goal event is not acceptable as would lead to a corrupted end result/state.


Having as prerequisite Testing setup described above:


- Switching the Service in the Cloud to production output first and then stopping the Service in On-Prem would lead to duplicated messages.
- Stopping the Service in On-Prem first and then switching the Service in the Cloud to production output would lead to missing messages.


In such cases, the rollout could be executed during the timeframe when there are no running games. If there is no such timeframe additional options could be considered:


- Downstream components are being switched to “Test” topics, so they will become new “Production” topics, and previous ones will be deprecated.
- Downstream components should be able to rebuild their state based on the new input stream.
- Use the approach with overlapping/duplicated messages from above. However, downstream components must support de-duplication logic based on idempotency techniques for example.
- Maintenance window to be considered.


### Rollback plan


A Rollout plan must imply a Rollback plan in case of any issue during execution. Thankfully, it is fairly simple and requires the execution of exactly the same Rollout steps in reverse and in the opposite order.


However, it must be explicitly planned as part of every service migration. In the case of simple service when duplicated messages are acceptable in output topics it looks like:


1. Start mirroring of output topics On-Prem → Cloud
2. Start On-Prem Service
3. Switch the Service’s configuration in Cloud to “Testing” one to stop publishing into the production topics
- Or stop the Cloud version of the Service
4. Start service verification process


A more complicated Service migration plan defines the Rollback in a similar way.


## What’s next


[The third part](https://medium.com/draftkings-engineering/migrate-hundreds-of-microservices-to-the-cloud-with-zero-downtime-part-3-075197dea5fb) of the article will be dedicated to HTTP API service migration process.
