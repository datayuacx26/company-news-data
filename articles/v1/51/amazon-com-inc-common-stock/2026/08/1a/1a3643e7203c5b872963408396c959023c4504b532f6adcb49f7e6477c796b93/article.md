---
schema_version: "1.0.0"
document_id: "1a3643e7203c5b872963408396c959023c4504b532f6adcb49f7e6477c796b93"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-msk-zk-kraft-migration/"
published_at: "2026-08-10T17:00:00+00:00"
first_seen_at: "2026-08-20T00:12:43.329206+00:00"
fetched_at: "2026-08-20T00:12:46.759003+00:00"
content_hash: "sha256:2671db353d19abf3d65c35248c94cadc6a6bb7f6111227696e9127993e716cbb"
---

# Amazon MSK now supports in-place migration from Apache ZooKeeper to KRaft

Amazon Managed Streaming for Apache Kafka (Amazon MSK) now supports in-place migration from Apache ZooKeeper to KRaft, enabling customers to migrate existing clusters to KRaft without provisioning new infrastructure, migrating data, or reconfiguring client applications.


KRaft is Apache Kafka's consensus protocol that eliminates the dependency on Apache ZooKeeper for metadata management. KRaft shifts metadata management from external ZooKeeper nodes to a group of controllers within Kafka itself, allowing metadata to be stored and replicated as topics within Kafka brokers, resulting in faster metadata propagation, improved scalability, and a simplified cluster architecture. Kafka 3.9.x is the last version to support ZooKeeper, and Kafka 4.x only supports KRaft. This in-place migration gives customers an easy path to move to KRaft on their existing clusters and upgrade to Kafka 4.x.


To get started, customers first upgrade to Kafka 3.9.x if they are on an older version, then customers can initiate the migration and the cluster remains available throughout the process. All steps can be performed through the MSK console, AWS CLI, or APIs. The in-place migration from Apache Zookeeper to Kraft is available today across all[AWS regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) where Amazon MSK provisioned is offered except European Sovereign Cloud (Germany). To learn how to get started, see the[Amazon MSK Developer Guide](https://docs.aws.amazon.com/msk/latest/developerguide/getting-started.html) .
