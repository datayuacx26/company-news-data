---
schema_version: "1.0.0"
document_id: "534de829990bdfe8b4b9dd02a963e3ec978f7696f0aaa3fe89fad329b82c54a7"
company_key: "yc-inscribe"
company: "Inscribe"
source_id: "yc-inscribe-news-import-71d84a865bd8"
canonical_url: "https://www.inscribe.ai/blog/llms-are-powering-the-next-leap-in-parsing"
published_at: "2025-09-29T00:00:00+00:00"
first_seen_at: "2026-07-22T00:02:47.549015+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:164651c2e81bd335d0899c0b721b0a95304eaaf5ea65cae8e61d9c929c1a1e04"
---

# LLMs are powering the next leap in parsing

At Inscribe, we know that accurate document understanding is the foundation of fraud detection. Better parsing means sharper detection, and as LLM technology advances, so do we. Over the past few months, we’ve rolled out upgrades that significantly improve parsing accuracy across a wide range of document types.


## Broader parsing coverage across documents


In our last update, we shared[how LLMs boosted bank statement parsing](https://www.inscribe.ai/blog/how-llms-boosted-our-bank-statement-parsing-coverage-by-up-to-5x) . Now, we’ve expanded that progress to payslips, invoices, utility bills, tax forms and IDs, while also improving parsing for business filings, financial statements, cheques, investment statements, leases, credit card statements, benefits statements, social security cards, and more. These improvements strengthen the extraction of key fields like names, dates, and addresses — the very details fraudsters are most likely to manipulate.


### Payslip parsing


We are very excited to announce that we have *just* released our enhanced payslip parsing solution! We will have metrics to share soon, but already we are seeing improvements in coverage and accuracy for name, pay date, address, gross pay, net pay, and pay period. We have also expanded our parsing solution to cover overtime pay, bonus pay, commission and pre-401k pay.


### Invoice parsing


Invoices now benefit from significant leaps in accuracy:


- Name coverage: **82% → 92%**
- Address coverage: **67% → 86%**
- Date coverage: **56% → 90%**


Over **98% of invoices** now return parsed details, with new fields like seller name (96%), total invoice amount (92%), and seller address (82%) captured at scale.


### Utility bill parsing


Utility bill parsing is now live for all customers, with performance gains across key fields:


- Name coverage: **88% → 97%**
- Address coverage: **75% → 97%**
- Date coverage: **77% → 93%**


We’re also capturing richer context like service provider name (92%), account number (92%), provider URL (83%), and amount due (73%).


### Tax form parsing


Tax form parsing has expanded to new subtypes and achieved step-change improvements:


- W2 EIN values: **44% → 99%**
- 1040 total income: **43% → 96%**
- Schedule C total other expenses: **22% → 96%**
- 1120 corporation name: **27% → 96%**
- 1120-S corporation address: **4% → 97%**
- 1065 ordinary business income: **26% → 99%**


### ID parsing


ID parsing now delivers stronger accuracy across the fields most critical for identity verification:


- Name coverage: **73% → 89%**
- Address coverage: **50% → 68%**
- Issue date coverage: **50% → 86%**


Alongside the issue date, we have also improved our parsing of date of birth (84%) and expiry date (73%). Beyond these core fields, we now extract the document’s country code (93%) and number (92%).


## What this means for fraud detection


Parsing improvements might not grab headlines like deepfake scams or synthetic fraud, but they’re the bedrock of effective fraud detection. When institutions can trust the accuracy of extracted names, addresses, and dates across invoices, utility bills, identity documents and tax forms, they can catch fraud signals earlier and with more confidence. Additionally, these improvements in name and address parsing assures that we have a more reliable foundation for validating identities.


With these updates, Inscribe customers benefit from stronger detection, broader coverage, and faster insights, ensuring fraudsters have fewer places to hide.


**Want to start your free trial?**[Request access today](https://www.inscribe.ai/demo-request) **.**
