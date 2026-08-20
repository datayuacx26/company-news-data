---
schema_version: "1.0.0"
document_id: "637fca2428c1e5b1f13ba238d6b9752007729f884813e2d2b11e4248188b23b3"
company_key: "pinterest-inc-class-a-common-stock"
company: "Pinterest Inc."
source_id: "pinterest-inc-class-a-common-stock-rss-45b43224fdd9"
canonical_url: "https://medium.com/pinterest-engineering/securing-infrastructure-at-scale-introducing-pinterests-resource-provisioner-pipeline-rpp-8283bb12cbe5"
published_at: "2026-07-22T16:01:02+00:00"
first_seen_at: "2026-07-22T17:07:30.054190+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:dfc8b981c37c40abc06cdd5ed4b0c34407f40dd70a6312b960fe7e64351e2a58"
---

# Securing Infrastructure at Scale: Introducing Pinterest’s Resource Provisioner Pipeline (RPP)

Pinterest


Engineering


Infrastructure As Code


Terraform


Cloud Infrastructure


# **Securing Infrastructure at Scale: Introducing Pinterest’s Resource Provisioner Pipeline (RPP)**


[Pinterest Engineering](https://medium.com/@Pinterest_Engineering?source=post_page---byline--8283bb12cbe5---------------------------------------)


6 min read


·


6 days ago


--


Ammar Ekbote | Senior Software Engineer
Chan Kim | Senior Software Engineer


Managing Infrastructure as Code (IaC) across a massive organization comes with a unique set of security and logistical challenges, particularly when operating within a distributed, multi-repository architecture. At Pinterest, we designed the **Resource Provisioner Pipeline (RPP)** , our specialized, proprietary Terraform execution engine to safely manage both critical and non-critical infrastructure changes.


In this post, we will look under the hood of the first iteration of the **RPP** system. We will explore how it established compliance and provides robust security guardrails for our global AWS operations by utilizing dual controls, centralized GitHub Actions execution, and a secure role-chaining mechanism. If your engineering team is looking to add strict guardrails to a GitHub workflow-driven Terraform setup, our architecture might serve as a helpful blueprint.


## What is RPP?


RPP delivers a secure, standardized CI/CD workflow for deploying and managing Pinterest’s foundational AWS infrastructure. Today, the system manages **hundreds of Terraform workspaces** that collectively govern **tens of thousands of resources** . This includes:


- **Security Policies:** Managing critical IAM roles and access policies.
- **Networking Infrastructure:** Configuring VPCs, Security Groups, Load Balancers, and DNS.
- **Storage & Compute:** Provisioning S3 buckets (including access controls) and Kubernetes clusters.


## The RPP GitHub Workflow


### The Multi-Repo Challenge


Pinterest’s Terraform code is currently distributed across multiple repositories, each owned and maintained by a completely different team. While we have an ongoing initiative to consolidate these into a unified mono-repository, RPP acts as a vital bridge that securely supports this legacy multi-repo structure.


### Centralized Execution Model


To maintain absolute control over infrastructure states, RPP enforces a centralized execution model for all code changes:


- **Workflow Invocation:** The deployment pipeline is automatically triggered by specific Pull Request (PR) events, such as a PR being created, updated, or commented on.
- **Centralized Composite Actions:** Execution relies on a central set of GitHub Actions (collectively called the RPP). These composite actions step in to operate directly on the PR.
- **Workspace Splitting:** Because a single PR can cross boundaries and affect multiple workspaces at once, RPP isolates the impact by executing individual *plans* or *applying* workflows for each workspace independently.
- **Dual Controls:** To eliminate accidental or malicious configurations, all code changes are bound by dual controls. An approved code reviewer on the respective repository must sign off, providing an extra layer of human scrutiny.


Press enter or click to view image in full size


## Deep Dive: Enforcing Least Privilege with Secure Role-Chaining


Allowing a CI/CD system broad permissions to execute cloud changes is a massive security risk. To uphold the principle of least privilege, RPP implements a strict **workspace-path-role mapping** process using secure role-chaining as shown below.


Press enter or click to view image in full size


Let’s break down the entire workflow step by step.


## Step 1: Assuming the RPPActionsRole


The system begins by assuming a centralized identity known as the` RPPActionsRole` RPPActionsRole. To ensure complete call authenticity, this role can *only* be assumed from specific, pre-authorized GitHub workflows. This restriction is strictly enforced at the cloud provider layer through GitHub OpenID Connect (OIDC) token validation.


Press enter or click to view image in full size


## Step 2: Workspace Property Determination


Once running within the context of the` RPPActionsRole` , the pipeline checks out a central **source-of-truth configuration file** to look up the exact properties of the workspace being altered. This file maps the workspace to its allowed repository name, directory path, team name, and execution role:


The following block illustrates how a team’s workspace maps to the root module code path. This mapping serves to ensure that only the associated code paths are permitted to execute against that specific workspace.


```text
team-abc = {      email       = "teamabc@pinterest.com"      description = "team ABC"      workspaces = [        {          name              = "workspace1"          description       = "dev configurations"          working_directory = "terraform/config-dev"   <--- repository path hosting root module code for workspace1          github_repo       = "pinterest/repo1"        },        {          name              = "workspace2"          description       = "prod configurations"          working_directory = "terraform/config-prod"  <--- repository path hosting root module code for workspace2          github_repo       = "pinterest/repo1"        }      ]      agent_iam_roles = [        "arn:aws:iam::<account-id>:role/RPP-ExecutionRole-TeamABC",      ]    }
```


## Step 3: Strict Backend Validation


The pipeline includes a critical workspace validation check to ensure that the code path strictly references the explicit S3 state backend blocks and KMS keys mapped to that workspace.


For example, the root module code for` workspace1` is located in` pinterest/repo1/terraform/config-dev` and the backend block is a function of the workspace name. If a developer mistakenly copies code or alters the backend block to point to` workspace2` while executing inside` workspace1` ’s directory, the validation logic catches the discrepancy and instantly fails the build.


```text
terraform {    backend "s3" {      bucket     = "pinterest-tf-team-abc"      key        = "workspace-state/rpp-workspace1/terraform.tfstate"      region     = "us-east-1"      kms_key_id = "alias/pinterest-tf-team-abc"    }    required_providers {      aws = {        source  = "hashicorp/aws"        version = "~> 5.73.0"      }      random = {        source  = "hashicorp/random"        version = ">=3.4.3"      }    }  }
```


The workspace validation logic would, in this scenario, guarantee the integrity of the utilized backend block.


## Step 4: Down-Scoping to the Team Execution Role


If validation passes, the` RPPActionsRole` assumes the designated, down-scoped` team_iam_role` . By linking specific code paths to specific workspaces and roles, we ensure that changes are made in the right environment with the absolute minimal IAM permissions required.


## Plan, Review, and Apply Execution


With the localized` team_iam_role` assumed, the deployment process moves forward with fine-grained precision:


1. **Linting** : The workspace initializes, and` terraform fmt` runs automatically to preemptively surface any syntax or formatting bugs.
2. **Planning** : The pipeline executes` terraform plan` and publishes the plan output directly as a comment on the GitHub PR. If the plan fails, the GitHub check is marked as failed, blocking the PR from being merged.
3. **Granular Control** : The Terraform configuration code itself specifies which role to assume, letting engineering teams retain exact control over role-to-workspace mapping.
4. **Deliberate Application** : Once the plan is approved, applying the changes requires an explicit comment on the PR (e.g., triggering the apply workflow). This intentional step guarantees that code owners are consciously authorizing infrastructure changes. The apply workflow runs through the exact same secure role-chaining process, triggers` terraform apply` , and posts the final output back to the PR for maximum visibility.


## Elevating Safety & The Benefits of Centralization


Beyond access management, the PR-driven workflow gives us a powerful platform to introduce advanced reliability and security practices:


- **Vulnerability Scanning:** Every PR automatically triggers static analysis checks via custom Semgrep rules alongside Helix/AI integrations to intercept insecure infrastructure configurations before they leave development.
- **Pre-Deployment Local Testing:** To test highly complex architectures safely, several teams leverage LocalStack within the pipeline to mock and preview live AWS behavior prior to hitting real cloud environments.


By consolidating our deployment logic into centralized composite execution steps, Pinterest achieves significant operational advantages:


- **Unified Auditing:** We can consistently track deployment actions across every infrastructure repository in the company.
- **Instant Systemic Patches:** If a widespread vulnerability occurs (such as a runner shell vulnerability), we can roll out a fix in a single centralized place instead of modifying hundreds of repositories.
- **Path-to-Workspace Enforcement:** Strict path mappings are locked down globally.
- **Resource Metrics:** We can centrally track and record statistics on newly added, modified, or deleted cloud resources.
- **Policy and Tag Enforcement:** Centralization sets the stage for future capabilities, such as automated tag enforcement across all cloud resources using Terraform.


## What’s Next?


By requiring dual controls, isolating execution environments, and using OIDC-driven role chaining, RPP provides a reliable framework for executing enterprise-scale infrastructure updates safely.


This is only the beginning of our IaC evolution. In upcoming blog posts, we share insights into a next-generation RPP engine designed for even more robust deployments. Stay tuned!
