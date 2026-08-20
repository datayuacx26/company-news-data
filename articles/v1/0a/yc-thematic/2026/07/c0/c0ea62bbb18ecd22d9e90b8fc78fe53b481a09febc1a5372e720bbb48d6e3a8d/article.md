---
schema_version: "1.0.0"
document_id: "c0ea62bbb18ecd22d9e90b8fc78fe53b481a09febc1a5372e720bbb48d6e3a8d"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/vendor-security-certifications"
published_at: null
first_seen_at: "2026-07-29T11:59:35.846279+00:00"
fetched_at: "2026-07-29T11:59:37.149226+00:00"
content_hash: "sha256:e55ca9ec8368069f88663f0dd2060066b9209415b441b5ac135af7058cff067e"
---

# What Security Certifications Should a Customer Feedback Analytics Vendor Have?

Feedback analytics is one of the few enterprise purchases where the vendor reads your customers' unedited words. Support tickets, survey verbatims, and chat transcripts arrive full of names, email addresses, and account numbers. Customers never expected a third party to see any of it. So the evaluation stalls in the same place every time: a security reviewer asks what certifications the vendor holds, and the sales team sends back a logo.


The certification to require is **SOC 2 Type II** , with the audit scope stated in writing, plus evidence of annual third-party penetration testing. Thematic holds SOC 2 Type II certification, audited every year by the independent firm A-LIGN. The scope covers the Security, Availability, and Confidentiality Trust Services Criteria. The reporting period closes 28 February each year. ISO/IEC 27001 is the reasonable second ask, especially for European buyers. ISO/IEC 42001 is becoming the third, now that feedback analytics is an AI purchase and not just a database purchase.


Below is what each certification actually proves, where the badges mislead, what Thematic holds and doesn't, and the questions that separate a real security posture from a logo on a web page.


## What "security certification" actually means here


A security certification is an independent auditor's opinion that a vendor's controls exist and work. It isn't a guarantee that your data is safe, and it isn't a technical standard the vendor passes or fails.


The distinctions that matter in an evaluation:


