---
schema_version: "1.0.0"
document_id: "7c9a73d70c4e78a11c3d458ff2eb157dbaac441df809ff46932a71db5ddeeacb"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/digitalidentity-what-is-selective-disclosure-and-how-do-verifiable-digital-credentials-reveal-only-whats-needed/"
published_at: "2026-07-02T18:09:47+00:00"
first_seen_at: "2026-07-24T02:02:18.818185+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:260488ee2f8052e3376bf1a0c978a639daf27a11692006eed96f8489686f5791"
---

# What Is Selective Disclosure, and How Do Verifiable Digital Credentials Reveal Only What's Needed?

A cashier asks to see your ID before selling you a bottle of wine. You hand over a plastic card, and in that moment, the cashier sees your full name, your home address, your license number, your exact date of birth, and whatever else the card carries. The only fact anyone needed was whether you were old enough to buy. Everything else was overexposure by default.


A verifiable digital credential can close that gap. Selective disclosure is the mechanism that lets it, and it is a foundational privacy property of well-designed credential systems, not a feature bolted on afterward.


## What is selective disclosure?


[Selective disclosure](https://spruceid.com/learn/selective-disclosure?ref=blog.spruceid.com) is the ability of a credential holder to reveal chosen attributes from a credential while keeping the rest hidden, without breaking the issuer's signature. The W3C Verifiable Credentials Data Model 2.0 defines it plainly as "the ability of a holder to make fine-grained decisions about what information to share."


In practice, that means a resident can prove a single claim, such as "age over 21" or "resident of this state," and a verifier can trust that claim as strongly as if the whole credential were shown. The credential stays under the resident's control on their device. The verifier learns the fact it needs and nothing more. This is data minimization made concrete: collect and expose only what the interaction actually requires.


## Why paper IDs cannot do this


A physical card is an all-or-nothing object. Its data is printed together, so anyone who inspects it to check one field sees all fields. There is no way to physically hide your address while showing your photo and birth year, and no cryptographic way for the checker to confirm the card is genuine without reading it in full.


Verifiable digital credentials separate two things that paper fuses together: the proof that an authority issued the data, and the data itself. That separation is what makes it possible to reveal one attribute at a time.


## How cryptography works, at a high level


The basic idea is that the issuer doesn't sign the actual information in a credential directly. Instead, it creates a unique digital fingerprint for each piece of information, such as a person's age or address. These fingerprints prove that the information is authentic without revealing the underlying data. The original values are then given to the credential holder as separate pieces, called disclosures, along with random data that prevents others from guessing or recreating the fingerprints.


When it's time to use the credential, the holder shares the issuer's signed credential along with only the disclosures they want to reveal. The verifier creates the same digital fingerprints from the shared information, confirms that they match the issuer's original signature, and verifies the issuer's signature. Information that isn't disclosed never leaves the holder's wallet, but the credential can still be trusted. The holder can't change a value without being detected, and the verifier can't see anything that wasn't intentionally shared. Privacy and trust work together.


## How ISO 18013-5 and SD-JWT implement it


**Standard:** ISO/IEC 18013-5 (mobile driver's license / mdoc)
**Where it is used:** In-person and, with 18013-7, online presentation of mDLs
**How selective disclosure works:** The issuer signs digests of individual data elements, and the wallet releases only the elements requested. The standard defines age attestations (age_over_NN), so a device can return "age over 21" without exposing the birthdate.


**Standard:** IETF SD-JWT
**Where it is used:** JSON/JWT-based credentials across many ecosystems
**How selective disclosure works:** The issuer creates and signs digital fingerprints of claims that can be selectively disclosed. The holder shares only the claims they want to reveal, and the verifier checks that those claims match the fingerprints in the signed credential.


The IETF SD-JWT specification describes the mechanism directly: "the digests of the Disclosures are embedded into the Issuer-signed JWT instead of the claims themselves," and when presenting, the holder "only includes the Disclosures for the claims that it wants to reveal to that Verifier." ISO/IEC 18013-5 pairs its selective-disclosure design with privacy guidance in Annex E, covering user control over what is shared and support for age attestations.


## Selective disclosure begins with the presentation request


Selective disclosure only matters if what gets asked for is constrained. A[presentation request defines exactly which attributes a verifier may ask a wallet to release](https://blog.spruceid.com/digitalidentity-what-is-a-presentation-request-and-who-controls-what-gets-asked-for/) , and the resident decides whether to consent. A well-scoped request for a bar or a liquor retailer asks for a single piece of information, not the whole license.


This is also[how presenting a digital ID online works](https://blog.spruceid.com/presenting-your-digital-id-online-how-it-works/)) : the same minimization that protects an in-person check extends to remote interactions. For the wider picture of issuance, holding, and verification, see[how verifiable digital credentials work](https://blog.spruceid.com/digitalidentity-how-do-verifiable-digital-credentials-work-a-non-technical-explanation/) .


## The payoff: resident control and civic trust


Selective disclosure changes the default from oversharing to proving. A resident proves they qualify for a service, a purchase, or a benefit without surrendering their full identity, and the agency or business still gets a verifiable answer it can rely on. Because the property is designed into the standards rather than added later, privacy does not depend on a vendor's promise or a policy that could change. It is enforced by cryptography and confirmed by open specifications. That is the quiet foundation of trust in public digital services: systems that ask for less, prove more, and leave the resident in control.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
