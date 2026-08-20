---
schema_version: "1.0.0"
document_id: "ff1e09963f62d1ec81fcbb136478f8f08f072d2b1e6a25ff0749de10e08a605a"
company_key: "yc-flightcontrol"
company: "Flightcontrol"
source_id: "yc-flightcontrol-news-import-33bfd5d239ea"
canonical_url: "https://roadmap.flightcontrol.dev/changelog/2026-04-02-significant-ecs-customization-options"
published_at: null
first_seen_at: "2026-08-09T22:19:16.006447+00:00"
fetched_at: "2026-08-09T22:19:17.909102+00:00"
content_hash: "sha256:fda1270cb527197782047064bde50c88766726a3cfc3d809c2c80d3fc9651d65"
---

# Significant ECS customization options

## 🚀 Features


-


Add custom Task definition fields


-


[See the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#task-definition-base)


-


Custom AMI for ECS EC2 instances


-


[See the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#target) for` target.ami`


-


EBS volume configuration for EC2 clusters


-


[See the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#target) for` target.storage`


-


Add` notesLink` config to all service types to display custom URL in service side pane


-


[See the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code)


-


New ECS container health check config


-


[See the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#ecs-container-health-check)


-


Add get job execution API endpoint


-


[See the docs](https://www.flightcontrol.dev/docs/reference/http-api/scheduler/get-job-execution)


-


Add ability to override image repository in create deployment API


-


(except for protected environments)


## ⚙️ Improvements


-


Deployments can now be cancelled during the deploy phase


-


Add task definition version and link on ECS deploy step accordion


-


Add` createdAt` &` updatedAt` to get services API endpoint


-


Enabled encryption for ECS EC2 instance volumes


## 🐞 Bug Fixes


-


Many bug fixes
