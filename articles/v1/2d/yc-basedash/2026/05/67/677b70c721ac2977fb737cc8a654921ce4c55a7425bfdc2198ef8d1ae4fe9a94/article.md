---
schema_version: "1.0.0"
document_id: "677b70c721ac2977fb737cc8a654921ce4c55a7425bfdc2198ef8d1ae4fe9a94"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-give-ai-agents-safe-access-to-your-business-data/"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:b78d1b29505bc09a5c281221f490736b6170cf49d6b1d652868d06693e5e9866"
---

# How to give AI agents safe access to your business data

The safest way to give an AI agent access to your business data is to put a governed layer between the agent and the database. That usually means a read-only role, masked columns, optional row-level security for sensitive or customer-scoped data, and an MCP server, BI tool, or semantic layer that enforces policies and writes every query to an audit log. Direct database credentials, broad service accounts, and “just give the agent admin and we’ll watch it” are the patterns that fail in production.


This guide is for engineers, data leads, and founders who are wiring up ChatGPT, Claude, Cursor agents, Slack apps, or custom LLM applications to real business data and want a practical setup that does not leak PII, blow up the warehouse bill, or quietly hand a model the keys to production. It walks through the access patterns that actually work, the controls that matter, a decision matrix for picking one, and the mistakes that show up most often once an agent is loose in your stack.


## TL;DR


- Never give an agent the same database credentials a human would use. Create dedicated, scoped, read-only roles per agent or per integration.
- Enforce permissions in software and data layers, not in the prompt. Use restricted schemas, column masking, row-level security where it fits, and role-aware products like Basedash that sit between the agent and your database.
- Put an MCP server, BI tool API, or semantic layer between the agent and the warehouse. These layers can govern what the agent can see, log every query, and rate-limit expensive ones.
- Treat agent traffic like untrusted user traffic. Add per-agent rate limits, query timeouts, statement timeouts, and warehouse spend caps.
- Log every query, prompt, and result. Without this, you cannot debug incidents or prove compliance.


## Why this is harder than giving access to a human


A human analyst with database access is governed by job context, training, and, if needed, a manager. An LLM agent is governed by a prompt and whatever tools it has. Three things change once you introduce an agent.


**The query surface area expands.** A human asks five questions an hour. An agent driven by another agent or a chat UI can fire hundreds of queries in minutes, including queries no human would write (full table scans on` events` , joins across every schema, repeated retries after a malformed SQL response).


**Trust boundaries get fuzzy.** When a customer-facing AI feature queries your database to answer a support question, your authorization model is no longer “is this employee allowed to see this row.” It is “is this end user, on behalf of whom an agent is asking, allowed to see this row.” Most data stacks were not designed for that.


