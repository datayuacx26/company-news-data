---
schema_version: "1.0.0"
document_id: "dffd5f4327effe4b5fe1f64bdbfa8405070ad67ec360b5aaf417e81cc7fcafe9"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-rss-e6182ea9811a"
canonical_url: "https://decentro.tech/blog/bookmyforex-case-study/"
published_at: "2026-05-05T08:53:29+00:00"
first_seen_at: "2026-07-20T23:23:43.928142+00:00"
fetched_at: "2026-07-28T21:14:22.706316+00:00"
content_hash: "sha256:4f01bb06714041004d3e62234cb8e98a479d9597578662b79964a0617234327e"
---

# How BookMyForex Cut KYC Time to 5 Seconds with Decentro

Table of Contents


India’s largest online forex marketplace eliminated verification bottlenecks with Decentro’s verification stack, turning a multi-day compliance process into a background check that completes in seconds.


##


The Forex Opportunity — And Its Compliance Burden


India’s outward remittance and retail forex market have grown sharply over the past decade, fuelled by rising international travel, overseas education, and global commerce. The[RBI’s Liberalised Remittance Scheme (LRS)](https://www.rbi.org.in/commonperson/english/scripts/FAQs.aspx?Id=1834) allows individuals to remit up to USD 250,000 per financial year, and digital-first platforms have made this easier than ever, collapsing what was once a branch-banking experience into a few taps on a smartphone.


But opportunity has a shadow. Every cross-border transaction sits squarely within FEMA’s regulatory perimeter. That means mandatory PAN verification for transactions above threshold limits, rigorous checks on beneficiary accounts, and an audit trail that regulators can inspect at will. For platforms operating at scale, this compliance burden — if managed manually — becomes a structural drag on growth.


*“In forex, time is literally money. A rate lock that expires while a customer is still navigating a KYC form isn’t a UX problem — it’s a direct revenue loss event.”*


##


About BookMyForex


###


India’s Largest Online Forex Marketplace


Launched as a disruptor to opaque, branch-dependent currency exchange,[BookMyForex](https://www.bookmyforex.com/) offers customers live interbank rates on foreign currency purchases, forex card loads, and international money transfers. The platform operates across 65+ cities with doorstep delivery, has processed over USD 1.5 billion in transactions, and has served 6,50,000+ customers through a network of 5,000+ partner banks and money changers.


Competing on rate transparency and speed, BookMyForex’s product promise is fundamentally linked to its ability to move fast, which means compliance infrastructure that slows onboarding is not merely an inconvenience; it is an existential risk to conversion.


##


The Problem


###


Two Verification Gaps Causing a Leaky Funnel


For a platform that competes on the speed of rate locks, any friction in the compliance journey is immediately visible in drop-off metrics. BookMyForex identified two critical verification bottlenecks that were degrading both conversion rates and regulatory standing.


**PAN Validation: Manual, Delayed, Risky**


Every forex transaction mandates PAN verification under FEMA. Without API-based automation, BookMyForex relied on multi-step, manually intensive checks that took longer time to complete. Customers attempting to lock in live exchange rates abandoned mid-journey when verification stalled, and each drop-off represented both lost revenue and potential FEMA exposure.


**Bank Account Validation: Error-Prone, Offline Checks**


Before processing Forex orders or issuing refunds, BookMyForex needed to confirm that every customer’s registered bank account was active, valid, and matched the account holder’s name. Offline or semi-manual processes made this check both time-consuming and error-prone, creating fraud risk and operational overhead even painful for the customers to share bank statements


##


The Solution


###


Verification as a Background Process, Not a Barrier


BookMyForex integrated two of Decentro’s verification APIs to address both friction points end-to-end, embedding compliance checks directly into the onboarding flow so customers never experience them as delays.


**PAN Verification API**


Decentro’s[PAN Verification API](https://decentro.tech/resources/pan-verification-api) replaced a multi-step manual check with instant, real-time validation, surfacing the cardholder’s identity, name, and tax status within seconds of entry. FEMA compliance is satisfied automatically, without any human-in-the-loop delay.


**How it works:**


1. Customer enters PAN number at the onboarding stage
2. API verifies PAN details in real time — no batch processing, no overnight queue
3. Validated cardholder name and status are returned instantly to the platform
4. Invalid or non-compliant PANs are flagged immediately, blocking downstream fraud before it begins


**Bank Account Validation API**


Decentro’s Bank Account Validation API confirmed the legitimacy of customer bank accounts before any remittance or refund was processed. This[verification mode](https://decentro.tech/resources/penny-drop-verification-api) enabled BookMyForex to match the method to the context, ensuring accuracy without unnecessary transaction friction.


**How it works** :


1. Customer enters bank account number and IFSC code
2. API triggers a micro-transaction
3. Verified account status returned in real time
4. Invalid or closed accounts are flagged before any remittance is initiated, eliminating misdirected fund risk


##


Business Stakes


###


Why Real-Time Verification Is Non-Negotiable for Forex


Unlike a lending application, where a few hours of processing delay is tolerable, forex customers are racing against a live market. Exchange rates fluctuate minute-to-minute. A rate lock that expires while KYC is pending isn’t a UX inconvenience; it’s a broken product promise. Decentro’s APIs addressed three specific commercial and regulatory risks simultaneously.


**Rate Lock Abandonment:** Customers failing to complete onboarding before their rate lock window expires is a direct conversion loss that worsens with every additional second of verification lag.


**Regulatory Non-Compliance** Unverified PAN records triggering FEMA violation, potentially resulting in transaction reversals, regulatory scrutiny, and reputational damage for the platform.


**Fraudulent Remittances** Funds dispatched to invalid, closed, or mismatched bank accounts, a financial and operational liability that scales dangerously with transaction volume.


##


Results & Impact


Post-integration, BookMyForex’s onboarding pipeline transformed from a multi-step compliance bottleneck into a frictionless, fully automated engine, eliminating the need for customers to upload additional documents or wait for manual review.


**Metric** **Before Decentro** **After Decentro**


PAN Verification Speed Manual; hours to days per check Real-time; **under 5 seconds**


Bank Account Validation Offline / semi-manual; error-prone Instant, API-driven; **three flexible modes**


Onboarding Drop-off High; verification delays are driving abandonment Significantly reduced; same-session completion


Compliance Risk Manual errors are creating audit gaps Automated, audit-ready verification trail


Time-to-First-Transaction Delayed by verification backlog Same-session transaction enabled


Lifetime API Hits (since 2024) **172,500+ across KYC, M2A & Data Pull**


Beyond the table, the integration has delivered measurable operational impact since go-live in 2024:


- **88,000+ KYC/Validate hits** — PAN verification now completes as a background check, invisible to the customer, compliant by design
- **72,000+ Mobile-to-Account validations** — every remittance and refund backed by a verified, name-matched bank account
- **12,500+ Data Pull calls** — enriched identity data supporting audit trails and downstream compliance workflows


##


**Voice of the Customer**


*“Forex is a time-sensitive product, our customers are locking in live exchange rates and expect a seamless experience from onboarding to transaction. Decentro’s PAN and Bank Account Verification APIs have been game-changers in making our onboarding compliance-ready without adding friction. What used to be a bottleneck is now a background check that completes in seconds.”*


– *Manik Varshney, Product, Processes & Regulatory Compliance*


##


In Closing


###


Compliance Infrastructure Is a Growth Lever, Not Just a Cost Centre


For BookMyForex, automated verification isn’t optional. It is the product. By integrating Decentro’s PAN Verification and Penny Drop APIs, BookMyForex converted its compliance layer from a friction point into a competitive advantage: customers onboard faster, rates are locked without interruption, and an audit-ready verification trail backs every transaction.


This mirrors results Decentro has delivered across the fintech ecosystem: 200% growth in[DigiLocker](https://decentro.tech/resources/digilocker-apis) adoption at[Olyv](https://decentro.tech/blog/olyv-case-study/) and several other key players, including[BimpaPay](https://decentro.tech/blog/bimapay-case-study/) ,[Volopay](https://decentro.tech/blog/volopay-case-study-expense-management/) , and more, reinforcing that smart, API-first verification at the point of onboarding is among the highest-ROI infrastructure investments a fintech platform can make.


Decentro’s verification infrastructure processes hundreds of thousands of validations daily, serving partners across lending, remittances, neo-banking, and investment platforms. The architecture scales with transaction volume and adapts to evolving regulatory requirements, so compliance never becomes a ceiling on growth.


Ready to streamline your onboarding?


[Let’s Connect](https://decentro.tech/signup?)
