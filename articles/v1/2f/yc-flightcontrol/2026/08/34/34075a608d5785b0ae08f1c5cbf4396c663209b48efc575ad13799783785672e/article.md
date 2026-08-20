---
schema_version: "1.0.0"
document_id: "34075a608d5785b0ae08f1c5cbf4396c663209b48efc575ad13799783785672e"
company_key: "yc-flightcontrol"
company: "Flightcontrol"
source_id: "yc-flightcontrol-news-import-33bfd5d239ea"
canonical_url: "https://roadmap.flightcontrol.dev/changelog/2025-09-12-managed-ecs-ec2-clusters-in-preview"
published_at: null
first_seen_at: "2026-08-09T22:19:16.006447+00:00"
fetched_at: "2026-08-09T22:19:17.909102+00:00"
content_hash: "sha256:67aeb919c636bfa1ca19651877b6422be1331806b57443c398da04f798bcc1c6"
---

# Managed ECS-EC2 Clusters in Preview

## 🚀 Features


-


**Managed ECS-EC2 clusters in preview**


-


AWS has a long standing issue with the ECS agent randomly disconnecting, resulting in orphaned EC2 instances which can cause traffic or deployment degradation.


-


We have attempted to solve this a few ways in the past, but there were still critical edge cases falling through.


-


So we bit the bullet, and developed a robust, full featured ECS cluster management solution to solve this problem once and for all.


-


It's currently in private preview. To get early access before we roll it out to everyone, contact support.


-


**Scaling API endpoint to manually scale servers up and down**


-


You can use this to implement queue-length based autoscaling OR to scale staging and preview environments to 0 outside work hours


-


[Read the docs](https://www.flightcontrol.dev/docs/reference/http-api/services/update-scaling)


-


**Ability to disable autoscaling in service config**


-


[Read the docs](https://www.flightcontrol.dev/docs/guides/flightcontrol/using-code/web#autoscaling)


-


**List services API endpoint**


-


[Read the docs](https://www.flightcontrol.dev/docs/reference/http-api/services/get-services)


## ⚙️ Improvements


-


Add download button to runtime logs


-


Scheduler UI improvements


-


Added link to ECS service AWS resource card


## 🐞 Bug Fixes


-


Fix rollback case where no services were available for rollback


-


Fix some deletion issues


-


Fix a budget UI not working


-


Fix some GitHub integration issues for some users
