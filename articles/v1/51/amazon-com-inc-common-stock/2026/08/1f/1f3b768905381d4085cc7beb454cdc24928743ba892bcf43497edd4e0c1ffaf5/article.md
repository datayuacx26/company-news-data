---
schema_version: "1.0.0"
document_id: "1f3b768905381d4085cc7beb454cdc24928743ba892bcf43497edd4e0c1ffaf5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/7/aws-transform-windows-sql-schema-aurora"
published_at: "2026-08-03T15:00:00+00:00"
first_seen_at: "2026-08-03T18:55:51.607567+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:78a8142cba97c96e2bb38792462b881ac4deb1e93bb21c51adbc81d1a568c699"
---

# AWS Transform for full-stack Windows modernization now supports offline schema transformation to Aurora PostgreSQL

Today, AWS Transform for full-stack Windows modernization announced general availability of offline source transformation, enabling customers to modernize Microsoft SQL Server databases to Amazon Aurora PostgreSQL without requiring a live database connection. AWS Transform now converts SQL Server storage objects, powered by AWS DMS, and code objects (stored procedures) using an agentic, interactive experience. Enterprises modernizing legacy .NET applications and their dependent SQL Server databases can now start their modernization by directly uploading the Data Design Language (DDL) source files from their databases.


With offline source transformation, customers upload SQL Server data design language (DDL) files, assess database and stored procedure complexity, and generate a customizable transformation plan. AWS Transform converts tables, schemas and converts code objects such as stored procedures and functions, validates functional equivalence, and deploys the converted schema to Aurora PostgreSQL. The same workflow transforms database dependent .NET applications to be PostgreSQL compatible .NET applications with updated connection strings, ADO.NET and Entity Framework data-access calls. To address remaining conversion issues, customers can iterate directly in the web console or hand off to their preferred IDE using the AWS Transform MCP server. A separate synthetic data workflow populates Aurora PostgreSQL with test data for end-to-end application validation.


AWS Transform for full-stack Windows modernization and offline source transformation is available in US East (N. Virginia). To get started, you can go to[AWS Transform product page](https://aws.amazon.com/transform/windows/) or see[AWS Transform for full-stack Windows documentation](https://docs.aws.amazon.com/transform/latest/userguide/windows-full-stack.html) .
