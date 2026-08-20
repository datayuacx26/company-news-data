---
schema_version: "1.0.0"
document_id: "3de7abff15c0a40d688c04fb98bef165ca147863a92acfa3bf72ab72f89606df"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/real-estate-accounting-bookkeeping"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:14:47.490481+00:00"
fetched_at: "2026-08-06T14:14:48.910031+00:00"
content_hash: "sha256:40545f2a3080b388add182775e800157ff1e53522e6a2b9301cbcc619aeefe6a"
---

# Real Estate Accounting: Bookkeeping, Taxes, and Financial Reporting

Real estate accounting is where a client's tax return can shift by five figures on decisions that were made or missed in general ledger months earlier. Whether a roof replacement is capitalized or expensed under tangible property regulations. Whether depreciation follows correct recovery period. Whether a §1031 exchange is documented cleanly enough to defer gain. Whether §199A safe harbor is available on rental income. All four decisions live in books, not on return.


## Per property books first rule


Every rental property should have its own set of books. That means:


- Its own bank checking account.
- Ideally its own credit card (or a dedicated card tag if consolidation is required).
- Its own set of P&L accounts, or class/location tracking in QBO Classes.
- Its own accumulated depreciation and net book value on FAS.
- Its own tenant security deposit liability line.


The single most common bookkeeping failure at real estate clients is commingled expenses across a portfolio. When repairs, mortgage interest, or utility payments cross properties without allocation discipline, per-property P&L is unreliable, and every downstream decision (sell / hold / refinance / cost seg / dispose) is made on bad data.


Multi-property portfolios still benefit from per-property tagging even when consolidated bank accounts are unavoidable. Every transaction hits general ledger with a property tag. The trial balance summarizes at entity level; reports drill to property level.


## The chart of accounts


Beyond a standard SMB chart, a rental real estate business needs:


**Revenue** (per property):


- Rental Income
- Late Fees
- Pet Fees / Other Fees
- Service Charges Passed Through (utility reimbursement, etc.)


**Operating expenses** (per property):


- Property Management Fees
- Repairs and Maintenance (see TPR treatment below)
- Utilities
- Property Insurance
- Property Taxes
- HOA / Condo Fees
- Advertising and Vacancy
- Mortgage Interest Rental (deductible)
- Professional Fees (legal, tax preparation portion)


**Depreciation** :


- Depreciation Expense Building (27.5 or 39-year, depending on use)
- Depreciation Expense Improvements (usually 15 or 27.5-year)
- Depreciation Expense Furniture and Equipment (5 or 7-year)


**Balance sheet** :


- Building Cost (excluding land)
- Land Cost (not depreciable)
- Building Improvements
- Accumulated Depreciation
- Mortgage Payable (split short-term / long-term)
- Tenant Security Deposits Liability (contra to a separate bank account holding deposits)


A rental P&L that lumps all repairs into a single "Maintenance" line and doesn't split building from land doesn't produce a defensible return.


## Depreciation 27.5 vs. 39 years


Under MACRS:


