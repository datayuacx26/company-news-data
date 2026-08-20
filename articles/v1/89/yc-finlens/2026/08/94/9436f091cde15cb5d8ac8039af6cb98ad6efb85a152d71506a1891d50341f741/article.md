---
schema_version: "1.0.0"
document_id: "9436f091cde15cb5d8ac8039af6cb98ad6efb85a152d71506a1891d50341f741"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/construction-accounting"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:14:47.490481+00:00"
fetched_at: "2026-08-06T14:14:48.910031+00:00"
content_hash: "sha256:c4f633f091b02c8ca872050d176ba212fc1f7f6a905e761fa97ffc7f44a22822"
---

# Construction Accounting: From Job Costing to Revenue Recognition

Construction accounting is vertical where a bookkeeping close is not a summary of past month it is a re-estimation of every open project's future. Every job is its own P&L, its own contract, its own timing of revenue and cost. The **WIP schedule** and **over/under billings** it produces are two workpapers that decide whether surety bond gets renewed, whether bank line stays open, and whether operator has any idea what business is actually earning.


## Why construction accounting is different


Every construction project is a separate contract with a specific price, cost estimate, schedule, and completion date. The GAAP question is not "what did we sell this month" but "how much of each open contract has been earned this month." Two consequences follow:


1. **Every dollar of revenue has to be tied to a specific job.** Cash receipts do not map to revenue on a one-to-one basis a contractor can be paid $500,000 in a month and have earned $300,000 (overbilled by $200K) or paid $200,000 and have earned $500,000 (underbilled by $300K).
2. **Every dollar of cost has to be tied to a specific job.** Labor, materials, subcontractors, equipment, and allocated overhead all attach to a job or an indirect pool. A P&L that lumps job cost into "Cost of Sales" without a job-level split is not a construction P&L.


The bookkeeping unit is **job** , not month. Everything else general ledger, WIP, P&L, tax return assembles from job-level data.


## The construction chart of accounts


A functional construction chart of accounts has three key dimensions:


**Revenue** (by job, contract, and phase where contract has phases):


- Contract Revenue Earned Job \[ID\]
- Change Order Revenue Earned
- Other Revenue (equipment rental, admin services, service work)


**Direct job cost** (attached to a specific job):


- Direct Labor
- Direct Materials
- Subcontractor Costs
- Equipment Costs
- Other Direct Costs (permits, engineering)


**Indirect job cost** (allocated to jobs):


- Indirect Labor (foreman rotation, general superintendents)
- Small Tools and Consumables
- Vehicle and Equipment (allocated across jobs)
- Job-Related Insurance (workers comp, GL by hour or by dollar)


**General and administrative** (never allocated to a job):


- Office Salaries
- Rent
- Marketing and Sales
- Owner Compensation


**Balance sheet accounts specific to construction:**


- Accounts Receivable Retainage (10% withheld until job complete)
- Costs and Estimated Earnings in Excess of Billings (underbillings asset)
- Billings in Excess of Costs and Estimated Earnings (overbillings liability)
- Accounts Payable Retainage Payable
- Contract Liability (customer deposits before work performed)


Every entry to general ledger tags to a **job number** in QBO Classes or a dedicated project accounting tool. Without job dimension, WIP cannot be produced.


## The two revenue recognition methods


Under **ASC 606** , most construction contracts are recognized under **percentage-of-completion (POC)** method using an input measure most commonly cost-to-cost.


**Cost-to-cost formula:**


```text
Revenue earned to date = POC % × total contract value.
Revenue   this   period = Revenue earned to date – revenue previously recognized.
```


**Example.** A $2M contract. Estimated total cost $1.6M. At month-end, actual costs incurred are $400K. POC = 400 / 1,600 = 25%. Revenue earned to date = $500K. If prior-period revenue was $300K, current-period revenue = $200K.


**Completed-contract method (CCM)** recognizes revenue only when contract is substantially complete. Under ASC 606 this is permitted only in narrow situations (short-duration contracts, or where reliable estimates cannot be made). For most GAAP-compliant contractors, POC is default.


For **tax purposes** , IRC §460 requires long-term contracts (contracts not completed within tax year they are entered into) to use POC. **Exception** : contractors with average annual gross receipts under §448(c) small-contractor threshold ($31M for 2026, indexed) may use **completed-contract or cash method for tax on non-home-construction contracts** , and home construction contracts are exempt from §460 regardless of size.


