---
schema_version: "1.0.0"
document_id: "b087b4b7aa6caf29a1f5cfd3b8b792e212663dd046857f5205a718b031acbc23"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-design-a-bi-permission-model-for-a-saas-team/"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:e9b477ec445c05cd7580aca5bc986e5b2bf0959888614fe9c61fe2d9e1a52eb1"
---

# How to design a BI permission model for a SaaS team

A BI permission model decides who can see which data, who can build dashboards, and who can change shared definitions. For most SaaS teams, the right model has four independent layers: identity (who the user is), workspace (which folders and projects they can open), dataset (which tables, columns, and metric definitions they can query), and row level (which rows inside those datasets they can see). Tools that collapse these layers into a single “role” are easy to set up and almost always wrong by the time the company has a finance team, an external contractor, and a customer-facing dashboard.


This guide is for founders, heads of data, and operators setting up or rebuilding BI permissions for a B2B SaaS team between roughly 10 and 300 people. It walks through the four-layer model, how modern BI tools implement each layer, a default role design that holds up as the company grows, and the mistakes that quietly break governance once a few teams are using the tool every day.


## TL;DR


- BI permissions have four layers: identity, workspace, dataset, and row. Each layer should be enforced independently. Skipping a layer is how leaks happen.
- Identity is the foundation. Federate every BI tool to your IdP (Okta, Google Workspace, Azure AD) and provision groups before you assign permissions.
- Workspace permissions decide what people can open. Dataset permissions decide what they can query. Conflating them produces either over-restrictive folders or “everyone can see everything” datasets.
- Row-level security (RLS) belongs as close to the warehouse as possible. Enforcing it only in the BI tool is fragile because AI features, raw SQL, and data exports often bypass dashboard-level rules.
- A simple default role set is enough for most SaaS teams under 200 people: admin, builder, contributor, viewer, plus a separate “external” role for customers and contractors.
- The fastest way to break a permission model is to use folders as the only access boundary, give the data team an unrestricted role for convenience, and forget that AI prompts are now the new SQL.


## What a BI permission model has to do


A BI permission model is the set of rules that connects an authenticated person to the data they are allowed to see and the actions they are allowed to take. In a SaaS company, it has to satisfy four things at once:


1. **Confidentiality.** Sensitive fields (compensation, customer PII, financials, security incidents) only reach the right people.
2. **Self-serve.** Non-technical teammates can answer their own questions without filing a ticket every time.
3. **Trust.** Definitions of important metrics like ARR, churn, and active users are not silently overwritten by a teammate who built a one-off chart.
4. **Auditability.** When someone asks “who saw this report?” or “who changed this metric?”, you can answer.


These goals pull in different directions. Tight confidentiality slows self-serve. Open self-serve risks trust. Locking down editing keeps definitions stable but pushes people back into spreadsheets. A good permission model is the smallest set of rules that satisfies all four without making any of them painful.


The cleanest way to design one is to stop thinking about a single “role” and start thinking about four independent layers.


## The four layers of BI permissions


### Layer 1: identity


Identity is the answer to “who is this user?” and “what groups are they in?”. It comes from your identity provider (IdP), not your BI tool.


For a SaaS team, identity should be federated through SSO and SCIM as soon as you have more than ten people in the BI tool. The reason is not security theater. It is that group membership is the actual currency of permissions, and you want a single source of truth for groups so they propagate when someone joins the finance team, leaves the company, or moves to a customer-facing role.


Practical rules:


- Use your IdP groups (` finance` ,` engineering` ,` customer-success` ,` external-contractors` ) as the unit of permission, not individual users.
- Provision via SCIM so deprovisioning is automatic. Manual offboarding always misses someone.
- Treat service accounts (CI, ETL, embedded analytics tokens) as their own identities with their own groups. They should never share a human’s credentials.


If your BI tool only supports usernames and passwords with manual group assignment, that is a hard ceiling on how seriously you can take governance.


### Layer 2: workspace


Workspace permissions answer “which folders, projects, or collections can this user see?”. They are the navigation layer, not the data layer.


Most BI tools call this something different. Tableau has projects. Power BI has workspaces. Looker has folders and groups. Metabase has collections. Sigma has folders and workbooks. Basedash has workspaces and folders. The shape is similar: a tree of containers that hold dashboards, reports, and saved queries, with permissions inherited from parent to child.


Workspace permissions are useful for two things:


