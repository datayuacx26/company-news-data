---
schema_version: "1.0.0"
document_id: "b1af9f00f52de3ac4843ba182177316e6ee004822159d9108f983f5306b03fab"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/passthrough-entity-tax-ptet-elections"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T08:12:02.916072+00:00"
fetched_at: "2026-07-30T08:12:04.374298+00:00"
content_hash: "sha256:cc78d2add78975c802f9e2300ed72b600cee6f432579f090970ca81a41c760b4"
---

# Passthrough Entity Tax (PTET) Elections

**A passthrough entity tax (PTET) election lets partnerships, S corporations, and certain LLCs pay state income tax at entity level creating a federal PTET deduction that bypasses individual $10,000 state and local tax (SALT) cap, with owners receiving a state PTET credit against their personal income tax.**


The election exists in more than 30 US states as of 2026, was made materially more valuable by OBBBA-era SALT cap adjustments, and has become one of highest-ROI tax planning moves a CPA firm makes for pass-through clients. This guide covers pass-through entity tax election mechanics, state-by-state variation, PTET deduction vs PTET credit flow, SALT cap workaround math, PTET refund treatment, and workflow a firm actually runs to file PTE tax election.


## **Key takeaways**


- PTET election = entity pays state income tax → business deducts it federally (unlimited by SALT cap) → owners get a state credit
- Available in 30+ states as of 2026; rates range ~4.95% (IL) to 9.3% (CA)
- Election is annual, on original return, typically irrevocable once made
- Deadlines vary California and several states require an estimated payment by June 15 during tax year
- Publicly traded partnerships excluded; consenting-owner rules and refundability vary by state
- Every PTET filing depends on a clean entity-level income number ledger cleanup is where firms lose time before filing.[Finlens](https://www.finlens.app/accountants) automates that layer directly in QuickBooks Online.


## What is a PTET election and why does it exist


The pass-through entity tax exists because of $10,000 SALT cap that TCJA imposed on individual itemized deductions in 2018. For high-income pass-through owners in high-tax states, that cap eliminated deductibility of state income tax on hundreds of thousands of dollars of state liability.


The workaround came from states themselves. Beginning in 2018 with Connecticut, then Wisconsin, Louisiana, Oklahoma, and Rhode Island in 2019, and rapidly expanding after IRS Notice 2020-75 blessed approach, states began enacting elective pass-through entity taxes. The mechanic is straightforward:


1. **The entity elects** to pay state income tax on its share of income at entity level, rather than passing that state tax obligation directly to owners.
2. **The entity deducts that state tax** as a federal business expense on Form 1065 (partnership) or 1120-S (S-corp), reducing federal K-1 income to owners.
3. **Owners receive a state PTET credit** usually on a state-specific K-1 equivalent that offsets their personal state income tax on same income, dollar-for-dollar (subject to state-specific refundability rules).


The net effect: SALT cap workaround moves state tax deduction from individual side (capped at $10,000) to entity side (uncapped for business).


For a partner or S-corp owner in California with $500,000 of pass-through income and a 9.3% PTET rate, federal deduction moves from ~$10,000 (under individual cap) to ~$46,500 (at entity level). At a 37% marginal federal rate, that's a federal tax savings of ~$13,500 on that one owner alone before considering state credit mechanics.


*Figure 1. The three-step flow of a PTET election. The federal deduction sits on entity's return; state credit sits on owner's return.*


‍


## Which entities can make pass-through entity tax election


Every state's PTET statute has its own list, but qualifying entity types are usually consistent:


- **Partnerships** filing Form 1065 (general, LP, LLP)
- **S corporations** filing Form 1120-S
- **Limited liability companies (LLCs)** taxed as partnerships or S corporations
- Some states extend to **certain trusts** with pass-through characteristics


Entities that generally **cannot** make election:


- Publicly traded partnerships (PTPs) excluded in most states
- Single-member LLCs treated as disregarded entities (they aren't pass-throughs in tax sense; owner files as an individual)
- C corporations they're already taxed at entity level and can deduct state tax without any cap
- Sole proprietorships filing Schedule C


Consenting-owner requirements vary. California requires all consenting owners to make election on a per-owner basis and only their share is subject to PTE tax. Illinois requires election to cover all partners/shareholders. Georgia requires 100% consent from all owners before election can be made. Every filing should verify state's consent rule before submission.


## PTET deduction and PTET credit two mechanics that make it work


The PTET workaround has two sides that CPAs often blur but should keep distinct:


### Side 1: The PTET deduction (federal, entity side)


The entity's payment of state PTE tax is a **federal business expense** deductible on Form 1065 line 14 (partnership) or Form 1120-S line 12 (S-corp) as "Taxes and Licenses" or similar. This reduces federal ordinary business income that flows to K-1s.


The PTET deduction is **not subject to $10,000 SALT cap** because SALT cap is an individual itemized-deduction limit under §164(b)(6). Business-level state tax is deducted under §164(a)(3), which cap does not touch.


### Side 2: The PTET credit (state, owner side)


Each state has its own credit mechanism, but pattern is consistent:


- Owner receives a state K-1 (or state equivalent) showing their share of PTET paid
- Owner claims a state PTET credit on their personal state income tax return
- Credit reduces state personal income tax dollar-for-dollar on PTET-taxed income
- Some states make credit **nonrefundable** (California, generally): excess credit above owner's state liability is either forfeited or carried forward
- Some states make it **refundable** (Minnesota): excess flows back as a PTET refund
- Owner's state itemized SALT deduction (on federal Schedule A) is reduced by credit amount to prevent double-dipping


The refundability rule is where PTET planning gets state-specific. A California resident with rental losses can end up with unused PTET credit that carries forward but generates no immediate refund; a Minnesota resident in same position receives a check.


## The SALT cap workaround math with a real example


The value of SALT cap workaround depends on three variables:


1. **The state PTE tax rate** 4.95% (IL) to 9.3% (CA) to 10.75% (some New Jersey brackets)
2. **The owner's federal marginal rate** typically 32% or 37% for ICP of this planning
3. **The owner's pre-election SALT cap headroom** most high-income owners in high-tax states are already fully capped at $10,000


Simplified example S-corp owner filing MFJ in California:


Line


Without PTET


With PTET


S-corp ordinary income (owner's share)


$500,000


$500,000


Entity-level PTE tax @ 9.3%


—


$46,500


Federal K-1 income to owner


$500,000


$453,500


California PTET credit to owner


—


$46,500


Owner's California liability (before credit)


$46,500


$46,500


Owner's California liability (after credit)


$46,500


$0


Owner's federal SALT itemized deduction


$10,000 (capped)


$10,000 (capped)


Owner's federal ordinary income


$500,000


$453,500


Federal tax @ 37% marginal


$185,000


$167,795


Federal tax saved via PTET


—


$17,205


The federal tax savings on this one owner is $17,205 in one year. On a five-owner S-corp with similar profiles, annual PTET benefit is often $70K–$100K+. This is why PTET has become highest-ROI tax planning move for high-income pass-through clients.


The math flips negative in a narrow set of cases: owners in low-tax states, owners with substantial passive losses that would otherwise absorb K-1 income, owners with an AMT-exposure profile, and out-of-state owners of an entity in a state with credit limitations for nonresidents. Every PTET planning conversation should model both scenarios.


## State-by-state PTET variation rules that actually differ


The states that offer a PTET are consistent on top-level structure but diverge on execution. The variables that matter for filing:


- **Election form and filing location.** California uses FTB 3804 filed with pass-through return. New York files via Department of Taxation and Finance online services. Illinois files on Form IL-1065 or IL-1120-ST. Every state's form is different.
- **Election deadline.** Most states require election on timely filed original return. Some (California, New York) require an *earlier* estimated payment during tax year California requires payment by June 15 to preserve full-year credit.
- **Consenting-owner rule.** Full consent required (Georgia), majority consent required (some states), or per-owner opt-in with only opting owners taxed (California).
- **Rate.** Illinois 4.95%, Virginia 5.75%, California 9.3%, New Jersey up to 10.75%. Rate is applied to entity's state-source income.
- **Refundability.** California nonrefundable with 5-year carryforward. Minnesota refundable. Georgia nonrefundable, no carryforward historically.
- **Sunset provisions.** Minnesota's PTET is available for tax years beginning after 12/31/2020 and before 1/1/2028 a scheduled sunset unless extended. Other states are permanent.
- **Nonresident owner treatment.** Some states limit or deny credit for nonresident owners; some require withholding on top of PTET.
- **Composite return interaction.** A few states require PTET election to run alongside (or instead of) composite return for nonresidents.


For any multi-state pass-through client, workpaper starts with a state-by-state matrix before return is even opened.


*Figure 2. The three variables CPA firms model per state: election deadline, rate, and refundability treatment.*


## PTET refund treatment and carryforward mechanics


The PTET refund question comes up when owner's credit exceeds their state personal income tax liability:


- **Refundable states (Minnesota, some others):** excess credit generates a state refund check.
- **Nonrefundable states with carryforward (California 5-year carryforward):** excess credit sits on owner's return and offsets state tax in future years.
- **Nonrefundable states without carryforward:** excess credit is forfeited.


The refund treatment matters most for owners with:


- Substantial passive activity losses that reduce state taxable income below credit amount
- AMT positions that limit effective credit
- Nonresident status in a state where credit is capped


For firms managing multiple PTET clients across states, carryforward tracking becomes a recurring workpaper each owner's unused PTET credit from prior years must be tracked and applied on subsequent year returns. Missing a carryforward is a real client-cost error.


Related:[tax resolution CPA firm process and fees](https://www.finlens.app/blogs/tax-resolution) covers how a firm structures multi-year tracking workpapers.


## The workflow a firm actually runs to file a PTE tax election


For any pass-through client where PTET election is on table, workflow looks like this:


**Step 1 Pre-election modeling (October–December of prior year, or January of election year).** Pull client's projected pass-through income by state. Model PTET-in vs PTET-out for each state. Confirm federal marginal rate assumption. Confirm owner consent posture.


**Step 2 Consent documentation.** For states requiring consent, get every affected owner's signed consent before election. Some states require this on a specific consent form; others accept board or partnership resolutions.


**Step 3 Estimated payment (mid-year, state-specific).** For California and other states requiring an estimated payment by June 15, make payment through state's business tax portal. Missing this deadline often costs year's PTET benefit even if election is later timely filed.


**Step 4 Election on original return.** File state PTET election form with pass-through return. In California, FTB 3804 attached to Form 565/568/100S. In Illinois, election checkbox on IL-1065/IL-1120-ST. Deadline is standard return due date (or extended due date if extension was filed).


**Step 5 State credit paperwork to owners.** Generate state K-1 (or state-specific PTET credit schedule) for each owner showing their share of PTET paid. Deliver to owners for their personal return preparation.


**Step 6 Federal K-1 adjustment.** The federal K-1 shows income net of PTET expense (already deducted at entity level). Owner's individual federal return does not add back PTET.


**Step 7 Owner personal return.** Owner claims state PTET credit on their personal state return; owner's federal Schedule A SALT deduction is reduced by PTET credit amount (per federal notice guidance to prevent double-benefit).


**Step 8 Multi-year carryforward tracking.** Log any unused PTET credit for future years.


## Where PTET calculation goes wrong upstream


Every step above depends on entity-level income number being right. The failure points on messy books:


- **Ordinary income vs separately-stated items misclassified** capital gains, interest income, dividends have to be split out before PTET-taxable base is calculated correctly
- **State-source income allocation wrong** a multi-state entity has to apportion income by state before PTET rate can be applied
- **Guaranteed payments to partners** treated differently across states for PTET purposes; some include in PTET base, some exclude
- **Stripe-processed revenue booked net of fees** understates gross revenue and therefore state-source income; PTET calc runs on a wrong base
- **Owner reasonable compensation on S-corps** has to be separated from K-1 ordinary income before PTET is computed


Finlens automates ledger cleanup that feeds these numbers: transaction categorization with per-client rules, Stripe payout decomposition, deferred revenue schedules, clean journal entries directly to QuickBooks Online. For pass-through clients where PTET is a material return item, that upstream cleanup is what makes entity income number defensible. Related:[Section 174 R&D capitalization guide](https://www.finlens.app/blogs/section-174-r-and-d-capitalization) covers same upstream-cleanup principle for R&E capitalization workpapers.


## Conclusion


**Pick one pass-through client with material state income in a high-tax state where PTET has not yet been elected bring three months of their QBO plus prior-year Form 1065 or 1120-S and we'll model PTET-in vs PTET-out federal savings on live numbers before estimated payment window closes.**


30+


states with PTET


$17K+


fed tax saved per owner (typical)


June 15


est. payment deadline


## Filing PTET but your
entity income isn’t clean?


Finlens categorizes activity in QuickBooks Online so the entity-level PTET base is defensible before the state form even opens.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


## Frequently asked questions


### What is a passthrough entity tax (PTET) election?


A PTET election is a state-level election that allows partnerships, S corporations, and certain LLCs to pay state income tax at entity level rather than passing that tax obligation directly to owners. The entity gets a federal PTET deduction (uncapped by $10K SALT cap), and owners get a state PTET credit on their personal state income tax.


### How does pass-through entity tax work as a SALT cap workaround?


The $10,000 SALT cap under §164(b)(6) applies only to individual itemized deductions. When pass-through entity pays state income tax itself, deduction moves from individual side (capped) to entity side (uncapped under §164(a)(3)). The entity then reduces federal K-1 income to owners by PTET amount, and owners claim a state PTET credit that offsets their personal state liability.


### Which entities qualify for a PTE tax election?


Partnerships (Form 1065), S corporations (Form 1120-S), and LLCs taxed as partnerships or S corporations. Publicly traded partnerships, single-member LLCs treated as disregarded entities, C corporations, and sole proprietorships generally cannot make election. State-specific consent rules vary some states require 100% owner consent; California allows per-owner opt-in.


### What states offer a PTET election in 2026?


More than 30 US states offer a PTET as of 2026, including California, New York, Illinois, Georgia, Virginia, Minnesota, New Jersey, Wisconsin, Connecticut, and Louisiana, among others. Rates range from ~4.95% (Illinois) to 9.3% (California) to 10.75% (some New Jersey brackets). Verify current PTET status in each state at state's Department of Revenue site before filing.


### What's difference between PTET deduction and PTET credit?


The PTET deduction is claimed on federal partnership or S-corp return as a business expense reduces federal K-1 income to owners. The PTET credit is claimed on owner's personal state income tax return offsets state tax on PTET-taxed income. The two mechanisms work together: business deducts federal, owner credits state.


### Is PTET credit refundable?


State-by-state. Minnesota and some other states offer a refundable PTET credit excess credit generates a state refund. California's PTET credit is nonrefundable with a 5-year carryforward excess credit rolls forward but does not generate an immediate refund. Georgia's PTET credit is nonrefundable, historically without carryforward. Every filing should verify state's refundability rule.


### When does PTET election deadline fall?


Most states require election on timely filed original pass-through return. Several states California, New York, and others also require an initial estimated payment during tax year (California requires payment by June 15) to preserve election or avoid credit reduction penalties. Missing mid-year estimated payment often costs full-year benefit in these states.


### Can I make a PTE tax election on an amended return?


Generally, no. In most states, PTET election must be made on timely filed original return. California and most other PTET states explicitly disallow election via amended return. Firms should not assume amendability verify state rule before advising a late election.


### What happens to unused PTET credit PTET refund question?


Refundable states (Minnesota, others): unused credit generates a state PTET refund. Nonrefundable states with carryforward (California 5 years): unused credit carries forward to offset future state tax. Nonrefundable states without carryforward: unused credit is forfeited. This is a recurring workpaper item for multi-year clients.


### Does PTET election work for out-of-state owners?


Sometimes. Some states limit or deny credit for nonresident owners; some require additional withholding on top of entity-level PTE tax; some interact with composite return filings for nonresidents. Multi-state pass-through clients need a full state-by-state matrix before PTET election is finalized.


### Was PTET election affected by OBBBA in 2025?


The OBBBA modified SALT cap itself but did not repeal PTET workaround. If anything, ongoing existence of SALT cap (in its modified form) preserves PTET's federal-deduction value. PTET remains one of highest-ROI tax planning moves for high-income pass-through clients through 2026 and beyond.


‍


*State PTET rates, deadlines, refundability rules, and eligibility criteria change frequently verify current guidance at each state's Department of Revenue site before filing. Federal PTET deduction treatment is per IRS Notice 2020-75 and subsequent guidance. Nothing in this article is legal or tax advice engage a licensed CPA, EA, or attorney for actual PTET planning and return preparation. Third-party trademarks referenced (QuickBooks®, Stripe®) belong to their respective owners.*


‍
