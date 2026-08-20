---
schema_version: "1.0.0"
document_id: "c102cad4b9aa95e0efe65a3a778e4acebc6fe9554272e4103e61d1ab4d944631"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-Lambda-provisioned-sqs-esm-max-pollers/"
published_at: "2026-08-03T18:00:00+00:00"
first_seen_at: "2026-08-04T01:38:17.140781+00:00"
fetched_at: "2026-08-04T05:15:07.724576+00:00"
content_hash: "sha256:aabe25752064d747ebcfd9b82eb56d2cc8f331982007f7cc91457c0c58664c30"
---

# AWS Lambda Provisioned Mode for Amazon SQS event source mappings now supports up to 10,000 event pollers

AWS Lambda now supports a maximum of 10,000 event pollers for Provisioned Mode for Amazon SQS event source mappings (ESMs), a 5x increase from the previous limit of 2,000. This enables you to reach up to 100,000 concurrent invocations, so you can build highly responsive and scalable event-driven applications with stringent performance requirements.


Customers use Amazon SQS as an event source for Lambda to build mission-critical applications such as real-time order processing, financial transaction pipelines, IoT telemetry ingestion, and large-scale fan-out workloads, where delays in event processing can directly impact business outcomes. Provisioned Mode for SQS ESM allows you to configure a minimum and maximum number of event pollers and optimize the throughput for your application. However, the maximum number of pollers was previously limited to 2,000, which constrained the number of concurrent invocations that could be reached per ESM, and required customers to split their workload across multiple ESMs. With this launch, customers can reach up to 10,000 event pollers and 100,000 concurrent invocations per ESM, enabling them to build at-scale workloads with stringent latency and throughput requirements.


This feature is generally available in all AWS Commercial Regions. You can activate Provisioned Mode for SQS ESM by configuring a minimum and maximum number of event pollers using the ESM API, AWS Console, AWS CLI, AWS SDK, AWS CloudFormation, and AWS SAM. You pay for the usage of event pollers based on a billing unit called Event Poller Unit (EPU). To learn more, read the[Lambda ESM documentation](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html#sqs-provisioned-mode) and[AWS Lambda pricing](https://aws.amazon.com/lambda/pricing/) .
