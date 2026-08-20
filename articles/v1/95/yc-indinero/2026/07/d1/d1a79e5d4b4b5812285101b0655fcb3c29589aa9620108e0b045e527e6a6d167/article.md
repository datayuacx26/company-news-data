---
schema_version: "1.0.0"
document_id: "d1a79e5d4b4b5812285101b0655fcb3c29589aa9620108e0b045e527e6a6d167"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/chart-of-accounts-for-saas/"
published_at: "2026-07-28T00:40:25+00:00"
first_seen_at: "2026-07-28T00:56:27.046171+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:015025a09a4d3448d74d584b1756b85090190e0dd745e24c47eea2f1c768e3e3"
---

# Chart of Accounts for SaaS Startups: A 2026 Template

## What Is a SaaS Chart of Accounts?


A SaaS chart of accounts is the master index of every general ledger account, organized into the GAAP financial-statement elements and arranged by a numbering convention so the financials roll up automatically. The conceptual basis for those elements is[FASB Concepts Statement No. 6, Elements of Financial Statements](https://storage.fasb.org/CON6_revised_11.28.2018.pdf) , which defines assets, liabilities, equity, revenues, expenses, gains, and losses. Every account in a SaaS COA is, at root, one of those elements. The numbering is just how you keep them in order.


What makes a SaaS chart distinct is not the elements but the account-level detail inside them. A software company carries deferred revenue, accrued revenue, and deferred cost of revenue, and almost never carries inventory. Its revenue lines split recurring subscription income from one-time services and usage. Its cost of revenue is dominated by cloud hosting, not raw materials.


A generic QuickBooks default template fails the test. It has no deferred revenue account, no hosting COGS line, and a single undifferentiated “Sales” account. As[Kruze Consulting](https://kruzeconsulting.com/startup-chart-accounts/) frames it, an analyst should be able to read your industry off your chart.


Account counts scale with stage. A seed-stage company runs lean at roughly 30 to 40 accounts. By Series A ($1M to $5M ARR) that expands to 50 to 70 with revenue breakouts and COGS detail, and Series B and beyond often reaches 80 to 100 or more once departmental tracking and multi-entity subaccounts come into play, per[DualEntry](https://www.dualentry.com/blog/saas-chart-of-accounts) . The structure stays constant. The granularity grows.


## The SaaS COA Structure


The standard SaaS chart of accounts uses a four-digit numbering convention where the first digit identifies the financial-statement class, with accounts running in balance-sheet-first, income-statement-second order. NetSuite, QuickBooks Online, and most mid-market ledgers share this logic. In[NetSuite’s own convention](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_N1440268.html) , all accounts beginning with 1 are assets, all beginning with 2 are liabilities, and so on, which keeps reporting consistent across periods. QuickBooks Online uses the same four-digit default once account numbers are enabled under Account and Settings, per[Intuit](https://quickbooks.intuit.com/learn-support/en-us/help-article/chart-accounts/use-account-numbers-chart-accounts/L7lRLN0Do_US_en_US) .


Here is the high-level SaaS chart of accounts template, with the caveat that exact range assignment varies by firm and tool while the classification logic does not.


Range Class SaaS-specific accounts


1000-1999 Assets Cash, AR, accrued / unbilled revenue (contract asset), deferred cost of revenue, deferred commissions, capitalized software, fixed assets


2000-2999 Liabilities AP, accrued liabilities, deferred revenue current, deferred revenue long-term, debt


3000-3999 Equity Common stock, preferred, APIC, retained earnings


4000-4999 Revenue Subscription, expansion, usage, setup, professional services, contra-revenue


5000-5999 COGS / Cost of revenue Hosting, embedded third-party software, CS / implementation labor, payment processing, amortization of capitalized software


6000-6999 Operating expense: R&D Engineering wages, dev tools, contractor engineering


7000-7999 Operating expense: S&M Sales wages, commissions, ad spend, marketing tools, events


8000-8999 Operating expense: G&A Exec, finance, HR, legal, rent, insurance, software


9000-9999 Other income / expense, tax Interest, FX, gains and losses, income tax


Where a firm draws the lines varies. Some collapse all operating expense into one band, and Kruze uses six-digit numbers with S&M and R&D in adjacent 600000 and 700000 ranges. The number itself is cosmetic. What matters is that each account is correctly classified, because that classification drives gross margin, operating margin, and every benchmark above and below them.


Two design rules apply across both tools. First, leave gaps between account numbers so related accounts can be inserted later without renumbering history. If your COGS accounts run 5000 to 5050, start the next at 5100, not 5051. Second, use parent and child subaccounts for discipline rather than a flat sprawl. QuickBooks Online writes this as` Hosting:AWS` , and NetSuite points a child at its parent via the subaccount field, per the[NetSuite chart-of-accounts management guide](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/chapter_N1439850.html) .


## SaaS-Specific Application


The SaaS accounting structure earns its keep in three account blocks: revenue segmented for ASC 606, COGS that isolates delivery cost, and operating expense split to match SaaS benchmarks. This is where a generic template and a purpose-built one diverge.


### Revenue accounts (4000s): segment by ARR type, tied to ASC 606


SaaS revenue must be segmented by recognition pattern, because ASC 606 cadence differs by stream and investors compute retention off the breakout. A workable block:


- **4000 Subscription revenue** (recurring, recognized ratably over the contract term)
- **4100 Expansion / upsell revenue** (recurring, separated so you can compute net revenue retention)
- **4200 Usage / overage revenue** (recognized as the usage is incurred)
- **4300 Implementation / setup revenue** (recognized over the period benefited, or at a point in time if a distinct performance obligation)
- **4400 Professional services revenue** (recognized as services are delivered)
- **4900 Contra-revenue: discounts and refunds**


Each stream maps to a different ASC 606 treatment, which is why blending them into one “Revenue” account is both an audit and an analytics failure. The[AICPA Audit and Accounting Guide: Revenue Recognition](https://www.aicpa-cima.com/cpe-learning/publication/revenue-recognition-audit-and-accounting-guide) devotes Chapter 9 to software entities, walking through when a setup fee is a distinct obligation versus a material right.


The reason expansion revenue gets its own account is metrics, not aesthetics. Net revenue retention is (beginning ARR minus churned minus contraction plus expansion) divided by beginning ARR, excluding new-customer revenue, per[ChartMogul](https://chartmogul.com/saas-metrics/nrr/) . Gross revenue retention strips out expansion and caps at 100%. If new, expansion, and base subscription all land in one account, neither number can be computed from the ledger. That metric only exists if the COA was built to produce it.


### COGS accounts (5000s): isolate the cost of delivering the product


SaaS cost of revenue is the single largest lever on gross margin, so the COGS block must capture every cost tied to delivering the product and nothing else. The rule is mechanical. If a cost is incurred to deliver the running product to existing customers, it is COGS. If it is incurred to build, sell, or run the company, it is operating expense, per[The SaaS CFO](https://www.thesaascfo.com/what-should-be-included-in-saas-cogs/) . A SaaS COGS block:


- **5000 Hosting and cloud infrastructure** (AWS, GCP, Azure production environments)
- **5100 Third-party software embedded in the product** (data, monitoring, security APIs that ship inside the product)
- **5200 Customer success and implementation labor** (the delivery headcount, not post-sale account management that is really sales)
- **5300 Payment processing fees** (Stripe and similar, where you are the merchant of record)
- **5400 Professional services COGS** (labor to deliver paid services revenue)
- **5500 Amortization of capitalized software** (the periodic charge on internally developed software)


The consequence of getting this wrong is direct. SaaS gross margins benchmark at 71 to 72% on a blended basis, with the strongest companies above 80%, per the[High Alpha / OpenView SaaS Benchmarks](https://www.highalpha.com/saas-benchmarks) . Drop hosting into operating expense and your gross margin reads artificially high, which dies in diligence. Drop a sales commission into COGS and your margin reads artificially low. Both distortions survive only until someone who knows SaaS reads the statements.


### Operating expense accounts (6000s to 8000s): split S&M, R&D, G&A


Operating expense must split into R&D, S&M, and G&A, with Customer Success sitting in COGS or G&A depending on whether the function delivers the product or manages the account. The split is not optional for a company that reports against benchmarks.


The R&D bucket (6000s) carries engineering wages, dev tooling, and contractor engineering, and it matters beyond reporting. Domestic engineering wages are the core of the qualified research expense base, and software development is treated as Section 174 research expenditure. New Section 174A permanently restored immediate expensing of domestic R&E for tax years beginning after December 31, 2024, while foreign R&E still amortizes over 15 years, per[Grant Thornton](https://www.grantthornton.com/insights/alerts/tax/2025/insights/full-expensing-of-domestic-research) . A chart that cleanly isolates domestic engineering wages makes both the credit calculation and the 174A treatment far easier to support, which is exactly why Indinero’s[business tax services](https://www.indinero.com/services/business-tax-services/) treat the COA design and the return as one workflow.


S&M (7000s) holds sales wages, commissions, advertising, and events. Capitalized commissions under ASC 340-40 sit on the balance sheet as a deferred cost asset, not here, with only the amortization flowing through. G&A (8000s) covers executive, finance, HR, and legal wages, rent, insurance, and general software. S&M must be separated from G&A because the magic number, CAC payback, and the Rule of 40 are all computed off isolated sales and marketing spend. Lump S&M into a generic SG&A account and none of those numbers can be pulled from the ledger. The same data feeds your board reporting, which is why[fractional CFO advisory](https://www.indinero.com/services/cfo-services/) and the COA design belong in the same engagement.


## Common Pitfalls


The recurring SaaS chart-of-accounts mistakes are predictable, and most trace back to using a generic template instead of a SaaS-built one. Each one distorts a number that someone in diligence will eventually reclassify.


1. **Hosting in operating expense instead of COGS.** AWS and GCP production spend is cost of revenue. Put it in OpEx and your reported gross margin inflates, then evaporates the moment a diligence team moves it, per[The SaaS CFO](https://www.thesaascfo.com/what-should-be-included-in-saas-cogs/) . The consequence is a margin that fails due diligence.
2. **No expansion-versus-new revenue separation.** One blended revenue account makes NRR, GRR, and NDR impossible to compute from the ledger, per[ChartMogul](https://chartmogul.com/saas-metrics/nrr/) . The consequence is broken retention analysis the night before a board meeting.
3. **Sales commissions misclassified as COGS.** Commissions are a selling expense, not a cost of delivering the product, per[DualEntry](https://www.dualentry.com/blog/saas-chart-of-accounts) . The consequence is inflated COGS and deflated gross margin.
4. **No deferred revenue account.** Collecting annual prepayments without a deferred revenue liability recognizes revenue prematurely. The consequence is a clear ASC 606 violation and an audit red flag.
5. **No current-versus-long-term split on deferred revenue.** A single deferred revenue account obscures the within-12-months versus beyond-12-months presentation auditors expect. The consequence is an audit finding on balance-sheet classification.
6. **S&M lumped with G&A.** The consequence is dead magic-number, CAC-payback, and Rule of 40 analysis, because all three are computed off isolated S&M.
7. **Over-granular accounts nobody maintains.** A 200-account chart that gets miscoded is worse than a 60-account chart coded correctly, per[Kruze Consulting](https://kruzeconsulting.com/startup-chart-accounts/) . The consequence is a chart that drifts and reclassification work after a tax period closes.


Most generalist firms miss these until an auditor flags them. Indinero’s books are audit-ready by default, because GAAP discipline is baked into how the team operates, not added later. The CPA team catches the misclassification during monthly close review, before it reaches a data room.


## The Audit-Ready Standard


An audit-ready SaaS chart of accounts is one where the financial statements roll up directly from the ledger with no off-system reclassification, every ASC 606 contract liability and contract asset is presented per ASC 606-10-45-1 through -3, and the revenue and cost segmentation serves both GAAP and the SaaS metrics in one structure. In practice, three things hold true.


First, classification matches the standards. Deferred revenue is a contract liability presented net per ASC 606-10-45-1, split current and long-term. Contract assets are presented separately from receivables per ASC 606-10-45-3. Capitalized commissions sit as deferred costs under ASC 340-40, and capitalized development as an intangible under ASC 350-40, each with amortization flowing to the right line.


Second, the chart was built before the transactions, not reverse-engineered after. The expensive version of this work is recategorizing historical transactions, which gets genuinely painful once a tax period has closed. Series A audit prep should be months of clean books, not weeks of scramble to reclassify two years of miscoded hosting spend. Our[audit preparation walkthrough](https://www.indinero.com/blog/audit-preparation/) covers what that readiness looks like in practice.


Third, the same chart serves GAAP and the board deck. Revenue segmented for ASC 606 is the same segmentation that produces NRR and GRR. Cost isolated for gross margin is the same data the Rule of 40 needs. When the COA is built once, correctly, the audit, the tax return, the R&D credit base, and the investor metrics all draw from one source of truth. Build it right the first time.


## How indinero Approaches the SaaS COA


Indinero’s CPA team builds the SaaS chart of accounts inside the tooling you already run, then maintains it as part of monthly close. The structure above is not a one-time setup deliverable. It is a living chart that has to stay correct every month as new revenue streams, products, and entities appear, which is where most internally maintained charts drift.


The GAAP discipline is handled upfront, so your month-end close runs in roughly 5 days instead of 45, and your audit prep is months of clean books instead of weeks of scramble. The SaaS COA is the foundation of that. Revenue segmented for ASC 606, hosting seated in COGS, S&M separated from G&A, and deferred revenue split current and long-term, all built and maintained so the statements roll up clean.


There is no platform to migrate to. Indinero builds and maintains the chart of accounts inside QuickBooks Online or NetSuite, integrating with the stack you already use, with no proprietary lock-in. The[accounting services](https://www.indinero.com/services/accounting-services/) scope covers the full monthly GAAP-compliant close, financial statement preparation across the P&L, balance sheet, cash flow, and statement of equity, cash-to-accrual conversion, multi-entity consolidation, ASC 606 revenue recognition, and audit prep and support.


The structural difference from a tool-only or bench-style provider is that the chart is built by CPAs and bundled with tax and fractional CFO under one monthly engagement. The same team that designs the revenue segmentation knows it has to feed the R&D credit base and the Section 174A treatment, because the same engagement files the return. Indinero has run continuously since 2009 under stable ownership, holds a 5-star Clutch rating, serves 500+ regular customers, is SOC 2 compliant (2026), and offers month-to-month engagements with pricing that starts at $750/mo.


For a growth-stage SaaS company heading into a Series A audit, the question is rarely whether you have a chart of accounts. It is whether the one you have was built to survive scrutiny. If you want a SaaS COA built right from day one,[reach out for a free consultation](https://www.indinero.com/contact) . We’d love to learn about your business and find where we can help.


## Frequently asked questions


Founders and finance leaders ask the same questions when they sit down to structure or restructure a SaaS chart of accounts. The answers below are grounded in the structure above and the work Indinero’s CPA team does building and maintaining audit-ready charts of accounts for growth-stage SaaS companies inside QuickBooks Online and NetSuite.


### Why do SaaS companies need a different chart of accounts than a traditional services business?


A SaaS chart of accounts carries deferred revenue, accrued revenue, deferred cost of revenue, and hosting COGS that a traditional services COA never needs. Software revenue splits recurring subscription from one-time services and usage, and cost of revenue is dominated by cloud hosting rather than raw materials or inventory. An analyst should be able to read your business model straight off the chart, which a generic QuickBooks default template with one undifferentiated Sales account cannot deliver.


### How granular should revenue accounts be in a SaaS COA?


SaaS revenue should be segmented by recognition pattern, typically subscription, expansion, usage, implementation, professional services, and a contra-revenue line. Each stream maps to a different ASC 606 treatment, so blending them into one Revenue account is both an audit failure and an analytics failure. Expansion needs its own account specifically so net revenue retention and gross revenue retention can be computed directly from the ledger. That metric only exists if the COA was built to produce it.


### Should hosting costs go in COGS or operating expenses?


Hosting and cloud infrastructure for production environments belong in COGS, because they are a direct cost of delivering the running product to existing customers. The rule is mechanical. If a cost is incurred to deliver the product, it is cost of revenue, and if it is incurred to build, sell, or run the company, it is operating expense. Drop hosting into OpEx and your gross margin reads artificially high, then evaporates the moment a diligence team reclassifies it.


### How do I structure S&M, R&D, and G&A for SaaS-style operating expense reporting?


Split operating expense into R&D, S&M, and G&A, and place Customer Success in COGS or G&A based on its role. R&D carries engineering wages, dev tooling, and contractor engineering. S&M holds sales wages, commissions, advertising, and events. G&A covers exec, finance, HR, legal, rent, and insurance. Keeping S&M isolated from G&A is what makes the magic number, CAC payback, and Rule of 40 computable from the ledger.


### When should I add departmental tags or classes to the COA?


Add departmental tags or classes once you need expense reporting by function or entity that account numbering alone cannot capture. That inflection usually arrives around Series B, as the chart reaches 80 to 100 accounts. Seed-stage companies run lean at 30 to 40 accounts, and Series A expands to 50 to 70 with revenue and COGS detail. The structure stays constant while granularity grows. Add tags before the sprawl, not after, so departmental P&L rolls up without manual reclassification.


### How do I avoid breaking historical comparability when I restructure the COA?


Leave numbering gaps between accounts so related lines insert later without renumbering history, and use parent and child subaccounts instead of a flat sprawl. If your COGS accounts run 5000 to 5050, start the next band at 5100, not 5051. Restructuring after a tax period has closed means recategorizing historical transactions, which is the expensive version of this work. Indinero’s CPA team builds the chart before the transactions and maintains it during monthly close, so prior periods stay comparable.


### Does QuickBooks Online or NetSuite affect how I structure the COA?


QuickBooks Online and NetSuite share the same four-digit numbering logic, so the classification structure of a SaaS COA stays identical across both tools. NetSuite assigns all 1-series accounts to assets and 2-series to liabilities, and QuickBooks Online uses the same four-digit default once account numbers are enabled. Subaccount syntax differs, with QuickBooks writing Hosting:AWS and NetSuite pointing a child at its parent. Indinero builds and maintains the chart inside whichever you already run, with no proprietary platform to migrate to.
