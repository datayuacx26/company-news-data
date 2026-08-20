---
schema_version: "1.0.0"
document_id: "2f0c69a81d8796a463ae0afdb4ceed68be672395bc5c1a1bfbb935da4c482c68"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-safely-connect-a-bi-tool-to-your-production-database/"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:085b833dcfd48c31eb9d6eac3bfd03e3fc0d22ef9d5ee9f830295f7fb8cb71a8"
---

# How to safely connect a BI tool to your production database

You can safely connect a BI tool to your production database if you do four things: create a dedicated read-only role, set a statement timeout so no analytics query can run forever, route the connection over TLS through an allowlist instead of the public internet, and move to a read replica once dashboards start competing with product traffic. Done that way, querying production for analytics is a normal, low-risk setup that most early-stage companies run for years.


This guide is for founders, engineers, and early data hires who want to point a dashboard or BI tool at their PostgreSQL or MySQL production database without slowing down the app, leaking customer data, or handing a reporting tool credentials that can write to live tables. It covers the exact roles, timeouts, and connection settings to use, with SQL you can copy, plus the point at which you should stop querying production and add a[data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) .


## Is it safe to connect a BI tool directly to production?


Yes, with controls. The risks people worry about are real but each has a standard mitigation:


- **A heavy query slows down the product.** Mitigate with statement timeouts, then a read replica.
- **The tool can write to or delete data.** Mitigate with a read-only role that has no` INSERT` ,` UPDATE` ,` DELETE` , or DDL privileges.
- **Someone exports sensitive columns.** Mitigate by restricting the role to specific schemas, masking PII, or using a dedicated reporting view layer.
- **Credentials leak.** Mitigate with TLS, IP allowlisting, rotated secrets, and a connection that is never exposed to the open internet.


None of these require a warehouse. They require a properly scoped connection. The failure mode is not “connected a BI tool to production.” It is “connected a BI tool to production using the same superuser credentials the app uses, with no timeout, over a publicly reachable port.”


## Step 1: Create a dedicated read-only role


Never reuse the application’s database user. Create a separate login whose only job is analytics, so you can audit it, rate-limit it, and revoke it independently.


In PostgreSQL:


```text
-- create a login role with no inherited superpowers
CREATE   ROLE   bi_readonly   LOGIN   PASSWORD   'use-a-secret-manager'  ;


-- allow it to reach the database and schema
GRANT   CONNECT   ON   DATABASE   app_production   TO   bi_readonly;
GRANT   USAGE   ON   SCHEMA   public   TO   bi_readonly;


-- read-only on everything that exists now
GRANT   SELECT   ON   ALL TABLES   IN   SCHEMA   public   TO   bi_readonly;


-- and on anything created later
ALTER   DEFAULT   PRIVILEGES   IN   SCHEMA   public
GRANT   SELECT   ON   TABLES   TO   bi_readonly;
```


In MySQL:


```text
CREATE   USER   '  bi_readonly  '@  '%'   IDENTIFIED   BY   'use-a-secret-manager'  ;
GRANT   SELECT   ON   app_production.  *   TO   'bi_readonly'  @  '%'  ;
FLUSH PRIVILEGES;
```


