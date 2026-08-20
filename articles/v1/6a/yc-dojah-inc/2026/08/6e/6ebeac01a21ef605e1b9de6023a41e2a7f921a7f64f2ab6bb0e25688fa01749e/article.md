---
schema_version: "1.0.0"
document_id: "6ebeac01a21ef605e1b9de6023a41e2a7f921a7f64f2ab6bb0e25688fa01749e"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/choosing-kyc-verification-flow"
published_at: "2026-08-11T06:10:36.721+00:00"
first_seen_at: "2026-08-11T16:33:26.783908+00:00"
fetched_at: "2026-08-11T16:33:27.669376+00:00"
content_hash: "sha256:378f0c51877411a27777ffd7ba855dc0f585db778e99eb421d87e4f25efceb66"
---

# How to Choose the Right KYC Verification Flow for Your Platform

Most teams pick a KYC flow based on what their developer can build fastest or what the vendor recommends by default. The ones that get it right pick based on their user base, their risk profile, their market, and the experience they want to deliver. Getting any one of those wrong means the flow will either block legitimate users, miss fraud, or fail a regulatory examination.


You have chosen your identity verification provider. The next decision is which KYC verification flow actually fits your platform. That choice affects conversion, compliance, fraud exposure, and how your users experience onboarding from day one.


This guide walks through each of those decisions in order. By the end, you will know exactly which KYC verification flow fits your use case and how to get it running on your platform.


### **What a KYC Verification Flow Actually Is**


A KYC verification flow is the sequence of checks a user goes through to confirm their identity on your platform. Once you have selected your provider, figuring out which flow works for your onboarding is the next critical step.


The checks you run, the order you run them in, and how you combine them determine three things: what fraud you catch, what compliance standard you meet, and how many legitimate users complete the process.


- **The sequence matters as much as the checks:** Running a database lookup before a liveness check is a different experience from running them in the opposite order. A well-sequenced flow catches failures early, before asking the user to do more work, and returns the highest-confidence result at the end.


- **The combination of checks determines what you catch:** A database lookup confirms an ID exists in a government database. A liveness check confirms a real person is present. A face match confirms that person is the cardholder. A document check catches forgeries. No single check does all four jobs. The flow you choose determines which fraud vectors you are protected against and which ones you are not.


- **The flow is also where you lose users:** Every additional step is a point where some users will drop off. For a fintech onboarding users across Africa, one extra screen at the wrong point and completion rates drop noticeably. The right flow is the one that finds the right balance for your specific situation, not the most complete flow by default.


With the concept clear, the next step is understanding which flow types are available and what each one is suited for.


### **The Main KYC Flow Types and What Each One Is For**


There are four main flow types, each suited to a specific combination of risk level, user base, and market.


- **Database lookup only:** The user submits an ID number, the platform queries the relevant government database, and the result confirms whether the ID is valid and the details match. This is well-suited to platforms where ID coverage is high and the risk level is low enough that a database match alone satisfies the compliance standard.


- **Document verification only:** The user uploads a photo of their ID document. The platform extracts the data and runs authenticity checks to confirm the document is genuine. Useful where database connectivity is limited or the document types in use are not covered by a direct database connection. Catches forgeries but does not confirm the person presenting the document is the cardholder.


- **Biometric only:** The user takes a selfie and completes a liveness check. The platform confirms a real person is present and, where a reference photo is available, matches the face to the cardholder. Fast and low-friction, but only works as a standalone check if something else has already established which identity the biometric is being matched against.


- **Layered flow:** A database lookup plus liveness plus face match, with a document check added for higher-risk cases. This is the standard for regulated financial products across most African markets. It catches the widest range of fraud and meets the most demanding compliance requirements.


Knowing which flows exist is only useful once you know which one matches your specific use-case.


### **Matching Your KYC Flow to Your User Base**


The right flow for a fintech onboarding users in Lagos is not the right flow for a gig platform onboarding rural delivery riders across multiple states. Your user base determines what checks are feasible and where drop-off risk sits.


**Document quality and availability:** Users with recently issued national ID cards or international passports are well-suited to document verification flows. Users presenting older voter cards, worn IDs, or documents with inconsistent print quality will produce higher error rates on OCR-heavy flows. If a significant portion of your user base falls into the second category, a database lookup flow with a lighter document requirement will produce better completion rates.


**Device capability and connectivity:** Liveness detection and face match require a working front-facing camera and enough bandwidth to upload a photo. For platforms onboarding users on entry-level smartphones or in areas with variable connectivity, a flow that depends on a high-quality selfie will produce more failures than one that relies primarily on data submission. Match the flow to your actual user base, not the ideal case.


