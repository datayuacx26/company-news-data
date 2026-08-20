---
schema_version: "1.0.0"
document_id: "2f87b78e60c7150fea854f33a4f316d33434c4ab89e66399357d515a4594ddbd"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/asc-718-stock-comp-explained/"
published_at: "2026-07-21T21:10:40+00:00"
first_seen_at: "2026-07-21T21:37:00.066225+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:21fed67c82538effd626f353ba24745ddb82774056407563ab7abedb1aafe3c8"
---

# ASC 718 Stock Compensation: How the 409A Drives the Expense

## What Is a 409A Valuation?


A 409A valuation is an independent appraisal of a private company’s common stock fair market value under IRC Section 409A. Congress added Section 409A to the Internal Revenue Code in 2004, and its primary job is to set a defensible strike price so options are granted at or above fair market value.


That same fair market value does double duty. It’s also a required input to ASC 718 stock compensation, the GAAP standard that expenses equity awards at grant-date fair value. For a private company, the current-price input to that expense is the 409A common stock FMV.


This is the heart of the ASC 718 vs 409A distinction. They aren’t competing standards. They’re two uses of one number, and diligence teams flag any mismatch between them.


What a defensible 409A includes:


- **An independent appraiser.** A qualified, unrelated valuation professional, not an internal guess.
- **A written report.** Dated, methodology-documented, and audit-referenceable.
- **A 12-month shelf life.** Valid up to a year, sooner if a material event lands.
- **A safe harbor path.** The presumption of reasonableness that makes the number defensible.
- **An ASC 718 downstream.** The same FMV becomes the input to your stock-based compensation accounting.


