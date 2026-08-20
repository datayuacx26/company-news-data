---
schema_version: "1.0.0"
document_id: "65d67ab00dd0454f6c9a5db902e0ff899b363dab75a445f82754fa1706e8689a"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/how-to-calculate-depreciation"
published_at: "2026-08-15T00:00:00+00:00"
first_seen_at: "2026-08-16T08:31:25.828538+00:00"
fetched_at: "2026-08-16T08:31:27.308199+00:00"
content_hash: "sha256:9b93b18224c9e569c22766e267e62442d35a42d3b01fdf1e661b7bafe7353db5"
---

# How to Calculate Depreciation: Methods, Formulas, and Tax Rules

**Depreciation is accounting method for spreading cost of a long-lived asset over its useful life** rather than expensing full purchase in year one. The calculation has three variables that go into every method: **cost, salvage value, and useful life** . Which method you use depends on whether calculation is for financial reporting (GAAP) or tax return (IRS MACRS) and two commonly produce different numbers for same asset.


This guide walks through four GAAP depreciation methods with formulas and examples, then MACRS tax method, and how Section 179 and bonus depreciation change picture in year one.


## The three inputs to every depreciation calculation


- **Cost** total amount paid to acquire asset, including sales tax, freight, installation, and any preparation costs to make asset ready for use
- **Salvage value** estimated amount you'd recover at end of asset's useful life (also called residual value). For tax purposes under MACRS, salvage value is always assumed to be zero
- **Useful life** estimated number of years asset will produce economic benefit. GAAP uses your best estimate; MACRS uses IRS-assigned recovery periods


Once you have these three numbers, calculation depends on method.


## Method 1 Straight-line depreciation


Straight-line is simplest method and most common for financial reporting. It allocates equal depreciation to every year of asset's useful life.


**Formula:**


(Cost − Salvage value) ÷ Useful life = Annual depreciation


**Example:** A business buys a $50,000 delivery truck with an estimated $5,000 salvage value and 5-year useful life.


($50,000 − $5,000) ÷ 5 = **$9,000 per year**


Each year for 5 years, business records $9,000 of depreciation expense on P&L and $9,000 of accumulated depreciation on balance sheet. After year 5, truck's book value equals its salvage value ($5,000).


**When to use:** most tangible assets for GAAP financial reporting. Buildings almost always use straight-line.


## Method 2 Declining balance depreciation


Declining balance is an accelerated method more depreciation in early years, less later. The most common variant is **double-declining balance (DDB)** , which uses twice straight-line rate.


**Formula:**


Book value at start of year × (2 ÷ Useful life) = Depreciation expense


**Example:** Same $50,000 truck, 5-year life, ignore salvage for DDB calculation itself.


- **Year 1:** $50,000 × (2/5) = $20,000
- **Year 2:** ($50,000 − $20,000) × (2/5) = $12,000
- **Year 3:** ($30,000 − $12,000) × (2/5) = $7,200
- **Year 4:** ($18,000 − $7,200) × (2/5) = $4,320
- **Year 5:** Switch to straight-line on remaining book value to fully depreciate to salvage. Depreciation = $6,480 − $5,000 = $1,480


Total depreciation over 5 years: $45,000 matches straight-line total but concentrates it in earlier years.


**When to use:** assets that lose economic value faster in early years (vehicles, technology). GAAP allows either straight-line or DDB, but DDB is more common on tax side of equation via MACRS.


## Method 3 Sum-of-years'-digits (SYD)


SYD is another accelerated method, less commonly used than DDB but occasionally seen.


**Formula:**


Remaining life ÷ Sum of years' digits × (Cost − Salvage) = Depreciation


For a 5-year asset, sum of years' digits is 5 + 4 + 3 + 2 + 1 = **15** .


**Example:** Same $50,000 truck, $5,000 salvage, 5-year life. Depreciable base = $45,000.


- **Year 1:** 5/15 × $45,000 = $15,000
- **Year 2:** 4/15 × $45,000 = $12,000
- **Year 3:** 3/15 × $45,000 = $9,000
- **Year 4:** 2/15 × $45,000 = $6,000
- **Year 5:** 1/15 × $45,000 = $3,000


Total: $45,000, ending at $5,000 book value.


**When to use:** rarely in modern practice. SYD is a legacy method that some industries still specify but has largely been replaced by DDB and MACRS.


## Method 4 Units of production


Units of production depreciates asset based on actual usage rather than time. Useful for manufacturing equipment where output volume varies year over year.


**Formula:**


(Cost − Salvage) ÷ Total expected units × Units this year = Depreciation


**Example:** A $500,000 machine expected to produce 1,000,000 units over its life, with $50,000 salvage.


Depreciation per unit = ($500,000 − $50,000) ÷ 1,000,000 = **$0.45 per unit**


If machine produces 100,000 units in Year 1, depreciation is 100,000 × $0.45 = $45,000.


**When to use:** manufacturing equipment, aircraft (based on flight hours), mining equipment (based on tons extracted).


## Tax depreciation MACRS


