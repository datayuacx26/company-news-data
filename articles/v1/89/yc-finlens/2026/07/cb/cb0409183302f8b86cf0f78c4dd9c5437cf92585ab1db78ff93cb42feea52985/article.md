---
schema_version: "1.0.0"
document_id: "cb0409183302f8b86cf0f78c4dd9c5437cf92585ab1db78ff93cb42feea52985"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/section-179-vs-bonus-depreciation"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T08:12:02.916072+00:00"
fetched_at: "2026-07-30T08:12:04.374298+00:00"
content_hash: "sha256:1667aba8baed5c5f172f92c6b42cdcd112356bd7402d68d3e2664105fe7ef177"
---

# Section 179 vs Bonus Depreciation (MACRS)

**Section 179 and bonus depreciation are two separate tax provisions inside same MACRS depreciation regime both let a business immediately expense cost of qualifying property in year placed in service, but they work differently on limits, net income constraints, asset-class rules, and interaction with net operating losses.**


For 2026, Section 179 deduction limit is $2,560,000 with a $4,000,000 phase-out threshold, and bonus depreciation returned to 100% permanently under One Big Beautiful Bill Act (OBBBA) for property acquired and placed in service after January 19, 2025. Section 179 vs bonus depreciation is not an either-or choice for most CPA firms; both are applied together on Form 4562, with §179 running first by IRS rule and bonus depreciation absorbing any remaining basis. This guide walks 2026 rules, MACRS interaction, OBBBA reset, per-asset planning trade-offs, and workflow firms actually run.


## **Key takeaways**


