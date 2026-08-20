---
schema_version: "1.0.0"
document_id: "5e7f8390669fb9c491dea4d7cd4b4a7b066c3152eb666ee0a7f7558fac213a99"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/two-way-sync-amazon-rds-netsuite"
published_at: "2026-07-22T10:10:00+00:00"
first_seen_at: "2026-07-22T16:41:29.064477+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:ebbca93f375a011723163330586e8136dc9aa1795708d7e6793045cafdabcb28"
---

# What a Real Two-Way Sync Between Amazon RDS and NetSuite Takes

Getting NetSuite data into an[Amazon RDS](https://www.stacksync.com/connectors/amazon-rds) instance is a solved problem. AWS DMS, a Glue job, or a scheduled extract will land customers, items, and transactions in Postgres or MySQL, and the analytics team can go to work the same week. The hard part starts when somebody asks for the other direction.


An operations app writes a shipment status into RDS and needs it on the sales order in NetSuite. A pricing service recalculates a customer's terms in Postgres and AR has to see the change in the ERP. That write-back is where a one-way pipeline stops being useful, and where a two-way sync between Amazon RDS and NetSuite turns into a different engineering problem. It is no longer about moving rows. It is about deciding what happens when both copies of the same field change.


This guide covers what genuine bidirectional sync has to solve between an ERP and a managed operational database, what Amazon RDS constrains specifically, what NetSuite constrains specifically, and how the four realistic solution categories compare. If you are earlier than that and still choosing a platform for the whole stack, start with the guide to an[enterprise iPaaS for Amazon RDS](https://www.stacksync.com/blog/enterprise-ipaas-amazon-rds) .


## What two-way sync between Amazon RDS and NetSuite has to solve


A one-way pipeline has a single job: read from a source, write to a target, keep up. Almost everything it can get wrong is a throughput problem, and throughput problems have familiar answers. Bidirectional sync adds a set of problems that have nothing to do with throughput, and they are the reason most write-back projects run long.


-


**Write-back.** Reading NetSuite is a query. Writing to NetSuite is a transaction against a system with validation rules, mandatory fields, posting periods and locked records. The read path tells you almost nothing about how the write path will behave.


-


**Echo suppression.** Every write the engine makes into RDS shows up in the write-ahead log a moment later and looks exactly like a human edit. Unless the engine recognises its own writes, it picks them up and sends them back to NetSuite, which emits another change, and the record bounces.


-


**Conflict resolution.** When the same field moves on both sides inside the same window, something has to decide the winner. Last writer wins makes that decision a function of job scheduling. Field-level precedence makes it a function of who owns the field.


-


**Idempotency and retry.** ERP writes fail for ordinary reasons: a throttle, a timeout, a temporarily locked record. A retry has to update the same record rather than create a second one, which means a stable external key on both sides.


-


**Referential order.** A sales order cannot reference a customer that does not exist yet. The engine has to hold the child until the parent lands rather than firing both and letting one fail.


-


**Rejection handling.** When NetSuite refuses a write, that refusal is information. It needs to reach a person with the ERP's own error attached, not vanish into a log file.


A replication task and a two-way sync are not the same shape of thing. The difference is everything on the right.


None of those six are exotic. They are the standard content of every real bidirectional integration, which is why the general pattern is worth understanding before the specific one. The worked examples in[bi-directional sync explained](https://www.stacksync.com/blog/bi-directional-sync-explained-3-real-world-examples) cover the same mechanics on other pairs.


## What Amazon RDS changes about the database side


Amazon RDS is a managed relational database service. It runs PostgreSQL, MySQL, MariaDB, SQL Server, Oracle and Db2, and Aurora is a separate service with its own storage engine. Managed is the operative word: you get the engine you know, minus the parts of it that assume you are root on the host. That changes how change data capture gets set up, and it introduces one failure mode that will take an instance down if nobody is watching.


There is no superuser on an RDS instance. The master user is granted the` rds_superuser` role on Postgres, which is broad but not unlimited, and only AWS-supported extensions can be installed. Anything a self-managed CDC setup does by editing` postgresql.conf` and restarting has to be done through a custom DB parameter group instead.


For Postgres, logical replication is off by default. Turning it on means creating a custom parameter group, setting the flag, attaching it to the instance and rebooting, because the setting is static. The replication user also needs the right role.


sql


```text
-- Postgres on RDS: set in a CUSTOM DB parameter group, then reboot
--   rds.logical_replication = 1


-- then grant the role the CDC user needs
GRANT rds_replication TO sync_user;


-- MySQL on RDS: row-based binlog, and keep it long enough to recover
CALL mysql.rds_set_configuration('binlog retention hours', 72);
```


Enabling logical replication also creates the risk worth planning for. A replication slot holds the write-ahead log until the consumer confirms it has read past that point. If the consumer stops, the slot stops advancing and the WAL keeps accumulating on the instance's storage. Long enough and the instance runs out of disk, which stops writes to the production database. The failure did not start in the database. It started in whatever was reading from it. Any sync platform pointed at RDS has to advance its slot reliably, alert when lag grows, and clean up slots it abandons.


Connections are the other constraint. The default` max_connections` on RDS scales with the instance class, so a small instance has a modest ceiling, and a sync engine that opens a connection per worker competes with the application for it. RDS Proxy in front of the instance keeps pooling under control, which matters more once a second system is writing into the database on a steady cadence.


RDS instances also live inside a VPC, so any external service needs a deliberate network path: a peering connection, PrivateLink, or an allowlisted route. Add IAM database authentication and KMS encryption at rest and the security review is straightforward, but the path has to be designed rather than assumed. The same set of constraints shows up in any RDS integration, including[syncing Amazon RDS with Salesforce](https://www.stacksync.com/blog/sync-amazon-rds-with-salesforce) .


## What NetSuite changes about the ERP side


NetSuite offers several ways in. SuiteTalk exposes both a SOAP web services API and a REST record service. RESTlets are custom SuiteScript endpoints you deploy yourself, which is how teams handle records or behaviour the standard APIs do not cover well. SuiteQL runs SQL-style queries against NetSuite records, which is efficient for reads and irrelevant for writes.


The constraint that shapes every design is governance. SuiteScript charges usage units per operation, and a script that exceeds its allowance is stopped. NetSuite also caps how many concurrent requests an account may run, shared across every integration you have. A write path that opens as many parallel requests as it likes will either be throttled or will starve the scheduled scripts and interactive users that need the same budget. Treat the limit as a design input: batch where the API supports it, hold your own concurrency below the account ceiling, and back off on a throttle rather than hammering.


Then there is order. NetSuite records depend on each other, and the dependencies are strict. A sales order needs its customer to exist. A customer payment needs its invoice. An item fulfilment needs its order. A pipeline that pushes records in the order it happened to read them from a database will fail on roughly the fraction of them whose parents have not landed yet, and those failures look random until somebody notices the pattern.


Rejections are the third piece. NetSuite will refuse a write for missing mandatory fields, a closed posting period, a failed validation, or a record another process has locked. The ERP is right to refuse, and the error text usually says exactly what is wrong. The integration's job is to keep that write in a retry queue where retrying helps, and to surface it with the ERP's own message where it does not. Silently dropping it is how a database ends up quietly disagreeing with the ledger. For more on the read side of the same system, see[second-level NetSuite change data capture](https://www.stacksync.com/blog/unlock-netsuite-real-time-cdc) .


## What actually happens to a single record


The pieces are easier to hold together as the states one record passes through, rather than as a set of jobs and schedules.


The states a record moves through in a two-way sync, including the three ways a write can fail.


Three of those states are the whole argument. **Held in order** is dependency handling. **Backing off** is governance handling. **Rejected** is the case a one-way pipeline never had to think about, because it never wrote to a system with an opinion. A design that only draws the happy path will meet all three in its first week of production.


## The solution categories, compared


Four approaches show up in practice. They differ less on whether data can move and more on who ends up owning the six problems above.


**Custom code on SuiteTalk and RESTlets.** Full control, no per-record cost, and a genuinely reasonable choice for one or two record types with simple rules. The cost is that loop suppression, conflict precedence, governance backoff, dependency ordering, idempotency keys and a retry queue are all yours to build and keep working through NetSuite release cycles and schema changes on both sides.


**AWS DMS or Glue plus a write-back job.** DMS is a strong one-way replication and migration service, and for Amazon RDS replication into a warehouse or between engines it is often the right tool. It is not a two-way sync: a task has one source and one target, change data flows in that direction, and NetSuite is not an endpoint type it speaks. Choosing this means building a second, unrelated write-back path and then reconciling two systems that have no shared idea of state.


**Generic iPaaS.** Fast to wire up and useful for orchestration across many apps. The friction is shape and price: most are workflow engines where bidirectional behaviour is two flows you keep in agreement by hand, and pricing per record or per task turns an ERP with steady daily volume into an unpredictable line item.


**A real-time two-way sync platform.** Bidirectional behaviour, field-level conflict policy, echo suppression, dependency ordering and retry are product features rather than code you maintain. The trade is that you configure within a model instead of writing arbitrary logic, which is the right trade for keeping records consistent and the wrong one for complex multi-step business processes.


Custom SuiteTalk code DMS plus write-back job Generic iPaaS Stacksync


Direction Both, if you build both One way per task Two flows to keep aligned Two-way by default


RDS change capture You build it Native CDC Usually polling Logical replication or binlog


Echo / loop suppression You build it Not applicable Manual guard fields Origin tagging built in


Conflict resolution You build it None Last writer wins Field-level precedence


Referential order You build it None Manual sequencing Dependency-aware writes


NetSuite governance You handle backoff Not applicable Varies by connector Batching and backoff built in


Rejected ERP writes Custom error handling Not applicable Flow error branch Retried, then surfaced with the error


Pricing shape Engineering time Instance hours Per record or per task Per synced record, flat


Who maintains it Your team Your team, both halves Your team The platform


The four realistic approaches to a two-way sync between Amazon RDS and NetSuite.


The row that decides most evaluations is the second one from the bottom. Custom code and DMS both look cheap until you price the engineering time to keep six behaviours correct across two systems that release on their own schedules.


## How to choose, and what to check first


Start from latency and volume, because they rule options out quickly. If the RDS copy only feeds dashboards and nothing writes back, you do not need a two-way sync and a nightly extract is fine. The comparison in[real-time sync versus batch ETL](https://www.stacksync.com/blog/real-time-sync-vs-batch-etl) is the right frame for that decision. The moment an application writes to RDS and that value has to reach NetSuite, batch stops working, because a value that arrives tomorrow morning is not a value anyone can act on.


Then count the record types and name an owner for every field on each one. Two record types with a clean owner split is a custom-code project. Eight record types where finance owns the terms and an application owns the fulfilment fields is a platform decision, and the field-level ownership map you just wrote is exactly the conflict policy you will configure.


Whatever you pick, four checks are worth running before it reaches production. Confirm the engine advances its RDS replication slot and alerts on lag, so a stalled consumer cannot fill the instance's storage. Confirm every write into NetSuite carries an idempotency key, so a retry updates rather than duplicates. Confirm writes the engine makes into RDS are recognised as its own and not re-emitted. And confirm a rejected NetSuite write reaches a person with the ERP's error text attached.


To see Amazon RDS and NetSuite kept in step in both directions,[book a demo](https://www.stacksync.com/book-a-demo) , look at the[Amazon RDS and NetSuite integration](https://www.stacksync.com/integrations/amazon-rds-and-netsuite) , or read the wider guide to[NetSuite two-way sync solutions](https://www.stacksync.com/blog/netsuite-two-way-sync-solutions-enterprise-integration-guide) .
