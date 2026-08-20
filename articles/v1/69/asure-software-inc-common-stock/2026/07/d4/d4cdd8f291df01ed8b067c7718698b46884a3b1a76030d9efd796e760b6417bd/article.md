---
schema_version: "1.0.0"
document_id: "d4cdd8f291df01ed8b067c7718698b46884a3b1a76030d9efd796e760b6417bd"
company_key: "asure-software-inc-common-stock"
company: "Asure Software Inc"
source_id: "asure-software-inc-common-stock-rss-2ceab48495f7"
canonical_url: "https://www.asuresoftware.com/payroll-tax-is-not-income-tax-and-growth-stage-companies-keep-getting-the-distinction-wrong/"
published_at: "2026-07-09T15:27:45+00:00"
first_seen_at: "2026-07-24T17:11:02.757338+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:a6ed3265ea2a23bdbbfecb0c415ca1cbce46f312b2585eec1c3096e8b5a96b29"
---

# Payroll Tax Is Not Income Tax, And Growth-Stage Companies Keep Getting The Distinction Wrong

Payroll tax and income tax withholding are not two names for the same obligation. They are structurally different liabilities with different owners, different IRS forms, and different consequences when misclassified. In Asure's work with growth-stage companies, conflating the two is the most common source of paystub errors, 940/941 misrouting, and employer-share omissions. Here is the pattern.


## Payroll tax and income tax withholding have different owners and that difference is the whole game


Every payroll cycle produces two categories of federal tax obligation that look similar on paper but behave completely differently in practice. Payroll tax, meaning Federal Insurance Contributions Act (FICA) tax and Federal Unemployment Tax Act (FUTA) tax, carries employer liability. Income tax withholding does not.


FICA funds Social Security and Medicare, and it is split between employee and employer. Under the current rate structure, the employee pays 6.2% toward Social Security and the employer matches that with another 6.2%, for a combined 12.4% up to the taxable wage base. For 2026, that wage base is $184,500, up from $176,100 in 2025, per the IRS (as of 2026). Medicare works the same way structurally, 1.45% from the employee and 1.45% from the employer, combined 2.9%, but with no wage cap. Employees earning more than $200,000 also owe an additional 0.9% Medicare surtax, and that surtax has no employer match at all. FUTA is different still. It is entirely employer-paid. The employee never sees a FUTA deduction on a paystub because there isn't one.


Federal income tax withholding operates on a different principle entirely. The employer withholds a portion of the employee's wages based on the employee's Form W-4 elections and remits that amount to the IRS on the employee's behalf. No employer match exists. No separate employer cost accrues. The employer is functionally a collection agent for a liability that belongs entirely to the employee.


This is the distinction that growth-stage finance teams miss most often, and it produces what is best described as the employer-share blindspot. A company building its first payroll process typically models the employee-side deductions correctly, because those numbers show up cleanly on every paystub and every payroll register. What gets missed is that the employer's FICA match is not a pass-through line item. It is a separate cash liability the business owes on top of gross wages, and it needs its own line in the company's cash forecasting, not just in the payroll software's back-end calculation.


In our work setting up payroll for growth-stage companies, the first question Asure asks a new client is whether the finance lead can identify which paystub line items represent employer cost versus employee deduction. The answer is almost always no, not because the information is hidden, but because most payroll tools present it as an aggregated afterthought rather than a distinct liability category. That gap between employee deduction and **employer FICA match** is not cosmetic. It is the seam where budgeting errors, understated labor cost, and downstream tax filing mistakes originate.


The ownership distinction is foundational, but it only becomes a compliance problem when it maps incorrectly onto IRS forms. That is where the second failure pattern lives.


## The 940/941 misrouting error is the costliest consequence of the confusion


Once a growth-stage company treats payroll tax and income tax withholding as a single undifferentiated category of "federal taxes," the next failure point is procedural. The IRS does not ask employers to file one federal payroll tax form. It requires two, and they carry entirely different obligations.


