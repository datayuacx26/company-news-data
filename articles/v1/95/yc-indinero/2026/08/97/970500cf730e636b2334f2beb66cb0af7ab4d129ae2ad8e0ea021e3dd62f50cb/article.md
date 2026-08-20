---
schema_version: "1.0.0"
document_id: "970500cf730e636b2334f2beb66cb0af7ab4d129ae2ad8e0ea021e3dd62f50cb"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/multi-entity-accounting-dental-dso/"
published_at: "2026-08-12T15:18:41+00:00"
first_seen_at: "2026-08-12T16:22:28.267940+00:00"
fetched_at: "2026-08-12T16:22:29.426031+00:00"
content_hash: "sha256:595afb568f5afaa65fc7229c5582796b43efe1bb9c6fe7524ed6b149954b6e32"
---

# Multi-Entity Accounting for Dental Groups and DSOs

## What Makes DSO Accounting Different


Dental DSO accounting differs from solo-practice bookkeeping because it runs across multiple legal entities that close separately, then consolidate. That structural split, not a single ledger, is the whole difference.


A single-owner practice keeps one set of books inside one legal entity. A dental support organization does not. It’s a group of entities, each with its own trial balance and its own close, combined into one financial statement package. To be clear, DSO here means dental support organization, the company that manages a group’s non-clinical operations. It never means a collections metric.


