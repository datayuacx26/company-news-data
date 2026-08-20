---
schema_version: "1.0.0"
document_id: "0ba35255698f137e187b176f6271bb0f77c4fa66a2eebe20f1f908c73d24e86d"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-database-migration-zero-downtime"
published_at: "2025-09-18T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:99dbdbcdaf3b240878259d5dc8c9a83511b14e3b0f1e8dd64231f68681013c58"
---

# How we did a database migration without logical replication - with zero downtime

At[Reducto](https://reducto.ai/) , our customer usage has been scaling extremely fast. To keep up, we designed an autoscaling system that spawns containers on-demand to process documents. This kept latency low and throughput high — until we hit a new bottleneck: our Postgres instance on Amazon RDS.


Naturally, we wanted to upgrade the database to a bigger instance type. However, at Reducto we serve many huge enterprises whose production code depends on our APIs running 24/7, so downtime wasn’t acceptable.


So began our search for a zero-downtime migration path.


## Options and constraints


### PlanetScale for Postgres


One of the first options we evaluated was *PlanetScale for Postgres* , mainly because:


1. They support upgrading instance types with zero downtime.
2. They even provide open source migration scripts ([postgres-direct](https://github.com/planetscale/migration-scripts/tree/main/postgres-direct) ).


This looks great on first look, but then I realized that in order to use Planetscale, our RDS database must have logical replication enabled rds.logical_replication, which we didn’t. Enabling it requires a reboot! If we had the luxury of rebooting, we could have updated the instance type as well and we wouldn't have needed to look any further beyond RDS.


Even if we had logical replication enabled, *PlanetScale for Postgres* had two more hurdles for us to overcome:


1. During our load test it required us to fine tune PSBouncer (since then they have changed name to PgBouncer) – their connection pooler. Currently we use RDS Proxy for connection pooling. RDS proxy felt like a better choice because we didn’t need to configure it as granularly as PSBouncer/PgBouncer and it just works.
2. Reducto has **Zero Data Retention (ZDR) policy** , where data older than 24hrs is automatically deleted. *PlanetScale for Postgres* doesn’t allow you to disable backups. It was a dealbreaker. So we decided to stick with RDS!


### Enter PgDog


The search continued, and we had a better sense of what we needed:


- No application changes.
- No logical replication.
- A way to “mirror” Postgres traffic from the old instance to a new one.


That led us to[PgDog](https://pgdog.dev/) , the successor to PgCat. PgDog natively[supports mirroring](https://docs.pgdog.dev/features/mirroring/) , which meant we could:


1. Run PgDog between the app and the old DB.
2. Mirror traffic to a new DB instance.
3. Wait 24 hours (enough to cover our ZDR window).
4. Swap databases with confidence that the data is in sync.


## How we did it


Since PgDog’s mirroring feature was marked experimental, we reached out directly to the founder for guidance. Together, we designed an evaluation setup that matched production as closely as possible!


What we tested:


1. **Connection limits** : We needed PgDog to handle our production-scale client connections while staying within the max_connections cap of our RDS database (which, annoyingly, requires a reboot to change). Luckily, PgDog doubles as a connection pooler—so it helped us stay within limits without touching the underlying DB settings.
2. **Mirroring metrics** : We wanted visibility into mirrored request counts, error rates, queue length, and dropped requests in order to determine when we could perform the database swap. The PgDog team actually built new metrics for us during this phase!
3. **Version upgrade** : Our prod DB was Postgres 16, so we took the opportunity to evaluate an upgrade to Postgres 17.


### PgDog Setup


We set up PgDog with two replicas for High Availability and behind a Network Load Balancer.


Key details:


- Admin password was required for[maintenance mode](https://docs.pgdog.dev/administration/maintenance_mode/) commands.
- Source (old) and destination (new) DBs had identical credentials (reducing swap-time variables).
- Configuration changes were managed via Helm and ConfigMaps (pgdog.toml).


This made it as easy as possible to perform the swap: we only needed to swap old and new hosts in` pgdog.toml` and everything stays the same.


Here’s a diagram of what that looks like:


### PgDog Configuration


This configuration is for Helm Chart and demonstrates the full configuration required for swapping.


#### Before Swap


```text
adminPassword: <admin-password>
mirrorQueue: 20000
minPoolSize: 50
defaultPoolSize: 2000


users:
- name: reducto
password: <password>
database: original # maps to database name in databases list


- name: reducto
password: <password>
database: new # maps to database name in databases list


mirrors:
- sourceDb: original
destinationDb: new


databases:
- name: original
host: original.xxxx.us-west-2.rds.amazonaws.com # direct connection to RDS
databaseName: postgres
- name: new
host: new.xxxx.us-west-2.rds.amazonaws.com # direct connection to RDS
databaseName: postgres
```


#### After Swap


Everything stays same except hosts in databases, which are swapped with each other:


databases:


```text
- name: original
host: new.xxxx.us-west-2.rds.amazonaws.com
databaseName: postgres
- name: new
host: original.xxxx.us-west-2.rds.amazonaws.com
databaseName: postgres
```


### Production Migration: Step by step


After running PgDog for 24 hours with mirroring, here are the steps we took:


1. Check PgDog Prometheus metrics on the Grafana dashboard to see if everything looks normal.
2. Compare the row counts of all tables in the old and new database and ensure that they are almost similar.


```text
sql  SELECT
table_schema,
table_name,
(xpath('/row/cnt/text()', xml_count))[1]::text::int as row_count
FROM (
SELECT
table_name,
table_schema,
query_to_xml(format('select count(*) as cnt from %I.%I', table_schema, table_name)
, false, true, '') as xml_count
FROM information_schema.tables
WHERE table_type = 'BASE TABLE'
AND table_schema NOT IN ('information_schema', 'pg_catalog')
) t
ORDER BY table_schema, table_name;
```


1. Ensure that mirror queue length is small in order of single digit on each PgDog pod with command \`SHOW MIRRORS;\`
2. Deploy new pgdog.toml config - which swaps hosts for old and new database instances - to Kubernetes cluster. On deploy ConfigMap is updated immediately, but because this ConfigMap is mounted as volume - we manually shelled into each PgDog pod to wait for config files to update on the filesystem at /etc/pgdog/pgdog.toml.
3. Turn maintenance mode ON - on each PgDog pod with command \`MAINTENANCE ON;\` - this “pauses” in-flight postgres connections and new queries.
4. Wait for the mirror queue to go to zero on each PgDog pod - we ran \`SHOW MIRRORS;\` until we saw queue length 0 on both pods.
5. Reload new config on each PgDog pod from filesystem at /etc/pgdog/pgdog.toml with command \`RELOAD;\`
6. Turn off maintenance mode on each PgDog pod with command \`MAINTENANCE OFF;\` - this “resumes” any “paused” connections or queries from step (4).
7. Now that PgDog is serving from the new database, update the application to use RDS proxy for database connection and remove PgDog.


## Results


The migration went exactly as our dry run during evaluation predicted. No downtime. No data inconsistencies. And as a bonus, we walked away with a major version upgrade (Postgres 16 → 17).


### Conclusion


What started as a straightforward need—“just upgrade the database”—turned into a deep dive through constraints, trade-offs, and tooling. PgDog’s mirroring, while still experimental, gave us a practical path forward that ended up being a smooth transition!


Thank you to the PgDog team for being great collaborators during this process, can’t recommend them enough!


*If solving problems like this is interesting to you, we’re hiring at Reducto:*[https://reducto.ai/careers](https://reducto.ai/careers)
