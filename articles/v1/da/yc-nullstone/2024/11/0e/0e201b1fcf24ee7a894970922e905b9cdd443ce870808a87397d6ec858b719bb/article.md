---
schema_version: "1.0.0"
document_id: "0e201b1fcf24ee7a894970922e905b9cdd443ce870808a87397d6ec858b719bb"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/gitops-launch-week-day-1"
published_at: "2024-11-11T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:119780cc1def71bce1b9d5be6f3dbc41e74020ee33d1484a0842b0e90c91f4a2"
---

# GitOps Launch Week, Day 1

## Overview


‍
At Nullstone, we believe developers should be able to submit a single pull request that contains everything needed to deliver a feature or bug fix. No manual configuration in another system. No manual steps at deploy time. Instead, fully automated and identical across environments.


All week, we’re launching Nullstone GitOps to achieving this goal. Nullstone GitOps is not just an infrastructure management tool and is not a replacement for ArgoCD/FluxCD. Stay tuned all week to learn more.


## GitOps (and IaC) for developers


Infrastructure-as-code (IaC) and GitOps tools were made for infrastructure experts, not for developers. However, the practices are simple and effective. A Git repository serves as a single source of truth to codify infrastructure which promotes reproducibility, transparency, testability, and easier diagnostics. This prompts the question: “Why can’t developers use a similar process for application architecture?”


With this launch, Nullstone is doing exactly that. Rather than defining granular cloud-based resources, a developer defines a set of application needs. Here are a few examples:


1. “My application needs a postgres database and a redis cluster.”


2. “My application needs a shared file system mounted at \`/usr/share/assets\`.“


3. “My application needs a set of JWT keys generated and injected.”


A developer is no longer burdened with figuring out regions, datacenters, access keys, security groups, IAM policies, secrets management, incompatible configurations, and much more. Instead, they can stay focused on their application and what customers want.


Read more at \[Nullstone docs\](https://docs.nullstone.io/gitops/overview.html).


## Sync pipeline environments


With this release, Nullstone allows you to configure branch-based GitOps synchronization for each pipeline environment. Each environment can be configured to enable/disable GitOps and which branch will trigger a synchronization. If you’re nervous about releasing changes to a production environment, you can also disable auto-apply which requires an authorized team member to execute infra changes.


## Monorepo/Microservice support


Whether your entire product is in a monorepo or you have multiple repos with several services, Nullstone will now allow you to connect to many repositories that synchronize 1 or many Nullstone blocks (infra components). This feature allows you to colocate your IaC configuration alongside the application code. Or, if you want to centralize your base configuration into a core repository, that will also work. This new GitOps structure is extremely flexible and supports all repository setups.


## Env-specific configurations


There *are* reasons why environments should be different. Here are a few examples:


- It’s too costly to run a postgres cluster in high availability for a sandbox environment.


- Our sandbox environment shouldn’t connect to our production Stripe account — accidents can affect customers.


- Our app needs to more than 1 container running and with more resources


These differences are more rare than the commonalities. Instead of optimizing for customizing an environment, Nullstone provides an overrides file (optionally, one per environment) to enable making env-specific changes to variables and environment variables.


‍
