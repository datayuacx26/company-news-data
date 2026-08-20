---
schema_version: "1.0.0"
document_id: "9ca2578b767790fc27626e4302225087f308684a5308a6dc278b12710cb6ffe1"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/cloud-database-shared-responsibility-model/"
published_at: "2026-06-17T14:15:16+00:00"
first_seen_at: "2026-07-22T13:37:02.744795+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:6806a66ebe2cd05819bcf698b5dd2d681a5e192095e58b2c53a5be85086b0551"
---

# Cloud Database Shared Responsibility Explained | SingleStore

# Why Shared Responsibility Isn't a Risk Transfer


## The phrase appears in every vendor security document. Here is what it should actually mean - and how to tell when it does not.


Jun 17, 2026


•


6 min read


•


[Jay Bhatt](https://www.singlestore.com/blog/author/jay-bhatt/) ,


[David Sharnoff](https://www.singlestore.com/blog/author/david-sharnoff/) ,


[Nishanth Singarapu](https://www.singlestore.com/blog/author/nishanth-singarapu/)


"Shared responsibility" is one of the most useful concepts in cloud security. It is also one of the most frequently misused. In its correct form, shared responsibility describes a clear division: the vendor secures the platform, the customer secures how they use it. Both parties have defined, bounded obligations. Both can be held accountable.


In its corrupted form, shared responsibility becomes a liability clause. The vendor ships a product with minimal defaults, calls the gap between their configuration and a secure state "the customer's responsibility," and walks away from the consequences when that gap causes a breach.


The difference between these two versions is not in the contract language. It is in the product's default configuration. All three major cloud providers - AWS, Azure, and Google Cloud - publish their own shared responsibility models defining what the provider secures and what the customer secures. What matters when evaluating a database vendor is where they sit within that model: do their defaults start you from a secure baseline, or do they leave the baseline for you to build?


## **The Right Question Is About Defaults**


The most useful question to ask any cloud vendor is not "what is your shared responsibility model?" It is: "what does my environment look like on day one, before I have configured anything?"


The answer to that question tells you more about a vendor's security posture, cloud risk, and risk management maturity than any certification or white paper. If the defaults are weak - public access enabled, no encryption, permissive network rules, or insufficient protection for sensitive data - then the vendor has made the customer responsible for building the security baseline from scratch. Every misconfiguration between the vendor's defaults and a genuinely secure state is the customer's problem.


If the defaults are strong, the situation is reversed. The customer inherits a secure baseline and adds the additional controls that reflect their specific organizational requirements.


## **What SingleStore Helios Ships With By Default**


The following controls are active from the moment a workspace is created, with no customer configuration required. The full technical detail is available in the


[SingleStore security documentation](https://docs.singlestore.com/cloud/security/) :


**Encryption at rest**


AES-256 encryption using cloud-managed KMS keys. Active on all storage - block devices and object storage. No configuration required.


**Encryption in transit**


TLS 1.2 or above enforced for all connections - client connections, internal cluster traffic between leaf nodes, and data transfers to and from object storage. No plaintext paths exist.


**No public access**


Cluster endpoints are not exposed to the public internet by default. Access requires explicit IP allowlisting. Users who want open access must actively configure it.


**Network boundaries**


Each deployment resides in a discrete account in the cloud provider region. Compute resources are isolated per customer. Each cluster gets a dedicated object storage bucket.


**Secure credential store**


Portal authentication credentials and management secrets are encrypted and managed in a secure credential store. Portal sessions are short-lived tokens, not persistent credentials.


The security gap customers need to close is not "how do I add encryption" or "how do I restrict network access" - those are already done. The gap is "which additional controls does my organization's security posture require on top of this foundation."


## **What Remains the Customer's Responsibility - and Why That Is Reasonable**


Genuine shared responsibility means the vendor does not do everything. There are controls that only the customer can configure, because they depend on the customer's specific organizational requirements, risk appetite, and existing infrastructure.


**Customer responsibilities in SingleStore Helios**


-


**IP allowlist configuration:**


Defining which IP addresses and networks are permitted to connect to your clusters. SingleStore provides the mechanism and enforces it; you define the approved list.


-


**Identity provider integration:**


Connecting your SAML or OIDC identity provider, configuring SCIM provisioning, and defining group-to-role mappings. SingleStore supports all of these; the configuration reflects your organizational structure.


-


**Database RBAC design:**


Defining which users and service accounts have access to which databases, tables, and operations. The engine enforces whatever policy you define.


-


**Customer-Managed Encryption Keys:**


If your workload requires key sovereignty, you supply the key and manage it in your cloud KMS. This is inherently a customer responsibility - the whole point is that SingleStore does not hold the key.


-


**Application-layer security:**


How your applications query the database, how they handle returned data, and how they manage their own credentials. This is true of any platform.


The common thread in this list is that these are customizations - controls that require knowledge of your organization's policies, infrastructure, and requirements. They cannot be pre-configured because SingleStore does not have that knowledge. What SingleStore can do - and does - is make the secure choice the default at every point where there is a choice to make.


SingleStore documents where that line sits in its


[shared responsibility model documentation](https://docs.singlestore.com/cloud/getting-started-with-singlestore-helios/about-singlestore-helios/shared-responsibility/) , which sets out exactly which controls sit with SingleStore and which sit with you.


## **What to Put in Your Vendor Due Diligence Questionnaire**


When evaluating any cloud database vendor on shared responsibility, cloud risk, and risk management, these questions reveal whether the model is genuinely balanced or designed to transfer risk to the customer:


-


**What is encrypted by default, without any customer configuration?**


-


**Is public access enabled or disabled by default?**


-


**What network restrictions are in place at deployment time before the customer configures anything?**


-


**How is the boundary between vendor responsibility and customer responsibility documented and contractually defined?**


-


**What does a misconfigured deployment look like - what is the worst-case state a customer can accidentally end up in?**


-


**Are there guardrails that prevent customers from accidentally creating insecure configurations?**


A vendor with strong defaults and a genuine shared responsibility model will answer the first three questions clearly and specifically. Shared responsibility, done right, is not a way to distribute blame after a breach. It is a way to ensure that both parties apply their respective expertise to the problem - the vendor to platform security, the customer to organizational configuration.


**Download the Full SingleStore Helios Cloud Security Whitepaper**


The SingleStore Helios Cloud Security White Paper covers the complete security architecture in depth - including platform architecture, network security, identity and access management, cryptography, logging and monitoring, SDLC practices, and incident management.


[Download the whitepaper](https://www.singlestore.com/resources/whitepaper-singlestoredb-cloud-security)


**This Article Is Part of a Series**


*Enterprise Security with SingleStore Helios - 7 articles exploring every layer of cloud database security.*


**1**


[The Compliance Question Enterprises Always Ask First](https://www.singlestore.com/blog/cloud-database-compliance-certifications)


**2**


[Enterprise Identity, Your Way](https://www.singlestore.com/blog/enterprise-database-sso-scim-identity-integration)


**3**


[Zero Trust Isn't a Checkbox](https://www.singlestore.com/blog/zero-trust-isnt-a-checkbox/)


**4**


[What Happens When Something Goes Wrong](https://www.singlestore.com/blog/cloud-database-disaster-recovery-rpo-rto)


**5**


[The Encryption Control Spectrum](https://www.singlestore.com/blog/customer-managed-encryption-keys-cloud-database-cmek)


**6**


**Why Shared Responsibility Isn't a Risk Transfer \[You are here\]**


**7**


[Security Engineering, Not Just Security Features](https://blog/cloud-database-security-engineering-sdlc)


*Questions about security on SingleStore Helios? Contact*


security@SingleStore.com


## On this page


- The Right Question Is About Defaults
- What SingleStore Helios Ships With By Default
- What Remains the Customer's Responsibility - and Why That Is Reasonable
- What to Put in Your Vendor Due Diligence Questionnaire


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fcloud-database-shared-responsibility-model%2F)


Last modified - July 20th, 2026


[Product](https://www.singlestore.com/blog/category/product/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Iceberg Needs an Open Catalog, Not a Walled Garden Product](https://www.singlestore.com/blog/iceberg-needs-an-open-catalog-not-a-walled-garden/)


[Blog Rethinking RAG: How GraphRAG Improves Multi-Hop Reasoning! Product](https://www.singlestore.com/blog/rethinking-rag-how-graphrag-improves-multi-hop-reasoning-/)


[Blog Introducing Powerful GPU and Flexible CPU Compute Options for Data and AI Workloads Product](https://www.singlestore.com/blog/introducing-powerful-gpu-and-flexible-cpu-compute-options-for-data-and-ai-workloads/)


[Blog A Practical Guide to Database Security Product](https://www.singlestore.com/blog/a-practical-guide-to-database-security/)


[Blog Build AI Prototypes in Minutes Using Plain English Product](https://www.singlestore.com/blog/build-ai-prototypes-in-minutes-using-plain-english/)


[Blog Turning Your Business Intelligence Stack Into an AI Engine Product](https://www.singlestore.com/blog/turning-your-business-intelligence-stack-into-an-ai-engine/)
