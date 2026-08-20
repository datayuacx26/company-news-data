---
schema_version: "1.0.0"
document_id: "d9fc77659715e5e06cff2d26e68f37ac7bedc3ce5131b335af890a96d59d7c6f"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/identity-security/cmmc-level-2-commercial-iam-audit-risk/"
published_at: "2026-07-30T07:00:00+00:00"
first_seen_at: "2026-07-31T19:31:06.584573+00:00"
fetched_at: "2026-07-31T19:31:07.432136+00:00"
content_hash: "sha256:046cb1c5670e1dfbd718f7d96a1ae952b230e41015890c8ab57adc2dca73c955"
---

# CMMC Level 2: Do you really need FedRAMP for your IAM?

### Topics


---


FedRAMP


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


**TL;DR:** Relying on commercial identity and access management (IAM) platforms for CMMC Level 2 compliance introduces significant audit risks. Although some auditors classify non-FedRAMP identity tools as security protection assets (SPAs), strict Certified Third-party Assessment Organization (C3PAO) often treat identity providers as external service providers (ESPs). To ensure audit success, those building their CMMC Level 2 architecture should deploy FedRAMP Moderate or High authorized identity environments that provide official Bodies of Evidence (BoE) and FIPS 140-2 validated cryptography.


I’ve been working with customers for a while now, and I’ve noticed a trend: some consultants believe that Okta (within the context of CMMC L2) doesn’t need to hit FedRAMP Moderate standards. In my (not so) humble opinion—that’s concerning. Your IAM system represents the “keys to the kingdom”—it's not something you want to leave unprotected.


While those consultants are technically correct on a strict reading of the regulation, relying on that limiting assertion introduces significant risk when it comes to the audit.


The debate centers on a very specific nuance in the[CMMC Scoping Guide](https://dodcio.defense.gov/cmmc/Resources-Documentation/) regarding how different cloud applications are categorized.


## The consultant’s argument: Security protection assets vs. cloud service providers


Auditors who make this argument are zeroing in on a specific technicality: the difference between a cloud service provider (CSP) that stores your actual data and a security protection asset (SPA) that just manages access.


- **The DFARS 7012 trigger:** As written, the Defense Federal Acquisition Supplement (DFARS) 252.204-7012(b)(2)(ii)(D) only mandates FedRAMP Moderate for external cloud providers used to **"store, process, or transmit"** Controlled Unclassified Information (CUI).
- **Okta’s role:** An IAM platform like Okta processes usernames, passwords, and multi-factor authentication (MFA) tokens—it does not store or transmit your actual CUI (like military schematics or contract details).
- **The Scoping Guide ruling:** In the official CMMC Level 2 Scoping Guide, tools that provide security capabilities but don't hold CUI are categorized as SPAs. The guide explicitly states that SPAs must be assessed against relevant NIST SP 800-171 controls, but it does not explicitly stamp them with the strict DFARS 7012 FedRAMP Moderate requirement.


Because of this regulatory nuance, some consultants argue that as long as you can prove Okta is configured securely and satisfies the Identification & Authentication (IA) controls of NIST SP 800-171, the underlying Okta cloud doesn't need a FedRAMP stamp of approval.


## Why relying on this loophole is risky


It might hold up on paper, but betting your CMMC assessment on a non-FedRAMP-certified IAM is a high-stakes gamble. Here is why:


### The "keys to the kingdom" reality


Okta governs access to the systems that do hold CUI (such as your GovCloud environment or local servers). If an attacker breaches your IAM layer, they gain direct access to your CUI. Many strict C3PAOs view any cloud service capable of granting access to CUI as an external service provider (ESP), inherently pulling it back into the FedRAMP Moderate bucket.


### The cryptographic requirements (FIPS 140-2)


To pass CMMC Level 2, any cryptography used to protect data—including the transmission and protection of authentication credentials—must be encrypted to the FIPS 140-2 validation standard. Most non-FedRAMP-certified IAMs in the market do not utilize FIPS-validated cryptographic modules by default. To get guaranteed FIPS compliance, you generally have to move to a federally aligned platform.


### Okta already built a solution for this


Okta itself recognizes this hurdle. We built[Okta for US Military](https://www.okta.com/industries/public-sector/department-of-defense/) and[Okta Government Cloud](https://www.okta.com/industries/public-sector/federal-government/) , which hold FedRAMP Moderate,[FedRAMP High](https://www.okta.com/blog/company-and-culture/okta-achieves-fedrampr-high-authorization/) , and[DoD Impact Level 5 (IL5)](https://www.okta.com/newsroom/articles/okta-achieves-department-of-war-impact-level-5-provisional-authorization/) authorizations. Because a fully compliant federal version exists, a defense assessor will question why a contractor protected DoD-related access paths using the commercial tier. Furthermore, our FedRAMP High and IL5 offerings meet the International Traffic in Arms (ITAR) requirements for “US persons.”


Okta strictly boundaries its compliance artifacts based on our product tiers:


- **Okta Commercial cloud:** We do not maintain a FedRAMP-aligned Body of Evidence (BoE) or 3PAO assessment for our commercial cloud. So, if your auditor requests backend security attestations to prove NIST SP 800-171/172 equivalence, the Okta Commercial cloud will not be able to provide them.
- **Okta for Government (Moderate/High) and Okta for US Military:** These are isolated, dedicated environments. We actively maintain and provide the official FedRAMP packages and agency Authorizations to Operate (ATOs) for these tiers. Federal and defense entities can request these packages directly through tools such as connect.gov or eMASS to hand over to their auditors.


## The bottom line


If you’re currently building out your CMMC Level 2 architecture, my advice is simple: **don't cut corners on your IAM provider** .


While you might find a permissive auditor willing to accept the Okta Commercial cloud as an SPA without a FedRAMP certificate, the safer, future-proof play is to utilize Okta’s FedRAMP Moderate/High-authorized identity solution. If you use anything that isn’t FedRAMP certified, you’re going to have to thoroughly document its isolation, create a bulletproof System Security Plan (SSP) narrative, and ensure your crypto implementations utilize FIPS-validated cryptography.


## Choose a secure path to federal compliance


Securing access paths for critical federal and military operations shouldn’t be a compromise. Implementing our dedicated, compliant identity solutions ensures your organization meets the most stringent federal requirements and is protected against sophisticated identity threats. We deliver the exact level of assurance your auditors expect, without sacrificing the user-friendly experience of modern IAM systems.


[Download the Okta for Government High datasheet](https://www.okta.com/resources/datasheets/okta-for-government-high/) to learn how to adhere to the highest FedRAMP standards and rapidly deploy modern MFA.


[Download the Okta for US Military datasheet](https://www.okta.com/resources/datasheets/okta-for-us-military/) to discover how to centralize secure access to CUI and protect against tactical edge deployments.


About the Author


[Naveed Mirza Senior Solutions Architect Naveed is a Senior Solutions Architect focusing on the DoD and Federal customer base. He has worked in cybersecurity since leaving the US Navy in the late 1990s. Before coming to Okta, Naveed was a consultant for several DoD customers, and he continues to offer advice via active participation in the DoD community. He grew up in Stafford, Virginia, and upon returning from active duty, took up residence there once more. In his free time, he enjoys beer brewing, gaming, and the occasional date night with his wife.](https://www.okta.com/blog/author/naveed-mirza/)
