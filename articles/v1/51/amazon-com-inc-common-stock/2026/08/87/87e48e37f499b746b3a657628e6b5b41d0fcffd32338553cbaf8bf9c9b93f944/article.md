---
schema_version: "1.0.0"
document_id: "87e48e37f499b746b3a657628e6b5b41d0fcffd32338553cbaf8bf9c9b93f944"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-ec2-auto-scaling-batch-termination"
published_at: "2026-08-17T18:11:00+00:00"
first_seen_at: "2026-08-17T19:26:36.697208+00:00"
fetched_at: "2026-08-17T19:26:38.478178+00:00"
content_hash: "sha256:5c25dba23d7ec9172de989a20b761b6b709364515fde5f389e07700b2133af9d"
---

# Amazon EC2 Auto Scaling now supports batch instance termination

Amazon EC2 Auto Scaling now supports batch instance termination in a single API call. You can now pass up to 100 instance IDs to the TerminateInstanceInAutoScalingGroup API to terminate them as a batch, reducing the number of API calls needed to scale down your Auto Scaling groups.


Batch termination is designed for workloads that need to rapidly scale down, such as AI/ML training jobs, container orchestrators, or event-driven architectures that spin up large fleets temporarily. All instances in a batch are validated atomically before termination begins, and existing Auto Scaling behaviors such as lifecycle hooks and load balancer connection draining are preserved for each instance in the batch.


This feature is available in all AWS Regions at no additional cost.


To learn more, visit[Amazon EC2 Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-scaling-manually.html) and[Amazon EC2 Auto Scaling API Reference Guide](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_TerminateInstanceInAutoScalingGroup.html) .
