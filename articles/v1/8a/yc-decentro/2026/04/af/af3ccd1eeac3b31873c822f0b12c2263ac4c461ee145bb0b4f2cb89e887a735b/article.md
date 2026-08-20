---
schema_version: "1.0.0"
document_id: "af3ccd1eeac3b31873c822f0b12c2263ac4c461ee145bb0b4f2cb89e887a735b"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-rss-e6182ea9811a"
canonical_url: "https://decentro.tech/blog/pocketly-case-study/"
published_at: "2026-04-20T07:02:44+00:00"
first_seen_at: "2026-07-20T23:23:43.928142+00:00"
fetched_at: "2026-07-28T21:56:42.162124+00:00"
content_hash: "sha256:3ab46dfc4f2c2d66e7963571a0b42cdec5327e9360d58fca5328a9004252707b"
---

# 7-Minute Loan Approvals: Pocketly’s Growth with DigiLocker SSO

Table of Contents


India’s fastest-growing digital lending platform for young borrowers eliminated onboarding drop-offs with Decentro’s DigiLocker SSO Suite, turning a multi-step compliance process into a seamless, government-verified background check.


##


The Credit Opportunity — And Its Onboarding Burden


India’s digital lending market has undergone a quiet revolution. Smartphone penetration, UPI infrastructure, and a generation of first-time borrowers have collapsed what was once a branch-banking experience, weeks of paperwork, physical document submission, and manual underwriting, into something that should take minutes on a phone screen.


But speed has a shadow. Every digital loan disbursement sits squarely within the RBI’s regulatory perimeter. That means mandatory KYC for every borrower, rigorous identity verification against government-issued documents, and an audit trail that lenders and their NBFC partners must maintain at all times. For platforms operating at the scale of millions of users, this compliance burden, if managed manually, becomes a structural drag on the very thing that makes digital lending valuable: speed.


*“In consumer lending, the onboarding experience is the product. A borrower who abandons a KYC form mid-journey isn’t just a lost application; it’s a broken promise. And at scale, broken promises are existential.”*


##


About Pocketly


**Credit for Young India**


Launched as a challenger to slow, document-heavy personal lending, Pocketly gives India’s next generation of borrowers access to instant loans of up to ₹25,000 at live-approval speed, with direct bank transfer and zero paperwork. The platform spans salaried employees, self-employed professionals, and first-time credit seekers. It has disbursed over 1.25 crore loans worth more than ₹3,500 crore to a user base that now exceeds 10 million.


Backed by a $3 million funding round and ISO 27001:2023 certified, Pocketly competes on three things: minimum KYC, a 100% digital journey, and sub-7-minute approvals. Its product promise is fundamentally tied to its ability to onboard fast, which means compliance infrastructure that slows the journey is not merely a UX problem. It is a direct threat to disbursement volume.


##


The Problem


**Two Onboarding Gaps Causing a Leaky Funnel**


For a platform that competes on approval speed, friction in the KYC journey shows up immediately in drop-off metrics. Pocketly identified two compounding issues that are degrading both conversion rates and confidence in compliance.


**Document Upload Flows: Manual, Friction-Heavy, Incomplete**


First-time borrowers attempting to complete KYC through traditional document-upload flows faced too many steps — photograph submissions, OCR extraction, and manual-review queues. The result was high abandonment precisely when borrower intent was highest. Each drop-off represented both a lost loan and a potential compliance gap: an incomplete KYC record that couldn’t be used downstream.


**Identity Verification: No Authoritative Source, High Forgery Risk**


Without a direct connection to government-issued, digitally signed document repositories, Pocketly’s verification layer relied on user-submitted photographs and OCR, methods that are both slower and less reliable than source-fetched verification. Tampered documents and mismatched identities created fraud risk that scaled dangerously with loan volume.


##


The Solution


**KYC as a Background Process, Not a Barrier**


