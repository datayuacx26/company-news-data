---
schema_version: "1.0.0"
document_id: "ecc327477c400d402b389e8157eeff8ed6e1624f52666427d34631bff1bb7c18"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/interoperability-using-a-mobile-drivers-license-at-the-airport-how-tsa-checkpoint-mdl-acceptance-works/"
published_at: "2026-07-02T22:49:38+00:00"
first_seen_at: "2026-07-24T02:02:18.818185+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:90cdfc89a28407843c36e9396ddd3b25298801663e3b2f91f6362bab5332752f"
---

# Using a Mobile Driver's License at the Airport: How TSA Checkpoint mDL Acceptance Works

You walk up to the podium, open your phone's wallet, and tap it against a small reader or scan a QR code on its screen. A few seconds later, the officer sees your photo and a short set of details, and you keep moving. No handing over a plastic card. That interaction looks simple, but it relies on a chain of standards and trust decisions working together. When one link is missing, the option disappears, and you reach for your physical ID instead.


This post walks through what actually happens at the reader, why an mDL is trustworthy without a network lookup of your record, and why airport digital ID acceptance is still uneven across states and airports.


## What happens at the reader when you tap or scan


The exchange follows[ISO/IEC 18013-5](https://www.iso.org/standard/69084.html?ref=blog.spruceid.com) , the international standard for the mobile driver's license application. It begins with device engagement: your phone and the reader establish a connection over a QR code, NFC tap, or Bluetooth Low Energy, and set up an encrypted session for that single interaction.


The reader then requests specific fields rather than reading the whole credential. For a TSA identity check, that is your portrait and the identifiers needed to confirm your identity, not your full record. This is selective disclosure: you share the data a transaction requires and nothing more. The same privacy property carries over to online use, which we cover in[Presenting Your Digital ID Online: How It Works](https://blog.spruceid.com/presenting-your-digital-id-online-how-it-works/) .


Crucially, the data comes from your device. ISO/IEC 18013-5 is designed for offline retrieval, so the reader does not have to call your state's DMV to pull your record in the moment. That protects both availability at a busy checkpoint and your privacy, because the state is not notified every time you show your ID.


## How the officer's system knows the ID is genuine


The reader trusts the credential because it can verify the issuing state's cryptographic signature, not because it recognizes the app or the phone. When a state issues an mDL, it signs the credential data with its private key. That signature travels with the credential.


To check it, the verifier needs the matching public key from a trusted source. In the United States, that source is the American Association of Motor Vehicle Administrators (AAMVA) Digital Trust Service, which distributes a Verified Issuer Certificate Authority List (VICAL). Participating states submit the public keys used to issue their mDLs, AAMVA assembles them into a trust list, and relying parties like TSA download it. AAMVA states that the service handles only cryptographic keys and "does not receive, store, share, or otherwise interact with the personally identifiable information of any mDL holders."


TSA reads and validates credentials at the checkpoint using its Credential Authentication Technology (CAT-2) units. The reader confirms three things: the credential was signed by an issuer on the trust list, the data has not been altered since it was signed, and the portrait matches the person presenting it. That combination is what makes a phone screen as trustworthy as a physical card, and it is a working example of what we mean by[interoperability in government digital services](https://blog.spruceid.com/interoperability-what-is-interoperability-in-government-digital-services-and-why-its-not-optional/) : a verifier can trust a credential from an issuer it has never directly integrated with.


## Why mDL acceptance varies by state and airport


The most common question travelers ask is why a mobile driver’s license worked for a colleague but not for them. Acceptance depends on three conditions, and all three must align at the point of verification.


**Reader deployment.** TSA only supports mDLs at airports and checkpoints equipped with compatible digital ID readers (such as CAT-2 units). Where those readers are not yet deployed, travelers must present a physical credential.


**Participating issuers.** The issuing state must provide a standards-based mDL and participate in TSA’s digital ID acceptance framework. As of 2026, more than 20 states and territories offer compatible digital IDs, with additional jurisdictions expanding deployment.


**Program and credential trust.** The mDL must come from an issuing authority that is part of TSA’s approved trust framework and meet required identity proofing and technical standards for secure verification.


Because these factors vary by location and issuer, TSA continues to recommend that travelers carry a physical ID as a backup. mDL acceptance is expanding quickly, but it is not yet universal across all airports and checkpoints.


## What genuine interoperability requires


The airport case shows that interoperability is not one feature. It is the product of open standards, a trust registry, and conformance testing, all being in place.


**Open standards** give every issuer and every reader the same rules for engagement, data format, and signatures, so a California credential and a Colorado reader can speak without a custom integration.


**Trust registries** like the AAMVA VICAL let a verifier decide which issuers to trust without contacting each one directly.


**Conformance testing** confirms that a given wallet and a given reader actually implement the standard the same way, which is where interoperability tends to break in practice. We explore how these pieces enable recognition across jurisdictions in[Cross-State Credential Recognition: How Standards Enable Interoperability](https://blog.spruceid.com/interoperability-cross-state-credential-recognition-how-standards-enable-interoperability/) .


For residents, the more realistic expectation is gradual expansion rather than a single nationwide switch. More states will continue issuing mDLs, more airport checkpoints will come online with digital ID readers, and participation will steadily broaden over time. In the meantime, it’s still a good idea to keep a physical ID with you when you travel. As these pieces come together, the experience becomes less about the format of the credential and more about the underlying standards quietly doing the work to establish trust.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
