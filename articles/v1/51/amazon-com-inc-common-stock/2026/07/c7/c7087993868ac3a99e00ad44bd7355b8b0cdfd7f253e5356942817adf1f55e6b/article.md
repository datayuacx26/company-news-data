---
schema_version: "1.0.0"
document_id: "c7087993868ac3a99e00ad44bd7355b8b0cdfd7f253e5356942817adf1f55e6b"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/durablefunctions-cmk/"
published_at: "2026-07-22T18:30:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:793e99f0975d8c8ae84dfb19ca130742c8020cca844f63a56f589e02dacc5104"
---

# AWS Lambda durable functions now supports customer managed key encryption

AWS Lambda durable functions now supports encryption of durable execution data with an AWS Key Management Service (AWS KMS) customer managed key.[Lambda durable functions](https://docs.aws.amazon.com/lambda/latest/dg/durable-functions.html) lets you build long-running, reliable workflows directly in your Lambda function code with automatic state management. Lambda encrypts execution state at rest by default with an AWS owned key. Now with support for AWS KMS, you can choose and manage the encryption key yourself.


If you operate in regulated industries such as financial services or healthcare, your data governance policies may require customer-owned encryption keys. You can now configure a customer managed key for durable execution data, giving you control over key rotation and who can access execution history and state. The durable execution key operates independently of the function-level key that protects environment variables and SnapStart snapshots, so you can manage access to execution data separately from function configuration.


This feature is available in all AWS Regions where Lambda durable functions is available. Standard AWS KMS charges apply for customer managed keys. There are no additional Lambda charges for this feature.


To learn more, see[Encrypting Lambda durable execution data](https://docs.aws.amazon.com/lambda/latest/dg/durable-encryption.html) in the AWS Lambda Developer Guide.
