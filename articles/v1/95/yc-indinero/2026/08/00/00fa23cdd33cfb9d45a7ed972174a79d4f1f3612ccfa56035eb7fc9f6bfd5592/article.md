---
schema_version: "1.0.0"
document_id: "00fa23cdd33cfb9d45a7ed972174a79d4f1f3612ccfa56035eb7fc9f6bfd5592"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/multi-state-payroll-compliance-for-growing-saas/"
published_at: "2026-08-17T01:54:44+00:00"
first_seen_at: "2026-08-17T05:41:57.280567+00:00"
fetched_at: "2026-08-17T05:41:59.655052+00:00"
content_hash: "sha256:d9c4bf23bdc853772f95137b9bcc198b3100d38ecb55722e94e8959a1d600dda"
---

# Multi-State Payroll Compliance for Growing SaaS Companies

## What Multi-State Payroll Triggers


Multi-state payroll compliance triggers the day you hire your first remote employee in a new state. Withholding registration comes before the first payroll run. Unemployment insurance registration attaches to the first dollar paid. No revenue threshold applies.


Headcount triggers, not revenue.


Sales tax nexus and income tax nexus have dollar thresholds you can watch on a dashboard. Payroll nexus doesn’t work that way. An employee performing services in a state creates a withholding obligation and an unemployment insurance obligation under that state’s own employer statute, whether or not you have an office, a customer, or a dollar of revenue there.


Companies miss it because the hire moves through recruiting and lands in finance as a line on the payroll register. Nobody asks the tax question. The first real signal is usually a notice from a state revenue department, arriving more than a year after the hire, with penalties and interest already running.


