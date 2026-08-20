---
schema_version: "1.0.0"
document_id: "d45c822277d529d8270c018cd2dd7f6d2d6046871fa8e722938930db8592c87d"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/two-way-sync-motherduck-postgresql"
published_at: "2026-07-23T10:00:00+00:00"
first_seen_at: "2026-07-23T18:54:25.804928+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:ecad30bd97837f37f612841ce54e47211a001e9fee0eb576338f3fe546309b45"
---

# MotherDuck and PostgreSQL: Why ATTACH Is Not a Sync

Put DuckDB and Postgres in the same sentence and the first answer you will find is` ATTACH` . Install the postgres extension, point it at a connection string, and MotherDuck can query a live PostgreSQL database as if its tables were sitting in the warehouse. It works, it takes about a minute, and it answers a lot of questions. It is also not a sync, and the distance between those two things is where most projects get caught out.


Query federation and two-way sync solve different problems. Federation borrows data at read time and forgets it. A sync keeps two independent copies in agreement over time, in both directions, and has to decide what happens when both copies move. This guide covers what` ATTACH` gives you, where it stops, what a real bidirectional setup between an operational[PostgreSQL](https://www.stacksync.com/connectors/postgresql) database and an analytical[MotherDuck](https://www.stacksync.com/connectors/motherduck) warehouse has to solve, and when you should not build one at all.


If you are earlier than this and still picking a platform for the whole stack, start with the guide to an[enterprise iPaaS for MotherDuck](https://www.stacksync.com/blog/enterprise-ipaas-motherduck) . If the other side of your sync is a SaaS application rather than a database,[syncing Salesforce with MotherDuck](https://www.stacksync.com/blog/sync-salesforce-with-motherduck) covers that case instead.


## What ATTACH actually gives you


DuckDB ships a` postgres` extension, usually called the Postgres scanner. It lets a DuckDB or MotherDuck session attach a running PostgreSQL database and read its tables through the normal SQL surface. Filters and projections are pushed down where they can be, scans run in parallel, and the attached tables appear in the catalog next to everything else.


sql


```text
INSTALL postgres;
LOAD postgres;


-- attach a live PostgreSQL database, read only
ATTACH 'dbname=app host=db.internal user=analytics' AS pg
(TYPE postgres, READ_ONLY);


-- query it as if the tables were local
SELECT status, count(*)
FROM pg.public.orders
WHERE created_at > now() - INTERVAL 7 DAY
GROUP BY 1;


-- or pull a one-off snapshot into the warehouse
CREATE TABLE orders_snapshot AS SELECT * FROM pg.public.orders;
```


That is genuinely useful. Joining a warehouse fact table against a live application table without building a pipeline first is a good afternoon. Prototyping a model against real production shapes beats guessing at them. And for a small reference table that barely changes, reading it live is simpler than copying it. The reason federation does not grow into a sync is not that federation is bad. It is that federation answers a question, and a sync maintains a state.


-


**There is no incremental copy.** Every query re-reads Postgres over the wire.` CREATE TABLE AS` gives you a snapshot that starts going stale the moment it lands, and refreshing it means reading the whole table again, because nothing tracked what changed.


-


**There is no history.** Federation shows the row as it is now. If someone asks what a customer's plan was in March, the answer had to be stored at the time, and a live read never stored anything.


-


**Read performance is tied to the live connection.** Pushdown helps, but a wide analytical scan still crosses the network into a database sized for short transactional work, competing with the application for connections and cache.


-


**Nothing decides who wins.** Federation has no notion of two editable copies, so it carries no conflict policy, no origin tracking and no retry. There is nothing to be in conflict with, which is exactly the point.


-


**It only speaks database.** The scanner family covers Postgres, MySQL and SQLite. A SaaS API is not an attachable database, so anything past your own databases needs a different mechanism anyway.


The write question deserves a straight answer, because it is the one people ask first. The postgres extension can execute DML against an attached database, so an` INSERT` into` pg.public.orders` from a DuckDB session will run. What you do not get is anything that makes it a write-back: no record of what changed since the last run, no idempotency key, no retry when a statement fails halfway, no suppression of the change your own write just caused, and no rule for the case where the row moved on both sides. That is a manual write, not a sync.


## Federation, replication and two-way sync are three different things


It helps to name the three shapes, because teams routinely evaluate one while describing another. **Query federation** is` ATTACH` : no copy, no schedule, no state. **One-way replication** is a pipeline, where Postgres change data lands in MotherDuck on a cadence, the warehouse copy is materialised and queryable without touching production, and nothing travels back. **Two-way sync** is a single engine that reads and writes both sides, tracks the origin of every write, and holds a policy for collisions.


ATTACH federation One-way replication Stacksync two-way sync


Copy of the data None, read at query time Materialised in MotherDuck Materialised, current both ways


Direction Reads Postgres only Postgres to MotherDuck Both, from one engine


Change tracking None WAL or a polled watermark WAL in, watermark out


Historical state None Whatever you choose to keep Whatever you choose to keep


Load on Postgres Every analytical scan One replication slot One replication slot


Writes back to Postgres Manual DML you issue Not part of it Batched, idempotent, retried


Loop prevention Not applicable Not applicable Origin tagging built in


Conflict policy None Not applicable Field-level precedence


Setup One SQL statement A pipeline to build and run Configuration


Good for Ad hoc joins, prototypes Analytics on production data Values that become operational


Three different shapes, routinely evaluated as if they were one.


Most teams need the middle column and believe they need the right one. The test is short: does a value that only exists in the warehouse ever have to change something in the application? If the answer is no, one-way replication is correct and a two-way sync is complexity you will maintain for nothing.


## What a real two-way sync between MotherDuck and PostgreSQL has to solve


Once the answer to that test is yes, five problems arrive together. None of them is exotic, and all five have to be owned by whatever sits between the two systems.


The four jobs ATTACH never does for you, and the reason a sync needs an engine in the middle.


**Change capture on the Postgres side.** Postgres has the good half of this problem solved already. Logical replication turns committed row changes into an ordered, durable stream a consumer can read, with no polling and no triggers. It needs` wal_level = logical` , a publication naming the tables, and a role with the replication attribute.


sql


```text
-- postgresql.conf (a restart is required)
--   wal_level = logical


CREATE PUBLICATION stacksync_pub FOR TABLE public.orders, public.customers;
CREATE ROLE sync_user WITH REPLICATION LOGIN PASSWORD '...';
GRANT SELECT ON ALL TABLES IN SCHEMA public TO sync_user;


-- the number to watch: an idle slot pins the WAL on disk
SELECT slot_name, active,
pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), restart_lsn)) AS retained
FROM pg_replication_slots;
```


That last query belongs on a dashboard. A replication slot holds the write-ahead log until its consumer confirms it has read past that point. If the consumer stops and the slot stays, the WAL keeps accumulating on the database's disk, and a full disk stops writes to production. The outage does not begin in Postgres. It begins in whatever stopped reading from it. The mechanics are covered in more depth in the guide to[PostgreSQL logical decoding plugins](https://www.stacksync.com/blog/postgresql-logical-decoding-plugins-developers-guide) .


**Watermarks on the MotherDuck side.** The warehouse has no equivalent. MotherDuck does not publish a row-level change feed, so "what changed here since the last run?" is not a question you can subscribe to. It has to be modelled. The usual answers are an` updated_at` column maintained by whatever writes to the table, or an explicit change table that every write appends to.


Watermarks are easy to get subtly wrong. One based on wall-clock time misses rows written by a transaction that started before the last run and committed after it, so a safe implementation re-reads a small overlap window and leans on idempotent application rather than on the boundary being exact. A monotonic sequence beats a timestamp wherever you can have one. The same trap on the other side of the wire is covered in[watermarks for parallel CDC backfills](https://www.stacksync.com/blog/postgres-watermarks-parallel-cdc-backfill-sync) .


**Batched writes instead of row-by-row updates.** DuckDB stores data in columns. Updating one row means rewriting the column segments that hold it, which is inexpensive once and painful ten thousand times in a loop. The pattern the engine is built for is to land the batch in a staging table and then apply it in a single statement with` MERGE` or` INSERT ... ON CONFLICT` , keyed on a stable business key. Ten thousand row updates become one statement. It also keeps a sync job from monopolising the compute instance an analyst is querying through, since MotherDuck allocates compute per user and per session.


**Loop prevention.** Write a row into Postgres and it appears in the write-ahead log a moment later, indistinguishable from a user edit. Without origin tracking the engine reads its own write, decides MotherDuck is stale, writes it there, and the record starts bouncing. Three things stop it: origin metadata on every write so the engine recognises its own echoes, a short suppression window keyed on the record and the field, and a value comparison so a write that did not actually change anything never emits an event.


**Conflict resolution, and who wins.** When the same field moves on both sides inside the same window, something has to decide. Last writer wins is the common default and it is a poor one, because it makes the outcome depend on which job happened to run last. Field-level precedence is better, and it is mostly a conversation rather than an engineering problem: Postgres owns the fields the application writes, MotherDuck owns the fields a model computes, and once every field has one owner most collisions stop happening. The ones that remain resolve the same way every time and get logged. The worked examples in[bi-directional sync explained](https://www.stacksync.com/blog/bi-directional-sync-explained-3-real-world-examples) apply the same rules to other pairs.


## What happens to a single row


Those behaviours are easier to hold together as the states one row passes through than as a set of jobs and schedules.


The states a row moves through, including the three ways a batch write ends up somewhere other than confirmed.


Three of those states carry the whole argument. **Echo dropped** is loop prevention doing its job. **Conflict** is the policy question, answered per field rather than per record. **Retrying** is the acknowledgement that a batch write fails for ordinary reasons and has to be reapplied without duplicating anything, which is why the batch needs a stable key. A design that only draws the straight line down the middle will meet all three in its first week.


## When you actually want two-way, and when you do not


Not every workload should be bidirectional, and choosing two-way by default has a real cost. Every field that syncs both ways is a field with an ownership question, a precedence rule to define, and a support conversation waiting for the first person who disagrees with the outcome.


Most warehouse workloads are read-only by nature. Dashboards, models, ad hoc analysis and reporting all consume data; none of them produce a value the application needs back. For those, one-way replication into MotherDuck is the right shape, and it is simpler, cheaper and easier to reason about at three in the morning.


What you are doing The shape that fits


Ad hoc joins against live application tables ATTACH federation, no pipeline at all


Dashboards and models over production data One-way replication into MotherDuck


A score or segment computed in the warehouse that the app reads Reverse ETL out of MotherDuck


A field both the application and an analyst can correct Two-way sync with field-level precedence


A warehouse table that is the record of truth for something operational Two-way sync, with Postgres reading from it


Match the mechanism to the direction the value actually travels.


The middle row is where reverse ETL lives, and it is a real option rather than a consolation prize. If the warehouse computes something and the application only reads it, a one-way push out of MotherDuck is enough, because one writer means nothing to reconcile. It stops being enough the moment the application can also edit that value, since then you have two writers and no rule.[ETL versus reverse ETL](https://www.stacksync.com/blog/etl-vs-reverse-etl-guide-modern-data-integration) is a useful frame for where that boundary sits.


## Choosing, and what to check before production


Start from latency, because it rules options out quickly. If the MotherDuck copy feeds dashboards and nothing writes back, a scheduled load is fine, and[real-time sync versus batch ETL](https://www.stacksync.com/blog/real-time-sync-vs-batch-etl) is the decision worth reading first. The moment a value has to travel back into Postgres and be acted on, batch stops working, because a number that arrives tomorrow morning is not a number anyone can use today.


Then write down every field that will sync and put one owner's name next to it. That list is your conflict policy, and writing it is usually the point where a ten-table plan turns into a three-table plan. If you cannot name an owner for a field, it probably should not be bidirectional yet.


Whatever you build or buy, four checks are worth running before it reaches production. Confirm the consumer advances the Postgres replication slot and alerts on retained WAL, so a stalled sync cannot fill the disk. Confirm writes into MotherDuck are batched and keyed, so a retry updates rather than duplicates. Confirm the engine recognises its own writes on both sides. And confirm every synced field has a named owner and a precedence rule recorded somewhere other than the engine's configuration screen.


To see PostgreSQL and MotherDuck kept in step in both directions, look at the[MotherDuck and PostgreSQL integration](https://www.stacksync.com/integrations/motherduck-and-postgresql) or[book a demo](https://www.stacksync.com/book-a-demo) . The same engine connects[DuckDB](https://www.stacksync.com/connectors/duckdb) and pairs like[MotherDuck and Snowflake](https://www.stacksync.com/integrations/motherduck-and-snowflake) , and the[MotherDuck connector announcement](https://www.stacksync.com/blog/introducing-the-motherduck-connector) covers what shipped. If you would rather have the backstory, the[DuckDB origin story](https://www.stacksync.com/blog/named-after-a-real-duck-the-origin-story-of-duckdb) explains where the name came from.
