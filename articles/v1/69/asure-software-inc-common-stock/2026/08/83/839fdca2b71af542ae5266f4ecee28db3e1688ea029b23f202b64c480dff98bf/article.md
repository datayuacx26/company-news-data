---
schema_version: "1.0.0"
document_id: "839fdca2b71af542ae5266f4ecee28db3e1688ea029b23f202b64c480dff98bf"
company_key: "asure-software-inc-common-stock"
company: "Asure Software Inc"
source_id: "asure-software-inc-common-stock-rss-2ceab48495f7"
canonical_url: "https://www.asuresoftware.com/why-multi-jurisdiction-payroll-tax-compliance-breaks-down-and-how-growth-stage-companies-fix-it-2/"
published_at: "2026-08-10T13:34:54+00:00"
first_seen_at: "2026-08-10T15:01:28.894037+00:00"
fetched_at: "2026-08-10T15:01:30.119683+00:00"
content_hash: "sha256:ed216fa90f90fc0fdea7071e385d9710ee2803438b3ead6a6bdb4ac22f427f4c"
---

# Why Multi-Jurisdiction Payroll Tax Compliance Breaks Down, and How Growth-Stage Companies Fix It

Growth-stage companies do not fail multi-jurisdiction payroll tax compliance because the rules are unknowable. They fail because they treat jurisdiction expansion as a one-time setup task rather than a continuous decision process. In Asure's experience with growing employers, the companies that get this right build that decision process before they touch a single rate table.


## The Moment a Company Crosses Into a Second State, Its Payroll Assumptions Break


Every payroll configuration rests on assumptions, even when nobody writes them down. If your company operates in one state, you likely run one unemployment insurance (UI) account, one state income tax withholding setup, and, depending on the state, one State Disability Insurance (SDI) or paid-leave program. Those assumptions hold up fine, right up until you hire into your second state.


That is the moment your configuration stops matching reality. Nothing fails loudly. Your payroll system keeps running the numbers you gave it; what breaks is quieter, because no setup screen prompts you to ask whether the new state has more than one program, or whether a city inside it layers on a program of its own.


Washington shows how layered a single state's payroll tax footprint can get. The state runs its own unemployment insurance program, a Paid Family and Medical Leave (PFML) program funded through a split employer and employee premium, and the WA Cares Fund, a long-term care insurance program funded through an employee payroll deduction. Seattle adds a Payroll Expense Tax on top of all three, a city-level tax charged to the employer rather than withheld from the employee, that applies once a company crosses specific payroll-size and compensation thresholds.


In Asure's work with growth-stage companies crossing into Washington for the first time, the surprise is consistent: the state runs three separate payroll tax programs, and Seattle layers on a fourth for employers who meet its size test.


Every one of these programs is documented by the agency or city that administers it. The rule was always there to find. What actually breaks is a narrower assumption: that a single-state configuration, once built, treats "payroll tax" as one filing per jurisdiction. A team that has only ever configured payroll in a state with one UI program and no local layer has no reason to expect four programs stacked on top of each other, some withheld from the employee, some paid entirely by the employer, some applying only above a size threshold.


This is not unique to Washington. Washington is simply the state where the stacking is most visible, because it combines a long-term care program, a split-premium leave program, and a major city with its own payroll tax in one place. When a company expands into other states, it runs into the same structural surprise in smaller doses: a county income tax here, a municipal withholding rule there, invisible until someone is hired who lives or works inside its boundary.


The failure is not random, though. It follows a shape, and that shape starts with how a payroll team categorizes the programs it is trying to configure in the first place. For definitions of these Washington and Seattle programs, see theglossary of state and local payroll tax terms ; for a closer look at how the categories interact as a company grows, seehow growth-stage companies build a jurisdiction expansion decision process .


## Payroll Programs Cluster Into Three Decision Types, and Most Teams Only Plan for One


Not every payroll tax program asks the same question. Some ask whether you are applying the correct rate. Others ask how to split a premium between employer and employee. Others ask whether the program applies at all. Treating all three as the same kind of lookup is where growth-stage payroll teams lose time, and eventually, money.


