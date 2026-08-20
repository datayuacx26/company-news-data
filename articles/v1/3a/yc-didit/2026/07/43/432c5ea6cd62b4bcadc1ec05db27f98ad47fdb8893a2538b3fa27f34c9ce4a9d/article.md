---
schema_version: "1.0.0"
document_id: "432c5ea6cd62b4bcadc1ec05db27f98ad47fdb8893a2538b3fa27f34c9ce4a9d"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/japan-s-e-kyc-overhaul-2026-2027-jpki-ic-chip-mandates-and-the-end-of-photo-based-account-opening/"
published_at: "2026-07-23T09:30:20.255+00:00"
first_seen_at: "2026-07-24T04:20:53.361784+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:0e92feb684622f53dbcd2e55cf0a9708e6e36927d0bc00cb56b55b726e01b0c4"
---

# Japan's eKYC Overhaul (2026–2027): JPKI, IC-Chip Mandates and the End of Photo-Based Account Opening

[Back to blog](https://didit.me/blog/) Blog · July 23, 2026


# Japan's eKYC Overhaul (2026–2027): JPKI, IC-Chip Mandates and the End of Photo-Based Account Opening


Japan is ending photo-based identity checks: the JPKI expansion since January 2026, the April 2027 IC-chip mandate, FSA AML rules, and how to prepare.


By Didit


·


July 23, 2026 ·


Updated Jul 23, 2026


Japan is carrying out a sweeping identity-verification reform. Through a revision of the Act on Prevention of Transfer of Criminal Proceeds (犯罪収益移転防止法), the country is phasing out photo-based identity checks for remote account opening: from April 2027, banks and financial institutions opening accounts remotely must read the embedded IC chip of a customer's My Number card or driver's licence, and submitting photos or photocopies of ID documents will be outlawed. Alongside it, the Financial Services Agency (FSA) has put revised AML/CFT guidelines into effect that sharpen the risk-based approach and push senior-management accountability for financial crime onto regulated financial institutions. This guide walks through what is changing, why, who is affected, and how to prepare.


> **The short version**


> - From **April 2027** , IC-chip-based identity verification becomes **mandatory** for non-face-to-face (remote) account openings at banks and financial institutions in Japan — photo and photocopy submissions of ID documents will be outlawed.


> - Since **mid-January 2026** , verification is available via **JPKI** (Japanese Public Key Infrastructure) using the My Number card, and by matching an ID's IC-chip digital data against the holder's facial image.


> - The FSA's revised **AML/CFT guidelines took effect 31 March 2026** : self-directed risk assessments, new obligations on outsourcing, technology and transaction monitoring, and direct senior-management accountability.


> - Baseline duties remain: verify customer identity, retain records for **7 years** , and file suspicious transaction reports with **JAFIC** .


> - Chip-based verification is already surging — one major Japanese provider reported chip-based checks growing **1.8x to 14 million** , within 60M+ total verifications.


> **A note on sources.** This article is current as of July 2026 and is drawn from FSA, JAFIC and Digital Agency publications, along with reporting from The Japan Times and Biometric Update. Regulations evolve; for authoritative detail on scope and implementation, consult the FSA, JAFIC and the Digital Agency directly. Spot an error? Tell us at[didit.me/contact](https://didit.me/contact) .


## **Why Japan is abandoning photo-based eKYC**


For years, the standard remote onboarding flow in Japan was simple: the applicant photographed an ID document, took a selfie, and the institution compared the two. The problem, as regulators concluded, is that forged and counterfeit IDs are too hard to detect from images alone. A photograph of a document tells you what the document *looks like* — not whether it is genuine.


That weakness is the stated rationale for the reform. The revised criminal proceeds act shifts remote verification from *images of documents* to *the documents' embedded IC chips* : government-issued chip data is read via NFC and matched against the holder's face — far harder to forge than an image. Image forgery that can fool a camera check is far harder to sustain against a cryptographic chip read.


### **Old flow vs. new flow**


Photo-based eKYC (being outlawed) IC-chip verification (mandated from April 2027 — JPKI is one available route)


**What the applicant submits** Photos or photocopies of an ID document, plus a selfie The IC chip of a My Number card or driver's licence, read/scanned directly


**How authenticity is checked** Visual inspection of the image Chip data is read digitally and matched against the holder's facial image


**Fraud exposure** Forged or counterfeit documents are hard to detect from images Reading the chip itself sidesteps image-based forgery


**Status** Prohibited for remote account opening from April 2027 Mandatory for remote account opening from April 2027


## **The JPKI expansion: live since January 2026**


Japan did not wait for the 2027 deadline to make the new methods available. Since mid-January 2026, two chip-based verification routes have been operational:


1. **JPKI with the My Number card.** The Japanese Public Key Infrastructure lets an applicant prove their identity using the My Number card. For specifics on how JPKI works, the Digital Agency's My Number FAQ ([digital.go.jp](https://www.digital.go.jp/) ) is the authoritative source.
2. **IC-chip data plus facial matching.** The digital data stored on an ID document's IC chip is read and matched against the holder's facial image — binding the verified document data to the live person completing the check.


The market has responded quickly. One major Japanese verification provider reported chip-based checks growing **1.8x to 14 million** , within a total of more than 60 million verifications — a clear signal that institutions are migrating ahead of the mandate rather than waiting for it.


## **The April 2027 mandate: what becomes law**


From April 2027, under the revised criminal proceeds act:


- **IC-chip-based identity verification becomes mandatory** for non-face-to-face account openings at banks and financial institutions.
- **Submitting photos or photocopies of ID documents will be outlawed** for these remote flows.
- Remote applicants must have the **embedded IC chip of their My Number card or driver's licence read or scanned** .


The practical consequence for compliance and product teams: any remote onboarding journey that today ends with "upload a photo of your ID" will be non-compliant for in-scope account openings. The capture step itself has to change — from camera-only to NFC chip reading — and that touches SDKs, device support, UX and fallback flows.


> **Where Didit helps:** Japan's shift to IC-chip verification maps directly to **NFC chip reading** — a capability Didit performs on chip-equipped identity documents, priced at $0.15 per check under User Verification — combined with **biometric face match** and **passive liveness** to tie the document to the live applicant. For the FSA's risk-based expectations, Didit adds **AML Screening** ($0.20/check against 1,300+ lists, with ongoing monitoring at $0.07/user/year) and **Transaction Monitoring** . Coverage spans 220+ countries and 14,000+ document types with sub-2s inference. See how this maps to Japanese requirements on Didit's[Japan solutions page](https://didit.me/solutions/countries/japan) . To be precise about scope: Didit is not a JPKI provider and holds no Japanese certification — confirm coverage of the specific documents in your onboarding flows with the Didit team.


## **The FSA's revised AML/CFT guidelines (effective 31 March 2026)**


The IC-chip mandate is only half of the overhaul. On **31 March 2026** , the FSA's revised AML/CFT guidelines took effect, aligning Japan with FATF standards after the effectiveness gaps flagged in FATF's 2021 mutual evaluation. The headline changes:


- **A sharpened risk-based approach.** Institutions must run self-directed risk assessments and implement mitigation proportionate to the risks they identify — not just follow a checklist.
- **New obligations around outsourcing and technology adoption.** Relying on third-party providers or new verification technology comes with explicit supervisory expectations.
- **Transaction monitoring obligations** , alongside suspicious-transaction-report data broken down **by country and customer attribute** .
- **Regulator access to board-level AML/CFT reports** and **direct senior-management accountability** — financial-crime compliance is now explicitly an executive-level responsibility, not a back-office one.


The baseline obligations continue unchanged: verify customer identity, retain records for **seven years** , and file suspicious transaction reports with **JAFIC** (the Japan Financial Intelligence Center).


## **Who is affected**


- **Banks and financial institutions** opening accounts remotely are squarely in scope of the April 2027 IC-chip mandate, and regulated financial institutions face the FSA's March 2026 supervisory expectations.
- **Crypto and stablecoin businesses:** Electronic Payment Instrument Service Providers (EPISPs) were brought fully into AML scope — including **Travel Rule obligations** — effective **August 2025** . Confirm with the FSA whether the March 2026 supervisory expectations reach your specific registration category.
- **Other businesses subject to the criminal proceeds act's non-face-to-face verification rules** should confirm their exact obligations and timelines with the FSA and JAFIC, since scope details sit with the regulators.


## **Compliance timeline: 2025 → 2027**


Date What happens Who it hits


**August 2025** EPISPs brought fully into AML scope, including Travel Rule obligations Stablecoin / electronic payment instrument providers


**Mid-January 2026** JPKI verification via My Number card, and IC-chip + facial-image matching, become available Institutions ready to adopt chip-based eKYC early


**31 March 2026** Revised FSA AML/CFT guidelines take effect Regulated financial institutions


**April 2027** IC-chip verification mandatory for remote account opening; photo/photocopy ID submission outlawed Banks and financial institutions onboarding customers remotely


## **Preparation roadmap**


With the mandate now under a year away, a realistic sequence looks like this:


1. **Inventory your remote onboarding flows.** Identify every non-face-to-face journey that currently accepts photographed or photocopied IDs. Each one needs a chip-based replacement before April 2027.
2. **Choose your chip-reading path.** That means supporting NFC reads of the My Number card and driver's licence, paired with facial matching against the chip data. Validate device coverage (NFC-capable smartphones) and design fallbacks for edge cases.
3. **Refresh your risk assessment.** The FSA's March 2026 guidelines expect self-directed risk assessments with documented mitigation. If yours predates the revision, it is due for a rewrite.
4. **Review outsourcing arrangements.** If a vendor performs verification or monitoring on your behalf, the new outsourcing obligations apply — contracts, oversight and reporting lines should reflect that.
5. **Brief the board.** Regulators can now access board-level AML/CFT reports, and senior management is directly accountable. Make sure reporting to the top actually exists and is current.
6. **Pressure-test transaction monitoring and STR processes.** STR data must be capable of breakdown by country and customer attribute; JAFIC filing pipelines and seven-year record retention should be verified end to end.


For regulatory specifics beyond this outline — exact scope, transitional arrangements, sector-by-sector applicability — go to the source: the **FSA** , **JAFIC** , and the **Digital Agency** .


## **The bottom line**


Japan has decided that a photograph of an ID is no longer proof of identity. Between the JPKI expansion already live since January 2026, the FSA's risk-based supervisory regime in force since March 2026, and the hard IC-chip mandate landing in April 2027, remote onboarding in Japan is being rebuilt around chip data and biometrics. Institutions that treat the surge in chip-based verifications as their cue — rather than waiting for the deadline — will onboard the next two years' customers without a compliance cliff.


If you are preparing your verification stack for Japan's chip-based era — NFC chip reading, face match, liveness, AML screening and transaction monitoring in one platform — you can[start with Didit today](https://business.didit.me/) .


*This article is general information, not legal advice. Consult qualified counsel and the relevant Japanese regulators for guidance on your specific obligations.*


Keep reading


## Related articles


- [PEP Screening: Definitions, Scope, and Monitoring](https://didit.me/blog/pep-screening-definitions-scope-monitoring/)
- [Enhanced Due Diligence (EDD): Compliance Guide](https://didit.me/blog/enhanced-due-diligence-edd-compliance-guide/)
- [MRZ Explained: Machine Readable Zone Technical Guide](https://didit.me/blog/mrz-machine-readable-zone-technical-guide/)
- [Age Verification Software: Buyer's Guide](https://didit.me/blog/age-verification-software-buyers-guide/)
- [Flutter SDK: Add Identity Verification to Your App](https://didit.me/blog/flutter-sdk-identity-verification-integration-guide/)
- [W3C Decentralized Identifiers (DIDs) Specification](https://didit.me/blog/w3c-decentralized-identifiers-dids-specification/)
