---
schema_version: "1.0.0"
document_id: "841afdbc8ad3196f4f71540094f227e03c2c29b0c526d10c1f873b0f1d5c6e44"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/postgresql-ha-kubernetes/"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:9bbf1829f5cdb638baec3a9c559949378ea4bef0573ef23172bf594a2ed38f09"
---

# When failover isn’t safe: Building high-availability PostgreSQL on Kubernetes

Shree Sampath


Gamedays are one of the most effective ways we proactively uncover gaps in our systems and processes. At Datadog, we regularly run a variety of gamedays to intentionally stress our platforms and learn how our systems and teams respond under real-world conditions. These exercises help us surface hidden vulnerabilities, strengthen our operational readiness, and continually raise the bar for our infrastructure.


During one such gameday, a simulated zonal failure introduced targeted disruptions in an availability zone on a staging environment by inducing network latency, which exposed a weakness in our PostgreSQL architecture. Several of our Kubernetes-based PostgreSQL clusters had primary or writer nodes running in the affected availability zone. As network latency spiked, those primaries could no longer communicate reliably with their replicas. Replication lag quickly grew, writes stalled, and applications began serving stale data. Because no replica was sufficiently up to date, failover wasn’t safe and the clusters were effectively stuck.


We rely on PostgreSQL as the backend database for many Datadog products, and this architecture has served us well under normal conditions. But the gameday revealed an uncomfortable truth: In the face of certain network failures, our setup prioritized **availability** over **durability** in ways that left us with no safe recovery path.


In practice, this meant the primary continued accepting writes even while replication to replicas was delayed due to elevated network latency. The system remained writable, but replication lag continued to grow, and replicas drifted further behind the primary. As a result, failover candidates could no longer be promoted safely without risking data loss. We were left with only one viable option: wait for latency to subside and for replicas to catch up.


