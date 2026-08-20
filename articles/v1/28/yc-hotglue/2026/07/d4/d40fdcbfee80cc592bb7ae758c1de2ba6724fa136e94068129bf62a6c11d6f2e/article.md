---
schema_version: "1.0.0"
document_id: "d40fdcbfee80cc592bb7ae758c1de2ba6724fa136e94068129bf62a6c11d6f2e"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-may-2025"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:91fd7c883eab1371c608c1bf814d7e7d645c7e8ae119f27d1027ab7ae1acd1bd"
---

# hotglue melt: May 2025

# 🚀 Product Updates


## 📖 Universal support for upserts with external IDs


When writing data to a connector there are many cases where you may want to later update that same record again. A handful of systems (such as Salesforce and NetSuite) natively support the concept of an **external id** – this essentially allows you to use your own ID to reference an object rather than having to store the internal id of that system.


hotglue now supports external id upserts natively in our[target-hotglue-sdk](https://gitlab.com/hotglue/target-hotglue-sdk) – this means that even systems that **do not** natively support external ids can support external ids via hotglue. Check out the demo below with HubSpot:


This feature is in beta currently, and we plan to expand support to other targets across our library. If you’re interested in trying it out, let us know! Read the docs here:[https://docs.hotglue.com/key-concepts/jobs/write-jobs#external-id](https://docs.hotglue.com/key-concepts/jobs/write-jobs#external-id)


## 🔨 Connector Builder Updates


We have had a great response to the new connector builder! It’s been so great to see what you all have been able to build with it already.


Many of you submitted feature requests on the connector builder, and we have added support for:


- Parent/Child streams
- OAuth
- Bearer token authentication
- Static streams


Check out the demo of creating a custom OAuth connector below:


Read the connector builder docs here:[https://docs.hotglue.com/custom-connectors/connector-builder](https://docs.hotglue.com/custom-connectors/connector-builder)


If you have questions on using the connector builder,contact us !


## 🖇️ Subtenant symlinks


There may be cases where it’s useful to reuse a connection across multiple subtenants.


For example, you may want to configure a unique field map per subtenant but read data from a single Salesforce instance. This is now possible using **symlinks** via our linked sources API!


Read the docs here:[http://docs.hotglue.com/api-reference/v1/linked-sources/link-a-source#body-source-symlink-tenant](http://docs.hotglue.com/api-reference/v1/linked-sources/link-a-source#body-source-symlink-tenant)


# 🔌 Connector Updates


## ⚡ Shopify GraphQL connector performance enhancements


For folks using our Shopify GraphQL connector (` shopify-v2` ), we have rolled out some major performance improvements.


When syncing` orders` , the previous performance of the Shopify connector was ~500 records/min – with the new version we are able to sync **~2.5k records/min** 🚀


The new version does include some changes to the schema of the data. If you’re interested in upgrading to the new version,contact us !


That’s all for this month! Thanks for reading :)


Want to chat with our team?[Book a demo](https://hotglue.com/demo) .


TABLE OF CONTENTS


- 🚀 Product Updates
- 📖 Universal support for upserts with external IDs
- 🔨 Connector Builder Updates
- 🖇️ Subtenant symlinks
- 🔌 Connector Updates
- ⚡ Shopify GraphQL connector performance enhancements


RECOMMENDED BLOGS


[hotglue melt: April 2025 Connector Builder, real-time write, streaming read jobs, accounting unified schema v2, and more!](https://hotglue.com/blog/hotglue-melt-april-2025)


[hotglue melt: March 2025 10x faster discovers for CRM connectors, dark mode, and more!](https://hotglue.com/blog/hotglue-melt-march-2025)


[Custom Mapping for CRM Integrations: Why It’s Better and How to Do It CRMs revolve around custom data. With custom mapping you can make your CRM integration more useful to users while decreasing onboarding time.](https://hotglue.com/blog/custom-mapping-for-crm-integrations-why-its-better-and-how-to-do-it)
