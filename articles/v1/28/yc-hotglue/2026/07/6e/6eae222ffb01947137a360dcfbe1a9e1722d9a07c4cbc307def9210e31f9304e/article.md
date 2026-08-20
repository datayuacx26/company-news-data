---
schema_version: "1.0.0"
document_id: "6eae222ffb01947137a360dcfbe1a9e1722d9a07c4cbc307def9210e31f9304e"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-december-2025"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:b7d8498f476235986a75ab4e897bb6040e4bc3d7f21e8ac4bd6cb929128f103b"
---

# hotglue melt: December 2025

# 🚀 Product Updates


## 📖 Job Logs page updates


We’ve refreshed the job details / logs page to give you more context about your jobs! Our goal was to improve the visibility into the job to make debugging various issues easier.


The following information is now accessible directly in the job logs page:


- Compute Metrics → the provisioned resources and usage for your job
- Connector Versions → the connector versions (tag + repository) that ran in this job
- Overrides → job-level overrides
- Record-level target state → previously, visibility into each record that was synced to a target was limited. In addition to the count, you can now drill deeper and review each record, along with associated errors.


If you have any feedback on the new job logs page, let us know!


## 🔨 Widget v3 updates


In October we launched the new[v3 Hotglue widget](https://hotglue.com/blog/introducing-the-new-and-improved-widget) – since then we’ve been steadily adding more functionality! New features include:


- Support for file uploads
- Ability for tenants to trigger jobs and review job history
- Support for cloud file storage connectors like Google Drive and SharePoint


To learn more about the v3 widget,[check out the docs](https://docs.hotglue.com/widget-v3/overview) .


## 🕵️ Audit Trail


We are excited to introduce an **Audit Trail** add-on to give you full visibility into what’s happening inside your Hotglue environment.


When changes happen, it’s critical to know:


- Was this triggered by an end-user (tenant)?
- Someone on your internal team?
- Or programmatically via the API?


With Audit Trail, you can now:


- See a chronological timeline of changes
- Identify exactly who (or what) initiated an action
- Quickly trace configuration updates, connection changes, and API-driven events


No more guessing. No more “it worked yesterday.”


Just a clear, searchable history directly inside Hotglue.


👉 Available as a paid add-on.Reach out if you'd like to try it out.


# 🔌 Connector Updates


## ⚡ BigQuery + Postgres target performance improvements


We’ve rolled out major performance upgrades to both our **BigQuery** and **Postgres** targets — with significant speed gains across the board.


### 🟢 BigQuery: 5–10x Faster


We’ve completely revamped how data is processed and loaded:


-


Optimized JSON serialization & deserialization


-


Faster JSON Schema validation


-


Improved date parsing


-


In-memory caching to reduce repeated computation


-


New bulk loading flow:


- Data is first processed into Parquet files (one per stream)
- Files are uploaded to GCS
- BigQuery loads directly from GCS


The result: dramatically faster and more reliable warehouse loads at scale.


### 🟢 Postgres: 3–10x Faster


Postgres performance also saw major improvements:


- Optimized JSON serialization & deserialization
- Faster JSON Schema validation
- In-memory caching
- Removal of legacy features that were unused but impacting performance


Net result: materially improved throughput, especially for high-volume syncs.


That’s all for this month! Thanks for reading :)


Want to chat with our team?[Book a demo](https://hotglue.com/demo) .


TABLE OF CONTENTS


- 🚀 Product Updates
- 📖 Job Logs page updates
- 🔨 Widget v3 updates
- 🕵️ Audit Trail
- 🔌 Connector Updates
- ⚡ BigQuery + Postgres target performance improvements


RECOMMENDED BLOGS


[Introducing the new and improved widget Announcing the brand new version of the widget, featuring enhanced performance, customizability, and a refreshed design.](https://hotglue.com/blog/introducing-the-new-and-improved-widget)


[JavaScript & TypeScript Support for Hotglue Transforms Hotglue now supports writing transformation scripts in JavaScript or TypeScript in addition to Python!](https://hotglue.com/blog/js-ts-support-for-hotglue-transforms)


[Optimizing Microsoft SQL insert speed in Python: pymssql vs. pyodbc vs. bcp Learn how we optimized our Microsoft SQL target by using bcp to boost insertion speed and switching from pymssql to pyodbc.](https://hotglue.com/blog/optimizing-microsoft-sql-insert-speed-in-python)
