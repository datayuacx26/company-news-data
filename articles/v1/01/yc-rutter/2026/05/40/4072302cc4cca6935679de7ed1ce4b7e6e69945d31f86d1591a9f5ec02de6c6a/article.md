---
schema_version: "1.0.0"
document_id: "4072302cc4cca6935679de7ed1ce4b7e6e69945d31f86d1591a9f5ec02de6c6a"
company_key: "yc-rutter"
company: "Rutter"
source_id: "yc-rutter-news-import-c12c34cd87f8"
canonical_url: "https://www.rutter.com/blog/erp-integration-methods"
published_at: "2026-05-27T13:00:01.645+00:00"
first_seen_at: "2026-07-22T12:26:40.375919+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:96d614e0db36bef640ad09e76720be51982454952798e8479a9441337a83bfb5"
---

# A guide to ERP integration methods: Accounting API, bank feeds, and fully embedded

When someone says they “integrate with an ERP,” they usually mean one of three different things. Buyers conflate them, engineering teams scope them differently, and confusing the three costs a fintech or bank months of build time and missed deal cycles. Mercury, PayPal, Airwallex, Payoneer, and 100+ other fintechs and banks use Rutter today to integrate with 30+ ERP and accounting platforms, and which method they use depends on the workflow they are delivering.


The three integration methods are:


Each one solves a different buyer problem, has a different technical shape, and has a different ERP coverage profile. This post defines the three, shows which method each major ERP supports, and explains how to pick between them.


“ERP integration” is not one thing. It is three things, and the difference matters.


## What is Accounting API integration?


**Accounting API integration** is the method where a fintech or bank’s product calls a unified API that reads from and writes to the customer’s ERP or accounting system. Rutter’s Accounting API handles the translation to each underlying ERP’s specific schema. Read use cases include pulling general ledger data, accounts payable balances, invoices, and transaction history. Write use cases include posting bills, journal entries, expense data, and reconciliation entries back into the system of record.


This is the right method when the product needs to ingest financial data for a decision (lending, underwriting, FP&A) or write transactions back to the customer’s books (bill pay, expense management, invoicing). Mercury uses it to write expense management and bill pay transactions back into the customer’s accounting system. PayPal uses it to reconcile merchant transactions back to the customer’s general ledger. Parafin uses it to underwrite DoorDash Capital loans by ingesting merchant accounting and commerce data through Rutter.


The Accounting API is the most flexible integration method, and it requires the fintech or bank to design and own the UI surface. The customer accesses the workflow inside the fintech or bank’s product, not inside their ERP.


## What is bank feeds integration?


**Bank feeds integration** delivers a bank’s or fintech’s transaction data into the customer’s ERP for reconciliation. The customer reconciles those transactions inside the ERP’s native bank feed module, the same way they reconcile any other bank account. Transactions flow from the bank or fintech, through Rutter’s bank feed infrastructure, into the customer’s QuickBooks, Sage, Xero, NetSuite, or other supported ERP.


This is the right method when the bank or fintech’s product is the source of transactions and the customer’s accounting team reconciles inside the ERP. Mercury, Visa, Teya, and Erebor use Rutter Bank Feeds today to deliver transaction reconciliation across major ERPs. Erebor deployed bank feeds across QuickBooks Online, NetSuite, Xero, and Sage in 2.5 weeks.


