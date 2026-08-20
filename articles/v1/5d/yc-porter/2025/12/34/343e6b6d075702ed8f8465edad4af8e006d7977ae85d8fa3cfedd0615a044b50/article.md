---
schema_version: "1.0.0"
document_id: "343e6b6d075702ed8f8465edad4af8e006d7977ae85d8fa3cfedd0615a044b50"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/audit-logs-custom-autoscaling-persistence-for-the-grafana-add-on-gpu-jobs-improved-deployments"
published_at: "2025-12-16T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T22:24:57.117171+00:00"
content_hash: "sha256:5b145b11e1b23a3e0d04a8484d6f2d98aa048c487442a79fa23717cac39074aa"
---

# Audit Logs, Custom Autoscaling, Persistence for the Grafana Add-on, GPU Jobs, and Improved Deployments

## **Audit Logs**


Porter admin users can now view audit logs for changes made by users and the Porter team under the Project Settings sidebar tab.


Currently, audit logs track changes made to applications, clusters, and environment variables (including Doppler and Infisical integrations), and monitor when the` porter datastore connect` command is run.


## **Custom Autoscaling**


Users can now self-serve autoscaling based on custom metrics, like Sidekiq queue length. Let us know if you’d like this feature enabled for your project!


First, configure the metrics exporter under the Advanced tab, specifying the host and the path. Once the user's application exporting the metric is deployed, that metric will be available for selection. More information can be found in our docs[here](https://docs.porter.run/observability/custom-metrics-and-autoscaling) .


## **Spot Instances for GCP**


Spot instances are now available when selecting a node group to deploy to on GCP.


Spot instances are not recommended for production applications as they can be interrupted based on availability by GCP.


## **Persistence for Grafana on AWS**


We’ve added the ability to configure persistence through RDS storage for the Grafana add-on when hosted on AWS. This allows for significant cost savings compared to Datadog, without sacrificing the reliability of your metrics.


## **GPU Jobs**


You can now schedule jobs on GPU instances. This is available across AWS/Azure/GCP.


## **Improved Deployments**


- Rebuilding is no longer required if a pre-deploy fails!


‍


- If you're using Docker BuildKit and have` DOCKER_BUILDKIT:1` set as an environment variable in your GitHub Actions file, you could achieve up to an additional 3x improvement in build speeds when using Docker build caching.


‍


- When configuring resource allocation for your applications, you can utilize input fields in addition to the sliders.


- Users can now preview and confirm app configuration changes before hitting deploy.
