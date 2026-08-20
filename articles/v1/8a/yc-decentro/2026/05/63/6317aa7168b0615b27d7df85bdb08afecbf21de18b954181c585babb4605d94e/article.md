---
schema_version: "1.0.0"
document_id: "6317aa7168b0615b27d7df85bdb08afecbf21de18b954181c585babb4605d94e"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-rss-e6182ea9811a"
canonical_url: "https://decentro.tech/blog/zypp-case-study/"
published_at: "2026-05-15T06:53:15+00:00"
first_seen_at: "2026-07-20T23:23:43.928142+00:00"
fetched_at: "2026-07-28T21:15:16.761066+00:00"
content_hash: "sha256:4ee4ad99378679cc2bf0bad9e6c0d962959d633a05af13dc3754c64bdfcc6611"
---

# How Zypp Electric Built a 3-Minute Rider Onboarding Flow

Table of Contents


*India’s leading EV rental platform replaced a fragmented rider verification process with Decentro’s verification stack, turning onboarding from a multi-day friction point into a same-session experience — so riders go from sign-up to first delivery without waiting.*


##


The EV Delivery Opportunity – and its Onboarding Challenge


India’s last-mile delivery market is undergoing a fundamental shift. As platforms like Zomato, Swiggy, Zepto, and Uber scale their delivery networks, the pressure to onboard and activate gig riders faster has never been greater. Electric vehicles are central to this shift — cheaper to run, cleaner to operate, and increasingly the preferred asset for delivery fleets chasing sustainability targets.


Zypp Electric sits at the intersection of these two forces. As India’s No.1 EV rental platform, Zypp deploys electric two- and three-wheelers to last-mile delivery partners, giving riders access to EVs without upfront ownership costs while giving enterprise clients a reliable, emission-free delivery fleet.


But scaling a rider network at this speed comes with a compliance reality that can’t be shortcut. Every rider must be KYC-verified before activation. Every payout must go to a validated, name-matched bank account. And with 20,000+ active riders across multiple cities, the cost of a slow or broken onboarding flow isn’t abstract — it’s measured in unactivated riders, delayed deliveries, and misdirected payouts.


##


About Zypp Electric


