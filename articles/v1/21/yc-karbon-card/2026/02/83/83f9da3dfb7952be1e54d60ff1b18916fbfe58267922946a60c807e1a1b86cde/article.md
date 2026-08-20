---
schema_version: "1.0.0"
document_id: "83f9da3dfb7952be1e54d60ff1b18916fbfe58267922946a60c807e1a1b86cde"
company_key: "yc-karbon-card"
company: "Karbon Card"
source_id: "yc-karbon-card-news-import-50a5f6bde3bc"
canonical_url: "https://www.karboncard.com/blog/accounting-foreign-vendor-payments"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-22T01:15:46.648745+00:00"
fetched_at: "2026-07-28T22:20:47.930048+00:00"
content_hash: "sha256:9ca6deb0c8c5e311761856bedb5393762fdfd8f7cb3aeee81c23a04c3d6f1cba"
---

# Master Foreign Vendor Accounting for Indian Freelancers & Agencies

## Key takeaways


- Always record the invoice at the spot rate on invoice date, then capture realised exchange differences and bank charges on the payment date.
- Revalue open AP at period end if you prepare monthly financials, this shows true INR liabilities and separates unrealised gains or losses.
- Keep bank charges separate from FX gains or losses, that way you can measure and negotiate payment costs clearly.
- Document your rate source policy, RBI reference rate, bank rate, or a reliable market feed, then apply it consistently.
- Maintain airtight documentation, invoices, contracts, bank remittance proofs, and[Form 15CA and 15CB](https://www.karboncard.com/blog/difference-between-15-ca-and-15-cb) , so your entries are audit ready.


## Who this guide is for


This walkthrough is written for solo freelancers, design studios, marketing agencies, and small software shops in India who pay foreign vendors regularly. You might be:


- [Hiring overseas contractors for overflow work or specialized skills your local team does not have](https://www.karboncard.com/blog/paying-international-vendors) .
- Subscribing to international SaaS platforms for project management, design tools, or CRM.
- Purchasing stock assets, plugins, or templates from global marketplaces.
- Working with foreign consultants on business strategy, branding, or technical audits.


If you use Tally, Zoho Books, QuickBooks, or spreadsheets, the same core principles apply to[foreign currency AP entries](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


## Quick glossary in plain English


- **Functional currency:** INR for most Indian businesses.
- **Foreign currency:** The currency you are billed in, USD, EUR, GBP, and so on.
- **Spot rate:** Exchange rate on invoice date.
- **Settlement date:** The actual bank debit date.
- **AP:** Liability you owe to vendors until paid.
- **Exchange differences:** Gains or losses when rates move between invoice and payment.
- **Bank charges:** Wire, intermediary, or SWIFT fees.


> Speak one language in your books, INR, yet respect the currency of your vendor. Consistency in dates, rates, and documents turns complexity into clarity.


Learn the journal logic here:[What is the journal entry for foreign currency transactions?](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions)


## Setup before you post entries


- *Pick one exchange rate source* and stick to it, RBI reference rate, your bank’s published rate, or a market feed. Document the policy.
- *Confirm your chart of accounts* includes AP, expense or asset accounts, Exchange Gain or Loss, Bank Charges, and optionally Unrealised FX Gain or Loss.
- *Gather documents* , invoice, contract or SOW, remittance advice, bank statement lines, and[Form 15CA and 15CB](https://www.karboncard.com/blog/difference-between-15-ca-and-15-cb) where applicable.


Reference primer,[journal entry for foreign currency transactions](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


## Core how to, the entries step by step


### 1. Initial purchase entry, foreign currency


Record the liability on the invoice date at the spot rate. This locks the INR amount for the expense and AP on that day.


```text
Date: Invoice date
Dr: Expense/Asset (e.g., Contractor Expense)    FC amount × Spot rate
Cr: AP – Foreign Vendor                         FC amount × Spot rate


```


*Example* : USD 1,000 at 83 on 1 March.


```text
Date: 1 March
Dr: Contractor Expense    83,000 INR
Cr: AP – Vendor X         83,000 INR


```


Foundational reading,[foreign currency purchase entries](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


### 2. Month end revaluation if invoice remains unpaid


Revalue open AP at closing rate to show true INR liability. This creates *unrealised* gains or losses.


```text
Dr or Cr: AP – Foreign Vendor     (Closing rate − Spot rate) × FC amount
Cr or Dr: Unrealised FX Gain/Loss (balancing)


```


Choose a reversal policy and keep it consistent. More on the logic,[FX revaluation entries](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


### 3. Settlement on payment date


Clear AP at its carrying INR value, record bank charges, and post the *realised* exchange difference as the balancing figure.


```text
Dr: AP – Foreign Vendor          (Carrying INR from step 1 or 2)
Dr: Bank Charges                 (Fee in INR)
Cr: Bank                         (Total INR outflow)
Dr or Cr: Realised FX Gain/Loss  (balancing)


```


*Example continued* : Pay USD 1,000 at 85 on 15 March, fee INR 500. Bank outflow 85,500, AP carries 83,000.


```text
Date: 15 March
Dr: AP – Vendor X        83,000 INR
Dr: Bank Charges            500 INR
Cr: Bank                 85,500 INR
Dr: Realised FX Loss      2,000 INR


```


Deep dive,[payment settlement entries](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


### 4. Partial payments and advances


Record advances to a separate asset, apply them to invoices at the chosen policy rate, and capture any FX difference on application or settlement.


```text
Advance:
Dr: Advance to Vendor     FC × Advance date rate
Cr: Bank                  Actual INR outflow


Apply advance:
Dr: AP – Foreign Vendor
Cr: Advance to Vendor


Final settlement:
Record fees and realised FX on the remaining balance


```


Concept refresher,[forex gain loss accounting](https://www.karboncard.com/blog/forex-gain-loss-accounting) .


### 5. Credit notes and returns


Reverse the relevant part of the original entry at the credit note date rate, then recognise any FX difference. If a refund arrives, book it at the actual bank rate and capture fees separately.


```text
Dr: AP – Foreign Vendor
Cr: Expense/Asset
Dr or Cr: FX Gain/Loss (balancing)


```


Additional perspective,[FX payments in accounts payable](https://www.highradius.com/resources/Blog/fx-payments-in-accounts-payable/) .


## Numerical walkthroughs, practical mini cases


### Case A, USD 1,000 invoice and INR depreciates


*Timeline* : Invoice 1 March at 83, month end 31 March at 84, payment 5 April at 85, fee INR 500.


```text
1 Mar (invoice):
Dr Expense 83,000
Cr AP 83,000


31 Mar (revaluation):
Dr AP 1,000
Cr Unrealised FX Loss 1,000


5 Apr (payment):
Dr AP 84,000
Dr Bank Charges 500
Cr Bank 85,500
Dr Realised FX Loss 1,000


```


P&L equals 85,500, which matches cash outflow, cleanly split across expense, FX, and fees. See,[journal entries for FX transactions](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


### Case B, EUR 500 monthly SaaS auto charge, no revaluation


Auto debit on charge date means no AP, expense plus fee post immediately.


```text
1 Apr at 90, fee 200:
Dr Software Subscription 45,000
Dr Bank Charges             200
Cr Bank                  45,200


```


Background reading,[FX payments in AP](https://www.highradius.com/resources/Blog/fx-payments-in-accounts-payable/) .


### Case C, USD 1,000 invoice with USD 300 advance and USD 700 final payment


```text
1 Feb advance at 83:
Dr Advance to Vendor 24,900
Cr Bank 24,900


1 Mar invoice at 83:
Dr Contractor Expense 83,000
Cr AP 83,000


1 Mar apply advance:
Dr AP 24,900
Cr Advance to Vendor 24,900


15 Mar final payment, 700 at 85, fee 300:
Dr AP 58,100
Dr Bank Charges 300
Cr Bank 59,800
Dr Realised FX Loss 1,400


```


Why the differences happen,[forex gain and loss accounting](https://www.karboncard.com/blog/forex-gain-loss-accounting) .


## Where each line lands in your reports


- **Expense or Asset** hits P&L, or Balance Sheet if capitalised.
- **Exchange differences** sit in Other Income or Expenses, or Finance Costs, be consistent.
- **Bank charges** go to Finance Costs or Admin Expenses, never mix with FX.
- **AP, Foreign Vendor** remains a current liability until paid.


Reference,[journal entry mapping for FX](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


## Indian specific compliance notes


- **GST Reverse Charge on imported services** , assess applicability, record GST payable and input credit correctly, then reconcile with GSTR filings.
- **Form 15CA and 15CB** , file and retain for eligible remittances, see[difference between 15CA and 15CB](https://www.karboncard.com/blog/difference-between-15-ca-and-15-cb) .
- **Purpose codes and documents** , align country, description, and INR amounts across forms to avoid delays.


More context,[foreign transaction entries](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


## Common mistakes to avoid


- Using payment date rate for the initial purchase entry, always use the invoice date spot rate.
- Skipping month end AP revaluation, this understates or overstates liabilities.
- Netting bank charges into FX differences, track them separately to control costs.
- Ignoring intermediary bank charges that short pay vendors, recognise and settle the shortfall.
- Switching rate sources month to month, document one policy and stay with it.


Further reading,[AP FX best practices](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


## Reconciliation checklist


- Vendor ledger ties to invoices and credit notes, one to one mapping.
- Bank statement matches outflows and remittance advice, including fees.
- Exchange differences posted and explained, realised for settlements, unrealised for open AP.
- Bank charges logged and matched to statements.
- Open AP revalued or reversals posted correctly.


Good refresher,[FX in AP workflows](https://www.highradius.com/resources/Blog/fx-payments-in-accounts-payable/) .


## Tool specific tips


- **Tally** , enable foreign currency ledgers with bill wise tracking, verify auto calculated FX, and record bank charges as separate lines.
- **Zoho Books and QuickBooks** , set vendor currency, override fetched rates to match your policy, map Bank Charges and Realised FX Gain or Loss to distinct categories, run unrealised reports at month end and post journals.
- **Spreadsheets** , maintain tabs for invoices, payments, AP ledger, compute FX differences with formulas, and reconcile totals to statements.


Broader perspective,[foreign exchange accounting guide](https://www.accountingseed.com/resource/blog/foreign-exchange-accounting-guide-for-businesses/) .


## Managing volatility and fees


- Stick to the invoice date rate policy, consistency beats cherry picking.
- Pay promptly to cut FX exposure, shorter cycles reduce realised differences.
- Track bank charges separately, use data to negotiate or switch channels.
- Consider forward contracts for large recurring payouts, lock in predictability.
- Apply the same principles to AR, record at invoice date rate, revalue open AR, and recognise realised differences at settlement.


> When receiving international client payments, clarity on rates and fees makes reconciliation effortless. Karbon Business offers mid market execution with transparent pricing, which helps separate exchange differences from bank charges cleanly in your books.


Learn more fundamentals,[forex gain loss accounting](https://www.karboncard.com/blog/forex-gain-loss-accounting) .


## Bringing it all together


Recording *AP journal entries for foreign vendor payments* becomes simple when you follow a three step rhythm.


First, record the invoice at spot rate on invoice date. Second, revalue open AP at month end if you close monthly. Third, settle using the actual bank rate, post bank charges separately, and let the balancing line be your realised FX difference.


With disciplined documentation, a single rate policy, and clean categorisation, your expense, FX impact, and fees tell a consistent story, your bank reconciliations tick, and audits turn into routine reviews.


## FAQ


### Which exchange rate should Indian freelancers use for foreign vendor bills, RBI rate, bank rate, or Xe type market feeds?


Pick one source and document it in a simple policy, then apply it every time. RBI reference rate is authoritative and public, bank rate mirrors your real settlement, market feeds like Xe are neutral benchmarks, consistency beats perfection. See this primer on[journal entries for foreign currency transactions](https://www.karboncard.com/blog/what-is-the-journal-entry-for-foreign-currency-transactions) .


### Invoice is in USD but I will pay next month, do I need month end revaluation in my AP?


If you prepare monthly financials, yes, revalue the open AP at the month end rate and post unrealised FX gain or loss, it keeps your balance sheet honest and avoids surprises at settlement.


### How do I post bank charges separately when my Indian bank shows a single INR debit for the USD payment?


Most statements show two lines, one for the converted amount and one for fees, credit Bank for the total, debit Bank Charges for the fee, clear AP at carrying value, and plug the realised FX difference as the balancing figure.


### What if the intermediary bank deducts charges and my overseas vendor receives short payment?


Record the intermediary fee to Bank Charges and recognise the remaining amount as payable to the vendor, then send the shortfall, this keeps vendor reconciliation clean and avoids disputes.


### I follow cash basis accounting, how should I treat foreign payments then?


On cash basis, book the expense at the actual INR outflow on payment date, there is no separate AP or revaluation, the FX impact is embedded in the expense itself. For background see this overview of[accounts payable journal entries](https://www.kodo.com/blog/accounts-payable-journal-entries-guide) .


### Can I club bank charges with FX gain or loss to reduce ledger clutter?


No, keep Bank Charges separate from FX Gain or Loss, otherwise you cannot analyse payment channel costs or negotiate better pricing with your bank or platform, a clean P&L category for fees is essential.


### How do partial payments or advances work in journals for foreign vendors in India?


Record advances to an Advance to Vendor asset at the advance date rate, apply them to the invoice at your policy rate, then settle the balance with realised FX and fees, document each step for audit clarity.


### What documents should I keep when paying international contractors from India?


Keep the vendor invoice, contract or SOW, bank remittance advice, bank statement entries, and applicable[Form 15CA and 15CB](https://www.karboncard.com/blog/difference-between-15-ca-and-15-cb) , this pack answers most audit questions in minutes.


### How do I reduce FX impact and fees when paying or receiving internationally as a freelancer?


Pay faster to cut exposure, track fees by channel, and consider platforms that use mid market rates with transparent pricing. For example, many freelancers prefer Karbon Business for receiving client payments because it offers live rates with clear fees, fast settlement, and WhatsApp tracking, which simplifies reconciliation.


### If I auto pay a EUR SaaS every month, should I still create AP and do revaluation?


No, for instant card charges you can book expense at the INR amount on the charge date, debit Bank Charges for the fee line, and credit Bank for the combined outflow, there is no open AP and no revaluation.


### What is the simplest way to explain realised versus unrealised FX for my CA or client?


Unrealised FX appears when you revalue unpaid invoices at period end, realised FX appears when you actually pay and the bank applies its rate, one is a paper movement, the other is a cash confirmed difference.


### Will tools like Tally, Zoho Books, or QuickBooks auto calculate these FX differences for AP?


Yes, multi currency features can compute realised FX on payment and help with revaluation reports, still, confirm the rates match your policy and always book Bank Charges as a separate line for accuracy.


### Can Karbon Business help me keep cleaner books for international inflows from clients?


Yes, by providing mid market execution, transparent platform fees, and quick settlement to your Indian account, Karbon Business helps isolate exchange differences from charges, so your journal entries and reconciliations stay precise.


### How do I map these AP lines to financial statements for quick reviews with investors?


Expense or Asset hits P&L or Balance Sheet if capitalised, AP sits in current liabilities, Bank Charges in Finance or Admin costs, FX differences in Other Income or Expenses, be consistent in placement every year for comparability.


### Do I need a forward contract as a freelancer paying the same USD amount every month?


Not always, but if volatility is hurting margins, ask your bank about a small forward to lock a predictable rate, or pay faster to reduce exposure, maintain a simple policy that you can execute reliably.
