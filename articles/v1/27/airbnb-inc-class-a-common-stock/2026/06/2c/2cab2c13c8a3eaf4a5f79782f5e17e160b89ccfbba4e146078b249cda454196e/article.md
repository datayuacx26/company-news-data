---
schema_version: "1.0.0"
document_id: "2cab2c13c8a3eaf4a5f79782f5e17e160b89ccfbba4e146078b249cda454196e"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-af0970d10a00"
canonical_url: "https://airbnb.tech/infrastructure/sitar-agent-building-a-reliable-dynamic-configuration-sidecar-at-scale/"
published_at: "2026-06-04T21:06:52+00:00"
first_seen_at: "2026-07-20T03:32:35.357269+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:e0db2c553fe25b3475a1b9d2c1a502abde855f9dc1c1c4b2edaae21e67a8a982"
---

# Sitar-agent: Building a reliable dynamic configuration sidecar at scale

**By** :[Bo Teng](https://www.linkedin.com/in/bo-t-b04912238/) ,[Cosmo Qiu](https://www.linkedin.com/in/cosmo-qiu/) ,[Siyuan Zhou](https://www.linkedin.com/in/siyuan-zhou-85ba8057/) ,[Ankur Soni](https://www.linkedin.com/in/ankursoni/) ,[Xin Huang](https://www.linkedin.com/in/xin-huang-b21588a2/) ,[Willis Harvey](https://www.linkedin.com/in/willish/)


## Introduction


In our[previous post](https://medium.com/airbnb-engineering/safeguarding-dynamic-configuration-changes-at-scale-5aca5222ed68) , we explored Airbnb’s dynamic configuration system, Sitar, with a focus on service architecture and configuration change safety. Now for the harder question: once a config change is committed, which happens several times each minute, how does it actually reach the thousands of Airbnb’s service instances reliably, quickly, and without redeploying the services?


This post describes sitar agent: a lightweight Kubernetes sidecar that runs alongside every subscribed service pod, continuously synchronizing the latest configurations from the service backend and making them available on the local filesystem for reads. In this post, we will first go through the configuration delivery life cycle, and then discuss some key design choices for the sitar-agent sidecar.


## Config delivery life cycle


The diagram below illustrates the end-to-end journey of a configuration change, from the developer-facing layer to the production service fleet.


Sitar config delivery lifecycle


**Step 1 — Config creation/update**


Developers create or update configuration values through either Git flow or the web UI. These changes are committed to the Sitar Service, where they are stored with full versioning, change logs, and ACL enforcement.


**Step 2 — Hourly snapshot upload**


The Snapshot Service periodically packages the full state of all config groups and uploads compressed snapshots to AWS S3.


**Step 3.1 — Preload snapshot from S3 (on pod startup)**


When a production service pod starts, the sitar-agent sidecar runs first. It downloads the latest snapshot for each subscribed tenant’s configs from S3 to the mounted disk (shared between sitar-agent and the main container). This allows the agent to bootstrap from a known-good state without fetching every config from the Sitar Service from scratch on every restart. Preloading the snapshots from S3 enables faster restarts, makes the service resilient to transient Sitar Service unavailability, and avoids load spikes during deployments.


**Step 3.2 — Preload latest config from Sitar Service (on pod startup)**


After loading the S3 snapshot, the agent performs an initial sync with the Sitar Service to catch up on any changes published since the last snapshot. Once this step succeeds, the agent signals readiness, unblocking the application main container from starting.


**Step 4 — Periodic update**


After startup, the agent enters a continuous polling loop (order of seconds with jitter). On each cycle, the sitar agent queries the Sitar Service for changes across all subscribed groups.


**Step 5 — Read config**


The application main container reads configurations from the mounted disk through the Sitar client library, which maintains an in-memory cache. The client detects file changes and refreshes its cache transparently.


With the delivery lifecycle in mind, the following sections walk through the major architectural choices that shaped the sidecar’s design.


## Key design decisions


In 2024, the sitar-agent underwent a full rewrite from Ruby to Java, Airbnb’s mainstream JVM language, giving the team an opportunity to modernize the architecture alongside the language migration. The snapshot-based S3 preload introduced in the previous section is one outcome of this effort: it dramatically reduces cold start time for the pod and decouples startup reliability from Sitar Service availability. The rewrite also led to several other deliberate design decisions around reliability, performance, and operational safety. The sections below walk through each of these choices.


## Requirements for the Sitar System


Before diving into specific design choices, it helps to understand the constraints that shaped every decision. At Airbnb, dynamic configuration delivery isn’t just a convenience: it controls critical features across thousands of services. That means configs must always be available, even when the Sitar Service itself is down; a slightly stale value is tolerable, but an unreadable config is not. At the same time, when an engineer pushes a change, it needs to reach every subscribed service within tens of seconds, not minutes. Making that work at scale is non-trivial: with tens of thousands of pods fetching updates simultaneously, the system has to absorb that load without degrading. And since Airbnb’s service fleet spans Java, Python, Go, Typescript, and Ruby, the solution needs to serve all of them, ideally minimizing the effort of maintaining separate per-language implementations.


The above requirements for reliability, performance, scalability, and multi-language support aren’t independent. As you’ll see, most of our design decisions, described below, come back to balancing one against another.


## Main container vs sidecar


The question of whether sitar-agent should run as a sidecar container or a process in the main container surfaced as a key architectural decision during the Java rewrite. We evaluated the pros and cons of each option as follows:


**Pros of moving to the main container:**


- Cost reduction. This is the main driver for moving to the main container: running the agent as a library eliminates the per-pod JVM overhead, allowing memory and CPU to be shared with the main container.
- Reduced operational surface. One fewer container means one fewer component for service owners to configure and tune. However, this advantage weakens when considering Airbnb’s multi-language service fleet.


**Cons of moving to the main container:**


- Multi-language complexity. Airbnb service languages span Java, Python, Go, Typescript, and Ruby. A library approach would require the existing sidecar logic to be implemented in all languages, significantly increasing development and maintenance effort.
- No isolation. Bugs or resource spikes in sitar logic can crash or starve the main container, and vice versa. This coupling increases incident blast radius and complicates resource attribution during debugging.
- Operational noise. Having the logs for Sitar and its cpu/memory usage mixed with the main process logs and its metrics makes it harder to debug both sitar and main process issues.
- Optimizability. Having a separate container allows the container to be optimized for its purpose, and eases testing and debugging.


**Decision:**


Despite the cost savings and reduced operational surface which would result from moving the sitar-agent logic to the main container, the projected savings were insufficient to justify the tradeoffs in reliability and operational overhead, and the development overhead of supporting the sidecar logic in multiple languages. We therefore decided to maintain the sitar-agent as an isolated sidecar container.


## The pull model and server-side optimization


Sitar-agent fetches configuration updates by polling the Sitar service every 10 seconds. This is a pull model: the agent drives the update cycle by periodically asking the server for changes. This pull-based architecture, while being simple and easy to maintain, generates unnecessary load on the server when there is no update needed.


A push-based architecture change can greatly reduce the server-side load and change propagation time, at the expense of a more complicated architecture. In order to keep the current simple architecture while reducing the service-side load, the sitar system implements the following optimizations:


1. Since the sitar config is mostly changed manually, which takes longer than several seconds, a slight delay in config update delivery is acceptable. Therefore, a server-side cache with a short TTL (10s) is a great way to reduce sitar server-side processing. Most of the sitar-agent calls to services hit the cache layer without triggering heavy server-side compute or database access, thus greatly reducing the resource usage of handling requests.
2. When there is a cache miss and the request actually triggers database access, it passes along a token (last scanned db row), which tells the service to skip scanning for changes before the last fetch, thus greatly reducing server-side processing and database access time during each periodic pull.


Given the above optimizations, the sitar-service can scale and perform quite well in handling the pull request from all service pods at Airbnb, and we can preserve the simple, stateless server-with-pull architecture.


**Decision:**


For sitar’s use case, polling latency on the order of seconds is acceptable; dynamic config is not a real-time signaling mechanism, and most config changes are manual, making a few seconds of propagation delay inconsequential. The pull model’s stateless simplicity is a strong operational advantage at Airbnb’s scale. The team elected to keep the pull model and invest instead in reducing per-poll cost.


## Local datastore selection


Sitar-agent maintains a local on-disk key-value store that the main container reads from. The legacy datastore is a[Sparkey](https://github.com/spotify/sparkey) -backed[internal implementation](https://medium.com/airbnb-engineering/hammerspace-persistent-concurrent-off-heap-storage-3db39bb04472#.stk4bq2lo) , with a thin layer around the Sparkey datastore for concurrent coordination. As the usage of Sitar continues to grow and evolve, the mismatch of the Sparkey-backed datastore and sitar’s needs have become evident:


- Sparkey is purpose-built for write-once, read-many workloads with no support for multi-thread read-write coordination. This requires a wrapper around the Sparkey datastore for concurrent coordination to support sitar’s frequent write to the datastore, adding to complexity and potentially becoming a source of latent bugs.
- Sparkey doesn’t include native concurrency support by design, and we needed an external locking mechanism that locks the entire datastore file on write. As update frequency increased across the datastore, this lock contention began to limit concurrent read/write performance.
- Since Sparkey’s design requires re-indexing of the entire datastore on each write, writing frequently to the Sparkey backed datastore became increasingly expensive. However, as Sitar has become widely used across almost all Airbnb services, the write to the datastore is very frequent; we see updates in configs in almost every pull cycle (every ~10 seconds)
- Sparkey has limited multi-language support: it does not have implementations in all languages Airbnb services require, and supporting all languages in Airbnb would require complex interop.


The team evaluated and benchmarked two candidates to replace the legacy Sparkey-based datastore: SQLite and RocksDB. A matrix of experiments were run across varying dataset sizes, read QPS, and memory allocations, fixing two of the three dimensions and varying the third in each run. We also researched community support, open source activity, supported languages, and adoption breadth of both. The following summarizes our findings:


**SQLite:**


**Pros:**


- Mature, widely-adopted library with officially maintained bindings for Java, TypeScript/Node.js, Python, Go and Ruby: all languages used by sitar’s service consumers.
- Built-in write-ahead logging (WAL) mode supports concurrent reads during writes, eliminating the need for a custom concurrency wrapper.
- Simple operational model: a single file, no background compaction or tuning required.
- Read and write performance is dramatically better than the Sparkey-backed datastore, and sufficient for sitar’s workload.


**Cons:**


- Read latency is 2–3x slower than RocksDB, and increases linearly with data size.
- Write latency also increases with larger data sizes.


**RocksDB:**


Pros:


- Best raw read/write performance across all test dimensions.
- Consistent read high-QPS performance; tested up to 1500 ops/sec with minimal degradation.


Cons:


- A more complex operational model; requires tuning of compaction, block cache, column families, and memory settings.
- The multi-language library ecosystem is less mature and less uniformly maintained than SQLite’s.
- Higher operational burden for a team without deep RocksDB expertise.


**Decision:**


In our tests, both RocksDB and SQLite significantly outperform Sparkey-backed datastores for our workload across all three test dimensions: data size, memory allocation, and read QPS. While RocksDB delivers better raw performance, sitar-agent’s workload operates comfortably within SQLite’s envelope. SQLite’s first-class multi-language library support, native WAL-based concurrent access model, and simpler operational footprint made it the better overall fit for a team supporting multiple language runtimes. The team selected SQLite as the replacement for the Sparkey-backed datastore.


**Safe migration from Sparkey to SQLite**


Operational safety was a top priority. Beyond extensive testing, we also we relied on two mechanisms to keep the rollout safe:


1. **Shadow reads** : Before migrating each service, we ran a shadow read-and-compare phase; services continued reading from Sparkey while SQLite results were fetched in parallel for validation.
2. **Feature flag-gated gradual rollout** : We migrated incrementally, starting from the least critical services and progressing toward the most critical. Some critical Tier 0 services were onboarded last, with dedicated coordination at each step.


## Conclusions


Sitar-agent sits at the core of Airbnb’s dynamic configuration delivery system. This post walked through how it works and the key tradeoffs we navigated during the Java rewrite: between cost and isolation, simplicity and push-based efficiency, and raw performance and operational practicality. Every decision came back to the same constraints: configs must always be available, changes must propagate quickly across a fleet of tens of thousands of pods, and the solution must work across Airbnb’s polyglot service stack without compounding the maintenance burden.


If this type of work interests you, check out some of our[related positions](https://careers.airbnb.com/) !


## Acknowledgments


Our progress with Sitar would not have been possible without the support and contributions of many people. We’d like to thank Craig Sosin, Nikolaj Nielsen, Daniel Fagnan, Alex Edwards, Nick Morgan, Carolina Calderon, Hanfei Lin, Yunong Liu, Lucas Rosa Galego, Yann Ramin, Denis Sheahan, Richa Khandelwal, Swetha Vaidy, Adam Kocoloski, Adam Miskiewicz, and all the other engineers and teams at Airbnb who joined design reviews and offered valuable feedback, as this work would not have been possible without them.


*All product names, logos, and brands are property of their respective owners. All company, product, and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
