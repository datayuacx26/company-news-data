---
schema_version: "1.0.0"
document_id: "36f980863e77bb55766fe180dc42b2d9c63e6bbc3cba326fd6011f3e7ac1c8f4"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/gitops-secrets-management"
published_at: "2025-01-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:759a9c03f681694e6614832d5d696edff49a02156b8bc4f682827c5b7db651bb"
---

# Secure GitOps Workflows: A Practical Guide to Secrets Management

GitOps is a set of practices that uses Git as the single source of truth for declarative infrastructure and applications, automating the deployment process through version control. In this model, any changes to the system are made through Git commits and automated pipelines, ensuring consistency and traceability.


Managing secrets in GitOps workflows presents unique challenges that require careful consideration of security, automation, and developer experience. This guide explores different approaches to secrets management in GitOps, with a focus on integrating **Infisical** into your workflow.


## Why Modern Secrets Management Tools Matter


Traditional GitOps approaches to secrets management commonly utilize tools like[Kubeseal](https://github.com/bitnami-labs/sealed-secrets?tab=readme-ov-file#kubeseal) and[SOPS](https://github.com/getsops/sops) to securely handle sensitive data in Git repositories. These tools employ public-key cryptography to encrypt confidential information before it's committed to version control. The encrypted secrets can be safely stored alongside application code, maintaining GitOps principles while protecting sensitive data.


When deployed, a dedicated secrets controller running in the target Kubernetes clusters (such as the[SealedSecrets controller](https://github.com/bitnami-labs/sealed-secrets) ) decrypts these encrypted files and creates the corresponding Kubernetes Secrets, enabling secure and automated secrets management throughout the deployment pipeline.


While this preserves version control benefits, it introduces significant operational challenges:


- **Secret rotation** requires careful orchestration to prevent service disruptions
- When secrets need **dynamic updates** , the version control workflow creates friction with real-time operational needs
- Operators running on the cluster side add **management overhead** and increase system complexity.
- **Managing encryption keys** becomes increasingly complex when handling multiple clusters and teams


Modern cloud-native architectures often require a more dynamic secrets management approach. That's why past a certain size, many teams are moving towards reference-based approaches like[External Secrets Operator](https://github.com/external-secrets/external-secrets) , which store only references to secrets in Git and retrieve the actual secrets from external secret management systems. This reference-based architecture delivers significant advantages:


- You can **update secrets dynamically** without modifying deployment pipelines or triggering new builds
- **Teams can rotate and manage secrets** more efficiently through a single interface
- A **centralized secrets manager** gives you comprehensive audit logs and granular access controls across your entire infrastructure
- By removing secrets management from your application workflows, you reduce the operational complexity of **maintaining separate encryption and rotation mechanisms** .


Let's explore how to implement these principles using Infisical, a modern secrets management tool designed with modern GitOps workflows in mind.


## Building a Secure GitOps Pipeline with Infisical


Let's build a secure GitOps pipeline using popular tools to demonstrate how Infisical can integrate into your workflow. We'll use a common stack that many teams already work with:


- [Terraform](https://github.com/hashicorp/terraform) manages infrastructure provisioning
- [GitHub Actions](https://github.com/features/actions) handles CI/CD processes
- [Argo CD](https://argo-cd.readthedocs.io/en/stable/) ensures declarative deployments
- [Infisical](https://github.com/Infisical/infisical) serves as the central secrets management platform


### 1. Infrastructure Provisioning with Terraform


#### Setting Up OIDC Authentication


The example workflow below uses[OpenID Connect (OIDC)](https://infisical.com/docs/documentation/platform/identities/oidc-auth/github) to authenticate with Infisical, eliminating the need for static tokens. To use this, configure a[Machine Identity](https://infisical.com/docs/documentation/platform/identities/machine-identities) with "OIDC Auth" method for your project.


```text
name: Terraform Apply
on:
push:
branches: [main]
paths: ['terraform/**']


jobs:
deploy:
runs-on: ubuntu-latest
permissions:
id-token: write
contents: read
steps:
- uses: actions/checkout@v3


- name: Request OIDC Token
run: |
echo "Requesting OIDC token..."
TOKEN=$(curl -s -H "Authorization: Bearer $ACTIONS_ID_TOKEN_REQUEST_TOKEN" "$ACTIONS_ID_TOKEN_REQUEST_URL" | jq -r '.value')
echo "INFISICAL_AUTH_JWT=$TOKEN" >> $GITHUB_ENV


- name: Apply Terraform
run: terraform apply -auto-approve


```


OIDC generates short-lived credentials during runtime, which removes long-lived secrets from the CI/CD pipeline. In this workflow, we need to explicitly request the OIDC token from GitHub Actions and set it as an environment variable (` INFISICAL_AUTH_JWT` ) that the Infisical Terraform provider will use for authentication. This additional step ensures secure authentication between your GitHub workflow and Infisical without storing any static credentials.


The requested OIDC token exists only temporarily during the workflow run and is never persisted in GitHub Actions environment storage between runs, significantly improving your security posture.


#### Consuming Secrets in Terraform


In your Terraform configuration, you can then use these injected credentials to securely access secrets through the[Infisical provider](https://registry.terraform.io/providers/Infisical/infisical/latest/docs/ephemeral-resources/secret) :


```text
terraform {
required_providers {
infisical = {
source = "infisical/infisical"
}
}
}


provider "infisical" {
auth {
oidc {
identity_id = "your-machine-identity-id"
# The provider will use the INFISICAL_AUTH_JWT environment variable
# that we set in the GitHub Action workflow
}
}
}


ephemeral "infisical_secret" "db_creds" {
name         = "DB_PASSWORD"
env_slug     = "prod"
workspace_id = var.infisical_workspace_id
folder_path  = "/"
}


# Use the secret in other resources
resource "aws_db_instance" "example" {
password = ephemeral.infisical_secret.db_creds.value
# ...
}


```


The` ephemeral` resource type ensures secrets are only accessed during Terraform operations and aren't stored in state files, reducing the attack surface throughout your infrastructure lifecycle. Read our in-depth guide on ephemeral resources: *[Everything You Need to Know About Terraform's Ephemeral Resources](https://infisical.com/blog/terraform-ephemeral-resources)*


This infrastructure setup gives us a rock-solid foundation for secrets management. Let's see how to adapt these principles for application pipelines.


### 2. Applications Secrets Provisioning


Next, we use the[Infisical Secrets Action](https://github.com/Infisical/secrets-action) to securely inject secrets during the application build pipeline:


```text
name: Build Container
on:
push:
branches: [main]
paths: ['app/**']


jobs:
build:
runs-on: ubuntu-latest
permissions:
id-token: write
contents: read
steps:
- uses: actions/checkout@v3


- uses: Infisical/secrets-action@v1.0.7
with:
method: oidc
identity-id: "your-machine-identity-id"
project-slug: "your-project-slug"
env-slug: "prod"


- name: Build application
run: |
docker build -t myapp:latest .


```


While our examples use GitHub Actions, the integration patterns remain similar across other CI/CD platforms. Whether you're using[GitLab CI](https://docs.gitlab.com/ee/ci/) or any other CI/CD platform like[CircleCI](https://circleci.com/) ,[Jenkins](https://www.jenkins.io/) , or[Azure DevOps](https://azure.microsoft.com/en-us/products/devops) , the principle remains the same: install the[Infisical CLI](https://infisical.com/docs/cli/overview) and use it to inject secrets during your pipeline execution.


### 3. Kubernetes Deployment with ArgoCD


When implementing secrets management in a GitOps workflow with Argo CD, there are three main approaches to integrate Infisical, each with different trade-offs for declarative configuration and security. These options allow teams to replace plain Kubernetes secrets with more secure and flexible methods while maintaining GitOps principles.


#### Option 1: Infisical Kubernetes Operator (most GitOps-native)


The[Infisical Operator](https://infisical.com/docs/integrations/platforms/kubernetes/overview) is a native solution that offers three Custom Resource Definitions (CRDs) with different capabilities:


1. ` InfisicalSecret` - Syncs secrets from Infisical into Kubernetes
2. ` InfisicalPushSecret` - Pushes new secrets from Kubernetes to Infisical
3. ` InfisicalDynamicSecret` - Manages dynamic secrets with automatic time-bound leases


Here is a sample manifest for the InfisicalSecret CRD:


```text
apiVersion: secrets.infisical.com/v1alpha1
kind: InfisicalSecret
metadata:
name: infisicalsecret-sample
labels:
label-to-be-passed-to-managed-secret: sample-value
annotations:
example.com/annotation-to-be-passed-to-managed-secret: "sample-value"
spec:
hostAPI: https://app.infisical.com/api
resyncInterval: 10
authentication:
kubernetesAuth:
identityId: <machine-identity-id>
serviceAccountRef:
name: <service-account-name>
namespace: <service-account-namespace>


```


This would result in a managed Kubernetes secret:


```text
apiVersion: v1
kind: Secret
metadata:
annotations:
example.com/annotation-to-be-passed-to-managed-secret: sample-value
secrets.infisical.com/version: W/"3f1-ZyOSsrCLGSkAhhCkY2USPu2ivRw"
labels:
label-to-be-passed-to-managed-secret: sample-value
name: managed-token
namespace: default
type: Opaque
data: ...


```


The operator continuously monitors for changes and automatically updates Kubernetes secrets to keep them in sync. It can also automatically reload dependent Deployment resources whenever relevant secrets are updated. Installation is handled via[Helm](https://infisical.com/docs/self-hosting/deployment-options/kubernetes-helm) , making it easy to integrate into existing Kubernetes workflows.


**Advantages:**


- Fully declarative
- Bi-directional sync
- Support for dynamic secrets
- All secret configurations are version-controlled alongside other manifests
- Automatic deployment reloading
- Natural integration with ArgoCD's auto-sync and health checking capabilities


**Best suited for:**


- Environments requiring bi-directional secret sync
- Use cases involving dynamic secrets
- Teams preferring native integrations over third-party operators


#### Option 2: External Secrets Operator


In the External Secrets Operator architecture, an[ExternalSecrets](https://external-secrets.io/v0.4.4/api-externalsecret/) declares what data to fetch. The controller then fetches the secrets from a secret store and injects the values into a[Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/) .


The example manifest below defines the Infisical SecretStore as the backend housing the service token, and an ExternalSecret referencing a sensitive` DATABASE_URL` :


```text
apiVersion: external-secrets.io/v1beta1
kind: SecretStore
metadata:
name: infisical-store
spec:
provider:
infisical:
serviceToken:
secretRef:
name: infisical-credentials
key: serviceToken
---
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
name: application-secrets
spec:
secretStoreRef:
name: infisical-store
kind: SecretStore
target:
name: app-secrets
data:
- secretKey: DATABASE_URL
remoteRef:
key: database_url


```


ExternalSecrets itself does not perform any cryptographic operations, such as encrypting or decrypting secrets, and fully relies on the security of the Secret Store backends.


**Advantages:**


- Native Kubernetes resource management
- Automatic secret synchronization
- Support for multiple secret backends
- Built-in secret rotation capabilities
- Works well with GitOps workflows


**Best suited for:**


- Teams already using Kubernetes operators
- Environments requiring automatic secret rotation
- Multi-cluster deployments


#### Option 3: Secrets Store CSI Driver


The[Secrets Store CSI driver](https://secrets-store-csi-driver.sigs.k8s.io/) (as the name suggests) is not implemented as a controller to reconcile the data into Secret resources, but instead uses a separate volume that is mounted to a Kubernetes pod to contain the secrets:


```text
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
name: infisical-secrets
spec:
provider: infisical
parameters:
serviceToken: "${SERVICE_TOKEN}"
projectId: "${PROJECT_ID}"
secrets: |
- secretPath: "database_url"
version: "latest"


```


[Read how to install the Infisical CSI provider](https://infisical.com/docs/integrations/platforms/kubernetes-csi) .


What is great about this approach is that it completely bypasses Kubernetes Secrets. On the other hand, of the three presented options, it is more focused on runtime secret mounting than declarative configuration, making the overall GitOps state harder to track.


**Advantages:**


- Direct pod-level secret mounting
- No intermediate Kubernetes secrets
- Automatic secret updates without pod restarts
- Lower operational overhead


**Best suited for:**


- Applications requiring direct secret mounting
- High-security environments
- Scenarios where secret immutability is important
- Performance-critical applications


---


Read also:[Kubernetes Secrets Management in 2025 - A Complete Guide](https://infisical.com/blog/kubernetes-secrets-management-2025)


## Why Use a Dedicated Secrets Management Service?


Native secret management solutions like GitHub Secrets, AWS Parameter Store, and AWS Secrets Manager work well within their ecosystems. However, using a dedicated secrets management service as your single source of truth strengthens your architecture and saves time across common scenarios like updating secrets, integrating new components, onboarding team members, or migrating between vendors:


1. Instead of scattering secrets across different tools and platforms, you have **one place to manage everything** . This means, you can easily control **who has access to what secrets** using policies and access controls.[Just-in-time access](https://infisical.com/docs/documentation/platform/access-controls/temporary-access) and[RBAC](https://infisical.com/docs/documentation/platform/access-controls/role-based-access-controls) with the possibility of granting[additional privileges](https://infisical.com/docs/documentation/platform/access-controls/additional-privileges) support the principle of least privilege while maintaining flexibility for urgent needs.
2. **When security tools are difficult to use, teams often work around them** , creating vulnerabilities. A unified solution with[an intuitive control panel](https://infisical.com/docs/documentation/guides/local-development) , comprehensive API, and full-featured CLI ensures consistent policy enforcement across your organization. This combination of security and usability helps teams follow best practices naturally.
3. Flexibility: Choose between[self-hosted](https://infisical.com/docs/self-hosting/overview) and cloud deployments based on your security requirements and infrastructure strategy. This flexibility helps you maintain control over your secret management infrastructure while adapting to changing needs.
4. A dedicated secrets management service acts as a **universal adapter for your entire technology stack** . This abstraction layer simplifies migrations between vendors or services, reducing lock-in risk and technical debt.
5. Track all secret access and modifications with detailed[audit logs](https://infisical.com/docs/documentation/platform/audit-logs) . This visibility is often required to maintain compliance and a real advantage if you need to investigate a security incident.
6. Features like continuous[secret scanning](https://infisical.com/docs/cli/scanning-overview) or secure[secret sharing](https://infisical.com/docs/documentation/platform/secret-sharing) help prevent accidental exposures, such as secrets committed to version control. This proactive approach reduces security risks before they become incidents.


## Security Best Practices for GitOps Secrets Management


### 1. Implement Least Privilege Access


**Why it matters:** Security breaches often expand through excessive permissions. Least privilege access minimizes your attack surface and contains potential security incidents. **Key considerations:**


- Create granular service accounts for different components
- Use temporary elevated access for maintenance
- Regularly audit and revoke unused permissions
- Implement clear access request procedures


### 2. Regularly Rotate Secrets


**Why it matters:** Static secrets become vulnerable over time through exposure and credential sharing. Regular rotation reduces the impact of potential breaches.


**Key considerations:**


- Use native platform authentication (like OIDC) to avoid managing "secret zero" credentials
- Implement automated rotation schedules
- Use short-lived credentials where possible
- Maintain secret versioning
- Plan for emergency rotation procedures


### 3. Enable Audit Trails


**Why it matters:** Without proper audit trails, detecting and responding to security incidents becomes nearly impossible. Audit logs help maintain compliance and enable incident investigation.


**Key considerations:**


- Enable detailed logging in Infisical:


- Track all secret access
- Monitor configuration changes
- Log administrative actions


- Implement logging in your Kubernetes clusters
- Set up log aggregation and analysis:


- Use tools like ELK stack or CloudWatch
- Set up alerts for suspicious activities
- Maintain log retention policies


### 4. Secure CI/CD Pipeline Configuration


**Why it matters:** CI/CD pipelines have access to both source code and production environments, making them very attractive targets for attackers.


**Key considerations:**


- Secure GitHub Actions workflows
- Implement branch protection
- Use OIDC for cloud provider authentication
- Implement required reviews for sensitive changes
- Scan dependencies and containers
- Maintain separate environments for staging and production


These practices are a good starting point for securing your GitOps workflows. The key is finding the right balance between security controls and developer productivity for your specific environment.


## How We Handle Our Own Secrets at Infisical


At Infisical, we take "eat your own dog food" seriously! **We'll soon publish a detailed blog post explaining exactly how we manage our own secrets in production.**


Spoiler: it's a pretty neat setup! We run everything through our own web app (of course!), and we've automated the sync with AWS Parameter Store and GitHub Secrets, so everything stays in harmony: for deployments, we've got GitHub Actions and Argo CD doing the heavy lifting, while our own Infisical Secrets Operator keeps our secrets in check in the Kubernetes clusters.


Oh, and obviously, we track every single access and change. When you're building a security tool, you kind of have to practice what you preach!


## Looking Forward: The Future of GitOps Secrets Management


Whether you're just starting with GitOps or managing a complex multi-cluster environment, the principles and patterns we've discussed here will help you build a more secure and maintainable system.


We're seeing some really cool trends in GitOps automation, developer experience, and security controls, and that's exactly why we built Infisical the way we did. We wanted to create something that just works - a central place for all your secrets, with tools developers actually want to use, and the flexibility to run it however you want, wherever you want.


Whether your team is five people or five hundred, whether you're all-in on cloud or keeping things on-prem, by implementing Infisical in your GitOps workflow, you create a foundation for secure, scalable, and developer-friendly secrets management that grows with your organization.