**Familiarity with digital verification:** A user who has completed a fintech onboarding flow before will move through a multi-step verification with less friction than a first-time user who has never been asked to take a selfie during a financial transaction. For platforms targeting less digitally familiar segments, simpler flows with clear step-by-step guidance outperform more sophisticated flows that assume prior experience.


Once you have mapped your user base, the next question is what type of platform you are running and what that means for how deeply you need to verify.


### **Matching Your KYC Flow to Your Platform Type**


Your platform type shapes both who your users are and what risk your product carries. A lending platform and a logistics platform may both operate in Nigeria and onboard similar users, but they need very different verification flows.


**Fintech and digital financial platforms:** If you run a regulated fintech offering wallets, savings, or payments, you need at minimum a database lookup plus liveness plus face match. The CBN sets BVN or NIN verification as the baseline for most financial products, and regulated products almost always need the layered flow. If you are expanding into multiple African markets, your flow also needs to cover the ID rails in each market, not just Nigeria.


**Lending and credit platforms:** Lending carries higher fraud risk because the incentive for identity fraud is direct. A borrower who passes a light check using a farmed or borrowed identity creates a loss the moment funds are disbursed. Lending platforms need the fullest version of the layered flow: database lookup, liveness, face match, and often document verification on top.


**Logistics and gig economy platforms:** If you onboard delivery riders, drivers, or independent contractors, the primary concern is confirming the person working under an identity is the person the identity belongs to, not financial fraud risk. A database lookup plus face match is usually sufficient, but the flow needs to be mobile-first and fast.


