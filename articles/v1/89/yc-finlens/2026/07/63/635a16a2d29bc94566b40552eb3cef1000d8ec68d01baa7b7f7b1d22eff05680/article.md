---
schema_version: "1.0.0"
document_id: "635a16a2d29bc94566b40552eb3cef1000d8ec68d01baa7b7f7b1d22eff05680"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/qbi-deduction"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T08:12:02.916072+00:00"
fetched_at: "2026-07-30T08:12:04.374298+00:00"
content_hash: "sha256:77ef4fedd5883aebef9f1144c2cf3ced58decad27b2f63764c044fc5de6b990b"
---

# QBI Deduction (Section 199A) in 2026: The CPA-Firm Practitioner Guide with Worked Examples

**The Section 199A Qualified Business Income (QBI) deduction lets eligible non-corporate taxpayers deduct up to 20% of qualified business income from a pass-through entity sole proprietorships (Schedule C), partnerships, S corporations, and certain trusts and estates on individual return.**


It is a **below-the-line deduction** : it reduces taxable income but not AGI, and it is available whether taxpayer itemizes or takes standard deduction. Made permanent under One Big Beautiful Bill Act (OBBBA) in 2025, deduction now sits on stable ground through 2026 tax year with widened phase-in windows and a new $400 material participation floor. This guide walks 2026 thresholds, three-tier framework, W-2 wage / UBIA formula, and worked calculation examples across income tiers with CPA optimization levers named at end.


## **Key takeaways**


- 20% × qualified business income, subject to two ceilings: 20% of modified taxable income, and (above threshold) greater of 50% of W-2 wages OR 25% of W-2 wages + 2.5% of UBIA.
- 2026 thresholds (OBBBA-widened): single $201,750 lower / $276,750 upper; MFJ $403,500 lower / $553,500 upper.
- Below lower threshold: flat 20%, no SSTB penalty, no wage/property limits.
- Above upper threshold: SSTB → $0; QTB → capped by W-2 wage / UBIA formula.
- $400 material-participation floor under §199A(i) protects small active businesses from computed-zero outcomes.
- Everything downstream S-Corp salary modeling, UBIA acceleration, aggregation elections depends on underlying books being right. Finlens automates ledger cleanup that makes QBI calc defensible.


## The 2026 threshold table start here


Filing status


Lower threshold (full 20% below this)


Upper threshold (limits fully in effect above this)


Phase-in window


Single / HoH


$201,750


$276,750


$75,000


MFJ


$403,500


$553,500


$150,000


MFS


$201,750


$276,750


$75,000


Estate / Trust


Same as single


Same as single


$75,000