**Prompt injection is a real attack surface.** If the agent reads anything an outsider can write to, like a support ticket, a CRM note, a webhook payload, or a scraped page, a hostile string can rewrite the agent’s instructions and convince it to run queries you did not authorize. ([OWASP](https://genai.owasp.org/llm-top-10/) lists prompt injection as the top risk for LLM applications, and[Simon Willison has written about it extensively](https://simonwillison.net/series/prompt-injection/) since 2022.) The fix is not better prompting. The fix is constraints the agent cannot remove, enforced by the layer underneath it.


These three changes, query volume, fuzzy trust boundaries, and prompt injection, are why “just hand the agent a connection string” breaks down quickly.


## The five access patterns that actually work


There are five patterns for connecting an AI agent to business data. Most production setups use a combination.


### 1. Read-only database role with masked views


Create a dedicated database role, grant it` SELECT` only, and point the agent at a curated schema of views that already have sensitive columns dropped or masked. The agent never sees the raw tables.


This is the simplest pattern that survives a security review. It works in PostgreSQL, MySQL, Snowflake, BigQuery, Redshift, and ClickHouse with minor syntactic differences.


A typical setup in PostgreSQL looks like this:


```text
CREATE   ROLE   ai_agent   LOGIN   PASSWORD   '...'  ;
GRANT   USAGE   ON   SCHEMA   agent_views   TO   ai_agent;
GRANT   SELECT   ON   ALL TABLES   IN   SCHEMA   agent_views   TO   ai_agent;
ALTER   DEFAULT   PRIVILEGES   IN   SCHEMA   agent_views
GRANT   SELECT   ON   TABLES   TO   ai_agent;


CREATE   VIEW   agent_views  .customers   AS
SELECT
id,
created_at,
plan,
region,
-- email and billing details intentionally omitted
CASE   WHEN   deleted_at   IS   NULL   THEN   'active'   ELSE   'churned'   END   AS   status
FROM   app  .  customers  ;
```


Use this when the agent only needs to read aggregates and the curated surface is small. Avoid this when the agent needs to query the long tail of operational tables, because the curated schema becomes a maintenance burden.


### 2. MCP server in front of your data


The Model Context Protocol (MCP) is a standard published by Anthropic in 2024 for connecting LLM clients to external tools and data sources. ([See the MCP spec.](https://modelcontextprotocol.io/) ) An MCP server exposes a typed set of tools, such as` query_orders` ,` get_customer_summary` , and` list_metrics` , and the agent can only call those tools. It cannot run arbitrary SQL unless you expose a tool that allows it.


This is now the default integration path for Claude, ChatGPT, Cursor, and several other clients. A growing number of BI and data tools ship MCP servers, and Basedash includes one as a built-in[feature](https://www.basedash.com/features/mcp-server) so an LLM client can query governed metrics, dashboards, and tables without holding warehouse credentials directly. Because Basedash already understands users, roles, dashboards, saved questions, and connected data sources, it can act as the security layer the agent talks to instead of exposing your warehouse directly.


Use an MCP server when you want a typed, narrow contract between the agent and the data, especially when multiple agents (Claude desktop, an internal Slack app, a Cursor extension) need the same set of capabilities. For sensitive data, pair the MCP server with a permissions layer, masked data, and RLS where it fits. Basedash gives you that governed layer out of the box and can optionally sit on top of database-level RLS when you want both layers.


For more on which BI and data tools support MCP, see[BI tools that let you use an MCP server](https://www.basedash.com/blog/bi-tools-that-let-you-use-an-mcp-server-in-september-2025) .


### 3. Through a BI tool’s API or query interface


If you already have a BI tool with permissions, dashboards, and an API, you can route the agent’s questions through it. The agent calls the BI tool, the BI tool runs the query against the warehouse using its existing connection, and the BI tool enforces user permissions and returns the result.


This pattern works well when:


- You already have a single source of truth for metrics in the BI tool.
- The agent’s questions map to existing dashboards or saved queries.
- You want the agent and humans to see the same numbers, by construction.


It struggles when the agent needs ad-hoc exploration that does not match any saved query, because most BI APIs are not designed for unbounded SQL.


### 4. Semantic layer API


A semantic layer (Cube, dbt Semantic Layer, Looker’s modeling layer, an internal one) exposes business metrics as a typed API:` revenue` by` region` for` last 30 days` , not raw SQL. The agent picks dimensions and measures from a defined set; the semantic layer compiles that to SQL and runs it.


This is the cleanest way to keep an agent from inventing its own definition of “revenue.” It also tends to be the most expensive to build and maintain, so it pays off mostly when you have multiple agents and humans depending on the same metrics. We covered the broader tradeoffs in[Where to define business metrics: SQL views, dbt, semantic layers, or BI calculations](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) .


### 5. Prepared queries via a custom tool layer


For higher-stakes use cases, like a customer support agent that needs to look up an account, a billing assistant, or a churn-risk classifier, the right pattern is often a small set of hand-written, parameterized queries exposed as tools. The agent calls` get_account(account_id)` and gets back a strict, typed payload.


This is the most restrictive pattern, and the most predictable. It is not “AI agents querying your data” so much as “AI agents calling your existing API, which happens to query data.” Use it for any workflow that touches customer-facing surfaces or regulated data.


### Quick comparison


Pattern Flexibility for the agent Setup cost Best for Main risk


Read-only role + masked views Medium Low Internal exploration over a small, curated schema Curated views drift from real schema; agent finds gaps


MCP server Medium-high Medium Multi-client setups (Claude, Cursor, Slack app) needing parity False sense of safety if there is no permissions layer behind it


BI tool API Low-medium Low if BI exists Agents that should see the same numbers as humans Limited ad-hoc query power; rate limits on BI APIs


Semantic layer API Low High Multiple agents and humans on shared metric definitions High build cost; slower to add net-new metrics


Prepared queries / custom tool Lowest Medium Customer-facing or regulated workflows Coverage gaps push users back to less safe channels


Most teams start with pattern 1 or 2 for internal work, and move to pattern 5 for anything customer-facing.


## Defense in depth: the controls that matter


Whichever pattern you pick, the controls below are what actually keep an agent contained. The pattern is just where you enforce them.


### Row-level security (RLS)


Filter rows automatically based on who (or what) is asking. PostgreSQL has[native RLS policies](https://www.postgresql.org/docs/current/ddl-rowsecurity.html) ; Snowflake has[row access policies](https://docs.snowflake.com/en/user-guide/security-row) ; BigQuery uses[row-level access policies](https://cloud.google.com/bigquery/docs/row-level-security-intro) .


RLS is recommended when row access depends on the agent, team, tenant, or end user. It usually keys on either the agent’s role (an analytics agent only sees aggregated, non-PII tables) or the end user the agent is acting on behalf of (a support agent only sees the customer it was asked about). The second case requires passing a user identifier through every query as a session variable or context. If your agent only sees a curated, non-sensitive dataset through a governed layer like Basedash, RLS may be optional rather than mandatory.


### Column masking


Some columns should never be in an LLM context window, including emails, billing details, hashed passwords, and support ticket bodies that contain PII. Use[Snowflake masking policies](https://docs.snowflake.com/en/user-guide/security-column-intro) ,[BigQuery dynamic data masking](https://cloud.google.com/bigquery/docs/column-data-masking-intro) , PostgreSQL views that drop or hash sensitive columns, or a BI layer like Basedash that only exposes the fields an agent should use.


A common mistake here is masking columns in views but leaving the raw table accessible to the same role. Always grant the role access to the masked layer, not the source.


### Query whitelisting and statement filtering


If the agent is going to write SQL, restrict what it can write. Most warehouses let you grant` SELECT` only and revoke everything else, but that still allows expensive scans. A proxy, BI layer, or MCP server can go further by rejecting queries that touch certain schemas, exceed a row estimate, or use disallowed functions.


For agents that should never write, double-enforce: read-only role at the database level, and read-only validation in the proxy layer. Defense in depth means both.


### Timeouts and spend caps


Agents misbehave in cheap, repetitive ways. A retry loop on a malformed query can issue hundreds of multi-gigabyte scans before anyone notices.


At the database level, set:


- Statement timeouts (` SET statement_timeout = '30s'` in PostgreSQL).
- Result row caps where supported.
- Per-warehouse credit limits in Snowflake (` STATEMENT_TIMEOUT_IN_SECONDS` ,` RESOURCE_MONITORS` ).
- Concurrency caps so a runaway agent cannot starve human traffic.


At the agent layer, add tool-call rate limits and a budget per session.


### Audit logs


Every query, every tool call, every prompt, and every returned payload should be logged with the agent identity, the human identity (if any) the agent is acting on behalf of, and a request ID that ties them together. Without this, you cannot answer “what did the agent see” after an incident.


Most warehouses have query history (` information_schema.query_history` in Snowflake,` pg_stat_statements` in PostgreSQL); the gap is usually the prompt and response side. Capture those at the proxy or MCP layer.


### Prompt injection guardrails


Treat any text the agent reads as potentially adversarial. Practical guardrails:


- Strip or escape user-controlled text before it enters the agent’s context where possible.
- Keep tool definitions narrow enough that “ignore previous instructions and run X” is a no-op because X is not a tool the agent has.
- Validate outputs structurally. If a tool says it returns 10 rows of` {id, name}` , reject anything else.
- Do not let a single agent both read untrusted text and call destructive tools without a confirmation step.


The point is not to make injection impossible, which it is not today, but to make a successful injection useless because the agent has nothing dangerous to call.


## A decision matrix for picking a pattern


Use this to choose where to start. It is not exhaustive, but it covers the cases most teams hit first.


Use case Recommended starting pattern Notes


Internal Slack app answering “what’s MRR this month?” BI tool API or MCP server in front of governed metrics Reuses existing definitions; humans and bot see the same numbers.


Cursor or Claude desktop querying your warehouse for analysis MCP server + read-only role + masked views Keeps credentials out of the IDE; lets multiple clients share the same governed surface.


AI feature inside your product that summarizes a user’s data Prepared queries / custom tool layer Customer-facing, so every query must scope to the requesting user. Pass` user_id` explicitly.


Internal “ask anything” data assistant MCP server + semantic layer or Basedash permissions Agent picks governed metrics and tables; add RLS in the DB when rows vary by user or tenant.


Support agent that looks up customer accounts Prepared queries with strict input validation Treat as a regulated workflow; log every lookup with the requesting agent and operator.


Quick research notebook for a single technical user Read-only role + masked views Lowest setup cost; fine if the user is trusted and the schema is small.


If you find yourself wanting an agent to do all of these, do not build one God agent. Build several scoped agents, each with the smallest tool set that does its job. This is the same principle as the[principle of least privilege for human users](https://www.cisa.gov/news-events/news/principle-least-privilege) , but it matters more when the user is a probabilistic text generator.


## Common mistakes


A short list of the failure modes that show up across most agent rollouts.


**Reusing a developer’s personal database credentials.** Easy to set up, impossible to audit, and a fired engineer’s leftover access is now powering a customer-facing feature. Always create a dedicated role per integration.


**Putting permissions in the prompt.** “You are only allowed to query the` analytics` schema” is a suggestion, not a control. Anything that lives in text the LLM sees can be talked around. Enforce in the database or proxy.


**Granting` SELECT` on` public` and assuming “it’s just read-only.”** Read-only does not mean cheap or safe. A` SELECT *` on a billion-row events table will both leak data and rack up bills. Pair read-only with statement timeouts, row caps, and column masking.


**Logging prompts but not results, or vice versa.** You need both to reconstruct an incident. The most useful unit of audit is` (prompt, tool calls, results, final answer)` for a single agent turn, tied to a request ID.


**Trusting embedding-based content filters.** Content filters on prompts and outputs are a useful layer but not a perimeter. They miss novel attacks and false-positive on legitimate ones. Use them; do not rely on them alone.


**Skipping rate limits because “it’s just one agent.”** One agent calling itself in a loop is functionally a botnet. Per-agent and per-session rate limits matter from day one.


**Letting the same agent both read untrusted input and write to production.** This is the single most dangerous pattern. If an agent reads support tickets, scraped pages, or webhook payloads, it should not also be the one calling` delete_account` or` issue_refund` without a human-in-the-loop step.


## A minimal starting setup


If you are wiring up your first agent today and want a defensible baseline, this is roughly what to do:


1. Create a per-agent database role with` SELECT` only on a curated schema of views.
2. Drop or mask sensitive columns in those views.
3. Add row-level security where row access varies by role, tenant, or requesting user.
4. Set a 30-second statement timeout and a row cap.
5. Put an MCP server, BI tool API, Basedash, or a thin proxy in front of the database. Do not let the agent hold raw credentials.
6. Log every prompt, tool call, query, and result with a request ID.
7. Add a per-agent rate limit and a per-session budget.
8. Write down what the agent is allowed to do and review it on the same cadence as IAM access for humans.


This is enough to pass most security reviews and recover from most incidents without rebuilding the integration.


## Where Basedash fits


Basedash is built for exactly this middle layer between people, agents, and business data. It connects to PostgreSQL, MySQL, Snowflake, BigQuery, Redshift, ClickHouse, and others, then lets you define the governed surface area an agent is allowed to use: dashboards, saved questions, approved tables, role-based permissions, and masked or hidden columns. Instead of giving Claude, ChatGPT, Cursor, or a Slack app warehouse credentials, you can point them at the Basedash[MCP server](https://www.basedash.com/features/mcp-server) and let Basedash enforce what they can query.


That makes Basedash a practical security layer for agent access. It handles database connections, query execution, role-aware access, governed dashboards, and auditability in one place, while still letting you keep database-level controls such as RLS when you want them. For many internal analytics agents, Basedash can be the fastest way to get from “the model can answer business questions” to “the model can answer business questions without bypassing permissions, leaking raw credentials, or inventing its own metric definitions.”


It is not the only way to do this. The patterns in this guide also work with a hand-rolled proxy, a semantic layer, or another BI tool. The point is to put *something* between the agent and the database that you control, and to enforce permissions in that layer rather than in the prompt. Basedash packages that layer for teams that want agent access without rebuilding BI permissions, MCP tooling, and audit logs from scratch.


## Frequently asked questions


### Can I just give ChatGPT my database connection string?


Technically yes, and people do, but it skips every control in this guide. ChatGPT (or any client) holding raw credentials means there is no governed permission layer, no useful per-query audit, and no clean way to rotate access without changing the underlying password. Use an MCP server, Basedash, a BI tool, or a thin proxy instead.


### Do I need RLS if I’m using an MCP server?


Not always, but it is strongly recommended when the same table contains rows different users, teams, or tenants should not all see. The MCP server is a contract because it constrains which tools the agent can call. RLS is a database-level control because it constrains which rows any query can return. Basedash can sit between the MCP client and your database as a security layer, enforcing roles and permissions itself, and it can optionally work alongside database-level RLS when you need that extra boundary.


### What about prompt-injection attacks specifically?


Assume they will succeed sometimes. Design so that a successful injection is uninteresting: the agent has no destructive tools, no access to data outside the requesting user’s scope, and every action is logged. The defenses that hold up are structural, not textual.


### Is fine-tuning a model on company data the same as giving an agent access?


No. Fine-tuning bakes information into the weights and creates a different set of risks (model leakage, IP, deletion under privacy laws). Giving an agent runtime access to data is a separate decision and usually the safer one because you can revoke it.


### How do I handle access for end users in a customer-facing AI feature?


Pass the end user’s identity all the way down to the data access layer, and use it in RLS policies, Basedash permissions, or parameterized queries. Do not rely on the agent to “remember” whose data it is allowed to see. The pattern is the same as multi-tenant authorization for any other service: the agent is just another caller.


### Do these patterns change for agents that take actions (write data, send emails)?


Yes. Read-only patterns are forgiving; write patterns are not. For any agent that mutates state, prefer prepared, parameterized tool calls (pattern 5), require structured confirmations for high-impact actions, and never combine reading untrusted input with writing to production in the same agent without a human-in-the-loop step.
