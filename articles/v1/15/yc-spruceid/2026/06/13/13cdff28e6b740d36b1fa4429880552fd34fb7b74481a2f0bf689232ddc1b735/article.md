---
schema_version: "1.0.0"
document_id: "13cdff28e6b740d36b1fa4429880552fd34fb7b74481a2f0bf689232ddc1b735"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/digitalidentity-credential-lifecycle-101-from-issuance-to-expiry-and-who-owns-each-stage/"
published_at: "2026-06-11T23:42:59+00:00"
first_seen_at: "2026-07-24T02:02:18.818185+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:73048f71b2111fc612b22da0945a193c598dd71538fe97f6a85dc88440353ca3"
---

# Credential Lifecycle 101: From Issuance to Expiry, and Who Owns Each Stage

When a government program issues a verifiable digital credential, the work does not end at delivery. A verifiable digital credential is created, used, updated, and eventually retired, and each of those stages involves different systems, different agencies, and different responsibilities. Getting clear on who owns what, at each point in that arc, is one of the more practical things a program can do to protect residents and build durable public trust.


## The Lifecycle Stages


**Enrollment and identity proofing:** Before a credential is issued, the applicant's identity needs to be verified to the level of assurance the program requires. This stage belongs to the issuer, or to a proofing service acting on their behalf. The quality of this step shapes everything downstream, as a credential is only as reliable as the proofing that preceded it.[What Is Identity Proofing and Why Does It Matter for Government Services?](https://blog.spruceid.com/digitalidentity-what-is-identity-proofing-and-why-does-it-matter-for-government-services/) covers what different proofing levels involve and what they are designed to establish.


**Issuance:** Once identity has been established, the issuer signs the verifiable digital credential and delivers it to the holder's mobile wallet. This stage involves key management, format selection, secure delivery, and more. The issuer is responsible for the accuracy of the credential's contents and for maintaining the signing keys used to issue it. Getting the details right at this stage (such as data accuracy, expiry dates, and required attributes) matters, since corrections could require going back to the holder for reissuance.


**Active use:** The holder stores the credential and presents it to verifiers as needed. The wallet preserves secure storage and enforces any presentation constraints the issuer has specified. The verifier is responsible for confirming that the credential is valid (checking the signature, expiry date, and revocation status) before relying on it. This is the stage residents interact with most directly, and where decisions about data sharing and verification determine what information is exchanged.[How Do Verifiable Digital Credentials Work? A Non-Technical Explanation](https://blog.spruceid.com/digitalidentity-how-do-verifiable-digital-credentials-work-a-non-technical-explanation/) walks through what verification involves at this stage.


**Revocation and status management:** When a credential needs to be invalidated before its natural expiry (because circumstances changed, an issuance error was identified, or a security event occurred), the issuer is responsible for updating the credential's status and ensuring verifiers can access current status information promptly. How revocation is implemented affects both how quickly status updates are reflected during verification and how resident privacy is maintained throughout the process. Choosing a revocation approach that preserves resident privacy is as much a program decision as a technical one.[Digital Identity Beyond Credentials: What Governments Actually Need](https://blog.spruceid.com/digital-identity-beyond-credentials-what-governments-actually-need/) addresses how these operational choices shape whether a program serves residents well in practice.


**Expiry and renewal:** Every credential has an expiry date, after which it is no longer valid. When a verifiable digital credential expires, the holder undergoes re-verification or renewal to obtain a new one. The issuer is responsible for clearly communicating expiry timelines, designing a renewal process that does not require full re-enrollment when not warranted, and ensuring that the transition from an expired credential to a new one does not create service gaps for residents.


## The Handoffs Between Stages


Each stage is manageable on its own. The complexity tends to surface at the boundaries.


A proofing stage that does not clearly pass enrollment data to the issuance system can introduce credential errors downstream. A revocation system without a clearly designated maintainer is at risk of becoming out of date. A renewal process that requires residents to restart full enrollment (where a lighter re-verification would suffice) can create friction that erodes trust in the program over time.


Thinking through these handoffs early, rather than treating them as implementation details to resolve later, tends to make the overall program more straightforward to operate and easier to improve.


## Governance Across Agencies


For programs that span multiple agencies, such as a state issuing agency and a network of local service verifiers, clearly defined ownership at each stage helps ensure accountability and smooth operations as the program evolves.


When a resident presents a credential that is not accepted and needs to understand why, the program's ability to help them depends on whether the right contacts and processes are in place for each stage. Documenting lifecycle responsibilities across agencies is how programs stay accountable to the people they serve.


SpruceID works with agencies at every stage of the credential lifecycle, from proofing and issuance through revocation and renewal. If you are designing or reviewing a verifiable digital credential program and want to think through these responsibilities, get in touch with us to learn more.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
