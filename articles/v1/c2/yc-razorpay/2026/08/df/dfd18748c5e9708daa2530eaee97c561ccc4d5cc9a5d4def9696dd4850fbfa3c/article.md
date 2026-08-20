---
schema_version: "1.0.0"
document_id: "dfd18748c5e9708daa2530eaee97c561ccc4d5cc9a5d4def9696dd4850fbfa3c"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/e-nach-upi-autopay-for-nbfcs-the-complete-collections-playbook-for-2026/"
published_at: "2026-08-04T12:04:48+00:00"
first_seen_at: "2026-08-04T17:41:11.570262+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:41be409b3aa1022000d2c6b24503db58b75451965147d3e5f4744cce7d8af6cb"
---

# e-NACH & UPI Autopay for NBFCs: The Complete Collections Playbook for 2026

Recurring EMI collection has quietly become one of the biggest determinants of NBFC profitability. As India’s lending stack digitises, the question is no longer whether to automate repayments but which rail to use, when, and how to stay compliant. This playbook is built for NBFC and fintech collections teams navigating e-NACH & UPI Autopay for NBFCs in a post-April-2026 regulatory environment. It covers rail selection, sponsor bank routing, non-revocable loan mandates, RBI compliance, and how to build a resilient hybrid collections stack that reduces failures and revenue leakage.


### Key Takeaways


- **Two rails, different strengths:** UPI Autopay suits speed and mobile-first, lower-ticket EMIs; e-NACH suits high-value, long-tenure loans with robust bank-validated mandates.
- **Limit snapshot:** Under the RBI e-mandate framework, recurring debits up to Rs 15,000 process without additional factor authentication (AFA), while amounts above require AFA validation; e-NACH supports higher-value mandates.
- **NBFC reality:** NBFCs cannot access NACH directly – e-NACH mandates must route through a sponsor bank that submits requests to NPCI.
- **Non-revocable mandates:** NPCI enabled non-revocable UPI Autopay mandates for loan repayment and EMI collection, preventing borrower-initiated app cancellation.
- **Hybrid wins:** Routing lower-ticket EMIs via UPI Autopay and higher-value loans via e-NACH within one platform reduces failures and operational complexity.
- **Compliance is non-negotiable:** The 2026 framework requires a pre-transaction notification at least 24 hours before every recurring debit.


## Why Recurring Collections Are Now a Strategic Priority for Indian NBFCs


The shift from paper mandates to digital rails has changed the economics of lending. For NBFC operations teams, collections efficiency now directly shapes portfolio quality and unit economics. Getting the rail strategy right is a competitive advantage, not a back-office chore.


### The Scale of the Problem – Manual EMI Collection Is Bleeding NBFCs Dry


Manual and cash-based repayment collection carries a heavy operational and financial cost. The stress is concentrated in specific pockets rather than spread evenly across the sector.


- Broad NBFC asset quality remains healthy, with GNPA improving to 4.2% as of June 2025.
- The small-ticket segment tells a sharper story: Portfolio at Risk (PAR 31-180 days) rose to 5.7% in June 2025 from 2.7% a year earlier.
- Cash-flow strain runs downstream, with an estimated Rs 8.1 lakh crore locked in delayed MSME payments.


Automated, non-revocable digital debits matter most in these stressed, small-ticket segments where first-attempt success drives recovery.


### The Digital Shift – NACH and UPI Growth Numbers That Demand Attention


The opportunity side is equally compelling, and the rails are now mature and mainstream.


- UPI accounts for around 49% of global real-time payment transaction volume, recognised as the world’s largest such system.
- UPI handled 84% of India’s retail payment volume in FY25, with value rising 30.3% to Rs 260.6 lakh crore.
- UPI Autopay processed more than 1.27 billion recurring mandates in November 2025.


