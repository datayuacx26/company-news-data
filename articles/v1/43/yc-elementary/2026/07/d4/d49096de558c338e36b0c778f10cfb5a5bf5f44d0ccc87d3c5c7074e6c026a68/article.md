---
schema_version: "1.0.0"
document_id: "d49096de558c338e36b0c778f10cfb5a5bf5f44d0ccc87d3c5c7074e6c026a68"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/october-product-announcements"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:67028853a2be5df7e196e0c88c6d6dad2826d0b4a476c1e357b08b824f2d0e2b"
---

# October Product Announcements | Elementary Data

This month, we’re expanding visibility into Iceberg environments, improving how you monitor and review data health, and continuing to refine stability and automation across the platform.


We also have something big coming soon — a major expansion that will make Elementary valuable for **an even broader set of data assets** . Stay tuned.


## 🧊 Iceberg Support with AWS Glue


Elementary now supports **AWS Glue** — our first **Iceberg Catalog** integration.


Iceberg’s flexibility gives data teams a lot of power, but it also makes observability challenging — metadata about write history can be split across different engines and layers.


Elementary solves this by connecting directly to **Iceberg’s own metadata** , specifically **Iceberg snapshots** .


Snapshots record every version of a table after each write, and their metadata includes timestamps and statistics that Elementary uses to **automatically monitor volume and freshness anomalies** .


Currently, Elementary supports the **Glue Iceberg catalog together with Dremio** , with **additional catalogs and engines** coming soon.


🔗 Learn more in the[Glue integration docs](https://docs.elementary-data.com/cloud/integrations/metadata-layer/glue) .


## 💡 Multiple Filters on Main and Data Health Dashboards


You can now **combine multiple filters** when reviewing dashboards.


Filter by domain, tag, owner, or priority to dig deeper into your data’s heath — faster and with more precision.


## ⚙️ Stability and Performance


We’ve put a lot of effort into **reducing sync times** and **improving overall system stability** .


These updates make Elementary faster and more reliable across large environments and heavy workloads.


## 📈 Automated Monitors improvements


We’re continuing to improve the accuracy and coverage of **Automated Monitors** , expanding the use cases and edge cases we support.


Recent updates include:


- Better support for **scheduled queries**
- A **Z-score fallback mechanism** when a pattern isn’t detected
- More precise handling of **small changes in large tables**


These refinements make anomaly detection more adaptive and resilient across a wider range of data behaviors.


## 🔔 Version Update Notifications


An important change based on your feedback: **Elementary will no longer open automatic pull requests for version updates.**


Instead, you’ll now receive **Slack and email notifications** whenever a new version is available, giving you full control over when to update.


If your team prefers to **keep automatic PRs** , let us know and we’ll keep it enabled for your organization.
