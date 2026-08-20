---
schema_version: "1.0.0"
document_id: "74dc869a10ad6b5cc2ed46f2e615b4d9871a7dbf6d98f0a4d3d6217afb40caaa"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/petty-cash-accounting"
published_at: "2026-08-15T00:00:00+00:00"
first_seen_at: "2026-08-16T08:31:25.828538+00:00"
fetched_at: "2026-08-16T08:31:27.308199+00:00"
content_hash: "sha256:2fef1290cb3dc5d40f694592f7f51b7a76e9eb431fb3378ee7912c8bb217781c"
---

# Petty Cash Accounting: How to Set Up, Track, and Reconcile Petty Cash Fund

**Petty cash is a small amount of physical currency a business keeps on hand for minor expenses too small to justify writing a check or using a corporate card** postage stamps, office supplies runs, employee lunch reimbursements, small vendor payments. The accounting mechanic is **imprest fund** total should always equal original petty cash amount, made up partly of cash and partly of receipts documenting disbursements. This guide walks through setup, journal entries, replenishment, reconciliation, and common mistakes.


Per[Investopedia's petty cash explainer](https://www.investopedia.com/terms/p/pettycash.asp) , imprest system is standard practice because it makes reconciliation simple: cash + receipts should always equal fund's original balance.


## Setting up petty cash fund


Two decisions at setup:


**How much?** Most small businesses run a petty cash fund of **$100–$500** . Larger businesses with heavy in-office petty spending might run $1,000. Too little means constant replenishment; too much creates unnecessary loss and security exposure.


**Who's custodian?** One person is designated to manage fund. That person keeps cash physically secure, records disbursements on a petty cash log, and requests replenishment when fund runs low. The controls fail if multiple people access fund independently.


The initial setup entry:


Debit


Credit


Petty Cash


(asset)


$200


Cash


(bank)


$200


Nothing hits P&L. The company just moved $200 from a bank account to a physical currency fund. Both are cash-equivalent assets on balance sheet.


## Recording disbursements from fund


Here's where petty cash accounting differs from other transactions. **Individual disbursements from petty cash are NOT recorded in general ledger as they happen.** Instead, custodian logs each disbursement on a paper or digital petty cash log with:


- Date
- Amount
- Purpose (office supplies, employee reimbursement, courier fee, etc.)
- Receipt attached (or noted if none is available)
- Recipient signature (for reimbursements)


The general ledger doesn't touch petty cash until **replenishment time** . This is imprest fund mechanic fund is refilled, and only then does accounting record aggregate expenses.


Example: over a month, custodian disburses:


- $28 for office supplies (Office Depot receipt)
- $15 for postage stamps (USPS receipt)
- $42 for employee lunch reimbursement (Chipotle receipt, employee signature)
- $80 for a courier delivery (invoice with signature)
- $12 for coffee run for a client meeting (Starbucks receipt)


Total disbursed: **$177** . Cash remaining in fund: $200 − $177 = **$23** . Total receipts and log entries: $177. Cash + receipts should equal $200 (original fund amount). If they don't, something is wrong.


## Recording replenishment


When fund is replenished, aggregate expenses hit general ledger and cash moves back into petty cash fund:


Debit


Credit


Amount


Office Supplies Expense


$28


Postage Expense


$15


Employee Meals Expense


$42


Delivery Expense


$80


Client Meeting Expense


$12


Cash (bank)


$177


Note: petty cash is NOT touched in this entry. The petty cash account balance stays at $200 (its original imprest amount). The bank account decreases by $177, and $177 flows to appropriate expense accounts on P&L.


Alternative entry if you prefer to route through petty cash (also valid):


Debit


Credit


Petty Cash


$177


Cash


(bank)


$177


Followed by expense recognition. Either method works most bookkeepers use direct expense-recognition method above because it's cleaner.


## Handling shortages and overages


If cash + receipts total doesn't match fund amount, you have a shortage or overage.


**Cash short** (fund is missing money): Debit Cash Short/Over Expense for difference. Common causes: wrong change given, unrecorded disbursement, theft.


**Cash over** (fund has extra money): Credit Cash Short/Over Income (or reduce Cash Short expense). Rare in practice.


For a $200 fund with $22 cash and $177 in receipts, total = $199. The fund is short by $1. Replenishment entry:


Debit


Credit


Amount


\[Various expenses\]


$177


Cash Short/Over


$1


Cash (bank)


$178


Persistent shortages (every replenishment cycle is short by $10–$30) point to control problems either poor receipt tracking or petty cash theft. Frequent shortages warrant reviewing custodian assignment and receipt discipline.


## The petty cash log format


A basic log has columns for:


Date


Description


Purpose (Account)


Amount


Receipt?


Recipient


3/2


Office Depot — pens/paper


Office Supplies


$28


Yes


3/8


USPS stamps


Postage


$15


Yes


3/15


Chipotle lunch — J. Smith


Employee Meals


$42


Yes


J.S.


3/22


Courier — legal docs


Delivery


$80


Yes


3/28


Starbucks — client mtg


Client Meals


$12


Yes


Digital petty cash logs in Excel, Google Sheets, or[accounting software](https://www.finlens.app/blogs/llc-bookkeeping-software) attachments work fine requirement is that log matches receipts and receipts match aggregate amount taken from cash.


## Petty cash controls


The controls that prevent petty cash from becoming a slush fund:


**1. One custodian.** Multiple people accessing fund without recording disbursements makes reconciliation impossible.


**2. Receipts required for every disbursement.** No receipt = no reimbursement. Exceptions for very small amounts (under $5) can be documented on log with a specific explanation. Receipt discipline matters both for internal control and for[business expense documentation](https://www.finlens.app/blogs/how-to-write-off-business-expenses) required at tax time.


**3. Signed acknowledgment for employee reimbursements.** If Jane got $42 for lunch, Jane signs log entry confirming she received it.


**4. Regular replenishment cycles.** Monthly is standard for most businesses. Longer intervals cause receipts to pile up and get lost.


**5. Reconciliation at each replenishment.** Cash + receipts = fund amount. If not, resolve discrepancy before replenishing.


**6. Independent verification.** Someone other than custodian should periodically count fund. Once a quarter is typical for small businesses.


**7. Written petty cash policy.** Even a one-page policy stating maximum disbursement amount (e.g., $50/transaction), acceptable purposes, and receipt requirements reduces ambiguity.


## Month-end reconciliation


At month-end, petty cash balance on trial balance should equal fund's imprest amount ($200 in example). If a replenishment happened during month, disbursement expenses hit appropriate expense accounts on that date.


The[monthly close](https://www.finlens.app/blogs/reconciliation-in-accounting) should confirm:


- Petty cash trial balance balance = imprest amount
- Petty cash physical count = trial balance amount
- No suspense items in Cash Short/Over unless documented


If trial balance shows $173 in petty cash when imprest is $200, fund was disbursed but not replenished do replenishment entry before closing period.


## When to eliminate petty cash


Many modern small businesses run without petty cash entirely, using corporate credit cards or expense reimbursement platforms instead. Eliminating petty cash simplifies bookkeeping and reduces theft/loss risk.


Signs you should eliminate petty cash fund:


- Corporate cards or debit cards are widely deployed
- Reimbursement processes (Expensify, Bill.com, or similar) handle small expenses
- Petty cash disbursements have dropped to <$50/month
- Shortages are persistent (control failures)


To eliminate: debit Cash (bank) for remaining balance, credit Petty Cash for imprest amount, and offset any residual receipts to expense accounts.


## Conclusion


**Petty cash accounting is imprest fund mechanics: fund equals cash + receipts, always. Log every disbursement, reconcile at replenishment, and never touch petty cash account balance until you replenish.** Get controls right (one custodian, receipts required, monthly replenishment) and petty cash stays simple. Get them wrong and small amounts add up to real losses over time.


## FAQ


### What is petty cash?


Physical currency a business keeps on hand for small expenses too minor to justify writing a check or using a corporate card. Typically $100–$500 in fund, managed by a designated custodian under imprest fund system.


### How do I record petty cash in accounting?


Set up a Petty Cash asset account on balance sheet at fund's imprest amount. Individual disbursements are logged on a petty cash log but NOT recorded in general ledger until replenishment. At replenishment, aggregate disbursement amounts hit appropriate expense accounts, and bank balance decreases.


### What is imprest fund system?


An accounting method where petty cash fund is always maintained at a fixed amount. Cash + receipts should always equal imprest amount. Replenishment covers total disbursed (in receipts) since last replenishment.


### Do I need receipts for petty cash?


Yes every disbursement should have a supporting receipt. This is both good internal control and IRS documentation for business expense deductions. Very small disbursements (under $5–$10) can sometimes be documented on log with a specific explanation.


### How often should I replenish petty cash?


Monthly is standard for most small businesses. Larger businesses with heavy petty spending replenish weekly or biweekly. Longer than monthly leads to receipts piling up and getting lost.


### What is a cash short/over account?


An account used to record differences between actual cash on hand and expected cash based on records. Short (missing cash) is an expense; over (extra cash) is income. Persistent shortages indicate control problems.


### Do I need a petty cash policy?


Yes for any business with employees. A basic policy states: maximum disbursement amount, acceptable purposes, receipt requirements, custodian responsibilities, and replenishment cycle. Even a one-page policy reduces ambiguity and enforces discipline.


### Should I still use petty cash in 2026?


Not necessarily. Many small businesses have eliminated petty cash in favor of corporate cards and expense reimbursement platforms. If petty cash disbursements are under $50/month and corporate cards cover most needs, eliminating fund simplifies bookkeeping.
