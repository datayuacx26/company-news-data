---
schema_version: "1.0.0"
document_id: "1f092c5e5dea609c6bdb5bb54bd3473687caad115c53b76bf8bc8d4991d217d1"
company_key: "yc-jitsu"
company: "Jitsu"
source_id: "yc-jitsu-news-import-65f0e2b767a6"
canonical_url: "https://jitsu.com/blog/introducing-notifications"
published_at: "2025-04-12T00:00:00+00:00"
first_seen_at: "2026-07-22T00:57:29.784311+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:7872a4dc55091bc9aa41511a88d43e4f8aeb23015fc48f60eeaab6c521c4ff47"
---

# Introducing Jitsu Notifications: Proactive Monitoring Made Simple

## Introduction


We’re excited to introduce automated notifications for job failures and recoveries!


Now, you’ll receive real-time Email or Slack alerts whenever a Connector Sync task or Data Warehouse Batch job fails or successfully recovers — ensuring you stay informed without constantly monitoring dashboards.


✉️ Jitsu Support / \[My workspace\] 🚨 Batch job failed: Site to Snowflake


### How it works


- Get notified when the first attempt to run a job is successful.
- Get notified immediately when a job fails, so you can take action fast.
- Get details about the failure in the notification, so you can quickly understand what went wrong.
- Receive a recovery notification when the job is back on track.
- Stay updated via email or Slack, based on your preferences.


But that isn’t all!


### Keeping Notifications Under Control


Jitsu ensures that you only receive relevant notifications, so you stay informed without unnecessary alerts:


- By default, for ongoing issues, you will receive additional notifications only once every 24 hours. You can adjust this frequency in theNotification Settings .
- Jitsu also detects so-called flapping statuses—intermittent failures followed by successful runs. When a flapping status is detected, Jitsu stops sending notifications for every individual status change. This ensures you aren’t overwhelmed with alerts for each failure and recovery.


### Events Queue size reporting


When a Data Warehouse Batch job doesn’t run successfully, you will receive a notification that includes the size of the events queue. This allows you to monitor the queue size and take action if needed.


See the screenshot above.


Queue size is also reported in **Data** -> **Live Events** -> **Batches & Data Warehouse Events** section of Jitsu UI.


### Partial Sync information


Source Connector Syncs are usually configured with multiple selected streams. During a Sync Task run, some streams may fail while others succeed. In such cases, Jitsu marks the sync as **partially successful** and sends a special notification that includes details about the failed streams.


The notification will include the following information::


```text
Last Status: PARTIAL
Streams Failed: 2 of 13


Details:
2025-04-03T23:00:55.496Z [PARTIAL] 2 of 13 streams failed. Failed streams: products, companies.


```


## How to enable


Email notifications are enabled by default for all workspace members.


Each user can manage their personal email notification preferences in the **Settings** -> **Notification Settings** .


You can also configure a Slack Notifications webhook in the same settings panel to receive the same notifications in a Slack channel of your choice.


Slack Notifications is the workspace-wide setting.


We hope you enjoy the new notifications feature and find it helpful in keeping your data pipeline running smoothly.
