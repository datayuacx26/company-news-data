---
schema_version: "1.0.0"
document_id: "b03818c890739bd11f55b90f4f54dbb969eacd323a3a7fe92fe1ebbd3bbcff22"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/multi-tenant-analytics-architecture-how-to-isolate-customer-data-in-embedded-dashboards/"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:6dbf1b83fc9464446eb69a0a38bc9421a18ebe35d6509b4effd52be55af39592"
---

# Multi-tenant analytics architecture: how to isolate customer data in embedded dashboards

Multi-tenant analytics is the architecture that lets one analytics layer serve dashboards to many customers from the same code path while guaranteeing that customer A never sees customer B’s data. Most SaaS teams get this wrong in one of three ways: they store everyone in one schema and rely on a hopeful` WHERE tenant_id = ?` in application code, they spin up a separate database per customer and watch operational cost balloon, or they trust the BI tool’s UI permissions and miss that the underlying SQL still has full access. The right answer is almost always a layered model where the warehouse, the query layer, and the embed token each enforce isolation independently.


This guide is for engineering leaders, platform engineers, and product owners building customer-facing analytics for a B2B SaaS product. It walks through the three isolation models, where to enforce tenant scoping, what a defensible signed-attribute pattern looks like, and the mistakes that quietly create cross-tenant leakage in production.


## TL;DR


