---
schema_version: "1.0.0"
document_id: "6ad29065ecce79a87912a6275afb4fdf92158d1235ffb0238cead688eecefc7f"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/terraform-and-opentofu-provider-for-authzed-dedicated"
published_at: "2025-10-30T10:40:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:076c069585059495f5f955596435c49075651277cfc62bcb31b1a7c18af0c9ec"
---

# Terraform and OpenTofu Provider for AuthZed Dedicated

Today, AuthZed is excited to introduce the Terraform and OpenTofu Provider for AuthZed Dedicated, giving customers a powerful way to manage their authorization infrastructure using industry standard best practices.


With this new provider, teams can define, version, and automate their resources in the AuthZed Cloud Platform - entirely through declarative infrastructure-as-code. This makes it easier than ever to integrate authorization management into existing operational workflows.


## Why It Matters


Modern infrastructure teams rely on Terraform and OpenTofu to manage everything from compute resources to networking and identity. With the new AuthZed provider, you can now manage your authorization layer in the same way — improving consistency, reducing manual configuration, and enabling repeatable deployments across environments.


## What You Can Manage


The Terraform and OpenTofu provider automates key components of your AuthZed Dedicated environment, including:


- Service Accounts - Create and manage programmatic access to your permission systems
- API Tokens - Securely provision and rotate tokens for authentication
- Roles and Policies - Define and apply fine-grained access control
- Permissions System Configuration - Maintain visibility and control over your authorization models


And we’re working to support additional resources in AuthZed Dedicated environments, including managing Permissions Systems.


### Example Usage


Below is a simple example of how to create a service account using the AuthZed Terraform provider:


hcl


1


2


3


4


5


6


7


8


```text
provider "authzed" {
token = var.authzed_token
}


resource "authzed_service_account" "example" {
name        = "ci-cd-access"
description = "Service account for CI/CD pipeline"
}


```


hcl


1


2


3


4


5


6


7


8


```text
provider "authzed" {
token = var.authzed_token
}


resource "authzed_service_account" "example" {
name        = "ci-cd-access"
description = "Service account for CI/CD pipeline"
}


```


This snippet demonstrates how straightforward it is to manage AuthZed resources alongside your existing infrastructure definitions.


## Seamless Integration


The introduction of the Terraform and OpenTofu provider makes it effortless to manage authorization infrastructure as code — ensuring your permission systems evolve safely and consistently as your organization scales.


For AuthZed customers interested in using the Terraform and OpenTofu provider, please contact your account manager for access.


To explore the provider and get started, visit the[AuthZed Terraform Provider on GitHub](https://github.com/authzed/terraform-provider-authzed) .


Not an AuthZed customer, but want to take the technology for a spin? Sign up for[AuthZed Cloud](https://authzed.com/cloud/signup) today to try it out.


On this page


- Why It Matters
- What You Can Manage
- Example Usage
- Seamless Integration


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
