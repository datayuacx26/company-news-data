---
schema_version: "1.0.0"
document_id: "77f949a9365ef106bb579a31a09a31249930fc740bbfcc4cb099d7d4197a7c60"
company_key: "yc-coris"
company: "Coris"
source_id: "yc-coris-news-import-dd89c9ff7287"
canonical_url: "https://www.coris.ai/blogs/high-risk-merchant-management"
published_at: null
first_seen_at: "2026-08-12T20:21:01.058904+00:00"
fetched_at: "2026-08-12T20:21:02.338502+00:00"
content_hash: "sha256:2deeba049f5cbfae3d210b36e65fd43ec25a50ad8b8fa4025ae68320a3c53a7e"
---

# High-Risk Merchant Management: What You Need to Know

High-risk merchant management is the set of practices payment companies use to onboard, monitor, and control merchants that carry elevated potential for chargebacks, fraud, or regulatory exposure. It covers everything from initial underwriting decisions to ongoing portfolio surveillance and payout controls.


Get it wrong, and one problematic merchant can trigger card network fines, MATCH listings, and program-wide scrutiny that affects your entire portfolio. This guide walks through what makes a merchant high-risk, how to underwrite and monitor them effectively, and how to scale these operations without scaling headcount.


‍


## What Is a High-Risk Merchant


A high-risk merchant is a business that payment processors and acquiring banks view as having elevated potential for chargebacks, fraud, or regulatory complications. The label comes from the acquirer or processor, not from the merchant itself, and it's based on factors like industry type, transaction patterns, business model, and financial history.


Once a merchant receives this classification, specialized payment processing kicks in. That typically means higher transaction fees, reserve requirements where a portion of revenue is held back, and more intensive monitoring throughout the relationship.


