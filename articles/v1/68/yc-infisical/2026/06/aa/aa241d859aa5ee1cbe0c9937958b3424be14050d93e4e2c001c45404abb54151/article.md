---
schema_version: "1.0.0"
document_id: "aa241d859aa5ee1cbe0c9937958b3424be14050d93e4e2c001c45404abb54151"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/azure-key-vault-secrets-management"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:26c73ed3e76527bf1bc8c4a42a1477061ba3903b122360c46c069c269ef7d8d2"
---

# Azure Key Vault & Secrets Management: The Complete Guide

Azure Key Vault is Microsoft's native service for storing secrets, keys, and certificates. For any team exclusively building on Azure, that makes it seem like an obvious choice for a[secrets manager](https://infisical.com/blog/secrets-management-complete-guide) :


- It connects to Microsoft Entra ID natively
- It authenticates workloads through managed identities
- It plugs natively into many Azure services


Like any other infrastructure, Azure Key Vault is great at some things but struggles with others. Security infrastructure migrations are painful, so choosing the right tooling is important to avoid future complications.


If even part of your infrastructure lives on Azure, it's worth understanding Key Vault's architecture, its access model, its peculiarities, and its limits before you commit.


## What is Azure Key Vault?


Key Vault is Azure's all-in-one vault. The naming hints at the design: while AWS and GCP split secrets management, key management, and[certificate management](https://infisical.com/blog/certificate-management) into separate products, Azure combines all three behind one service, one access model, and one audit trail.


That consolidation simplifies certain operational concerns, but can also make it more complex. The three jobs all involve sensitive data you don't want compromised, but are distinct in their use cases.


### Secret management in Azure Key Vault


Azure Key Vault's number one use case is centralized secret management: instead of hardcoding credentials in` .env` files, your application authenticates to the vault and retrieves credentials at runtime. This eliminates[secret sprawl](https://infisical.com/blog/what-is-secret-sprawl) and gives you a single audited place where access to passwords, API keys, and tokens is governed and logged.


### Certificate management in Azure Key Vault


Certificate management in Azure Key Vault covers the lifecycle of the TLS/SSL certificates that authenticate and encrypt traffic for internal services and public endpoints. Key Vault can generate certificates, integrate with certificate authorities, and track expiry and renewal to avoid certificate-related outages.


### Key management in Azure Key Vault


Key management in Key Vault generates and stores RSA and elliptic-curve keys and performs operations like signing, wrapping, and encryption without the private key ever leaving the vault to keep key material out of application code.


These three use cases share the same interface, but differ in use case, day-to-day operations, and often ownership.


Key Vault aims to be a comprehensive solution, but doesn't include configuration.


### Azure Key Vault vs. Azure App Configuration


Azure App Configuration (ACC) is typically used in conjunction with Key Vault, but doesn't overlap with it. ACC centralizes application settings and feature flags, while Key Vault stores secrets, keys, and certificates.


Both offer encrypted storage, but aren't the same:


- Sensitive data like API keys, access tokens, credentials, and other secrets belong in Key Vault.
- Configuration like endpoints, timeouts, and feature flags belong in App Configuration.


The reason to keep these separate is that their sensitivity differs: you don't want to leak your config, but it needs to be in the repo, stored in logs, etc. You can't do that with secrets. Configuration also tends to be long-lived while secrets can and should change frequently.


The best practice is to have App Configuration reference Key Vault items to keep the config in one place while the secret value never leaves the vault.


This trifurcation might sound confusing, but it all rests on a simple foundation.


## How Azure Key Vault works


A vault fundamentally does three things: it encrypts data, controls access, and lets workloads authenticate to it. For all but the most basic solutions, this is not the full feature set, but these three things are what turn a database into a functional vault.


Azure Key Vault does these in a unique way.


### Encryption


Every secret, certificate, or key in a vault is encrypted at rest. Key Vault defaults to standard software encryption, but the Premium tier offers Hardware Security Module (HSM) support. This is a security measure required by some regulations and compliance frameworks.


### Azure Key Vault access control: access policies vs. Azure RBAC


