---
schema_version: "1.0.0"
document_id: "5ae561afc6e250d9db3491e5716b9d2dcaf8b2aa9ec61b8e6dfe0cafc6c39106"
company_key: "yc-mindsdb"
company: "MindsDB"
source_id: "yc-mindsdb-news-import-209cdb193474"
canonical_url: "https://mindshub.ai/blog/mindsdb-product-updates-february-2026"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-22T04:26:30.664731+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:21cc529d72b2739c2231da3e93fb2d82c59e5c8882e0674173b55db16fd80124"
---

# MindsDB Product Updates - February 2026

February brings a focused set of improvements across performance, stability, security, and usability in MindsDB. This product update explores strengthening the foundation of Knowledge Bases, expands and refines integrations, improves SQL reliability, enhances GUI performance, and continues our commitment to security and maintainability.


MindsDB has officially released[v26.0.0](https://github.com/mindsdb/mindsdb/releases/tag/v26.0.0) , marking a major platform upgrade that brings broad improvements to SQL robustness, integrations, knowledge base behavior, bug fixes, documentation clarity, and security hardening - all designed to boost reliability, performance, and user confidence in production workloads. The release of[v26.0.0](https://github.com/mindsdb/mindsdb/releases/tag/v26.0.0) delivers more predictable query behavior, smoother developer workflows, expanded cloud reach, and a cleaner, more secure codebase, making it an exciting step forward for teams building real-world AI and analytics solutions


## **Knowledge Bases**


Docker Compose deployments now default to[PGVector](https://docs.mindsdb.com/integrations/vector-db-integrations/pgvector) as the[Knowledge Base](https://docs.mindsdb.com/mindsdb_sql/knowledge_bases/overview) store, providing a more consistent and production-ready vector backend out of the box, with improved reliability for semantic search workloads. We also fixed an issue where mixed-case column names did not display correctly, ensuring schemas are reflected exactly as defined and reducing confusion when working with structured metadata. A bug that could cause duplicate[Knowledge Base](https://docs.mindsdb.com/mindsdb_sql/knowledge_bases/overview) IDs on a second insert has been resolved, along with related[PGVector](https://docs.mindsdb.com/integrations/vector-db-integrations/pgvector) handling improvements, increasing data integrity and preventing unexpected conflicts.


Finally, inserts into[Knowledge Bases](https://docs.mindsdb.com/mindsdb_sql/knowledge_bases/overview) are now batched by default, significantly improving ingestion performance and making it faster and more efficient to load large datasets. Together, these updates make[Knowledge Bases](https://docs.mindsdb.com/mindsdb_sql/knowledge_bases/overview) more robust, predictable, and scalable for real-world AI applications.


## **Integrations**


Integration READMEs are now pulled statically from GitHub instead of being locally served, ensuring documentation stays up to date with the latest changes and reducing inconsistencies between releases. We enhanced the[Shopify](https://docs.mindsdb.com/integrations/app-integrations/shopify#shopify) ,[Confluence](https://docs.mindsdb.com/integrations/app-integrations/confluence#confluence) ,[Databricks](https://docs.mindsdb.com/integrations/data-integrations/databricks#databricks) ,[HubSpot](https://github.com/mindsdb/mindsdb/tree/main/mindsdb/integrations/handlers/hubspot_handler) , and NetSuite handlers to improve reliability and interaction quality, resulting in smoother connections and more predictable behavior across these systems. The[Shopify](https://docs.mindsdb.com/integrations/app-integrations/shopify#shopify) handler now includes additional validation for target lists, helping prevent configuration errors before they cause issues downstream.


To streamline the codebase and reduce maintenance overhead, we deprecated Dspy, Chromedb, and legacy ML handlers, making MindsDB lighter, cleaner, and easier to evolve moving forward. We also bound the extra` numba` dependency version to strengthen security and reduce potential compatibility risks. Documentation for Integrations received visual improvements as well, with images added to README files to make integration steps clearer and easier to follow.


We enabled NetSuite Cloud support, expanding MindsDB’s availability across more cloud environments. This allows users to access MindsDB capabilities directly within NetSuite Cloud while giving organizations greater flexibility to operate on the platforms they are most comfortable with. Together, these updates improve stability, clarity, security, and platform reach across the ecosystem.


## **Bug Fixes and Improvements**


Issues related to README locations and language permission handling has been resolved to ensure documentation is accessible and correctly structured across environments. Improvements to the[Shopify](https://docs.mindsdb.com/integrations/app-integrations/shopify#shopify) handler query limit help prevent unexpected failures when working with larger datasets, while a patched file upload vulnerability enhances overall platform security.


We also addressed reported bugs affecting file datasource visibility after Docker upgrades and resolved a query charting failure that could interrupt analytics workflows. In addition, Postgres queries used for[Data Catalog](https://docs.mindsdb.com/data_catalog/overview#data-catalog) retrieval have been optimized for better efficiency. A specific issue involving aggregates from subselect statements has been fixed, significantly improving performance for more complex queries that rely on nested data selection. Together, these updates deliver stronger security, smoother upgrades, and faster, more reliable query execution for production workloads.


## **SQL Operations**


We’ve delivered several stability and usability improvements across SQL handling and integrations. GROUP BY WITH ROLLUP has been fixed to make aggregations more robust and reliable, especially for advanced reporting scenarios. We added validation for the` MINDSDB_DB_CON` environment variable to prevent configuration errors and ensure smoother deployments. The` track_column` parameter is now case-insensitive, reducing friction and eliminating avoidable mistakes when defining workflows.


SQL parsing has been enhanced with improved pruning of ambiguous columns and more accurate extraction of query targets in the API handler, leading to cleaner execution and fewer edge-case failures. We also improved memory handling when using[DuckDB](https://docs.mindsdb.com/integrations/data-integrations/duckdb#duckdb) to better manage resource usage under heavier workloads, and updated the available memory checks in the[Snowflake](https://docs.mindsdb.com/integrations/data-integrations/snowflake#snowflake) handler to ensure safer, more predictable query execution in cloud environments. Together, these updates improve reliability, reduce configuration errors, and strengthen performance across complex analytical workloads.


##


## **Documentation**


MindsDB has removed unnecessary and unused files from the codebase as part of an ongoing effort to keep MindsDB clean, maintainable, and easier to navigate. While this may not be directly visible in day-to-day usage, it reduces technical debt, improves overall code clarity, and makes future development faster and more reliable. For users, this means a more stable platform, quicker iteration on new features, and a healthier foundation for long-term innovation.


## **Security Upgrades**


MindsDB implemented multiple security upgrades across a wide range of core dependencies, including packages such as urllib3, Starlette, Keras, Protobuf, NumPy, aiohttp, Fugue, sqlparse, filelock, azure-core, pyasn1, Mintlify, sentencepiece, marshmallow, and others. These updates address known vulnerabilities, improve compatibility, and strengthen overall system resilience. For users, this means a more secure and stable platform, reduced exposure to third-party risks, and greater confidence when running MindsDB in production environments.


## **GUI**


In this GUI release, we focused heavily on performance, usability, and stability to create a smoother day-to-day experience. One of the biggest changes is **unlimited tabs** in the interface. You can now open as many tabs as needed without hitting a cap, supported by improvements to IndexedDB and table data handling to ensure performance remains fast and responsive even with heavier workloads. We also refined the Respond Chat experience, improving the “thoughts” section to make interactions clearer and more intuitive.


Performance was a major priority across the board. Based on Lighthouse audits, we implemented optimizations that make the interface load faster and feel more responsive. We eliminated redundant network requests when switching or updating tabs, reducing unnecessary load and improving overall efficiency. To prevent confusion during interruptions, we added a helpful popup that appears if the connection to the MindsDB server is lost, clearly explaining what happened and guiding users on next steps.


We expanded functionality as well. The GUI now supports **Graph Queries** , giving users new ways to explore and visualize complex data relationships directly in the interface. In the integrations panel, we introduced clearer labeling by adding a` support_level` indicator, so users can easily see which integrations are officially supported by the MindsDB team and which are community-built. This provides better transparency around feature maturity and production readiness.


A number of important bugs were resolved to improve reliability. We fixed issues that prevented adding datasources in development environments, uploading files through the GUI, and adding Amazon Bedrock as an LLM during onboarding or in model management settings. We resolved duplicate “Visualize” buttons, popup errors when canceling SQL queries, confirm button failures in the tab limit window, and visual inconsistencies related to Amazon Bedrock. Compatibility has also been aligned with Backend 26.0.0 to ensure seamless operation across versions.


Under the hood, we strengthened automated testing across key areas including the Respond (Chat) tab, SQL Editor (Unify tab), Settings pages, Integrations management, Models configuration, and the “Report a Bug” feature. We also fixed an issue where the GUI was sending requests on every key press, reducing unnecessary API calls and improving responsiveness. Additional datasource form fixes now allow Hacker News and Reddit to be added successfully through the GUI.


The majority of the security updates involved upgrading frontend dependencies to their latest stable or secure versions. These updates address known security alerts, improve compatibility, and ensure the interface remains protected against emerging vulnerabilities. By keeping core packages up to date, we strengthen the overall security posture of the GUI while maintaining stability and performance for users.


Together, these enhancements make the interface faster, clearer, more powerful, and more reliable for building and managing AI-driven workflows.


## **Conclusion**


Across Knowledge Bases, integrations, SQL operations, documentation, security, and the GUI, this release is about strengthening the core. We’ve improved ingestion performance, enhanced reliability in complex queries, increased transparency in integrations, tightened security across dependencies, and made the interface smoother and more responsive.


As always, these updates are part of our ongoing effort to make MindsDB a stable, scalable, and enterprise-ready platform for building AI-powered analytics and data workflows. Thank you to our community and users for the feedback that continues to help shape each release. Check out the full[Release Notes here.](https://github.com/mindsdb/mindsdb/releases)
