---
schema_version: "1.0.0"
document_id: "5f7a302a77e898a52ecf7567f5a5d9d0a82a6c734129eaa403f4470c32cd4675"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/external-secrets-operator-paused"
published_at: "2025-08-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:659825e0aeb7fa32ea2624a8fc5d68d04ab47afab8277e1c79d5facc12f80b1c"
---

# External Secrets Operator is paused. What’s next?

> **Update (June 2026):** This post was written in August 2025, when External Secrets Operator (ESO) had paused new releases and its future looked uncertain. That's no longer the case. The community stepped up with new maintainers, development resumed, and ESO is actively maintained again. The migration walkthrough below still holds if you're consolidating onto Infisical, but the urgency around ESO winding down no longer applies. As a rule of thumb: if your team already runs ESO, the simplest path is to keep it and[add Infisical as a provider](https://external-secrets.io/latest/provider/infisical) . For new, Infisical-focused setups, we generally recommend the native[Infisical Kubernetes Operator](https://infisical.com/docs/integrations/platforms/kubernetes/overview) instead, since it gives you tighter control and a more tailored integration.


External Secrets Operator (ESO) is an open-source service that extra information from secrets managers (e.g. AWS Secrets Manager) and automatically injects the values as Kubernetes Secrets. Accordingly, ESO serves as a nifty bridge between Kubernetes and a third-party service.


Unfortunately, on August 13th, 2025, the ESO team[announced](https://github.com/external-secrets/external-secrets/pull/5137) that they’re officially pausing additional releases until they can rebuild the ESO team. ESO cited that their team is unsustainably small to manage the release schedule demanded by their users. They’re pausing all new features, patches, and container images; they’re also pausing support. That said, they’ll continue to merge PRs submitted by the community.


ESO was a neat OSS project that was used by many critical organizations looking for a lightweight solution to reconcile two independent secrets managers. We hope that the ESO team is able to build a more suitable team for the task; however, many ESO users are understandably afraid that the project will lose momentum and die off. While we strongly hope that isn’t the case, we want to share how you could switch from ESO to a commercial open source platform like Infisical.


But first, let’s first revisit what ESO is and why they’re pausing development. Then, we’ll discuss how Infisical works.


## What is External Secrets Operator?


External Secrets Operator (ESO) is a Kubernetes operator that bridges a Kubernetes cluster with external secret managers, with native support for AWS Secrets Manager, Infisical, HashiCorp Vault, Google Secret Manager, Akeyless, Azure Key Vault, Pulumi ESC and many more.


The goal of secrets managers is to avoid committing sensitive data to repositories; instead, they’re referenced by strings and fetched at build or run-time. ESO is *not* a secrets manager; instead, it’s a bridge that fetches data from an external secrets manager and syncs it with Kubernetes native secrets support.


The goal of ESO is to allow developers to use Kubernetes while consolidating and standardizing their secrets operations outside of Kubernetes. External secrets managers offer robust techniques to safeguard secrets across the stack. For instance, a secrets manager will have rotate and dynamically generate secrets. They’ll also consolidate secrets management with certificates management, providing a single system for managing access parameters.


In short, ESO decouples secret storage and rotation from a Kubernetes cluster runtime, allowing developers to keep centralized management with Kubernetes-native consumption.


## Why is ESO pausing development?


Unfortunately, ESO is pausing development due to burnout. The ESO team is very small, with only a single maintainer working on the project quasi full-time. The team has faced a lot of stress from ESO users that are looking for immediate fixes to problems that they’ll encounter.


“[People don’t always respect our time](https://www.thestack.technology/open-source-devs-burn-out-crucial-projects-in-jeopardy) , our effort, our anything” says ESO maintainer[Gergely Brautigam](https://github.com/Skarlso?ref=thestack.technology) , adding that, “People feel entitled to say ‘I’m using this and this and it’s not working so fix it’, but that’s not how this works.”


That’s an unfortunate reality of many independent open-source projects without a commercial backer, where indie developers are treated like a fully-staffed company despite the project existing for strictly pro-bono reasons.


Despite many pleas[from the public](https://www.linkedin.com/posts/jkroepke_kubernetes-prometheus-grafana-activity-7361409948003594240-LCiq/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAC5EaCkB3TnonEQz6wAdlHZEOJwP13aY0dg) for maintainers to join the project, there currently is no evidence that ESO is close to building a long-term team. We hope that they do, however, as ESO serves as a critical, minimalist solution for companies that use two package managers.


## Are ESO and Infisical alternatives to each other?


ESO and Infisical are distinct open source products. ESO is a bridge—it connects external secrets managers (like Infisical) with Kubernetes. In fact, ESO has a dedicated documentation[on supporting Infisical](https://external-secrets.io/latest/provider/infisical) .


However, unlike many rudimentary secrets managers, Infisical also has[first-party support for Kubernetes](https://infisical.com/docs/integrations/platforms/kubernetes/overview) , including[a native injector.](https://infisical.com/docs/integrations/platforms/kubernetes-injector) In that sense, despite being a significantly more comprehensive solution, Infisical is also an alternative to ESO—on the condition that developers use Infisical as their package manager.


Let’s explore how Infisical natively integrates with Kubernetes. Then, we’ll discuss the process of switching from ESO (and your package manager) to Infisical to consolidate secrets and your Kubernetes integration into a single solution.


## How does Infisical integrate with Kubernetes?


There are a few components that tightly integrate Infisical with Kubernetes. Infisical has *multiple* strategies for syncing secrets with Kubernetes, accounting for various build strategies.


### The Infisical Operator


Infisical ships[a Kubernetes Operator](https://infisical.com/docs/integrations/platforms/kubernetes/overview) (installed via Helm) that continuously reconciles your cluster with Infisical.


It introduces CRDs to sync secrets from Infisical, push secrets to Infisical, and manage dynamic secrets. When configured, the operator keeps[Kubernetes resources up to date](https://infisical.com/docs/integrations/platforms/kubernetes/infisical-secret-crd#automatic-redeployment) and can trigger rolling restarts of workloads that consume updated secrets. It also supports advanced Go template transformations (with helper functions) and custom CA trust for self‑hosted deployments.


There are three CRDs that connect Infisical and Kubernetes.


**InfisicalSecret CRD**


[InfisicalSecrets CRD](https://infisical.com/docs/integrations/platforms/kubernetes/infisical-secret-crd) syncs secrets from Infisical to Kubernetes as a native` Secret` (and optionally` ConfigMap` ) resources. The project, environment, or path can be scoped and fetched recursively.


**InfisicalPushSecret CRD**


[InfisicalPushSecret CRD](https://infisical.com/docs/integrations/platforms/kubernetes/infisical-push-secret-crd) does the opposite—it pushes a Kubernetes Secret into Infisical to centralize management. It can overwrite secrets and delete secrets.


**InfisicalDynamicSecret CRD**


[InfisicalDynamicSecret CRD](https://infisical.com/docs/integrations/platforms/kubernetes/infisical-dynamic-secret-crd) creates time‑bound leases for dynamic secrets in Infisical (e.g. database creds), syncing them to a Kubernetes Secret. The operator renews/rotates before expiry, where lease revocation policies could be set.


### **Kubernetes Agent Injector**


Another strategy is to use the Agent Injector, a mutating admission webhook that pushes secrets into a Pod by including an Infisical Agent container.


### Kubernetes CSI


Infisical also provides a CSI (Container Storage Interface) provider that works with the[Secrets Store CSI Driver](https://github.com/kubernetes-sigs/secrets-store-csi-driver) to mount secrets directly as files in Pods, where no Kubernetes Secret object is required. Here, developers define a SecretProviderClass to map Infisical secrets to file paths.


## How do you migrate from ESO to Infisical?


Migrating from ESO to Infisical is a simple two-step process.


### First, choose to migrate to Infisical as your Secrets Manager


Migrate your existing secrets to Infisical as your centralized secrets manager. For some options, Infisical has a dedicated guide; for instance, you can follow this resource to migrate secrets[from Vault to Infisical](https://infisical.com/docs/documentation/platform/external-migrations/vault) .


Infisical is a *very* advanced secrets solution with additional robust support for certificate management, secrets scanning, privileged access management, and more. The platform generally has feature parity with other alternatives; it is also significantly more flexible than rigid cloud provider solutions like AWS Secrets Manager.


### Second, migrate from ESO to using The Infisical Operator, Kubernetes Agent Injector and Kubernetes CSI.


Switching from ESO to Infisical means choosing the right Infisical strategy.


The Infisical Operator is best for teams looking for a fully automated solution to sync and manage secrets directly within Kubernetes using CRDs. The Kubernetes Agent Injector is ideal when you need secrets injected directly into Pods without creating Kubernetes Secret objects. The Kubernetes CSI is a great choice for mounting secrets as files in Pods, especially when you want to avoid using Kubernetes Secret objects altogether.