Access control affects both security and operational efficiency. Convenient, but overprovisioned access enlarges the blast radius of any potential breach, but complex policies with long approval flows can hinder engineering. The ideal policy follows secrets management best practices without slowing down engineering. Key Vault has two models for authorizing access.


Access policies are a legacy model that mainly exists for legacy implementations. It assigns access at the vault level, which is too broad. Anyone who needs one secret gets the entire vault. So if a website redesign contractor needs access to your CMS, they may suddenly have access to all frontend credentials (depending on the vault's scope). This violates the principle of least privilege, a best practice which dictates that anyone should have only the access they absolutely need, not what they might find useful.


Azure role-based access control (RBAC) is the default for new projects. RBAC lets you scope a role assignment to each secret object. It uses the same role model as the rest of Azure and enables granular role assignments.


RBAC splits access leanly by object type, which matters because most workloads only touch one. There are parallel data-plane roles for each:


- Key Vault Secrets User and Secrets Officer for secrets
- Key Vault Crypto User and Crypto Officer for keys
- Key Vault Certificates Officer for certificates
- Key Vault Reader for metadata-only access
- Key Vault Administrator for full data-plane control


Grant the role that matches the object type and access a workload needs, so a service that only reads one TLS certificate never gets near your signing keys.


A contractor working on a redesign could receive a specific role that grants read access to the CMS auth token without knowing the existence of any other credentials.


If you're adopting Azure Key Vault, use Azure RBAC, scoped tightly. From the CLI, that's a single assignment scoped to the resource:


```text
az role assignment create \
--role "Key Vault Secrets User" \
--assignee <managed-identity-object-id> \
--scope "/subscriptions/<sub>/resourceGroups/<rg>/providers/Microsoft.KeyVault/vaults/prod-payments-kv/secrets/db-password"


```


Scoping the assignment down to` /secrets/db-password` rather than the whole vault is best practice to prevent a single compromised service snowballing into a major incident. Vault-level access would give an attacker access to further secrets, which would escalate their power.


The only reason to use access policies nowadays is to maintain legacy implementations. Some niche use cases (notably Azure DevOps key vaults behind a private endpoint) still require the access policy model as well.


### Authentication with managed identities


An Azure resource (a VM, an App Service, an Azure Kubernetes Service pod) can be given a managed identity, an Entra ID identity that Azure manages and rotates automatically. The resource requests a token, presents it to Key Vault, and RBAC decides whether it can see a given secret, key operation, or certificate.


This solves the "secret zero" problem (the issue of how to authenticate to the thing that manages authentication). There's no bootstrap secret to store, which solves the chicken-and-egg problem. Always prefer a managed identity over a client secret or certificate for service-to-service authentication.


## Getting started in the portal


The fastest way to create your first object is the Azure portal. Create a Key Vault (choose a region, a tier, and importantly leave the access model on Azure RBAC), then open it.


Each object type has its own section:


- The Secrets blade adds a secret with a name and value.
- The Keys blade generates or imports a cryptographic key, where you pick the type (RSA or elliptic-curve) and size.
- The Certificates blade either generates a certificate against an issuance policy or imports an existing one, and from then on it tracks that certificate's lifecycle.


Naming convention matters. Names are unique within their store in a vault. A clear hierarchy that identifies the environment, service, and credential type such as` prod-payments-db-password` ) makes RBAC assignments, search, and auditing dramatically easier as your object count grows. The portal is fine for a first object and occasional inspection. For anything repeatable, move to the CLI or infrastructure as code.


## Secrets Management in the Azure CLI


Day-to-day work happens in the` az keyvault` commands, and the three object types share the same command shape. Setting a secret is one line:


```text
az keyvault secret set \
--vault-name prod-payments-kv \
--name db-password \
--value "REPLACE_ME"


```


Retrieving it is just as direct:


```text
az keyvault secret show \
--vault-name prod-payments-kv \
--name db-password \
--query value \
--output tsv


```


Keys and certificates have parallel command groups. Creating an RSA key:


```text
az keyvault key create \
--vault-name prod-payments-kv \
--name signing-key \
--kty RSA


```


And generating a certificate from the built-in default policy:


