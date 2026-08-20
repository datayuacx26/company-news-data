---
schema_version: "1.0.0"
document_id: "15e173b3e7d0453794c72783a5ef642e820b37d7a02369274a96c4717c0abe3f"
company_key: "yc-flightcontrol"
company: "Flightcontrol"
source_id: "yc-flightcontrol-news-import-33bfd5d239ea"
canonical_url: "https://roadmap.flightcontrol.dev/changelog/2026-01-14-advanced-autoscaling-and-bring-your-own-subnets"
published_at: null
first_seen_at: "2026-08-09T22:19:16.006447+00:00"
fetched_at: "2026-08-09T22:19:17.909102+00:00"
content_hash: "sha256:0eb97174c105fb3ab94e569267f522bec49c0f7f0881d9588b59e52f1d616324"
---

# Advanced autoscaling and bring your own subnets

Aside from the below, we are hard at work getting the major new version ready for early access testing!


## 🚀 Features


-


**Protected Environments**


-


Now you can enable delete protection under environment settings in the dashboard


-


Also restricts actions for users with the new **Restricted Developer** role


-


**New Restricted Developer RBAC role**


-


Same as Developer, **except in protected environments** they cannot view env var values or connection strings, cannot edit environment config, cannot add or delete services, cannot add or delete domains, but can change service config, add & update env var values, and deploy.


-


**Fine grained autoscaling config**


-


Add following fields:


-


` scaleInCooldownTimerSecs` (falls back to deprecated` cooldownTimerSecs` if not present)


-


` scaleOutCooldownTimerSecs` (falls back to deprecated` cooldownTimerSecs` if not present)


-


` scaleInProtectionCPU`


-


` scaleInProtectionMemory`


-


` scaleInProtectionRequests`


-


[Read the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#autoscaling)


-


**Use your own VPC subnets**


-


Previously we would create new subnets in your VPC


-


Now you can connect your existing subnets


-


[Read the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code#option-2-use-existing-subnets)


## 🐞 Bug Fixes


-


Fix various ECS-EC2 deployment issues


-


Fix blue/green deployment issues


-


Fix port form error not showing on network server


-


Misc fixes and improvements
