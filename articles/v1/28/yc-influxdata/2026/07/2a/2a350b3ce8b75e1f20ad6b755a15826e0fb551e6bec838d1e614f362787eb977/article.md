---
schema_version: "1.0.0"
document_id: "2a350b3ce8b75e1f20ad6b755a15826e0fb551e6bec838d1e614f362787eb977"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/whats-new-in-influxdb-telegraf-q2-2026-product-updates/"
published_at: "2026-07-01T06:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:9229f775d33861f9dbfdf352b8e1c993269e4c0b6cef5e1c6e26e2012b387416"
---

# What's New in InfluxDB and Telegraf: Q2 2026 Product Updates

Summary


Q2 was about giving teams more leverage with less overhead. Between April and June 2026, releases across Telegraf, InfluxDB 3, and InfluxDB 3 Explorer focused on reducing manual work and putting more control directly in their hands as they scale. Telegraf Enterprise reached general availability, giving teams a centralized way to manage, monitor, and support tens of thousands of Telegraf agents. InfluxDB 3.10 expanded our performance beta with enterprise features, and InfluxDB 3 Explorer added broader query support, including an AI-assisted Flux-to-SQL converter and first-class InfluxQL support.


Table of Contents


Here’s everything that shipped.


## Telegraf Enterprise reaches general availability


Telegraf is the open source standard for collecting telemetry from infrastructure, applications, and devices. But what happens at scale? An enterprise running thousands of agents doesn’t have one collection problem; it has thousands of slightly different configs, no single view of agent health, and no safe way to roll out changes with confidence. Today, that usually means leaning on Ansible, Puppet, Chef, or some homegrown script nobody fully trusts. We built Telegraf Enterprise to solve that.


