---
schema_version: "1.0.0"
document_id: "68fe56087c4b6718bd4de609e279563c672f8840408c17e5a86b2d0168c7a5ae"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/secrets-management-requirements-pci-dss"
published_at: "2025-07-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:c85222f70f9fd10a697f48bcbd2eff93705264b6a6acf9f3270017db59623593"
---

# Secrets Management Requirements for PCI DSS 4.0 Compliance

Today, we’ll do a deep dive on the PCI DSS V4.0 requirements that specifically involve secrets management. While PCI DSS V4.0 has broad security guidelines, it does have very pointed secrets management tenets. At a high level, this means securely handling and storing sensitive data like credentials, encryption keys, and API tokens to stay compliant with the standard.


## So What Exactly is PCI DSS V4.0?


[PCI DSS](https://www.pcisecuritystandards.org/about_us/) stands for Personal Card Identification Data Security Standards. It was introduced by the major credit card companies (VISA, Mastercard, etc.) in 2004 to help businesses secure cardholder data and prevent fraud. It’s a constantly evolving standard, and V4.0 is the latest iteration, having emerged in early 2022.


One big shift in[PCI DSS V4.0](https://www.pcisecuritystandards.org/document_library/) is that it pushes for continuous security practices instead of static “once-a-year” audits. This makes perfect sense, given that modern data breaches are often caused by secret sprawl: things like leaked passwords, exposed API keys, or cryptographic keys that were never rotated.


That’s why this guide breaks down the PCI requirements tied to[secrets management](https://infisical.com/blog/secrets-management-complete-guide) and points you towards trusted tools to help you meet those standards.


## How Does PCI DSS Scope Shape Compliance Work?


Before we start, it’s important to clarify how PCI DSS defines scope and why that matters.


PCI DSS V4.0 is a fairly long protocol, but some companies are able to sidestep a majority of the requirements. How? PCI DSS V4.0 has tiered[PCI SAQs](https://www.pcisecuritystandards.org/faq/articles/Frequently_Asked_Question/what-is-a-pci-dss-self-assessment-questionnaire/) (Self-Assessment Questionnaires), where the appropriate SAQ is determined by how much of the payment process is handled by the organization.


By trusting external vendors that are PCI compliant, like[Stripe](https://stripe.com/) ,[Shopify](https://www.shopify.com/) , or[Evervault](https://evervault.com/) , organizations can limit their compliance requirements and follow a simpler SAQ.


<aside>


**More about SAQs:**


PCI SAQs (Self-Assessment Questionnaires) are a way for merchants and service providers to assess their PCI DSS compliance on their own, using tools from the Payment Card Industry Security Standards Council (PCI SSC).[Which SAQ applies to you depends on how your organization stores and handles payment card data.](https://www.pcisecuritystandards.org/faq/articles/Frequently_Asked_Question/why-are-there-multiple-pci-dss-self-assessment-questionnaires-saqs/)


</aside>


Here’s a breakdown of the most common **PCI SAQ types** :


SAQ Type For Whom? Cardholder Data Stored? Networked? Controls


[A](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-A.pdf) Fully outsourced (e.g., hosted payment page) No No Low


[A-EP](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-A-EP.pdf) E-commerce merchants with some web control No Yes High


[B](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-B.pdf) Dial-out terminals or imprinters No No Low


[B-IP](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-B-IP.pdf) IP-connected standalone terminals No Yes Moderate


[C-VT](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-C-VT.pdf) Single computer with virtual terminal No Yes Moderate


[C](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-C.pdf) POS systems on isolated networks No Yes High


[D (Merchant)](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-D-Merchant.pdf) Complex or custom environments Maybe Yes Full


[D (Service Provider)](https://listings.pcisecuritystandards.org/documents/PCI-DSS-v4-0-SAQ-D-Service-Provider.pdf) Third parties handling data Maybe Yes Full


Our goal here is to show how secrets management fits into the bigger picture of PCI DSS compliance. Third-party payment providers and lighter SAQs can reduce scope, but they don’t eliminate the need to protect secrets. Put simply, without a proper secrets management system, meeting these requirements is not possible.


Next, we’ll walk through the PCI DSS V4.0 requirements and show how strong secrets hygiene can help you stay compliant.


## What are the Secrets Requirements for PCI DSS V4.0?


### PCI DSS Requirement 3: Protect Stored Cardholder Data (Encryption and Key Management)


**What PCI requires**


The general guidance is clear: don’t store cardholder data unless it's absolutely necessary. This means removing sensitive data from magnetic stripes or chips immediately after authentication, and checking data retention periods for exceeding limits at least every[3 months](https://www.pcisecuritystandards.org/faq/articles/Frequently_Asked_Question/what-is-the-maximum-period-of-time-that-cardholder-data-can-be-stored/) .


Any data that must be stored (e.g., PANs) should be strongly encrypted, and the keys must be rotated regularly. Encryption keys themselves are sensitive secrets. Mishandling them can expose entire cardholder databases, even if the data itself is encrypted.


Limit the risk by following dual control measures, where no single party holds total control over encryption keys. Keeping secrets separate adds an extra layer of defense (and also makes them easier to manage).


**Recommended Tools**


[Vormetric’s Data Security Platform](https://cpl.thalesgroup.com/encryption/vormetric-data-security-platform) handles encryption-at-rest and PAN tokenization, helping you meet strict cryptographic requirements. This can be paired with a secrets management platform (like[Infisical](https://infisical.com/) ) to store and rotate those keys dynamically across your infrastructure.


### PCI DSS Requirement 4: Encrypt Transmission of Cardholder Data Across Open Networks (TLS/Certificate Management)


**What PCI requires**


Encryption is just as important for keeping sensitive data safe as it moves across open networks. In practice, you should use strong protocols like TLS 1.2 or higher, and unprotected PANs must never travel over unsecured channels like plain HTTP, email, or chat.


API keys and session tokens used in calls should be handled dynamically. To reduce exposure, secrets should be injected just in time when data is in transit and stored securely in a separate vault when at rest.


Also, certificates used for securing communications should be updated regularly. Good monitoring tools will flag certificates that are near their expiration date or set up incorrectly.


**Recommended Tools**


Use[Evervault](https://evervault.com/) to protect sensitive payment data during transmission over open networks alongside a[secrets management platform](https://infisical.com/blog/best-secret-management-tools) to store and inject TLS private keys, API keys, and other credentials when they’re needed.


### PCI DSS Requirement 7: Restrict Access to Cardholder Data by Business Need-to-Know (Role-Based Access Control for Secrets)


**What PCI requires**


Poor access controls are an opening for attackers to exploit. That’s why access should be limited to only those people and systems that truly need it ([the principle of least privilege](https://infisical.com/blog/vibe-coding-security-playbook) ). In practice, this means restricting secrets so they’re accessible only for specific, well-defined tasks.


[RBAC (Role-Based Access Control)](https://infisical.com/docs/documentation/platform/access-controls/role-based-access-controls) ties secrets to specific roles. In CI/CD pipelines or cloud environments, secrets should only be accessible at runtime and immediately removed after. The goal is to reduce how long sensitive credentials are exposed.


**Recommended Tools**


Strong secrets management platforms build access controls into their design. For example,[Infisical’s RBAC](https://infisical.com/docs/internals/permissions/overview) lets you set predefined or custom roles that link permissions to specific users and identities.


### PCI DSS Requirement **8: Identify and Authenticate Access to System Components (MFA and Secure Credential Storage)**


**What PCI requires**


Giving each individual their own unique identification guarantees that any actions taken can be traced back to the user. This applies not only to people but also to service accounts and automated systems accessing payment data.


Beyond unique IDs, multifactor authentication (MFA) adds extra security to stop unauthorized access, even if a credential is stolen. MFA secrets, like OTP seeds, should never be stored in config files. Instead, a secrets vault can inject them in real time when needed.


**Recommended Tools**


[Cisco Duo](https://duo.com/) is a reliable way to implement MFA, but it needs a strong secrets management system alongside it to handle storing, injecting, and rotating MFA seeds or SSH keys.


### PCI DSS Requirement **10: Track and Monitor All Access to Network Resources and Cardholder Data (Secrets Access Monitoring)**


**What PCI requires**


You need solid logging to catch suspicious behavior and trace what users do. Without it, pinpointing the source of an issue becomes far more difficult. Because secrets are prime targets, any unusual access (e.g., unexpected vault admin activity) should be captured.


Monitoring should stay proactive. Unscheduled access is a clear red flag and should trigger an immediate alert with key details: when it happened, who was involved, and what actions were taken (e.g., read, write, delete). Secrets need monitoring, too. Unusual activity can be an early sign of a compromised credential.


**Recommended Tools**


Set up[Splunk Enterprise Security](https://www.splunk.com/en_us/products/enterprise-security.html) to centralize logs and catch unauthorized access or failures. If you need a more affordable option,[Axiom](https://axiom.co/) integrates well and keeps costs down. Either way, make sure your secrets manager has built-in auditing to show who accessed which secrets and in what context.


## Conclusion


All these PCI DSS V4.0 requirements share one thing in common: they all tie back to effective secrets management. That’s why the industry is shifting towards solutions that embed PCI safeguards into every layer of the secrets lifecycle.


[Infisical](https://infisical.com/) was built from the ground up with these controls in mind.


**Advantages Include:**


- Built-in support for[dynamic secrets and automated rotation](https://infisical.com/blog/secret-rotation-vs-dynamic-secrets)
- Fully open-source codebase that slots into any DevOps workflow
- Flexible, multi-level secret hierarchies for apps, envs, and teams
- [Native SSH credential issuance](https://infisical.com/blog/ssh-certificates-guide) and end-to-end certificate management
- AES-256-GCM encryption with enforced TLS
- [Granular RBAC](https://infisical.com/docs/internals/permissions/overview) with time-bound, just-in-time access requests
- Turn-key audit trails that stream to[Splunk](https://www.splunk.com/) ,[Datadog](https://www.datadoghq.com/) , and other platforms


If you want to learn more, check us out[here](https://infisical.com/) .
