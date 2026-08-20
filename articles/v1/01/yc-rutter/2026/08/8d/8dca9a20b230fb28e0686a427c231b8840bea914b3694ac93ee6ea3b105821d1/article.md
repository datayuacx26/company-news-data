---
schema_version: "1.0.0"
document_id: "8dca9a20b230fb28e0686a427c231b8840bea914b3694ac93ee6ea3b105821d1"
company_key: "yc-rutter"
company: "Rutter"
source_id: "yc-rutter-news-import-c12c34cd87f8"
canonical_url: "https://www.rutter.com/blog/remittance-advice-payment-reconciliation"
published_at: "2026-08-18T00:35:47.404+00:00"
first_seen_at: "2026-08-07T11:22:37.990696+00:00"
fetched_at: "2026-08-07T11:22:39.903353+00:00"
content_hash: "sha256:0fa9f68a9e2eeba2afe6fff78787471b8146861df473b79463e9047dda496086"
---

# Remittance Advice: The Missing Link Between Payment and Reconciliation

Remittance advice is information a payer sends to explain what a payment covers. It may identify one invoice, several invoices, credit adjustments, tax amounts, discounts, or a partial payment. The payment moves funds. The remittance advice tells the recipient how to apply them.


Without that context, accounts receivable teams may receive the correct amount and still spend hours determining which open records to close. The problem becomes more common when one payment settles several invoices or when fees and deductions make the received amount differ from the invoice total.


## Remittance advice is not a payment confirmation


A payment confirmation proves that an instruction was submitted, accepted, or completed, depending on the system. Remittance advice describes the commercial records associated with the payment.


- **Payment instruction:** What money should move, where, and when?
- **Payment status:** Was the instruction accepted, settled, returned, or failed?
- **Remittance advice:** Which invoices, credits, or obligations does the payment cover?
- **ERP posting:** How should the event affect the books?


One message may contain more than one of these elements, but product teams should preserve the distinctions. A successful payment without usable remittance data can still create a cash-application exception.


## What remittance advice should contain


Useful remittance data depends on the use case. Common fields include payer and payee identifiers, payment reference, invoice number, invoice date, original amount, amount paid, currency, discounts, credits, tax details, and free-text notes.


Stable identifiers are more reliable than text. "April invoices" may be readable to a person and difficult to match. An invoice ID, end-to-end payment ID, and customer account identifier create a stronger join across systems.


ISO 20022 treats remittance as structured payment information. Its[remittance advice message definition](https://www.iso20022.org/sites/default/files/2020-12/ISO20022_MDRPart1_StandAloneRemittanceAdvice_2020_2021_v1_ForSEGReview.docx) allows an originator to provide details associated with a payment, including invoice line information. Remittance can travel with the payment or separately, as long as the recipient can reliably associate the two.


## Why ERP context matters


ERP records contain the invoices, customers, bills, vendors, credits, and accounts the payment needs to resolve. When remittance information reaches the ERP with stable identifiers, a system can propose or complete a match and retain the evidence beside the accounting entry.


Problems arise when the data is truncated, reformatted, or stored only in a bank portal. A bank statement description may preserve a payer name and short reference while losing the invoice-level breakdown. An emailed PDF may contain the detail but remain disconnected from the transaction.


Rutter's[Accounting API](https://www.rutter.com/product/accounting-api) gives fintech products read and write access to invoices, bills, payments, and related ERP records. Rutter's[Bank Feeds](https://www.rutter.com/bank-feeds) deliver transaction data into accounting systems. A complete reconciliation product needs a reliable way to associate the transaction with the relevant remittance and ERP records.


## One payment may settle many invoices


Batch settlement is where remittance advice earns its keep. Suppose a customer sends $48,500 against five invoices totaling $50,000 and applies a $1,500 credit. Amount matching alone sees a discrepancy. Structured remittance can explain the five allocations and the credit.


Partial payments create a related case. Advice should identify the invoice and amount paid without marking the entire receivable as settled. Overpayments may create a customer credit or unresolved balance depending on policy.


Nacha's[ACH file details](https://achdevguide.nacha.org/ach-file-details) show how addenda records can carry payment-related information for certain ACH uses. Available space and conventions vary, so product teams should test what survives through every bank, rail, and ERP involved in the flow.


## Design for separation and reassociation


Remittance advice sometimes arrives before the payment, after it, or through a different channel. The system needs an identifier that allows the records to reunite. Good candidates include a payment ID, end-to-end ID, customer reference, or another unique value agreed across the workflow.


Avoid matching on amount and date alone when the payment volume makes collisions plausible. Payer identity and currency help. Invoice references provide stronger evidence. Confidence should determine whether the product auto-applies the payment or proposes a match for review.


Corrections need their own path. A payer may send revised advice after noticing a misapplied invoice. The recipient should be able to update the allocation without losing the original message or bank transaction.


## Embedded reconciliation puts the evidence together


An ERP-native workflow can show the transaction, remittance details, candidate invoices, credits, and final accounting result in one place.[Rutter Embedded ERP](https://www.rutter.com/embedded-erp) supports banking, payment, and reconciliation experiences inside supported ERPs while banks and fintechs retain their own financial rails. A related guide explains how[automated reconciliation](https://www.rutter.com/blog/automated-reconciliation-banks-payments-erps) turns that context into matched records and reviewable exceptions.


Rutter's article on[embedded reconciliation infrastructure](https://www.rutter.com/blog/bank-feeds-embedded-reconciliation) focuses on getting transaction data into the accounting system. Remittance advice is the context layer that can make that transaction easier to apply once it arrives.


## What product teams should test


Test one-to-one, one-to-many, partial, overpaid, short-paid, credited, refunded, fee-adjusted, and foreign-currency scenarios. Confirm maximum field lengths and character handling on every rail. Check whether structured fields survive aggregator and bank transformations. Verify that late or corrected advice can be reassociated without duplicating the payment.


Track unapplied cash, automatic-match rate, time to apply, corrections, and remittance completeness. Those measures show whether the feature closes work or merely transports another document.


Remittance advice looks secondary because it does not move money. For finance teams trying to reconcile at scale, it carries the explanation that turns movement into an accounting event.
