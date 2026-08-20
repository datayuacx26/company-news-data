---
schema_version: "1.0.0"
document_id: "842d805c278e4b43647207e9b0833c6519caed86927765a30458ce0427701924"
company_key: "asure-software-inc-common-stock"
company: "Asure Software Inc"
source_id: "asure-software-inc-common-stock-rss-2ceab48495f7"
canonical_url: "https://www.asuresoftware.com/how-to-handle-payroll-taxes-for-llc-and-s-corp-owners-in-5-procedures/"
published_at: "2026-08-10T13:34:56+00:00"
first_seen_at: "2026-08-10T15:01:28.894037+00:00"
fetched_at: "2026-08-10T15:01:30.119683+00:00"
content_hash: "sha256:d2bb62e408c765bbb2385189b4e29ca5a423d30d3b9b211b466dad587291633f"
---

# How to Handle Payroll Taxes for LLC and S Corp Owners in 5 Procedures

Two payroll tax errors do the most damage to growing LLCs and S corps: misclassifying a worker and paying an S corp owner too little through payroll. This playbook sequences five procedures, obligation mapping, worker classification, owner compensation, withholding setup, and contractor handling, in the order an operator actually executes them. For the underlying rules in question and answer form, see the[LLC and S corp worker classification and owner compensation FAQ hub](https://www.asuresoftware.com/worker-classification-and-owner-compensation-frequently-asked-questions/) .


## Foundation Procedures


Before any payroll tax obligation can be calculated or withheld correctly, an operator needs to know which taxes apply to which people. These two procedures establish that foundation.


### How to Map Payroll Tax Obligations by Entity Type and Owner Role


How to Map Payroll Tax Obligations by Entity Type and Owner Role is the procedure for determining which federal and state payroll taxes apply based on your entity structure, LLC or S corp, and each owner's role. It is executed by the business owner or their accountant at formation or restructuring and produces a written payroll tax obligation matrix. Use this procedure before configuring a payroll system or filing any payroll tax form.


**Prerequisites**


- An EIN issued by the IRS.
- Entity type confirmed: single-member LLC, multi-member LLC, or S corp election filed on Form 2553.
- Every state where an owner or employee performs work identified.
- Each owner's role documented as member-manager, passive member, or shareholder-employee.


**Steps**


1. Confirm your entity classification. Check whether your LLC is taxed as a sole proprietorship, a partnership, or has elected S corp status, since the classification on file with the IRS determines which rules apply to owner pay.
2. Document each owner's role. Record whether each owner is an active member-manager or a passive member, because active involvement drives self-employment tax exposure in an LLC taxed as a partnership.
3. Map federal FICA obligations. FICA, the Federal Insurance Contributions Act, funds Social Security and Medicare through a 6.2% Social Security tax and a 1.45% Medicare tax, split between employee and employer. Confirm FICA applies to any S corp shareholder-employee wages. An LLC taxed as a sole proprietorship or partnership instead applies self-employment tax, 15.3% (12.4% Social Security plus 2.9% Medicare) on net earnings once those earnings reach $400 in a year, per the[IRS self-employment tax page](https://www.irs.gov/businesses/small-businesses-self-employed/self-employment-tax-social-security-and-medicare-taxes) , to the active owner's net earnings instead.
4. Identify FUTA exposure. FUTA, the Federal Unemployment Tax Act, is an employer-only tax of 6.0% on the first $7,000 of each employee's wages, reduced by a credit of up to 5.4% for timely state unemployment payments, for a net rate many employers pay of about 0.6%, per the[IRS Instructions for Form 940](https://www.irs.gov/instructions/i940) . Confirm whether your entity has W-2 wages, including S corp shareholder-employee wages, that trigger this tax.
5. Map state unemployment obligations. SUTA, or state unemployment tax, is set independently by each state where you have employees. Confirm your registration requirements and rate assignment directly with each state's unemployment agency, since rates and wage bases vary by state.
6. Document the obligation matrix. Build a one-page table listing each tax, FICA, FUTA, SUTA, and federal income tax withholding, who it applies to, and the applicable rate.
7. Validate the matrix with a payroll tax professional. Have a CPA or a payroll specialist review it before configuring a payroll system, since entity-classification errors made here compound through every later step.


Expected outcome: a completed payroll tax obligation matrix naming every applicable federal and state tax, the owners and employees it applies to, and the rate for each, ready to hand to whoever configures your payroll system.


Use this procedure at entity formation, immediately after filing an S corp election, or before hiring your first employee. Do not treat it as a substitute for legal advice if you are actively considering a change in entity type.


Common pitfalls include assuming an LLC owes no payroll tax at all. An active member-manager of an LLC taxed as a partnership owes self-employment tax on net earnings even though nothing runs through a payroll system. Skipping state unemployment registration is the other frequent miss. Missing a state's SUTA registration deadline creates a penalty exposure entirely separate from federal compliance.


Operators who build this matrix directly inside AsureCentral, Asure's connected payroll and HR platform, can carry the obligation matrix straight into system configuration instead of re-keying it later.


### How to Determine Worker Classification for Payroll Tax Purposes


How to Determine Worker Classification for Payroll Tax Purposes is the procedure for applying the IRS behavioral-control, financial-control, and type-of-relationship tests to a worker engagement to reach a defensible W-2 or 1099 classification. It is executed by the business owner or HR lead before onboarding any new worker and produces a documented classification decision. Use this procedure for every new worker engagement.


**Prerequisites**


- A written description of the worker's role, schedule, and deliverables.
- A draft or executed engagement agreement, either an offer letter or a contractor agreement.
- The obligation matrix from the mapping procedure completed.
- IRS Publication 15-A available for reference.


**Steps**


1. Apply the behavioral control test. Assess whether your company controls how the work gets done, the tools used, the hours set, the process dictated. Control signals employee status.
2. Apply the financial control test. Assess whether the worker has a real investment in their own tools and equipment, bears unreimbursed expenses, and is free to work for other clients. These factors signal independent contractor status.
3. Apply the type-of-relationship test. Check whether a written contract exists, whether you provide benefits, and whether the relationship is ongoing or tied to a single project. Permanence and benefits signal employee status.
4. Score the three tests together. Write down the outcome of each test on a classification worksheet, noting which factors point toward employee status and which point toward contractor status.
5. For a genuinely close call, note that a separate framework exists. The IRS's own three-factor test above is what determines payroll tax classification. The Department of Labor applies a different economic-reality test for wage-and-hour purposes under the FLSA, and the two agencies can reach different conclusions about the same working relationship. Treat a borderline IRS score as a signal to get a professional opinion, not as license to apply the DOL's test in place of the IRS's.
6. Record the decision. Document the classification, W-2 or 1099-NEC, the reasoning behind it, and the date, and keep it in the worker's onboarding file.
7. Issue the correct onboarding paperwork. For a W-2 employee, collect Form W-4 and set up withholding. For a 1099 contractor, collect Form W-9 and confirm that no withholding applies.
8. Set a reclassification review trigger. Schedule an annual check on any contractor engagement that scored close to the line, since a relationship that evolves over time can shift the correct classification.


Expected outcome: a signed, dated classification decision record for each worker, with the three test results documented, the primary artifact you would produce if the classification were ever challenged.


Use this before every new worker engagement, regardless of how the worker describes themselves. Do not rely on a worker's own preference to be treated as a 1099 contractor as a substitute for running the tests.


A job title is not a classification. Calling someone a contractor in an agreement does not make them one if the behavioral-control factors point to employee status. Skipping the annual review is the other common miss. A long-term contractor whose role has grown into something closer to employment can cross the classification line without anyone noticing, since nothing forces a second look.


AsureWorks specialists apply this same three-factor framework across client onboarding and provide a classification worksheet as part of that support, giving operators a documented decision trail before the first payment goes out. AsureWorks is a managed service, not a PEO. You remain the employer of record throughout.


## Owner Compensation Procedures


Of the five procedures in this sequence, the one below carries the least room for error.


### How to Set Reasonable Compensation as an S Corp Shareholder-Employee


How to Set Reasonable Compensation as an S Corp Shareholder-Employee is the procedure for setting a defensible salary for an owner who performs services for the S corp, so FICA withholds correctly and the company avoids the IRS's sharpest scrutiny area for S corp owner pay. It is executed by the shareholder-employee and their CPA annually and produces a documented compensation rationale. Use whenever the owner actively works in the business.


Reasonable compensation is the IRS requirement that an S corp shareholder-employee who performs services for the corporation be paid a defensible, benchmark-supported W-2 salary before taking any distributions.


**Prerequisites**


- S corp election confirmed, Form 2553 accepted by the IRS.
- The owner's services to the corporation documented: role, hours, responsibilities.
- Comparable salary data sourced for the owner's role and geography from a salary survey or industry benchmark.
- Payroll system configured and EIN active.


**Steps**


1. Document the owner's services. Write a short description of what the shareholder-employee actually does for the company, including role, time commitment, and contribution to revenue.
2. Source comparable compensation data. Pull at least two independent salary benchmarks for the owner's role, industry, and geography.
3. Set the compensation figure. Choose a salary within the benchmark range that reflects the owner's experience, the company's revenue, and the scope of services performed, and write down the reasoning.
4. Confirm the salary covers FICA minimums. The 2026 Social Security taxable wage base is $184,500, per the[Social Security Administration's October 2025 announcement](https://www.ssa.gov/news/en/press/releases/2025-10-24.html) . Medicare withholding has no wage cap, and wages above $200,000 also trigger the 0.9% Additional Medicare Tax, per[IRS Topic No. 560](https://www.irs.gov/taxtopics/tc560) , which the employer must withhold once an individual employee's wages cross that threshold.
5. Run the salary through payroll. Process the shareholder-employee's compensation as W-2 wages, withholding federal income tax and the employee share of FICA, and remit the matching employer share.
6. Document the decision annually. Keep the benchmark sources, the rationale memo, and the payroll records together in the corporate file.
7. Review and adjust every year. Revisit the figure annually as revenue, role, and market benchmarks change.


Expected outcome: a documented, benchmark-supported compensation figure processed through payroll as W-2 wages, with a written rationale retained in the corporate file.


Use this whenever an S corp shareholder performs services for the corporation. Do not apply it to a passive S corp investor who performs no services; a passive investor owes no FICA on distributions.


Setting salary at zero or a token amount is the single riskiest move in this sequence. Compensation set well below market invites the IRS to recharacterize distributions as wages, which brings back FICA taxes, interest, and accuracy-related penalties along with it. Treating distributions as a substitute for salary carries the same exposure. Distributions are not subject to FICA, but they cannot stand in for a reasonable salary tied to services actually performed.


AsureWorks pairs independent benchmark sourcing with payroll configuration in a single workflow, so the rationale memo and the payroll setup exist together before the first payroll run, not stitched together after an IRS notice arrives. That work happens inside a managed service relationship, not a PEO arrangement, so you remain the shareholder-employee's employer of record throughout.


## Workforce Tax Procedures


Once entity obligations and owner compensation are settled, two execution obligations remain: withholding for employees and tax handling for contractors.


### How to Configure Payroll Tax Withholding for W-2 Employees


How to Configure Payroll Tax Withholding for W-2 Employees is the procedure for setting up correct federal and state withholding in a payroll system for each W-2 employee, including an owner-employee at an S corp. It is executed by the payroll administrator at onboarding and produces a verified withholding configuration. Use for every new W-2 hire, including a worker reclassified from contractor status.


**Prerequisites**


- Form W-4 and any applicable state withholding certificate collected from the employee.
- The employee's filing status, allowances, and any additional withholding amount confirmed from the W-4.
- Payroll system access with the current employer FICA and FUTA rates loaded.
- State SUI rate and any local tax rates confirmed, drawing on the obligation matrix from the mapping procedure.


**Steps**


1. Enter W-4 data into the payroll system. Input the filing status, withholding elections, and any additional flat-dollar withholding exactly as the employee stated it.
2. Configure employee FICA withholding. Set Social Security withholding at 6.2% up to the annual wage base, $184,500 for 2026 per the[Social Security Administration](https://www.ssa.gov/news/en/press/releases/2025-10-24.html) , plus Medicare at 1.45%, and add the 0.9% Additional Medicare Tax once wages exceed $200,000, per[IRS Topic No. 560](https://www.irs.gov/taxtopics/tc560) .
3. Configure the employer FICA match. Set the employer share at the same 6.2% Social Security and 1.45% Medicare rates. These are employer-side costs, never withheld from the employee.
4. Configure FUTA. Set federal unemployment tax at 6.0% on the first $7,000 of each employee's wages, reduced by a credit of up to 5.4% for timely state unemployment payments, per the[IRS Instructions for Form 940](https://www.irs.gov/instructions/i940) .
5. Configure SUTA. Enter the state unemployment rate assigned to your employer account and the wage base for each state where you have employees; confirm both directly with your state unemployment agency, since figures vary by state.
6. Run a test payroll calculation. Process one pay period manually and confirm that gross pay, every withholding line, and net pay match what you expect before running live payroll.


Expected outcome: a verified withholding configuration for each W-2 employee that produces correct federal income tax, FICA, FUTA, and SUTA calculations on every pay run.


Use this at every new W-2 hire and after any W-4 update. Do not apply it to a 1099 contractor; no withholding is configured under this procedure for contractor payments.


Running that test cycle inside AsureCentral surfaces a misconfigured filing status or a missed local tax before it ever reaches a live paycheck.


### How to Handle Payroll Tax Obligations for 1099 Contractors


How to Handle Payroll Tax Obligations for 1099 Contractors is the procedure for processing contractor payments with no payroll tax withholding, collecting the required documentation, and meeting year-end filing obligations. It is executed by the payroll or accounts payable administrator throughout the engagement and at year-end and produces a compliant contractor payment record. Use for every worker classified as an independent contractor.


**Prerequisites**


- Worker classification confirmed as an independent contractor through the classification procedure above.
- Form W-9 collected from the contractor before the first payment.
- The contractor's legal name, address, and taxpayer identification number verified against the W-9.
- Accounts payable or payroll system configured to track contractor payments by taxpayer identification number.


**Steps**


1. Verify the W-9 is complete. Confirm it includes a valid taxpayer identification number, a legal name matching IRS records, and a signature. An incomplete W-9 triggers backup withholding, a mandatory 24% withholding on the payment, per the[IRS Instructions for the Requester of Form W-9](https://www.irs.gov/instructions/iw9) .
2. Confirm that no withholding applies. Document in the payment record that this worker is a 1099 contractor and that your company remits no FICA, FUTA, or income tax withholding on their behalf.
3. Process payments at the full agreed amount. The contractor is solely responsible for self-employment tax, 15.3% on net earnings once those earnings reach $400 in a year and requiring Schedule SE, covering both the Social Security and Medicare shares, per the[IRS page on self-employment tax](https://www.irs.gov/businesses/small-businesses-self-employed/self-employment-tax-social-security-and-medicare-taxes) .
4. Track cumulative payments by taxpayer identification number. Record every payment to each contractor throughout the year so you know who crosses the filing threshold.
5. File Form 1099-NEC on time. For calendar year 2026 and after, the reporting threshold is $2,000 paid to a contractor in a year, up from the $600 threshold that governed 2025 payments, per[IRS Notice 2025-62](https://www.irs.gov/pub/irs-drop/n-25-62.pdf) , which implements a provision of the One Big Beautiful Bill Act. File with the IRS and furnish a copy to the contractor by January 31, or the next business day if that date falls on a weekend.
6. Retain the records. Keep W-9s, payment records, and 1099-NEC copies on file for several years, and confirm the right retention window with your CPA. The IRS assessment period can extend well beyond the standard rule in cases involving fraud or an unfiled return.


Expected outcome: a complete contractor payment record with a W-9 on file, no improper withholding applied, and Form 1099-NEC filed on time for every contractor who crosses the reporting threshold.


Use this for every worker confirmed as a 1099 contractor under the classification procedure. If a contractor is later reclassified as a W-2 employee, move immediately to the withholding configuration procedure above, and file Form SS-8 if the earlier classification is disputed.


Waiting until year-end to collect W-9s is the most common operational miss. Chasing down documentation retroactively is painful and risks backup withholding exposure if a contractor is unresponsive. Treating payments under the threshold as invisible is the other one. The $2,000 threshold triggers mandatory 1099-NEC filing, but smaller payments can still carry reporting obligations in other contexts.


AsureWorks folds W-9 collection, taxpayer identification number verification, and 1099-NEC filing into a single onboarding workflow, cutting down the year-end scramble for operators with a growing contractor population, all without any co-employment; you remain the employer of record.


## How to Sequence These Procedures


Run these five procedures in this order, because each one gates the next. Start with obligation mapping regardless of entity type; skipping it is the root cause of most downstream payroll tax errors. Once the obligation matrix is complete, classify every worker before onboarding, since a classification error cannot be corrected retroactively without exposing the business to back taxes and penalties. If any worker is an S corp shareholder-employee, set reasonable compensation before running the first payroll; the salary figure has to exist before withholding can be configured around it. Then configure withholding for every W-2 employee, including the owner-employee. Handle contractor tax obligations in parallel with withholding setup for any 1099 workers, since contractor handling depends on classification being finished, not on withholding being configured.


## Getting the Highest-Stakes Decisions Right


Of the five procedures above, getting S corp compensation and worker classification right carries the most weight. A salary set too low invites the IRS to recharacterize distributions as wages, and a misclassified worker cannot be fixed after the fact without exposure to back taxes, interest, and penalties on every payment already made. Neither problem announces itself until an agency notice arrives.


You can run all five procedures yourself inside AsureCentral, Asure's connected payroll and HR platform, or hand the recurring work to AsureWorks, Asure's managed payroll and HR service, which pairs benchmark sourcing with payroll configuration and handles contractor onboarding as one workflow. AsureWorks is not a PEO. There is no co-employment, and you remain the employer of record either way. The platform stays the same. The choice of who does the work is yours.


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