- Organizing dashboards by team or project so people see what is relevant to them.
- Coarse-grained access control where the dashboard itself is the secret (HR dashboards, security incident reviews, board metrics drafts).


They are not enough on their own. A workspace permission says “you can open this dashboard.” It does not say “you can only see your team’s data inside it.” For that, you need dataset and row-level rules.


The most common workspace mistake is using folders as the only access boundary. A finance dashboard sitting in an “executive” folder feels protected until a builder duplicates it into an open folder for a one-off question. The underlying dataset still allows the query, so the data leaks even though the original dashboard was locked down.


### Layer 3: dataset


Dataset permissions answer “which tables, columns, models, and metrics can this user query?”. This is where most teams underinvest.


In a modern BI stack, the dataset layer usually has three sub-layers:


- **Source tables.** The raw warehouse tables a user can query.
- **Semantic models.** The governed views, semantic-layer models, or LookML explores that wrap the raw tables with joins, business logic, and metric definitions.
- **Saved queries and metrics.** Reusable building blocks that other dashboards depend on.


A useful dataset model lets you grant access at any level. A customer success manager might be able to query the` customers` semantic model (which already hides cost-of-acquisition columns) without having any access to the raw` customer_costs` table.


A few rules that hold up over time:


- Default deny on raw warehouse access. Most teammates should query semantic models, not raw tables. The semantic layer is where joins, filters, and column-level redaction live.
- Sensitive columns (salary, customer email, SSN, credit card last four) should be masked or excluded in the semantic layer, not just hidden in the BI tool’s UI. Hidden columns reappear in CSV exports and AI-generated SQL more often than people expect.
- Metric definitions belong to a small, named group (“metric stewards”), not “everyone who can build dashboards.” Otherwise the definition of ARR drifts every few months.


