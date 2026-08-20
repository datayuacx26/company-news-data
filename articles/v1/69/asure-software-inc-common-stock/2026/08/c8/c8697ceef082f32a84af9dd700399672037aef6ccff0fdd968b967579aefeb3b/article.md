---
schema_version: "1.0.0"
document_id: "c8697ceef082f32a84af9dd700399672037aef6ccff0fdd968b967579aefeb3b"
company_key: "asure-software-inc-common-stock"
company: "Asure Software Inc"
source_id: "asure-software-inc-common-stock-rss-2ceab48495f7"
canonical_url: "https://www.asuresoftware.com/how-to-manage-payroll-deductions-5-step-by-step-procedures-for-growing-companies/"
published_at: "2026-08-10T13:34:57+00:00"
first_seen_at: "2026-08-10T15:01:28.894037+00:00"
fetched_at: "2026-08-10T15:01:30.119683+00:00"
content_hash: "sha256:5f901a4c290eee279448613e16df979d1a6ea818d106b963e78977f6294c46fc"
---

# How to Manage Payroll Deductions, 5 Step-by-Step Procedures for Growing Companies

Getting mandatory vs voluntary payroll deductions right once is not the same as keeping them right as headcount grows.[Asure's mandatory vs. voluntary payroll deductions FAQ](https://www.asuresoftware.com/mandatory-vs-voluntary-payroll-deductions-frequently-asked-questions/) covers what counts as mandatory versus voluntary and why. This guide is the operational companion. It walks through the ordered steps for classifying a new deduction, calculating federal payroll tax withholdings correctly, setting up a voluntary deduction with proper authorization, and auditing what is already running in your payroll system before a misclassification compounds into a Form 941 discrepancy or a penalty notice.


This guide presents five procedures for managing mandatory and voluntary payroll deductions at a growth-stage company: how to classify a payroll deduction as mandatory or voluntary, how to calculate and remit FICA withholdings, how to calculate federal income tax withholding using the W-4, how to set up a voluntary deduction with employee authorization, and how to audit payroll deduction compliance. Each includes prerequisites, ordered steps, and an expected outcome.


## Classification


### How to Classify a Payroll Deduction as Mandatory or Voluntary


How to Classify a Payroll Deduction as Mandatory or Voluntary is the procedure for determining the legal status of any withholding before it is configured in payroll. It is executed by the payroll operator or HR lead during new hire onboarding or benefits setup and produces a documented deduction classification record. Use this procedure when adding any new deduction type to the payroll system.


**Prerequisites**


- Complete list of all current and proposed deductions for the pay period
- Access to federal and applicable state withholding requirement schedules
- Employee Form W-4 and state equivalent on file
- Benefits enrollment documentation for any proposed voluntary deductions


**Steps**


1. **List every deduction.** Compile all current and proposed withholdings into a single deduction register for the employee or employee class. This is the working document your mandatory payroll deductions list and voluntary deductions list will both be built from.
2. **Apply the legal requirement test.** For each deduction, determine whether a federal statute, state law, or court order mandates it. If yes, classify it as mandatory.
3. **Identify FICA components.** Flag Social Security (6.2%) and Medicare (1.45%) as mandatory employee withholdings, and note the corresponding employer match obligation.
4. **Identify income tax withholdings.** Flag federal income tax, per Form W-4, state income tax, and any local income tax as mandatory based on the employee's work and residence jurisdictions.
5. **Flag court-ordered deductions.** Identify any active garnishment orders, child support, tax levy, or creditor garnishment, and classify them as mandatory upon receipt of the withholding order.
6. **Classify remaining deductions as voluntary.** Any deduction not covered by steps two through five, including health insurance premiums, 401(k) contributions, FSA elections, and supplemental insurance, is voluntary and requires written employee authorization.
7. **Document and store the classification record.** Record each deduction's classification, legal basis, and effective date in the employee's payroll file.


**Expected outcome:** A complete deduction classification register for the employee, with every withholding labeled mandatory (with legal citation) or voluntary (with authorization reference), ready for payroll system configuration.


**When to use, and when not to:** Use this procedure at new hire setup, when adding a new benefit, or when a court order is received. Do not use it as a substitute for legal counsel when a deduction's classification is genuinely ambiguous under state law.


**Common pitfalls**


- Treating employer-only taxes as employee deductions. FUTA and state unemployment tax never appear on the employee's paycheck, so flag them in a separate employer-obligation register instead of the deduction register.
- Skipping state and local tax research. Multi-state employees require jurisdiction-by-jurisdiction classification, and a single federal classification is not sufficient.


AsureWorks specialists begin every new client's payroll setup with this classification step, before any calculation or configuration happens, to prevent miswithholding errors before the first payroll run.


## Calculation


### How to Calculate and Remit FICA Withholdings


How to Calculate and Remit FICA Withholdings is the procedure for computing the employee Social Security and Medicare deductions and the matching employer contributions for each pay period. It is executed by the payroll operator each pay cycle and produces a FICA liability entry and a timely deposit to the IRS. Use this procedure for every U.S. employee payroll run.


**Prerequisites**


- Employee gross wages for the pay period confirmed
- Year-to-date gross wages per employee on file, to track the Social Security wage base
- Current IRS FICA rates and the 2026 Social Security wage base of $184,500 ([IRS Publication 926, "Household Employer's Tax Guide," for use in 2026](https://www.irs.gov/pub/irs-pdf/p926.pdf) )
- IRS EFTPS account active and employer EIN confirmed


**Steps**


1. **Confirm gross wages.** Pull the confirmed gross pay figure for each employee for the current pay period before any deductions.
2. **Check year-to-date wages against the Social Security wage base.** If an employee's year-to-date wages have reached or will exceed $184,500 for 2026, cap Social Security withholding at the wage base limit for that employee.
3. **Calculate employee Social Security withholding.** Multiply eligible gross wages by 6.2% to determine the employee's Social Security deduction.
4. **Calculate employee Medicare withholding.** Multiply gross wages by 1.45%, with no wage base cap, and apply the additional 0.9% Additional Medicare Tax to wages exceeding $200,000 in the calendar year. This threshold has been unchanged since 2013 and carries no employer match ([IRS Topic no. 560, "Additional Medicare Tax"](https://www.irs.gov/taxtopics/tc560) ).
5. **Calculate the employer FICA match.** Compute the employer's matching 6.2% Social Security and 1.45% Medicare contributions. These are employer costs, not employee deductions, a distinction at the center of employee deductions vs employer contributions.
6. **Record both employee and employer FICA amounts.** Enter employee withholdings as payroll liabilities and employer contributions as payroll tax expenses in the general ledger.
7. **Aggregate FICA deposits by deposit schedule.** Combine employee withholdings and the employer match into the total FICA deposit amount, and apply the employer's monthly or semi-weekly deposit schedule based on its lookback period. Employers depositing $50,000 or less in employment taxes during the lookback period are monthly depositors, due the 15th of the following month. Employers depositing more than $50,000 are semi-weekly depositors. Any employer that accumulates $100,000 or more in a single day must deposit by the next business day and becomes a semi-weekly depositor for the rest of that year and all of the next ([IRS Tax Topic 757, "Forms 941 and 944, Deposit Requirements"](https://www.irs.gov/taxtopics/tc757) ).
8. **Remit through EFTPS by the deposit due date.** Submit the FICA deposit through the Electronic Federal Tax Payment System before the applicable deadline to avoid the IRS failure-to-deposit penalty.


**Expected outcome:** FICA withholdings correctly deducted from employee pay, employer match calculated, total FICA liability recorded, and deposit submitted to the IRS on schedule with a confirmation number retained.


**When to use, and when not to:** Use this procedure every pay cycle for all W-2 employees. Do not apply it to 1099 independent contractors, who are responsible for their own self-employment tax.


**Common pitfalls**


- Missing the Social Security wage base cutoff mid-year. Failing to stop withholding at $184,500 results in over-withholding and an employee refund obligation.
- Treating FICA and federal income tax withholding as one blended figure. Form 941 requires both reported as distinct line items, and mixing them up creates reconciliation errors at quarter end.


In AsureCentral, FICA calculation runs automatically once wage and deposit-schedule data are configured, which reduces the manual wage-base tracking that becomes harder to manage by hand as headcount and pay frequency scale.


### How to Calculate Federal Income Tax Withholding Using the W-4


How to Calculate Federal Income Tax Withholding Using the W-4 is the procedure for determining the correct federal income tax to withhold from each employee's paycheck based on current Form W-4 elections. It is executed by the payroll operator each pay cycle and produces a per-employee federal withholding amount. Use this procedure for every active W-2 employee.


**Prerequisites**


- Current Form W-4 (2020 or later version) on file for the employee
- Employee's gross wages for the pay period confirmed
- IRS Publication 15-T for use in 2026, the federal income tax withholding tables ([IRS Publication 15-T](https://www.irs.gov/publications/p15t) )
- Payroll period type confirmed (weekly, biweekly, semi-monthly, or monthly)


**Steps**


1. **Retrieve the employee's current Form W-4.** Confirm the most recent W-4 is on file. If the employee has not submitted a 2020 or later W-4, apply the default single filing status with no adjustments, per IRS guidance.
2. **Identify the withholding method.** Select either the Percentage Method or the Wage Bracket Method from Publication 15-T, based on payroll system capability and pay period type. Publication 15-T also includes a computational bridge for employees still on a pre-2020 W-4.
3. **Adjust gross wages for Step 4(b) deductions.** Subtract any pre-tax deductions the employee claimed on W-4 Step 4(b), such as student loan interest or IRA contributions, from gross wages to arrive at the adjusted wage amount.
4. **Apply the withholding table.** Using the adjusted wage amount, payroll period, and filing status from the W-4, look up or calculate the tentative withholding amount from Publication 15-T.
5. **Add any additional withholding.** If the employee entered an additional flat dollar amount on W-4 Step 4(c), add it to the tentative withholding amount.
6. **Reduce for any tax credits claimed.** Apply any child tax credit or dependent care credit amounts the employee entered on W-4 Step 3 to reduce the withholding amount. It cannot go below zero.
7. **Record the final withholding amount.** Enter the calculated federal income tax withholding in the payroll system for the current pay period, and retain the calculation basis in the payroll audit log.


**Expected outcome:** A documented, per-employee federal income tax withholding amount for the pay period, calculated per Publication 15-T, ready for payroll processing and Form 941 reporting.


**When to use, and when not to:** Use this procedure every pay cycle for W-2 employees, and update the calculation immediately when an employee submits a new W-4. Do not continue using a superseded W-4 after the employee has submitted a replacement.


**Common pitfalls**


- Applying pre-2020 allowance-based logic to a post-2020 W-4. Publication 15-T's computational bridge exists precisely because mixing old and new logic produces systematic under-withholding or over-withholding.
- Delaying an update when a new W-4 arrives. The IRS requires employers to implement a new W-4 promptly rather than carrying forward a superseded form indefinitely.


Asure HR Compliance specialists review W-4 configurations and withholding table versions as a standard step in a broader payroll compliance review for growth-stage companies, catching version mismatches before they reach a full pay cycle.


## Setup


### How to Set Up a Voluntary Deduction with Employee Authorization


How to Set Up a Voluntary Deduction with Employee Authorization is the procedure for establishing a new pre-tax or post-tax voluntary withholding, such as a 401(k) contribution, health insurance premium, or FSA election, in the payroll system with proper written consent. It is executed by HR or payroll operations during open enrollment or a qualifying life event and produces an authorized, configured deduction. Use this procedure when adding any benefit-related withholding.


**Prerequisites**


- Benefits plan documents or carrier invoices confirming the deduction amount and type
- Completed employee authorization form (salary reduction agreement or benefits enrollment form), signed and dated
- Determination of pre-tax versus post-tax status (Section 125 cafeteria plan, 401(k) elective deferral, or after-tax)
- Payroll system access to create or modify deduction codes


**Steps**


1. **Obtain signed employee authorization.** Collect a completed, dated authorization form specifying the deduction type, amount or percentage, effective date, and pre-tax or post-tax designation, before any withholding begins.
2. **Verify plan eligibility.** Confirm the employee meets the plan's eligibility criteria, such as a minimum hours requirement or waiting period, before activating the deduction.
3. **Determine tax treatment.** Classify the deduction as pre-tax, which reduces federal taxable wages under a Section 125 or 401(k) arrangement, or post-tax, deducted after tax calculation, and configure it accordingly.
4. **Create or activate the deduction code.** Enter the deduction in the payroll system with the correct code, amount, frequency, and start date matching the authorization form.
5. **Confirm the deduction on the next payroll preview.** Run a payroll preview or register report to verify the deduction amount, tax treatment, and effective date before finalizing the pay run.
6. **File the authorization form.** Store the signed authorization in the employee's benefits file, physical or digital, and retain it per the company's document retention policy.


**Expected outcome:** A configured, authorized voluntary deduction active in the payroll system for the correct effective date, with the signed authorization form on file and the deduction visible among the employee's voluntary deductions from paycheck on the next pay stub.


**When to use, and when not to:** Use this procedure for every new voluntary deduction, including mid-year additions triggered by a qualifying life event. Do not activate a voluntary deduction without a signed authorization form. Doing so creates wage deduction liability under state labor laws.


**Common pitfalls**


- Starting withholding before the authorization form is signed. Even one pay period of unauthorized deduction can trigger a state labor board complaint.
- Misclassifying a pre-tax deduction as post-tax. This overstates the employee's taxable wages, causing over-withholding of federal income tax and FICA.


AsureWorks specialists coordinate benefits enrollment and payroll deduction setup together, so a signed authorization form and a correctly configured deduction code land in the same pay period rather than trailing it.


## Compliance Audit


### How to Audit Payroll Deduction Compliance


Of the five procedures in this guide, this audit is the one that matters most. It catches classification, calculation, and setup errors before they surface on a Form 941, a W-2, or a state filing.


How to Audit Payroll Deduction Compliance is the procedure for systematically reviewing all active employee deductions and employer tax obligations to identify miswithholding, missing authorizations, or deposit timing errors before they compound into penalties. It is executed by the payroll operator or HR compliance lead quarterly and produces a deduction audit report with findings and remediation actions. Use this procedure at least once per quarter.


**Prerequisites**


- Payroll register for the audit period, covering all pay periods in scope
- Employee W-4s and voluntary deduction authorization forms on file
- IRS deposit confirmation records (EFTPS payment history)
- Current FICA rates, federal income tax withholding tables, and state tax schedules
- Prior quarter Form 941 for reconciliation reference


**Steps**


1. **Pull the payroll register for the audit period.** Export a complete payroll register showing gross wages, each deduction line item, net pay, and employer tax amounts for every employee across all pay periods in scope.
2. **Verify mandatory deduction rates against current IRS schedules.** Confirm Social Security (6.2%), Medicare (1.45%), and federal income tax withholding rates match the current Publication 15 and 15-T tables.
3. **Reconcile W-4 elections to actual withholding.** For a representative sample of employees, compare the W-4 filing status and elections to the actual federal income tax withheld per pay period, and flag discrepancies.
4. **Confirm Social Security wage base compliance.** Identify any employee whose year-to-date wages exceeded the 2026 wage base of $184,500, and verify that Social Security withholding stopped at the correct pay period.
5. **Audit voluntary deduction authorizations.** For every active voluntary deduction, confirm a signed authorization form exists, the deduction amount matches the form, and the pre-tax or post-tax classification is correct.
6. **Verify the employer FICA match and FUTA amounts.** Confirm the employer's FICA match equals the employee FICA withheld, and that FUTA is calculated on an employer-side register only, never deducted from employee wages.
7. **Reconcile EFTPS deposit history to Form 941 liability.** Compare total FICA and federal income tax deposits made through EFTPS to the tax liability reported on the most recent Form 941, and investigate any variance.
8. **Document findings and assign remediation actions.** Produce a written audit report listing each finding, the affected employees, the dollar variance, and the corrective action with a due date. Escalate material errors to a tax advisor.


**Expected outcome:** A completed deduction audit report identifying any miswithholding, missing authorizations, or deposit discrepancies, with documented remediation actions and a clean sign-off record for the audit period.


**When to use, and when not to:** Use this procedure quarterly as a standard compliance control, and run it immediately after any payroll system migration, HRIS change, or major headcount event that could introduce configuration errors.


**Common pitfalls**


- Auditing only employee deductions and ignoring employer-side taxes. FUTA errors are invisible in the employee payroll register and require a separate employer tax ledger review.
- Treating the audit as a one-time event. Deduction errors compound across pay periods, and a quarterly cadence catches them before they appear on annual W-2s or trigger an IRS notice.


AsureWorks specialists and Asure HR Compliance professionals run this type of deduction audit as a recurring part of managed payroll and HR support for growth-stage clients, rather than treating it as a one-time fix.


## How to Sequence These Procedures


Start every new payroll setup with Classification. No calculation or configuration is valid until every deduction is correctly labeled mandatory or voluntary. Once classification is complete, run FICA Calculation and Federal Income Tax Withholding in parallel for mandatory deductions, then complete Voluntary Deduction Setup for each authorized benefit. After the first full payroll run, schedule a Compliance Audit within 30 days to catch configuration errors before they compound.


For ongoing operations, repeat FICA and federal income tax calculation every pay cycle, run Voluntary Deduction Setup at each open enrollment period or qualifying life event, and run the Compliance Audit quarterly. If a payroll system migration or a major headcount event occurs, re-run Classification and the Compliance Audit immediately, regardless of the regular schedule.


## Apply the Compliance Audit


Treat the first audit as a lightweight go-live check, not the full quarterly review. Run it within 30 days of any new payroll system go-live to catch configuration errors, a missed classification, a wrong FICA setup, a missing authorization, before they compound across multiple pay periods. The full audit described above, reconciling a complete pay period's data against EFTPS deposits and Form 941, needs at least one full quarter of payroll history to run properly, and that fuller review is the cadence to repeat going forward.


If your team is running this process by hand and headcount keeps outpacing your bandwidth, AsureWorks specialists can run payroll and manage day-to-day HR administration, including this kind of audit, while you remain the employer of record. Asure HR Compliance professionals can also support a standalone review of your deduction setup. Talk to Asure about which model fits where your payroll process stands today.


## Related Questions


**Does FUTA come out of an employee's paycheck?**


No. FUTA is an employer-only tax, paid entirely by the employer on the first $7,000 of each employee's wages, at a rate of up to 6.0%, typically reduced to an effective 0.6% for employers who pay state unemployment tax on time ([IRS Publication 926, for use in 2026](https://www.irs.gov/pub/irs-pdf/p926.pdf) ). It is never deducted from employee wages or shown on a pay stub.


**What are the two main types of payroll deductions?**


The two main types are mandatory deductions, legally required withholdings including federal, state, and local income taxes and FICA (Social Security and Medicare), and voluntary deductions, which include benefit-related withholdings such as health insurance premiums, 401(k) contributions, and FSA elections that require written employee authorization.


**What is the difference between employee deductions and employer contributions?**


Employee deductions are amounts withheld from an employee's gross wages, such as their share of FICA, federal income tax, and health insurance premiums. Employer contributions are separate costs the employer pays on top of wages, such as the employer FICA match and FUTA. Employer contributions do not reduce employee net pay.


**Which of the following is not a payroll deduction, FUTA, Social Security, Medicare, or federal income tax?**


FUTA is not a payroll deduction from employee wages. Social Security, Medicare, and federal income tax are all withheld from employee paychecks. FUTA is an employer-only tax, calculated and paid by the employer separately, and it never appears as a line item on an employee's pay stub or W-2.


**Are payroll taxes mandatory for employees?**


Yes. Federal law requires employers to withhold Social Security (6.2%), Medicare (1.45%), and federal income tax from every eligible employee's wages ([IRS Publication 926, for use in 2026](https://www.irs.gov/pub/irs-pdf/p926.pdf) ). State and local income tax withholding is mandatory in jurisdictions that impose those taxes. Employees generally cannot opt out of mandatory payroll tax withholding.


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