- **SOC 2** is an audit report against the[Trust Services Criteria](https://www.aicpa-cima.com/resources/download/2017-trust-services-criteria-with-revised-points-of-focus-2022) , a control framework developed by the American Institute of Certified Public Accountants (AICPA). The vendor scopes it, so two SOC 2 reports can cover very different ground.
- **Type I versus Type II** is the difference between a snapshot and a track record. Type I examines controls at a single point in time. Type II tests whether they operated effectively across a period.
- **[ISO/IEC 27001](https://www.iso.org/standard/27001)** is a certifiable international standard for an information security management system. It overlaps with SOC 2 by roughly 70 to 80 percent on controls. But it's a standard rather than an auditor's report, and European buyers often treat it as the default.
- **[ISO/IEC 42001](https://www.iso.org/standard/42001)** is the first international AI management system standard, published in December 2023. Certification is voluntary. As of 2026 it isn't a harmonized standard under the European Union Artificial Intelligence Act (EU AI Act), so holding it doesn't by itself grant a presumption of conformity.
- **General Data Protection Regulation (GDPR)** is a regulation, not a certification. Nobody is "GDPR certified." A vendor can only tell you what role it plays and what safeguards it provides.


The practical consequence: the badge tells you less than the scope behind it.


## The one detail that makes most SOC 2 claims worth checking


The Trust Services Criteria define five categories: security, availability, processing integrity, confidentiality, and privacy. **Only the security category is required in every SOC 2 audit.** The other four are optional, and the vendor decides which ones to include.


So "we're SOC 2 certified" is compatible with an audit that tested the security category alone, at a single point in time, and said nothing about whether the platform stays up or keeps your data confidential. Ask which categories were in scope, and whether the report is Type I or Type II. A vendor who can't answer in one sentence hasn't read its own report.


What you're given What it proves What it doesn't prove


SOC 2 Type I Controls were designed appropriately on one date That they worked for any length of time


SOC 2 Type II Controls operated effectively across a defined period Anything outside the categories the vendor scoped in


ISO/IEC 27001 A certified information security management system How the vendor's AI models handle your text


ISO/IEC 42001 A governed AI management system Conformity with the EU AI Act on its own


A GDPR statement The vendor's role and contractual commitments Nothing is certified; regulations have no badge


## What an enterprise security review typically requires


Beyond the certificate itself, a review at enterprise scale asks for a consistent set of artifacts.[Feedback analytics](https://getthematic.com/insights/customer-feedback-analysis-with-text-analytics) draws extra scrutiny because the payload is[unstructured personal data](https://getthematic.com/insights/generative-ai-security-concerns) .


**Independent penetration testing, on a stated cadence.** A certification report describes controls. A penetration test describes what happened when someone tried to break in. Ask how often, by whom, and against what methodology.


**Encryption standards, named.** "Encrypted in transit and at rest" isn't an answer. The answer names the cipher and the protocol version.


**Tenant isolation.** With feedback data, the question is whether another customer's comments and yours can ever share the same logical space.


**A documented processor relationship.** Under GDPR you're the controller and the vendor is the processor. That belongs in a signed contract, not on a web page.


**Retention and destruction terms.** How long the vendor keeps your data after the contract ends, and by what method it's destroyed.


**Personal data handling before analysis.** Whether personally identifiable information (PII) can be masked before the platform ever models it, and whether that's standard or an add-on.


**Access control that matches your identity stack.** Single sign-on (SSO), automated provisioning, and immediate revocation when someone leaves.


**Subprocessor transparency.** Who else touches the data, and how you find out when that list changes.


## Where feedback analytics vendors commonly fall short


The gaps cluster in predictable places:


- A Type I report presented as though it were Type II.
- A SOC 2 scoped to security alone, with confidentiality left out.
- Penetration testing described as "regular," with no cadence, tester, or methodology.
- PII redaction offered as a roadmap item rather than something you can watch working.
- No documented destruction method, only a promise to delete.
- An[AI governance answer](https://getthematic.com/insights/auditable-transparent-ai-feedback-analytics) that describes the model vendor's posture instead of the platform's own. The[risks specific to large language models](https://getthematic.com/insights/lethal-trifecta-llm-security) are the platform's to design around, not the model vendor's.


One question separates the postures at demo time: **ask which Trust Services Criteria are in scope on the current report, then ask to receive the report itself.** A vendor with a real posture hands it over under a non-disclosure agreement within days. A vendor without one offers a summary page.


## What Thematic holds, and what it doesn't


**SOC 2 Type II, independently audited every year.** Thematic holds SOC 2 Type II certification, audited annually by A-LIGN. The scope covers the Security, Availability, and Confidentiality Trust Services Criteria. The reporting period closes 28 February each year. Customers can request the report through their customer success manager or atinfo@getthematic.com .


**Annual external penetration testing against OWASP methodology.** Thematic completes annual third-party external penetration tests of the web application, assessing security, vulnerability, and intrusion exposure. Testing follows OWASP-based methodologies that go beyond the[OWASP Top 10](https://owasp.org/www-project-top-ten/) and the CWE/SANS Top 25, combined with in-house processes.


**Named encryption standards.** Thematic encrypts data at rest with AES (FIPS-validated) and data in transit with TLS 1.3, under a formal key-management policy. Application secrets are held in AWS Secrets Manager.


**Database-level tenant isolation.** Each Thematic customer is isolated at the database level, with additional cloud security controls layered on top.


**A GDPR processor model with data subject support.** Thematic operates as a processor: the customer decides what to upload, from whom, and why. Thematic signs a contract governing the processing of EU personal data. Customers can modify and delete individual datasets, which supports erasure rights.


**PII redaction before analysis, as a priced add-on.** Thematic can mask emails, web addresses, unique identifiers, encoded data, street addresses, names, and phone numbers before analysis, and strip quoted email history. It's a priced, best-effort add-on rather than a default. Better to know that during evaluation than after signature.


**Classification, retention, and destruction terms in writing.** Thematic classifies data in three tiers (Restricted, Private, Public) and treats all customer data as Restricted. Customer data is kept for the contract lifetime, then deleted within 30 days of termination using[NIST 800-88](https://csrc.nist.gov/pubs/sp/800/88/r2/final) sanitization, with backups purged on their own rotation. Personal data on a user account is kept for the account duration plus three months.


**Access control that fits an enterprise identity stack.** Thematic supports SAML 2.0 single sign-on and publishes an app in the Okta app store. It offers just-in-time provisioning on first login, and supports SCIM v2 for automated provisioning and deprovisioning.


Thematic's full posture is published on its[security and compliance](https://getthematic.com/product/security-and-compliance) page.


**What Thematic doesn't hold: ISO/IEC 27001.** Thematic isn't ISO 27001 certified. If your procurement process treats ISO 27001 as mandatory rather than preferred, raise it early. The overlap with SOC 2 Type II is substantial, and the Thematic report covers confidentiality and availability on top of security. But overlap isn't equivalence, and it shouldn't be sold as such.


## A buyer's checklist for vendor security review


These ten questions sit inside the broader set of[feedback analytics RFP requirements](https://getthematic.com/insights/customer-feedback-analytics-rfp-requirements) worth scoring a vendor against.


1. Is the report Type I or Type II, and what period does it cover?
2. Which Trust Services Criteria are in scope?
3. Who performed the audit, and when does the current period close?
4. Can you receive the full report under NDA, not a summary?
5. How often is penetration testing performed, by whom, and against what methodology?
6. Which cipher and protocol version protect data at rest and in transit?
7. Can personal data be masked before analysis, and is that standard or priced separately?
8. How is data destroyed at contract end, and to what standard?
9. Where is data hosted, and can that region be specified?
10. How are you notified when the subprocessor list changes?


## The short answer


Require SOC 2 Type II with the audit scope stated in writing, plus evidence of annual independent penetration testing. Treat ISO/IEC 27001 as a strong preference and ISO/IEC 42001 as the emerging AI governance expectation. Treat GDPR as a contractual question, not a certificate. Thematic holds SOC 2 Type II, audited annually by A-LIGN across Security, Availability, and Confidentiality. Thematic doesn't hold ISO 27001 today.


The test that cuts through fastest: ask for the report itself, then check whether confidentiality was in scope. Most of what you need to know is on that page.
