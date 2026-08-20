---
schema_version: "1.0.0"
document_id: "7df99a3c8244b7d2749d509254b9641a928f1d6cd73a0f22b857535fa213d48d"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aurora-serverless-instant-12-acu-scaling"
published_at: "2026-08-05T15:00:00+00:00"
first_seen_at: "2026-08-05T19:58:05.832601+00:00"
fetched_at: "2026-08-05T19:58:07.748988+00:00"
content_hash: "sha256:cb1bffc52bffee22144700c716b93cfb016fbe6dffc3139e49422ea9b36c615c"
---

# Amazon Aurora serverless now scales faster to support agentic AI and other bursty workloads

[Amazon Aurora serverless](https://aws.amazon.com/rds/aurora/serverless/) now delivers higher initial capacity during scale-up events, reaching up to 12 ACUs within a second and continuing to scale up to 256 ACUs as your workload grows. When the workload finishes, Aurora serverless automatically scales down to zero. This makes it especially well-suited for agentic AI applications, which typically have bursts of activity, long idle windows, and unpredictable traffic patterns. Aurora serverless handles all of it automatically, scaling capacity with your agents, so you only pay for what you use.


This enhancement is enabled by default on all Aurora serverless clusters running on platform version 3 or 4, with no configuration changes required. Existing clusters on platform versions 1 and 2 can upgrade directly to the latest platform version 4 to benefit from these improvements. You can verify your cluster's platform version in the AWS Management Console under the instance configuration section, or via the RDS API's ServerlessV2PlatformVersion parameter.


For pricing details and Region availability, visit[Amazon Aurora Pricing](https://aws.amazon.com/rds/aurora/pricing/) . To learn more, read the Aurora serverless scaling[documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless-v2.html) , and get started by creating an Aurora serverless database in just a few steps in the[AWS Management Console](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26state%3DhashArgsFromTB_us-east-2_a53ff6382623e9a3&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&forceMobileApp=0&code_challenge=SCavOinSDkGpz8021yaDgWGy_E5aIbgB7HoRsIR47N0&code_challenge_method=SHA-256) .