The model is spreading fast, which is why the discipline matters. The share of U.S. dentists affiliated with a DSO climbed from 8.8% in 2017 to 13% in 2022, and for dentists less than ten years out of school that figure reached 23%, according to the[American Dental Association’s Health Policy Institute](https://www.ada.org/resources/research/health-policy-institute/dental-practice-research/how-big-are-dental-service-organizations) . The[Association of Dental Support Organizations](https://www.theadso.org/) reports its members alone support more than 15,000 dentists across roughly 8,500 practices.


Here’s what actually changes on the books versus a solo practice:


- **Two or more entities, not one.** Clinical revenue and provider pay sit in the professional corporation. Rent, staff, equipment, marketing, and back office sit in the management company. Each has its own close.
- **Intercompany transactions become routine.** The management fee, shared-service allocations, cash sweeps, and inter-entity loans all get booked on both sides and reconciled every period.
- **Eliminations are required.** Under ASC 810-10-45-1, intra-entity balances and transactions are eliminated in full on the consolidated statements, so the management fee nets to zero.
- **Consolidation can be a control question, not an ownership question.** Because the management company usually can’t own the practice’s equity, consolidation often turns on the variable interest entity model rather than a majority-vote test.
- **Reporting is expected at three levels.** Owners, lenders, and operators want location-level, entity-level, and consolidated views from one chart of accounts.


The difference isn’t more transactions. It’s a different reporting architecture, and getting the entity map wrong early is expensive to unwind later.


## PC and MSO Structures Explained


The PC and MSO split exists because of law, not accounting preference. The corporate practice of dentistry doctrine, in force across a majority of states, bars a general business corporation or a non-dentist-owned LLC from practicing dentistry, employing the treating dentists, or collecting the professional fees patients pay for care. The stated purpose is to keep clinical judgment with licensed professionals rather than lay owners, and it’s documented in the[U.S. House Committee on Oversight survey of state corporate-practice laws](https://oversight.house.gov/wp-content/uploads/2012/04/4-25-12-Survey-of-State-Laws-Governing-the-Corporate-Practice-of-Dentistry.pdf) .


That answers a common question. Can a non-dentist own a dental practice? In most states, not the clinical entity. A non-dentist investor or operator can own the business that supports the practice, but a licensed dentist must own the entity that delivers care.


To operate at scale within that rule, groups use a two-entity model. This is the mso pc accounting structure that defines dental group accounting.


**The professional corporation (PC), sometimes a professional LLC.**
– Owned by a licensed dentist or dentists, as state law requires.
– Holds the clinical license and the provider agreements.
– Earns and books the patient and payer clinical revenue.
– Pays provider compensation and clinical supply costs.
– Signs the long-term management services agreement.


**The management services organization (MSO, usually branded as the DSO).**
– Can be owned by non-dentists, including private equity sponsors.
– Owns or leases the real estate, equipment, and non-clinical assets.
– Employs the front desk, billing, HR, marketing, and IT staff.
– Provides administrative services to the PC under the management services agreement.
– Charges the PC a management fee for those services.


So what’s the difference between a DSO and an MSO? In dentistry the terms get used interchangeably, but structurally the MSO is the non-clinical services and asset company, and DSO is the market label for that support organization and the group it manages. The DSO doesn’t own the clinical practice. It contracts with it.


The management services agreement is the legal spine of the whole structure. It defines what the MSO provides, from revenue cycle and payroll to marketing and facilities, and it sets the fee. For the accounting team, that agreement is also the source document governing how every intercompany charge gets booked and later defended.


## Location-Level Profit and Loss Reporting


In a DSO, the location is the unit of economics, and the chart of accounts has to prove it. A group that can only produce a blended, all-locations statement can’t tell which office funds the platform and which one drags it.


The mechanism is dimensional accounting. Instead of a separate general ledger for every office, mature groups tag each transaction with a location dimension, and often a provider and payer dimension, inside one system, then report by dimension. That lets a single chart of accounts produce a per-location income statement, a per-provider production report, and a consolidated group statement without re-keying anything. This is where multi location dental practice accounting either scales cleanly or breaks.


The metrics that belong on a per-location P&L or scorecard:


- **Production and net production** , gross versus after adjustments and write-offs.
- **Collections rate** , collections divided by net production. Weakness here is usually a revenue-cycle problem inside the MSO, not a clinical one.
- **Payer mix** , the split across fee-for-service, PPO, and Medicaid, since mix drives realized revenue per procedure.
- **Doctor production versus hygiene production.** Hygiene is the recurring-revenue engine, commonly benchmarked around 3.0 to 3.5 times hygienist wage cost.
- **Chair utilization and production per chair-hour** , with practices often targeting 85% to 90% utilization.
- **Labor ratios** , provider compensation as a percent of production and staff cost as a percent of collections.
- **Adjusted EBITDA by location.** After paying every dentist a fair-market clinical wage, solo owner-operator offices often run 15% to 18%, while multi-provider offices run 18% to 25% or higher.


That last metric is the currency of the whole model. In a DSO roll-up, adjusted EBITDA by location is what a practice gets bought and sold on, which is why the add-back schedule for owner-comp normalization and one-time items has to be clean and consistent across every office. Location-level P&L discipline isn’t internal management color. It’s the exact data the next transaction will be priced on.


## Intercompany Management Fees in DSO Structures


The management fee is the single most scrutinized number in DSO accounting. It’s the payment the PC makes to the MSO for administrative services, and it has to satisfy three audiences at once. The accountant who books and eliminates it. The tax authority that could recharacterize it. The state regulator watching for illegal fee-splitting.


**How the fee is booked.** The fee is ordinary revenue to the MSO and an operating expense to the PC. Both sides record it every period. On consolidation it’s eliminated in full under[ASC 810-10-45-1](https://www.fasb.org/) , so it never inflates combined revenue. Book it asymmetrically or skip the monthly reconciliation, and the intercompany accounts drift until the close breaks.


**How the fee should be set.** Regulators and valuation professionals expect a fee that reflects fair market value, meaning the price two unrelated parties would agree to at arm’s length, and that’s commercially reasonable for what the MSO actually delivers. Common methods include a fixed fee, a cost-plus fee, or a percentage of net revenue. Cost-plus is widely viewed as the most defensible, because it ties directly to the MSO’s documented cost of services. Percentage-of-revenue fees draw more scrutiny in states that treat revenue-linked fees as possible fee-splitting.


**How the fee should be documented.** The management services agreement should state a pricing method that can be validated against the financial records and tested for fair market value. Practitioners often build a fair-market-value defense file that lists each service the MSO provides and its corresponding market rate. If the agreement is silent on pricing, that gap itself creates exposure on a state tax examination.


Documentation protects both sides. In corporate-practice states, a fee that looks like a share of the dentist’s professional income rather than payment for defined services can be attacked as unlawful fee-splitting. On the tax side, a fee set too high or too low between related parties invites recharacterization, which is why the fee and its treatment belong inside coordinated[business tax services](https://www.indinero.com/services/business-tax-services/) rather than handled in isolation. Get the fee and its file right, and the structure holds.


## Consolidated Reporting for DSO Groups


Once a group runs more than one entity, its lenders and investors want one combined picture, and producing it correctly is the technical heart of dso financial reporting. The complication is that the MSO usually can’t own the PC’s stock, so the group can’t lean on the ordinary majority-vote consolidation test. It has to work through the variable interest entity model in ASC 810.


**Why the VIE model applies.** A legal entity falls into the VIE model when it has any of the characteristics in ASC 810-10-15-14, including equity at risk that can’t finance the entity without additional subordinated support, or equity holders who lack the power to direct the activities that most significantly affect the entity’s economic performance, or who lack the obligation to absorb losses and the right to receive residual returns. In a DSO, the PC’s nominal owner is often a single dentist whose control is heavily constrained by the management services agreement, restrictive transfer and succession terms, and the MSO’s control of the assets and cash. Those constraints frequently push the PC into VIE territory.


**Who consolidates.** A management services agreement can itself be a variable interest, because a service or management contract is one of the arrangements ASC 810-10-55 identifies. The party that consolidates a VIE is its primary beneficiary. Under ASC 810-10-25-38A, a reporting entity is the primary beneficiary when it has both the power to direct the activities that most significantly affect the VIE’s economic performance and the obligation to absorb losses or the right to receive benefits that could be significant to the VIE. An MSO that directs the PC’s non-clinical operations and captures its economics through the management fee typically meets both, so the MSO consolidates the PC. Where entities are under common control, the related-party guidance in ASC 810-10-25 has to be worked through before concluding, and the[primary-beneficiary analysis published by PwC](https://viewpoint.pwc.com/dt/us/en/pwc/accounting_guides/consolidation_and_eq/consolidation_and_eq_US/chapter_5_identifying_the/5_1_identifying_the.html) is a useful reference on the power-plus-economics test.


**What the package contains.** A credible consolidation includes each entity’s standalone trial balance, a documented elimination of the management fee and any inter-entity loans under ASC 810-10-45-1, and combined statements that present the group as one economic unit while preserving location-level detail. Practices acquired along the way are folded in under ASC 805. The acquirer applies the acquisition method under ASC 805-10, recognizes identifiable assets and liabilities at fair value under ASC 805-20, and books the residual of consideration over net identifiable assets as goodwill under ASC 805-30-30-1. Dental deals generate large goodwill balances, so many groups that qualify as private companies elect the ASC 350-20 alternative to amortize goodwill on a straight-line basis over ten years or less. Running that combined close cleanly each month is what a disciplined[year-end book close](https://www.indinero.com/blog/business-bookkeeping-closing-your-companys-books/) makes routine.


**Why lenders and sponsors demand it.** Private equity is a major force in the sector, and roll-up economics run on trust in the numbers. Add-on practices commonly transact at mid-single-digit EBITDA multiples while the assembled platform is valued in the low-to-mid double digits, so the consolidated statements, the covenant math, and the quality-of-earnings support are what protect that spread. Credit facilities are underwritten and tested against the consolidated group, not any single office, which is exactly why sponsors expect[GAAP-based investor reporting](https://www.indinero.com/blog/gaap-what-it-is-why-your-investors-expect-it/) . When the consolidation is late or the eliminations are sloppy, the covenant math is unreliable and the next raise gets harder.


## How Indinero Supports Dental Groups and DSOs


Growing dental groups hit a wall, usually somewhere between the third and fifth location, where the setup that worked for two offices stops holding. Cash from every office pools in one MSO account. The management fee is a plug instead of a documented number. There’s no clean location-level P&L, and the lender wants consolidated statements next quarter. That’s the point where in-house bookkeeping alone stops being enough.


Indinero’s accounting team is CPA-led and GAAP-first, and it handles PC and MSO entity books, location-level reporting, and consolidated financials for multi-location groups. What matters here is that the same team also owns the tax position and the CFO-level view. The entity structure, the ASC 810 consolidation treatment, and the management-fee tax position all have to stay consistent, and they don’t when accounting, tax, and CFO work are split across three vendors who never talk. Keep them in one place, and the fee that gets booked and eliminated is the same fee defended on a state exam and the same fee the CFO models into the next acquisition.


Indinero also serves multi-location healthcare practices among other verticals, so the team applies the same multi-entity discipline that mid-market and growth-stage companies rely on, tuned to the PC and MSO reality of dentistry.


Here’s the practical fit for a group deciding between building in-house and outsourcing:


- **Entity-level close** for each PC and the MSO, on a repeatable monthly cadence.
- **Intercompany and management-fee discipline** , booked symmetrically, reconciled, and eliminated on consolidation.
- **Location-level P&Ls** by office, provider, and payer, so EBITDA by location is board- and buyer-ready.
- **Consolidated, GAAP-first statements** built for credit facilities and PE-sponsor reporting, with ASC 805 acquisition accounting when the group buys the next practice.
- **CPA-led tax and fractional CFO advisory** on the same engagement, so structure, consolidation, and tax stay aligned.


That bundle is the difference between reporting that reacts to the next transaction and reporting that’s ready for it. Indinero has maintained continuous operations since 2009 with stable ownership, holds a 5-star Clutch rating, and is SOC 2 compliant (2026). Pricing starts at $750/mo on month-to-month terms. You can see the full scope of the[outsourced accounting services](https://www.indinero.com/services/accounting-services/) behind that, and step up to[fractional CFO services](https://www.indinero.com/services/cfo-services/) when the group is underwriting its next add-on.


If your dental group is outgrowing a single bookkeeper, it might be time for a different approach. Reach out for a free consultation. We’d love to learn about your group and find where we can help.


## Frequently asked questions


Common questions dental-group owners and controllers ask about DSO structure, management fees, consolidation, and when to bring in outside help.


### How does a DSO differ from a traditional dental group for accounting purposes?


A DSO differs from a traditional dental group because it runs across multiple legal entities that close separately, then consolidate into one statement package. A single-owner practice keeps one set of books inside one legal entity. A dental support organization splits clinical revenue into the professional corporation and back-office costs into the management company, so intercompany transactions and eliminations become routine every period. The difference isn’t more transactions, it’s a different reporting architecture.


### Why do dental organizations separate the PC from the MSO?


Dental organizations separate the PC from the MSO because corporate-practice-of-dentistry laws bar non-dentists from owning the clinical entity or collecting professional fees. A licensed dentist must own the professional corporation that delivers care, while non-dentist investors can own the management company that supplies staff, equipment, and back office. The two-entity model lets a group operate at scale without breaking that rule, with a management services agreement linking the two.


### How should management fees between the MSO and each PC be set and documented?


Management fees should be set at fair market value, the price two unrelated parties would agree to at arm’s length for defined services. Common methods include a fixed fee, cost-plus, or a percentage of net revenue, and cost-plus is widely viewed as the most defensible. Document the method inside the management services agreement, and keep a fair-market-value file listing each service and its rate so the fee holds on a state exam.


### What KPIs should a dental group track for each location?


A dental group should track production, collections rate, payer mix, chair utilization, labor ratios, and adjusted EBITDA for each location. Split doctor production from hygiene production, since hygiene is the recurring-revenue engine, and read a weak collections rate as a revenue-cycle issue inside the management company. Adjusted EBITDA by location matters most, because that’s the number a practice gets bought and sold on in a roll-up.


### When does a growing dental group need consolidated financial statements?


A growing dental group needs consolidated financial statements once it runs more than one entity and a lender or investor wants one combined picture. That usually hits between the third and fifth location, when cash pools in one management-company account. Because the MSO can’t own the PC’s stock, consolidation runs through the variable interest entity model in ASC 810 rather than a majority-vote test, which is where indinero builds the eliminations and combined statements.


### What accounting software do multi-location dental groups typically use?


Multi-location dental groups typically use a general ledger that supports dimensional tracking, such as QuickBooks Online, Xero, or NetSuite as they scale. The goal is dimensional accounting, tagging each transaction with a location, provider, and payer inside one system, so a single chart of accounts produces per-location, per-entity, and consolidated views without re-keying. Indinero works in QuickBooks, Xero, and NetSuite and builds the chart of accounts to report cleanly at every level.


### When should a dental group outsource its accounting instead of hiring in-house?


A dental group should outsource its accounting when it outgrows a single bookkeeper, usually between the third and fifth location, and in-house alone stops holding. That’s the point where the management fee becomes a plug, there’s no clean location-level P&L, and the lender wants consolidated statements next quarter. Indinero keeps PC and MSO books, location reporting, GAAP-first consolidation, and CPA-led tax and fractional CFO advisory on one engagement. Pricing starts at $750/mo.
