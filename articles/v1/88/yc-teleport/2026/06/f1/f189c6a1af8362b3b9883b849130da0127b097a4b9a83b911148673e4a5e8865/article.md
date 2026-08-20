---
schema_version: "1.0.0"
document_id: "f189c6a1af8362b3b9883b849130da0127b097a4b9a83b911148673e4a5e8865"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/soc2-non-human-identities/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:af9fa2656e3833d748ac722e10d6afcf91e18474c3ca8a02cb36e480b9d3f6aa"
---

# SOC 2 Controls for Non-Human Identities: CC6, CC7, and CC8

#### Read this article to learn:


- How Teleport's workload attestation, short-lived certificates, and audit logs map directly to CC6, CC7, and CC8
- How capabilities like workload attestation and declarative configs produce the audit trail evidence collectors need for each control
- What Teleport captures by default that supports evidence collection across CC6, CC7, and CC8


SOC 2 audits require organizations to demonstrate logical access controls, but most control mappings address only human users, such as engineers SSHing into servers, developers accessing Kubernetes clusters, or administrators approving access requests. But modern infrastructure now runs on non-human identities like CI/CD pipelines, AI agents, microservices, and automated bots. These entities request credentials, access databases, and move data, but they're frequently excluded from the same auditing rigor applied to human access.


