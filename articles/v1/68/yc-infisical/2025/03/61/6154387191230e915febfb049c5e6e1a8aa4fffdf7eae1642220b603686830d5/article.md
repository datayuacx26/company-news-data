---
schema_version: "1.0.0"
document_id: "6154387191230e915febfb049c5e6e1a8aa4fffdf7eae1642220b603686830d5"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/gitlab-secrets"
published_at: "2025-03-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:20625a9bb0daac565bf12136bd03cf38467f12059dd436f4c74be35a5e965932"
---

# Secrets Management in GitLab CI/CD

GitLab CI/CD provides a powerful platform for automating software delivery pipelines, but managing secrets securely within these requires striking the right balance between security, automation, and developer experience. In this guide, we'll explore best practices for managing secrets in GitLab CI/CD and review integration patterns with **Infisical** to help you build a secure and efficient CI/CD pipeline.


## Challenges of Managing Secrets in CI Systems


CI systems typically rely on variables to execute their jobs, either to control the behavior of jobs and[pipelines](https://docs.gitlab.com/ci/pipelines/) , to store common values, or to avoid hard-coding them in the file declaring the instructions, which in the case of GitLab CI is the` .gitlab-ci.yml` file.


However, for sensitive variables like secrets, the CI systems' built-in mechanism for storing and exposing these values through environment variables is neither secure nor practical. **You should replace it with proper secrets management as soon as possible to prevent issues as your operations or team grows** .


It's clear why[CI systems aren't designed for storing secrets](https://infisical.com/blog/secrets-management-cicd) :


- They don't allow **centralizing your secrets** in one secure location, leading to secret duplication across projects and increasing your supply chain attack surface.
- They don't provide **fine-grained access controls** needed to follow the least-privilege principle.
- They lack **automated rotation mechanisms** , creating significant operational overhead when secrets need rotation.
- They don't offer **audit capabilities** required for compliance and incident response.
- They're often not secure enough and become a weak link if compromised, as shown by the 2022 CircleCI breach that exposed all their customers' secrets.


Today's cloud systems need a better way to handle secrets. Let's look at how to use these ideas with Infisical, a new secrets management tool built for modern CI/CD workflows.


## A Brief Introduction to Infisical


The secret management landscape has long been dominated by HashiCorp Vault, but since[HashiCorp switched Vault from MPL to BSL license in 2023](https://infisical.com/blog/hashicorp-new-bsl-license) , many teams have been searching for alternatives. Infisical has emerged as the natural choice for developers, offering several key benefits that make it an attractive option:


- **Developer-first approach** that scales with your team
- **All-in-one solution** for provisioning, versioning, access control, and rotation
- **Intuitive UI/CLI** for seamless development workflows
- **Adaptable architecture** that fits your existing infrastructure
- **Open-source** with MIT license (self-hostable)
- **Actively maintained** with 17k+ GitHub stars
- **Extensive integrations** across CI/CD platforms and tools


## Understanding Infisical's Core Features


Infisical provides several powerful features that make it particularly well-suited for GitLab CI/CD environments:


1. **Environment-specific secrets** - Separate secrets for development, staging, and production environments
2. **Secret referencing** - Reuse secrets within other secrets using syntax like` ${SECRET_NAME}`
3. **Granular access controls** - Control who can access which secrets at a fine-grained level
4. **Audit logging** - Track all access and modifications to secrets
5. **Multiple authentication methods** - Support for OIDC, tokens, and service accounts


## Building a Secure GitLab CI/CD Pipeline with Infisical


Let's walk through the process of integrating Infisical with GitLab CI/CD, step by step. We'll explore two primary integration patterns:


1. **[Native Integration](https://infisical.com/docs/integrations/cicd/gitlab#standard)** - Syncing secrets to GitLab CI/CD variables
2. **[Pipeline Integration](https://infisical.com/docs/integrations/cicd/gitlab#pipeline)** - Using secrets directly from Infisical with OIDC authentication


### 1. Native Integration: Quick Start


The standard integration automatically synchronizes secrets from Infisical to GitLab CI/CD variables, which makes it easy to get started with minimal configuration:


1. In the[Infisical dashboard](https://infisical.com/blog/app.infisical.com) , navigate to your project, select Integrations > Native Integrations and click "Add Integration"
2. Select "GitLab" from the integrations list
3. Authorize the integration to sync secrets from Infisical to GitLab
4. Choose the environment (e.g., Development, Staging, Production)
5. Select your GitLab project
6. Optionally, configure prefix/suffix settings in the "Options" tab
7. Save the integration


Once configured, Infisical will automatically create CI/CD variables in your GitLab project, add the prefix/suffix to the variable names in GitLab, and synchronize the secrets. This makes the secrets available to your CI/CD pipelines as normal environment variables:


While this approach provides a simple getting started experience, **it still stores secrets in GitLab CI/CD** . For enhanced security, let's explore a more advanced integration pattern.


### 2. Pipeline Integration with OIDC Authentication


For maximum security, we can use the Infisical CLI directly within our pipeline, authenticated via[OpenID Connect (OIDC)](https://infisical.com/docs/documentation/platform/identities/oidc-auth/gitlab) . This eliminates the need to store secrets in GitLab CI/CD at all.


#### Setting Up OIDC Authentication


First, we need to configure OIDC authentication between GitLab and Infisical:


1. In Infisical, navigate to **Organization Settings** > **Access Control** > **Identities**
2. Create a new identity with the role "Member"


1. Delete the default "Universal Auth" configuration
2. Add a new "OIDC Auth" configuration
3. Configure the audience and custom claims as needed


> Read more about
>
>
> - [Creating and adding an identity to a project, then using the identity to access Infisical within GitLab CI](https://infisical.com/docs/documentation/platform/identities/oidc-auth/gitlab#guide)
> - [Using GitLab OpenID Connect as an authentication provider](https://docs.gitlab.com/administration/auth/oidc/)
> - [GitLab id_tokens](https://docs.gitlab.com/ci/yaml/#id_tokens)


#### Using Infisical in GitLab CI/CD Pipelines


Now, we can *delete the synchronized secrets in GitLab CI/CD variables* and configure a pipeline to use the Infisical CLI with OIDC authentication:


```text
image: python:3.13


stages:
- test


before_script:
- apt update && apt install -y curl
- curl -LsSf https://astral.sh/uv/install.sh | sh
- curl -1sLf 'https://dl.cloudsmith.io/public/infisical/infisical-cli/setup.deb.sh' | bash
- apt-get update && apt-get install -y infisical
- pip install uv


test:
id_tokens:
INFISICAL_ID_TOKEN:
aud: https://infisical.example.com
script:
- export INFISICAL_TOKEN=$(infisical login --method=oidc-auth --machine-identity-id=<your-machine-identity-id> --oidc-jwt=$INFISICAL_ID_TOKEN --silent --plain)
- infisical run --projectId=<your-project-id> --env=dev -- uv run -- pytest


```


This pipeline:


1. uses a Python image as the base, and installs Infisical CLI and UV (used for testing) and dependencies.
2. Sets up a GitLab ID token for OIDC authentication
3. Installs the Infisical CLI
4. Logs in using OIDC authentication
5. Uses` infisical run` to inject secrets as environment variables for the subsequent command` uv run -- pytest`


This pipeline:


1. Installs the Infisical CLI and UV (used for testing) on a Python base image.
2. Logs in the Infisical CLI using the` INFISICAL_ID_TOKEN` . Make sure the` machine-identity-id` and` aud` match the values from Infisical.
3. Uses` infisical run` to inject secrets from a project + environment as environment variables for the subsequent command` uv run -- pytest`


This means secrets are injected on the fly without being stored in GitLab CI at all! This approach brings several significant security advantages:


- Secrets are never stored in GitLab CI/CD, reducing the exposure to supply chain attacks
- Authentication uses short-lived tokens instead of long-lived credentials
- Secret access is limited to the specific commands that need them
- All access is logged in Infisical's audit system


### 3. Infrastructure Provisioning with Terraform


When it comes to infrastructure provisioning, Infisical integrates seamlessly with Terraform to manage cloud credentials and other sensitive infrastructure data. Terraform 1.10 introduced a powerful new feature called "ephemeral resources" that perfectly complements GitLab CI/CD security practices.


Ephemeral resources are temporary resources that don't persist in Terraform state or plan files - they exist only during execution, making them ideal for handling secrets. The[Infisical Terraform provider](https://infisical.com/docs/integrations/frameworks/terraform) supports this feature, allowing you to securely access secrets during infrastructure provisioning without storing them in state files.


For a detailed explanation of Terraform ephemeral resources and how to use them with Infisical, check out our in-depth guide:[Everything You Need to Know About Terraform's Ephemeral Resources](https://infisical.com/blog/terraform-ephemeral-resources)


### 4. Kubernetes Deployments with Infisical


For GitOps-based continuous deployment workflows, Infisical offers multiple integration options with Kubernetes to ensure secure and effective secrets management. When deploying applications to Kubernetes using GitLab CI/CD, you can leverage these approaches to maintain GitOps principles while enhancing security.


Infisical provides three primary integration methods for Kubernetes:


1. **Infisical Kubernetes Operator** - A native Kubernetes solution with custom resources for syncing, pushing, and managing dynamic secrets
2. **External Secrets Operator** - Integration with the popular ESO framework for fetching secrets from external providers
3. **Secrets Store CSI Driver** - Direct pod-level secret mounting without intermediate Kubernetes secrets


Each approach offers different tradeoffs between declarative configuration, security, and operational complexity. The ideal solution depends on your specific requirements and existing infrastructure.


For a comprehensive guide to implementing secure GitOps workflows with Kubernetes and detailed examples of each integration option, see our detailed article:[Secure GitOps Workflows: A Practical Guide to Secrets Management](https://infisical.com/blog/gitops-secrets-management)


## Security Best Practices for GitLab CI/CD Secrets Management


### 1. Implement Least Privilege Access


**Why it matters:** Security breaches often expand through excessive permissions. Least privilege access minimizes your attack surface and contains potential security incidents.


**Key considerations:**


- Create granular service accounts for different pipeline stages
- Use temporary elevated access for administrative tasks
- Regularly audit and revoke unused permissions
- Implement clear access request procedures


### 2. Regularly Rotate Secrets


**Why it matters:** Static secrets become vulnerable over time through exposure and credential sharing. Regular rotation reduces the impact of potential breaches.


**Key considerations:**


- Use OIDC authentication to eliminate long-lived credentials
- Implement automated rotation schedules
- Use short-lived credentials whenever possible
- Maintain secret versioning
- Plan for emergency rotation procedures


### 3. Enable Audit Trails


**Why it matters:** Without proper audit trails, detecting and responding to security incidents becomes nearly impossible. Audit logs help maintain compliance and enable incident investigation.


**Key considerations:**


- Enable detailed logging in Infisical:


- Track all secret access
- Monitor configuration changes
- Log administrative actions


- Set up alerts for suspicious activities
- Maintain log retention policies


### 4. Secure CI/CD Pipeline Configuration


**Why it matters:** CI/CD pipelines have access to both source code and production environments, making them attractive targets for attackers.


**Key considerations:**


- Secure GitLab CI/CD configurations
- Implement branch protection
- Use OIDC for authentication
- Require reviews for sensitive changes
- Scan dependencies and containers
- Maintain separate environments for staging and production


## Why Use a Dedicated Secrets Management Service?


Native secret management solutions like GitLab CI/CD variables work well within their ecosystems. However, using a dedicated secrets management service as your single source of truth strengthens your architecture and saves time across common scenarios:


1. **Centralized management** provides a single place to control and audit all secrets, eliminating silos and inconsistencies.
2. **Enhanced security controls** including fine-grained access policies, temporary access, and RBAC ensure that only authorized users and systems can access sensitive information.
3. **Improved developer experience** through intuitive interfaces and CLI tools encourages teams to follow security best practices naturally.
4. **Flexibility in deployment options** allows you to choose between self-hosted and cloud deployments based on your security requirements.
5. **Vendor independence** creates an abstraction layer that simplifies migrations between cloud providers or services.
6. **Comprehensive audit capabilities** track all secret access and modifications, supporting compliance requirements and security investigations.


## Summary


In this guide, we've explored how to implement secure secrets management in GitLab CI/CD using Infisical. From simple synchronization of secrets to GitLab variables to advanced OIDC-based authentication and integration with Kubernetes, Infisical provides a flexible and secure solution for organizations of all sizes.


The key takeaways are:


- Traditional GitLab CI/CD variables have significant limitations for complex environments
- Infisical provides a centralized, secure, and developer-friendly alternative
- OIDC authentication eliminates the need for long-lived credentials
- Integration options exist for various deployment models and workflows
- Following security best practices is essential regardless of the tools used


By implementing these patterns and practices, you can create a more secure, maintainable, and developer-friendly secrets management workflow that grows with your organization.
