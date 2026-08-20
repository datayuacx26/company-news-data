---
schema_version: "1.0.0"
document_id: "82ba2c7f4e4cb23da7014dca4ae043f0122269ecc7e0433a3715044fba285cce"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/eu-cra-secure-by-design/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:4073f5a16d0d3d248679ae8cbf325f3ba6e043e1675034b8fa61062c153f30a7"
---

# How Teleport Operationalizes the EU Cyber Resilience Act's Secure-by-Design Mandate

**Read this article to learn:**


- How ENISA’s secure-by-design playbook translates CRA requirements into practical infrastructure security expectations.
- Where cryptographic identity, least privilege, and a unified audit log fit into CRA-aligned infrastructure controls.
- How Teleport maps to ENISA Playbook principles like Least Privilege, Strong Identity & Auth, and Logging, Monitoring & Alerting.


> *Beyond asking you to think about security, the CRA requires proof — in your architecture, your defaults, and your audit trail. Learn how Teleport maps to the CRA’s secure-by-design mandate.*


The[EU Cyber Resilience Act (CRA)](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act) enters its enforcement window in 2027, but preparations should start now.[ENISA's Secure by Design and Default Playbook](https://www.enisa.europa.eu/sites/default/files/2026-03/ENISA_Secure_By_Design_and_Default_Playbook_v0.4_draft_for_consultation.pdf) (v0.4, March 2026) translates the CRA's legal text into 22 actionable security playbooks, structured around architectural foundations, operational integrity, default hardening, and guided protection. Together, they represent the most prescriptive infrastructure security framework the EU has ever published.


The CRA's broad scope covers nearly every connected product sold in the EU. For the teams building and operating products with digital elements, security must be embedded into infrastructure, and proven.


Teleport is modernizing identity, access, and policy for infrastructure, providing[audit-ready controls](https://goteleport.com/use-cases/compliance/) designed around the same principles the CRA now mandates. This article walks through this alignment in detail.


## 1. Cryptographic identity replaces static credentials


**CRA requirement:** Products must ensure protection from unauthorized access by appropriate control mechanisms (Annex I, Part 1, §2d) and protect the confidentiality of data via encryption (§2e).


**ENISA Playbooks:** Strong Identity and Authentication Architecture (4.3), Unique Device Identity and Secrets by Default (4.18), Restrictive Initial Access (4.16).


**How Teleport delivers:**


Teleport eliminates static credentials entirely: no SSH keys, database passwords, or long-lived API tokens. Every identity, human or machine, is authenticated via short-lived X.509 certificates issued by Teleport's internal[Certificate Authority (CA)](https://goteleport.com/docs/reference/architecture/authentication/) . These certificates are:


- **Tied to a user or service identity:** Each certificate is signed by the CA and bound to a unique, traceable user or service identity.
- **Task-based:** Access is granted based on role, device trust, and context — and expires automatically when the task is complete. There are no credentials to rotate, vault, or leak.
- **Mutually authenticated:** All connections use mTLS. Both client and server present certificates. Man-in-the-middle attacks are architecturally prevented.


For[machine and workload identities](https://goteleport.com/platform/machine-and-workload-identity/) , Teleport issues SPIFFE-compatible credentials (both JWT and X.509 SVID formats), following the[SPIFFE standard for service-to-service authentication](https://goteleport.com/docs/machine-workload-identity/workload-identity/spiffe/) . The` tbot` agent continuously writes short-lived certificates for CI/CD pipelines, services, and AI agents — replacing the hardcoded secrets that remain the leading attack vector in supply chain compromises.


This directly addresses the CRA's requirement that products ship with unique, per-device credentials and cryptographic identity (Annex I, Part 1, §2d), and ENISA's Playbook 4.18, which mandates that "compromise of a single device or a leaked credential can be used to attack other customers or the wider installed base."


## 2. Deny-by-default access control


**CRA requirement:** Products must be made available with a secure by default configuration (Annex I, Part 1, §2b) and be designed to limit attack surfaces (§2j).


**ENISA Playbooks:** Least Privilege (4.2), Attack Surface Minimisation (4.4), Minimisation of Default Services (4.15), Restrictive Initial Access (4.16).


**How Teleport delivers:**


Teleport is restrictive by default. No user, service, or process can access any resource without an explicit grant. This is enforced at the architectural level through Teleport's[role-based access control (RBAC)](https://goteleport.com/docs/zero-trust-access/rbac-get-started/role-demo/) , where:


- **Deny rules take precedence over allow rules** , ensuring that guardrails cannot be overridden by permissive role assignments.
- Roles are **scoped to specific resources via labels** (e.g.,` env:production` ,` team:platform` ), enabling fine-grained access segmentation.
- Certificate subjects embed the user's roles, so **authorization is cryptographically bound** to the session — it cannot be spoofed or escalated without issuing a new certificate.


This maps precisely to ENISA's Playbook 4.2 ("Every user, service, and process operate with the feasible minimum permissions needed to do its job, no more, no longer than necessary.") and Playbook 4.15 ("Disable the non-essential features and services by default.").


## 3. Zero standing privileges and just-in-time access


**CRA requirement:** Reduce the impact of an incident using appropriate exploitation mitigation mechanisms (Annex I, Part 1, §2k).


**ENISA Playbooks:** Least Privilege (4.2), Defence in Depth (4.5).


**How Teleport delivers:**


[Just-in-time (JIT) access requests](https://goteleport.com/docs/identity-governance/access-requests/) allow, engineers to request access to specific resources or roles on-demand.Access granted through Teleport is:


- **Dual-authorized:** Configurable to require two or more reviewers. Requesters cannot approve their own requests.
- **Time-bound:** All elevated access automatically expires after a configurable period.
- **Fully audited:** The entire request lifecycle — who requested what, who approved it, when it expired — is captured in the audit log.


Approval workflows integrate with operational tools (Slack, Jira, PagerDuty, ServiceNow), and auto-approval rules can be conditioned on real-time signals like PagerDuty on-call status — enabling speed without sacrificing governance.


This is the operational implementation of ENISA's framing claim that "Security is not a static state, but a continuous process involving specific development guidelines, threat modelling, and the integration of security controls at the earliest stages of design to minimize the impact of cyberattacks." Standing privileges are a static-state model. JIT access makes privilege a controlled, auditable event.


## 4. Hardware-backed device trust


**CRA requirement:** Ensure an appropriate level of cybersecurity based on the risks (Annex I, Part 1, §1) and protection from unauthorized access (§2d).


**ENISA Playbooks:** Trust Boundaries and Threat Modelling (4.1), Defence in Depth (4.5).


**How Teleport delivers:**


Teleport's[Device Trust](https://goteleport.com/docs/reference/architecture/device-trust/) enforces that only registered, enrolled devices can reach infrastructure:


- **macOS:** Attestation uses the Secure Enclave processor to store device credentials and solve cryptographic challenges.
- **Windows and Linux:** Attestation uses TPM 2.0, with a private key that never leaves the hardware module.


Enrolled devices receive certificate extensions (` teleport-device-id` ,` teleport-device-asset-tag` ) that are verified at every connection. Enforcement can be applied per role or cluster-wide, covering SSH, databases, Kubernetes, applications, and Windows desktops. MDM integration with Jamf Pro and Microsoft Intune provides continuous device compliance verification, not just point-in-time enrollment.


ENISA's Playbook 4.1 states that "secure architectures make trust explicit rather than assumed." Device Trust makes the implicit assumption that "any laptop with the right password is trusted" explicit and verifiable.


## 5. Immutable audit trail with AI-powered risk scoring


**CRA requirement:** Provide security-related information by recording and monitoring relevant internal activity (Annex I, Part 1, §2l).


**ENISA Playbooks:** Logging, Monitoring, and Alerting (4.10), Incident Response and Recovery (4.12).


**How Teleport delivers:**


Every action in Teleport produces a structured audit event: authentication, session start and stop, access request lifecycle, configuration changes, and administrative operations. These events are:


- **[Structured as JSON and exportable](https://goteleport.com/docs/zero-trust-access/export-audit-events/)** via HTTP to SIEMs (Elastic, Splunk, Datadog, Panther).
- **[Recorded as full interactive sessions](https://goteleport.com/docs/reference/architecture/session-recording/)** for SSH, Kubernetes, databases, and Windows RDP, enabling forensic playback.
- **[Analyzed by AI](https://goteleport.com/platform/identity-security/session-analysis/)** , which sends session recordings to an LLM that produces structured summaries and risk classifications (Low, Medium, High, Critical) based on commands executed, obfuscation techniques, and behavioural patterns.


ENISA's Playbook 4.10 requires that "Systems should generate appropriate security-relevant logs, retain them for a defined period, and protect them from tampering so they can support investigation and compliance needs." Teleport's unified audit trail satisfies this structurally: logs are generated by the[Auth Service](https://goteleport.com/docs/reference/architecture/authentication/) , the single source of truth, and are exported, not stored in user-modifiable locations.


## 6. Secure communication by default


**CRA requirement:** Protect confidentiality of transmitted data via encryption (Annex I, Part 1, §2e) and protect the integrity of transmitted data, programs, and configuration (§2f).


**ENISA Playbooks:** Secure Communication by Default (4.17), Open Design (4.6).


**How Teleport delivers:**


All Teleport connections are encrypted and mutually authenticated from the first connection. The[Proxy Service](https://goteleport.com/docs/reference/architecture/proxy/) exposes only port 443 and uses reverse tunnels from private networks, eliminating the need for inbound firewall rules or VPN infrastructure. Teleport VNet proxies connections directly to internal resources, replacing traditional VPN tunnels entirely. There are no insecure fallback protocols,plaintext options, or "convenience" modes that weaken transport security.


This aligns with ENISA's Playbook 4.17 word-for-word: "All external communications must be encrypted and authenticated by default, with no support for insecure or plaintext protocols for convenience."


## 7. Unified identity and access controls across the full stack


**CRA requirement:** Products must be designed to limit attack surfaces, including external interfaces (Annex I, Part 1, §2j).


**ENISA Playbooks:** Attack Surface Minimisation (4.4), Configuration and Change Management (4.11).


**How Teleport delivers:**


Fragmented identities— different tools for SSH, different credentials for databases, separate policies for Kubernetes — introduce risk. Teleport provides a unified audit trail for every resource. This is what ENISA means when it says security must be "operationalised end-to-end across the full product lifecycle."


Teleport provides a centralized identity and policy framework across:


Resource Mechanism


SSH servers Certificate auth, full session recording, RBAC


Kubernetes HTTP proxy to K8s API, supporting all CNCF-certified distributions


Databases Native protocol proxy for PostgreSQL, MySQL, MS SQL, and more


AI agents and autonomous processes tbot-issued SPIFFE SVIDs (X.509/JWT), RBAC via bot roles


MCP servers Application Service proxy, RBAC with tool-level filtering


Web applications HTTP/TCP proxy via mTLS on port 443


Windows desktops RDP proxy with session recording, no exposed RDP ports


Cloud consoles On-demand, least-privileged access to AWS, Azure, GCP


GitHub Hardware MFA on every developer interaction


## 8. Access governance and lifecycle management


**CRA requirement:** Ensure an appropriate level of cybersecurity based on the risks (Annex I, Part 1, §1).


**ENISA Playbooks:** Lifecycle Management (4.7), Configuration and Change Management (4.11).


**How Teleport delivers:**


Teleport's[Access Lists](https://goteleport.com/docs/identity-governance/access-lists/) provide structured, auditable long-term access governance:


- Each Access List defines membership, owners, eligibility rules, and time-boxed enrollment.
- Periodic access reviews occur on configurable schedules, with owners controlling membership and maintaining historical records.
- SCIM integration with Okta, Microsoft Entra ID, AWS IAM Identity Center, and SailPoint automatically creates, updates, and deletes users and Access List memberships based on upstream identity provider changes — including automated deprovisioning on offboarding.


ENISA's Playbook 4.7 states that "security responsibilities extend beyond initial development. Components must be maintained, updated and eventually retired in a controlled manner." Access governance is one of the most commonly neglected aspects of lifecycle management. Teleport makes it structural rather than aspirational.


## 9. Agentic identity: Extending CRA controls to AI workloads


**CRA requirement:** Products with digital elements shall be designed, developed and produced in such a way that they ensure an appropriate level of cybersecurity based on the risks (Annex I, Part 1, §1).


**ENISA Playbooks:** Supply Chain Controls (4.14), Trust Boundaries and Threat Modelling (4.1).


**How Teleport delivers:**


As AI agents become components within products with digital elements, they fall within the CRA's scope. Teleport’s unified identity layer governs AI agents and MCP (Model Context Protocol) servers:


- Every AI agent receives a[cryptographic identity](https://goteleport.com/learn/what-is-cryptographic-identity/) with scoped access and a full audit trail.
- [Beams](https://www.beams.run/) provides ephemeral Firecracker VM environments with delegated cryptographic identity and no embedded secrets.
- MCP connections are authenticated, authorized and logged.


This is forward-looking but already operationally relevant. ENISA's Playbook 4.14 requires that developers "protect product integrity... focusing on the points where a compromise would have the largest impact…” As agentic workflows proliferate, unaudited AI access to infrastructure becomes exactly that kind of high-impact compromise point.


## Mapping Teleport to ENISA Secure-by-Design Principles


ENISA Principle Teleport Capability


4.1 Trust Boundaries & Threat Modelling Device Trust, Access Graph, mTLS everywhere


4.2 Least Privilege Deny-by-default RBAC, JIT access requests


4.3 Strong Identity & Auth Certificate-based auth, SPIFFE workload identity, MFA


4.4 Attack Surface Minimisation Unified identity layer, single port 443, no VPN


4.5 Defence in Depth Device Trust + cryptographic identity + RBAC + session controls + audit


4.6 Open Design Open-source core (Apache 2.0)


4.7 Lifecycle Management Access Lists, periodic reviews, SCIM deprovisioning


4.8 User-Centric Design Approval workflows via Slack/Jira, VNet for seamless access


4.10 Logging, Monitoring & Alerting Structured audit logs, SIEM export, AI session analysis


4.11 Configuration & Change Management RBAC-as-code, label-based resource targeting


4.12 Incident Response & Recovery Session recording, forensic playback, AI risk scoring


4.14 Supply Chain Controls Workload identity, agentic identity, build pipeline auth


4.15 Minimisation of Default Services Deny-by-default, no services exposed without explicit config


4.16 Restrictive Initial Access No default credentials, certificate-only auth


4.17 Secure Communication by Default mTLS on all connections, no plaintext fallback


4.18 Unique Device Identity & Secrets Per-session certificates, SPIFFE workload IDs


4.21 Transparent Security Posture Access Graph, audit dashboards, risk classification


4.22 Secure Recovery & Ownership Access Lists with ownership transfer, SCIM lifecycle


## The architectural argument


The CRA and ENISA's playbooks share a thesis that the security industry has debated for decades but is now codifying into law: security is an architectural property, inherent to how a system is designed. Teleport's alignment with the CRA isn't incidental. Both are built on the same foundational assumptions:


1.


**Contain and control production infrastructure with strong identity implementation.** The CRA requires protection from unauthorized access "by appropriate control mechanisms." Teleport implements this by making cryptographic identity the single prerequisite for every connection.


2.


**Defaults must be restrictive.** The CRA mandates secure-by-default configuration. Teleport's deny-by-default RBAC makes permissive access require explicit, auditable decisions.


3.


**Auditability is non-negotiable.** The CRA requires recording and monitoring of relevant internal activity. Teleport's built-in audit controls produce a structured, exportable, tamper-resistant record of every access event.


4.


**Credentials are the attack surface.** The CRA requires unique device identity and prohibits the use of shared secrets. Teleport eliminates static credentials entirely, replacing them with short-lived certificates that cannot be shared or leaked.


For organizations preparing for CRA enforcement, Teleport’s infrastructure identity and access capabilities align with the ENISA specification: identity, authentication, authorization, encryption, audit, and governance.


---


> *The EU Cyber Resilience Act (Regulation 2024/2847) applies to products with digital elements placed on the EU market. ENISA's Secure by Design and Default Playbook (v0.4) provides non-binding implementation guidance. This article maps Teleport capabilities to CRA requirements for informational purposes and does not constitute legal advice. Organizations should consult qualified legal counsel for compliance determinations.*


---


#### Accelerate EU CRA compliance with unified identity


[Discover how Teleport accelerates compliance](https://goteleport.com/use-cases/compliance/) with a unified identity layer that:


- Issues cryptographic identity to all human, machine, and AI actors
- Eliminates static credentials with short-lived certificates
- Provides a unified, identity-aware audit trail


Authored by[Think Ahead Technologies](https://think-ahead.tech/) , a Teleport partner.


---


## Maximilian Heck


Maximilian Heck is Head of Regulatory & Security at Think Ahead Technologies, where the EU Cyber Resilience Act is day-to-day business. He leads Kunnus, the company's CRA compliance platform, and advises manufacturers on CRA implementation across SBOM, vulnerability handling, ENISA reporting, and CE documentation. Before tech, he was office manager and spokesperson for a member of the German Bundestag and still follows EU regulation closely.


---


## Waldemar Kindler


Waldemar Kindler is CEO and Co-Founder of Think Ahead Technologies and an engineer by training. Before founding the company, he was Lead Infrastructure Consultant at ThoughtWorks, Developer Advocate at Schwarz IT, and a lecturer in platform engineering at DHBW Mosbach. Think Ahead is an official Teleport reseller and support partner, and his focus is making the EU Cyber Resilience Act requirements practical instead of painful. He is a certified NIS-2 expert and iSAQB-certified software architect.