The book-tax difference for POC-book / CCM-tax contractors is often largest single M-1 reconciling item on return. Tracking it on ledger, not at return time, is point of construction accounting.


## The WIP schedule


The WIP schedule is workpaper surety, bank, and CPA all read first. It has one row per open contract and these columns:


Column


Definition


Contract price


Original + approved change orders


Estimated cost at completion


Latest estimate


Estimated gross profit


Contract price – estimated cost


Costs incurred to date


Actual job cost


POC %


Costs to date ÷ estimated total costs


Revenue earned to date


Contract price × POC %


Billings to date


Invoices issued to owner


Over/underbilling


Billings – revenue earned


**Overbilling** (billings > revenue earned) is a liability contractor has been paid for work not yet performed. Common in first half of a job when material deposits are billed early.


**Underbilling** (revenue earned > billings) is an asset contractor has performed work not yet invoiced. Common near end of a job when retainage and final punch list drag out billing.


Every month, every open contract's estimate is re-examined. A change in estimated cost at completion changes POC and produces a **catch-up adjustment** to current-period revenue a prior-period estimate change flows through current P&L, not restated.


The two most common WIP errors:


1. **Front-loading job cost.** A subcontractor bills a big portion of their contract early. If cost is capitalized as incurred without any completion review, POC will overstate progress and produce revenue that will need to reverse later.
2. **Missing job costs.** A material invoice hits AP but is not job-costed. Costs to date understate, POC understates, revenue understates in current period and will need catch-up when cost is finally job-costed.


## Retainage receivable that hides


Every construction contract includes retainage typically 5–10% of each progress billing withheld by owner until final completion. Retainage sits on balance sheet as **Accounts Receivable Retainage** and does not flow to regular A/R.


For a contractor doing $10M a year at 10% retainage, that's $1M sitting on balance sheet at any given time. Aging out this retainage knowing which jobs are 90 days past substantial completion with retainage uncollected is a monthly workpaper. Contractors go out of business waiting on retainage they never collected.


On payables side, retainage payable to subcontractors sits in **AP Retainage** and is released when sub reaches their contractual milestone.


Both retainage receivable and retainage payable are contract-specific. They cannot be netted against regular A/R and A/P on balance sheet under GAAP.


## Change orders and unapproved change orders


Every construction contract accumulates change orders. GAAP treatment under ASC 606:


- **Approved change orders** (owner signed) add to contract price and estimated cost, flow through POC calculation normally.
- **Unapproved change orders in dispute** costs are incurred but price adjustment is not yet agreed. Under ASC 606, if collection is probable, include in transaction price up to amount that is highly probable. Track separately on WIP.
- **Claims** (contractor asserting damages against owner for delay, differing site conditions, etc.) recognize revenue only when collection is probable and amount is reasonably estimable, and never above costs incurred.


Every WIP schedule CPA firm signs should show change order status separately. Undisclosed unapproved change orders are single most common source of surety-bond disputes.


## Certified payroll and Davis-Bacon


Federally funded construction projects (over $2,000 in value, since Davis-Bacon Act 1931) require **prevailing wage payment** and **weekly certified payroll** on Form WH-347. State-funded projects often have parallel "little Davis-Bacon" requirements.


Requirements payroll process must handle:


- Prevailing wage rate by craft classification for specific project location
- Fringe benefit contributions (either paid in wages or via approved fringe plans)
- Weekly submission of WH-347 payroll certification
- Retention of certified payroll records for three years


Certified payroll doesn't change general ledger, but a contractor bidding on federal or state jobs without a compliance process will lose job or face back-wage assessments. QBO Payroll alone doesn't produce WH-347; a specialized payroll tool (or a QBO integration) is required.


## Sales and use tax on construction materials


Sales tax on construction materials is one of messiest areas of state tax. The general framework:


- **Time-and-materials contracts** contractor is a retailer of materials. Sales tax is charged to owner on material portion.
- **Lump-sum contracts** contractor is consumer of materials. Sales tax is paid by contractor at purchase and built into contract price.


But framework varies by state, and mixed contracts (which most are) get partitioned. Some states also have use-tax obligations on materials purchased out-of-state without sales tax collected. Every construction client operating across state lines needs a state-specific sales-tax analysis on largest project types.


