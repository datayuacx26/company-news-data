---
schema_version: "1.0.0"
document_id: "93acd0d3601f4bf02866d801b9f16fa62103730128f45b7780b5582141af4e5d"
company_key: "yc-flightcontrol"
company: "Flightcontrol"
source_id: "yc-flightcontrol-news-import-33bfd5d239ea"
canonical_url: "https://roadmap.flightcontrol.dev/changelog/2025-10-13-managed-ecs-ec2-is-ga"
published_at: null
first_seen_at: "2026-08-09T22:19:16.006447+00:00"
fetched_at: "2026-08-09T22:19:17.909102+00:00"
content_hash: "sha256:c4ceef7c23ed37462f553d08e5d883429d3fd350808fc11e1220c8e0575e7fba"
---

# Managed ECS-EC2 is GA

Another round of exciting updates for you all! We're now working on blue/green deployments for ECS based services, in addition to the ongoing major pipelines upgrade still in progress that will unlock infinite flexibility for you.


## 🚀 Features


-


**Managed ECS-EC2 clusters now enabled for everyone**


-


AWS has a long standing issue with the ECS agent randomly disconnecting, resulting in orphaned EC2 instances which can cause traffic or deployment degradation.


-


We have attempted to solve this a few ways in the past, but there were still critical edge cases falling through.


-


So we bit the bullet, and developed a robust, full featured ECS cluster management solution to solve this problem once and for all.


-


It's enabled for everyone and working very well


-


**New "Server instances below minimum" notification**


-


Enabled by default for all. This will notify you if your running server instances go below your configured "minimum instances" count.


-


If you receive this, it means one or more of your servers likely crashed (it will be auto replaced by ECS).


-


**Add Linux Capabilities config to ECS based services**


-


[Read the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#linux-capabilities)


## ⚙️ Improvements


-


Support ElastiCache Valkey 8.1


## 🐞 Bug Fixes


-


Fix` environmentType` filter in` GET /services` API


-


Misc fixes
