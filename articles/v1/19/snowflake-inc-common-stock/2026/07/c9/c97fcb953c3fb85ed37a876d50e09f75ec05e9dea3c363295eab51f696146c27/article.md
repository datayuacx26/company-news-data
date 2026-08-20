---
schema_version: "1.0.0"
document_id: "c97fcb953c3fb85ed37a876d50e09f75ec05e9dea3c363295eab51f696146c27"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/bidirectional-interoperability-snowflake-horizon-databricks"
published_at: "2026-07-15T19:38:00+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:288e0ef1a9eb93ed04ee0e5ff999b54fddab17b584ec5a13347eaade8696145c"
---

# The Open Interoperability Standard: Why Bidirectional Iceberg REST Matters

# Open Interoperability: Delivering Customer Choice


While intuitively apparent to the readers of this blog, let's define open interoperability first. Open interoperability is the ability to architect your organizations' data platform to securely access governed data through **open, published, standards-based interfaces** , **reducing proprietary lock-in** and enabling broader ecosystem compatibility.


Open interoperability changes the relationship organizations have with their data. It provides them with agency over their data, the ability to freely choose the best engine and tool for each workload without onerous migrations or data movement. This definition is core to how Snowflake delivers first-class experiences for Apache Iceberg™ and Apache Polaris™. The result allows customers to platform their Iceberg tables on Snowflake or elsewhere, while being able to fully read and write to these tables without forcing migration to our catalog.


Databricks Unity Catalog also claims to be interoperable, yet upon close examination, we see that it fails to meet the most basic promises of open interoperability: the ability to read or write to any Iceberg table and to do so using open security mechanisms that avoid lock-in to any one tool.


## Read and write to any Iceberg table


Choosing a catalog for your tables is the most critical decision in your architecture. It determines what operations you can do on these or other tables, expanding or limiting your choice. This is where true interoperability comes in. True interoperability requires both inbound and outbound federation via open community driven standards. Inbound federation means engines external to your catalog can read and write to your Iceberg tables, and outbound federation means that you can read or write to Iceberg tables managed by a different catalog.


### Unity Catalog fails the interoperability test