[Teleport Machine & Workload Identity](https://goteleport.com/platform/machine-and-workload-identity/) helps close this gap by extending essential identity, access, and audit controls to non-human identities. This article walks through specific control mappings across CC6, CC7, and CC8, explaining how Teleport capabilities satisfy each requirement as well as what evidence can be provided to auditors.


## CC6.1: Restricting logical access for non-human identities


**Requirement:** SOC 2 control CC6.1 requires restricting logical access to authorized entities, including non-human identities such as machine or agentic identities.


**Teleport implementation:** Machine authorization requires verification of what the workload actually is, not just possession of a static key. Teleport enforces this through a process called workload attestation: a lightweight agent (` tbot` ) installed alongside workloads checks properties of the requesting process, such as which Kubernetes namespace it runs in, which service account it uses, or which Linux user owns the process before issuing any credential. Administrators write access rules that define which combinations of these properties are allowed to receive credentials. This replaces static secrets with policy-driven, short-lived access.


**Evidence provided:** Auditors can use` WorkloadIdentity` access rule definitions, audit logs of denied credential issuances, and, where Sigstore attestation is enabled, Sigstore policy configurations as evidence.


**Benefit:** Verifying workload attributes before issuing short-lived credentials reduces reliance on static secrets and limits the blast radius of credential leakage. It also creates an auditable trail showing which workload attributes and` WorkloadIdentity` rules allowed or denied credential issuance for Teleport-mediated access.


Your browser does not support the video tag.


## CC6.2: Registering and authorizing machine identities before credential issuance


**Requirement:** SOC 2 control CC6.2 requires formal registration and authorization before issuing credentials.


**Teleport implementation:** Machine identity access through Teleport requires prior[configuration of a bot identity](https://goteleport.com/docs/ver/19.x/reference/machine-workload-identity/bound-keypair/getting-started/) and its verification method. For example, by presenting a[platform-issued join token](https://goteleport.com/docs/ver/19.x/reference/architecture/machine-id-architecture/) from AWS or Kubernetes, or by completing a cryptographic challenge using a bound keypair. This registration step binds a specific bot name to a specific proof method, preventing unauthorized machines from joining. Administrators can revoke access immediately by locking a bot identity or revoking its issued credentials, both of which take effect across the cluster without waiting for certificates to expire.


**Evidence provided:** Bot resource definitions, join token configurations, revocation records with timestamps, and bot instance records.


**Benefit:** Formal registration helps ensure only approved bots can obtain Teleport-issued credentials and access resources through Teleport. It also provides auditors verifiable evidence of authorization and recorded revocation/lock actions (with timestamps) to support deprovisioning.


Your browser does not support the video tag.


## CC6.3: Reviewing access roles and rules for bots and workloads


**Requirement:** Least privilege applies to machines as much as humans. SOC 2 control CC6.3 requires periodic review of access roles and rules.


**Teleport implementation:** Teleport manages all workload access rules and bot role assignments as[versioned, audit-logged resources](https://goteleport.com/docs/reference/machine-workload-identity/workload-identity/workload-identity-resource/) . When an administrator changes which permissions a bot holds or which workloads qualify for a credential,[the change is recorded](https://goteleport.com/docs/reference/machine-workload-identity/workload-identity/revocations/) with a timestamp and the identity of the person who made it. Administrators can list and export the current set of rules and role assignments to review whether each bot still requires its existing access, supporting periodic least-privilege reviews. The[access graph feature](https://goteleport.com/docs/identity-security/usage/graph-explorer/) extends this by visualizing which resources each bot can access, tracing access paths through roles, and surfacing bots with excessive standing privileges across connected infrastructure.


**Evidence provided:** Exported resource definitions, audit logs of rule changes, revocation logs, bot access paths, standing privilege exposure.


**Benefit:** Regular reviews help identify and remediate permission creep in automated systems that outlive their original purpose. Maintaining least privilege reduces attack surface and makes it easier to produce consistent evidence of current access rules and access-rule changes over time.


## CC6.6: Protecting authentication credentials in transit


**Requirement:** SOC 2 control CC6.6 requires protecting authentication credentials during transmission. Static secrets pose inherent risk.


**Teleport implementation:** Teleport issues[short-lived certificates](https://goteleport.com/docs/reference/machine-workload-identity/workload-identity/workload-identity-resource/) with a default 1-hour time to live (TTL) for bot and workload credentials, and can automatically renew them before expiration. Because credentials expire quickly, a compromised certificate has a narrow window of usefulness. All communication between Teleport components uses TLS encryption, and connections to the central Auth Service can use[certificate authority (CA) pinning](https://goteleport.com/docs/admin-guides/management/operations/ca-rotation/) to prevent man-in-the-middle attacks.


For initial machine onboarding, Teleport supports a[bound keypair method](https://goteleport.com/docs/reference/machine-workload-identity/bound-keypair/concepts/) where the machine generates its own cryptographic keypair and Teleport trusts the public half for future join attempts. This reduces the need to distribute shared secrets between systems. For short-lived workloads like CI/CD jobs, Teleport can run in[one-shot mode](https://goteleport.com/docs/ver/19.x/reference/machine-workload-identity/bound-keypair/getting-started/) , where a credential is issued, the job uses it, and the process exits. Credentials can be held in memory only, so they never are written to disk.


**Evidence provided:**` tbot` configuration, storage permissions, and TLS settings.


**Benefit:** Short-lived certificates and encrypted transmission significantly reduce the long-term exposure that static secrets can create. If a credential is compromised, its usefulness is time-bounded (for example, by the certificate TTL), reducing potential impact compared to long-lived secrets.


Your browser does not support the video tag.


## CC7.2: Detecting unauthorized software execution at the source


**Requirement:** SOC 2 control CC7.2 requires detecting unauthorized activity.


**Teleport implementation:** Teleport denies credentials to any workload that does not pass its configured attestation and policy checks. These checks happen at credential request time, before the workload gains access to protected resources.[Sigstore attestation](https://goteleport.com/docs/reference/machine-workload-identity/workload-identity/sigstore-attestation/) checks whether a container image was signed by a trusted party and carries valid build provenance, such as an SLSA attestation from a CI pipeline. The attestation agent can compute a SHA-256 hash of the requesting process's binary, which administrators can reference in access rules to restrict credential issuance to specific, expected executables. Workloads that fail any of these checks are denied credentials and the denial is recorded as a structured audit event.


**Evidence provided:** Signature verification policy definitions and audit log entries showing denied credential issuance for non-compliant workloads.


**Benefit:** Enforcing signature/provenance checks at credential request time can stop untrusted software from obtaining Teleport-issued credentials, reducing the scope and potential impact of supply chain attacks. This shifts enforcement earlier in the access path and can reduce incident response efforts by denying access before sensitive resources are reached.


## CC7.3: Evaluating security events for machine identity issuance


**Requirement:** SOC 2 control CC7.3 requires evaluating security events and classifying severity.


**Teleport implementation:** Teleport provides audit events for authentication and credential issuance-related activities, including whether the action succeeded or was denied. When a policy violation occurs — for example, a workload requesting credentials from an unauthorized namespace — a log entry is generated that records what was requested, what rule evaluated it, and the outcome. These audit events can be[exported to SIEM platforms](https://goteleport.com/docs/zero-trust-access/export-audit-events/) like Splunk, Datadog, or Elastic for real-time alerting on repeated denials or anomalous patterns.


[Join method anomalies](https://goteleport.com/docs/ver/19.x/reference/machine-workload-identity/bound-keypair/concepts/) that may indicate credential theft or other forms of compromise can automatically trigger lockouts. This lockout applies when join state verification is enabled, which is the default configuration.


**Evidence provided:** Filtered audit logs and configured SIEM alert rules.


**Benefit:** Structured audit events support faster triage and severity classification of credential issuance failures and policy violations, particularly when forwarded to a SIEM for alerting and correlation. Bound keypair join-state lockouts can also shorten the time a stolen bot keypair remains useful by blocking further access after suspicious rejoin behavior is detected.


## CC8.1: Managing changes to machine identity configuration


**Requirement:** SOC 2 control CC8.1 mandates a controlled change management process. Machine identity configurations require the same rigor as production infrastructure.


**Teleport implementation:** Teleport supports this through[declarative resource definitions](https://goteleport.com/docs/machine-workload-identity/access-guides/tctl/) . Administrators define bot registrations, join methods, access rules, and signature verification policies in YAML or JSON files. These files can be stored in version control (e.g., Git), reviewed through pull requests, and applied through automated pipelines to establish a traceable record of proposed, approved, and applied changes.


Teleport also provides a[Terraform provider](https://goteleport.com/docs/reference/machine-workload-identity/terraform-provider-mwi/) for managing configuration programmatically. This includes managing cluster resources like access rules and bot definitions, and generating short-lived credentials that downstream Terraform providers can use to authenticate to external systems. All changes to these resources are recorded in Teleport's audit log alongside the principal identity.


**Evidence provided:** Git history, Terraform logs, and audit entries for resource modifications.


**Benefit:** Treating machine identity configurations as code helps route changes through version control and peer review, creating a clear audit trail of who modified access rules and when. Combined with Teleport audit logs, this approach reduces configuration drift risk and streamlines evidence collection for change management.


Your browser does not support the video tag.


## Conclusion


Non-human identities are no longer a secondary concern. They access production databases, deploy code, and move sensitive data, which means that SOC 2 auditors will ask how organizations control, monitor, and manage these entities with the same rigor applied to human users. Teleport[Machine & Workload Identity](https://goteleport.com/docs/reference/machine-workload-identity/) provides both the controls and the evidence trail. Every credential issuance is attested, every policy decision is logged, and every configuration change is auditable.


The control mappings covered here address seven controls across CC6, CC7, and CC8. Organizations preparing for SOC 2 should evaluate whether their current access control strategy adequately covers the non-human identities running their infrastructure.


#### Simplify SOC 2 compliance: Human and non-human identities


[Read the full SOC 2 control mapping](https://goteleport.com/use-cases/soc2-compliance/) to learn how Teleport:


- Proves human and non-human activity across infrastructure
- Enforces least privileged access across all identities
- Gives auditors complete audit trails for every human, machine, and AI agent


---


> **About the author:**[Kayne McGladrey](https://goteleport.com/learn/author/kayne-mcgladrey/) is a CISSP, Former Defense Industrial Base CISO, senior IEEE member, and cybersecurity strategist with extensive experience in governance, risk, and compliance programs. Kayne advises organizations on aligning emerging technologies, including AI systems, with established security frameworks such as SOC 2, ISO 27001, and NIST.
