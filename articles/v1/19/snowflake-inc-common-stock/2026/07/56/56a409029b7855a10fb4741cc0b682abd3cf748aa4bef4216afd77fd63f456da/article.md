---
schema_version: "1.0.0"
document_id: "56a409029b7855a10fb4741cc0b682abd3cf748aa4bef4216afd77fd63f456da"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/snowflake-google-cloud-open-lakehouse"
published_at: "2026-07-29T16:21:00+00:00"
first_seen_at: "2026-07-29T20:25:10.388349+00:00"
fetched_at: "2026-07-29T20:25:12.148799+00:00"
content_hash: "sha256:b3cfc98358fe60cde61bb06eeada10e6cae1965f28f790f7cc261d6de8f66b24"
---

# Apache Iceberg Lakehouse: Snowflake & Google Cloud

## Iceberg: From silo to interoperability


Every database used to own its data, which worked when organizations had one analytics engine. Today's data teams often combine Apache Spark™, BigQuery, Gemini Enterprise Agent Platform, Snowflake (including Snowflake's CoCo and CoWork) and other services depending on the job — often within the same pipeline.


This created a dichotomy — choose flexibility or consistency:


- Let every team use its preferred engine and replicate data across systems, introducing redundancy, inconsistency and risk.
- Force everyone through one engine and sacrifice the specialization and autonomy that multi-engine architectures provide.


The industry needed a third option, and it emerged from a fundamental insight known as the Data Locality principle: It is faster, cheaper and more secure to move a small piece of executable code to where data already lives than to move massive volumes of data across a network.


These ideas converge on a single pattern: Instead of copying data to each engine, bring each engine's compute to the data. All data sits in the customer's own storage bucket, and all engines agree on how it is physically laid out so each can read and write directly to the same table.


This requires an agreed-upon open table format — a shared language all engines understand.


