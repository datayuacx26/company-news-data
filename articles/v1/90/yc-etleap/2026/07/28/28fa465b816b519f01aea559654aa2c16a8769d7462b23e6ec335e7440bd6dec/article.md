---
schema_version: "1.0.0"
document_id: "28fa465b816b519f01aea559654aa2c16a8769d7462b23e6ec335e7440bd6dec"
company_key: "yc-etleap"
company: "Etleap"
source_id: "yc-etleap-news-import-75d796be5177"
canonical_url: "https://etleap.com/blog/introducing-etleap-s-sap-odata-connector"
published_at: null
first_seen_at: "2026-07-24T03:08:49.604130+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:27f737bdee141c30f6ad94186b8e8cd50615deef358656b494facc9ad26d7c86"
---

# Introducing Etleap’s SAP OData Connector

Getting SAP data into modern analytics platforms has always been harder than it should be. Recent changes in SAP’s licensing and cloud strategy are pushing everyone toward OData/CDS as the standard interface - but running those interfaces reliably at scale is non-trivial.


Etleap’s new SAP OData connector plugs directly into ODP/CDS and streams SAP data into your analytics platform of choice.


We built it to solve two core problems. First, keeping SAP extractions stable at scale: handling delta tokens, paging, retries, and upserts while staying within SAP’s limits. Second, avoiding a permanent engineering project just to keep custom connectors alive as volumes and schemas change. Etleap takes on both. You get a fully managed, production-grade pipeline from SAP to analytics that is fast, resilient, and straightforward to operate.


Etleap delivers SAP data into the analytics platform customers already use - whether that’s Snowflake, Amazon Redshift, Databricks, or Iceberg on AWS - without custom pipelines or infrastructure to maintain.


The connector runs natively on AWS. Pipelines execute inside the customer’s own Amazon VPC on Amazon EMR, write data directly to the customer’s Amazon S3 buckets, and register metadata in AWS Glue Data Catalog. From there, the data is ready to query with services like Amazon Redshift, Amazon Athena, Amazon SageMaker, or any engine that reads from S3 or Iceberg tables. Teams avoid brittle extraction jobs, manual schema triage, and overnight maintenance windows just to keep SAP data usable.


Building on years of successful SAP data integration, even with prior generation extraction methods, Etleap has a proven track record of helping customers transition from brittle, high-maintenance infrastructure to rock-solid, production-grade pipelines. This new OData connector extends that legacy, delivering the same ease of setup and zero-maintenance reliability for modern SAP landscapes.


“Our goal is to make working with SAP data as seamless as working with any other source,” said Christian Romming, CEO of Etleap. “This integration delivers performance, robustness, and simplicity - so teams can build modern lakehouse and warehouse architectures on AWS with confidence.”


Etleap’s SAP OData connector is available now. To see snapshot-to-delta in action and talk through deployment for your environment, request a demo at[etleap.com/demo](http://etleap.com/demo) .
