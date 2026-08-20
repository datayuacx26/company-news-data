---
schema_version: "1.0.0"
document_id: "b9d72f37bbf77cfd996cf6827fc1a0f7628a1d8f41b5776e56b686fead94aeaa"
company_key: "yc-truewind"
company: "Truewind"
source_id: "yc-truewind-rss-014af42b96d1"
canonical_url: "https://www.truewind.ai/blog/amortization-accounting-definition-examples"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.745562+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:5ddcaa385bb94f797e5d52ddbb18a65229040106d2eaf815e200e64bd126ed45"
---

# Amortization Definition, Formula & Examples in Accounting (July 2026)

Amortization means something different depending on context: in mortgage and real estate, it's the schedule that maps how each payment splits between interest and principal. In accounting, it's the systematic allocation of an intangible asset's cost across its useful life. In tax, goodwill gets amortized over 15 years under Section 197 regardless of how it's treated on the books. Getting those distinctions wrong shows up fast, usually at close, when a loan amortization schedule hasn't been updated or an intangible asset register falls out of sync with the GL. This covers what amortization is in accounting with examples, the amortization formula for both loans and assets, how amortization of goodwill works under GAAP and for tax, amortization vs. depreciation, and the journal entries and amortization schedules behind each.


**TLDR:**


- Amortization spreads a cost across defined periods: either a loan's principal balance or an intangible asset's carrying value
- Use straight-line for intangibles: (Cost - Residual Value) / Useful Life; a $120,000 patent over 10 years = $12,000/year
- Goodwill follows two separate rules: no amortization for public companies under GAAP, but a 15-year straight-line schedule for tax under IRC Section 197
- A missed amortization entry overstates the asset balance and understates expense; correcting it retroactively requires an adjusting entry and prior-period explanation
- Truewind reads GL data on QuickBooks Online or Sage Intacct, generates straight-line journal entries per period, and posts through the API after your team reviews


## What Amortization Is and Why It Shows Up at Close


Amortization is the systematic allocation of a cost across defined periods, whether that cost is a loan's principal balance or an intangible asset's carrying value, so each period absorbs its proportional share of the underlying obligation or consumption.


