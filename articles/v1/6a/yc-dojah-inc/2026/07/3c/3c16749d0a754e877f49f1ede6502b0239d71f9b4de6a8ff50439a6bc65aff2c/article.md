---
schema_version: "1.0.0"
document_id: "3c16749d0a754e877f49f1ede6502b0239d71f9b4de6a8ff50439a6bc65aff2c"
company_key: "yc-dojah-inc"
company: "Dojah Inc"
source_id: "yc-dojah-inc-news-import-dac996b1a8e0"
canonical_url: "https://dojah.io/blog/kyc-farming-africa"
published_at: "2026-07-21T13:56:44.476+00:00"
first_seen_at: "2026-07-25T01:49:05.386345+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:da281f4c257e9a4d1d9257d4b78325da041debba05b10e1d6e20e42e36e3edf5"
---

# KYC Farming and Identity Recycling: What African Fintechs Need to Know

Someone signs up for what they believe is a legitimate grant programme or SIM registration exercise and willingly submits their identity documents. Months later, that same verified identity is used to open accounts across multiple fintech platforms without their knowledge. Every verification check passes because the identity belongs to a real person.


This is KYC farming, and it exploits a gap most fintechs do not close at onboarding. Verifying that an identity is genuine is not the same as knowing whether that identity has already been harvested and reused elsewhere.


This article explains how KYC farming and identity recycling work, why standard verification misses them, and what African fintechs can do to detect them earlier.


### **What KYC Farming Actually Is**


KYC farming is the organised collection of real, verified identities for reuse across multiple platforms. Because the identity is genuine, standard verification passes.


The fraud starts with how those identities are collected:


- **Fake grant and registration schemes:** Fraud rings often present identity collection as a legitimate opportunity. The schemes may appear as grant applications, SIM registration campaigns, or loan offers. People submit genuine identity documents and complete verification. Many never realise their verified identities may later be reused without their knowledge.


- **Underground identity markets:** Once collected, those identities enter underground markets. Fraud rings trade them across multiple platforms, complete with ID scans, selfies, and liveness captures. In the


