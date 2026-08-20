---
schema_version: "1.0.0"
document_id: "9c6eb1211998b683bdb16a9287c3a5366745a7e1354507f9d552a39a822510eb"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-march-2024"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:bb1cc4e75d3454c9f3faca6bca1bc4e35eccb4d93cc0e6603744ac6c6e9eee94"
---

# hotglue melt: March 2024

Hey hotgluers,


**Welcome to the hotglue melt.**


Before we jump into what’s new in hotglue, we have a few major updates that we are slowly rolling out in a limited beta.


**Real-time writes** remove the need for a job process to POST data using our unified API. If you use our API to write data (not a reverse ETL from a Database or filestore), this lets you see the validations or success responses right when you POST. The limited beta is for Quickbooks and Shopify, with more to come with the general release.


The **real-time webhook handler** runs high-throughput trigger-based workflows without the need for full job processes. This is built on top of our real-time triggers–now they are just much faster, smaller, and more flexible.


If these are relevant to you, and you want to try them out before general release, let us know!


Reach out to us athello@hotglue.xyz


Now, for what’s new:


## **Connector-level schedules 🗓️**


With connector-level schedules, you can maintain individual schedules for each connector - either as an alternative or an addition to existing flow-level schedules. This enables use cases like:


- Running multiple schedules for the same connection, like an hourly sync and a weekly catch-all.
- Building automations to pause and re-enable certain integrations.


## **Updated email notification system 📢**


If you use emails to track job failures, you may have noticed some changes to the emails you get:


- **You can now configure email notifications on an environment-level, rather than across all environments** . Environment-level emails can be configured in **Settings** > **Notifications** , while Organization-level emails remain in your account settings.
- **Daily report emails now display the error messages and their associated tenants, rather than a list of failing tenants** . This should make it easier to track error patterns without needing to navigate your hotglue admin panel.


## **Other features 🚀**


-


**Restrict upload file types in the widget.
**In **Settings** > **Widget** , you can now configure a list of supported file types for file uploads. Use this to only allow discoverable files (csv and excel), or otherwise limit uploads to files that your ETL is prepared to handle.


-


**Jupyter speed boost
**Your environment and script-level dependencies are now downloaded and cached the first time you run Jupyter. That means that subsequent Jupyter startup times are significantly faster now.


-


**Memory failure auto-retriggers
**Large jobs occasionally need more memory and storage than is provisioned by default. In Settings > Jobs > Error management, you can now configure jobs to automatically retrigger with increased memory on OOM failures.


—


Got ideas? Shoot us a message in Slack or send us suggestions athello@hotglue.xyz .


See you next month! 👋


TABLE OF CONTENTS


RECOMMENDED BLOGS


[hotglue melt: February 2024 This is your monthly update for February!](https://hotglue.com/blog/hotglue-melt-february-2024)


[hotglue melt: 2023 Feature Roundup This is your 2023 hotglue feature update!](https://hotglue.com/blog/hotglue-melt-2023-Feature-Roundup)


[hotglue 101 Learn about the power of seamless integrations for your SaaS app by offering easy customer facing integrations quickly with hotglue.](https://hotglue.com/blog/hotglue-101)
