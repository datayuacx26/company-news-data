---
schema_version: "1.0.0"
document_id: "caaedd439c2d60bd6e5b525950ab6d16c4e59ca6bbdd61a411013d34a938a6bb"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/restaurant-accounting"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:14:47.490481+00:00"
fetched_at: "2026-08-06T14:14:48.910031+00:00"
content_hash: "sha256:1a55b425c92759e9de7a0ef4acb921b1885e3793ccb2c2b99fd406e1932f88fa"
---

# Restaurant Accounting: How to Manage Your Restaurant Finances

Restaurants are one of hardest verticals to keep on books. Cash and card revenue arrives from three or four channels, tips flow through payroll on a lag, third-party delivery apps take a commission out of top and remit net, and inventory turns over in days rather than months. Then industry runs on prime-cost margins where a two-point drift in food cost is difference between profit and loss. Most SMB bookkeeping playbooks weren't written for this workflow.


## Prime cost one metric that matters


Prime cost is sum of **Cost of Goods Sold** (food + beverage) and **total labor** (wages, benefits, payroll taxes). Industry benchmarks:


- Full-service restaurant: prime cost ≤ 60% of sales
- Limited-service / QSR: prime cost ≤ 55% of sales
- Bars with light food: prime cost ≤ 50% of sale


Prime cost above 65% is a going-concern signal. Every CPA report to a restaurant client should lead with prime cost, calculated weekly if possible, with food cost and labor cost broken out separately.


Food cost percentage benchmarks by concept:


- Fine dining: 30–35%
- Casual dining: 28–32%
- Pizza / limited menu: 25–30%
- Bars: 18–24%


A restaurant P&L that shows a "food & beverage" line without splitting food, beer, wine, liquor, and non-alcoholic beverage is not useful. Beverage cost is dramatically lower than food cost, and mix drives blended food-cost percentage. The chart of accounts has to split them.


## The 4-week / 13-period accounting cycle


Standard calendar months break down at restaurants because a January with five Fridays does not compare fairly to a February with four. Most multi-unit operators and hospitality-focused CPA firms run on a **4-week / 13-period cycle** year is divided into thirteen 28-day periods, each with same count of Fridays and Saturdays.


Under 13-period accounting:


- Prime-cost trends are actually comparable year-over-year.
- Payroll periods align cleanly (13 × 28 = 364 days, plus a stub week in most systems).
- Menu-mix analysis compares like against like.


Independent operators on a single unit can defensibly stay on calendar months, but any client with more than one location should move to 13-period cycle within first year of engagement.


## Daily deposit reconciliation


The most common bookkeeping failure at a restaurant is deposit-side reconciliation. The chain of events for a single day of sales:


1. POS records sales, taxes, tips, and payments by tender type (cash, credit, gift card, third-party delivery).
2. Credit card processor batches day at cutoff.
3. Merchant deposits arrive one to three days later, net of processor fees.
4. Cash is deposited to bank by operator, minus any over/short from drawer.
5. Third-party delivery platforms (DoorDash, Uber Eats, GrubHub) hold sales and remit weekly, net of commissions.


The daily worksheet must reconcile POS-reported sales to:


- Actual bank deposits (with merchant fee and cash over/short as reconciling items)
- Sales tax accrued to sales-tax liability account
- Tips accrued to a tip-payable account (later cleared through payroll)
- Third-party delivery receivable (cleared when platform remits)


Missing this rec is how sales tax underpayments accumulate silently, how tip liability drifts, and how third-party delivery revenue disappears entirely from books. Every restaurant close begins with previous period's daily deposit rec.


## Chart of accounts a restaurant actually needs


Beyond a standard SMB chart of accounts, a restaurant needs these specific accounts


**Revenue** split by day-part and channel:


- Food Sales Dine-In
- Food Sales Takeout
- Food Sales Third-Party Delivery (gross of commission)
- Beverage Sales Beer
- Beverage Sales Wine
- Beverage Sales Liquor
- Beverage Sales Non-Alcoholic
- Gift Card Redemption (contra to Gift Card Liability)
- Catering / Private Event


**COGS** parallel to revenue:


- Food Cost sub-categorized (proteins, produce, dairy, dry goods)
- Beverage Cost Beer / Wine / Liquor / N-A parallel
- Paper & Packaging (takeout containers, straws)


**Labor** :


- Wages FOH (front of house)
- Wages BOH (back of house)
- Wages Management
- Tips Paid pass-through, not expense
- Payroll Taxes
- Benefits


**Operating expenses** :


