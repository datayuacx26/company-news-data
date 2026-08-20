---
schema_version: "1.0.0"
document_id: "8ab4850f21f542695e69f0343b9c24a406715a040a8aa148c56f3f8d663b8e52"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/ghana-card-verification-api"
published_at: "2026-08-14T11:10:13.154+00:00"
first_seen_at: "2026-08-14T14:18:36.599508+00:00"
fetched_at: "2026-08-14T14:18:38.591854+00:00"
content_hash: "sha256:c08fab9bc067bcf78e8bb118bf4f280c3c9973ba2da1e116dec7c1aaa1a0fac7"
---

# Ghana Card Verification API: How to Implement It on Your Fintech

If you are onboarding customers in Ghana,


[the Bank of Ghana's 2026 Supervisory Guidance Note](https://www.bog.gov.gh/wp-content/uploads/2026/01/SUPERVISORY-GUIDANCE-NOTE-2025.pdf) makes the requirement clear: the Ghana Card is the only accepted identification document for financial onboarding, and verification must happen against the


[NIA database](https://nia.gov.gh/service/verification-services/) in real time. Accepting another document type or manually checking a card no longer meets the current KYC standard.


For fintech and digital platforms scaling into Ghana, this means building a verification flow that connects directly to the NIA, handles the full range of verification outcomes, and sits alongside liveness detection and face match as part of a complete onboarding process.


This guide walks you through how the Ghana Card verification API works for verifying users in Ghana and how to fit it into your overall onboarding flow.


### **Ghana Card as the Only Accepted ID: What You Need to Know**


The Ghana Card is the national identity document issued by the


[National Identification Authority (NIA)](https://nia.gov.gh/service/verification-services/) . It contains a unique Ghana Card number, biometric data, and personal details stored in the NIA database. This database is used as the reference point for verifying customer identity during financial onboarding in Ghana.


The regulatory requirements are specific:


- **It is the only accepted ID for financial onboarding:**


[the Bank of Ghana’s 2026 Supervisory Guidance Note](https://www.bog.gov.gh/wp-content/uploads/2026/01/SUPERVISORY-GUIDANCE-NOTE-2025.pdf) requires regulated financial institutions to use the Ghana Card as the sole identification document for customer onboarding. For Ghanaian citizens, accepting another document type as the primary ID does not meet the current BoG standard.


- **Customers without a Ghana Card cannot transact:** Under the directive, customers who cannot present a Ghana Card are barred from conducting financial transactions. For non-citizen residents, the Non-Citizen Ghana Card issued by the


[NIA](https://nia.gov.gh/) serves the same purpose. Foreign non-residents staying in Ghana for less than 90 days may use a valid international passport alongside additional KYC checks.


- **Verification must occur against the NIA database in real time:** collecting a Ghana Card number or manually checking the document is not enough. The 2026 KYC framework requires platforms to verify customer details against the NIA database in real time to confirm the card’s authenticity and the customer’s identity. This is the role a Ghana Card verification API plays in the onboarding process.


Meeting the compliance standard starts with understanding what the API verifies.


**Related:**


[A guide on how to run AML checks for business customers](https://dojah.io/blog/business-aml-checks-fintech)


### **What the Ghana Card Verification API Checks**


A Ghana Card verification API verifies submitted identity information against the National Identification Authority (NIA) database in real time. It checks whether the submitted details match the record linked to the provided Ghana Card number before your platform proceeds with onboarding.


The verification check involves three things:


- **What gets submitted:** A verification request typically includes the Ghana Card number together with one or more identity details, such as the customer's full name and date of birth, for comparison against the NIA record. Depending on the implementation, the request may also include the card's expiry date or the customer's photo for face match verification.


- **What the database check confirms:** The NIA database query confirms whether the submitted Ghana Card number is valid and active and whether the customer's name and date of birth match the official record. Where available, it also returns a photo for biometric matching.


- **What it does not check:** The Ghana Card verification API confirms identity against the NIA record, but it does not assess fraud risk, perform sanctions or PEP screening, or verify liveness. Those checks belong to separate stages of the onboarding flow and should run alongside identity verification, not replace it.


The verification result provides the information your platform needs to determine the next step in the onboarding process.


### **What the Ghana Card API Returns**


Once the verification request is complete, your platform receives a structured response that helps determine the next step in the onboarding process. In most cases, the information returned is enough to support a pass, flag, or fail decision without manual review.


The main response fields are:


- **Name match:** The response indicates whether the submitted name matches the NIA record and, where applicable, the degree of match. This helps account for common variations in how names are recorded across different records.


- **Date of birth confirmation:** The response confirms whether the submitted date of birth matches the NIA record, does not match, or is absent from the record. A date of birth mismatch alongside a matching name is better handled through further review than an automatic rejection.


- **Photo where available:** Where the NIA record includes a photo, it is returned in the response for face match verification against a captured selfie. Since not every NIA record contains a photo, your onboarding flow should be able to handle cases where one is unavailable.


- **Match confidence:** A confidence indicator shows how closely the submitted information matches the NIA record. Your onboarding flow can use this result to determine whether the verification should pass, be flagged for review, or fail.


The response is only one part of the process. Your onboarding flow also needs to handle cases where the verification does not return a clean match.


### **Handling Mismatches, Partial Matches, and Failed Lookups**


A verification flow that only works when every check returns a clean result is not ready for production. Mismatches, partial matches, timeouts, and failed lookups are expected parts of onboarding, and how your platform handles them affects both customer experience and compliance.


Here are the common cases to plan for:


- **Name mismatches and partial matches:** A customer's name may appear differently across their Ghana Card, bank records, and submitted details. A partial match should not automatically fail verification; instead, the flow should allow for additional review before making a final decision.


- **No photo returned:** If the NIA record does not contain a photo, the verification response cannot provide one for face match. Your onboarding flow should have a defined fallback, such as using another verification step or routing the case for manual review where required.


- **Primary data discrepancies:**


[The Bank of Ghana's 2026 Supervisory Guidance Note](https://www.bog.gov.gh/wp-content/uploads/2026/01/SUPERVISORY-GUIDANCE-NOTE-2025.pdf) requires primary data discrepancies, such as a customer's name, date of birth, or nationality, to be corrected directly through the NIA. Your platform should clearly communicate this to the user instead of treating it as a standard verification failure.


- **Timeouts and failed lookups:** Verification requests may fail because of connection issues or temporary database availability problems. Your flow should include retry logic and a fallback process, such as holding the request for another attempt or sending it for manual review instead of immediately rejecting the customer.


With these edge cases handled, the Ghana Card check can work effectively as part of a complete onboarding flow.


### **How Ghana Card Verification Fits Into a Complete Onboarding Flow**


A Ghana Card database lookup confirms that the submitted details match the NIA record. It does not confirm that the person presenting the card is the cardholder, so liveness detection and face match need to work alongside the database check.


Here is how the verification checks fit together:


- **Database check plus liveness plus face match:** The Bank of Ghana's 2026 framework requires biometric verification alongside document verification for compliant onboarding. In practice, the Ghana Card lookup verifies the card and identity details, liveness detection confirms that a real person is present, and face match confirms that the person matches the cardholder.


- **Sequencing the checks correctly:** Running the database lookup first allows your platform to use the returned photo for a face match where one is available. If no photo is returned, the face match can use the document capture instead, while liveness detection runs in parallel with or immediately before the selfie capture.


- **AML screening sits after identity is confirmed:** PEP and sanctions screening should happen after identity verification, not before. Screening an identity that has not been verified makes it difficult to confidently associate the results with the actual customer.


Once your onboarding flow is in place, the next step is connecting it to


[Dojah's Ghana Card verification API](https://dojah.io/all-products/government-id-verification) .


### **How Dojah's Ghana Card Verification API Works**


[Dojah's Ghana Card verification API](https://docs.dojah.io/docs/ghana/lookup-card) connects directly to the government database to verify customer identity in real time.


- **Direct government database connection:** Dojah queries the NIA database directly rather than routing requests through intermediaries, helping improve response times and reduce latency that can slow customer verification.


- **API and EasyOnboard integration options:** For teams integrating via API, the endpoint returns first name, last name, and gender from the NIA record. For teams using


[EasyOnboard](https://dojah.io/all-products/easy-onboard) , the full verification flow also surfaces date of birth, face match, liveness confirmation, and a match confidence score, giving your onboarding flow everything it needs to make a pass, flag, or fail decision.


- **Fits into a complete onboarding flow:** Ghana Card verification works alongside Dojah's


[liveness detection](https://dojah.io/all-products/liveness-check) and face match within the same integration, enabling identity verification, biometric confirmation, and AML screening to run from a single platform rather than managing separate vendor integrations.


- **Sandbox environment for testing:** A sandbox environment lets your team test edge cases, including partial matches and failed lookups, before moving the integration to production.


Building a compliant Ghana onboarding flow means connecting identity verification, liveness detection, face match, and AML screening in the right sequence. Dojah's ID verification covers each of those layers without requiring a separate vendor for each.


Ready to start verifying with the Ghana Card API?


[Sign up](https://app.dojah.io/signup) or


[explore the Dojah API documentation](https://docs.dojah.io/docs/ghana/lookup-card) to get started.


### **Frequently Asked Questions**


**1. What is a Ghana Card verification API?**


A Ghana Card verification API allows your platform to verify a customer's identity by checking the submitted Ghana Card details against the National Identification Authority (NIA) database in real time. The response helps determine whether the identity information matches the official record.


**2. What information is needed to verify a Ghana Card?**


A verification request typically includes the Ghana Card number and one or more identity details, such as the customer's full name and date of birth. Depending on the implementation, it may also include the card's expiry date or a photo for face match verification.


**3. Is a Ghana Card verification API enough for KYC compliance?**


No. A Ghana Card verification API confirms identity against the NIA record, but it does not replace liveness detection, face match, or AML screening. These checks work together to support a complete onboarding flow.


**4. What happens if the Ghana Card verification does not return a match?**


A failed or partial match should not always result in an automatic rejection. Your onboarding flow should be able to handle cases such as name variations, missing photos, or temporary lookup failures before making a final decision.


**5. Does Dojah's Ghana Card verification API support sandbox testing?** Yes. Dojah provides a sandbox environment that lets your team test verification requests, including edge cases like partial matches and failed lookups, before moving the integration to production.
