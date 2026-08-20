---
schema_version: "1.0.0"
document_id: "b7e8ab268b165e0341b002c7fa34bce59365a4d585b675ae44c6942159b512dc"
company_key: "asure-software-inc-common-stock"
company: "Asure Software Inc"
source_id: "asure-software-inc-common-stock-rss-2ceab48495f7"
canonical_url: "https://www.asuresoftware.com/5-procedures-for-clarifying-employer-vs-employee-payroll-tax-liability-in-growth-stage-companies/"
published_at: "2026-08-10T13:34:57+00:00"
first_seen_at: "2026-08-10T15:01:28.894037+00:00"
fetched_at: "2026-08-10T15:01:30.119683+00:00"
content_hash: "sha256:8e90b908375ceb9e87896f0497c40b155c2fce6ad697f7223e681a569c8e4b55"
---

# 5 Procedures for Clarifying Employer vs Employee Payroll Tax Liability in Growth-Stage Companies

Employer vs employee payroll tax liability is fixed by statute, but staying compliant with it is an operational job, not a one-time lookup. This playbook walks through five procedures growth-stage payroll teams run in sequence: mapping the liability split, configuring FICA withholding and employer contributions, verifying pay stub employer contribution accuracy, calculating and remitting FUTA and SUTA as employer-only taxes, and reconciling employer tax contributions against Form 941 and W-2 filings. Asure built this sequence from hands-on payroll configuration work with growth-stage companies standing up or auditing payroll.


## How to Sequence These Procedures


Run these five procedures in order for a clean payroll tax setup. Start with Procedure 1, mapping the liability split, since the documented map it produces is the input every downstream procedure depends on. Run Procedure 2, configuring FICA, immediately after, using that map to drive your system settings. Before your first live payroll run, execute Procedure 3, verifying pay stub employer contribution accuracy, while labeling and calculation errors are still easy to fix. Procedure 4, calculating and remitting FUTA and SUTA, runs every quarter as a standalone exercise. It does not depend on Procedure 3, but it does depend on the wage base and rate data established in Procedure 1. Close each quarter with Procedure 5, reconciling against Form 941 and W-2 filings, to confirm that what your system calculated, what you deposited, and what you reported all agree. If your company inherited an existing payroll setup rather than building one from scratch, start with Procedures 3 and 5 as diagnostics before re-running Procedures 1 and 2 to correct what you find.


## How to Map Employer vs Employee Payroll Tax Liability


Mapping employer vs employee payroll tax liability is the first procedure in this playbook. It establishes, tax by tax, which payments are the company's obligation, which belong to the employee, and which are split, before any payroll system is configured. The payroll owner or HR ops lead runs it during initial setup or a provider transition, and it produces a documented liability map that drives every downstream configuration decision. For the definitional groundwork on which taxes fall into which category,[the employer vs employee payroll tax liability FAQ](https://www.asuresoftware.com/employer-vs-employee-payroll-tax-liability-faq/) covers that in depth; this procedure is about turning that knowledge into a working configuration reference.


**Prerequisites**


- Federal EIN and state employer tax account numbers obtained
- Employee classifications finalized, since this playbook covers W-2 payroll tax liability
- States of employment identified, since SUTA rates and wage bases are set by each state workforce agency
- Payroll system access with tax configuration permissions


**Steps**


