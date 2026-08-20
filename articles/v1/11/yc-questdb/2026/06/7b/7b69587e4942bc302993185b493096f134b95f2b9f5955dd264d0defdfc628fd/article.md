---
schema_version: "1.0.0"
document_id: "7b69587e4942bc302993185b493096f134b95f2b9f5955dd264d0defdfc628fd"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/questdb-enterprise-3-3-1-release/"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:026a997c87d556d9e250eadc8d5355f1ed9381f8b0d91e730e497eb24456d7f9"
---

# QuestDB Enterprise 3.3.1: storage policies, custom CA, and finer-grained access control

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


QuestDB Enterprise 3.3.1 is out. It runs on the same QuestDB 9.4.2 engine we[just released](https://questdb.com/blog/questdb-9-4-2-release/) , and bundles the headline features from the 3.3.0 line: a new **storage policy** engine for tiering partitions to Parquet, **posting indexes** , and a move to **JDK 25** . On top of that, 3.3.1 adds a **custom root CA** option for replication object stores, **column-level` GRANT` /` REVOKE` with` EXCLUDE`** , and a round of correctness and resilience fixes.


We did not publish a separate post for 3.3.0, so this is also the first proper write-up of the storage policy and posting index work.


---


## Storage policy: tier partitions to Parquet on a schedule


The headline of this release is the storage policy engine. A storage policy automatically converts partitions from QuestDB's native format to Parquet on a schedule, with queries reading transparently across both. Parquet partitions compress better and, for selective queries, read less data thanks to row-group level bloom filters and statistics, so certain queries also get faster.


You define the rules once, as part of the table, and the engine handles the rest:


Tier to Parquet, with per-column encoding and a bloom filter on symbol


```text
CREATE TABLE trades (       ts TIMESTAMP,       symbol SYMBOL PARQUET(rle_dictionary, zstd(3), BLOOM_FILTER),       price DOUBLE PARQUET(plain, zstd(6)),       amount DOUBLE PARQUET(plain, zstd(6))   ) TIMESTAMP(ts) PARTITION BY DAY       STORAGE POLICY(TO PARQUET 3d, DROP LOCAL 1M)       WAL;
```


Here a partition stays in native format for its first 3 days, converts to Parquet once its whole time range ages past that window, and has its local copy dropped after a month. Each stage has its own independent TTL, so you can keep data queryable in compact Parquet long after the native files are gone, or skip` DROP LOCAL` entirely and just compress in place. You can also apply a policy to an existing table with` ALTER TABLE trades SET STORAGE POLICY(...)` .


The per-column` PARQUET(...)` clauses tune how each column is written once a partition is converted. They are ignored while the partition is still native, so there is no cost up front. Here` symbol` uses dictionary encoding plus a` BLOOM_FILTER` , which lets queries skip entire row groups on equality and` IN` predicates over symbols, while the high-cardinality` price` and` amount` columns use plain encoding with heavier` zstd` compression. Encoding, compression, and bloom filters are all optional and fall back to type-appropriate defaults. See the[CREATE TABLE reference](https://questdb.com/docs/query/sql/create-table/#per-column-parquet-encoding-compression-and-bloom-filters) for the full list of encodings and codecs.


Storage policies are replication-aware. A policy is enforced on replicas, and it is applied even when the policy definition arrives before the table it targets.


INFO


Storage policy is the foundation for the upcoming **Cold Storage** feature, which extends it to schedule offloading of Parquet partitions to S3 with transparent pullback.


Local Parquet partitions now also carry a metadata sidecar file that the query planner uses to prune and plan more efficiently, so the planning cost of querying across many Parquet partitions stays low.


See the[storage policy docs](https://questdb.com/docs/concepts/storage-policy/) for how to define and apply one.


---


## Posting indexes


Enterprise 3.3.1 adds support for the **posting index** , a more compact and faster-to-query index type than the original bitmap index. Posting indexes can also carry` INCLUDE` column sidecars, forming a covering index optimized for per-symbol, long-horizon queries where the index alone can answer the query without touching the table.


A plain` INDEX TYPE POSTING` declaration with no` INCLUDE` is non-covering. If you want the designated timestamp folded into the covering set automatically, the` cairo.posting.index.auto.include.timestamp` flag does that for you. See the[posting index deep-dive](https://questdb.com/docs/concepts/deep-dive/posting-index/) for details.


---


## Custom root CA for the replication object store


Replication and backup can now target object stores fronted by a private or internal CA, a self-signed certificate, or a TLS-intercepting proxy. The` s3` ,` azblob` , and` gcs` object-store configuration strings accept two new optional parameters:


- ` ca_cert_file=/path/to/ca.pem` trusts a private CA root or bundle **in addition to** the built-in Mozilla/webpki roots.
- ` ca_builtin_roots=false` trusts **only** the file you provide.


This is aimed squarely at regulated, on-premises deployments where the object store sits behind your own PKI.


WARNING


Object-store configuration parsing is now stricter. A` ;` -separated parameter with no` =` is rejected at startup with` InvalidObjectStoreConfigurationException` instead of being silently dropped. Well-formed` key=value;` configurations, including a trailing` ;` , are unaffected, and certificate paths must not contain a` ;` .


See[TLS with a private or self-signed CA](https://questdb.com/docs/high-availability/setup/#tls-with-a-private-or-self-signed-ca) .


---


## Column-level GRANT / REVOKE with EXCLUDE


Column-level grants and revokes can now name an` EXCLUDE` set, applying the permission to every column of a table except the ones you list:


Grant SELECT on all columns except counterparty


```text
GRANT SELECT ON trades(* EXCLUDE (counterparty)) TO john;
```


This is much easier than enumerating a long allow-list when you only need to hide a handful of sensitive columns.


See[excluding columns from the wildcard](https://questdb.com/docs/query/sql/acl/grant/#exclude-columns-from-the-wildcard) .


---


## Now on JDK 25


The runtime has been upgraded to JDK 25.


---


## Everything from QuestDB 9.4.2


Enterprise 3.3.1 runs on the[QuestDB 9.4.2](https://questdb.com/blog/questdb-9-4-2-release/) engine, a hardening release driven by continued fuzz testing and stricter query-result assertions. It folds in the additions from 9.4.1 and 9.4.2: the` array_agg()` and` regr_r2()` aggregates, window functions for` DECIMAL` columns, much faster materialized view refresh during out-of-order backfills, and a batch of correctness fixes across Parquet, posting indexes, temporal joins, and group-by. Read the[full 9.4.2 blog post](https://questdb.com/blog/questdb-9-4-2-release/) for the details.


---


## Hardening and bug fixes


Much of 3.3.1 is hardening of the 3.3.0 storage policy and posting index work, plus fixes pulled from the OSS engine:


- Fixed a posting-index JVM crash and missing/short row counts after partition squash, O3 commit, Parquet reseal, or WAL fast-lag apply.
- Tables written by a 3.3.0-beta build now migrate correctly on upgrade.
- Replication no longer logs spurious errors or slows uploads while a WAL segment is still open.
- Backup restore now retries transient object-store errors instead of aborting.
- A range of OSS SQL fixes for Parquet random access,` LATEST ON` ordering, dense ASOF join re-reads, cross-join` LIMIT` ,` count(*)` over empty partitions, and` UNION ALL` under aggregates.


See the full details on the[release notes page](https://questdb.com/release-notes/) .


---


## Getting the update


Self-managed enterprise customers will find the binaries at the usual download location. BYOC enterprise customers will be contacted for upgrading.


Not on QuestDB Enterprise yet? Learn more about[QuestDB Enterprise](https://questdb.com/enterprise/) and[BYOC](https://questdb.com/byoc/) , or[contact the QuestDB team](https://questdb.com/enterprise/contact/) for a conversation or a demo.
