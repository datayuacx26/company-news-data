---
schema_version: "1.0.0"
document_id: "92fe0e2de653356aca3b36bb7b9b090f3e83cf9eab54f9d83de9668253a18bd2"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/what-is-state-endorsed-digital-identity-sedi/"
published_at: "2026-08-17T21:26:52+00:00"
first_seen_at: "2026-08-17T21:49:12.564212+00:00"
fetched_at: "2026-08-17T21:49:15.227978+00:00"
content_hash: "sha256:8b23513e3fe9a6b0bfcd78b19d9d76ccf6b2d807a111e8650ea40e840ede187f"
---

# What Is State-Endorsed Digital Identity (SEDI)?

State-Endorsed Digital Identity, or SEDI, is a rights-first approach to digital identity that defines how governments can establish trusted digital identity infrastructure without taking ownership or control of an individual’s identity. The model was pioneered in Utah and is now informing a broader multi-state conversation about how digital identity can support secure verification while preserving individual liberty, privacy, choice, and decentralized control. Utah has begun[convening peer states](https://www.sedisummit.com/?ref=blog.spruceid.com) around these principles, with the goal of developing a shared framework that can extend beyond a single state or implementation.


**At the center of SEDI is a distinctive premise: identity belongs to the individual.**


Government does not create a person’s identity; it endorses claims about that identity according to objective standards. Utah’s[Implementation Guide](https://sedi.utah.gov/implementation-guide/?ref=blog.spruceid.com) translates that principle directly into system architecture, stating that the state’s role should be endorsement rather than origination.


SEDI is therefore not a new credential format, nor is it simply another name for a mobile driver’s license (mDL). It is a framework for establishing the rights, rules, and technical requirements that digital identity systems should satisfy. Different credentials, wallets, and technologies can potentially operate within that framework, provided they meet the required standards for privacy, security, interoperability, and individual control.


Utah established the legal foundation for this model through ⁠[S.B. 275](https://le.utah.gov/~2026/bills/static/SB0275.html?ref=blog.spruceid.com) , which created a comprehensive Digital Identity Bill of Rights and requirements for State-Endorsed Digital Identity. Those protections include the right to continue using physical identification, the right to choose which identity attributes to disclose, protections against surveillance of identity presentations, and the right to use a digital identity without surrendering a personal device. Utah’s draft ⁠Implementation Guide is now translating those statutory rights into technology-neutral requirements for implementation, evaluation, and certification.


SpruceID outlined our interpretation of how SEDI could operate in practice in[our response to Utah’s SEDI Request for Information](https://spruceid.com/resources/spruceid-utah-sedi-rfi-response.pdf?ref=blog.spruceid.com) . The response covers credential architecture, privacy, standards, interoperability, wallets, verification, and governance, and translates the broader SEDI principles into a set of concrete implementation recommendations.


One of the central questions we address is what, exactly, the state is endorsing. Our recommended interpretation is that SEDI is most useful not as a single canonical digital credential, but as an endorsement framework: a set of rights, technical controls, and trust requirements that different credentials and implementations can satisfy.


## Why SEDI was created


Digital identity can make it easier to prove trusted information, reduce fraud, and share only the information a transaction requires. But without clear safeguards, it can also make it easier to track where an identity is used, collect unnecessary data, or make individuals dependent on a particular government or technology provider.


SEDI starts with the rights digital identity infrastructure should preserve. Utah’s[Protecting Liberty in the Digital Age](https://privacy.utah.gov/wp-content/uploads/SEDI_ProtectingLiberty.pdf?ref=blog.spruceid.com) argues that convenience, interoperability, and innovation should not require people to give up privacy or individual control. Instead, those protections should be built into the rules and architecture of the system from the beginning.


That rights-first approach is notable in the U.S. digital identity landscape. The[American Civil Liberties Union (ACLU) called](https://www.aclu.org/news/privacy-technology/utah-digital-id-law?ref=blog.spruceid.com) Utah’s 2026 legislation the strongest state digital identity bill it had seen, highlighting protections around surveillance, selective disclosure, wallet choice, and data minimization, while noting that implementation will ultimately determine how well those protections work in practice.


## The rights at the center of SEDI


SEDI’s protections are intended to shape how digital identity systems actually work. The core principles can be understood in a few practical categories:


1. **Choice and individual control:** Using digital identity should remain voluntary. People should be able to keep using physical credentials and decide when and how a digital credential is presented.
2. **Data minimization and selective disclosure:** Individuals should disclose only what a transaction requires. For example, proving that someone is over 21 should not require sharing a home address, exact birth date, or driver license number.
3. **Freedom from surveillance:** Presenting a digital credential should not automatically tell the issuer where, when, or to whom it was presented. SEDI is designed to avoid “phone home” architectures that could create a centralized history of identity use.
4. **Wallet choice and portability:** Individuals should not be locked into a single wallet or technology provider in order to use a state-endorsed identity. The framework is intended to support choice while preserving a consistent baseline of protections.
5. **Transparency and accountability:** People should be able to understand the standards and rules governing the system, including what protections apply and how their identity data can be used.
6. **Duty of Loyalty:** Organizations that process identity attributes have responsibilities to the individuals whose data they handle. In practice, that means issuers, wallets, and verifiers should not treat access to identity data as permission to collect, retain, or use more than the transaction requires.


## From rights on paper to technical requirements


For these rights to be meaningful, they have to be reflected in how the technology actually works. Utah’s draft Implementation Guide begins translating the Digital Identity Bill of Rights into architectural requirements: selective disclosure can minimize the data shared in a transaction, offline or local verification can prevent an issuer from tracking where a credential is used, and open protocols can allow credentials to work across different wallets and verification systems.


Architecture can enforce protections that policy alone cannot. A system cannot meaningfully promise freedom from surveillance, for example, if every identity presentation must contact a central server controlled by the issuer. As Utah continues developing its implementation and conformance requirements, this raises a central architectural question: What exactly should qualify for the state’s endorsement?


## Our RFI recommendation: SEDI is an endorsement framework, not one credential


In[our response](https://spruceid.com/resources/spruceid-utah-sedi-rfi-response.pdf?ref=blog.spruceid.com) to Utah’s SEDI Request for Information, SpruceID recommends interpreting SEDI as a framework of statutory and technical controls rather than as a single, universal credential. Under that model, the state defines what a State-Endorsed Digital Identity must be able to prove and the protections it must provide. A credential that satisfies those requirements can earn the SEDI endorsement, regardless of whether every SEDI implementation uses the same credential format or technology stack.


Consider a digital state ID that meets the full SEDI requirements for identity assurance, individual control, privacy, unlinkability, security, and interoperability. That credential could qualify as a SEDI credential. Another foundational identity credential could potentially qualify as well if it meets the same bar. The common denominator is not what the credential is called or which vendor built it; it is whether the credential and the systems around it conform to the SEDI framework.


This distinction also allows SEDI’s protections to extend beyond credentials that carry the SEDI endorsement. A professional license, permit, or eligibility credential may serve a narrower purpose and may not need to meet the same identity assurance requirements as a foundational identity credential. But it could still adopt SEDI controls such as selective disclosure, privacy-preserving verification, or no-phone-home behavior.


The result is a layered model rather than a single digital ID program. SEDI can set a high bar for credentials that represent state-endorsed identity while also establishing principles that improve the broader ecosystem of government-issued digital credentials. It also gives the framework room to evolve: standards and credential technologies can change without requiring the underlying rights and trust requirements to change with them.


## What does that ecosystem look like in practice?


Imagine a resident receives a digital identity credential from an authoritative issuer and stores it in a wallet of their choice. Later, they need to prove a specific fact about themselves to a government agency or private-sector service. Their wallet can present the necessary information, the verifier can establish that it came from a trusted issuer and has not been tampered with, and the resident can avoid sharing information the transaction does not require.


Importantly, that interaction does not have to route back through the issuer. If the system is designed appropriately, the issuer does not need to learn where the resident presented the credential or maintain a record of every interaction. The wallet can act on behalf of the individual rather than as an extension of the issuer, while the verifier receives the trustworthy information it needs.


That illustrates the distinct roles in a SEDI ecosystem. Issuers provide authoritative credentials, wallets allow individuals to receive and present them, and verifiers or relying parties validate information for a particular interaction. The individual remains at the center, controlling the credential and its presentation.


The state’s role is to establish the trust framework around those interactions rather than necessarily operating every component itself. Multiple issuers, wallets, credential technologies, and verification systems can participate if they meet the applicable requirements. Open standards, including[ISO mobile document specifications](https://spruceid.com/learn/iso-23220?ref=blog.spruceid.com) ,[W3C Verifiable Credentials](https://spruceid.com/learn/w3c-vc?ref=blog.spruceid.com) , and[OpenID protocols](https://spruceid.com/learn/oid4vp?ref=blog.spruceid.com) , can help those components interoperate across providers, organizations, and eventually jurisdictions rather than creating a closed ecosystem specific to one state.


## A model for other states


The questions SEDI is trying to answer are not unique to Utah. Every state developing digital identity infrastructure will eventually need to decide who controls identity data, what privacy protections are required, how credentials work across systems and jurisdictions, and what role government should play in establishing trust.


That makes the policy question bigger than whether a state should offer a digital ID. States can instead ask:


> **What conditions should digital identity infrastructure have to meet before we are willing to endorse it?**


SEDI offers a model in which government defines and enforces the trust requirements without needing to operate every wallet, issue every credential, or sit in the middle of every identity transaction. For states beginning to make these decisions, that distinction can create more room for interoperability and innovation while keeping the underlying rights consistent.


## What comes next for SEDI


S.B. 275 establishes the legal foundation for SEDI, but the framework is intentionally not finished. Utah’s current Implementation Guide remains draft guidance, and practical questions around conformance, certification, verifier behavior, governance, and enforcement will be answered as the model moves from legislation into real-world implementation.


The next phase is also becoming a multi-state effort. At the upcoming[2026 SEDI Summit](http://sedisummit.com/?ref=blog.spruceid.com) , state leaders will come together to discuss how these principles can inform trusted digital identity infrastructure across jurisdictions. That conversation matters because a SEDI model becomes substantially more powerful if an identity endorsed under one state’s framework can eventually be understood and trusted elsewhere without sacrificing the rights that made it trustworthy in the first place.


As SEDI continues to develop, we’ll update this resource to reflect new guidance, implementation decisions, and lessons from the growing multi-state conversation. For a deeper look at how we believe SEDI’s principles can translate into practice, read[our full response](https://spruceid.com/resources/spruceid-utah-sedi-rfi-response.pdf?ref=blog.spruceid.com) to Utah’s SEDI Request for Information.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
