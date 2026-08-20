---
schema_version: "1.0.0"
document_id: "3f55044c6b203d5c5d73bb58dccf65f52f7e32e68b04bd78f7b4645bd336001e"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/eu-cyber-resilience-act/"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:9e14a4ebf261283782cdea92d28c50094f09f4bb8a07d854aa343f102ea7a707"
---

# How to Meet EU Cyber Resilience Act (CRA) Requirements

**Read this article to learn:**


- Why the majority of the CRA's Annex I requirements are determined by how infrastructure manages identity, access, encryption, and audit.
- How fragmented security toolchains (separate VPNs, SSH keys, and PAM solutions) lead to compromise risks that violate CRA requirements.
- What CRA-compliant infrastructure looks like in practice: short-lived cryptographic identities, hardware-rooted device trust, and a unified audit trail.


In March 2026, attackers from the TeamPCP group compromised Trivy ([CVE-2026-33634](https://nvd.nist.gov/vuln/detail/CVE-2026-33634) ) — a widely-deployed open-source vulnerability scanner running in thousands of CI/CD pipelines — and turned it into a credential harvester. SSH keys, Kubernetes secrets, cloud tokens — secrets accessible to any pipeline that ran a compromised version — were exposed. The attacker retained access long enough to exfiltrate newly rotated secrets before the window closed.


The EU Cyber Resilience Act exists because the tools you trust to secure your infrastructure can also become the fastest path through it.


## What is the EU Cyber Resilience Act?


The Cyber Resilience Act (CRA), formally[EU Regulation 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) , is a European Union regulation that entered into force in December 2024, establishing mandatory cybersecurity requirements for all products with digital elements sold in the EU market. Full enforcement begins in December 2027, and non-compliance carries fines of up to €15 million or 2.5% of global annual turnover, whichever is higher.


### Which products are in scope for EU Cyber Resilience Act requirements?


The Cyber Resilience Act covers almost every product with digital elements that you deliver to a customer. That includes the app on a smartphone, the firmware in an IoT sensor, the software in a connected machine, and the on-premise system installed in a factory. If a product can connect to a device or a network, assume it is in scope for CRA.


The line between what is in scope or not sits at the customer site. For example, pure cloud services and web applications stay outside the scope of the CRA as long as there is nothing to install on the customer's side. However, the moment your service ships an agent, edge component, or any software that runs on the customer's hardware, you are back in scope.


The sector exceptions are few, and exist because those sectors already carry their own cybersecurity rules. For example:


- Motor vehicles fall under Regulation (EU)[2019/2144](https://eur-lex.europa.eu/eli/reg/2019/2144/oj/eng) , which brings in[UN Regulation No. 155](https://unece.org/transport/documents/2021/03/standards/un-regulation-no-155-cyber-security-and-cyber-security?__cf_chl_f_tk=0tJ_TkB2LlnexH73absjijDyVoiSja2ax0MbfwN5u7A-1782920641-1.0.1.1-BfDvqRPUH5t10geFVAkpYWbA4i5cndKNmIrUdOzM8ho) . That carve-out only covers systems delivered inside a type-approved vehicle. Supplier components outside that regime stay in scope.
- Civil aviation products certified under Regulation (EU)[2018/1139](https://eur-lex.europa.eu/eli/reg/2018/1139/oj/eng) are excluded.
- So are medical devices and IVDs under Regulations (EU)[2017/745](https://eur-lex.europa.eu/eli/reg/2017/745/oj/eng) and[2017/746](https://eur-lex.europa.eu/eli/reg/2017/746/oj/eng) , and marine equipment under[Directive 2014/90/EU](https://eur-lex.europa.eu/eli/dir/2014/90/oj/eng) .


For most manufacturers, the starting assumption is that they are in scope for CRA.


Most coverage of the CRA focuses on the obvious: vulnerability disclosure, software bills of materials, secure update mechanisms. Important, yes. But they're the visible tip of a much larger obligation. Read the CRA's Annex I closely — the part that lists what products actually *must do* — and you will find the majority of its essential requirements are not about code quality or patch cadence. Instead, they are about **how your infrastructure handles identity, access, encryption and audit** . They are about the architectural decisions that determine whether an attacker who finds a vulnerability can actually *exploit* it.


The CRA is, at its core, an infrastructure challenge. Here's why.


## What does the CRA actually require?


Strip away the legal language, and the CRA's essential requirements (outlined in[Annex I](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_I) ) cluster into two domains, each of which lives at the infrastructure layer rather than the application layer.


### Identity, access, and data protection


**Identity and access control** (Annex I, Pt I, (2)(d)): Products must "ensure protection from unauthorized access by appropriate control mechanisms." How every connection is authenticated is an infrastructure architecture decision, not an application feature. Shared secrets and default credentials are explicitly targeted. Users, machines, workloads, devices, and resources should each be assigned a unique[cryptographic identity](https://goteleport.com/learn/what-is-cryptographic-identity/) .


**Encryption in transit and at rest** (Annex I, Pt I, (2)(e)): All stored and transmitted data must be protected via encryption by default. Every protocol, connection path, and data store must enforce encryption at the infrastructure layer.


**Integrity protection** (Annex I, Pt I, (2)(f)): The integrity of data, programs, and configuration must be protected. This covers configuration drift, unauthorized changes, and tampered build artifacts — the kind of integrity that can only be enforced where infrastructure and deployment intersect.


### Observability and resilience


**Attack surface minimization** (Annex I, Pt I, (2)(j)): Products must "be designed to limit attack surfaces, including external interfaces." Every open port, exposed service, and unused protocol is a liability. How you design your infrastructure topology can either reduce your attack surface or expand it.


**Monitoring and audit** (Annex I, Pt I, (2)(l)): Products must "provide security-related information by recording and monitoring relevant internal activity.": Not optional logging, but built-in, operational monitoring, which requires infrastructure-level instrumentation instead of application-level log statements.


**Incident impact reduction** (Annex I, Pt I, (2)(k)): Products must "be designed to reduce the impact of an incident using appropriate mitigation mechanisms." Blast radius containment is an infrastructure design property. Assume breach and architect accordingly.


## How the ENISA made CRA requirements concrete


In March 2026, the European Union Agency for Cybersecurity (ENISA) published the[Security by Design and Default Playbook (v0.4)](https://www.enisa.europa.eu/sites/default/files/2026-03/ENISA_Secure_By_Design_and_Default_Playbook_v0.4_draft_for_consultation.pdf) , translating the CRA's legal requirements into 22 actionable security principles. The draft removes ambiguity from existing obligations.


Where the CRA says "appropriate control mechanisms," ENISA specifies *the definition of appropriate* as every user, service, and process operating with minimum privilege. Where the CRA says "unique cryptographic identity," ENISA specifies *how unique:* compromise of one device must not enable compromise of another, and identity must be designed as core architecture covering users, devices, services, and administrators alike. And where the CRA says "limit attack surfaces," ENISA clarifies that trust boundaries must be explicit and enforced wherever data crosses from a trusted domain to a less trusted one.


The Playbook makes something clear that the legal text only implies: **the CRA's requirements are deeply architectural.** You cannot patch your way to compliance or add a logging library and call it monitoring. Instead, these obligations will require your infrastructure to work differently, rather than simply adding different features.


## Why do traditional security approaches fall short?


Traditionally, organizations have approached security by adding layers on top of their infrastructure, such as:


- VPNs for network access
- SSH key management tools
- Separate PAM solutions for privileged accounts
- A SIEM that ingests logs from dozens of sources
- Password vaults for credentials


Under the CRA, this fragmented approach creates three problems.


### 1. Every integration point adds to the attack surface


The CRA requires minimizing attack surfaces, including external interfaces. A typical infrastructure stack exposes[SSH on port 22](https://goteleport.com/blog/ssh-port-22/) , RDP on 3389, database ports, Kubernetes API servers, VPN endpoints, and application ports — each with its own authentication mechanism, its own credential lifecycle, and its own audit trail (or lack thereof). Every integration point between security tools is a potential misconfiguration and a potential harvest point.


The TeamPCP attack demonstrated this risk and its consequences. The compromise expanded from multiple tools into GitHub Actions. Attackers didn't need to break each tool independently. Instead, they exploited the trust relationships *between* integrated tools. One compromised node in the security toolchain gave them access to credentials managed by adjacent tools.


### 2. Static credentials are the vulnerability the CRA targets


The CRA requires unique cryptographically secure identity and the elimination of shared secrets. But most infrastructure still runs on static credentials, including SSH keys that never expire, database passwords stored in environment variables, and API tokens committed to repositories.


Post-incident analysis of the TeamPCP attack revealed that credential remediation was not comprehensive, which enabled retained secrets to be reused in subsequent attacks. A leaked credential that grants access to multiple systems directly violates the CRA's requirement that compromise of one component should not enable compromise of others. Hundreds of thousands of static, long-lived secrets risked being exposed and harvested.


### 3. Audit trails are fragmented or absent


The CRA requires recording and monitoring of relevant internal activity. But when identity and access is managed by five different tools, audit events are now scattered across five different log formats, retention policies, and levels of completeness. Reconstructing what happened during an incident becomes an archaeological exercise rather than a simple query.


Detection strategies should be centred on unified telemetry pipelines capable of correlating signals across the credential lifecycle — precisely the kind of architectural visibility that fragmented identity and access toolchains cannot provide.


## Infrastructure identity as the control point


The CRA's requirements converge on a single architectural insight: infrastructure identity and access is the control point where most of these obligations are either met or missed.


Think about what happens when an engineer connects to a production database:


- **Identity** : Who are they? How was their identity verified? Is the verification cryptographic or password-based?
- **Authorization** : Do they have permission? Is that permission explicit or inherited from an overly broad role? When was it last reviewed?
- **Device trust** : Is the device they're connecting from enrolled, patched, and attested?
- **Encryption** : Is the connection encrypted end-to-end? Is there a plaintext fallback?
- **Audit** : Is this connection recorded? Can it be replayed? Will it appear in the SIEM within seconds?
- **Blast radius** : If this connection is compromised, what else can the attacker reach?


Every one of these questions maps to a specific CRA requirement. And every one of them is determined by infrastructure identity and access controls , not the application code, the CI/CD pipeline, or the vulnerability scanner. This is why the CRA is an infrastructure problem. The legislation targets the exact layer that most organizations treat as plumbing: invisible, assumed to work, and rarely audited.


## What does CRA-ready infrastructure look like?


Based on the CRA's essential requirements and the ENISA's Playbook, CRA-compliant infrastructure identity and access has specific, testable properties:


**No static credentials anywhere** : Every connection is authenticated via short-lived, cryptographic identities. There are no SSH keys to leak, database passwords to rotate, or secrets to vault.[Ephemeral privileges](https://goteleport.com/learn/what-are-ephemeral-privileges/) expire after a brief period, ensuring that access is only available for the duration necessary to complete specific tasks, leaving nothing for an attacker to steal that remains valid long enough to use.


**Deny-by-default access** : No user, service, or process can reach any resource without an explicit grant. Permissions are limited to specific resources, time-bound, and auditable with[just-in-time (JIT) access](https://goteleport.com/use-cases/just-in-time-access/) , where elevated access requires approval and expires automatically.


**Hardware-rooted device trust** : The device connecting to infrastructure is attested via hardware (TPM 2.0, Secure Enclave), not just checked for a software agent. Device identity is cryptographically bound to every session.


**Encrypted, authenticated connections without exception** : All communication uses[mutual TLS](https://goteleport.com/learn/what-is-mtls/) or equivalent. No plaintext fallback, "convenience" protocols, or VPNs as a trust boundary.


**A unified audit trail** : Every access event across SSH, databases, Kubernetes, applications, and cloud consoles produces a structured, exportable audit event. Session recordings enable forensic playback, and logs are exported to SIEMs in real time.


**Unified policy across all protocols** : One[unified identity layer](https://goteleport.com/platform/unified-identity-layer/) , policy engine, and centralized audit trail. Not five tools with different security postures and gaps.


## Complying with CRA requirements: Before and after


Consider a single operation: a platform engineer needs to query a production PostgreSQL database to diagnose a latency spike.


**Before (traditional stack):** The engineer VPNs in using a shared team profile. They SSH to a bastion host with a key pair generated eighteen months ago. From there, they connect to PostgreSQL using a service account password stored in a shared vault. The VPN logs show a connection, SSH logs show a session, and database logs show a query, but these are three separate systems and formats with nothing linking them. The credentials used will still be valid tomorrow, next month, or next year,and if any of those credentials were exfiltrated today, there is no expiry to limit the damage.


**After (CRA-ready infrastructure):** The engineer authenticates via SSO with hardware-backed MFA. They request database access through an infrastructure identity platform that checks their role, the time of day, and their device posture. After TPM attestation confirms the laptop is enrolled and patched, the platform issues a short-lived X.509 certificate that provides read-only access to that specific database and automatically expires after 30 minutes. Every step — authentication, authorization, the database session, and the queries executed — appears as structured events in a single audit trail, linked by identity, and exported to the SIEM in real time. There are no static credentials left behind and no standing privileges that persist beyond the session.


In the first scenario, security depends on every individual credential remaining uncompromised across three independent systems, none of which are aware of each other. A single leaked key, password, or token grants persistent access with no expiration and no way to trace the resulting activity back through a unified record. In the second, CRA requirements are ingrained into the infrastructure layer itself. Access requires proof of identity, proof of device integrity, and an explicit policy grant, all of which produce correlated audit events and none of which persist beyond the session.


While the CRA does not prescribe specific tooling, it does require the outcomes that only the second architecture can produce: cryptographic identity, minimal privilege, integrity-attested devices, encryption without exception, and a[complete audit trail](https://goteleport.com/use-cases/compliance/) that can answer "who did what, when, and from where" in a single query.


---


> *The EU Cyber Resilience Act (Regulation 2024/2847) applies to products with digital elements placed on the EU market. ENISA's Secure by Design and Default Playbook (v0.4) provides non-binding implementation guidance. This article is for informational purposes and does not constitute legal advice.*


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
