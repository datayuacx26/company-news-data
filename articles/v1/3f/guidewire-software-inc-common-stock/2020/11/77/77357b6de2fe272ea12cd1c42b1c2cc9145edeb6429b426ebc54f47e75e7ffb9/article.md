---
schema_version: "1.0.0"
document_id: "77357b6de2fe272ea12cd1c42b1c2cc9145edeb6429b426ebc54f47e75e7ffb9"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/introducing-guidewire-data-platform"
published_at: "2020-11-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:4b61631fcbde1e377ad75ef550925a48e739b817309f794ea63e6dda3b41dae3"
---

# Introducing Guidewire Data Platform

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Introducing Guidewire Data Platform


## An enterprise-grade, internet-scale, and cloud-native Big Data platform


Guidewire Data Platform is designed to ingest petabytes of data from Guidewire and third-party source systems in near real-time, unifies, transforms, and curates this data, and provides services and APIs to access this curated data in raw format or as visualizations with business intelligence. Multiple curation engines running in the unified data platform auto-curate ingested data and feed a rich set of advanced business analytics, and AI/ML SaaS solutions.


Guidewire Data Platform is designed to provide the following capabilities:


- *Data Ingestion:* Ingests unstructured, semi-structured, and structured relational business data
- *Curation Engines & Realtime streaming capabilities:* Curates data in batch mode or in real-time, and supports multiple curation engines
- *Data Lake:* Stores raw and curated data in a highly scalable Data Lake
- *Data Catalog:* Uses a Data Catalog for metadata management to make data secure, discoverable, self-serviceable, and traceable
- *APIs:* Provides access to raw and curated data
- *Enterprise-grade security and governance:* Provides enterprise-grade security and governance, including security and privacy governance for PII, RBAC, GDPR, and other requirements
- *Controlplane and Operational Automation:* Supports automated deployment, scaling, tenant onboarding, and observability


## 10,000-foot view of Guidewire Data Platform


Current Data Platform customers can leverage these robust capabilities to access raw and transformed data. The following two services are already an integral part of the Guidewire Cloud Data Platform:


- Guidewire Explore: A data analytics and visualization application that provides business insights and accelerates decision making.
- Guidewire Cloud Data Access: A solution that extracts incrementally changing and transactional raw data to an S3 bucket in Parquet format.


## Zoomed-in view of technology stack


GWDP uses the latest and greatest big data tech stacks on AWS, such as Kafka, EMR, Spark, Flink, and Kubernetes, with a totally elastic, horizontally scalable, API-first, and metadata-driven architecture.


## Architecture


## Data Ingestion


Data Platform provides Data Registration and Ingestion APIs to register data producers and capture real-time streaming data. One of our first goals was to stream every event/change from the Guidewire InsuranceSuite products (such as ClaimCenter, PolicyCenter and BillingCenter) in near real-time to Data Platform. To achieve this goal, we looked at proven technologies that we leveraged in our Live Analytics products, including Kafka Connect and Kafka, to collect the data. Using Kafka as our initial offering for ingestion allowed us to also provide an interface for other Guidewire applications to start publishing structured data for integration.


## Streaming CDC data from core systems