- Rent
- Utilities
- Repairs & Maintenance
- Supplies (cleaning, kitchen)
- Marketing
- Third-Party Delivery Commissions (contra revenue in some setups, expense in others)
- Credit Card Processing Fees
- Music / Entertainment Licensing (BMI, ASCAP, SESAC)
- Insurance (GL, workers comp, liquor liability)


**Liabilities** :


- Sales Tax Payable
- Tips Payable
- Gift Card Liability
- Third-Party Delivery Payable (or receivable, depending on timing)


Every account in operating chart should map to a line on P&L that operator actually looks at. Ten "miscellaneous" accounts under Other Operating produce a report operator ignores.


## Third-party delivery reconciliation nobody teaches


Third-party delivery (3PD) is single largest source of P&L confusion at modern restaurants. Two accounting treatments are defensible, and wrong one at scale produces a materially misstated income statement.


**Gross method (preferred).** The full menu price rings through revenue. The delivery platform's commission (typically 15–30%) is recorded as a separate expense "Third-Party Delivery Commissions" below top line. The operator sees true gross margin per dollar of food produced.


**Net method.** Only net remittance from platform is booked as revenue. Commissions are netted out. This shrinks reported top-line revenue and inflates apparent food-cost percentage (since COGS stays constant against a smaller revenue number).


The gross method is what GAAP would require under ASC 606 for a principal-vs-agent analysis where restaurant controls food before delivery. Most CPA firms should book gross. The reconciliation each week: platform payout statement + platform commission report → 3PD sales revenue + 3PD commissions expense, tied to receivable balance.


Missing this is why a Toast POS reporting $80,000 of weekly sales might show as $58,000 in QBO missing $22,000 is uncaptured 3PD gross-up.


## The FICA tip credit Form 8846


Every restaurant with tipped employees is entitled to **FICA Tip Credit under IRC §45B** . The credit equals employer's share of Social Security and Medicare tax (7.65%) paid on employee tips above federal minimum wage rate computed as if minimum wage were pre-2007 $5.15 per hour. For most tipped-employee-heavy restaurants, this credit runs $2,000 to $15,000+ per year.


The credit is claimed on **Form 8846** , flows through Form 3800 (General Business Credit), and offsets federal income tax at entity level (or at partner/shareholder level for pass-through entities). It is one of most consistently missed tax credits on SMB restaurant returns.


Requirements to claim:


- Employees must have received tips (allocated or reported)
- Tips must have been on food and beverage service for consumption on premises (or off-premises in states allowing tipping)
- Federal payroll taxes must have been paid on tips
- No double-dip with Work Opportunity Tax Credit on same wages


Every CPA firm reviewing a restaurant return should verify Form 8846 is attached. If it isn't, three years of amended returns are first conversation.


## Sales tax mechanics


Restaurants operate in one of most complex sales-tax environments in retail. Beyond standard state sales tax, jurisdictions layer:


- **Meals tax** some states or municipalities (Massachusetts, Vermont, several Virginia localities) impose a separate meals tax on top of state sales tax.
- **Alcohol tax** often at a different rate than food.
- **Non-alcoholic beverage tax** some cities apply a sugar or soda tax.
- **Local option taxes** county or city surcharges on prepared food.


The POS should be programmed with correct combined rate per menu category. The daily deposit rec must sanity-check accrued sales tax against posted sales × rate. Any deviation is an error in POS tax setup that will compound until caught.