The thresholds are too low to work as a buffer. The[California EDD](https://edd.ca.gov/en/payroll_taxes/Am_I_Required_to_Register_as_an_Employer/) requires registration within 15 days of paying more than $100 in wages in a calendar quarter. New York’s unemployment insurance liability begins on the first day of any calendar quarter in which you pay $300 or more in remuneration. Federal FUTA liability attaches at $1,500 of quarterly wages.


A single part-time engineer clears all three in week one.


Trigger events worth flagging at offer-letter stage:


- **A first remote hire in a state where you have no employees.** The most common trigger, and the one most often missed entirely.
- **An existing employee relocating.** No requisition, no offer letter, no HR workflow. The withholding map changes anyway.
- **A sales hire working from a home office.** Field sales creates payroll obligations and frequently income tax nexus in the same week.
- **Headcount growth past a state’s employer-size cutoffs.** Several paid-leave programs exempt small employers from the employer share, and those exemptions expire as you grow.
- **An employee working across a state line.** Reciprocity may apply, but only if you collect the right certificate.


New York-headquartered companies carry an extra wrinkle. New York applies a convenience-of-the-employer test, so a nonresident whose primary office is in New York has telecommuting days treated as New York working days unless the employer has established a bona fide employer office at the telecommuting location. A fully remote engineer in Florida can still generate New York-source wages. The nexus map and the withholding map are not the same map, which is worth reading alongside how[payroll and physical presence create state tax nexus](https://www.indinero.com/blog/startup-tax-nexus/) .


## Federal Requirements


Federal payroll obligations don’t change when you add states. They change when you add people. The federal layer is also where the penalties bite hardest, because trust fund taxes carry personal liability.


Form Purpose Due date Late-filing exposure


Form 941 Quarterly federal income tax withheld, plus employer and employee Social Security and Medicare April 30, July 31, October 31, January 31. 10 extra days if all deposits were timely Failure-to-deposit penalties of 2 to 15 percent, plus daily compounding interest


Form 940 Annual FUTA return January 31, or February 10 if all FUTA deposits were timely Quarterly deposits required once liability passes $500 for the year


Forms W-2 and W-3 Annual wage statements to employees and the Social Security Administration February 1, 2027 for tax year 2026 $60 to $340 per return, $680 for intentional disregard


Form 1099-NEC Nonemployee compensation paid to contractors January 31 Same information return penalty schedule as W-2


Form 8974 Carries the elected R&D credit onto the employment tax return Attached to each Form 941 No penalty, but a missed election defers the cash


For 2026, Social Security runs 6.2 percent on the employer and 6.2 percent on the employee, capped at the $184,500 wage base. Medicare is 1.45 percent on each side with no cap. The[IRS instructions for Form 941](https://www.irs.gov/instructions/i941) carry the current calendar, and our own[walkthrough of the quarterly Form 941](https://www.indinero.com/blog/form-941/) covers the line-by-line mechanics.


Multi-state employers hit one Form 940 wrinkle that catches finance teams late in the year. Schedule A allocates wages by state and computes credit reduction. A state with outstanding federal unemployment loan balances on January 1 for two consecutive years, that doesn’t repay by November 10 of the second year, becomes a credit reduction state. A 0.3 percent reduction lifts the effective FUTA rate there to 0.9 percent. The Department of Labor doesn’t publish the final list until November, and the extra liability is treated as incurred in the fourth quarter and due January 31. That true-up belongs in the forecast by Q3.


### Deposit schedules and the four-tier late penalty


Deposit frequency is set by your lookback period, not by preference. Report $50,000 or less in the lookback period and you’re a monthly depositor. Above $50,000, semiweekly. A monthly depositor that accumulates $100,000 of liability on any single day becomes a semiweekly depositor the next day, which is exactly what a large equity vest or a bonus cycle does to a growing SaaS payroll.


Late deposits are penalized under IRC section 6656 on a four-tier schedule, published on the[IRS failure-to-deposit penalty page](https://www.irs.gov/payments/failure-to-deposit-penalty) :


Days late Penalty


1 to 5 calendar days 2 percent of the unpaid deposit


6 to 15 calendar days 5 percent of the unpaid deposit


More than 15 calendar days 10 percent of the unpaid deposit


More than 10 days after the first IRS notice, or upon an immediate payment notice 15 percent of the unpaid deposit


The tiers don’t stack. You pay the highest applicable rate, not the sum. Interest runs on top of it. For the calendar quarter beginning July 1, 2026, the federal underpayment rate is 7 percent compounded daily, and 9 percent for large corporate underpayments.


### The 1099-NEC threshold moved to $2,000


The One, Big, Beautiful Bill Act raised the Form 1099-NEC reporting threshold from $600 to $2,000 for payments made after December 31, 2025, with inflation indexing after 2026. The $600 figure had been in place since 1954, which is why most payroll checklists and most competing guides still print it. Treasury and the IRS estimated that for tax year 2024, roughly 3.3 million payors filed 18.8 million Forms 1099-NEC reporting payments of at least $600 and less than $2,000.


Two cautions before you change your contractor process. State thresholds did not automatically move with the federal one, so check each state’s own 1099 filing rule. And electronic filing is mandatory once you file 10 or more information returns in aggregate across the W-2, 1099, 1098, and 1095 series combined. Ten is a low bar for a 30-person company that also pays contractors. If you’re sorting out which form applies to which payee, start with the[1099 form types and what each one reports](https://www.indinero.com/blog/types-of-1099-forms/) .


### The penalty that reaches personal balance sheets


Under IRC section 6672, a responsible person is personally liable for 100 percent of unpaid trust fund taxes, meaning withheld income tax and the employee share of FICA. It doesn’t extend to the employer share of FICA or to FUTA. A responsible person can be an officer, a partner, a sole proprietor, or an employee with the duty and power to direct collecting, accounting for, and paying over trust fund taxes.


Founders, CFOs, and controllers all fit that definition. Corporate structure doesn’t shield them.


One federal item almost nobody uses is worth 20 minutes. Employers must report each new hire to the State Directory of New Hires within 20 days of the date of hire. A multistate employer that reports electronically may elect to report all new hires to a single state, after notifying the Secretary of Health and Human Services in writing of the chosen state. For a company hiring across ten states, that single election removes nine reporting tracks.


## State Requirements


Remote employee state tax registration is four registrations per state, not one. The remote employee payroll setup sequence runs in this order:


1. **State department of revenue or taxation.** The income tax withholding account. Required before the first payroll run in that state.
2. **State department of labor or workforce agency.** The unemployment insurance account, with its own experience rate assignment and its own rate notice cycle.
3. **Workers’ compensation coverage.** Mandatory in every state except[Texas](https://www.indinero.com/state-tax-guides/texas-tax-rates/) , where coverage is elective for most private employers. Texas non-subscribers have to notify both the Division of Workers’ Compensation and their employees in writing.
4. **State-specific insurance and paid-leave programs.** California SDI, New York DBL and PFL, New Jersey TDB and FLI, Washington PFML, Colorado FAMLI, and Minnesota Paid Leave, among others.


Account numbers typically arrive in 2 to 6 weeks. Payroll platforms can’t file without them. That’s why the registration decision belongs at offer-letter stage, not at first-payroll stage.


### Rates and wage bases vary by an order of magnitude


California sets a UI taxable wage limit of $7,000 per employee per year, a 3.4 percent new employer rate, and an Employment Training Tax of 0.1 percent on the same $7,000. Washington’s UI taxable wage base for 2026 is $78,200, up from $72,800 in 2025. Same employee, same salary, and Washington’s base is more than eleven times California’s. Any per-employee cost model running one blended UI rate across states is wrong before it’s built.


The state disability and paid-leave layer moves independently of unemployment insurance:


State Program 2026 rate Wage base Split


California SDI 1.3 percent employee No wage cap since January 1, 2024 Employee-funded


New York PFL 0.432 percent of gross wages Max annual employee contribution $411.91 Employee-funded


New Jersey TDI and FLI Employee 0.23 percent, employer TDI 0.10 to 0.75 percent Employee base $171,100, employer base $44,800 Split


Washington PFML 1.13 percent total, up from 0.92 percent Up to $184,500 Employer 28.57 percent, employee 71.43 percent, employers under 50 exempt from the employer share


Colorado FAMLI 0.88 percent total Up to $184,500 0.44 percent each, employers under 10 nationwide exempt from the employer share


Minnesota Paid Leave 0.88 percent total, 0.66 percent small employer rate Statutory cap Employer may collect up to 0.44 percent from employees


If you’re modeling a first hire in a specific jurisdiction, our state guides carry the surrounding income tax picture for[California](https://www.indinero.com/state-tax-guides/california-tax-rates/) ,[New York](https://www.indinero.com/state-tax-guides/new-york-tax-rates/) , and[Washington](https://www.indinero.com/state-tax-guides/washington-tax-rates/) .


### The paid-leave map keeps moving


Minnesota Paid Leave launched January 1, 2026, with contributions and benefits starting at the same time. Delaware Paid Leave began collecting contributions in January 2025 with benefits starting January 2026. Maine’s Paid Family and Medical Leave program began collecting in January 2025 with benefits beginning May 2026, at 1.0 percent for employers with 15 or more employees and 0.5 percent for smaller ones. Maryland FAMLI contributions begin January 2027 at 0.9 percent split evenly, with benefits beginning January 2028.


Four programs have changed status in roughly eighteen months. If your payroll cost model was built in 2024, it’s already stale in at least four states, and the variance shows up in gross margin before anyone flags it in a rate notice.


### Reciprocity is paperwork, not a setting


Reciprocity means the work state agrees not to tax a resident of the other state on wages subject to employer withholding. It removes double withholding, but only when the employer collects the certificate. Pennsylvania maintains reciprocal agreements with Indiana, Maryland, New Jersey, Ohio, Virginia, and West Virginia.


The New Jersey and Pennsylvania pair is the one SaaS companies hit most, and it has a specific mechanic. An employer isn’t required to withhold New Jersey tax on compensation paid to a Pennsylvania resident working in New Jersey if the employee files Form NJ-165, the Employee’s Certificate of Nonresidence in New Jersey, and the employer retains it. If the employee doesn’t complete the certificate, the employer must withhold New Jersey tax.


Two limits are worth knowing. The agreement covers compensation only, so it doesn’t extend to non-employee compensation. And it doesn’t excuse a Pennsylvania employer from withholding local wage taxes, including the City of Philadelphia wage tax.


The default in payroll software is to withhold.


### Filing cadence runs in parallel with federal


Form 941 does not satisfy state obligations. Each state has its own return and its own calendar. California requires the DE 9 quarterly contribution return and the DE 9C employee-level wage detail, plus DE 88 deposits. Late payments draw a 15 percent penalty plus interest, and a $20 penalty per unreported employee applies if the DE 9C isn’t filed within 15 days of a written demand, according to the[EDD’s required filings and due dates](https://edd.ca.gov/en/payroll_taxes/required_filings_and_due_dates/) . New York consolidates withholding, wage reporting, and unemployment insurance into Form NYS-45, filed electronically.


Ten states means ten registration tracks, ten quarterly filings, ten annual rate notices, and ten separate penalty regimes. The federal calendar is the easy part.


## Common Mistakes


Most multi-state payroll failures aren’t filing errors. They’re decisions made before anyone asked the tax question.


Start with the number that reframes the first two mistakes. California’s doing-business test under Revenue and Taxation Code section 23101(b) treats a taxpayer as doing business in the state if California compensation exceeds the lesser of an indexed threshold or 25 percent of total compensation. For 2026, the[California Franchise Tax Board](https://www.ftb.ca.gov/file/business/doing-business-in-california.html) sets that compensation threshold at $75,707. Two senior engineers at market SaaS comp clear it on their own. The entity is now doing business in California, owes the $800 minimum franchise tax, and files a California return. That’s an entity-level tax question wearing a payroll costume, and it’s the kind of call our[business tax services](https://www.indinero.com/services/business-tax-services/) team makes before the offer letter goes out.


1.


**Registering too early.** Registering in a state before you have an employee there isn’t a harmless precaution. Registration is frequently the same act as, or an admission of, doing business in the state, which pulls in entity-level minimum taxes. A foreign corporation that qualifies with the California Secretary of State owes the $800 minimum franchise tax whether or not it has income, and Massachusetts imposes a $456 minimum corporate excise. **The correction:** register when the hire is signed, not when the hiring plan is drafted. Speculative registration across twelve states on a plan that later gets cut buys a permanent filing footprint you then have to formally withdraw from.


2.


**Registering too late.** The mirror-image error, and the more expensive one. Late registration produces retroactive liability plus penalties plus interest, and it frequently triggers a state examination of the entire employment relationship. Missed withholding creates IRC section 6672 exposure federally, where a responsible person is personally liable for the full trust fund portion. **The correction:** most states run voluntary disclosure programs that cap the lookback period, and they cost far less than a state-initiated assessment. That decision has to be made before the state contacts you, not after.


3.


**Missing reciprocity certificates.** Double withholding across a reciprocal pair is refundable at year end. It’s still a cash flow hit for the employee for up to twelve months, and it generates support tickets that land on the finance lead’s desk in the middle of close. **The correction:** collect the certificate at onboarding, not after the first paycheck. Treat NJ-165 and its equivalents as part of the new-hire packet, the same way you treat an I-9.


4.


**Assuming the payroll platform will tell you.** Gusto, Rippling, and ADP each run multi-state filings competently once accounts exist. What payroll tax filing services don’t do is decide whether to register, when to register, or whether the entity-level nexus consequence is acceptable. That requires knowing your entity structure, your state apportionment, and your hiring roadmap. **The correction:** treat registration as an advisory decision with a named owner, and give that owner visibility into the hiring plan.


5.


**Misclassifying workers.** Consequences run both directions. Treating an employee as a contractor produces back withholding, back FICA, back unemployment contributions, and potential section 6672 personal liability. Treating a contractor as an employee produces unnecessary employer FICA, unemployment contributions, and state registrations you may not have needed. Federal posture is in flux, with a February 26, 2026 Notice of Proposed Rulemaking to rescind the 2024 independent contractor rule and Field Assistance Bulletin No. 2025-1 directing investigators not to apply that rule’s analysis in current enforcement. State posture is not in flux. States running an ABC test, California under AB 5 among them, are unaffected by federal rulemaking. **The correction:** classify against the strictest applicable test, which is usually the state’s. Our breakdown of[employee versus contractor status](https://www.indinero.com/blog/employee-vs-contractor/) covers where the lines actually fall.


6.


**Missing the Form 8974 election.** For an R&D-heavy SaaS company, this is the mistake with the largest dollar figure attached, and it’s the one a payroll platform will never surface. The next section walks the whole chain.


## The R&D Credit + Payroll Connection


For a pre-profit SaaS company, engineering payroll is both the largest cash outflow and the largest input into the R&D credit. The chain runs in one direction. Form 6765 Section D carries the payroll tax election, Form 8974 translates the elected amount into a quarterly claim, and the credit lands on Form 941 line 11 against that quarter’s employment tax liability.


Most generalist firms run the credit study and the payroll calendar as two unrelated projects. They’re the same project.


### Who qualifies: the two-part QSB test


Under IRC section 41(h), a qualified small business making the payroll tax election has to clear two tests. Gross receipts of less than $5 million for the tax year. And no gross receipts for any tax year before the 5-tax-year period ending with the tax year.


The second test is the one that quietly disqualifies companies. A business with any gross receipts more than five years ago fails, regardless of how small those receipts were. A $4,000 consulting invoice in year one closes the door in year six. Your first revenue dollar starts a five-year clock on the election, which means a company that expects to stay pre-profit for a while should be modeling the election window from the first invoice, not from the first credit study. Our[R&D tax credit guide](https://www.indinero.com/resources/rd-tax-credit-guide/) covers the qualification path in more depth.


### How the $500,000 cap actually applies


Section 13902 of the Inflation Reduction Act of 2022 raised the maximum payroll tax research credit a qualified small business may elect from $250,000 to $500,000, for tax years beginning after December 31, 2022. That remains the cap.


The split is widely misstated, and it matters for modeling. Per the[IRS instructions for Form 8974](https://www.irs.gov/instructions/i8974) , the first $250,000 can only be used against the employer share of Social Security tax, capped at $250,000 per quarter. Any remaining credit reduces the employer share of Medicare tax for the quarter. Unused credit carries forward.


Run the arithmetic against your own payroll. Employer Social Security is 6.2 percent of wages up to $184,500 per employee in 2026, so roughly $4 million of qualifying payroll generates about $248,000 of employer Social Security tax annually. That’s the practical ceiling on how fast the first tranche gets absorbed, and it’s the number that tells you whether the full $500,000 is realistically reachable this year. The mechanics of[applying the R&D credit against payroll taxes](https://www.indinero.com/blog/rd-tax-credit-against-payroll-taxes/) go quarter by quarter.


### The timing rule that makes extensions a cash decision


The credit can first be claimed in the first calendar quarter beginning after the date the qualified small business filed its income tax return. Read that carefully, because it turns a compliance date into a cash date.


A calendar-year company that files its 2026 return on March 15, 2027 can first claim on the Q2 2027 Form 941, filed July 31, 2027. The same company on extension to September 15, 2027 doesn’t claim until Q4 2027, filed January 31, 2028. Two quarters of offset, deferred by a filing choice that usually gets made for reasons that have nothing to do with payroll.


The extension decision is a cash decision.


That connection stays invisible unless one team sees both the corporate return and the payroll calendar. When the tax preparer, the bookkeeper, and the payroll advisor are three different firms, nobody owns the sequencing.


### Section G, and what current law actually says about Section 174


The redesigned Form 6765 includes Section G, business component detail. Section G is optional for all filers for tax years beginning before 2026 and required for tax years beginning after 2025. For a calendar-year SaaS company, that means the 2026 return filed in 2027 has to carry business component reporting. Substantiation gets built during 2026, not reconstructed in 2027, and an R&D study assembled from an org chart rather than from payroll data will not hold up.


On expensing, the current law is not what most published content still says. The One, Big, Beautiful Bill Act added section 174A, allowing a deduction for domestic research or experimental expenditures paid or incurred in tax years beginning after December 31, 2024. Revenue Procedure 2025-28 supplies the election and accounting-method-change procedures, including a small business retroactive method for tax years beginning after December 31, 2021 and before January 1, 2025, and a late section 280C(c)(2) election for eligible small business taxpayers. Domestic research is expensable again. Any guide still describing mandatory five-year domestic amortization is out of date.


Qualified research expenses under IRC section 41(b) are in-house wages for qualified services, supplies, and 65 percent of contract research. For a software company with no lab and no materials, wages dominate the calculation. The W-2 Box 1 figure, the employee-level time allocation, and the project mapping all originate in payroll, which is why a payroll process that captures nothing but “engineering, $X” leaves the credit undocumented and the[Form 6765 instructions](https://www.irs.gov/instructions/i6765) unsatisfiable at Section G.


## How Indinero Handles Multi-State Payroll


Indinero doesn’t sell payroll software. We handle the layer above it, the one that decides what the software gets told to do. Most payroll services for startups stop at execution, and execution was never the hard part.


- **We support the platform you already run.** Gusto, Rippling, and ADP all file multi-state competently once accounts exist. There’s no reason to rip out a working platform. Platform choice matters far less than whether anyone is making the registration and nexus calls the platform doesn’t make.
- **We make the registration call before the offer goes out.** Whether to register, when, with which agencies, and what the entity-level consequence is. A California hire is an $800 minimum franchise tax decision, not a payroll setup ticket.
- **We connect payroll to the close.** Payroll accruals, PTO liability, employer tax accruals, and the FUTA credit reduction true-up land in the same general ledger our[bookkeeping team](https://www.indinero.com/services/online-bookkeeping-services/) is already closing. A payroll platform can’t reconcile a close it doesn’t run.
- **We run Form 6765 to Form 8974 to Form 941 as one workflow.** Wage data feeds the credit study, the study drives the Section D election, the election flows to Form 8974, and the credit lands on line 11. Same team, same data, same calendar, including the extension timing that decides which quarter the cash arrives.
- **We handle equity comp payroll alongside 409A and ASC 718.** Option exercises, RSU vesting, and the supplemental withholding that follows are payroll events driven by valuation work, and they’re multi-state events when your engineers are.


All of it sits inside one monthly engagement with bookkeeping, accounting, tax, and[fractional CFO advisory](https://www.indinero.com/services/cfo-services/) , so your payroll position, your tax position, and your financial statements stay consistent instead of reconciling to each other twice a year. Continuous operations since 2009, 500+ regular customers, and 100+ years combined team experience sit behind that work.


Payroll software executes. Nobody’s deciding.


Multi-state payroll compliance rarely fails at the filing. It fails at the decision nobody was assigned. If your hiring plan is moving faster than your registration list, reach out for a free consultation. We’d love to learn about your business and find where the gaps are.


## Frequently asked questions


A handful of questions come up on nearly every multi-state payroll engagement, usually within a week of the first out-of-state offer being signed. Here are the ones we answer most often.


### What actually triggers multi-state payroll compliance?


Hiring a single employee in a new state triggers multi-state payroll compliance, regardless of revenue, customers, or whether you have an office there. Withholding registration is due before the first payroll run in that state. Unemployment insurance registration attaches to the first dollar paid. California requires registration within 15 days of paying more than $100 in wages in a calendar quarter, and New York’s unemployment liability begins at $300 of remuneration in a quarter. A part-time engineer clears both in week one.


### What federal payroll filings does a growing SaaS need to file?


A growing SaaS files Form 941 quarterly, Form 940 annually, Forms W-2 and W-3 for employees, and Forms 1099-NEC for contractors paid $2,000 or more. Form 941 is due April 30, July 31, October 31, and January 31. Form 940 is due January 31, or February 10 with timely deposits. W-2 and W-3 filings go to the Social Security Administration by February 1, 2027 for tax year 2026. R&D-heavy companies also attach Form 8974 to each Form 941, which is where indinero sequences the credit.


### What state registrations kick in when I hire a remote employee?


A remote hire usually requires four state registrations, not one: income tax withholding, unemployment insurance, workers’ compensation coverage, and any state-specific paid-leave program. Withholding registration goes through the state revenue department and has to be done before the first payroll run. Unemployment insurance runs through the state labor agency with its own experience rate. Workers’ compensation is mandatory everywhere except Texas for most private employers. Account numbers typically take 2 to 6 weeks, which is why indinero makes the registration call at offer-letter stage.


### What are the most common multi-state payroll mistakes?


Common multi-state payroll mistakes are registering too early, registering too late, missing reciprocity certificates, misclassifying workers, and assuming the payroll platform will tell you. Registering speculatively buys entity-level minimum taxes you didn’t need, like California’s $800 minimum franchise tax or the $456 Massachusetts minimum corporate excise. Registering late buys retroactive liability, penalties, interest, and often a state exam. Missing a certificate such as New Jersey Form NJ-165 double-withholds an employee for up to a year.


### How does the R&D tax credit interact with payroll for QSBs?


A qualified small business can elect to apply up to $500,000 of its section 41 R&D credit against payroll taxes instead of income tax. Qualifying takes two tests under IRC section 41(h). Gross receipts under $5 million for the tax year, and no gross receipts for any tax year before the 5-tax-year period ending with that year. Your first revenue dollar starts a five-year clock on the election. Wages dominate qualified research expenses for a software company, so payroll data is both the input and the offset.


### What is Form 8974 and how does the payroll offset election work?


Form 8974 carries the R&D credit elected on Form 6765 onto the employment tax return, where it applies on Form 941 line 11. Of the $500,000 annual cap, the first $250,000 offsets only the employer share of Social Security tax, capped at $250,000 per quarter. Any remaining credit reduces the employer share of Medicare tax for that quarter, and unused credit carries forward. The credit is first claimable in the first calendar quarter beginning after the income tax return is filed, so an extension defers the cash.


### Can my existing payroll provider (Gusto, Rippling, ADP) handle multi-state by default?


Gusto, Rippling, and ADP file multi-state payroll well once accounts exist, but registering you in a new state is a separate service with real gaps. None of them decides whether to register, when to register, or whether the entity-level nexus consequence is acceptable. A provider filing on your behalf doesn’t shift liability either. Under IRC section 6672, a responsible person at your company stays personally liable for unpaid trust fund taxes. Indinero makes those registration calls on the platform you already run.