The industry converged on[Apache Iceberg](https://iceberg.apache.org/) ™ as that format. Developed at Netflix for petabyte-scale table management and later donated to the Apache Software Foundation, Iceberg is supported by Spark, Trino, Flink, BigQuery, Snowflake and dozens of other engines. With Iceberg as the shared language, every engine participates without proprietary adapters.


## Catalog: From Iceberg to lakehouse


An open format largely solves interoperability, but it introduces a new question: Who is in charge? When multiple engines can read and write the same files, someone must manage table metadata, enforce access policies, coordinate concurrent writers and ensure no engine sees stale or inconsistent state. That role belongs to the catalog, which serves as the lakehouse's governance layer. It is the single authority that knows which tables exist, what their schemas look like, who can access them and where data files physically reside. Without a catalog, open data is ungoverned data.


The catalog also controls storage access through vended credentials. When an engine requests table data, the catalog returns short-lived, narrowly scoped storage tokens rather than standing bucket credentials, so engines do not gain persistent access to the underlying storage layer.


For this model to work across engines built by different vendors, catalogs need a standardized integration protocol. Without one, every engine would require a custom integration with every catalog. The[Iceberg REST Catalog (IRC)](https://iceberg.apache.org/rest-catalog-spec/) is the open API specification that solves this: It defines how clients discover namespaces, load table metadata and commit updates. Every catalog exposes an IRC endpoint, and every engine connects as a client.


This combination of vended credentials and a standard protocol makes it practical for engines to cross catalog boundaries — what the industry calls federation. When a Snowflake query reaches into Google Cloud's Lakehouse catalog, or a BigQuery job reads from Snowflake Horizon, each catalog issues only the narrow, time-limited tokens the requesting engine actually needs, so governance is enforced regardless of which engine initiates the request. True interoperability requires this in both directions: inbound federation, where engines external to your catalog can read and write your Iceberg tables, and outbound federation, where your compute can read and write Iceberg tables managed by a different catalog.


Together, the catalog and vended credentials provide a single-authority governance layer enforcing security, controlling access and centralizing audits. Customers can now get bidirectional access to Iceberg tables across catalogs, allowing for zero-copy architecture across Iceberg-compatible engines.


The key architectural question becomes: Who manages the catalog? Customers can deploy a DIY self-managed catalog or more likely use a managed Iceberg REST Catalog.


For enterprises who want to DIY, they can deploy Apache Polaris™, a popular open source IRC implementation, originally co-created by Snowflake and Dremio and donated to the Apache Software Foundation. This gives these organizations full control, but it also shifts full operational responsibility — including infrastructure, scaling, patching and availability — to the customer's team. Unlike managed solutions, DIY catalogs are not serverless and do not scale to zero when not in use, creating an ongoing operational cost.


In practice, most organizations choose a managed IRC so they can focus on data rather than catalog operations. When Snowflake and Google Cloud are both in the picture, each provides a natural fit:


- [Lakehouse runtime catalog](https://cloud.google.com/products/lakehouse?e=a) (Google Cloud's managed Iceberg catalog, part of[Lakehouse for Apache Iceberg](https://cloud.google.com/products/lakehouse) ) is a serverless and scalable metastore that serves as a single source of truth for your data lakehouse. It allows multiple engines (BigQuery, Google-managed Spark, Apache Spark, Trino, Snowflake) to access the same copy of data across open formats like Apache Iceberg. Federation allows agents and engines in Google Cloud to access data across borders from catalogs in Snowflake, Databricks and AWS Glue.
- [Snowflake Horizon Catalog](https://www.snowflake.com/en/product/features/horizon/) integrates Apache Polaris and embeds standards-compliant IRC endpoints to every Snowflake account with no additional setup. Beyond basic catalog operations, Horizon layers on enterprise governance: RBAC, column-level masking, row access policies, data lineage and audit logging. It allows Iceberg-compliant engines like Trino, BigQuery, Apache Flink and many others to read and write to Snowflake-managed Iceberg tables.


## Federation: From lakehouse to open lakehouse


Lakehouse runtime catalog, Snowflake Horizon Catalog and other managed catalog options are not mutually exclusive. They can coexist in the same organization through[catalog federation](https://docs.cloud.google.com/lakehouse/docs/use-catalog-federation) , enabling organizations to start with a single catalog and federate incrementally as needs evolve.


Each catalog manages its own tables while all engines access them through IRC.


When Google Cloud manages the catalog via the Lakehouse runtime catalog, Snowflake connects through a[Catalog-Linked Database (CLD)](https://docs.snowflake.com/en/user-guide/tables-iceberg-catalog-linked-database) that automatically discovers and syncs tables from its IRC endpoints. Snowflake users interact with those Google Cloud's Lakehouse Iceberg tables using standard SQL —` SELECT` ,` INSERT` ,` UPDATE` ,` DELETE` — or via Snowflake's CoCo or CoWork, as if they were native tables, with Snowflake governance (RBAC, masking, lineage) layered on top.


Similarly, when Snowflake manages the catalog via Horizon, Google Cloud services reach those tables through Lakehouse catalog federation — BigQuery, Managed Service for Apache Spark and other Iceberg-compatible engines in Google Cloud connect to Horizon's IRC endpoint and read/write to Snowflake-managed Iceberg tables directly.


The result is a fully open, interoperable data platform: All engines read and write directly from customers' Cloud Storage Bucket with no ETL between systems, and each team picks the best tool for the job, based on the team's unique preferences. Meanwhile, the catalog layer enforces unified governance across all of them.


## Semantic context: From open lakehouse to AI-ready


A well-architected lakehouse largely solves data interoperability, but interoperability alone does not make data useful to AI. Between "data is accessible" and "AI produces accurate, trusted answers and actions" lie three gaps:


- Programmatic access
- Semantic grounding
- Contextual intelligence


**Programmatic access**
Human analysts query tables through their preferred engine, but AI agents need their own channels. Snowflake exposes model context protocol (MCP) servers that allow any MCP-compatible client to discover, query and reason over lakehouse data. MCP is an open standard that provides a universal interface between AI applications and data sources, meaning Gemini Enterprise, code assistants and custom agent frameworks all connect through the same protocol rather than requiring bespoke integrations. Likewise, BigQuery also offers an MCP server that is accessible from both first-party and third-party AI agents, enabling programmatic access to lakehouse assets for agentic workflows across Google Cloud and multi-cloud environments.


For applications that need direct programmatic control, such as orchestration frameworks, custom pipelines or embedded analytics, Cortex Agents and Gemini Enterprise agents are also accessible via REST API. Together, MCP and REST give AI agents the same governed, authenticated access to Iceberg tables that human analysts have, regardless of which catalog manages them.


**Semantic grounding**
AI systems hallucinate when they don't understand what data represents. Semantic models bridge this by defining business logic on top of physical tables: metrics with calculation rules, dimensions with hierarchies, relationships between entities and verified query patterns that encode institutional knowledge. The critical insight is that business logic belongs in the data layer, not in AI prompts. When definitions live in a semantic model atop the data, every AI system inherits the same correct logic, reducing drift, inconsistency and guesswork.


[Semantic View Autopilot](https://docs.snowflake.com/en/user-guide/views-semantic/autopilot) , part of Horizon Context, makes this sustainable at scale. Rather than requiring analysts to manually define hundreds of semantic models, Autopilot automatically discovers and maintains them by learning from query patterns, user behavior, table relationships, BI tool integrations and report usage. For teams that prefer to author and refine models in natural language, Semantic Studio's built-in CoCo assistant (in private preview) works alongside a visual and YAML editor with Git integration, so semantic definitions can be version-controlled and reviewed like any other code asset. As data evolves, semantic models stay current so AI accuracy doesn't degrade over time. The result is less hallucination, higher accuracy and the business logic grounded in the actual data. Similarly, one can leverage[Google Cloud](https://www.linkedin.com/company/google-cloud/) 's Universal Semantic Layer, leveraging Looker's new support for[in-database analytics models](https://docs.cloud.google.com/looker/docs/analytic-models) , which include[new integration with BigQuery Graph and Snowflake Semantic Views](https://www.youtube.com/watch?v=ifMWVn8R9Sw) , delivering the power of Graph models to business users in key agentic journeys.


**Contextual intelligence**
Context is the surrounding knowledge that tells an AI system not just what the data is, but how to interpret it correctly.[Snowflake Horizon Context](http://snowflake.com/context) creates and applies this contextual layer: table and column descriptions in business terms, data quality signals (freshness, completeness, caveats), usage patterns (which tables are authoritative for which questions) and domain relationships across the lakehouse. Because this context lives in Horizon, it inherits the same governance as the data itself, so agents and users only ever receive context and answers they are authorized to see. Similarly, Google Cloud Lakehouse provides trusted context through[Knowledge Catalog](https://cloud.google.com/products/knowledge-catalog?e=a) , delivering Gemini-powered insights for lakehouse tables that help mitigate hallucinations in AI agents and enable users to turn insights into action. This context applies to any Iceberg table, whether Snowflake-native, CLD-linked or federated from Google Cloud. The AI receives data along with the context needed to interpret it correctly, which is what separates a generic LLM answer ("revenue might be...") from a grounded response ("Q4 revenue was $47.2M from the authoritative finance.revenue table, filtered by fiscal quarter definition").


## Agentic AI: From data to action


With enterprise context and semantic views in place, AI agents operate across structured and unstructured data with high accuracy.[Snowflake CoWork](https://www.snowflake.com/en/product/snowflake-cowork/) is a personal work agent for every knowledge worker — one that reasons deeply, automates routine tasks and accelerates the path from ideas to decisions to action. Rather than waiting for instructions, CoWork works proactively, surfacing insights, running analysis in the background and delivering governed, traceable answers grounded in how your business actually defines its data.


[Snowflake CoCo](https://www.snowflake.com/en/product/snowflake-coco/) extends this into the development workflow, serving as a data-native coding agent across enterprise data lifecycle work: build and maintain pipelines, create Cortex Agents and Streamlit apps, and generate validated SQL and Python grounded in actual catalog metadata. Both knowledge workers and data engineers benefit from the same semantic grounding and operate with governance and observability across human and agent actions.


That same federated foundation also makes Google Cloud's AI capabilities available natively. Because the lakehouse runs on Google Cloud, Gemini is natively available across the stack. Gemini's multimodal understanding (text, images, video, code in a single context) means lakehouse data isn't limited to structured tables — documents in GCS, images and unstructured content all participate in the AI workflow. Its long context windows ingest extensive schemas, semantic model definitions and query histories simultaneously, improving accuracy on complex analytical questions.


Gemini Enterprise connects to the lakehouse through MCP (via Snowflake's MCP servers or BigQuery MCP server) with Lakehouse for Apache Iceberg. Employees using Gemini Enterprise ask natural-language questions answered with governed data from Iceberg tables and their queries route through Cortex Agents and semantic models to ensure grounded responses. Whether a user interacts through CoWork, Cortex Code, Gemini Enterprise or a custom REST agent, the same semantic layer, the same context and the same governance apply.


The result is not just an open lakehouse — it is an AI-ready open lakehouse where data is interoperable, governed, semantically rich and accessible to both humans and AI agents through standardized protocols.


## Continue learning


- [Discover the latest product enhancements for building an interoperable lakehouse in Snowflake](https://www.snowflake.com/en/blog/interoperable-lakehouse-architecture/)
- [Cross-Cloud Lakehouse with Snowflake and Google Cloud](https://docs.cloud.google.com/lakehouse/docs/about-cross-cloud-lakehouse)
- [Explore more from Snowflake on Google Cloud](https://www.snowflake.com/en/blog/interoperable-lakehouse-architecture/)


*Note: Apache Iceberg, Apache Polaris and Apache Spark are trademarks of the Apache Software Foundation.*