One important distinction: high-risk is not the same as prohibited. A[prohibited merchant](https://www.coris.ai/blog/prohibited-restricted-and-high-risk-businesses-what-they-are-and-how-to-automatically-screen-for-them) cannot be onboarded at all due to legal restrictions or card network rules. A high-risk merchant, by contrast, can absolutely be approved and serviced, just with appropriate controls in place.


‍


## Characteristics That Make a Merchant High-Risk


Risk designation rarely comes down to a single factor. Processors look at the full picture: how the business operates, who it sells to, what it sells, and what its financial track record looks like.


### High Chargeback and Refund Ratios


Chargeback ratio is calculated by dividing the number of disputes by total transactions. When this ratio climbs above card network thresholds, typically somewhere between 0.9% and 1%, acquirers face fines, increased scrutiny, and potential program termination.


A merchant with historically elevated chargebacks signals ongoing risk, even if recent numbers have improved. The pattern matters.


### Regulated or Restricted Products


Selling products that require age verification, licensing, or that operate in legal gray areas increases compliance burden significantly. Think alcohol, certain supplements, or products with varying legality across states. The documentation requirements alone can be substantial.


### Cross-Border and Card-Not-Present Exposure


Card-not-present (CNP) transactions, where the physical card isn't swiped or inserted, carry[fraud rates approximately 15 times higher](https://www.rapyd.net/blog/card-present-vs-card-not-present-transactions/) than in-person payments. When you add international sales to the mix, risk compounds further because identity verification tools vary by country and fraud patterns differ across regions.


### Subscription and Recurring Billing Models


Recurring billing creates a specific risk pattern that's worth understanding. Customers forget they subscribed, then dispute charges months later. Or they struggle to cancel and file a chargeback out of frustration. This "friendly fraud" now accounts for[over 45% of all chargebacks](https://www.mastercard.com/us/en/news-and-trends/Insights/2024/first-party-fraud-why-is-it-so-hard-to-tackle.html) , driving disputes even when the merchant did nothing wrong.


### Thin Operating History or Weak Financials


New businesses lack the track record acquirers use to predict risk. Inconsistent revenue, poor credit history, or previous account terminations all signal that a merchant may not survive long enough to cover future disputes. Without history, there's no baseline for what "normal" looks like.


‍


## High-Risk Industries and MCC Codes to Watch


Merchant Category Codes (MCCs) are four-digit codes assigned by card networks to classify business types. Certain MCCs trigger automatic enhanced scrutiny during underwriting, though specific risk appetite varies by acquirer.


### CBD, Nutraceuticals, and Supplements


Regulatory uncertainty around health claims and FDA oversight creates compliance risk in this space. Chargeback rates also tend to run high when products don't deliver the results customers expected.


### Adult Content and Online Dating


Reputational concerns for acquirers, age verification requirements, and elevated chargeback rates from discreet billing disputes make these industries consistently high-risk. The billing descriptor alone can trigger disputes when customers don't recognize charges.


### Online Gambling and Gaming


Licensing complexity across jurisdictions, high transaction volumes with[fraud attacks up 76% in 2025](https://risk.lexisnexis.com/global/en/about-us/press-room/press-release/20260326-ccr-global-fraud) , and regulatory scrutiny define this category. Legal gaming platforms can be serviced, but the compliance burden is substantial and varies dramatically by location.


### Travel, Timeshare, and Ticketing


Fulfillment risk is the core issue here. Long gaps between payment and service delivery, sometimes months, create chargeback exposure if trips get canceled, events are postponed, or sellers go bankrupt before delivering what was promised.


### Firearms, Tobacco, and Vape


Age-restricted sales requirements, shipping restrictions, and evolving regulations increase compliance burden. As laws change, monitoring requirements change with them.


### Debt Collection and Credit Repair


High consumer complaint rates, CFPB scrutiny, and reputational concerns make acquirers cautious about these merchants. Underwriting requirements tend to be among the strictest.


‍


## Why High-Risk Merchants Require Dedicated Management


Standard onboarding and monitoring workflows aren't built for high-risk portfolios. The exposure is simply too significant to treat a supplements merchant the same as a low-risk retail shop.


Here's what's at stake:


- **Chargeback exposure:** Losses hit the acquirer when merchants can't cover disputes out of their own funds
- **Regulatory liability:** Card network fines and compliance penalties flow to the acquiring party, not just the merchant
- **Portfolio contagion:** One problematic merchant can trigger program-wide scrutiny from Visa or Mastercard, affecting every other merchant in the portfolio
- **MATCH listing risk:** Terminated merchants get added to an industry database, creating long-term consequences for everyone involved


To make this concrete: imagine a payfac onboards a supplements merchant without enhanced monitoring. Chargebacks spike to 2% within three months. The card network flags the entire program for review, not just the one merchant causing problems, but every merchant in the portfolio. That's portfolio contagion in action.


‍


## How to Underwrite High-Risk Merchants


Underwriting is the first line of defense. High-risk merchants require deeper due diligence than standard[Know Your Business (KYB)](https://www.coris.ai/blogs/kyb-software-for-payment-platforms) checks.


### 1. Verify Business Identity and Beneficial Ownership


Start with business registration, EIN verification, and beneficial ownership disclosure. Synthetic business identities, fabricated companies designed to defraud acquirers, are a growing fraud vector.[High-risk business screening](https://www.coris.ai/blogs/high-risk-business-screening) catches many of these before they become problems.


### 2. Screen Against MATCH, Sanctions, and Adverse Media


MATCH (Member Alert to Control High-Risk Merchants) is the industry database of terminated merchants. Screening against MATCH, OFAC sanctions lists, and[adverse media](https://www.coris.ai/blogs/adverse-media-screening) helps identify merchants with problematic histories. Better to discover issues during underwriting than after onboarding.


### 3. Assess Website, Product, and Marketing Compliance


The merchant's website is evidence of what they actually sell and how they market it. Card networks have specific rules around pricing transparency, refund policies, and prohibited content. Underwriters can verify compliance directly by reviewing the site.


### 4. Model Chargeback and Fraud Exposure


[Estimating expected chargeback rates](https://www.coris.ai/merchant-exposure-calculator) based on industry benchmarks, business model characteristics, and historical data (when available) helps quantify risk before the first transaction processes.[Fraud scoring models](https://www.coris.ai/blogs/fraud-detection-software) add another layer of assessment.


### 5. Set Approval Conditions and Risk Tiers


Approval isn't binary. High-risk merchants may be approved with conditions: reserves, volume caps, or enhanced monitoring requirements. Risk tiering allows you to match controls to actual risk levels rather than applying the same restrictions to every merchant.


‍


## How to Monitor High-Risk Merchants After Onboarding


Underwriting is point-in-time, but risk is continuous. Merchants change, sometimes intentionally, sometimes not, and those changes affect risk profiles.


Ongoing monitoring typically covers:


- **Transaction velocity tracking:** Sudden spikes in volume may indicate fraud or undisclosed business model changes
- **Chargeback ratio monitoring:** Watching trends before they breach card network thresholds gives time to intervene
- **Website and product changes:** Merchants sometimes pivot to prohibited products after approval, and[continuous website monitoring](https://www.coris.ai/blog/introducing-website-monitoring) catches these shifts automatically
- **Business health signals:** Closures, litigation, and licensing changes all affect risk


[Manual monitoring doesn't scale](https://www.coris.ai/blog/automation-of-merchant-risk-monitoring-is-essential) well. Teams managing hundreds or thousands of merchants benefit from automated alerts and portfolio-wide visibility to catch problems early. Platforms like[Coris](https://www.coris.ai/) provide this kind of continuous monitoring without requiring proportional headcount growth.


‍


## Chargeback and Fraud Controls for High-Risk Portfolios


High-risk portfolios benefit from proactive fraud prevention rather than reactive dispute management. By the time a chargeback arrives, the money has already moved.


Key controls include:


- **Transaction monitoring:**[Real-time payment analysis](https://www.coris.ai/blogs/transaction-monitoring) catches anomalies before settlement occurs
- **Velocity rules:** Limits on transaction counts, amounts, or frequency trigger review when exceeded
- **3D Secure and authentication:** Shifts liability and reduces card-not-present fraud
- **Chargeback alerts:** Early warning systems allow refunds before disputes escalate to formal chargebacks


Here's how this works in practice: a merchant's average ticket suddenly jumps from $50 to $500. Automated rules pause payouts and flag the account for review. The risk team investigates before any losses occur, rather than discovering the problem weeks later in a chargeback report.


‍


## Reserves, Velocity Caps, and Payout Controls


These controls protect acquirers from losses while still allowing high-risk merchants to process payments. Each serves a different purpose.


‍


Control Type What It Does When to Use


Rolling reserve Holds a percentage of settlements to cover future chargebacks High chargeback risk merchants


Capped reserve Fixed amount held until relationship ends New merchants with thin history


Velocity caps Limits on daily or monthly processing volume Merchants with unpredictable volume


Payout delays Extended settlement windows Fulfillment risk industries like travel


‍


Finding the right balance matters. Controls that are too aggressive affect merchant cash flow and retention. Controls that are too loose leave the acquirer exposed.


‍


## Automating High-Risk Merchant Management at Scale


[Manual processes break down](https://www.coris.ai/blog/the-true-cost-of-manual-merchant-underwriting-and-how-to-automate-it) as merchant portfolios grow. What works for 50 merchants becomes unsustainable at 500 or 5,000.


Automation opportunities include:


- **Automated underwriting workflows:**[Rules-based decisioning](https://www.coris.ai/blogs/automated-merchant-underwriting) handles routine applications while human review focuses on edge cases
- **Continuous monitoring alerts:** Portfolio-wide signals surface automatically instead of requiring manual discovery
- [AI-driven case prioritization](https://www.coris.ai/blog/risk-ai-agent-risk-management-fraud) **:** Analyst attention focuses on highest-risk merchants first
- **Configurable playbooks:** Standardized responses to common risk scenarios reduce inconsistency and speed up resolution


Platforms like[Coris](https://www.coris.ai/) enable teams to operationalize these workflows without building custom infrastructure, combining[merchant intelligence](https://www.coris.ai/blogs/real-time-merchant-intelligence-how-fintech-platforms-scale-growth-without-weakening-risk-infrastructure) , risk orchestration, and transaction monitoring in one system. The goal is scaling risk operations without scaling headcount proportionally.


‍


## Frequently Asked Questions About High-Risk Merchant Management


### What is considered a high-risk merchant?


A high-risk merchant is a business that payment processors classify as having elevated potential for chargebacks, fraud, or regulatory issues based on industry, business model, or financial history. The classification comes from the acquirer, not the merchant.


### Which MCC codes are high-risk?


Common high-risk MCC codes include those for nutraceuticals, online gambling, adult content, travel services, and subscription businesses. However, risk designation depends on the acquiring bank's specific policies, not just the code itself.


### How is a high-risk merchant different from a prohibited merchant?


High-risk merchants can be onboarded with enhanced controls and monitoring. Prohibited merchants cannot be accepted under any circumstances due to legal restrictions or card network rules.


### What triggers a merchant to be placed on the MATCH list?


Merchants are added to MATCH when terminated by an acquirer for excessive chargebacks, fraud, violation of card network rules, or illegal activity. The listing follows the merchant and affects their ability to get approved elsewhere.


### How often should high-risk merchants be reassessed?


High-risk merchants benefit from continuous automated monitoring, with formal reviews triggered by threshold breaches, business changes, or at minimum quarterly intervals. Point-in-time assessments miss risk that develops between reviews.
