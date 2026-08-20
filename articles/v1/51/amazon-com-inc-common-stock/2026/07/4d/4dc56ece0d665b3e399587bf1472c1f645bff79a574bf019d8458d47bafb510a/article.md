---
schema_version: "1.0.0"
document_id: "4dc56ece0d665b3e399587bf1472c1f645bff79a574bf019d8458d47bafb510a"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-glue-rest-connector-filtering-partitioning-vpc"
published_at: "2026-07-29T20:18:00+00:00"
first_seen_at: "2026-07-30T01:33:37.280825+00:00"
fetched_at: "2026-07-30T01:33:40.021912+00:00"
content_hash: "sha256:6f3053c2ca2a197a57cff90c521bdebe3295373e16c0009d7e33bf3735400c0f"
---

# AWS Glue announces VPC support, filter pushdown, and partition support for the REST API connector

AWS Glue now supports VPC connections, filter pushdown, and partition support for the REST API connector. The REST API connector enables you to ingest data from any source that exposes a REST-based API, including proprietary systems and emerging platforms without native AWS Glue connectors. With this launch, you can operate your ETL pipelines from data sources with REST API endpoints by securely connecting to private endpoints, transfering only the data they need, and parallelizing reads for faster ingestion, all without writing custom code


With VPC support, you can use the REST API connector to access data sources hosted in private subnets or connected through VPNs or AWS PrivateLink, without exposing traffic to the public internet. Filter pushdown translates your query predicates into API-native parameters, so only matching records leave the source, reducing data transfer costs and improving job performance. Partition support splits large datasets across multiple Spark workers using field-based or record-count strategies, providing parallel reads that reduce ingestion time for high-volume, paginated APIs.


These capabilities are available in all AWS commercial regions where AWS Glue is available.


To get started, visit the AWS Glue[REST API connector documentation](https://docs.aws.amazon.com/glue/latest/dg/connecting-to-data-rest-api.html) .
