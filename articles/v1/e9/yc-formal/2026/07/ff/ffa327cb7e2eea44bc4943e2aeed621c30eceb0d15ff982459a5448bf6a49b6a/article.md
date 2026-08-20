---
schema_version: "1.0.0"
document_id: "ffa327cb7e2eea44bc4943e2aeed621c30eceb0d15ff982459a5448bf6a49b6a"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/connector-v2/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:f9c37415df626ed9dba5b7b570d7ed2d49478c1aabe6b3d83b7d7f7ad3d93b3e"
---

# Announcing Connector v2.0.0

Today, we’re releasing version 2.0.0 of the Formal Connector, which significantly overhauls the way the Connector synchronizes data with the control plane. Our new system substantially improves data efficiency and reliability. We’ve been testing it with several customers over the past few months, and we’re excited to make it generally available today.


## Change synchronization, more efficiently


The v1 Connector relied on a long-lived outbound gRPC connection, through which the control plane pushed configuration and policy changes in real time. Because network disruptions often caused the Connector to miss these pushed events, we had to add a frequent polling mechanism that dramatically increased database load and wasted bandwidth.


Our new synchronization mechanism fully decouples the Connector from the primary database. Under this architecture, the Connector never reads directly from the primary database. Instead, it reconstructs its state from two sources: a periodic snapshot and a chronologically ordered event log.


Every mutation to our Postgres database state (e.g. a resource is created, a policy is updated, a user is removed) is appended to an immutable, ordered event log. Each event carries a sequence number that is strictly greater than the sequence number of the event it follows. Connectors fetch ordered events from the event log via our API every 3 seconds.


To prevent having Connectors from reconstructing the entire history of database state one event at a time, we also perform periodic snapshots. A new snapshot is generated at most every hour. Each snapshot is versioned with a monotonically increasing sequence number and a timestamp. Snapshots are stored independently from the primary database, so their availability is not affected by database outages, migrations, or connectivity issues.


A snapshot represents a consistent, point-in-time view of the entire system state. Because it is generated from a read replica, it is never in a half-applied state — it either reflects the full state before a migration or the full state after.


With connectors reading from snapshots on startup followed by the event log for updates, the Connector state should always correspond to an exact state of the database at some point in time.


Connectors using this new architecture cannot miss updates from the control plane because the polling mechanism keeps track of an event cursor corresponding to its most recently processed sequence number. It only queries for new events after processing the previously ordered events.


Beyond reliability and data efficiency, we’ve also made other improvements to connector functionality associated with the new event synchronization architecture. In particular, **changes to resource and listener configuration should no longer require a Connector restart** to take effect.


## How this affects you


You can update to version 2.0.0 today by pulling the latest image from our repo. You should not need to make any additional changes to connector configuration, as the new system simply seamlessly replaces our old event synchronization mechanism.


We are also announcing the deprecation of the v1.x.x series of Connectors. New functionality and bug fixes will only be released to v2.x.x Connectors from today onwards. We will continue to maintain the control plane infrastructure to support v1.x.x Connectors for 90 days (until October 6, 2026) to give you time to migrate. However, since there are no breaking changes in this release, we don’t expect migration to be more complicated than simply bumping your Connector version.
