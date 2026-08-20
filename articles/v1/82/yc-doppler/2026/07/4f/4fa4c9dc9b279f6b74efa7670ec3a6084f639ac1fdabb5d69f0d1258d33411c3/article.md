---
schema_version: "1.0.0"
document_id: "4fa4c9dc9b279f6b74efa7670ec3a6084f639ac1fdabb5d69f0d1258d33411c3"
company_key: "yc-doppler"
company: "Doppler"
source_id: "yc-doppler-news-import-86fd2de2d678"
canonical_url: "https://www.doppler.com/blog/migrate-from-hcp-vault-secrets-to-doppler"
published_at: null
first_seen_at: "2026-07-25T01:53:43.668103+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:6add37fe9c44fd4055917de5955ea466e3f426f8f2121c043f8f1e4ac5fca28a"
---

# HashiCorp Vault Secrets is going away. What now?

It is no longer news that HCP Vault Secrets is shutting down on July 1, 2026. But what does this mean for teams that built their secrets workflows around its simplicity, flat structure, and developer-friendly experience?


HashiCorp's recommended path is HCP Vault Dedicated, a single-tenant Vault Enterprise cluster that starts at roughly $1,152 per month for a production-grade configuration, with additional per-client fees on top. HCP Vault Dedicated requires HCL policies, namespace design, auth method configuration, and unseal key management. None of that was part of the HVS experience.


Given these changes, now is a good time to evaluate your migration options. We'll explain HashiCorp's recommended path, then show why many teams may find migrating to Doppler to be the simpler long-term solution.


## What HashiCorp is recommending and what it costs


When you look at the criteria to get HVD running, you'll realize it's a completely different product from HVS. Even Infisical's published analysis concurs with this: "No migration path from Vault Secrets. If you were on HCP Vault Secrets, moving to Vault Dedicated is a full migration, not an upgrade. They are fundamentally different products."


The migration requires that you provision a single-tenant HCP Vault cluster dedicated to HVD, generate a cluster admin token through the HCP portal, and manually configure an external authentication method. Authentication involves setting up OpenID Connect (OIDC), JSON Web Token (JWT), Kubernetes auth, or a cloud-native IAM role from providers like AWS or Azure.


Beyond that, you have to write custom HCL policies for each capability set (such as read, write, and update), then attach those policies to specific auth roles with strict time-to-live (TTL) settings. You also have to manually enable and mount the key-value (K/V v2) secrets engine within the admin namespace and remap every soon-to-be-deleted HVS application to the new secrets path or namespace you have set up.


Finally, you have to update every application/workload (like GitHub Actions, Terraform, Kubernetes via VSO, and AWS Lambda) consuming secrets to point away from the HVS API endpoint to the path-based Vault API you set up.


This setup also hints at how different the two products are operationally. Here is a table for a quick glance:


Capability HCP Vault Secrets (Sunsetted) HCP Vault Dedicated


Static secrets


Managed SaaS experience


Supported


Auto-rotating secrets


Supported


Supported


Dynamic secrets


Limited beta support


Full Vault secrets engines


Secrets sync


Native sync destinations


Requires Vault integrations/configuration


Kubernetes integration


VSO HVS source


Vault auth + VSO/Vault integration


Pricing model


Per-secret SaaS pricing


Cluster pricing + per-client fees


Operational overhead


Minimal


High


The pricing model becomes more expensive once you move into production. While an extra-small HVD cluster costs about $450/month ($0.62/hour), it comes with no production SLA, making it unsuitable for live applications.


The smallest production-ready option, Essentials Small, starts at $1,152/month ($1.58/hour). From there, pricing increases with authenticated clients, which are billed at $72.92 each. Since authenticated clients include applications, CI runners, and services, costs can grow quickly. For example, an Essentials Small deployment with 50 authenticated clients costs nearly $4,800/month:


- Base cluster: $1,152/month
- 50 authenticated clients: $3,646/month
- Total: ~$4,798/month


HVD is not necessarily the wrong option to switch to, but your workflow actually has to need the features it provides for this overhead to be justified. For teams that truly require Vault's dynamic secrets engines, Hardware Security Module (HSM) auto-unseal, and other Vault Enterprise features, HVD might be the correct path. Otherwise, you will be heading in the opposite direction from the one that led you to choose HVS initially.