```text
az keyvault certificate create \
--vault-name prod-payments-kv \
--name tls-cert \
--policy "$(az keyvault certificate get-default-policy)"


```


The pattern holds across all three.` az keyvault secret` ,` az keyvault key` , and` az keyvault certificate` share the same structure, so what you learn for one transfers to the others. One caution worth internalizing for secrets management: any value passed inline on the command line can land in your shell history and in process listings. For sensitive values, use` --file` to read from a file and delete it afterward instead of typing it into the terminal.


## Using Key Vault from Kubernetes (AKS)


For teams running on Azure Kubernetes Service (AKS), the most popular integration is[External Secrets Operator](https://external-secrets.io/latest/) (ESO), a vault-agnostic operator that fetches secrets to Kubernetes. This simplifies much of the secrets logic teams would otherwise have to build and maintain themselves.


Another option is the Azure Key Vault provider for the Secrets Store Container Storage Interface (CSI) Driver. It mounts secrets, keys, and certificates from a vault directly into a pod as a CSI volume, so your application reads them from the filesystem without ever calling the Key Vault API itself.


The driver supports mounting multiple objects as a single volume, can sync mounted content into native Kubernetes secrets, and supports autorotation of mounted values. Authentication is handled through a managed identity (or workload identity / OpenID Connect), so the cluster authenticates as itself with no static credential in the manifest.


This is the cleanest way to get Key Vault objects into containerized workloads, and it's worth setting up early rather than retrofitting.


## Using Azure Key Vault for Azure DevOps secrets management


The most common reason teams adopt Key Vault is for Azure DevOps secrets management. The integration is purpose-built because it's a common use case:


1. In Azure Pipelines, you create a variable group and toggle on Link secrets from an Azure Key Vault as variables.
2. You pick a service connection and a vault, and the secret names are mapped into the group.
3. The values are fetched fresh from the vault at pipeline runtime, so nothing sensitive is stored in the pipeline definition.


This keeps plaintext secrets out of logs and disks and gives you full control over secrets management directly from DevOps.


There are a few limitations:


- The variable-group integration maps secrets only, not keys or certificates.
- The service connection needs at least` Get` and` List` permissions on the vault.
- RBAC vaults work over a public endpoint, but an RBAC vault behind a private endpoint isn't supported by Azure DevOps. That configuration requires falling back to the vault access-policy model.


## Managing Key Vault with Terraform


If you run infrastructure as code, Key Vault fits into Terraform through` azurerm_key_vault` plus a resource per object type:` azurerm_key_vault_secret` ,` azurerm_key_vault_key` , and` azurerm_key_vault_certificate` . A secret looks like this:


```text
resource "azurerm_key_vault_secret" "db_password" {
name         = "db-password"
value        = var.db_password
key_vault_id = azurerm_key_vault.payments.id
}


```


Keys and certificates follow the same shape with their own arguments, such as key type and size for` azurerm_key_vault_key` , or an issuance policy for` azurerm_key_vault_certificate` .


The same caveat applies with any cloud secrets manager: anything Terraform manages goes into Terraform state, and state stores values in plaintext. A password in your configuration lives in state and plan output. A key or certificate you generate through Terraform can expose sensitive data the same way.


Source values from variables marked` sensitive = true` , or create the object container with Terraform and write the actual value through a separate process so it never enters state. Treat your state backend as a secret store in its own right. ([Here's how we think about Terraform secrets generally.](https://infisical.com/blog/how-to-manage-secrets-on-terraform-using-infisical) )


## Rotating secrets, keys, and certificates with Azure Key Vault


Rotation is where the three use cases diverge most. A rotation means changing a secret, certificate, or key, either on a schedule or as part of incident response. Rotations are done to invalidate credentials in case they leak (or after they have leaked).


Azure Key Vault provides built-in rotation policies for keys and certificates: you can configure an auto-rotation schedule, and Key Vault manages renewal with integrated certificate authorities.


Azure Key Vault lacks a built-in secrets rotation engine. Key Vault gives every secret optional activation and expiration dates, but automatically generating a new credential and updating the downstream system requires building it yourself, typically with an Azure Function.


Homegrown functions and automations work, but do require maintenance. This is why many teams implement secrets management solutions like Infisical on top of Key Vault, which handles the automation without maintaining workarounds (while also giving you a centralized secrets management solution that works beyond Azure).


## Soft-delete and purge protection


Soft-delete is mandatory in Azure Key Vault and can't be turned off. Deleting a key, certificate, or an entire vault, puts it into a recoverable soft-deleted state for a customizable retention period of up to 90 days (90 by default) before it's permanently removed.


Purge protection is an even firmer policy: with it enabled, not even a vault administrator can hard-delete an object during the retention window. Once purge protection is on, it can never be turned off.


These are generally good policies, but a deleted object still counts against the global uniqueness of its name. You can't recreate a vault or object with the same name, which is a common CI/CD and Terraform headache. If secrets metadata is hardcoded, this can break downstream builds and cause outages.


Purge protection is great for resilience against ransomware and protects against accidental deletion. But it's a one-way door, so it's usually an obstacle for short-term testing vaults, but great protection for production vaults.


These characteristics make Azure Key Vault a solid secrets manager. Like any solution, its architecture and functionality does have drawbacks, especially if you ever move beyond Azure.


## Azure Key Vault pros and cons


Azure Key Vault combines three tools other providers often keep separate and is Azure-native. This means it has a unique set of benefits and drawbacks:


### Pros


- Consolidates secrets, keys, and certificates in one service, with certificate lifecycle management and key rotation built in.
- Managed identities remove the bootstrap-secret problem for Azure-to-Azure authentication entirely.
- Azure RBAC gives per-object access control, split cleanly by object type, using the same role model as the rest of Azure.
- Deep, native integrations across the Azure ecosystem: AKS via the CSI driver, Azure DevOps variable groups, App Service, and more.
- Soft-delete and purge protection provide strong resilience against accidental or malicious deletion.


### Cons


- Consolidation stops at the Azure boundary. There's no answer for workloads on other clouds, on-premises, or on other third-party SaaS, so multi-cloud teams re-fragment.
- Its all-in-one architecture can be confusing, as not all features work for all object types and neither keys nor secrets or certificates offer enough functionality to comply with best practices out of the box.
- Secret rotation is largely do-it-yourself, with no turnkey rotation.
- The access policies vs. RBAC duality can cause misconfiguration and keeps some legacy implementations on suboptimal security policies.
- No built-in approval workflows for accessing production credentials, no change management, and no unified cross-team audit view.
- The per-transaction pricing model can surprise high-throughput workloads.


## Cost and how to control it


Key Vault's charges per operation: It's a few cents per 10,000 transactions for secret and standard key operations. HSM-protected keys in Premium add a per-key monthly charge, and certificate renewals cost a few dollars per request.


What starts as a small bill can balloon at scale. If every request across an autoscaling fleet fetches a secret directly from the vault, you can generate millions of operations quickly. This also disincentivizes secrets management best practices, which typically require more secrets and more frequent operations.


Beyond monetary cost, Azure also applies a request limit to each vault per region, which can slow down operations. Workloads that exceed the limit are throttled via HTTP 429 responses rather than billed more.


High request rates make this an architecture constraint, which is the reason heavy consumers shouldn't treat the vault as a per-request lookup. Workarounds exist, but they add complexity to workflows and add additional homegrown logic that requires maintenance.


-> We go deeper in our[Azure Key Vault pricing guide](https://infisical.com/blog/azure-key-vault-pricing)


## Azure Key Vault best practices


**Default to Azure RBAC, scoped tightly.** Grant the role that matches what a workload actually touches, never blanket access. Reserve the access-policy model for the narrow cases (like DevOps private-endpoint vaults) that still require it.


**Scope RBAC by object type.** Give a workload a secrets, keys, or certificates role based on what it uses, so reading a TLS certificate never implies access to your signing keys.


**Authenticate with managed identities.** For any Azure-to-Azure access, use a managed identity so there's no bootstrap secret to store or rotate. Pair this with[secret scanning](https://infisical.com/docs/documentation/platform/secret-scanning/overview) across your repositories to catch the credentials that slip in by hand.


**Use a clear naming hierarchy.** A scheme like` <env>-<service>-<purpose>` makes RBAC, search, and auditing scale with your object count instead of fighting it.


**Enable purge protection on production vaults.** The one-way-door tradeoff is worth it for anything you can't afford to lose to accidental or malicious deletion.


**Cache reads and keeps config out of the vault.** This protects your latency, your bill, and your request quota, and keeps the vault focused on actual secrets, keys, and certificates. Push non-sensitive settings to App Configuration.


**Audit with Azure Monitor and diagnostic logs.** Route Key Vault logs to a Log Analytics workspace so you have a record of who accessed which object and when. It's both an operational tool and a compliance requirement under frameworks like SOC 2 and ISO 27001.


**Plan your secret rotation story up front.** Because secret rotation is do-it-yourself, design the Event Grid and Functions pipeline (or an external system) before you have hundreds of long-lived credentials to retrofit. Keys and certificates can lean on built-in rotation.


## When Azure Key Vault isn't enough


Key Vault's best quality is the all-in-one design. Putting secrets, keys, and certificates behind one service, one access model, and one audit trail is simpler than stitching together three separate tools.


But the all-in-one quality only holds as far as Azure goes. That's fine if you never plan on leaving Azure, but relying on a single provider is rare: 73% of organizations now run a hybrid cloud setup, according to[Flexera's 2026 State of the Cloud report](https://www.flexera.com/about-us/press-center/flexera-finds-cloud-value-is-rising-while-ai-waste-grows) . Many more want to diversify their infrastructure or at least preserve the option to do so.


The moment you run workloads on AWS, GCP, or on-premises, solutions fragment: You end up with three sets of parallelized infrastructure (for secrets, keys, and certificates) that require separate maintenance. Each requires its own access model and audit view.


Native integrations between these competing cloud providers don't exist and workarounds create brittle connections. Multi-cloud secrets management (or certificate or key management) always requires a dedicated solution that can handle all environments or integrates with all the cloud provider-native solutions (Infisical can do both).


More advanced workflows like secrets rotation or dynamic secrets require homegrown logic. Maintaining that logic becomes harder the more secrets you manage. Workflows also tend to be shallow, as even essential automations require workarounds:


- approval workflows for production credentials
- granular change management
- dynamic short-lived secrets generated on demand


### Why some teams add Infisical instead of (or on top of) Azure Key Vault


Many organizations adopt centralized[secrets management tools](https://infisical.com/blog/best-secret-management-tools) like Infisical to skip maintaining their own logic for advanced workflows, and to manage secrets, certificates, and keys across environments.[Infisical](https://infisical.com/) is an open-source identity security platform that applies Key Vault's all-in-one instinct across any infrastructure rather than one cloud. It covers the same three primitives Key Vault does, secrets management,[key management (KMS)](https://infisical.com/docs/documentation/platform/kms/overview) , and[certificate management](https://infisical.com/blog/certificate-management) , in a single platform that spans AWS, GCP, Azure, and on-premises with one access model and one audit view.


On top of that breadth, it closes the workflow gaps:


- a usable dashboard
- approval workflows for production access
- dynamic secrets that are generated on demand and expire on their own


Infisical is a simple-to-use security tool that's used by everyone from startups to enterprises.


Adopting it isn't a disruptive migration. Infisical's[Azure Key Vault sync](https://infisical.com/docs/integrations/secret-syncs/azure-key-vault) connects to existing vaults directly, so you can import the secrets you already have, keep them synchronized, and adopt centralized workflows without ripping anything out. It can also push secrets straight into[Azure DevOps variable groups](https://infisical.com/blog/azure-devops-secrets-management-with-infisical) with no pipeline changes.


Key Vault keeps doing what it's good at inside Azure, and Infisical becomes the consistent layer across everything else. If you're actively weighing options, our rundown of[Azure Key Vault alternatives](https://infisical.com/blog/azure-key-vault-alternatives) puts the landscape side by side.


[Try Infisical today for free](https://app.infisical.com/signup) or[book a demo](https://infisical.com/talk-to-us) to see it in action.
