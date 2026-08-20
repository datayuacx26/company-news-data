---
schema_version: "1.0.0"
document_id: "1ee7dcb153536247dc01338c005d4d526889aaa02a0546cd7d3cebd090e79fb9"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/form-w2"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T17:53:17.757440+00:00"
fetched_at: "2026-08-19T17:53:18.803914+00:00"
content_hash: "sha256:34b19bfaaed13a8de932ed44a2d553c09eeae9ad41dfcdf46560d17ddb4b3aed"
---

# ‍Form W-2: Wage and Tax Statement. What Every Box Means (2026 Guide)

**Form W-2 is annual wage and tax statement your employer sends you (and Social Security Administration) reporting what you earned and what was withheld during year.** Every W-2 employee gets one by January 31 following tax year. The form is primary input for your Form 1040 wages go on Line 1a, federal withholding goes on Line 25a, and Box 12 codes drive most of retirement, HSA, and stock-comp adjustments on Schedule 1.


This guide walks through Form W-2 box by box, January 31 dual deadline (employee + SSA), W-3 transmittal that pairs with it, and how W-2 differs from Form W-4 and Form 1099.


## What Form W-2 is


Per[IRS Form W-2 page](https://www.irs.gov/forms-pubs/about-form-w-2) , every employer engaged in a trade or business who pays remuneration for services performed by an employee must file Form W-2 for each employee. It's annual employer-to-employee (and employer-to-SSA) statement that closes year on payroll withholding.


The W-2 pairs with two other quarterly/annual filings:


- [Form 941](https://www.finlens.app/blogs/form-941) quarterly employer return that reports same wages/withholding on a rolling four-quarter basis
- **Form W-3** annual transmittal that summarizes all your W-2s when filing with SSA


The totals of your four Form 941s + Form W-3 must reconcile to sum of Boxes 1, 3, 5 across all your W-2s. Mismatches trigger CP notices.


## Who must file (and who gets one)


**Employers must issue a W-2 to any employee who:**


- Was paid $600 or more in year, OR
- Had any income tax, Social Security, or Medicare tax withheld from wages (regardless of amount)


**Employees who receive W-2s:**


- Full-time, part-time, and seasonal employees
- Statutory employees (Box 13 will be checked they report on Schedule C)
- Household employees paid $2,700+ in cash wages in 2024 (Schedule H filers issue W-2s too)


**Who does NOT get a W-2:**


- Independent contractors and freelancers → 1099-NEC instead ([see W-9 guide](https://www.finlens.app/blogs/form-w9) )
- Corporate officers of an S-corp who take distributions only (they should be taking reasonable comp via W-2, but that's a separate issue)
- Sole proprietors you don't W-2 yourself
- Partners in a partnership they receive a K-1, not a W-2


## The two January 31 deadlines


**W-2 to employee January 31.** Employers must furnish W-2 to each employee by January 31 following tax year. Electronic delivery is allowed if employee consents; otherwise paper delivery.


**W-2 to SSA January 31.** Since 2016, W-2/W-3 filing with Social Security Administration is due on same January 31 date (previously it was February 28 paper / March 31 electronic). Late filing penalties per Form 8809 start at $60 per W-2 (up to $232,500 per employer for smallest tier) and scale up quickly.


Employers filing 10 or more information returns (across all types W-2s, 1099s, etc.) must file electronically. This threshold dropped from 250 to 10 starting with 2023 tax year.


## The six copies of Form W-2


Each W-2 is printed in six copies, each going to a different place:


Copy


Goes to


A


Social Security Administration (with W-3 transmittal)


1


State, city, or local tax department


B


Employee — to attach to their federal return


C


Employee — for their personal records


2


Employee — to attach to their state/local return


D


Employer — for their own records (retain 4+ years)


‍


E-filing collapses much of this (Copy A goes electronically to SSA, no paper), but box structure and reporting requirements are identical.


## Box-by-box: what each field means


**Box a Employee's Social Security Number.** Verify with employee's Social Security card. A wrong SSN causes SSA to reject earnings from your record, and employee misses Social Security credits for year.


**Box b Employer Identification Number (EIN).** Your business EIN.


**Box c Employer's name, address, ZIP.**


**Box d Control number.** Optional field employers use to track W-2s internally.


**Box e Employee's name.** Legal name as shown on SS card.


**Box f Employee's address.**


**Box 1 Wages, tips, other compensation.** Federal-taxable wages. This is what flows to Form 1040 Line 1a. Excludes pre-tax 401(k), HSA, and Section 125 cafeteria plan contributions.


**Box 2 Federal income tax withheld.** The total federal withholding based on employee's Form W-4 elections. Flows to Form 1040 Line 25a.


**Box 3 Social Security wages.** Wages subject to 6.2% SS tax, capped at annual SS wage base ($168,600 for 2024, $176,100 for 2025). Does NOT exclude 401(k) contributions (those are still subject to SS/Medicare).


**Box 4 Social Security tax withheld.** Should equal Box 3 × 6.2%. If it doesn't, there's a payroll error.


**Box 5 Medicare wages and tips.** Wages subject to 1.45% Medicare tax, no cap. Includes 401(k) contributions.


**Box 6 Medicare tax withheld.** Box 5 × 1.45%, plus 0.9% Additional Medicare on wages above $200,000 per calendar year (employee-only withholding no employer match on additional 0.9%).


**Box 7 Social Security tips.** For tipped employees.


**Box 8 Allocated tips.** For food/beverage establishments allocating tips per Section 6053(c).


**Box 10 Dependent care benefits.** Employer-provided dependent care up to $5,000 excludable ($2,500 if MFS).


**Box 11 Nonqualified plans.** Distributions from nonqualified deferred comp plans.


**Box 12 Codes.** The most information-dense box. Uses lettered codes for various compensation and benefits some of common ones:


- **A** Uncollected SS tax on tips
- **B** Uncollected Medicare tax on tips
- **C** Group-term life insurance over $50,000 (imputed income)
- **D** 401(k) elective deferrals
- **E** 403(b) elective deferrals
- **DD** Employer-sponsored health coverage cost (informational, not taxable)
- **P** Excludable moving expense reimbursement (active-duty military only post-TCJA)
- **W** HSA employer + employee pre-tax contributions
- **AA** Roth 401(k) contributions
- **BB** Roth 403(b) contributions
- **EE** Roth 457(b) contributions


Some W-2s have multiple Box 12 entries; each uses a separate line labeled 12a, 12b, 12c, 12d.


**Box 13 Checkboxes.** Statutory employee (report on Schedule C, no SE tax), Retirement plan participant, Third-party sick pay.


**Box 14 Other.** Employer catch-all for state disability, union dues, uniform allowance, PTE elective withholding, or anything else not covered by a specific box.


**Boxes 15–20 State and local wage and tax reporting.**


## Reconciling W-2 to Form 941


Employers must reconcile totals on their four quarterly Form 941s to annual W-3 transmittal + all W-2s issued. Specifically:


- Sum of Form 941 Line 2 (wages, tips, other comp) × 4 quarters = W-3 Box 1 total
- Sum of Form 941 Line 5a (SS wages) × 4 quarters = W-3 Box 3 total
- Sum of Form 941 Line 5c (Medicare wages) × 4 quarters = W-3 Box 5 total


Mismatches trigger a **Schedule D (Form 941)** filing explaining discrepancy usually caused by year-end bonuses, imputed income, or acquisition/M&A wage transfers. Chronic mismatches lead to combined-annual-wage-reporting (CAWR) inquiries from SSA and IRS.


## Common W-2 mistakes


**Wrong SSN on Box a.** Causes SSA to reject earnings record. Employee misses Social Security credits until corrected via Form W-2c.


**Box 1 including 401(k) contributions:** Box 1 should EXCLUDE pre-tax 401(k) deferrals (they're in Box 12 code D). Box 3/5 should INCLUDE them. Getting this backward is most common employee-side W-2 error.


**Missing Box 12 code DD (employer health coverage):** Required for employers issuing 250+ W-2s (smaller employers optional). This is informational-only it doesn't add to taxable income.


**Late issuance past January 31:** $60 per W-2 for first 30 days late, escalating to $310 per W-2 if past August 1. Willful failure to file can go higher.


**Sending Copy A on wrong paper:** Copy A submitted to SSA on paper must use official red-ink pre-printed form. Downloaded PDFs of Copy A won't be accepted SSA will bounce them back. E-filing avoids this entirely.


**Not issuing a W-2c after correcting a payroll error:** If you fixed a payroll mistake mid-year but didn't update W-2, year-end totals won't match your ledger. Issue Form W-2c to correct.


## W-2 vs. related forms


Form


Direction


Purpose


W-2


Employer → Employee + SSA


Annual report of wages + withholding


W-3


Employer → SSA


Transmittal summarizing all W-2s


W-4


Employee → Employer


Withholding elections


W-9


Contractor → Payer


TIN certification for 1099 reporting


1099-NEC


Payer → Contractor + IRS


Payments $600+ to non-employees


941


Employer → IRS


Quarterly employment tax return


‍


## Conclusion


**Form W-2 is annual close of your payroll year for employee AND employer.** Every number on it feeds a downstream filing: employee's 1040, SSA's earnings record, and reconciliation to Form 941. Boxes 1, 3, and 5 rarely match each other exactly, and that's by design.


## Frequently asked questions


### **When should I get my W-2?**


By January 31 following tax year. If you haven't received it by mid-February, contact your employer. If that fails, contact IRS after February 15 to initiate a Form 4852 substitute W-2.


### **What if my W-2 is wrong?**


Ask your employer to issue Form W-2c (Corrected Wage and Tax Statement). Don't file your return using an incorrect W-2 IRS matches against what SSA has on file.


### **Do I need my W-2 to file taxes?**


Yes. You can technically file with Form 4852 (substitute W-2) if employer refuses, but IRS holds your refund pending SSA verification, which delays processing weeks or months.


### **Can an employer send my W-2 electronically?**


Yes, with your prior consent. Without consent, they must mail a paper copy.


### **Is Box 1 my "salary"?**


Not exactly. Box 1 is your federal-taxable wages typically salary minus pre-tax 401(k), HSA, and cafeteria plan contributions. If you're comparing offers, "gross salary" is closer to Box 3 (SS wages before wage-base cap) or Box 5 (Medicare wages).


### **Why is Box 1 different from Box 3 and Box 5?**


Different tax bases. Box 1 excludes 401(k) and pre-tax retirement contributions. Box 3 and Box 5 include them. Box 3 is capped at SS wage base; Box 5 has no cap.


### **What's difference between W-2 and 1099-NEC?**


W-2 is for employees (subject to withholding, employer pays FICA). 1099-NEC is for independent contractors (no withholding, contractor pays[self-employment tax](https://www.finlens.app/blogs/self-employment-tax) ). See[difference between W-2 and 1099](https://www.finlens.app/blogs/difference-between-w2-and-1099) guide for classification test.


### **Do I need to keep old W-2s?**


Yes. Keep at least 4 years for federal purposes (IRS statute of limitations). Best practice is 7 years for state and Social Security record verification.
