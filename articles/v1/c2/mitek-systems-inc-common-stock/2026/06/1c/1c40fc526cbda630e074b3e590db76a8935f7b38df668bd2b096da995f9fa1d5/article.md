---
schema_version: "1.0.0"
document_id: "1c40fc526cbda630e074b3e590db76a8935f7b38df668bd2b096da995f9fa1d5"
company_key: "mitek-systems-inc-common-stock"
company: "Mitek Systems Inc."
source_id: "mitek-systems-inc-common-stock-rss-3bf79578716e"
canonical_url: "https://www.miteksystems.com/blog/how-synthetic-identity-fraud-is-built-and-how-to-detect-it-before-losses-occur"
published_at: "2026-06-13T17:58:53+00:00"
first_seen_at: "2026-07-20T23:21:50.456876+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:3194bbba2a87fadfd79601bd9a19d750bd631b9979072741db8f02806e896b10"
---

# How synthetic identity fraud Is built—and how to detect it before losses occur

Understanding how synthetic identities are built is essential for the fraud teams that must combat them. The process is a long and deliberate one; it also leaves specific traces that modern detection systems can identify, if they know where to look.


Point-in-time detection systems that focus on activities - like examining a check used to open an account, or only reviewing an account when there’s an alert when a payment is missed - aren’t sufficient to detect synthetic identity fraud, which is a lifecycle crime that requires a lifecycle approach.


## How criminals build synthetic identities


Constructing a synthetic identity is a multi-stage process that requires a combination of real and created data. The first step for the fraudster is to acquire a real Social Security number (SSN) with little to no history, and low likelihood of being monitored – like children, the deceased, homeless individuals, recent immigrants, and older persons. This foundational element serves as the anchor for the synthetic identity, allowing it to pass government and credit bureau notification.


Next, they construct the supporting elements, like a name, date of birth, address, phone number, and email address, from elements that are internally consistent and won’t trigger anomaly flags – for example, the address might be a real street location but a nonexistent unit number, and the date of birth would align to when the SSN was issued. Altogether, this creates a plausible identity.


The next step is counterintuitive in a way, but it’s important: the synthetic identity starts applying for credit, and expects to receive rejections, at least the first time. But that declined application prompts credit bureaus to start building a file for the identity, even if it’s a “thin” file. Sometimes, in lieu of building a file through initial rejections, fraudsters will use “piggybacking” – paying to be added as an authorized user on a creditworthy real account or even adding it to one of their other, stronger synthetic identities, so that it inherits some positive credit history and increases in creditworthiness faster.


AI and automation have streamlined the speed at which this process can be enacted. Everything that used to require manual effort, like researching SSN formats, generating consistent profiles, managing multiple applications, and even creating valid identity documents, can now be handled by systems that are capable of managing entire portfolios of synthetic identities and even testing them against simulated detection systems before deploying them into the wild.


## Where synthetic fraud appears in the lifecycle


Synthetic fraud looks different at every stage of the customer lifecycle. The need to examine activity across the customer lifecycle contributes to making it so challenging to detect, especially using traditional controls.


During onboarding, the synthetic identity makes its first contact with the financial institution. Onboarding is a high-leverage point for detection and rejection, because at this point, the financial institution has yet to incur any fraud costs. But it’s also a difficult moment to detect this kind of fraud, as the identity has been carefully constructed to appear legitimate (and might even have a credit file already, from previous rejections or as an authorized user). This means that standard document reviews and Know Your Customer (KYC) checks used to verify a customer’s identity and assess risk might not catch a well-constructed synthetic identity that’s been cultivated for a few months.


It’s even harder to distinguish a synthetic identity from a real person in its credit-building phase. During this phase, the synthetic identity is behaving like a model customer, making its payments on time and staying well within its credit limits, all with the goal of obtaining higher lines of credit. This phase might last months or years, and during this phase, the institution sees it as a performing account with no signals to investigate.


The final phase is the bust-out. This is when the fraudster takes advantage of all of those credit lines he or she carefully nurtured for the synthetic identity and draws them all down simultaneously, via cash advances, purchases, or balance transfers. In the span of one billing cycle, the accounts have gone from a performing ideal customer to over limits and delinquent. Within two or three more billing cycles, the accounts have been charged off and the financial institution records it as a credit default – often still having no clue that fraud was involved.


Lifecycle-based detection requires capabilities across all of these phases, with the ability to detect signals that block synthetic identities from onboarding, detect sleeper synthetic identities in the credit building phase, and can distinguish between a legitimate person’s credit default and losses due to bust-outs.


## How synthetic identities evolve


The bust-out is the biggest payday and what synthetic identities have come to be best known for, but that’s not the only fraudulent activity for which they are used. Fraudsters have found ways to get them involved in a wide range of financial crimes.


