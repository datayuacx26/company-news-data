---
schema_version: "1.0.0"
document_id: "2efeeb53b006ff69dc4669219e57e2ba8f942d37afacad321dc4bd4960be1b91"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/hashicorp-vault-pricing"
published_at: "2024-12-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:2ebc5b64d44e583780bbe3f8ab1c840a4ee59c565b0238b5bdeea8b126e1d6e3"
---

# Hashicorp Vault Pricing | Complete Guide [2025 Edition]

## What is HashiCorp Vault?


HashiCorp Vault is a secrets management solution for storing and controlling access to passwords, API keys, encryption keys, and certificates across cloud infrastructure.


A few things have changed since Vault first gained popularity:


- **No longer open source.** In August 2023, HashiCorp[switched from the MPL 2.0 to the Business Source License (BSL 1.1)](https://infisical.com/blog/hashicorp-new-bsl-license) . The source code is viewable, but it carries restrictions on commercial use.
- **Now owned by IBM.** IBM completed its $6.4 billion acquisition of HashiCorp in February 2025. Since then, community engagement and support response times have slowed as Vault becomes part of IBM's broader hybrid cloud strategy.
- **Product lineup is shrinking.** HCP Vault Secrets (the SaaS-only product) was sunsetted in mid-2025. The HCP Vault Dedicated Starter tier was also discontinued. Organizations on those products are being pushed toward Vault Community Edition, HCP Vault Dedicated (Standard or Plus), or Vault Enterprise, all of which come with higher complexity and cost.


HashiCorp's remaining Vault products include **HCP Vault Dedicated** (managed, single-tenant cloud), **Vault Enterprise** (self-hosted, same binary as Dedicated), and **Vault Community Edition** (free, limited features). Vault Enterprise pricing has zero transparency and requires talking to sales.


Below, we break down how these solutions compare on pricing and features.


Note: If you're seeking alternatives to Vault, you can read this article about[HashiCorp Vault alternatives](https://infisical.com/blog/hashicorp-vault-alternatives) , or this one about[the best secrets management tools](https://infisical.com/blog/best-secret-management-tools) . Whether you need an on-premises installation or a cloud SaaS product,[Infisical](https://infisical.com/) might be the right solution for you.


## Vault Community Edition (Free, Self-Hosted)


[Vault Community Edition](https://developer.hashicorp.com/vault/tutorials/get-started/available-editions) is the free, self-hosted version of Vault. It was previously known as "Vault OSS" before HashiCorp rebranded it after switching to the BSL license in 2023.


Community Edition covers the core secrets management use cases: KV secrets storage, dynamic secrets, transit encryption, and a wide range of auth methods (AWS, Azure, LDAP, OIDC, Kubernetes, AppRole, and more). It also includes a full HTTP API and CLI.


However, it is missing several features that most production-grade organizations need:


- **No namespaces.** You cannot create logical isolation between teams or projects within a single Vault cluster.
- **No disaster recovery or performance replication.** Community Edition operates within a single cluster. There is no built-in cross-region replication.
- **No Sentinel policies or control groups.** Fine-grained, policy-as-code governance is an Enterprise-only feature.
- **No HSM support.** Auto-unseal via hardware security modules and seal wrapping are not available.
- **No SLA or premium support.** You are limited to community forums and documentation.


Community Edition is a reasonable fit for development environments, proofs of concept, or small teams with straightforward secrets management needs. For production workloads at scale, most organizations will need to move to HCP Vault Dedicated or Vault Enterprise.


## Vault Enterprise (Self-Hosted)


Vault Enterprise is HashiCorp's self-hosted solution for organizations in regulated industries with specific security, compliance, and operational requirements. It runs the same binary as HCP Vault Dedicated but is deployed and managed entirely by your team.


It comes in three tiers: Standard, Plus, and Premium.


Feature Standard Plus Premium


Disaster recovery Yes Yes Yes


Multi-datacenter replication Yes Yes Yes


Namespaces Yes Yes Yes


HSM auto-unseal and seal wrap Yes Yes Yes


Multi-factor authentication No Yes Yes


Sentinel policies No Yes Yes


Control groups No Yes Yes


Secret sync No No Yes


Support level Premium Premium Premium


### Vault Enterprise Pricing: What We Know


HashiCorp does not publish Enterprise pricing. You have to talk to their sales team to get a quote, and the final number depends on your deployment size, client count, and compliance requirements.


That said, there is a consistent picture across community forums, review sites, and negotiation platforms that paints a clear picture of what to expect:


- **Low six figures is the baseline.** Multiple engineers on Hacker News report being quoted six-figure annual contracts for relatively small installations. One user[described getting a quote for a 100-token agreement](https://news.ycombinator.com/item?id=38336084) that landed "somewhere in the low six figures."
- **Per-client pricing adds up fast.** Vault Enterprise uses identity-based pricing where every pod, container, user, or service that authenticates counts as a "client." In Kubernetes-heavy environments with hundreds of microservices, this gets expensive quickly. A[former HashiCorp sales employee confirmed](https://news.ycombinator.com/item?id=39780253) that the pricing model was the single biggest barrier to closing deals internally.
- **Costs can dwarf your cloud bill.** One engineer[reported that their Vault Enterprise quote](https://news.ycombinator.com/item?id=39734395) was larger than their entire cloud spend, including other third-party enterprise tools billed through the marketplace.
- **Renewal price increases are well-documented.** HashiCorp is known for[raising prices at contract renewal](https://www.reddit.com/r/hashicorp/comments/12jtyzq/enterprise_price_increases_for_vault_renewal/) . This has been discussed across Reddit and other community forums, and it is a pattern that may continue or accelerate under IBM ownership.
- **Hidden costs add 25-60% on top.** According to[HashiCorp procurement data on Vendr](https://www.vendr.com/marketplace/hashi) , contracts often include hidden costs like support tier upgrades and professional services that inflate the initial quote significantly.


### The "Client" Definition Problem


The biggest frustration engineers raise about Vault Enterprise pricing is the[ambiguity around what counts as a "client."](https://discuss.hashicorp.com/t/hcp-vault-per-client-pricing/26968)


- Any pod or running container that authenticates to Vault counts as a separate client.
- Once a client token is claimed, it cannot be reused by a different client for the entire billing period (typically a year).
- This means you can burn through your client allocation with test environments, short-lived deployments, or architecture changes, even if those workloads are no longer running.


For organizations running Kubernetes at scale, this model creates unpredictable costs and makes it extremely difficult to budget accurately. As[one reviewer on PeerSpot noted](https://www.peerspot.com/questions/what-is-your-experience-regarding-pricing-and-costs-for-hashicorp-vault) : "They have some confusing terms... what they call a client is quite loose."


## HCP Vault Dedicated (Managed Version)


[HCP Vault Dedicated](https://developer.hashicorp.com/hcp/docs/vault/get-started/deployment-considerations/tiers-and-features) is a fully managed, cloud-hosted version of Vault Enterprise. It provides a single-tenant environment deployed on AWS or Azure, with automated backups, scalability options, and direct support from HashiCorp. You can choose your preferred region for data storage, which helps with data residency compliance.


Since the IBM acquisition, the tier names and structure have changed. The old "Starter" tier has been[discontinued as of August 2025](https://support.hashicorp.com/hc/en-us/articles/36950769223187-HCP-Vault-Dedicated-Starter-SKU-End-Of-Life) , and the old "Standard/Plus" naming has been replaced with **Development** , **Essentials** , and **Standard** .


### Cluster Pricing


All HCP Vault Dedicated clusters are[billed hourly](https://www.ibm.com/products/hashicorp/pricing) . Here is the current pay-as-you-go pricing from the official IBM HashiCorp pricing page:


Cluster Size Development Essentials Standard


Extra Small $0.62/hr (~$450/mo) N/A N/A


Small N/A $1.58/hr (~$1,152/mo) $1.84/hr (~$1,345/mo)


Medium N/A $3.16/hr (~$2,307/mo) $3.69/hr (~$2,694/mo)


Large N/A $7.49/hr (~$5,468/mo) $9.41/hr (~$6,870/mo)


On top of cluster costs, Essentials and Standard tiers charge **$72.92/month per client** (any unique application, service, or user that authenticates to Vault).


### Tier Comparison


The following table is based on HashiCorp's[official tier comparison](https://developer.hashicorp.com/hcp/docs/vault/get-started/deployment-considerations/tiers-and-features#compare-tiers) . It is not exhaustive of all features.


Feature Development Essentials Standard


All Vault community features Yes Yes Yes


Enterprise namespaces Yes Yes Yes


Terraform providers (HCP and Vault) Yes Yes Yes


Audit log and metric streaming No Yes Yes


Cross-region disaster recovery No Yes Yes


Version upgrade management No Yes Yes


No client limits No (max 25) Yes Yes


Performance replication No No Yes


Secrets sync No No Yes


Secrets import No No Yes


Sentinel policy support No No Yes


A few additional details from HashiCorp's tier descriptions that are not in the feature table but worth knowing:


- **Development** is a single-node instance. It does not include high availability, audit log downloads, or snapshot restoration. It is not covered by the[HashiCorp Cloud SLA](https://portal.cloud.hashicorp.com/sla) and is limited to Silver support with no Sev-1 coverage.
- **Essentials** is production-grade with a 99.9% SLA and Silver support. It includes audit log streaming, backup and restore, and version management.
- **Standard** includes everything in Essentials plus performance replication, Sentinel policies, control groups, and[advanced data protection](https://www.hashicorp.com/products/vault/advanced-data-protection) . It comes with Gold support.


### What This Actually Costs at Scale


The cluster pricing looks manageable at first glance. But the per-client charges are where costs escalate quickly.


For example, a Standard tier small cluster with 50 clients would cost roughly:


- Cluster: ~$1,345/mo
- Clients: 50 x $72.92 = ~$3,646/mo
- **Total: ~ $4,991/mo (~ $59,892/yr)**


That lines up with[procurement data from Vendr](https://www.vendr.com/marketplace/hashi) , which reports that a typical 50-client HCP Vault deployment lists at around $51,241 annually, though companies regularly negotiate 28-74% discounts depending on deal size. If you want to model your own numbers, our[Vault TCO calculator](https://infisical.com/vault-tco-calculator) walks through the same cluster and per-client math using your inputs.


A few other things worth noting:


- **Clients are counted monthly.** Once a client authenticates, it counts for the entire billing month, even if the workload is short-lived. You[cannot delete or reduce client counts](https://developer.hashicorp.com/hcp/docs/hcp/admin/billing/pricing-definitions) mid-cycle.
- **Contract pricing is available.** Essentials and Standard tiers offer contract-based pricing through sales, which can be more cost-effective than pay-as-you-go for predictable workloads.
- **No migration path from Vault Secrets.** If you were on HCP Vault Secrets, moving to Vault Dedicated is[a full migration](https://developer.hashicorp.com/hcp/docs/vault-secrets/migrate-vault) , not an upgrade. They are fundamentally different products.


## HCP Vault Secrets (SaaS Only) — Sunsetted


**Important: HCP Vault Secrets has been discontinued.** HashiCorp announced end of sale on June 30, 2025, with a final end-of-life date of July 1, 2026. Existing customers are being directed to migrate to HCP Vault Dedicated or Vault Community Edition.


We're including pricing details here for reference, since many teams are still evaluating alternatives during the migration window.


HashiCorp launched Vault Secrets in June 2023 as their first SaaS-only secrets management product. It offered three tiers: Free, Essentials, and Standard.


Feature Free Essentials Standard


Static secrets 25 2,500 25,000


Applications 25 1,000 10,000


Versions per secret 5 50 50


Secret sync destinations 5 200 2,000


Auto-rotating secrets No No Up to 5,000


Dynamic secrets No No Up to 5,000


Support level Bronze Silver Silver


### Vault Secrets Limitations


Even before the product was sunsetted, Vault Secrets had significant constraints that made it a tough fit for larger organizations:


- **Rate limits on all tiers:** 6,000 secret access requests per minute and 2,000 requests for other API operations.
- **Hard resource caps per app:** Each app could only manage 300 secrets, 10 dynamic secrets, and 15 syncs. Each project was limited to 100 apps.
- **No auto-rotation or dynamic secrets on lower tiers.** These features were locked to the Standard tier only.


These[resource restrictions](https://developer.hashicorp.com/hcp/docs/vault-secrets/get-started/plan-implementation/tiers-features#resource-limits) forced teams into architectural workarounds or multiple projects to accommodate even moderately sized deployments. For enterprise-scale infrastructure, Vault Secrets was never a realistic option.


## HashiCorp Vault Products at a Glance


With the sunsetting of HCP Vault Secrets and the Starter tier, here is what remains in HashiCorp's active Vault lineup and how they compare.


Vault Community HCP Vault Dedicated (Essentials) HCP Vault Dedicated (Standard) Vault Enterprise


**Deployment** Self-hosted Managed cloud (AWS/Azure) Managed cloud (AWS/Azure) Self-hosted


**Relative cost** Free $$ $$$ $$$$ (sales only)


**Starting price** $0 ~$1,152/mo + $72.92/client/mo ~$1,345/mo + $72.92/client/mo Contact sales


**High availability** Manual setup Managed by HCP Managed by HCP Manual setup


**Namespaces** No Yes Yes Yes


**Disaster recovery** No Cross-region Cross-region Yes


**Performance replication** No No Yes Tier-dependent


**Sentinel policies** No No Yes Tier-dependent


**Secrets sync** No No Yes Tier-dependent


**HSM support** No No No Yes


**SLA** None 99.9% 99.9% N/A (self-managed)


**Support** Community only Silver Gold Premium


**Best for** Dev/test, small teams Production workloads needing managed infra Orgs needing replication, governance, compliance Regulated industries with strict on-prem requirements


## Should You Use HashiCorp Vault?


Vault is a capable product with a long track record, but there are real trade-offs to consider, especially after the IBM acquisition and the recent product changes. Here is a summary of what to weigh before committing.


- **It is one of the most established secrets management tools on the market.** Vault has broad community adoption, extensive documentation, and support for a wide range of auth methods, secrets engines, and integrations.
- **There is no free production-ready tier.** Vault Community Edition is free but lacks enterprise features like namespaces, disaster recovery, and performance replication. The cheapest production-grade option (HCP Vault Dedicated Essentials, small cluster) starts at roughly[$1,152/month before client fees](https://www.ibm.com/products/hashicorp/pricing) .
- **Per-client pricing makes costs unpredictable at scale.** Every pod, container, service, and user that authenticates to Vault counts as a client at[$72.92/month each](https://www.ibm.com/products/hashicorp/pricing) . For organizations running Kubernetes with hundreds of microservices, this adds up fast and makes budgeting difficult.
- **Enterprise pricing requires a sales conversation.** There is no public pricing for Vault Enterprise (self-hosted). HashiCorp is[known for increasing prices at contract renewal](https://www.reddit.com/r/hashicorp/comments/12jtyzq/enterprise_price_increases_for_vault_renewal/) , and[hidden costs can add 25-60%](https://www.vendr.com/marketplace/hashi) on top of the initial quote.
- **Operational complexity is high.** Vault runs on proprietary Raft consensus for replication, requires specialized knowledge to deploy and maintain, and often demands[dedicated engineering staff](https://news.ycombinator.com/item?id=39734395) . Multiple enterprises have reported months-long onboarding timelines.
- **The product direction is now shaped by IBM.** Since the[acquisition closed in February 2025](https://newsroom.ibm.com/2025-02-27-ibm-completes-acquisition-of-hashicorp,-creates-comprehensive,-end-to-end-hybrid-cloud-platform) , HCP Vault Secrets was sunsetted, the Starter tier was discontinued, and response times in community channels have slowed. The long-term roadmap is being driven by IBM's hybrid cloud strategy.


## HashiCorp Vault Alternative: Infisical


[Infisical](https://infisical.com/) is an open-source platform for secrets, certificates, and privileged access management with[25,000+ GitHub stars](https://github.com/Infisical/infisical) . What started as a secrets management tool has expanded into an all-in-one platform covering secrets management, certificate lifecycle management (PKI), privileged access management (PAM), secrets scanning, and AI agent security.


Infisical is built on Postgres (not proprietary Raft), deploys anywhere (cloud, self-hosted, or multi-cloud), and is designed to deliver value on day one without months of onboarding or specialized certification.


For platform and DevOps engineers, the key differences from Vault include:


- **Out-of-the-box integrations.** 60+ native integrations with tools like[Kubernetes](https://infisical.com/docs/integrations/platforms/kubernetes) ,[Terraform](https://infisical.com/docs/integrations/frameworks/terraform) ,[Docker](https://infisical.com/docs/integrations/platforms/docker-intro) ,[Ansible](https://infisical.com/docs/integrations/platforms/ansible) ,[GitHub Actions](https://infisical.com/docs/integrations/cicd/githubactions) , and more. No custom orchestration required.
- **Built-in workflows.** Approval workflows, temporary access controls, secret rotation, and dynamic secrets come ready to use, not as frameworks you build on top of.
- **Intuitive UI and CLI.** Designed for both developers and security teams. You do not need CLI-only expertise or a custom-built dashboard to manage secrets.
- **Transparent pricing.** Free for up to 5 identities with up to 10 integrations. The[Pro plan](https://infisical.com/pricing) starts at $18/month per identity with up to 50 integrations. No per-client billing surprises.
- **Truly open source.** MIT-licensed, with a codebase the community can audit, contribute to, and verify. Not source-available under BSL.


Infisical is trusted by organizations including Hugging Face, LG, Volkswagen, Hinge Health, and HeyGen, as well as banks, pharmaceutical companies, and government agencies that demand strict compliance standards.


For a detailed feature-by-feature comparison, see[Infisical vs HashiCorp Vault](https://infisical.com/compare/infisical-vs-hashicorp-vault) .


### Infisical Pricing


Infisical is free for up to 5 identities with up to 10 integrations included, covering platforms like[GitHub Actions](https://infisical.com/docs/integrations/cicd/githubactions) ,[Vercel](https://infisical.com/docs/integrations/secret-syncs/vercel) ,[CircleCI](https://infisical.com/docs/integrations/secret-syncs/circleci) ,[Docker](https://infisical.com/docs/integrations/platforms/docker) ,[Kubernetes](https://infisical.com/docs/integrations/platforms/kubernetes) , and[Terraform](https://infisical.com/docs/integrations/frameworks/terraform) .


For features like[SAML SSO](https://infisical.com/docs/documentation/platform/sso/overview#sso-overview) ,[point-in-time recovery](https://infisical.com/docs/documentation/platform/pit-recovery#point-in-time-recovery) ,[SCIM](https://infisical.com/docs/documentation/platform/scim/overview#scim-overview) ,[temporary access controls](https://infisical.com/docs/documentation/platform/access-controls/temporary-access#temporary-access) ,[HSM integration](https://infisical.com/docs/documentation/platform/kms/hsm-integration#hsm-integration) , more integrations, or larger team sizes, the[Pro plan](https://infisical.com/pricing) starts at $18/month per identity. If you need help figuring out the right option, you can[talk to an expert](https://infisical.com/talk-to-us) .


## FAQ


### Is HashiCorp Vault free to use?


Yes, but with significant limitations.[Vault Community Edition](https://developer.hashicorp.com/vault/tutorials/get-started/available-editions) is free and self-hosted. It covers core secrets management features like KV storage, dynamic secrets, transit encryption, and a wide range of auth methods.


However, Community Edition lacks enterprise features like namespaces, disaster recovery, performance replication, Sentinel policies, and HSM support. It also has no SLA and no official support beyond community forums.


HCP Vault Secrets previously offered a free SaaS tier, but that product was[sunsetted in mid-2025](https://developer.hashicorp.com/hcp/docs/vault-secrets/end-of-sale-announcement) and is no longer available to new customers.


### Is HashiCorp Vault expensive?


For most organizations, yes. The cheapest production-grade managed option is an HCP Vault Dedicated Essentials small cluster at roughly[$1,152/month](https://www.ibm.com/products/hashicorp/pricing) , before adding per-client fees of $72.92/month for each authenticated identity.


Vault Enterprise (self-hosted) pricing is not published and requires talking to sales. Community reports consistently describe quotes in the[low six figures annually](https://news.ycombinator.com/item?id=38336084) for relatively small deployments, with[hidden costs adding 25-60%](https://www.vendr.com/marketplace/hashi) on top of the initial number. HashiCorp is also[known for raising prices at renewal](https://www.reddit.com/r/hashicorp/comments/12jtyzq/enterprise_price_increases_for_vault_renewal/) .


Beyond licensing, there is a significant operational cost. Vault requires specialized knowledge to deploy, maintain, and upgrade. Many organizations report needing dedicated engineering staff to manage their Vault infrastructure.


### What is the difference between Vault Community and Enterprise?


Vault Community Edition is free and covers the core secrets management use cases. Vault Enterprise is a self-hosted, paid product designed for organizations with strict security, compliance, and operational requirements.


Enterprise adds capabilities that Community Edition does not include:


- **Namespaces** for multi-tenant isolation within a single cluster.
- **Disaster recovery replication** to protect against data loss during system failures.
- **Performance replication** to distribute read workloads across regions and reduce latency.
- **Sentinel policies** for fine-grained, policy-as-code access control.
- **HSM auto-unseal and seal wrap** for hardware-backed key management.
- **Control groups** for requiring multiple approvals before accessing sensitive data.


Enterprise is divided into three tiers (Standard, Plus, and Premium), with certain features gated to higher tiers. Pricing is fully custom and requires[contacting sales](https://www.hashicorp.com/en/contact-sales) .