The first archetype is employer-only programs, where the employer bears the full cost and the only real configuration question is rate accuracy. State Unemployment Insurance (SUI), sometimes called State Unemployment Tax Act (SUTA) tax, is the most common example: Florida's Reemployment Tax and Texas's SUI program both fall into this category. Employees never see a line item for these; the job is simply to apply the correct, current employer rate.


The second archetype is split-premium programs, where employer and employee each fund a portion of the same program, and the configuration question is allocation logic, not just rate lookup. New Jersey's Unemployment Insurance program, Washington's PFML, and SDI programs in California, New Jersey, New York, and Rhode Island all require a payroll team to correctly separate an employer share from an employee share, often on the same paycheck, and remit both correctly.


The third archetype, and the one growth-stage companies are least prepared for, is location-triggered local programs, where the question is not what the rate is but whether the program applies to a given employee at all, based on where that employee lives or works. Seattle's Payroll Expense Tax, Pittsburgh's Local Services Tax, Indiana's county income tax system, and Ohio's municipal withholding rules all fall into this category.


The way Asure approaches payroll configuration for growth-stage employers treats these three archetypes as genuinely different problems, because the failure modes do not overlap: employer-only programs fail on rate accuracy, split-premium programs fail on allocation logic, and location-triggered programs fail on applicability detection.


Indiana shows most clearly why location-triggered programs need a different process than a rate lookup. All 92 Indiana counties levy their own county income tax, and the rate that applies to a given employee is set by that employee's county of residence as of January 1 of the tax year, or, if the employee resided out of state on that date, by the county of their principal place of employment in Indiana, according to the[Indiana Department of Revenue](https://www.in.gov/dor/files/dn01.pdf) . A team that treats this the way it treats a single state rate, something to look up once and set permanently, will miss the fact that the applicable county, and therefore the rate, is a snapshot in time, not a fixed setting.


A rate table solves the first two archetypes reasonably well. Applicability detection, the third, requires something closer to an ongoing question a payroll team asks about every employee's location, not a table it consults once at hire. That distinction between rate lookup and applicability detection is exactly where the next failure shows up. Seehow growth-stage companies build a jurisdiction expansion decision process and theprocedure for configuring employer versus employee splits .


## The Programs Most Likely to Generate Retroactive Liability Are the Ones Teams Do Not Know to Look For


The conventional wisdom about payroll tax risk points toward the programs with the highest rates and the most visible penalties, which usually means federal and state programs. That is a reasonable place to focus attention. It is also not where most growth-stage companies actually get exposed.


What generates retroactive liability, penalty assessments, and back-payment obligations for growing employers is disproportionately local programs that were never on a payroll team's radar in the first place. These programs share three traits: they are administered by a city, county, or school district rather than a state, they are not typically surfaced during standard payroll-software onboarding, and they have no federal or state analog that would prompt a team to go looking for them.


Seattle's Payroll Expense Tax is the clearest current example. For 2026, it applies to employers with $9,074,409 or more in 2025 Seattle payroll expense, and at least one employee with 2026 compensation of $194,452 or more, per the[City of Seattle](https://www.seattle.gov/city-finance/business-taxes-and-licenses/seattle-taxes/payroll-expense-tax) . Both thresholds adjust every January 1 based on the prior year's change in the Consumer Price Index for All Urban Consumers (CPI-U) for the Seattle-Tacoma-Bellevue area, which means a company can owe the tax this year without changing anything about its workforce, simply because payroll expense or an employee's compensation crossed the moving line. This is a tax on the employer's payroll expense, a different configuration question entirely from an employee withholding like WA PFML or WA Cares.


Pittsburgh's tax is now formally the Local Services Tax (LST), the modern name for what many payroll teams still call by its older name, the Occupational Privilege Tax. Pennsylvania caps the LST statewide at $52 per employee per year, no matter how many Pennsylvania jurisdictions that employee works in during the year, and jurisdictions that levy more than $10 of it must exempt employees whose total earned income from all sources is $12,000 or less, according to the[Pennsylvania Department of Community and Economic Development](https://dced.pa.gov/local-government/local-income-tax-information/local-services-tax/) . Pittsburgh levies the full $52, split $47 to the city and $5 to the school district, under its own City Code. A remote employee who occasionally works from a Pittsburgh location is easy to miss precisely because this is a flat, small-dollar tax rather than a percentage of pay.


