---
schema_version: "1.0.0"
document_id: "faab83275fa1ddf5f39ee11509c08e80231729ab51450659b92e9bdd792d5d39"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/timestream-influxdb-backup-restore/"
published_at: "2026-08-07T19:21:00+00:00"
first_seen_at: "2026-08-07T20:32:10.069700+00:00"
fetched_at: "2026-08-07T20:32:12.526409+00:00"
content_hash: "sha256:54f84125c4744a9e4a76cc057d4dd26058ab19e2af21a49f14959576b4af9ffe"
---

# Amazon Timestream for InfluxDB now supports backup and restore

Amazon Timestream for InfluxDB now lets you create and manage your own backups and restore your data on demand. You can trigger one-time, on-demand backups, schedule automated recurring backups at the frequency and retention you choose, and restore a backup to a new resource or in place of an existing one. This capability is available for both the InfluxDB 2 and InfluxDB 3 engines through the AWS Management Console, the AWS CLI, and the Timestream for InfluxDB API.


With this capability, you control your data protection strategy. You can take an on-demand backup before a risky migration or configuration change. You can also define up to four automated backup configurations per resource using hourly, daily, weekly, monthly, or custom schedules, each with its own retention period. The first backup captures a full copy of your database, and subsequent backups are incremental, reducing the performance impact of ongoing backups. When you restore a backup, you can create a new resource that inherits the source configuration or replace an existing resource. If the source resource uses a Customer Managed key (KMS), its backups use the same key.


Customer-driven backup and restore is available in all AWS[Regions](https://docs.aws.amazon.com/general/latest/gr/timestream.html) where Amazon Timestream for InfluxDB is available. To get started, open the Amazon Timestream console. For more information, see the[Amazon Timestream for InfluxDB documentation](https://docs.aws.amazon.com/timestream/) and[pricing page](https://aws.amazon.com/timestream/pricing/) .
