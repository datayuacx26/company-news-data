---
schema_version: "1.0.0"
document_id: "511fd9686ffa7ce8304b139b2aa57e80117aa321b90556996a83034eb4f02771"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/book-to-tax-reconciliation"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T22:39:39.250313+00:00"
fetched_at: "2026-07-31T22:39:40.641602+00:00"
content_hash: "sha256:99233836235ec87609b242683d0a99ce77cf5d00aa0649c7a8ff6489e085c4e5"
---

# Book to Tax Reconciliation (2026): Schedule M-1, M-3

## **Key takeaways**


- Book to tax reconciliation adjusts GAAP book income to taxable income on Form 1120/1120-S/1065
- Two types of differences: permanent (never reverse, no deferred tax) vs temporary (reverse over time, create deferred tax under ASC 740)
- Schedule M-1 for <$10M total assets, Schedule M-3 for ≥$10M (granular line-by-line)
- OBBBA 2025 introduced new differences §174A R&E expensing reversal, 100% bonus depreciation, PTET deduction interaction
- Deferred tax assets and liabilities are ASC 740 output of temporary difference tracking
- [Finlens](https://www.finlens.app/accountants) automates ledger cleanup categorization, Stripe payout decomposition, deferred revenue schedules so M-1/M-3 workpaper builds from clean book income


## What book to tax reconciliation actually does


Book income and taxable income are two different measures of same underlying business activity. They diverge because:


- **GAAP (book)** is governed by FASB pronouncements (ASC 606 revenue, ASC 842 leases, ASC 740 taxes, etc.) and aims to fairly present financial position
- **Tax (taxable)** is governed by Internal Revenue Code and its regulations, which reflect Congressional policy choices accelerating deductions to encourage investment (§179, bonus depreciation), disallowing certain expenses (fines, 50% of meals), granting exemptions (muni bond interest)


The book to tax reconciliation walks from book income to taxable income line by line. The bridge for a US C-corporation on Form 1120 sits in Schedule M-1 (or Schedule M-3 for larger entities) and looks like this:


```text
Net income per books (Schedule M-  1   line   1  )
+ Federal income tax per books
+ Excess   of   capital losses over capital gains
+ Income subject to tax but not on the books
+ Book expenses not deducted on the   return   (M&E, penalties, etc.)
+ Depreciation excess (  if   book depreciation > tax depreciation)
─ Income on books but not on the   return   (muni bond interest, life insurance proceeds)
─ Deductions on the   return   but not on books (§  179  , bonus depreciation excess)
= Taxable income (Form   1120   line   28  , before NOL and special deductions)
```


Every add-back and every subtraction on this bridge is a book-tax difference mechanical output of reconciliation.


Related:[Section 174 R&D capitalization guide](https://www.finlens.app/blogs/section-174-r-and-d-capitalization) covers one of biggest current-cycle book-tax differences.


## Permanent differences vs temporary differences framework


Every book-tax difference falls into one of two buckets. Get classification wrong and deferred tax accounting under ASC 740 gets wrong.


### Permanent differences


Items recognized for **book OR tax purposes but never both** . They do not reverse over time. They do not create deferred tax assets or liabilities. They show up in effective tax rate reconciliation on financial statement footnote they permanently change effective rate.


**Common permanent differences:**


- **Tax-exempt municipal bond interest** income on books, not on return
- **Non-deductible fines and penalties** expense on books, not deductible on return
- **50% of meals and entertainment** (business meals) half is disallowed for tax under §274
- **Non-deductible portion of officer life insurance premiums** (§264) expense on books, not deductible
- **Non-deductible lobbying expenses** expense on books, not deductible
- **Dividends received deduction (DRD)** on C-corp dividends from other US corps deduction on return, not a book item
- **Federal income tax expense** a book expense but not deductible on federal return itself
- **Goodwill amortization** some cases where book impairment or amortization differs from §197 15-year amortization


### Temporary differences


Items in both **book and tax income but recognized in different periods** . They reverse over time. Under ASC 740 they create deferred tax assets (DTA) or deferred tax liabilities (DTL) on balance sheet.


**Common temporary differences:**


- **Depreciation** GAAP straight-line vs §168 MACRS + §179/bonus. Tax typically front-loads deduction; book spreads it. Creates DTL (tax basis < book basis).
- **Bad debt reserve** book uses allowance method; tax uses specific write-off under §166. Creates DTA (book expense not yet deductible for tax).
- **Deferred revenue** book recognizes revenue when earned (ASC 606); tax recognizes on receipt under §451(c) for advance payments. Creates DTL (tax income before book income).
- **Warranty accruals** book accrual under ASC 460; tax deduction on payment. Creates DTA.
- **Inventory reserves** book LCM (lower of cost or market) vs tax cost basis. Creates DTA.
- **Prepaid expenses** book capitalizes and amortizes; tax generally deducts on payment (12-month rule). Creates DTL.
- **Deferred compensation and pension** timing differences on §162(m) and §404 deductibility.
- **NOL carryforwards** created book/tax difference; ASC 740 DTA for future NOL utilization (subject to valuation allowance).


The distinction matters because:


1. **Reversal timing** temporary differences will unwind on future returns; permanent differences will not
2. **Deferred tax computation** only temporary differences generate ASC 740 DTA/DTL
3. **Effective tax rate impact** permanent differences change ETR; temporary differences do not (they change current-vs-deferred split of tax expense)


*Figure 1. Permanent differences never reverse; temporary differences do. Only temporary create deferred taxes under ASC 740.*


## Schedule M-1 vs Schedule M-3 which one to file


The IRS uses total assets at year-end to determine which schedule entity files.


Total assets


Schedule required


Forms it appears on


< $10 million


Schedule M-1 — basic summary bridge


Form 1120 (C-corp), Form 1120-S (S-corp), Form 1065 (partnership)


≥ $10 million


Schedule M-3 — granular line-by-line


Same forms; Schedule M-3 replaces M-1


≥ $50 million (partnership) or $10M (corp)


Schedule M-3 mandatory, cannot elect M-1


Same


‍


*Total assets are measured on entity's Schedule L (Balance Sheet per Books). Verify current thresholds against current IRS Form 1120 / 1120-S / 1065 instructions before filing.*


### Schedule M-1 structure (basic)


- **Line 1** Net income (loss) per books
- **Lines 2–4** Add-backs: federal tax, excess capital losses, income on return not on books
- **Line 5** Expenses on books not deducted on return (M&E, penalties, depreciation excess if any)
- **Line 6** Total (Lines 1–5 sum)
- **Lines 7–8** Deductions: income on books not on return, deductions on return not on books
- **Line 9** Subtotal
- **Line 10** Income per return (should equal Form 1120 line 28)


The M-1 is designed to fit on a single page. It's shortest workpaper on return.


### Schedule M-3 structure (granular)


Schedule M-3 is a multi-page workpaper with three parts:


- **Part I** Financial statement reconciliation. Starts with net income per worldwide consolidated GAAP financial statements, adjusts for entities NOT included on US tax return, arrives at net income of US filer.
- **Part II** Income and expense items with book-tax differences, broken out line-by-line. Each line has four columns: Income Per Income Statement, Temporary Difference, Permanent Difference, Income Per Tax Return.
- **Part III** Same four-column structure for expense items.


Part II and Part III together typically produce 100+ line items on a complex Schedule M-3. This is where large-entity book-tax reconciliation becomes a genuine multi-person workpaper.


Related:[QBI deduction (Section 199A) 2026](https://www.finlens.app/blogs/qbi-deduction) covers how reconciled taxable income flows to QBI calculation on pass-through returns.


## OBBBA 2025 new book-tax differences that entered workpaper this year


The One Big Beautiful Bill Act (P.L. 119-21, signed 2025) introduced several changes that show up in 2025 and 2026 book to tax reconciliations:


### §174A restoration R&E expensing reversal


- Pre-OBBBA (2022–2024): §174 forced R&E capitalization over 5 years domestic / 15 years foreign
- Post-OBBBA (tax years beginning after 12/31/2024): domestic R&E is immediately deductible under §174A; foreign R&E still capitalized 15 years
- **New book-tax difference type in 2025/2026 M-1 or M-3:** Book depreciation of §174 balance (capitalized under GAAP) vs tax immediate expensing (§174A). This creates a large temporary difference REVERSAL in 2025 for calendar-year taxpayers accelerating unamortized 2022–2024 §174 balance.
- **Small business retroactive election** (≤$31M gross receipts) requires amended prior-year book-tax reconciliations


Full detail:[Section 174 R&D capitalization](https://www.finlens.app/blogs/section-174-r-and-d-capitalization) .


### 100% bonus depreciation restoration


- OBBBA restored 100% bonus depreciation permanently for property acquired AND placed in service AFTER 1/19/2025
- **Book-tax difference:** book depreciation (straight-line under GAAP over asset life) vs tax depreciation (100% year-one expensing). Massive current-year temporary difference (DTL) that reverses over book depreciable life.
- Pre-1/19/2025 acquisitions stay on TCJA phase-down (60% bonus in 2024, etc.)


Full detail:[Section 179 vs bonus depreciation (MACRS)](https://www.finlens.app/blogs/section-179-vs-bonus-depreciation) .


### QBI deduction interaction


- Pass-through entities: QBI deduction is claimed on individual return, not pass-through's Form 1065 or 1120-S. It does NOT appear on pass-through's Schedule M-1/M-3.
- However, book-tax differences at entity level flow through K-1s and affect owner's QBI base
- OBBBA-widened phase-in windows change effective per-owner QBI benefit, which changes value of entity's book-tax planning


Full detail:[QBI deduction (Section 199A) 2026](https://www.finlens.app/blogs/qbi-deduction) .


### PTET deduction interaction


- Under IRS Notice 2020-75, a pass-through entity's PTET payment is a federal business deduction on Form 1065/1120-S no SALT cap
- On M-1/M-3, PTET expense (deductible for tax) may not be on book P&L in same period (if book records PTET as owner-level state tax through equity)
- **Book-tax difference:** PTET expense deducted on tax return but recorded as equity distribution (not P&L expense) on books


Full detail:[Passthrough entity tax (PTET) elections](https://www.finlens.app/blogs/passthrough-entity-tax-ptet-elections) .


*Figure 2. Book income + permanent adjustments + temporary adjustments = taxable income. The workpaper structure is same on M-1 and M-3 M-3 just breaks it into more lines.*


## A worked example $620K book to $510K taxable, line by line


A C corporation with $5,000,000 in revenue closes year with $620,000 of net income per its GAAP financial statements. The Form 1120 will show taxable income of $510,000 a $110,000 gap Schedule M-1 walks through:


Line


Adjustment


Amount


Type


M-1 Line 1


Net income per books


$620,000


—


M-1 Line 2


+ Federal income tax expense per books


+$130,000


Permanent (M-1 sense)


M-1 Line 5


+ 50% of business meals disallowed


+$8,000


Permanent


M-1 Line 5


+ Fines and penalties


+$2,000


Permanent


M-1 Line 5


+ Bad debt reserve increase (allowance not written off)


+$10,000


Temporary


M-1 Line 7


- Tax-exempt municipal bond interest


-$15,000


Permanent


M-1 Line 8


- Bonus depreciation in excess of book depreciation


-$245,000


Temporary


M-1 Line 10


Taxable income per return


$510,000


—


‍


Net permanent impact: −$5,000. Net temporary impact: −$235,000 in current year (reverses over asset depreciable life for $245K, and unwinds when bad debt is actually written off for $10K).


Under ASC 740, $235,000 net temporary difference creates a **deferred tax liability** of approximately $49,350 at a 21% federal rate (before considering state tax). That DTL sits on balance sheet and unwinds as temporary differences reverse in future years.


## Advanced items that show up on real M-3 workpapers


Competitor CPA firm guides (Beancount.io, TaxAct, Thomson Reuters) surface several items that don't fit simple M-1 walk but show up frequently on M-3 filings and audit workpapers. Every mature book-to-tax reconciliation checks for these:


- **Section 481(a) adjustments** when entity changes an accounting method (e.g., accrual to cash, R&E capitalization to §174A expensing), cumulative catch-up flows through M-1/M-3 and creates a multi-year temporary difference under Form 3115 automatic-consent procedure. On OBBBA-year returns, §481(a) is mechanism that carries pre-2025 §174 capitalized balance through to current year.
- **Stock-based compensation timing** GAAP recognizes book expense over vesting period at grant-date fair value under ASC 718. Tax deduction on non-qualified stock options occurs at exercise (spread between exercise price and FMV); qualified incentive stock options (ISOs) may generate no tax deduction at all. The book vs tax gap on stock-based comp rarely matches in a given year.
- **Accrued expenses and 2.5-month rule** under §461(h) economic performance rules, most accrued liabilities are deductible only when paid, with exceptions for recurring items paid within 2.5 months after year-end. Book expense at accrual, tax expense at payment (subject to 2.5-month safe harbor) is a common temporary difference source.
- **Charitable contribution carryovers** C corporations are limited to 10% of taxable income for charitable deductions, with 5-year carryovers. Book expenses full contribution when made; tax may defer it under 10% limit. Multi-year DTA schedule required.
- **Schedule UTP (Uncertain Tax Positions)** entities with $10M+ total assets AND an ASC 740-10 (FIN 48) reserved position must disclose uncertain tax positions on Schedule UTP. This is not itself a book-tax difference but is tied to same ASC 740 workpapers that drive deferred tax computation.
- **Section 267 related-party expense timing** accrued payments to related parties are not deductible until paid; if related party is on cash basis, book expense recognition without tax deduction creates a temporary difference.


## The Corporate Alternative Minimum Tax (CAMT) a 2026 book-income wrinkle for large corporations


Introduced by Inflation Reduction Act, Corporate Alternative Minimum Tax (CAMT) imposes a **15% minimum tax on Adjusted Financial Statement Income (AFSI)** for corporations with average AFSI over $1 billion across a three-year period. For tiny subset of C corporations subject to CAMT, book-to-tax reconciliation gets a second layer AFSI adjustments (which are book-income modifications, not tax-income modifications) get their own workpaper, filed on Form 4626.


For CPA firms serving CAMT-subject clients (typically largest audit and specialty tax practices), M-3 workpaper feeds into but does not equal CAMT AFSI computation. This is a rising 2026 topic Treasury guidance on AFSI has been unfolding since CAMT took effect for tax years beginning after 12/31/2022.


For sub-$1B clients (vast majority of CPA firm engagements), CAMT does not apply but book-tax reconciliation still needs to be defensible against possibility that firm grows into CAMT range in future years.


## Why sloppy reconciliations get flagged in 2026 IRS AI audit selection


The IRS's Automated Under Reporter (AUR) system has been augmented in 2025-2026 with machine-learning-driven audit selection, especially for partnerships, S corporations, and businesses with layered ownership. **Schedule M-3 is one of richest structured-data sources IRS has** every line is comparable across peers and prior years. Algorithmic triage now flags:


- Large unexplained "other adjustments" totals if majority of reconciliation sits on generic "other" lines, examiner assumes disorganization or concealment
- Year-over-year swings in categories without an obvious driver reserves that double, bonus depreciation that triples, charitable carryovers that suddenly appear
- Permanent differences classified as temporary (or vice versa) misclassification signals preparer doesn't understand underlying transactions
- Book numbers on Schedule L that don't tie to financial statements provided to lenders, investors, or filed with SEC increasingly cross-checkable as IRS gains access to more third-party data


The cure is unglamorous: clean, well-documented books. The reconciliation is only as defensible as what sits beneath it.


## The workflow a CPA firm actually runs


For every C-corp, S-corp, or partnership return:


**Step 1 Confirm which schedule applies.** Pull Schedule L (Balance Sheet per Books) to check total assets at year-end. Under $10M → M-1. At or above → M-3.


**Step 2 Establish book income starting point.** Pull financial statement net income from QBO (or audit-adjusted financial statements). This is Line 1 of Schedule M-1 or top of Schedule M-3 Part I.


**Step 3 Inventory permanent differences.**


- Tax-exempt interest income
- Non-deductible fines and penalties
- 50% of business meals (§274)
- Officer life insurance premiums (§264)
- Non-deductible lobbying (§162(e))
- DRD on C-corp dividends received (Form 1120 only)
- Federal income tax expense (per books) added back on M-1 Line 2


**Step 4 Inventory temporary differences.**


- Depreciation: book vs §168 MACRS + §179 + bonus depreciation. Pull full fixed-asset schedule from Form 4562.
- Bad debt: book allowance vs specific write-off
- Deferred revenue: book (ASC 606) vs tax (§451(c) or §451(b) as amended by TCJA)
- Prepaid expenses: book amortization vs tax 12-month rule
- Warranty accruals: book accrual vs tax deduction on payment
- Inventory reserves: book LCM vs tax cost
- Deferred compensation


**Step 5 Add OBBBA-year adjustments.**


- §174A: prior-year §174 capitalized amounts being accelerated (temporary difference reversal)
- Bonus depreciation on post-1/19/2025 acquisitions (temporary difference creation)
- PTET: entity-level state tax deduction if paid


**Step 6 Compute reconciliation.** Book income + permanent add-backs + net temporary add-backs − permanent subtractions − net temporary subtractions = taxable income. Match to Form 1120 line 28 (or 1120-S line 21, 1065 line 22).


**Step 7 Compute ASC 740 deferred taxes (financial statement side).** For each temporary difference, compute deferred tax asset or liability at enacted future rate. Update balance sheet deferred tax accounts. Document valuation allowance analysis if applicable.


**Step 8 Prepare Schedule M-1 (or M-3 with all schedules).**


- M-1: single-page fill in
- M-3: Part I (financial statement rec) + Part II (income items) + Part III (expense items). Verify total-book-income and total-tax-income reconcile.


**Step 9 Document workpaper.** Every difference permanent or temporary needs a workpaper reference showing source (fixed asset schedule, meal log, muni bond statement, warranty accrual analysis). This is what defends return on IRS examination.


**Step 10 Multi-year tracking.** Temporary differences roll forward. Every year's book to tax reconciliation extends prior year's deferred tax schedule.


## Common book to tax reconciliation traps


**Trap 1 Depreciation mismatch.** Firms compute tax depreciation on Form 4562 without pulling corresponding book depreciation from QBO. The result: a plug figure on M-1 that doesn't tie to fixed asset ledger. Fix: pull book depreciation directly from QBO, tax depreciation from Form 4562, difference is temporary difference.


**Trap 2 Meals treated as 100% deductible.** Post-TCJA, most business meals are 50% deductible. Book P&L records 100% of meal cost; 50% haircut is a permanent add-back on M-1 Line 5. Missing this is one of most common IRS exam findings.


**Trap 3 Deferred revenue treated as fully recognized.** SaaS and subscription businesses often have material deferred revenue balances. Book recognizes revenue over subscription period (ASC 606); tax may accelerate under §451(c) advance payment rules. Missing tax-side acceleration understates current-year taxable income.


**Trap 4 §179 and bonus depreciation reversed as permanent instead of temporary.** These are TEMPORARY differences tax basis is lower than book basis, but both eventually recover same total cost. Recording as permanent skips deferred tax liability recognition under ASC 740.


**Trap 5 Not reconciling to Schedule L.** Schedule L (Balance Sheet per Books) has to tie to year-end trial balance. If Schedule L doesn't match audited financials, M-1/M-3 that flows from it will be off. Fix: reconcile Schedule L to audited or reviewed balance sheet FIRST.


**Trap 6 Ignoring M-3 detail requirement.** Above $10M total assets, M-3 requires line-by-line breakdown. Rolling up 20 individual meals into a single "Meals & Entertainment" line meets M-1 requirements but not M-3. IRS examiners flag M-3 aggregation errors quickly.


## Where book to tax reconciliation goes wrong upstream


Every M-1 or M-3 workpaper depends on underlying book income being right. The failure points:


- **Stripe revenue booked net of fees** book income understates gross revenue; M-3 reconciliation to gross tax income doesn't tie
- **Deferred revenue schedules not maintained** book vs tax timing difference cannot be computed without a schedule showing when each contract's revenue recognizes book-side vs tax-side
- **Fixed asset schedule blended with expense line items** capital purchases expensed instead of capitalized never generate a depreciation difference on M-1 because they're not on Schedule L to begin with
- **Foreign R&E costs blended with domestic** post-OBBBA two get different treatment; §174A restoration applies to domestic only
- **PTET payment recorded through equity instead of P&L** P&L side of entry needs a tax adjustment; equity-side entries don't create an obvious book-tax difference in base M-1 walk


Finlens automates QBO ledger cleanup that feeds these numbers transaction categorization with per-client rules, Stripe payout decomposition, deferred revenue schedules, clean journal entries for fixed asset capitalization vs expensing. For clients where book to tax reconciliation is a material return workpaper, that upstream cleanup is what makes M-1 or M-3 defensible. Related:[1099-K reporting](https://www.finlens.app/blogs/1099-k-reporting-cpa-guide) and[tax resolution CPA firm process and fees](https://www.finlens.app/blogs/tax-resolution) cover parallel upstream-cleanup workflows.


## Conclusion


**Pick one C-corp, S-corp, or partnership client where 2026 book to tax reconciliation is coming up and underlying QBO isn't fully clean bring three months of their QBO and prior-year return, and we'll walk book-income starting point live before M-1 or M-3 workpaper is drafted.**


$10M


M-1 vs M-3 threshold


2 buckets


permanent vs temporary


ASC 740


deferred tax follows


## M-1 or M-3 coming up?
Book income needs to tie.


Finlens categorizes activity, decomposes Stripe payouts, and builds deferred revenue schedules so Schedule L and M-1/M-3 walk cleanly to Form 1120.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


‍


## Frequently asked questions


### What is difference between Schedule M-1 and Schedule M-3?


Schedule M-1 is a single-page summary bridge between net income per books and taxable income, filed by entities with total assets under $10 million. Schedule M-3 is a multi-page granular line-by-line reconciliation, required for entities with total assets at or above $10 million. M-3 includes Part I (financial statement reconciliation), Part II (income items with book/tax/temporary/permanent columns), and Part III (expense items with same four columns).


### What are permanent differences in book to tax reconciliation?


Permanent differences are items recognized for book OR tax purposes but never both. They do not reverse over time and do not create deferred taxes under ASC 740. Common examples: tax-exempt municipal bond interest (book income only), non-deductible fines and penalties (book expense only), 50% of business meals under §274, non-deductible portion of officer life insurance premiums, and non-deductible lobbying expenses. They change effective tax rate but not current-vs-deferred split of tax expense.


### What are temporary differences in book to tax reconciliation?


Temporary differences are items recognized in both book income and tax income but in different reporting periods. They reverse over time and create deferred tax assets (DTA) or liabilities (DTL) under ASC 740. Common examples: MACRS tax depreciation vs GAAP straight-line, bad debt allowance vs specific write-off, deferred revenue (ASC 606 vs §451(c)), prepaid expense amortization, and warranty accruals.


### What is book income vs taxable income?


Book income is net income calculated under GAAP for financial statement purposes. Taxable income is income calculated under Internal Revenue Code and reported on Form 1120, 1120-S, or 1065. The two diverge because GAAP and tax law measure income differently accelerated tax depreciation, disallowed meals/entertainment, tax-exempt interest exclusions, and dozens of other book/tax differences produce gap. The book to tax reconciliation walks from one to other.


### What are book to tax differences?


Book to tax differences are specific items that cause book income and taxable income to diverge in a given year. They fall into two buckets: permanent differences (never reverse muni bond interest, non-deductible penalties, 50% M&E) and temporary differences (reverse over time depreciation methods, bad debt, deferred revenue, warranty accruals). Every M-1 or M-3 line item is a book to tax difference.


### What is Schedule M-1?


Schedule M-1 is IRS reconciliation schedule filed with Form 1120, 1120-S, or 1065 for entities with total assets under $10 million. It's a single-page workpaper starting with net income per books (Line 1), adding back federal income tax and expenses not deductible on return, subtracting income on books not on return and deductions on return not on books, and arriving at income per tax return (Line 10, which should equal Form 1120 line 28).


### What is Schedule M-3?


Schedule M-3 is IRS reconciliation schedule filed by entities with total assets at or above $10 million. It replaces Schedule M-1 with a granular line-by-line reconciliation. Part I reconciles worldwide consolidated GAAP net income to net income of US filer. Part II breaks out income items with four columns (Income Per Income Statement / Temporary Difference / Permanent Difference / Income Per Tax Return). Part III does same for expense items.


### How does OBBBA affect book to tax reconciliation in 2026?


OBBBA (P.L. 119-21, signed 2025) introduced several new book-tax differences: (1) §174A restoration of domestic R&E immediate expensing creates a temporary difference reversal for prior TCJA-capitalized amounts; (2) 100% bonus depreciation restoration for post-1/19/2025 acquisitions creates large current-year temporary differences (DTL); (3) PTET deduction interaction between entity-level state tax expense and equity distribution treatment creates a book/tax timing consideration; (4) QBI-related timing adjustments flow through pass-through K-1s to owner-level QBI calculation.


### How do deferred tax assets and liabilities relate to book to tax reconciliation?


Under ASC 740 (US GAAP for income taxes), every temporary difference creates a deferred tax asset (if book basis exceeds tax basis, deductible in future) or a deferred tax liability (if tax basis exceeds book basis, taxable in future). The book to tax reconciliation identifies temporary differences; ASC 740 workpaper computes DTA/DTL at enacted future tax rate and posts them to balance sheet. Permanent differences do not create DTA/DTL they only affect effective tax rate reconciliation.


### Can Finlens help with book to tax reconciliation?


Finlens doesn't compute M-1/M-3 or ASC 740 deferred taxes directly tax software (Drake, Lacerte, ProSeries, UltraTax) and CPA's workpaper application handle that. What Finlens automates is upstream ledger cleanup: transaction categorization, Stripe payout decomposition, deferred revenue schedules, fixed asset capitalization vs expense classification. That gives tax preparer a clean book income starting point top of Schedule M-1 Line 1 or M-3 Part I instead of a spreadsheet rebuild at filing time.


*Schedule M-1 and M-3 total-asset thresholds, filing requirements, and OBBBA-year book-tax differences change frequently. This article reflects guidance current as of 2026-07-30 based on P.L. 119-21 (OBBBA), IRS Form 1120 / 1120-S / 1065 instructions, and ASC 740 as publicly available at that date. Verify current thresholds at*[irs.gov](https://www.irs.gov/forms-pubs) *before filing. Nothing in this article is legal or tax advice engage a licensed CPA, EA, or attorney for actual book-to-tax reconciliation and return preparation. Third-party trademarks (QuickBooks®, Stripe®) belong to their respective owners.*


‍
