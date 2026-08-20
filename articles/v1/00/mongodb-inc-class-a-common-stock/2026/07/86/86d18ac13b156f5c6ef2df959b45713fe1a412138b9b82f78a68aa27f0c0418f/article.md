---
schema_version: "1.0.0"
document_id: "86d18ac13b156f5c6ef2df959b45713fe1a412138b9b82f78a68aa27f0c0418f"
company_key: "mongodb-inc-class-a-common-stock"
company: "MongoDB Inc."
source_id: "mongodb-inc-class-a-common-stock-news-import-efe1743dc302"
canonical_url: "https://www.mongodb.com/company/blog/product-release-announcements/atlas-stream-processing-brings-operational-data-to-apache-iceberg"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-24T04:07:24.248205+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:c6b2ca548112e6dcde5a9dd885ed10e7020c7287f684e3d571e0128548e72ae9"
---

# Atlas Stream Processing Brings Operational Data to Apache Iceberg

Every order placed, message sent, or sensor reading captured lands in an operational database. Analysts, data scientists, and AI teams all want that data, because the freshest signal about a business lives in the systems running it. But getting this data from the application layer to the lakehouse, continuously and at scale, has been one of the most stubborn problems in the modern data stack.


Today,


[MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database) closes that gap—without collapsing operational and analytical workloads into a single bottleneck.


MongoDB


[Atlas Stream Processing](https://www.mongodb.com/docs/atlas/atlas-stream-processing/) now writes operational data directly to Apache Iceberg through a new


*$iceberg*


aggregation stage, continuously synchronizing Atlas collections to Iceberg tables on AWS object storage. The data that powers your applications becomes available to your lakehouse in near real time, and the tangle of custom infrastructure that used to sit in the middle—along with the cost of running it—largely disappears.


## **The gap between operational data and the lakehouse**


For most organizations, these are already two different worlds. MongoDB Atlas supports dynamic, transactional applications, as well as real-time search and retrieval that operate on the same operational data. A lakehouse platform runs the analytics. Apache Iceberg sits between them, the open table format teams have standardized on, so analytical data stays portable across engines. The work of building on operational data depends on getting it from the first world into the second.


That handoff is where the difficulty lives. Bridging it has meant assembling a pipeline: capture change data from MongoDB, route events through streaming middleware, build and maintain connectors, land the data in object storage, then separately manage schema changes, updates, deletes, metadata registration, and file compaction. Every stage adds additional cost and operational overhead. Every tool adds a dependency, a place to break, and another reason analytics falls behind the application it depends on.


Schema drift is where it really hurts. A developer adds or removes a field, and brittle pipelines stall until someone fixes them by hand. What should be a routine application change becomes an analytics outage, and the bill keeps climbing across connector fees, the underlying infrastructure, and the engineering hours spent holding it together.


## **One stage, from change stream to Iceberg**


Native Iceberg support collapses that pipeline into a single aggregation stage. With the new *$iceberg*


stage, a team can define a continuous, schema-aware, fully managed pipeline directly in the Atlas control plane in a few lines of configuration, rather than a stack of standalone services.


From there, Atlas Stream Processing does the rest. It reads the[change stream](https://www.mongodb.com/docs/manual/changestreams/) , processes and transforms data in flight, writes it to Iceberg v2 tables on AWS S3, and registers that data through catalog integration so downstream tools can find it. The connectors, transport layer, and maintenance jobs that used to carry out this work now live within a single managed stage that runs where the data already is.


**Figure 1.** Flow of data from MongoDB to Apache Iceberg.


MongoDB Atlas captures operational data in the OLTP database, processes change events through Atlas Stream Processing, and automatically applies inserts, updates, and deletes to an Apache Iceberg table in Amazon S3, with AWS Glue handling metadata updates for downstream table access.


## **Engineered for the realities of production**


Getting data into Iceberg is the part everyone expects. The real work shows up once that data is flowing at scale, and that is where this release does the heavy lifting.


Take schema. MongoDB Atlas infers Iceberg schemas directly from the output stream, so you do not have to define target schemas up front. When your application’s data models change, the *$iceberg*


updates the Iceberg table schema to match—a change that used to break a downstream pipeline now flows through, and your analytics stay current with your application.


Partitioning matters just as much for query speed. The *$iceberg*


stage handles identity, truncate, bucket, and temporal partitions such as year, month, day, and hour, and Atlas applies sensible type-aware defaults when it creates a table. You get layouts built for fast writes and fast queries without turning them by hand.


Then there is finding the data once it lands. When integrated with AWS Glue Data Catalog, the Iceberg tables that Atlas Stream Processing writes appear across the rest of the analytics stack, including Snowflake, Databricks, and AWS Athena. You write the data once, and it is ready wherever teams already work.


Streaming into object storage also tends to leave behind a pile of small files that drag down query performance. Atlas Stream Processing batches the writes and runs compaction as data arrives, so your tables stay fast to query as they grow.


## **From operational data to analytics and AI**


The result is a simpler architecture, and the benefits compound:


-


Fewer systems to maintain, and fewer failure points to chase.


-


Operational data reaches the lakehouse fast enough to support timely decisions.


-


Analytics delivery runs on the same platform as the applications it draws from.


-


Engineering time once spent keeping pipelines alive shifts to the analytics and AI work the data supports.


-


Teams maintain fewer systems and remove the failure points that came with each one, and operational data reaches the lakehouse fast enough to support timely decisions.


Native Apache Iceberg support is generally available today for MongoDB Atlas customers on the SP10 and above tiers of Atlas Stream Processing.


## **Where we’re taking it next**


This release is the first step in a broader investment in open lakehouse interoperability, and the roadmap is built around meeting teams where they already run.


On the catalog side, we are currently planning to expand beyond AWS Glue Data Catalog to support Snowflake Open Catalog and Databricks Unity Catalog, so Iceberg tables written by Atlas can register cleanly in more catalogs governing a teams’ analytics estates. On the cloud side, we are currently planning to extend Iceberg support to Microsoft Azure, with writes to Azure Blob Storage and ADLS Gen2, so teams standardized on Azure get the same managed path from operational data to the lakehouse.


The direction is consistent: more catalogs, more clouds, and a single managed way to deliver operational data into open analytical formats wherever it needs to land.


###### Next Steps


Ready to connect your operational data to your lakehouse? Dive into the MongoDB Atlas Stream Processing[documentation](https://www.mongodb.com/docs/atlas/atlas-stream-processing/sp-agg-iceberg/) to configure your first $iceberg stream processor today and start delivering operational data to Apache Iceberg on AWS.


***Note:*** ** *This blog post includes certain "forward-looking statements" within the meaning of Section 27A of the Securities Act of 1933, as amended, or the Securities Act, and Section 21E of the Securities Exchange Act of 1934, as amended, including relating to our product roadmap. These forward-looking statements reflect our current views about our plans, intentions, expectations, strategies, and prospects, which are based on the information currently available to us and on assumptions we have made. Actual results may differ materially from those described in the forward-looking statements and are subject to a variety of assumptions, uncertainties, risks, and factors that are beyond our control, including those risks detailed under the caption “Risk Factors” and elsewhere in our Securities and Exchange Commission filings and reports. Except as required by law, we undertake no duty or obligation to update any forward-looking statements contained in this release as a result of new information, future events, changes in expectations, or otherwise.*
