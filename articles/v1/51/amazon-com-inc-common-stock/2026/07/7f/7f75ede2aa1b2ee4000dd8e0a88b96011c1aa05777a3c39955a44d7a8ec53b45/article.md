---
schema_version: "1.0.0"
document_id: "7f75ede2aa1b2ee4000dd8e0a88b96011c1aa05777a3c39955a44d7a8ec53b45"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/synthetics-customer-managed-keys/"
published_at: "2026-07-16T20:34:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:0766cdd05b3c6c2f571a2aea48f5b3e9873a64b052bafdbb31f584954324f33c"
---

# Amazon CloudWatch Synthetics now supports customer managed encryption keys

Amazon CloudWatch Synthetics now supports customer managed AWS Key Management Service (KMS) keys for encrypting canary environment variables, giving you full control over the encryption of sensitive configuration data such as API keys, credentials, and tokens. Previously, environment variables were encrypted at rest using only an AWS owned key. Now, in addition to the default AWS owned key, you can specify your own symmetric KMS key for encryption at rest, and you can also encrypt individual values client-side before they are stored.


With encryption at rest, you specify a customer managed KMS key when creating or updating a canary, and CloudWatch Synthetics uses a grant on the key to handle encryption and decryption transparently. With client-side encryption, you encrypt values before storage, and your canary script decrypts them at runtime using the AWS KMS Decrypt API. This benefits teams in regulated industries that require organizational key management policies, auditability, or key rotation controls across all services.


Amazon CloudWatch Synthetics customer managed key encryption is available in all commercial AWS Regions. Multi-location canaries can use a different KMS key per replica Region.


To learn more, see[Encrypting environment variables](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries_CommonFeatures.html#CloudWatch_Synthetics_function_encryption) in the Amazon CloudWatch User Guide.
