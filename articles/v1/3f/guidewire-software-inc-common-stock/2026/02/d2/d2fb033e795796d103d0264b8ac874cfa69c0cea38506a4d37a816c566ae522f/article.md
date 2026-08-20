---
schema_version: "1.0.0"
document_id: "d2fb033e795796d103d0264b8ac874cfa69c0cea38506a4d37a816c566ae522f"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/why-a-security-first-dev-lifecycle-is-the-only-option-in-guidewire-cloud"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:59b19347231985744dcdd0fbaead3af1b84716b691120aad7a6f0cadccaa64cd"
---

# Why a Security-First Dev Lifecycle is the Only Option in Guidewire Cloud

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Why a Security-First Dev Lifecycle is the Only Option in Guidewire Cloud


When security is treated like a final audit, a checkbox at the end of a long, exhausting sprint, everyone pays the price. The code may be elegant and the logic sound, but success in the cloud depends on a clear understanding of where Guidewire’s work ends and yours begins. Think of your security posture not as a gatekeeper, but as the high-performance resume for your entire InsuranceSuite deployment.


In Guidewire Cloud, security is part of the build process from the first line of code through deployment.


*Join our[developer newsletter](https://www.guidewire.com/developers/developer-resources/developer-newsletter) to receive more information on code security and other articles like this delivered directly to your inbox.*


## What Shared Responsibility Means for Developers


Your role in the cloud hinges on the Shared Responsibility Model. While Guidewire secures the underlying SaaS infrastructure, you’re the architect of the application-level defense.


- **Guidewire's Responsibility** : Guidewire manages the security of the cloud infrastructure (IaaS/PaaS), the underlying runtime environment, and core network infrastructure, like VPCs and gateways
- **Your Responsibility** : You own application-level security, which includes managing user identity and role-based access control (RBAC), enforcing multi-factor authentication (MFA), and securing custom integrations and code


The system's composable, API-first architecture gives development teams the freedom to build integrations rapidly, while establishing a foundation for continuous innovation.


## Why Sample Roles Should Never Make It to Production


One of the most common sources of privilege escalation risk stems from a seemingly harmless shortcut: using out-of-the-box (OOTB) sample roles in a live environment. These roles are built for demonstration and often include combinations of permissions that don't align with the principle of least privilege.


- **Audit and Define** : During initial development, audit the OOTB roles to understand available permissions and define your own custom roles based on specific job functions
- **Build with Least Privilege** : Create new custom roles from scratch, granting only the minimum permissions necessary for each role
- **Don’t Clone** : Avoid cloning OOTB roles so you don’t carry over excessive permissions
- **Deactivate and Remove** : Before go-live, ensure all OOTB sample roles are unassigned and then deactivated or deleted


## Securing the Full Development Lifecycle


Security carries through the entire development cycle. As developers, you own it from the first line of Gosu to the final configuration.


- **Treat SCM as a Source of Truth** : Your Guidewire-provided BitBucket repository is the single source of truth. Every change should be versioned and tracked through a formal pull request and review process
- **Manage Third-Party Risks** : You’re solely responsible for patching all customer-introduced open-source software (OSS). Continuously monitor for vulnerabilities (CVEs) and use Software Composition Analysis (SCA) to remediate dependencies before deployment.
- **Minimize the Attack Surface** : Unused features and internal tools, like the Profiler or Debug panels, must be disabled in all production environments


## Data Integrity and the Masking Mandate


How sensitive information is handled in lower-level environments (Dev, Test) is a critical pillar of your security approach. You can’t protect what you haven’t identified, which makes data classification your foundation.


- **Classification Inventories** : Create and maintain a data classification inventory for all personally identifiable information (PII) and financial data before go-live.
- **Proactive Masking** : Don’t use production PII in non-production environments unless it’s properly masked. Use data masking to prevent PII from being accessible during development, testing, or support activities.
- **Secure Secrets** : Manage all API keys and credentials through designated secrets management services. Never store secrets in plaintext within configuration files or source code.


## Security as Your Competitive Advantage


The strength of your InsuranceSuite deployment depends on how security is handled from the start. When security is built into the development process, deployments are more stable, compliance is easier to manage, and risk is reduced. This is how you build confidence into every release.


**Ready to harden your implementation?** Visit the[Guidewire Security Hub](https://docs.guidewire.com/security) for hardening guidance, detailed control objectives, and additional technical examples. Use it as your definitive starting point to secure your implementations and master your side of the shared responsibility model.


[Subscribe to the Developer Newsletter](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
