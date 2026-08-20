---
schema_version: "1.0.0"
document_id: "81c98bf05c6cc49d24fe66f1b29aabc5a8fe8af1c0eb93f502d9109beaf47a3e"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/dojah-government-id"
published_at: "2026-08-13T06:29:29.545+00:00"
first_seen_at: "2026-08-13T11:06:26.885908+00:00"
fetched_at: "2026-08-13T11:06:28.332657+00:00"
content_hash: "sha256:eaa83fa6596361524728b4a861d12bd0290a8f5d9f58f50a49b825e11f8a8b0b"
---

# How Dojah's Government ID Verification Works: A Product Walkthrough

Most fraud during customer onboarding is carried out by individuals using stolen, fabricated, or borrowed identity details. Verifying those details against official government records before an account is created is what separates a compliant onboarding flow from one that is open to abuse.


Dojah connects directly to official government databases across eight African countries, allowing platforms to verify customer identities in real time, reduce onboarding friction for legitimate users, and stop fraud before an account is ever created.


### **Direct Government Database Access**


Verifying identity means checking submitted details against the source, not against a document or a self-reported record. Dojah's platform connects directly to the primary government databases responsible for issuing identity credentials in each supported market.


Within the Dojah dashboard, verification services are organised under the Verify section, divided into Individual Verification and Business Verification. Government data lookups sit at the core of individual identity verification.


Dojah Platform Navigation


└── Verify


├── Individual Verification


│ ├── Government Data Verification (Core Focus)


│ ├── Address Verification


│ └── Document Analysis


└── Business Verification


Dojah supports direct government data verification across eight African countries:


**Country**


**Supported Identity Types**


Nigeria


National Identification Number (NIN), Bank Verification Number (BVN), Phone Number (government-linked ID)


Ghana


Voter's ID, International Passport, Driver's License, SSNIT ID


Kenya


KRA PIN, National ID, International Passport


South Africa


South African National ID


Uganda


Ugandan NIN


Zimbabwe


Zimbabwean National ID


Angola


Angolan National ID


Zambia


National Card


### **Three Ways to Integrate**


Dojah supports three deployment pathways depending on how much technical resource your team has available.


**1. Manual lookups via dashboard:** For individual compliance reviews or manual checks, administrators can run lookups directly from the Dojah dashboard:


1. Select the target country.


2. Select the specific official ID type.


3. Enter the official ID number and select Run Lookup.


The system queries the official government database and returns a result within seconds, confirming whether the ID exists, whether it has been flagged, and pulling the relevant customer data for onboarding records.


**2. Direct API integration:** For technical teams building custom onboarding flows within their own mobile or web applications, verification runs entirely through Dojah's API.


***What is an API?** An API acts as a secure digital messenger. It allows your system to send an ID number to Dojah and receive verified identity data back in milliseconds, automatically populating customer profile records in your backend.*


API responses are processed within milliseconds. If an entry error occurs or a record cannot be matched, the API returns structured error codes to guide the application on how to handle the outcome smoothly.


**3. EasyOnboard widgets and shareable links:** For teams that need to deploy identity verification without writing code,


[EasyOnboard](https://dojah.io/all-products/easy-onboard) provides a visual workflow builder. Non-technical teams can configure a verification flow, copy a generated link, and send it directly to customers. Technical teams can embed the same flow as a widget within their application.


Once a customer completes verification inside an EasyOnboard widget, Dojah sends a webhook back to the business's server.


***What is a webhook?** A webhook is an automated notification sent between systems. The instant a user completes verification, Dojah pings your system to confirm completion, triggering your backend to unlock the user's account without manual intervention.*


### **Face Match, Liveness Detection, and Match Thresholds**


Confirming that an ID number exists in a government database is only part of the check. A stolen ID number passes a database lookup. What it cannot pass is a face match against the person submitting it.


Dojah combines government data lookups with physical verification checks:


**Document comparative checks:** When a user uploads a physical document image, Dojah extracts the document details and runs a comparative analysis between the document photo and a live photo of the applicant to confirm ownership.


**Liveness detection:** Integrated directly within EasyOnboard flows, liveness checks confirm that the applicant is a physically present person, not a static photo or a screen replay.


**Biometric and demographic guardrails:** Verification flows can enforce automated fraud parameters including face matching, age limit restrictions, and document integrity checks.


Multi-Layered Verification Architecture


┌────────────────────────┐ ┌────────────────────────┐ ┌────────────────────────┐


│ Government Database │ ──► │ Physical ID & Photo │ ──► │ Biometric Liveness │


│ Instant Data Lookup │ │ Comparative Check │ │ & Age Thresholds │


└────────────────────────┘ └────────────────────────┘ └────────────────────────┘


**Configurable match thresholds:** Within EasyOnboard, businesses can adjust the sensitivity threshold for identity matching. Platforms facing higher risk of duplicate submissions or near-match photo attempts can raise the threshold so that only high-confidence matches are automatically approved, while edge cases are flagged for review.


### **Data Security: Zero-Retention Architecture**


Handling government identity data requires stringent security protocols. Dojah's platform is built around direct data access and zero persistent data storage.


- **Direct sourcing:** Data is retrieved directly from official government databases, ensuring records are authentic and current.


- **End-to-end encryption:** All data transmitted during a lookup, from the client to the government database and back, is protected with end-to-end encryption.


- **Data destruction policy:** Dojah does not retain or store sensitive personal identity data once verification is complete. Information is processed to deliver the result to the business, after which the underlying transactional data is destroyed.


### **Handling Exceptions and Edge Cases**


During identity checks, the system may encounter partial mismatches or low confidence scores, for example when a user's appearance has changed significantly from an older document photo.


When a match score falls below the required threshold or an API lookup returns a mismatch, Dojah provides built-in resolution pathways:


- **System guidance:** API responses include descriptive error messages specifying whether a failure came from invalid inputs, non-existent records, or a biometric mismatch.


- **Support escalation:** For ambiguous outcomes or persistent edge cases, operational teams can connect directly with Dojah's support team, so legitimate users experiencing document discrepancies can be reviewed without compromising security.


For platforms building fraud-resistant onboarding across multiple African markets, Dojah's government data verification connects database checks, biometric confirmation, and flexible deployment into one stack.


[Explore the Dojah docs](https://docs.dojah.io/) to get started or


[book a demo](https://dojah.io/contact-sales) to see the full verification flow in action.


**FAQs**


**1. Which African countries does Dojah's government data verification cover?** Dojah supports direct government database verification in Nigeria, Ghana, Kenya, South Africa, Uganda, Zimbabwe, Angola, and Zambia, covering the primary official ID types used for financial onboarding in each market.


**2. Does Dojah store identity data after a verification is complete?** No. Dojah operates a zero-retention architecture. Identity data is processed to return a verification result and then destroyed. No sensitive personal identity data is retained after the check is complete.


**3. Can I run government data verification without a developer?** Yes. EasyOnboard lets non-technical teams configure a verification flow using a visual editor, generate a shareable link, and send it directly to customers without writing any code.


**4. What happens when a verification returns a partial match or low confidence score?** The API returns structured error codes describing the specific reason for the mismatch, whether it is an invalid input, a non-existent record, or a biometric mismatch. For persistent edge cases, Dojah's support team can assist with manual review.


**5. Is a government database lookup enough for KYC compliance?** A government database lookup confirms that an ID is valid and the submitted details match the official record. For full KYC compliance, it should run alongside liveness detection, face match, and AML screening as part of a complete onboarding flow.
