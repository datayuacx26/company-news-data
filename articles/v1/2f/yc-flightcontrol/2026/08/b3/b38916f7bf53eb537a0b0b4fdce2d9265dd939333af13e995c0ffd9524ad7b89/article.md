---
schema_version: "1.0.0"
document_id: "b38916f7bf53eb537a0b0b4fdce2d9265dd939333af13e995c0ffd9524ad7b89"
company_key: "yc-flightcontrol"
company: "Flightcontrol"
source_id: "yc-flightcontrol-news-import-33bfd5d239ea"
canonical_url: "https://roadmap.flightcontrol.dev/changelog/2026-01-20-ai-skill-and-config-validate-cli"
published_at: null
first_seen_at: "2026-08-09T22:19:16.006447+00:00"
fetched_at: "2026-08-09T22:19:17.909102+00:00"
content_hash: "sha256:f58f207c50bb1619605c29f4403d997688133497064ec3d5d3b2b5550ccff4cd"
---

# AI Skill and Config Validate CLI

## 🚀 Features


-


AI Skill


-


[See the docs](https://www.flightcontrol.dev/docs/ai-agents#skills)


-


` ecsStopTimeoutSecs` config option to all ECS based services


-


[See the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#ecs-stop-timeout)


-


CLI to validate` flightcontrol.json`


-


[See the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/validate-config)


-


Get job execution API endpoint


-


[See the docs](https://www.flightcontrol.dev/docs/reference/http-api/scheduler/get-job-execution)


-


Bring your own subnets now supported for preview environments


-


New docs:


-


[How to fix random 502 errors](https://www.flightcontrol.dev/docs/troubleshooting/load-balancer-502-keepalive-timeout)


-


[Block load balancer access](https://www.flightcontrol.dev/docs/guides/aws/allow-only-cloudfront-to-alb)


## ⚙️ Improvements


-


Ability to override image repository in create deployment API


-


Enable encryption to instance root volumes for all ECS EC2


-


Better handling of install failures for EC2 builders


-


Improve deletion


-


Add` createdAt` and` updatedAt` to get services API endpoint


-


Improve ECS error messages


## 🐞 Bug Fixes


-


Fix log view on short screens


-


Fix cloudmap namespaces not being cleaned up


-


Fix issue with preview environment domains


-


Fix shell escaping issues for env vars in EC2 builders


-


Fix issue connecting Github account


-


Fix user can’t accept invite if they have no existing organization access


-


Fix API error being swallowed
