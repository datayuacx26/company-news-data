---
schema_version: "1.0.0"
document_id: "7befc14395918cea5e2078bd93ec804fb13497e07206349c3912ad2e1921f393"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/secrets-manager-update-notifications"
published_at: "2026-07-22T07:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:aff4ca66b13a6c72af76e8c210b95623e9ba04d912b022faff4f944a4e83a1b4"
---

# AWS Secrets Manager now publishes secret update notifications to Amazon EventBridge

AWS Secrets Manager now automatically publishes events to Amazon EventBridge whenever your secret values change, enabling you to build event-driven workflows that respond in real time to secret updates.


Until now, you had to rely on AWS CloudTrail events parsed into EventBridge to know when a secret value changed — requiring you to match multiple API events such as rotation success, PutSecretValue, and UpdateSecretValue. With this launch, Secrets Manager publishes events directly into EventBridge whenever your secret value changes. You can use EventBridge rules to detect when the active secret value changes — such as during rotation — and route notifications to targets like AWS Lambda, Amazon SNS, Amazon SQS, or Amazon Step Functions. This enables you to proactively refresh cached credentials in your applications, restart dependent services, or update compliance reports for secret rotation.


Secret update notifications are published to your default event bus automatically with no additional configuration or opt-in required. This feature is available in all AWS Regions where AWS Secrets Manager is available at no additional cost. To get started, see[secret event notifications](https://docs.aws.amazon.com/secretsmanager/latest/userguide/secret-event-notifications.html) in the AWS Secrets Manager User Guide.
