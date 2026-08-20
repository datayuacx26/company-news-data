---
schema_version: "1.0.0"
document_id: "1ed583cda64279476d0b77411d76b4e1a001087eb1a2e7e1335396459f9b7b7c"
company_key: "yc-ballerine"
company: "Ballerine"
source_id: "yc-ballerine-news-import-9376e18e0cef"
canonical_url: "https://ballerine.com/blog/bram-and-transaction-laundering-what-mmsp-monitoring-covers"
published_at: "2026-07-23T10:33:20.735+00:00"
first_seen_at: "2026-07-23T02:58:11.390770+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:cc86e29849341d0da3a75e99c49086a90efe92378afc7ab7fae8f649770e4639"
---

# BRAM and Transaction Laundering: What MMSP Monitoring Covers

‍


Merchant monitoring under the Mastercard Merchant Monitoring Program (MMP) is designed to detect two distinct risk types: Business Risk Assessment and Mitigation (BRAM) violations and merchant transaction laundering. These are not interchangeable concepts.


‍


Each describes a different failure mode in the acquiring relationship, and each requires a different detection approach. Understanding both is foundational to building a monitoring program that meets Mastercard's requirements and holds up under investigation.


‍


BRAM and transaction laundering


### Two risk types, two detection paths


Content-based risk


#### BRAM


What is the merchant offering?


Restricted products


Prohibited services


Brand-damaging content


Risky content changes


**Example:** a compliant supplements site later adds prescription drugs.


Signal-based risk


#### Transaction laundering


Who or what is actually being processed?


Hidden sub-merchants


Proxy merchant structures


Payment flow mismatch


MCC or business-model mismatch


**Example:** a gift shop processes for an undisclosed high-risk business.


**BRAM surfaces in content** Products, services, claims, categories, and website changes.


**Laundering surfaces in structure** Payment behavior, MCC mismatch, proxy flows, and hidden entities.


‍


## What BRAM Is


‍


The Business Risk Assessment and Mitigation (BRAM) program is Mastercard's compliance framework for identifying and addressing merchant activity that poses fraud, regulatory, or legal risk, or that may cause reputational damage to the Mastercard network. Mastercard's own published documentation describes BRAM as designed to ensure that Mastercard cards are not associated with illegal or brand-damaging activities.


‍


BRAM violations occur when a merchant's products, services, or website content fall into categories that Mastercard prohibits or restricts. The list of prohibited categories is not static. Mastercard updates it periodically in response to emerging risk patterns. Categories that Mastercard has identified in published program communications include: the sale of prescription pharmaceuticals without appropriate licensing or prescription verification, prohibited adult content, counterfeit or intellectual property-infringing goods and services, unlawful gambling transactions, and the sale of illegal firearms and weapons.


‍


The acquirer's exposure under BRAM is direct. Mastercard holds acquirers responsible for ensuring their merchants do not engage in prohibited activity. When a BRAM violation is confirmed, Mastercard may assess fines, require merchant termination, and in some cases result in the merchant's listing on the Mastercard Alert to Control High-risk Merchants (MATCH) database. The acquirer's ability to demonstrate that it was actively monitoring for such violations, and how quickly it responded once a violation was flagged, are both factors Mastercard considers when determining the appropriate response.


‍


*A concrete BRAM scenario.* A merchant onboards as a general health supplements retailer. Its website at onboarding presents compliant products. Six months later, the merchant adds a product line of prescription pharmaceuticals sold without prescription verification or pharmacy licensure. If the acquirer is running persistent monitoring, the content change triggers an alert. If the acquirer is not, the violation continues undetected until Mastercard initiates an investigation. At that point, the acquirer has no monitoring record to submit and no evidence of early detection or prompt action.


‍


‍


‍


## What Merchant Transaction Laundering Is


‍