The close relevance is direct. Missed or misstated amortization entries are among the most consistent sources of balance sheet variance at period end. A prepaid schedule not[rolled forward in accounting](https://www.truewind.ai/blog/rollforwards-in-accounting) leaves an asset balance overstated by the full monthly recognition amount. A loan where the principal reduction wasn't posted leaves both the liability and interest expense wrong at once. Across multiple accounts and a compressed close window, those errors compound quickly.


## Two Types of Amortization: Loan vs. Asset


Amortization applies to two distinct financial contexts, and confusing them is a common source of errors in financial reporting.


The first is loan amortization, which describes how a borrower repays a debt through scheduled payments over time. Each payment covers both interest and principal, with the interest portion shrinking and the principal portion growing as the balance decreases.


The second is asset amortization, which spreads the cost of an intangible asset across its useful life as an accounting expense.


## The Amortization Formula


The basic amortization formula for intangible assets divides the asset's cost by its useful life:


**Amortization Expense = (Cost of Asset - Residual Value) / Useful Life**


For loans, the monthly payment formula accounts for principal, interest rate, and term:


**M = P × \[r(1+r)^n\] / \[(1+r)^n - 1\]**


Where M is the monthly payment, P is the principal loan amount, r is the monthly interest rate, and n is the total number of payments.


### Straight-Line Amortization Example


Straight-line is the most common method for intangible assets. If a company pays $120,000 for a patent with a 10-year useful life and no residual value, the annual amortization expense is $12,000. That same amount hits the income statement each year until the asset is fully amortized.


### Loan Amortization Example


A $200,000 mortgage at 6% annual interest over 30 years produces a fixed monthly payment of roughly $1,199. Early payments are weighted heavily toward interest. As the principal balance falls, each subsequent payment directs more toward principal and less toward interest, which is why an amortization schedule shows the interest-to-principal ratio shifting over the loan term. In month one, $1,000 of that payment covers interest and only $199 reduces principal, leaving a balance of $199,801. By month 12, the split is $990 interest and $209 principal. By month 60, $267 goes to principal and $932 to interest.


## Amortization Schedule Examples


A loan amortization schedule maps every payment across the life of a loan, showing exactly how much goes toward interest versus principal each period.


### Mortgage Example


On a 30-year fixed mortgage of $200,000 at 6% annual interest, the monthly payment comes to roughly $1,199. In month one, about $1,000 covers interest and only $199 reduces the principal. By year 20, that split reverses as the outstanding balance falls.


### Intangible Asset Example


A patent acquired for $120,000 with a 10-year useful life amortizes at $12,000 per year under the straight-line method, reducing the carrying value on the balance sheet by that amount annually until the asset reaches zero.


## How to Calculate Amortization


Two methods cover most amortization calculations you'll encounter: the straight-line method and the effective interest method.


### Straight-Line Method


The straight-line method spreads the cost of an intangible asset evenly across its useful life. The formula is:


**Annual Amortization = (Cost of Asset - Residual Value) / Useful Life**


For example, if a company acquires a patent for $120,000 with a 10-year useful life and no residual value, the annual amortization expense is $12,000.


### Loan Amortization Formula


For loans, each monthly payment covers both interest and principal. The standard formula for a fixed monthly payment is:


**M = P × \[r(1+r)^n\] / \[(1+r)^n - 1\]**


Where M is the monthly payment, P is the principal, r is the monthly interest rate, and n is the total number of payments.


A $200,000 mortgage at 6% annual interest over 30 years produces a monthly payment of roughly $1,199. Early payments go mostly toward interest; later payments shift toward principal reduction.


### Amortization Schedule


An amortization schedule tracks each payment period across the loan's life, showing:


- The opening principal balance for that period
- The interest portion of the payment calculated on the remaining balance
- The principal portion applied to reduce the balance
- The closing balance carried forward to the next period


Loan amortization schedule Excel templates automate this breakdown, recalculating each row as the balance declines. Many borrowers also use a simple monthly amortization calculator to model payoff scenarios before committing to extra payments.


## Amortization of Intangible Assets


Intangible assets are non-physical assets with measurable value: patents, trademarks, customer lists, software licenses, and acquired technology. When a business acquires one of these assets, it spreads the cost across the asset's useful life through amortization, recording a periodic expense in each period instead of a lump sum at acquisition.


### How It Works


The straight-line method is the standard approach. Divide the asset's cost by its useful life, and you get the annual amortization charge.


**Formula:** Annual Amortization = Cost of Intangible Asset / Useful Life


For example, a $500,000 patent with a 10-year useful life produces a $50,000 annual amortization expense. Each year, that amount flows through the income statement and reduces the asset's carrying value on the balance sheet.


### Definite vs. Indefinite Life Assets


Not all intangible assets are amortized.


- Assets with a definite useful life, such as patents, copyrights, customer relationships, and non-compete agreements, are amortized over that life.
- Assets with an indefinite useful life, such as certain trademarks and trade names, are not amortized. Instead, they are tested for impairment at least annually under both US GAAP and IFRS.


### Journal Entry


The standard journal entry records amortization expense as a debit and reduces the intangible asset account as a credit:


Some companies credit the intangible asset account directly, skipping a separate accumulated amortization account. Both treatments are acceptable under US GAAP.


### Treatment in Cash Flow Statements


Because amortization is a non-cash expense, it gets added back to net income in the operating activities section of the cash flow statement, the same way depreciation does.


## Goodwill Amortization: GAAP vs. Tax Treatment


Goodwill sits at the intersection of two very different accounting regimes, and the treatment depends entirely on which set of rules applies.


Under GAAP, goodwill is no longer amortized for public companies. Since[FASB ASC 350](https://www.fasb.org/page/PageContent?pageId=/staticpages/goodwill-impairment-testing.html&isStaticPage=true) took effect, public entities test goodwill for impairment annually; no fixed write-down schedule applies. Private companies have a separate option: under the Private Company Council alternative, they may elect to amortize goodwill on a straight-line basis over a period not to exceed 10 years, which reduces the frequency of impairment testing.


For tax purposes, the rules are different. Under[IRC Section 197](https://www.irs.gov/businesses/small-businesses-self-employed/intangibles) , goodwill acquired in a business purchase is amortized over 15 years on a straight-line basis, regardless of whether the goodwill has an indefinite useful life under GAAP. This creates a common book-tax difference that shows up in deferred tax calculations.


A few specifics worth knowing:


- The 15-year tax amortization period applies to all Section 197 intangibles, goodwill included. Customer lists, non-competes, and trade names acquired in the same transaction follow the same schedule.
- Goodwill amortization is generally tax deductible in asset acquisitions. In stock acquisitions, the buyer does not get a stepped-up basis, so no Section 197 deduction is available unless a Section 338 election is made.
- Internally generated goodwill is not amortizable under either GAAP or tax rules. The Section 197 deduction applies only to purchased goodwill.


The book-tax gap between indefinite GAAP life and 15-year tax amortization is one of the more routine permanent or timing differences finance teams encounter during provision work.


## Amortization vs. Depreciation


Both concepts spread a cost over time, but they apply to different asset types and follow different accounting rules.


Amortization covers intangible assets: patents, licenses, customer lists, and goodwill (for private companies). Depreciation covers physical assets: equipment, vehicles, and buildings. The mechanics look similar on paper, but the treatment diverges in practice.


### Key Differences at a Glance


Factor Amortization Depreciation


Asset type Intangible Physical


Residual value Typically zero Often nonzero


Method Usually straight-line Straight-line or accelerated


Salvage value used Rarely Commonly


Amortization almost always uses the straight-line method, spreading cost evenly across the useful life with no assumed salvage value. Depreciation allows accelerated methods like double-declining balance, which front-loads expense recognition and reflects how physical assets lose value faster early in their life.


The other practical gap is salvage value. A patent expires worthless. A delivery truck still has scrap value. That difference shapes how each expense is calculated from day one.


## Amortization Journal Entry and Financial Statement Impact


When an intangible asset or loan cost is amortized, the journal entry follows a consistent structure: debit Amortization Expense and credit the Accumulated Amortization contra-asset account. This reduces the net book value of the asset on the balance sheet while recognizing the expense on the income statement.


On the cash flow statement, amortization gets added back as a non-cash adjustment under operating activities, since no cash changed hands when the entry was recorded.


## Where Amortization Breaks Down Manually


Manual amortization tracking fails in predictable ways. Schedules built in spreadsheets drift when loan terms change,[prepaid expense schedules](https://www.truewind.ai/blog/prepaid-expenses-schedule-and-journal-entries) go unrecorded, or a formula error in one cell propagates across every subsequent period. Intangible asset registers maintained outside the[GL as system of record](https://www.truewind.ai/blog/gl-system-of-record-automation-tools) fall out of sync with the balance sheet during close, producing mismatches that take hours to trace. Journal entry preparation becomes a recurring bottleneck when each period requires reconstructing figures from a static schedule; pulling from a live calculation would close that gap, which is exactly what a[close checklist](https://www.truewind.ai/blog/building-a-close-checklist-that-actually-works) helps surface before it compounds.


The compounding problem is volume. A single loan amortization schedule is manageable. A portfolio of debt instruments, a library of software costs recorded as intangible assets, and a handful of acquired intangibles running simultaneously across multiple entities is not a workflow; it is a reconciliation backlog waiting to happen.


AI-assisted accounting tools can flag schedule drift, auto-generate period entries from stored terms, and surface exceptions before they reach the trial balance. The question worth asking is whether your current setup catches a missed amortization entry before the books close, or after.


## Amortization in Nonprofit and Family Office Accounting


Nonprofits and family offices deal with amortization in ways that differ from standard corporate accounting, and the distinctions matter for accurate financial reporting.


For[nonprofit accounting teams](https://www.truewind.ai/solutions/non-profit) , amortization shows up most often with intangible assets like software licenses, donor database systems, and website development costs recorded as intangible assets. These get spread across their useful lives on a straight-line basis, reducing the net asset balance on the statement of financial position each period. Grants tied to specific program periods can also carry deferred costs that get amortized as the related expenses are incurred.


Family offices encounter amortization in a few distinct contexts:


- Bond premium and discount amortization affects[investment rollforwards for family offices](https://www.truewind.ai/blog/rollforward-gl-automation) . When a family office holds fixed-income securities purchased above or below par, the premium or discount gets amortized over the bond's remaining term, which changes the effective yield recognized each period compared to the coupon rate.
- Loan origination fees on credit facilities are amortized as interest expense over the loan term, not expensed upfront, which affects[cash vs. accrual](https://www.truewind.ai/blog/cash-vs-accrual-how-to-help-clients-choose-the-right-accounting-method) comparisons across periods.
- Intangible assets from operating company interests, such as trade names or customer relationships acquired through a direct investment, may require separate amortization tracking, which is a common pain point for[family office accounting](https://www.truewind.ai/solutions/family-office) teams managing these outside the GL automatically.


The close implication for both entity types is the same: amortization entries need to post correctly every period without manual recalculation. Misapplied useful lives or stale schedules create flux variances that require explanation during review, and in audit-ready environments, the supporting schedule needs to tie directly to the GL balance.


## Frequently Asked Questions About Amortization


### What is the difference between amortization and depreciation?


Amortization applies to intangible assets like patents, customer lists, and software licenses, while depreciation applies to physical assets like equipment and vehicles. The mechanics are similar, but depreciation often factors in a salvage value at end of life, while amortization almost always runs to zero.


### Can you amortize a fixed asset?


Fixed assets are depreciated, not amortized. The distinction matters in financial reporting because depreciation can use accelerated methods and generally assumes some remaining value at the end of the asset's useful life.


### What happens when you miss an amortization entry?


The asset's carrying value stays overstated on the balance sheet, and the related expense is understated on the income statement for that period, a pattern also seen in[deferred revenue automation gaps](https://www.truewind.ai/blog/sage-intacct-deferred-revenue-automation) . Correcting it retroactively requires an adjusting entry, and in audit-ready environments, you will also need to explain the prior-period gap.


### How do you handle partial-period amortization for mid-month additions?


The most common approach is to prorate based on the number of days the asset was in service during the month, though a half-month convention is also widely used. Whichever method you choose, document it in your accounting policy and apply it consistently across all additions.


### Which accounts does an amortization entry touch?


The debit goes to Amortization Expense and the credit goes to Accumulated Amortization, a contra-asset account that sits directly below the intangible asset on the balance sheet.


## How Truewind Automates Amortization Schedules at Close


Amortization schedules involve repetitive, rule-based calculations that carry real risk when done manually: wrong useful life inputs, missed entries at period-end, and schedules that fall out of sync with the general ledger. Truewind handles this at the execution layer, sitting on top of QuickBooks Online or Sage Intacct as an additional layer, not a replacement.


When an intangible asset is recorded, Truewind reads the GL data, applies the straight-line calculation across the defined useful life, and generates the corresponding journal entries for each period, using the same approach as when[prepaid schedules post into Sage Intacct](https://www.truewind.ai/blog/truewind-posts-prepaid-schedules-into-sage-intacct) directly. Your team reviews and approves; Truewind posts directly through the API. Nothing gets missed at close because the schedule runs as part of the close workflow, not as a separate manual step.


For teams managing multiple intangible assets across entities, this matters. One patent, one software license, and one customer list each have different useful lives and different amortization amounts. Tracking those manually in a spreadsheet across a close cycle is where errors compound. Truewind keeps each schedule tied to its source entry and surfaces any variance before it reaches the trial balance.


The practical outcome: senior accountants stop spending close time rebuilding amortization schedules from prior periods and start spending it on review.


## Final Thoughts on Amortization Calculations, Schedules, and Accounting Treatment


Whether you're tracking a patent, a mortgage, or goodwill acquired in a business purchase, the underlying logic is the same: allocate the cost correctly, post the entry on time, and keep the schedule tied to the GL. The errors that show up at close almost always trace back to a schedule that wasn't updated or an entry that didn't post. Keeping those two things in sync is where the real work is. If your team spends close time rebuilding schedules from prior periods,[a Truewind demo](https://www.truewind.ai/see-a-demo) shows a different way to run it.


## FAQ


### What is amortization in simple words?


Amortization is the process of spreading a cost across defined periods so each period absorbs its proportional share. For intangible assets like patents or software licenses, that means dividing the cost by the asset's useful life and recording a fixed expense each period. For loans, it describes how each scheduled payment covers both interest and principal, with the split shifting toward principal as the balance declines.


### How do you calculate amortization for intangible assets vs. loan amortization?


For intangible assets, the straight-line formula is: Annual Amortization = (Cost of Asset - Residual Value) / Useful Life. A $120,000 patent with a 10-year useful life and no residual value produces $12,000 per year. For loans, the monthly payment formula is M = P × \[r(1+r)^n\] / \[(1+r)^n - 1\], where P is the principal, r is the monthly interest rate, and n is the total number of payments. A loan amortization schedule then maps each payment period, showing exactly how much reduces principal versus covers interest.


### Amortization vs. depreciation: what is the actual difference?


Amortization applies to intangible assets such as patents, customer lists, and software licenses; depreciation applies to physical assets such as equipment and vehicles. Amortization almost always uses the straight-line method and runs the asset's carrying value to zero, since intangibles rarely have salvage value. Depreciation allows accelerated methods like double-declining balance and often factors in a nonzero residual value at end of life.


### Is goodwill amortized under GAAP, and is goodwill amortization tax deductible?


Under GAAP, public companies do not amortize goodwill; they test it for impairment annually under FASB ASC 350. Private companies may elect the Private Company Council alternative and amortize goodwill straight-line over a period not exceeding 10 years. For tax purposes, IRC Section 197 requires goodwill acquired in a business purchase to be amortized over 15 years, regardless of GAAP treatment, and that amortization is generally tax deductible in asset acquisitions. In stock acquisitions, no Section 197 deduction is available unless a Section 338 election is made.


### What happens when you miss an amortization journal entry at close?


The intangible asset's carrying value stays overstated on the balance sheet and the related expense is understated on the income statement for that period. Correcting it retroactively requires an adjusting entry, and in audit-ready environments, you will also need to document the prior-period gap. Across multiple accounts running simultaneously, missed entries compound quickly, which is why amortization schedules need to post as part of the close workflow, not as a separate manual step.
