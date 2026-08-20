---
schema_version: "1.0.0"
document_id: "3b2e54c9053427d087744b5fb560ebf8e98436fc79ee1eea554c7af3b2f63fc5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-deadline-cloud-now-supports-ebs-persistent-volume-cost-tracking/"
published_at: "2026-08-12T16:00:00+00:00"
first_seen_at: "2026-08-17T17:11:19.733819+00:00"
fetched_at: "2026-08-17T17:11:22.677930+00:00"
content_hash: "sha256:072bc474ed319540b81faffc104f7c3768c13f51c3cbbf690ae395dabe49b1a4"
---

# AWS Deadline Cloud Now Supports EBS Persistent Volume Cost Tracking

AWS Deadline Cloud now surfaces costs related to Amazon Elastic Block Store (Amazon EBS) persistent volumes in the Usage Explorer in the Deadline Cloud Monitor app. AWS Deadline Cloud is a fully managed service that helps teams run compute-intensive workloads in the cloud for visual effects, animation, product design, simulation, and gaming.


Customers have historically been able to see compute and license costs related to Deadline Cloud. With the recent release of support for persistent EBS volumes, customers needed a way to see storage costs as well. Now, Deadline Cloud tracks the cost for each EBS volume from creation until deletion, independent of any job or session. These costs appear as a new persistent volume usage type in Usage Explorer and the statistics APIs. These are attributed to the fleet that owns the volume so customers can aggregate spend by fleet, farm, or time period. This helps customers spot idle volumes and decide whether to adjust a time-to-live or let it expire. Cost tracking is enabled automatically, so no action is required.


Persistent volume cost tracking is available in all AWS Regions where Deadline Cloud is offered. Cost tracking introduces no additional charges. To learn more, visit the[AWS Deadline Cloud product page](https://aws.amazon.com/deadline-cloud/) or our[user guide](https://docs.aws.amazon.com/deadline-cloud/latest/userguide/) .