- §179 2026 limit: $2,560,000; $4,000,000 phase-out threshold; cannot create net loss (§179 income limitation)
- Bonus depreciation 2026: 100% permanent under OBBBA for property acquired + placed in service after 1/19/2025; can create NOL
- Both provisions run inside MACRS depreciation; §179 applied first, then bonus depreciation on remainder, then regular MACRS on any residual basis
- §179 is picky (asset-by-asset); bonus depreciation is class-wide (all 5-year property or none)
- The choice depends on income posture, QBI planning, state add-back rules, and NOL carryforward strategy modeled per client per year
- [Finlens](https://www.finlens.app/accountants) automates fixed-asset and ledger cleanup that feeds Form 4562 original cost, in-service dates, asset class tags so depreciation election runs off clean data


## The 2026 baseline table Section 179 vs bonus depreciation


Dimension


Section 179 (2026)


Bonus Depreciation (2026)


Deduction limit


$2,560,000


No overall dollar cap


Phase-out threshold


Begins at $4,000,000 in qualifying purchases


None


Net income limitation


Cannot exceed business net taxable income (no NOL)


Can create or increase NOL


Asset selection


Pick individual items (surgical)


All-or-nothing per MACRS asset class


Order of application


Applied first (IRS rule)


Applied after §179 on remaining basis


Qualifying property


Tangible personal property, off-the-shelf software, some qualified improvement property (QIP)


Same, plus certain used property post-TCJA


Recovery period requirement


≤ 20-year MACRS class


≤ 20-year MACRS class


Statute


IRC §179


IRC §168(k)


Form


Form 4562, Part I


Form 4562, Part II


*Baseline per IRS Rev. Proc. 2024-40 (§179 limits) and OBBBA (P.L. 119-21) as of 2026-07-29. Verify current thresholds at*[irs.gov](https://www.irs.gov/instructions/i4562) *.*


The tension between two is not "which is better" it's "which combination makes most sense for this client this year." Every mature depreciation workpaper models both.


*Figure 1. The two provisions live side by side inside MACRS. §179 runs first, bonus depreciation runs second on remainder.*


## How MACRS depreciation ties them together


The Modified Accelerated Cost Recovery System (MACRS) is standard depreciation regime for most business property placed in service after 1986. It assigns each asset to a recovery period based on class life:


- **3-year property** certain race horses, qualified rent-to-own property
- **5-year property** computers, office equipment, cars, trucks, vans, R&D equipment
- **7-year property** office furniture, fixtures, most machinery
- **10-year property** single-purpose agricultural/horticultural structures
- **15-year property** qualified improvement property (QIP), certain restaurant/retail improvements
- **20-year property** farm buildings, other longer-lived improvements
- **27.5-year property** residential rental real property (not §179/bonus eligible)
- **39-year property** nonresidential real property (not §179/bonus eligible)


MACRS depreciation uses declining-balance methods (200% for 3–10 year property, 150% for 15–20 year, straight-line for real property). Section 179 and bonus depreciation both operate on top of MACRS: they front-load recovery of qualifying property. The IRS ordering rule is:


1. Apply **Section 179 expensing** to selected assets first (up to $2.56M cap and net-income limit)
2. Apply **bonus depreciation** to remaining basis of qualifying assets in each MACRS class
3. Apply **regular MACRS depreciation** to any residual basis using class-appropriate declining-balance method and half-year (or mid-quarter) convention


Firms that skip step 1 and jump straight to bonus lose QBI and taxable-income control that §179 provides. Firms that skip bonus lose ability to generate NOL for carryforward planning.


Related:[QBI deduction (Section 199A) 2026](https://www.finlens.app/blogs/qbi-deduction) covers how depreciation election flows into QBI calculation.


## OBBBA and 100% bonus depreciation reset


Bonus depreciation was scheduled to phase down under TCJA 100% in 2017–2022, then 80% in 2023, 60% in 2024, and eventually 0% by 2027. The bonus depreciation phase out was one of largest scheduled tax increases in recent memory for capital-intensive businesses.


**OBBBA (P.L. 119-21, signed 2025) reset that trajectory.** For qualifying property **acquired AND placed in service after January 19, 2025** , bonus depreciation returned to **100% permanently** . Property acquired *before* January 19, 2025 stays on pre-OBBBA phase-down schedule.


The acquired-vs-placed-in-service distinction is where filings get audited:


- **"Acquired"** = taxpayer entered into a binding written contract for property (or, for self-constructed property, when physical work of a significant nature began)
- **"Placed in service"** = property is ready and available for its intended use


An asset acquired in December 2024 and placed in service in March 2025 is on pre-OBBBA schedule (60% bonus). An asset acquired in April 2025 and placed in service in October 2025 gets 100% bonus. The contract date drives treatment not invoice date, not payment date, not delivery date.


For CPA firms handling capital-heavy clients (construction, manufacturing, real estate, e-commerce with warehouse buildouts), OBBBA bonus depreciation acquisition rule is worth its own worksheet on every 2025 and 2026 return.


## The §179 income limitation one rule that trips up most filings


Section 179 has a hard rule most bonus-depreciation-focused firms miss: **§179 cannot exceed business's net taxable income.**


- If a partnership has $500,000 of net ordinary income before §179 and elects $700,000 of §179 expense, §179 deduction is limited to $500,000 in current year
- The remaining $200,000 of §179 election carries forward and can be claimed in a future year when taxable income is high enough
- The income limitation runs at both entity level AND owner level (owner must have sufficient active business income to absorb their share)


This is why §179 is often described as "control" it lets firm dial deduction to a specific taxable income target. If firm wants to preserve $100,000 of taxable income for QBI purposes or a refundable credit, it elects §179 up to point that leaves $100,000 on table, then lets bonus depreciation handle remainder.


**Bonus depreciation has no such rule.** A business with $500,000 of net income and $2,000,000 of qualifying purchases can elect 100% bonus and generate a $1,500,000 NOL, carried forward under post-TCJA 80%-of-taxable-income limitation.


The choice between two is often driven by NOL strategy, not depreciation percentages.


*Figure 2. Section 179 preserves taxable income control. Bonus depreciation maximizes deduction and can generate NOL. Two levers, per client, per year.*


## When to prefer Section 179 over bonus depreciation


- **Client wants a specific taxable income target.** §179 is surgical pick assets that hit exactly deduction client needs. Bonus is all-or-nothing per class.
- **Preserving QBI deduction.** §199A QBI is calculated on net income after depreciation. §179 lets firm stop depreciation at level that keeps QBI at its optimal point (below threshold, or above wage/UBIA limit).
- **State conformity issues.** Many states (California, New Jersey, others) do not conform to federal bonus depreciation and require add-back. §179 has better state conformity. If state tax is material, §179 avoids state add-back workpaper.
- **Refundable credit planning.** If client is claiming an R&D credit or other refundable credit, preserving taxable income to absorb credit matters. §179 gives that control.
- **Mixed asset classes.** Client bought a $50K vehicle (5-year property) and $200K worth of restaurant leasehold improvements (15-year QIP). Firm can §179 vehicle and skip §179 on QIP, letting bonus handle improvements impossible with bonus alone because bonus is class-wide.


## When to prefer bonus depreciation over Section 179


- **Client is above §179 phase-out.** Once qualifying purchases exceed $4M, §179 phases out dollar-for-dollar. At $6.56M of purchases, §179 is fully phased out. Bonus has no phase-out.
- **NOL carryforward is strategically valuable.** A high-income year followed by expected lean years is classic case for generating an NOL now to shelter future income. §179 can't create an NOL; bonus can.
- **Class-wide election is fine.** If client bought only one class of assets (say, all 5-year manufacturing equipment), "all-or-nothing" limitation of bonus depreciation doesn't cost anything.
- **State conformity is not an issue.** Some states fully conform to federal bonus depreciation; check client's state and treat accordingly.
- **Speed matters more than precision.** Bonus is set-and-forget for whole asset class. §179 requires per-asset selection and worksheet documentation.


## Vehicle depreciation where §179 and bonus interact with §280F


Vehicles are highest-frequency §179 / bonus depreciation planning question because §280F caps luxury auto depreciation regardless of §179 or bonus


- **Passenger autos under 6,000 lbs GVW:** subject to §280F luxury caps ($20,400 in year 1 for 2026, including bonus). Both §179 and bonus depreciation are capped by §280F on these vehicles.
- **Passenger autos over 6,000 lbs GVW (SUVs, pickups):** §179 is capped at $31,300 for 2026, but bonus depreciation is NOT §280F-capped on these vehicles 100% bonus can apply to full basis.
- **Heavy vehicles over 14,000 lbs GVW (larger trucks, box vans):** no §280F cap; full §179 or bonus applies.


The 6,000-lb-GVW threshold is why heavy-SUV planning works vehicle escapes §280F entirely, letting bonus depreciation take full purchase price in year 1. Multi-state clients with mixed vehicle fleets need schedule mapped GVW-by-GVW.


## The 2026 Section 179 limit and phase-out mechanics


Section 179 has two key numbers for 2026:


- **Deduction limit:** $2,560,000 maximum §179 expense a taxpayer can elect
- **Phase-out threshold:** $4,000,000 total qualifying purchases above this dollar-for-dollar reduce §179 limit


**Phase-out math:** For each dollar of qualifying purchases above $4M, §179 limit drops by $1. At $6,560,000 in purchases, §179 is fully phased out.


Example S-corp buys $5M of equipment in 2026:


- Qualifying purchases: $5,000,000
- Excess over threshold: $5,000,000 − $4,000,000 = $1,000,000
- §179 limit reduced from $2,560,000 by $1,000,000 = **$1,560,000** available §179
- The remaining $3,440,000 of basis can absorb bonus depreciation (100% if post-1/19/2025)


The takeaway: §179 is designed for small-to-mid-sized businesses. Above ~$6.5M in annual capital purchases, §179 becomes irrelevant and bonus depreciation is entire game.


## The workflow a CPA firm actually runs on Form 4562


For each client's 2026 depreciation calculation:


**Step 1 Build placed-in-service schedule.** Every asset placed in service during 2026 with:


- Cost (original basis)
- Placed-in-service date
- Acquired date (for OBBBA bonus depreciation cutoff)
- MACRS class (3, 5, 7, 10, 15, 20, 27.5, 39)
- §179-eligible flag
- Bonus-depreciation-eligible flag
- GVW if a vehicle (for §280F)


**Step 2 Model §179 vs bonus depreciation on total pool.** Run calc under three scenarios:


- Maximum §179 (up to limit), no bonus
- No §179, maximum bonus
- Mixed (§179 on selected assets to hit a taxable income target, bonus on rest)


**Step 3 Overlay constraints.** Apply §179 net income limitation, §179 phase-out, §280F auto caps, state conformity add-backs. Confirm QBI and R&D credit interactions.


**Step 4 File Form 4562.**


- Part I: §179 election
- Part II: bonus depreciation
- Part III: MACRS regular depreciation
- Part V: §280F listed property (vehicles)


**Step 5 Update depreciation carryforward schedule.** Any §179 disallowed under income limitation carries forward. Track it.


**Step 6 Multi-year tax planning tie-back.** Depreciation choices this year set up QBI, NOL, and state-tax profile for next 3–5 years. Every §179 vs bonus depreciation election is a multi-year decision, not a single-return one.


## Where depreciation calculation goes wrong upstream


The Form 4562 workpaper is only as clean as underlying books. The failure points:


- **Fixed asset purchases blended with ordinary expenses** a $8,000 computer booked as "office supplies" instead of a capitalized asset never makes it onto depreciation schedule
- **Placed-in-service date not tracked** asset showed on P&L in March but wasn't operational until August; depreciation start date is August, not March
- **Acquired date not documented** post-OBBBA acquired-vs-placed-in-service distinction is where 100% bonus vs 60% bonus is decided
- **Vehicle GVW not tagged** §280F auto caps depend on GVW; a heavy-SUV election worth $80K+ in deductions gets missed if GVW is not on asset record
- **Stripe-processed equipment vendor payments net of processing fees** understates capitalized basis; bonus depreciation applies to gross cost, not net-of-Stripe-fee cost


Finlens automates ledger cleanup that feeds these numbers: transaction categorization with per-client rules to capture fixed asset purchases correctly, Stripe payout decomposition so equipment vendor payments hit at gross, deferred revenue schedules to ensure income timing feeds QBI planning cleanly. For capital-heavy clients where §179 vs bonus depreciation is a material return decision, that upstream cleanup is what makes Form 4562 workpaper defensible. Related:[Section 174 R&D capitalization](https://www.finlens.app/blogs/section-174-r-and-d-capitalization) covers same upstream-cleanup principle for R&E workpapers.


## Conclusion


**Pick one client where 2026 has meaningful capital purchases and §179 vs bonus depreciation election is still open bring fixed asset list, three months of QBO, and projected P&L, and we'll walk depreciation input schedule live before Form 4562 is drafted.**


$2.56M


§179 limit


100%


bonus post-1/19/2025


Form 4562


election filing


## §179 or bonus depreciation?
The schedule lives in your ledger.


Finlens categorizes fixed asset purchases with placed-in-service dates and MACRS class tags so Form 4562 runs off a defensible input pool.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


## Frequently asked questions


### What is difference between Section 179 and bonus depreciation?


Section 179 is an election under IRC §179 that lets a business expense cost of qualifying property up to $2,560,000 in 2026 (with a $4M phase-out threshold), and cannot exceed net taxable income. Bonus depreciation under IRC §168(k) has no dollar cap, can create a net operating loss, and applies class-wide (all 5-year property or none). Both live inside MACRS; §179 is applied first, then bonus depreciation on remaining basis.


### Which is better, Section 179 or bonus depreciation?


Neither is universally better. Section 179 gives surgical control over which assets to expense and preserves net income for QBI, refundable credits, or state add-back planning. Bonus depreciation maximizes deduction, can create an NOL for carryforward, and has no phase-out. The choice is per-client, per-year, driven by taxable income posture, QBI strategy, state conformity, and NOL planning.


### Can I use Section 179 and bonus depreciation on same asset?


Not on same portion of basis. But firms routinely use both on same return: §179 covers selected assets first, then bonus depreciation absorbs remaining basis of qualifying assets. IRS ordering rules require §179 to be applied first, then bonus, then regular MACRS. On any specific asset, once §179 has been elected for a portion, only remaining basis is available for bonus depreciation.


### What is Section 179 deduction limit for 2026?


$2,560,000 for 2026 per Rev. Proc. 2024-40. The phase-out threshold begins at $4,000,000 in total qualifying purchases; each dollar above $4M reduces §179 limit by $1. Section 179 is fully phased out at $6,560,000 in qualifying purchases.


### Was bonus depreciation restored to 100% under OBBBA?


Yes, for qualifying property acquired AND placed in service after January 19, 2025. The One Big Beautiful Bill Act (P.L. 119-21) restored 100% bonus depreciation permanently for such property. Property acquired on or before January 19, 2025 remains on pre-OBBBA phase-down schedule (60% in 2024, would have been 40% in 2025, 20% in 2026).


### What is MACRS depreciation?


The Modified Accelerated Cost Recovery System standard depreciation regime under IRC §168 for most business property placed in service after 1986. MACRS assigns each asset to a recovery period (3, 5, 7, 10, 15, 20, 27.5, or 39 years) and applies a declining-balance method (200% for 3–10 year, 150% for 15–20 year, straight-line for real property). §179 and bonus depreciation both operate on top of MACRS to accelerate recovery of qualifying property.


### What was bonus depreciation phase out schedule?


Under TCJA: 100% in 2017–2022, 80% in 2023, 60% in 2024, 40% in 2025, 20% in 2026, and 0% in 2027 for property acquired after Sept 27, 2017. OBBBA reset this for property acquired after 1/19/2025 back to 100% permanent. Property acquired before 1/19/2025 is still on TCJA phase-down.


### Can bonus depreciation create a net operating loss?


Yes. Bonus depreciation is one of primary tools for generating an NOL business's tax deduction can exceed net income, creating an NOL that carries forward (subject to post-TCJA 80%-of-taxable-income limitation). Section 179, in contrast, cannot exceed net taxable income and cannot create an NOL.


### Do heavy SUVs get 100% bonus depreciation in 2026?


For SUVs with GVW over 6,000 lbs acquired and placed in service after 1/19/2025, bonus depreciation is 100% under OBBBA and is NOT subject to §280F luxury auto caps. Section 179 on same heavy SUV is capped at $31,300 in 2026 per §280F. Passenger autos under 6,000 lbs GVW are subject to §280F caps on both §179 and bonus depreciation.


### Does state tax conformity apply to bonus depreciation?


Not uniformly. States that fully conform to federal bonus depreciation: many, but not all. Non-conforming states (California, New Jersey, and others) require an add-back on state return taxpayer gets federal deduction but state depreciation follows a slower schedule. §179 has broader state conformity than bonus depreciation. Model state add-back before finalizing federal election.


### What qualifies for bonus depreciation?


Tangible personal property with a MACRS recovery period of 20 years or less; certain qualified improvement property (QIP); computer software; certain used property post-TCJA. Land, buildings (27.5 and 39 year real property), and non-MACRS property do not qualify. Verify current qualifying property definitions in IRC §168(k) and Treas. Reg. §1.168(k)-2 before final election.


### Does Finlens compute depreciation directly?


No. Finlens automates upstream ledger cleanup that feeds Form 4562 categorizing fixed asset purchases correctly, tracking placed-in-service dates via transaction posting date and human-in-the-loop review, decomposing Stripe payments to capture gross cost of equipment vendors. The tax software (Drake, Lacerte, ProSeries, UltraTax) runs §179 election, bonus depreciation, and MACRS math off that clean data. Finlens sits below tax return, ensuring depreciation input schedule is right before return preparer opens Form 4562.


‍


*Section 179 limits, bonus depreciation percentages, OBBBA acquired-date cutoffs, MACRS classes, and §280F luxury auto caps change frequently. This article reflects guidance current as of 2026-07-29 (P.L. 119-21 and Rev. Proc. 2024-40). Verify current thresholds and IRS Form 4562 instructions at*[irs.gov/instructions/i4562](https://www.irs.gov/instructions/i4562) *before filing. Nothing in this article is legal or tax advice engage a licensed CPA, EA, or attorney for actual depreciation planning and return preparation. Third-party trademarks (QuickBooks®, Stripe®) belong to their respective owners.*


‍