Federal tax depreciation runs on **Modified Accelerated Cost Recovery System (MACRS)** , defined in[IRS Publication 946](https://www.irs.gov/publications/p946) . MACRS is required for tax filings you cannot use straight-line for tax return unless specific class allows it as an election.


MACRS assigns each asset to a **property class** with a set recovery period:


Property class


Common assets


Recovery period


3-year property


Some racehorses, over-the-road tractor units


3 years


5-year property


Cars, light trucks, computers, office equipment, R&D equipment


5 years


7-year property


Office furniture, appliances, most manufacturing equipment


7 years


10-year property


Vessels, single-purpose agricultural structures


10 years


15-year property


Qualified improvement property, land improvements


15 years


20-year property


Farm buildings, municipal sewers


20 years


27.5-year property


Residential rental real estate


27.5 years


39-year property


Nonresidential real estate


39 years


MACRS uses **200% declining balance method** for 3-, 5-, 7-, and 10-year property, switching to straight-line when that produces a larger deduction. 15- and 20-year property use 150% DB. Real estate (27.5 and 39-year) uses straight-line only.


**Conventions:** MACRS applies a partial-year convention in year 1 and final year. Most assets use **half-year convention** (6 months of depreciation in year 1 and final year). If more than 40% of assets are placed in service in Q4, **mid-quarter convention** applies. Real estate uses **mid-month convention** depreciation starts mid-month regardless of exact placed-in-service date.


Practical impact: for a 5-year MACRS asset costing $50,000, first-year depreciation using half-year convention is 20% of cost = $10,000. Year 2 is 32%. See MACRS tables in Publication 946 for exact percentages by property class and year.


## Section 179 and bonus depreciation


Two special rules can accelerate first-year depreciation dramatically:


**Section 179** elect to expense up to $1.22 million of qualifying property in year 1 (2024 limit; adjusts annually). Phases out dollar-for-dollar when total Section 179 property exceeds $3.05 million. Applies to tangible personal property, off-the-shelf software, and certain building improvements.


**Bonus depreciation (IRC §168(k))** additional first-year depreciation on qualified property. Phase-out schedule: 100% (through 2022) → 80% (2023) → 60% (2024) → 40% (2025) → **20% (2026)** → 0% (2027) unless Congress extends.


The two can be combined. For a $50,000 5-year asset placed in service in 2026:


1. Section 179 election: $50,000 (if elected reduces to $0 remaining basis)
2. If Section 179 not elected: 20% bonus depreciation on full $50,000 = $10,000, then MACRS on remaining $40,000
3. If neither elected: straight MACRS = $10,000 year-1 depreciation (half-year convention)


Our guide on[Section 179 vs. bonus depreciation](https://www.finlens.app/blogs/section-179-vs-bonus-depreciation) covers when each election is preferable.


## Book vs tax depreciation difference matters


Financial statements (GAAP) and tax returns (MACRS) usually produce different depreciation numbers for same asset. This difference creates a **deferred tax** item on balance sheet timing difference between when depreciation reduces GAAP income versus taxable income.


Practical impact:


- Book depreciation → hits your P&L for management reporting, investor discussions, bank covenants
- Tax depreciation → hits your tax return and affects tax liability


Most small businesses keep two[fixed asset schedules](https://www.finlens.app/blogs/fixed-asset-accounting) one for GAAP, one for MACRS. Modern accounting software with[automated GAAP schedules](https://www.finlens.app/uses/automated-gaap-schedule-software) maintains both automatically once asset is set up with two different useful lives and methods.


## Conclusion


**Three inputs cost, salvage, useful life feed every depreciation calculation.** GAAP financial reporting uses straight-line for most assets and declining balance for some. Tax reporting uses MACRS with recovery periods set by IRS. Section 179 and bonus depreciation compress first-year deduction dramatically when elected.


## FAQ


### What is formula for depreciation?


The straight-line formula is (Cost − Salvage Value) ÷ Useful Life = Annual Depreciation. Other methods use different formulas: double-declining balance uses Book Value × (2 ÷ Useful Life); units of production uses (Cost − Salvage) ÷ Total Units × Units This Year.


### How do I calculate straight-line depreciation?


Subtract salvage value from cost, then divide by useful life. Example: a $20,000 asset with $2,000 salvage and 5-year life depreciates at ($20,000 − $2,000) ÷ 5 = $3,600 per year for 5 years.


### What is MACRS depreciation?


Modified Accelerated Cost Recovery System required tax depreciation method under IRC §168 and IRS Publication 946. MACRS assigns assets to property classes (5-year for cars and computers, 7-year for furniture, 27.5-year for residential rental, 39-year for commercial real estate) with defined recovery periods.


### How is depreciation different for tax vs financial reporting?


GAAP financial reporting uses straight-line or declining balance based on your best estimate of useful life. Tax reporting uses MACRS with IRS-assigned recovery periods, plus Section 179 or bonus depreciation elections. The difference creates deferred tax on balance sheet.


### What is bonus depreciation in 2026?


Under current IRC §168(k) phase-out schedule, bonus depreciation is 20% in 2026 (down from 40% in 2025 and 60% in 2024). Unless Congress extends, bonus depreciation drops to 0% in 2027.


### Can I depreciate a personal asset used for business?


Only business-use percentage. If you use a $30,000 vehicle 60% for business, you can depreciate 60% of cost. This is why documenting business use mileage logs, use records matters for depreciation deductions.


### What assets cannot be depreciated?


Land is never depreciated (does not wear out). Inventory is not depreciated (accounted for through COGS). Assets held less than 12 months are expensed under de minimis safe harbor rather than depreciated. Personal-use assets not used for business.


### Do I still need to depreciate if I use Section 179?


Section 179 fully expenses asset in year 1 (up to limit), eliminating further depreciation. If Section 179 is limited (income limit) or bonus depreciation is used, remaining basis is depreciated under MACRS in subsequent years.
