---
schema_version: "1.0.0"
document_id: "d460bcd6b38ee1154c441aef6c8fd45df6aab7bfe5c6182e9e051ab4d5105113"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/certificate-based-authentication-payment-banking/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T23:45:22.302892+00:00"
fetched_at: "2026-07-31T23:45:23.345437+00:00"
content_hash: "sha256:6358a992c0d47e676f90372feec22976c341fd4d4c5ec40e0fbb8c990f4762c8"
---

# Guide: Certificate-Based Authentication for Payment & Banking Infrastructure

**Read this guide to learn:**


- The challenges and risks of static credentials in payment and banking infrastructure
- How to eliminate those challenges with certificate-based authentication
- How certificate authentication, machine and workload identity, and hardware attestation work in on-prem, cloud and Kubernetes infrastructure


[Payment and banking infrastructure](https://goteleport.com/use-cases/digital-payments-and-banking/) continues to grow. Bare-metal servers and mainframes now sit alongside Kubernetes clusters, microservice architectures, and CI/CD pipelines running across multiple clouds and on-prem data centers. Every new environment adds its own accounts, tokens, and access paths to manage.


But because this infrastructure powers live transactions, there is no room for downtime or disruptions. As a result, engineers may still use[static SSH keys](https://goteleport.com/learn/ssh-keys-explained/) , shared database passwords, or service accounts running on long-lived tokens. This means that audit logs, already scattered across dozens of systems, have no identity attribution.


The following guide is for the platform and infrastructure engineers responsible for addressing the key challenges that static credentials create in payment, banking, or financial services environments.


## The risks and challenges of static credentials


The Kubernetes clusters orchestrating transaction workloads, databases storing cardholder and account data, and CI/CD pipelines deploying code to production dozens of times per day all operate between identity providers (IdPs), cloud IAMs, and secrets vaults. Access to these resources depends on static and often unmanaged credentials, such as SSH keys, embedded service account tokens, or stored/shared database passwords.


In payment infrastructure, an outage during peak processing hours carries a cost the business feels immediately. Pulling a credential without knowing what depends on it isn't a risk teams can afford to take, but revoking a credential also requires verifying every dependency in a pipeline. Static credentials persist not because teams are unaware of the risk, but because the operational consequences of removing them from a system are often measurable in ways the security risk is not.


### Credential rotation is disruptive (and itself a risk)


SSH keys sit in` authorized_keys` files on hosts, in secrets injected as environment variables on build servers, and in some environments directly into container images stored in registries. This means that even rotating a single key may require identifying each host and downstream automation that trusts it, then verifying nothing breaks once it's replaced. But while config management can help push a new key to thousands of hosts in minutes, it can’t tell you in advance which pipeline, cron job, or integration is dependent on the previous key.


Service accounts are also highly dependent on static credentials, which adds their own challenges. For example, an account created for a database integration years ago may still authenticate to a production database. These static credentials may exist in Kubernetes secrets, environment variables, or CI/CD configuration files, and potentially at the same time. But it’s not just the operational toll that encourages persistent credentials. If a key is missed during rotation, the revenue impact of an outage or broken pipeline can be devastating.


### Standing privileges accumulate over time


Most Kubernetes environments in digital payment or banking infrastructure default to a small set of already-built roles. Typically, an admin role and a standard user role.


Authoring a narrowly scoped role for every task is ongoing engineering work most teams don't prioritize. However, compliance mandates least privileged access restricted to the minimum necessary for the job function. For example, if an engineer receives admin access during a break-glass incident, but no process revokes the access after the incident closes, the engineer has acquired standing admin privileges.


Group memberships can also create standing privileges. For example, an engineer joins a database access group during a migration project, but their membership continues after the project ends. Each stale group membership creates a new opportunity for an attacker to exploit standing privileges if an identity is compromised.


### Audit logs are siloed and lack identity context


Each tool in an infrastructure stack generates its own audit log. For example, an IdP will write a log entry when a person authenticates, or a bastion host will create a log entry when an SSH session opens. Even Kubernetes clusters log when a kubectl command executes.


However, each of these log entries is collected in a separate system with its own audit format, and often without including critical identity context. If a compromised user or workload navigates between multiple systems, it may be virtually impossible to provide a complete audit trail without requiring teams to manually pull and correlate audit logs across dozens of individual systems.


### PCI DSS compliance challenges


The growing volume of static credentials also introduces challenges for compliance with Payment Card Industry Data Security Standard (PCI DSS). PCI DSS 4.0 requirements mandate strict controls over accessing cardholder data environments and what audit and forensic requirements organizations must prove. For example:


- **Requirement 8.6.2:** Prohibits hardcoding credentials for application and system accounts capable of interactive login. It specifies that passwords and passphrases must not be embedded within scripts, configuration or property files, or custom source code.
- **Requirement 8.6.3:** Requires credentials for application and system accounts to be changed periodically or upon compromise to prevent indefinite use or misuse.
- **Requirement 10.2 :** Requires that audit logs are enabled and active for all system components and cardholder data to capture all actions taken by an individual, including use of application or system accounts.


Meeting these requirements using multiple IdPs, cloud IAMs, and secrets vaults requires bridging gaps between tools that were not natively designed to share an identity or access model, let alone an audit trail. These are the environments where certificate-based authentication and unified identity layer can make an enormous difference.


> Learn how Teleport helps organizations[meet critical PCI DSS requirements](https://goteleport.com/use-cases/pci-dss-compliance/) .


## How certificate-based authentication solves these challenges


Certificate-based authentication eliminates the need for credential rotation by ensuring credentials (and the privileges they extend) expire before rotation is necessary by using short-lived certificates, which are digitally signed documents that contain an identity, a set of permissions, and an expiration time.


### Trusted, short-lived access to servers, databases, and clusters


Instead of trusting individual and long-lived credentials, resources like servers, databases, and Kubernetes clusters can be configured to trust certificates issued by a central[certificate authority (CA)](https://goteleport.com/blog/ssh-certificate-authorities-explained/) . While an SSH key or service account may work until it has been completely revoked, a certificate issued by a certificate authority will only provide access for as long as the CA has encoded it at the moment of issuance.


The server validates multiple attributes when an engineer's SSH client presents the certificate:


1. *Did the CA sign the certificate?*
2. *Has the certificate expired?*
3. *Do the roles encoded in the certificate grant access to the server?*
4. *Can the client prove possession of the private key that corresponds to the certificate?*


If each of these checks pass, the server allows the connection. However, if any check fails, the connection is rejected.


This validation process must also apply to databases and Kubernetes clusters, enabling certificate authentication instead of password-based logins to databases or static service account tokens to clusters. This ensures that every resource (server, cluster, or database) validates the same type of credential from the central CA and that each credential expires automatically.


### Identity and automatic renewal for machines, pipelines, and service accounts


Human users are only half of the static credential problem (and depending on your environment, potentially much less than half). However, in payment and banking infrastructure, machines and automated processes may hold the lion’s share of static credentials, including service accounts created years ago and static credentials living in Kubernetes secrets, environment variables, or CI/CD configuration files.


Solving the static machine credential problem requires running a lightweight agent alongside each pipeline or service process. Like a human user, machines and workloads should also request a certificate from the central CA, and receive a short-lived certificate encoding the workload's identity and permissions that can be automatically renewed, preventing pipeline disruptions.


Consider a CI/CD pipeline. Once the pipeline completes and stops requesting certificate renewal, the issued certificate's validity window should close within minutes. This way, if an attacker compromises the build server after the pipeline finishes, all they will find is an expired certificate instead of a usable static credential.


### Hardware checks before authentication


While certificate-based authentication prevents an attacker from reusing a stolen SSH key or password, binding certificate issuance to a hardware identity on a user’s device closes any remaining credential-theft-and-replay path to compromise.


Modern Linux and Windows devices contain a Trust Platform Module (TPM) (with Secure Enclaves as the macOS equivalent). A TPM stores cryptographic keys in hardware that cannot be extracted or copied to another device. When an organization enrolls a device and the CA records that device’s TPM identity as enrolled, the CA can check during authentication whether the device presenting the request contains a TPM matching an enrolled device. And if the TPM does not match, the CA can refuse to issue a certificate even if the engineer's presented IdP credentials are valid.


Adding this hardware check to authentication defeats two kinds of attack scenarios:


1. If an attacker steals a certificate from a laptop and attempts to use it from a different device, authentication will fail because using the certificate requires signing a challenge with the private key generated inside the enrolled TPM.
2. If a user attempts to authenticate from a personal laptop that is not enrolled in the organization's Mobile Device Management (MDM) tool, authentication will fail at the CA's enrollment check since no TPM identity is on record for that device.


Not all roles will require the same level of hardware verification. For example, roles granting access to staging environments may not need a hardware check on every session. But for firms handling sensitive cardholder or financial data, roles granting access to cardholder data environments must require a hardware check on every session.


## How Teleport implements certificate-based authentication


Replacing SSH keys and passwords with certificates across payment infrastructure requires a certificate authority that can integrate with an organization's identity provider and issue credentials across each protocol and resource type an engineer may use to reach infrastructure. This is the service that Teleport provides.


The[Teleport Auth Service](https://goteleport.com/docs/reference/architecture/authentication/) acts as the CA, issuing certificates and maintaining the audit log, while the Proxy Service is the single point resources are reached through from outside the network, and an Agent deployed alongside each resource checks the presented certificate and enforces permissions before granting access.


When an engineer logs in, Teleport connects to the organization's IdP, pulls the engineer's identity and group memberships, and signs a short-lived certificate encoding those attributes with a time to live the administrator controls per role. That certificate is the engineer's only credential from that point forward, and is presented for validation for every SSH client, kubectl, or database connection. When the certificate expires, the engineer can then reauthenticate through the IdP to receive a fresh certificate.


For databases, Teleport runs in front of each protected instance and holds the relationship with the database, including any TLS certificates the managed provider issued during provisioning. When an engineer connects, the[Database Service](https://goteleport.com/docs/enroll-resources/database-access/) presents a Teleport-issued certificate to the database on the engineer's behalf, which means the engineer never sees the database's own credentials or types a password.


## How Teleport issues certificates to machines, pipelines, and service accounts


Service accounts and CI/CD pipelines use the majority of stored static credentials in most payment environments, and those credentials cannot reauthenticate through an IdP the way a human can. Latency is also critical at the speed and scale of payment infrastructure, and understandably, engineering teams may not want a proxy sitting in the path of production queries. Teleport solves the machine side of the problem without placing a proxy in the transaction path.


[Teleport Machine & Workload Identity](https://goteleport.com/platform/machine-and-workload-identity/) runs a lightweight agent called` tbot` on any machine or pipeline that needs a certificate. This agent authenticates to the Teleport cluster through a join method, which is a mechanism that proves the agent's identity without a pre-shared secret. On AWS, for example, the agent can present its EC2 instance identity document. On GitHub Actions, the agent presents the OIDC token GitHub injects into the workflow.


This join method eliminates the bootstrapping problem where a credential is needed to get a credential. Instead, after joining,` tbot` requests certificates, writes them to a local file or Kubernetes secret, and renews them before they expire. For CI/CD pipelines,` tbot` can run in one-shot mode, which fetches one certificate for the run and exits when the run finishes.


What distinguishes Teleport from a generic certificate authority is the split between two operating modes:


1. **Through the proxy** : Teleport provides full session recording and RBAC enforcement, suited for interactive access and audit-heavy environments.
2. **Direct with Workload Identity** : Teleport provides certificate issuance without the proxy in the data path, suited for high-throughput service-to-database traffic where latency and availability matter more than session-level recording.


When a workload connects through the Teleport proxy, the proxy enforces role-based access controls and records the full session. When a workload connects directly to the target, Teleport acts purely as the CA. It issues a[SPIFFE Verifiable Identity Document](https://goteleport.com/docs/machine-workload-identity/workload-identity/spiffe/) , and the workload presents that certificate straight to the database or service without the proxy touching the traffic. Teleport logs the issuance event so an auditor can correlate the certificate grant with the database's own query logs, but the data path stays between the workload and the database.


Many managed databases from cloud providers ship with their own TLS certificates. Teleport takes the cloud provider's CA certificate during setup and uses it to verify it is talking to the correct database. From there, Teleport handles all client authentication so that the end user only ever receives a Teleport-issued certificate. The cloud provider's certificate only exists in the background to secure the connection between Teleport and the database.


> [Watch a demo of Machine & Workload Identity to see how it works.](https://goteleport.com/resources/demo-videos/machine-workload-id-overview/)


## How Teleport binds sessions to enrolled hardware


A certificate that expires in minutes still carries privileges for every one of those minutes. If an attacker compromises an engineer's active session and replays the certificate from another machine, the attacker inherits whatever roles the certificate encodes.


Teleport can turn certificate issuance into a[two-factor decision using enrolled hardware](https://goteleport.com/docs/zero-trust-access/device-trust/) . During authentication, Teleport checks the TPM on Windows and Linux or the Secure Enclave on macOS, verifying that the chip's identity matches a device the organization has registered. Teleport also integrates with[tools like Jamf](https://goteleport.com/docs/zero-trust-access/device-trust/jamf-integration/) or[Microsoft Intune](https://goteleport.com/docs/zero-trust-access/device-trust/intune-integration/) to pull device inventories and cross-check device profiles against the hardware the device actually presents, which prevents enrollment spoofing via fabricated device metadata. After enrollment, the hardware check runs on every authentication, and the device's identity appears in the issued certificate and subsequent audit event.


In this example certificate, you can see where` teleport-device` appears in the` Extensions` field:


```text
tsh    status
>    Profile URL:           teleport.example.com:443
Logged in as:          myuser
Cluster:               teleport.example.com:443
Roles:                 access,    auditor,    editor
Logins:                root
Kubernetes:            enabled
Valid until:           2026-07-31 04:06:53    -0400    EDT   [ valid    for    12h0m0s  ]
Extensions:            login-ip,    ...    teleport-device-asset-tag,    teleport-device-credential-id,    teleport-device-id


```


Payment and banking infrastructure includes environments that operate at multiple different sensitivity levels. Teleport also allows teams to tune hardware enforcement based on role. For example, an engineer accessing a development namespace may not require a hardware check on every session, but an engineer accessing a cardholder data environment governed by PCI DSS faces one every time. Read the documentation to[learn more about Device Trust.](https://goteleport.com/docs/zero-trust-access/device-trust/)


## What changes after eliminating static credentials


Payment infrastructure teams that adopt identity and certificate-based authentication no longer need to distribute and manage static credentials across systems, users, and workloads. This cascades into numerous operational and security benefits:


- Onboarding a new engineer becomes an IdP group assignment rather than a provisioning ticket to place SSH keys on dozens of servers.
- Static tokens that service accounts long depended on are replaced by workload identities that last only as long as the process each one serves.
- Compliance evidence no longer requires weeks of log correlation because every access event, human or machine, traces back to an identity in a unified audit trail.


And while eliminating static credentials shrinks the attack surface, the reduction in manual work for engineering and platform teams is what makes Teleport the best choice for identity and access at the scale and velocity of modern payment-processing infrastructure.


> “We needed secure access that scales with our infrastructure — without adding complexity for engineers or risk for our customers.” — Bjarne Lamczak, Cloud DevOps Engineer, PPI Financial Services


#### See Teleport in real-world payment infrastructure


[Read this case study](https://goteleport.com/case-study/ppi-financial/) to learn how PPI Financial Services, a managed cloud platform provider for payment and financial infrastructure, uses Teleport to:


- Eliminate manual certificate management
- Unify identity and access for compliance-ready auditability
- And more
