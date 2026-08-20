---
schema_version: "1.0.0"
document_id: "c9f93ac48503d011ccec4b606ae6efaceabc6bcdff0dff3744d102193a68a424"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-v1-39-released"
published_at: "2024-12-20T09:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:c8852407717ba8105a7df223ed4a9245a30c391e93a2f1f61a49901ae9c072d9"
---

# SpiceDB v1.39 Released

To close out 2024 SpiceDB development, we are releasing SpiceDB v1.39.


In this release we introduce:


- 📊 Native histograms for gRPC server metrics
- 🏥 Smarter health checks with user-specified set of safe migrations
- 🗃️ Improvements for[transaction metadata](https://authzed.com/docs/spicedb/concepts/watch#transaction-metadata)


Foundation for future features:


- ⌛ Expiring relationships: native support for defining relations in schema that last for set amount of time
- 🎼 Composable schemas: initial steps to support multi-part SpiceDB schema


Notable change:


- 🧹 Remove LookupResources v1 implementation (LookupResources v2 was made the default in SpiceDB v1.37)


Refer to the[full release notes](https://github.com/authzed/spicedb/releases/tag/v1.39.0) for detailed info. Let’s explore the highlights of this release.


## Native Histograms for gRPC Server Metrics


SpiceDB has been cloud native operable since its beginnings. By enabling native histograms for gRPC server metrics, it’s easier for SpiceDB users to monitor the health of their permissions systems and collect actionable and meaningful metrics more efficiently.


Native histograms are an experimental feature first released in Prometheus v2.40. They enable higher bucket resolution across arbitrary value ranges while reducing storage and query costs, making even label-based histogram partitioning more practical and cost-effective. Histograms are treated as first-class data citizens rather than breaking them down into float components (as the classic version did). This innovation brings several key advantages, including sparse bucket representation for efficient handling of empty buckets, full float64 range coverage, elimination of bucket boundary configuration during instrumentation, dynamic resolution based on simple parameters, sophisticated exponential bucketing schemas that ensure mergeability, and efficient data representation for both exposition and storage. Head over to the[Prometheus page](https://prometheus.io/docs/specs/native_histograms/) to learn more.


Source:[Grafana Blog](https://grafana.com/blog/2020/06/23/how-to-visualize-prometheus-histograms-in-grafana/)


## Smarter Health Checks


Smarter health checks ensure SpiceDB users can specify datastore versions that are considered “safe” so that SpiceDB health checks pass in environments running multiple versions of SpiceDB or datastores.


For example, consider deployments that run new migrations in a test environment while continuing to run an “old” instance of SpiceDB in production. Because a new migration has occurred, the SpiceDB health check might fail and cause a service interruption in prod if that “old” instance needs to restart. With smarter healthchecks, an engineer can specify the “old” version of SpiceDB to be “safe” and therefore avoid any service interruption due to an erroneously failed healthcheck.


One thing to note, most SpiceDB migrations are backwards compatible by at least one version, so the existing healthcheck is extremely strict by default out of an abundance of caution.


## Future Features


This release also contains foundational work for 2 future features. As an open source project, SpiceDB development is conducted in the open and we use proposals to collect feedback and foster discussion about features. Read through to get the context for the 2 upcoming features:


Expiring relationships:[GC of relationships expired via caveats](https://github.com/authzed/spicedb/issues/1262)


Composable schemas:[Support for composed schemas and multi-test validation of schemas](https://github.com/authzed/spicedb/issues/1437)


## Happy Holidays!


While this is just a high-level overview of some of the newest features in the latest release, we hope you enjoy SpiceDB v1.39. If you are new to the project, have time over the holidays, and want to get up to speed on all things SpiceDB, check out the[SpiceDB playlist](https://www.youtube.com/watch?v=AoK0LrkGFDY&list=PLJ01yXf4uxmgWmzwa0zQ-2kC8G2qzb4Jz) and[Google Zanzibar Basics playlist](https://www.youtube.com/playlist?list=PLJ01yXf4uxmg0a2XJEUi6FarwUwI9xjuH) on our YouTube channel.


Stay up to date on everything AuthZed by signing up for our newsletter and following us on[BlueSky](https://bsky.app/profile/authzed.com) and[LinkedIn](https://www.linkedin.com/company/authzed/) .
