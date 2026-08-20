---
schema_version: "1.0.0"
document_id: "fef57b7a0b4303b9f70f82242527cb733fd2b6f72d6296467cf9f344f26ec26a"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/enterprise-ipaas-mariadb"
published_at: "2026-07-22T09:00:00+00:00"
first_seen_at: "2026-07-22T16:41:29.064477+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:3b30b63ee8a33857aaf418d73c02c6049e20503bee79c01d1cb26e32ff1e575e"
---

# MariaDB Holds the Truth. Most Integrations Only Read It.

Ask a room of engineers where the real state of the business lives and you get an argument. Ask their database and you get an answer. Orders, subscriptions, entitlements, inventory positions, the row that decides whether an account can log in: in a large number of enterprises that data sits in MariaDB, and every other system is downstream of it.


The trouble starts with the word integration. In most stacks it means a nightly job that copies MariaDB tables into a warehouse so analysts can query them. That is extraction. It is useful, and it is one direction, one consumer, nothing flowing back. When a rep updates an account in Salesforce or support changes an entitlement in a ticket, the database everything else trusts stays wrong until somebody writes a script.


This guide covers what an integration platform actually has to handle when MariaDB is the system of record: how it detects change, how it writes back without knocking the database over, what happens when the schema moves under it, and how it behaves against read replicas and a Galera cluster. It also covers the MariaDB and MySQL differences that show up at exactly this layer.


## Why MariaDB ends up holding the operational truth


