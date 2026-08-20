---
schema_version: "1.0.0"
document_id: "c4163a238733281cd0751712526cc703940fd6de646073b947153be4689126ec"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/japan-ic-chip-verification-mandate-april-2027/"
published_at: "2026-07-23T09:31:49.358+00:00"
first_seen_at: "2026-07-24T04:20:53.361784+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:dc82bff892e093a483650f4be9878c0260668eb3cdea81f9e577420c0fc53b05"
---

# Japan's April 2027 IC-Chip Verification Mandate: What Changes for Remote Onboarding

[Back to blog](https://didit.me/blog/) Blog · July 23, 2026


# Japan's April 2027 IC-Chip Verification Mandate: What Changes for Remote Onboarding


From April 2027, Japan outlaws photo-based ID checks for remote account opening. What the IC-chip mandate requires, who must comply, and how to prepare.


By Didit


·


July 23, 2026 ·


Updated Jul 23, 2026


Japan is about to close one of the oldest loopholes in remote customer onboarding: the photographed ID. Under a revision to the Act on Prevention of Transfer of Criminal Proceeds (犯罪収益移転防止法), from April 2027 banks and financial institutions will no longer be allowed to verify remote applicants using photos or photocopies of identity documents. Instead, non-face-to-face account openings will require reading the embedded IC chip of a My Number card or driver's licence — cryptographic data pulled straight from the document, not a picture of it. If you onboard Japanese customers online, this is a hard deadline, and the operational work starts well before 2027.


> **The short version**


> - From **April 2027** , IC-chip-based identity verification becomes **mandatory** for non-face-to-face (remote/online) account openings at banks and financial institutions in Japan; submitting photos or photocopies of ID documents will be outlawed.


> - The rationale is counterfeit detection: forged IDs are too hard to spot from images, but an IC chip's digital data is cryptographically signed and far harder to fake.


> - Remote applicants will need the chip of their **My Number card or driver's licence** read; since mid-January 2026, verification via **JPKI** (using the My Number card) and via IC-chip data matched against the holder's facial image is already available.


> - Separately, the FSA's revised **AML/CFT guidelines took effect 31 March 2026** , sharpening the risk-based approach and putting senior management directly on the hook.


> - Institutions should treat 2026 as the migration window: chip-capable verification flows take time to roll out across an entire customer base.


> **A note on sources.** This article is current as of July 2026 and is drawn from publications and reporting by Japan's Financial Services Agency (FSA), JAFIC, the Digital Agency (digital.go.jp), The Japan Times, and Biometric Update. Regulations evolve and implementation details are still being finalised in places — for authoritative specifics, consult the FSA, JAFIC, or the Digital Agency directly. Spotted something that needs correcting? Tell us at[didit.me/contact](https://didit.me/contact) .


## **What the criminal-proceeds-act revision actually changes**


The Act on Prevention of Transfer of Criminal Proceeds is Japan's core AML statute. It is the law that obliges financial institutions to verify customer identity at onboarding, retain records for seven years, and file suspicious transaction reports with JAFIC (the Japan Financial Intelligence Center).


Until now, the act permitted several ways to verify a remote applicant, including having the customer photograph their ID card or mail in a photocopy. That flexibility made online account opening easy — and it made identity fraud easy too. Regulators concluded that forged and counterfeit IDs are simply too difficult to detect reliably from an image. A photo of a fake driver's licence can look pixel-perfect; the fraud lives in the physical document, not the picture.


The revision resolves this by anchoring remote verification to the one thing a counterfeiter cannot cheaply replicate: the IC chip. Japanese My Number cards and driver's licences carry embedded chips containing digitally signed identity data. Reading the chip — typically by tapping the card against an NFC-capable smartphone — retrieves that data directly and lets the verifier confirm it hasn't been tampered with.


From April 2027:


- **Photo and photocopy submission of ID documents is outlawed** for non-face-to-face account openings at banks and financial institutions.
- **Chip reading becomes the required path** for remote applicants, using the My Number card or driver's licence.


## **Old vs new: remote verification methods**


Before the revision From April 2027


**Photographing an ID and uploading the image** Permitted for remote onboarding **Outlawed**


**Mailing a photocopy of an ID** Permitted **Outlawed**


**Reading the IC chip of a My Number card or driver's licence** Optional; increasingly common **Mandatory** for non-face-to-face account opening


**JPKI verification via My Number card** Available since mid-January 2026 Continues as a compliant chip-based path


**Chip data matched against the holder's facial image** Available since mid-January 2026 Continues as a compliant chip-based path


The two chip-based methods that went live in mid-January 2026 preview what compliant onboarding looks like: either JPKI — Japan's Public Key Infrastructure, using the certificate on the My Number card — or extracting the digital identity data from an ID's IC chip and matching it against the applicant's face. In both cases, the source of truth is cryptographic chip data plus biometrics, not a photograph of a plastic card.


## **Why chips beat photos**


An IC chip changes the fraud economics in three ways:


1. **Data integrity.** Chip contents are digitally signed by the issuing authority. Alter the name or photo stored on the chip and the signature breaks. A visual forgery — however good — carries no valid signature.
2. **Liveness of the document.** Reading a chip proves the physical, genuine document is present at the moment of onboarding. A screenshot, a printout, or a stolen image of someone's ID cannot produce a chip read.
3. **A clean biometric anchor.** The chip stores the holder's facial image as issued. Matching that high-quality reference against a live selfie is far more reliable than matching against a photographed card under bad lighting.


The market is already moving. IC-chip and JPKI verifications are surging in Japan — one major Japanese provider reported chip-based checks growing 1.8x to 14 million, within a total of more than 60 million verifications. The April 2027 deadline will turn that trend into the baseline.


## **The other half of the story: the FSA's revised AML/CFT guidelines**


The chip mandate is one piece of a broader tightening. On **31 March 2026** , revised FSA AML/CFT guidelines took effect, responding to effectiveness gaps flagged in FATF's 2021 mutual evaluation of Japan. The revisions align Japan with FATF standards and include:


- A **sharpened risk-based approach** — institutions must run self-directed risk assessments and implement mitigation proportionate to their own exposure, not just follow checklists.
- **New obligations around outsourcing, technology adoption, and transaction monitoring** — using modern tooling is now an expectation, and outsourced functions remain the institution's responsibility.
- **Suspicious-transaction-report data broken down by country and customer attribute** , sharpening how institutions analyse and report risk.
- **Regulator access to board-level AML/CFT reports** and **direct senior-management accountability** — AML is now explicitly a boardroom matter, not a back-office one.


Crypto is inside the perimeter too: Electronic Payment Instrument Service Providers (EPISPs) — the stablecoin intermediaries — were brought fully into scope, including Travel Rule obligations, effective August 2025.


## **Compliance timeline: 2025 → 2027**


Date What happens


**August 2025** EPISPs (stablecoin intermediaries) fully in scope of AML obligations, including the Travel Rule


**Mid-January 2026** JPKI verification via My Number card and IC-chip-plus-facial-image verification become available


**31 March 2026** Revised FSA AML/CFT guidelines take effect: risk-based approach, outsourcing/technology/monitoring obligations, senior-management accountability


**April 2027** IC-chip verification **mandatory** for non-face-to-face account opening; photo/photocopy ID submission **outlawed**


## **Who must comply, and what to do now**


The mandate lands on banks and financial institutions covered by the criminal proceeds act that open accounts remotely. If your onboarding funnel includes a "take a photo of your ID" step for Japanese customers, that step has an expiry date.


A practical checklist for the migration window:


- **Audit your remote onboarding flows.** Identify every path that relies on photographed or photocopied IDs for Japanese account openings.
- **Add NFC chip reading to your verification stack.** Chip-equipped My Number cards and driver's licences can be read with ordinary NFC-capable smartphones — no special hardware for the end user.
- **Pair chip reads with biometric face matching.** Matching the chip's stored facial image (or the document holder) against a live selfie with liveness detection is central to the new model.
- **Revisit your risk assessment.** The FSA's March 2026 guidelines expect a documented, self-directed assessment — and your board will be asked about it.
- **Keep the baseline obligations tight.** Identity verification, seven-year record retention, and suspicious transaction reports to JAFIC remain the foundation everything else sits on.


> **Where Didit helps:** Japan's shift to IC-chip verification maps directly to NFC chip reading — a capability Didit performs on chip-equipped identity documents — combined with biometric face match, passive liveness, AML screening against 1,300+ lists, and transaction monitoring for the FSA's risk-based expectations. Didit verifies documents from 220+ countries with 14,000+ supported document types and sub-2s inference, with a full KYC bundle at $0.33 per successful check (500 free core-KYC checks per month) and NFC chip reading priced at $0.15 under User Verification. See how this fits the JAFIC and criminal-proceeds-act context on[Didit's Japan solutions page](https://didit.me/solutions/countries/japan) .


## **The bigger picture**


Japan is making a bet other regulators are watching closely: that image-based remote identity verification has reached the end of its useful life, and that chip-based cryptographic verification should be the floor, not the premium option. For institutions, the message is unambiguous. The photograph-your-ID era in Japanese financial onboarding ends in April 2027 — and the institutions that migrate during 2026, while the JPKI and chip-plus-face paths are already live, will spend the deadline year tuning their funnels instead of rebuilding them.


Ready to bring chip-based verification, face matching, and AML screening into your Japan onboarding flow before the deadline?[Get started with Didit](https://business.didit.me/) .


*This article is general information, not legal advice. For guidance on your specific obligations under Japanese law, consult qualified counsel or the FSA, JAFIC, and Digital Agency directly.*


Keep reading


## Related articles


- [PEP Screening: Definitions, Scope, and Monitoring](https://didit.me/blog/pep-screening-definitions-scope-monitoring/)
- [Enhanced Due Diligence (EDD): Compliance Guide](https://didit.me/blog/enhanced-due-diligence-edd-compliance-guide/)
- [MRZ Explained: Machine Readable Zone Technical Guide](https://didit.me/blog/mrz-machine-readable-zone-technical-guide/)
- [Age Verification Software: Buyer's Guide](https://didit.me/blog/age-verification-software-buyers-guide/)
- [Flutter SDK: Add Identity Verification to Your App](https://didit.me/blog/flutter-sdk-identity-verification-integration-guide/)
- [W3C Decentralized Identifiers (DIDs) Specification](https://didit.me/blog/w3c-decentralized-identifiers-dids-specification/)