If your BI tool ships a semantic layer (Looker’s LookML, Cube, dbt’s semantic layer, Sigma’s data models, Basedash’s[semantic layer](https://www.basedash.com/blog/introducing-basedash-semantic-layer) , Power BI’s tabular models), the dataset layer is where most of your real work lives. If it does not, the dataset layer leaks back into the warehouse, and you have to enforce it there with database roles and views.


### Layer 4: row


Row-level permissions answer “within a dataset the user is allowed to query, which rows can they actually see?”. This is the layer that fails most often.


The standard pattern is attribute-based: every fact table has a` tenant_id` ,` region` ,` team_id` , or` account_owner` column, and a rule like` region = $user.region` is injected into every query that touches it. Some tools enforce this in the BI layer, some in the warehouse, and the better setups enforce it in both.


A few patterns to know:


- **Attribute-based RLS** is the default. The user’s IdP attributes (region, department, tenant) flow into the BI tool and become predicates on every query.
- **Group-based RLS** works when you have a small number of clean buckets (“US sales sees US accounts, EU sales sees EU accounts”). It does not scale to per-tenant customer dashboards.
- **Warehouse-side RLS** uses the database’s own policies (PostgreSQL row-level security, Snowflake row access policies, BigQuery row-level security) so the rule applies regardless of which BI tool, AI agent, or notebook hits the warehouse.


For internal SaaS analytics, BI-layer RLS is usually enough. For customer-facing or embedded dashboards, you should layer warehouse RLS underneath the BI tool’s enforcement. The cost of one cross-tenant leak in an embedded dashboard is much higher than the cost of writing a few row access policies. The[multi-tenant analytics architecture guide](https://www.basedash.com/blog/multi-tenant-analytics-architecture-how-to-isolate-customer-data-in-embedded-dashboards) covers this case in more depth.


The single most common RLS bug is a cache that ignores the user attribute. If two users with different tenant IDs both query “monthly revenue,” the second user can be served the first user’s cached result if the cache key is just the query text. Whatever tool you pick, confirm that the query cache keys include user attributes, not just SQL.


## How major BI tools structure permissions


The four layers exist in every modern BI tool, but the names and the depth differ. The table below summarizes how a few common platforms map to the model.


Tool Identity Workspace Dataset Row level


Tableau SSO + groups via Tableau Server Projects with inherited permissions Data sources, plus row-level filters in published data sources User filters and entitlement tables


Power BI Microsoft Entra ID groups Workspaces and apps Datasets, semantic models DAX-based static and dynamic RLS


Looker SSO + LookML user attributes Folders and groups LookML models and explores Access filters and access grants in LookML


Metabase SSO in paid plans, groups Collections Database and table permissions per group Sandboxing (paid) for column and row filters


Sigma SSO + SCIM Folders and workbooks Datasets and data models User attributes and security rules


Basedash SSO + SCIM, IdP groups Workspaces and folders Connections, semantic models, saved queries Database-level RLS plus per-user attributes for embedded dashboards


Mode SSO + groups Spaces Connections and datasets Parameter-based filters; relies on warehouse RLS for strict isolation


There is no universal “best” model here. The right pick depends on whether your team primarily writes SQL (where Looker, Mode, and Basedash give you fine-grained dataset control), whether you live in the Microsoft stack (Power BI’s RLS is mature and tightly coupled to Entra ID), and whether you need to embed dashboards in your own product (Sigma, Basedash, and tools designed for embedding tend to have stronger per-user attribute models).


## Designing roles for a SaaS team


The four-layer model is the structure. Roles are how you instantiate it. For a SaaS team between 10 and 300 people, a small role set tends to work better than a large one. Five roles cover almost every case:


### 1. Admin


Owns identity, billing, and the permission model itself. Can create groups, change role definitions, and connect new data sources. Should be a small group (one or two people, plus a backup) and should not be the same people who build day-to-day dashboards.


### 2. Builder


Can connect to approved datasets, write SQL, build dashboards, and define new metrics. This is the role for the data team and analytics engineers. Builders can publish to shared workspaces but cannot grant new dataset access; that is the admin’s job.


### 3. Contributor


Can build personal dashboards, save queries, and edit dashboards in workspaces they have been added to. Cannot publish to top-level shared workspaces. This is the right role for operators and PMs who want to do their own analysis without becoming a data engineer.


### 4. Viewer


Can open dashboards in workspaces they have access to and ask follow-up questions through the tool’s AI or filter UI, but cannot write SQL or change definitions. Most of the company should sit here.


### 5. External


Customers, contractors, partners, or auditors. Should always have a dedicated role, not just be added to an existing one. External users see only the workspaces and rows that have been explicitly opened to them, and their access has an expiration date.


A few principles for assigning these roles:


- Default new hires to viewer. Promotion to contributor or builder is a deliberate decision, not a side effect of joining a Slack channel.
- The data team does not need an unrestricted “superuser” role for daily work. They need a builder role plus admin escalation for the rare case they actually need it.
- External users should never share a role with employees, even if the permissions look similar. A separate role gives you a single place to revoke access during an offboarding or a security review.


## Default deny: a rubric for new permissions


When a teammate asks for access to something, it helps to have a small set of rules to apply rather than deciding case by case.


A simple rubric:


1. **What is the data?** If it includes PII, financials, security data, or anything you would not want a screenshot of in a Slack DM, it needs an explicit approval, not a self-serve grant.
2. **What is the smallest dataset that answers the question?** Grant access to a semantic model, not the underlying raw table, whenever possible.
3. **What is the smallest scope that answers the question?** Grant access to a single workspace, not “all workspaces.”
4. **Does this need to be a permanent grant?** Time-bounded grants (30 days, 90 days, end of contract) avoid the slow drift toward “everyone has access to everything.”
5. **Is the access logged?** If you cannot see, six months from now, who has access and why, the grant is not done.


Most BI tools will not enforce all of this for you. The rubric is a habit, not a feature. The point is to have a default of “no, plus the smallest yes that solves the problem” instead of “sure, I’ll add you to the all-access group.”


## AI features change the permission model


Two years ago, dataset permissions were mostly about who could write SQL. Now an AI assistant inside the BI tool can write SQL on behalf of any user with a chat input, often against any table the underlying connection can reach.


This breaks two assumptions that older permission models depend on.


First, the set of possible queries is no longer bounded by the dashboards the data team has built. A viewer who can ask “show me last month’s revenue” can also ask “show me employee compensation” if the AI has access to that table. Hiding columns in the BI UI does not help; the AI sees the schema.


Second, audit trails get harder. A SQL query has a clear author. An AI-generated query has a prompt, a model, and a user, and the relationship between them is not always obvious in logs.


The implication is that AI features push more of the permission model into the dataset and row layers. If your AI assistant can only query semantic models that already hide sensitive columns and apply RLS predicates, it stays useful and stays safe. If it can query the raw warehouse on behalf of any logged-in user, you have effectively given everyone in the company a SQL editor regardless of their role.


Concrete checks for AI-enabled BI:


- The AI uses the same dataset and row-level rules as the human equivalent, with no separate “AI service account” that bypasses them.
- Sensitive columns are excluded or masked in the semantic models the AI is allowed to see, not just hidden in the UI.
- AI-generated queries are logged with the prompt, the user, and the resulting SQL so you can audit specific answers later.
- The AI cannot grant itself access to new datasets, even when a user asks it to.


The[data governance guide for AI-powered BI](https://www.basedash.com/blog/data-governance-for-ai-powered-bi-row-level-security-access-controls-and-compliance) goes deeper on the AI-specific governance patterns.


## Common mistakes


A short list of the failure modes that show up in almost every BI permission audit:


- **Folders as the only boundary.** A dashboard in an executive folder feels safe until a builder duplicates it elsewhere. The dataset layer is where access actually lives.
- **One “data team” group with full access.** Convenient for the data team, terrible for offboarding. Split it into builder (daily work) and admin (rare changes).
- **Per-user grants instead of group grants.** The first time you have to offboard someone, you discover that “Sarah from finance” had access to seven things nobody remembers granting.
- **No separation between internal users and external users.** When a contractor leaves and you only realize three weeks later, this is the mistake you regret.
- **Sensitive columns hidden only in the UI.** They reappear in CSV exports, scheduled emails, and AI-generated answers. Mask them in the semantic layer or warehouse view.
- **No audit log review.** Most BI tools have one. Most teams never look at it. A quarterly fifteen-minute review catches almost every accumulated drift.
- **Trusting BI-layer RLS for embedded customer dashboards.** This is the single highest-cost failure. Layer warehouse-side RLS underneath.


## A lightweight implementation checklist


If you are setting up or rebuilding a permission model in a SaaS BI tool, the work is roughly:


1. Federate the BI tool to your IdP. Provision via SCIM. Create the groups you will actually use (` finance` ,` engineering` ,` customer-success` ,` executives` ,` external-contractors` ).
2. Define the five roles (admin, builder, contributor, viewer, external) and write down what each can do at each of the four layers. Keep it on one page.
3. Identify the sensitive datasets (compensation, customer PII, financial detail, security data). Decide where they live and which group can query them. Mask sensitive columns in semantic models.
4. Set workspace defaults so new dashboards land in a team workspace, not “everyone.” Lock down the executive workspace explicitly.
5. Configure RLS on every fact table that has a tenant, region, or owner attribute. Verify the cache keys include the user attribute.
6. Audit the AI assistant’s dataset access separately. Make sure it cannot reach raw tables that humans cannot reach.
7. Set a quarterly calendar reminder to review the audit log, deprovisioned accounts, and external user expirations.


This is roughly a week of focused work for a single person at a 50-person SaaS, plus ongoing maintenance. The cost of skipping it shows up later as either a security incident or a panic during the next SOC 2 audit.


## When the four-layer model is overkill


Not every team needs all four layers from day one. A pre-seed startup with five engineers and a shared Postgres replica can usually get away with workspace permissions and one shared dataset for a while. The point at which the four-layer model starts paying off is roughly:


- The first non-engineering hire who needs read access to production data.
- The first customer-facing dashboard or embedded analytics use case.
- The first regulated dataset (HIPAA, PCI, financial reporting) the team has to handle.
- The first SOC 2 or ISO audit on the calendar.


Any of those is a good signal to stop using a single shared role and start treating permissions as a product. The teams that do this early have an easier time scaling self-serve analytics later, because the governance scaffolding is already in place when adoption picks up.


## Where Basedash fits


Basedash applies the four-layer model directly. Identity comes from your IdP via SSO and SCIM. Workspaces and folders organize dashboards by team. Dataset permissions are scoped through connections and the[Basedash semantic layer](https://www.basedash.com/blog/introducing-basedash-semantic-layer) , so non-technical users query governed models with metric definitions baked in instead of raw tables. Row-level security is enforced at the database level, which means PostgreSQL or Snowflake RLS policies apply automatically to AI-generated queries, embedded dashboards, and SQL written by hand.


For most SaaS teams between 10 and 300 people, the right starting point is to federate identity, set up the five roles above, define a small set of governed semantic models, and turn on warehouse-side RLS for any tenant-scoped data. Whether the BI tool is Basedash, Looker, Power BI, or something else, the model that holds up over time is the same: four layers, default deny, and a small habit of reviewing the audit log every quarter.