MariaDB began in 2009 as a fork of MySQL, taken by Michael Widenius after Oracle's acquisition of Sun put the original project's future in doubt. He named it after his younger daughter, Maria, the same way MySQL was named after his elder daughter, My. That story is worth reading in full, and we told it in[the origin story of MariaDB](https://www.stacksync.com/blog/the-father-who-forked-his-own-database-to-save-his-daughter-s-name-the-origin-story-of-mariadb) . The reason it matters here is practical: a fork that stayed open source under a foundation is a fork plenty of enterprises adopted deliberately, and it has been drifting from upstream MySQL ever since.


That drift is invisible until you connect something to it. Both databases speak the same wire protocol and both write a binary log, so a MySQL client connects without complaint. The details an integration depends on are no longer identical.


MariaDB MySQL 8


GTID format Domain, server, sequence (` 0-1-1234` ) Source UUID plus transaction number


JSON column Alias for` LONGTEXT` with a check constraint Native binary JSON type


System-versioned tables Built in, queried with` FOR SYSTEM_TIME` Not available


Common cluster shape Galera multi-primary, multi-source replication Group replication, InnoDB Cluster


Drivers MariaDB Connector/J, /C, /ODBC MySQL Connector/J, /C, /ODBC


Same protocol, different internals. An integration that assumes MySQL will eventually trip on one of these rows.


None of that stops a connection from opening. It does mean a change capture implementation written against MySQL's GTID format has to understand MariaDB's, and a field mapping that expects a native JSON column has to handle a` LONGTEXT` that happens to contain JSON. If you run both engines in the same estate, the[MariaDB connector](https://www.stacksync.com/connectors/mariadb) and the[MySQL connector](https://www.stacksync.com/connectors/mysql) are treated as separate systems for this reason.


## Change capture: read the binary log, do not poll the table


There are two ways to find out that a row in MariaDB changed. You can ask, repeatedly, or you can be told.


Asking means polling: a query every 30 or 60 seconds against an` updated_at` column, ordered and limited, per table. It works on small tables and gets expensive quickly. It needs an indexed timestamp on every table you care about, it misses hard deletes entirely, it misses any update that does not touch the timestamp, and it competes with your application for the same buffer pool and the same connections. At a few hundred tables it becomes background load a DBA will notice.


Being told means change data capture from the binary log. MariaDB already writes every committed row change there for replication and point-in-time recovery. A sync engine registers as a replica, reads the stream from a GTID position, and receives inserts, updates, and deletes in commit order with no query load on the tables. Set` binlog_format=ROW` so events carry before and after images rather than statement text, and keep` binlog_row_image=FULL` if you want conflict resolution to compare the old value.


Polling a timestamp column Binary log change capture


Latency One poll interval, 30 to 60 seconds Sub-second, as the transaction commits


Load on the database A query per table, per interval One replication stream, no table reads


Hard deletes Missed unless the app soft deletes Captured as delete events


Ordering Best effort, per table Commit order, across tables


Setup cost An indexed timestamp on every table` binlog_format=ROW` and a replication user


The difference is not only speed. Polling cannot see a hard delete at all, and no amount of tuning fixes that.


One engine attached to the cluster, with the business systems never talking to MariaDB directly.


That shape is the one to hold a vendor to. One stream in, writes back out through the same engine, and every application system on the far side going through the integration layer rather than opening its own connection to your primary.


## Writing back into MariaDB is the half most tools skip


Reading is the easy direction. Every ELT vendor can extract from MariaDB. Ask what happens when the value changes on the other side and the answers thin out fast.


A write-back path has to solve problems an extract never meets. It has to write a single row rather than reload a table, because reloading a 40 million row orders table to change one status is not something you do to a production primary. It has to write inside a transaction and be idempotent, so a retry after a network timeout does not create a duplicate. And it has to respect what is already on the table: foreign keys, unique indexes, triggers, and any` ON UPDATE CASCADE` relationship that quietly fans one write out across several tables. We walked through that fan-out in[cascading changes and best practices](https://www.stacksync.com/blog/mysql-cascading-changes-and-best-practices) , and it applies unchanged to MariaDB.


Then there is the connection budget.` max_connections` on a busy primary is a finite number, often set between 150 and 500, and it is already spoken for by the application's pool. An integration that opens a connection per worker, per table, or per sync run is competing with the thing that pays the bills. A platform should hold a small pooled count, release connections quickly, and back off rather than pile up when the server is under load. Ask for that number before you sign anything. The configuration walkthrough in[the MySQL two-way sync guide](https://www.stacksync.com/blog/mysql-two-way-sync-configuration-fast-real-time-guide) covers the same settings, and most of it transfers directly.


## Schema drift, replica lag, and Galera


Databases move. Someone adds a column, widens a` VARCHAR` , renames a field, or runs an online schema change that rebuilds the table under a new name and swaps it in. If the sync engine holds a cached picture of the schema and has no way to notice, one of two things happens: it starts writing into a column that no longer means what it did, or it stops.


-


**DDL in the binary log.**` ALTER TABLE` shows up in the stream. An engine that reads it can update its own mapping and surface new fields as unmapped, instead of failing on the next row event.


-


**Online schema change tools.**` gh-ost` and` pt-online-schema-change` build a shadow table and cut over. Change capture has to follow the cutover rather than keep reading a table that is about to be dropped.


-


**Replica lag.** Reading from a read replica takes load off the primary and costs you freshness.` Seconds_Behind_Master` is an estimate, not a guarantee, and a lagging replica turns a two-way sync into a race.


-


**Galera.** A Galera cluster is multi-primary with certification-based replication and per-node binary logs. Attach change capture to one pinned node, or the same transaction can be seen twice after a node rejoins.


Origin tracking is what makes the whole thing safe. Every write the platform makes into MariaDB is tagged, so when that row comes back through the binlog a moment later it is recognised as an echo and dropped rather than pushed back out to the CRM. Without it, a two-way sync is an infinite loop with a delay on it.


## What the integration layer has to own


Put those requirements together and the integration stops being a script attached to one end. It is a tier with its own responsibilities, sitting between the cluster and the systems that read from it.


The sync engine is a tier, not a cron job bolted to either end.


A shortlist should be tested against these, in a trial, against a copy of a table you actually run:


-


**Change capture from the binary log** , with a GTID position that survives a restart and a failover, and documented behaviour when that position is no longer available on the server.


-


**Row-level write-back inside a transaction** , idempotent on retry, through a pooled and capped set of connections.


-


**Field-level conflict resolution.** When the same field changes in MariaDB and in the CRM in the same minute, a policy decides, not whichever job happened to run last.


-


**Schema drift handling.** New columns surface as unmapped rather than being silently ignored, and a type change does not corrupt the target.


-


**TLS and least privilege.** A replication user with` REPLICATION SLAVE` and` SELECT` for reads, a separate scoped user for writes, TLS on both, and no copy of your data parked in the vendor's store.


-


**An audit trail per write.** Which system wrote this value, when, and what it replaced. That is what makes a change defensible six months later.


-


**Transforms where they belong.** Most mapping should be no-code. The exceptions are not, so the platform should let you drop into Python for the one field that needs a lookup or a format change, without moving the whole integration into a codebase.


## One-way ETL is not integration


A pipeline that copies MariaDB into a warehouse every night answers questions about yesterday. It does not keep the account status in the CRM aligned with the row that decides whether the customer can log in. Those are different problems, and only one of them is what people mean when they say the database has to be integrated.


Stacksync connects MariaDB to more than 1,000 systems on a single engine, in real time and in both directions, with change capture from the binary log, row-level write-back, no-code field mapping with Python transforms where you need them, and an audit trail on every write. Start with the[MariaDB connector](https://www.stacksync.com/connectors/mariadb) , the pair pages for[MariaDB and Salesforce](https://www.stacksync.com/integrations/mariadb-and-salesforce) and[HubSpot and MariaDB](https://www.stacksync.com/integrations/hubspot-and-mariadb) , or the two guides that go with this one:[how to sync MariaDB with Salesforce](https://www.stacksync.com/blog/sync-mariadb-with-salesforce) and[two-way sync between MariaDB and HubSpot](https://www.stacksync.com/blog/two-way-sync-mariadb-hubspot) . To see it running against your own schema,[book a demo](https://www.stacksync.com/book-a-demo) .