First-party fraud is one evolution. Especially with digital lending and buy-now-pay-later (BNPL) products, dispute resolution is consumer-friendly by design. Fraudsters will act as the account holder and open disputes or request chargebacks, exploiting consumer protection rules and submitting a multitude of claims. In a new Datos Insights report,[The Synthetic Identity Crisis: Detection, Prevention, and the AI Arms Race](https://www.miteksystems.com/resource-library/white-papers/the-synthetic-identity-crisis-detection-prevention-and-the-ai-arms-race) *,* a fraud executive at a major financial institution estimated that approximately one-third of their firm’s first-party check-fraud losses are directly attributable to synthetic identities. The persistence of this threat is also tracked in Datos’ report: 55% of fraud executives reported increases in first-party check-fraud losses in 2025, and that growth was substantial, with 16% reporting growth of 10% or more.


Credit abuse is similar to bust-out, but with a higher level of coordination. In credit abuse, the synthetic identity simultaneously accumulates credit products at several institutions and uses each relationship to support the creditworthiness of the others. This allows them to maximize total available credit across multiple institutions before the final draw-down. Fraud rings are able to coordinate these activities across a large portfolio of synthetic identities. This creates significant exposure for multiple institutions, often concentrated in specific products, when the bust-out wave is triggered.


Money muling, or the use of accounts held by synthetic identities to receive and transfer criminal proceeds, is also becoming increasingly common. The synthetic identity has the apparent legitimacy of an established account and can be used to separate the funds from criminal activity from their real source and ultimate beneficiary, making it less likely that the transactions will trigger suspicious activity reporting (SAR reports). Because of their potential use as a vehicle for money muling, identifying synthetic identity infrastructure can also be a powerful tool for financial intelligence teams as they monitor for money laundering. Identifying one mule account can often open up an avenue to identifying multiple synthetic identity; as cited by Datos, the average fraud event involves not just one but three mule accounts, and organized criminal rings find it useful to exploit synthetic identities to create and control mule accounts at scale, because doing so eliminates the need to recruit and manage external co-conspirators.


Monitoring for these activities is a best practice for financial institutions, and can provide additional signal that suggests an account should be further examined to confirm there is a real person behind it.


## Signals that differentiate real vs synthetic identities


Often, there’s no smoking gun. Modern synthetic identities are very sophisticated, and detection will require assembling multiple signals into a clear picture. Some signals are visible during onboarding, but other important signals will not emerge until afterward.


The first signal category is identity consistency. Does the combination of name, SSN, date of birth, and address make logical sense, or are there issues like the SSN’s issue date predating the date of birth? Are there any issues where different names have been used with the same SSN, or where the same name has been used with different SSNs, across multiple application attempts? Cross-referencing these attributes, both against each other and against external data sources, can uncover inconsistencies.


Behavioral or velocity anomalies after onboarding are often more revealing. For example, a new account immediately requesting large credit line increases or an account with a thin credit history that’s immediately exhibiting disciplined and sophisticated behavior (like keeping utilization under 10% and paying the card in full before the statement close) doesn’t always mean the account isn’t held by a real person, but it can merit closer scrutiny.


Clustering is another characteristic that only becomes apparent after accounts are established and signals can be analyzed across them rather than discretely. For example, a collection of accounts from different identities that are managed on the same device or IP address can signal an automated fraud ring operator.


Cross-signal mismatches are an extension of this. Phone numbers, email addresses, device fingerprints and digital footprints that are associated with multiple identities can indicate coordinated fraud. They can also provide weak signals that become stronger in combination, like if an email address has been created the same day as an account was applied for, or a phone number is newly issued or has no prior association to the claimed identity.


Document verification and liveness detection are also essential. In a fully digital onboarding flow, they serve as a first line of defense. These tools verify that the identity documents presented are genuine, that the face on the document matches the person applying, and that a real, live human being is present. These tools are invaluable for catching synthetic identities that are relying on fabricated or manipulated identity documents or are using physical or AI-generated materials to attempt to bypass liveness detection.


Looking ahead over the next three years, Datos found that 74% of fraud executives across global financial institutions expect to see significant growth in attacks that use AI-powered voice cloning and deepfake fraud. This makes investments in robust liveness detection and document forensics increasingly non-negotiable for institutions seeking to stay ahead of this threat.


## Why early, lifecycle-based detection matters


Put simply, the earlier the detection, the lower the cost to your institution. A synthetic identity that’s caught during the onboarding process represents little to no loss to the institution. If the identity is caught during the credit-building phase, but before any significant credit lines or products have been extended, losses are minimized. If it’s detected as the bust-out starts, institutions can still freeze accounts and limit exposure. But if it’s not detected before the bust-out is completed, the loss can be tens or hundreds of thousands of dollars per synthetic identity.


Every stage of the customer lifecycle is a potential detection opportunity, and every missed opportunity to do so compounds the eventual losses, making the importance of layering in detection clear. Onboarding checks will block many submissions, but well-constructed synthetic identities may still get through. Monitoring during the credit-building phase can detect the behavioral signatures that are consistent with how synthetic identities operate, catching many in their incubation phase, before they can do real damage. And establishing bust-out detection triggers enables rapid account freezing when the more visible rapid drawdown patterns start to appear, preventing a full loss.


Practically, detection infrastructure needs to operate continuously rather than episodically to be effective. Effective detection doesn’t create a series of gates that the identity either passes or fails. Instead, it performs the equivalent of ongoing monitoring that regularly reassesses the risk signals presented by any given account’s activity and flags accounts when those signals shift in a way that makes them consistent with synthetic identity behavior.


Continuous monitoring requires a meaningful operational investment, but also is the only approach that delivers the insights needed to match the synthetic identity threat. This type of fraud is designed to exploit point-in-time controls and counts on being allowed to operate unfettered once the accounts are created. Institutional defense must move beyond point-in-time thinking.
