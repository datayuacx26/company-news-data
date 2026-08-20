---
schema_version: "1.0.0"
document_id: "19745f7c16d350e1e9e35f255fdb32b42ca9cf66f76937638e3aafcb2d202ce9"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/new-application-create-flow-revamped-datastore-create-and-update-flow-multiple-node-groups-and-temporal-based-autoscaling"
published_at: "2025-12-24T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T22:24:49.305726+00:00"
content_hash: "sha256:7265d006cffe10f0a103b16605224b03920333cf8acea1925fe77f3fa57cc6f4"
---

# New Application Create Flow, Revamped Datastore Create and Update Flow, Multiple Node Groups, and Temporal-based Autoscaling

## Application Create Flow v2


Users can now deploy multiple applications in one repository at the same time. Porter will now detect users’ stack when building with Github repositories.


## Revamped Datastore Create and Update Flow


**‍** Users can now update their Porter-managed datastores from the dashboard. Additional support added for Elasticache metrics, all PostgreSQL versions, and snapshot/clone validation.


## New Dedicated Settings Page


**‍** This page includes updated notifications options, including a[PagerDuty](https://www.pagerduty.com/) integration,[Slack](https://slack.com/) , and email notifications for application CI and runtime alerts.


## Preview Environments Update


**‍** Users can now dynamically create URLs based on branch names or PR numbers.


## Disk Updates


Users can configure disk sizes per node group and will receive alerts when applications are impacted by disk pressure.


## Creating Node Groups


Users can now create multiple node groups - example use cases for additional node groups include GPUs, spot instance for interruptible workloads, running nodes in a public subnet to reduce NAT gateway costs, and isolating compute demands across development, staging, and productions workloads running in the same cluster.


## Cluster Selector Update


**‍** Clusters are now alphabetically sorted by default, and scrollable. Users can also keyword search for clusters.


## Temporal-based Autoscaling


**‍** Porter can now monitor your[Temporal](https://temporal.io/) task queues and automatically adjust worker count. More info in the docs[here](https://docs.porter.run/configure/temporal-autoscaling) .
