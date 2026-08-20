---
schema_version: "1.0.0"
document_id: "cb610d813ac19749a040aa68767e2098982664336335ae50eb5a9ab9d3d1a6bd"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/feature-highlight-transaction-metadata"
published_at: "2024-11-07T17:34:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:648e2c0370934b5a3c0dd6092c8ac7dbde7feb5790fcf0a24cf153df0e0be56a"
---

# Feature Highlight: Transaction Metadata

The release of SpiceDB v1.38 debuts new foundational API functionality for SpiceDB: Transaction Metadata. When writing relationships, you can now include arbitrary metadata that will be stored alongside the transaction and published to consumers of the[Watch API](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.WatchRequest) .


We designed Transaction Metadata as a powerful building block for solving an array of challenges brought to our attention by users and customers.


Simply associating more information with a particular change to the underlying data powering authorization decisions unblocks countless potential use cases. For example, you can now include an identifier for a user whose action triggered SpiceDB to delete a relationship, making it possible to easily attribute permissions changes to a particular end user.


Additionally, you could use transaction metadata to store a distributed tracing identifier in a transaction threaded through SpiceDB and eventually published to an event streaming technology like Kafka or[Red Panda](https://www.redpanda.com/) and asynchronously processed by other systems.


Please bear in mind: SpiceDB limits the size of metadata to 65k of total storage, regardless of datastore. This limit is in place to preserve consistency across all datastores and to eventually maximize compatibility when migrating relationship data across datastores. We based this limitation on[MySQL's maximum size for text fields](https://dev.mysql.com/doc/refman/8.4/en/storage-requirements.html#data-types-storage-reqs-strings) .


## Origin Story


We first encountered friction while working with a customer that has a mature architecture built around Kafka streams. Together, we were struggling to measure the end-to-end performance for requests flowing through their event streams where SpiceDB was only one of many systems doing the processing. In retrospect, the problem was obvious: async workflows need observability just as much as synchronous ones.


SpiceDB had the typical support you'd expect for OpenTelemetry tracing: synchronous operations (like requests and responses) included the standard trace ID headers. However, changes emitted from the Watch API did not retain the trace ID that was included in the request that wrote the transaction. Thus consumers had no way to associate a change with a trace ID and it was impossible to observe timing data as it flowed through the message bus and into other systems.


We could've tackled this problem by explicitly supporting trace IDs in the Watch API, but we decided instead to wait and see if other users would share related challenges that could perhaps require a more holistic solution.


## Building Transaction Metadata


While adding support for the metadata itself was relatively straightforward, all datastore implementations had to be modified to the extent that they require executing migrations when upgrading SpiceDB to v1.38. Datastore implementations based on relational databases have a new column in their transactions table for metadata. CockroachDB is implemented slightly differently: metadata is stored in its own table because SpiceDB uses CockroachDB's native transactions and emits events to the Watch API via CockroachDB's changefeed. We often pay extra attention when designing SpiceDB code that relies on CockroachDB's changefeed functionality, such as the Watch API, for two reasons:


1. Its behavior has surprised us multiple times in the past
2. There are two different versions of changefeeds (enterprise and core)


We originally laid the foundation for this feature in Q2, but we wanted more time to experiment with designs for the CockroachDB implementation. As a team, we decided to prioritize the Tech Preview of Relationship Integrity and General Availability of LookupResources v2 before returning back to work on Transaction Metadata now in Q4.


## Conclusion


We've covered two use cases in this post that drove our development of the Transaction Metadata, but I'm positive there are plenty more we haven't thought of yet. That's what makes me the most excited about shipping foundational features: they allow for the creativity of our community to shine light on new ways to keep their authorization data correct, fast, and secure.


On this page


- Origin Story
- Building Transaction Metadata
- Conclusion


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)
