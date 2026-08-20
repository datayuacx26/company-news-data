---
schema_version: "1.0.0"
document_id: "f39fe39e903fbfd12b0f235f1d8a6a3cca9ebf4102b39795d755d0ecb82993c5"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/terraform-ephemeral-resources"
published_at: "2025-02-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:7d7efc3e2617741946970dcb21db33f78325d8b60b4ee3c95d7307915f394937"
---

# Everything you Need to Know About Terraform's Ephemeral Resources

> **TL;DR** : Terraform v1.10's ephemeral resources are temporary resources that don't persist in state/plan files. They exist only during execution, making them ideal for handling secrets, dynamic tokens, or any data that shouldn't be stored. Currently supported by Infisical, AWS, Azure, GCP, and Kubernetes providers.


Whether you are using Terraform for managing cloud infrastructure or on-premises resources, you've likely encountered the challenge of managing secrets securely. Secrets such as private keys, certificates, and API tokens are essential to operate your infrastructure - whether authenticating with cloud providers, accessing protected resources, or establishing secure connections between services.


Until now, these secrets in Terraform have been stored in plaintext in both plan and state files, introducing significant security risks if these files are compromised or accidentally exposed.


**Ephemeral resources,** a new concept introduced in Terraform v1.10, aims to address these security concerns. True to their name, they are never stored in any artifact, solving the long-standing problem of secret values being persisted in plaintext, and reducing your attack surface.


While currently supported by only a few providers, including[Infisical](https://infisical.com/docs/integrations/frameworks/terraform) , this feature represents a significant step forward for infrastructure security.


Let's explore this important security advancement and walk through a practical example that will help strengthen your security posture.


## What Are Ephemeral Resources?


Ephemeral resources are temporary Terraform-managed entities introduced in[Terraform v1.10.](https://www.hashicorp.com/en/blog/terraform-1-10-improves-handling-secrets-in-state-with-ephemeral-values) Unlike standard resources or data sources, they are never persisted in the Terraform state file, making them ideal for handling sensitive or short-lived data like secrets, temporary credentials, or dynamic tokens. There are actually two language constructs that can be "ephemeral":


- **Ephemeral resources:** these are a new resource block which declare that something needs to be created or fetched separately for each Terraform phase, then used to configure some other ephemeral object, and then explicitly closed before the end of the phase.
- **Ephemeral input and output variables:** when a variable or an output is marked` ephemeral = true` , its value will only exist temporarily, such as a short-lived token or session identifier. It acts as a guarantee that the corresponding value will not appear in plaintext in the state file.


### Key Characteristics:


- **Temporary Lifecycle** : Ephemeral resources exist only while Terraform needs them. Terraform creates or retrieves them on demand, renews them if needed (like extending a secret lease), and destroys them when they’re no longer required.
- **State Protection** : Terraform excludes ephemeral resource values from state and plan files to keep sensitive data secure.
- **Limited Usage** : You can only reference ephemeral resources in temporary contexts like` locals` ,` provider` settings, or other` ephemeral` resources.


## Benefits of Ephemeral Resources


Just as DevOps and platform engineers have grown accustomed to the operational benefits of "ephemeral" environments, ephemeral resources apply the same "use-and-dispose" approach. This strategy helps teams implement robust security practices while maintaining the automation and efficiency that makes Terraform great. It's a paradigm shift in Terraform's security model that enhances your infrastructure-as-code practices in several ways:


1. **More secure** : These short-lived resources minimize the exposure window for sensitive information, significantly reducing security risks.
2. **More efficient** : By automatically destroying unused resources after execution, teams can optimize cloud costs and prevent wasteful resource consumption.
3. **More flexible** : Teams can dynamically manage resources based on runtime needs - whether it's injecting temporary secrets, running infrastructure tests, or handling bootstrapping processes.
4. **GitOps-ready** : The approach aligns perfectly with GitOps workflows by replacing static secrets in version control with ephemeral references.


*Read our*[GitOps Secrets Management Best Practices](https://infisical.com/blog/gitops-secrets-management)


## Example: Injecting Secrets Using Infisical


The following example demonstrates using an ephemeral resource to dynamically fetch a database password through the[Infisical provider](https://registry.terraform.io/providers/Infisical/infisical/latest/docs/ephemeral-resources/secret) and configure a PostgreSQL provider without persisting secrets in the state:


```text
# Configure Infisical provider
terraform {
required_providers {
infisical = {
source = "infisical/infisical"
}
}
}


provider "infisical" {
# Authentication handled via e.g, GitHub Actions OIDC
}


# Fetch secret ephemerally
ephemeral "infisical_secret" "db_creds" {
name         = "DB_CREDENTIALS"
env_slug     = "prod"
workspace_id = var.infisical_workspace_id
folder_path  = "/"
}


# Decode secret and configure PostgreSQL provider
locals {
credentials = jsondecode(ephemeral.infisical_secret.db_creds.value)
}


provider "postgresql" {
host     = data.aws_db_instance.example.address
port     = data.aws_db_instance.example.port
username = local.credentials["username"]
password = local.credentials["password"]
}


# Use the secret in infrastructure (e.g., RDS)
resource "aws_db_instance" "example" {
password = local.credentials["password"]
# ...
}


```


## How It Works:


- The` ephemeral` block fetches the secret` DB_CREDENTIALS` from Infisical during Terraform execution.
- The secret is decoded into a` local` value, which is ephemeral by association.
- The PostgreSQL provider uses the secret dynamically, avoiding storage in state files.


As an added bonus, using Infisical’s OIDC integration ensures credentials are short-lived and never persisted in CI/CD logs. A full example including OIDC authentication for a GitHub Action is available[here](https://infisical.com/blog/gitops-secrets-management#1:~:text=1.%20Infrastructure%20Provisioning%20with%20Terraform) .


## Supported Providers


Since this feature is relatively new, only a select few providers like Infisical currently support these resources. Here's a list of major providers you can use to create ephemeral resources:


Provider Ephemeral Resources


[Infisical](https://github.com/Infisical/terraform-provider-infisical)` infisical_secret`


[AWS](https://github.com/hashicorp/terraform-provider-aws)` aws_secretsmanager_secret_version` ,` aws_lambda_invocation` ,` aws_kms_secrets` ,` aws_cognito_identity_openid_token_for_developer_identity`


[Azure](https://github.com/hashicorp/terraform-provider-azurerm)` azurerm_key_vault_secret` ,` azurerm_key_vault_certificate`


[Google Cloud](https://github.com/hashicorp/terraform-provider-google)` google_service_account_access_token` ,` google_service_account_id_token` ,` google_service_account_jwt` ,` google_service_account_key`


[Kubernetes](https://github.com/hashicorp/terraform-provider-kubernetes)` kubernetes_token_request` ,` kubernetes_certificate_signing_request_v1`


## Best Practices


- **Limit References:** Only use ephemeral resources in allowed contexts (e.g.,` provider` blocks,` locals` ).
- **Combine with OIDC** : Use OpenID Connect (OIDC) for secure, tokenless authentication in CI/CD pipelines.
- **Monitor Lifecycles:** Ensure ephemeral resources are closed promptly to avoid lingering access.


## Conclusion


Ephemeral resources address critical security and operational gaps in Terraform by enabling temporary, state-excluded data handling. They are particularly powerful for secrets management in GitOps workflows, as demonstrated by the Infisical example. As more providers adopt this feature, ephemeral resources will become a cornerstone of secure, dynamic infrastructure-as-code practices.


For further details, refer to Terraform’s official documentation or Infisical’s[integration guide](https://infisical.com/docs/integrations/frameworks/terraform) .
