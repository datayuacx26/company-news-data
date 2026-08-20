---
schema_version: "1.0.0"
document_id: "71c5313d2dc123735010b0f01d8eba83d50b49a2b49399a0d2d8f779d3c4f939"
company_key: "yc-inscribe"
company: "Inscribe"
source_id: "yc-inscribe-news-import-71d84a865bd8"
canonical_url: "https://www.inscribe.ai/blog/how-to-use-configurable-transaction-and-balance-insights-to-detect-high-risk-applicants"
published_at: "2025-11-14T00:00:00+00:00"
first_seen_at: "2026-07-22T00:02:47.549015+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:1d1ba6b1a68c8eb293c55b2a2abd33b6c7a8bff0d65ee97c3318027cabbed70f"
---

# Why high-risk applicants slip through and how custom transaction insights can stop them

Bank statement fraud is not an edge case. The FBI reported nearly $12.5 billion in fraud losses from U.S. consumers in just the first three quarters of 2025, a 25% year-over-year increase. Our[2026 Document Fraud Report](https://www.inscribe.ai/reports/2026-document-fraud-report) found roughly 1 in 16 documents showing signs of manipulation, with bank statements flagged as the top concern by 85.6% of fraud and risk leaders.


The problem isn't just fraud volume. It's that most detection tools weren't built with your risk policies in mind. What's a red flag at one institution is a non-issue at another. And when the tools don't reflect that, high-risk applicants slip through.


That's what custom transaction insights are designed to fix.


## ‍ **Deep bank statement analysis, built around your risk criteria**


A bank statement tells you a lot more than a balance. It shows you how someone behaves with money over time, and that's usually where the real picture emerges. It's worth noting that over[90% of flagged documents contain altered financial details](https://www.inscribe.ai/reports/2026-document-fraud-report) , meaning the risk is almost always hiding in the numbers.


With custom transaction insights, you define what you're looking for.


Build rules based on transaction category, merchant, payment processor, method, amount, or timeframe. Fire them based on raw transaction count or percentage of total activity. Set balance-based thresholds, track average daily balance, or flag volatility against a rolling average.


Once your signals are live, Inscribe runs the analysis automatically. No manual cross-referencing. No relying on reviewers to remember your institution's specific criteria. The flags are there when they open the file.


You're not looking at a generic risk score. You're looking at a bank statement through the lens of your own policies.


## **How fraud and risk teams are putting it to work**


A few examples based on how Inscribe customers are already using this:


### Consumer lending (B2C)


Teams commonly create insights that flag:


- **Overdrawn balance:** Detect if an applicant has overdrawn their account at any point in the uploaded statement.
- **Self transfers:** Flag when more than 25% of transactions are self-transfers, which can indicate attempts to inflate account activity.
- **Daily balance volatility:** Identify accounts where daily balances fluctuate by more than 50% of the average daily balance over a 30-day period.
- **Loan repayments:** Flag applicants whose transaction history contains an unusually high percentage of loan repayments.
- **NSF transactions:** Surface accounts with multiple non-sufficient funds (NSF) transactions.
- **Risky spending:** Detect transactions categorized as gambling or cryptocurrency.


### Business lending (B2B)


Commercial lenders often configure different rules, such as:


- **Overdrawn balance:** Detect whether the business account has ever been overdrawn.
- **Daily balance volatility:** Flag businesses with highly volatile cash flow over time.
- **Large cash deposits:** Surface accounts with cash deposits exceeding a defined threshold.
- **NSF transactions:** Identify businesses with repeated NSF activity.
- **MCA transactions:** Detect evidence of Merchant Cash Advance repayments.


Before enabling any insight, you can backtest it against historical applicants to understand exactly how many customers would be flagged and fine-tune thresholds before rolling it into production.


What's more, before anything goes live, you can backtest against your existing applicant population to see exactly how many would be flagged and what your flag rate looks like.


## **You control how these insights affect the risk rating**


Inscribe produces a customer-level risk rating across all submitted documents. With custom transaction insights, you choose whether each signal rolls into that rating or surfaces as additional context.


This also means you can test new signals without affecting decisions, phase in stricter rules gradually, or keep certain flags as FYIs before they influence outcomes automatically.


## **Built for the way fraud actually works**


Off-the-shelf document fraud solutions tell you what they were trained to flag. But fraud has gotten more layered.[60% of cases now involve both identity and document falsification](https://www.complianceweek.com/opinion/a-snapshot-of-the-state-of-financial-crime-in-the-united-states/36498.article) , up nearly 20% year over year.


Catching it requires detection that reflects how risk actually shows up at your institution, not someone else's.


Interested in learning more?[Book time with us](https://www.inscribe.ai/demo-request) .
