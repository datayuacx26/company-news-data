---
schema_version: "1.0.0"
document_id: "981fab6854fc414b384737db07f64b71dcbf486a46eec7a7cfeb8f541169f270"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/changelog-first-deployment-onboarding-flow"
published_at: "2023-05-31T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:613ee6e3e2a4fb6212b4e0244921a45d1e1f2ddc8eeec122586e5fb67b383373"
---

# Changelog: First Deployment Onboarding Flow

## Features


### New First Time Users Onboarding Flow


As with any expert system, the first interaction can be daunting. We want to make sure that your first experience with Massdriver shows you the power of the platform in as short a time as possible. To that end, we have created a new onboarding flow which will deploy a demo app for you to experiment with before trying your hand at your own infrastructure.


These deployments are compatible with Azure, AWS, and GCP. There are options for serverless or serverful deployments powered by Kubernetes.


After completing the onboarding flow,[check out our mission logs](https://docs.massdriver.cloud/missionlog/add-dns) for bite-sized challenges to customize the deployment!


### Automatic Infrastructure Generation


Automatic Infrastructure


Generating infrastructure should be as easy as describing the dependencies of your application. Massdriver is bringing the dream to reality! Based on your environment’s credentials, we can now generate complex infrastructure based on your application’s declared dependencies. If your application needs a Postgres database, let Massdriver determine the dependencies automatically.


### Set Credentials When Creating A New Environment


Credentials In Environment Creation


Security is essential to every business that operates in the cloud. Massdriver allows our users to provide discreet credentials between environments. When creating a new environment, Massdriver will prompt for a credential so you can use different creds between staging, production, or enterprise deployments for your customers.


### Improved Deployment Errors


Better Deployment Errors


Errors happen. We strive to make sure our users never see them but sometimes they are unavoidable. We have updated our errors panel to give much more detail giving you the tools necessary to resolve issues with confidence.


### Improved Deployment Diffs


Deployment Diffs


Seeing changes to your applications and infrastructure overtime is a crucial part of a great experience in the cloud. We have made the process of seeing those differences much more enjoyable by giving our deployment diffs a huge makeover. Numbered deployments make it easy to find the point in time and the diffs now only show what changed so you can find it easily.


### Artifact Import Via CLI


Planning big migrations can be challenging, to help with the transition you can now import artifacts for resources outside of Massdriver and connect to them in the UI. Paired with our VPC Peering bundles, your migration to Massdriver is now a little easier.


For more information[download the cli](https://github.com/massdriver-cloud/mass/releases) and run


` mass artifact import -h`


## Bundles


We are excited to announce the release of several new bundles for our users! Our team ships open-source best-practice reference architectures to the Massdriver Package Manager weekly.


These bundles make it easy for teams to experiment in the cloud and ship new features quickly, helping your organization stay ahead of the competition.


Don’t see what you are looking for? Your team can[develop their own bundles](https://docs.massdriver.cloud/bundles) to extend the platform to meet your needs.


- [aws-mq-rabbitmq](https://github.com/massdriver-cloud/aws-mq-rabbitmq)
- [azure-elasticpool-mssql](https://github.com/massdriver-cloud/azure-elasticpool-mssql)
- [aws-s3-logs-bucket](https://github.com/massdriver-cloud/aws-s3-logs-bucket)
- [k8s-loki-s3](https://github.com/massdriver-cloud/k8s-loki-s3)
- [gcp-healthcare-dataset](https://github.com/massdriver-cloud/gcp-healthcare-dataset)
- [gcp-healthcare-fhir-store](https://github.com/massdriver-cloud/gcp-healthcare-fhir-store)
- [gcp-healthcare-hl7v2-store](https://github.com/massdriver-cloud/gcp-healthcare-hl7v2-store)
- [k8s-datadog](https://github.com/massdriver-cloud/k8s-datadog)
- [azure-event-hubs](https://github.com/massdriver-cloud/azure-event-hubs)
- [azure-cognitive-service-openai](https://github.com/massdriver-cloud/azure-cognitive-service-openai)


‍
