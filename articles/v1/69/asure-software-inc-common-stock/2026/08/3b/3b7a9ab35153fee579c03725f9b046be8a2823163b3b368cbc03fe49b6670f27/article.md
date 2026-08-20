---
schema_version: "1.0.0"
document_id: "3b7a9ab35153fee579c03725f9b046be8a2823163b3b368cbc03fe49b6670f27"
company_key: "asure-software-inc-common-stock"
company: "Asure Software Inc"
source_id: "asure-software-inc-common-stock-rss-2ceab48495f7"
canonical_url: "https://www.asuresoftware.com/futa-tax-deposit-and-form-940-payment-procedures-for-growth-stage-payroll-teams/"
published_at: "2026-08-10T13:35:02+00:00"
first_seen_at: "2026-08-10T15:01:28.894037+00:00"
fetched_at: "2026-08-10T15:01:30.119683+00:00"
content_hash: "sha256:146845965bd35066d0c3ed4336fa1fa86999248480d01e8aadea04702d95a144"
---

# FUTA Tax Deposit and Form 940 Payment Procedures for Growth-Stage Payroll Teams

This guide sequences five procedures for FUTA (Federal Unemployment Tax Act) tax deposits and Form 940, the IRS's annual FUTA return, in the exact order a payroll operator executes them across the quarterly and annual cycle: calculate quarterly liability, evaluate the $500 deposit threshold, deposit via EFTPS (the IRS's Electronic Federal Tax Payment System) or carry the balance forward, track cumulative carryforward, and pay the annual Form 940 balance due. Each procedure lists prerequisites, steps, and outcomes.


AsureCentral supports this exact sequence, with the wage base cap, credit calculation, deposit thresholds, and deadline tracking built into the platform, so quarterly deposits and the annual Form 940 filing reconcile against one record instead of a spreadsheet you rebuild every quarter. Growth-stage teams can run this playbook themselves inside AsureCentral, or hand execution to Asure specialists through AsureWorks, the same platform operated on your behalf with no co-employment and your company remaining the employer of record. Either way, the next step is the same: run the first procedure below at your next quarter close.


## Quarterly Deposit Workflow


Run these three procedures at the close of every calendar quarter, in this order.


### How to Calculate Quarterly FUTA Liability


How to Calculate Quarterly FUTA Liability is the procedure for determining your FUTA tax obligation for a single calendar quarter. Execute it at the close of each quarter to produce a verified net liability figure, the required input for the threshold evaluation that follows.


**Prerequisites**


- **Quarterly payroll register** : gross wages paid to every employee during the quarter, exported from your payroll system.
- **Year-to-date wages per employee** : needed to identify who has already crossed the $7,000 FUTA wage base for the year.
- **Current FUTA rate** : 6.0% gross on the first $7,000 of each employee's wages per year, reducible to an effective 0.6% net rate for employers who qualify for the full state credit ([IRS.gov, Instructions for Form 940](https://www.irs.gov/instructions/i940) ).
- **Current credit-reduction state list** : published via Schedule A (Form 940) each year; confirm it before applying the credit in step five.


**Steps**


