---
schema_version: "1.0.0"
document_id: "296c5e6f00ae4a4ddc8a2fbdfdc5fec90668e7ec3b72f39cbb484db4681c91ae"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/cyberark-conjur-pricing"
published_at: "2025-04-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:9479d2a29e08341a965a518a2a6562407c377e667d3d2169f8e4900dc9a3e07b"
---

# CyberArk Conjur Pricing | Complete Guide [2025 Edition]

> As of May 2026, CyberArk's products are part of the Idira platform after its acquisition by Palo Alto Networks. It's difficult to ascertain CyberArk Conjur's present and future. Conjur's open-source library remains public, but received its latest commit in 2025. CyberArk's website features a separate product called *Secrets Manager, SaaS* while Palo Alto Networks' website features another *Secrets Manager* product under the Idira umbrella. The CyberArk Conjur-related information in this article should remain directionally correct, though details may have changed.


CyberArk Conjur is a secrets management platform purpose-built for DevOps, cloud-native, and containerized environments. It securely stores and centrally manages sensitive credentials—such as API keys, database passwords, and TLS certificates—for non-human identities like containers, microservices, CI/CD pipelines, and service accounts. Conjur enforces fine-grained access controls, automates credential rotation, provides detailed audit trails, and integrates natively with orchestrators including Kubernetes, OpenShift, and leading CI/CD tools.


Conjur exists in three flavours, all built on the same core:


Offering Typical use case Hosting model


**Conjur OSS (Open Source)** Learning, POCs, labs Self-hosted (you run everything)


**Conjur Enterprise**
*(now sold as **CyberArk Secrets Manager – Self-Hosted** )* Production at scale, strict compliance, air-gap Self-hosted


**CyberArk Secrets Manager – (Conjur Cloud, SaaS)** "Set-and-forget" secrets-as-a-service Fully hosted by CyberArk


**TL;DR:**


-


**Conjur OSS** is a free, community-supported entry point offering the core functionality via a REST API, CLI, and client SDKs. It also includes the Conjur Open Source Suite — a collection of plugins for integrations with common DevOps platforms and tools (like Kubernetes, AWS, Azure, GCP, Ansible, Jenkins, Terraform), plus specialized secrets delivery tooling.


-


**Missing from Conjur OSS** are certain features—most notably, a **web dashboard UI** and native **integration with CyberArk's core PAM Vault** for secrets synchronization.


-


Conjur OSS is *not* the same as **CyberArk Secrets Hub** . Secrets Hub is a separate SaaS product focused on syncing secrets from external stores like AWS Secrets Manager or Azure Key Vault, *not* managing secrets itself.


-


**Secrets Manager – Self-Hosted (Conjur Enterprise)** delivers a full enterprise-grade solution for organizations needing complete control over their environment, backed by official support. However, it requires the customer to manage infrastructure and often incurs **significant professional services costs** .


-


**Secrets Manager – SaaS (Conjur Cloud)** offers a managed service experience that minimizes operational overhead, with pricing that typically scales based on the number of human and non-human identities (workloads) managed.


-


**CyberArk Conjur pricing is highly opaque** across all tiers. There is very little public pricing information, making it difficult to estimate total cost of ownership (TCO) without contacting their sales team. For Self-Hosted deployments in particular, hidden costs like professional services and infrastructure requirements can be substantial.


-


