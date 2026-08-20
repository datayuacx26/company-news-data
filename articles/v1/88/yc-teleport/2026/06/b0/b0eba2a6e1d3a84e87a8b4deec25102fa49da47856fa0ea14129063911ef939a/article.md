---
schema_version: "1.0.0"
document_id: "b0eba2a6e1d3a84e87a8b4deec25102fa49da47856fa0ea14129063911ef939a"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/automating-identity-access-fedramp-20x/"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:6c39bacf13a7a3af8bc50591d849da1da60cacf4d5948ac4a8f56038bf72b9af"
---

# Automating Identity and Access for FedRAMP 20x KSIs with Teleport

#### Read this article to learn:


- Why traditional identity and access models struggle to meet FedRAMP 20x persistent validation requirements
- Why machine-to-machine authentication is the most common gap in FedRAMP 20x readiness and how to close it
- How a unified audit trail produces the continuous, machine-readable evidence that satisfies FedRAMP 20x Key Security Indicator (KSI) validation


Cloud service providers preparing for FedRAMP 20x are encountering a fundamentally different authorization model than the one their compliance programs were built around. The traditional FedRAMP path produced lengthy System Security Plans, point-in-time assessments, and human-readable narrative evidence. FedRAMP 20x replaces much of that with[Key Security Indicators (KSIs)](https://www.fedramp.gov/docs/20x/key-security-indicators/) ; measurable security capabilities that must be persistently validated through machine-readable evidence rather than described in narratives.


For the Identity and Access Management (KSI-IAM) and Monitoring, Logging, and Auditing (KSI-MLA) themes in particular, this shift moves the conversation from “is there a policy” to “can you continuously prove the capability is functioning.” That bar is harder to clear with the credential-and-bastion access patterns that emerged in earlier cloud builds, where MFA, least privilege, and audit completeness are implemented per-platform and held together by manual processes.


At[Coalfire](https://coalfire.com/) , FedRAMP 20x readiness engagements reveal a consistent pattern: organizations with strong day-to-day security practices nonetheless struggle to produce the persistent, automated evidence the new model expects. As Coalfire works with CSPs on transition strategies, Teleport has become a common architectural component for organizations consolidating human and machine access onto a unified identity layer that can produce the kind of continuous, machine-readable signal that KSI validation depends on.


## Why traditional identity and access models struggle under persistent validation


Traditional FedRAMP environments often rely on long-lived credentials that can create challenges under[20x's persistent validation model](https://www.fedramp.gov/docs/20x/persistent-validation-and-assessment/) , including:


- CI/CD pipelines that authenticate to cloud APIs using static IAM keys committed to secret stores
- Service-to-service authentication that depends on shared tokens or hardcoded credentials with no automated rotation path
- Administrative access that flows through bastions and VPNs with per-platform MFA implementations
- Audit telemetry that sits in tool-specific logs that must be manually correlated when an assessor asks for it


While these patterns can pass a point-in-time review, they cannot easily produce the persistent evidence FedRAMP 20x asks for under KSI-IAM-SNU (Securing Non-User Authentication), KSI-IAM-MFA (Enforcing Phishing-Resistant MFA), and KSI-MLA-OSM (Operating SIEM Capability). All of these requirements expect the capability to be continuously verifiable, not periodically attestable.


***Coalfire Observation:*** *During FedRAMP 20x readiness work, the most consistent gap observed is machine-to-machine authentication. Static IAM access keys in CI/CD pipelines, service account tokens that never expire, and shared workload credentials are the dominant failure modes against KSI-IAM-SNU. However, these are rarely security oversights — instead, they are the result of a credential-issuance model that was not built to be automated, rotated, or continuously validated.*


## Short-lived cryptographic identity as a continuous compliance foundation


A typical FedRAMP 20x-aligned architecture deployed by Coalfire positions the[Teleport Auth Service](https://goteleport.com/docs/reference/architecture/#teleport-auth-service) as the issuing authority for short-lived[cryptographic identities](https://goteleport.com/learn/what-is-cryptographic-identity/) for humans and machines. Engineers authenticate through the[Teleport Proxy](https://goteleport.com/docs/reference/architecture/proxy/) via SSO with phishing-resistant MFA enforced per session. Workloads, CI/CD pipelines, and service accounts authenticate through[Teleport Machine & Workload Identity](https://goteleport.com/docs/machine-workload-identity/introduction/) , receiving short-lived, SPIFFE-compatible certificates issued at runtime in place of static cloud keys or long-lived tokens. Federation into cloud native services eliminates the need for the long-lived cloud keys that drive most KSI-IAM-SNU gaps.


All identity issuance, access events, and credential rotation events flow into a unified audit trail that downstream SIEM tooling consumes as a continuous evidence stream. For CSPs targeting a FedRAMP Moderate or High, Teleport Enterprise FIPS builds ensure the cryptographic modules underlying that identity layer compile against an actively-validated FIPS 140 module, addressing the validated-modules expectation that becomes required at High and recommended at Moderate.


## How Teleport implements capabilities for FedRAMP 20x KSI-IAM, KSI-MLA, and cryptographic modules


The capabilities most relevant to FedRAMP 20x cluster around the IAM and MLA themes, with Teleport’s FIPS posture relevant to the cryptographic modules expectation. The framing below uses the distinction Coalfire applies during readiness: implements where the capability meets the KSI out of the box, supports or enables where it provides the mechanism but requires configuration, and accelerates where the value is primarily in continuous evidence.


***Identity and Access Management (KSI-IAM):***


- Teleport implements KSI-IAM-MFA by enforcing per-session phishing-resistant MFA at the proxy across all infrastructure protocols, applying a single MFA policy where multiple identity providers and tool-specific implementations would otherwise be in play.
- Teleport implements KSI-IAM-SNU through Machine & Workload Identity. Non-human identities authenticate via short-lived certificates issued at runtime, federated into cloud IAM where applicable, removing the static-keys condition that drives most non-user authentication gaps.
- Teleport implements KSI-IAM-JIT through[Access Requests](https://goteleport.com/docs/identity-governance/access-requests/) , which provide role and attribute-based just-in-time authorization for humans, machines, and AI agents. Teleport supports KSI-IAM-ELP (least privilege) and KSI-IAM-AAM (automated account management) through RBAC, Access Lists for periodic review, and SSO-driven role mapping.


***Monitoring, Logging, and Auditing (KSI-MLA):*** Teleport implements KSI-MLA-LET for the infrastructure access event class. Every session, command, file transfer, database query, and credential issuance is captured with verified identity attribution. Teleport accelerates KSI-MLA-OSM by producing a centralized, tamper-resistant audit stream that a CSP’s SIEM can consume directly, supporting the continuous, machine-readable evidence model that 20x emphasizes.


***Using Cryptographic Modules:*** For CSPs at FedRAMP Moderate (where validated modules are recommended) or High (where they are required), Teleport Enterprise FIPS implements the validated-modules expectation by compiling against a cryptographic module with an active CMVP certificate. This is one of the rare places where the underlying tooling either is or is not authorized; there is no configuration path that makes a non-FIPS build acceptable in a High environment.


## How Teleport produces the kind of evidence FedRAMP 20x expects


The major structural change in FedRAMP 20x is that evidence stops being assembled at assessment time and starts being generated continuously by the systems themselves. Identity and access is one of the cleanest places to demonstrate that shift, because with Teleport, every action (authentication, authorization, session, credential issuance, revocation) is an event with a verifiable identity.


When Coalfire supports CSPs through 20x readiness, the most common question on KSI-IAM is some form of “show us continuous proof that this capability is functioning.” Environments that have consolidated human and machine identity into a single platform like Teleport can easily answer that question by pointing the assessor to a comprehensive audit trail across every resource that clearly attributes sessions to a verified identity and generates evidence continuously. Environments that have identities spread across multiple tools answer it by writing a query against five sources and hoping the time-stamps align. The KSI language is the same in both cases; the operational maturity is not.


## Building a continuous compliance posture for the FedRAMP 20x era


FedRAMP 20x reflects a broader direction in federal compliance: from documentation about security to demonstrated, continuously validated security capability. Identity is one of the highest-leverage places to make that shift, because IAM and MLA capabilities together touch the majority of KSI themes and produce the most directly automatable evidence.


At Coalfire, leveraging Teleport within FedRAMP 20x-aligned customer architectures has supported CSPs working to consolidate human and machine identity, eliminate the long-lived credentials that drive non-user authentication findings, and produce the continuous audit signal that KSI validation depends on. For organizations planning their entry path under the post-Phase 2 standard, Teleport provides audit-ready continuous validation and machine-readable evidence aligned to FedRAMP 20x KSIs.


#### Accelerate FedRAMP compliance: Humans, machines, and AI


[Discover how Teleport simplifies FedRAMP compliance](https://goteleport.com/use-cases/fedramp-compliance/) , increases infrastructure resilience, and accelerates audits.


---


---


## Nicolas Morris


Nicolas Morris is Managing Principal in Coalfire’s Cloud Services practice, leading strategy, advisory, design, deployment, and managed services for private sector SaaS, PaaS, and enterprise clients. He has deep expertise guiding organizations through secure cloud transformations across AWS, Azure, GCP, and hybrid environments, aligning with compliance frameworks including FedRAMP, FISMA, NIST CSF, HITRUST, and PCI DSS. Since joining Coalfire in 2015, Nicolas has developed high-impact services and capabilities that drive growth in cloud and managed services, leveraging a background in public sector cybersecurity for DHS, the National Reconnaissance Office (NRO), and the U.S. Coast Guard (USCG) and Navy (USN). He holds an M.S. in Systems Engineering (Cyber Security) from Virginia Tech, a B.S. in Systems Engineering from the University of Virginia, and multiple industry certifications such as CISSP.