Sales-tax audits are common in restaurant sector because industry is cash-heavy and state has an interest in verifying reported sales against Form[1099-K](https://www.finlens.app/blogs/1099-k-reporting-cpa-guide) totals from processors. Every restaurant client needs a monthly sales-tax reconciliation and a filing calendar that hits due dates without exception.


## Book-tax reconciliation


Most independent restaurants use cash-basis accounting for tax under IRC §448 (gross receipts under small-business threshold, $31M for 2026) and accrual for book. The book-tax difference each year flows through[Schedule M-1](https://www.finlens.app/blogs/book-to-tax-reconciliation) on corporate return or Schedule M-1 on partnership return:


- **Accrual AR** (uncommon at restaurants but happens with catering) reverses for cash-basis tax.
- **Accrual AP** for food and beverage invoices unpaid at year-end reverses.
- **Gift card liability** under deferral method, gift card sales are deferred for two years for tax; book recognizes on redemption.
- **§179 kitchen equipment expensing** up to $1,220,000 for 2026 property placed in service. Bonus[depreciation](https://www.finlens.app/blogs/depreciation-methods) on balance (40% for 2026 acquisitions).
- **Section 45B FICA tip credit** reduces tax liability, no book impact.


Section 179 on kitchen equipment is largest book-tax difference at a startup or expansion year a $150,000 hood-and-fryer package expensed in full for tax, depreciated over 15 years for book.


## Common failure modes


The recurring bookkeeping errors at restaurants:


- Sales booked from bank deposits rather than POS reports (missing gross → net compression from processor fees).
- Third-party delivery booked net rather than gross.
- Tips expensed as labor rather than passed through tips payable account.
- Sales tax not reconciled to POS category totals.
- Gift card liability never trued up sold gift cards booked as revenue immediately.
- Meals tax and state sales tax comingled in one liability account.
- Comps and voids never reconciled POS reports gross-of-comps, deposit is net.


Every one of these traces back to reconciliation discipline, not to complexity.


## How Finlens keeps a restaurant close on schedule


Finlens reconciles QBO general ledger for restaurant clients to POS, merchant processor, and third-party delivery platforms every day, not just at month-end.


- **POS to QBO daily.** Toast, Square, Clover, or Lightspeed sales journals reconcile to QBO by tender type, with sales tax, tips, gift cards, and 3PD channels booked to correct accounts.
- **Third-party delivery gross-up.** DoorDash, Uber Eats, and GrubHub payouts reconcile against platform commission reports, so revenue reports gross and commissions land in a dedicated expense line.
- **Merchant deposit rec.** Credit card batches match bank deposits with processor fees split out as an expense; discrepancies are queued for review.
- **Tip liability roll-forward.** Tips flow through payable account and clear through payroll cleanly, so balance sheet doesn't accumulate a stale tip liability.
- **Sales tax by jurisdiction.** State, meals, and local option taxes stay in separate liability accounts and reconcile to POS category totals each period.
- **FICA tip credit tracking.** Tips subject to credit are tagged on ledger so Form 8846 builds directly from payroll data.


Finlens does not replace Toast or Restaurant365 operational side (menu, recipe costing, labor scheduling) stays in specialized tool. Finlens is ledger and month-end workpaper layer that keeps numbers defensible.


## Conclusion


**Restaurant accounting is reconciliation-first, prime-cost-second, everything-else-third.** The daily deposit rec, third-party delivery gross-up, and FICA tip credit claim are workpapers that decide whether a client's P&L reflects reality and whether their return leaves money on table.


see how Finlens reconciles Toast or Square to QBO daily, splits third-party delivery gross-of-commission, and tracks tip credit eligibility straight from payroll.


Prime cost


≤ 60% target


13-period


accounting cycle


Form 8846


FICA tip credit


## POS says $80K.
QBO says $58K. Why?


Finlens reconciles Toast or Square to QBO daily, splits third-party delivery gross-of-commission, tracks tip credit eligibility from payroll, and keeps sales tax tied to POS category totals every close.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


Bring file for restaurant client whose books show $58K of Toast weekly sales in QBO against an $80K POS report, no tip credit on last year's return, and a sales tax liability that hasn't tied to POS category totals since March. That's file this workflow is built for.


## Frequently asked questions


### **Should a restaurant use cash or accrual accounting?**


Accrual is more accurate for management reporting (matches food cost to sales it produced). Most independent restaurants file taxes on cash method under §448(c) if under $31M gross receipts threshold. The book-tax difference flows through Schedule M-1.


### **What is a good prime cost percentage?**


55% for QSR / limited-service, 60% for full-service, 50% for bars with light food. Above 65% is a going-concern warning. Prime cost should be reviewed weekly, not monthly.


### **How is third-party delivery revenue reported?**


Gross of commission on revenue line, with commission recorded as a separate expense. Booking net understates top-line sales and misstates food-cost percentage.


### **What is FICA tip credit?**


Under IRC §45B, employers claim a federal income tax credit equal to 7.65% employer FICA/Medicare paid on tips above pre-2007 federal minimum wage. Claimed on Form 8846. Most restaurants qualify; many CPAs miss it on return.


### **Do restaurants need to reconcile daily?**


Yes. The daily deposit rec (POS sales → merchant deposit → bank) is foundation of restaurant accounting. Waiting until month-end guarantees drift.


### **What accounting software works for restaurants?**


QuickBooks Online + a restaurant-specific integration (Toast POS, MarginEdge, or Restaurant365 for larger operators). Sage and Xero also work for smaller independents. The specialty tool handles recipe costing and labor scheduling; QBO handles general ledger.


The authoritative reference for FICA tip credit is[IRS Form 8846 Instructions](https://www.irs.gov/instructions/i8846) . For kitchen equipment side of return Section 179 and bonus depreciation on a hood-and-fryer package see Finlens guide to[Section 179 vs. bonus depreciation](https://www.finlens.app/blogs/section-179-vs-bonus-depreciation) .
