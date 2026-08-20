---
schema_version: "1.0.0"
document_id: "6ae68647cfe405e5a540b9475b41cf60f222d78f6715a8d35225988337f3bdee"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-lambda-network-bandwidth/"
published_at: "2026-08-05T21:35:00+00:00"
first_seen_at: "2026-08-05T22:17:28.341507+00:00"
fetched_at: "2026-08-05T22:17:30.878857+00:00"
content_hash: "sha256:775620c53012681e05e7b8a37c3cf336d444ace4309950b7863da060ad202937"
---

# AWS Lambda announces scalable network bandwidth up to 3,000 Mbps for functions outside a VPC

[AWS Lambda](https://aws.amazon.com/lambda/) now supports scalable network bandwidth for Lambda functions, enabling faster data transfer to and from your execution environment for latency-sensitive workloads. This feature enables functions outside a VPC configured with 2 GB of memory or more to access network bandwidth that scales proportionally, from 625 Mbps at 2 GB up to 3,000 Mbps at 10 GB.


Customers use Lambda to build latency-sensitive data processing workloads, which need to transfer large volumes of data - up to several terabytes - from external data sources into the function’s execution environment for processing. As data volume and performance requirements grow, the existing limit of 625 Mbps can constrain data transfer speeds to and from an execution environment. With this launch, network throughput increases proportionally from 625 Mbps at 2 GB up to 3,000 Mbps at 10 GB, helping reduce function execution times and per-invocation costs while improving end-user experience.


To get started, submit a request through[AWS Service Quotas](https://console.aws.amazon.com/servicequotas/home/services/lambda/quotas) under the Network bandwidth per execution environment quota to enable scalable network bandwidth on your account. Once enabled, bandwidth will scale automatically based on your function's memory configuration for all functions outside a VPC in your account.


Scalable network bandwidth for functions outside a VPC is available at no additional charge in all commercial[AWS Regions](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/) . To learn more, visit the[Lambda quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) page.
