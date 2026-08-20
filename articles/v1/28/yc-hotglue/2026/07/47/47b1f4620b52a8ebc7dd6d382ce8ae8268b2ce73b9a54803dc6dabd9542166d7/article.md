---
schema_version: "1.0.0"
document_id: "47b1f4620b52a8ebc7dd6d382ce8ae8268b2ce73b9a54803dc6dabd9542166d7"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-april-2025"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:9ebab70749dd89d3a12740f7eb3b95ae635f4d2e210a02922fade17e35551a5b"
---

# hotglue melt: April 2025

# 🚀 Product Updates


## 🔨 Connector Builder


We are excited to release our new connector builder for taps! Using the connector builder, you can create taps on hotglue without writing any code at all. Check out the demo below:


Read the docs here:[https://docs.hotglue.com/custom-connectors/connector-builder](https://docs.hotglue.com/custom-connectors/connector-builder)


If you have questions on using the connector builder,contact us !


## ⚡ Real-time write


When writing data to a connector – for example, creating a Contact in Salesforce or an Order in Shopify – seeing that data go through immediately and giving the user instant feedback can be a game changer.


Instead of kicking off an asynchronous hotglue job to write data, you can now use our API to send data to connectors **instantly** . Check out the demo below:


This feature is in beta and we are expanding which connectors are supported (at the time of writing, Salesforce and Shopify are live). If you’re interested in trying this feature out, let us know! More info on the docs:[https://docs.hotglue.com/api-reference/real-time/write](https://docs.hotglue.com/api-reference/real-time/write)


## ⛓️ Streaming Read Jobs


In cases where the Python transformation layer is not needed, you can now **stream** data directly from a connector to a target.


### What does that mean?


Traditionally hotglue jobs go through each step of the ETL pipeline:


- extract (read raw data from a connector like HubSpot),
- transform (use our Python layer to process the data), and finally
- load (write the final data to a target like your API or a database).


Instead of following that lifecycle, streaming jobs do both the extract and load steps concurrently, which means we send data to your target as we are pulling it from the source system.


This can be super useful for scenarios where you’re importing lots of historical data, and want your customers to see that data being populated while the import is in progress.


Check out the demo here:


This feature is in beta currently, and we plan to expand support to database targets such as Postgres, BigQuery, and more. If you’re interested in trying it out, let us know!


# 🔌 Connector Updates


## 🧾 Accounting unified schema v2 for NetSuite (now in beta!)


Our engineering team has been hard at work on creating v2 of our accounting unified schema! We are launching beta access to the new version with NetSuite, and we plan to release support for Microsoft Dynamics Business Central and Sage Intacct next.


The new version features major improvements including:


- better performance across read and write jobs
- improved error messages and validation for writing data
- clear documentation of supported objects and how they map to each connector
- schema and type enforcement across connectors for both read and write jobs to ensure high data quality
- target state for write jobs with summaries of posted records and detailed validation errors


Check out the docs here:[https://hotglue.com/docs/unified/accounting](https://hotglue.com/docs/unified/accounting)


That’s all for this month! Thanks for reading :)


Want to chat with our team?[Book a demo](https://hotglue.com/demo) .


TABLE OF CONTENTS


- 🚀 Product Updates
- 🔨 Connector Builder
- ⚡ Real-time write
- ⛓️ Streaming Read Jobs
- 🔌 Connector Updates
- 🧾 Accounting unified schema v2 for NetSuite (now in beta!)


RECOMMENDED BLOGS


[hotglue melt: March 2025 10x faster discovers for CRM connectors, dark mode, and more!](https://hotglue.com/blog/hotglue-melt-march-2025)


[Custom Mapping for CRM Integrations: Why It’s Better and How to Do It CRMs revolve around custom data. With custom mapping you can make your CRM integration more useful to users while decreasing onboarding time.](https://hotglue.com/blog/custom-mapping-for-crm-integrations-why-its-better-and-how-to-do-it)


[Introducing Magic Link Imagine if you could just send a link to your users to connect all their integrations… now you can using our new Magic Link feature 🪄](https://hotglue.com/blog/introducing-magic-link)