Unlike a priced-round valuation that sets the price of preferred stock, a 409A specifically values common stock for option grants. For the full arc, see[the essential guide to 409A valuations](https://www.indinero.com/resources/the-essential-guide-for-409a-valuations/) .


## Who Needs a 409A


Any private company that grants employee stock options needs a 409A valuation before the first grant. If you issue equity, you need one.


The trigger is the grant, not the size of the company. Common moments that require a fresh 409A:


- **Your first option grant.** Before any employee equity is priced.
- **A new priced funding round.** New preferred class, new common FMV.
- **M&A discussions or an LOI.** A change-of-control signal resets the analysis.
- **IPO preparation.** Auditors and underwriters expect a defensible grant history.


Skip it and the cost is real. Under Section 409A, options granted below fair market value are treated as deferred compensation. That triggers immediate income tax on the spread as it vests, an additional 20 percent federal tax, and interest on the underpayment. Clean common-stock valuation is what keeps the grant compliant, and it flows straight into how you[record equity in accounting](https://www.indinero.com/blog/equity-accounting/) .


There’s a second layer most early teams miss. If you grant equity and report on a GAAP basis, ASC 718 stock compensation expense is mandatory, and the 409A that set the strike is the same 409A you’ll use to value that expense. Many seed and Series A companies run cash-basis and carry no ASC 718 charge yet.


The expense doesn’t disappear. It accrues in the background, and the first GAAP conversion pulls it forward to every grant date.


## The Safe Harbor Framework


The reason auditors and the IRS trust a 409A is the safe harbor presumption in Treasury Regulation Section 1.409A-1(b)(5)(iv)(B). Under that rule, certain valuation methods are presumed to produce a reasonable fair market value, and the IRS can only rebut the presumption by showing the method or its application was grossly unreasonable. The regulation text lives at[26 CFR 1.409A-1](https://www.law.cornell.edu/cfr/text/26/1.409A-1) .


Three methods qualify for the presumption:


1. **Independent appraisal.** A valuation by a qualified independent appraiser, dated within 12 months of the grant. This is the standard path for growth-stage SaaS and mid-market companies.
2. **Binding formula method.** A set formula applied consistently to every transaction, not just option grants.
3. **Illiquid startup method.** An internal written valuation by a qualified person, available to companies generally under 10 years old with no public securities and no anticipated change of control within 12 months.


What voids the presumption: a material misstatement, an omission, or a failure to refresh after a material event. Stale 409As fail. Almost every funded company uses the independent appraisal method, because it produces the cleanest audit posture and the strongest ASC 718 support file. For the eight essentials behind a defensible valuation, see[the 8 essentials of 409A valuations](https://www.indinero.com/blog/irs-409a/) .


## The Valuation Methodology


A 409A appraisal builds enterprise value, then allocates it across the cap table to isolate the common stock FMV. Two steps, two sets of methods.


Building enterprise value uses one or more of three approaches:


- **Income approach.** A discounted cash flow based on your projections.
- **Market approach.** Comparable public companies and precedent transactions.
- **Asset approach.** Net asset value, rarely used for a going concern.


Allocating that value across share classes uses:


- **OPM, the option pricing model.** Treats each equity class as a call option on enterprise value. Most common for growth-stage SaaS with uncertain exit timing.
- **PWERM, the probability-weighted expected return method.** Models discrete exit scenarios, used when an IPO or sale is a defined near-term path.
- **Hybrid.** Blends OPM and PWERM.


The AICPA’s accounting and valuation guidance is the reference most appraisers follow for this work. The output that matters downstream is the per-share common FMV. That single figure becomes the strike price for new grants and the current-price input for your Black-Scholes stock comp calculation.


If the methodology is weak, both the tax position and the book expense inherit the weakness. It’s the same number. That’s why the valuation and the ASC 718 work belong with one team. See[the 409A engagement timeline](https://www.indinero.com/blog/409a-engagement-timeline/) for how the analysis is built step by step.


## What Drives 409A Cost


409A cost tracks complexity, not company size alone. A single-common-class early company runs light. A multi-round, multi-entity company with an active audit runs heavier.


The main cost drivers:


- **Financing history.** More rounds and share classes mean more preferred structure and liquidation preferences to model.
- **Allocation method.** OPM is lighter to run than PWERM or a hybrid.
- **Instrument complexity.** SAFEs, convertible notes, secondary transactions, and the option pool each add work.
- **Audit and ASC 718 support.** When the valuation has to feed both the tax position and an audited book expense, the documentation bar rises.


Standalone 409A platforms price the valuation as a one-off deliverable and stop at the strike price. Indinero bundles bookkeeping, accounting, tax, and fractional CFO advisory under one monthly engagement, with pricing starting at $750/mo, so the 409A isn’t a separate line item you renegotiate every year.


The valuation feeds the same team that runs your close and your tax return. The real savings don’t show up in the appraisal fee. They show up in not paying three vendors to reconcile the same number. For how providers scope and price the work, see[409A pricing information](https://www.indinero.com/blog/409a-pricing-information/) .


## When to Refresh


A 409A is valid for up to 12 months, but a material event resets the clock sooner. Refresh before the next grant, not after.


Under the safe harbor, the appraisal has a 12-month shelf life. These events end it early:


- **A new priced financing round.** New preferred terms change the common FMV. A[priced funding round](https://www.indinero.com/blog/rounds-of-funding/) is the most common trigger.
- **A material acquisition or divestiture.**
- **A significant change in projections or business model.**
- **Secondary sales at a materially different price.**
- **Any move toward an IPO or change of control.**


Granting options on a stale 409A is the most common compliance failure under Section 409A. It breaks the safe harbor presumption, and it undermines the grant-date fair value your ASC 718 expense depends on. Both the strike and the book expense inherit a bad date.


This is where a transactional, once-a-year 409A hurts. With indinero, refresh cadence is coordinated with your funding calendar, because the same team sees the round closing, the projections moving, and the grant schedule at the same time. The refresh happens ahead of the grant cycle, not in a scramble after it.


## The 409A and ASC 718 Connection


The 409A sets the strike, and ASC 718 stock compensation accounting turns that same number into an expense on your income statement. This is the mechanic that ties the whole cluster together.


ASC 718, formally FASB Accounting Standards Codification Topic 718, Compensation, Stock Compensation, and formerly FAS 123R, requires equity-classified awards to be measured at grant-date fair value and expensed over the vesting period. The fair value is fixed on the grant date. It isn’t remeasured when the stock price moves later, unless the award is modified.


Raise the 409A FMV, and the fair value per option rises with it. That’s the connection, stated numerically, and it’s why the strike and the book expense should never live with two different providers.


### How the stock comp expense calculation works


The stock comp expense calculation runs in four steps a Controller can audit line by line.


1. **Measure grant-date fair value per award.** For options, run a Black-Scholes-Merton or lattice-based binomial model. For RSUs and restricted stock, fair value is generally the grant-date common stock FMV from the 409A.
2. **Multiply by awards granted.** Fair value per unit times units gives total award fair value.
3. **Attribute over the service period.** Recognize the total across the vesting period. Straight-line is standard for service-only awards. Graded vesting can use straight-line or an accelerated, tranche-by-tranche method as a policy election.
4. **Adjust for forfeitures.** Under ASU 2016-09, you make an entity-wide election to either estimate forfeitures at grant with a later true-up or record them as they occur.


A four-year option vesting 25 percent a year books roughly a quarter of its total fair value each year, subject to the attribution method and forfeiture policy. The number never moves just because the stock price does later.


### What goes into the Black-Scholes stock comp model


The Black-Scholes stock comp model takes six inputs, and for a private company four of them trace back to the 409A engagement or to public data.


- **Current stock price.** The 409A common stock FMV.
- **Exercise price.** The 409A FMV for at-the-money grants.
- **Expected term.** For plain-vanilla options without enough exercise history, the SEC’s[SAB 107](https://www.sec.gov/rules-regulations/staff-guidance/staff-accounting-bulletins/staff-accounting-bulletin-no-107) simplified method uses the midpoint of the vesting period and the contractual term. A four-year vest with a 10-year term yields roughly 6.25 years.[SAB 110](https://www.sec.gov/rules-regulations/staff-guidance/staff-accounting-bulletins/staff-accounting-bulletin-no-110) extended that method past its original sunset for companies whose own history isn’t a reasonable basis.
- **Expected volatility.** Derived from a set of comparable public guideline companies over a period matching the expected term, since private stock has no trading history.
- **Risk-free interest rate.** The U.S. Treasury zero-coupon yield for a term matching the expected term.
- **Expected dividend yield.** Typically zero for growth-stage companies that don’t pay dividends.


Expected term and expected volatility are the two inputs auditors scrutinize hardest. Both require judgment, not an observable market quote.


### How the expense flows to EBITDA and board reporting


ASC 718 stock compensation expense is a non-cash charge, but it’s a real GAAP expense. It flows through the P&L, reduces operating income, and reduces GAAP net income. On the cash-flow statement it’s added back as a non-cash item, so it never touches cash.


For board decks and investor reporting, the number is almost always added back to reach adjusted EBITDA and disclosed separately as stock-based compensation. This is where consistency matters. The 409A that sets the FMV, the ASC 718 schedule that expenses it, and the adjusted-EBITDA add-back in the board deck should all reconcile to one source.


When the valuation and the accounting sit with two providers, the footnote and the add-back drift, and the audit committee notices. Clean[GAAP accounting for startups](https://www.indinero.com/blog/gaap-accounting-for-startups/) is what keeps those three views tied together.


### The R&D credit trap most teams miss


Here’s the subtle, expensive one. Under IRC Section 41, the wage portion of qualified research expenses must be taxable Form W-2 Box 1 wages, defined by Section 3401(a). The[IRS audit techniques guidance for Section 41](https://www.irs.gov/businesses/audit-techniques-guide-credit-for-increasing-research-activities-ie-research-tax-credit-irc-ss-41-qualified-research-expenses) and the statute at[26 U.S. Code Section 41](https://www.law.cornell.edu/uscode/text/26/41) both anchor on cash wages.


The ASC 718 book charge is non-cash. It is not a Section 3401(a) cash wage, so pulling the GAAP stock-based compensation line straight into your R&D credit wage base is a common and costly error.


What can qualify is the actual taxable spread that hits Box 1 when a nonqualified stock option is exercised or an RSU vests, and only when the employee’s work is qualified research. Incentive stock option spreads, which generally don’t appear in Box 1 at exercise, generally aren’t wages for this purpose. The book expense and the qualified research wage are two different numbers from two different events.


This is why the 409A and the ASC 718 work belong on one desk. Indinero’s CPA team handles both the valuation and the stock-based compensation accounting, so the FMV, the Black-Scholes expense, the adjusted-EBITDA add-back, and the R&D wage base all reconcile to one source of truth. For the standard your investors expect from that reporting, see[why investors expect GAAP](https://www.indinero.com/blog/gaap-what-it-is-why-your-investors-expect-it/) .


## How Indinero Approaches 409A


With indinero, your 409A is one input inside a CFO advisory engagement, not a standalone platform fee or a once-a-year transaction. The same team that runs your bookkeeping, close, and tax sees the 409A inputs in real time, so the valuation and the stock-based compensation accounting stay in sync.


That team runs 409As for a wider range of companies than platform-led providers typically serve. Indinero works with bootstrapped founders, PE-backed operators, LLC and S-Corp structures, and multi-entity growth companies, not only VC-backed Delaware C-Corps. If your cap table doesn’t fit a standard VC template, the valuation still has to be defensible.


A few things follow from doing it this way:


- **One engagement, four connected deliverables.** The 409A, the ASC 718 expense, Section 83(b) election support, and the R&D credit wage base are coordinated by the same people.
- **No platform lock-in.** The 409A is a written report, portable across cap-table providers, not a feature trapped in a subscription.
- **A record you can lean on.** indinero has operated continuously since 2009, serves 500+ regular customers, is SOC 2 compliant (2026), and holds a 5-star Clutch rating.


The point isn’t a cheaper appraisal. It’s that the tax number and the book number come from one place, so nothing drifts between your strike price, your P&L, and your R&D credit. If that isn’t how your 409A works today, it might be time for a different approach. See how the pieces fit on the[409A valuation services](https://www.indinero.com/services/409a-valuation-services/) page, or how the valuation sits inside[fractional CFO services](https://www.indinero.com/services/cfo-services/) .


## Frequently asked questions


Common questions from VP Finance and Controller teams on how ASC 718 stock compensation follows the 409A, from the expense calculation to the Black-Scholes inputs, EBITDA impact, and the R&D credit interaction. Short, direct answers are below.


### What is ASC 718 and how does it connect to my 409A?


ASC 718 is the GAAP standard that expenses equity awards at grant-date fair value, using your 409A common stock FMV as its input. Your 409A sets the option strike price, and that same fair market value becomes the current-price input to the book expense, so the tax and book numbers trace to one figure. Indinero runs both the valuation and the ASC 718 accounting from one CPA team, so they never drift apart.


### How is stock comp expense actually calculated under ASC 718?


Under ASC 718, you measure each award’s grant-date fair value, multiply by units granted, attribute the total over the vesting period, and adjust for forfeitures. For options you run a Black-Scholes or lattice model, while RSUs use the grant-date common stock FMV from the 409A. A four-year option vesting 25 percent annually books about a quarter of its fair value each year, and indinero’s CPA team keeps that schedule tied to the same 409A that set the strike.


### What inputs go into the Black-Scholes calculation?


The Black-Scholes calculation takes six inputs: current stock price, exercise price, expected term, expected volatility, the risk-free rate, and expected dividend yield. For a private company, the current price and exercise price come from the 409A common stock FMV, and dividend yield is typically zero. Expected term often uses the SEC’s SAB 107 simplified method, and expected volatility comes from comparable public companies, the two inputs auditors scrutinize hardest. Indinero pulls the 409A-linked inputs from the same valuation engagement that set your strike price.


### How does stock comp expense affect EBITDA and board reporting?


ASC 718 stock comp expense is a non-cash GAAP charge that reduces operating income and net income, then gets added back to reach adjusted EBITDA. It flows through the P&L but never touches cash, so the cash-flow statement adds it back as a non-cash item. For board decks, the 409A FMV, the ASC 718 schedule, and the adjusted-EBITDA add-back should all reconcile to one source, which is exactly what indinero’s single CPA team delivers.


### How does ASC 718 interact with the R&D tax credit wages calculation?


The ASC 718 book charge is non-cash, so it cannot enter your R&D tax credit wage base, which requires taxable Form W-2 Box 1 wages. Under IRC Section 41, qualified research wages must be Section 3401(a) cash wages, so pulling the GAAP stock-based comp line into the credit is a common, costly error. What can qualify is the taxable Box 1 spread when a nonqualified option is exercised, and indinero’s CPA team keeps the book expense and the R&D wage base as two separate numbers.


### Do I need ASC 718 stock comp expense if I’m not GAAP-reporting yet?


If you report on a cash basis, you carry no ASC 718 stock comp expense yet, which is common for seed and Series A companies. The expense doesn’t disappear, though. It accrues in the background, and your first GAAP conversion pulls it forward to every grant date. Because the same 409A that set your strike will value that expense, indinero tracks both from day one so the GAAP switch isn’t a scramble.


### What changes about ASC 718 at audit and during due diligence?


At audit and in due diligence, ASC 718 raises the documentation bar, and reviewers flag any mismatch between the 409A and the booked expense. Expected term and expected volatility get the hardest scrutiny, since both require judgment rather than an observable market quote. Diligence teams reconcile the strike, the P&L expense, and the R&D wage base to one source, so indinero runs the 409A and the ASC 718 work on one desk to keep that file clean.
