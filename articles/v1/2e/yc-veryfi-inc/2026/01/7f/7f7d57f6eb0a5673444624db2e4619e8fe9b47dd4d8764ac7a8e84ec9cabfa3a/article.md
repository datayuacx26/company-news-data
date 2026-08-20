---
schema_version: "1.0.0"
document_id: "7f7d57f6eb0a5673444624db2e4619e8fe9b47dd4d8764ac7a8e84ec9cabfa3a"
company_key: "yc-veryfi-inc"
company: "Veryfi, Inc."
source_id: "yc-veryfi-inc-news-import-05be82a7e37e"
canonical_url: "https://www.veryfi.com/technology/introducing-the-new-embedded-awards-engine/"
published_at: "2026-01-09T17:46:58+00:00"
first_seen_at: "2026-07-26T04:33:53.637378+00:00"
fetched_at: "2026-07-28T22:23:45.657833+00:00"
content_hash: "sha256:efafe9f6c146580a1fd3146c667c47482e4d99ce7940394fba2b508d88e5629e"
---

# Introducing the New Embedded Awards Engine

introducing-the-new-embedded-awards-engine


Design custom loyalty programs with a configurable receipt processing pipeline


The new Embedded Awards Engine is now live on QA and Staging!


This update introduces a major evolution in how Embedded processes receipts. Instead of fixed logic, customers now have full control over how receipts are validated, evaluated, and responded to, unlocking the ability to design truly custom loyalty and rewards programs.


## **Why We Built a New Awards Engine**


As loyalty programs become more sophisticated, customers need more than basic “receipt in → points out” logic. They need:


- Flexible validation rules
- Transparent award logic
- Custom responses tailored to each user and submission


The new Embedded Awards Engine was designed to give customers the building blocks to model their own receipt processing logic without sacrificing reliability or scale.


## **A Three-Step Receipt Processing Pipeline**


The new engine reframes receipt processing as a **three-step data pipeline** , configured directly in the **Rules** step of the campaign builder.


### **1. Validate**


Validation is the first gate in the pipeline. Customers can configure checks that run against:


- The campaign configuration
- The receipt itself
- The individual submitting the receipt


These checks ensure that only eligible receipts move forward in the pipeline, reducing fraud and enforcing campaign-specific requirements before any rewards are calculated.


### **2. Award**


The Award step is where receipts turn into rewards.


In this stage, Embedded:


- Identifies qualifying products on the receipt
- Generates reward points
- Evaluates award eligibility using configurable rules


If a user qualifies, Embedded records that award event and makes the data available for:


- UI visibility (coming soon)
- Data exports
- Downstream analytics and reporting


This separation of “points generation” and “award qualification” gives customers fine-grained control over how and when rewards are issued.


### **3. Respond**


Once processing is complete, customers can define exactly how Embedded communicates outcomes back to users.


Responses are powered by a templating system, allowing messages to dynamically include:


- Validation outcomes
- Award status
- Points earned
- User-specific information generated during processing


This makes it easy to deliver clear, personalized feedback – whether a receipt was accepted, rejected, or rewarded.


## **What’s Changed in the Campaign Builder**


This release updates **only the Rules section** of the campaign builder. The new rules builder enables customers to visually model the **Validate → Award → Respond** pipeline when creating campaigns.


A few important notes:


- The new rules builder is available only on new SMS and Web campaigns
- WhatsApp campaigns are not yet supported
- Award and point data is not yet visible at a user-by-user level in the UI (this is coming next)


## **What This Unlocks**


At a high level, the new Embedded Awards Engine gives customers everything they need to design and run **custom loyalty programs** , including:


- Multi-step receipt logic
- Campaign-specific reward models
- Personalized user messaging
- A foundation for richer user-level reward insights


This is a foundational step toward more powerful, flexible, and transparent loyalty experiences built on Embedded.


Currently the rules engine is in invitation only testing, if you’re interested in evaluating it against your business use case, reach out to support@veryfi.com


Hang tight, more updates coming soon!
