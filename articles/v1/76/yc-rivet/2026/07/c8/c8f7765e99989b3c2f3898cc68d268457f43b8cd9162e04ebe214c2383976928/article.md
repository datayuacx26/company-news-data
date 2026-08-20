---
schema_version: "1.0.0"
document_id: "c8f7765e99989b3c2f3898cc68d268457f43b8cd9162e04ebe214c2383976928"
company_key: "yc-rivet"
company: "Rivet"
source_id: "yc-rivet-news-import-b9b70e7746c3"
canonical_url: "https://rivet.dev/blog/2026-07-31-how-we-built-the-first-zero-disk-s3-tiered-storage-engine-for-sqlite/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T19:11:06.802677+00:00"
fetched_at: "2026-07-31T19:11:08.509085+00:00"
content_hash: "sha256:d8fa323dfc32c7e4cecd1d92e5738939c564987b7d338d635b01b42bccd91f4a"
---

# How We Built the First Zero-Disk, S3-Tiered Storage Engine for SQLite

Every Rivet Actor gets its own **isolated SQLite database** . In practice, that means running **millions of databases** that start instantly, cost nothing but storage while idle, and never touch a local disk.


**TL;DR:** Rivet’s SQLite storage engine now ships with:


- **S3 tiered storage** : cold data automatically offloads to S3-compatible object storage
- **Point-in-time recovery** : recover any database to a point in time
- **Massively improved SQLite performance** : commits return in low milliseconds, Actors start instantly with no restore, and hot pages read at near-in-memory speed


##


Why build yet another SQLite storage engine?


When designing SQLite support for Actors, we looked at a lot of off-the-shelf options, and none of them quite fit the bill. It came down to **eight main constraints** that caused us to build a new SQLite storage layer from the ground up.


###


The eight constraints


**Speed:**


1. **Fast, durable writes** : How fast data can be written to the SQLite database directly impacts user-facing performance. Writes must return in low milliseconds while still maintaining at least triple redundancy.
2. **Instant startup with no local data** : Actors open their SQLite database on startup, so opening must be instant to make sure there is no delay when Actors crash, upgrade, or migrate.


**Simplicity:**


1. **Zero-disk compute for elastic scaling** : Machines running Actors hold no volumes and no local state, so compute can scale up and down freely without moving data around.
2. **Self-hostable, no proprietary dependencies** : There must be no dependencies on proprietary cloud services, and it must be easily self-hostable in a range of environments, from local development to highly scalable distributed environments.
3. **No new database to operate, no stateful components** : Rivet is frequently self-hosted, so it runs exclusively on mainstream databases teams already know how to operate and adds no new stateful components.


**Scale:**


1. **Bottomless storage per SQLite db** : One of the best-known limitations of Durable Objects and D1 is their 10 GB database cap, meaning that if you have a customer’s database that hits that cap, they have a complete outage. We chose this as a constraint to avoid for Rivet Actors.
2. **Billions of databases per cluster** : Rivet Actors get created at very high frequency, for everything from as coarse as agent sessions to as granular as every user that visits a site. Creating a database must be instant, per-database overhead must be near zero, and a single cluster must comfortably hold billions of mostly idle databases.
3. **An idle database costs no resources** : When an Actor goes to sleep, the only resources its database takes should be disk or S3, not live resources like you get with a Postgres server.


###


The existing SQLite engine landscape


SQLite already has a rich ecosystem of replication and hosting options, but each category falls short of these constraints:


Category Projects Approach Why it doesn’t fit


