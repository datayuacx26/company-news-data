---
schema_version: "1.0.0"
document_id: "ba8b2a999fb9e1a7df6361e7fe0e7bc11275bb4e62adaa55ae21db85cc230c46"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/sama-adgm-dfsa-compliance/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:14:11.262520+00:00"
fetched_at: "2026-08-13T02:14:12.652858+00:00"
content_hash: "sha256:69593b3969b41acf9fc232cda6cb4a4fd7953a4d94b586e83cf3b555facdee8d"
---

# Navigating SAMA, ADGM & DFSA Requirements with Teleport

**Read this article to learn:**


- How SAMA's Cyber Security Framework, ADGM's Cyber Risk Management Rules, and the DFSA Rulebook define identity, access, and audit trail expectations for cloud service providers.
- How long-lived credentials, VPNs, bastions, and platform-specific access controls can make governance difficult to centralize and demonstrate to regulators.
- How a unified identity layer helps CSPs operationalize controls without sacrificing the agility sovereign cloud environments demand.


The Middle East, especially the UAE and Saudi Arabia, has become a priority market for cloud service providers (CSPs) as governments accelerate digital transformation across public and private sectors.


The opportunity is real, but so is the complexity.


In our work with clients and regulators, Coalfire has seen that market entry often hinges less on commercial certifications and more on meeting strict data sovereignty and cybersecurity requirements.


