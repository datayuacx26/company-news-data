---
schema_version: "1.0.0"
document_id: "695ddd82964fbe6f0d622152969277244b2cc2823fa6029864772fa3cb5995a6"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-glue-data-quality-catalog-anomaly-detection-write-results"
published_at: "2026-07-27T20:45:00+00:00"
first_seen_at: "2026-07-28T00:43:02.218346+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:0321122335776d9dcad177caa6b98c8fb6e0a4cc5e858b2725cad4226becc504"
---

# AWS Glue Data Quality now supports anomaly detection and writing results to the AWS Glue Data Catalog

AWS Glue Data Quality now supports anomaly detection for Catalog-based data quality evaluations and the ability to write evaluation results to AWS Glue Data Catalog (GDC) tables. These capabilities work across both ETL jobs and Catalog evaluations, giving you a consistent data quality experience regardless of workflow type.


With anomaly detection support for GDC, you can identify unexpected changes in data statistics such as sudden drops in distinct values or row count spikes in your GDC tables using ML-powered time-series forecasting, without writing rules with explicit thresholds. This is especially valuable for data engineers monitoring hundreds of tables in the GDC who need to surface issues automatically.


With results storage **** in GDC, Data Quality rule outcomes, profiling metrics, and anomaly predictions (with confidence bounds) are written back to GDC tables, creating a queryable record of all quality evaluations. Whether the evaluation runs in an ETL job or directly on a Catalog table, results can be queried at any time using standard SQL.


AWS Glue Data Quality anomaly detection and Catalog results storage are available in all AWS commercial regions and AWS GovCloud (US) regions.


To get started, visit the[AWS Glue Data Quality documentation](https://docs.aws.amazon.com/glue/latest/dg/data-quality.html) .