## Why most HVS users chose HVS in the first place


Teams that picked HVS did so to avoid tightly integrating their workflows with native Vault features, whereas HVD requires you to run Vault directly. HVS users didn’t need to know about HCL policy authoring, namespace design, unseal key ceremonies, auth method configuration, integrated Raft storage management, or TLS certificate provisioning. When you adopted HVS, you chose to offload much of this technical overhead.


For example, setting up secrets for GitHub followed a few steps: creating an App in the UI, adding the secret name and value, then syncing with GitHub in a few clicks. There was no need to write authorization wrappers or strict HCL policies.


HVS really is a developer-facing software-as-a-service (SaaS) tool for small- to mid-sized teams, not meant to be an enterprise secrets engine. Its[documented limits](https://developer.hashicorp.com/hcp/docs/vault-secrets/get-started/plan-implementation/tiers-features#resource-limits)


, such as up to 300 secrets per app, 15 sync destinations per app, and 100 apps per project, reinforce that.


The gap between what HVS required and what HVD introduces is the entire operational surface of Vault Enterprise. If you chose HVS to avoid that surface, you definitely wouldn’t want to use HVD. In reality, you would look for an alternative that delivers a similar developer experience.


## Doppler as an HVS alternative


The HVS architecture that you are used to isn't completely out of your reach. It just lives in a different dashboard. Doppler preserves that SaaS-hosted,[developer-friendly secrets management model](https://www.doppler.com/doppler-vs-vault)


that HVS users chose. Here is how similar they are.


First, HVS applications, used to segment environment variables, map directly to Doppler projects and configs. With it, you can separate configs by different environments such as development, staging, and production. This maintains that flat, scannable structural layout, instead of an endless directory tree.


HVS roles such as App Manager and App Secrets Reader are similar to Doppler's workplace roles and project-level permissions. These Doppler permissions allow you to restrict access to production environments to senior engineers, while keeping the other environments open to the rest of the engineering team.


Just as HVS secrets sync destinations such as AWS Secrets Manager, GitHub Actions, Vercel, and GitLab, Doppler provides integrations for the same destinations and more. It actually supports 30-plus native integrations.


You might have relied on the HVS Vault Secrets Operator to map to local cluster workloads. Doppler also supports the Doppler Kubernetes Operator or the External Secrets Operator, configured to use Doppler as a ClusterSecretStore to achieve similar mapping.


Even the HVS CLI can be replaced by the Doppler CLI. You can use doppler run --\[command\]


for runtime injection, or doppler secrets get


for file mounting.


For an IaC layer, Doppler's Terraform Provider offers similar workflows to the HVS Terraform Provider.


Even the cost difference between the two tools is proportional to team size, preventing massive single-tenant pricing. HVS offers a free tier that allows up to 25 secrets, and a $0.50-per-secret-per-month standard tier. Doppler has a free Developer plan for up to three users and a Team plan starting at[$21 per user per month](https://www.doppler.com/pricing)


.


You can see the similarity in how Doppler and HVS affect operations. Shifting to Doppler gives you a similar experience and operational surface. The table below summarizes how they map.


HVS concept Doppler equivalent Operational notes


Application


Project & Configs


Applications map to Projects, while environments map to Configs.


App Roles


Workplace/Project Roles


Role-based access control boundaries remain similar.


Secrets Sync


Native Integrations


Supports integrations for AWS, GitHub, Vercel, GitLab, and more.


Kubernetes Integration (VSO)


Doppler Kubernetes Operator / ESO


Replaces HVS-based Kubernetes secret syncing workflows.


HCP CLI


Doppler CLI


Supports runtime injection and secret retrieval workflows.


Terraform Provider


Doppler Terraform Provider


Supports Infrastructure-as-Code secret management workflows.


Dynamic Secrets (Beta)


Dynamic Secrets


Supports dynamic secret generation.


Free Tier / $0.50 per secret/mo


Free Tier / $21 per user/mo (Team)


Pricing scales more gradually with team size.


Once you've decided that Doppler aligns with your workflow, the next step is planning the migration.


## How to migrate from HCP Vault Secrets to Doppler


For most teams, the migration from HVS to Doppler can be completed within a day. The steps usually are exporting your secrets, importing them into Doppler, and rewiring your integrations. Make sure you are authenticated into both the HCP and Doppler CLIs before starting the migration.


### Step 1: The export phase


Use the hcp vault-secrets secrets list


command with the --app


flag to target a specific HVS application and output its secrets. Adding the global --format=json


flag dumps the keys, metadata, and payloads into a local JSON file. The full command has to be run against each distinct HVS application to produce a separate export file on your machine.


### Step 2: The structure phase


Access the Doppler dashboard or use IaC to create an individual Doppler project for each HVS application. Doppler automatically creates a structure where each environment (development, staging, production) within the project has a config. This mirrors the HVS environment model: HVS staging environments map to Doppler staging configs, just like every other environment. You can create more configs if that better mirrors your HVS structure.


Doppler project structure showing separate configs for development, staging, and production environments within a single project. This layout mirrors the environment segmentation commonly used in HVS applications.


### Step 3: The import phase


You can conveniently import secrets into Doppler using either the Doppler CLI or the dashboard. For the CLI, use the doppler secrets upload


command, pointing it at the JSON file from the export phase. For the dashboard, use the import feature, which accepts[.env file contents](https://www.doppler.com/blog/environment-variables-secrets-management)


, JSON, or YAML pasted directly into the interface. The panel below summarizes the export and import process between HVS and Doppler.


### Step 4: The rewire phase


For each sync destination, replace the HVS connection with the Doppler equivalent. For GitHub, replace the hashicorp/hcp-vault-secrets-action


integration and configure Doppler to automatically push variables to the repository environment. Another option is to use Doppler's dopplerhq/secrets-fetch-action@v2


GitHub Action to[avoid hardcoding secrets](https://www.doppler.com/blog/remove-hardcoded-secrets-github-actions)


or storing long-lived DOPPLER_TOKEN


values in workflow configurations. Similar replacement steps go for the other integrations, be it Terraform, Kubernetes, etc.


In practice, for most small to mid-sized teams, steps one through three take under an hour. Step 4 depends on the number of integrations and may take a full day for teams with many applications and workloads consuming secrets. For secrets hygiene, rotate any secrets that were stored in HVS after the import, since they may have been accessible in plain text during the export step.


Once the migration is complete, the final step is to verify that every integration, secret path, and consuming workload has been safely moved away from HVS and integrated into Doppler. These are the steps to take if your goal is to maintain a workflow close to the HVS experience.


## Migration checklist


Use this checklist before, during, and after migration to Doppler, to confirm that you completed the process correctly before HVS officially sunsets on July 1, 2026.


Before migration:


- Export all secrets from every HVS application using the HCP CLI.
- Document every integration consuming HVS secrets, including GitHub Actions, Terraform, Kubernetes via VSO, AWS, Vercel, or other sync destinations.
- Note which HVS features you actively used, such as static secrets only, auto-rotating secrets, dynamic secrets, or secrets sync. This determines whether Doppler or HVD is the right migration target.


During migration to Doppler:


- Create Doppler Projects and Configs matching your HVS application and environment structure.
- Import secrets from the exported JSON using doppler secrets upload


or the dashboard import.
- Replace each HVS sync destination with the Doppler equivalent integration.
- For Kubernetes users, remove the HVS source from the Vault Secrets Operator and configure the Doppler Kubernetes Operator or External Secrets Operator instead.
- Update all CI/CD workflows, Terraform configurations, and application startup commands to reference Doppler rather than the HVS API.
- Test all integrations and workloads in staging before cutting over to production environments.


After migration:


- Verify that no application is still reading from the HVS API.
- Set up Doppler audit logs to confirm all expected secret access is occurring correctly.
- Rotate any secrets that were stored in HVS as a hygiene step, since they may have been exposed in plain text during the export process.
- Decommission or delete unused HVS applications before the July 1, 2026, shutdown deadline.


## Time to migrate


You don't have to trade a simple developer experience for enterprise complexity. Doppler makes it easy to migrate your secrets, connect your existing integrations, and keep developers productive with a familiar workflow. Talk to our team


to start your migration to Doppler today.
