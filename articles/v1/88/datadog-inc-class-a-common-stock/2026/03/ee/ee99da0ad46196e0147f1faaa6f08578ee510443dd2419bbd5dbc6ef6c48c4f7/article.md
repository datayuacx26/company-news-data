---
schema_version: "1.0.0"
document_id: "ee99da0ad46196e0147f1faaa6f08578ee510443dd2419bbd5dbc6ef6c48c4f7"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/debugging-postgres-performance/"
published_at: "2026-03-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:012abb6323b17925a584ce789d5ca3fade793b523bfde9d38412c4f2c83c56a3"
---

# When upserts don’t update but still write: Debugging Postgres performance at scale

Anthonin Bonnefoy


At Datadog, we track the life cycle of millions of ephemeral hosts that report telemetry data to our platform. When a host stops emitting data, we eventually need to clean it up to avoid bloating our metadata store.


To detect inactive hosts, the Datadog team that manages the host metadata store introduced a new upsert to track the last time a host was seen. We expected this new query to have minimal impact. Each host would be updated at most once a day, so even at 25,000 upserts per second, most queries should have been no-ops.


But when we rolled out the new query, disk writes doubled and[Write-Ahead Logging](https://www.postgresql.org/docs/current/wal-intro.html) (WAL) syncs quadrupled. We discovered that even when an upsert doesn’t change any values, it still locks the conflicting row, which is recorded in the WAL. Given that a Postgres cluster can only have a single writer, there’s a hard limit to how many writes it can handle. The increase in disk writes introduced by the new query was consuming too much of this limited budget and had to be fixed.


In this post, we’ll walk through how we diagnosed the unexpected overhead by inspecting Postgres’s WAL and how we rewrote the query to eliminate the cost without sacrificing correctness.


## Designing a low-overhead upsert in Postgres


When the[Datadog Agent](https://docs.datadoghq.com/agent/?tab=Host-based) runs, it collects host metadata—such as` host_id` , availability zone, and tags—and sends it to Datadog, where we store it in a Postgres database. Because hosts have a limited lifetime, they eventually terminate and stop emitting metrics. At that point, we can safely delete them from the database to keep it from growing indefinitely.


However, we don’t have a specific signal when a host has been terminated, other than the fact that it has stopped sending data. We decided that if a host hasn’t emitted metrics for 7 days, we can safely delete it. To support that, we needed a way to track the last time a host was seen. Updating a row every time we see a host is prohibitively expensive: In our largest data centers, this means **more than 25,000 updates per second** .


We needed to design our table and queries to avoid overloading our database, with three constraints in mind:


-


**Reduce update cost.** Postgres uses[MultiVersion Concurrency Control](https://www.postgresql.org/docs/current/mvcc-intro.html) (MVCC), so every update creates a new row version. Adding a` last_ingested` column to the existing host table would copy all host metadata on each update. Moving this field to a dedicated table will reduce the amount of data written per update.


-


**Minimize write amplification by relying on**[Heap-Only Tuples](https://www.postgresql.org/docs/current/storage-hot.html) **(HOT) updates.** By default, updates add entries to all indexes on the table. If an update does not modify indexed columns and there is enough free space on the page, Postgres can apply a HOT update, avoiding index writes and making updates significantly cheaper.


-


**Accept coarse-grained freshness.** We only need to know whether a host has emitted data within the past 7 days, so 1-day granularity is sufficient.


With those constraints, we created the following table:


```text
1   CREATE     TABLE     host_last_ingested   (    2          host_id   BIGINT     PRIMARY KEY  ,    3          last_ingested   TIMESTAMP WITH TIME ZONE     NOT NULL     default     now  ()    4   )   WITH   (fillfactor=  80  );
```


We didn’t create an index on the` last_ingested` column so that updates only modify unindexed columns, allowing Postgres to apply HOT updates. We also set the table’s` fillfactor` to 80%. This means inserts will only fill 80% of an 8 kB page, leaving free space on each page so that subsequent updates can stay on the same page and qualify for HOT updates.


For the query, we used this upsert:


```text
1   INSERT INTO   host_last_ingested   as   t    2       VALUES  ($  1  ,   now  ())    3       ON   CONFLICT (host_id)    4        DO   UPDATE    5         SET   last_ingested = EXCLUDED.last_ingested    6         WHERE   t.last_ingested < EXCLUDED.last_ingested -   '1 day'  ::interval;
```


This is a standard` ON CONFLICT DO UPDATE` upsert. If the host doesn’t exist, we insert it. Otherwise, we run the update. The` WHERE` condition limits updates to a host to once a day.


If we run the query twice on a nonexistent host:


```text
1   <query>    2   INSERT 0 1    3   <query>    4   INSERT 0 0
```


The` INSERT` command outputs the number of` processed_rows` in the second integer (the first one is always 0). The first query inserts 1 row, and the second reports 0 processed rows because the` WHERE` condition prevents the update from being applied.


With this query, we expected that most upserts would behave like no-ops. But that assumption turned out to be false.


## What we saw when we rolled out the upsert: Writes without updates


The new upsert was gradually rolled out on our smaller data centers until we reached about 500 upserts per second:


We saw an initial spike in insertions and no updated rows, which matched our expectation that most upserts would be no-ops:


However, the number of write IOPS more than doubled, even though the update rate remained flat:


Thankfully, it was a small data center and 500 IOPS was still within manageable limits. More importantly, WAL syncs increased by roughly the same amount:


In Postgres, all changes—such as inserting rows or modifying indexes—are recorded in the WAL. When a transaction commits, the WAL is flushed to disk to ensure durability, while table and index pages are flushed later by the background writer or during checkpoints. If a crash occurs, Postgres can replay changes from the WAL to restore consistency. The WAL sync metric reports the number of times these logs are flushed to disk.


Since the only change we’d introduced was the new query, it was likely the source of the WAL syncs, even though the rows weren’t actually being updated. To understand why, we needed to look deeper into what the upsert was writing to the WAL.


## Digging into Postgres WAL to explain the IOPS spike


Starting in Postgres 15, the[pg_walinspect](https://www.postgresql.org/docs/current/pgwalinspect.html) extension provides debugging functions to inspect the WAL. Since our metrics were pointing squarely at WAL activity, this was the most direct way to see what the upsert was actually doing. We installed the extension with:


```text
1   CREATE   EXTENSION pg_walinspect;
```


Once installed, the extension exposes the` pg_get_wal_records_info` function, which returns all WAL records between two log sequence numbers (LSNs). To make the extension easier to work with, we added the following shortcuts to our` ~/.psqlrc` :


```text
1   \  set   wal_lsn_start   'SELECT pg_current_wal_lsn() \\gset'    2   \  set   wal_records   'SELECT * FROM pg_get_wal_records_info(:''pg_current_wal_lsn'', ''FFFFFFFF/FFFFFFFF'');'
```


-


` :wal_lsn_start` saves the current LSN to the` pg_current_wal_lsn` variable.


-


` :wal_records` displays all WAL records between the saved LSN and the WAL end, using` FFFFFFFF/FFFFFFFF` as the upper bound (the maximum valid LSN value).


With those shortcuts available, we could reproduce the upsert and check whether it was generating WAL records:


```text
1   :wal_lsn_start    2   INSERT INTO   host_last_ingested   as   t    3       VALUES  (  1  ,   now  ())    4       ON   CONFLICT (host_id)    5        DO   UPDATE    6         SET   last_ingested = EXCLUDED.last_ingested    7         WHERE   t.last_ingested < EXCLUDED.last_ingested -   '1 day'  ::interval;    8   INSERT     0     0    9   :wal_records
```


The query didn’t update any rows, but` pg_walinspect` still showed WAL activity:


```text
1   -[ RECORD 1 ]----+-----------------------------------------------------------------    2   start_lsn        | 0/02469D30    3   end_lsn          | 0/02469D68    4   prev_lsn         | 0/02469CF8    5   xid              | 818    6   resource_manager | Heap    7   record_type      | LOCK    8   record_length    | 54    9   main_data_length | 8    10   fpi_length       | 0    11   description      | xmax: 818, off: 2, infobits: [LOCK_ONLY, EXCL_LOCK], flags: 0x00    12   block_ref        | blkref #0: rel 1663/5/16426 fork main blk 0    13   -[ RECORD 2 ]----+-----------------------------------------------------------------    14   start_lsn        | 0/02469D68    15   end_lsn          | 0/02469D98    16   prev_lsn         | 0/02469D30    17   xid              | 818    18   resource_manager | Transaction    19   record_type      | COMMIT    20   record_length    | 46    21   main_data_length | 20    22   fpi_length       | 0    23   description      | 2025-10-16 15:33:10.406748+00    24   block_ref        | <null>
```


Two WAL records were written:


-


A` Heap LOCK` record indicates a row lock on the relation` 16426` , which happens to be our new` host_last_ingested` table.


-


A` COMMIT record` is written because a transaction ID (` xid` ) was assigned to the transaction, and it must either be committed or rolled back.


To confirm what was generating the` LOCK` record, we attached a debugger to the Postgres backend process (` lldb -p $backend_pid` ) and added a breakpoint on` WALInsertLockAcquire` . The backtrace (abridged) showed:


```text
1   #0: postgres`WALInsertLockAcquire at xlog.c:1391:6    2   #1: postgres`XLogInsertRecord(...) at xlog.c:823:3    3   #2: postgres`XLogInsert(...) at xloginsert.c:524:12    4   #3: postgres`heap_lock_tuple(...) at heapam.c:5244:12    5   #4: postgres`heapam_tuple_lock(...) at heapam_handler.c:380:11    6   #5: postgres`table_tuple_lock(...) at tableam.h:1554:9    7   #6: postgres`ExecOnConflictUpdate(...) at nodeModifyTable.c:2749:9    8   #7: postgres`ExecInsert(...) at nodeModifyTable.c:1152:10
```


Looking at the code around the backtrace, we could trace what happens:


-


The upsert[checks for the existence of the tuple](https://github.com/postgres/postgres/blob/b7cc6474e930d4429b15657d6910e1e32066de5e/src/backend/executor/nodeModifyTable.c#L1121-L1154) .


-


The tuple exists, so[ExecOnConflictUpdate](https://github.com/postgres/postgres/blob/b7cc6474e930d4429b15657d6910e1e32066de5e/src/backend/executor/nodeModifyTable.c#L2743-L2754) runs and locks the row.


-


Locking the row modifies its metadata to include the locking transaction ID, assigning a transaction ID (` xid` ).


-


A[LOCK](https://github.com/postgres/postgres/blob/b7cc6474e930d4429b15657d6910e1e32066de5e/src/backend/access/heap/heapam.c#L5215-L5247) record is written to the WAL.


-


The` WHERE` condition evaluates to false, so the row is not updated.


-


Because a[transaction ID was assigned](https://github.com/postgres/postgres/blob/b7cc6474e930d4429b15657d6910e1e32066de5e/src/backend/access/transam/xact.c#L1350-L1355) , the transaction must be committed


-


A` COMMIT` record is written, and the WAL is flushed to disk.


Postgres’s` INSERT` documentation explicitly confirms this[lock behavior](https://www.postgresql.org/docs/current/sql-insert.html) for` ON CONFLICT DO UPDATE` :


> Only rows for which this expression returns true will be updated, although all rows will be locked when the` ON CONFLICT DO UPDATE` action is taken. Note that` condition` is evaluated last, after a conflict has been identified as a candidate to update.


This explains the surge in IOPS and WAL syncs we saw. Each upsert triggers a WAL sync, and each sync writes an 8 kB page to disk. With 500 upserts per second, we had 500 WAL syncs per second using 500 IOPS.


There’s only a limited amount of sync a disk can handle. We used[pg_test_fsync](https://www.postgresql.org/docs/current/pgtestfsync.html) to estimate how many WAL syncs per second the hardware could sustain. On a gp3 EBS disk, we reach around 1,000 syncs per second using 8 kB writes. Once that limit is reached, Postgres starts batching commits, which introduces latency.


This particular cluster handled 500 upserts per second just fine. But our biggest clusters will add 25,000 upserts per second. That would overwhelm the sync capacity for a query that was expected to behave like a no-op.


We needed to change our approach to avoid this overhead.


## Rewriting the query to avoid upsert overhead


We couldn’t rely on` ON CONFLICT DO UPDATE` , but we still needed to track each host’s last ingested time. The application also has strict latency requirements, so everything needs to happen in a single query. To do that, we emulated an upsert by using a[data-modifying Common Table Expression](https://www.postgresql.org/docs/current/queries-with.html#QUERIES-WITH-MODIFYING) (CTE).


Here’s how it works:


-


The CTE attempts to insert the row using` ON CONFLICT DO NOTHING` , which avoids locking existing rows.


-


If the insertion succeeds, the CTE returns the inserted row.


-


If no insertion occurs, the CTE returns nothing.


-


The outer` UPDATE` uses the presence (or absence) of a returned row to decide whether to run.


```text
1   WITH   insert_attempt   AS   (    2         -- Try to insert    3         INSERT INTO   host_last_ingested (host_id)    4         -- Values to insert    5         VALUES   (:host_id)    6         -- Only try to insert if row doesn't already exist    7         ON   CONFLICT DO NOTHING    8          RETURNING *    9   )    10   -- Do the update    11   UPDATE   host_last_ingested    12         SET   last_ingested=  now  ()   WHERE   host_id=:host_id    13         -- Only modify if last_ingested is older than 1 day    14         AND   last_ingested <   now  () -   '1 day'  ::interval    15         -- And only if no insertion was done    16         AND     NOT     EXISTS   (  SELECT   *   FROM   insert_attempt);
```


Compared to` ON CONFLICT DO UPDATE` , this version introduces potential race conditions. For example:


-


**Tx1** : The insert attempt fails because the row already exists.


-


**Tx2** : Deletes the row.


-


**Tx1** : Attempts to update the row that was deleted.


This happens because we’re not locking the row. This trade-off is acceptable for our use case: Host tracking can be imprecise, and the likelihood of seeing a host again after 7 days of inactivity is low.


We then checked the WAL records generated by this query to confirm that it behaved as expected.


### WAL record behavior


If the row **doesn’t exist** , we see the following records:


xid resource_manager record_type


798 Heap INSERT+INIT


798 Btree NEWROOT


798 Btree INSERT_LEAF


798 Heap HEAP_CONFIRM


798 Transaction COMMIT


Those WAL records add a new row to the table (` Heap INSERT+INIT` ) and a new entry in the index (` Btree NEWROOT + INSERT_LEAF` ). Because the insert uses` ON CONFLICT DO NOTHING` , it is performed speculatively and needs to be confirmed via the` HEAP_CONFIRM` WAL record.


If the row **already exists** and` last_ingested` is older than 1 day, we see the following records:


xid resource_manager record_type


800 Heap HOT_UPDATE


800 Transaction COMMIT


In this case, the update qualifies as a HOT update and does not require any index changes.


And if the row exists and` last_ingested` age is less than 1 day, no WAL records are generated.


After confirming that the new query avoided unnecessary WAL records, we rolled it out again to our small data center:


We saw the expected spike in update queries during the rollout:


This time, the increase in WAL syncs closely matched the actual row updates. When no rows were modified, the WAL sync rate remained steady:


## Lessons learned: WAL and query optimization


By using` pg_walinspect` , we were able to pinpoint the source of our unexpected overhead: During conflict resolution, an upsert locks the row, even if it doesn’t update anything. That lock modifies the row’s metadata with a transaction ID. Once a transaction ID is allocated, the transaction must be committed—even if no row was updated—triggering a WAL sync.


Thanks to this information, we:


-


**Rewrote the query to avoid the implicit row lock**


-


**Validated the new behavior by inspecting the resulting WAL records**


-


**Successfully removed the unnecessary overhead while preserving correctness**


Postgres’s WAL tooling has broader applications, too. For example,[pg_get_wal_stats](https://www.postgresql.org/docs/current/pgwalinspect.html#PGWALINSPECT-FUNCS-PG-GET-WAL-STATS) provides insight into the types and sizes of WAL records, which can help quantify the cost of index maintenance or the impact of[full page images](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-FULL-PAGE-WRITES) .


When you’re operating databases at scale, even “no-ops” queries can turn into performance surprises. Tools like` pg_walinspect` help make those surprises visible and fixable.


If you enjoy digging into systems like this to uncover hidden performance costs and building scalable solutions to fix them, we’re hiring! Explore[open roles on our engineering team](https://careers.datadoghq.com/all-jobs/?parent_department_Engineering%5B0%5D=Engineering&child_department_Engineering%5B0%5D=Backend&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025--postgresql&gh_src=hm4uekgj1us) .


-
-
-