What we have seen repeatedly in Asure's work with growth-stage companies is that the programs generating the most retroactive exposure are not the ones with the highest rates. They are the ones that were never on anyone's checklist, because nothing about a federal or state filing calendar would have prompted a team to add them.


Indiana's county income tax system creates a related but distinct trap for hybrid employees: because the applicable rate follows county of residence, not county of work, an employee who splits time between an office in one county and a home in another can be classified incorrectly if a payroll team assumes the office location, rather than the residence address on file, determines the rate. See theFAQ hub for state and local payroll programs andcurrent thresholds for Seattle's Payroll Expense Tax .


Knowing which programs to look for closes part of the gap. The harder problem, and the one that has gotten measurably harder since 2020, is keeping that knowledge current as employees' locations change.


## Remote and Hybrid Work Turned a Static Configuration Problem Into a Dynamic One


Before 2020, multi-jurisdiction payroll complexity was mostly a function of where a company opened offices, a knowable, stable, and finite list. A payroll team could configure a jurisdiction once, when a location opened, and revisit the list only when another one opened.


Remote and hybrid work broke that model. Complexity is now a function of where employees live, which is not knowable at the time of hire in any durable sense, is not stable as people move, and is not bounded by any list a company controls. A single remote hire in a new state can trigger a cascade of questions a payroll team has to answer correctly before the first paycheck goes out. Does the state have income tax withholding? Does it have SUI? Does it have SDI or a paid family and medical leave program? Does the employee's county levy a local income tax? Does the employee's city levy a payroll expense tax?


Indiana shows how this cascade plays out over time, not just at hire. Because Indiana's county income tax rate follows an employee's county of residence as of January 1 of the tax year, an employee who moves from one Indiana county to another mid-year does not trigger an immediate rate change. The new county's rate applies starting with the next January 1 snapshot, per the[Indiana Department of Revenue](https://www.in.gov/dor/files/dn01.pdf) , which means a payroll team has to track the move, confirm the employee's residence as of that date, and update withholding accordingly, rather than assuming the change is either instant or irrelevant.


