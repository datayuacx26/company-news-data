---
schema_version: "1.0.0"
document_id: "932e0d64b6d12cb732a435c9fbd1f45ae265c04944642f21792995f1205d015a"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/secrets-manager-pricing"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:05d170fc1342ebdef0ef2de05a7812267846b9eb08966945e94f8fd6293407c9"
---

# Secrets Manager Pricing: How Much Do Security Tools Cost in 2026?

The pricing of[secrets management tools](https://infisical.com/blog/best-secret-management-tools) looks straightforward: Some charge per user, others per secret stored, per user, per API call, per cluster, or combinations.


But pricing pages can’t capture the engineering time required, the indirect costs of security breaches, and operational slowdowns from inefficient processes. This factors into the total cost of ownership, even if it doesn’t show up on an invoice.


If you’re currently evaluating secrets management tools, it’s worth understanding the most important players’ pricing schemes and what they look like in practice, what traps exist, and realistic scenarios.


## How secrets manager pricing models work


Most tools use one or a combination of a few pricing models:


**Per-secret/per-operation pricing** charges you for each secret stored and sometimes for each API call made against it.[AWS Secrets Manager](https://infisical.com/blog/aws-secrets-manager-complete-guide) ,[Azure Key Vault](https://infisical.com/blog/azure-key-vault-secrets-management) , and[GCP Secret Manager](https://infisical.com/blog/gcp-secret-manager) all use variants of this model. It scales with secrets volume and access patterns, which means cost can grow quickly as your product and team grows.


**Seat-based pricing** charges per user or identity. Doppler and Infisical use this model. Costs don’t scale with secret volume, which requires less cost monitoring and obviates certain considerations around best practices.


**Client-based pricing** is a variant of seat-based where the unit is any client connecting to the service, including machine identities like service accounts and CI/CD runners. Akeyless uses this model.


**Infrastructure-based pricing** charges for the underlying cluster or compute resources. HCP Vault Dedicated uses this model. Your cost depends on the cluster size you provision, regardless of how many secrets you store or how many users you have.


**Contract pricing** is used by most providers in some capacity, featuring custom, non-public pricing that requires talking to sales. This works for deeply customized implementations.


Most hidden costs live outside any of these models: ops overhead, onboarding time, and the engineering investment needed to get the tool doing what you actually need.


## Infisical pricing


Infisical’s secrets management pricing is distinct from its prices for[PAM](https://infisical.com/docs/documentation/platform/pam/overview) or[PKI](https://infisical.com/docs/documentation/platform/pki/overview) .[Secrets management](https://infisical.com/blog/secrets-management-complete-guide) uses identity-based pricing, where an identity is any human user or machine identity (service account, API key, bot, CI/CD runner) that uses the platform.


**Open-source:** Infisical’s[open-source secrets manager](https://github.com/Infisical/infisical) is free to use.


**Free:** Infisical’s free tier is $0/month for up to five identities. It includes the dashboard UI, CLI, SDKs, Kubernetes Operator, Infisical Agent, all integrations, secret referencing and overrides, secret scanning, and community Slack support. Supports both Infisical Cloud and self-hosting.


**Pro:** $18/identity/month. Adds secret versioning, point-in-time recovery, role-based access controls, secret rotation, temporary access provisioning, SAML SSO, IP allowlisting, and 90-day audit log retention.


**Enterprise:** Custom pricing. Adds dynamic secrets, dedicated infrastructure, enterprise SCIM, LDAP authentication, approval workflows, access requests, Gateways, KMIP, KMS and HSM support, audit log streaming, custom rate limits, user groups, custom roles, and a 99.99% SLA.


What Pro pricing looks like at different identity counts:


Identities Monthly cost


Up to 5 (Free) $0/mo


10 $180/mo


25 $450/mo


50 $900/mo


100 $1,800/mo


Infisical is open source, and self-hosting is supported across all tiers. Roughly half of Infisical's deployments are self-hosted, including large enterprises that require full control over their infrastructure.


Infisical includes many advanced features that other vendors lack (and which teams frequently end up building and maintaining in-house), which significantly lowers the indirect cost of maintaining it as a secrets manager and the resources required to operate it.


## AWS Secrets Manager pricing


AWS Secrets Manager’s pricing is usage-based at its core. It charges $0.40 per secret per month and $0.05 per 10,000 API calls.


A small production application will run somewhere between $5 and $25 per month at moderate API call volumes. That's cheap enough for smaller companies, but can still get expensive:


- Secrets replicated across regions are billed per region
- More complex infrastructure (e.g. Kubernetes, Terraform) can skyrocket API calls and secret count


AWS publishes its own examples:


Scenario Secrets API calls/day Monthly cost


Small web app 15 ~135 total/day ~$6


Mid-size team 1,500 20/secret/day ~$605


Large org 10,000 40/secret/day ~$4,060


These example numbers look relatively tame, but secrets management at scale may look different in reality. A mid-size team may realistically have 1,500 secrets, but pay for a multiple of that when those secrets are replicated across multiple environments and regions.


AWS Secrets Manager is a low-friction choice if you're exclusively on AWS, but also entrenches your dependency. AWS Secrets Manager doesn’t provide integrations to workloads on Azure, GCP, or your own infrastructure. This fragments the security ops of any multi-cloud setup because it provides no unified view, audit trail, or access model.


It’s possible to connect AWS Secrets Manager to non-AWS tools with complex Lambda functions, but these workarounds will be brittle. Beyond the engineering resources this consumes, the maintenance and potential errors in secrets syncs would likely raise the operational burden.


Most teams building multi-cloud secrets management use centralized secrets managers like Infisical to either circumvent cloud-provider-native secrets managers or act as a layer that orchestrates them via native integrations.


## Azure Key Vault secrets management pricing


Azure Key Vault unites secrets management, key management, and[certificate management](https://infisical.com/blog/certificate-management) , but its most common use case is secrets management. Azure Key Vault generally charges per operation, not per secret. You don’t pay per secret, but are billed for each operation.


**Standard tier:**


- Secrets operations: $0.03 per 10,000 transactions
- Certificate operations: $0.03 per 10,000 transactions
- Certificate renewals: $3.00 per renewal request


**Premium tier (HSM-protected keys):**


- RSA 2048-bit keys: $1.00 per key per month
- RSA 3072/4096-bit and ECC keys: $5.00 per key per month
- All operation charges still apply on top


At low operation volumes, Azure Key Vault is inexpensive for secrets (sub-dollar amounts for small teams). Storing Hardware Security Module (HSM)-protected keys or issuing many certificates changes this calculus.


Azure Key Vault is designed for its own ecosystem. It natively integrates with Microsoft Entra ID for authentication and other Azure services, but requires workarounds as soon as you move beyond Azure. Additionally, Key Vault doesn’t have native[secrets rotation](https://infisical.com/docs/documentation/platform/secret-rotation/overview) , requiring you to write your own logic. This can increase the effective cost, as you maintain homegrown features on top of Key Vault.


## GCP Secret Manager pricing


[GCP Secret Manager](https://infisical.com/blog/gcp-secret-manager) offers a small free tier, then per-version and per-access-operation charges.


**Free tier (per account per month):**


- Six active secret versions
- 10,000 access operations
- Three rotation notifications


**Beyond the free tier:**


- Active secret versions: $0.06 per version per month
- Access operations: $0.03 per 10,000 operations
- Rotation notifications: $0.05 per notification


GCP's example for 250 active secrets with 50,000 access operations per month works out to roughly $15-$20 per month, making it competitive at a modest scale. Like AWS and Azure’s solutions, GCP Secret Manager works best for GCP-native workloads and creates the same cross-cloud management fragmentation.


Without proper setup, it’s easy to overpay for GCP Secret Manager.[One user described](https://www.nektarinsights.com/insights/gcp-secret-manager-costs) GCP Secret Manager reaching 55% of their entire Google Cloud bill for a modest Flask app. The issue wasn’t extortionate pricing: an unfortunate setup caused too many events and secret versions. This is easily avoidable, but testament to the fact that you need to monitor costs and optimize custom logic when paying for usage.


## HashiCorp Vault pricing


Vault is a capable legacy solution in secrets management. In general, Vault's pricing has two distinct paths: open-source self-hosted and managed cloud (HCP Vault Dedicated).


### Vault open source (self-hosted)


Vault’s OSS version (also called community edition) is tricky. While it’s “free” on GitHub, the infrastructure to run it, the engineers to operate it, and the work of the initial configuration aren’t.


Vault is not a ready-to-use product, but a key-value store with an API. It means teams build most day-to-day things themselves: workflows, access UIs, rotation automation, and change management processes. This can take months and requires so much domain knowledge that some engineers build their careers on being Vault engineers. Many larger companies have multiple engineers exclusively working on Vault.


Larger companies that switch from Infisical to Vault frequently spend more than a million a year on Vault without ever getting an invoice from HashiCorp (or IBM, which acquired HashiCorp).


### HCP Vault Dedicated (managed cloud)


Since IBM's acquisition of HashiCorp closed in early 2025, HCP Vault Dedicated pricing is listed in HashiCorp's Flex consumption pricing table. There are two main pricing vectors:


- An hourly rate per cluster, based on edition, cluster size, cloud provider, region, and support level
- A monthly per-client fee for unique active clients, where a client is a unique application, service, or user that consumes HCP Vault Dedicated


Development clusters are priced separately and are intended for non-production use. Production clusters use the Standard or Plus editions, and client fees are tiered rather than a single flat rate. As of the current Flex table, Silver Support client pricing starts at $112.168/client/month for the first 1-9 clients, then $86.826/client/month for the next 10-24 clients, with lower unit prices at higher volumes.


While Vault Dedicated offers many turnkey features the open source version lacks, it remains a complex product that requires configuring your own access policies, secret engine, auth, and more.


Cluster size Tier Hourly rate Est. monthly, cluster only*


Development Development $0.030/hr ~$22/mo


Small Standard $1.578/hr ~$1,152/mo


Medium Standard $3.163/hr ~$2,309/mo


Large Standard $7.489/hr ~$5,467/mo


Small Plus $1.843/hr ~$1,345/mo


Medium Plus $3.692/hr ~$2,695/mo


Large Plus $9.406/hr ~$6,866/mo


*Cluster cost only, calculated at 730 hrs/month, excluding per-client fees. Rates shown are representative Silver Support rates from HashiCorp's Flex table and may vary by region, support level, contract, taxes, and product availability.


A small production deployment with a Standard small cluster and 10 unique active clients would cost roughly $2,250/month before taxes and any additional contract-specific charges: about $1,152/month for the cluster plus about $1,096/month in client fees.


HCP Vault Enterprise and contract pricing may require contacting sales.


(If HCP Vault Dedicated is one of the options you're weighing, our[Vault TCO calculator](https://infisical.com/vault-tco-calculator) breaks down the cluster and per-client math in more detail.)


### HashiCorp Vault pricing after the IBM acquisition


Something worth noting is that IBM's acquisition of HashiCorp could affect pricing. Post-acquisition, IBM has already cut a Starter tier and sunset HCP Vault Secrets, which was the previous entry-level product.


[Users complained](https://www.reddit.com/r/IBM/comments/1lgbq51/no_more_hcp_vault_secrets_what_is_your_cost/) about IBM shutting down this critical service with little notice and leaving them without security architecture or forcing them into vastly more expensive tiers.


It’s uncertain whether IBM plans future price raises, but the new ownership seems focused on enterprise deals, with[others complaining](https://www.reddit.com/r/hashicorp/comments/1m0yz3w/hashicorp_vault_enterprise_renewal/) about decreased support from their account team and increasing prices at renewal.


## Doppler pricing


Doppler is a closed-source secrets management tool that uses seat-based pricing across three tiers:


**Developer:** Free for up to three users, $8/month for each additional user. Includes the Doppler CLI, service tokens, five config syncs, and three days of activity log retention.


**Team:** $21/user/month (14-day free trial). Adds SAML SSO, role-based access controls, automatic secret rotation, service accounts, 90-day audit log retention, and up to 100 config syncs.


**Enterprise:** Custom pricing. Adds dynamic secrets, enterprise key management (EKM), custom roles, enterprise SCIM, log forwarding, and a 99.95% Service Level Objective (SLO).


Doppler recently launched an on-prem version of its product. On-prem pricing requires a demo, so it’s likely that this is enterprise only.


What Team pricing looks like at different team sizes:


Team size Monthly cost


10 users $210/mo


25 users $525/mo


50 users $1,050/mo


100 users $2,100/mo


Doppler is closed source, and its self-serve product is cloud-based. For teams with data residency requirements, compliance programs that require self-hosted deployments, or a preference for open-source auditability, this can make it less workable unless the Enterprise on-prem option fits their requirements.


The tiered subscription model is transparent and produces a simple overview, but can become a problem if you need feature configurations outside of that exact bundle.


## Akeyless pricing


Akeyless uses a client-based pricing model, where a client is any human user, application, or server that connects to the service. Multiple instances of the same application count as one client.


**Free plan:** Up to five clients, 500 static secrets, five dynamic secrets, five rotated secrets, one gateway cluster, and three days of audit log retention.


**Enterprise:** Custom pricing, contact sales. Includes full secrets management as well as advanced encryption, authentication and other enterprise features.


Akeyless offers pure, cloud-hosted SaaS and hybrid SaaS (which includes a self-hosted gateway). It does not offer a fully self-hostable solution where everything lives on the customer’s infrastructure.


## CyberArk Conjur pricing


CyberArk is one of the oldest security vendors, with Conjur being its secrets management product. Palo Alto Networks announced its acquisition of CyberArk in 2025 and completed it in 2026. CyberArk's secrets management products remain enterprise-oriented and relatively opaque from a public pricing standpoint.


The current structure can be confusing, but the public landscape is roughly:


- CyberArk Conjur’s open-source version still exists, though it has limited public release activity compared with newer secrets managers.
- CyberArk offers enterprise secrets management products for SaaS and self-hosted deployments.
- Public self-serve pricing for CyberArk secrets management is not generally available, so most buyers should expect a sales-led quote.


Because pricing is not published in a simple self-serve table, any third-party or marketplace numbers should be treated as directional rather than definitive. As a legacy enterprise security vendor, CyberArk has historically served large organizations with larger security budgets, and the real cost can increase if you require other CyberArk products, enterprise support, or broader identity-security features.


For teams comparing secrets managers, the main takeaway is that CyberArk/Conjur pricing is custom and opaque. It may fit organizations already standardized on CyberArk, but it is hard to model accurately without a vendor quote.


## How usage-based pricing can affect your security posture


Usage-based pricing for secrets managers has a structural problem: practices that improve your security posture tend to increase your bill. It’s best practice to frequently rotate secrets (invalidating a credential and adding a new one). Many forms of usage-based pricing disincentivize this exact behavior. The same is true for dynamic secrets (short-lived, one-time use credentials), using many tightly-scoped secrets over one broad credential, and so on.


As organizations grow, so do secret and identity counts. This is normal, but can skyrocket when you adopt:


- Autoscaling/ephemeral workloads
- Replication across environments/regions
- CI/CD pipelines


Provider-native secrets managers rarely cause the types of expenses LLM APIs or EC2 instances do. But many of the costs of secrets managers don’t show up as a line item.


## What pricing doesn't capture


What a vendor charges you and the actual cost of a secrets manager are not the same. Total Cost of Ownership (TCO), the actual economic impact of secrets management, is often much higher than the invoice. A few factors can change the real cost:


The first is that operating the tool itself can make it more expensive. Especially complicated tools like HCP Vault require ongoing and specialized engineering attention: upgrades, backup configuration, and any manual workflows add to the bill. Some ops overhead is unavoidable, but your choice of secrets manager lowers or increases it. There’s a massive cost difference in a DevOps engineer dedicating a few hours a week vs. hiring additional engineers for your security infrastructure.


Setup and integration is another factor that can increase the TCO. Building your actual secret policies, workflows, and automations takes resources. This cost is much lower if you don’t need to build and maintain your own integrations or manually build workflows which you can simply toggle on in other tools.


Vendor lock-in can become a big cost if you become too dependent on any one ecosystem or secrets manager. For instance, the cloud-native tools by AWS, GCP, and Azure are low-friction entry points, but switching costs accumulate. If you eventually need to switch, migrations are more expensive the more custom logic you’ve built and more disruptive the more entrenched the workflows are.


Outages and operational slowdowns are another cost. A secrets manager that frequently breaks down will cause constant engineering friction and outages. A cheap tool can become very expensive if it doesn’t do its job well (or just doesn’t work for your use case).


The[best secrets manager](https://infisical.com/blog/best-secret-management-tools) choice is the one that feels like the best infrastructure for you: something you can build on for the long-term and that you have confidence in.


## Is Infisical the best-priced secrets manager?


Infisical covers the full secrets lifecycle and offers automations that other vendors require you to wire up yourself (e.g. secret rotations, dynamic secrets, and Kubernetes secrets management). For most customers, it also requires fewer engineering resources than other sophisticated secret managers.


Infisical works for everyone. It’s beloved by everyone from hobbyists who keep API keys out of git to global corporations who secure multi-cloud secrets setups with thousands of identities.


The pricing structure mirrors this: The basic features are free, while Pro offers competitive self-serve pricing for growing teams. Enterprise offers what large companies need, with custom pricing to reflect sophisticated requirements.


You can start[using Infisical for free](https://infisical.com/talk-to-us) , or[talk to the team](https://infisical.com/talk-to-us) if you're evaluating at scale.
