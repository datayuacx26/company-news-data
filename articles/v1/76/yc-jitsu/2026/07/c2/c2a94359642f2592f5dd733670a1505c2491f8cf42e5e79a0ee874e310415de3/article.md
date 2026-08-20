---
schema_version: "1.0.0"
document_id: "c2a94359642f2592f5dd733670a1505c2491f8cf42e5e79a0ee874e310415de3"
company_key: "yc-jitsu"
company: "Jitsu"
source_id: "yc-jitsu-news-import-65f0e2b767a6"
canonical_url: "https://jitsu.com/blog/jitsu-2-14"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-22T00:57:29.784311+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:6913afc3b395b9ff05097dbc0cb32bb515e8cbfb551bd18a47bc762f8fff8b2a"
---

# Jitsu 2.14 is now public: Kubernetes-native for production self-hosting

## Jitsu 2.14 makes production self-hosting Kubernetes-native


Functions, Profile Builder v2, and connector syncs now run as **managed Kubernetes workloads** — giving self-hosted teams a cleaner path to autoscaling, isolation, sharding, and cloud-portable operations.


It's our first stable public release in nearly a year, and it's a big one — roughly **1,600 commits** . Most of that work went into making Jitsu more reliable, easier to operate, and ready for larger self-hosted deployments. The headline is the architecture: the pieces that used to run inside a single process are now first-class workloads you can scale independently.


## What "Kubernetes-native" means here


In the old runtime, functions, profile builders, and connector syncs were background pieces sharing one process. In 2.14 each becomes its own managed workload, using a few new building blocks:


- **Function Servers** — dedicated pods that run your user[Functions](https://jitsu.com/docs/functions) (the JavaScript that filters, reshapes, and routes each event) and **Profile Builder v2** (Jitsu's engine for rolling a user's events up into a single profile). Splitting them out is what makes autoscaling, workload isolation, sharding, and separate execution classes possible.
- **The Jitsu operator** — a Kubernetes operator that creates and reconciles those workloads for each deployment, so you don't hand-manage the underlying resources.
- **` syncctl`** — the controller that schedules connector syncs. It now reconciles scheduled syncs directly into the cluster as **Kubernetes CronJobs** , with each sync running as its own pod. That replaces the previous Google Cloud Scheduler dependency and makes self-hosting far more portable.


The net result: scaling functions, syncs, and event processing no longer ties core behavior to one cloud provider or one process.


## Upgrading? Read this first


If you self-host Jitsu, a few things changed that you need to plan for. **Read the[release notes](https://github.com/jitsucom/jitsu/releases/tag/2.14.0) and the[self-hosting docs](https://docs.jitsu.com/self-hosting) before you upgrade.**


- **Kubernetes is now required for feature-complete production self-hosting** — specifically, for deployments that use functions, profile builders, and connector syncs. Smaller or exploratory setups still have lighter options (see below).
- **Deploy the operator before upgrading rotor** — event delivery now depends on function-server routing, so the operator has to be in place first.
- **Docker Compose is deprecated for full production use.** It still exists for exploration and development, but the recommended development setup is now the **Helm chart** — and 2.14 ships development Helm charts that run the full architecture locally.
- **Google Cloud Scheduler support has been removed** (replaced by Kubernetes CronJobs).
- **Some console defaults changed** : host-only auth cookies by default, a new **maintenance mode** replacing the old read-only flag, default API rate limiting, stricter config validation, and a lower default ingest payload size.


## What else is new


Alongside the architecture work, 2.14 lands a long list of product and developer-facing features:


- **MCP server support in Jitsu Console** — connect an AI agent directly to your workspace (see[Jitsu now speaks MCP](https://jitsu.com/blog/jitsu-mcp) ).
- **OpenAPI spec and a built-in API reference.**
- **User API tokens** with names, types, and expiration dates.
- **SOC 2-oriented audit logs.**
- **Maintenance mode** for safe operational windows.
- **Dead-letter queue and event reprocessing.**
- **Better sync and connection notifications.**
- **Redesigned signup flow.**
- **Segment-style` sentAt` clock-skew correction.**
- **ClickHouse TTL-based events-log retention.**


## More and better integrations


We also added and improved a range of integrations:


- **Destinations:** Resend, SendGrid, Statsig, DuckDB / MotherDuck, Xero.
- **Warehouse auth:** Snowflake key-pair auth, Postgres Private Service Connect auth, and Redshift IAM role improvements.
- **Syncs:** Firebase subcollection syncs.


## Get started


2.14 is a big step toward running Jitsu seriously in your own infrastructure — a cleaner foundation for scaling event processing without a hard dependency on any single cloud.


- **Upgrading an existing deployment?** Start with the[self-hosting guide](https://docs.jitsu.com/self-hosting) , which now walks through the Helm chart.
- **Trying it for the first time?** The development Helm chart spins up the full architecture locally — no Kubernetes cluster of your own required.
- **Want the full changelog?** Read the[2.14.0 release notes on GitHub](https://github.com/jitsucom/jitsu/releases/tag/2.14.0) .
- **Questions or hit a snag?** Open an issue on[github.com/jitsucom/jitsu](https://github.com/jitsucom/jitsu) — we read every one.
