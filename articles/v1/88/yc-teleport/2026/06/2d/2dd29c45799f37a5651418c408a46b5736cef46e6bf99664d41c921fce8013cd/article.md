---
schema_version: "1.0.0"
document_id: "2dd29c45799f37a5651418c408a46b5736cef46e6bf99664d41c921fce8013cd"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/postgresql-control-audit-agent-access/"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:42d3361bafe922477717b2ea7942c911cd5aa8f3d175f3d3a8df346b6551e122"
---

# PostgreSQL: How to Control and Audit Agent Access with Identity

[AI agents](https://goteleport.com/learn/agentic-ai/) querying databases pose well-documented risks. What gets less attention is the fact that[PostgreSQL has no native concept](https://goteleport.com/blog/securing-postgres-postgresql/) of an agent as a distinct actor. This means DBAs are managing access for something that appears in` pg_stat_activity` like any other role created with` CREATE ROLE` , with no distinguishing attributes and no indication of who or what initiated the connection. AI agents have no distinct identity when interacting with PostgreSQL. They authenticate as a role, meaning there is no way to distinguish them from any other connection.


**Read this article to learn:**


- How the lack of identity for AI agents impacts access control in PostgreSQL databases
- Why static service account credentials fail for agentic workloads
- How short-lived, task-scoped database access works as an alternative


## AI agents create database risks that humans don’t


Unlike a human employee, an agent won't pause to question whether it should run a query. Because the agent has no identity beyond the role it was assigned, it runs whatever its PostgreSQL role permits, without context.


Agent query patterns are non-deterministic, meaning "normal" agentic behavior covers a wide range of activity across tables, schemas, and row sets. That makes slow, low-volume misuse difficult to distinguish from legitimate activity, especially when a single PostgreSQL role has broad` SELECT` or` INSERT` privileges across multiple schemas. An agent with that level of access privilege can be used as an exfiltration tool by anyone who can control its inputs, and the actor performing the exfiltration need not touch the database directly.


## What went wrong in a real-world agentic AI database breach


You’ve probably heard about the AI agent that recently found a production API token in a file unrelated to its task and subsequently[deleted a production database](https://x.com/lifeof_jer/status/2048103471019434248) . The agent had no distinct identity of its own; it simply located a usable credential and acted on the access that credential provided.


This is the same exposure that AI agents have when they’re given[long-lived service account credentials](https://goteleport.com/blog/eliminate-shared-database-passwords/) . A static credential that grants broad database access requires no further exploitation after it is compromised and grants whoever holds it the same access the agent has, with no time limit or restrictions.


## How traditional database access controls fail for AI agents


Agents are typically granted the same PostgreSQL role as the service account they run under, but their behavior doesn't remain within that account's boundaries. As agent capabilities expand, DBAs may add privileges via` GRANT` statements that are rarely reviewed or revoked, resulting in these roles having broader access than the original use case requires. Credentials for these roles are usually stored in` .pgpass` files, environment variables, or connection strings. Rotating them requires an` ALTER ROLE` command and a manual update at every location where the credential is stored.


Attribution is a bigger problem. For example, when a DBA queries` pg_stat_activity` , they see a role name, a client address, and a current query. However, there is no identity data; none of the information identifies whether the connection is from an agent, which agent, or what human request initiated it.` application_name` can be set by the client, but it is optional and not enforced.


## What agent identity should look like


Treating agents as service accounts creates an identity problem. An identity is a verifiable, time-bound assertion about who or what is making a request and what it is authorized to do. However, a service account is simply a static credential attached to a system function, and makes no claims about identity or authorization.


An agent connecting to PostgreSQL should present a[cryptographic identity](https://goteleport.com/learn/what-is-cryptographic-identity/) issued for a specific task that provides only the level of access the task requires, and automatically expires when the work is complete. Each connection should be attributable to a specific agent and the request (and requestor) that triggered it.


## How to secure agent PostgreSQL access with identity


Moving from static credentials and service accounts to per-task identities requires enforcement at both the connection layer and the permission layer.Teleport's[auto user provisioning for PostgreSQL](https://goteleport.com/docs/enroll-resources/database-access/auto-user-provisioning/postgres/) creates a database user at connection time and drops it when the session ends, leaving no persistent credential to compromise. Each connection is purposed for the task that requires it, and the agent's database permissions expire once the task is finished, leaving behind an audit trail that clearly attributes events to the agent identity.


At the permission layer, schema-level grants are too broad for agent access. Granting` SELECT ON ALL TABLES IN SCHEMA public` , and relying on application-layer filtering to protect sensitive columns, means the data is reachable at the database level regardless of what the application does. Column-level privileges are more precise:


```text
GRANT  SELECT   (customer_id, order_date, status)  ON   orders  TO   agent_role;


```


For row-level restrictions, PostgreSQL's row security policies enforce access rules at query execution time instead of at the application layer:


```text
ALTER TABLE orders ENABLE ROW LEVEL SECURITY ;
CREATE POLICY agent_access ON orders
FOR SELECT TO agent_role
USING ( region   = current_setting( 'app.region'  )) ;


```


Audit logs are not a substitute for either of the above. PostgreSQL's pg_audit extension records what queries ran, but that record is written after the query executes. Permissions determine what can happen; logs confirm that permissions are working as intended. Teleport's[database access controls](https://goteleport.com/docs/enroll-resources/database-access/rbac/) add an enforcement layer before queries reach PostgreSQL, reinforcing the same principle at the connection level.


## Auditing agent queries with identity attribution


While PostgreSQL's` pg_audit` extension records query activity, every event is attributed to the PostgreSQL role that executed the query. If multiple agents and human users all connect through the same` app_service role` ,` pg_audit` cannot distinguish between them or prove that the connection was authorized in the first place.


Teleport's database audit event solves this attribution problem by recording the user identity alongside every database event. When an agent connects to PostgreSQL through Teleport, the` db.session.start` event captures the identity that initiated the connection, the database and role used, the target server, and whether the connection succeeded or was denied.


Every query the agent executes during that session is recorded as a` db.session.query` event. The event includes the full query text, any bound parameters for prepared statements, and the same identity fields that tie the query back to a specific agent and session:


```text
{
"cluster_name"  :    "production"  ,
"code"  :    "TDB00I"  ,
"db_name"  :    "customers"  ,
"db_protocol"  :    "postgres"  ,
"db_service"  :    "postgres-primary"  ,
"db_uri"  :    "postgres-primary.internal:5432"  ,
"db_user"  :    "order_agent"  ,
"event"  :    "db.session.start"  ,
"sid"  :    "63b6fa11-cd44-477b-911a-602b75ab13b5"  ,
"success"  :    true   ,
"time"  :    "2026-06-15T14:22:08.014Z"  ,
"uid"  :    "eac5b6c8-384a-4471-9559-e135834b1ab0"  ,
"user"  :    "order-processing-agent"
}


```


The user field identifies the Teleport identity, rather than the PostgreSQL role. This ensures that even if multiple agents query a database, you can still identify which agent is responsible for a specific query. Combined with the session ID (sid), DBAs are able to quickly reconstruct the full sequence of queries an agent executed during a connection.


Teleport also supports interactive session recording and playback. For example, running` tsh play` with a session ID produces a readable transcript of what occurred during the session:


```text
tsh play  63  b6fa11 -  cd44 -477  b -911  a -602  b75ab13b5


Session started  to   database "customers"  at   Sun Jun  15    14  : 22   UTC
postgres =  >    SELECT   customer_id, order_date, status  FROM   orders  WHERE   region  =    'us-east-1'  ;
SUCCESS ( 48    rows   affected)
postgres =  >    UPDATE   orders  SET   status  =    'processing'    WHERE   order_id  =    'ord-8812'  ;
ERROR: permission denied  for    table   orders ( SQLSTATE    42501  )
Session ended  at   Sun Jun  15    14  : 23   UTC


```


This data is also available in JSON format (` tsh play --format json` ) for integration with SIEM tools or automated analysis pipelines. Failed attempts are also recorded with the same level of detail as successful ones, including the error message PostgreSQL returned, so investigations do not depend on correlating logs across separate systems.


## Control and audit agent access to critical databases with identity


PostgreSQL does not distinguish an AI agent from any other role. As agents become more integrated into production systems, the distance between "what the role permits" and "what the agent should actually be doing" increases. Short-lived credentials, column- and row-level permissions, and detailed audit trails are best practices for controlling agentic access to databases like PostgreSQL and beyond. Teleport's[database access controls](https://goteleport.com/docs/enroll-resources/database-access/) help you enforce those practices at the connection layer without requiring changes to your application code.


Learn more about[Teleport for secure database access and audit](https://goteleport.com/platform/protected-identities/databases/) for pgAdmin, PopSQL, DBeaver, MySQL Workbench, MS SQL Management Studio, and more.
