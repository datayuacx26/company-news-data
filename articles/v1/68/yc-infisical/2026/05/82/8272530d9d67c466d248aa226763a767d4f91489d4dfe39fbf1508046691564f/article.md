---
schema_version: "1.0.0"
document_id: "8272530d9d67c466d248aa226763a767d4f91489d4dfe39fbf1508046691564f"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/pam-for-cloud-environments"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:5db02290471e8eaed321bcabca350a786d3466a1c73ee715d9c8faa6531363e3"
---

# PAM for Cloud Environments: AWS, Azure, GCP & Multi-Cloud

Traditional[privileged access management](https://infisical.com/blog/what-is-privileged-access-management) was developed for a world in which you knew your servers (all on-premises), had a list of privileged accounts, and your credentials were as static as your infrastructure.


That world has been supplanted by ephemeral infrastructure used by thousands of identities, which makes previous access review cycles obsolete. Legacy PAM practices are as outdated as the assumptions they were built for: they're less secure and strain engineering organizations that resort to workarounds or manual workflows.


Cloud environments have shifted the bedrock under security practices, so it's time those evolve.


## Why do cloud environments change the PAM equation?


Today, infrastructure is more abstract than the physical server basements we used to access. The workloads are short-lived. Auto-scaling groups, containerized services, and serverless functions are spun up and torn down faster than any access review cycle. There is no ongoing server list to manage because the infrastructure does not stay active long enough to justify one.


Conventional PAM also assumed each credential would be used by a human. Today, the identity surface has expanded beyond human users. In a typical multi-cloud environment, you are managing IAM roles attached to EC2 instances, ECS tasks, Lambda functions, CI/CD pipelines, and Terraform runners alongside a sprawl of service accounts.


Legacy PAM requests required one person to get access to one piece of infrastructure to perform a task, for which a short-lived credential would be issued. Now, hundreds of non-human identities need credentials to access a whole set of VMs, containers, pods, or other bits of infrastructure. Nobody on the planet has the resources to provision hundreds of credentials, monitor all of them, and revoke them on time.


To make this more convenient (but sacrificing security), credentials are often over-permissioned at creation and never revisited. Non-human identities now outnumber human ones in most cloud-native environments, which gets exacerbated by AI agents making direct infrastructure calls at scale. Static, overpermissioned PAM credentials can create security vulnerabilities, as examples like[Okta's 2023 breach](https://sec.okta.com/articles/2023/11/unauthorized-access-oktas-support-case-management-system-root-cause/) show.


This is where legacy PAM falls apart.


The processes were designed for static infrastructure and human users, but today's reality confronts them with transient infrastructure and non-human identities. Vaulting a credential, rotating it on a schedule (measured in months), and recording a session all assume continuous and known endpoints.


PAM for cloud environments understands workloads don't persist, identities are mostly automated, and the scope of potential damage extends across multiple cloud providers like AWS, Azure, and GCP. To keep up with the demands of modern infrastructure, teams need to understand where cloud PAM falls short and what a modern privileged access strategy needs to look like.


## What does a modern cloud PAM strategy look like?


Cloud PAM is not rebranded traditional PAM, but a reimagined way of managing privileged access control. Short-lived cloud infrastructure and proliferating non-human identities shift the foundations.


Fundamentally, cloud-native PAM needs to automatically provision access to a whole host of identities (human and non-human) that need to act on a long list of infrastructure. This introduces massive added complexity.


Misconfigured roles may result in a significant blast radius across accounts and regions. Modern strategy shifts from managing static perimeters to managing dynamic, identity-centric controls. A cloud-native PAM approach must prioritize a few principles and capabilities to ensure both scalability and security.


### JIT access vs. standing privileges


Just-in-time (JIT) access, meaning access should only be granted when needed and revoked immediately after, should be the default. It flips the model from standing privileges to access granted on demand, scoped to what's needed, and revoked automatically.


In practice, an engineer debugging a production RDS instance may get elevated access for 30 minutes, tied to a specific approval, with no residual permissions afterward. During those 30 minutes, a specialized PAM module logs every command and action taken.


### Session recording and auditability


Session recordings should cover the full surface area of the environment. In compliance frameworks like SOC 2 and FedRAMP, knowing who accessed what and what actions they took is an explicit expectation.


This was easy when you used SSH to log into fixed servers, but gets complex with[EKS clusters](https://infisical.com/blog/kubernetes-secrets-management) , rotating bastions, and browser-based console access with no terminal session to record. Your session recording needs to cover SSH, RDP,` kubectl exec` , web console access, and database sessions with query-level logging. Logs need to be tamper-evident and stored somewhere that the admins being audited can't modify.


### Integration with existing workflows


Access requests belong where workflows already exist, so that you don't create parallel security practices that require separate upkeep. Defaulting to a separate ticketing portal for access requests is how PAM programs die. Teams will route around friction if it disrupts the workflow and thus compromise security.


Requests need to live in Slack, Teams, PagerDuty, or other access approval chains where integration expedites approval during active incidents. Approval requirements should also be tiered by risk: accessing a dev account mid-afternoon does not have the same risk profile as accessing production at 2 AM.


Infisical's supports[approval workflows](https://infisical.com/docs/documentation/platform/pam/product-reference/access-policies) that follow approval policies which can be defined according to a variety of conditions.


## Multi-cloud PAM: a compounding problem


Running a multi-cloud platform compounds the complexity. It becomes a matter of managing multiple credential types, session tooling approaches, audit log formats, and mental models for privileges. An access review process that works in AWS doesn't map cleanly to GCP's permission structure. A session recording setup for Azure Bastion doesn't extend to EC2 instances.


The temptation to treat each cloud in isolation and manage each with its own native tools works at a small scale, but becomes a maintenance burden as the environment grows. Audit coverage becomes inconsistent. Policy enforcement drifts between providers. Onboarding and offboarding access across all cloud environments turns into a multi-step manual process that someone has to own.


Centralizing policy management across cloud boundaries is no longer a luxury. It is a necessity for maintaining least-privilege enforcement in an era of ephemeral workloads and rapidly growing non-human identities.


## What are the PAM challenges specific to major cloud providers?


Each major cloud provider offers native Identity and Access Management (IAM) frameworks designed to secure cloud resources. However, these native tools are fundamentally built for resource isolation and perimeter enforcement rather than comprehensive, cross-platform privileged access workflows. Operational friction arises because of the stark architectural differences between AWS, Azure, and GCP.


Because each provider structures its infrastructure differently, they introduce unique privilege escalation risks and frequently fall short of the automated orchestration required for a modern cloud PAM strategy. To build an effective cloud PAM architecture, organizations must understand the baseline mechanics of how these native services function, their intended purpose, and where security gaps inevitably emerge.


### AWS: hidden escalation and the complexity of role sprawl


The AWS security ecosystem relies on a granular permission model governed by AWS IAM to handle resource isolation. To manage privileged access, AWS provides several specialized native utilities:


- AWS IAM Identity Center handles human single sign-on and identity federation
- AWS Systems Manager (SSM) Session Manager enables secure, auditable terminal access to EC2 instances without requiring open SSH ports or bastion hosts
- AWS CloudTrail records an ongoing audit log of programmatic API calls across the environment.


While powerful individually, these tools do not natively compose into a coherent PAM workflow because they lack built-in session recording tied to a centralized cross-account audit store or an out-of-the-box approval workflow for temporary privilege elevation. This environment introduces distinct challenges.


#### Unintuitive privilege escalation via permission combinations


AWS contains over 15,000 individual permissions, making privilege escalation paths not obvious and easy to miss during manual access reviews. A prominent example is the combination of` iam:PassRole` and` ec2:RunInstances` . Individually, their purposes are standard:` ec2:RunInstances` allows a user to spin up a virtual machine, and` iam:PassRole` allows an identity to assign an existing IAM role to an AWS service.


However, when granted together, they form a severe privilege escalation vulnerability. A low-privileged user or an attacker can launch a new EC2 instance and use` iam:PassRole` to assign a highly privileged role (such as` AdministratorAccess` ) to that instance. By subsequently logging into the newly created instance via SSM Session Manager, the user effectively inherits the high-level administrative permissions assigned to that instance, completely bypassing intended permission boundaries.


A policy granting both permissions looks innocuous at a glance:


```text
{
"Effect": "Allow",
"Action": [
"ec2:RunInstances",
"iam:PassRole"
],
"Resource": "*"
}


```


New variations of these escalation vectors emerge regularly as AWS introduces new services.


#### Custom JIT orchestration overhead


Because AWS lacks a native, automated JIT access request engine, engineering teams are forced to build temporary access solutions in-house. This typically requires wiring together custom Lambda functions, EventBridge event buses, and AWS Access Analyzer to dynamically attach and detach IAM policies. While functional, maintaining this custom infrastructure introduces massive operational complexity at scale.


#### Role assumption audit blind spots


AWS relies heavily on role assumption chains, where a service or user assumes an IAM role, which might subsequently assume another cross-account role. When these chains lengthen, the identity of the original human or non-human actor often gets lost or obscured within raw CloudTrail logs, although a` sourceIdentity` attribute brings some clarity. Without significant custom log correlation tooling, security teams face severe visibility gaps during an incident response investigation.


Infisical mitigates AWS role sprawl and escalation risks by[dynamically generating ephemeral, time-bound AWS credentials](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) on demand. Instead of maintaining permanent, high-privilege roles that can be passed or exploited, Infisical orchestrates the JIT lifecycle automatically, rendering temporary credentials useless after a task is completed.


### Azure: dividing Entra ID from resource management


Microsoft Azure's security model is built upon a strict operational divide between identity governance and resource architecture. Microsoft Entra ID (formerly Azure AD) serves as the centralized identity provider, managing user authentication, directory groups, and application registrations. Meanwhile, the Azure Resource Manager (ARM) fabric dictates how infrastructure resources are deployed and managed via Azure Role-Based Access Control (RBAC).


To address privileged access across this divide, Microsoft offers Azure Privileged Identity Management (PIM), which allows administrators to make users "eligible" for time-bound role activations, and Azure Bastion, a fully managed PaaS service that provides secure RDP and SSH access to virtual machines directly through the Azure portal. Implementing a unified Azure PAM strategy across these components, however, reveals notable structural gaps.


#### PIM scalability and governance constraints


While Azure PIM provides native time-bound role activation, it struggles to scale cleanly across complex enterprise environments featuring multi-tiered management groups and hundreds of individual cloud subscriptions. Over time, role eligibility assignments proliferate, policy definitions drift out of sync between environments, and compliance access reviews default back to manual, error-prone spreadsheets.


#### Disconnected control planes and attack surfaces


Because Entra ID and ARM resource controls run on entirely separate permission models, a dangerous disconnect exists between identity security and resource infrastructure. Over-permissioned application registrations, broad Microsoft Graph API permissions, and legacy authentication protocols within the Entra ID control plane create lateral movement paths. Attackers can exploit these identity-level misconfigurations to escalate privileges over physical cloud resources without ever triggering standard Azure RBAC resource alerts.


Infisical unifies the Azure PAM experience by bridging the gap between Entra ID and resource controls. It provides a centralized control plane to manage elevated access requests and credentials seamlessly, eliminating the blind spots caused by disconnected cloud identity models and providing encrypted, centralized auditing across human and machine workflows.


### GCP: project inheritance and service account impersonation


Google Cloud Platform (GCP) organizes its entire resource landscape into a strict, logical hierarchy consisting of Organizations, Folders, Projects, and individual Resources. Permissions granted at a higher level of the tree are automatically inherited downward by default, making resource management highly predictable but complicating security audits.


To govern this environment, Google provides the Organization Policy Service, which allows administrators to enforce programmatic guardrails globally (such as blocking public IP creation), alongside GCP's native Privileged Access Manager (PAM), which provides basic JIT entitlement capabilities. Managing a secure cloud PAM framework within this structure exposes specific vulnerabilities.


#### The service account impersonation vector


GCP workloads rely extensively on service accounts for programmatic access. To facilitate automation, administrators often grant identities the` iam.serviceAccounts.getAccessToken` permission, which enables service account impersonation. Its intended purpose is to let a developer or service generate short-lived OAuth 2.0 access tokens to act as that service account temporarily.


However, this permission is a major privilege escalation vector. Anyone with the ability to impersonate a service account silently inherits every single permission assigned to that service account across the entire project or folder hierarchy. Because these inherited permissions are not directly attached to the user's primary identity, users quietly accumulate undocumented, permanent access privileges over time that bypass standard human-centric access monitoring. An IAM binding that grants this capability looks like this:


```text
bindings:
- role: roles/iam.serviceAccountTokenCreator
members:
- user:developer@example.com


```


That single binding may quietly grant` developer@example.com` the full permission set of a service account that was provisioned with` roles/owner` at the project level.


#### Governance guardrails vs. operational PAM tooling


While GCP's Organization Policy Service is effective for establishing high-level architecture guardrails, it is fundamentally a compliance enforcement tool, not a privileged access management workflow engine. It provides no mechanism for live session visibility, peer-based approval workflows, or detailed access request auditing. Furthermore, GCP's native Privileged Access Manager is restricted in functional scope and provider integration, preventing it from serving as a comprehensive multi-cloud PAM solution.


#### Static credential sprawl and tracking hardship


Programmatic credential management for GCP service accounts frequently defaults to static service account keys. These keys are long-lived cryptographic strings that are easily downloadable and incredibly difficult to rotate or track at scale. When developers hard-code these static keys into CI/CD pipelines, container images, or local environment variables, the ability to audit non-human identity access becomes nearly impossible.


```text
# Static service account key downloaded once, valid indefinitely by default
gcloud iam service-accounts keys create ~/key.json \
--iam-account=my-service-account@my-project.iam.gserviceaccount.com


```


Infisical replaces the dependency on risky service account keys and unmonitored impersonation loops. By acting as the dynamic secrets and access engine for GCP workloads, Infisical orchestrates short-lived tokens and automates non-human identity validation, ensuring that every programmatic action is authorized, audited, and completely decoupled from static, long-lived credentials.


## Where is PAM for cloud environments heading?


Given the operational realities of current cloud infrastructure, it is imperative to transition from static access controls, which struggle to manage short-term workloads and dynamic scaling. Non-human identities, AI agents requiring infrastructure access, and ephemeral environments require granular and short-lived permissions. This simplifies the work of both provisioning and using PAM solutions.


Bolting together native cloud tools is an inefficient way to manage cross-cloud credential sprawl and inconsistent audit formats. A sustainable PAM strategy centralizes policy management to ensure that just-in-time access is the default for every interaction.


By treating all identities with the same rigor and reducing credential lifetimes to the absolute minimum, platform teams can secure their environments without introducing added friction. This transition to modern PAM is essential for maintaining visibility as infrastructure continues to grow in complexity.


## Unify your cloud PAM strategy with Infisical


Native cloud utilities provide a solid foundation for resource isolation, but they leave dangerous visibility gaps and operational silos across multi-cloud environments. Infisical bridges these gaps by providing a unified, developer-friendly platform designed for modern cloud infrastructure.


By centralizing policy management, automating JIT access, and eliminating permanent credentials across AWS, Azure, and GCP, Infisical allows engineering teams to enforce zero-standing privileges without interrupting velocity. For a full comparison of where Infisical stands against legacy PAM tools, see the[best privileged access management solutions for 2026](https://infisical.com/blog/best-privileged-access-management-solutions) .


Ready to secure your human and non-human identities across the cloud?[Talk to us](https://infisical.com/talk-to-us) to discover how to eliminate credential sprawl with seamless, just-in-time protection.
