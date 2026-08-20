---
schema_version: "1.0.0"
document_id: "e6fb9e1acf7078e9941ad2deb2d6c53238b0145129439b1de04b1c74ac1b9c40"
company_key: "yc-afternoon-co"
company: "Afternoon.co"
source_id: "yc-afternoon-co-news-import-d204747fe0e0"
canonical_url: "https://www.afternoon.co/blog/chart-of-accounts"
published_at: null
first_seen_at: "2026-07-21T20:47:37.461730+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:a6d7b5727722ba762fb9c5797d17281ba19f6b1e2e53539d5dcc1357b4a85463"
---

# Chart of Accounts 101: What Accounts Every SaaS Startup Actually Needs In 2026

A chart of accounts (COA) is the backbone of your accounting system. If your COA is too simple, your financials won’t answer basic questions. If it’s too complex, close turns into a monthly cleanup project.


This guide lays out the accounts most SaaS startups actually need, plus a few optional accounts that make sense once you hit real scale (usage billing, multi-entity, audit, sales tax, R&D credits).


Last verified: Jan 2026


---


## What a Chart of Accounts Is


A chart of accounts is the list of accounts your general ledger uses to categorize transactions. It’s how you turn raw bank and card activity into financial statements.


A good COA does three things:


- supports accurate financial statements
- makes month-end close faster
- maps cleanly to tax filings and audit requests