[Telegraf Enterprise](https://www.influxdata.com/blog/telegraf-enterprise-ga/) reached general availability on June 24, 2026, giving teams a centralized way to manage, monitor, and support tens of thousands of Telegraf agents. It combines Telegraf Controller, a console for fleet management, with official InfluxData support. Open source Telegraf doesn’t change—teams get the same lightweight agent and 400+ official plugins. What changes is the operational layer above it: one place to see every agent’s health, standardize configurations, and roll out fleet-wide changes with confidence.


For platform teams running telemetry across hundreds of environments, that visibility is the difference between reacting to problems and operating with control.


Telegraf Controller (free) Telegraf Enterprise


Managed agents Up to 100 Enterprise scale (10,000s of agents)


Configurations Up to 20 Unlimited


Audit logging No Yes


LDAP/OIDC No Yes


Support Community Official support for Controller and Telegraf


The free tier is enough to evaluate Telegraf Controller. Telegraf Enterprise removes the ceiling, adds the audit trail and identity integrations that production environments need, and upgrades in place with no disruption or re-platforming.


[Download Telegraf Controller](https://www.influxdata.com/products/telegraf-enterprise/?utm_source=website&utm_medium=direct&utm_campaign=telegraf-enterprise-ga&utm_content=blog&dl=telegraf-controller) or[contact us about Telegraf Enterprise](https://www.influxdata.com/contact-sales-telegraf-enterprise/) to get started.


## InfluxDB 3.9 and 3.10 close the gap before production


Running a database at the center of your stack takes more than speed. It takes operational control: who can access it, how quickly you can recover, how easily access scales with your team, and how reliably it performs under heavy load.[InfluxDB 3.9](https://www.influxdata.com/blog/influxdb-3-9/) and[InfluxDB 3.10](https://www.influxdata.com/blog/influxdb-3-10/) , released June 17, 2026, deliver both. 3.9 strengthened day-to-day operations; 3.10 built on top of it, advancing the performance beta.


**InfluxDB 3.9 focused on day-to-day operations** , the kind that matters once a database is someone’s responsibility, not just a tool they query. 3.9 came with


- **CLI:** new flags for headless automation and data validation
- **Database lifecycle:** background resources like triggers now clean up properly on deletion
- **Access control:** improved visibility across permissions and product identity for Core and Enterprise builds
- **Performance beta** (opt-in, Enterprise): optimized single-series queries, smoother resource usage under heavy compaction or ingestion, wider and sparser schema support, and automatic distinct value caching to cut metadata query latency


**InfluxDB 3.10 expanded the beta with enterprise features** . Most teams aren’t held up by the database engine itself when they’re ready to go live. The basics hold them up: can we back this up, can we delete what we’re not supposed to keep, can we control who has access to what. 3.10 closes those gaps, offering:


- **End-to-end backup and restore**
- **Row-level deletes** by time range or tag predicate
- **Bulk import** from Parquet files or directories
- **Multi-user auth and RBAC** (preview): built-in Admin, Auditor, and Member roles with OAuth and OIDC support, no changes to existing token-based workflows


Outside the beta, every InfluxDB 3 Enterprise deployment gets cross-database plugin queries, a new /` ready` endpoint that checks object store connectivity before reporting healthy, and parallel compaction to keep query nodes current under heavy ingest.


On the performance side, 3.10 improves query latency for single-series lookups, last-value fetches, and hot-data access, with a reduction in average query latency over prior InfluxDB 3 releases on workloads that hit those paths. Metadata queries like` SHOW TAG VALUES` see the biggest jump, thanks to automatic distinct value caching. Results are workload-dependent, as always, but the direction is consistent: the trade-offs between scale, flexibility, and performance that real-time systems have to navigate are getting smaller, and the upgrade itself requires no migration and no architecture change.


## Query with SQL, InfluxQL, or Flux in InfluxDB 3 Explorer


The Explorer UI started as a place to query data and ended up able to manage schema, move data in, stream it continuously, and speak whichever query language a team already runs.


You can now create, inspect, and delete tables directly in Explorer—no API calls, no CLI—with just the UI, plus a guided import from any v1, v2, or v3 instance and a Transform Data section for renaming, converting, filtering, and downsampling on ingest.


InfluxDB 3 Explorer also learned to keep data moving. MQTT, Kafka, and AMQP streams now wire directly into databases, two new Live Data plugins write continuously, and line protocol validates itself before a single row is written.


The headline addition in InfluxDB 3 Explorer 1.9 is an AI-assisted **Flux-to-SQL converter** that explains its own translations line by line, so migrating off Flux doesn’t mean rewriting every query by hand. Alongside it, **InfluxQL became first-class** in the Data Explorer, schema commands, per-tag charts, so teams that came up on InfluxDB 1.x or 2.x don’t have to leave the UI to use the commands they already know.


Teams keep the queries, data, and workflows they already rely on, and get there faster in InfluxDB 3.


## A few things people have asked us


**What’s the real difference between free Telegraf Controller and Telegraf Enterprise?** Mostly limits and accountability. Free caps you at 100 agents and 20 configs with no audit trail. Enterprise removes the caps and adds the audit logging, LDAP/OIDC, and Enterprise Support that a real production fleet needs.


**Do I have to migrate anything to get InfluxDB 3.10?** No. It’s a seamless upgrade for existing InfluxDB 3 Enterprise and Cloud Dedicated customers. No migration tooling, no architecture change.


**Can Explorer actually convert my Flux queries?** Yes, the Flux-to-SQL converter is available in beta. Paste in a Flux query and get back equivalent SQL, plus a line-by-line explanation of how it got there.


**Is InfluxQL fully supported in InfluxDB 3 now?** As of Explorer 1.9, yes. It’s a first-class language in the Data Explorer alongside SQL, including the schema-exploration commands InfluxQL veterans rely on.


## Looking ahead


Some of the most important work this quarter is still in progress. If you’re testing the performance beta or trying the Flux-to-SQL converter, we’d love to hear what’s working, what isn’t, and what you’d like to see next. That feedback plays a direct role in where we go from here.