We set out to fix this failure mode. Our goal was to make failover both automatic and safe, without compromising PostgreSQL’s performance characteristics more than necessary. To do this, we rearchitected our PostgreSQL deployment to use synchronous replication for failover candidates, coordinated by[Patroni](https://github.com/zalando/patroni) , an open source high-availability manager.


In this post, we’ll walk through how we redesigned our Kubernetes-based PostgreSQL clusters for failover safety, how we balanced durability against latency, and what we learned while validating this approach through benchmarking and failure testing.


## PostgreSQL on Kubernetes: Our baseline architecture


Our Kubernetes-based PostgreSQL clusters are organized into two main pools: a **leader pool** and a **read replica pool** . In our architecture, PostgreSQL is a single-writer system, and this separation lets us scale reads independently without overwhelming the leader with a mix of reads and writes. As a result, we can increase read capacity as demand grows while keeping write latency predictable and stable.


The leader pool consists of a single active writer node that handles all write operations, along with two standby nodes. These standbys do not serve application traffic, but they can be promoted if the leader becomes unavailable.


The read replica pool includes multiple nodes that handle read-only traffic. These replicas are optimized for read scalability and query isolation, so they are intentionally excluded from failover.


This design worked well under normal operating conditions, but as we discovered during a zonal failure, it also imposed strict limits on which nodes could safely take over when the leader was impaired.


PostgreSQL high-availability architecture on Kubernetes using Patroni and ZooKeeper. The leader pool manages write operations, while read replicas serve read-only traffic. ZooKeeper coordinates leader election and ensures consistent cluster state. PostgreSQL high-availability architecture on Kubernetes using Patroni and ZooKeeper. The leader pool manages write operations, while read replicas serve read-only traffic. ZooKeeper coordinates leader election and ensures consistent cluster state.


We use **Patroni** to manage replication, failovers, and leader elections for our PostgreSQL clusters. Patroni relies on a **distributed configuration store (DCS)** —in our case, **ZooKeeper** —to coordinate leader election, maintain a shared view of cluster state, and enforce a single active leader at any point in time.


ZooKeeper stores metadata, including the current leader key/lock, cluster configuration, and each member’s replication state, such as its latest log sequence number (LSN). Patroni uses this information to make conservative decisions about promotion and demotion, prioritizing data consistency over aggressive failover.


When a new node joins the cluster, it first checks ZooKeeper to determine whether a leader already exists. If no leader is present, the node attempts to acquire the leader key by creating an ephemeral znode. ZooKeeper guarantees only one node can acquire this leader key, which prevents multiple primaries from forming. If a leader already exists, the joining node configures itself as a replica and starts streaming replication.


During a network partition, this caution becomes especially important. A replica that loses connectivity to either the leader or ZooKeeper cannot reliably determine the cluster’s current state. Rather than risk an unsafe promotion, Patroni pauses or demotes the affected node until leadership can be verified.


Similarly, if the leader loses connectivity, Patroni coordinates with ZooKeeper to ensure that only a single, eligible standby can acquire the leader lock. This process guarantees that failover happens in a controlled way, even under partial network failure. The following diagram shows how Patroni safely promotes a new primary during a network partition and how the original leader demotes itself after failing to reacquire the leader lock once connectivity is restored.


Sequence of events during a network partition in a Patroni-managed PostgreSQL cluster. When the primary loses connectivity, ZooKeeper releases the expired leader key, allowing an eligible standby to acquire the lock and become the new primary. Once the original primary regains connection, it demotes itself to standby to prevent split brain and rejoins replication. Sequence of events during a network partition in a Patroni-managed PostgreSQL cluster. When the primary loses connectivity, ZooKeeper releases the expired leader key, allowing an eligible standby to acquire the lock and become the new primary. Once the original primary regains connection, it demotes itself to standby to prevent split brain and rejoins replication.


## Why our architecture couldn’t fail over safely


Our PostgreSQL architecture uses a single-writer model: Only one leader node accepts writes. During a failure, Patroni is responsible for electing a new leader from among healthy standby nodes.


To protect against data loss, Patroni performs a series of safety checks before promoting a standby. One of the most important is verifying that **replication lag** is within an acceptable threshold, configured through the` maximum_lag_on_failover` parameter. If a standby has fallen behind the leader, promoting it could result in missing or inconsistent data.


This safeguard became the limiting factor during our gameday. When the primary node lost connectivity, all available standbys had accumulated replication lag beyond the configured threshold. Because no standby was sufficiently up to date, Patroni correctly rejected the failover attempt. The cluster remained without a safe writable primary not due to Patroni, but because there was no safe promotion candidate.


The following diagram illustrates how Patroni evaluates replication lag during failover and why it refuses promotion when all standbys exceed the` maximum_lag_on_failover` limit.


Patroni checks replication lag before promoting a standby. If the lag on all standby nodes exceeds the configured maximum_lag_on_failover threshold, failover is rejected to prevent data loss. Patroni checks replication lag before promoting a standby. If the lag on all standby nodes exceeds the configured maximum_lag_on_failover threshold, failover is rejected to prevent data loss.


To improve the availability of our PostgreSQL clusters, we revisited our replication strategy, specifically the two modes supported by PostgreSQL’s streaming replication.


In streaming replication, the leader continuously streams write-ahead logs (WALs) to its replicas. These logs capture all changes to the database. Replicas stay in sync by applying the WALs locally.


PostgreSQL supports two modes of streaming replication: **asynchronous replication** , which is the default, and **synchronous replication** .


### Asynchronous replication (default)


In our original setup, PostgreSQL used asynchronous replication. In this mode, the leader does not wait for acknowledgment from replicas before committing a transaction.


This configuration minimizes write latency and supports high-throughput workloads. However, if the leader fails, transactions that were committed on the primary but not yet replicated to a standby can be lost during leader promotion.


### Synchronous replication


PostgreSQL also supports synchronous replication. In this mode, the leader waits for acknowledgement from at least one replica before sending the transaction response to the client. This significantly reduces the risk of at least one replica drifting too far behind the primary and provides stronger durability guarantees compared to asynchronous replication, since committed transactions are confirmed to exist on another node before the client sees a successful response.


With synchronous replication, failover candidates are more likely to be up to date, and a standby can be promoted without risking data divergence.


### Our hybrid replication setup


To balance durability, latency, and throughput, we adopted a hybrid replication model:


-


**Standby nodes in the leader pool participate in synchronous replication.** This allows the leader to wait for confirmation from designated synchronous standbys before committing writes.


-


**Read replicas continue to use asynchronous replication.** They serve read-only traffic and are not considered for failover, which helps limit replication overhead on the leader pool.


This approach lets us apply stricter durability guarantees to failover candidates without imposing the same latency costs on read replicas.


## How we tuned PostgreSQL and Patroni for safe failover


Enabling synchronous replication required changes in both PostgreSQL and Patroni, which manages leader election and failover for our clusters.


The following table summarizes the parameters we adjusted, the systems that manage them, and the values we set.


Parameter Description Default Managed by Required or optional Value we set


` **synchronous_mode**` Enables Patroni’s synchronous replication. When` true` , the leader waits for confirmation from synchronous standby(s) based on` synchronous_node_count` value before committing.` false` Patroni Required` true`


` **synchronous_node_count**` Number of synchronous standby nodes. Patroni uses this to generate the` **synchronous_standby_names**` list.` 1` Patroni Optional` 1`


**` synchronous_mode_strict`** Enforces strict synchronous mode. When` true` and if no replicas (read replicas or standbys) are available, the leader blocks writes instead of switching to asynchronous mode.` false` Patroni Optional` true`


**` synchronous_commit`** PostgreSQL setting for commit durability.` on` PostgreSQL Optional` remote_apply`


After applying these changes, the leader node sends the transaction response to the client only after a synchronous standby has confirmed receipt and application of the data.


Patroni configuration enabling strict synchronous replication, with synchronous_commit set to remote_apply to ensure writes are applied on a standby before commit. Patroni configuration enabling strict synchronous replication, with synchronous_commit set to remote_apply to ensure writes are applied on a standby before commit.


Patroni designates a synchronous standby that must acknowledge writes, ensuring a failover candidate is always up to date. Patroni designates a synchronous standby that must acknowledge writes, ensuring a failover candidate is always up to date.


### Balancing durability and latency


Synchronous replication improves data durability, but it comes at a cost. Because the leader waits for confirmation from a synchronous standby before sending the transaction response to the client, write latency increases and write throughput can be affected under sustained load.


The performance impact depends on two main factors:


-


The number of synchronous standbys` synchronous_node_count`


-


The level of write durability configured through` synchronous_commit`


PostgreSQL provides several levels of durability through the` synchronous_commit` parameter, each with different latency and risk trade-offs.


Value What it does Trade-off


**` remote_apply`** The leader waits for the replica to write, flush, and replay the WAL changes. Strongest consistency, highest latency.


` **on(remote_flush internally)**` The leader waits for the replica to flush WAL to disk. Strong durability, but data is not yet readable on the standby.


**` remote_write`** The leader waits for WAL to reach the replica’s OS cache (not disk). Lower latency, but vulnerable to OS crashes.


**` local`** The leader commits after the local disk flush without waiting for a standby. No cross-node durability guarantee.


**` off`** The leader commits before local WAL flush. Lowest latency, highest risk of data loss.


Choosing the right option depends on tolerance for risk and performance requirements. To make an informed decision, we benchmarked each mode in our environment to measure real-world latency impact. These results helped us balance durability guarantees with operational costs and select the safest configuration that met our performance targets.


## Benchmarking synchronous replication performance


Enabling synchronous replication adds latency because each commit must wait for acknowledgement from one or more standby nodes. To quantify this impact across different` synchronous_commit` modes, we ran benchmarks using[pgbench](https://www.postgresql.org/docs/current/pgbench.html) , PostgreSQL’s standard load-testing tool. The Patroni version used in this benchmarking is 3.2.1.


We used the TPC-B transaction suite, which simulates a mix of simple read and write transactions. For each test, we measured two key metrics:


-


**Average latency:** Mean time (in milliseconds) to process each transaction


-


**Transactions per second (tps):** Number of completed transactions per second, excluding connection setup time


### Test parameters


To simulate realistic production-like conditions, we varied:


-


**Scale factor:** Increased to evaluate performance across different database sizes


-


**Number of clients:** Adjusted to simulate different levels of concurrent user traffic


-


**Thread count:** Tuned to reflect available CPU and test parallelism


-


**Number of transactions:** Varied to observe behavior at different workload intensities


**Note:** We did not test synchronous replication with quorum commit mode as part of this benchmark.


### What the benchmarks showed


Across both metrics, stronger durability guarantees came with a performance cost.


#### Write latency impact


The **percentage increase** in average latency for different` synchronous_commit` modes was:


-


53% for` remote_apply`


-


46% for` on`


-


38% for` remote_write`


-


32% for` local`


#### Transaction throughput impact


The **percentage reduction** in transaction throughput (` tps` ) was:


-


34% for` remote_apply`


-


31% for` on`


-


27% for` remote_write`


-


23% for` local`


Stronger durability guarantees increase write latency, with remote_apply incurring the highest cost. Stronger durability guarantees increase write latency, with remote_apply incurring the highest cost.


Throughput increases as durability guarantees are relaxed, with local delivering higher transaction rates. Throughput increases as durability guarantees are relaxed, with local delivering higher transaction rates.


These results matched our expectations: **Stronger durability comes with measurable performance costs** .` remote_apply` , which waits for replicas to replay and apply WALs, consistently showed the highest latency and lowest throughput. This mode also provides the strongest consistency guarantees, which made it suitable for safe failover in our setup.


After benchmarking, we deployed` remote_apply` across several high-write clusters. Despite the additional replication coordination, we did not observe a significant impact on application-level write latency or throughput, even under sustained production load.


To mitigate the performance risks associated with synchronous replication, we rolled out the feature gradually across datacenters and workload tiers, with bake-in periods and continuous monitoring between stages. For example, a high-throughput resource-processing workload continued operating without observable processing lag or downstream backlog after synchronous replication was enabled, despite an increase in database write latency. Additionally,` synchronous_commit` can be adjusted instantly using` patronictl edit-config` without requiring downtime, giving us the flexibility to quickly reduce commit durability for extremely high-throughput workloads if needed.


## Validating failover through failure scenarios


To validate our high availability setup, we tested how Patroni behaves under different failure scenarios. These tests were designed to validate that our configuration, with synchronous replication and strict failover controls, could protect data integrity, prevent split-brain, and recover automatically from unexpected outages.


### Scenario 1: Loss of a synchronous standby


When a synchronous standby fails, Patroni attempts to maintain synchronous replication by assigning another eligible standby.


The leader node’s Patroni process monitors` pg_stat_replication` to detect dropped, stalled, and lagging streaming connections and relies on ZooKeeper to track the membership status of replicas. If a synchronous standby is lost, Patroni recalculates the list of healthy and eligible streaming replicas and updates PostgreSQL’s` synchronous_standby_names` based on` synchronous_node_count` . This allows the cluster to continue operating with synchronous replication enabled.


Patroni dynamically assigns a synchronous standby to ensure at least one replica is available to acknowledge writes. Patroni dynamically assigns a synchronous standby to ensure at least one replica is available to acknowledge writes.


Patroni reassigns the synchronous standby to a healthy replica, ensuring a failover candidate remains up to date. Patroni reassigns the synchronous standby to a healthy replica, ensuring a failover candidate remains up to date.


### Scenario 2: Loss of all synchronous standbys


When all nodes participating in synchronous replication become unavailable, Patroni’s behavior depends on the value of synchronous_mode_strict.


#### Non-strict mode: Favoring write availability


In non-strict mode, Patroni temporarily disables synchronous replication by clearing the` synchronous_standby_names` parameter. The leader reverts to asynchronous mode, allowing writes to continue without **** synchronous **** replication guarantees until a healthy replica rejoins.


With only the leader remaining and no eligible standbys, the cluster cannot fail over and may block writes in strict synchronous mode. With only the leader remaining and no eligible standbys, the cluster cannot fail over and may block writes in strict synchronous mode.


With no eligible synchronous standbys, Patroni removes the synchronous configuration and falls back to asynchronous replication to keep writes available. With no eligible synchronous standbys, Patroni removes the synchronous configuration and falls back to asynchronous replication to keep writes available.


PostgreSQL query confirming synchronous_standby_names is empty, indicating the cluster has fallen back to asynchronous replication. PostgreSQL query confirming synchronous_standby_names is empty, indicating the cluster has fallen back to asynchronous replication.


#### Strict mode: Blocking writes to preserve safety


In strict mode, Patroni sets` synchronous_standby_names` to` *` . This allows PostgreSQL to continue accepting and locally committing write transactions even if no replicas are explicitly configured as synchronous standbys, but client responses remain blocked until a replica acknowledges the WAL. When a suitable replica that can join synchronous replication reconnects, Patroni assigns it the synchronous standby role.


Replication lag makes all standby nodes ineligible for synchronous replication, leaving the cluster without a safe failover candidate. Replication lag makes all standby nodes ineligible for synchronous replication, leaving the cluster without a safe failover candidate.


In strict mode, Patroni sets synchronous_standby_names to *, allowing any available replica to act as a synchronous standby and avoid blocking writes. In strict mode, Patroni sets synchronous_standby_names to *, allowing any available replica to act as a synchronous standby and avoid blocking writes.


PostgreSQL query confirming synchronous_standby_names is *, confirming that any standby server that connects will be treated as a potential synchronous standby. PostgreSQL query confirming synchronous_standby_names is *, confirming that any standby server that connects will be treated as a potential synchronous standby.


### Scenario 3: Unavailability of all standby and replica nodes


If all replica nodes are unavailable and` synchronous_mode_strict = true` , PostgreSQL stalls transaction confirmations until at least one eligible replica comes back online. This behavior preserves data consistency but results in temporary write unavailability at the application level.


With no active replication connections, the cluster has no available standbys and cannot fail over safely. With no active replication connections, the cluster has no available standbys and cannot fail over safely.


With no synchronous standby available, writes block waiting for replication; canceling them risks data loss. With no synchronous standby available, writes block waiting for replication; canceling them risks data loss.


### Scenario 4: Leader failure during synchronous commit


This edge case occurs when the leader is waiting for confirmation from a synchronous standby and is interrupted before the acknowledgment is received.


Common causes include:


-


A client canceling a transaction mid-commit


-


A crash or termination of the leader PostgreSQL process


-


A network partition during the commit phase


In these cases, PostgreSQL may flush the WAL locally but fail to replicate it to a standby. Because no acknowledgment was received, the transaction remains invisible to replicas. If the leader crashes before the WAL is replicated to a synchronous standby, and a failover promotes that standby, the transaction can be lost. At that point, the old leader and the newly promoted primary can have diverged histories. To rejoin the cluster as a standby, the old leader uses pg_rewind, which identifies the point where the timelines diverged and rewinds the old leader’s data directory to match the new primary’s timeline, discarding local changes that were never replicated.


Under load, writes stall waiting for synchronous replication, causing transactions to abort and the workload to fail. Under load, writes stall waiting for synchronous replication, causing transactions to abort and the workload to fail.


During failover, Patroni promotes an eligible node to leader while preventing other replicas from being promoted. During failover, Patroni promotes an eligible node to leader while preventing other replicas from being promoted.


After failover, a new leader is established with an up-to-date synchronous standby, confirming a consistent cluster state. After failover, a new leader is established with an up-to-date synchronous standby, confirming a consistent cluster state.


Patroni does not manage this transaction-level behavior. It results from PostgreSQL’s internal handling of synchronous commits. This scenario highlights why quorum settings and acknowledgment mechanics need to be carefully tuned and monitored.


### Scenario 5: ZooKeeper unavailability


Patroni relies on ZooKeeper to coordinate leader elections and maintain cluster state. When ZooKeeper becomes unavailable, Patroni can no longer verify leadership or initiate new elections. To prevent data inconsistency, it falls back to conservative behavior.


#### What happens


When the current leader loses connectivity to ZooKeeper, Patroni’s behavior depends on whether failsafe mode is enabled or disabled. In both cases, the cluster may transition to read-only mode, but the trigger differs. With failsafe mode enabled, the leader actively checks whether all cluster members remain reachable, and demotes itself if any are not. With failsafe mode disabled, the leader continues accepting writes for as long as it holds the leader lock. Once the lock can no longer be renewed, Patroni demotes the leader and switches the cluster to read-only mode.


##### When failsafe mode is disabled


When failsafe mode is disabled and ZooKeeper is unreachable, writes can continue as long as the leader is still accessible and all nodes remain healthy. This continues only until the leader lock’s TTL expires. Once the loop time for refreshing the leader key passes and the lock cannot be renewed, Patroni demotes the leader and transitions the cluster to read-only mode.


After failover, the cluster stabilizes with a synchronous standby in place, maintaining durability for ongoing writes. After failover, the cluster stabilizes with a synchronous standby in place, maintaining durability for ongoing writes.


With a healthy synchronous standby, the system sustains workload execution without blocking or data loss. With a healthy synchronous standby, the system sustains workload execution without blocking or data loss.


When the leader loses access to the DCS, it demotes itself to prevent split-brain and maintain data consistency. When the leader loses access to the DCS, it demotes itself to prevent split-brain and maintain data consistency.


PostgreSQL shows synchronous_standby_names empty after leader demotion, indicating no synchronous standby is configured. PostgreSQL shows synchronous_standby_names empty after leader demotion, indicating no synchronous standby is configured.


When a node is in read-only recovery mode, write operations fail immediately, causing the workload to abort. When a node is in read-only recovery mode, write operations fail immediately, causing the workload to abort.


##### When failsafe mode is enabled


When failsafe mode is enabled and if the current leader loses connectivity to ZooKeeper, Patroni on the leader node continuously determines whether all cluster members are reachable through the REST API. The leader node continues to accept writes only if every member remains accessible; otherwise, Patroni demotes the cluster to read-only mode.


Patroni logs showing the leader test-db-1 checks connectivity with all the standbys through the REST API and receives accepted responses from all cluster members during ZooKeeper connectivity loss, allowing the leader to continue accepting writes in failsafe mode. Patroni logs showing the leader test-db-1 checks connectivity with all the standbys through the REST API and receives accepted responses from all cluster members during ZooKeeper connectivity loss, allowing the leader to continue accepting writes in failsafe mode.


PostgreSQL shows a specific synchronous standby configured, ensuring writes are acknowledged by a designated replica. PostgreSQL shows a specific synchronous standby configured, ensuring writes are acknowledged by a designated replica.


When the DCS is unreachable, patronictl cannot retrieve cluster state, preventing visibility and coordination operations. When the DCS is unreachable, patronictl cannot retrieve cluster state, preventing visibility and coordination operations.


## Manual failovers and switchovers under synchronous replication


While Patroni handles automatic failovers and switchovers based on health checks and ZooKeeper coordination, it also supports manual failover or switchover operations using` patronictl` commands. When synchronous replication is enabled, not all standby nodes are valid candidates, and Patroni enforces guardrails to protect data integrity.


### Failing over to an asynchronous standby


With synchronous replication enabled, asynchronous nodes are not considered safe failover candidates unless forced.


Command Behavior


**` patronictl failover`** Fails if the selected node is asynchronous.


**` patronictl switchover`** Fails if the selected node is asynchronous.


**Note:** Forcing a failover to an asynchronous node while synchronous replication is enabled bypasses durability guarantees and may lead to data loss.


### When targeting a synchronous standby


If the target is a healthy synchronous replica, Patroni allows the operation as expected.


Command Behavior


**` patronictl failover`** Succeeds. The leader is switched to the synchronous standby.


**` patronictl switchover`** Succeeds. Performs a graceful handoff between leader and sync standby.


After validating how different` synchronous_commit` modes behaved and how Patroni’s guardrails handled failovers, we enabled synchronous replication on production clusters with high write volume, high read volume, and mixed workloads.


We observed no measurable impact on latency or transaction throughput in these clusters, which gave us confidence to roll out synchronous replication in all our clusters.


Patroni also makes it simple to disable synchronous replication without downtime. If replication lag or disk write latency becomes problematic, we can safely revert to asynchronous replication by updating the configuration and configuring the setting` synchronous_mode: false` .


Synchronous replication can be disabled dynamically to restore availability when latency or blocking becomes problematic. Synchronous replication can be disabled dynamically to restore availability when latency or blocking becomes problematic.


After disabling synchronous replication, PostgreSQL removes synchronous standby requirements and resumes asynchronous operation. After disabling synchronous replication, PostgreSQL removes synchronous standby requirements and resumes asynchronous operation.


After disabling synchronous replication, the cluster continues operating normally without a synchronous standby, prioritizing availability over durability. After disabling synchronous replication, the cluster continues operating normally without a synchronous standby, prioritizing availability over durability.


## Why we didn’t choose DRBD


As part of our high availability evaluation, we also considered **Distributed Replicated Block Device (DRBD)** , a Linux-based system that replicates data at the block level rather than the application or transaction level. DRBD mirrors entire volumes, including PostgreSQL’s data directory and WAL files, from one server to another, creating a standby replica in near real time.


While DRBD can offer lower latency than PostgreSQL’s built-in streaming replication, adopting it would have required a substantial architectural shift, including new infrastructure, monitoring, and operational playbooks. Given the maturity of our Kubernetes-based setup and the fine-grained control provided by PostgreSQL’s synchronous replication, we opted to replicate at the database level, where we had better visibility, flexibility, and operational confidence.


## Monitoring synchronous replication for failure and performance


Once we enabled synchronous replication using Patroni, we closely monitored both replication health and failover readiness. Two signals in particular help us maintain stability at scale.


### SyncRep wait events


These occur when the primary waits for acknowledgment from a synchronous standby before completing a transaction commit and returning the status to the client. Some wait events are expected, but long or frequent waits can indicate performance issues on the replica or network latency between nodes.


**Why it matters:** Prolonged waits increase write latency and reduce throughput.


**What we track:** The duration and frequency of wait events such as` SyncRep` and` WalSenderWaitForReply` , using PostgreSQL wait event metrics. This metric is sourced from the` postgresql.activity.waits` Datadog metric filtered by the` wait_event:SyncRep` tag, which in turn queries the` pg_stat_activity` table in the PostgreSQL database.


### No synchronous standby detected


If Patroni cannot detect a healthy synchronous standby for an extended period, the cluster loses its ability to fail over safely.


**Why it matters:** Without a synchronous standby, the cluster is vulnerable to data loss during failover.


**What we alert on:** Any sustained period where` patroni_sync_standby` is empty triggers a high-availability (HA) health alert. This metric is sourced from OpenMetrics data and there is no native Datadog integration.


Synchronous replication improves durability, but reduces availability and performance when replicas are unhealthy or unreachable. Monitoring wait times and standby availability is essential to maintain both availability and performance under load.


## Making failover safe by design


A simulated zonal failure revealed a critical weakness in our PostgreSQL architecture: Failover wasn’t safe. Because replicas lagged behind the leader, we were forced to choose between waiting out the network outage or risking data divergence. In production, that is an unacceptable trade-off.


By adopting synchronous replication with Patroni **** and carefully tuning for both durability and latency, we made failover both possible and safe, even under degraded network conditions. We validated these changes through benchmarking and repeated failure simulations, confirming that our clusters can recover predictably without compromising performance at scale.


By blocking writes during synchronous replication outages, failures are surfaced explicitly to upstream services rather than writes being silently lost. This gives consumers the opportunity to respond—for example, through retries or queuing—making failure modes more visible and recoverable compared to asynchronous replication, where writes may be lost without the application ever being notified.


We’re continuing to evolve this setup, exploring quorum-based commit modes and deeper observability into replication health.


We’re always looking for engineers who care deeply about reliability and system design. If building resilient, high-performance databases at global scale sounds exciting,[come work with us](https://careers.datadoghq.com/all-jobs/?s=infrastructure) .


-
-
-
