---
schema_version: "1.0.0"
document_id: "2a8f573b3abe1adba41a84b8a129d97163b93d2ac174c6d1e3ab4ed04a9e149c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-backup-amazon-s3-direct-access/"
published_at: "2026-08-06T10:00:00+00:00"
first_seen_at: "2026-08-07T00:09:00.624860+00:00"
fetched_at: "2026-08-07T00:09:02.541202+00:00"
content_hash: "sha256:72f4c66765f5e9ce3a1ff07f7d56a379a86518cf19a68adfa6f4361efc332042"
---

# AWS Backup for Amazon S3 now supports direct access to backup data

AWS Backup for Amazon S3 now supports creating S3 Access Points, providing immediate read-only access to backup data using standard S3 APIs without initiating a restore. This enables targeted file recovery, data validation, compliance auditing, and forensic investigation while your backup data remains protected in your backup vault.


You can create an access point for an S3 recovery point and read backup data using standard S3 operations such as GetObject, HeadObject, and ListObjectsV2. Access points work with both snapshot and continuous (point-in-time) recovery points stored in standard backup vaults or logically air-gapped vaults, including recovery points shared across accounts through AWS Resource Access Manager or Multi-party approval. While an access point is active, the associated recovery point is protected from deletion.


Get started with this capability by creating access points for your S3 recovery points using the AWS Backup console, API, or CLI. This capability is available in[select AWS Regions](https://docs.aws.amazon.com/aws-backup/latest/devguide/backup-feature-availability.html#features-by-region) .


To learn more, see[Access points](https://docs.aws.amazon.com/aws-backup/latest/devguide/backup-access-points.html) and[Amazon S3 backups](https://docs.aws.amazon.com/aws-backup/latest/devguide/s3-backups.html) in the AWS Backup Developer Guide and read the[launch blog](https://aws.amazon.com/blogs/storage/access-amazon-s3-backup-data-directly-using-s3-access-points-in-aws-backup/) . For pricing information, see[AWS Backup pricing](https://aws.amazon.com/backup/pricing/) .
