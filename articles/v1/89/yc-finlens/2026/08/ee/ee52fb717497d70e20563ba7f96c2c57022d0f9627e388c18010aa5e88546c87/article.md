---
schema_version: "1.0.0"
document_id: "ee52fb717497d70e20563ba7f96c2c57022d0f9627e388c18010aa5e88546c87"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/payroll-tax"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T17:53:17.757440+00:00"
fetched_at: "2026-08-19T17:53:18.803914+00:00"
content_hash: "sha256:3006ae957d2133cb1d1c73350a2cf7791a8ad6fae3aea01239c86f6fb42f06d4"
---

# Payroll Tax: What Employers and Employees Actually Pay (2026 Rates + Deposits)

**Payroll tax is umbrella term for federal (and state) taxes that flow out of every paycheck some withheld from employee, some paid entirely by employer, and some split between two.** Every $1,000 in gross wages triggers roughly $76.50 in FICA withholding from employee, $76.50 in FICA match from employer, some federal income tax withholding, plus FUTA and often SUTA on top. Understanding what each tax funds and who bears it is difference between an accurate labor budget and a surprise CP notice from IRS.


This guide breaks down every federal payroll tax by rate, wage base, deposit frequency, and which form reports it plus state-level SUTA layer that varies by employer.


## The four federal payroll taxes


