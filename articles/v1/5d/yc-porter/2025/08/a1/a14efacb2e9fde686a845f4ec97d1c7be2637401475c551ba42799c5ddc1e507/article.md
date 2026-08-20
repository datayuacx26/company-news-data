---
schema_version: "1.0.0"
document_id: "a14efacb2e9fde686a845f4ec97d1c7be2637401475c551ba42799c5ddc1e507"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/datastore-updates-metrics-and-logs-v2-dismiss-notifications-cancel-pre-deploy-jobs"
published_at: "2025-08-28T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:dd6110b3f712f66fb66b11629b424cbb8cca8b0d62780ccdc537975471025825"
---

# Datastore Updates, Metrics and Logs v2, Dismiss Notifications, Cancel Pre-deploy Jobs

## **Datastore Updates**


### **Aurora One-Click Restore from Snapshot**


To enable better disaster recovery and mirror functionality for Porter-managed RDS instances, we’ve added the ability to create a new database from a DB cluster snapshot for Aurora.


### **Cloud SQL for` porter.yaml`**


Users on GCP can now configure Cloud SQL through` porter.yaml` and manage their service account credentials in one place to reuse across services.


### **DB Metrics**


Metrics are now available for Porter-managed RDS instances. This includes CPU, memory, and network usage.


**Porter-managed RDS instances now come with metrics out of the box.**


## **Metrics v2**


The Metrics tab has been improved with faster load times, better time range selection, and synced tooltips across all of the charts. We'll be adding additional charts for advanced networking and jobs soon.


## **Cancel Pre-deploys**


Users can now force delete the pod that is running a pre-deploy job.


## **Dismiss Notifications**


Notifications on the` Activity Tab` can now be dismissed (and un-dismissed using the` Filter` ).


## **Invite Flow Improvement**


We’ve made inviting new users to your Porter project a bit easier - users invited through` Project Settings` will now be prompted to join your project through the invite email rather than having to create a new project to create a Porter account.


## **Updated Logs Viewer**


Logs have been updated to include easier selection of a time range, the ability to "View In Context" when searching log, and filters are now preserved in the URL to make it easier to share links to logs.


**Logs on the dashboard have been improved for greater functionality.**