[CheckIn by Dojah](https://checkin.dojah.io/) is built specifically for this use case with no developer or technical setup required.


**Microfinance institutions and agent networks:** MFBs and agent banking platforms often onboard users who are less digitally familiar and may present older or lower-quality ID documents. A database lookup plus a simple selfie check often produces better completion rates than a full document verification flow.


**Quick reference:**


**Platform type**


**Recommended flow**


**Key consideration**


Regulated fintech


Layered (database + liveness + face match)


Must meet CBN BVN/NIN baseline


Lending and credit


Full layered + document verification


Fraud cost is a defaulted loan


Logistics and gig


Database lookup + face match


Use CheckIn for no-code, mobile-first verification


MFBs and agent networks


Database lookup + simple selfie


EasyOnboard supports API, SDK, or no-code link


Once you know your platform type and user base, the next variable is how much risk your product actually carries.


### **Matching Your KYC Flow to Your Risk Profile**


A savings product with a low transaction limit carries a different fraud and compliance risk than a lending product or a crypto exchange.


**Define your risk tiers before choosing a flow:** Most platforms have more than one user or product type, each carrying different risk. A tiered approach applies a lighter flow to lower-risk users and a deeper flow to higher-risk ones. This is the risk-based approach that regulators like the CBN explicitly expect.


Defining those tiers before integration starts prevents the most common mistake: applying the same verification depth to every user regardless of their actual risk.


**Higher-risk products need the layered flow:** If you run a lending platform, a crypto exchange, or a high-value payment product, a database lookup alone is not enough. These use cases need, at minimum, a database lookup plus liveness plus face match, and often document verification on top. The regulatory standard in most African markets requires it for these product types.


**Lower-risk products can use lighter flows:** If your platform offers micro-savings, small-limit wallets, or low-value payments, you do not need to put every user through a full biometric flow. A database lookup that confirms the ID is real and the details match may satisfy both the compliance requirement and your onboarding goals.


Risk profile tells you how deep to go. The market you are operating in tells you which checks are available to run.


### **Matching Your KYC Flow to Your Market**


Different African markets have different database coverage and regulatory requirements. A flow that works in Nigeria may not be available, sufficient, or compliant in Ghana, Kenya, or South Africa without adjustment.


**Which ID types are accepted and available:** Nigeria's primary verification rails are


[BVN and NIN](https://www.cbn.gov.ng/) , with support for voter cards, driver's licences, and passports.


[Ghana's Bank of Ghana directive](https://www.bog.gov.gh/wp-content/uploads/2026/01/SUPERVISORY-GUIDANCE-NOTE-2025.pdf) now requires the Ghana Card as the only accepted ID for financial onboarding.


The documents your flow accepts need to match what is required in each market.


**Database coverage and reliability by market:** Not all government databases across African markets have the same coverage or uptime. A database lookup flow that relies on a government database with incomplete records or frequent downtime will produce more false fails in that market. Understand the actual coverage and reliability of the databases behind each check before going database-heavy in a specific market.


**Regulatory requirements that set the minimum flow:** Nigeria's CBN framework requires BVN or NIN verification for most financial products. Ghana's BoG requires Ghana Card verification with biometric confirmation. These requirements set the minimum flow for your market.


With user base, platform type, risk profile, and market mapped, the final decision is how you integrate the flow into your platform.


### **SDK, API, or EasyOnboard: Choosing How to Integrate**


The integration method you choose affects how much control you have over the user experience, how quickly you can go live, and how much engineering effort the build requires.


Here’s how it works:


**SDK: go live faster, less control:** An SDK is a pre-built verification interface that drops into your app or web flow. It handles the capture, the API calls, and the result logic out of the box. You go live faster, but you have less say over how the verification screens look and behave. Good starting point if you need to move quickly or have limited frontend capacity. Dojah provides SDKs for


[React, Flutter, Swift, Kotlin, React Native, and JavaScript](https://docs.dojah.io/sdks/overviews) .


**API: full control, more build time:** A direct API integration means you build the UI, handle the calls, and design the result routing yourself. You get a seamless, brand-consistent experience, but it takes more engineering time and requires careful handling of edge cases before go-live.


**EasyOnboard: the middle ground:**


[EasyOnboard](https://docs.dojah.io/overview/identity-hub/easy-onboard) is a no-code KYC onboarding builder that lets you configure verification steps, set fraud rules, and deploy via a shareable link, SDK, or API without building the full interface from scratch. For most African fintech use cases, a well-configured EasyOnboard flow gets you most of the benefit of a full API integration at a fraction of the build time.


**Quick comparison:**


**Integration method**


**Speed to go live**


**Control over UX**


**Engineering effort**


SDK


Fastest


Low


Minimal


EasyOnboard


Fast


Medium


Low


API


Slower


Full


High


The integration method is the last decision before go-live. The flow itself is what Dojah's infrastructure is built to support across all three.


### **How Dojah Supports the Right KYC Verification Flow for Your Platform**


Every decision this guide has covered points to the same need: a verification infrastructure flexible enough to match the flow to your platform. Dojah covers each integration path and flow configuration from one platform.


- **Government Data Lookup for direct database checks:** If you are building via API and need to verify identities directly against government databases,


[Dojah's Government Data Lookup](https://docs.dojah.io/overview/identity-hub/government-lookup) connects to BVN, NIN, Ghana Card, Kenyan National ID, South African ID, and more across African markets. Expanding into a new market does not mean rebuilding the integration from scratch.


- **EasyOnboard for configurable, no-code flows.** If your team needs to go live quickly,


[EasyOnboard](https://docs.dojah.io/overview/identity-hub/easy-onboard) lets you configure verification steps, set fraud rules, and deploy via a shareable link, SDK, or API without building a full flow from scratch. It supports layered flows combining government data, liveness, face match, and document verification in any sequence you need.


- **SDKs for native mobile and web integration.** If you are building a native mobile or web app and want full control over the verification experience, Dojah provides SDKs for


[React, Flutter, Swift, Kotlin, React Native, and JavaScript](https://docs.dojah.io/sdks/overviews) so you can embed verification flows directly without depending on a pre-built interface.


- **Risk-based flow configuration across all three paths.** Whether you use the API, EasyOnboard, or an SDK, Dojah supports tiered verification flows that apply different check depths to different user or product risk tiers, matching the risk-based approach the CBN and other African regulators expect.


If you are a founder, product lead, or technical team ready to configure the right KYC flow for your platform,


[explore the Dojah docs](https://docs.dojah.io/) to see which integration path fits your stack, or


[book a demo](https://dojah.io/contact-sales) to see the flows in action.


**FAQs**


**1. What is the difference between a database lookup and a document verification flow?** A database lookup submits an ID number to a government database and confirms whether it is valid. Document verification checks a photo of the ID for authenticity. A database lookup is faster where coverage is strong. Document verification is useful where database connectivity is limited or the ID type is not supported by a direct connection.


**2. Do I need a layered flow for regulatory compliance in Nigeria?** For most regulated financial products, yes. The CBN requires BVN or NIN verification as the baseline, and most product types also require liveness and face match. Lower-risk products may satisfy compliance with a lighter flow, but the choice needs to be documented against a risk assessment.


**3. Can I use different flows for different users on the same platform?** Yes. A tiered approach applies lighter checks to lower-risk users and fuller checks to higher-risk ones. This is the risk-based approach regulators expect, and it improves completion rates for the majority of users without reducing the compliance standard for higher-risk cases.


**4. What integration method suits a platform without a large engineering team?** EasyOnboard is usually the right starting point. It is a no-code builder that lets you configure which checks run and in what order, deploy via a shareable link or SDK, and go live faster than a full API build.


**5. How do I choose the right flow when expanding into multiple African markets?** The flow needs to cover the ID types required in each market, meet the minimum regulatory standard per jurisdiction, and account for database coverage differences. A single integration covering multiple African ID rails is more efficient than building separate flows per market.