Per[IRS Understanding Employment Taxes page](https://www.irs.gov/businesses/small-businesses-self-employed/understanding-employment-taxes) , federal employment taxes fall into four categories:


1. **Federal income tax withholding** variable, based on employee W-4
2. **Social Security tax** 6.2% employee + 6.2% employer = 12.4% up to wage base
3. **Medicare tax** 1.45% employee + 1.45% employer + 0.9% additional (employee only, on high earnings)
4. **Federal Unemployment Tax Act (FUTA)** 6% on first $7,000 per employee (effectively 0.6% after state credit)


Then state-level:


1. **State income tax withholding** varies by state, some (TX, FL, WA, NV, TN, SD, WY, AK, NH) have none
2. **State Unemployment Tax Act (SUTA)** varies widely by state and employer experience rating


## 1. Federal income tax withholding


**Who pays:** Employee (withheld from paycheck) **Rate:** Variable based on employee's Form W-4 elections, filing status, and payroll frequency **Wage base:** No cap; withholding is on all wages


Every paycheck, employer uses IRS Publication 15-T withholding tables (or Percentage Method) to calculate how much federal income tax to withhold. The math depends on:


- Filing status from Form W-4 Line 1
- Multiple jobs / working spouse box (Step 2c)
- Dependents claim ([Step 3](https://www.finlens.app/blogs/form-w4) )
- Other income and deductions (Step 4)
- Payroll frequency (weekly, biweekly, semimonthly, monthly)


The withheld amount is not employee's actual tax it's a rolling estimate. Come April, year's total withholding is credited against actual tax liability on Form 1040 Line 25a. Over-withheld → refund; under-withheld → balance due.


## 2. Social Security tax (part of FICA)


**Who pays:** Employee 6.2% + Employer 6.2% = 12.4% total **Wage base:** $168,600 for 2024 · $176,100 for 2025 **Above wage base:** SS stops entirely


Social Security funds OASDI (Old Age, Survivors, and Disability Insurance) trust fund. Both employee and employer contribute equally.


Once an employee's YTD wages hit annual wage base, SS withholding stops for rest of year (both sides). If employee has multiple jobs and combined YTD wages exceed base, excess withholding is refunded on Form 1040 Line 11 of Schedule 3 employer doesn't get their overage back automatically.


The SS wage base is indexed to national average wage growth, not CPI, so it typically rises 3–5% per year.


## 3. Medicare tax (part of FICA)


**Who pays:** Employee 1.45% + Employer 1.45% = 2.9% total, plus employee-only 0.9% additional on high earnings **Wage base:** No cap for 1.45% + 1.45% **Additional Medicare 0.9%:** Kicks in when employee wages exceed $200,000 in a calendar year (regardless of filing status $250K MFJ / $125K MFS thresholds apply only when reconciling on Form 1040)


Medicare funds HI (Hospital Insurance) trust fund. Unlike SS, Medicare has no wage cap on regular 1.45%+1.45%.


The **Additional Medicare Tax** is employee-only there's no employer match on 0.9%. Employers must begin withholding it as soon as an employee's YTD wages cross $200,000 within calendar year, regardless of whether employee will actually owe it on their return (they might not, if MFJ combined income is below $250K).


## Combined FICA at a glance


Component


Employee


Employer


Combined


Social Security up to wage base


6.2%


6.2%


12.4%


Medicare all wages


1.45%


1.45%


2.9%


Basic FICA subtotal


7.65%


7.65%


15.3%


Additional Medicare wages > $200K


0.9%


0%


0.9%


‍


For a self-employed person, all 15.3% is their responsibility that's[self-employment tax](https://www.finlens.app/blogs/self-employment-tax) , calculated on Schedule SE.


## 4. FUTA Federal Unemployment


**Who pays:** Employer only (0% employee) **Nominal rate:** 6% on first $7,000 per employee **Effective rate:** 0.6% after 5.4% state credit (in most states)


FUTA funds federal oversight of state unemployment programs and provides loans to states running low on SUTA reserves. The nominal 6% rate is reduced by up to 5.4% credit if employer is current on their state SUTA payments so most employers pay just 0.6% of first $7,000, or $42 per employee per year.


**Credit reduction states:** If a state has an outstanding federal unemployment loan from prior years, FUTA credit gets partially reduced, so employers in that state pay MORE than 0.6%. For 2023–2024, California and New York were on credit reduction list at various points. The DOL publishes list annually.


FUTA is reported on **Form 940** (annual, due January 31), separate from Form 941. Deposits are quarterly if FUTA liability exceeds $500 for quarter.


## State-level layer: SUTA + state income tax


**SUTA (State Unemployment Tax).** Every state levies its own unemployment tax on employers. Rates vary widely new employers typically start at a "new employer rate" (often 2.7–3.5%), and after a few quarters of experience, get moved to an "experience rate" that reflects their layoff history. Wage bases also vary from $7,000 (federal match, some states) up to $67,600 (Washington, 2024).


**State income tax withholding.** 41 states + DC have state income tax; 9 states don't (TX, FL, WA, NV, TN, SD, WY, AK, NH). Local income tax (NYC, Philadelphia, some Ohio cities) adds another layer for affected employers.


## Deposit schedules monthly vs. semiweekly


Federal income tax + SS + Medicare are deposited to IRS via EFTPS on one of two schedules:


- **Monthly depositor:** Total employment tax in lookback period ≤ $50,000. Deposit by 15th of following month.
- **Semiweekly depositor:** Total employment tax in lookback period > $50,000. Deposit Wednesday (for paychecks issued W/T/F) or Friday (for paychecks issued S/S/M/T).


The "lookback period" is July 1 through June 30 of year prior to current calendar year. See our[Form 941 guide](https://www.finlens.app/blogs/form-941) for full mechanics.


**One-day rule:** If tax liability hits $100,000 or more on any single day, next-business-day deposit is required, and you're a semiweekly depositor for rest of year and all of next year.


**FUTA deposits:** Quarterly if $500+ liability; otherwise carry to next quarter.


## Which forms report which taxes


Tax


Reported on


Frequency


Federal income tax withholding


Form 941 (quarterly) + W-2 (annual)


Quarterly + January 31


Social Security


Form 941 + W-2 Box 3/4


Quarterly + January 31


Medicare


Form 941 + W-2 Box 5/6


Quarterly + January 31


Additional Medicare


Form 941 + W-2


Quarterly + January 31


FUTA


Form 940


Annual (January 31)


State income tax


State form (varies)


State schedule


SUTA


State unemployment form


Usually quarterly


‍


Reconciliation is critical: sum of your four Form 941 filings + Form 940 must tie to Form W-3 (transmittal of all W-2s) + state annual reports. See our[Form W-2 guide](https://www.finlens.app/blogs/form-w2) for box-by-box reconciliation.


## Common payroll tax mistakes


**Not depositing on time:** FTD (Failure-to-Deposit) penalties start at 2% (1–5 days late), scale to 5% (6–15), 10% (16+), and 15% after IRS notice. Interest also accrues. These penalties dwarf taxes themselves for small employers.


**Missing SUTA rate update:** State unemployment rates are reset annually (often in November/December for next year). Missing update leaves you under-remitting all year.


**Treating owner draws as wages:** For a sole proprietor or single-member LLC, owner draws are NOT wages and don't pay payroll tax they flow through Schedule C. For S-corp shareholders, you MUST take reasonable compensation via W-2 (which does pay payroll tax) before distributions.


**Not tracking multi-state employees:** Remote workers who cross state lines trigger nexus and multi-state withholding obligations. Payroll systems must be configured for employee's WORK state, which may differ from residence.


**Missing additional Medicare threshold:** Employers must start withholding 0.9% additional Medicare as soon as an employee's YTD wages cross $200K, not when they will owe it on Form 1040.


**Reclassifying employees as contractors:** The IRS is aggressive about misclassification. If someone is legally an employee, treating them as a 1099 contractor exposes you to unpaid FICA (both halves), unpaid federal withholding, unpaid FUTA/SUTA, and penalties. Section 530 relief only applies if certain criteria are met. See our[W-9 vs. W-4 guide](https://www.finlens.app/blogs/form-w9) for classification test.


## Conclusion


**Payroll tax is mechanical layer under every paycheck six separate taxes (federal income + SS + Medicare + FUTA + state income + SUTA) that need to hit six separate schedules on six separate forms.** Miss one deposit and FTD penalty is often larger than underlying tax.


## Frequently asked questions


### **What's difference between payroll tax and income tax?**


Income tax is a general-revenue tax on income. Payroll tax is a dedicated tax funding Social Security and Medicare (FICA) and unemployment insurance (FUTA/SUTA). Both come out of paychecks, but they're separate systems with separate wage bases.


### **Is FICA same as payroll tax?**


FICA is Social Security + Medicare portion of payroll tax (7.65% × 2 = 15.3%). Payroll tax also includes federal income tax withholding and FUTA/SUTA, so FICA is a subset.


### **Do payroll taxes apply to bonuses?**


Yes. Bonuses are supplemental wages subject to same FICA rates and either 22% flat federal withholding (up to $1M) or aggregate method combining with regular wages.


### **Do payroll taxes apply to overtime pay?**


Yes. Overtime is regular wages for tax purposes.


### **Are health insurance premiums pre-tax for FICA?**


Section 125 cafeteria plan contributions (including health insurance premiums via a POP plan) reduce FICA-taxable wages. So yes, pre-tax health premiums lower both employee and employer FICA.


### **Are 401(k) contributions pre-tax for FICA?**


No. 401(k) contributions reduce federal income tax withholding (Box 1) but NOT FICA (Box 3 SS wages, Box 5 Medicare wages). This is why Box 1 and Boxes 3/5 differ on a W-2.


### **What's SUTA "experience rating"?**


After a few quarters, state unemployment agencies assign your business a rate based on your layoff history. Fewer layoffs → lower rate; more layoffs → higher rate. Rates can range from ~0.1% to over 10% depending on state and history.


### **Does an S-corp owner pay payroll tax on distributions?**


No only on their reasonable-compensation W-2 wages. The K-1 profit distribution is not subject to FICA. Setting reasonable comp too low invites an IRS audit that reclassifies distributions as wages.
