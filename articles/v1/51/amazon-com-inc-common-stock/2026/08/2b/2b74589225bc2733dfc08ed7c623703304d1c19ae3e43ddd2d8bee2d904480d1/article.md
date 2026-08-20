---
schema_version: "1.0.0"
document_id: "2b74589225bc2733dfc08ed7c623703304d1c19ae3e43ddd2d8bee2d904480d1"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/cloudwatch-alarms-wallclock-evaluation"
published_at: "2026-08-07T08:00:00+00:00"
first_seen_at: "2026-08-11T17:03:23.922129+00:00"
fetched_at: "2026-08-11T17:03:27.183313+00:00"
content_hash: "sha256:e6dbaa9c236fa22147d4c12f7efebc0313b92676c0e602cf82601f1ed2998648"
---

# Amazon CloudWatch Alarms now supports wall clock evaluation windows

Today, Amazon CloudWatch announces wall clock evaluation windows for metric alarms, enabling customers to align alarm evaluations to fixed calendar boundaries such as the top of the hour, midnight, or the start of the week. This new option complements the existing sliding window behavior and is designed for customers who monitor scheduled or business-aligned workloads.


With wall clock evaluation windows, customers can avoid false alarms that occur when events cross rolling window boundaries. For example, a daily backup alarm using a sliding window can trigger incorrectly if consecutive backups are slightly more than 24 hours apart, even though each calendar day had a successful backup. A wall clock window evaluates each calendar day independently, eliminating this issue. Customers can also specify a time zone so that daily alarms align to their local business day, with daylight saving time transitions handled automatically.


Wall clock evaluation windows for CloudWatch Alarms are available in all AWS Regions where Amazon CloudWatch is available, except the Middle East (UAE) and Middle East (Bahrain) Regions.


To get started, see[Alarm evaluation window](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/alarm-evaluation-window.html) in the Amazon CloudWatch User Guide. To learn more about Amazon CloudWatch Alarms, visit the[Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) product page.