- A multi-tenant analytics setup has to enforce tenant isolation in at least two independent layers. Authentication tokens alone are not enough; the query that hits the warehouse must also be scoped.
- There are three isolation models: silo (database or schema per tenant), pool (shared tables with a` tenant_id` column), and bridge (shared schema with row-level security). Most SaaS teams should start with pool plus row-level security and only move to silo for regulated or very large customers.
- The single most reliable pattern is a signed JWT that carries the tenant ID and user attributes, a query layer that injects those attributes as bind parameters, and warehouse-side row-level security that enforces the same predicate even if the query layer is bypassed.
- Embedded analytics platforms differ mostly in how they handle this token-to-predicate handoff.[Basedash](https://www.basedash.com/) , Explo, Embeddable, Cube, and Metabase all support signed embed tokens with user attributes; the implementation details (how attributes map to SQL, whether they can override metric definitions, where caching keys are scoped) decide whether the result is actually safe.
- The most common production failures are cache keys that ignore tenant ID, joins that reach across tenants through dimension tables, and AI-generated SQL that includes tenants in` WHERE` clauses by convention rather than by enforcement.


## What multi-tenancy means in analytics


In application code, multi-tenancy usually means one set of services that serve many customers, with tenant context passed through every request. Analytics adds two complications.


First, the queries are arbitrary. A typical SaaS API surface is a finite set of endpoints, each carefully scoped to a tenant. A dashboard or AI agent can generate any SQL the underlying schema allows. The bigger the surface area of queries, the more places isolation can fail.


Second, analytics workloads frequently pre-aggregate and cache. A cached chart that does not include` tenant_id` in its cache key is a cross-tenant data leak waiting for the right combination of users to hit it.


For a SaaS product embedding dashboards, multi-tenant analytics has to satisfy three properties:


1. **Isolation.** Customer A’s queries cannot return customer B’s rows under any circumstance, including caching, joins through shared dimension tables, and ad hoc SQL or natural-language questions.
2. **Performance parity.** A small customer’s queries should not be visibly slower because a large customer is running heavy reports against the same warehouse.
3. **Operational simplicity.** Onboarding a new customer should not require a new database, a new dashboard, or a redeploy.


The three isolation models below trade these properties off in different ways.


## The three isolation models


### Silo: separate database or schema per tenant


Every customer gets a dedicated database (or, more commonly, a dedicated schema inside a shared database). Queries against tenant A’s data never touch tenant B’s tables because they live in different namespaces.


Silo is the default in regulated industries and for very large enterprise customers who explicitly want their data physically separate. It is the easiest model to explain to a security review, and it makes per-customer backups, key rotation, and deletion straightforward.


The cost is operational. Schema migrations have to fan out across hundreds or thousands of schemas. Adding a column means a script, a runbook, and a maintenance window. Cross-tenant analytics (anything you would build for your own internal product team) becomes a union over every schema. Connection pooling gets harder because warm connections are scoped to a database or a search path.


A typical silo setup pairs Postgres or Snowflake schemas with a routing layer that picks the right schema based on the authenticated tenant. Snowflake makes this slightly more pleasant because schema-level access controls and per-warehouse compute let you isolate both data and resources.


### Pool: shared schema with a` tenant_id` column


Every table has a` tenant_id` (or` account_id` ,` workspace_id` ,` customer_id` ) column, and every query filters on it. One set of tables holds data for every customer.


Pool is the right starting point for most SaaS products. Schema changes are a single migration. Cross-customer reporting is a normal query. Onboarding a new customer is an` INSERT` into a` tenants` table, not a deploy.


The risk is that isolation now lives in` WHERE` clauses. If any query forgets the predicate, runs a` JOIN` that loses the predicate through a dimension table, or relies on a dashboard filter to exclude other tenants, the data leaks. This is why pool models almost always need to be paired with database-level row-level security so the predicate is enforced even if the application code is wrong.


### Bridge: shared schema with row-level security


The bridge model is pool plus an enforcement layer in the warehouse itself. Postgres row-level security policies, Snowflake row access policies, BigQuery row-level access policies, and Databricks row filters all let you attach a predicate to a table that the database itself applies before returning rows.


In Postgres:


```text
alter   table   orders   enable   row   level   security  ;


create   policy   tenant_isolation   on   orders
using   (tenant_id   =   current_setting(  'app.tenant_id'  )::uuid);
```


Every connection sets` app.tenant_id` after authenticating, and Postgres rewrites every query against` orders` to include the predicate. A developer who writes` select * from orders` from inside the application gets only their tenant’s rows.


Bridge is the strongest practical model for a typical SaaS product. The schema is shared, migrations are simple, cross-tenant analytics still works for internal tooling (using a service role that bypasses the policy), and isolation is enforced at the database boundary instead of in application code.


The tradeoff is performance. Row-level security adds a predicate to every query and can disable certain index scans if the planner has to evaluate a function call. For most workloads, the cost is small. For very high-throughput analytics on hot tables, you should benchmark.


## Comparing the three models


Property Silo (DB/schema per tenant) Pool (shared schema) Bridge (shared schema + RLS)


Schema migrations Fan out per tenant Single migration Single migration


Cross-tenant analytics Union across schemas Native Native (via service role)


Onboarding cost New schema, new pipeline config DB row in` tenants` table DB row in` tenants` table


Isolation enforcement Namespace boundary` WHERE` clause in app code Database policy


Risk of leakage Very low Higher (relies on app code) Low (DB enforces predicate)


Per-tenant performance isolation Strong if compute is also per-tenant Weak Weak (shared compute)


Compliance review Easiest Hardest Defensible


Typical fit Healthcare, finance, large enterprise Early-stage SaaS, freemium Most SaaS at scale


Most B2B SaaS teams should start in pool, move to bridge before any customer with a real security review signs, and only adopt silo for regulated workloads or contractually required isolation.


## The four layers where isolation has to be enforced


Tenant isolation is not one decision. It has to hold at four layers, and any one of them being weak invalidates the others.


### 1. Authentication and identity


The frontend should never send a` tenant_id` to the analytics backend in a way the user could change. Tenant context belongs in a server-issued, server-signed token, almost always a JWT, that the analytics layer verifies before doing anything else.


A minimal embed token payload looks like this:


```text
{
"sub"  :   "user_8c2f..."  ,
"tenant_id"  :   "acct_4d1a..."  ,
"user_attributes"  : {
"role"  :   "viewer"  ,
"region"  :   "EU"
},
"iat"  :   1717100000  ,
"exp"  :   1717103600
}
```


The token is signed on your backend with a secret that the analytics layer holds. The analytics layer never accepts tenant context from anywhere else. If a user can manipulate` tenant_id` by editing a request, you do not have multi-tenancy; you have a vulnerability with extra steps.


### 2. Connection or query routing


Once the request is authenticated, the query layer needs to attach tenant context to the connection or the query.


In Postgres, this usually means setting a session variable per request:


```text
set   local   app  .  tenant_id   =   'acct_4d1a...'  ;
```


In Snowflake, it might mean assigning a session role or setting a session parameter that row access policies read. In a query layer like Cube or dbt’s semantic layer, it means passing the tenant ID into the query templating or` securityContext` .


The important property is that the tenant context is set per request, scoped to the current transaction, and cleared afterward. A pooled connection that retains the previous user’s session variable is a leak.


### 3. Predicate enforcement


This is where row-level security earns its keep. Even if the query layer forgets to add` WHERE tenant_id = ?` , the database refuses to return rows for other tenants. RLS is the floor under everything else.


For warehouses without first-class RLS, the equivalent is a parametric view per tenant:


```text
create   secure view orders_for_tenant   as
select   *   from   orders
where   tenant_id   =   invoker_share()::uuid;
```


The query layer points at the view, not the table. The base table is not directly accessible to the analytics role.


### 4. Caching, joins, and metric definitions


The subtle leaks happen above the database.


Cache keys must include` tenant_id` . A chart cached as` revenue_by_month_2026_05` will hand customer B’s numbers to customer A the next time the same query runs. The fix is a cache key like` revenue_by_month_2026_05::acct_4d1a` . Most embedded analytics platforms do this automatically; verify it for any homegrown layer.


Joins through dimension tables need predicates on the dimension side too. If` orders` is RLS-protected but` products` is shared, a join can still expose which products customer B sells through aggregate counts. Either replicate the predicate on the joined side, or treat dimension tables as truly shared and reference data only.


Metric definitions in a semantic layer should treat tenant context as immutable. A user-modifiable metric that takes` tenant_id` as a parameter is the same vulnerability as the frontend trick above.


## The signed-attribute pattern


The pattern that holds these layers together is a signed JWT with user attributes that the BI layer reads and turns into SQL predicates.


The flow looks like this:


1. The user logs into your SaaS app. Your backend already knows their tenant and their role.
2. When the user opens an embedded dashboard, your backend mints a short-lived JWT that includes` tenant_id` and any other attributes (region, role, plan, etc.). It is signed with the embed secret you share with the BI tool.
3. The frontend passes the token to the embedded iframe or SDK.
4. The BI tool verifies the signature, extracts the attributes, and uses them as bind parameters in queries:` WHERE tenant_id = {{ jwt.tenant_id }}` .
5. The warehouse, with RLS or row access policies, applies the predicate again as a safety net.


Two important properties of this pattern:


- **Attributes are read-only at the BI layer.** A user cannot change` tenant_id` because they do not have the signing secret.
- **Tokens are short-lived.** Five to fifteen minutes is typical. Long-lived tokens that get logged or cached become an exfiltration risk.


This pattern is what lets you embed the same dashboard for a thousand customers and trust that each one only sees their own rows.


## Connection pooling, isolation, and warehouse cost


Multi-tenant analytics workloads run hot, and cost behavior depends heavily on whether compute is shared.


For pool and bridge models, all customers share the same warehouse compute. This is operationally simple but means a single heavy customer can starve others. The mitigation is a query queue with per-tenant concurrency limits, or warehouse-level features like Snowflake resource monitors and BigQuery slot reservations.


For silo models, you can give each tenant their own compute. Snowflake makes this practical with multiple virtual warehouses. The cost is real: idle warehouses still incur per-second charges if you keep them warm. Most teams end up with a small pool of shared warehouses for the long tail of customers and dedicated warehouses for the largest.


A common middle ground for embedded analytics is to maintain a connection pool per tenant tier. Free and trial customers share one pool, paying customers share another, and large enterprise contracts get dedicated pools. This keeps the operational story manageable and isolates the worst-behaving free traffic from paid customers.


If you cache at the BI layer (most platforms do), audit the cache size and the eviction policy. Cached results scoped to a tenant are fine, but a cache that grows linearly with tenant count needs sizing.


## Common mistakes


Five patterns show up in almost every cross-tenant incident postmortem.


**Trusting the frontend with` tenant_id` .** A` tenant_id` query parameter in the URL is convenient until someone changes it. Always derive tenant from a server-signed token.


**Cache keys without tenant context.** A query result cache that hashes only the SQL string will return tenant A’s data for tenant B’s identical query. Cache keys must include the tenant context applied to the query.


**Forgetting RLS on lookup tables.** Teams correctly enable RLS on` orders` and` subscriptions` and forget the` users` or` customers` table. A join that pulls customer names through that table leaks names across tenants.


**Allowing AI-generated SQL to write its own predicates.** When an AI agent generates SQL, it might include` WHERE tenant_id = 'acct_4d1a'` because it was prompted to. That is a convention, not enforcement. The query has to run against an RLS-protected table or a tenant-scoped view; the predicate the AI writes is decorative until the database enforces the same thing.


**Per-tenant connections without cleanup.** A long-lived application connection that sets` app.tenant_id` once and reuses the connection across users is the fastest way to leak data. Every request must set the variable in a transaction or session-scoped block, and pooled connections must be reset on release.


For a deeper treatment of how AI-driven analytics surfaces these risks, the[data governance for AI-powered BI guide](https://www.basedash.com/blog/data-governance-for-ai-powered-bi-row-level-security-access-controls-and-compliance) covers the policy patterns.


## When to choose each isolation model


A simple decision rubric:


- **Use pool** if you are pre-launch or pre-Series A, your customers are small, and your data team is one or two people. Move to bridge before you sign anything that requires a SOC 2 or ISO 27001 review.
- **Use bridge (pool plus RLS)** if you have customers who would notice a security incident, you need to run cross-tenant analytics for your own product team, and you want a single schema to operate. This is the right fit for most B2B SaaS products with embedded analytics.
- **Use silo** if you have at least one of: HIPAA, FedRAMP, PCI-DSS at the highest tier, or contractual physical isolation requirements; a small number of very large customers where dedicated compute is justified; or data that cannot share a backup or key with other tenants.


Most teams end up with a hybrid: bridge for the long tail and silo for the top 10 customers. A routing layer in the application chooses the right backend based on tenant ID.


## How modern BI tools handle multi-tenancy


The BI layer is where this pattern actually meets the user. The relevant capabilities to evaluate when shopping for an embedded analytics platform are:


- **Signed embed tokens with user attributes.** The platform should accept a JWT signed with a shared secret, extract custom claims, and make them available as bind parameters in queries and metrics.
- **Predicate injection in SQL and the semantic layer.** User attributes should be usable inside` WHERE` clauses, metric definitions, and joins. They should not be overridable by the embedded user.
- **Per-tenant cache scoping.** Cache keys should include user attributes by default, and the platform should expose tenant-aware invalidation.
- **Audit logs.** Every query should log the tenant context it ran under, for incident investigation.
- **AI scoping.** If the platform offers natural-language analytics, the AI must run inside the same tenant context as a SQL query. Asking the agent for “all customers” must not bypass RLS.


A short look at how a few common options approach this:


- **Basedash.** Embed the entire app or a single dashboard with a JWT that carries the tenant and user attributes. Row-level security uses the same attributes for SQL queries and the AI agent, so a customer asking the agent a question only ever sees their own rows. The cache is scoped per tenant, and customization controls let you decide which features each customer sees inside the embed.
- **Explo and Embeddable.** Both support signed JWT user attributes and predicate injection through their query layer. Both are good fits for SaaS teams that want a hosted embed component without managing a warehouse-side semantic layer.
- **Cube.** A query layer rather than a full BI tool. Cube’s` securityContext` reads the JWT and injects predicates into pre-aggregations and queries. Strong fit when you have a homegrown front-end and want server-side enforcement.
- **Metabase.** Embedded analytics with signed parameters. Works well for simpler embed needs, but the AI/natural-language story is less mature than the platforms built around it.
- **Looker (embedded).** The original signed-attribute embed model. Strong on LookML-driven RLS, weaker on cost for SaaS embed pricing.


For a broader survey, see the[best embedded analytics platforms compared in 2026](https://www.basedash.com/blog/best-embedded-analytics-platforms-compared-2026) and the[build vs buy decision framework](https://www.basedash.com/blog/build-vs-buy-embedded-analytics-a-decision-framework-for-saas-teams) .


## A short implementation checklist


Use this list when standing up multi-tenant analytics on a new SaaS product:


1. Pick a model: pool, bridge, or silo. Default to bridge unless you have a reason otherwise.
2. Add` tenant_id` to every fact and dimension table. Make it` NOT NULL` and indexed.
3. Enable row-level security on every table that contains tenant data, with a policy that reads the tenant context from a session variable or token claim.
4. Issue short-lived signed JWTs from your backend with` tenant_id` and any other attributes the BI layer needs.
5. Configure your embedded analytics platform to verify the JWT and inject attributes as bind parameters.
6. Verify cache keys include tenant context. Run a manual test: open the same dashboard as two tenants and confirm cache headers and result hashes differ.
7. Audit joins. Any table joined into a tenant-scoped query must either be tenant-scoped itself or be truly shared reference data.
8. Add an alert on the embed token issuer for tokens that exceed your expected expiry or include unexpected claims.
9. Run a red-team exercise: as one customer, try to retrieve another customer’s data through every input the dashboard exposes (URL params, filters, AI prompts, exported CSVs).
10. Review the audit log monthly for queries that ran without tenant context.


## FAQ


### Is row-level security enough on its own?


No. RLS is the safety net, not the only mechanism. The query layer should still scope by tenant so that the predicate is visible in execution plans and cache keys, the embed token should still bind tenant context to the user, and the application should still verify tenant on every request. RLS catches the cases where one of those layers is wrong, but it should not be the only thing standing between a user and another tenant’s data.


### Should every customer have a separate database?


Usually not. A separate database per customer is the right answer for a small set of regulated or contractually isolated customers, and the wrong default for everyone else. Schema migrations, observability, and cross-tenant product analytics all get harder linearly with the number of databases. Start with a shared schema and row-level security; promote large or regulated customers to dedicated infrastructure.


### How do AI agents fit into multi-tenant analytics?


An AI agent that generates SQL must run inside the same tenant context as any other query. Practically, that means the agent calls a query API that already has tenant context applied (via JWT and RLS), not the raw warehouse. The agent’s prompt should not include the tenant ID; the surrounding system should attach it as a bind parameter so the model cannot accidentally or intentionally reach across tenants.


### What does “signed user attributes” actually mean?


It means user identity and permissions are encoded in a JWT signed by your backend with a secret that only your backend and the analytics platform know. The user’s browser holds the token but cannot modify it without invalidating the signature. The analytics platform reads claims from the verified token and uses them as bind parameters in queries. This is the standard pattern across embedded analytics platforms in 2026.


### How do I test for cross-tenant leakage?


Set up two test tenants with distinct data. Create a known-unique value in tenant A (a row with a UUID nobody else has). From tenant B’s authenticated session, try to retrieve that value through every input the dashboard accepts: URL parameters, filter changes, manual SQL if the platform allows it, AI prompts, scheduled exports, and shared links. The unique value must never appear in tenant B’s responses. Repeat the test after every architecture change, and add the most useful checks to a CI suite if your platform supports it.


### Can I retrofit multi-tenancy onto a single-tenant warehouse?


Yes, but it is a project. The shape of the work is: add` tenant_id` to every fact table (often via a backfill from a customer foreign key), update every dashboard and metric to filter on it, enable row-level security with a policy that reads from a session variable, and update the application to set that variable on every connection. Most teams underestimate the dashboard rewrite, which scales with the number of saved queries and reports. Plan the migration in phases: pool the data first, then add RLS, then move the largest customers to dedicated compute.
