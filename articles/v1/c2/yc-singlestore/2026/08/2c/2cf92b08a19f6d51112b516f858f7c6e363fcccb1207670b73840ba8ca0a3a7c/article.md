---
schema_version: "1.0.0"
document_id: "2cf92b08a19f6d51112b516f858f7c6e363fcccb1207670b73840ba8ca0a3a7c"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/cloud-database-pci-dss-compliance/"
published_at: "2026-08-03T13:14:36+00:00"
first_seen_at: "2026-08-03T20:25:04.686479+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:a3b2643f7f4b181271748b474beb10001777c34297db5fe8b0168ecf038531a6"
---

# SingleStore Helios Achieves PCI DSS v4.0.1 Compliance

# SingleStore Helios Achieves PCI DSS v4.0.1 Compliance


Aug 3, 2026


•


6 min read


•


[Nishanth Singarapu, Security Leader - Cyber Security Architecture, GRC & Product](https://www.singlestore.com/blog/author/nishanth-singarapu/)


SingleStore Helios and Helios BYOC have been assessed against PCI DSS v4.0.1, the current version of the payment card security standard, by an independent Qualified Security Assessor.


SingleStore Helios (Enterprise Edition) and SingleStore Helios BYOC have achieved PCI DSS v4.0.1 compliance. The assessment was carried out by ControlCase, an independent Qualified Security Assessor (QSA), and reviewed the security controls in place across the in-scope Helios cloud platform to validate its PCI compliance.


PCI DSS is the security standard for environments that handle payment card data. PCI compliance means an independent assessor has evaluated SingleStore Helios against the standard's requirements. If you run payment or other cardholder data workloads on Helios, the managed platform underneath your application already sits inside a validated control set. It does not make your application compliant on its own, but it takes the platform layer off your list of things to assess and evidence from scratch.


> *“Security is more than a compliance requirement for us; it is a commitment to our customers. Reaching PCI DSS v4.0.1 reflects the work of our engineering, cloud infrastructure, site reliability, security, and compliance teams. As customers build more of their critical applications on SingleStore Helios, we will keep investing in the controls and processes that let them trust the platform with sensitive workloads.”*


> **Nishanth Singarapu**
>
>
> , Associate Director, Governance, Risk & Compliance, SingleStore


# Why PCI DSS matters


The Payment Card Industry Data Security Standard is maintained by the


[PCI Security Standards Council](https://www.pcisecuritystandards.org/) (PCI SSC), the body the major card brands set up to define one common baseline for protecting cardholder data. It sets requirements across twelve areas, including access control, encryption, network security, logging and monitoring, and vulnerability management.


v4.0.1 is the current version of the standard, and since March 2025 its full set of requirements has been mandatory. Reaching compliance now means meeting all of them, not the smaller set that applied while the standard was phasing in.


# Scope of the assessment


The assessment covers two products:


-


SingleStore Helios (Enterprise Edition)


-


SingleStore Helios BYOC (Bring Your Own Cloud)


As part of the PCI compliance assessment, ControlCase validated the controls across SingleStore's fully managed cloud database platform. That includes the Enterprise Database-as-a-Service (DBaaS) offering, multi-tenant cloud environments, cloud-native infrastructure, and the storage and security services that support them. The BYOC model, where Helios runs inside your own cloud account, falls inside the same assessed control set.


# Security controls across the Helios platform


PCI DSS maps onto controls Helios already applies as its default posture. The assessment reviewed those controls across several areas.


### Identity and access management


Access to production systems requires Multi-Factor Authentication (MFA) and runs through centralized identity management. Administrative access goes over secure VPN connectivity and through bastion hosts rather than direct connections to production. On the customer side, Helios supports single sign-on (SSO) through SAML and OIDC identity providers and SCIM provisioning, with role-based access control (RBAC) at both the portal and the database level. The


[identity integration](https://www.singlestore.com/blog/enterprise-database-sso-scim-identity-integration) and


[zero trust](https://www.singlestore.com/blog/zero-trust-isnt-a-checkbox/) posts go into how that access model is built.


### Encryption and secure communications


Connections to Helios use encrypted HTTPS/TLS, enforced at TLS 1.2 or above for client connections, internal traffic between nodes, and transfers to and from object storage, so there are no plaintext paths. Data at rest is encrypted with AES-256 using cloud-managed Key Management Service (KMS) keys. Customers with key-sovereignty requirements can supply and control their own keys through


[Customer-Managed Encryption Keys](https://www.singlestore.com/blog/customer-managed-encryption-keys-cloud-database-cmek) .


### Continuous monitoring


Logs from infrastructure and applications are centralized so the security team can detect, investigate, and respond to suspicious activity. Audit logging covers both control-plane and database-level operations, which is what produces the evidence trail an assessment like this depends on.


### Vulnerability management


The platform is scanned for vulnerabilities on a regular schedule, with defined remediation and change-management processes to fix and track what the scans surface.


### Secure cloud infrastructure


Helios runs on Amazon Web Services (AWS). Each deployment sits in a discrete AWS account within the cloud region and uses dedicated Virtual Private Clouds (VPCs) and network segmentation to isolate compute, with a dedicated object storage bucket per cluster and cloud-native security controls applied at the infrastructure layer. Cluster endpoints are not exposed to the public internet by default; access requires explicit IP allowlisting. The


[security documentation](https://docs.singlestore.com/cloud/security/) sets out the platform's baseline in full.


# Where PCI DSS fits with SingleStore's other certifications


PCI DSS is not the first set of security standards against which the platform has been independently assessed. Helios already holds SOC 2 Type II and ISO/IEC 27001, and supports customers with HIPAA, GDPR, and CCPA obligations. PCI DSS v4.0.1 adds payment card data to that set. For teams evaluating the platform, the practical effect is that more of your own compliance scope can inherit from an already-assessed foundation, whichever framework you report against. The


[compliance certifications](https://www.singlestore.com/blog/cloud-database-compliance-certifications) post covers how that inheritance works, and the


[security page](https://www.singlestore.com/security/) lists the current set.


# Shared responsibility: what this covers, and what stays with you


SingleStore's compliance covers the managed platform: the infrastructure, operational controls, and security capabilities Helios provides. It does not extend to the applications and data you deploy on top of it.


Under the shared responsibility model, SingleStore secures the platform and you secure how you use it. In concrete terms, SingleStore owns the encryption defaults, network isolation, patching, and the operational controls behind the assessment. You own your IP allowlist, your identity provider integration and group-to-role mappings, your database RBAC design, your own keys if you use CMEK, and how your application handles any cardholder data it touches. The platform gives you a compliant foundation. It does not make your application compliant on its own. The


[shared responsibility](https://www.singlestore.com/blog/cloud-database-shared-responsibility-model/) post and the


[shared responsibility documentation](https://docs.singlestore.com/cloud/getting-started-with-singlestore-helios/about-singlestore-helios/shared-responsibility/) set out exactly where the line sits.


# Compliance as an operating commitment


PCI DSS v4.0.1 compliance is not a one-time certification, and v4.0 was written specifically to discourage treating it that way. Maintaining PCI DSS compliance takes ongoing work: regular assessments, vulnerability management, change control, and coordination across the engineering, infrastructure, site reliability, security, compliance, and operations teams that run the platform. For customers, PCI DSS v4.0.1 is one more independently verified control in the platform they build on, and one SingleStore intends to keep.


# Further reading


This announcement sits alongside the


*Enterprise Security with SingleStore Helios*


series, which covers each layer of the platform's security in more depth:


-


[The compliance question enterprises ask first](https://www.singlestore.com/blog/cloud-database-compliance-certifications)


-


[Enterprise identity, your way (SSO, SCIM)](https://www.singlestore.com/blog/enterprise-database-sso-scim-identity-integration)


-


[Zero trust isn't a checkbox](https://www.singlestore.com/blog/zero-trust-isnt-a-checkbox/)


-


[What happens when something goes wrong (disaster recovery)](https://www.singlestore.com/blog/cloud-database-disaster-recovery-rpo-rto)


-


[The encryption control spectrum (CMEK)](https://www.singlestore.com/blog/customer-managed-encryption-keys-cloud-database-cmek)


-


[Why shared responsibility isn't a risk transfer](https://www.singlestore.com/blog/cloud-database-shared-responsibility-model/)


-


[Security engineering, not just security features](https://www.singlestore.com/blog/cloud-database-security-engineering-sdlc)


For the full architecture, see the


[SingleStore Helios Cloud Security whitepaper](https://www.singlestore.com/resources/whitepaper-singlestoredb-cloud-security) .


## On this page


- Why PCI DSS matters
- Scope of the assessment
- Security controls across the Helios platform


- Identity and access management
- Encryption and secure communications
- Continuous monitoring
- Vulnerability management
- Secure cloud infrastructure


- Where PCI DSS fits with SingleStore's other certifications
- Shared responsibility: what this covers, and what stays with you
- Compliance as an operating commitment
- Further reading


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fcloud-database-pci-dss-compliance%2F)


[Product](https://www.singlestore.com/blog/category/product/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Enterprise Identity, Your Way Product](https://www.singlestore.com/blog/enterprise-database-sso-scim-identity-integration/)


[Blog Data Lakehouses Are Being Redefined by Real-Time Expectations Product](https://www.singlestore.com/blog/data-lakehouses-are-being-redefined-by-real-time-expectations/)


[Blog Build and Deploy an App Prototype with an AI Agent using MCP in an Afternoon Product](https://www.singlestore.com/blog/build-and-deploy-an-app-prototype-with-mcp-and-ai-agent/)


[Blog Scaling Time-Series Data for AI Models Product](https://www.singlestore.com/blog/scaling-time-series-data-for-ai-models/)


[Blog Why Agentic AI Relies on Your Database Product](https://www.singlestore.com/blog/why-agentic-ai-relies-on-your-database/)


[Blog The Lakebase Vision Is Right. Who Will Build It First? Product](https://www.singlestore.com/blog/lakebase-vision-is-right/)