Many CSPs built their go-to-market models around a familiar split: commercial cloud offerings optimized for speed, scale, and feature velocity, and public sector offerings designed for higher security baselines, tighter operational controls, and reduced data movement. That distinction is starting to blur in Western markets as[regulations like NIS2](https://goteleport.com/use-cases/nis2-compliance/) expand baseline security expectations and modernization efforts[such as FedRAMP 20x](https://goteleport.com/blog/automating-identity-access-fedramp-20x/) push for faster qualification of commercial solutions.


In the Gulf, regulators have taken a centralized and national approach to cybersecurity and data governance from the outset, especially in sensitive sectors such as financial services. That has raised the bar for data localization, operational control, and access security. For CSPs, this creates a new challenge: success in these markets requires more self-contained, region-specific architectures than the commercial cloud patterns they have used elsewhere.


From a continuous compliance perspective, this challenge also extends to how identities, privileged access, third-party access, and audit evidence are managed across region-specific environments. Traditional patterns built around long-lived credentials, VPNs, bastions, and platform-specific access controls can make that governance difficult to centralize and even harder to demonstrate. This is where a[unified identity and access layer](https://goteleport.com/platform/unified-identity-layer/) becomes strategically important.


## Meeting Kingdom of Saudi Arabia SAMA requirements


The Saudi Arabian Monetary Authority (SAMA) is Saudi Arabia’s central bank and a primary financial-sector regulator, overseeing banks, insurers, financing companies, credit bureaus, and financial market infrastructure. Its[Cyber Security Framework (CSF)](https://rulebook.sama.gov.sa/en/cyber-security-framework-2) establishes cybersecurity expectations for these regulated Member Organizations.


The CSF draws on established industry and international standards, including NIST, ISF, ISO, Basel, and PCI. It combines governance, risk management, identity and access management, operational security, third-party oversight, and measurable control effectiveness. Importantly for cloud service providers, the framework also establishes a strong data-location expectation: Member Organizations should use cloud services located in Saudi Arabia unless they obtain explicit approval from SAMA to use services outside the Kingdom.


Specific to the Identity and Access Management (IAM) domain, the guidelines within section 3.3.5 are clear to emphasize systematic solutions that support centralization, automation, MFA for remote access and privileged access, and a comprehensive audit trail for IAM activities. Further, the CSF has defined (within section 3.3.13) MFA enforcement for a slew of service user activities, starting with basic registration and sign-on. Above all, the CSF requires strong, metrics-driven effectiveness measurement by organizations throughout its narrative, making the case for an agile, programmatic approach to identity management.


## Meeting United Arab Emirates FSRA and DFSA requirements


A major financial hub connecting the EU, Asia and Africa, the UAE has consistently promoted business friendly environments that greatly attract FinTech ventures. With the advancements in digital banking, the country’s two premier “financial free zones” have established strict cybersecurity requirements commensurate with industry best practices.


The[Abu Dhabi Global Market (ADGM)](https://www.adgm.com/about) is highly competitive in digital assets, FinTech, and requires regulated operators to demonstrate strong Cyber Risk Management activities prescribed by its regulator, the Financial Services Regulatory Authority (FSRA).


On the topic of protecting ICT assets, the framework recommends centralized and automated access management (3.5.7), overseen by a designated individual. ADGM expects firms to manage access through approval workflows, least privilege, prompt revocation, and recurring access reviews, while also treating privileged accounts with extra care – the assignment of a unique identifier, rather than privilege escalation to a regular operations account. Specifically, the framework also calls for strong authentication, MFA for internet-facing systems and privileged access, and encryption for user-to-system communications (3.5.8). The regulator emphasizes the need for demonstrable evidence for access risk mitigation activities, placing the onus on operators to approach IAM strategically.


The[Dubai International Financial Center (DIFC)](https://www.difc.com/who-we-are) is a longer established zone, hence featuring a deep, mature ecosystem for traditional banking, wealth management, and FinTech. The regulator DFSA urges member organizations to establish a Cyber Risk Management framework consulting well known standards from ISO, NIST, Cloud Security Alliance (CSA) and the CIS.


DFSA expects firms to manage user access through formal approval, least-privilege assignment, prompt revocation, and regular access reviews, including detection of dormant or excessive accounts (5.5.8). DFSA also calls for MFA on all internet-accessible systems and privileged access, plus encrypted communication between users and systems (5.5.9). Like the ADGM, the DFSA also wants firms to separate admin activity from normal business use and keep privileged rights tightly limited.


## How Teleport supports compliance in Saudi Arabia and the UAE


As a global compliance advisory and engineering firm, Coalfire is a trusted name that builds sovereign cloud solutions embedded with automation, programmatic management capabilities and latest security technologies/practices.


Where strict data localization requirements are mandated, Coalfire packages environment specific IAM solutions that can help federate multiple sources, apply comprehensive access control requirements, and maintain a rigorous audit posture with Teleport.


### A unified identity layer


Teleport provides a unified identity layer for managing users, assets, AI agents, and machines across cloud, on-premises, or hybrid infrastructure, treating each as a first class identity while streamlining Single Sign On (SSO), MFA,[Role Based Access Control (RBAC)](https://goteleport.com/docs/zero-trust-access/rbac-get-started/) and Attribute Based Access Control (ABAC).[Device Trust](https://goteleport.com/docs/zero-trust-access/device-trust/) policies support formal authorization, least privilege, and identity attribution across the enterprise, rather than being limited to cloud tenancy.


### Just-in-time access and eliminating standing privileges


Extending administrative functions to support Infrastructure as code (IaC), Teleport is tailormade for agile cloud environments that are managed programmatically. Access Requests provide[Just-in-Time (JIT) elevation](https://goteleport.com/use-cases/just-in-time-access/) and approval workflows for sensitive infrastructure, while per-session MFA,[short-lived X.509 certificates](https://goteleport.com/docs/reference/architecture/authentication/#x509-certificates) , passwordless authentication, and automatic expiry eliminate standing privileges, shared credentials, and long-lived SSH keys — high risk elements within a sensitive data storage environment.


Read this guide to learn more about short-lived certificates for financial infrastructure:[Certificate-Based Authentication for Payment & Banking Infrastructure](https://goteleport.com/blog/certificate-based-authentication-payment-banking/)


### Consistent controls across infrastructure


Teleport applies these controls consistently across servers, databases, Kubernetes clusters, cloud consoles, internal applications, and desktops, with resource-level policies that help limit lateral movement and control third-party access. Teleport’s[Machine & Workload Identity](https://goteleport.com/docs/machine-workload-identity/getting-started/) extends the same model to service accounts, CI/CD pipelines, workloads, and AI agents through short-lived, automatically renewable credentials.


### Automatic detection and enrollment of infrastructure resources


A frequently underserved area within IAM solutions is continuous inventory management. Teleport’s unified inventory and[auto-discovery](https://goteleport.com/docs/enroll-resources/auto-discovery/) provides increased visibility into protected infrastructure and can be used to map identities to assets.


These features help organizations understand the threat landscape from an unauthorized access lens. Automatic access reviews and[identity locking](https://goteleport.com/docs/identity-governance/locking/) support timely revocation and periodic governance for dynamic environments, where traditional human lead reviews are simply unscalable.


### Audit-ready visibility


[Session recording](https://goteleport.com/docs/reference/architecture/session-recording/) , live session monitoring, detailed and centralized event logs across resources, and SIEM integrations create evidence for access requests, approvals, privileged activity, and security monitoring.


## Compliance beyond data sovereignty


For cloud service providers entering the Gulf’s regulated financial markets, compliance is not simply a matter of where data is stored and encrypted. There is great emphasis on how access is granted, protected, monitored, and evidenced across the environments that process critical data.


SAMA, ADGM, and DFSA each articulate this expectation within their guidelines, and the common theme is that organizations need stronger control over identities, privileged access, third parties, and continuous auditability to minimize access risks. Teleport can help CSPs operationalize these controls through a unified identity layer, supporting secure access without sacrificing the agility required to operate sovereign cloud environments.


#### Accelerate KSA and UAE compliance


Learn how Teleport helps organizations around the world[accelerate compliance and simplify audits with unified identity, zero trust, and identity-traceable audit trails.](https://goteleport.com/use-cases/compliance/)
