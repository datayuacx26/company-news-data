---
schema_version: "1.0.0"
document_id: "a2b9a741cf7a801827047a64c91112af85699f0e861454333e83596e87c07587"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/law-firm-accounting"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:14:47.490481+00:00"
fetched_at: "2026-08-06T14:14:48.910031+00:00"
content_hash: "sha256:d0817184a026ad7b1aba98883b788ce843da772a03c5d5818a415a567778c1ae"
---

# Law Firm Accounting: The CPA Firm's Trust Compliant Playbook

Law firm accounting is one of two client verticals where a bookkeeping mistake can lead to a client losing their professional license. The other is medical practices. That reality and specific compliance framework it produces is what separates law firm accounting from ordinary small-business bookkeeping.


Every state bar in United States requires attorneys to maintain client funds in a separate trust account, to reconcile that account on a defined cadence, and to produce records on demand. The **ABA Model Rule 1.15** and state rules built on top of it govern how those funds are held, tracked, and disbursed. A[CPA firm](https://www.finlens.app/accountants) serving a lawyer client is not just keeping books; it is producing workpaper that will be inspected next time state bar or a random audit lands.


## The IOLTA framework


An **Interest on Lawyers' Trust Account (IOLTA)** holds unearned client funds advance retainers, settlement proceeds, real estate escrow deposits, and any money belonging to a client or third party rather than firm. Interest earned on pooled IOLTA balance is remitted to state bar or a legal aid foundation, not to client or firm.


Every state has an IOLTA program. Most jurisdictions require attorneys to participate; a small number make it opt-in. The banking rules are set at state level, and bank offering account is required to comply with state IOLTA program's remittance rules.


Four accounts most law firms need:


1. **Operating checking account** firm revenue, operating expenses, payroll.
2. **Operating savings account** reserves.
3. **IOLTA trust account** client funds only.
4. **Business credit card** expense separation.


Comingling firm funds with trust funds even a $50 deposit into wrong account is a Rule 1.15 violation. Overdrawing trust account is a bar-reportable event in most jurisdictions.


## The three way reconciliation


The core discipline in law firm accounting is **three-way trust reconciliation** , performed monthly at minimum (some states require more frequent). Three balances must agree at each reconciliation date:


1. **Bank statement balance** for IOLTA account (adjusted for outstanding items).
2. **Trust account general ledger balance** total across all client sub-accounts in firm's books.
3. **Sum of individual client ledger balances** every client's specific trust balance, added up.


If all three do not tie to penny, reconciliation fails and underlying error must be found and corrected before workpaper is signed. Most state bars require reconciliation to be retained for five to seven years.


The rec fails most often for four reasons:


- A client deposit was recorded against wrong client ledger.
- A disbursement cleared bank before invoice was recorded.
- Bank fees were withdrawn from IOLTA (firm must reimburse trust account immediately and process any bank charges against operating account).
- A client refund was booked to operating account instead of clearing client's trust ledger


For a CPA firm, three-way reconciliation workpaper is deliverable. Signing it means numbers tie and underlying subledgers are supported.


## Cash vs. accrual and M-1 reconciliation


Most law firms default to **cash method** for tax purposes under IRC §448 attorneys are personal service corporations, but law firms structured as partnerships or PLLCs with average annual gross receipts under §448(c) threshold ($31M for 2026, indexed) can use cash method.


The book method is often accrual for internal management reporting (to see WIP and receivables) and cash for tax filing. This creates a book-to-tax difference that flows through[Schedule M-1](https://www.finlens.app/blogs/book-to-tax-reconciliation) on partnership return every year:


- **Accrual book WIP and A/R** need to reverse for cash-basis tax income.
- **Accrual book A/P** needs to reverse for cash-basis tax deductions.
- **Client cost advances** treated as receivables on books need to be evaluated for tax treatment (see below).


Firms above §448(c) threshold, C-corp law firms, and any firm with material inventory must use accrual. This is rare for practicing attorneys but common for legal-services businesses that own their real estate or provide non-legal services alongside legal work.


## Client cost advances hard vs. soft


Client cost advances are one of largest recurring transactions on a law firm's books, and one of most-misclassified. Two categories


**Hard costs (advanced client costs).** Expenses paid on behalf of a specific client that will be billed back to that client and reimbursed. Filing fees, court costs, deposition transcripts, expert witness fees, medical records, service of process. These are treated as **loans to client** recorded as an asset (receivable), not as an expense. When reimbursed, receivable clears; there is no tax deduction and no tax income.


**Soft costs (indirect client costs).** Firm overhead loosely attributable to a case photocopies, postage, mileage, courier, telephone. These are usually expensed as incurred and, if billed back, produce a small amount of taxable revenue on reimbursement


The IRS treats hard costs as loans under **Boccardo v. Commissioner (56 F.3d 1016, 9th Cir. 1995)** leading case. Deducting hard costs as they are paid, rather than recording them as receivables, results in a book-to-tax difference and a common IRS examination adjustment


The chart of accounts must distinguish two categories. A single "case expenses" account that comingles them makes return preparer's life much harder every March.


## The chart of accounts law firms need


Beyond a standard SMB chart of accounts, a law firm's books need these specific accounts:


**Assets**


- IOLTA Trust Cash (contra to IOLTA Trust Liability on balance sheet; totals equal)
- Client Cost Advances (Hard Costs) receivable
- Accounts Receivable Client (billed but unpaid fees)
- Unbilled Fees / WIP (accrual method only)


**Liabilities**


- IOLTA Trust Liability Client (matched to IOLTA Trust Cash)
- Client Retainers on Deposit (if separate from IOLTA)


**Revenue**


- Attorney Fees Hourly
- Attorney Fees Flat Fee
- Attorney Fees Contingent
- Referral Fees
- Client Cost Reimbursements Soft Costs


**Expenses**


- Client Cost Advances Soft (photocopies, mileage, postage)
- Attorney Salaries
- Paralegal / Support Salaries
- Professional Liability Insurance
- CLE / Bar Dues / Licensing
- Legal Research (Westlaw, LexisNexis)
- Practice Management Software (Clio, MyCase, PracticePanther)


Sub-ledgers by client and by matter are essential every trust account transaction, receivable, and WIP entry must be traceable to a specific client-matter combination.


## Fee arrangement mechanics


Four common fee arrangements, each with a different accounting flow:


**1. Hourly with retainer.** Client deposits a retainer into IOLTA. Attorney bills time. Invoice is generated. Attorney requests written client authorization (in most states) to withdraw earned fees from IOLTA and deposit to operating. Trust liability decreases; operating revenue increases.


**2. Flat fee.** Client pays a fixed amount. If fee is earned upon receipt (rare and jurisdiction-specific), it goes directly to operating. If not, it goes to IOLTA and is transferred to operating as work is performed.


**3. Contingent fee.** No fee received up front. Recovery is deposited to IOLTA. Firm's percentage (typically 33% to 40%) is transferred to operating; balance and any settled costs go to client. State-specific written fee agreement rules apply.


**4. Evergreen retainer.** Client maintains a minimum balance in trust. As fees are billed and transferred out, client tops up to minimum. Requires close monitoring a client who forgets to top up can push sub-account to zero and interrupt case work.


For all four, invoice is generated in[practice management](https://www.finlens.app/blogs/best-practice-management-software-accounting-firms-2026) system (Clio, MyCase, PracticePanther, Filevine), and associated journal entry lands in QuickBooks Online. The two systems must be reconciled practice management A/R and time reports need to match[QBO](https://www.finlens.app/resources/quickbooks-automation) general ledger every close.


## The common Rule 1.15 violations


Every state bar disciplinary board publishes most common trust account violations. The recurring pattern:


- Firm expenses paid directly from IOLTA (comingling).
- Earned fees left in IOLTA past a "reasonable time" (state-specific often 30 days).
- Client funds deposited to operating rather than IOLTA on receipt.
- Trust checks issued in excess of a client's individual balance ("borrowing" from other clients' funds this is fastest route to disbarment).
- Missing three-way reconciliation records.
- Bank fees debited from IOLTA without immediate reimbursement.


Every one of these traces back to a bookkeeping error, not a legal decision. The CPA firm serving attorney is last line of defense against a Rule 1.15 disciplinary complaint.


## How Finlens keeps a law firm client audit-ready


Finlens keeps QBO general ledger for law firm reconciled month-by-month with a workflow specifically designed for trust compliance.


- **Three-way reconciliation workpaper.** Finlens produces monthly three-way rec IOLTA bank statement, trust liability GL account, sum of client ledger balances with tie-out documented and any exceptions flagged for attorney to resolve.
- **Hard-cost vs. soft-cost tagging.** Every client-cost transaction is tagged in QBO with correct classification, so receivable side of balance sheet is accurate and M-1 book-tax difference is calculated on ledger.
- **Practice management integration.** Finlens reconciles Clio, MyCase, or PracticePanther A/R and trust balances to QBO every close, catching entries where two systems have drifted apart.
- **Rule 1.15 exception flagging.** Any transaction that looks like a common Rule 1.15 violation an operating expense paid from IOLTA, a fee left in trust past state's "reasonable time" window is queued for attorney review before it's posted.
- **Bar audit package.** When state bar or a random audit hits, three-way rec history, client ledgers, and trust-liability roll-forward are already produced and retained.


Finlens does not replace Clio or MyCase practice management side (timekeeping, invoicing, matter management) still lives in specialized tool. Finlens is ledger and workpaper layer that keeps trust account defensible.


## Conclusion


**Law firm accounting is trust-first bookkeeping.** The three-way rec, hard-vs-soft classification, and practice-management-to-QBO reconciliation are workpapers that decide whether client keeps their license.


Model Rule 1.15


trust compliance


3-way


monthly reconciliation


Hard vs. Soft


cost tagging


## IOLTA out of
tie?


Finlens produces the monthly three-way reconciliation, reconciles Clio or MyCase to QBO, and tags hard-vs-soft client costs correctly — so the state bar audit finds nothing to flag.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


‍ see how Finlens produces monthly three-way reconciliation, tags hard and soft client costs correctly, and reconciles Clio or MyCase to QBO every close.


Bring file for attorney client whose IOLTA has drifted out of sync with practice management A/R, whose hard costs are booked as expenses, and whose three-way rec hasn't been produced since last April. That's file this workflow is built for


## Frequently asked questions


### **Is IOLTA required for every law firm?**


**‍** In most states, yes for any attorney holding client funds. A small number of states are opt-in. Even in opt-out states, general trust account requirement applies; only interest-remittance mechanism differs.


### **How often should three way reconciliation be performed?**


Monthly at minimum. Some states require monthly explicitly; others require it "at reasonable intervals" but define reasonable as monthly in bar guidance. Weekly reconciliation is practical standard for firms with high trust account volume.


### **Can a law firm use cash basis accounting?**


Yes, for tax purposes, if average annual gross receipts are under §448(c) threshold ($31M for 2026). Book method is separate and often accrual for internal management. The book-tax difference flows to Schedule M-1.


### **What happens if trust account is overdrawn?**


It is a bar-reportable event in most jurisdictions. The bank is required to notify state bar when a trust check bounces. Overdrafts are single most common trigger for a disciplinary investigation.


### **Can hard client costs be deducted as expenses?**


Under Boccardo v. Commissioner (9th Cir. 1995), hard costs advanced on behalf of clients are loans, not deductible expenses. Deducting them creates a book-tax difference and a common IRS examination adjustment on law firm returns.


### **Does state bar audit my books?**


Most state bars conduct random audits of trust accounts, and every disciplinary complaint triggers a targeted review. Some states (New York, Illinois, others) require an annual attestation of trust compliance signed by attorney.


The authoritative source is[ABA Model Rule 1.15 Safekeeping Property](https://www.americanbar.org/groups/professional_responsibility/publications/model_rules_of_professional_conduct/rule_1_15_safekeeping_property/) . Every jurisdiction's trust account rule is built on this framework. For broader service-line pricing conversation with a law firm client, see Finlens guide to[bookkeeping services fees](https://www.finlens.app/blogs/bookkeeping-services-fees) .