Form 941, the Employer's Quarterly Federal Tax Return, reports Social Security and Medicare tax (FICA) together with federal income tax withheld from employees, all in the same quarterly filing. Form 940, the Employer's Annual Federal Unemployment (FUTA) Tax Return, is a different animal entirely. It reports FUTA tax exclusively, and FUTA is paid solely by the employer. Per the IRS instructions for Form 940, "only employers pay FUTA tax," and it is never collected or deducted from employee wages. The FUTA rate is 6.0% on the first $7,000 paid to each employee annually, though most employers receive a credit of up to 5.4% for timely state unemployment tax payments, bringing the effective rate down to roughly 0.6% (IRS.gov, Instructions for Form 940).


The 940/941 misrouting error happens when a growth-stage company, working from a mental model of "federal payroll taxes" as one bucket, either folds FUTA calculations into its quarterly 941 process or fails to track FUTA liability separately until the annual Form 940 comes due and the number does not reconcile. This is not a hypothetical edge case. It is the direct, predictable outcome of not knowing that Form 941 is quarterly and FICA-plus-withholding, while Form 940 is annual and FUTA-only. A company can operate for several quarters with this confusion baked into its process before anyone notices the mismatch, because the error does not surface on any single filing. It surfaces when someone finally reconciles the year.


The cost of getting this wrong is not abstract. The IRS applies a tiered failure-to-deposit penalty structure: 2% for deposits one to five days late, 5% for six to fifteen days late, 10% for deposits sixteen or more days late (or for amounts paid within 10 days of the first IRS notice), and 15% if the amount remains unpaid more than 10 days after the IRS issues an immediate-payment notice or a notice and demand for payment. These tiers do not stack. A deposit that is more than 15 days late is penalized at 10% total, not the sum of the lower tiers (IRS.gov, Failure to Deposit Penalty). A misrouted or underreported FUTA liability that surfaces late in an audit cycle can trigger exactly this kind of penalty exposure, on top of the administrative cost of amending prior filings.


Asure treats 940/941 misrouting as a first-pass audit item in every growth-stage payroll setup Asure reviews, precisely because it is the failure mode most likely to have compounded quietly across multiple quarters before anyone catches it. On the AsureCentral platform, the payroll register separates FUTA liability from FICA and withholding by form and by owner from the first pay run, so the two obligations never blur into a single "federal taxes" bucket in the first place. The fix is not more diligence at filing time. It is knowing, from the first payroll run, that federal unemployment tax and FICA tax live on different forms with different cadences and different owners. Understanding **how to file Form 940 and Form 941** correctly starts with treating them as two separate compliance obligations, not variations on the same task.


The form-level error is visible in the filings themselves, once someone knows to look. The third failure pattern is less visible. It does not show up until W-2 season.


## W-2 box misallocation is the downstream artifact of upstream classification confusion


It is tempting to treat W-2 errors as a year-end problem, something that happens because someone rushed the annual reconciliation. In practice, W-2 box misallocation is rarely a year-end mistake. It is a payroll-setup mistake that simply does not become visible until the W-2 gets generated.


Form W-2 separates wage and tax information into distinct boxes that correspond to the same ownership distinction discussed above. Box 1 reports federal taxable wages and the federal income tax withheld from those wages. Boxes 3 and 5 report Social Security wages and Medicare wages respectively, the wage bases used to calculate FICA. Boxes 4 and 6 report the FICA actually withheld, Social Security tax in Box 4 and Medicare tax in Box 6. These are not interchangeable figures reporting the same number four different ways. The wage base used for income tax withholding purposes and the wage base used for FICA purposes are not always identical.


The reason they diverge is pre-tax benefits. Certain benefit elections, such as contributions to a traditional retirement plan or specific cafeteria-plan benefits, can reduce the wage figure used for one tax calculation without reducing the wage figure used for the other, depending on how that particular benefit is treated under the tax code. The general mechanism matters more than any single example: a benefit that lowers taxable wages for income tax withholding purposes does not automatically lower the wage base used for Social Security and Medicare, and the reverse can also be true depending on the benefit type. When a payroll system is configured incorrectly at setup, meaning a benefit is coded as reducing the wrong wage base, or the same wage base across both categories, that error runs quietly through every single pay cycle for a full year before the W-2 makes it visible.


