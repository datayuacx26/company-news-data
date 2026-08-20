---
schema_version: "1.0.0"
document_id: "ee834a54497f790a68a3d3a091ad670f490d6b885192f499644e1223443f066a"
company_key: "yc-propexo"
company: "Propexo"
source_id: "yc-propexo-news-import-1c9cf0eb2f62"
canonical_url: "https://propexo.com/blog/legacy-pms-data-integration/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T00:22:12.149801+00:00"
fetched_at: "2026-08-15T00:22:13.714040+00:00"
content_hash: "sha256:cf2b897da76af0450da7e64c90e4d98cee7e669537aa64f22f9fe6982cc5cd06"
---

# The PMS Data Egress Menu

For most of the last decade, the safe assumption about property management systems was that data had exactly one way out: the API, and often an archaic one. That assumption is now wrong, and it is wrong in an interesting way. The two largest platforms by units under management have each built a menu of egress paths, and the newest entries are weeks old. The constraint on PMS data is shifting from whether it can get out to which door you take.


## What egress paths does Yardi actually offer?


Five, spanning roughly twenty-five years of data architecture. Counting them is worth doing, because most integration conversations still assume there is one.


Path What it is Freshness What it asks of you


ETL file exports Scheduled CSV, XML or XLSX extracts from Voyager, the oldest door still in service Batch, on a schedule File handling, and all modeling downstream


SOAP and REST APIs The surface most third-party integrations are built on Full refresh per sync, in practice Static credentials somebody holds and rotates


Yardi Replicate Change-data-capture streaming of raw Voyager tables to an endpoint you control Near real time, incremental Private Cloud hosting, separate licensing, in-house engineers


Yardi Data Connect A curated semantic model, hundreds of dimensions and measures, delivered into Power BI Scheduled refresh An Azure and Power BI shaped stack


Virtuoso MCP connector Conversational read and write access for AI assistants, inside Yardi security controls Interactive Yardi Virtuoso, and an appetite for agent workflows


The two middle rows are the ones that break the old story.[Yardi Replicate](https://www.yardi.com/product/replicate/) uses change data capture to move only the records that changed, in near real time, to Snowflake, AWS or Azure.[Yardi Data Connect](https://www.yardi.com/products/data-connect/) goes the other direction: instead of raw tables it ships a governed, report-ready model into your own Power BI environment. And the[Virtuoso MCP connector](https://claude.com/connectors/yardi-virtuoso) points at where this is heading, letting an AI assistant query portfolio performance or manage invoice approvals conversationally rather than through a pipeline at all.


## What did RealPage just launch?


RealPage announced[Lumina Connect](https://www.realpage.com/solutions/lumina-connect/) in August 2026, and it is two products. Lumina Data Cloud provides zero-copy, read-only access to analytics-ready RealPage data inside Snowflake, Databricks, Microsoft Fabric and Google Cloud, with data available within hours of being updated and activation targeted at under an hour. Lumina Model Connect exposes governed data models to AI agents over MCP, the Model Context Protocol, with ChatGPT supported first and Claude and Microsoft Copilot support planned.


The design choices mirror Yardi’s menu without copying it. Zero-copy sharing means no replication pipeline at all, which is a genuinely different architecture from Replicate’s streaming. Both vendors, notably, kept governance in-house: the data arrives with the vendor’s definitions and metadata attached, and access respects the permissions and licensing already configured in the source system.


## If the data can get out, what is the hard part now?


Choosing, and then living with the choice. Each door trades something, and the trades compound.


**Prerequisites and cost.** Replicate requires Yardi Private Cloud hosting, its own license, and engineers to model what arrives. Data Connect assumes your reporting lives in Power BI. Lumina assumes one of four cloud platforms. Every path is a licensed add-on, not a default.


**Curated versus complete.** The friendliest paths expose a curated model, not the database. A governed semantic layer is a gift for BI and a ceiling for everything else: if the measure you need is not among the vendor’s definitions, or you disagree with how they compute it, you are back at a rawer door.


**Shape.** Data Connect data arrives Power BI shaped. Lumina data arrives inside a specific cloud platform. Replicate lands raw Voyager tables. Choose two doors for two workloads and you own the reconciliation between them.


**The multi-vendor gap.** Every path above stops at its own vendor’s boundary. An operator running Yardi in one region and RealPage in another now has two well-engineered proprietary data products that do not speak to each other, plus a long tail of leasing, maintenance and IoT systems with no menu at all.


## Is the legacy egress problem solved, then?


No, and it is worth being precise about what changed. The classic API surface is what it always was: Yardi’s integration APIs still authenticate with a username and password over SOAP and REST, and data still moves by full refresh on that path. The new channels sit alongside the old ones as licensed products, adoption is early, and nothing equivalent exists yet across most of the category.


What changed is the direction of travel. Legacy, in data terms, was never about the age of the software: it is about how the system was designed to let data out. By that definition the largest vendors are actively un-legacying themselves, one egress product at a time, and pressure on the rest of the category to follow will be real. A prediction worth writing down: within a few years, egress design will be a line item in PMS selection the way mobile apps were a decade ago.


## How should a data team use the menu?


**Inventory the doors before building anything.** The right first question changed this year. It is no longer whether data can leave your PMS but which of several supported paths fits your stack, your licensing, and your freshness requirement.


**Match the door to the workload.** Single-vendor BI in Power BI is what Data Connect exists for. A warehouse-centric team on Snowflake with RealPage should look hard at Lumina. Interactive questions belong to MCP connectors, which are conversation tools, not pipelines. Raw completeness, custom modeling and multi-source joins point at replication, or at a managed pipeline.


**Keep your own semantic layer.** Vendor-governed definitions are convenient until you need a metric they did not anticipate, or run two systems whose definitions disagree. Landing source data in your own warehouse and modeling it there keeps the definitions versioned, testable and yours.


**Plan for the multi-vendor reality.** This is where the menu runs out, and where we should be direct about our own position.[Propexo Connect](https://propexo.com/connect/) is a managed extract-and-load pipeline that lands source-native tables from Yardi, RealPage, Entrata, AppFolio and the rest of the multifamily stack in the warehouse you own. The[Unified API](https://propexo.com/unified-api/) then returns a consistent schema across those systems, so a query for vacant units behaves the same everywhere. For the mechanics across three systems at once, see[building a single source of truth across Yardi, RealPage and Entrata](https://propexo.com/how-to/single-source-of-truth-across-yardi-realpage-entrata/) .


The egress menu is good news, including for us: every door the vendors open makes the data conversation easier to have. But a menu is not a meal. Somebody still has to pick the doors, pay for them, and make what comes out of each of them agree.
