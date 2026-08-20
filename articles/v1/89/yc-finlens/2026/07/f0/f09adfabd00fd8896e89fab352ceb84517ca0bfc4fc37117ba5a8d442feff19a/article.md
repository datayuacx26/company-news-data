---
schema_version: "1.0.0"
document_id: "f09adfabd00fd8896e89fab352ceb84517ca0bfc4fc37117ba5a8d442feff19a"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/foreign-tax-credit-form-1116"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T22:39:39.250313+00:00"
fetched_at: "2026-07-31T22:39:40.641602+00:00"
content_hash: "sha256:1bbd24c1b699c7153e60a423f236d186e54f8f8eb7237c6f4f794159cc437743"
---

# Foreign Tax Credit Form 1116 (2026): The CPA Firm Practitioner Guide

## **Key takeaways**


- Form 1116 required when foreign taxes > $300 single / $600 MFJ; narrow passive-only exception applies below that
- Separate Form 1116 for each of 7 income categories (passive, general, §951A GILTI, foreign branch, §901(j), treaty-resourced, lump-sum)
- FTC limitation formula: (foreign-source taxable income in category ÷ worldwide taxable income) × US tax
- Carryback 1 year, carryforward 10 years except §951A GILTI, which has NO carryover
- Cannot claim FTC on income excluded via Form 2555 (Foreign Earned Income Exclusion); must reduce foreign taxes proportionally
- [Finlens](https://www.finlens.app/accountants) automates QBO ledger cleanup per-jurisdiction categorization, transaction tagging, foreign-source income allocation so Form 1116 Part I builds off clean data


## What Form 1116 actually does?


Form 1116 exists because US taxes its citizens, resident aliens, and green card holders on **worldwide income** while country where income was earned typically also taxes that income. Without relief, same dollar of income gets taxed twice.


The foreign tax credit is primary relief mechanism. It reduces US tax liability dollar-for-dollar by amount of qualifying foreign income tax paid or accrued, subject to a limitation that prevents credit from exceeding US tax attributable to foreign-source income.


**Form 1116 is required when any of these apply:**


- Creditable foreign taxes exceed $300 (single) or $600 (MFJ)
- Foreign income is NOT limited to passive-category income reported on qualified payee statements (1099-DIV, 1099-INT, Schedule K-1, Schedule K-3)
- Foreign tax credit carryovers or carrybacks apply
- Foreign tax redeterminations occurred during year
- Foreign income spans multiple categories


**The $300/$600 exception when you can skip Form 1116:**


- ALL foreign-source gross income is passive category income, AND
- ALL foreign income and foreign taxes are reported on qualified payee statements, AND
- Total creditable foreign taxes ≤ $300 (single) or ≤ $600 (MFJ)


When all three conditions are met, FTC is claimed directly on Schedule 3 (Form 1040), line 1 without filing Form 1116.


## Form 1116 vs Form 2555 two double-taxation tools


Every expat return decides between foreign tax credit (Form 1116) and Foreign Earned Income Exclusion (Form 2555). Both reduce double taxation but work differently.


Factor


Form 1116 (FTC)


Form 2555 (FEIE)


Mechanism


Credit against US tax dollar-for-dollar


Excludes foreign earned income from US taxable income


2025 income cap


None on credit amount itself


$130,000 per qualifying person


Income types


Earned + passive (dividends, interest, rents, royalties)


Earned income only (wages, self-employment)


Eligibility


Foreign-source income + creditable foreign tax


Foreign earned income + tax home abroad + bona fide residence OR 330 days abroad in 12 months


Carryovers


1 year back, 10 years forward by category


None


Best for


High-tax countries (Germany, France, Japan, UK)


Low-tax or no-tax countries (UAE, Singapore, Cayman)


IRA implications


Preserves compensation for IRA eligibility


Excluded income generally does NOT count for IRA


Coordination


Cannot claim FTC on income excluded under §911 (Form 2555)


Cannot exclude foreign taxes paid on excluded income


*Comparison sourced from IRS.gov, Taxes for Expats (accessed 2026-07-30), and Publication 54.*


**Practical rule:** in a high-tax country, Form 1116 usually delivers more relief because foreign taxes exceed US tax on same income. In a low-tax country, Form 2555 usually wins because exclusion removes income before US tax is calculated. Families with qualifying children need particular care Form 2555 can reduce refundable credit outcomes by removing earned income from calculation.


Related:[QBI deduction (Section 199A) 2026](https://www.finlens.app/blogs/qbi-deduction) covers a parallel election-driven optimization that interacts with FTC on cross-border business income.


*Figure 1. Form 1116 credits foreign taxes against US liability; Form 2555 excludes income. Choose by country tax rate, income type, and carryover value.*


## The 7 income categories separate Form 1116 per category


The IRS requires a separate Form 1116 for each income category. Mixing categories on one form distorts limitation calculation and produces an incorrect credit.


Category


Common example


Carryover allowed?


Passive category


Foreign dividends, interest, rents, royalties, annuities, most investment gains


1 back / 10 forward


General category


Foreign wages, self-employment income, active business income


1 back / 10 forward


Section 951A (GILTI)


Global Intangible Low-Taxed Income of US shareholders of CFCs


No carryover


Foreign branch category


Business profits from a qualified business unit (QBU) in a foreign country


1 back / 10 forward


Section 901(j) sanctioned-country


Income from IRS-designated sanctioned countries; special credit restrictions


1 back / 10 forward


Treaty-resourced income


Income treated as foreign-source by treaty provision


1 back / 10 forward


Lump-sum distributions


Certain foreign pension or retirement lump-sum payments


1 back / 10 forward


*Categories per IRC §904(d) and current Form 1116 instructions.*


**Section 951A (GILTI) is trap.** A US shareholder of a controlled foreign corporation (CFC) reports GILTI on their return; associated foreign tax credit uses its own limitation calc AND has **no carryback or carryforward** . Any GILTI-category excess foreign tax is permanently lost in year incurred. This is why GILTI planning for CFC owners often involves timing foreign tax payments carefully you cannot bank them for future use.


The most common category-mixing error: foreign dividends (passive) and foreign wages (general) from same country get combined on a single Form 1116. The limitation is computed against wrong base, generating either too much or too little credit.


## The foreign tax credit limitation formula


The FTC is limited to US tax attributable to foreign-source income in each category:


**FTC limitation = (foreign-source taxable income in category ÷ worldwide taxable income) × US tax before credit**


Excess foreign tax paid above this limitation becomes a carryover:


- 1 year back (via Form 1040-X for prior year), OR
- Up to 10 years forward
- Both by category passive carryovers cannot offset general-category income


## Worked example $78K wages + $2K dividends, dual-category filing


Adapted from Taxes for Expats (accessed 2026-07-30). US citizen living in a high-tax country with 2025 foreign income:


Item


Amount


Foreign wages (general category)


$78,000


Foreign dividends (passive category)


$2,000


Foreign tax on wages


$16,000


Foreign tax withheld on dividends


$300


Worldwide taxable income


$90,000


US tax before credits


$13,500


**Passive category Form 1116 (dividends only):**


- Limitation = ($2,000 ÷ $90,000) × $13,500 = **$300**
- Foreign tax on dividends = $300
- **Allowed credit: $300** (full amount, limitation not binding)


**General category Form 1116 (wages only):**


- Limitation = ($78,000 ÷ $90,000) × $13,500 = **$11,700**
- Foreign tax on wages = $16,000
- **Allowed credit: $11,700** (limitation binding)
- **Excess foreign tax: $4,300** (carries back 1 year or forward up to 10 years, general category only)


**Total FTC claimed: $12,000** (from two separate Forms 1116, summarized on Part IV).


Same file, different result if taxpayer had confused two: adding $78,000 + $2,000 into one Form 1116 would produce a limitation of $12,000 against $16,300 total foreign tax, allowed credit still $12,000 but passive-vs-general carryover tracking would be wrong going forward, blocking optimization in future years.


*Figure 2. Each category has its own limitation. Excess general-category tax carries forward; passive tax was fully absorbed in year 1.*


## Currency conversion payment-date rule (not annual average)


One of highest-frequency errors on Form 1116 is currency conversion. The IRS rule:


- **Cash-basis foreign taxes paid** convert at exchange rate **on date paid or withheld** , not an annual average
- **Accrual-basis foreign taxes accrued** may use **average exchange rate for tax year** if taxpayer elects and meets specific conditions
- **Passive income reported on qualified payee statements** IRS accepts US-dollar amount shown on Form 1099-DIV or similar; no separate conversion required


Practical implication: a taxpayer with 12 monthly foreign payroll withholdings in a currency that fluctuated 8% during year should NOT convert all 12 at year-end rate or annual average. The correct method is 12 separate conversions at each pay-date rate then sum US-dollar amounts. On a $16,000 foreign tax bill, difference between correct per-date conversion and lazy annual-average conversion can be $500–$1,500.


The Form 1116 instructions specifically require a **detailed explanation attached** whenever foreign currency is converted showing source of rate, date used, and conversion methodology.


## The high tax kickout (HTKO) rule a trap most preparers miss


When passive category income is taxed abroad at a rate **exceeding highest US tax rate that can be imposed on that income after expense allocation** , IRS mandatorily reclassifies that income out of passive category and into another category (usually general).


The mechanic:


1. Passive income (foreign dividend, interest) hits return
2. Taxpayer computes effective foreign tax rate after expense allocation
3. Compares to highest applicable US tax rate on that income type
4. If foreign > US rate, income is "kicked out" of passive
5. Both passive Form 1116 and receiving-category Form 1116 must have "HTKO" entries showing reclassification


Why it matters: high-taxed passive income and its associated foreign tax move together to a different category. This can move a taxpayer from generating passive-category excess (that carries) to using higher tax in general category (where it may be absorbed by wage-based limitation headroom).


Most preparers apply passive/general split mechanically based on income type. The HTKO adjustment is not optional it's an IRS-required reclassification triggered by effective-rate test.


## Schedules B and C 2021+ additions


Since tax year 2021, Form 1116 has two supporting schedules that catch out unwary preparers:


- **Schedule B (Form 1116) Foreign Tax Credit Carryover Reconciliation.** Required when current year uses a prior-year carryover OR generates a current-year carryover. Reconciles carryover balance by category and by year. Do not confuse with Schedule B (Form 1040) that reports interest and dividends and is a separate form.
- **Schedule C (Form 1116) Foreign Tax Redeterminations.** Required when a prior-year foreign tax was later refunded, reduced, or additionally assessed by foreign government. Reports redetermination by category and by year, plus resulting US tax redetermination.


If a foreign tax redetermination occurs and Form 1116 Schedule C is not filed, IRS can impose a **failure-to-notify penalty** in addition to any tax adjustment. The 10-year record retention window matters here a 2023 foreign tax refund on a 2020 return can require an amended 2020 US return in 2026.


## Special situations that show up on real Form 1116 returns


Extracted from IRS.gov and TFX (accessed 2026-07-30):


- **French CSG/CRDS taxes** under a 2019 diplomatic memo, IRS agreed that French Contribution Sociale Généralisée and Contribution au Remboursement de la Dette Sociale are NOT social taxes covered by US-France totalization agreement. Individual US taxpayers who paid CSG/CRDS but did not claim FTC on those amounts can amend prior returns (10-year refund window). Write "French CSG/CRDS Taxes" in red at top of Form 1040-X.
- **Charitable contributions to Mexico, Canada, or Israel** unusually, charitable contributions to charities in these three countries MUST be apportioned against foreign-source income for FTC limitation. Contributions to charities in other countries generally are NOT apportioned.
- **Interest expense apportionment** mortgage interest and investment interest must be apportioned between US and foreign source income under Treas. Reg. §1.861-9T. Most tax software handles this poorly.
- **Foreign-sourced qualified dividends and long-term capital gains** taxed at reduced US rate (0%/15%/20%). Must be adjusted on Form 1116 line 1a to reflect reduced-rate treatment. Missing this adjustment overstates FTC.
- **Treaty-based reduced foreign tax** amount qualifying for FTC is treaty-reduced tax, NOT higher amount actually withheld. If a treaty allows a 15% withholding but foreign country withheld 30%, only 15% qualifies for FTC. The excess can be claimed as a refund from foreign country.
- **Nonrefundable social security tax** under a US totalization agreement, foreign social security paid does NOT qualify for FTC (it's covered by treaty separately). The exceptions are countries WITHOUT totalization agreements.


## 9 common Form 1116 errors CPA firms see


Adapted from Taxes for Expats and Mahoney CPA competitive analysis:


1. **Wrong income basket** combining passive dividends and general wages on a single Form 1116
2. **Crediting non-income taxes** VAT, sales tax, property tax, foreign interest/penalties do NOT qualify for FTC
3. **Single yearly exchange rate** using annual average on paid taxes when payment-date conversion is required
4. **Forgetting Form 2555 reduction** claiming FTC on income excluded under FEIE (must reduce proportionally)
5. **Country columns treated as income-type** Form 1116 Part II columns A/B/C are countries, not wage/dividend/interest
6. **Missing Schedule B** required for carryovers used or generated in current year
7. **Uncategorized carryovers** treating passive carryovers as usable against general-category limitation
8. **Ignoring foreign tax redeterminations** Schedule C required for prior-year foreign tax refunds or reassessments
9. **Missing explanation statement** currency conversions, amended foreign taxes, unusual allocations need a short statement per IRS Form 1116 instructions


## The workflow a CPA firm actually runs


For every client with foreign-source income:


**Step 1 Inventory foreign income by category and country.** Cross-check against 1099-DIV Box 7 (foreign tax paid), Schedule K-1 items 21A/L/M, K-3 Part II, W-2 Box 14 foreign tax, and any foreign tax return.


**Step 2 Verify each foreign tax against 4 IRS tests.** Imposed on taxpayer; paid or accrued in tax year; legal obligation; income tax or tax in lieu of income tax.


**Step 3 Segregate by category.** Passive, general, GILTI, foreign branch, 901(j), treaty-resourced, lump-sum. Assign each foreign tax to its category.


**Step 4 Apply HTKO test on passive income.** Compute effective foreign rate on passive income; compare to highest US rate; reclassify if HTKO applies.


**Step 5 Convert foreign taxes to USD at payment/withholding date.** Attach a detailed exchange-rate schedule if requested.


**Step 6 Compute limitation per category.** (Foreign-source income ÷ worldwide taxable income) × US tax.


**Step 7 Determine allowed credit per category.** Lesser of foreign tax paid OR limitation.


**Step 8 Track excess for carryover.** Per category, back 1 year OR forward up to 10 years. Update Schedule B.


**Step 9 Handle Form 2555 coordination.** Reduce foreign taxes by portion allocable to excluded income.


**Step 10 File return.** Form 1116 for each category, Schedule B for carryovers, Schedule C for redeterminations, Part IV summary. Total flows to Schedule 3 (Form 1040) line 1.


Related:[1099-K reporting for CPAs](https://www.finlens.app/blogs/1099-k-reporting-cpa-guide) covers a parallel PSE-based reconciliation workflow.


## Where FTC calculation goes wrong upstream


Every Form 1116 depends on clean foreign-source income classification at transaction level. The failure points:


- **Foreign-source revenue blended with US-source in general ledger** a client with cross-border operations whose QBO revenue accounts don't tag jurisdiction can't produce accurate foreign-source income for Form 1116 Part I
- **Foreign currency payments booked at bank-deposit rates instead of payment-date rates** understates or overstates foreign tax paid depending on FX movement
- **Foreign tax withholdings blended with US tax withholdings** losing split between what qualifies for FTC and what doesn't
- **1099-DIV Box 7 (foreign tax paid) not tracked against dividend source country** cannot pass HTKO test or category split
- **K-3 partnership foreign items missed on flow-through returns** a US shareholder of a foreign partnership may have FTC-relevant items on K-3 that never get to individual return


Finlens automates QBO ledger cleanup transaction categorization with per-client rules including jurisdiction tags, Stripe payout decomposition (which surfaces foreign-processed transactions), deferred revenue schedules that maintain source-country attribution. For expat and cross-border clients where FTC is a material return item, that upstream cleanup is what makes Form 1116 Part I defensible. Related:[Section 174 R&D capitalization](https://www.finlens.app/blogs/section-174-r-and-d-capitalization) covers parallel domestic-vs-foreign R&E cleanup.


## Conclusion


**Pick one expat or cross-border client whose 2025 return needs Form 1116 and underlying QBO isn't jurisdiction-tagged yet bring three months of their QBO and last-year Form 1116, and we'll walk foreign-source income input pool live before return goes to tax preparer.**


7 categories


separate Form 1116


1-yr back / 10-yr fwd


carryover


Payment-date


currency conversion


## Form 1116 season?
Foreign-source income needs to tie.


Finlens tags QBO transactions per jurisdiction and per category so Form 1116 Part I builds from a defensible source-of-income schedule, not a spreadsheet rebuild.


[Book a Walkthrough →](https://cal.com/finlens/intro?utm_campaign=foreign-tax-credit-form-1116)[See how it works →](https://www.finlens.app/accountants)


‍


## Frequently asked questions


### What is Form 1116 used for?


IRS Form 1116 is used by individuals, estates, and trusts to calculate and claim foreign tax credit a dollar-for-dollar credit against US tax liability for foreign income taxes paid or accrued on foreign-source income. Corporations use Form 1118 for same purpose.


### When is Form 1116 required?


Form 1116 is required when creditable foreign taxes exceed $300 (single) or $600 (MFJ) OR foreign income is not limited to passive category income reported on qualified payee statements OR foreign tax credit carryovers/carrybacks apply OR foreign tax redeterminations occurred OR foreign income spans multiple income categories.


### Can I skip Form 1116?


Yes, in one narrow case: ALL foreign-source gross income is passive category income, AND all foreign income and taxes are reported on qualified payee statements (1099-DIV, 1099-INT, K-1, K-3), AND total creditable foreign taxes are ≤ $300 (single) or ≤ $600 (MFJ). In that case, claim FTC directly on Schedule 3 (Form 1040) line 1 without filing Form 1116.


### What is foreign tax credit limitation?


The FTC limitation is maximum credit allowed per income category, computed as: (foreign-source taxable income in category ÷ worldwide taxable income) × US tax before credits. The allowed credit is smaller of foreign tax paid or this limitation.


### How does foreign tax credit carryover work?


Excess foreign tax paid above current-year limitation carries back 1 year (via Form 1040-X on prior return) OR forward up to 10 years. Carryovers are tracked by income category passive carryovers cannot offset general-category limitation. Section 951A (GILTI) has NO carryover; excess GILTI-category foreign tax is permanently lost in year incurred.


### What's difference between foreign tax credit and foreign earned income exclusion?


Form 1116 (FTC) reduces US tax dollar-for-dollar by foreign taxes paid on foreign-source income. Form 2555 (FEIE) excludes up to $130,000 (2025) of foreign earned income from US taxable income. FTC works with earned and passive income; FEIE only for earned. FTC has 1-back/10-forward carryovers; FEIE has none. High-tax country → usually FTC; low-tax country → usually FEIE.


### How do I calculate foreign tax credit?


Step 1: identify foreign-source income by category. Step 2: sum foreign taxes paid or accrued in USD at payment-date rates. Step 3: compute limitation per category = (foreign-source income in category ÷ worldwide taxable income) × US tax. Step 4: allowed credit = smaller of foreign tax paid OR limitation. Step 5: handle excess as carryover per category rules.


### How many Form 1116s do I file if I have foreign wages and foreign dividends?


Two one Form 1116 for general category (wages) and one for passive category (dividends). Each category is calculated separately with its own limitation. Part IV of "primary" Form 1116 summarizes credits from all categories.


### What is high-tax kickout rule?


The HTKO rule mandates that passive-category income taxed abroad at a rate exceeding highest US rate that could be imposed on that income (after expense allocation) is reclassified out of passive category into another category (usually general). Both Form 1116s (passive one and receiving category) must show "HTKO" entries. This is not optional IRS requires reclassification.


### Can I claim FTC and Foreign Earned Income Exclusion together?


Yes, but not on same income. Any foreign taxes paid on income excluded under Form 2555 must be reduced proportionally before claiming FTC. Common structure: exclude first $130,000 of foreign wages via Form 2555, then claim FTC on wages above $130,000 plus foreign passive income.


### Do I need to file Schedule B (Form 1116)?


Yes, if current-year Form 1116 uses a foreign tax credit carryover from a prior year OR generates a current-year carryover. Schedule B reconciles carryover balances by income category and year. Schedule B (Form 1116) is NOT same as Schedule B (Form 1040) they are entirely different forms.


### What happens if my foreign tax gets refunded or reassessed after I file?


File Schedule C (Form 1116) in year of redetermination, and file Form 1040-X to amend US return for year of original foreign tax. Failure to notify IRS of a foreign tax redetermination can trigger a failure-to-notify penalty in addition to tax adjustment.


### Does Finlens compute foreign tax credit directly?


No tax software (Drake, Lacerte, ProSeries, UltraTax) runs Form 1116 and limitation calc. What Finlens automates is upstream QBO ledger cleanup: transaction categorization with per-jurisdiction tags, Stripe payout decomposition, deferred revenue schedules maintained per country. That gives tax preparer a defensible foreign-source income and foreign tax pool for Form 1116 Part I and Part II.


*Form 1116 categories, thresholds, and mechanics change frequently. This article reflects guidance current as of 2026-07-30 based on IRC §901, §904(d), §911, Form 1116 instructions (2025), and IRS Publication 514. Verify current instructions at*[irs.gov/instructions/i1116](https://www.irs.gov/instructions/i1116) *before filing. Nothing in this article is legal or tax advice engage a licensed CPA, EA, or attorney for actual foreign tax credit calculation and return preparation. Third-party trademarks (QuickBooks®, Stripe®) belong to their respective owners.*


‍