` SELECT` is the only privilege this role needs. Withholding everything else means a misbehaving tool, a bad natural-language-to-SQL translation, or a curious user cannot mutate data even if they try. PostgreSQL’s privilege system is documented in the official[GRANT reference](https://www.postgresql.org/docs/current/sql-grant.html) , and` ALTER DEFAULT PRIVILEGES` is what keeps new tables readable without re-granting every migration.


## Step 2: Limit what the role can see


A read-only role can still read every column, including password hashes, tokens, and PII you would rather not surface in a dashboard. Tighten the surface in one of three ways, from simplest to most controlled:


1. **Restrict to specific schemas or tables.** Grant` SELECT` only on the tables analytics actually needs instead of the whole schema.
2. **Expose a reporting view layer.** Create views that select only safe columns (and pre-join common tables), then grant the BI role access to the views rather than the base tables. This doubles as a place to centralize business logic.
3. **Mask or omit sensitive columns.** For regulated data, keep PII out of the analytics path entirely or mask it in the view. If you need per-customer scoping, apply[row-level security](https://www.basedash.com/blog/best-bi-tools-for-row-level-security-compared-2026) so each viewer only sees their own rows.


A reporting view layer is the highest-leverage option for most teams. It gives you a stable, documented interface that does not break when the application schema changes, and it is where a clear[BI permission model](https://www.basedash.com/blog/how-to-design-a-bi-permission-model-for-a-saas-team) starts to pay off.


## Step 3: Set a statement timeout


This is the single most important protection against analytics taking down the product. A timeout caps how long any query from the BI role can run, so a careless` JOIN` across millions of rows fails fast instead of saturating CPU and blocking product writes.


In PostgreSQL, set it on the role so it applies to every session that user opens:


```text
ALTER   ROLE   bi_readonly   SET   statement_timeout   =   '30s'  ;
```


You can also set` idle_in_transaction_session_timeout` to clean up connections a tool forgets to close. The` statement_timeout` parameter is part of[PostgreSQL client connection defaults](https://www.postgresql.org/docs/current/runtime-config-client.html) .


In MySQL, the equivalent only applies to read queries, which is exactly what you want here.` max_execution_time` is set in milliseconds:


```text
SET   GLOBAL   max_execution_time   =   30000  ;   -- 30 seconds, SELECT statements only
```


Thirty seconds is a reasonable starting point for interactive dashboards. If you have queries that legitimately need longer, that is a strong signal the workload belongs on a replica or a warehouse, not that the timeout should be raised.


## Step 4: Move to a read replica when queries compete with production


A timeout stops a single runaway query. It does not solve sustained analytical load. Once dashboards refresh on a schedule, more than a handful of people run ad hoc queries, or you see analytics queries showing up in your slow-query logs during peak hours, point the BI tool at a read replica instead of the primary.


A replica gives you a near-real-time copy that absorbs read load without touching the database that serves customers. Managed databases make this a configuration change rather than a project: Amazon RDS and Aurora, Google Cloud SQL, and most Postgres and MySQL hosts let you provision a replica and hand its read-only endpoint to your BI tool. Replication lag is usually well under a second, which is fine for nearly all reporting.


The decision rule:


- **Primary is fine** when analytics is a few internal users and scheduled refreshes a few times a day.
- **Add a replica** when reporting load is steady, concurrency climbs, or analytics queries start appearing in slow-query logs.
- **Add a warehouse** when you need to combine production data with Stripe, HubSpot, or Segment, or when transformations and historical snapshots become routine. We cover that threshold in detail in[when to add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) .


## Step 5: Secure the connection itself


A scoped role is useless if the credentials travel in the clear or the database port is open to the internet.


- **Require TLS.** Force encrypted connections so credentials and query results are not sniffable in transit. Most managed databases support` sslmode=require` or stricter.
- **Allowlist, do not expose.** Restrict inbound access to the BI tool’s known IP ranges, or connect through a private network, VPC peering, or an SSH tunnel. A production database should never be reachable on` 0.0.0.0` .
- **Store secrets in a secret manager.** Keep the BI role’s password out of config files and shared docs. Rotate it on a schedule and immediately if someone with access leaves.
- **Prefer a managed connection.** Reputable BI tools connect through a fixed set of IPs you can allowlist and store credentials encrypted. Self-hosting the tool inside your own network avoids exposing the database at all.


## Step 6: Add timeouts, pooling, and an audit trail


Two operational details prevent the slow degradation that shows up months later:


- **Connection pooling.** BI tools can open many short-lived connections. A pooler like PgBouncer (or your managed database’s built-in pooling) keeps the primary from running out of connection slots.
- **Query logging.** Log who ran what and when. An audit trail is how you debug a sudden load spike, prove compliance, and find the dashboard that is hammering a table on a five-minute cron. If you cannot answer “which query slowed us down at 9am,” you are flying blind.


For more on keeping dashboards responsive once they are live, see[how to make slow BI dashboards fast](https://www.basedash.com/blog/how-to-make-slow-bi-dashboards-fast-a-performance-playbook) .


## The production-database connection checklist


Use this before you hand a BI tool a connection string:


- Dedicated` bi_readonly` role, separate from the application user
- ` SELECT` only, no write or DDL privileges
- Access limited to needed schemas, or a reporting view layer
- Sensitive columns masked, omitted, or row-level-scoped
- ` statement_timeout` /` max_execution_time` set on the role
- TLS required on the connection
- Database port allowlisted or private, never publicly open
- Credentials in a secret manager, rotation scheduled
- Connection pooling in place
- Query logging or an audit trail enabled
- A plan to move to a replica when load grows


If every box is checked, connecting a BI tool to production is a sound default, not a risk to apologize for.


## When not to query production directly


Skip the production connection and start with a replica or warehouse if any of these are true from day one:


- Your largest tables are already in the hundreds of millions or billions of rows.
- Reporting needs to join production data with billing, marketing, or product-analytics data.
- You have strict compliance requirements that forbid analytics access to the production environment.
- Dozens of business users will run unpredictable ad hoc queries.


In those cases the cost of a separate analytics store is justified upfront. For most companies between zero and roughly $10M in revenue, it is not.


## Common mistakes


- **Reusing the app’s database user.** You lose the ability to audit, rate-limit, or revoke analytics access independently, and the tool inherits write privileges it should never have.
- **No statement timeout.** One bad query can block product writes. This is the most common way analytics takes down an app.
- **Granting on` public` once and forgetting` ALTER DEFAULT PRIVILEGES` .** New tables become invisible to the BI role after the next migration, and someone “fixes” it by granting superuser.
- **Exposing the database port to allowlist later.** Later never comes. Lock it down first.
- **Raising the timeout instead of moving the workload.** A query that needs more than 30 seconds belongs on a replica or warehouse.


## How Basedash fits


Basedash connects directly to a PostgreSQL or MySQL database (production, a read replica, or a warehouse) using read-only credentials, so the patterns above apply cleanly. It connects over TLS from a fixed set of IPs you can allowlist, runs queries through a scoped role, and logs activity for auditing. Non-technical teammates can ask questions in natural language and Basedash writes the SQL against the connection you configured, which means the read-only role and statement timeout you set are doing real work in the background. For teams that start with a single application database, that is enough to run analytics for a long time before a warehouse is worth the operational overhead. You can read more about the[AI-native BI approach](https://www.basedash.com/) on the homepage.


## FAQ


**Do I need a data warehouse to use a BI tool?** No. A BI tool can query a production database or a read replica directly. A warehouse becomes worthwhile when you need to combine multiple data sources, run heavy transformations, or support many concurrent business users.


**How do I create a read-only user for a BI tool in PostgreSQL?** Create a` LOGIN` role, grant` CONNECT` on the database and` USAGE` on the schema, grant` SELECT ON ALL TABLES` , and add` ALTER DEFAULT PRIVILEGES ... GRANT SELECT` so future tables stay readable. Do not grant any write or DDL privileges.


**Will connecting a BI tool slow down my database?** It can if a query is expensive or many run at once. A statement timeout prevents single runaway queries, and a read replica offloads sustained reporting load from the database that serves your product.


**Should I use a read replica or query the primary?** Query the primary while analytics is a few internal users and scheduled refreshes. Move to a replica once reporting load is steady, concurrency rises, or analytics queries appear in your slow-query logs.


**How do I stop a BI tool from seeing sensitive columns?** Grant access to a reporting view that selects only safe columns, restrict the role to specific tables, or mask PII. For per-customer data, apply row-level security so each viewer sees only their own rows.


**Is it safe to expose my production database to a cloud BI tool?** Only over TLS with the database port allowlisted to the tool’s known IPs or reachable through a private network. Never leave the port open to the public internet, and store credentials in a secret manager.