As of this writing, Unity Catalog only supports full interoperability in one direction, inbound. For inbound access, external engines are able to connect to Unity Catalog's Iceberg REST Catalog end points to get access to the table. For outbound access, the picture is fundamentally different. When accessing Iceberg tables from external catalogs like Snowflake Horizon, Databricks does not use the Iceberg REST Catalog APIs. Instead, it uses the Snowflake Catalog SDK which uses JDBC to fetch the metadata.json path, and then accesses the underlying storage directly using preconfigured IAM credentials ([docs](https://docs.databricks.com/aws/en/query-federation/snowflake-catalog-federation) ). What's more, this only works for read operations. Iceberg tables managed by external catalogs remain explicitly read-only; writes to external Iceberg tables are not supported ([docs](https://docs.databricks.com/aws/en/iceberg/) ).


The impact to your ability to unlock agency over your data is undeniable. When you platform your workloads with Databricks and Unity Catalog, you are effectively required to migrate your entire data estate to Unity Catalog, undermining the basic promise of interoperability and their own claim of Unity Catalog as the most "interoperable catalog on the market."


Snowflake's Horizon Catalog is fundamentally different. Built on the foundation of Apache Polaris, Snowflake Horizon Catalog provides an end-to-end implementation of the Iceberg REST Catalog protocol. Horizon is Snowflake's native governance layer where classification, access policies, lineage, quality and catalog management are applied consistently across Iceberg tables Snowflake manages or accesses without limiting read or write operations. What's more, data teams acting on data from Snowflake get a comprehensive picture of where sensitive data lives across their full estate, not a partial view bounded by platform or table type. This works because Horizon Catalog connects to external catalog like Unity Catalog via a feature called Catalog Linked Databases and directly calls the catalog's Iceberg REST Catalog endpoint to get access from the catalog for that specific query. There's no need to file a ticket with your platform team to try to negotiate for a new IAM role, a long-lived credential or to manage a new security mechanism.


## Why open security matters to interoperability


The Iceberg REST Catalog defines how catalogs should securely talk to each other. The feature is called vended credentials. Vended credentials are short-lived tokens delegated by the source catalog for specific operations. It provides a mechanism for your catalog to be the source of truth for your tables and security posture. However, while it is defined in the Iceberg REST Catalog spec, there's no requirement that catalogs implement it. The alternative to vended credentials is broad based access via IAM roles and long-lived credentials like access keys. These approaches add complexity and potential risk to your security posture. First, they aren't scoped for the specific query or, in practice, a specific table. Second, policies must be duplicated in your source catalog *and* your IAM layer, doing away with the foundation for secure open interoperability as recommended in the Iceberg REST spec.


Snowflake Horizon Catalog fully implements vended credentials for both inbound and outbound federation, providing you a single pane of glass to manage your Iceberg tables and access policies for any engine that accesses your Snowflake-managed Iceberg tables. In practice, this means you can define access policies in Horizon Catalog and have them enforced in your preferred engine without additional work. While this has worked at the object level (catalog, namespaces and tables), the newly introduced Scan Plan API, in private preview, extends governance to fine-grained row and column level access controls for compatible engines. As of this writing, Unity Catalog does not respect your source catalog's access policies because it does not consume credentials vended by external catalogs.


### Full breakdown: Most interoperable catalog? You decide


Capability Snowflake Horizon Databricks UC What the gap means for you


Iceberg REST Catalog credential vending implementation 🟢 Bidirectional: per-query vended credentials:[Inbound](https://docs.snowflake.com/en/user-guide/tables-iceberg-access-using-external-query-engine-snowflake-horizon) and[outbound federation](https://docs.snowflake.com/en/user-guide/tables-iceberg-configure-catalog-integration-vended-credentials) 🟡 Inbound only. Outbound connections do not use IRC.1 Unity Catalog introduces unnecessary security risks by requiring long-lived IAM credentials and may increase cost per query by routing queries via JDBC connections for tables not managed by Unity Catalog.


Read + write to external catalog tables 🟢[Full DML on external catalog tables via Iceberg REST Catalog](https://docs.snowflake.com/en/user-guide/tables-iceberg-externally-managed-writes) 🟡 Lakehouse Federation is read-only. No INSERT, UPDATE, DELETE or MERGE on external catalog tables.2 Unity Catalog requires users to either migrate all tables or limit operations to read only, which may delay time to value and reduce the efficacy of your AI workflows.


Enforcing ABAC when querying across Snowflake and non-Snowflake (external) engines 🟢 Row and column masking and row access policies[GA for Spark;](https://docs.snowflake.com/en/user-guide/tables-iceberg-query-using-external-query-engine-snowflake-horizon-enforce-access-policies) expanding to any Iceberg REST-compatible engines via Scan Plan API (Public Preview soon, reach out to your account team for additional details). 🟡 Beta for external Iceberg engines. Not production-ready.3 Security policies are GA on Snowflake but in preview on Databricks.


Replication and disaster recovery 🟢[Full support for Snowflake-managed tables.](https://docs.snowflake.com/en/user-guide/tables-iceberg-replication) 🟡 Tables with row filters or column masks are excluded from replication and hold up RPO. Managed DR is gated, Preview, and a paid add-on.4 Unity Catalog forces you to remove access policies from the tables to be eligible for replication and disaster recovery.


## You can still use Databricks but without Unity Catalog


While the limitations described apply to Unity Catalog, Databricks compute can still achieve full read/write capabilities with external catalogs that support Iceberg REST (IRC) APIs, such as Snowflake Horizon. Customers can achieve this by configuring a Databricks Spark cluster without Unity Catalog enabled. In this configuration, the cluster maintains full read/write access to any IRC-compliant catalog and each query respects access policies. However, enabling Unity Catalog on the cluster restricts this functionality, confirming that this is a specific limitation of Unity Catalog rather than the broader Databricks platform. Using Spark session variables to connect to Iceberg is supported by open source Spark. Please note if you utilize this method you are operating outside the Unity Catalog integration model.


## The path to true data agency


True data agency is built on freedom of choice; the freedom to choose the best engines and tools for your unique workloads without the tax of forced migrations or siloed security models. Interoperability is more than a technical specification; it is a commitment to a customer-first architecture.


When selecting a catalog, the difference lies in the implementation of open standards. A catalog that restricts interoperability to inbound-only connectivity forces you to choose between your preferred compute engine and your desired governance model. Conversely, an end-to-end implementation of the Iceberg REST Catalog protocol — like the one we have built into Snowflake Horizon — enables bidirectional access that respects your security posture regardless of the engine you choose.


For data leaders, the goal is clear: Ensure your architectural decisions expand your future options rather than narrowing them. We encourage you to prioritize catalogs that treat open security and bidirectional interoperability as first-class citizens. By adopting a truly open approach, you ensure that your data remains a strategic asset that you control, rather than a dependency locked into a single provider.


*This post references features that are in Public Preview and subject to change. Preview features may not be suitable for production use. See Snowflake's documentation for current feature availability.*


- [Inbound](https://docs.snowflake.com/en/user-guide/tables-iceberg-access-using-external-query-engine-snowflake-horizon) and[outbound federation](https://docs.snowflake.com/en/user-guide/tables-iceberg-configure-catalog-integration-vended-credentials) via vended credentials
- [Write support for externally managed tables](https://docs.snowflake.com/en/user-guide/tables-iceberg-externally-managed-writes)


*Apache Iceberg and Apache Polaris are trademarks of the Apache Software Foundation*


---


1. "Credential vending on Foreign Iceberg tables is not supported."[docs.databricks.com/aws/en/external-access/iceberg](https://docs.databricks.com/aws/en/external-access/iceberg)
2. "External federated Hive metastore and all foreign tables accessed through Lakehouse Federation are read-only."[docs.databricks.com/aws/en/tables/foreign](https://docs.databricks.com/aws/en/tables/foreign)
3. "With cross-engine attribute-based access controls (ABAC) now in Beta, Unity Catalog extends attribute-based access control to Iceberg clients using the Iceberg REST Catalog Scan APIs."[databricks.com/blog/unity-catalog-and-next-era-apache-icebergtm](https://www.databricks.com/blog/unity-catalog-and-next-era-apache-icebergtm)
4. "Tables with row filters or column masks and ABAC-tagged resources are flagged as failed to replicate in the system table, and these failures hold up RPO until you remove the resource from the failover group's scope."[docs.databricks.com/aws/en/admin/managed-disaster-recovery](https://docs.databricks.com/aws/en/admin/managed-disaster-recovery) (updated Jun 19, 2026)