Pocketly integrated[Decentro’s DigiLocker Suite](https://decentro.tech/resources/digilocker-apis) , including the SSO DigiLocker APIs, to embed government-backed identity verification directly into its mobile onboarding flow, so borrowers never experience compliance as a delay.


**SSO DigiLocker Suite**


[Decentro’s SSO DigiLocker APIs](https://docs.decentro.tech/docs/kyc-and-onboarding-identities-digilocker-services) replaced a multi-step document upload flow with a single, consent-driven session that fetches government-verified identity documents directly from the UIDAI DigiLocker repository, no photographs, no uploads, no OCR errors.


How it works:


- Borrower signs up on Pocketly with their mobile number
- Decentro checks for an existing DigiLocker account, routing returning users to a **pinless sign-in** screen, and new users to a frictionless sign-up flow
- An SSO DigiLocker session is initiated; the borrower authenticates within the Pocketly app without being redirected away
- The platform fetches the borrower’s list of issued documents and retrieves their government-signed identity and address proof in a single step
- Explicit, purpose-bound consent is captured for every document fetch, satisfying RBI and DPDP requirements automatically
- Loan application proceeds. Funds are disbursed. Total time: under 7 minutes from sign-up


##


Business Stakes


**Why Real-Time KYC Is Non-Negotiable for Digital Lending**


Unlike a salaried bank loan, where days of processing are the norm and the borrower expects to wait, Pocketly’s customers are often in the middle of a financial need. A medical expense. A rent shortfall. An opportunity that won’t wait. A[KYC flow](https://decentro.tech/products/kyc-onboarding) that stalls mid-journey isn’t a UX inconvenience; it’s a broken product promise. Decentro’s integration addressed three specific commercial and regulatory risks simultaneously.


**Onboarding Abandonment.** Borrowers failing to complete KYC in a single session is a direct conversion loss that compounds with every additional step in the verification flow.


**Regulatory Non-Compliance.** Incomplete or manually verified KYC records create audit exposure for Pocketly and its NBFC lending partners, a risk that scales with loan volume and becomes increasingly costly as the book grows.


**Document Fraud.** Identity documents submitted via photograph or OCR carry forgery risk. Without source-fetched, digitally signed government documents, fraud scales with application volume and is difficult to detect in real time.


##


Results & Impact


Post-integration, Pocketly’s onboarding pipeline transformed from a document-upload bottleneck into a frictionless, fully automated compliance engine — without requiring borrowers to submit a single photograph or wait for manual review.


**Before Decentro** **After Decentro**


**KYC Method** Manual document uploads; OCR-based extraction Government-issued, digitally signed via DigiLocker


**Verification Speed** Multi-step; high abandonment mid-flow Real-time, single-session completion


**Identity Source** User-submitted photographs UIDAI-issued e-Aadhaar via DigiLocker


**Fraud Risk** OCR errors; tampered document exposure Eliminated; source-fetched, tamper-proof


**Compliance Trail** Manual; audit gaps Automated, consent-logged, audit-ready


**Time to Disbursement** Delayed by verification backlog Sub-7-minute approval enabled


Beyond the table, the integration delivered measurable operational impact:


- **3X** increase in KYC completion within a single borrower session
- **60%** reduction in onboarding abandonment directly attributed to verification friction
- **40%** growth in first-loan disbursements within 30 days of go-live


##


Voice of the Customer


*“At Pocketly, our promise to borrowers is simple: fast credit, zero hassle. Decentro’s DigiLocker SSO integration has been instrumental in delivering on that promise at scale. By replacing manual document uploads with a government-backed verification experience, we’ve been able to serve more borrowers faster while staying fully compliant. The pinless sign-in experience has been a meaningful unlock for our repeat user base — reducing drop-offs at what was previously a friction-heavy step. Decentro’s infrastructure handles the complexity so our team stays focused on what matters: building great products for Young India.”*


— **Keshav Nagpal, Software Engineer, Pocketly**


##


In Closing


**KYC Infrastructure Is a Growth Lever, Not Just a Cost Centre**


For Pocketly, automated identity verification isn’t optional. It is the product. By integrating Decentro’s DigiLocker SSO Suite, Pocketly converted its compliance layer from a friction point into a competitive advantage: borrowers onboard faster, approvals happen within the same session, and a consent-logged, audit-ready verification trail backs every loan disbursed.


This mirrors results Decentro has delivered across the fintech ecosystem — 200% growth in DigiLocker adoption at[Olyv,](https://decentro.tech/blog/olyv-case-study/) and measurable onboarding improvements across partners, including[SalarySe](https://decentro.tech/blog/salaryse-case-study/) , and others, reinforcing that smart, API-first KYC at the point of onboarding is among the highest-ROI infrastructure investments a digital lending platform can make.


Decentro’s verification infrastructure processes hundreds of thousands of validations daily, serving partners across lending, remittances, neo-banking, and investment platforms. The architecture scales with loan volume and adapts to evolving regulatory requirements, so compliance never becomes a ceiling on growth.


**Ready to streamline your onboarding?**


[Let’s Connect](https://decentro.tech/signup?)