The IRS also emphasizes that good recordkeeping helps you prepare financial statements, identify income sources, track deductible expenses, and support items on your tax return:[IRS recordkeeping overview](https://www.irs.gov/businesses/small-businesses-self-employed/recordkeeping) .


---


## COA Design Rules That Keep You Out of Trouble


### Keep it minimal, then add accounts only when you need them


Start with the smallest COA that produces useful reporting. Add accounts when one of these is true:


- you need to track a category separately for decision-making (compute vs payroll vs contractors)
- it has different tax treatment (meals, fixed assets, sales tax)
- it gets asked for in diligence or audit (deferred revenue, AR, accrued expenses)


### Separate what is “timing” versus “classification”


Many accounting issues are not about what it is, but when it hits the P&L:


- prepaid expenses vs expense
- accrued expenses vs paid expenses
- deferred revenue vs revenue


A COA should make timing accounts obvious, so you can reconcile them monthly.


### Build for your billing model


A pure subscription business can run a simple revenue structure. The moment you have:


- usage charges
- annual prepay
- credits
- refunds and chargebacks you need a few extra accounts to keep revenue and AR accurate.


Revenue recognition under ASC 606 is a bigger topic, but the core principle (recognize revenue to depict transfer of promised goods/services) is the foundation for how you should structure revenue and deferred revenue accounts:[FASB Topic 606 (ASC 606) update PDF](https://storage.fasb.org/ASU%202016-10.pdf) .


---


## The Minimum COA Every SaaS Startup Needs


This is the baseline set that works for most early-stage SaaS startups and produces investor-usable financials.


## Balance Sheet Accounts


### Assets


Cash


- Cash
- Cash equivalents (optional)


Accounts receivable and contract balances


- Accounts receivable
- Allowance for doubtful accounts (optional; becomes useful when you have meaningful AR)
- Contract assets (optional; if you have ASC 606 timing differences)


Prepaids and other current assets


- Prepaid expenses
- Deferred offering costs (only if relevant)
- Deposits (rent/security deposits)


Fixed assets


- Computer equipment
- Furniture and fixtures (optional)
- Leasehold improvements (optional)
- Accumulated depreciation


Other assets (as needed)


- Capitalized software development costs (only if you are capitalizing eligible costs)
- Deferred contract costs (optional; if you capitalize certain sales commissions)


### Liabilities


Accounts payable and accruals


- Accounts payable
- Accrued expenses


Payroll and payroll taxes


- Wages payable
- Payroll taxes payable
- Employee benefits payable (optional)


Customer-related liabilities


- Deferred revenue
- Refund liability (optional, but useful if refunds are common)


Tax-related liabilities


- Sales tax payable (only if you collect and remit)
- Income tax payable (usually minimal for early-stage, but still possible)
- Franchise tax payable (state-specific)


Debt and other


- Credit card payable (if you prefer to track as a liability instead of netting into cash)
- Notes payable
- Accrued interest payable


### Equity


- Common stock
- Additional paid-in capital
- Retained earnings
- Current year earnings (system-generated)


---


## P&L Accounts Most SaaS Startups Use


### Revenue


Keep revenue categories aligned to how you sell and how you analyze growth.


A practical structure:


- Subscription revenue
- Usage revenue (if applicable)
- Professional services revenue (if applicable)


Contra revenue and adjustments (important)


- Refunds
- Discounts (or sales promotions)


If you report gross revenue and net it with contra accounts, you can see:


- how much you’re giving up in discounts
- how much churn or refund behavior is impacting revenue


### Cost of goods sold


COGS is the cost to deliver your product to customers. SaaS teams commonly include:


- Hosting and infrastructure (production)
- Third-party APIs used to deliver customer features (production)
- Customer support (depends on your policy; some classify support in OpEx)
- Payment processing fees (many treat as COGS if directly tied to revenue)


A common mistake is lumping everything into “software” expense. Split customer-delivery costs from internal tooling when you can.


### Operating expenses


The basic operating expense categories:


- Research and development
- Sales and marketing
- General and administrative


A practical COA uses those three buckets and then adds accounts that frequently need separate tracking:


- Payroll
- Contractors
- Software and tools
- Legal and professional fees
- Recruiting
- Travel
- Meals (separate from travel for tax reasons)
- Rent and occupancy
- Insurance
- Depreciation and amortization


The IRS maintains a mapping of business expense topics and resources (and notes changes to older publications):[IRS guide to business expense resources](https://www.irs.gov/forms-pubs/guide-to-business-expense-resources) .


---


## SaaS-Specific Accounts That Are Worth Adding Early


These are the accounts that make SaaS financials easier to trust.


### Deferred revenue


If you invoice annually upfront, or you collect cash before you deliver service, deferred revenue is the standard liability account you reconcile monthly.


### Accounts receivable and payment processor clearing


If customers pay via card/ACH through a processor, a clearing account prevents timing confusion:


- Payment processor clearing
- Payment processing fees (COGS or OpEx, depending on your policy)


This is especially helpful when payouts lag invoices.


### Refunds, chargebacks, and disputes


If refunds happen, separate these:


- Refunds (contra revenue)
- Chargebacks and disputes (either contra revenue or a dedicated expense, but be consistent)
- Refund liability (balance sheet, optional, if you have predictable refund exposure)


### Sales tax payable (only if you collect)


If you collect sales tax, it is not revenue. It is a liability:


- Sales tax payable


This becomes critical once you start collecting in multiple states or on platforms/marketplaces.


---


## Accounts for Usage-Based Billing and Credits


If you bill based on usage, add a few accounts so invoices and revenue tie out.


Revenue


- Usage revenue


Contra revenue


- Usage credits applied (optional; helps explain “why invoice was lower”)
- Refunds


Liabilities


- Deferred revenue (for prepaid credits sold)
- Contract liability for credits (some teams separate from general deferred revenue)


Assets


- Unbilled receivable (optional; if you recognize usage revenue before invoicing)


The goal is that your billing data can map to the ledger without manual reclassification each month.


---


## Accounts for Payroll, Hiring, and Contractors


SaaS startups often overcomplicate payroll accounts. You usually want:


- Payroll expense (by department)
- Payroll taxes
- Benefits
- Contractors (by department)


If you have international contractors, you may want to split:


- US contractors
- International contractors


That makes 1099 season and vendor reviews simpler.


---


## Accounts for Leases and Offices


If you have office space, equipment leases, or other leasing arrangements, your COA should support lease accounting and clean reconciliations.


At minimum:


- Rent expense
- Security deposits
- Leasehold improvements (if you build out an office)


Lease accounting requirements are governed by ASC 842:[FASB leases project page (Topic 842)](https://fasb.org/projects/current-projects/leases) .


---


## R&D Credit and How It Shows Up in Your COA


The R&D tax credit does not change how you run your COA day-to-day, but a good COA makes it easier to build the credit file because most startups’ qualified research expenses are heavily wage-based.


Practical COA implications:


- keep payroll separated by department (R&D vs non-R&D)
- avoid burying contractors in a generic “misc expense” account
- track third-party dev work cleanly


If you claim the federal R&D credit payroll tax offset, it is tied to employer payroll taxes, but the underlying inputs still come from wage and contractor tracking. The IRS overview is here:[IRS R&D credit against payroll tax for small businesses](https://www.irs.gov/credits-deductions/research-credit-against-payroll-tax-for-small-businesses) .


If you want to centralize your GL, AP, AR, tax, compliance, bookkeeping, R&D credit workflow, and sales tax tracking in one place, Afternoon is building towards an ERP-style platform that covers those functions.


---


## What You Probably Don’t Need Yet


A COA gets messy when you add accounts just because you can. Early-stage teams usually do not need:


- dozens of software sub-accounts by vendor (use vendor reporting instead)
- separate accounts for every marketing channel (use classes or tags, or a separate KPI model)
- overly granular travel sub-accounts
- separate deferred revenue accounts for every product line (unless pricing is meaningfully different)


If you need analysis, use dimensions (classes, departments, locations, projects) instead of adding 50 accounts.


---


## COA Naming and Numbering That Scales


A simple numbering scheme that scales:


Assets: 1000–1999
Liabilities: 2000–2999
Equity: 3000–3999
Revenue: 4000–4999
COGS: 5000–5999
Operating expenses: 6000–7999
Other income/expense: 8000–8999


Naming rules that help:


- use consistent prefixes (Revenue:, COGS:, G&A:, R&D:, S&M:)
- reserve “Other” accounts for truly rare items, then review quarterly


---


## Month-End Close: What You Reconcile with This COA


If your COA is set up well, close becomes a checklist of reconciliations:


Cash


- tie bank balances to cash accounts


AR and billing


- tie AR aging to accounts receivable
- confirm billing cutoffs (invoices issued vs revenue recognized)


Deferred revenue


- tie deferred revenue to billing schedules and revenue recognition model


Accrued expenses


- ensure major invoices and payroll liabilities are captured in the right period


Sales tax payable (if collecting)


- tie collected tax to filings/remittances


This is why the COA matters: it defines what you can reconcile.


---


## Summary


Topic Details


What a COA is The account structure your general ledger uses to categorize transactions


Goal Clean financial statements, faster close, easier taxes and diligence


Minimum accounts Cash, AR, prepaids, fixed assets, AP, accruals, payroll liabilities, deferred revenue, basic revenue/COGS/OpEx


SaaS must-haves Deferred revenue, refunds/discounts, payment processing clearing, hosting/infra COGS


Add for usage billing Usage revenue, credits/discounts, prepaid liabilities, optional unbilled receivable


Add for sales tax Sales tax payable (not revenue)


R&D credit tie-in COA helps by keeping wage and contractor costs cleanly tracked for documentation


---


## Official References


- [IRS recordkeeping overview](https://www.irs.gov/businesses/small-businesses-self-employed/recordkeeping)
- [IRS: What kind of records should I keep?](https://www.irs.gov/businesses/small-businesses-self-employed/what-kind-of-records-should-i-keep)
- [IRS Publication 583 (Starting a Business and Keeping Records)](https://www.irs.gov/publications/p583)
- [IRS Publication 583 PDF (Rev. December 2024)](https://www.irs.gov/pub/irs-pdf/p583.pdf)
- [IRS guide to business expense resources](https://www.irs.gov/forms-pubs/guide-to-business-expense-resources)
- [FASB Topic 606 update PDF (Revenue from Contracts with Customers)](https://storage.fasb.org/ASU%202016-10.pdf)
- [FASB leases project page (Topic 842)](https://fasb.org/projects/current-projects/leases)
- [IRS R&D credit against payroll tax for small businesses](https://www.irs.gov/credits-deductions/research-credit-against-payroll-tax-for-small-businesses)
