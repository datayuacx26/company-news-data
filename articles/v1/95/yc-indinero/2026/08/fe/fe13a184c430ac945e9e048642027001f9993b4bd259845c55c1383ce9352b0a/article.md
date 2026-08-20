---
schema_version: "1.0.0"
document_id: "fe13a184c430ac945e9e048642027001f9993b4bd259845c55c1383ce9352b0a"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/equity-compensation-payroll-for-saas/"
published_at: "2026-08-17T01:54:51+00:00"
first_seen_at: "2026-08-17T05:41:57.280567+00:00"
fetched_at: "2026-08-17T05:41:59.655052+00:00"
content_hash: "sha256:b4d568db1d222efa0e4a7a4a1df0b83284a08dcfba8199059fd58ba39a4046f6"
---

# Equity Compensation Payroll for SaaS: ISO, NSO, RSU, Restricted Stock

## The Categories: ISO / NSO / RSU / Restricted Stock


Four instruments cover nearly every grant a growth-stage SaaS company issues, and equity compensation payroll treats them very differently. Two create a withholding event you can predict. Two do not.


The distinction that matters for payroll isn’t the one printed on the cap table. It’s whether the instrument produces wages, when it produces them, and which employment taxes attach to those wages. A grant labeled ISO on the cap table can be part NSO once it hits payroll. A double-trigger RSU can sit dormant for four years and then drop its entire withholding obligation onto one settlement date.


Here’s how each one behaves.


### ISO: Incentive Stock Options Under IRC 422


An ISO is a statutory option. On exercise there’s no regular federal income tax event for the employee, the spread is an alternative minimum tax preference item, and the company withholds nothing.


