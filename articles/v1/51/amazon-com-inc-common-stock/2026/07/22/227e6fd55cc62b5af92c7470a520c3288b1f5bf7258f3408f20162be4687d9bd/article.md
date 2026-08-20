---
schema_version: "1.0.0"
document_id: "227e6fd55cc62b5af92c7470a520c3288b1f5bf7258f3408f20162be4687d9bd"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-cloudwatch-lookup-processor/"
published_at: "2026-07-14T22:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:51d0e1fe0509afc03eaf37bc7d237b580558e484400abd51f27344f9c76e24ee"
---

# Amazon CloudWatch announces lookup processor for log enrichment

Amazon CloudWatch now supports lookup processor, enabling you to enrich log events with additional context by matching fields in your logs against a lookup table directly within your CloudWatch Pipeline.


With the lookup processor, you can upload CSV files containing reference data and configure your pipeline to match incoming log fields against this data to add enriched metadata. For example, you can upload a CSV mapping IP addresses to application teams and automatically tag VPC Flow Logs with team ownership information as logs are ingested. The lookup processor matches fields in your log events against fields in a lookup table and adds specified fields from matching rows to your log events. This enables data enrichment scenarios such as mapping user IDs to user details, product codes to product information, or error codes to human-readable error descriptions, thereby eliminating the need to build and maintain custom enrichment logic outside of CloudWatch. By enriching logs at ingestion time, your queries, dashboards, and alarms immediately benefit from the added context without any post-processing.


The lookup table processor is available in all AWS commercial regions that support Amazon CloudWatch pipelines. You can add a lookup processor to your pipeline using the AWS Management Console, AWS CLI, or AWS SDKs. To get started, see the Amazon CloudWatch Logs[documentation.](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/transformation-processors.html#lookup-processor)
