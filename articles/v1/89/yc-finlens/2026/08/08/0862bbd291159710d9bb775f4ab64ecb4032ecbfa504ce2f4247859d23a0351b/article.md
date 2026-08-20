---
schema_version: "1.0.0"
document_id: "0862bbd291159710d9bb775f4ab64ecb4032ecbfa504ce2f4247859d23a0351b"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/fixed-asset-accounting-schedule-maintenance"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:14:47.490481+00:00"
fetched_at: "2026-08-06T14:14:48.910031+00:00"
content_hash: "sha256:ec310ad4ea8ea232cef11de615c451bf4f6f15f94748966ce7c46b399bb7d4e8"
---

# Fixed Asset Accounting: The CPA Firm's Perpetual Schedule Playbook

Fixed asset accounting is workpaper category where concept-versus-operations gap is widest. Every accounting textbook covers definition of PP&E,[depreciation methods](https://www.finlens.app/blogs/depreciation-methods) , and impairment triggers. Almost none cover actual operational mechanic how a[CPA firm](https://www.finlens.app/accountants) maintains a perpetual fixed asset schedule (FAS) across a book of SMB clients, keeps[QuickBooks](https://www.finlens.app/resources/quickbooks-automation) Online tied to schedule month-by-month, and produces roll-forward workpaper an auditor accepts on first pass.


## What counts as a fixed asset


A fixed asset is a tangible asset used in operations for more than one year that is not held for sale in ordinary course of business. Under GAAP, these appear on balance sheet as **Property, Plant, and Equipment (PP&E)** at historical cost, less accumulated depreciation.


The four tests for capitalization:


1. **Tangible** physical form (buildings, machinery, vehicles, furniture, IT equipment, leasehold improvements).
2. **Useful life greater than one year** asset's benefit extends beyond current reporting period.
3. **Not held for resale** inventory is expensed differently.
4. **Above capitalization threshold** entity's policy dollar floor (often $2,500 or $5,000; see below).


Intangibles like software licenses and internally developed software are also capitalized under separate GAAP guidance (ASC 350) but they are recorded outside PP&E account.


## The capitalization policy


Every set of GAAP books needs a written capitalization policy stating:


- **The dollar threshold.** Purchases below threshold are expensed. Common private-company thresholds: $2,500 or $5,000, aligned with IRS §1.263(a)-1(f) de minimis safe harbor.
- **The class-by-class useful life table.** Buildings 30–40 years, leasehold improvements shorter of useful life or lease term, machinery 5–10 years, vehicles 3–5 years, computers 3–5 years, furniture 7 years.
- **The depreciation method by class.** Most SMBs use straight-line for financial reporting; MACRS is for tax only.
- **What costs are capitalized as part of acquisition.** Purchase price + sales tax + freight + installation + testing + interest on constructed assets (during construction period).
- **The distinction between capitalization and repair.** Additions that increase service potential are capitalized; those that maintain existing service potential are expensed.


Every audit begins with capitalization policy. If policy does not exist in writing, workpaper is already deficient before auditor opens FAS.


## Acquisition cost what gets capitalized


Historical cost includes all expenditures necessary to bring asset to its intended use:


**Capitalized:**


- Invoice price minus discounts
- Sales tax
- Freight, shipping, insurance in transit
- Installation and setup
- Testing and calibration
- Site preparation
- Legal fees for real estate acquisitions
- Interest on borrowings used to finance construction (during construction period only, ASC 835-20)


**Expensed:**


- Training costs
- Maintenance contracts
- Repairs to bring a used asset up to prior operating condition
- Interest after asset is placed in service


For constructed assets, all materials, labor, overhead allocated to construction, and capitalizable interest are included in cost. This is where many SMB books err internal labor cost of building out a warehouse fit-up is often expensed when it should be capitalized as leasehold improvements.


## Depreciation book vs. tax


The most persistent source of client confusion: **book depreciation and tax depreciation are different, and both need to run in parallel.**


**Book depreciation** (GAAP financial statements):


- Straight-line is default and most common method.
- Useful life is estimated **service life** how long asset will be used in operations, which may be shorter than its physical life.
- Salvage value is estimated at acquisition and deducted from cost before depreciation.
- Methods available: straight-line, units of production, sum-of-the-years'-digits, declining balance, double-declining balance.


**Tax depreciation** (Form 4562, IRS):


- MACRS (Modified Accelerated Cost Recovery System) fixed class lives set by IRS (5-year, 7-year, 15-year, 27.5-year, 39-year).
- Bonus depreciation and §179 expensing may accelerate write-off further.
- No salvage value assumed.
- Half-year and mid-quarter conventions apply.


Every FAS a CPA firm maintains needs two parallel columns: book accumulated depreciation and tax accumulated depreciation. The difference between them is a **temporary book-to-tax difference** that flows to[Schedule M-1](https://www.finlens.app/blogs/book-to-tax-reconciliation) or M-3 on corporate return and to deferred tax asset/liability on balance sheet.


Skipping this reconciliation is number-one M-1 audit finding on SMB C-corp returns.


## Section 179 and bonus depreciation tax side interaction


For 2026, §179 expensing allows up to $1,220,000 of qualifying property to be expensed in year placed in service, with a phase-out beginning at $3,050,000 of total qualifying property. Bonus depreciation under §168(k) is at 40% for property placed in service in 2026 (down from 100% pre-2023; scheduled to phase to 20% in 2027 and 0% in 2028 absent legislation).


The impact on FAS: tax basis of a §179 or bonus asset can be reduced (or eliminated) in year one for tax purposes, but book depreciation continues over asset's service life. The book-tax difference in year one is largest, then reverses over remaining book life.


Every §179 election and bonus depreciation calculation needs to live on FAS alongside book depreciation schedule, not on a separate tax schedule that only gets touched once a year at return time.


## Impairment testing


Fixed assets are tested for impairment when events or changes in circumstances indicate carrying value may not be recoverable (ASC 360-10-35-21). The five triggering events every workpaper should reference:


1. Significant decrease in market price of asset.
2. Significant adverse change in extent or manner in which asset is used.
3. Significant deterioration in physical condition.
4. Costs of acquisition or construction significantly exceed original expectations.
5. Current-period operating loss combined with a history of losses that indicate future losses.


The recoverability test: undiscounted future cash flows expected from use and eventual disposition of asset are compared to its carrying value. If cash flows are lower, impairment is recognized as difference between carrying value and fair value.


Impairment is an acceleration of depreciation it is charged to operations in same section of income statement as depreciation, not below line as a special item. For SMB clients, isolated one-off equipment breakdowns rarely warrant impairment; a change in depreciation schedule is usually right response.


## Journal entries four life-cycle events


**1. Acquisition**


```text
Dr. Fixed Asset (at total capitalized cost)   XXX
Cr. Cash / Accounts Payable / Note Payable       XXX
```


**2. Monthly depreciation**


```text
Dr. Depreciation Expense                       XXX
Cr. Accumulated Depreciation                     XXX
```


**3. Disposal sale**


```text
Dr. Cash                                       XXX
Dr. Accumulated Depreciation                   XXX
Cr. Fixed Asset                                  XXX
Cr. Gain on Sale   of   Asset (  if   applicable)        XXX
```


Or if sold at a loss:


```text
Dr. Cash                                       XXX
Dr. Accumulated Depreciation                   XXX
Dr. Loss on Sale   of   Asset                      XXX
Cr. Fixed Asset                                  XXX
```


**4. Disposal retirement / abandonment**


```text
Dr. Accumulated Depreciation                   XXX
Dr. Loss on Asset Disposal                     XXX
Cr. Fixed Asset                                  XXX
```


The critical mechanic: **on disposal, full cost and full accumulated depreciation both come off balance sheet.** A common SMB error is netting disposal into "cash proceeds minus book value" as a single journal entry, which leaves original cost on books.


## The perpetual FAS what schedule tracks


A defensible fixed asset schedule maintains, per asset:


- Asset ID / tag number
- Description
- Class (buildings, machinery, vehicles, etc.)
- Location
- Vendor
- Acquisition date and placed-in-service date (they can differ)
- Original cost (with capitalized costs itemized)
- Salvage value
- Book useful life (years)
- Book depreciation method
- Book depreciation start date
- Prior book accumulated depreciation
- Current-period book depreciation
- Book NBV
- Tax class life (MACRS)
- §179 election amount
- Bonus depreciation applied
- Prior tax accumulated depreciation
- Current-period tax depreciation
- Tax NBV
- Disposal date (if applicable)
- Disposal proceeds
- Gain / loss on disposal


Most SMB clients keep this on a single Excel tab that has drifted out of sync with general ledger. The CPA firm's job is to bring two back into agreement every month, not once a year at close.


## The roll-forward workpaper


The fixed asset roll-forward is audit-ready reconciliation of movement from beginning to ending balance for each asset class:


Buildings


Machinery


Vehicles


IT Equip.


Total


Cost


Beginning balance


+ Additions


- Disposals (at cost)


Ending balance


Accumulated Depreciation


Beginning balance


+ Current-year depreciation


- Disposals (at accum. dep.)


Ending balance


Net Book Value


The roll-forward reconciles to:


- The FAS detail (sum of individual asset NBVs = ending balance)
- The general ledger (PP&E and accumulated depreciation accounts)
- The prior-year audited balance
- The depreciation expense on income statement


Every audited SMB financial statement needs this workpaper. Every non-audited compilation should have one anyway, because it's what surfaces M-1 book-tax difference for return.


## The physical inventory reconciliation


At least annually, FAS should be reconciled to a physical inventory of assets. This is workpaper item most commonly skipped and most commonly caught in an audit:


- Assets on schedule that are not physically present (write-off candidates)
- Assets physically present that are not on schedule (unrecorded additions or under-threshold items in use)
- Assets that have moved locations (tag update)
- Assets that have changed use (impairment trigger)


For SMB clients with more than fifty assets, a full annual physical inventory should be conducted. For smaller books, a rotating sample by location is defensible.


## How Finlens keeps FAS reconciled to QBO


Finlens keeps general ledger for client reconciled month-by-month so FAS side of balance sheet reflects reality every close, not just once a year.


- **Every capitalizable transaction flagged.** Finlens tags QBO transactions above client's capitalization threshold and routes them to a capitalization queue for CPA review before they hit FAS.
- **Monthly depreciation entries posted.** Once FAS is built (in firm's tool of choice Sage Fixed Assets, BNA, or in-house), Finlens posts monthly book depreciation JE by asset class to QBO so balance sheet stays current.
- **Book-vs-tax parallel.** Finlens carries tax depreciation column alongside book, so M-1 book-tax difference is calculated by ledger, not reconstructed at return time.
- **Disposal handling.** When an asset is sold, retired, or traded, Finlens produces disposal journal entry with correct cost and accumulated depreciation debits, and posts gain or loss to right income statement line.
- **Roll-forward reporting.** Finlens produces cost and accumulated-depreciation roll-forward by asset class every month, tied to trial balance and to underlying FAS detail.


Finlens is not a fixed asset management engine individual asset schedules still live in a specialized tool for larger books. Finlens is operational bridge that keeps QBO tied to FAS so roll-forward workpaper is always current.


## Conclusion


**Fixed asset accounting is not hard but it is unforgiving of neglect.** The gap between a client's spreadsheet-drift FAS and an audit-ready perpetual schedule closes only when ledger side runs continuously alongside schedule, not in a once-a-year sprint.


$2,500 / $5,000


cap threshold


Book + Tax


parallel depreciation


Monthly


roll-forward


## Fixed asset schedule
out of sync?


Finlens flags capitalizable QBO transactions, posts monthly depreciation entries by asset class, and produces the cost-and-accumulated-depreciation roll-forward tied to the trial balance every close.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


see how Finlens flags capitalizable transactions in QBO, posts monthly depreciation entries by asset class, and produces cost and accumulated-depreciation roll-forward reconciled to trial balance every close.


Bring file for client whose FAS hasn't been touched since last year's return and whose books show two years of depreciation booked as a single lump adjustment. That's file this workflow is built for.


## Frequently asked questions


### **What is difference between a fixed asset schedule and a fixed asset register?**


The terms are used interchangeably in practice. Both refer to detailed listing of every capitalized asset, its cost, accumulated depreciation, and net book value. Some firms distinguish "register" as master list of all assets and "schedule" as a roll-forward summary but this is not a GAAP-defined distinction.


### **What is a reasonable capitalization threshold for a small business?**


$2,500 is IRS de minimis safe harbor for taxpayers without an applicable financial statement; $5,000 with one. Most CPA firms align GAAP threshold with tax threshold to avoid parallel tracking. Any threshold is defensible if it is documented in a written policy and applied consistently.


### **Are internally developed software costs capitalized?**


Under ASC 350-40, costs incurred during application development stage are capitalized (coding, testing, installation). Preliminary project stage costs (planning, evaluation) and post-implementation costs (training, maintenance) are expensed.


### **How often should a physical inventory of fixed assets be performed?**


At least annually for audited entities. For non-audited compilations, every two to three years, with a rotating sample in intervening years


### **What is correct treatment of leasehold improvements under ASC 842?**


Leasehold improvements are capitalized separately from lease and depreciated over shorter of their useful life or remaining lease term. Under ASU 2023-01, leasehold improvements on common-control leases are depreciated over their useful life to common-control group, not limited to lease term.


### **Can same useful life be used for book and tax?**


Yes, but only if book useful life is at least as long as MACRS class life for asset. Using MACRS class lives for book purposes is not GAAP-compliant if asset's actual service life is longer. This is a common M-1 finding.


### **What happens if a §179 or bonus-depreciated asset is disposed of before end of its book life?**


The book gain or loss is calculated using book cost and book accumulated depreciation. The tax gain (or recapture) is calculated using reduced tax basis after §179 or bonus, and may generate ordinary income on recapture. The book-tax difference reverses at that point.


The authoritative depreciation reference for tax purposes is[IRS Publication 946 How to Depreciate Property](https://www.irs.gov/publications/p946) . For year-one §179 and bonus depreciation interaction, see Finlens guide to[Section 179 vs. bonus depreciation](https://www.finlens.app/blogs/section-179-vs-bonus-depreciation) .