- **Residential rental property** 27.5-year straight-line
- **Commercial (non-residential) rental property** 39-year straight-line
- **Land** not depreciable
- **Land improvements** (paving, fencing, landscaping) 15-year
- **Personal property inside a rental** (appliances, furniture, carpeting) 5-year
- **Qualified Improvement Property (QIP)** for commercial interiors 15-year with[bonus depreciation](https://www.finlens.app/blogs/section-179-vs-bonus-depreciation) eligible


The **50/50 test on residential** : a property is residential rental if 80% or more of gross rental income is from dwelling units. Mixed-use properties (retail on ground floor, apartments above) require allocation between 27.5 and 39-year components.


Land allocation is practitioner's first decision. The purchase price is split between building and land based on:


1. Local tax assessor's ratio, or
2. An appraisal at acquisition, or
3. Insurance replacement cost documentation


The IRS accepts any reasonable method, but choice is documented in workpaper on day one. A common practitioner error: using tax assessor's ratio blindly when assessor's land value is understated for property tax purposes.


## The Tangible Property Regulations repair vs. capitalize


Every real estate expense that touches a building falls under **Treas. Reg. §1.263(a)** tangible property regulations (TPR). The framework:


**Deductible repairs** are amounts paid to keep property in its ordinarily efficient operating condition routine maintenance that does not materially add to value or prolong life of property.


**Capitalized improvements** are betterments, restorations, or adaptations under BAR test:


- **Betterment** an improvement that ameliorates a material condition or defect, is a material addition, or is a material increase in productivity, efficiency, strength, or quality.
- **Adaptation** an amount that adapts a unit of property to a new or different use.
- **Restoration** a replacement of a major component, restoration of property after casualty loss, restoration of a property to like-new condition, or rebuild after end of class life.


For rental real estate, **unit of property** is generally entire building but nine building systems are treated as separate units of property for improvement analysis:


1. HVAC
2. Plumbing
3. Electrical
4. Escalators / elevators
5. Fire protection / alarm
6. Security
7. Gas distribution
8. Structural components
9. Other structural systems


Replacing a component of a system (a single unit of HVAC, one water heater, a section of roof) is often deductible as a repair. Replacing majority of a system (all HVAC units, entire roof, all electrical) is capitalized.


The **de minimis safe harbor** under §1.263(a)-1(f) allows expensing items ≤$2,500 per invoice (or ≤$5,000 with an AFS). The **routine maintenance safe harbor** under §1.263(a)-3(i) allows expensing recurring maintenance expected to occur more than once during property's class life (10 years for buildings). The **small taxpayer safe harbor** allows a landlord with under $10M in unadjusted basis and average annual gross receipts under $10M to expense up to lesser of $10,000 or 2% of unadjusted basis per building per year.


Every rental property client should have written safe-harbor elections on file. The elections are made on a timely-filed original return and continue until revoked.


## 1031 exchange bookkeeping


A §1031 like-kind exchange defers gain when a real property held for investment or business use is exchanged for another qualifying real property. Since Tax Cuts and Jobs Act, only real property qualifies (personal property exchanges were eliminated).


The bookkeeping mechanics:


1. **Relinquished property** cost basis and accumulated depreciation come off books. The realized gain (fair value less adjusted basis) is calculated but not recognized.
2. **Boot received** any non-like-kind property or cash received in exchange is taxable up to amount of realized gain. Booked as ordinary income or capital gain to extent recognized.
3. **Replacement property basis** carryover basis from relinquished property is adjusted for boot given, boot received, and any additional cash paid. Depreciation on replacement continues on two schedules: carryover basis depreciates on original schedule (as if exchange never happened), and any excess basis depreciates as new property.
4. **Qualified intermediary escrow** funds held by QI never touch taxpayer's books except as a receivable and eventual cash-in on replacement close.


The 45 day identification and 180-day exchange window are strict. Miss either and exchange fails, deferral collapses, and entire realized gain is taxable in year of relinquished-property sale.


Every real estate client considering a sale needs a 1031 conversation before property goes under contract. Once sale closes and cash hits seller's account, exchange is gone.


## Passive activity loss limitations


Under IRC §469, rental real estate losses are passive by default and deductible only against passive income subject to two important exceptions:


**Real estate professional status** (§469(c)(7)) if taxpayer meets 750-hour and 50% tests and materially participates in each rental activity (or elects aggregation under §1.469-9(g)), losses are nonpassive and offset ordinary income. Covered in detail in[Finlens REPS blog](https://www.finlens.app/blogs/cost-segregation-for-real-estate) .


**$25,000 active participation allowance** taxpayers with AGI under $100,000 who actively participate in rental real estate can deduct up to $25,000 of losses against ordinary income. Phased out from AGI $100K–$150K.


Losses in excess of what's allowed under either exception carry forward as **suspended passive activity losses** and release when passive income is generated, when taxpayer qualifies as a REP, or when activity is fully disposed of in a taxable transaction.


Every rental property client's PAL schedule needs to be maintained on ledger property by property, year by year. The PAL number that shows up on Form 8582 following March should be reconciled to workpaper.


## Section 199A safe harbor for rentals


Under **Rev. Proc. 2019-38** , rental real estate can qualify as a §162 trade or business and therefore for §199A 20% QBI deduction under a safe harbor requiring:


- Separate books and records for each rental enterprise.
- At least 250 hours of rental services per year (documented time log).
- Contemporaneous records including time reports, description of services, and identification of who performed them.


The safe harbor does not apply to personal residences used by taxpayer, triple-net-lease properties, or property occupied for any part of year as a residence by taxpayer.


For QBI-eligible taxpayers below taxable income threshold (approximately $391K/$782K single/MFJ for 2026 indexed), safe harbor is often difference between a $0 and a $10,000+ QBI deduction. It requires same recordkeeping discipline as REPS a contemporaneous time log and often gets skipped.


## Tenant security deposits


Security deposits are not revenue and not expense. They are **liabilities** :


- Deposit received: Dr. Bank (dedicated security-deposit account); Cr. Tenant Security Deposit Liability.
- Deposit refunded at move-out: Dr. Tenant Security Deposit Liability; Cr. Bank.
- Deposit withheld for damages: Dr. Tenant Security Deposit Liability; Cr. Repair Expense (or reduce repair expense to net effect).


Most states require security deposits to be held in a separate account, and some require interest to be paid to tenant. Booking security deposits as revenue is a common error that materially misstates P&L and creates a tax return problem when deposit is refunded.


## How Finlens keeps real estate ledger tied


Finlens reconciles QBO general ledger for real estate clients with a per-property workflow that keeps depreciation, PAL, and TPR classification current every close.


- **Per-property tagging.** Every transaction hits QBO with a property class or location code. Reports drill from portfolio-level to property-level without exception.
- **TPR classification queue.** Repairs above de minimis threshold are queued for CPA classification repair vs. capitalization before they post to FAS.
- **Depreciation schedule.** Buildings, land improvements, personal property, and QIP are on FAS with correct recovery periods and cost seg schedules where applicable.
- **PAL roll-forward.** Suspended passive activity losses are carried per property per year on ledger, feeding directly into Form 8582 at return time.
- **§199A safe harbor documentation.** Time-log requirements and 250-hour tracking are queued for client, with a status flag on each rental enterprise.
- **Tenant security deposits.** Deposits sit in a dedicated liability account, tied to a dedicated bank account, and refunds process cleanly through liability without touching P&L.
- **1031 exchange support.** When a client enters an exchange, Finlens tracks QI escrow, carryover basis calculation, and two-schedule depreciation continuation on replacement property.


Finlens does not replace Buildium, AppFolio, or RentRedi operational side (tenant management, rent collection, work orders) stays in property management tool. Finlens is ledger and workpaper layer that keeps general ledger tied to property management system every month.


## Conclusion


**Real estate accounting is a per-property discipline before it is a return-preparation exercise.** The depreciation schedule, TPR classifications, PAL schedule, §199A time log, and 1031 carryover basis all live in ledger and return is only as defensible as those workpapers.


27.5 / 39


yr depreciation


TPR


repair vs. capitalize


§199A


250-hour safe harbor


## Twelve doors.
One spreadsheet FAS?


Finlens tags every transaction to a property, queues repair-vs-capitalization decisions before they post, and carries the PAL, depreciation, and §199A time logs on the ledger tied to the trial balance.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


see how Finlens tags every transaction to a property, queues repair-vs-capitalization decisions before they post, and carries PAL and depreciation schedules on ledger tied to trial balance.


Bring file for client with twelve rental doors, a spreadsheet FAS three years out of date, no §199A time logs, and a suspended PAL number that hasn't been reconciled since last CPA. That's file this workflow is built for.


## Frequently asked questions


### **Should each rental property have its own LLC?**


Asset-protection question, not accounting. Bookkeeping-wise, per-property books can be maintained inside one LLC using classes; LLC decision is driven by liability exposure and lender requirements. Discuss with client's attorney.


### **What is difference between residential and commercial rental depreciation?** ‍


Residential is 27.5-year straight-line; commercial is 39-year. A property is residential if 80%+ of gross rental income is from dwelling units. Mixed-use requires allocation.


### **Can I deduct all repairs to a rental property?**


Only routine repairs. Under Treas. Reg. §1.263(a)-3, betterments, adaptations, or restorations under BAR test must be capitalized and depreciated. The de minimis, routine maintenance, and small taxpayer safe harbors allow expensing of many smaller items with proper elections.


### **How does cost segregation affect FAS?**


A cost seg study reclassifies portions of building cost (typically 20–30% for residential, 25–40% for commercial) into 5, 7, or 15-year property, accelerating depreciation. The reclassification produces a §481(a) catch-up in year of study (or over four years for negative adjustments). Every cost seg client needs full FAS rebuilt on ledger.


### **When is a rental a business for §199A?**


When Rev. Proc. 2019-38 safe harbor is met (separate books, 250 hours of services, time log) or when rental rises to a §162 trade or business under general principles (multiple properties, active management, business-like operations).


### **Are tenant security deposits taxable income?**


No they are liabilities until earned. Only portion actually retained for damages or unpaid rent becomes income (or offsets related expense). Booking deposits as revenue is a recurring bookkeeping error.


The authoritative reference for rental property accounting on tax side is[IRS Publication 527 Residential Rental Property](https://www.irs.gov/publications/p527) . For front-loaded depreciation strategy that pairs with real estate holdings, see Finlens guide to[cost segregation for real estate](https://www.finlens.app/blogs/cost-segregation-for-real-estate) .


‍