ISO disqualifying disposition payroll handling is the part that gets botched constantly, and the trigger is a sale inside two years from grant or one year from the transfer of the share. It creates ordinary compensation income the company must report on Form W-2. It does not create FICA, FUTA, or federal income tax withholding. IRC 3121(a)(22) excludes ISO and ESPP stock from FICA wages, IRC 3306(b)(19) does the same for FUTA, and the final sentence of IRC 421(b) removes the chapter 24 withholding obligation.[IRS Notice 2002-47](https://www.irs.gov/pub/irs-drop/n-02-47.pdf) confirmed the agency would not assess any of the three, and the statutory exclusions enacted by the American Jobs Creation Act of 2004 made that permanent.


Report, don’t withhold.


Two more ISO details land directly in payroll. IRC 422(d) provides that where the aggregate fair market value of stock for which options are first exercisable in a calendar year exceeds $100,000, those options are treated as options that are not incentive stock options, counted in grant-date order. The excess tranche is an NSO, and NSOs do create withholding. Separately, IRC 422(c)(2) caps the reportable income at the lesser of the exercise-date spread or the actual gain on sale, which matters on a down-round secondary.


### NSO: Nonqualified Stock Options Under IRC 83


NSO payroll withholding starts at exercise. Not at grant, not at vest. Treasury Regulation 1.83-7(a) applies sections 83(a) and 83(b) at the time an option with no readily ascertainable fair market value at grant is exercised or otherwise disposed of.


The compensation amount is the bargain element. Fair market value at exercise minus the strike price, times shares. That number is wages. It flows to W-2 Boxes 1, 3 up to the Social Security wage base, and 5, it carries Box 12 code V, and it’s subject to federal income tax withholding, Social Security, Medicare, Additional Medicare Tax, and FUTA.


NSOs are also the instrument most likely to be held by someone who left. A former employee exercising eighteen months after termination, or a non-employee director who was never on payroll, changes both the available withholding method and the reporting form. That determination has to happen before the payroll run, not after the W-2s go out.


### RSUs and the Double-Trigger Timing Problem


An RSU produces ordinary income equal to the fair market value of the shares at settlement, with no purchase price to subtract. The full value is wages. That makes RSU payroll tax the simplest calculation on this page and the hardest number to fund.


Timing runs on two clocks. For FICA, the amount is taken into account on the later of when services are performed or when the right is no longer subject to a substantial risk of forfeiture. For income tax, the IRS position in GLAM 2020-004 treats the stock underlying an RSU as transferred when the employer initiates payment under the award.


Private SaaS companies overwhelmingly use double-trigger RSUs, where time vesting and a liquidity event are both required. That structure defers the entire withholding obligation to a single day, usually an IPO or an acquisition close. Years of accrued vesting land in one payroll run, and the company has to have cash sitting there when it does.


### Restricted Stock and the Section 83(b) Election


Restricted stock is actual stock, transferred subject to a vesting condition. Absent an election, income arises as restrictions lapse, tranche by tranche, at each tranche’s then-current fair market value.


An IRC 83(b) election flips that. The recipient elects to include the value at transfer, so the whole grant is taxed once at the early low valuation and all subsequent appreciation is capital gain. The statute gives 30 days from the date of transfer. There is no late relief.


Filing mechanics changed, and older content still repeats the previous version. The IRS released Form 15620 in late 2024 as a standardized Section 83(b) election form and opened an electronic filing option for it in July 2025. Use of Form 15620 isn’t mandatory. A conforming written statement filed on paper still works, and the recipient must still furnish a copy to the company.


The employer’s duty here is narrow but real. Flag the 30-day clock, collect the copy, and put the income into that period’s payroll. An 83(b) election on unvested stock issued to an employee produces W-2 wages at grant, subject to withholding, and that’s exactly the line item that disappears when equity gets administered outside payroll. The same grant also drives the[stock compensation expense that hits your books](https://www.indinero.com/blog/equity-accounting/) , which is why the two records need to agree.


## Payroll Tax Treatment


Equity income is supplemental wages by regulation, not by convention, and the tax treatment splits along statutory versus nonstatutory lines. Treasury Regulation 31.3402(g)-1 lists income recognized on the exercise of a nonstatutory stock option and wage income recognized on the lapse of a restriction on restricted property as supplemental wages by name.


Event FITW Social Security Medicare FUTA W-2


ISO exercise No No No No None


ISO disqualifying disposition No No No No Box 1 only


ESPP disqualifying disposition No No No No Box 1 only


NSO exercise Yes Yes Yes Yes Boxes 1, 3, 5 plus Box 12 code V


RSU settlement Yes Yes Yes Yes Boxes 1, 3, 5


Restricted stock, no 83(b), at lapse Yes Yes Yes Yes Boxes 1, 3, 5


Restricted stock with 83(b), at grant Yes Yes Yes Yes Boxes 1, 3, 5


83(i) qualified equity grant inclusion Yes, at 37% Per 83(i) rules Per 83(i) rules Per 83(i) rules Box 12 codes GG and HH


That last row surprises people. IRC 3402(t) requires deferred income under a qualified equity grant election to be withheld at the maximum rate under section 1, which is 37% for 2026. There’s no 22% option on 83(i) income.


### 2026 rates and thresholds


- **Supplemental flat rate, 22%.**[IRS Publication 15](https://www.irs.gov/publications/p15) keeps the supplemental withholding rate at 22%, rising to 37% once supplemental wages paid to an employee during the calendar year exceed $1 million.
- **The 37% tier is mandatory.** Once cumulative supplemental wages from one employer pass $1,000,000, Treasury Regulation 31.3402(g)-1 requires the highest rate under section 1 on the excess. Form W-4 entries do not override it.
- **Social Security, 6.2% each side on the first $184,500.** The 2026 wage base rose from $176,100, putting the maximum at $11,439 for the employee and $11,439 for the employer.
- **Medicare, 1.45% each side, uncapped.** Additional Medicare Tax of 0.9% applies to wages over $200,000, employee-only, and you must begin withholding it once wages cross $200,000 regardless of the employee’s filing status.
- **FUTA, 6.0% on the first $7,000** with the 5.4% credit producing a 0.6% effective rate, before credit reduction states. California is a credit reduction state for 2026.


### The flat rate has conditions, and terminated employees can fail them


The 22% flat method is optional, and one of its conditions is that income tax was withheld from the employee’s regular wages during the calendar year of the payment or the preceding year. A former employee exercising NSOs eighteen months after termination may fail that test. So may a director or advisor who was never on payroll.


When the condition fails, the aggregate procedure applies instead, which means running the equity income through the employee’s Form W-4 as if it were a regular payroll period. Most systems will happily apply 22% anyway, because nothing in the software knows to check. This is a determination, not a setting, and it belongs next to the rest of your[payroll tax obligations](https://www.indinero.com/blog/define-payroll-tax/) rather than buried in an off-cycle earning code.


### The deposit-timing trap


This is the section most equity comp content skips, and it’s where the money is.[IRS Topic 757](https://www.irs.gov/taxtopics/tc757) sets the deposit schedule. Lookback-period liability of $50,000 or less makes you a monthly depositor, more than $50,000 makes you semiweekly. Then comes the override. Accumulate $100,000 or more of employment taxes on any day during a deposit period and the deposit is due the next business day. A monthly depositor that trips it becomes a semiweekly depositor for the rest of the calendar year and all of the following year.


A single RSU vest or a cluster of NSO exercises at a Series C company clears $100,000 easily. On $600,000 of taxable RSU value in one settlement, federal supplemental withholding alone at 22% is $132,000, before FICA and before state.


IRC 6656 penalties escalate fast. 2% for not more than 5 days late, 5% for more than 5 but not more than 15 days, 10% beyond 15 days, and 15% once the tax stays undeposited after notice and demand. Narrow relief exists. An administrative waiver at IRM 20.1.4.26.2 can excuse the failure-to-deposit penalty where taxes are deposited within one business day of the settlement date and settlement occurred within two business days of the exercise or RSU payment-initiation date. The 2020 update extended it to stock-settled RSUs and stock-settled SARs.


That window got tighter. The SEC moved securities settlement from T+2 to T+1 effective May 28, 2024, so a Wednesday sale that used to settle Friday with taxes due Monday now settles Thursday with taxes due Friday. Employers lost a full business day to calculate, fund, and deposit. All of it eventually reconciles on[Form 941](https://www.indinero.com/blog/form-941/) , but the penalty attaches to the deposit, not the return.


### Funding the withholding: sell-to-cover versus net settlement


- **Sell-to-cover.** A broker sells enough shares on the open market to fund the withholding and remits cash to the company. It requires a market, so it’s generally post-IPO, and it’s the mechanism that puts you on the T+1 clock because the cash doesn’t exist until settlement.
- **Net settlement.** The company retains shares equal in value to the withholding and funds the deposit from its own cash. No market required, so it works pre-IPO. It also consumes real corporate cash on the vest date, and it’s capped. Under ASC 718 as amended by ASU 2016-09, share withholding stays equity-classified only up to the maximum statutory tax rates in the applicable jurisdiction. Exceed that and the whole award flips to liability classification and marks to market every period.
- **Cash exercise plus cash withholding.** The employee wires the strike price and the withholding. Cleanest for the company, hardest for the employee, common on early-stage NSO exercises.


One more thing worth telling people in advance. Withholding at 22% satisfies your legal obligation. It does not satisfy an employee’s actual liability if they sit in the 32%, 35%, or 37% bracket, and the gap shows up in April as a surprise you’ll hear about.


## Reporting Forms


Three forms carry equity compensation to the IRS. Form W-2 for wage income, Form 3921 for ISO exercises, and Form 3922 for ESPP transfers. Two of the three create no withholding at all and still carry real penalties.


### Form W-2 box mapping


The[General Instructions for Forms W-2 and W-3](https://www.irs.gov/instructions/iw2w3) define the codes that matter here.


- **Box 12 code V.** Income from the exercise of nonstatutory stock options. NSO only.
- **Box 12 code Z.** Income under a nonqualified deferred compensation plan that fails section 409A. This is where a mispriced option lands.
- **Box 12 code GG.** Income from qualified equity grants under section 83(i).
- **Box 12 code HH.** Aggregate deferrals under section 83(i) elections as of the close of the calendar year.


There is no Box 12 code for ISO or ESPP income. Disqualifying disposition income from an incentive stock option or an ESPP goes in Box 1, plus Boxes 16 and 18 where the state and locality tax it. It stays out of Boxes 3 and 5, because IRC 3121(a)(22) excludes it from FICA wages, and it carries no Box 12 code at all. Any content that assigns ISOs a Box 12 code is wrong, and it’s a common enough error that it’s worth checking your provider’s mapping directly. The 2026 Form W-2 filing deadline with the SSA is February 1, 2027, since January 31 falls on a Sunday, for both paper and electronic filers.


### Form 3921 and Form 3922


Form 3921 is filed by every corporation that transfers a share of stock to any person during the calendar year pursuant to that person’s exercise of an incentive stock option. One form per exercise, per employee. It reports grant date, exercise date, exercise price per share, fair market value per share on the exercise date, and number of shares transferred.


This one catches early-stage companies because the trigger is the exercise itself, and exercises happen whether or not anyone tells finance. It’s purely informational. No withholding, no deposit, just a filing obligation that accrues quietly.


Form 3922 covers ESPPs. It’s filed by every corporation that records, or has by its agent recorded, a transfer of legal title of a share acquired under an employee stock purchase plan described in section 423(c). It applies to the first legal-title transfer, not to every subsequent trade.


### Deadlines and penalties


Furnish the employee statement by January 31. Paper file with the IRS by the end of February, and file electronically by March 31. Electronic filing became mandatory at 10 or more returns when Treasury Decision 9972 dropped the threshold from 250, counted by form type, so 10 or more ISO returns or 10 or more ESPP returns triggers it. Form 8809 buys an automatic 30-day extension of the IRS filing deadline if you file it before the due date, and it does nothing for the January 31 recipient statement.


Penalty amounts for tax year 2026 returns due in 2027 follow the same schedule as[other information returns](https://www.indinero.com/blog/types-of-1099-forms/) :


Timing Per return


Corrected within 30 days $60


31 days late through August 1 $130


After August 1 or not filed $340


Intentional disregard $680, no cap


Annual maximums for 2026 are $4,191,500 for filers with average annual gross receipts above $5,000,000 and $1,397,000 for filers at or below that line. Here’s the part that stings. IRC 6721 covers filing with the IRS and IRC 6722 covers furnishing to the payee, and they’re separate penalties at the same per-return amount. Missing both sides of a single Form 3921 is $680, not $340. Fifty ISO exercises missed entirely is $34,000 before anyone argues intentional disregard.


## The 409A Connection


The 409A valuation isn’t a compliance artifact that lives in a folder. It’s the first term in the payroll arithmetic, and every number downstream inherits its accuracy.


### From appraisal to withholding base


1. The 409A appraisal establishes fair market value per share of common stock.
2. The board sets the option strike price at or above that FMV.
3. FMV at exercise minus strike equals the spread.
4. The spread is the wage base for federal income tax withholding, Social Security, Medicare, and FUTA on an NSO exercise.


Break the first link and every number after it is wrong.[Treasury Regulation 1.409A-1(b)(5)](https://www.law.cornell.edu/cfr/text/26/1.409A-1) exempts a stock right from 409A only where the exercise price may never be less than the fair market value of the underlying stock on the grant date. The regulation grants a presumption of reasonableness to an independent appraisal, and to illiquid start-up stock valued reasonably and in good faith in a written report. That presumption dies two ways. The valuation fails to reflect information available after it was calculated that may materially affect the value of the corporation, or the valuation is more than 12 months old on the date you’re relying on it. A priced round, a term sheet, an acquisition offer, a large customer loss, and the simple passage of twelve months all end it. Our[409A valuation services](https://www.indinero.com/services/409a-valuation-services/) exist largely to keep that clock from running out unnoticed.


### What a stale 409A costs


Granting options off a stale 409A is granting at a discount, and a discounted option is nonqualified deferred compensation that fails 409A under IRC 409A(a)(1)(B). The consequence lands on the employee, which is why it becomes a founder problem fast.


- **Immediate income inclusion** of the vested deferred amount in the year of vesting, before any exercise and before any liquidity.
- **An additional tax equal to 20 percent** of the compensation required to be included in gross income.
- **A premium interest charge** at the underpayment rate plus 1 percentage point on the underpayments that would have occurred had the amount been includible when first deferred.


Several states pile on. California has historically imposed its own additional tax on 409A failures, on top of the federal exposure. Payroll is where all of this surfaces, because the employer reports the failed amount in W-2 Box 12 with code Z. A company that finds a mispriced tranche is looking at amended W-2s, corrected 941s, and a conversation with employees who owe tax and a 20% penalty on paper gains they can’t sell. The operational rule is short. Refresh at least every 12 months and on every material event, grant only off a current valuation, and keep the valuation, the cap table, and the payroll system reading the same number. Our[guide to 409A valuations](https://www.indinero.com/resources/the-essential-guide-for-409a-valuations/) and the[background on IRC Section 409A](https://www.indinero.com/blog/irs-409a/) cover the refresh triggers in detail.


## The Multi-State Complexity


Equity compensation is sourced to where the services were performed during the award’s earning period. Not where the employee lived on the vest date, and not where the company is headquartered.


Equity comp follows the work.


The mechanism is a workday allocation fraction. Days worked in the state during the allocation period, divided by total days worked everywhere during that period, multiplied by the income. The problem is that the two states that pursue this hardest do not use the same allocation period.


### New York allocates grant to vest


[TSB-M-07(7)I](https://tax.ny.gov/pdf/memos/income/m07_7i.pdf) sets New York’s treatment of stock options, restricted stock, and stock appreciation rights for tax years beginning on or after January 1, 2006, and it uses a grant-to-vest allocation. The numerator is days worked in New York for the grantor during the allocation period. The denominator is days worked everywhere for the grantor during that same period.


Nonresidents who worked both in and out of New York across the grant-to-vest window file Form IT-203-F, the multi-year allocation form. New York also applies its convenience-of-the-employer test, which can pull in a remote nonresident who never set foot in the state. If you have New York headcount, the[New York state tax rules](https://www.indinero.com/state-tax-guides/new-york-tax-rates/) matter to every grant you’ve issued to those people, not just to their salary.


### California allocates grant to exercise


California uses a different window for options. FTB Publication 1004, the state’s[equity-based compensation guidelines](https://www.ftb.ca.gov/forms/misc/1004.html) , allocates on California workdays over total workdays from the grant date to the exercise date. 500 California workdays out of 1,000 total gives a 50% allocation ratio applied to total option income. For ESPPs, the period is the offering period instead.


California also has a supplemental rate trap. The state’s Employer’s Guide, DE 44, sets the flat supplemental personal income tax withholding rate at 10.23% for bonuses and stock options, and 6.6% for other supplemental wages. A payroll system that defaults equity income to the generic 6.6% under-withholds California PIT on every option exercise and every RSU vest. Worth checking against the broader[California tax rates](https://www.indinero.com/state-tax-guides/california-tax-rates/) before your next vest date.


### What this does to your withholding footprint


The fractions don’t sum to 100%. An employee who worked in New York from grant to vest, then moved to Texas and exercised two years later, allocates substantially to New York under the grant-to-vest rule. Add a California leg and the longer grant-to-exercise denominator produces a different fraction again. Overlapping claims are normal, not an error. Resident-state credits sort it out on the individual return, and you still have to withhold and remit correctly in each state in the meantime.


Withholding obligations also trail the employee. When someone relocates, the former state keeps its claim on the portion of equity income earned there, so you can owe withholding in a state where that person no longer works and where you may have no other payroll. Registering in a state solely because of one vest is a common surprise, and it’s related to the broader question of[where your company has created tax nexus](https://www.indinero.com/blog/startup-tax-nexus/) . No-income-tax states simplify the arithmetic, so a[Texas](https://www.indinero.com/state-tax-guides/texas-tax-rates/) -based leg of an allocation period reduces the state layer rather than adding one.


The operational requirement is a workday location record per employee per award, retained for the life of the award. Cap-table platforms don’t keep it. Payroll systems don’t keep it by grant. It’s the single most common gap at companies that went remote.


## How Indinero Handles Equity Comp Payroll


Stock compensation payroll breaks for a structural reason. The data lives in three systems owned by three different people. The cap table sits in Carta or Pulley with the founder or general counsel. The payroll system sits with People Ops. The 409A valuation and the ASC 718 expense sit with accounting. Nobody owns the handoff, and the handoff is the whole job.


Payroll vendors have made real progress on data sync and are honest about the limits. Carta publishes integrations with both Rippling and Gusto that sync employee records, termination dates, and equity data so the two systems agree on who holds what. What no integration does is decide whether a tranche is ISO or NSO under the 422(d) $100,000 limit, decide whether the flat method or the aggregate method applies to a terminated exerciser, calculate a state-by-state workday allocation, or warn you that Thursday’s settlement puts you over $100,000 and onto a next-business-day deposit. ADP, Gusto, and Rippling all accept equity income as a supplemental or off-cycle earning. None of them owns the determination of what to enter.


Indinero’s payroll work is advisory-layer. It runs on top of whatever payroll system you already have, which is the only sensible answer when you’re on Gusto or Rippling or ADP and have no interest in migrating.


- **Classification before the event.** Which tranche is a true ISO and which spilled past the 422(d) limit into NSO treatment. Whether the exerciser is a current employee, a former employee, or a non-employee director, which changes both the withholding method and the form.
- **The withholding math and the deposit call.** Applying 22% or the mandatory 37% above $1,000,000, confirming the optional flat rate is even available, layering FICA against the $184,500 wage base and the $200,000 Additional Medicare threshold, and computing state withholding at rates like California’s 10.23%. Then the part nobody else does. Modeling accumulated liability against the $100,000 next-day threshold before settlement, and mapping the T+1 funding timeline so the cash is there.
- **409A and ASC 718 on the same desk as payroll.** The valuation that sets the strike, the expense that hits the P&L, and the withholding base that hits the 941 are three uses of one number. Running them in one place is how a stale 409A gets caught before it becomes a code Z on a W-2.
- **Multi-state sourcing tracked per award.** Workday location history maintained across both the grant-to-vest and grant-to-exercise windows, so New York and California allocations compute from the same underlying record and a relocation doesn’t become an unregistered withholding obligation.
- **The reporting calendar owned end to end.** W-2 box mapping, Form 3921 for every ISO exercise, Form 3922 for every ESPP transfer, furnished by January 31 and filed electronically by March 31 once you cross 10 returns.


That coordination sits inside the same engagement as the[bookkeeping and monthly close](https://www.indinero.com/services/accounting-services/) the equity expense flows into, the R&D credit that offsets payroll tax on Form 8974 using W-2 wage data, and the[business tax planning](https://www.indinero.com/services/business-tax-services/) that anticipates the year the RSUs land. Continuous operations since 2009, 500+ regular customers, and 100+ years combined team experience sit behind it.


No payroll platform can run this, because it isn’t a payroll platform problem. It’s a payroll plus valuation plus equity accounting plus multi-state tax problem, and those four answers have to come from the same team reading the same cap table. If equity comp currently gets handled by whoever notices it first, that’s worth a conversation.


## Frequently asked questions


These are the questions that come up on nearly every equity comp event, answered from the employer’s side. Withholding, reporting, and timing, not personal tax planning.


### What is the difference between ISO, NSO, RSU, and restricted stock?


ISOs are statutory options with no withholding, NSOs create wages at exercise, RSUs create wages at settlement, and restricted stock is taxed as it vests. For payroll, the label on the cap table isn’t the determination. IRC 422(d) treats ISOs first exercisable above $100,000 in a calendar year as NSOs, counted in grant-date order, so part of one grant can carry withholding while the rest carries none. Confirm that split before the payroll run, not after the W-2s go out.


### How is each type of equity comp taxed at vesting or exercise?


NSO exercises and RSU settlements produce ordinary wage income, ISO exercises produce none for regular tax, and restricted stock is taxed as restrictions lapse. The NSO amount is fair market value at exercise minus the strike price. The RSU amount is the full settlement-date value with nothing to subtract. An ISO spread is an alternative minimum tax preference item, not a payroll event. A Section 83(b) election on restricted stock pulls the whole grant into W-2 wages at grant.


### What payroll tax withholding applies to RSU vesting?


RSU settlement value is supplemental wages, subject to 22% federal withholding plus Social Security, Medicare, FUTA, and state withholding. The rate jumps to a mandatory 37% once supplemental wages from one employer pass $1,000,000 in a calendar year, and a Form W-4 entry can’t override it. Watch the deposit clock too. Accumulate $100,000 or more of employment taxes on any day and the deposit is due the next business day, which one large vest clears easily.


### How does an ISO disqualifying disposition flow through payroll?


An ISO disqualifying disposition creates ordinary income reported in W-2 Box 1 only, with no FICA, no FUTA, and no federal income tax withholding. IRC 3121(a)(22) excludes the income from FICA wages, IRC 3306(b)(19) does the same for FUTA, and the last sentence of IRC 421(b) removes the withholding duty. Report, don’t withhold. There is no Box 12 code for ISO income, and the reportable amount is the lesser of the exercise-date spread or the actual gain on sale.


### How does the 409A valuation connect to NSO exercise payroll withholding?


The 409A valuation sets the fair market value that fixes the option strike price, and the strike price drives the NSO withholding base. Fair market value at exercise minus strike is the spread, and that spread is the wage base for withholding and FICA. A valuation over 12 months old or overtaken by a material event makes the grant discounted deferred compensation, triggering income at vesting, a 20% additional tax, and W-2 Box 12 code Z. Indinero keeps the valuation, the cap table, and payroll on one number.


### How does equity comp sourcing work for multi-state employees?


Equity comp is sourced to where the employee worked during the award’s earning period, using a workday allocation fraction, not to the residence state. New York allocates from grant to vest under TSB-M-07(7)I. California allocates from grant to exercise under FTB Publication 1004, so one employee can produce two different fractions. Overlapping claims are normal. Indinero tracks workday location per award across both windows, so a relocation doesn’t become an unregistered withholding obligation.


### What payroll providers handle equity comp natively?


No payroll provider handles equity compensation end to end, though ADP, Gusto, and Rippling all accept equity income as a supplemental or off-cycle earning. Carta publishes integrations with Rippling and Gusto that sync employee records, termination dates, and equity data. What no integration does is classify a tranche under the 422(d) limit, pick the withholding method, compute state workday allocation, or flag the $100,000 next-day deposit. Indinero owns those calls on top of the payroll system you already run.