[Zypp Electric](https://zypp.app/) was founded on a simple premise: make electric mobility accessible to India’s gig economy. By offering EVs on a rental model — with swappable battery scooters, IoT-enabled fleet management, and AI-powered maintenance — Zypp has eliminated the barriers that kept delivery riders on petrol bikes.


Today, Zypp operates with:


- 20,000+ active riders
- 110 million+ deliveries enabled
- 2 lakh+ lives impacted
- 55 million+ kg of CO₂ reduced


Its enterprise client roster includes Zomato, Swiggy, Zepto, Uber, and Rapido. The platform’s promise to these clients is capacity and reliability — and that promise begins the moment a new rider enters the onboarding funnel.


##


The Problem


###


Two Verification Gaps Stalling Rider Activation


For a platform that competes on fleet scale and delivery capacity, every day a rider spends in a pending verification queue is a day of lost earnings for that rider — and lost throughput for Zypp’s enterprise clients. Two structural gaps were creating exactly this problem.


**Identity Verification: Fragmented, Slow, and Drop-Off Prone**


Riders arrive with varying levels of digital readiness. Some have Aadhaar-linked DigiLocker accounts; others carry physical documents they need to upload. Without a unified, automated verification layer, Zypp relied on semi-manual document checks that took hours to days to complete. Riders who dropped off mid-onboarding — frustrated by upload loops or waiting periods — represented lost supply that Zypp could not afford at scale.


**Bank Account Validation: A Payout Timebomb**


Zypp processes payouts to thousands of riders on a regular cycle. Without automated bank account validation at the point of onboarding, the platform faced a persistent risk: funds dispatched to closed, invalid, or mismatched accounts. Every such incident meant manual reconciliation, delayed rider earnings, and an erosion of trust at exactly the moment it matters most — when a rider receives their first payment.


##


The Solution


###


Verification as a Background Process


Zypp integrated four APIs from Decentro’s verification platform, assembling a complete KYC and payout-readiness flow that runs entirely in the background — invisible to the rider, compliant by design.


**DigiLocker API**


For riders with Aadhaar-linked DigiLocker accounts,[Decentro’s DigiLocker API](https://decentro.tech/resources/digilocker-apis) enables consent-gated retrieval of government-verified documents — Aadhaar, driving licence, and RC — without any manual upload. The document data is authoritative, tamper-proof, and returned in real time.


**How it works:**


- Rider authenticates via DigiLocker using their Aadhaar-linked credentials and OTP
- Consent granted — Zypp fetches Aadhaar, driving licence, and RC directly from DigiLocker
- Government-verified data is stored and passed downstream for cross-validation — no OCR ambiguity, no fake document risk


**OCR API**


For riders who upload physical document images, Decentro’s OCR API extracts structured fields — name, date of birth, document number, address — from Aadhaar cards, PAN cards, and driving licences with high accuracy, converting unstructured image uploads into validated, machine-readable data.


**How it works:**


- Rider uploads a document image via the Zypp app
- [OCR API](https://decentro.tech/api-hub/ocr-api) processes the image and extracts key fields: name, ID number, DOB, address
- Extracted data is passed downstream for PAN validation and identity cross-matching


**PAN Verification API**


Before any rider account is activated,[PAN is validated](https://decentro.tech/resources/pan-verification-api) against NSDL records in real time — confirming the rider’s identity, name, and tax status within seconds of entry. Invalid or mismatched PANs are flagged immediately, blocking downstream risk before it begins.


**How it works:**


- Rider’s PAN — entered manually or auto-populated via OCR — is submitted to the API
- API validates PAN details against NSDL in real time; no batch processing, no overnight queue
- Validated cardholder name and status are returned to the platform instantly
- Invalid or non-compliant PANs are flagged; account activation is blocked until resolved


**Bank Account Validation API**


Before any payout is initiated, Decentro’s[Bank Account Validation API](https://decentro.tech/resources/bank-account-validation-api) confirms that the rider’s bank account is active, valid, and name-matched against their verified KYC identity. This single check eliminates the risk of misdirected transfers.


##


Why this Matters for an EV Rental Platform


Unlike a lending or investment platform, where a few hours of verification delay is tolerable, Zypp’s product promise is tied to the speed of rider activation. Enterprise clients need delivery capacity on demand. Riders need income from day one. A rider who drops off during onboarding doesn’t come back — and a payout that bounces or misdirects on the first cycle destroys the trust that brings riders back for the second week.


Decentro’s APIs addressed three specific risks simultaneously:


- **Onboarding drop-off** : Riders failing to complete verification before frustration sets in is a direct supply loss — worsening with every additional manual step in the flow.
- **Identity fraud** : Unverified or falsified documents entering the rider network create liability for Zypp and safety risks for its enterprise clients’ end customers.
- **Payout failure** : Funds dispatched to invalid or mismatched accounts create reconciliation burden, delayed rider earnings, and reputational damage that compounds at scale.


##


Results and Impact


Post-integration, Zypp’s onboarding pipeline transformed from a fragmented, multi-step compliance process into a fully automated, same-session experience — eliminating manual document review and the payout reconciliation burden that came with unvalidated bank accounts.


**Metric** **Before Decentro** **After Decentro**


**Identity verification** Manual checks; hours to days Real-time via DigiLocker + OCR


**PAN validation speed** Manual; delayed, error-prone Under 5 seconds, fully automated


**Bank account validation** Offline / semi-manual Instant, API-driven, name-matched


**Onboarding drop-off** High; friction at every step Significantly reduced; same-session completion


**Average onboarding time** ~30 minutes (manual process) Maximum 3 minutes post-integration


**Payout misdirection risk** Present; manual reconciliation needed Eliminated; pre-validated accounts only


**Compliance readiness** Manual audit trail gaps Automated, audit-ready verification at every step


Beyond the table, the integration has delivered measurable operational impact since go-live:


**Metric** **Validated Number**


DigiLocker document verifications **19,227**


PAN verifications **87,289**


Bank account validations **98,815**


**Process Time Improvement:**


- ***Before integration (manual process): ~30 minutes per rider***
- ***After integration: Maximum 3 minutes per rider***


##


Voice of the Customer


> *“For a platform that activates riders for enterprise clients, onboarding speed is a direct business metric. Decentro’s verification stack lets us run a complete, compliant KYC flow in the background — riders go from sign-up to activated in a single session, and every payout goes to a verified, name-matched account from day one.”*
>
>
> **Rahul Yadav, SVP Technology, Zypp Electric**


##


In Closing


###


Compliance Infrastructure Is a Rider Experience Problem


For Zypp Electric, automated KYC isn’t a regulatory box to tick — it’s the foundation of the rider relationship. By integrating Decentro’s DigiLocker, OCR, PAN Verification, and Bank Account Validation APIs, Zypp converted its compliance layer from a drop-off point into a competitive advantage: riders onboard faster, payouts go to the right accounts from day one, and an audit-ready verification trail scales with the fleet.


This mirrors results Decentro has delivered across the ecosystem — 200%+ growth in DigiLocker adoption at Olyv, real-time KYC at[BookMyForex](https://decentro.tech/blog/bookmyforex-case-study/) , cutting verification to under 5 seconds, and verification infrastructure powering platforms from[BimpaPay](https://decentro.tech/blog/bimapay-case-study/) to[Volopay](https://decentro.tech/blog/volopay-case-study-expense-management/) — reinforcing that API-first verification at the point of onboarding is among the highest-ROI infrastructure decisions a growth-stage platform can make.


Decentro’s[verification infrastructure](https://decentro.tech/products/kyc-onboarding) processes hundreds of thousands of validations daily, serving partners across lending, remittances, gig economy platforms, and neo-banking. The architecture scales with transaction and rider volume, and adapts to evolving regulatory requirements — so compliance never becomes a ceiling on growth.


**Ready to automate your rider or customer KYC?**


[Let’s Connect](https://decentro.tech/signup?)