WA Cares is the program we see misconfigured most often for remote employees. It is funded entirely by an employee payroll deduction of 0.58% of gross wages for 2026, with no wage cap, and it applies to Washington employees regardless of where, physically, they perform their work, according to the[WA Cares Fund](https://wacaresfund.wa.gov/employers) . Employees can apply for an exemption, for reasons including existing private long-term care coverage, military spouse status, out-of-state residency, or certain visa categories, but those exemptions are employee-initiated, and some require ongoing eligibility verification rather than a one-time approval. A team that treats an approved exemption as permanent, rather than a status to periodically confirm, can end up under-collecting without realizing it.


Asure's approach to remote-workforce payroll treats an employee's location as a piece of information that can change on any given day, not a field filled in once at hire, because the compliance obligation shifts the moment a home address does, and most payroll systems do not surface that shift automatically.


Ohio adds a structural wrinkle other states do not share in quite the same way. Under Ohio Revised Code (ORC) 718.011, an employee's home is explicitly excluded from the definition of "worksite location," so the state's 20-day de minimis rule, which normally excuses withholding for short-term work at a non-principal location, does not apply to a remote employee working from home, according to the[Ohio Revised Code](https://codes.ohio.gov/ohio-revised-code/section-718.011) . That matters because Ohio's pandemic-era temporary relief, which had allowed continued withholding based on an employee's pre-pandemic principal workplace, ended January 1, 2022. Since then, the default for a remote or hybrid Ohio employee working from home has been withholding based on that employee's home municipality, not the employer's office municipality.


The thread running through Indiana, Washington, and Ohio is the same. The trigger for a compliance obligation is no longer a decision the company makes, like opening an office. It is a decision an employee makes, like moving, that the company has to detect and respond to. See theprocedure for updating payroll when a remote employee's location changes andobservations on post-2020 remote work payroll compliance .


## The Companies That Get Multi-Jurisdiction Payroll Right Treat It as an Ongoing Audit


The companies that sustain compliance across a growing number of jurisdictions do not have noticeably better rate tables than the ones that struggle. What they have is a review cadence. The ones that hold up over time run a quarterly jurisdiction audit rather than an annual one, and that audit asks three questions. Have any employees changed their work or home location since the last review? Have any jurisdiction thresholds changed that affect whether a program now applies? Have any new programs been enacted in a jurisdiction where the company already has employees?


An annual review misses changes that take effect mid-year, and 2026 supplied two clear examples. Washington's PFML premium rate rose to 1.13% of gross wages for 2026, up from 0.92% in 2025, split 28.57% employer and 71.43% employee, according to the[Washington Employment Security Department](https://esd.wa.gov/about-us/news-release/2025/paid-family-medical-leave-premium-rate-increases-113-2026) . Employers with fewer than 50 employees are not required to pay the employer share, but they still have to collect and remit the employee share, which means the rate change affects payroll configuration even for employers who owe none of the employer portion. Seattle's Payroll Expense Tax thresholds move every January 1 by CPI-U, per the[City of Seattle](https://www.seattle.gov/city-finance/business-taxes-and-licenses/seattle-taxes/payroll-expense-tax) , exactly the kind of change an annual setup, completed once at the start of a fiscal year, would not catch until it was already retroactive.


In Asure's experience, the payroll teams that maintain clean multi-jurisdiction compliance are not the ones running the most sophisticated software. They are the ones with the most disciplined review cadence, because jurisdictions change faster than any configuration can keep up with on its own.


Reporting cadence is its own discipline, separate from rate and threshold monitoring. New Jersey shows how easily two related obligations get conflated. Form NJ-927 (and NJ-927-W for weekly filers) reports quarterly gross income tax withholding, while Unemployment Insurance, Temporary Disability Insurance (TDI), Family Leave Insurance (FLI), and Workforce Development/Supplemental Workforce Fund (WF/SWF) contributions are reported separately, on Form WR-30 (Employer Report of Wages Paid), according to the[New Jersey Department of Labor and Workforce Development](https://www.nj.gov/labor/ea/employer-services/rate-info) . A quarterly audit that checks both filing obligations against the calendar catches a missed WR-30 filing before it becomes a penalty notice. An annual setup that only confirmed the initial configuration would not.


None of this requires more sophisticated technology to solve. It requires treating the jurisdiction list as something reviewed on a schedule, the same way a company reviews its bank reconciliation, rather than something configured once and left alone. See thequarterly jurisdiction audit procedure andcurrent WA PFML rates and NJ filing calendars .


## Bottom Line


Multi-jurisdiction payroll tax is a decision architecture problem, not a configuration problem. A single-state setup breaks the moment a second state enters the picture. Payroll programs split into three archetypes with three distinct failure modes. Local programs, not the highest-rate state and federal ones, generate the most retroactive liability for growth-stage companies. Remote work turned a stable, office-based problem into a dynamic, address-based one. And the companies that hold up over time run a quarterly audit rather than an annual setup. The better investment for growth-stage payroll leaders is a continuous decision process rather than a more complete rate table. For a team that wants to run that process internally, AsureCentral brings payroll, HR, tax, and compliance into one connected system built for growing employers managing this work themselves. For a team already running an established enterprise payroll system such as Workday, Oracle, or SAP that needs multi-jurisdiction filing and agency-notice tracking layered alongside that system rather than a replacement, Asure Payroll Tax Management fits that gap. For a team that wants Asure specialists tracking the jurisdictions, filings, and program changes directly, AsureWorks operates on that same platform, without co-employment and without giving up employer-of-record status.


## Related Questions


**Does Seattle have a payroll tax separate from Washington state payroll taxes?** Yes. Seattle's Payroll Expense Tax is a city-level tax charged to the employer based on total payroll expense, not an employee withholding, and it is separate from Washington's state-level programs like PFML and WA Cares. For 2026, it applies to employers with $9,074,409 or more in 2025 Seattle payroll expense, and at least one employee with 2026 compensation of $194,452 or more, with both thresholds adjusting every January 1 by CPI-U, per the[City of Seattle](https://www.seattle.gov/city-finance/business-taxes-and-licenses/seattle-taxes/payroll-expense-tax) . Because it is calculated on employer payroll expense rather than withheld from an employee's pay, it sits in a different configuration category than a deduction like PFML or WA Cares.


**What is the difference between SDI and SUI on a paycheck?** State Disability Insurance (SDI) is an employee-funded short-term disability program that exists in a handful of states, including California, New Jersey, New York, and Rhode Island, and typically appears as a deduction from the employee's pay. State Unemployment Insurance (SUI), sometimes called SUTA tax, is primarily an employer-funded program, though a few states, including New Jersey, also require an employee-side contribution. In New Jersey, both can appear on the same paycheck, the employee sees an SDI-related deduction and, separately, an employee-side unemployment insurance contribution, while the employer separately owes its own SUI contribution, per the[New Jersey Department of Labor and Workforce Development](https://www.nj.gov/labor/ea/employer-services/rate-info) .


**How do Indiana county income taxes work for remote employees?** Indiana has 92 counties, and each levies its own county income tax rate. The rate that applies to a given employee is based on that employee's county of residence as of January 1 of the tax year, or, if the employee resided out of state on that date, the county of their principal place of employment in Indiana, per the[Indiana Department of Revenue](https://www.in.gov/dor/files/dn01.pdf) . A mid-year move to a different Indiana county does not change withholding immediately; it updates at the next January 1 snapshot, so a payroll team needs to confirm and record each employee's residence as of that date.


**Who pays NJ SUI tax, the employer, the employee, or both?** New Jersey's Unemployment Insurance program is primarily employer-funded, but the state also requires an employee-side contribution alongside related Temporary Disability Insurance (TDI), Family Leave Insurance (FLI), and Workforce Development/Supplemental Workforce Fund (WF/SWF) contributions. For 2026, employee-side rates are 0.3825% for UI, 0.19% for TDI, 0.23% for FLI, and 0.0425% for WF/SWF, with a $44,800 taxable wage base for UI and WF/SWF and a $171,100 taxable wage base for TDI and FLI, per the[New Jersey Department of Labor and Workforce Development](https://www.nj.gov/labor/ea/employer-services/rate-info) . This split is not captured on Form NJ-927 or NJ-927-W, which report quarterly gross income tax withholding only; the UI, TDI, FLI, and WF/SWF split is reported separately on Form WR-30.


**What is WA Cares and how does it differ from Washington PFML?** The WA Cares Fund is a long-term care insurance program funded entirely through an employee payroll deduction, 0.58% of gross wages for 2026, with no wage cap; employees can apply for an exemption in specific circumstances, such as existing private long-term care coverage, per the[WA Cares Fund](https://wacaresfund.wa.gov/employers) . Washington's PFML program is a separate benefit funded through a split premium, 1.13% of gross wages for 2026, paid 28.57% by the employer and 71.43% by the employee, with no employee opt-out, per the[Washington Employment Security Department](https://esd.wa.gov/about-us/news-release/2025/paid-family-medical-leave-premium-rate-increases-113-2026) . Both apply to Washington employees and both appear as distinct payroll deductions, but they are administered separately, funded differently, and have different exemption rules.


**Do Ohio employers have to withhold local income taxes for employees working remotely from home?** Generally, yes. Under Ohio Revised Code 718.011, an employee's home is excluded from the definition of "worksite location," so the state's 20-day de minimis rule, which can excuse withholding for short-term work away from an employee's principal workplace, does not apply to remote work performed from home, per the[Ohio Revised Code](https://codes.ohio.gov/ohio-revised-code/section-718.011) . Ohio's temporary pandemic-era relief, which had allowed continued withholding based on an employee's pre-pandemic office location, ended January 1, 2022, making home-municipality withholding the default for remote and hybrid workers since then.


For related county and city payroll tax questions, see theFAQ hub for state and local payroll programs .


{


"@context": "https://schema.org",


"@type": "FAQPage",


"mainEntity": \[


{


"@type": "Question",


"name": "Does Seattle have a payroll tax separate from Washington state payroll taxes?",


"acceptedAnswer": {


"@type": "Answer",


"text": "Yes. Seattle's Payroll Expense Tax is a city-level tax charged to the employer based on total payroll expense, not an employee withholding, and it is separate from Washington's state-level programs like PFML and WA Cares. For 2026, it applies to employers with $9,074,409 or more in 2025 Seattle payroll expense, and at least one employee with 2026 compensation of $194,452 or more, with both thresholds adjusting every January 1 by CPI-U."


}


},


{


"@type": "Question",


"name": "What is the difference between SDI and SUI on a paycheck?",


"acceptedAnswer": {


"@type": "Answer",


"text": "SDI (State Disability Insurance) is an employee-funded short-term disability program in states including California, New Jersey, New York, and Rhode Island. SUI (State Unemployment Insurance) is primarily employer-funded, though states like New Jersey also require an employee-side contribution, and both can appear on the same paycheck with different employer/employee splits."


}


},


{


"@type": "Question",


"name": "How do Indiana county income taxes work for remote employees?",


"acceptedAnswer": {


"@type": "Answer",


"text": "Indiana's 92 counties each levy their own income tax rate, applied based on the employee's county of residence as of January 1 of the tax year. A mid-year move to a different Indiana county updates withholding only at the next January 1 snapshot, not immediately."


}


},


{


"@type": "Question",


"name": "Who pays NJ SUI tax, the employer, the employee, or both?",


"acceptedAnswer": {


"@type": "Answer",


"text": "New Jersey's Unemployment Insurance program is primarily employer-funded, but the state also requires an employee-side contribution alongside TDI, FLI, and WF/SWF contributions. For 2026, employee-side rates are 0.3825% for UI, 0.19% for TDI, 0.23% for FLI, and 0.0425% for WF/SWF, reported on Form WR-30, not Form NJ-927."


}


},


{


"@type": "Question",


"name": "What is WA Cares and how does it differ from Washington PFML?",


"acceptedAnswer": {


"@type": "Answer",


"text": "WA Cares is a long-term care insurance program funded entirely by an employee payroll deduction of 0.58% of gross wages for 2026, with no wage cap. Washington PFML is a separate paid family and medical leave program funded through a split premium of 1.13% of gross wages for 2026, paid 28.57% by the employer and 71.43% by the employee."


}


},


{


"@type": "Question",


"name": "Do Ohio employers have to withhold local income taxes for employees working remotely from home?",


"acceptedAnswer": {


"@type": "Answer",


"text": "Generally, yes. Under Ohio Revised Code 718.011, an employee's home is excluded from the definition of worksite location, so the 20-day de minimis rule does not apply to remote work from home. Ohio's pandemic-era withholding relief ended January 1, 2022, making home-municipality withholding the default for remote workers since then."


}


}


\]


}


### Related reading


Federal payroll tax obligations for growing employers |Payroll compliance for remote and distributed teams |Employer UI and SUI rate management and experience rating |PFML and SDI program administration


## Related posts


-


### [Onboarding Isn’t Paperwork—It’s a Launchpad for Growth](https://www.asuresoftware.com/onboarding-isnt-paperwork-its-a-launchpad-for-growth-2/)


-


### [Why Leadership Training Is the #1 Growth Differentiator](https://www.asuresoftware.com/why-leadership-training-is-the-1-growth-differentiator-2/)


-


### [Benefits & Deductions — How to Manage Payroll Deductions Accurately and Transparently](https://www.asuresoftware.com/benefits-deductions-how-to-manage-payroll-deductions-accurately-and-transparently-2/)


-


### [TOP HR Issues to Watch in 2014](https://www.asuresoftware.com/top-hr-issues-to-watch-in-2014/)


-


### [4 Ways to Control Employee Chaos](https://www.asuresoftware.com/4-ways-to-control-employee-chaos/)


-


### [How to Deal with Absenteeism](https://www.asuresoftware.com/how-to-deal-with-absenteeism/)
