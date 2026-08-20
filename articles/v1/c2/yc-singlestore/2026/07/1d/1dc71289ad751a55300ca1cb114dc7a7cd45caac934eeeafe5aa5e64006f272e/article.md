---
schema_version: "1.0.0"
document_id: "1dc71289ad751a55300ca1cb114dc7a7cd45caac934eeeafe5aa5e64006f272e"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/iceberg-needs-an-open-catalog-not-a-walled-garden/"
published_at: "2026-07-22T18:09:57+00:00"
first_seen_at: "2026-07-23T01:37:19.565412+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:6691d6bccd94e76bb5842f68a74cd24079137aa5b347baa7aa01d4557e656ffb"
---

# Iceberg Needs an Open Catalog, Not a Walled Garden

# Iceberg Needs an Open Catalog, Not a Walled Garden


Jul 22, 2026


•


3 min read


•


[Raj Verma, CEO](https://www.singlestore.com/blog/author/raj-verma/)


Iceberg is popular because it gives customers something enterprise software has spent decades taking away: leverage.


If your data is trapped inside one vendor’s warehouse, catalog, file format, permissions model and optimization path, then every future architecture decision starts with that vendor’s roadmap. Iceberg changes the starting point. It gives customers a durable table layer in object storage, with metadata, schema evolution, snapshots and interoperability that are not supposed to belong to any one compute engine.


Customers want their data to remain open, reachable and usable without rebuilding the foundation every time a new workload arrives or a better engine becomes available. That is the point of open table formats. Iceberg does not solve every problem in the data stack, but it solves an important one: it gives customers a vendor-neutral table layer on top of cloud object storage. The implementation details decide whether that promise survives contact with the rest of the platform.


## The ERP Lesson for the Lakehouse


This should feel familiar to anyone who lived through enterprise software consolidation. ERP suites promised simplicity through standardization. Buy finance, then procurement, then HR, then supply chain, then analytics, then the integration layer that became necessary because the first five systems were now in charge of half the company. The pitch was coherence. The result was often dependency.


Once a vendor owned the process model, data model, permissions, workflows, integrations and reporting surface, “choice” became something customers technically had and practically could not exercise without a long, expensive extraction project. The cost of leaving had become part of the architecture.


The lakehouse can repeat that pattern with more modern nouns. The lock-in may be subtler than the old warehouse model, and for that reason easier to miss at the beginning and harder to unwind later. It no longer has to live in the storage format or execution engine. It can live in the catalog and control plane. A customer may own the files in object storage and still be operationally dependent on one vendor to make those files usable, governed, performant and safe across the organization.


## The Iceberg Catalog Becomes the Contract


In practice, the catalog is where an open table format becomes an enterprise system.


Iceberg can define the table, but the catalog defines the working contract around it: who is allowed to see it, which version is trusted, how schemas change, how policies are applied, how engines coordinate, and what happens when multiple teams try to read, write, optimize and govern the same data at the same time.


That is where the architecture becomes real. Not in the claim that another engine can technically read the files, but in whether that engine can participate without losing the things enterprises actually depend on: consistent permissions, current metadata, safe writes, predictable performance and operational support.


A catalog that welcomes other engines as equal participants strengthens Iceberg. A catalog that makes every other engine feel like a guest at someone else’s house weakens it.


## Real-Time AI Raises the Stakes


This becomes more important as data applications become more operational. Traditional analytics could tolerate delay, duplication and platform-specific workflows because the work was often retrospective. A dashboard that updates later than the source system is annoying, but usually survivable. AI applications and real-time operational systems behave differently. They need fresh data, low-latency access, transactional updates, search, vectors and analytical context while the business is changing.


For those workloads, open storage is necessary but not sufficient. Customers still need an execution engine that can handle the workload, and they need an openness model that does not trap the data behind a proprietary control layer. That is why the catalog question is not a governance footnote. It affects what applications can be built, how quickly new engines can be adopted and whether AI systems can access the right data without turning every deployment into another integration project.


## The Next Lakehouse Battle Is Control


Open data should let customers use the architecture their applications require. For real-time AI and operational applications, that means fresh data, low latency, high concurrency, search, vectors, and transactional updates without forcing every access path through one vendor’s catalog. The data architecture should not force every access path through one proprietary catalog because that catalog has become the real system of record.


Iceberg was supposed to reduce lock-in, not move it upstairs into the catalog.


The next phase of the lakehouse market will not be decided only by who supports Iceberg. Support is now table stakes. The more important question is who preserves the customer’s freedom after Iceberg is adopted.


## On this page


- The ERP Lesson for the Lakehouse
- The Iceberg Catalog Becomes the Contract
- Real-Time AI Raises the Stakes
- The Next Lakehouse Battle Is Control


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Ficeberg-needs-an-open-catalog-not-a-walled-garden%2F)


[Product](https://www.singlestore.com/blog/category/product/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Open Intelligence, Open Data: SingleStore delivers Zero-copy Compute on Apache Iceberg Product](https://www.singlestore.com/blog/open-intelligence-open-data-singlestore-delivers-zero-copy-compute-on-apache-iceberg/)


[Blog Enterprise Identity, Your Way Product](https://www.singlestore.com/blog/enterprise-database-sso-scim-identity-integration/)


[Blog Float16 Vector Type Support in SingleStore: Cheaper, Faster, Better Product](https://www.singlestore.com/blog/float16-vector-type-support-in-singlestore-cheaper-faster-better/)


[Blog Data Lakehouses Are Being Redefined by Real-Time Expectations Product](https://www.singlestore.com/blog/data-lakehouses-are-being-redefined-by-real-time-expectations/)


[Blog Build AI Prototypes in Minutes Using Plain English Product](https://www.singlestore.com/blog/build-ai-prototypes-in-minutes-using-plain-english/)


[Blog How to Build High-Performance AI Applications on Snowflake: A Complete Guide Product](https://www.singlestore.com/blog/build-high-performance-ai-apps-on-snowflake/)
