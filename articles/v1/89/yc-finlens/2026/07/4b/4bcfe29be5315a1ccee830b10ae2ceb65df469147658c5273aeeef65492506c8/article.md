---
schema_version: "1.0.0"
document_id: "4bcfe29be5315a1ccee830b10ae2ceb65df469147658c5273aeeef65492506c8"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/estimated-tax-payments"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T22:39:39.250313+00:00"
fetched_at: "2026-07-31T22:39:40.641602+00:00"
content_hash: "sha256:77d580be34b32fe594b46ac258d539d5dc2f712d952de05641d04241d7a65f17"
---

# Estimated Tax Payments: The CPA Firm Practitioner Guide to Form 1040-ES and Safe Harbor Rules

## **Key takeaways**


- $1,000 individual / $500 corporation threshold triggers estimated tax obligation
- Safe harbor: lesser of 90% current OR 100% prior (110% if AGI > $150K)
- 2026 deadlines: Apr 15, Jun 15, Sep 15, Jan 15 2027 quarters are 2-3-3-4 months, not equal
- Underpayment penalty computed on Form 2210 / Form 2220
- Annualized income installment method (Schedule AI, Form 2210) reduces penalty for uneven-income taxpayers
- [Finlens](https://www.finlens.app/accountants) automates QBO ledger cleanup that feeds current-year projection driving each quarter's Form 1040-ES worksheet


## Who must make estimated tax payments


Per IRS.gov (accessed 2026-07-30), following taxpayers generally must make estimated tax payments:


- **Individuals** (including sole proprietors, partners, S-corp shareholders) who expect to owe $1,000 or more in tax when return is filed
- **Corporations** that expect to owe $500 or more in tax
- **Recipients of significant non-W-2 income** interest, dividends, alimony, self-employment income, capital gains, prizes, awards, rental income, or K-1 flow-through income
- **Business owners** whose distributions are not subject to withholding
- **High-income W-2 employees** whose withholding is insufficient because compensation includes RSUs, ISOs at exercise, bonuses, or large capital gains events


Estimated tax covers not just income tax it also covers **self-employment tax** (15.3% for first $168,600 of 2026 self-employment income, then 2.9% Medicare) and **alternative minimum tax** if applicable.


**Three conditions to skip estimated taxes entirely** (individuals only):


1. You had no tax liability for prior year (total tax was $0 OR you weren't required to file)
2. You were a US citizen or resident alien for whole prior year
3. Your prior tax year covered a full 12-month period


If all three conditions apply, no estimated tax payments are required in current year regardless of income.


## The 2026 deadlines not equal-length quarters


The IRS's "quarterly" estimated-tax rhythm is not truly quarterly. The four periods are unequal:


Quarter


Income period


Due date


Q1


January 1 – March 31 (3 months)


April 15, 2026


Q2


April 1 – May 31 (2 months)


June 15, 2026


Q3


June 1 – August 31 (3 months)


September 15, 2026


Q4


September 1 – December 31 (4 months)


January 15, 2027


*Source: IRS Publication 505 and Baker Tilly (accessed 2026-07-30). If any due date falls on a Saturday, Sunday, or federal holiday, deadline shifts to next business day.*


The 2-3-3-4 month pattern trips up self-employed clients who assume even quarters. The Q2 window is only 2 months long a taxpayer with steady monthly income earns just 2/12 of annual income by June 15 deadline but must have paid roughly 3/12 of annual tax by then if paying evenly. The mismatch is why many practitioners use **annualized income installment method** (below) for uneven-income clients.


## The safe harbor rule how to avoid underpayment penalty


The IRS underpayment penalty applies when a taxpayer paid too little tax through withholding + estimated payments during year. But three "safe harbors" excuse penalty:


**Safe harbor 1 De minimis.** Owe less than $1,000 in tax after subtracting withholding and refundable credits. No penalty regardless of when payments were made.


**Safe harbor 2 90% current-year rule.** Paid at least 90% of current-year tax through combined withholding + timely estimated payments. Works when current-year tax can be estimated accurately.


**Safe harbor 3 100% (or 110%) prior-year rule.** Paid at least 100% of prior-year total tax through combined withholding + timely estimated payments. **The rule shifts to 110% of prior-year tax if prior-year AGI exceeded $150,000** ($75,000 if MFS). This is "prior-year safe harbor" most CPA-firm-managed clients target because prior-year numbers are already known no forecasting required.


**The penalty is calculated per quarter, not annually.** Missing any single deadline creates a small quarterly penalty even if annual total is correct by year-end. Form 2210 (individuals) or Form 2220 (corporations) computes exact penalty at IRS quarterly interest rate Q3 2026 was 8% APR. On a $10,000 underpayment held for two quarters, that's roughly $400 in penalty.


*Figure 1. Three safe harbors excuse underpayment penalty. Most firm-managed clients target 100% (or 110%) of prior-year tax because prior-year numbers are known.*


## The annualized income installment method for uneven cash flow


When income is uneven a self-employed consultant with a big Q4 project, a real estate investor with a mid-year closing, an executive with an RSU vesting event standard 25%-per-quarter payment often creates a penalty in earlier quarters despite annual total being on target.


The IRS's answer is **annualized income installment method** , computed on **Schedule AI of Form 2210** for individuals (or equivalent on Form 2220 for corporations). The method:


1. Compute year-to-date taxable income as of each quarter-end (Mar 31, May 31, Aug 31, Dec 31)
2. Annualize that income by multiplying by 12/3, 12/5, 12/8, 12/12 (respectively)
3. Compute annualized tax
4. Multiply by 22.5%, 45%, 67.5%, 90% (cumulative required percentages by quarter)
5. That's required payment through each quarter


**Worked example.** A consultant with a $180K income year that lands unevenly:


Quarter-end


YTD income


Annualized


Annualized tax @ 24%


Required cumulative payment


Mar 31


$20,000


$80,000


$19,200


$4,320 (22.5%)


May 31


$40,000


$96,000


$23,040


$10,368 (45%)


Aug 31


$80,000


$120,000


$28,800


$19,440 (67.5%)


Dec 31


$180,000


$180,000


$43,200


$38,880 (90%)


Under standard method, consultant would owe $9,720 by Apr 15 (25% of full-year $38,880). Under annualized method, only $4,320 is required by Q1 matching what was actually earned. The remaining $34,560 backloads into later quarters as income arrives.


The election is made on current-year return by filing Schedule AI along with Form 2210. It cannot be made retroactively for multi-year strategies taxpayer must maintain contemporaneous records of quarterly income.


Related:[QBI deduction (Section 199A) 2026](https://www.finlens.app/blogs/qbi-deduction) covers how projected taxable income for QBI phase-in feeds estimated-tax planning.


## Payment channels Executive Order 14247 pushing electronic


**Executive Order 14247 (signed 2025)** directs federal agencies to encourage electronic payment methods over paper-based transactions. Estimated tax payment options now include:


- **IRS Direct Pay** direct debit from bank account, no fee. Individuals only. Handles $10 million per transaction, up to two payments per day.
- **IRS Online Account** schedule up to 12 months of payments in advance, view history.
- **Business Tax Account** new business-side interface for estimated tax + federal tax deposits + notices.
- **Direct Pay for Businesses** corporation and partnership estimated payments.
- **IRS2Go mobile app** Direct Pay from mobile.
- **EFTPS (Electronic Federal Tax Payment System)** required for some business tax payments; free but 24-hour enrollment lead time.
- **Credit or debit card** via IRS-approved third-party processors; ~2% fee. Not usually cost-effective except for reward-earning strategies.
- **Same-day wire** bank-initiated; typically $30-$50 wire fee; for last-minute payments.
- **Form 1040-ES + check by mail** traditional; deadline based on USPS postmark date, not IRS receipt date.


Baker Tilly (2026-05-28) noted shift: "individuals making estimated tax payments may want to consider using IRS Direct Pay, EFTPS or IRS2Go mobile app" as electronic-first defaults.


Related:[1099-K reporting for CPAs](https://www.finlens.app/blogs/1099-k-reporting-cpa-guide) covers payment-network reporting side of same income streams.


*Figure 2. Seven payment channels for estimated taxes. Executive Order 14247 (2025) pushes electronic-first most CPA firms move clients to IRS Direct Pay or EFTPS.*


## Special rules that show up on real returns


Extracted from IRS Publication 505 and Baker Tilly (2026-07-30):


- **Farmers and fishermen** if two-thirds of gross income is from farming or fishing, only one estimated payment is required (January 15), OR they can skip estimated payments entirely and file/pay full return by March 1.
- **Higher-income taxpayers** 110% (not 100%) prior-year safe harbor applies when prior-year AGI exceeded $150,000 (or $75,000 if MFS). Missing this bumps taxpayer from safe harbor into penalty territory.
- **New businesses / first-year filers** no prior-year safe harbor available (prior-year tax was $0). Must use 90% current-year rule OR de minimis $1,000 rule. This is where first-year self-employed clients typically miss estimated taxes and face year-end penalty.
- **Retirement / disability waiver** IRS waives underpayment penalty if taxpayer retired after age 62 or became disabled during current or preceding tax year AND underpayment was due to reasonable cause not willful neglect. Requires Form 2210 with waiver box checked.
- **Casualty / disaster waiver** penalty waived when underpayment stemmed from a casualty, disaster, or other unusual circumstance and imposing penalty would be inequitable.
- **RSU / equity-comp events** a large RSU vesting mid-year can create a large tax shortfall. Employers withhold at a supplemental rate of 22% (or 37% above $1M), which is often below taxpayer's marginal rate. Use annualized income method OR make an ad-hoc estimated payment in quarter of event.


## Sample workflow for Cap firm


For every quarterly estimated-tax cycle:


**Step 1 Pull prior-year total tax.** From client's most recently filed Form 1040 (Line 24) or Form 1120 (Line 31). This anchors 100%/110% prior-year safe harbor.


**Step 2 Determine current-year projection.** Ordinary income from wages + business + K-1s + investment income − deductions. Overlay any known material events (RSU vesting, business sale, large capital gain).


**Step 3 Compute safe harbor floor.** Lesser of:


- 90% × projected current-year tax
- 100% × prior-year tax (or 110% if AGI > $150K)


**Step 4 Split into quarterly amounts.** Divide safe-harbor floor by 4 for even-income clients. Use annualized income method for uneven-income clients.


**Step 5 Deduct expected withholding.** Withholding counts as paid evenly across year regardless of when actually withheld. So a $60K W-2 job withholding $12K counts as $3K/quarter of estimated tax already paid.


**Step 6 Generate Form 1040-ES worksheet.** From tax software OR manual. Each quarter's payment = (safe-harbor floor − expected withholding) ÷ 4.


**Step 7 Set client payment channel and reminders.** Confirm IRS Direct Pay or EFTPS is set up. Automate calendar reminders 10 days before each deadline.


**Step 8 Monitor income deltas quarterly.** If client's actual quarterly income diverges materially from projection, recalculate for remaining quarters. Prior-year safe harbor lets you leave payments unchanged if current-year projection worsens; 90% current-year rule requires you to increase.


**Step 9 Year-end true-up.** In Q4 (November-December), review current-year actuals. If projection was too low, catch up on Q4 payment. If too high, deliver a refund via tax return.


**Step 10 File Form 2210 or Form 2220 if any penalty was incurred.** Compute penalty by quarter using IRS interest rate table. Elect annualized income method if beneficial.


## Where estimated-tax calculation goes wrong upstream


Every quarterly payment depends on a clean current-year income projection. The failure points:


- **Business owner's YTD income unknown until books are closed** if QBO isn't reconciled monthly, Q2 or Q3 projection is a guess
- **Stripe revenue understated because payments are booked net of fees** projection uses $50K gross when actual gross is $58K, understating estimated tax
- **Deferred revenue timing wrong** subscription income booked when cash received instead of over service period distorts monthly income run rate
- **K-1 estimates missed for pass-through clients** S-corp or partnership K-1 not projected until year-end return prep
- **RSU / bonus / equity events not fed to projection** comes as a Q4 surprise


Finlens automates QBO ledger cleanup that feeds these projections: transaction categorization with per-client rules, Stripe payout decomposition, deferred revenue schedules maintained monthly. For self-employed and pass-through clients where estimated tax is a recurring quarterly workpaper, that upstream cleanup is what makes Form 1040-ES worksheet defensible. Related:[book to tax reconciliation](https://www.finlens.app/blogs/book-to-tax-reconciliation) covers how same clean books feed year-end tax reconciliation.


## Conclusion


**Pick one self-employed or pass-through client where current-year projection is stale and Q3 or Q4 estimated tax is coming up bring three months of their QBO and prior-year Form 1040, and we'll walk current-year projection live before next quarter's Form 1040-ES is drafted.**


$1,000


individual threshold


90% / 100% / 110%


safe harbor


8% APR


underpayment penalty


## Q3 estimated tax coming up?
Books need to project.


Finlens keeps QBO reconciled month-by-month so the current-year projection driving Form 1040-ES stays defensible before the next deadline hits.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


‍


## Frequently asked questions


### What are four due dates for quarterly estimated tax payments?


For 2026: April 15, June 15, September 15, and January 15, 2027. These correspond to income periods of Jan 1-Mar 31, Apr 1-May 31, Jun 1-Aug 31, and Sep 1-Dec 31 respectively note periods are 2-3-3-4 months, not equal quarters. If a due date falls on a weekend or holiday, it shifts to next business day.


### How do I pay estimated taxes to IRS?


Several channels: IRS Direct Pay (free, from bank account), IRS Online Account (schedule ahead), IRS2Go mobile app, EFTPS (business payments), Direct Pay for Businesses, credit/debit card via third-party processor (fee applies), or Form 1040-ES by mail. Executive Order 14247 (2025) encourages electronic payment methods over paper.


### What is safe harbor rule for estimated tax?


No underpayment penalty applies if you paid LESSER of: (a) 90% of current-year tax, OR (b) 100% of prior-year tax through withholding and timely estimated payments. **The rule shifts to 110% of prior-year tax if prior-year AGI exceeded $150,000** ($75,000 if MFS). Most CPA-firm-managed clients target prior-year safe harbor because prior-year numbers are already known.


### What happens if I miss an estimated tax payment?


The IRS assesses an underpayment penalty computed per quarter on Form 2210 (individuals) or Form 2220 (corporations), calculated at prevailing IRS interest rate (8% APR in Q3 2026). The penalty is based on how much was underpaid AND how long underpayment persisted. Missing a single deadline creates a small quarterly penalty; missing all four creates a substantially larger annual penalty.


### Can I skip a quarterly estimated tax payment?


Technically yes, but you'll pay an underpayment penalty for each quarter skipped unless: you meet safe harbor by paying enough in other quarters, OR you meet de minimis rule (less than $1,000 owed after withholding), OR you qualify for one of penalty waivers (age 62+ retirement, disability, casualty/disaster).


### What is Form 1040-ES?


Form 1040-ES is IRS worksheet + payment voucher individuals use to figure and pay estimated tax. Nonresident aliens use Form 1040-ES(NR). Corporations use Form 1120-W. The worksheet estimates AGI, taxable income, taxes, deductions, and credits for year; voucher accompanies mail payments. Electronic payments do not require paper voucher.


### How do I calculate estimated tax payments?


Two methods: (1) **Standard** divide safe-harbor floor by 4 for equal quarterly payments; (2) **Annualized income installment method** compute payment based on income actually earned by each quarter-end, using 22.5% / 45% / 67.5% / 90% cumulative-required percentages. The annualized method benefits uneven-income taxpayers by matching payment timing to income timing.


### What is underpayment penalty?


The underpayment penalty is a per-quarter interest charge on amount underpaid, at IRS quarterly interest rate (typically 3-8% APR in recent years). Computed on Form 2210 (individuals) or Form 2220 (corporations). Not treated as a "penalty" for deduction purposes it's non-deductible.


### What is annualized income installment method?


An IRS-provided alternative penalty computation on Schedule AI of Form 2210 for uneven-income taxpayers. It computes required payments based on income actually earned by each quarter-end rather than dividing annual tax evenly. Beneficial when income concentrates in later quarters (RSU vesting, Q4 business sale, seasonal income). Election is made on current-year return.


### How do I avoid estimated tax penalties if I'm self-employed?


Three practical rules: (1) target prior-year safe harbor pay 100% of last year's tax (or 110% if AGI > $150K) evenly across four quarters; (2) reconcile QBO monthly so current-year projection stays accurate; (3) if income is uneven, use annualized income installment method to match payments to income timing. Set up IRS Direct Pay or EFTPS with calendar reminders 10 days before each deadline.


### Does Finlens help with estimated tax payment calculations?


Finlens doesn't file Form 1040-ES or compute underpayment penalty directly that's tax software (Drake, Lacerte, ProSeries, UltraTax) and CPA workpaper. What Finlens automates is upstream QBO ledger cleanup: transaction categorization, Stripe payout decomposition, deferred revenue schedules maintained monthly. That gives tax preparer a clean current-year projection input, so each quarter's Form 1040-ES worksheet doesn't require a spreadsheet rebuild.


*Estimated tax rules, deadlines, penalty rates, and safe-harbor thresholds change annually. This article reflects guidance current as of 2026-07-30 based on IRC §6654 (individuals), §6655 (corporations), IRS Publication 505, and Form 1040-ES / 2210 / 2220 instructions publicly available at that date. Verify current deadlines and thresholds at*[irs.gov/businesses/small-businesses-self-employed/estimated-taxes](https://www.irs.gov/businesses/small-businesses-self-employed/estimated-taxes) *before filing. Nothing in this article is legal or tax advice engage a licensed CPA, EA, or attorney for actual estimated-tax planning and Form 1040-ES preparation. Third-party trademarks (QuickBooks®, Stripe®) belong to their respective owners.*


‍


‍