[Infisical](https://infisical.com/) could be a good alternative with more transparent pricing, deeper secrets management features, and a more developer-friendly experience.


---


This guide does its best to untangle the available deployment options, pricing structure, and potential hidden costs associated with CyberArk Conjur. However, due to the general lack of transparency, please treat the pricing information provided here as rough estimates—not precise or guaranteed figures.


## Conjur Service Tiers & Pricing Metrics


Unlike cloud provider products that price per *operation* , CyberArk prices Conjur Enterprise and SaaS primarily by **number of "identities"** (apps, pods, hosts) that request secrets.


Tier Pricing metric Publicly visible price signals*


**OSS** Free, Apache-2.0/LGPL v3 $0 software
(you still pay for compute)


**Enterprise** Annual subscription per identity (or fixed 10-/20-/50-identity packs)[AWS Marketplace "DevSecOps Secrets Manager 20 Users"](https://aws.amazon.com/marketplace/pp/prodview-eyoutsyv3bbii) – **$23,328 / year**
Street price: **$1k – $1.5k per identity / year** on mid-size deals


**SaaS** Annual subscription per identity Quote-only. Typically 10-15% premium vs. self-hosted but infra is included


For most teams, especially those running Self-Hosted Conjur, it's important to budget for **CyberArk Professional Services** (or certified partners). These services often handle initial setup, integration with your existing CI/CD, cloud platforms, or SIEM tools, policy development, upgrades, and health checks. While technically optional, they’re often needed for smooth, secure deployments, especially in complex environments. Keep in mind that these services can **significantly increase your total cost of ownership (TCO)** , so be sure to factor them into your rollout planning.


Here is an exercise in estimating your TCO based on the number of identities you plan to manage and the chosen tier:


### Example Pricing Breakdown


Scenario Identities Deployment License* Infra & Ops **1-Year TCO**


Small pilot 20 AWS Marketplace $23,328 $0 (included) **$23.3k**


Mid-size prod 250 Self-hosted $250k $36k (K8s cluster, 2 × m6g.large + EBS) **$286k**


Enterprise HA 1,000 SaaS $900k (bulk tier) $0 **$900k**


*Note: License prices assume a 20% discount for a 3-year term.*


### Additional Pricing Considerations


When planning your budget, be sure to account for these additional pricing factors:


- **Identity Counting** – Every container, Lambda function or CI job that talks to Conjur is an identity. Ephemeral workloads can quickly inflate identity counts if not properly managed.
- **HA Architecture** – Deployments requiring high availability (HA) or disaster recovery (DR) can add 30–50% to licensing costs due to the need for follower nodes and database replicas.
- **Self-Hosted Overheads** – Self-managing Conjur requires investment in patching, backups, monitoring, on-call coverage, and security hardening, adding substantial labor costs.
- **Marketplace Advantage** — Buying via AWS/Azure marketplace allows you to use committed cloud spend and lock in pricing for the full contract term.


Other important add-on costs to factor in:


Item Indicative Price Notes


**Follower (read-only) node license** 25–40% of primary node Needed for multi-region HA/DR deployments


**Premium Support 24×7** +18% of annual license cost Optional for Self-Hosted; included with SaaS


**Professional Services** $2,000–$2,400/day Typical 5–20 day engagement for design and rollout


**Bring-your-own HSM** Hardware + integration costs Supported via PKCS#11 plugin


In short: **CyberArk Conjur delivers robust, enterprise-grade secrets management** —but its opaque pricing structure, hidden operational costs, and dependency on professional services mean organizations should **carefully evaluate their needs, budget realistically, and compare alternatives** before making a commitment.


## Should You Use CyberArk Conjur?


### Advantages


1. **Enterprise-grade controls** – FIPS-validated crypto, tamper-proof audit, fine-grained RBAC and SIEM integrations.
2. **Unified with CyberArk PAM** – If you already run CyberArk Privileged Access Manager, Conjur slots into the same policy model and reporting.
3. **Multi-platform** – Works on-prem, in any cloud or hybrid. OSS and Enterprise share APIs so migration is straightforward.


### Considerations


1. **Pricing opacity** – No public list except small packs; negotiating is a must.
2. **Identity-based billing** – Pricing is based on the number of identities (such as microservices or workloads), so it's important to factor this into your planning, especially for environments with many ephemeral or distributed components.
3. **Operational burden (self-hosted)** – You manage PostgreSQL, appliances, upgrades and DR unless you pay for SaaS.
4. **Feature overlap** – If you only need CI/CD secret injection, some tools like[Infisical](https://infisical.com/) may be cheaper and faster to deploy.
5. **Overall Fit** – Best suited for large enterprises with existing CyberArk investments or stringent audit/compliance needs. Smaller teams may find alternatives like Infisical, Doppler, or HashiCorp Vault simpler and more cost-effective.


## CyberArk Conjur Alternatives


Product Model Starting price Notable strengths


**Infisical** OSS, Cloud & Self-hosted Free tier / $18 identity / mo Modern UI, environment separation, developer workflows


**HashiCorp Vault** Self-hosted & SaaS $0 OSS / $14,238 / year Rich plugin ecosystem, flexible building block


**AWS Secrets Manager** SaaS $0.40 per secret + $0.05 / 10k calls Tight AWS integration, serverless


*Read our complete[CyberArk Conjur Alternatives \[2025\]](https://infisical.com/blog/cyberark-conjur-alternatives) guide.*


## F.A.Q.


#### Is Conjur OSS really free for production?


The software is free, but CyberArk's enterprise plugins (LDAP/SAML, audit streaming, FIPS modules) are not. Most production environments eventually need those, pushing you to Enterprise.


#### How does Conjur pricing compare to HashiCorp Vault?


Vault Secrets lists at $0.50 per secret per month, plus $0.10 per 10k secret reads (Standard Tier), while HCP Vault Enterprise is much more expensive. For steady-state workloads with >150 secrets/identity, Conjur's identity model is typically 10–15 % cheaper—but may be more expensive for spiky on-demand jobs.


For deeper comparisons, check out:


- [Cyberark Conjur vs Hashicorp Vault](https://infisical.com/blog/cyberark-conjur-vs-hashicorp-vault)
- [Hashicorp Vault Pricing Complete Guide](https://infisical.com/blog/hashicorp-vault-pricing)


#### Can I move from Conjur OSS to Enterprise later?


Yes. Both share the same API and underlying data model. Upgrade is mostly a licence swap and adding enterprise plugins.


#### Does the SaaS tier support on-prem runners?


Yes. You can deploy "Conjur Followers" on-prem or in private clouds that sync secrets from the SaaS master, preserving low-latency access without exposing your network.


#### What happens if I exceed my licensed identity count?


Conjur does not hard-stop requests but flags over-usage. CyberArk will true-up at renewal, often offering volume discounts on the excess identities.


---


## Final Thoughts


CyberArk Conjur is a powerful solution for organizations with strict security, compliance, and audit requirements—but it comes with significant trade-offs that teams should not overlook.


First, the **true cost of ownership (TCO)** can be much higher than initial licensing suggests. Especially for Self-Hosted deployments, costs for professional services, infrastructure, operational staffing, and premium support can add 30–70% on top of the base license price. Even Conjur Cloud (SaaS) carries a hefty per-identity cost, often putting it out of reach for startups or lean teams.


Second, **pricing transparency is poor** . Very little public information is available for larger deployments, forcing customers into opaque, sales-driven negotiations. This makes it difficult to benchmark Conjur against alternatives during the evaluation phase, and complicates long-term budgeting.


Third, organizations should weigh the **vendor lock-in risks** carefully. While Conjur OSS provides an open-source foundation, many critical features (such as enterprise integrations, high-availability enhancements, and compliance tooling) are gated behind CyberArk’s commercial offerings. Once deeply integrated—especially if you're already invested in CyberArk's broader PAM ecosystem—switching to a different secrets management platform can become operationally and financially painful.


**Bottom line:**
CyberArk Conjur is best suited for enterprises already aligned with the CyberArk ecosystem. For teams seeking more developer-friendly, faster-to-deploy, and cost-predictable solutions, it’s worth carefully comparing alternatives (e.g.,[Infisical](https://infisical.com/) ) before committing.