*2026 baseline figures per Revenue Procedure 2025-32 and OBBBA implementing guidance. Some regional CPA data indicates final localized inflation prints of ~$203,000 / ~$406,000 verify against*[IRS Form 8995-A instructions](https://www.irs.gov/instructions/i8995a) *before filing.*


The three tiers below drive every downstream calculation and strategy question.


*Figure 1. The tier a taxpayer sits in determines entire calculation path. SSTB vs QTB only matters in phase-in and above-threshold tiers.*


## The three-tier framework how deduction changes as income rises


### Tier 1 Below lower threshold (flat 20%)


- Every eligible taxpayer gets **20% × QBI** , capped by 20% × modified taxable income.
- **SSTB status does not matter here.** A doctor, lawyer, accountant, or consultant below $201,750 (single) / $403,500 (MFJ) gets same deduction as any QTB owner.
- W-2 wage and UBIA limits do not apply.
- File on **Form 8995 (Simplified)** no phase-in math required.


### Tier 2 Inside phase-in range


- Single: $201,750 → $276,750. MFJ: $403,500 → $553,500.
- **SSTB deduction begins to taper toward zero** proportionally through window.
- **QTB W-2 wage and UBIA limits begin to phase in** deduction transitions from unlimited to formula-capped over same window.
- Requires **Form 8995-A (Full Calculation)** .


### Tier 3 Above upper threshold


- **SSTB deduction is $0.** No 20%, no partial deduction. Only QBI from non-SSTB streams counts.
- **QTB deduction is hard-capped** by wage/property formula:


- The QBI component cannot exceed greater of:


- **50% of W-2 wages paid by business** , OR
- **25% of W-2 wages + 2.5% of UBIA of qualified property**


- Requires Form 8995-A with full wage/property calculation.


## SSTB vs QTB classification that matters at and above threshold


Every trade or business is one or other. Get this wrong and entire deduction line is wrong.


Classification


Definition


Above upper threshold


QTB Qualified Trade or Business


Any trade or business other than an SSTB and other than performing services as an employee. Manufacturing, retail, wholesale, real estate rental (if trade-or-business), software product companies, construction, restaurants (non-service-heavy).


Deduction available


, capped by W-2 wage / UBIA formula.


SSTB Specified Service Trade or Business


Fields involving health, law, accounting, actuarial science, performing arts, consulting, athletics, financial services, brokerage services, or where principal asset is reputation or skill of one or more employees or owners.


Deduction is $0


above upper threshold. Full deduction below lower threshold. Phased down in range.


**Common classification traps CPAs see:**


- Medical practices are SSTB (health). Medical device manufacturing is not.
- CPA firms and law firms are SSTB. Software products built by CPA-firm-owner or lawyer-owner are typically not, if software is sold product.
- Consulting is SSTB. Selling proprietary software that consultants use is not.
- Investment management is SSTB (financial services). Owning rental real estate directly is typically QTB (if trade-or-business status is met).
- "Reputation or skill" trap: a personal brand endorsement business (celebrity licensing, athlete endorsements) is SSTB.


If an entity mixes SSTB and QTB streams for example, a firm that offers both tax advisory (SSTB) and sells a software product (QTB) **streams can sometimes be separated** if QTB portion operates with independent books, records, and employees. This is de-aggregation strategy covered below.


## The QBI calculation formula mechanical detail


For every eligible business, compute QBI component. Then sum across businesses. The deduction is lesser of that total or 20% of modified taxable income.


**Step 1 Determine QBI per business.** QBI includes:


- Net domestic ordinary income from an active trade or business
- Deductible portions of self-employment tax, self-employed health insurance, and qualified retirement contributions


QBI excludes:


- Capital gains and losses
- Dividends and interest income (unless investment income is part of business's ordinary operations)
- Reasonable compensation paid to S-Corp shareholders
- Guaranteed payments to partners
- W-2 wages received as an employee
- Income earned outside United States


**Step 2 Determine tentative deduction.** 20% × QBI for each business.


**Step 3 Apply threshold-based limits.**


- Below lower threshold: no further limits.
- Phase-in range: apply proportional SSTB haircut (for SSTB) and proportional wage/property limit (for QTB).
- Above upper threshold: SSTB → $0; QTB → hard cap at greater of:


- **50% of W-2 wages** OR
- **25% of W-2 wages + 2.5% of UBIA of qualified property**


**Step 4 Add REIT and PTP components.** Qualified REIT dividends and qualified publicly traded partnership income get a separate 20% deduction computed at line-item level, no SSTB/QTB test.


**Step 5 Apply taxable income ceiling.** Total deduction cannot exceed **20% × (taxable income − net capital gains − qualified dividends)** .


**Step 6 Apply $400 minimum floor.** Under §199A(i), if aggregate QBI from all active §469(h) trades or businesses is at least $1,000, deduction floor is $400 regardless of primary calc.


## Worked examples three tiers, three real calculations


### Example 1 Below threshold, SSTB owner


Sarah is a solo consultant (SSTB) filing MFJ. Consulting QBI: $180,000. Taxable income before QBI deduction: $350,000. She is below $403,500 MFJ lower threshold.


- Tentative deduction: 20% × $180,000 = **$36,000**
- Taxable income ceiling: 20% × ($350,000 − 0 capital gains) = $70,000 (not binding)
- **Deduction: $36,000**


The SSTB status is irrelevant because Sarah is below lower threshold.


### Example 2 Above upper threshold, QTB with wages


Marco owns a QTB manufacturing business as an S-Corp filing MFJ. Business QBI: $500,000. W-2 wages paid by business: $250,000. UBIA of qualified property: $800,000. Taxable income before QBI: $700,000. He is above $553,500 MFJ upper threshold.


- Tentative deduction: 20% × $500,000 = **$100,000**
- Wage/property limit greater of:


- 50% × $250,000 = **$125,000** , OR
- 25% × $250,000 + 2.5% × $800,000 = $62,500 + $20,000 = **$82,500**


- Cap = **$125,000** (greater of two)
- Tentative deduction ($100,000) is under cap → no reduction
- Taxable income ceiling: 20% × $700,000 = $140,000 (not binding)
- **Deduction: $100,000**


Marco's W-2 wages carry deduction. If he had paid himself only $80,000 as reasonable compensation (a common S-Corp salary-minimization pattern), wage/property limit would fall to greater of $40,000 or $20,000 + $20,000 = $40,000 cutting deduction by $60,000.


### Example 3 Above upper threshold, SSTB


Priya is a CPA firm partner (SSTB) filing single. Firm QBI allocable to her K-1: $350,000. Taxable income before QBI: $400,000. She is above $276,750 single upper threshold.


- SSTB above upper threshold = **$0 deduction on CPA firm income**


If Priya's household also had $50,000 of QBI from a rental real estate operation qualifying as a QTB trade-or-business, that stream would generate a separate deduction subject to W-2 wage / UBIA formula on rental SSTB $0 outcome only zeroes out SSTB stream.


*Figure 2. Three real calculations across three tiers. Above upper threshold, wage/UBIA formula is binding constraint for QTBs.*


## OBBBA (One Big Beautiful Bill Act) what changed in 2025 that matters in 2026


- **Made permanent.** Section 199A no longer sunsets. The 2025 expiration removed.
- **Wider phase-in windows.** Single window widened to $75,000 (was $50,000). MFJ window widened to $150,000 (was $100,000). Upper-middle-class owners get gentler benefit ramps.
- **New $400 minimum deduction floor.** Under §199A(i), if a taxpayer's aggregate QBI from all §469(h)-material-participation trades or businesses is at least $1,000, deduction cannot fall below $400. Protects small actively-run businesses from being computed to zero.
- **Retained core framework.** SSTB definition, wage/UBIA formula, aggregation rules, REIT/PTP component all unchanged in substance.


The permanent-law status makes 2026 first year firms can plan multi-year QBI optimization without sunset overhang. Related:[bookkeeping services fees in 2026](https://www.finlens.app/blogs/bookkeeping-services-fees) covers how firms are pricing multi-year advisory work that OBBBA permanence enables.


## CPA optimization strategies four levers firms actually pull


### Lever 1 S-Corp salary calibration


For high-income S-Corp shareholders above upper threshold, 50% W-2 wage baseline is binding constraint on deduction. Firms model exact salary level where marginal tax savings on an additional dollar of QBI deduction just exceeds marginal payroll tax cost on corresponding wage.


Rough shape of math:


- Extra $1 of W-2 wage → $0.20 more QBI deduction (via 50% wage baseline) if wage limit is binding constraint
- $0.20 QBI deduction × 37% marginal rate = $0.074 tax savings
- Extra $1 of wage → $0.153 in employer + employee payroll tax (up to Social Security wage base) plus 2.9% Medicare above
- Below Social Security wage base, extra wage costs more in payroll tax than it saves in income tax but above wage base, only 2.9% Medicare applies, so calc flips


This is why S-Corp salary optimization above SS wage base changes character. Firms build spreadsheets for each client that stress salary across wage-base breakpoint.


### Lever 2 UBIA (capital expenditure) acceleration


For asset-heavy QTBs facing W-2 wage limit, second half of formula **25% of wages + 2.5% of UBIA** becomes driver. UBIA is *unadjusted* basis of qualified property immediately after acquisition, not depreciated basis.


Key point: taking Section 179 or bonus depreciation on qualified property **does not reduce UBIA** . The property remains in UBIA pool at its original cost for depreciable recovery period. So firms can:


- Deduct asset cost immediately via §179 or bonus depreciation
- Simultaneously preserve full original-cost UBIA for 2.5% QBI calculation


In 2026, bonus depreciation phases down to 60%. Combining §179 (which stays at 100% up to cap) with bonus becomes standard playbook.


### Lever 3 De-aggregation of SSTB and QTB streams


If an entity mixes an SSTB service (say, consulting or CPA advisory) with a QTB product (say, licensed software or a proprietary financial tool), SSTB above-threshold zeroing hurts. The de-aggregation strategy: split QTB stream into an independent legal structure with its own books, employees, and revenue attribution.


Requirements to defend de-aggregation:


- Independent chart of accounts and records
- Separate employees (or clearly-allocated employee time)
- Independent revenue streams (customer contracts with QTB entity, not SSTB firm)
- Arm's-length intercompany pricing if two entities transact


This is where underlying bookkeeping actually matters. If books can't defend split, IRS treats combined entity as SSTB.


### Lever 4 Aggregation elections on Form 8995-A


The reverse of de-aggregation. If an owner has multiple QTBs where one has high profit but low W-2 wages and another has high W-2 wages but low profit, wage-poor entity is hitting wage limit while wage-rich entity has unused headroom. Aggregating businesses under IRC §199A(d)(3) and Reg. §1.199A-4 lets excess W-2 wages of one flow through to lift wage limit on other.


Aggregation requires:


- Same person or group owning 50%+ of each entity
- Same tax year
- No SSTB in aggregation group
- Two of three: same products/services, same customers or client base, or operational interdependence


Aggregation is elected on Form 8995-A Schedule B. Once elected, it must be maintained in future years unless facts change materially.


Related:[best practice management software for small accounting firms](https://www.finlens.app/blogs/best-practice-management-software-accounting-firms) covers workflow tools firms use to run these multi-entity engagements.


## Where QBI calculation goes wrong and what fixes it upstream


Every optimization above assumes one thing: underlying QBI number is correct. In practice, that number is only right if books are right. The common failure points:


- **Stripe revenue booked net of fees.** QBI is calculated on gross domestic ordinary income; if P&L shows net-of-fees revenue, QBI is understated and deduction is understated.
- **Deferred revenue schedules missing.** Subscription businesses that don't run proper deferred revenue schedules end up recognizing revenue in wrong year. QBI computed against wrong year's income = wrong deduction.
- **SE health insurance and retirement contributions not deducted correctly.** These reduce QBI on way up. Firms that skip them overstate QBI (and often deduction), triggering audit exposure.
- **Book-to-tax adjustments unreconciled.** Schedule M-1 items that aren't properly tracked create QBI mismatches between financial statement and return.
- **UBIA schedule missing or wrong.** Above-threshold QTBs need a live UBIA schedule showing original cost, acquisition date, remaining depreciable period. Firms that maintain UBIA on fly at return time frequently miss assets.


[Finlens](https://www.finlens.app/accountants) is designed for upstream cleanup that makes QBI calculation defensible. Stripe payout decomposition, transaction categorization with per-client rules, automated deferred revenue schedules, clean journal entries directly to QuickBooks Online. For SaaS and e-commerce clients, this is where QBI number stops being an estimate and becomes an auditable output. Related:[tax resolution CPA firm process and fees](https://www.finlens.app/blogs/tax-resolution) covers how same cleanup layer supports resolution engagements downstream.


## Forms 8995 and 8995-A which one to file


- **Form 8995 (Simplified).** Use if taxable income is below lower threshold. One-page calculation. No SSTB analysis, no wage/UBIA math.
- **Form 8995-A (Full Calculation).** Required in every other case inside phase-in window, above upper threshold, aggregation elections, patron reductions for co-op members, or REIT/PTP dividends when taxpayer is above threshold.


Form 8995-A schedules:


- **Schedule A** SSTB reduction (phase-in tapering)
- **Schedule B** Aggregation of Business Operations
- **Schedule C** Loss Netting and Carryforward
- **Schedule D** Special rules for patrons of agricultural or horticultural cooperatives


The full instructions live at[irs.gov/instructions/i8995a](https://www.irs.gov/instructions/i8995a) and simplified form at[irs.gov/instructions/i8995](https://www.irs.gov/instructions/i8995) .


## Frequently asked questions


### What is QBI deduction?


The Section 199A Qualified Business Income deduction lets eligible non-corporate taxpayers deduct up to 20% of qualified business income from a pass-through entity sole proprietorships, partnerships, S corporations, and certain trusts and estates. It is a below-the-line deduction on individual return, available whether taxpayer itemizes or takes standard deduction.


### What is Qualified Business Income (QBI)?


QBI is net domestic ordinary income from an active trade or business, including deductible portions of self-employment tax, self-employed health insurance, and qualified retirement contributions. It excludes capital gains, dividends, interest income not tied to business, reasonable compensation paid to S-Corp shareholders, guaranteed payments to partners, and W-2 wages received as an employee.


### How do I calculate QBI deduction?


Take 20% of QBI. Below lower threshold, that's your deduction (capped at 20% of modified taxable income). Above upper threshold, an SSTB gets $0 and a QTB is capped at greater of 50% of W-2 wages OR 25% of W-2 wages + 2.5% of UBIA. Inside phase-in window, both limits apply proportionally. Final deduction cannot exceed 20% × (taxable income − net capital gains − qualified dividends).


### What are 2026 income thresholds for QBI deduction?


For 2026 thresholds are $201,750 (lower) and $276,750 (upper) for single filers, and $403,500 (lower) and $553,500 (upper) for MFJ. The phase-in windows are $75,000 (single) and $150,000 (MFJ). These figures are OBBBA-widened baselines; verify against Rev Proc 2025-32 and IRS Form 8995-A instructions before filing.


### What is a Specified Service Trade or Business (SSTB)?


An SSTB is any trade or business in health, law, accounting, actuarial science, performing arts, consulting, athletics, financial services, brokerage services, or any business where principal asset is reputation or skill of one or more employees or owners. Below lower threshold, SSTB status doesn't matter; above upper threshold, SSTB deduction is $0.


### Can accounting and CPA firms claim QBI deduction?


Yes, if individual partner or owner's taxable income is below lower threshold. Accounting is an SSTB, so above upper threshold QBI deduction on CPA firm income is $0. Below threshold, CPA firm partners claim full 20%.


### Does taking Section 179 or bonus depreciation reduce UBIA?


No. UBIA is *unadjusted* basis immediately after acquisition. Section 179 and bonus depreciation reduce depreciable basis for income-tax purposes but do not touch UBIA for QBI purposes. The property remains in UBIA pool at original cost through depreciable recovery period.


### What is $400 minimum deduction floor under OBBBA?


Under §199A(i), if a taxpayer's aggregate QBI from all §469(h)-material-participation trades or businesses is at least $1,000, QBI deduction floor is $400 even if primary calculation would yield less. Protects small active businesses from being computed to zero.


### Should S-Corp shareholders raise their W-2 salary to increase QBI deduction?


It depends. Above upper threshold, higher W-2 wages expand 50% wage baseline that caps QBI deduction. Below Social Security wage base, extra payroll tax typically costs more than QBI benefit adds. Above wage base, only 2.9% Medicare applies, and QBI benefit often wins. This calc has to be modeled per client per year.


### Which form is used to claim QBI deduction?


Form 8995 (Simplified) if taxable income is below lower threshold. Form 8995-A (Full Calculation) in every other case including phase-in range, above upper threshold, aggregation elections, patron reductions, and above-threshold REIT/PTP dividends.


### Was QBI deduction extended past 2025?


Yes. The One Big Beautiful Bill Act (OBBBA), signed in 2025, made Section 199A permanent, widened phase-in windows, and added $400 minimum deduction floor under §199A(i). The core 20% × QBI framework is unchanged.


### Does Finlens help with QBI compliance?


Finlens doesn't file returns or run Form 8995-A calculation itself that's CPA's job. What Finlens does is upstream: automate categorization, decompose Stripe payouts, run deferred revenue schedules, and write clean entries to QuickBooks Online. That's what makes QBI input number defensible. For SaaS and e-commerce clients where Stripe revenue is booked incorrectly, QBI deduction on return is wrong before CPA even opens Form 8995-A.


## Conclusion


**Pick one client with material Stripe or subscription revenue who is filing above $403,500 MFJ QBI threshold this year bring three months of their QBO and we'll walk through QBI input number live before return goes to tax preparer.**


[Book a 20-minute walkth](https://cal.com/finlens/intro) with Finlens team.


‍


*Tax thresholds, phase-in windows, and OBBBA implementation details reflect guidance publicly available as of 2026-07-29 including Revenue Procedure 2025-32 and IRS instructions for Form 8995 and Form 8995-A. Some regional CPA sources indicate slightly different final localized inflation prints (e.g., ~$203,000 / ~$406,000) verify against current IRS Form 8995-A instructions at*[irs.gov/instructions/i8995a](https://www.irs.gov/instructions/i8995a) *before filing. Nothing in this article is legal or tax advice engage a licensed CPA, EA, or attorney for actual tax planning and return preparation. Third-party trademarks referenced (QuickBooks®, Stripe®) belong to their respective owners.*


‍
