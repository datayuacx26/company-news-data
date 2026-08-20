---
schema_version: "1.0.0"
document_id: "d47914b3b4c439d855e4e50e5134e97461363446b3c41a859360446e1b52d2f9"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/presenting-your-digital-id-online-how-it-works/"
published_at: "2026-06-26T21:18:47+00:00"
first_seen_at: "2026-07-24T02:02:18.818185+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:29a918499476e463e2e0b36c20b7c0ce39cd323013e5c6c04d645660d7cd8c3c"
---

# Presenting Your Digital ID Online: How It Works

Showing a mobile driver's license at an airport is relatively straightforward: your phone, their reader, a quick tap or QR scan. But what happens when you need to prove something about yourself to an online service? A government benefits portal, a bank onboarding flow, an age-restricted site?


The interaction is less visible, but the same principles apply. Your wallet needs a way to send your verifiable digital credential to a verifier, the verifier needs a way to request only what it actually needs, and you need to stay in control of what gets shared. The protocol that makes this possible is called[OID4VP](https://spruceid.com/learn/oid4vp?ref=blog.spruceid.com) (OpenID for Verifiable Presentations), and understanding how it works helps explain why verifiable digital credentials are more private and more reliable than their alternatives.


## The Problem with How Credential Sharing Works Today


When you prove your identity online today, you typically upload a document or photo of your ID, type in personal information, or authorize one service to share your data with another. None of these approaches gives you much control. You share everything the service asks for, often more than it actually needs, and you rarely know what happens to that data afterward.


Verifiable digital credentials are designed to change this. Instead of uploading a scan of your driver's license, you present a cryptographically signed credential from your wallet, one that contains only the specific attributes the verifier requested. But for this to work across different wallets and different verifiers, everyone needs to speak the same language. That's what OID4VP provides.


## What OID4VP Actually Does


OID4VP is a standard developed by the OpenID Foundation that defines how a digital wallet presents verifiable digital credentials to a verifier over the internet. It extends OAuth 2.0 and OpenID Connect, the same infrastructure that handles billions of secure logins every day, to support credential presentation.


The exchange works in three steps. First, the verifier sends a presentation request specifying exactly which credential attributes it needs, not your whole driver's license, but perhaps just your age or your state of residence. Second, your wallet displays that request, and you decide whether to approve it. Third, if you approve, your wallet generates a verifiable presentation, a cryptographically signed response containing only the attributes you agreed to share, and sends it back.


The verifier receives the presentation and validates two things: the issuer's cryptographic signature, confirming the credential is authentic and unaltered, and the credential's current status, confirming it hasn't been revoked or suspended since issuance. That second check is what makes online verification distinctly useful; unlike offline checks against preloaded keys, it can confirm validity at the exact moment of use.


## Why a Common Protocol Matters


Without a standard like OID4VP, every verifier would need a custom integration with every wallet. A state benefits portal would need one connection to the DMV's wallet app, another to a federal benefits wallet, and another to a commercial wallet, each of which would work differently.


OID4VP establishes a universal interface. Any wallet built to the standard can present credentials to any verifier built to the same standard, regardless of who made them. This is the same logic that made email interoperable across providers and web browsers interoperable across operating systems.[Open standards create interoperability at scale](https://blog.spruceid.com/interoperability-without-lock-in-why-standards-matter/) , not because everyone agreed to use the same product, but because everyone agreed to speak the same language.


For government agencies deploying digital identity programs, this matters in a practical way: it means a resident's wallet doesn't have to be issued by the same organization running the verifier. A state DMV-issued mDL can be presented to a federal benefits portal, a bank, or a pharmacy, as long as all parties support the protocol.


## What the Resident Experience Looks Like


From a resident's perspective, presenting a digital credential online via OID4VP should feel simple. A verifier's website or app sends a request to your wallet. Your wallet shows you what's being asked for, for example, "This service is requesting proof that you are over 18," and you approve or decline.


You don't see the cryptographic operations underneath. You see a clear, consent-based prompt: here is what's being requested, here is who is requesting it, do you want to share it?


This design reflects a core principle in how[verifiable digital credentials](https://blog.spruceid.com/digitalidentity-how-do-verifiable-digital-credentials-work-a-non-technical-explanation/) are built: the resident stays in control. The credential doesn't leave the wallet without explicit approval. The verifier receives only what was requested.


## Privacy Built Into the Protocol


One of the more important properties of OID4VP is its support for selective disclosure, the ability to share specific attributes of a credential without revealing the entire document.


If a service needs to verify that you're a licensed professional in a particular state, your wallet can present that claim on its own, without disclosing your address, date of birth, or license number. The verifier gets what it needs. You share only what's necessary.


This is privacy by design. The limitation on what gets shared is enforced by the protocol itself, not by the service's promise to use your data responsibly.[Data minimization](https://blog.spruceid.com/zerotrust-what-is-data-minimization-and-why-does-it-matter-for-government-services/) is built into how the exchange works, at the technical level, not layered on afterward.


## mDLs Online: How ISO 18013-7 and OID4VP Work Together


For mobile driver's licenses specifically, there are two complementary standards worth knowing about. ISO 18013-5 governs how an mDL is presented in person - a tap at a TSA checkpoint or a QR scan at a bar.[ISO 18013-7](https://spruceid.com/learn/iso-18013-7?ref=blog.spruceid.com) extends that to the internet, defining how an mDL can be presented to online services like bank account opening, government benefit portals, or age verification for online purchases.


OID4VP and ISO 18013-7 work in conjunction. ISO 18013-7 defines what gets presented and how the mDL data is structured for online use; OID4VP provides the transport and presentation protocol that carries it. Together, they provide states with a complete framework for mDL use in both in-person and online contexts.


The practical implications are significant. A resident with a state-issued mDL can use it to complete KYC verification at a bank, access a federal benefits portal, or verify their age for an online purchase, all without uploading a photo of their physical license or filling out a form.


## Where This Is Heading


OID4VP is now part of the technical foundation for several major digital identity programs. The EU Digital Identity Wallet mandate requires OID4VP support. US federal agencies are evaluating it for online credential presentation. The standard is also being incorporated into commercial identity verification flows where privacy and interoperability are requirements.


By building on OAuth 2.0 and OpenID Connect infrastructure that organizations already have in place, OID4VP is designed to extend existing systems rather than replace them, lowering the barrier to adoption for developers and agencies alike.


Digital identity in the US is shifting from physical to digital, and from siloed systems to interoperable infrastructure. OID4VP and ISO 18013-7 are part of the protocol layer that makes that transition possible. SpruceID builds the tools and infrastructure that help governments and enterprises issue, hold, and verify digital credentials across open standards. To learn more, visit[spruceid.com](https://spruceid.com/?ref=blog.spruceid.com) or get in touch.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
