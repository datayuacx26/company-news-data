---
schema_version: "1.0.0"
document_id: "31347cb61884c523c1296c49acd1cae65eeb0d3f77c1643906c5958782749ee9"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/granular-query-permissions-secoda"
published_at: null
first_seen_at: "2026-07-22T12:57:57.585252+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:d27e9f6c70eb2a87c2deb10eb0c56c598aeccfdaeb3cc3f877cd4ccd676367c7"
---

# Introducing granular query permissions in Secoda

Controlling who can query data is a core part of any organization’s governance setup. As more organizations rely on Secoda’s querying tools across AI-generated SQL, query blocks, and previews, we consistently heard the same feedback: customers wanted a better way to control who can run queries and how.


This update introduces new granular permissions that give admins precise control over query access by role or group. Whether you're restricting Secoda AI to your data engineering team or limiting previews to just analysts, this change simplifies access management without disrupting existing workflows.


Streamlined querying settings per integration, like Snowflake


## What's new in query permissions?


Secoda now offers granular query permissions that let you control access for each query type individually, all in one place. These apply to the three query types in Secoda:


- [Query blocks](https://docs.secoda.co/features/queries/running-queries-in-secoda)
- [Running SQL](https://docs.secoda.co/features/ai-assistant#tools) through Secoda AI
- [Data previews](https://docs.secoda.co/features/data-previews)


Permissions are managed per integration. To explore or make changes to these settings:


#### Step 1: Go to Integrations


#### Step 2: Select your data source you’d like to manage


#### Step 3: Navigate to Preferences > Query permissions


You can assign access by role (admin, editor, viewer) or by group, giving you full control over who can query what. This makes it easier to align access with how your teams are organized, whether that’s by function, department, or project.


For example, you can give your data engineering group access to Secoda AI querying, while limiting query block access to analysts.


Need more flexibility? You can create and manage custom roles and groups in Settings > Administration > Members and permissions. Once created, they’ll appear as options when assigning query permissions within each integration.


This update replaces the old permissions model with a simpler, more flexible experience that gives you more control without adding more complexity.


## Why does this matter?


In the past, Secoda’s query permissions were limited and often unclear, making it difficult for organizations to apply the right level of control across teams.


Having granular, role- and group-based permissions matters because not every team needs the same level of access and to the same data sources. For example:


- Data engineers may need full access to Secoda AI and Postgres for writing and testing queries.
- Business analysts might only need access to query blocks in Snowflake to support reporting workflows.
- Product managers could benefit from data previews but shouldn’t have permission to run live queries.
- Contractors or external collaborators may require read-only access to specific sources, with no access to sensitive environments.


Without this level of control, some admins struggled with over-permissioning or under-permissioning users, which either created risk or slowed teams down. This update gives organizations the flexibility to meet security and compliance requirements while enabling users to move quickly with the tools they need.


## What happens to existing permissions?


To make this transition smooth, all existing query permissions have been automatically migrated to the new model:


- Any previous settings have been backfilled into equivalent roles or groups.
- User access remains unchanged unless you decide to update it.
- You’ll see new group names created based on your old configuration. These can be edited or removed at any time if you want to reorganize or simplify.


No setup is required to maintain your current access settings; everything continues to work as expected.


## Ready to get started?


This update is designed to give data teams the control they’ve been asking for: clear, flexible permissions that reflect how modern organizations actually work. By managing query access per integration and per query type, you can confidently support technical and non-technical users without over-permissioning or adding complexity.


Whether you're scaling your team, tightening governance, or streamlining your admin setup, these new permissions make it easier to align access with your structure and security needs.


If you have questions or want help setting up roles and groups, reach out to our Customer Success team at[\[email protected\]](https://www.secoda.co/cdn-cgi/l/email-protection#70130503041f1d1502300315131f14115e131f) . We’re here to help.


‍