Single-primary replication[Litestream](https://litestream.io/) ,[LiteFS](https://fly.io/docs/litefs/) One machine owns the writable database and streams changes out, to object storage (Litestream) or to read replicas (LiteFS) Commits are durable on only one machine before async replication, so a lost machine loses recent writes. Read replicas can fetch pages from S3 on demand, but the writable primary must hold the full database on its local disk and fully restore it before taking writes (violates 1, 2, 3, 6)


Full-copy clusters[rqlite](https://rqlite.io/) ,[dqlite](https://dqlite.io/) ,[cr-sqlite](https://github.com/vlcn-io/cr-sqlite) ,[Marmot](https://github.com/maxpert/marmot) Every node holds the full database, kept consistent with Raft (rqlite, dqlite) or merged eventually with CRDTs (cr-sqlite) or gossip replication (Marmot) Resident processes and a full copy per node cap the database at machine size and make billions of idle databases impossible (violates 2, 3, 6, 7, 8). The eventual-consistency options can also lose acknowledged writes on merge or node loss (violates 1)


Hosted server-side SQLite[Turso](https://turso.tech/) (libSQL) A hosted SQLite server that your app queries over the network. Its “diskless” architecture describes the server fleet: stateless SQLite servers caching over object storage Queries run against a remote server instead of in your process. Getting SQLite back in-process requires embedded replicas, which hold the full database on local disk and forward writes to the server (violates 2, 3, 6). When Rivet says zero-disk, it means the opposite: SQLite runs in-process and the database file never exists on the machine’s disk. Turso is an exciting project, but still early for us to bet all of the Rivet developers on it for now


Object-storage LSM[SlateDB](https://slatedb.io/) LSM tree that writes directly to object storage. Not SQLite itself, but it could sit under a SQLite VFS Durable commits wait on an object-storage PUT, putting writes in the 50 to 100 ms range on S3 Standard (violates 1). A single writer owns each database, and per-database manifest and compaction overhead makes billions of small databases impractical (violates 7). It’s also a pre-1.0, nonstandard database teams aren’t used to running (violates 5)


Distributed storage engine[mvSQLite](https://github.com/losfair/mvsqlite) Swap SQLite’s storage layer for a distributed database, keep SQLite itself unmodified This is the closest category to Rivet. mvSQLite is pre-1.0, requires operating FoundationDB plus its mvstore service (violates 5), and has no S3 tiering


###


Design implications


Together, these constraints imply five things about the design:


- **We can’t use a local disk because of elastic scaling:** Data stored on the machine running the Actor breaks zero-disk compute and instant startup.
- **Reads must be on demand:** A database can never be fully downloaded before serving, so pages must be fetched the instant they’re needed, with caching and prefetching to keep queries fast.
- **Writes must be redundant:** A commit that is durable on only one machine is not durable.
- **Cold data must move to cheap storage:** Billions of mostly idle, bottomless databases cannot all live on replicated fast storage, so data that goes cold has to be offloaded automatically.
- **We can’t write directly to S3:** A PUT request answers in roughly 30 to 45 milliseconds, far too slow for a commit inside a user-facing request, and paying for a request on every commit is excessively expensive. S3 Express One Zone is faster, but it is expensive and AWS-only, which breaks self-hosting.


##


The Rivet virtual filesystem and the hot tier


###


Database pages, a quick primer


Pages are what let Actors run with zero-disk compute without downloading the entire database, so they’re worth a quick primer.


A SQLite database is just a file on disk, and that file is broken down into **pages** . Each page is a 4 KiB block that holds rows, indexes, and metadata.


Every time you run a SQLite query, you are reading or writing database pages, which include the rows you’re working with.


Normally that SQLite file sits on a local disk. However, Actors follow a zero-disk architecture for elastic scaling, so page reads and writes need to go somewhere else.


###


Swapping out the filesystem


Instead of using a file on disk, Rivet uses a feature of SQLite called the[virtual filesystem](https://www.sqlite.org/vfs.html) (VFS). The VFS lets an application provide its own implementation of the page reads and writes that would normally go to the host filesystem. This is the hook that lets us swap out the storage medium entirely, which we’ll get to in a moment.


SQLite’s virtual filesystem interface boils down to a few calls:` xRead` ,` xWrite` , and` xSync` .


###


Achieving zero-disk compute for Actors


With the virtual filesystem, instead of needing the entire database to already sit on disk, pages are **fetched the instant they’re needed** . Combined with intelligent prefetching, this makes it possible to **instantly start an Actor** on a machine that does not already have its database, while caching keeps frequently read pages at **near-in-memory speed** .


Rivet’s VFS implements that interface against our storage layer instead of a disk, meaning that every page read and write becomes a function call we can control.


That storage layer is the **hot tier** : a separate set of machines that holds all actively used data, built for fast, durable reads and writes. It can run on RocksDB, Postgres, or FoundationDB depending on the deployment.


The Rivet VFS converts each of these operations to the corresponding operation on the storage layer. For example,` xRead` will read the appropriate page from the storage layer, and` xWrite` will write the dirty pages.


###


Scaling compute for Actors separately from storage


Now that we can start Actors without the SQLite database already sitting on disk, we can scale compute for Actors separately from storage.


That means that as Actors scale up and down, we can “hug the curve” with how much compute is allocated for running Actors, instead of having a fixed pool size. It also means a sudden surge of Actors can be absorbed efficiently, since new machines can start serving Actors immediately without moving any data.


###


Why zero-disk + elastic scaling matter so much


The most important constraint of this entire architecture is the combination of **zero-disk compute and elastic scaling** .


Rivet’s architecture is optimized for self-hosting. When developers building on Rivet see a huge surge in traffic, they have to scale up their Kubernetes (or equivalent) pods to meet demand and scale them back down when demand goes away. Scaling this elastically requires a stateless architecture. If Actors kept their data on the machines they run on, scaling up couldn’t rebalance existing Actors without moving their data, and scaling down would mean draining every database off a machine before it could be removed.


The Rivet virtual filesystem satisfies most of the eight constraints, except one: keeping every Actor’s data in the hot tier forever gets expensive.


##


Tiering to cold storage


###


The cost benefit of tiered storage


The hot tier is built for fast, cheap reads and writes, but at the cost of expensive storage.


Storage Read latency Write latency


Block storage (EBS gp3) ~1 ms ~1 ms


S3 Standard ~30 to 45 ms ~30 to 100 ms


This can be solved by **offloading data to S3 when it goes idle** , meaning that hot data can be stored in the faster, more expensive tier (RocksDB, Postgres, or FoundationDB) while data that is not frequently accessed can be offloaded to S3.


Storage $/GB-month 5 TB stored Write 1 GiB Read 1 GiB


Block storage, replicated 3x (EBS gp3) $0.24 $1,230 Included Included


S3 Standard, replication included $0.023 $118 ~$0.02 ~$0.002


*We estimate block storage at 3x because we assume triple redundancy for writes in the hot tier, while S3 Standard provides replication as part of its default pricing. The S3 write cost assumes 256 KiB objects, which we’ll come back to below.*


As the table shows, the hot tier costs **roughly 10x as much as S3** for storage. However, its reads and writes cost nothing per operation, while S3 bills for every request.


This makes the hot tier the right place for **data you read and write frequently** , while **less frequently accessed data can be offloaded to S3** automatically.


For example, data from use cases like agent sessions, audit logs, and edit histories frequently should not be preserved in the hot tier. This change enables those databases, or chunks of those databases, to be offloaded to the cold tier.


###


Grouping pages into chunks


To offload data, the storage layer needs a bigger unit than a 4 KiB page. S3 bills for every request, so uploading each page as its own object would mean **64x more PUT requests** , costing far more in requests than the storage itself.


To solve this, the storage engine groups pages into **chunks** : 256 KiB groups of 64 database pages. Chunks are the unit the storage engine tracks, caches, and offloads.


###


Moving idle data to S3


With pages grouped into chunks, tiering becomes simple: chunks that haven’t been touched in a while move out of the hot tier and into S3 in the background.


Tiering in the SQLite engine has two components:


- **The compactor** : finds chunks of Rivet databases that have not been touched for 7 days (configurable) and offloads them from the hot tier to S3.
- **Rehydration on read** : when cold data is read, it is rehydrated into the hot tier and sent to the VFS layer.


###


The three places a chunk can live


With the cold tier added, a chunk can now live in **three places** : the VFS’s in-memory chunk cache, the hot tier, or the cold tier on S3.


###


The life of a chunk


For example, when SQLite writes page 123, that write lands in chunk 1 (chunks hold 64 pages) and is written to the hot tier. The compactor later sees chunk 1 has not been touched for 7 days and writes it to S3. When SQLite later reads page 123, the storage layer sees chunk 1 is not in the hot tier, pulls it from S3 back into the hot tier, and returns the page to SQLite.


###


Bonus: backups and point-in-time recovery, for free


In addition to being a cost-efficient, scalable way of storing Actor data, S3 is also an industry-standard place for backing up databases. This means the cold tier in Rivet **doubles as a backup mechanism** for the database.


Rivet also provides **point-in-time recovery** with configurable granularity and retention, which lets you recover a database to a point in time from S3. Point-in-time recovery is optional, as it does require more storage.


##


Eight constraints, satisfied


Putting it all together, here’s how the storage engine holds up against the eight constraints we started with:


1. **Fast, durable writes** : commits go to the replicated hot tier and return in low milliseconds. S3 never sits on the write path.
2. **Instant startup with no local data** : any machine can serve any Actor by fetching pages. No volume attach, no restore.
3. **Zero-disk compute for elastic scaling** : machines running Actors hold no state, so compute scales up and down freely while storage stays put.
4. **Self-hostable, no proprietary dependencies** : a pluggable hot tier (FoundationDB, Postgres, RocksDB) and an S3-compatible cold tier.
5. **No new database to operate, no stateful components** : the hot tier runs on databases teams already operate, the cold tier is any S3-compatible store, and the engine itself holds no state of its own.
6. **Bottomless storage per SQLite db** : cold chunks offload to object storage with no hard database ceiling.
7. **Billions of databases per cluster** : a database is rows in a shared storage layer, not a volume plus a filesystem plus a process.
8. **An idle database costs no resources** : no process, no CPU, no memory, just storage, and the cold majority of it at S3 prices.


##


FAQ


**Can this SQLite layer be used without Rivet Actors?**


We believe Rivet Actors are the best way to work with server-side SQLite, since Actors provide single-writer semantics at their core and provide the stateful architecture needed to run SQLite. That said, the core components are trivial to split out, since the engine already supports native SQLite. It would require splitting out the internal depot and depot-client libraries and running the protocol over a new network transport. Please let us know if you do this. We would love to support this use case.


**How does it handle multiple writers?**


It doesn’t. Actors, and by association SQLite, are **single-writer by design** : there can only be one instance of a given Actor running at a time. As a fallback, there is a guard preventing dual writes. This is enforced structurally, unlike replication tools such as Litestream that only detect conflicting writers after the fact.


**How is this different from Cloudflare Durable Objects?**


Durable Objects also pair each object with its own SQLite database, and their durability is also off-machine: writes are replicated across their network before clients see a response and stream to object storage. The differences are placement and scaling. A Durable Object is pinned to the data center where it was created and never moves, with SQLite running on the same machine as the object. That works because Cloudflare provisions a giant fleet ahead of demand, and any one customer’s surge is absorbed across the millions of tenants sharing it. Rivet is built for self-hosting, where a surge means adding and removing your own machines, so Actors must be able to start on any machine with no data on it. Pinning also means each database is served by exactly one machine and has to restore quickly on failover, which is why Durable Objects cap databases at 10 GB while Rivet’s are bottomless.


**Doesn’t Turso already have a diskless S3 architecture?**


Yes, but it means something very different from what we mean. When we say zero-disk, we mean you don’t need the SQLite file on the same machine as the one running it, while still getting native SQLite performance. Turso has two modes of operation: you can submit queries to their servers, which incurs a network hop for every query, or you can run an embedded replica, which requires syncing the full database locally before you can use it.


**You can run SQLite on Postgres?**


Yes. Rivet’s storage layer is meant to be flexible enough to run anywhere. Most teams know how to operate Postgres or already have a managed Postgres deployment, so it’s plug and play.


**Aren’t reads over the network slow?**


Uncached reads do go over the network. But most cloud VMs today already sit on a network-attached block device like EBS, so uncached reads were going over the network anyway. Caching and prefetching keep the hot path in memory.


**Why build a custom VFS instead of using a FUSE filesystem?**


A FUSE filesystem would let us mount remote storage as a regular filesystem and run stock SQLite on top of it, with no VFS needed. We didn’t for two reasons:


- FUSE requires custom configuration: a kernel module, privileged mounts, and access to` /dev/fuse` , which aren’t available in many of the environments Rivet runs in.
- SQLite does not run well on FUSE without lots of work specific to SQLite. By default, SQLite fetches pages serially, so every page miss pays a full network hop one at a time, and a filesystem has no visibility into query patterns to prefetch around it. The VFS sits at the right layer to batch, cache, and prefetch.


##


Get started


SQLite tiered storage is available today. Get started by installing RivetKit:


```text
npm   install   rivetkit


```


- [SQLite documentation](https://rivet.dev/docs/actors/sqlite)
- [GitHub](https://github.com/rivet-dev/rivet)
- [Discord](https://rivet.dev/discord)