[Dojah Fraud Insights Report 2025](https://dojah.io/fraud-insights-2025) , Yinka Avoseh, Fraud Manager at Flutterwave, describes this trade as organised and systematic. Some harvested identities remain dormant for years before they are reused.


- **The human cost is real:** The people behind these identities often have no idea they have been caught up in fraud. The Dojah Fraud Insights Report 2025 highlights cases where individuals were implicated in schemes they never participated in because someone else reused their verified identities. As Avoseh puts it, *"the card owner is not always the cardholder."*


A farmed identity rarely stays on one platform. It moves from one onboarding process to another. That is where identity recycling begins.


**Related** :


[What is spoofing and how do African fintechs detect it](https://dojah.io/blog/spoofing-identity-fraud)


### **What Identity Recycling Looks Like in Practice**


A harvested identity rarely stays with the first platform where it was used. Fraud rings reuse the same verified identity across other platforms, often with little or no change.


The pattern usually looks like this:


- **One face, multiple BVNs.**


[The Fraud Insights Report 2025](https://dojah.io/fraud-insights-2025) also describes a case shared by


[Gbenga Ojerinde](https://ng.linkedin.com/in/gbenga-ojerinde-cfe-a570832b4?trk=public_post-text) , Fraud Manager at FairMoney, in which an individual used multiple BVNs with identical face images to apply for loans across different states in Nigeria. The face matched every application. Government ID verification alone did not detect the reuse.


- **One identity, multiple platforms:** Fraud rings do not retire a verified identity after one successful onboarding. They reuse it across fintechs, lending platforms, and crypto exchanges to open accounts, access credit, or create wallets. Your team sees a legitimate customer. It cannot see where that same identity has already been verified.


- **Small variations, same identity:** Fraud rings make small changes to avoid simple matching rules. They may switch devices, adjust the photo, or target a different type of platform. The identity stays the same, but the pattern becomes harder to spot when each platform only sees its own data.


The identity passes every check because nothing about it is fake. The challenge is recognising the reuse across multiple onboarding attempts.


### **How the Fraud Plays Out at Scale**


KYC farming and recycling are usually organised and designed to scale. Fraud rings coordinate identities across multiple platforms, making the activity much harder for any single fintech to detect.


**1. Coordinated deployment across platforms**


Fraud rings move quickly once they have a pool of verified identities. They submit multiple applications across fintechs, lending platforms, and crypto exchanges within a short period. By the time your team identifies suspicious activity, the same operation may already have succeeded elsewhere.


**2. Deliberate dormancy before activation**


Not every account becomes active immediately. Some remain inactive for weeks or months after onboarding, creating the appearance of a legitimate low-activity customer. When fraudulent activity begins, your onboarding checks have long since been completed.


**3. Standard KYC cannot see the full picture**


A one-time verification only tells you the identity was legitimate at onboarding. It does not show how that identity is reused or how its risk changes over time. The earlier cited report identifies one-off KYC verification as an ecosystem weakness.


[Gbenga Ojerinde](https://ng.linkedin.com/in/gbenga-ojerinde-cfe-a570832b4?trk=public_post-text) , Fraud Manager at FairMoney, recommends making KYC continuous rather than static because fraud has moved beyond what a single verification check can detect.


Catching this requires more than verifying an identity once. You need detection that connects activity before, during, and after onboarding.


### **What Detection Actually Requires**


Detecting KYC farming means looking beyond a successful verification. You need to identify the patterns that emerge across the user lifecycle.


That requires four key detection capabilities:


**1. Identity reuse detection**


The strongest signal is not a fake identity but a reused one. If the same identity appears across multiple accounts or platforms within a short period, your verification system should surface that pattern before the account is approved.


**2. Device intelligence**


Fraud rings often reuse a small number of devices to onboard many identities. Device intelligence helps you spot when different identity submissions originate from the same device or device cluster. That is not a pattern you would expect from legitimate customers.


**3. Face match consistency**


A matching face can confirm an identity, but it can also reveal reuse. If the same face appears across different identity submissions, that pattern becomes a fraud signal. It gives your team another layer of visibility beyond document verification alone.


**4. Post-onboarding behavioural monitoring**


Some fraud signals only appear after onboarding. Coordinated account activity, unusual transaction patterns, or behaviour that does not match the verified customer profile can all indicate that a farmed identity has been activated. If monitoring stops at onboarding, your team misses those signals.


These capabilities help you detect the patterns that a one-time verification cannot. That is the detection layer Dojah is built to provide.


### **How Dojah helps you detect KYC Farming and Identity Recycling**


Catching KYC farming requires detection at two points: during onboarding and after it. Dojah's


[EasyOnboard](https://dojah.io/all-products/easy-onboard) covers the verification signals at onboarding, while


[Profiled Risk](https://profiledrisk.com/) covers what happens once an account is live.


**At onboarding with EasyOnboard:**


- **Connected verification layer:** Document verification, biometric matching, and liveness checks run together in a single flow rather than as separate steps. When your team reviews a verification, they see the full picture, not just whether a document scanned clean, but whether the face matched and whether liveness confirmed a real person was present.


- **Face match consistency:** If the same face appears across multiple verification attempts on your platform, EasyOnboard surfaces that pattern. For a fraud ring reusing harvested identities, this is one of the clearest signals that something is wrong before an account is approved.


- **Configurable risk thresholds:** Your fraud team sets the confidence level that determines whether a verification passes, gets flagged for review, or is rejected outright. So when a submission looks slightly off, whether the face match score is borderline or a document detail is inconsistent, it goes to review instead of slipping through.


**After onboarding with Profiled Risk:**


- **Behavioural monitoring across the customer lifecycle:** When a farmed identity activates weeks after onboarding and starts transacting in ways that don't match the verified customer profile, Profiled Risk catches it. It tracks how behaviour evolves, so a dormant account that suddenly processes unusual transactions stands out rather than blending in.


- **Account clustering:** When multiple accounts on your platform show similar behavioural patterns or transaction flows, Profiled Risk connects those dots. For a fraud ring that has deployed dozens of farmed identities across your platform, that clustering is exactly the signal that reveals the operation.


If your fraud detection stops at onboarding, farmed identities that activate later won't show up until the damage is done.


See how


[EasyOnboard](https://dojah.io/all-products/easy-onboard) closes the verification gap at onboarding, and how


[Profiled Risk](https://profiledrisk.com/) catches what activates after.


**FAQs**


**1. What is KYC farming?**


KYC farming is the collection of real, verified identities for fraudulent use. Those identities are later reused across multiple platforms, often without the owner's knowledge.


**2. Why is identity recycling difficult for African fintechs to detect?**


Standard verification confirms that an identity is genuine, but it cannot tell whether that identity has already been reused. Detecting it requires looking beyond a single onboarding check at patterns that emerge across accounts and over time.


**3. How can African fintechs detect KYC farming and identity recycling?**


By combining identity verification with face match consistency checks, device intelligence, and post-onboarding behavioural monitoring to identify suspicious identity reuse earlier in the customer lifecycle.


**4. What should a fintech do if they discover a farmed identity was used to open an account?**


The account should be flagged and reviewed immediately. If fraudulent activity is confirmed, the account should be restricted and the incident documented. Depending on the nature of the activity, filing a suspicious transaction report with the NFIU may also be required.
