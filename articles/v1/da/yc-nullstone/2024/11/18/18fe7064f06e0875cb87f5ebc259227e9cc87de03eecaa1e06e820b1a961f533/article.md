---
schema_version: "1.0.0"
document_id: "18fe7064f06e0875cb87f5ebc259227e9cc87de03eecaa1e06e820b1a961f533"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/gitops-launch-week-day-5"
published_at: "2024-11-15T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:a4abffcccb042077079152bf684f6acb14be53a0f0a2e5c2f0f12b4ada3d9e14"
---

# GitOps Launch Week, Day 5

## Overview


At Nullstone, we believe developers should be able to submit a single pull request that contains everything needed to deliver a feature or bug fix. No manual configuration in another system. No manual steps at deploy time. Instead, fully automated and identical across environments.


All week, we’re launching Nullstone GitOps to achieving this goal. Nullstone GitOps is not just an infrastructure management tool and is not a replacement for ArgoCD/FluxCD. Stay tuned all week to learn more.


## Slack Notifications defined in IaC


One of the most requested features from our developers is the ability to send slack notifications when an app is deployed or an infra change needs approval. Today, we’re releasing the ability to configure events through IaC files to trigger slack notifications. This is the first of many features planned for Nullstone that illustrates the power of GitOps — the ability to architect the entire software delivery process through IaC files tracked alongside your application code.


See examples and reference for configuration at \[events\](https://docs.nullstone.io/gitops/iac/events.html) in GitOps reference.


## GitOps Slack Notifications


After defining your notifications in IaC, you can subsequently receive notifications about GitOps events to your slack channels. These notifications provide context and links to Nullstone workflows for quick analysis and interaction.


Other GitOps solutions require you to query Kubernetes or visit a UI which requires you to actively wait for completion. For example, it may be valuable for another team member to be notified when infrastructure changes need approval. Or, it may be helpful to know if a team member made updates to infrastructure as a result of GitOps changes.


For this release, we added support for the following event actions:
- App Deployed
- App Deployed for the first time
- Infra Launched
- Infra Updated
- Infra Destroyed
- Infra Needs Approval


We also allow you to filter messages by the action status:
- Completed
- Failed
- Cancelled
- Disapproved


‍
