---
schema_version: "1.0.0"
document_id: "23631a926aa484b9d2aa86950f7bad339a61a651e1fdf265be2fa8bbc409683e"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/lambda-self-managed-code-storage/"
published_at: "2026-07-15T07:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:7472ef9e46d9021ba8950d5f6fb58d9abb2cc3bc9ff9d1d03957d5065f8d2a2c"
---

# AWS Lambda announces self-managed code storage

[AWS Lambda](https://aws.amazon.com/lambda/) now supports self-managed Amazon S3 buckets for code storage, enabling you to reference source code directly from your own S3 buckets without Lambda creating intermediate copies. This eliminates code storage limits and reduces function activation time after function creates and updates by removing the copy step.


AWS Lambda is a serverless compute service that runs your code without requiring you to manage servers. Customers who deploy many functions and additional code as Lambda layers often need more than 75GB of code storage per Region, requiring support tickets to increase this quota. Previously, Lambda always copied your deployment package to Lambda-managed storage during function and layer creation, counting against this limit. Now, with self-managed code storage, Lambda references your code directly in your Amazon S3 bucket without creating a copy, so you can store as much function and layer code as your bucket allows. You maintain a single source of truth for your deployment packages in your own account. No additional Lambda charges apply for self-managed storage; you only pay for standard Amazon S3 storage and, where applicable, cross-Region data transfer rates. In addition, Lambda has increased the default limit for Lambda-managed code storage from 75GB to 300GB per Region per account.


Self-managed Amazon S3 code storage is available in all commercial AWS Regions.


To get started, set the \`S3ObjectStorageMode\` parameter to \`REFERENCE\` when creating or updating functions and layers through the AWS CLI, AWS CloudFormation, AWS SAM, or AWS SDKs. You must grant the Lambda service principal \`s3:GetObject\` and \`s3:GetObjectVersion\` permissions on your S3 bucket. You can also update a function to use self-managed code storage via the Lambda Console. To learn more, visit the[AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/configuration-self-managed-storage.html) .
