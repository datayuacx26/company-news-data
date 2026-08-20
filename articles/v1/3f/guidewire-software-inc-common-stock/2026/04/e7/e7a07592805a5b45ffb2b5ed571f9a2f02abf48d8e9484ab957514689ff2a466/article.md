---
schema_version: "1.0.0"
document_id: "e7a07592805a5b45ffb2b5ed571f9a2f02abf48d8e9484ab957514689ff2a466"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/why-data-exposure-happens-before-go-live-and-how-to-prevent-it"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:715e4b5b10929c61ca0a0c87c2faa4ef01214edda675d027d1fd0d362a9fe14b"
---

# Why Data Exposure Happens Before Go Live and How to Prevent It

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


- Why Data Exposure Happens Before Go Live (and How to Prevent It)


This post is the third installment in our security series. We started by discussing[why a security-first lifecycle is non-negotiable](https://www.guidewire.com/resources/blog/developers/why-a-security-first-dev-lifecycle-is-the-only-option-in-guidewire-cloud) and then moved into[the three intentional IAM decisions](https://www.guidewire.com/resources/blog/developers/three-intentional-iam-decisions-that-strengthen-insurancesuite) that lock down access. Today, we’re looking at another piece of the puzzle: protecting the data itself before it ever reaches production.


A developer needs realistic data to debug a claims issue, so they copy a production dataset into a test environment. It’s a routine step. Most teams don't think twice about it. But that dataset contains PII and financial records, and test environments typically have broader access permissions than production. Suddenly, more people than you ever intended can see real policyholder data.


***Stay ahead of the curve** : If you’re building on the Guidewire platform,[subscribe to our Developer Newsletter](https://www.guidewire.com/developers/developer-resources/developer-newsletter) for the latest security patterns, API updates, and technical deep dives delivered straight to your inbox.*


This exposure typically stems from practical decisions made to keep work moving. When the easy path leads to sensitive data exposure, the default workflow requires adjustment.


In our previous deep dive on IAM, we looked at who should have access to your systems. But even with perfect identity management, the actual policyholder data is at risk if it’s handled incorrectly during development. In InsuranceSuite, security is a shared responsibility. Guidewire secures the underlying cloud platform—infrastructure, runtime, and encryption. Your team manages what happens next: how data is classified, masked, and accessed within your specific implementation.


Here’s how to get your data governance right before go-live so you can prevent these exposures from the start.


## Why You Shouldn’t Skip Classification


All security decisions about data begin with knowing what data you have. You can't enforce masking policies on fields you haven't identified as sensitive. You can't audit access to data you haven't classified.


Before you go live, your team needs a documented data classification inventory. This must cover every field in your InsuranceSuite implementation that contains PII or financial data. Treat this inventory as a living technical artifact, rather than letting it reside solely as institutional knowledge.


## The Masking Gap Teams Can Overlook


Production data in test and development environments is one of the most predictable compliance exposures in InsuranceSuite implementations. Dev and test environments routinely have broader access controls than production. Putting real policyholder data there means accepting that exposure as part of your governance posture.


Under[GDPR Article 5(1)(b)](https://gdpr-info.eu/art-5-gdpr/) , using real PII for debugging is difficult to justify when masked or synthetic data is available. The standard control is to mask the data before it ever hits a non-production environment.


Guidewire Cloud provides the Snapshot Export service for this specific purpose. Use Snapshot Export to pull your production data, apply your external masking tools to sanitize the PII, and then reimport that clean dataset. By incorporating Snapshot Export into your standard environment refresh pipeline—a key component of the[security-first dev lifecycle discussed in part one](https://www.guidewire.com/resources/blog/developers/why-a-security-first-dev-lifecycle-is-the-only-option-in-guidewire-cloud) —your team gets the realistic data shapes they need for testing while keeping policyholder records secure.


Direct copy vs. Snapshot Export


## Logs Must Not Capture PII


Securing your logs is a coding decision. Every application and workflow service must be built so that logs never capture PII.


These breakdowns often occur when a developer logs a full object for debugging and the serialization includes sensitive fields by default.


### The Pattern to Follow:


Avoid logging entire objects or request payloads, such as a claims object or a policyholder bean. These often contain PII that isn't immediately obvious. Instead, log the specific event and use correlation IDs for operational context.


**Wrong pattern: ✗ - logging the full object**


*// Logging the full object — toString() serializes all fields*


**var**


policy


=


PolicyPeriod


.


getNamedInsured


()


logger


.


info


(


"Processing update for: "


+


policy


.


toString


())


*// policy.toString() may include name, address, contact details*


*// None of this belongs in a log*


**Right pattern: ✓ - log the event, not the data**


*// Log operational context only — no PII*


logger


.


info


(


"Policy update completed"


, {


"policyId"


:


policy


.


getPublicID


(),


*// internal ID — not PII*


"eventType"


:


"POLICY_UPDATED"


,


"correlationId"


:


correlationId


*// trace ID for debugging*


})


*// This pattern demonstrates allowlist logging.*


*// Adapt to your Guidewire implementation.*


## Secrets Management in InsuranceSuite


API keys, OAuth secrets, and database strings belong in the designated Guidewire Cloud secrets management service. Storing these in source code, configuration files, or environment variables creates unnecessary risk.


A credential hardcoded during development can easily migrate through the pipeline into production.


**Wrong pattern: ✗ - hardcoded in configuration**


*// Hardcoded in config — now in source control,*


*// build logs, and every environment this deploys to*


integrations


:


partnerApi


:


endpoint


:


"https://api.partner.com/v2"


apiKey


:


"pk_live_abc123xyz"


*// ← never do this*


**Right pattern: ✓ - runtime retrieval**


*// Retrieved from Guidewire Cloud secrets management at runtime*


*// Never appears in source control or build logs*


*// Rotated on schedule without a code change*


integrations


:


partnerApi


:


endpoint


:


"https://api.partner.com/v2"


apiKey


:


"${secrets:partner-api-key}"


*// This pattern demonstrates runtime secret retrieval.*


*// Adapt to your Guidewire implementation.*


For a deeper dive into these requirements, including the full prohibition on source-code credential storage, check out the[Hardening Guidance for InsuranceSuite: Data Security and Governance](https://docs.guidewire.com/security/hardening-guidance-for-insurancesuite/data-security-and-governance) .


### Next Steps


To learn more about how we handle cloud security,[visit Guidewire Security on Guidewire Documentation](https://docs.guidewire.com/security/) (login required).


🗓️ **Join the Deep Dive Webinar -[Register Here](https://success.guidewire.com/2026-04-03---Developer-Webinar---Security_LP-Registration.html)**


We’ve covered the philosophy, the access, and the data patterns. Now, let’s look at the code. Join us for this developer-focused session where we will walk through the concrete steps for locking down your systems based on all three parts of this series:


**Security Secrets: Hardening InsuranceSuite — Show Me the Code**


**When** : Thursday, April 30, 2026 | 8:30 a.m. PST


**Speaker** : Mark Sayewich, Director, Security Evangelism


[Register Here](https://success.guidewire.com/2026-04-03---Developer-Webinar---Security_LP-Registration.html)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
