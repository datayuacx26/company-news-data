---
schema_version: "1.0.0"
document_id: "0dbf5b1e375e18e3921ee6a33298b71891b1b69bbd4a4091814ef5ff09beec95"
company_key: "yc-telmai"
company: "Telmai"
source_id: "yc-telmai-rss-c2782ae860ac"
canonical_url: "https://www.telm.ai/blog/telmai-on-the-google-cloud-lakehouse-a-trust-layer-built-for-joint-customers/"
published_at: "2026-06-25T14:06:56+00:00"
first_seen_at: "2026-07-20T23:20:35.564140+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:01489d23fdf8c4851a29764366fa2a306ba8386de83623eeb9200411443a6793"
---

# Telmai on the Google Cloud Lakehouse: A Trust Layer Built for Joint Customers

With native Apache Iceberg support and change data capture inside BigQuery, the cross-cloud Google Cloud Knowledge Catalog, and Iceberg REST as the connective tissue across clouds, Google Cloud Lakehouse has closed the gap between open-format data and warehouse-grade compute. The open lake is no longer a place where you stage data—it’s where enterprises run analytics and, increasingly, where AI agents take their clues for action.


That foundation is powerful. It also raises the stakes on a question every data leader is now asking: how do we know the data on the lake is trustworthy enough for agents to act on it?


That’s the question Telmai answers. As a Google Cloud partner built natively on Google Cloud, we answer it right inside the lakehouse our joint customers already run.


## A native partner, designed for Open Lake


Telmai isn’t a tool you attach to Google Cloud after the fact. Our AI-driven data reliability layer is built natively on Google Cloud, available on Google Cloud Marketplace, and runs inside the customer’s own Google Cloud account. For joint customers, that partnership shows up as three concrete advantages:


**Data stays in place.** Telmai runs in your Google Cloud environment, so records never leave it for profiling. You get full observability without copies, keeping security, residency, and compliance teams comfortable from day one.


**No load on the warehouse.** Telmai’s petabyte-scale engine runs on Apache Spark and Google Cloud services, including Dataproc, and it decouples quality analysis and scoring from the underlying warehouse. Joint customers get full-volume profiling and anomaly detection without slowing BigQuery or inflating compute spend.


**Coverage across a hybrid data ecosystem.** Telmai runs on Google Cloud and detects quality issues, anomalies, and drift across the full Google Cloud stack (including BigQuery, Lakehouse, Google Cloud Storage, Dataflow, and Pub/Sub) while extending the same monitoring to data wherever it lives, including AWS Redshift and S3, Azure, Snowflake, and beyond. Joint customers get one reliability layer across their hybrid estate, surfacing the right context whether the consumer is an engineer triaging a pipeline or an AI agent acting on a data product.


You can see the full integration on our[BigQuery data quality page](https://www.telm.ai/integrations/bigquery-data-quality/) .


## Native to the open lakehouse: Iceberg today, Delta where you need it


Google Cloud is standardizing the lakehouse on Apache Iceberg, and Telmai connects to it in the native way: through the **Lakehouse Metastore using the Iceberg REST catalog interface** , the same catalog standard Google Cloud is using to bridge clouds. Setup is no-code. Point Telmai at your REST catalog URI, Google Cloud project, region, warehouse, and default branch, grant a small set of read-only permissions, and start scanning assets in minutes. The walkthrough is in our docs:[Connecting to Iceberg REST](https://docs.telm.ai/telmai/connect-to-data/data-connections/iceberg-rest) .


Because Telmai reads through the same shared catalog your query engines use, its quality signals reflect exactly what BigQuery and Spark see no drift between what’s monitored and what’s consumed.


And because real enterprise estates are rarely single-format, Telmai observes data at the record-value level across Iceberg, Delta, and flat files, using both ML-driven baselines and user-defined expectations. Joint customers don’t have to choose a single table format to get a single, consistent view of data health.


## What the partnership means for joint customers


The point of a native partnership isn’t the integration, it’s the outcome for the teams running on it. Our joint customers show what that looks like in production.


[ZoomInfo](https://www.telm.ai/case-studies/customer-case-study-zoominfo/?utm_source=Blog&utm_medium=Social&utm_campaign=google_lkhs_blg_promo&utm_id=912-912-5484+&utm_content=zoominfo_casesstudy) runs an AI-driven go-to-market platform for more than 35,000 businesses, processing over a billion data points a day across Iceberg, Google Cloud Storage, BigQuery, and Snowflake. ZoomInfo selected Telmai to monitor data quality across billions of records and terabytes of semi-structured data without sampling, and deployed it across their Iceberg environment in under two weeks. As their data architecture team describes it, Iceberg solved their operational pain around upserts, schema evolution, and time travel; Telmai added the lifecycle-wide observability that open formats alone don’t provide.


[PropertyGuru](https://www.telm.ai/case-studies/how-propertyguru-scaled-data-quality-for-ai-driven-products-with-telmai/?utm_source=Blog&utm_medium=Social&utm_campaign=google_lkhs_blg_promo&utm_id=912-912-5484+&utm_content=ppg_casesstudy) manages property data across multiple markets and a multi-cloud infrastructure, where customers rely on accurate, trustworthy listings to make one of the biggest decisions of their lives. Their Director of Engineering for Data has called Telmai a critical partner in helping the company stay ahead of data reliability at that scale and complexity.


The common thread: these teams moved from reactive, retroactive data cleansing to reliability embedded in the fabric of their lakehouse pipelines, native format support, lifecycle-wide observability, and ML-driven intelligence working together.


## Trust that travels with context including across clouds


Google Cloud’s own lakehouse direction acknowledges two realities that Telmai was built for. First, context: raw data isn’t enough for agents, and the hardest context to produce is operational trust metadata quality scores, freshness, anomaly history, and incident context. You don’t document that; you measure it continuously at the source. Second, multi-cloud: enterprise data estates span clouds by default, not by design.


Telmai is built for both. It’s MCP-compliant, validating data as it lands and pushing trust signals to people, AI agents, and the catalogs and metadata systems teams already use, so trust travels with context to wherever the data is consumed. And because Telmai is zero-copy, in-VPC, and runs on any cloud, joint customers get one consistent reliability layer across Google Cloud, AWS, and Azure, reading data where it lives.


## The lakehouse foundation is set. Telmai makes it trustworthy.


Google Cloud has made the open lakehouse fast, governed, and borderless. As a native Google Cloud partner, Telmai adds the layer that makes it dependable for analytics and agents alike—in your own account, across BigQuery, Lakehouse Google Cloud Storage, and open formats like Iceberg and Delta, without adding load to your warehouse.


If you’re building on Google Cloud Lakehouse, see what the partnership can deliver with your own data:[connect an Iceberg REST source](https://docs.telm.ai/telmai/connect-to-data/data-connections/iceberg-rest) , explore the[BigQuery integration](https://www.telm.ai/integrations/bigquery-data-quality/) , or[request a demo](https://www.telm.ai/get-started/) .


To learn more about how Telmai can help you build trusted, AI-ready data pipelines,[book a tailored demo](https://www.telm.ai/get-started/) with our team of experts today.


Want to stay ahead on best practices and product insights?Click here to subscribe to our newsletter for expert guidance on building reliable, AI-ready data pipelines.