In our work reviewing growth-stage payroll setups, Asure consistently finds that W-2 corrections filed in February trace back to a payroll setup decision made in January of the prior year. The classification was wrong from day one, and twelve months of payroll ran on that flawed foundation before Box 1 and Box 3 stopped matching up in a way that finally forced someone to look. This is the practical cost of treating payroll tax and income tax withholding as synonymous at the point of setup rather than as two parallel calculations with two different wage bases. Reviewing **W-2 Box 1 vs Box 3 wages** side by side, rather than assuming they should match, is often the fastest way to catch a classification error before it compounds across another filing year.


These three failure patterns, the employer-share blindspot, the 940/941 misrouting error, and W-2 box misallocation, share a root cause. Payroll systems surface a single "taxes" line on the paystub, and that visual compression hides the structural distinction that practitioners actually need to see.


## Paystub compression is the interface problem that sustains the confusion


Most payroll platforms are designed to make the employee-facing paystub as simple as possible, and that design choice makes sense from the employee's point of view. An employee generally needs to know gross pay, total deductions, and net pay. Aggregating FICA withholding, federal income tax withholding, and state income tax withholding into a single "Taxes" or "Deductions" block accomplishes that goal cleanly.


The problem is that this same aggregated view is often the only view a growth-stage company's finance or HR team ever looks at, and it is the wrong document for compliance verification. A single "Taxes" line cannot tell a payroll administrator which portion of that deduction is FICA, which portion is federal income tax withholding, which portion is state income tax withholding, or which of those categories has a matching employer liability sitting elsewhere in the ledger. This pattern is best described as paystub compression, a deliberate simplification for the employee that becomes an obstacle for anyone responsible for compliance.


The correct tool for compliance review is not the paystub. It is the payroll register, sometimes called the employer-facing payroll report, which disaggregates every tax type by category, by IRS form, and by which party (employer or employee) owns the liability. A well-structured payroll register shows FICA employee withholding and FICA employer match as two separate line items, shows FUTA as a distinct employer-only liability, and shows federal and state income tax withholding as employee pass-through amounts. This is the document that should drive quarterly and annual reconciliation, not the paystub, because it is the only one built to answer the ownership question rather than the take-home-pay question. AsureCentral's payroll reporting separates each tax type and each owner within the platform, so a growth-stage finance lead running payroll in-house is not left reverse-engineering ownership from a compressed paystub.


For companies that would rather have that register review done for them, Asure builds a mandatory employer-register review into the first several pay cycles of every AsureWorks managed payroll engagement, for exactly this reason, not because the paystub is inaccurate, but because the paystub was never designed to answer a compliance question. Building a **payroll register vs paystub compliance review framework** into the first weeks of a new payroll setup, whether that review is run internally on AsureCentral or handled by AsureWorks specialists, catches classification errors while they are still a one-cycle fix rather than a twelve-month correction.


For growth-stage companies adding states, the paystub compression problem does not stay contained. It multiplies. State income tax withholding and state unemployment tax introduce additional liability layers that the federal FICA-versus-income-tax distinction does not fully prepare a company to handle.


## Multi-state expansion turns the distinction into a multi-variable compliance problem


Hiring a first remote employee in a new state introduces two separate compliance questions that growth-stage companies routinely try to answer with a single rule, and that approach fails immediately. The two questions are which state gets income tax withholding, and which state gets the unemployment tax payment, generally referred to as State Unemployment Tax Act (SUTA) tax.


These are not the same question, and they do not always have the same answer. State income tax withholding generally follows the employee's work-location state, the state where the employee physically performs the work, subject to that state's own withholding rules and any reciprocity agreements between neighboring states. State unemployment tax follows a different logic. SUTA is paid to the state where the employee is covered under the employer's state unemployment insurance account, and that state of coverage can differ from the employee's work-location state depending on the employer's registration and the specific facts of the work arrangement. A company that applies its FICA-versus-federal-income-tax mental model to the state level, assuming that wherever it withholds income tax is automatically also where it owes SUTA, can get the unemployment registration wrong without ever realizing the two obligations were never linked in the first place.