1. **Pull the quarterly payroll register** : Export gross wages paid to all employees during the quarter from your payroll system.
2. **Apply the $7,000 wage base cap** : For each employee, subtract any wages already taxed above $7,000 year-to-date, so only eligible wages remain.
3. **Sum eligible wages** : Total the capped taxable wages across all employees to get the quarter's FUTA wage base.
4. **Apply the 6.0% rate** : Multiply the FUTA wage base by 0.06 to produce the gross quarterly FUTA liability.
5. **Apply the available credit** : Subtract the State Unemployment Tax Act (SUTA) credit, up to 5.4%, if your state is not on the current credit-reduction list, bringing the effective rate down to as low as 0.6% ([IRS.gov, Instructions for Form 940](https://www.irs.gov/instructions/i940) ). For tax year 2025, California (a 1.2% reduction) and the U.S. Virgin Islands (a 4.5% reduction) are the only credit-reduction jurisdictions, as of the November 10, 2025 repayment deadline ([Federal Register, Notice of FUTA Credit Reductions Applicable for 2025](https://www.federalregister.gov/documents/2026/01/12/2026-00342/notice-of-the-federal-unemployment-tax-act-futa-credit-reductions-applicable-for-2025) ); the Department of Labor updates this list annually, so confirm the current year's Schedule A before assuming it still applies for 2026.
6. **Record the net quarterly liability** : Document the final figure in your FUTA tracking ledger alongside the quarter's end date.


**Expected outcome** : A verified net quarterly FUTA liability figure, recorded in your FUTA tracking ledger, ready for threshold evaluation.


**When to use** : Use this procedure at the close of every calendar quarter (March 31, June 30, September 30, and December 31). Do not run it mid-quarter; a partial-quarter calculation produces an inaccurate threshold decision downstream.


**Common pitfalls**


- **Skipping the wage base cap** : for employees who crossed $7,000 mid-year, forgetting to cap wages inflates the quarter's liability. Check year-to-date wages before applying the rate every time.
- **Assuming your state has no credit reduction** : the list changes based on which states carry outstanding federal unemployment loan balances. Verify Schedule A (Form 940) each year rather than assuming last year's list still applies.


AsureCentral applies the wage base cap and available credit calculation directly in the platform, so payroll teams running 10 to 500 employees are not recalculating this by hand every quarter.


### How to Evaluate the $500 FUTA Deposit Threshold


How to Evaluate the $500 FUTA Deposit Threshold is the procedure for determining whether your quarterly FUTA liability triggers a required electronic deposit or may be carried forward. Run it immediately after calculating quarterly liability, every quarter without exception, to produce a documented deposit-or-carry-forward decision.


**Prerequisites**


- **Verified net quarterly FUTA liability** : the figure produced by the calculation procedure above.
- **Carryforward balance** : the cumulative undeposited FUTA liability from prior quarters, pulled from your FUTA tracking ledger.
- **Current quarter identifier** : knowing whether you are in the first, second, third, or fourth quarter, since fourth-quarter liability is treated differently.


**Steps**


1. **Retrieve the carryforward balance** : Pull the cumulative undeposited FUTA liability from your FUTA tracking ledger.
2. **Add the current quarter's liability** : Sum the carryforward balance and the current quarter's net FUTA liability to get total undeposited liability.
3. **Compare to the $500 threshold** : If total undeposited liability is $500 or more, a deposit is required. If it is under $500, carry the balance forward ([IRS.gov, Tax Topic 759](https://www.irs.gov/taxtopics/tc759) ).
4. **Flag the fourth-quarter exception** : If the current quarter is the fourth quarter, a deposit or Form 940 balance-due payment is required regardless of the $500 threshold ([IRS.gov, Instructions for Form 940](https://www.irs.gov/instructions/i940) ).
5. **Record the decision** : Document "deposit required" or "carry forward" in your FUTA tracking ledger, along with the date and the total undeposited amount.


**Expected outcome** : A documented deposit-or-carry-forward decision, with the triggering liability amount and quarter recorded, the direct input for either the EFTPS deposit procedure or the carryforward-tracking procedure.


**When to use** : Run this procedure immediately after calculating quarterly liability, every quarter. Do not skip it in the fourth quarter even if the cumulative balance sits under $500; the fourth quarter always requires resolution.


**Common pitfalls**


- **Evaluating only the current quarter's liability** : adding the carryforward balance is what causes a deposit to trip over $500 even when the current quarter alone looks small. Skipping this step is a common cause of a missed deposit.
- **Treating the fourth quarter like the first three** : fourth-quarter liability must be resolved, by deposit or by the Form 940 balance-due payment, even when the cumulative balance never reached $500 all year.


AsureCentral builds this threshold comparison into its deposit tracking, so growth-stage teams see a deposit-or-carry-forward flag rather than reconstructing the cumulative balance by hand each quarter.


### How to Deposit FUTA Tax via EFTPS


How to Deposit FUTA Tax via EFTPS is the procedure for submitting a required quarterly FUTA deposit through EFTPS. The payroll operator or designated tax contact executes it whenever the threshold evaluation returns a deposit-required decision, and it must be completed by the deadline for that quarter.


**Prerequisites**


- **Active EFTPS enrollment** : your employer identification number (EIN) registered at eftps.gov; new enrollments can take up to five business days to process ([IRS.gov, EFTPS Electronic Federal Tax Payment System](https://www.irs.gov/payments/eftps-the-electronic-federal-tax-payment-system) ).
- **EFTPS PIN and internet password** : issued during enrollment.
- **Verified deposit amount** : the total undeposited FUTA liability confirmed in the threshold evaluation procedure.
- **Deposit deadline confirmed** : quarterly FUTA deposits are due by the last day of the month following the quarter's end, April 30 for the first quarter, July 31 for the second quarter, October 31 for the third quarter, and January 31 for the fourth quarter, shifting to the next business day when that date falls on a weekend or holiday ([IRS.gov, Instructions for Form 940](https://www.irs.gov/instructions/i940) ). For tax year 2025, the fourth-quarter deadline moves to February 2, 2026, because January 31, 2026 falls on a Saturday.


**Steps**


1. **Log in to EFTPS** : Navigate to eftps.gov and authenticate with your EIN, PIN, and internet password.
2. **Select the tax form and period** : Choose Form 940 as the tax form and select the applicable quarter and year.
3. **Enter the deposit amount** : Input the total undeposited FUTA liability confirmed in the threshold evaluation.
4. **Schedule the payment date** : Set the payment date at least one calendar day before the deposit deadline, and no later than 8 p.m. Eastern time that day, for the payment to count as timely ([IRS.gov, Tax Topic 757](https://www.irs.gov/taxtopics/tc757) ).
5. **Confirm and submit** : Review the payment summary screen, EIN, amount, period, and date, then submit and capture the EFT confirmation number.
6. **Record the deposit** : Log the confirmation number, payment date, amount, and quarter in your FUTA tracking ledger, and reset the carryforward balance to zero.


**Expected outcome** : An EFTPS payment submitted with a confirmed EFT number, your FUTA tracking ledger updated with the deposit record, and the carryforward balance reset to zero.


**When to use** : Run this procedure whenever the threshold evaluation returns a deposit-required decision. Do not use it for the Form 940 annual balance-due payment; that uses a separate EFTPS payment type, covered in the final procedure below.


**Common pitfalls**


- **Scheduling the payment on the deadline date itself** : EFTPS requires at least one calendar day of lead time before 8 p.m. Eastern. Schedule the payment a day earlier than the deadline, not on it.
- **Selecting the wrong tax period** : confirm the quarter-and-year combination before submitting. An incorrect period selection typically requires correcting correspondence with the IRS.


Because a missed or misdirected FUTA deposit exposes your business to IRS penalties and interest, AsureCentral tracks the deposit deadline and flags the payment window in advance, so this step gets scheduled ahead of the cutoff instead of discovered at it.


## Annual Filing and Balance-Due Payment


Run these two procedures once, at year-end, after your last quarterly threshold evaluation of the year is complete.


### How to Track Sub-$500 FUTA Carryforward Across Quarters


How to Track Sub-$500 FUTA Carryforward Across Quarters is the procedure for maintaining an accurate running balance of undeposited FUTA liability when a quarter's amount falls under the $500 deposit threshold. The payroll operator runs it on a rolling quarterly basis whenever the threshold evaluation returns a carry-forward decision, producing a current carryforward ledger.


**Prerequisites**


- **FUTA tracking ledger** : a spreadsheet or payroll-system field holding the prior quarter's carryforward balance.
- **Verified net quarterly FUTA liability** : the figure produced by the calculation procedure.
- **Carry-forward decision confirmed** : the threshold evaluation procedure must have returned "carry forward" for the current quarter.


**Steps**


1. **Open the FUTA tracking ledger** : Navigate to the running carryforward balance record for the current tax year.
2. **Enter the current quarter's liability** : Add the verified net quarterly FUTA liability to the existing carryforward balance.
3. **Confirm the new total is still under $500** : If the cumulative total has crossed $500, stop here, return to the threshold evaluation procedure, and run the EFTPS deposit procedure instead.
4. **Record the updated balance** : Document the new cumulative total, the quarter, and the date in the ledger.
5. **Flag fourth-quarter resolution** : If this is the third quarter and the carryforward balance is greater than zero, set a reminder to resolve the balance through the annual Form 940 payment procedure by the filing deadline.


**Expected outcome** : An updated FUTA tracking ledger showing the current cumulative carryforward balance, with a fourth-quarter resolution reminder set if a balance remains.


**When to use** : Use this procedure only when the threshold evaluation confirms carry-forward status. Do not use it as a substitute for the EFTPS deposit procedure once the cumulative balance meets or exceeds $500.


**Common pitfalls**


- **Letting the ledger go stale** : if the carryforward balance is not updated every quarter, the total undeposited liability entering the next threshold evaluation will be wrong, understating how close you are to the $500 line.
- **Forgetting the balance still needs annual resolution** : a small carryforward that never crosses $500 during the year is still due in full with the annual Form 940 filing. It does not disappear.


### How to Pay Form 940 Balance Due at Annual Filing


How to Pay Form 940 Balance Due at Annual Filing is the procedure for remitting any FUTA liability not already covered by quarterly deposits when filing the annual Form 940. The payroll operator or tax preparer executes it once per year, by the filing deadline, and it produces a filed Form 940 with a zero outstanding balance.


**Prerequisites**


- **Completed Form 940 draft** : annual FUTA liability calculated, with quarterly deposits credited on Part 4.
- **Balance-due amount** : the figure on Form 940, Part 4, Line 14.
- **Active EFTPS enrollment** : the same credentials used for quarterly deposits.
- **Filing deadline confirmed** : Form 940 is due January 31, extended to February 10 for employers who deposited all FUTA tax in full and on time during the year ([IRS.gov, Instructions for Form 940](https://www.irs.gov/instructions/i940) ). For the 2025 tax year, filed in 2026, the standard deadline shifts to February 2, 2026, and the extended deadline is February 10, 2026, because January 31, 2026 falls on a Saturday.


**Steps**


1. **Finalize Form 940 calculations** : Complete Form 940 through Part 4 to confirm the balance-due amount on Line 14.
2. **Verify quarterly deposit credits** : Confirm every EFTPS deposit made during the year is reflected on Form 940, Part 4, Line 13, to avoid overpaying.
3. **Log in to EFTPS** : Authenticate at eftps.gov with your EIN, PIN, and internet password.
4. **Select the Form 940 annual payment** : Choose Form 940 and select the annual tax period, the filing year, not a quarter.
5. **Enter the balance-due amount** : Input the Line 14 figure exactly as it appears on the completed Form 940.
6. **Schedule the payment on or before the deadline** : Set the payment date to meet the filing deadline, and capture the EFT confirmation number.
7. **File Form 940 electronically** : Submit Form 940 through your payroll software or a third-party e-file provider, after the payment is confirmed.
8. **Record the annual filing** : Log the payment amount, EFT confirmation number, and Form 940 filing confirmation number in your FUTA tracking ledger, and close the tax year.


**Expected outcome** : Form 940 filed with the IRS, the balance-due payment submitted through EFTPS, and your FUTA tracking ledger closed for the tax year with every confirmation number recorded.


**When to use** : Run this procedure once per year at Form 940 filing, regardless of which path, deposit or carryforward, the year's quarters followed. Do not use it for quarterly deposits; those use the EFTPS deposit procedure with a quarterly period selection, not the annual period.


**Common pitfalls**


- **Paying the full annual liability without crediting deposits** : reconcile every quarterly deposit against Form 940, Part 4, before entering the balance-due amount, or you risk overpaying.
- **Assuming the extended deadline automatically applies** : the extension to February 10 (February 10, 2026, for the 2025 tax year) only applies if every required quarterly deposit was made in full and on time. Verify your deposit records before assuming eligibility.


AsureCentral provides audit-ready reporting that cross-references every EFTPS deposit made during the year against the draft Form 940 before the filing deadline, so the reconciliation in steps two and eight starts from a matched record instead of a manual review.


## How to Sequence These Procedures


Run these five procedures in order every quarter, and once more at year-end. At each quarter's close, calculate liability first, then evaluate the $500 threshold immediately after. The threshold evaluation is the routing gate: if cumulative undeposited liability is $500 or more, deposit via EFTPS and reset the carryforward to zero; if it is under $500, update the carryforward ledger and, in the third quarter, set a fourth-quarter resolution reminder. At year-end, regardless of which path the year's quarters followed, pay the Form 940 balance due and file the annual return. Never skip the threshold evaluation, and never run the EFTPS deposit and the carryforward-tracking procedure in the same quarter for the same liability balance.


## Applying This Playbook to Your Payroll Calendar


For most growth-stage payroll teams, the highest-risk procedure in this playbook is the EFTPS deposit. A missed or misdirected payment does not just cost a resubmission; it creates exposure to IRS penalties and interest. The prerequisites in that procedure, active EFTPS enrollment, the correct tax period selection, and the one-calendar-day lead time before 8 p.m. Eastern, are the most common failure points growth-stage teams hit.


AsureCentral tracks deposit deadlines and flags the payment window in advance, so the deposit step above gets scheduled ahead of the cutoff instead of discovered at it. Growth-stage teams can run this playbook themselves inside AsureCentral, where the calculations, thresholds, and deadlines are already tracked, or hand execution to Asure specialists through AsureWorks, the same platform operated on your behalf with no co-employment and your company remaining the employer of record. To put this playbook to work, map your quarterly FUTA deposit windows against AsureCentral before your next quarter close.


## Frequently Asked Questions


**Are FUTA tax deposits the same as biweekly or semiweekly payroll tax deposits?** No. FUTA deposits follow a quarterly schedule tied to the $500 threshold, separate from the biweekly or semiweekly deposit schedule used for federal income tax withholding and FICA (Social Security and Medicare tax). FUTA liability is evaluated once per quarter, after calculating liability and checking the threshold; it is never deposited on a biweekly payroll cycle.


**Can I pay FUTA tax by check instead of electronically?** No. Federal tax deposits, including FUTA, must be made electronically ([IRS.gov, EFTPS Electronic Federal Tax Payment System](https://www.irs.gov/payments/eftps-the-electronic-federal-tax-payment-system) ). EFTPS is the standard channel for both quarterly FUTA deposits and the annual Form 940 balance-due payment.


**Is FUTA tax paid once a year or quarterly?** Both, depending on your liability. A quarterly deposit is required whenever cumulative undeposited FUTA liability reaches $500. If the cumulative balance stays under $500 all year, the full amount is paid with the annual Form 940 filing instead. Many growing, multi-state employers will trigger at least one quarterly deposit during the year.


**Where do I send my FUTA tax payment?** Every FUTA deposit and Form 940 balance-due payment is made electronically through EFTPS at eftps.gov. There is no separate mailing address for a FUTA deposit itself.


**What happens if my quarterly FUTA liability is under $500?** You carry the amount forward to the next quarter. The balance accumulates until it reaches $500, which triggers an EFTPS deposit, or until the fourth quarter, when any remaining balance must be resolved with the annual Form 940 filing regardless of amount.


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