## How Finlens keeps construction ledger current


Finlens reconciles QBO general ledger for construction contractors with a job-costing workflow that keeps WIP schedule current every month rather than reconstructed at year-end.


- **Job cost tagging.** Every material invoice, subcontractor bill, and labor entry is tagged to a job in QBO before it posts, so costs-incurred-to-date is a live number.
- **WIP schedule build.** Finlens produces monthly WIP with contract price, estimated cost at completion, costs to date, POC %, revenue earned, billings to date, and over/underbilling per job. Tied to trial balance.
- **Retainage tracking.** Retainage receivable and retainage payable stay in separate balance-sheet accounts, aged by job, with days-since-substantial-completion flagged for follow-up.
- **Change-order status.** Approved, pending, and disputed change orders are tracked separately and flow through WIP with correct GAAP treatment.
- **Book-tax parallel.** For §460-exempt small contractors on CCM or cash for tax, Finlens carries M-1 reconciling entries on ledger every close, so return preparer isn't rebuilding book-tax difference in March.
- **Job cost detail on P&L.** Job-level cost and revenue reports feed both a company P&L and a job-by-job profitability view, so operator sees WIP and job cost report every close.


Finlens does not replace Sage Intacct Construction, Foundation, or Procore operational side of project management stays in specialized tool. Finlens is ledger and workpaper layer that keeps QBO tied to job cost system.


## Conclusion


**Construction accounting is job-cost first, WIP second, everything else third.** The general ledger only tells truth about a contractor's business when every dollar is tagged to a job and every open contract is re-estimated at close.


see how Finlens tags job costs to QBO in real time, produces monthly WIP with over/underbillings by job, and carries retainage and change orders on ledger tied to trial balance.


WIP


monthly re-estimate


ASC 606


POC method


Retainage


aged from substantial completion


## WIP schedule
months stale?


Finlens tags job cost to QBO in real time, produces the monthly WIP with over/underbillings by job, and carries retainage and change orders on the ledger tied to the trial balance every close.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


‍


Bring file for contractor with $8M of open contracts, a WIP that hasn't been updated since Q1, $600K of retainage nobody's aging, and a §460-exempt tax method reconciliation being rebuilt every March. That's file this workflow is built for.


## Frequently asked questions


### **Can a small contractor use cash basis accounting?**


Yes for tax, if average annual gross receipts are under §448(c) threshold ($31M for 2026) and contractor doesn't have long-term contracts subject to §460 POC. Home construction contracts are always exempt from §460 regardless of size. For book purposes, GAAP requires POC for most contracts.


### **What is difference between an overbilling and an underbilling?**


Overbilling = billings issued > revenue earned. Sits on balance sheet as a contract liability. Underbilling = revenue earned > billings issued. Sits as a contract asset. Both are recomputed monthly from WIP schedule.


### **Do change orders get recognized before they're signed?**


Under ASC 606, unapproved change orders can be included in transaction price if collection is highly probable but amount recognized must be constrained to what is highly probable to be sustained. Most CPA firms require documentation of change order communication trail before recognizing any revenue.


### **How is retainage aged?**


Retainage receivable is aged from date of substantial completion, not invoice date. 90+ days past substantial completion is a follow-up trigger; 180+ days is a collection concern.


### **What accounting software works for construction?**


For small contractors under ~$10M revenue,[QuickBooks](https://www.finlens.app/resources/quickbooks-automation) Online + a job costing overlay (Knowify, BuilderTrend integration) is defensible. Above that, specialized tools Sage Intacct Construction, Foundation, Viewpoint Spectrum, or a Procore-plus-financial-integration setup become necessary.


### **Do I need to track certified payroll?**


Only for federally funded (Davis-Bacon), state-funded (little Davis-Bacon), or specific municipal projects. If contract references prevailing wages or WH-347, yes. Otherwise, no.


The authoritative tax reference for long-term contracts is[IRS Publication 538 Accounting Periods and Methods](https://www.irs.gov/publications/p538) , which covers §460 percentage-of-completion rules and small-contractor exception. For book-tax difference this creates every year on M-1, see Finlens guide to[book-to-tax reconciliation](https://www.finlens.app/blogs/book-to-tax-reconciliation) .


‍
