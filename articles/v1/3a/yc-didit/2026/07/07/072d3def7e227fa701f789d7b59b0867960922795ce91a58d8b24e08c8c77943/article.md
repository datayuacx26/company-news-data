---
schema_version: "1.0.0"
document_id: "072d3def7e227fa701f789d7b59b0867960922795ce91a58d8b24e08c8c77943"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/jpki-my-number-card-verification-explained/"
published_at: "2026-07-23T09:33:15.813+00:00"
first_seen_at: "2026-07-24T04:20:53.361784+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:13c54fae40ea28cee1528c9f8b3d006ebb7ca171fd2344510fb332360bd516be"
---

# JPKI and the My Number Card: How Japan's Public-Key Identity Verification Works

[Back to blog](https://didit.me/blog/) Blog · July 23, 2026


# JPKI and the My Number Card: How Japan's Public-Key Identity Verification Works


What JPKI is, how My Number card chip certificates verify identity remotely, Japan's January 2026 expansion, and the April 2027 IC-chip eKYC mandate.


By Didit


·


July 23, 2026 ·


Updated Jul 28, 2026


Japan is quietly running one of the world's most ambitious experiments in government-backed digital identity. At its centre sits JPKI — the Japanese Public Key Infrastructure — a system that lets a person prove who they are online using cryptographic certificates stored on the IC chip of their My Number card. Since mid-January 2026, that capability has been available for identity verification in remote onboarding, and from April 2027 chip-based verification becomes mandatory for non-face-to-face account openings at banks and financial institutions. This explainer covers what JPKI is, how it works, what changed in January 2026, and how it compares with the document-photo eKYC most platforms use today.


> **The short version**


> - JPKI verifies identity using digital certificates stored on the My Number card's IC chip — cryptographic proof rather than a photo of a plastic card.


> - Since mid-January 2026, remote identity verification is available via JPKI and via matching an ID document's IC-chip data against the holder's facial image.


> - From April 2027, IC-chip-based verification becomes mandatory for remote account openings; submitting photos or photocopies of ID documents will be outlawed.


> - Japan's FSA also revised its AML/CFT guidelines, effective 31 March 2026, sharpening risk-based-approach expectations for financial institutions.


> - Chip reading defeats the core weakness of photo-based eKYC: forged or counterfeit documents that are too convincing to catch from an image.


> **A note on sources.** This article is current as of July 2026 and draws on publications and reporting from Japan's Financial Services Agency (FSA), JAFIC, the Digital Agency, The Japan Times, and Biometric Update. Regulations evolve; for authoritative detail, consult the FSA, JAFIC, and Digital Agency directly. Spotted an error? Tell us at[didit.me/contact](https://didit.me/contact) .


## **What is JPKI?**


JPKI stands for Japanese Public Key Infrastructure — the national framework that puts government-issued digital certificates onto the IC chip of the My Number card, the identity card available to residents of Japan.


The idea is straightforward even if the cryptography isn't. When the card is issued, certificates are written to its chip. Later, when the holder needs to prove their identity online, the chip performs a cryptographic operation that only the genuine card can perform, and the relying service validates the result against the public-key infrastructure. Certificate status can be checked, so lost or invalidated cards stop working for verification.


The practical consequence: a service verifying someone through JPKI is not looking at a picture of a card and guessing whether it is genuine. It is receiving cryptographic proof that a valid, government-issued certificate — bound to a real, registered identity — was used, by someone who could unlock it.


JPKI was built for e-government procedures such as online tax filing, and its use has progressively expanded into private-sector services. The Digital Agency's My Number FAQ (digital.go.jp) is the best plain-language reference for how the card and its certificates work.


## **How the My Number card verifies identity remotely**


According to the Digital Agency's documentation, the My Number card's chip carries two electronic certificates, each serving a different job:


Certificate What it does How it's unlocked


**Electronic signature certificate** Digitally signs documents and applications, proving the content came from the holder and wasn't altered An alphanumeric password set by the holder


**User identification certificate** Proves "the person at the other end of this connection is the registered holder" — the workhorse of login and identity checks A short numeric PIN set by the holder


A remote verification flow typically looks like this:


1. The applicant starts onboarding on a bank's or provider's app.
2. They hold their My Number card against their smartphone's NFC reader.
3. They enter the PIN that unlocks the relevant certificate.
4. The chip responds cryptographically; the response is validated against the JPKI infrastructure, including certificate status.
5. The service receives confirmation that a genuine, valid card was used by someone who knows its PIN.


Two factors are combined: possession of the physical card (the chip cannot be photographed or photocopied into existence) and knowledge of the PIN. That combination is what makes JPKI materially stronger than photo uploads.


## **What changed in January 2026**


Since mid-January 2026, two chip-based routes are available for remote identity verification in Japan:


1. **JPKI verification** using the My Number card — the certificate-based flow described above.
2. **IC-chip data plus facial-image matching** — reading the digital data stored on an ID document's chip (including the chip-stored portrait) and matching it against a facial image of the applicant captured during onboarding.


The second route matters because it extends chip-grade assurance beyond the My Number card. Under the revised rules taking effect in April 2027, remote applicants must have the embedded IC chip of their My Number card **or driver's licence** read. The chip-plus-face route is how a driver's licence — which carries chip data but doesn't participate in JPKI's certificate flow — delivers strong remote verification: the chip proves the document is genuine, and the biometric match proves the person presenting it is its holder.


The market is already moving. One major Japanese provider reported chip-based checks growing 1.8x to 14 million, within a total of more than 60 million verifications — adoption is running ahead of the mandate.


> **Where Didit helps:** Japan's pivot to chip-based verification maps directly onto capabilities Didit already provides globally. NFC chip reading (a $0.15 line item under User Verification) extracts and validates the cryptographically signed data on chip-equipped identity documents, and biometric face match compares the chip-stored portrait against a live selfie — the same chip-plus-facial-image logic Japan's new rules describe. Around it, the full KYC bundle ($0.33 per check: ID Verification, Passive Liveness, Face Match 1:1, IP Analysis), AML Screening against 1,300+ lists, and Transaction Monitoring address the FSA's risk-based expectations. Coverage spans 220+ countries and 14,000+ document types with sub-2s inference. See how this fits Japan's criminal-proceeds-act framework on the[Didit Japan solutions page](https://didit.me/solutions/countries/japan) .


To be precise about scope: Didit is not a JPKI provider and is not integrated with Japan's My Number infrastructure. The relevant mapping is chip reading and biometric matching on chip-equipped documents — the verification logic Japan is mandating — not the JPKI certificate scheme itself.


## **JPKI and chip reading vs document-photo eKYC**


Most eKYC worldwide still works the same way: the applicant photographs their ID, takes a selfie, and software (plus sometimes a human) judges whether the document looks genuine and the faces match. Japan's regulators have concluded that, for account openings, this is no longer good enough — forged and counterfeit IDs have become too difficult to detect from images. Here is how the approaches compare:


Document-photo eKYC IC-chip / JPKI verification


**What's checked** Visual appearance of the document Cryptographically signed chip data or certificates


**Forgery resistance** Limited — high-quality fakes can pass visual inspection Strong — forging a valid chip signature is not practical


**Screenshot / replay risk** Photos can be doctored, reused, or synthetically generated Chip must be physically present and read via NFC


**Holder binding** Selfie compared to printed portrait Face match against chip-stored portrait, or PIN-unlocked certificate


**Status in Japan from April 2027** Outlawed for remote account openings Mandatory


The honest trade-off: chip-based flows require an NFC-capable smartphone and a chip-equipped document, and PIN-based flows fail when holders forget their PIN. Those friction points are real, but Japan's regulators have judged the fraud-prevention gain worth it.


## **The compliance timeline: 2025 to 2027**


Japan's shift is one strand of a broader AML/CFT modernisation under the Act on Prevention of Transfer of Criminal Proceeds, aligning Japan with FATF standards after the effectiveness gaps flagged in FATF's 2021 mutual evaluation.


Date What happens


**August 2025** Electronic Payment Instrument Service Providers (stablecoin intermediaries) brought fully into AML scope, including Travel Rule obligations


**Mid-January 2026** Remote verification available via JPKI (My Number card) and via IC-chip data + facial-image matching


**31 March 2026** FSA's revised AML/CFT guidelines take effect: sharpened risk-based approach, obligations on outsourcing, technology adoption and transaction monitoring, STR data broken down by country and customer attribute, regulator access to board-level reports, direct senior-management accountability


**April 2027** IC-chip-based verification mandatory for non-face-to-face account openings; photo/photocopy ID submission outlawed


Baseline obligations continue throughout: verify customer identity, retain records for seven years, and file suspicious transaction reports with JAFIC.


## **What this means for onboarding teams**


If you onboard Japanese customers remotely — as a bank, fintech, crypto exchange, or payment provider — the practical checklist looks like this:


- **Audit your current flow.** If it relies on photographed or photocopied IDs, it has a hard end-of-life date of April 2027 for account openings.
- **Plan for NFC.** Your onboarding app needs to read IC chips on My Number cards and driver's licences, and handle applicants whose devices can't.
- **Pair chip reading with biometrics.** Chip-plus-facial-image matching is an approved route; robust face matching and liveness are core, not optional.
- **Don't stop at verification.** The FSA's March 2026 guidelines expect self-directed risk assessment, transaction monitoring, and senior management that can answer for the programme — onboarding is only the front door.
- **Move before the deadline.** Migrating verification flows takes quarters, not weeks, and adoption data suggests competitors are already switching.


## **The bottom line**


JPKI is what identity verification looks like when a government decides that photographs of plastic cards are no longer proof of anything: cryptographic certificates on a chip, unlocked by the holder, validated in real time. The January 2026 expansion made chip-grade verification broadly available; April 2027 makes it compulsory for remote account openings. For compliance teams, the message is simple — the photo-upload era of Japanese eKYC is ending on a known date, and the replacement technology is already live.


If you're building or upgrading identity verification for the Japanese market — or anywhere chip-equipped documents can raise your assurance level — Didit offers NFC chip reading, biometric face matching, AML screening, and transaction monitoring on a single platform, with 500 free core-KYC checks every month.[Create a free Didit business account](https://business.didit.me/) and test the flow today.


*This article is general information, not legal advice. For guidance on your specific obligations under Japanese law, consult qualified counsel and the FSA, JAFIC, and Digital Agency's official publications.*


Keep reading


## Related articles


- [PEP Screening: Definitions, Scope, and Monitoring](https://didit.me/blog/pep-screening-definitions-scope-monitoring/)
- [Enhanced Due Diligence (EDD): Compliance Guide](https://didit.me/blog/enhanced-due-diligence-edd-compliance-guide/)
- [MRZ Explained: Machine Readable Zone Technical Guide](https://didit.me/blog/mrz-machine-readable-zone-technical-guide/)
- [Age Verification Software: Buyer's Guide](https://didit.me/blog/age-verification-software-buyers-guide/)
- [Flutter SDK: Add Identity Verification to Your App](https://didit.me/blog/flutter-sdk-identity-verification-integration-guide/)
- [W3C Decentralized Identifiers (DIDs) Specification](https://didit.me/blog/w3c-decentralized-identifiers-dids-specification/)
