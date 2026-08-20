---
schema_version: "1.0.0"
document_id: "b0bfe31709adec68f862548f041c1faf4be24c1b17cbe49622c61498319b9f61"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-data-exports-amazon-bedrock-product-metadata/"
published_at: "2026-07-20T18:29:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:8aaf96dc61fa13d2c0e4d71aff286fa3969928a4ab5c810f1a05a8d5cbe3a0fe"
---

# AWS Data Exports now provides standardized Amazon Bedrock product metadata

Today, AWS announces standardized product metadata for Amazon Bedrock in AWS Data Exports (Cost and Usage Report), giving FinOps teams and cloud administrators consistent, structured attributes to understand Bedrock costs. AWS Data Exports lets you create customized exports of your AWS cost and usage data and deliver them to Amazon S3 for querying with Amazon Athena or loading into your data warehouse. With these attributes, you can attribute Bedrock spend without building custom logic to parse various product metadata in CUR 2.0.


The standardized attributes include model provider, model name, pricing unit, inference type (such as input tokens or output tokens), and feature (the inference serving mode, such as On-Demand or Batch), along with a unified "Amazon Bedrock" product family name that consolidates all Bedrock costs. In CUR 2.0, the model provider, model name, inference type, and feature attributes are available in the product map column, and pricing unit is available as a column. The standardized fields are available by default, at no additional cost, to Amazon Bedrock customers using AWS Data Exports.


To learn more, visit the[Amazon Bedrock](https://aws.amazon.com/bedrock/) product page, and see[Product columns](https://docs.aws.amazon.com/cur/latest/userguide/table-dictionary-cur2-product.html) in the AWS Data Exports User Guide for the standardized product attributes.