Bank feeds is narrower than the Accounting API. It is one-directional (bank to ERP), it serves one use case (reconciliation), and it depends on direct ERP listings or aggregator partnerships (Plaid, Yodlee) for distribution. See[Why bank feeds became baseline infrastructure for embedded reconciliation](https://www.rutter.com/blog/bank-feeds-embedded-reconciliation) .


## What is fully embedded integration?


**Fully embedded integration** is a native financial application that runs inside the customer’s ERP, using the ERP’s own UI, permissions, and workflows. The fintech or bank’s product is not a separate destination the customer logs into. It is a layer inside the system the customer already uses. AP automation, treasury, reconciliation, FX, and bank feeds all run on the customer’s existing approvals and audit chain, branded under the bank or fintech.


This is the right method when the customer is mid-market or enterprise and works primarily inside their ERP. Common fully embedded use cases include AP automation, treasury, reconciliation, embedded FX, and payments, all surfaced inside the customer’s ERP under the bank or fintech’s brand. See[Why embedded finance is about where the product lives, not just what it integrates](https://www.rutter.com/blog/embedded-finance-where-product-lives) and[Why the financial OS for midmarket businesses lives inside the ERP](https://www.rutter.com/blog/financial-os-midmarket-erp) .


Fully embedded is the deepest integration method. It also has the narrowest ERP coverage today, available across 10 major ERPs including NetSuite, Microsoft Dynamics 365 Business Central, Microsoft Dynamics 365 F&O, Sage Intacct, Workday, Acumatica, Oracle Fusion ERP Cloud, and SAP S/4HANA.


## Which integration method does each ERP support?


The three integration methods have different ERP coverage profiles. Mid-market and enterprise ERPs (NetSuite, Microsoft Dynamics, Sage Intacct, Workday, SAP S/4HANA, Oracle Fusion) generally support all three methods. SMB ERPs (QuickBooks Online, Xero, FreshBooks) support the Accounting API and bank feeds, but they do not support fully embedded integration.


Why SMB ERPs do not support fully embedded integration: QuickBooks Online, Xero, FreshBooks, and similar SMB platforms were not architected with native app ecosystems. They expose APIs for reading and writing financial data, and bank feed modules for reconciliation, but they do not provide the extension frameworks, permissions models, or UI patterns required to host a third-party financial application natively inside the ERP. NetSuite, Microsoft Dynamics, Sage Intacct, Workday, and SAP S/4HANA were built with those frameworks because their mid-market and enterprise customers expect a partner app marketplace.


For the full coverage grid across all 30+ supported platforms, see[Rutter’s platform coverage page](https://www.rutter.com/platform-coverage) .


## Which method should you use?


The decision usually comes down to two questions. First, what is the workflow you are delivering? Second, where does your customer work?


If your product needs to ingest data for a decision (lending, FP&A) or write transactions back to the books (bill pay, expense management), and your customers are happy logging into your product, use the Accounting API.


If your product is a source of transactions (a bank account, a payment processor) and your customer’s accounting team reconciles inside the ERP, use bank feeds integration.


If your customers are mid-market or enterprise and live inside their ERP, and your product needs to surface AP, treasury, reconciliation, or payments inside their existing workflows, use fully embedded integration.


Many fintechs and banks use more than one method, depending on which segment of their customer base they are serving. Rutter delivers all three through a single integration.


Rutter is the only platform that delivers all three integration methods through one integration layer. One contract, one API, every method your customers need. NetSuite, Microsoft Dynamics, Sage Intacct, Workday, SAP, plus 25+ others.


[→ See how it works](https://www.rutter.com/contact)


## Frequently asked questions


### What does it mean to integrate with an ERP?


“Integrate with an ERP” usually refers to one of three different methods. Accounting API integration uses a unified API to read from and write to the ERP. Bank feeds integration delivers transactions into the ERP’s bank feed module for reconciliation. Fully embedded integration runs a native financial application inside the ERP’s own UI.


### What is the difference between the Accounting API and bank feeds?


The Accounting API is bidirectional and covers many use cases (lending, FP&A, expense management, bill pay). Bank feeds is one-directional, serves only reconciliation, and depends on direct ERP listings or aggregator partnerships for distribution.


### What does ERP-native or fully embedded mean?


A fully embedded financial product runs inside the customer’s ERP, using the ERP’s own UI, permissions, and workflows. The customer never leaves their system of record to use the product. This is what enables mid-market and enterprise customers to coordinate financial operations from inside the ERP.


### Why do SMB ERPs like QuickBooks Online and Xero not support fully embedded integration?


SMB ERPs were not architected with native app ecosystems. They expose APIs and bank feed modules, but they do not provide the extension frameworks or UI patterns required to host third-party financial applications natively inside the product. Mid-market and enterprise ERPs (NetSuite, Microsoft Dynamics, Sage Intacct, Workday, SAP) were built with those frameworks because their customers expect partner app marketplaces.


### Which ERPs support fully embedded integration?


Fully embedded integration is available for 10 major ERPs today, including NetSuite, Microsoft Dynamics 365 Business Central, Microsoft Dynamics 365 F&O, Sage Intacct, Workday, Acumatica, Oracle Fusion ERP Cloud, Exact Online, and SAP S/4HANA.


### Does Rutter support my ERP?


Rutter supports 30+ ERP and accounting platforms. See our[Platform Coverage](https://www.rutter.com/platform-coverage) for the full grid, with each platform marked for which integration methods are available.


### Can a fintech or bank use more than one integration method?


Yes. Many fintechs and banks use multiple methods depending on the segment of their customer base. For example, a fintech might use the Accounting API for lending decisions, bank feeds for SMB customer reconciliation, and fully embedded integration for mid-market customers. Rutter delivers all three through a single integration layer.
