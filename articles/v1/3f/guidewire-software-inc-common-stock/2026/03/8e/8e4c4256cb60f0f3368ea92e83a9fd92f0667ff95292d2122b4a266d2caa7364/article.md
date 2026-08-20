---
schema_version: "1.0.0"
document_id: "8e4c4256cb60f0f3368ea92e83a9fd92f0667ff95292d2122b4a266d2caa7364"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/three-intentional-iam-decisions-that-strengthen-insurancesuite"
published_at: "2026-03-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:17:07.377837+00:00"
content_hash: "sha256:1e0b5b6cadeb6507584d57dc8ff26391a9217796b29dc6fbc8ba66ebe0b21789"
---

# Three Intentional IAM Decisions That Strengthen InsuranceSuite

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


- Three Intentional IAM Decisions That Strengthen InsuranceSuite


Identity and access management (IAM) failures in InsuranceSuite are rarely the result of bad decisions. Most of the time there was no decision.


In most cases, nothing went wrong. It simply wasn’t reviewed.


This post covers three places where IAM gaps appear consistently in InsuranceSuite implementations. The full control details are in the[IAM hardening guidance](https://docs.guidewire.com/security/hardening-guidance-for-insurancesuite/identity-and-access-management#principle-3-manage-the-full-user-account-lifecycle) , with specific control objectives for each area.


*Want to stay in the loop about what’s happening for Guidewire developers? Subscribe to our **developer newsletter** for a monthly update on events, news, and release highlights delivered directly to your inbox.*


## The roles that were never touched


InsuranceSuite ships with sample roles. They exist to demonstrate what the permission model can do, rather than serving as a starting point for production. The permissions inside them are broad by design. A demo role needs to show a lot, while a production role for a commercial underwriter needs to show very little.


Nobody on most implementation teams decides to use sample roles in production. But if nobody decides to remove them either, they sit there through user acceptance testing (UAT), through staging, through go-live. Assigned to users. With permissions, nobody reviewed.


The[Open Web Application Security Project (OWASP)](https://owasp.org/) ,[A01:2021 (Broken Access Control)](https://owasp.org/Top10/2021/A01_2021-Broken_Access_Control/) and[Common Weakness Enumeration (CWE)-269 (Improper Privilege Management)](https://cwe.mitre.org/data/definitions/269.html) both describe this failure mode as access that exceeds what a role requires, where the excess is invisible because it was inherited rather than deliberately assigned. The hardening guidance is explicit on the fix. Build from zero, not from clone-and-trim. That distinction matters because it changes what you can actually prove.


## What federated identity actually covers


Teams that complete an SSO integration often assume they've handled authentication security. The Identity Provider (IdP) is connected, Hub is brokering tokens, users are logging in with corporate credentials. Job done. The problem is that connecting your IdP to Guidewire doesn't configure the IdP. That work is still yours.


Multi-factor authentication (MFA) is the obvious place this surfaces. Guidewire Cloud Platform supports SSO through your corporate IdP. MFA is not on by default in Guidewire products. It has to be configured and enforced in your identity platform. If your IdP doesn’t require a second factor for every user, that gap exists inside your Guidewire environment whether you know it or not.[The National Institute of Standards and Technology (NIST) SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html) is clear. Single-factor authentication isn't appropriate for systems handling sensitive personal data. InsuranceSuite handles sensitive personal data constantly.


Password policy sits in the same place. Complexity, rotation, history, compromised-credential checks: all of it lives in your IdP, not in Guidewire. Policies need to be in force before any credential gets accepted and passed to Hub. The hardening guidance covers this and references NIST SP 800-63B as the applicable baseline.


*Important note: **The MFA enrollment policy for your InsuranceSuite user groups is configured in your IdP admin console, not in Guidewire. If you haven't reviewed that policy recently, that's the place to look.***


## Service accounts accumulate permissions too


The out-of-the-box (OOTB) role problem is about something sitting in a place that nobody removed. The service account problem is different. It's about something that gets built up gradually, one reasonable-looking decision at a time, until the result is unreasonable.


A vehicle valuation service gets its own account. A fraud detection integration needs ClaimCenter access, so someone reuses the existing account because it's already set up and adding another feels like overhead. A premium calculation service gets added the same way.


By the time anyone looks closely, one credential can read vehicle data, view all claims, and execute rating logic. If that credential is compromised, the blast radius covers everything it ever accumulated. This is the same failure mode as OOTB role inheritance. Permissions grow without anyone deciding to grow them.


Scoping each integration to its own OAuth (Open Authorization) token is a well-established application of least privilege, referenced in NIST SP 800-53 AC-6 and OWASP Security by Design principles. Each token covers exactly what that integration needs and nothing more.


Account lifecycle fits here, too. An employee leaves and IdP access gets revoked, but if deprovisioning doesn't include an API call to revoke InsuranceSuite access, the account sits there. Inactive but still fully permissioned. Automated deprovisioning triggered by your IdP on termination events is the control. A manual offboarding checklist is not. The hardening guidance recommends having this in place and tested before go-live.


## The audit that ships your IAM posture, or closes it


Three problems, three different causes. OOTB roles persist because nobody deactivated them. MFA gaps exist because teams assume federation means security is handled. Service accounts accumulate because each individual reuse decision looks reasonable. What they have in common is that none of them require a wrong decision to happen. The pre-go-live audit is where they either get caught or ship.


The IAM hardening guidance at[docs.guidewire.com/security](https://docs.guidewire.com/security) covers the control objectives for each area. That's where “done” is clearly defined.


[Subscribe to the Developer Newsletter](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