The[broader digital payments ecosystem](https://razorpay.com/x) has scaled to a point where automated recurring collection is the default, not the exception.


## Understanding e-NACH: How It Works for NBFC Collections


e-NACH is the bank-validated backbone for high-volume recurring collections used widely across digital lending. The sections below define it, cover its authentication paths, and explain the sponsor bank model.


### What Is e-NACH and the e-Mandate?


- **e-NACH** is the fully digital version of the National Automated Clearing House mandate, letting borrowers authorise recurring bank-account debits without paper forms.
- The terms[e-NACH and e-mandate](https://razorpay.com/e-mandate/) are used interchangeably in lending operations.
- Unlike paper NACH, which requires physical form submission and manual processing, e-NACH activates electronically through digital authentication.
- e-NACH supports higher-value recurring debits under the NPCI e-mandate scope and limit framework, making it well-suited to larger EMIs.


### Did You Know?


Under NPCI’s NACH Procedural Guidelines, a dishonoured e-NACH debit is treated on par with a dishonoured cheque, giving lenders stronger legal recourse.


### The Three e-NACH Authentication Paths


Match the authentication path to your borrower segment to maximise activation.


#### API-Based (Netbanking or Debit Card)


- The borrower authenticates via netbanking credentials or debit card details.
- Fastest digital flow, ideal for tech-comfortable, urban borrowers.


#### Aadhaar-Based (eSign)


- The borrower authorises the mandate through an Aadhaar eSign flow.
- Extends reach into semi-urban and rural segments; typically priced higher than API-based flows.


#### Physical NACH


- The traditional paper-form fallback for borrowers without digital banking access.
- Slower activation, but preserves coverage across the widest borrower base.


Verifying borrower bank details upfront through[bank account verification](https://razorpay.com/x/bank-account-verification) reduces mandate rejections across all three paths.


### How NBFCs Route e-NACH in Practice – The Sponsor Bank Architecture


A critical implementation reality most guides skip: NBFCs cannot participate in NACH directly and must route through a sponsor bank.


1. The NBFC partners with a sponsor bank (directly or via a lending technology provider).
2. The NBFC initiates the borrower mandate through its digital flow.
3. The sponsor bank submits the mandate and debit requests to NPCI.
4. NPCI routes the request to the borrower’s destination bank.
5. Debit outcomes flow back to the NBFC via the sponsor bank.


> **Pro-Tip:** Ask payment technology partners which sponsor banks they integrate with. A broader network lifts mandate activation across more destination banks, especially in Tier 2 and Tier 3 markets.


### When to Choose e-NACH for NBFC Products


Default to e-NACH when your product profile includes:


- High-ticket EMIs beyond UPI Autopay’s AFA-free threshold.
- Long-tenure loans needing durable, bank-validated mandates.
- Borrower segments with low UPI familiarity.
- Cases where legal enforceability and institutional collections are a priority.


## How Razorpay’s Subscriptions Product Handles Both UPI Autopay and e-NACH Mandates in One Integration


Maintaining two separate mandate rails – one for UPI Autopay and another for e-NACH – forces collections teams to juggle two integrations, two reconciliation flows, and two sets of failure-handling logic. Razorpay’s Subscriptions product is designed to remove that operational split. Learn more about Razorpay’s digital lending stack.


- **Unified mandate management:** Create and manage both UPI Autopay and e-NACH mandates through a single API integration, so your team builds and maintains one flow instead of two.
- **Built-in retry logic:** Failed debits are automatically re-presented within NPCI-permitted windows, reducing the manual effort of chasing unsuccessful collections.
- **Real-time webhooks:** Mandate lifecycle events – creation, successful debit, failure, and cancellation – are pushed in real time, enabling downstream automation across your collections, accounting, and risk systems.


The result is one consolidated view of recurring collections across both rails, letting operations teams focus on strategy rather than plumbing.


## UPI Autopay for NBFCs: Mandate Limits, Flows, and Loan Use Cases


UPI Autopay is the mobile-first counterpart to e-NACH, built for speed and borrower convenience. The sections below cover how it works, its limits, and the lender-specific non-revocable feature.


### What Is UPI Autopay and How Does the Mandate Work?


- UPI Autopay lets borrowers authorise recurring debits directly inside their UPI app using their UPI PIN.
- Mandates can be **fixed** (same amount each cycle) or **variable** (up to a set cap).
- It supports frequencies from daily to yearly, plus an “as and when presented” option.
- Borrowers retain in-app visibility and control over their active mandates.


[Razorpay’s Subscriptions product](https://razorpay.com/subscriptions/) supports UPI Autopay mandate creation across major UPI apps, allowing NBFCs to initiate recurring EMI mandates through their borrower-facing digital flows.


### UPI Autopay Mandate Limits – What Rs 15,000 and Rs 1,00,000 Actually Mean


- The standard AFA-free debit limit is Rs 15,000 per transaction under the RBI e-mandate framework.
- For NPCI-approved categories such as credit card bills, mutual funds, and insurance, the per-transaction ceiling is Rs 1,00,000.
- Debits above the applicable threshold require the borrower to re-authenticate with AFA.
- For most small-ticket EMIs, this means fully AFA-free recurring collection.


### Did You Know?


NPCI raised the AFA-free UPI Autopay limit for credit card bills, mutual funds, and insurance from Rs 15,000 to Rs 1,00,000 per transaction.


### Non-Revocable UPI Autopay Mandates – The NBFC-Specific Feature Most Teams Don’t Know


This is one of the strongest differentiators available to lenders, and it is rarely explained.


- NPCI introduced non-revocable UPI Autopay mandates specifically for the loan repayment and EMI collection category.
- Under a standard mandate, borrowers can pause, modify, or revoke it directly in their UPI app.
- Non-revocable mandates restrict this in-app cancellation for regulated lenders, protecting the collection cycle from borrower-initiated disruption.
- This directly supports portfolio quality by keeping active EMI mandates in place through the loan tenure.
- Eligibility is tied to the regulated-lender loan repayment category, balancing lender protection with borrower rights.


> **Pro-Tip:** Explicitly verify with your payment partner whether their UPI Autopay API exposes the non-revocable mandate parameter, and confirm the activation flow before go-live.


### When UPI Autopay Is the Right Rail for NBFC Products


Choose[UPI Autopay](https://razorpay.com/upi-autopay/) when your product fits these profiles:


- EMIs at or below the Rs 15,000 AFA-free threshold.
- Short-tenure loans and consumer-durable finance.
- Mobile-first, digitally native borrower segments.
- Microfinance and embedded finance use cases where onboarding speed matters.


## e-NACH Registration Process for Lenders: A Step-by-Step Guide


Setting up an e-NACH mandate for EMI collection follows a clear four-step sequence. Remember that a pre-transaction notification at least 24 hours before each debit is mandatory throughout.


### Step 1: NBFC Onboarding and Sponsor Bank Linkage


Complete onboarding with your payment partner and establish the sponsor bank linkage that lets your mandates reach NPCI.


### Step 2: Initiating the Mandate for a Borrower


Trigger the mandate from your borrower flow, capturing loan amount, EMI value, frequency, and tenure, then route the borrower to their chosen authentication path.


### Step 3: Bank Validation and Activation


The destination bank validates the mandate details and account status. On approval, the mandate is activated and ready for recurring debits.


### Step 4: Running Recurring Debits and Failure Handling


Present debits on schedule after sending the pre-debit notification, and handle failures through permitted re-presentation and audit logging.


## Compliance Guardrails: RBI Rules Every NBFC Must Build Into Its Collections Stack


The RBI Digital Payments – E-mandate Framework, 2026, consolidated earlier rules into a single directive. Building these guardrails in from day one is non-negotiable.


### The 24-Hour Pre-Debit Notification Requirement


- The framework requires a pre-transaction notification at least 24 hours before each recurring debit.
- The notification must carry the merchant name, amount, debit date and time, mandate reference, reason for debit, and an opt-out option.


Razorpay’s recurring payments infrastructure includes configurable pre-debit notification flows, helping NBFCs support the RBI’s 24-hour pre-debit alert requirement without building notification logic in-house.


### Mandate Limits and AFA Thresholds – Stay Current


- Keep debit routing aligned to the Rs 15,000 AFA-free threshold and category exceptions.
- The first transaction under a mandate requires AFA; subsequent debits within the threshold do not.


### Failed Debits and Dispute Resolution


- Re-present failed debits within NPCI-permitted windows.
- Maintain audit-ready records of mandates, notifications, and debit outcomes for dispute readiness.


### Did You Know?


UPI accounts for around 49% of global real-time payment transaction volume, making India’s recurring payment rails among the most stress-tested worldwide.


## Designing a Hybrid Collections Strategy: Routing Logic for NBFCs


The winning approach is not choosing one rail but running both intelligently.


### Ticket-Size Routing Framework


Loan Profile Recommended Rail Why


EMI at or below Rs 15,000 UPI Autopay[AFA-free, mobile-first, fast activation](https://www.npci.org.in/product/autopay)


High-value or long-tenure EMI e-NACH[Durable, bank-validated, higher-value mandates](https://www.npci.org.in/PDF/nach/circular/2020-21/Circular_no_010-E-Mandate_scope_and_limit_enhancement.pdf)


> **Pro-Tip:** Route EMIs at or below Rs 15,000 through UPI Autopay for AFA-free debits; default higher-value or long-tenure loans to e-NACH for mandate robustness.


NBFCs integrating through Razorpay can manage both UPI Autopay and e-NACH mandates from a unified dashboard, giving collections teams a consolidated view of mandate status and debit schedules across both rails.


### Salary-Cycle Timing for First-Debit Success


> **Pro-Tip:** Avoid debit dates on the 25th to 31st; align to the 3rd to 7th alongside the 24-hour pre-debit notification to raise first-attempt success while staying fully compliant.


### Fallback Stack for Failed Collections


- Re-present within permitted windows before escalating.
- Trigger borrower reminders and offer an alternate rail where a mandate has lapsed.
- Log every attempt for reconciliation and dispute resolution.


## How Razorpay Powers Recurring Collections for NBFCs and Lending Businesses


Razorpay lets NBFCs manage both UPI Autopay and e-NACH mandates at scale from a single integration, so collections teams work from one consolidated system instead of stitching rails together.


Feature What It Does for the NBFC


Subscriptions (UPI Autopay + e-NACH) Runs both mandate rails through one integration


Unified mandate dashboard Consolidated view of mandate status and debit schedules


Retry logic for failed debits Re-presents failures within permitted windows


Configurable pre-debit notification flows Supports the 24-hour alert requirement


Real-time webhooks Powers downstream collections automation


RBI-authorised Payment Aggregator (licence received 2022) Regulated infrastructure for recurring collections


Developer-friendly APIs and SDKs Faster integration for engineering teams


[Explore Razorpay Subscriptions](https://razorpay.com/subscriptions/)


## Conclusion


For most NBFCs, the smartest collections strategy in 2026 is not picking a single rail but orchestrating both. Route lower-ticket, mobile-first EMIs through UPI Autopay for AFA-free convenience, and default high-value or long-tenure loans to e-NACH for mandate durability and legal recourse. Layer in sponsor bank routing, non-revocable loan mandates where eligible, and compliant pre-debit notifications, and you build a collections engine that reduces failures and protects portfolio quality. With the consolidated e-mandate framework now in force, getting this stack right is both a compliance necessity and a competitive edge.


## Frequently Asked Questions


**1. What is the difference between NACH, eNACH, and UPI Autopay for loan EMI collection?**
NACH is the bank-led recurring debit rail requiring paper mandates. e-NACH is its fully digital version, activated through electronic authentication and suited to higher-value EMIs. UPI Autopay is a UPI-app-based recurring debit mechanism ideal for smaller, mobile-first collections.


**2. How do I set up an eNACH mandate for EMI collections as an NBFC?**
NBFCs cannot access NACH directly. You link with a sponsor bank that submits mandates to NPCI, initiate the mandate from your borrower flow, let the destination bank validate and activate it, then run recurring debits with a 24-hour pre-debit notification each cycle.


**3. What is the UPI Autopay mandate limit for loan repayments?**
The standard AFA-free limit is Rs 15,000 per transaction. Select NPCI-approved categories carry a Rs 1,00,000 ceiling. Debits above the applicable threshold require borrower re-authentication.


**4. Can a borrower cancel their UPI Autopay mandate for a loan EMI?**
Under a standard mandate, borrowers can pause or revoke it in-app. However, NPCI enables non-revocable mandates for loan repayment and EMI collection, which restrict borrower-initiated app cancellation for regulated lenders.


**5. What happens if an eNACH or UPI Autopay debit fails?**
Failed debits can be re-presented within NPCI-permitted windows. NBFCs should trigger borrower reminders, maintain audit-ready records of each attempt, and use a fallback rail where a mandate has lapsed.


**6. Is pre-debit notification mandatory for NBFC recurring collections?**
Yes. The RBI e-mandate framework requires a pre-transaction notification at least 24 hours before every recurring debit, carrying details such as merchant name, amount, debit date and time, mandate reference, reason, and an opt-out option.
