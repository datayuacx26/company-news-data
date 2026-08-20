---
schema_version: "1.0.0"
document_id: "37900222868791afa2d4d1e34132c2800ca33d997d0712fe3b3a5eb01c2fb551"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-news-import-5371cda5d0b0"
canonical_url: "https://www.riskified.com/blog/nacha-ach-fraud-monitoring-rules-2026/"
published_at: "2026-07-14T19:21:42+00:00"
first_seen_at: "2026-07-23T23:08:45.292953+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:baae7e15a0508327c52f2f2775477acce52fbdc1c9b0fd445849936102a2b0fb"
---

# What Nacha's amended rules mean for your ACH fraud monitoring

Despite being an older technology,[ACH](https://www.riskified.com/blog/ach-is-making-a-comeback-in-ecommerce/) (Automated Clearing House) payment has grown into one of the most popular online payment rails. Merchants offer WEB ACH because the economics are compelling: bank declines are significantly lower than with credit cards, and per-transaction fees are a fraction of card interchange fees. According to Nacha, the organization that governs the ACH network, internet payments on the ACH network[reached](https://www.nacha.org/content/ach-network-volume-and-value-statistics) 11.41 billion in 2025, totaling $6.98 trillion — a 6.1 percent year-over-year increase in payment volume.


The threat environment has grown alongside that adoption. ACH fraud, including unauthorized debits, account takeover, and fraudulent account funding, is on the rise, and it forms a meaningful slice of a much larger problem. The Federal Trade Commission (FTC) tracks fraud across every category and the numbers keep climbing: $12.5 billion in[reported](https://www.ftc.gov/news-events/news/press-releases/2026/03/ftc-testifies-joint-economic-committee-agencys-efforts-combat-fraud) losses in 2024 and $15.9 billion in 2025. ACH-related fraud is one contributor within those totals, and the real figure runs higher. Law enforcement estimates that only 2 percent to 6.7 percent of victims ever report their losses, which led the FTC to[estimate](https://www.ftc.gov/system/files/ftc_gov/pdf/P144400-OlderAdultsReportDec2025.pdf) the true 2024 toll at $196 billion.


## **Why are ACH payments susceptible to fraud?**


ACH’s low fees and broad coverage make it an attractive target: the same economics that benefit legitimate merchants also appeal to fraudsters looking to exploit the settlement lag.


Take remittance payments, for example. Remittances are inherently time-sensitive. Customers expect instant delivery, but ACH settlement takes days. That gap creates an opening. Most platforms begin fulfillment as soon as an account is verified, yet the funds may take multiple business days to actually arrive. This lag is what fraudsters exploit. A bad actor initiates a transfer, and the recipient abroad gets paid right away. The ACH return takes days to come back, and by then, there’s no dispute mechanism to recover the funds, leaving platforms with no recourse. Even legitimate customers with insufficient balances can receive their payout before anyone catches the shortfall.


To reduce exposure to higher-risk ACH payments, many platforms use delayed fulfillment, or holding goods or services until a transaction fully settles. By waiting for settlement before releasing funds, platforms close that window and keep their losses in check. The key is knowing the differences among your customers, so you’re not applying a blanket policy that hurts the experience for good customers or leaves room for exploitation.


## **How do merchants remain compliant under the new rules?**


Nacha recently amended its rules to require fraud monitoring across the ACH ecosystem. Nacha rolled these requirements out in two phases: Phase 1 took effect March 20, 2026, for Originators, Third-Party Service Providers, and Third-Party Senders with 2023 origination volume of six million entries or more, plus all ODFIs. Phase 2 took effect June 22, 2026, extending the requirement to every remaining non-consumer Originator, regardless of volume. The industry is still working through what full compliance looks like in practice.


These rules set a baseline expectation for fraud monitoring, but stop short of prescribing specific technologies. Nacha intentionally adopted a technology-neutral, outcomes-based framework, giving institutions flexibility in how they comply as long as the results hold up.


The amendment has two parts worth understanding. As merchants originating ACH payments, the first part applies to you: it places requirements on non-consumer ACH Originators, Third-Party Service Providers (TPSPs), and Originating Depository Financial Institutions (ODFIs). If you initiate ACH payments online, you’re an Originator. The second part targets receiving financial institutions (RDFIs, the banks that receive funds from scam victims) and doesn’t apply to merchants, so we will not go into those details here.


## **What is the new requirement?**


The updated Nacha rules require merchants to have fraud prevention processes in place. Most originators already had risk-based processes to identify fraudulent ACH debit transactions before they are sent, as Nacha has required account validation (confirming that the account exists and belongs to the person submitting payment) for the first use of a WEB debit since 2021. What’s new in this amendment is broader: monitoring is no longer limited to WEB debits, and Nacha now expects tracking of return codes across all entry types. Return codes for fraud (ISF: R01, R07; Fraud: R10, R29) indicate whether your controls are working.


Merchants best positioned to meet this requirement have programs with three layers: verification at the point of payment (using account connectivity services to confirm identity), intelligent decisioning on each transaction, and a recovery protocol when returns arrive.


If your ACH fraud management program is new or still evolving, this rule change is useful. It provides a business case for proactively funding an updated monitoring process. Working with a fraud solution that has supported ACH for years gives you a real advantage. A seasoned provider understands the nuances of ACH fraud in ways a newer tool can’t. It knows the fraudsters themselves, aided by a cross-merchant network that surfaces bad actors across the ecosystem. It recognizes their methods of operation and accounts for the risks unique to ACH, such as the insufficient funds example illustrated above. A solution that’s been tested at scale is a different thing from one built in response to a regulatory notice. Your originating bank will notice the difference, and so will your fraud rates.


## **Let Riskified help you**


If you accept ACH payments and want to know whether your fraud program aligns with the new Nacha Rules,[talk to one of our experts](https://www.riskified.com/contact/) today.


Riskified reviews every ACH payment and returns an approve or decline decision. For approved transactions, Riskified’s decision includes a fulfillment recommendation — instant or delayed — based on the risk profile so that lower-risk customers get faster service without leaving you exposed to potential fraud or insufficient funds. Every transaction Riskified approves is backed by a financial guarantee; if an ACH payment is later returned, Riskified covers the loss.
