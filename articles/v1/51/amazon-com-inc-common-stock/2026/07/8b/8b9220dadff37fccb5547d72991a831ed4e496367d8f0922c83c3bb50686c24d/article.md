---
schema_version: "1.0.0"
document_id: "8b9220dadff37fccb5547d72991a831ed4e496367d8f0922c83c3bb50686c24d"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-msk-streaming-tables-for-apache-iceberg"
published_at: "2026-07-30T14:07:00+00:00"
first_seen_at: "2026-07-31T16:43:36.463186+00:00"
fetched_at: "2026-07-31T16:43:37.565078+00:00"
content_hash: "sha256:3f466e17ccbf074b89412fc6c135a73ce034f913c70cff5247e784e390cb9fec"
---

# Amazon MSK Express brokers now deliver data to streaming tables for Apache Iceberg

[Amazon MSK Express brokers](https://aws.amazon.com/msk/features/express-brokers-for-amazon-msk/) now deliver data to streaming tables for Apache Iceberg, a new capability that continuously materializes Apache Kafka topics as Apache Iceberg tables on Amazon S3 Tables. Amazon MSK data delivery to streaming tables can reduce the cost of ingesting and delivering Apache Kafka data into Amazon S3 Tables by up to 60% versus self-managed deployments and reduces downstream query costs by up to 30% versus self-managed Apache Kafka deployments.


Customers rely on Apache Kafka to ingest real-time data for use cases like fraud detection and personalization and increasingly want to unify that data with Apache Iceberg tables for near real-time analytics but integrating the two forces them to operate complex custom pipelines, manage format conversions, and contend with the small-file problem, where high-volume ingestion creates many small parquet files that slow downstream queries and increase costs. With this capability, intelligent inline compaction eliminates the performance impact of small files and keeps query performance predictable without sacrificing data freshness, while built-in coordination resolves concurrent writer conflicts across high-throughput consumers. Amazon MSK supports throughput of up to 10 GB/s for delivery to Apache Iceberg on Amazon S3 Tables, and because this native capability adds no broker egress throughput, customers avoid the incremental infrastructure costs of scaling connector pipelines and match capacity to actual demand rather than peak. Customers deliver data to streaming tables and query or transform the data with any engine of their choice, including Apache Spark, Trino, or Apache Flink.


To get started, customers open the Amazon MSK console, select the Express cluster, and enable the capability in a few clicks, or use the MSK APIs or MCP server. Amazon MSK data delivery to streaming tables is available today in every[AWS Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) where Amazon MSK Express brokers are offered. For pricing information, visit the[pricing page](https://aws.amazon.com/msk/pricing/) . To learn more, visit the[Amazon MSK Developer Guide](https://docs.aws.amazon.com/msk/latest/developerguide/msk-data-delivery.html) and[Amazon MSK AI skills](https://github.com/aws/agent-toolkit-for-aws/tree/main/skills/specialized-skills/analytics-skills/managing-amazon-msk) .