Complicating the picture further, nine states currently levy no state individual income tax at all: Alaska, Florida, Nevada, New Hampshire, South Dakota, Tennessee, Texas, Washington, and Wyoming (Tax Foundation, 2026 State Income Tax Rates and Brackets). An employer hiring in one of these states has no state income tax withholding obligation to resolve, but that does not eliminate the SUTA question, which remains a separate registration decision regardless of whether the state taxes income. The factors that actually determine the right answer, in general terms, are the employee's work-location state, the employer's nexus in that state, any applicable reciprocity agreements, and whether the state taxes individual income in the first place.


In our multi-state setup process, Asure treats work-location state, nexus, and unemployment coverage as three separate determinations rather than one combined judgment call, which is what prevents a registration error before the first payroll cycle in that state ever runs. A documented **multi-state payroll setup process** that treats income tax withholding and **SUTA vs FUTA** obligations as independent decisions, rather than variations on the same federal-tax logic, is what keeps expansion from turning into a compliance backlog, whether that process runs on AsureCentral or is handled through AsureWorks.


## Bottom line


Three failure patterns, the employer-share blindspot, the 940/941 misrouting error, and W-2 box misallocation, are not separate problems. They are the compounding result of treating payroll tax and income tax withholding as synonyms rather than as structurally distinct liabilities with different owners and different forms. Paystub compression and multi-state expansion are the two operational contexts where that confusion accelerates fastest, because both hide the ownership distinction behind a single aggregated number or a single mental model applied at the wrong scale.


Growth-stage companies do not need a vocabulary lesson. They need a payroll setup process that disaggregates liability ownership from day one, before the first Form 941 is filed and before the first remote hire crosses a state line. Asure offers two paths to that same disaggregation, built on the same underlying system. Teams that want to keep running payroll internally get it natively on AsureCentral, where the payroll register separates FUTA, FICA, and withholding by form and by owner from the first pay run, so the finance lead sees ownership clearly instead of reverse-engineering it from a compressed paystub. Teams that would rather have that work done for them get that same disaggregation as a managed service through AsureWorks, where the employer-register review, the 940/941 first-pass audit, and the multi-state setup review are the operational expression of the distinction this piece has named. AsureWorks is a managed service and PEO alternative with no co-employment, and the client remains the employer of record throughout, whichever path a company chooses.


## Related questions


**Is FICA the same as federal income tax withholding?** No. FICA, Social Security and Medicare tax, is a payroll tax with an employer match, while federal income tax withholding is an employee-only pass-through with no employer cost. Both amounts appear together on Form 941, which is the source of most of the confusion, but co-location on the same quarterly form does not make them the same liability.


**Is FICA reported on Form 940 or Form 941?** FICA is reported on Form 941, filed quarterly. Form 940 is used exclusively for FUTA, the federal unemployment tax, which is a separate, annual, employer-only obligation. Treating FUTA and FICA as interchangeable because both are "federal payroll taxes" is precisely the 940/941 misrouting error described above.


**Are payroll taxes included in an employee's salary or wages?** Payroll taxes are calculated based on wages, but they are not part of the wage figure itself. The employee-side FICA amount is deducted from gross pay, while the employer's FICA match, FUTA, and any SUTA obligation are additional employer costs layered on top of gross wages, not portions of the employee's stated salary.


**Is FUTA the same as federal income tax withholding?** No. FUTA is paid entirely by the employer, with no employee-side component at all, while federal income tax withholding is strictly an employee obligation that the employer collects and remits on the employee's behalf. Structurally, the two sit at opposite ends of the ownership spectrum, one fully employer-owned and one fully employee-owned.


**Does state income tax withholding work the same way as state payroll tax, SUTA?** No. State income tax withholding generally follows the employee's work-location state and is an employee obligation, while SUTA is an employer-only tax tied to the state where the employer maintains unemployment coverage for that employee, which can be a different state. Reciprocity agreements between states and the nine states with no individual income tax add further variation that a growth-stage company needs to check state by state rather than assume.


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