1. List every active tax type. Document Social Security, Medicare, federal income tax withholding, FUTA, SUTA, and any state or local income taxes that apply to your workforce as rows in a working spreadsheet.
2. Assign a liability owner to each tax type. Mark each one as employer-only, employee-only, or split, using IRS Publication 15 and your state's employer tax guide as the reference.
3. Record the statutory rate for each split tax. Enter Social Security at 6.2% for the employer and 6.2% for the employee, and Medicare at 1.45% for the employer and 1.45% for the employee, per[IRS Publication 15](https://www.irs.gov/publications/p15) .
4. Flag the employer-only taxes and their rate basis. Mark FUTA, at 6.0% on the first $7,000 of each employee's wages and typically reduced to a net 0.6% by the standard credit, and SUTA, at your state's assigned rate, as employer-only with no employee deduction.
5. Confirm federal income tax withholding sits entirely with the employee. There is no employer federal income tax rate. The employer withholds it from wages and remits it as an agent, not as a co-obligor.
6. Save the completed liability map as your configuration reference. Share it with whoever configures your payroll system next, so the FICA setup in Procedure 2 reflects accurate liability ownership from day one.


In Asure's work with growth-stage payroll teams, this mapping step is where most inherited configuration errors get caught, before they compound into a filing correction or a pay stub dispute.


**Expected outcome** A documented liability map assigning every active payroll tax to its owner and statutory rate, ready to drive payroll system configuration in Procedure 2.


**When to use it** Use this at initial payroll setup, during a provider transition, or when auditing an inherited configuration. It is not a substitute for state-specific legal counsel in states with non-standard local payroll taxes.


**Common pitfalls**


- Treating federal income tax withholding as an employer tax. It is the employee's liability; the employer remits it as a withholding agent.
- Missing state-specific employer-only taxes. Some states impose additional employer-only payroll taxes, such as state disability insurance or paid family leave contributions, that don't appear in federal guidance.


## How to Configure FICA Withholding and Employer Contributions


Configuring FICA withholding and employer contributions is the procedure for entering the Social Security and Medicare split into your payroll system so both the employee deduction and the employer contribution calculate correctly on every payroll run. The payroll admin executes it during initial system setup, at the start of each calendar year when wage bases reset, or after a rate change, and it produces a verified FICA configuration in your payroll platform. Run it immediately after completing the liability map from Procedure 1.


**Prerequisites**


- Completed liability map from Procedure 1
- Payroll system admin access with tax table configuration rights
- Current Social Security wage base confirmed for the tax year
- Employee roster with year-to-date wages loaded, for mid-year setups


**Steps**


1. Locate the federal tax configuration panel in your payroll system. It is typically labeled Federal Taxes or Tax Setup.
2. Set the Social Security employee withholding rate to 6.2%. Confirm the wage base ceiling is set to the current-year limit, $184,500 for 2026 per[IRS Publication 15](https://www.irs.gov/publications/p15) , so withholding stops automatically once an employee crosses it.
3. Set the Social Security employer contribution rate to 6.2%. Confirm the same $184,500 wage base ceiling applies on the employer side; the split is equal and the cap is shared.
4. Set the Medicare employee withholding rate to 1.45% with no wage base ceiling. Enable the Additional Medicare Tax trigger at $200,000 in year-to-date wages, per the[IRS Additional Medicare Tax Q&A](https://www.irs.gov/businesses/small-businesses-self-employed/questions-and-answers-for-the-additional-medicare-tax) , so the extra 0.9% withholds only above that threshold.
5. Set the Medicare employer contribution rate to 1.45% with no ceiling and no Additional Medicare Tax. Confirm the system applies no portion of the 0.9% surtax to the employer side; there is no employer match for it.
6. Run a test payroll calculation for one employee below the Social Security wage base. Confirm the employee deduction and employer contribution each equal 7.65% of gross wages on the payroll preview screen.
7. Save and document the configuration. Record the configuration date, the rates applied, and who completed the setup, for your compliance file.


If your team runs payroll on AsureCentral, this configuration is applied at the platform level using current-year federal wage bases and rates, rather than something your team maintains manually each year.


**Expected outcome** FICA withholding and employer contributions calculate correctly on every payroll run, with the $184,500 Social Security wage base ceiling enforced for 2026 and the Additional Medicare Tax applied only on the employee side above $200,000.


**When to use it** Use at initial setup and again at the start of each calendar year when the Social Security wage base resets. This procedure does not cover tipped-employee or clergy FICA configuration, which follow separate rules.


**Common pitfalls**


- Leaving last year's wage base in place. The Social Security wage base rose to $184,500 for 2026 from $176,100 in 2025, and a stale ceiling causes over-withholding and over-contribution once an employee crosses the real cap.
- Applying the Additional Medicare Tax to the employer side. That 0.9% surtax is employee-only; configuring it on both sides creates an employer overpayment that requires an amended filing to correct.


## How to Verify Pay Stub Employer Contribution Accuracy


Verifying pay stub employer contribution accuracy is the procedure for confirming that the employer contribution line items on employee pay stubs reflect the company's own tax liability, not the employee's, before payroll goes out. The payroll owner or HR ops lead runs it after FICA configuration and before the first live payroll run, and it produces a verified pay stub template with accurate employer contribution labels. Run it again after any payroll system migration.


**Prerequisites**


- FICA configuration completed per Procedure 2
- Sample pay stub generated from the payroll system's preview or test-run output
- Liability map from Procedure 1 available for cross-reference
- Access to the payroll system's pay stub template editor, if custom labels are used


**Steps**


1. Pull a sample pay stub from the payroll system's preview or test-run output. Use a mid-range gross wage employee so both FICA components are visible.
2. Locate the employer contribution section on the pay stub. Confirm it is labeled clearly as an employer contribution, appearing as an informational disclosure rather than a deduction from employee net pay.
3. Verify the employer Social Security amount equals 6.2% of the employee's gross wages, up to the wage base. Cross-reference the figure against your liability map and flag any variance.
4. Verify the employer Medicare amount equals 1.45% of gross wages with no cap. Confirm no Additional Medicare Tax appears on the employer side.
5. Confirm FUTA and SUTA do not appear as employee deductions. Both are employer-only taxes and should never show up in the employee deduction section of the pay stub.
6. Check that federal income tax withholding is labeled as an employee deduction, not an employer contribution. The employer has no federal income tax rate; withholding belongs entirely in the employee deduction column.
7. Document your findings and correct any labeling or calculation errors in the payroll system before approving the first live run.


In Asure's managed service work through AsureWorks, this pay stub review is a standard step before a new client's first live payroll run, since labeling errors are one of the more common sources of employee pay disputes.


**Expected outcome** A verified pay stub template where every employer contribution line is accurate and correctly labeled, with no employer-only taxes appearing as employee deductions or vice versa.


**When to use it** Use before every first live payroll run and after any payroll provider migration. If your company has already run payroll for more than a full quarter without this check, treat it as a diagnostic and pair it with the reconciliation in Procedure 5 rather than relying on this step alone.


**Common pitfalls**


- Accepting default pay stub labels without review. Many payroll platforms use abbreviated labels that employees can misread as an additional deduction from their own pay.
- Missing FUTA or SUTA on the employer side. If your payroll system omits these from the employer contribution display, you may be underestimating your total cost per employee.


## How to Calculate and Remit FUTA and SUTA as Employer-Only Taxes


Calculating and remitting FUTA and SUTA is the procedure for computing your federal and state unemployment tax liability, applying the correct wage bases and credit offsets, and remitting on the required schedule. The payroll owner or finance lead executes it quarterly and at year-end Form 940 filing, and it produces accurate FUTA deposits and SUTA filings with zero employee deductions, since both are payroll taxes paid by the employer only. This procedure runs every quarter regardless of whether Procedure 3 has been completed, but it depends on the wage base and rate data established in Procedure 1.


**Prerequisites**


- FUTA wage base ($7,000 per employee) and current gross FUTA rate (6.0%) confirmed, per IRS Publication 15
- State SUTA rate and wage base obtained from your state workforce agency
- SUTA payment history for the quarter confirmed, since it determines the FUTA credit
- EFTPS enrollment active for federal tax deposits
- State unemployment tax account active and in good standing


**Steps**


1. Calculate each employee's FUTA-taxable wages for the quarter. Apply the $7,000 annual wage base; once an employee's year-to-date wages exceed that amount, no further FUTA liability accrues for that employee.
2. Multiply FUTA-taxable wages by 6.0% to get gross FUTA liability. This is the pre-credit rate, per[IRS Publication 15](https://www.irs.gov/publications/p15) ; do not remit this amount yet.
3. Confirm your state is not a credit-reduction state and that SUTA payments are current. Timely SUTA payments in a non-credit-reduction state entitle you to a 5.4% FUTA credit, reducing the net FUTA rate to 0.6%, per the[IRS FUTA Credit Reduction page](https://www.irs.gov/businesses/small-businesses-self-employed/futa-credit-reduction) .
4. Apply the FUTA credit to arrive at net FUTA liability. Multiply FUTA-taxable wages by 0.6%, or by the applicable reduced rate if your state is a credit-reduction state. For tax year 2025, the Form 940 due February 2, 2026, only California (a 1.2% reduction) and the U.S. Virgin Islands (a 4.5% reduction) carried a credit reduction, per[IRS Schedule A of Form 940](https://www.irs.gov/pub/irs-pdf/f940sa.pdf) . The Department of Labor redetermines this list annually, typically each November, so reconfirm it for the current tax year rather than assuming it repeats.
5. Deposit FUTA liability through EFTPS once your cumulative quarterly liability exceeds $500. If the running total for the quarter is $500 or less, carry it forward to the next quarter.
6. Calculate SUTA liability separately, using your state's wage base and assigned rate. Remit it to the state workforce agency on that state's required schedule.
7. Confirm no FUTA or SUTA deduction appears on any employee pay stub. Both are employer-only, and an employee deduction for either is a compliance error needing immediate correction.


When you run payroll through AsureCentral, the platform calculates FUTA and SUTA liability as part of each pay run, using the wage base and rate data your team established in the liability map.


**Expected outcome** FUTA deposited on the correct EFTPS schedule with the credit applied, SUTA remitted to the state agency, and zero employee deductions for either tax, ready for the annual Form 940 reconciliation.


**When to use it** Use every quarter for active payroll. This procedure does not cover household employers, who follow Schedule H rules, or employees based in U.S. territories, who follow separate FUTA rules.


**Common pitfalls**


- Forgetting the credit reduction for affected states. States with outstanding federal unemployment loans can carry a reduced FUTA credit that raises the net rate; check IRS Schedule A each year rather than assuming the prior year's list still applies.
- Deducting SUTA from employee pay. A small number of states permit voluntary employee SUTA contributions, but in most states SUTA is an employer-only tax and must not appear as a deduction.


## How to Reconcile Employer Payroll Tax Contributions Against Form 941 and W-2 Filings


Reconciling employer payroll tax contributions against Form 941 and W-2 filings is the procedure for verifying that your FICA contributions, federal income tax withholding remittances, and W-2 box amounts match your Form 941 quarterly filings and year-end totals. The payroll owner or controller executes it quarterly and at year-end, and it produces a signed reconciliation that closes the compliance loop before you file. Run it before submitting each Form 941 and before distributing W-2s.


**Prerequisites**


- Payroll register for the quarter, or the full year, exported from your payroll system
- Prior Form 941 filings and EFTPS deposit history available
- Draft W-2 data (boxes 3, 4, 5, and 6) pulled from the payroll system
- Liability map from Procedure 1 available for reference
- Access to your EFTPS account to verify deposit receipt confirmations


**Steps**


1. Export the quarterly payroll register. Sum total gross wages, employee FICA withheld, and employer FICA contributions, separating Social Security and Medicare into distinct columns.
2. Compare total employee Social Security withheld to 6.2% of total Social-Security-taxable wages. A meaningful variance signals a configuration error or a missed wage base cutoff that needs correction before filing.
3. Compare total employer Social Security contribution to that same 6.2% figure. The employer and employee amounts must be equal; any asymmetry signals a system configuration error.
4. Repeat the comparison for Medicare at 1.45% on each side. Confirm the Additional Medicare Tax appears only on the employee side for wages above $200,000, per the[IRS Additional Medicare Tax Q&A](https://www.irs.gov/businesses/small-businesses-self-employed/questions-and-answers-for-the-additional-medicare-tax) , and flag any employer-side amount as an overpayment.
5. Cross-reference total federal income tax withheld against your EFTPS deposit records. Confirm every deposit was made on your required schedule, per IRS Publication 15 depositor rules.
6. Reconcile Form 941 lines 5a through 5d (FICA) and line 3 (federal income tax withholding) against your payroll register totals. Explain and correct any differences before submission.
7. At year-end, verify W-2 box 3 (Social Security wages), box 4 (Social Security withheld), box 5 (Medicare wages), and box 6 (Medicare withheld) match your annual payroll register totals. Distribute W-2s only after this reconciliation is signed off.


The stakes for skipping this step are personal, not just corporate. If withheld taxes are not remitted, the IRS can assess the Trust Fund Recovery Penalty against responsible individuals, including owners, officers, and payroll managers, not only against the business itself.


In Asure's work with growth-stage clients, this quarterly reconciliation is one of the procedures most often handed to Asure specialists through AsureWorks, since it requires cross-checking deposit records, filings, and payroll registers on a recurring schedule.


**Expected outcome** A signed reconciliation confirming that your Form 941 filings, EFTPS deposit history, and W-2 box amounts all agree with the payroll register, with no unexplained variances before submission or distribution.


**When to use it** Use before every Form 941 submission and before W-2 distribution. If a prior quarter's 941 has already been filed with an error, this procedure is not a substitute for the amendment process; that requires Form 941-X.


**Common pitfalls**


- Skipping the mid-year reconciliation check. Errors caught at year-end require a W-2c correction; catching them quarterly avoids that step.
- Treating an EFTPS deposit confirmation as proof of a correct amount. A deposit can be made in the right amount but applied to the wrong tax period, so verify the period designation in EFTPS, not just the dollar figure.


## Putting It Together


These five procedures work best in sequence. The liability map from Procedure 1 feeds every configuration decision downstream, FICA setup and pay stub verification protect the first live payroll run, and FUTA and SUTA calculation and 941 and W-2 reconciliation repeat every quarter to keep the whole system honest. Companies inheriting an existing payroll setup should treat Procedures 3 and 5 as diagnostics first, then re-run Procedures 1 and 2 to correct whatever they find. How payroll taxes are split between employer and employee will not change; what changes is how disciplined your process is for keeping that split accurate quarter after quarter.


Asure supports both paths through this playbook. Configure and run all five procedures yourself inside AsureCentral, the connected payroll and HR platform where your liability map, FICA tax tables, FUTA and SUTA calculation, and 941 and W-2 data live in one system of record. Or hand the ongoing execution, pay stub verification, and quarterly reconciliation work to Asure specialists through AsureWorks, Asure's managed payroll service, without triggering co-employment and while remaining the sole employer of record throughout.


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
