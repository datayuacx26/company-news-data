---
schema_version: "1.0.0"
document_id: "d5284fa5e519c3229c1f416882680b707300201a84199645f23843de221d5240"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/finance-ops-series-b-playbook/"
published_at: "2026-08-18T15:44:38+00:00"
first_seen_at: "2026-08-18T16:19:10.982901+00:00"
fetched_at: "2026-08-18T16:19:12.094635+00:00"
content_hash: "sha256:d46a72f93a5fb0f2abcce316f24191b618bde63315c93af81f0335f07f6080da"
---

# Series B Finance Operations Playbook for SaaS Startups

## What Changes at Series B


The Series B finance org chart changes shape because the audience for your numbers changes. At Series A you built the books. At Series B someone else grades them.


The profile is consistent across the stage. Most Series B SaaS companies sit at $5M to $15M ARR with 50 to 150 employees and a growth-stage institutional lead on the cap table. That headcount band is arithmetic, not folklore.[SaaS Capital’s 2026 revenue per employee benchmarks](https://www.saas-capital.com/blog-posts/revenue-per-employee-benchmarks-for-private-saas-companies/) put the median for private SaaS companies at $141,125, with equity-backed companies in the $5M to $10M ARR band running a median of $152,295 per employee against $177,240 for bootstrapped peers. At that rate, a $10M ARR company lands near 66 employees and a $15M ARR company near 99.


The finance budget scales with the reporting burden, not with revenue alone.[SaaS Capital’s 2026 spending benchmarks](https://www.saas-capital.com/blog-posts/spending-benchmarks-for-private-b2b-saas-companies/) , drawn from a survey of more than 1,000 private B2B SaaS companies completed in March 2026, put median G&A at 15% of ARR, up from 14% in 2025. Equity-backed companies spend 64% more on G&A than bootstrapped ones, and the survey attributes the gap directly to the finance and administrative team required to support investor reporting, board meetings, and audits.


Five things change concretely once the round closes:


- **An outside auditor enters the picture.** Under[AICPA AU-C Section 510 on opening balances in initial audit engagements](https://us.aicpa.org/content/dam/aicpa/research/standards/auditattest/downloadabledocuments/au-c-00510.pdf) , the auditor has to obtain sufficient appropriate evidence about opening balances, including contingencies and commitments that existed at the start of the period. They don’t start with your current year. They start with the balance sheet nobody audited.
- **The close stops being a report and becomes a control.** The same trial balance now feeds the audit, the board deck, the covenant package, the 409A, the R&D credit study, and the state filings. A number nobody can trace to source isn’t annoying anymore. It’s a finding.
- **Legal entity structure multiplies.** International hires, an R&D subsidiary, a UK or Canadian sales entity, or an acqui-hire all create a consolidation. Entity-level books and consolidated reporting become two separate monthly outputs.
- **FP&A separates from accounting.** Through Series A one person can own both the close and the model. At Series B the two jobs pull opposite directions, and whoever does both ships the close late or the forecast thin.
- **Treasury becomes a real job.** Once cash is a meaningful multiple of FDIC insurance limits and a venture debt facility introduces monthly covenant testing, somebody has to own sweep policy, counterparty diversification, and the covenant calendar.


If the books are still cash-basis or accrual-ish, this is the stage where that gets expensive. Most companies settle the[GAAP accounting foundation](https://www.indinero.com/blog/gaap-accounting-for-startups/) before the engagement letter is signed rather than during fieldwork.


### Headcount math for a Series B finance team


The most useful published dataset on this question analyzed 218 active B2B Y Combinator companies from Winter 2015 through Spring 2025, covering 3,597 finance FTE records across 2,075 unique job titles. That[finance team structure study](https://www.getaleph.com/blog/finance-team-structure) breaks average finance headcount down by company size:


Company headcount Average finance FTEs Finance as % of company


5 to 50 0.35 1.3%


51 to 250 5.29 3.5%


251 to 500 10.86 2.9%


500+ 49.2 not stated


Two findings matter. Only about 17% of companies in the 5 to 50 band carry any internal finance staff at all, and most make their first full-time finance hire late in the 30 to 50 FTE range. In the 51 to 250 band, where a Series B company sits, the recurring pattern is a finance lead plus a Controller plus an FP&A analyst. So the honest answer to “how big is a Series B finance team” is roughly five people at about 3.5% of total headcount, with wide variance driven by how much sits outside the company. A[stage-based view of finance team scaling](https://www.mostlymetrics.com/p/building-finance-teams) corroborates the band, putting growth-stage companies at 3 to 15 finance people with priority hires of CFO, Controller, and FP&A Manager. Series B is the seam between two rows on that chart, which is exactly why it’s the hardest stage to staff.


### The Series B finance org chart, role by role


Read it as three layers. Layer one is the finance leader, reporting to the CEO. Layer two is two branches, a Controller who owns the ledger and an FP&A lead who owns the plan. Layer three is the execution bench under the Controller. State the reporting lines out loud, because that’s where most org charts go fuzzy: Controller reports to the CFO, FP&A lead reports to the CFO, Senior Accountant and AP/AR report to the Controller, the Revenue Accountant reports to the Controller with a dotted line to RevOps, and any FP&A Analyst reports to the FP&A lead.


Role Reports to Owns At $5M to $8M ARR At $10M to $15M ARR


CFO or VP Finance CEO Capital allocation, the Series C narrative, banking and debt relationships, the auditor relationship Often fractional Usually full time


Controller CFO or VP Finance GAAP close, audit, ASC 606 and ASC 842 memos, consolidation, internal controls, tax calendar Yes, the anchor in-house hire Yes


FP&A Manager or Head of FP&A CFO or VP Finance Operating model, hiring plan, departmental P&Ls, board reporting, cohort and NDR analysis Sometimes, often still held by the finance leader Yes


Senior Accountant or Accounting Manager Controller Close mechanics, reconciliations, flux analysis, the audit PBC list Yes Yes, sometimes two


Revenue Accountant Controller, dotted line to RevOps ASC 606 contract review, multi-element arrangements, usage-based and ramped billing Rare Yes, once contracts outgrow a spreadsheet


AP/AR or Payroll Specialist Controller Vendor payments, collections, payroll processing, expense workflow One hybrid role Splits into two


FP&A Analyst FP&A lead Model maintenance, variance commentary, sales capacity modeling No Often


Equity and stock administration is rarely a full role here. It’s a named responsibility owned by the Controller or shared with Legal. One diagnostic worth running quarterly: watch the accounting-to-FP&A ratio. A team that’s four-to-one accounting-heavy at this scale usually means planning is under-built and the board deck is being assembled by the person who should be closing the books. If the titles themselves are still blurry, the[Controller versus comptroller versus CFO breakdown](https://www.indinero.com/blog/controller-vs-comptroller-vs-cfo/) draws the horizon distinction cleanly.


### Fractional CFO or full-time CFO at Series B


Don’t answer this with a revenue threshold. Every competing page already does, and revenue isn’t what breaks a part-time arrangement. Four tests are:


1. **Transaction complexity, not transaction volume.** Multi-entity consolidation, foreign subsidiaries, transfer pricing, and revenue arrangements with variable consideration are what overwhelm fractional hours. A clean single-entity $15M ARR business often runs fine on fractional leadership. A $6M ARR business with a UK subsidiary and usage-based pricing frequently doesn’t.
2. **Board and investor cadence.** If the lead expects a monthly package with variance commentary, a live model, and the finance leader in the room, the required hours stop looking fractional.
3. **The audit.** A first audit is a project with a named owner, a PBC list, and a schedule. Fractional leadership can run it. It can’t run it in six hours a month.
4. **The next raise.** A Series C process consumes a finance leader for a quarter.[CRV’s guide to hiring a CFO](https://www.crv.com/content/how-to-hire-a-cfo) puts a full-time search at roughly 10 to 16 weeks from kickoff to start date before any ramp, so a raise 9 to 12 months out makes this a decision for this quarter.


Here’s the position most content avoids. A fractional CFO paired with an in-house Controller is a sound Series B structure at the lower end of the band, and it usually beats a rushed full-time hire. What isn’t defensible is fractional leadership with no in-house accounting owner. At Series B, the close needs somebody whose full-time job is the close. If you’re weighing timing, the criteria in[when to hire a fractional CFO](https://www.indinero.com/blog/when-to-hire-a-fractional-cfo/) and the scope notes on[hiring an interim CFO](https://www.indinero.com/blog/hire-interim-cfo-for-startups/) both map onto this stage.


## Service Requirements


Series B accounting requirements come down to one test. Can every number in the board deck be traced to a document an auditor will accept. That reframes the buying decision, because you’re not shopping for bookkeeping anymore. You’re buying a finance function that produces audit-defensible output every month on a schedule somebody outside the company sets.


Service Required at this stage? Why indinero service line


Full accrual GAAP close Yes Not accrual-ish. Tie-out binder, reconciliations for every balance sheet account, revenue schedules that agree to contracts, documented policies[Accounting services](https://www.indinero.com/services/accounting-services/)


Audit readiness Yes Opening balance substantiation, equity and cap table tie-out, lease inventory, accrual completeness, and a policy memo file built before fieldwork[Accounting services](https://www.indinero.com/services/accounting-services/)


Audit management Yes Running the engagement itself, PBC list ownership, auditor questions, technical memo drafting[Accounting services](https://www.indinero.com/services/accounting-services/)


Multi-entity consolidation Yes if a second entity exists Intercompany eliminations, consistent policies across entities, correct model selection under ASC 810[Accounting services](https://www.indinero.com/services/accounting-services/)


Transaction processing and close mechanics Yes AP, AR, reconciliations, and expense workflow that leave an audit trail rather than a Slack thread[Online bookkeeping](https://www.indinero.com/services/online-bookkeeping-services/)


Deeper FP&A Yes Departmental P&Ls, a driver-based model, rolling forecast, cohort and NDR analysis, board reporting that reconciles to the financials[CFO services](https://www.indinero.com/services/cfo-services/)


Multi-state and international tax Yes Federal returns, state income and franchise filings wherever nexus exists, sales tax, and foreign filings where subsidiaries exist[Business tax services](https://www.indinero.com/services/business-tax-services/)


R&D credit at scale Yes if you fund engineering Bigger study, heavier documentation, and a credit that now lands on the income tax return instead of the payroll return[Business tax services](https://www.indinero.com/services/business-tax-services/)


409A refresh cadence Yes At least every 12 months and immediately after the round closes, because a priced round is material information[409A valuation services](https://www.indinero.com/services/409a-valuation-services/)


CFO advisory Yes, fractional or full time Capital allocation, covenant management, the Series C narrative, pricing and unit economics[CFO services](https://www.indinero.com/services/cfo-services/)


BI and reporting dashboards Yes Dashboards pulling from the ledger and the billing system rather than a manually maintained workbook Technology and business intelligence


Treasury and covenant management Yes if venture debt exists Weekly cash forecasting, sweep and counterparty policy, monthly covenant testing[CFO services](https://www.indinero.com/services/cfo-services/)


Two rows deserve extra attention. Revenue recognition stops being theoretical at Series B because auditors test it, and the five-step model in[FASB ASC 606](https://storage.fasb.org/ASU%202014-09_Section%20A.pdf) governs how every contract lands in the ledger. Private companies have applied it to annual statements since 2020 and to interim periods since 2021.


The other is the 409A. The regulations at[26 CFR 1.409A-1](https://www.ecfr.gov/current/title-26/chapter-I/subchapter-A/part-1/section-1.409A-1) treat a valuation as unreasonable once it’s more than 12 months stale, or once it fails to reflect information that materially affects value. Closing a priced round is exactly that kind of information. And the reconciliation point in the FP&A row is where most Series B board packages fall over. Boards notice when the ARR in the deck doesn’t tie to the revenue in the statements. So do auditors.


## Compliance Triggers


Nine compliance obligations change or arrive at Series B. Each carries a delivery date, which is the part founders usually discover late.


1. **Your first audit arrives as a covenant, not a law.** There’s no statutory financial statement audit requirement for a US private company at any revenue level. The trigger is contractual, from an audit covenant in the Investors’ Rights Agreement, a requirement in a venture debt or revolving credit facility, or M&A and IPO diligence. Read the agreement, find the covenant, note the delivery deadline, and back-schedule 8 to 16 weeks for a first-year engagement including readiness. Miss it and you’re negotiating a waiver with the same investor you’re about to ask for a Series C.
2. **ASC 606 gets tested, not just applied.** Revenue recognition is one of the harder areas to audit, as the[Journal of Accountancy’s guidance on auditing revenue recognition](https://www.journalofaccountancy.com/issues/2020/jul/auditing-revenue-recognition-challenges-to-consider/) sets out, and multi-element arrangements, ramps, and usage-based pricing are where SaaS contracts get complicated. Your revenue schedules have to agree to signed contracts, line by line. If they don’t, the finding lands on the line every investor reads first. A refresher on[what ASC 606 means in practice](https://www.indinero.com/blog/asc-606-what-you-need-to-know/) is worth an hour before the auditor’s first call.
3. **ASC 842 lease accounting is already in force.** The[FASB effective dates schedule](https://www.fasb.org/standards/accounting-standard-updated-effective-date) put it in effect for private companies for fiscal years beginning after December 15, 2021, and interim periods within fiscal years beginning after December 15, 2022. Your office lease and any multi-year hosting or hardware commitment produce a right-of-use asset and a lease liability. Findings cluster in four places: leases scattered across the company, embedded leases hidden inside service contracts, incremental borrowing rate determination, and spreadsheet-only tracking that can’t support remeasurement. The balance sheet change can also move leverage-based covenant math.
4. **A second entity triggers consolidation under ASC 810.** Consolidation runs through two models, the variable interest entity model and the voting interest entity model, and identifying which applies is a threshold question rather than a formality, as[KPMG’s consolidation handbook](https://kpmg.com/kpmg-us/content/dam/kpmg/frv/pdf/2023/handbook-consolidations-22.1.pdf) lays out. Intercompany transactions have to be eliminated and policies have to be consistent across entities. Inconsistent policies are a documented source of consolidated statements that fail to comply with US GAAP, and the fix happens during fieldwork at fieldwork rates.
5. **Multi-state sales tax registration becomes a standing project.** Skip the blanket “$100,000 or 200 transactions” rule, because the transaction-count prong is being dismantled state by state. Illinois removed its 200-transaction threshold effective January 1, 2026, leaving a $100,000 gross receipts test, per the[Sales Tax Institute](https://www.salestaxinstitute.com/resources/illinois-economic-nexus-update-no-transaction-threshold) . Utah eliminated its threshold effective July 1, 2025, Alaska effective January 1, 2025, and Kentucky’s elimination takes effect August 1, 2026. Whether SaaS is taxable at all is a separate question per state. Texas treats it as a taxable data processing service with 20% of the charge exempt, meaning 80% is taxable, per the Comptroller’s[data processing services publication](https://comptroller.texas.gov/taxes/publications/94-127.php) . Unregistered exposure compounds with penalties and interest until you disclose voluntarily.
6. **Income tax nexus and payroll registration follow your remote hires.** The Multistate Tax Commission’s revised[statement on Public Law 86-272](https://www.mtc.gov/wp-content/uploads/2023/04/025-MTC-Statement-on-PL-86-272.pdf) , adopted August 4, 2021, treats interactions with in-state customers through a seller’s website or apps as business activities in the taxing state, which leaves P.L. 86-272 protection thin for a SaaS company. Remote hires separately create employer registration obligations, not just employee-side withholding. Fifteen states of sales tax registration and fifteen states of payroll registration are two different projects with two different calendars. Map the[startup tax nexus rules](https://www.indinero.com/blog/startup-tax-nexus/) before the next hiring wave, not after.
7. **Foreign subsidiaries trigger Forms 5471 and transfer pricing.**[Form 5471](https://www.irs.gov/instructions/i5471) is filed by certain US persons who are officers, directors, or shareholders of certain foreign corporations under Sections 6038 and 6046, attached to the income tax return and due with it including extensions. Separately,[Section 482](https://www.irs.gov/businesses/international-businesses/transfer-pricing) requires intercompany prices to produce results consistent with what uncontrolled parties would have realized, and Section 482 adjustments can carry a 20% accuracy-related penalty under Section 6662. Build the intercompany services agreement, the documented cost-plus markup, and the supporting file in month one. Retrofitting it during Series C diligence is expensive.
8. **The R&D credit changes shape, and Form 6765 Section G becomes mandatory.** Under current law, Section 174A permits full expensing of domestic research or experimental expenditures paid or incurred after December 31, 2024, while foreign research stays under legacy Section 174 with 15-year amortization. The[instructions for Form 6765](https://www.irs.gov/instructions/i6765) make Section G business component reporting optional for tax years beginning before 2026 and required for tax years beginning after 2025, with a narrow exception for filers whose total QREs are $1.5 million or less at the control group level. A $300,000 federal credit under the alternative simplified credit method implies roughly $2 million or more of QREs, so a company claiming the credit at scale is exactly the filer for whom Section G is mandatory. Business-component documentation is now a filing input, not an exam defense. The small-business retroactive Section 174A election carried a July 6, 2026 deadline that has already passed, so confirm with your preparer what remains available.
9. **The payroll offset is no longer yours, and Delaware franchise tax gets bigger.** The[qualified small business payroll tax credit](https://www.irs.gov/businesses/small-businesses-self-employed/qualified-small-business-payroll-tax-credit-for-increasing-research-activities) under Section 41(h) is available only to a corporation or partnership with gross receipts under $5 million for the tax year, so a $5M to $15M ARR company has aged out of it. The credit doesn’t disappear. It becomes an income tax credit that offsets liability and carries forward, which for a pre-profit company is a deferred asset rather than cash this quarter. That shift away from[claiming the R&D credit against payroll taxes](https://www.indinero.com/blog/rd-tax-credit-against-payroll-taxes/) is the single most common surprise at this stage. Meanwhile authorized shares balloon after a priced round, and[Delaware’s franchise tax calculation](https://corp.delaware.gov/frtaxcalc/) under the Assumed Par Value Capital Method runs $400 per $1 million of assumed par value capital with a March 1 deadline. Companies that never switch methods routinely overpay by five figures, so the[Delaware incorporation tax mechanics](https://www.indinero.com/blog/delaware-incorporation-taxes/) deserve 20 minutes each January.


## The Typical Tooling Stack


The Series B tooling question is never which product is best. It’s what breaks first, and what the replacement costs in quarters. Every row below has a trigger attached, because buying ahead of the trigger wastes money and buying behind it wastes the close.


Category Tool(s) Why


Accounting platform NetSuite, Sage Intacct, QuickBooks Online Advanced The migration trigger is rarely transaction volume. It’s multi-entity consolidation, multi-currency, and an audit trail across entities.[Intuit’s usage limits](https://quickbooks.intuit.com/learn-support/en-us/help-article/intuit-subscriptions/learn-usage-limits-quickbooks-online/L6THMltE4_US_en_US) give QuickBooks Online Advanced up to 25 billable users plus 3 accountant users with unlimited classes and locations, while lower tiers cap both


Billing and revenue recognition A dedicated billing and rev rec system Needed once contracts include ramps, usage components, or mid-term modifications. A revenue schedule living in a workbook one person maintains is a single point of failure an auditor will find


AP automation Bill.com, Ramp Approval workflows that produce an actual audit trail, plus vendor records that survive a staffing change


Spend management Brex, Ramp Card programs at full deployment, with policy enforcement and coding at the point of spend rather than at close


Payroll Gusto, Rippling, ADP Multi-state registration, equity compensation reporting, and quarterly filings in every state where you now have people


Equity administration Carta, Pulley Cap table, 409A support, and ASC 718 stock compensation expense that feeds the close instead of arriving as a manual journal entry


SaaS metrics ChartMogul, Maxio ARR, NDR, churn, and cohort views generated from billing data, so the board deck and the financials draw from one source


Sales tax Avalara, TaxJar, Anrok Per-state, per-period threshold tracking and taxability determination, which stops being a spreadsheet job once you’re registered in 15 states


Lease accounting A dedicated ASC 842 tool or module Ongoing remeasurement, embedded lease tracking, and disclosure support that spreadsheets can’t sustain


FP&A Cube, Mosaic, Vena, Anaplan The trigger isn’t model size, it’s contributor count. Once department heads own their budget lines, one shared spreadsheet stops working


BI dashboards Power BI, Tableau A reporting layer over the ledger and billing data, so the monthly package is generated rather than assembled


Two sequencing rules save more money than any product choice. Budget an ERP migration as a two-quarter project with a parallel-run period, and never run it in the same quarter as the first audit. If you’re weighing the platform decision itself, the[NetSuite versus QuickBooks comparison](https://www.indinero.com/blog/netsuite-vs-quickbooks/) covers where the line actually falls for a growth-stage company.


One thing that shouldn’t factor into the decision is your accounting partner. Indinero integrates with your existing stack, QuickBooks, Xero, NetSuite, Stripe, Brex, and Ramp, with no proprietary platform lock-in. Your ledger stays yours, which matters most in the exact window when you’re moving it.


## Common Mistakes at This Stage


Five failure patterns show up repeatedly in the $5M to $15M ARR band. None are exotic. All are expensive.


1. **Excel-based reporting that breaks at scale.** The board deck gets assembled by hand each month from exports pasted into a workbook nobody else can open. It works until the CFO travels during close week, or until an auditor asks how a number was derived and the answer is “the tab.” This is the most common Series B failure, and it’s a large part of why equity-backed companies carry that 64% higher G&A load. The correction is to build the reporting layer over the ledger and the billing system before the workbook becomes load-bearing.
2. **Starting Series B audit prep when the audit starts.** Because AU-C 510 requires evidence about opening balances, a first-year engagement reaches into periods nobody expected anyone to examine. Companies that begin readiness one quarter before fieldwork spend the engagement producing documentation instead of answering questions, and the fee reflects it. Audit prep starts 6 months out. The[audit preparation checklist](https://www.indinero.com/blog/audit-preparation/) is the right place to start counting backward from your covenant date.
3. **Missing state R&D credits and mishandling state Section 174 disconformity.** The federal study gets done. The state credits don’t, or they get computed off the federal number in states that diverge, like Pennsylvania, which requires five-year amortization for domestic research expenses and disallows the catch-up deduction. California’s credit is 15% of the excess of qualified research expenses over base period expenses per the[Franchise Tax Board](https://www.ftb.ca.gov/file/business/credits/california-research.html) , but total business credits including carryovers can’t reduce tax by more than $5,000,000 for taxable years beginning on or after January 1, 2024 and before January 1, 2027. Texas replaced its regime with a new Subchapter T franchise tax credit effective January 1, 2026, repealing both the sales tax exemption for qualified research and the prior credit, per the[Texas Comptroller](https://comptroller.texas.gov/taxes/franchise/research-dev-credit.php) . The correction is to run the state analysis as its own workstream with a named owner, state by state.
4. **Treating consolidation as addition.** Two trial balances summed is not a consolidation. Intercompany eliminations, consistent policies across entities, and correct application of ASC 810 are all required, and inconsistent policies are a documented driver of audit rework. The correction is to write the consolidation policy memo the same month the second entity is formed, while the transactions are still few enough to reconstruct.
5. **Deferring the tooling decision to the quarter of the audit.** ERP migration, billing implementation, and audit fieldwork are each a full-quarter project. Doing two at once means doing neither well. Sequence it: fix the close, then fix the systems, then run the audit. In the same family are letting the 409A go stale past 12 months or past the round close, registering for sales tax where SaaS isn’t taxable while missing states where it is, and hiring a full-time CFO 18 months early, which burns roughly $400,000 of runway on capability the company isn’t using yet.


## What’s Coming Next: Series C Triggers


Four signals tell you Series B finance maturity has hit its ceiling. The first is that pre-IPO readiness enters the conversation. For a company on a listing path, the audit standard shifts from AICPA to PCAOB and historical periods have to be audited to that standard, with structured preparation typically starting 12 to 24 months before a listing. Even for companies that will never list, the diligence bar in a Series C or a strategic acquisition approximates it.


The second is that international expansion moves from one subsidiary to a structure. Transfer pricing stops being a single intercompany services agreement and becomes a policy set, Forms 5471 multiply by entity, and statutory audit and local GAAP obligations appear in the foreign jurisdictions themselves. The third is that treasury becomes a named role rather than a hat somebody wears, once debt covenants, currency exposure, counterparty policy, and yield on a large balance all land on one desk.


The fourth is specialization. Companies in the 251 to 500 headcount band average 10.86 finance FTEs, roughly double the 5.29 average in the 51 to 250 band. The Controller becomes a Controller plus an Assistant Controller plus a Technical Accounting Manager, FP&A splits into corporate FP&A and embedded business partners, and tax becomes a named role rather than an outsourced deliverable. The[KeyBanc and Sapphire Ventures private SaaS company survey](https://investor.key.com/press-releases/news-details/2025/PRIVATE-SAAS-COMPANY-SURVEY-REVEALS-AI-DRIVEN-TRANSFORMATION-AND-SUSTAINED-OPERATIONAL-EXCELLENCE/default.aspx) polls a population whose median ARR sits well above the Series B band, which is a useful picture of where the org is headed.


Here’s the test our Series C playbook opens with. A company ready for Series C can produce audited financials, a consolidated multi-entity close inside 10 business days, a driver-based model that ties to those financials, and a clean answer to any diligence question about revenue recognition, equity, or state tax exposure, without a fire drill. One that isn’t ready can produce all of it eventually, if you give it three weeks and don’t ask how. The difference isn’t effort. It’s infrastructure. Running a[financial due diligence process](https://www.indinero.com/blog/financial-due-diligence/) once, deliberately, before you need to, is the cheap version of that lesson.


## How Indinero Supports Series B Operators


With indinero, you scale from monthly bookkeeping to full fractional-CFO advisory in the same engagement, no rip-and-replace as your needs grow. For Series B operators that matters for a specific reason. The $5M to $20M ARR band is where companies outgrow providers built for pre-revenue and seed-stage volume and still can’t justify a practice priced and staffed for companies several times their size. Switching vendors during an ERP migration or a first audit is the most expensive vendor change there is.


What that looks like in practice for a Series B finance operations build:


- **The close and the audit under one roof.** Full accrual GAAP close, audit readiness, and audit management run by the same team that maintained the prior-year books, which is exactly what AU-C 510 opening-balance work depends on.
- **The CFO seat, sized to your structure.** indinero can hold the fractional CFO seat while your in-house Controller owns the close, or hold both while you decide whether the full-time hire is 6 months out or 18. The[outsourced CFO services](https://www.indinero.com/blog/outsourced-cfo-services/) scope notes cover how those hours get allocated.
- **Tax across the whole footprint.** Multi-entity and multi-state filings, sales tax nexus, R&D credit at scale including state credits, and 409A refresh cadence on the same calendar as the close.
- **SaaS specifics handled natively.** ARR and deferred revenue mechanics, ASC 606 contract review, and board reporting that reconciles to the statements, through[SaaS accounting services](https://www.indinero.com/industries/saas-accounting-services/) built for this revenue band.


You also don’t have to be a VC-backed Delaware C-Corp to fit. Indinero serves bootstrapped, PE-backed, LLC, S-Corp, and multi-entity growth companies, which matters more at Series B than at any earlier stage, because that’s when entity structure stops being simple. The equity-backed G&A premium documented in[SaaS Capital’s spending benchmarks](https://www.saas-capital.com/blog-posts/spending-benchmarks-for-private-b2b-saas-companies/) is real, and part of the job is spending it on capability rather than on coordinating four vendors who each need the same trial balance explained to them.


Track record matters when your auditor asks who maintained the books three years ago. Continuous operations since 2009, 500+ regular customers, and a team that stays with the account through the migration and the audit. Pricing starts at $750/mo, and a Series B engagement is customized pricing based on your specific needs, with month-to-month engagements available.


You’re not just looking for an accounting vendor at this stage. You’re looking for a finance partner who can hold the org chart together while you decide what the permanent one looks like. If that’s the conversation you’re in,[reach out for a free consultation](https://www.indinero.com/contact/) . We’d love to learn about your business and where the next audit lands.


## Frequently asked questions


These come up in nearly every Series B finance conversation, usually in the same order. Short answers below, with the detail in the sections above.


### What changes about finance at Series B?


At Series B, finance shifts from producing numbers to proving them to an outside auditor, a lender testing covenants, and Series C diligence. In practice that means a first audit, consolidation once a second entity exists, FP&A splitting off from accounting, and treasury becoming a real job. The same trial balance now feeds the audit, the board deck, the covenant package, the 409A, and the state filings. Indinero often works alongside an in-house Controller at this stage rather than replacing the whole function.


### When does a Series B company need its first audit?


A Series B company needs its first audit when a contract requires it, usually an investor rights agreement or venture debt covenant, not a statute. No US private company faces a statutory audit requirement at any revenue level, so read the agreement, find the delivery deadline, and back-schedule from it. Plan 8 to 16 weeks for a first-year engagement including readiness, because opening balances from unaudited periods get tested. Indinero handles readiness and audit management with the same team that maintained the prior-year books.


### Does a Series B company need a full-time CFO or can fractional still work?


Fractional still works at Series B, but only with an in-house Controller underneath it owning the close full time. Complexity forces the full-time hire, not revenue. Multi-entity consolidation, foreign subsidiaries, transfer pricing, a monthly board package with a live model, a first audit, and a Series C inside 12 months each add hours a part-time seat can’t absorb. A full-time search runs roughly 10 to 16 weeks before ramp, and indinero can hold the fractional CFO seat while you decide.


### What does a typical Series B finance team org chart look like?


A Series B finance org chart runs three layers, a finance leader over a Controller and an FP&A lead, with an execution bench beneath them. The Controller owns the GAAP close, audit, consolidation, and tax calendar. The FP&A lead owns the model, departmental P&Ls, and board reporting. Beneath them sit a Senior Accountant, an AP/AR or payroll specialist, and a Revenue Accountant once ASC 606 contracts outgrow a spreadsheet. Companies in the 51 to 250 headcount band average 5.29 finance FTEs, about 3.5% of headcount.


### When does multi-entity consolidation become necessary?


Consolidation becomes necessary the moment a second legal entity exists and the parent controls it, usually a foreign subsidiary, an R&D entity, or an acqui-hire. Two trial balances summed isn’t a consolidation. ASC 810 requires the right model, intercompany eliminations, and consistent policies across entities. Write the policy memo the month the second entity is formed. Entity-level books and consolidated reporting become two monthly outputs, and indinero runs both in one engagement.


### How does international expansion change finance ops at Series B?


International expansion adds four obligations at once, Forms 5471, transfer pricing under Section 482, foreign research amortization, and local statutory and payroll filings. Forms 5471 apply to US officers, directors, and shareholders of the foreign corporation and file with the income tax return. Section 482 requires an intercompany agreement, a documented arm’s-length markup, and contemporaneous support, since adjustments carry a 20% accuracy-related penalty. Foreign research stays on 15-year amortization even though domestic costs are expensed under Section 174A. Build the agreement in month one.


### What separates a Series B company that’s ready for Series C from one that isn’t?


A Series C ready company produces audited financials, a consolidated multi-entity close inside 10 business days, and a model that ties to those financials. It also answers any diligence question about revenue recognition, equity and 409A history, or state tax exposure without a fire drill. An unready company gets there eventually, from spreadsheets, if nobody asks how a number was derived. The difference isn’t effort. It’s infrastructure. Indinero builds that infrastructure alongside your in-house team, keeping close, audit, and 409A cadence on one calendar.
