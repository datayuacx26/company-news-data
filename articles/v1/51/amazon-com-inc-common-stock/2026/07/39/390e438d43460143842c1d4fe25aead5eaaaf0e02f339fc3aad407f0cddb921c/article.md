---
schema_version: "1.0.0"
document_id: "390e438d43460143842c1d4fe25aead5eaaaf0e02f339fc3aad407f0cddb921c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/s3-removes-30-day-transitions-standard-ia-one-zone-ia"
published_at: "2026-07-16T15:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:1981b6aa8c24343a846729e6147ed3a55be81bbb58781331a41995bb569fe487"
---

# Amazon S3 removes 30-day minimum for transitions to S3 Standard-IA and S3 One Zone-IA

You can now transition objects to S3 Standard-Infrequent Access (S3 Standard-IA) and S3 One Zone-Infrequent Access (S3 One Zone-IA) as soon as the day they are created, without the previous 30-day minimum retention in S3 Standard. These storage classes offer up to 40% lower storage costs than S3 Standard while still providing millisecond access when needed, making them ideal for backups, log analytics, and compliance workloads where data becomes cold within hours or days.


To get started, create new S3 Lifecycle rules to transition objects to S3 Standard-IA and S3 One Zone-IA as soon as 0 days after creation. You can configure these rules using the S3 console, AWS CLI, or SDKs. This update is available in all AWS Regions where S3 Standard-IA and S3 One Zone-IA are available. For pricing details, visit the[Amazon S3 pricing page](https://aws.amazon.com/s3/pricing/) . To learn more, visit the[overview page](https://aws.amazon.com/s3/storage-classes/) and[documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-transition-general-considerations.html) .
