---
schema_version: "1.0.0"
document_id: "cb8775821f89ef9118e4f3cc78e108c5e48e7f233ea97eb5a2251ca0edbafdd0"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/gitops-launch-week-day-2"
published_at: "2024-11-12T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:8ea9ca0ee23f1ea947abbb98a78aba6927522d29234bac794520413ab8ff3dbb"
---

# GitOps Launch Week, Day 2

## Overview


At Nullstone, we believe developers should be able to submit a single pull request that contains everything needed to deliver a feature or bug fix. No manual configuration in another system. No manual steps at deploy time. Instead, fully automated and identical across environments.


All week, we’re launching Nullstone GitOps to achieving this goal. Nullstone GitOps is not just an infrastructure management tool and is not a replacement for ArgoCD/FluxCD. Stay tuned all week to learn more.


## GitOps for Ephemeral Environments


Ephemeral Environments are a cheap, effective tool for managing production-like environments for demoing features to customers, testing and collaborating on new features, and onboarding trial customers. Unlike non-production environments such as staging, ephemeral environments are temporary. If you’ve managed a staging environment before, you know how tedious and frustrating it can be.


Nullstone GitOps brings a framework and tooling to improve the repeatability and ease-of-use for developers to launch and destroy environments. This engine was built to handle resource collisions, database creation, unique subdomains, automatic certificate provisioning, shared cluster resources, and many more “last mile” problems that prevent automated environment provisioning.


## GitOps for Preview Environments


With a foundation of repeatability in place, we wrapped ephemeral environments with GitHub integrations. These preview environments provide automated launch/destroy when a pull request is opened/closed as well as notifications on the pull request.


To improve launch time and reduce costs of preview environments, Nullstone gives you the ability to share cloud resources across multiple preview environments. As mentioned in the previous section, Nullstone provides namespaces in clusters and datastores to ensure proper isolation of preview environment data while sharing compute resources.


Finally, it’s quite common for teams to have multiple repositories for their product. This can make preview environments difficult to coordinate. Nullstone provides a quick and simple UI when creating a preview environment that lets you enable a repository and select the branch/pull request to use for app code *and* infra code.


## Verify GitOps before shipping to production


When testing or experimenting with infrastructure, it’s common for you to stand up a necessary set of base infrastructure. For example, you need a network to launch a Kubernetes cluster.


Since Nullstone GitOps can codify an entire environment, the Nullstone IaC files can be used as a test harness for running automated infra tests in module repositories. Using the Nullstone CLI, you can automate the launch/destroy of this base infrastructure within your CI/CD pipeline for the infrastructure module.


‍