Data Platform uses the open-source frameworks[Kafka Connect](https://docs.confluent.io/3.0.0/connect/intro.html) and[Debezium](https://debezium.io/documentation/reference/1.1/connectors/postgresql.html#amazon-rds) to stream CDC data to Kafka. We are considering other open-source connectors to extend our CDC streaming options. Configuration of these connectors is automated — you only need to register a Data Product producer along with database connection details…that’s it! The rest of the configuration is handled by Data Platform!


## Data Producers


With the Aspen release of Data Platform, we can leverage external data producers from other Guidewire systems and ingest data from their Data Products into a secure, managed Kafka topic. After registration, all features of Data Platform such as the Data Catalog, Data Security, Data Lake access, and Visualization are available to that Data Product.


The Aspen release sets the stage for future capabilities such as supporting semi-structured or unstructured data from batch or API-based data producers. Guidewire Data Platform supports both JSON and Avro data formats.


## Data Catalog


Metadata management is a foundational feature of Guidewire Data Platform, and allows us to unlock capabilities such as Role/Attribute-Based Access (RBAC/ABAC), Curated Datasets (via Curation Engines), and Business Analytics (via Visualization Platform).


## Discovery


For data in any data platform to be usable, it must first be discoverable. In Data Platform, discovery starts with the Catalog service. You can traverse the metadata for all Data Products ingested into Data Platform for a given tenant and from there, learn about available datasets, data sources, and data lineage, as well as methods to address the data within the platform.


## Schema Evolution


The primary piece of metadata that we track for systems is the schema (via schema-on-write). Schema is also used as a means of tagging sensitive data at the field level by the rest of Data Platform. Every version of every schema written is tracked and stored by the Catalog service, enabling it to reconstruct any record ingested into Guidewire Data Platform at any point in time.


As schemas change in a source system, so does the metadata. Each schema is given a unique identifier (aka fingerprint) that allows us to correlate a given record in the platform to its schema.


## Security


Guidewire Data Platform covers security from ideation to deployment. Our data products use security methods that ensure Confidentiality, Integrity, and Availability. Rules can be written within our application to enforce data access and protection. Policies for data access are managed via integration with the Open Policy Agent (OPA), which defines declarative policies that can be tested and applied across entire Data Products. We have developed the Grant Update Service (GUS) to further enhance and enforce security. GUS can generate field-level access IAM policies from OPA policies and Schema tags. The IAM policies applied to the Data Lake offer federated and secure role-based access.


## Data Lake


All data ingested into Guidewire Data Platform is processed and stored in a long-term storage format using several AWS technologies such as Lake Formation, S3, and Glue, that allow for unlimited scalability. Data Platform supports “Lake Formation-izing” of data, which allows for fine-grained security access rules to be generated. Lake Formation is a fairly new AWS service intended to marry the scalability of S3 with low-level security policies that you find in a traditional datastore. Data stored in S3 can be “Lake Formation-ized”, which provides a way to allow for fine-grained IAM Policy to be crafted against individual attributes. The Data Lake can also be accessed using AWS Athena, which is a managed version of Apache Presto. AWS Athena allows for SQL queries to be executed over many distributed Parquet files. Because it is SQL-based, both JDBC and ODBC libraries can be used to access Athena (and transitively the Data Lake). Using IAM as an authorization provider to Athena ensures that all security policies crafted and translated by GUS are applied at all levels of access including JDBC/ODBC.


## Curation Engines


Data ingestion focuses on raw data sources. However, making sense of that raw data and creating views that are useful for downstream consumption is difficult and expensive. Data Platform provides multiple curation engines and a Designer API interface to simplify the process of creating curated views. Views published from the curation engine are registered as new Data Products that are ingested and cataloged like any other data product ingested.


## Designer API


Simplifying the inputs needed for data curation is where the Designer API shines. Designer understands how to inspect a Data Platform data product’s metadata and bootstrap the curation engine with all of the information it needs in order to publish a new curated Data Product. The API takes simple SQL input and constructs an execution plan in the curation engine to materialize a new view of data. This plan can include things such as joining data from different datasets, filtering data based on some criteria, or aggregating data into a de-normalized view. Designer can also manage both streaming and batch modes of operation (streaming is the default).


## Engines


In order to scale on-demand the processing capabilities required for these individual curations, we needed distributed processing frameworks (e.g. EMR -Spark, Hive, HDFS) and multi-node clusters that can handle large workloads and auto-scale based on real-time requirements. Based on the data type and data processing needs, the generic interface layer can seamlessly use different analytics engines such as EMR Curation Engine, Relational Engine, and Flink. Most curated views call for complex dereferencing, joins, and aggregations.


## Operational Automation and Controlplane


The built-in Controlplane makes it possible to deploy, operate, and auto-scale a complex cloud-native distributed system like Guidewire Data Platform.


We hope to never again have to ask questions such as “Where did this come from?” or “Who created this?”. By utilizing Infrastructure as Code (IaC) and tools such as Terraform, Cloud Formation, Kubernetes (EKS), we are able to fully capture every element that operates in Data Platform including microservices, configuration, and even VPC/networking details. We have developed a series of Terraform modules for Data Platform that allow configuration to drive the creation of uniform, standardized, and predictable environments. In addition to IaC, we took a step back and looked at the gathering of observability data at an infrastructure level. Logging and metric gathering/aggregation services are first-class citizens in Data Platform and follow the same quality standards and release practices that a business-centric service would. Monitoring and logging is a big part of the success behind Guidewire Data Platform. Having a single pane of glass to correlate all this information is an important step in being able to know when something goes wrong (or is about to go wrong).


## Guidewire Data Platform Summary


As you can see, we have been quite busy with our initial release. Look for future blog posts that will break down the current capabilities and underlying technologies into much finer detail, and also discuss some extremely difficult problems that we encountered and solved in the Aspen release of Guidewire Data Platform!


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
