---
schema_version: "1.0.0"
document_id: "b66bf08231cd94a8d8d0fe6fee1be5e9234a0f233445818f613cbcc935104ac8"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-drs-fast-hydration/"
published_at: "2026-07-14T22:00:56+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:25cc6eeed77c014d8089f2464cf9b37d45c2d4ccf2764966e2b4b940e22425ec"
---

# AWS Elastic Disaster Recovery now supports Amazon EBS volume initialization rate

AWS Elastic Disaster Recovery (AWS DRS) now supports the Amazon EBS volume initialization rate, helping recovered volumes reach full performance faster during drills and recoveries. When DRS restores EBS volumes from snapshots, the data loads from Amazon S3 in the background, and I/O to blocks that haven't loaded yet can be slower until initialization finishes. With this launch, you can set a volume initialization rate on your DRS-managed EC2 launch template, and DRS applies it automatically when it creates volumes during recovery — bringing your applications to full storage performance on a predictable timeline.


This is especially valuable for I/O-intensive workloads such as databases, where fast, consistent storage performance is critical to meeting your recovery time objectives. You set the rate once on the launch template, and DRS preserves it across the updates it makes for rightsizing or disk changes. If the rate cannot be applied for a given recovery, DRS completes recovery without it, so your recovery is never blocked.


AWS DRS support for the EBS volume initialization rate is available in all AWS Regions and environments where the EBS volume initialization rate is offered. You are charged per GB based on the full snapshot size and the rate you specify; for details, see[Amazon EBS pricing](https://aws.amazon.com/ebs/pricing/) . To learn more, see the[AWS Elastic Disaster Recovery User Guide](https://docs.aws.amazon.com/drs/) .