Merchant[transaction laundering](https://ballerine.com/glossary/transaction-laundering) occurs when a legitimate merchant account is used to process card transactions on behalf of another entity that is not disclosed to the acquirer or to Mastercard. The undisclosed entity is typically operating in a prohibited or high-risk category that would not have been approved for its own merchant account.


‍


Transaction laundering is structurally different from BRAM content violations. The merchant's website may present no prohibited content at all. The violation is in the payment flow, not the storefront. A compliant-looking merchant is accepting payments for goods or services that the acquirer never reviewed, never approved, and may never see unless monitoring extends to transaction-level indicators and sub-merchant structures.


‍


Mastercard's program documentation notes that merchant transaction laundering may itself trigger BRAM noncompliance, as well as separate assessments for Merchant Category Code (MCC) miscoding violations and failure to register high-risk merchants through the applicable Mastercard registration programs. The two risk types therefore overlap: transaction laundering is often accompanied by BRAM violations once the actual activity is surfaced.


‍


*A concrete transaction laundering scenario.* A merchant registered as an online gift retailer processes transactions normally for several months. Investigation reveals that a portion of its transaction volume relates to a second, unregistered business operated by the same principals, selling products in a category that would not have been approved for direct acquiring. The gift retailer's merchant account is the processing vehicle. Without transaction-level analysis connecting the processing patterns to a separate commercial operation, the structure remains invisible in a standard content scan.


‍


‍


‍


## How MMSP Monitoring Addresses Both Risk Types


‍


An approved Merchant Monitoring Service Provider (MMSP) is required to perform monitoring services covering both BRAM violations and transaction laundering. An acquirer participating in the[Merchant Monitoring Program](https://ballerine.com/solutions/mmp-standards) may choose a single MMSP that covers both, or engage separate providers for each. Either arrangement must be registered with Mastercard.


‍


The two detection requirements operate differently in practice.


‍


MMSP monitoring


### One workflow, two monitoring lenses


MMSP monitoring


#### Content lens


BRAM detection


Public pages


Product listings


Gated areas


Prohibited category match


Timestamped evidence


#### Signal lens


Laundering detection


Transaction behavior


MCC mismatch


Proxy structures


Undisclosed sub-merchants


Payment flow anomalies


Alert


Report to acquirer


Resolve and document


**5 business days** MMSP reports a potential violation.


**15 calendar days** Acquirer investigates and stops the activity.


‍


BRAM detection is primarily content-based. The MMSP scans merchant web presence, including public pages, product listings, and, as required by the MMP's January 2026 standards, restricted and members-only areas. Content is evaluated against Mastercard's prohibited category list. Alerts are generated when content matches prohibited types, with timestamped evidence captured for documentation purposes. This applies at initial onboarding scan and on an ongoing basis throughout the merchant relationship.


‍


Transaction laundering detection is primarily signal-based and structural. It looks for discrepancies between what a merchant is registered to process and what the transaction data actually reflects. Indicators include proxy merchant structures, undisclosed sub-merchants, payment flows inconsistent with the declared business model, and MCC codes that do not match the actual goods or services being sold. Effective transaction laundering detection requires access to both web-facing data and transaction-level behavior, analyzed together.


‍


The MMSP's obligation under MMP does not end with detection. When a potential violation is identified, the MMSP must report it to the acquirer within five business days. The acquirer then has 15 calendar days to investigate, ensure the violating activity ceases, and report the resolution back to the MMSP. That resolution record forms part of the monthly report submitted to Mastercard. If a BRAM investigation is opened, the acquirer must submit an unaltered incident report generated by the MMSP, covering the monitoring period and detailing any alerts generated and actions taken.


‍


Acquirers that fully participate in MMP with a registered MMSP, and that meet all reporting requirements, may be eligible for mitigation of assessments related to BRAM violations. That mitigation is not available to acquirers using unregistered monitoring vendors, regardless of the quality of the monitoring performed.


‍


‍


‍


## About Ballerine


‍


Ballerine is a certified Mastercard MMSP. The platform's AI agents are built to detect both BRAM content violations and transaction laundering indicators, covering public content, gated merchant areas, and transaction-level signals within a single[monitoring workflow aligned to Mastercard's MMP standards](https://ballerine.com/blog/mastercard-mmsp-explained) . Initial scans at onboarding and persistent post-onboarding monitoring produce the timestamped, unaltered evidence records that MMP compliance and investigation response require.


‍


*Disclaimer: The information in this article is provided for general educational purposes and is not endorsed by or affiliated with Mastercard. Readers should consult Mastercard's official Rules, Security Rules and Procedures, and applicable program documentation for definitive requirements.*


‍
