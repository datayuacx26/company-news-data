---
schema_version: "1.0.0"
document_id: "140e8ec0396e0204a1c3ca7397c9e3ec10f02320e96614ace1f4ec4369ef3466"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/delaware-franchise-tax-calculator-guide/"
published_at: "2026-08-16T01:26:10+00:00"
first_seen_at: "2026-08-16T03:33:31.835012+00:00"
fetched_at: "2026-08-16T03:33:33.358679+00:00"
content_hash: "sha256:7cc93174f5045f54a152a2265982adb63d4c995fb1f1ee17ab9b56a14c058124"
---

# Delaware Franchise Tax: Calculator and 2026 Guide

## What Is Delaware Franchise Tax?


Delaware franchise tax is the annual price of a Delaware charter under 8 Del. C. Section 501, and every Delaware franchise tax startup owes it. It is not a tax on franchise businesses. It is not an income tax. It is not triggered by nexus, payroll, or a dollar of Delaware revenue.


That distinction is the whole logic of the tax. A pre-revenue company with $0 of income, a full net operating loss, no employees, and no Delaware footprint owes the same $175 minimum as a profitable one, plus the $50 annual report fee. Delaware’s own guidance answers the “I haven’t started doing business yet” question by citing Section 503 and saying yes, you still file and you still pay. If the concept itself is new to your team, start with[what a franchise tax actually is](https://www.indinero.com/faq/what-is-franchise-tax/) .


Two mechanics turn that minimum into a real line item for growth-stage companies.


**Delaware defaults you into the expensive method.** The state prints tax notices in December and mails them to your registered agent, not to your office. Those notices are computed on the Authorized Shares Method, because authorized shares are the only figure Delaware already holds. It doesn’t know your issued share count or your total gross assets until you tell it.


**The recalculation is elective and self-service.** Section 503 lets a corporation pay the lesser of the two computations. Delaware publishes a[Delaware franchise tax calculator](https://corp.delaware.gov/taxcalc/) through the Division of Corporations, but it only computes what you feed it, and the numbers arrive inside your annual report filing.


Delaware won’t run it for you.


### The Delaware franchise tax authorized shares method


The default method needs one input. Authorized shares, nothing else.


Authorized shares Tax


5,000 or fewer $175, the minimum


5,001 to 10,000 $250


Each additional 10,000 shares, or any fraction of a block add $85


Maximum $200,000


Above 10,000 shares the formula is $250 plus $85 times the number of 10,000-share blocks, rounded up. Delaware publishes two check figures on its[How to Calculate Franchise Taxes](https://corp.delaware.gov/frtaxcalc/) page, and both are worth testing your spreadsheet against. A corporation with 10,005 authorized shares owes $335, because $250 plus one $85 block covers those 5 extra shares as a full fraction. A corporation with 100,000 authorized shares owes $1,015, which is $250 plus nine blocks at $85.


Here is a figure competing guides don’t publish. The $200,000 cap is reached at exactly 23,510,000 authorized shares, because $250 plus 2,350 blocks at $85 equals $250 plus $199,750. Listed companies above certain revenue and asset tests pay a flat $250,000 as Large Corporate Filers under Section 503(c), a category no private venture-backed company reaches.


### The Delaware franchise tax assumed par value method


The alternative method needs four inputs: authorized shares by class, issued shares by class, stated par value by class, and total gross assets. Six steps.


1. **Compute assumed par.** Divide total gross assets by total issued shares across all classes. Delaware carries this to six decimal places.
2. **Test each class.** Compare assumed par to that class’s stated par value.
3. **Build the capital base per class.** Multiply the class’s authorized shares by whichever figure is greater.
4. **Sum the classes** to get assumed par value capital.
5. **Round up to the next whole $1,000,000** and multiply by $400.
6. **Apply the floor and the cap.** Minimum $400, maximum $200,000.


Two rules trip nearly everyone. The per-class comparison uses the greater of assumed par and stated par, so a class carried at $5.00 stated par gets billed on $5.00 even when assumed par is $2.06. And the multiplier in step 3 is authorized shares, not issued shares.


Total gross assets means the “total assets” line reported to the IRS on Form 1120 Schedule L, line 15, column (d), for the fiscal year ending in the calendar year before you file, per 8 Del. C. Section 503(i). Gross, not net. Don’t subtract liabilities, deferred revenue, or convertible notes. On this method the $200,000 cap bites at $500,000,000 of assumed par value capital, since 500 whole millions at $400 each is $200,000.


### Example A. The Series A case, Assumed Par Value wins by $81,165


**Facts:** 10,000,000 authorized common shares, single class. 5,000,000 issued. $0.0001 stated par. $5,000,000 total gross assets at fiscal year end December 31.


**Authorized Shares Method**


Step Arithmetic Result


1. Base for first 10,000 shares flat $250


2. Shares above 10,000 10,000,000 minus 10,000 9,990,000


3. Blocks of 10,000 9,990,000 / 10,000 999 blocks


4. Block charge 999 x $85 $84,915


5. Total $250 plus $84,915 $85,165


**Assumed Par Value Capital Method**


Step Arithmetic Result


1. Assumed par $5,000,000 / 5,000,000 issued $1.000000


2. Compare to stated par $1.000000 vs $0.0001 assumed par greater, use $1.000000


3. Assumed par value capital $1.000000 x 10,000,000 authorized $10,000,000


4. Whole millions, rounded up $10,000,000 / $1,000,000 10


5. Tax 10 x $400 $4,000


6. Floor test $4,000 vs $400 minimum $4,000 stands


Pay $4,000. Add the $50 annual report fee and the March 1 wire is $4,050. The December notice said $85,215. Savings from running the second method: $81,165. Because $4,000 sits below the $5,000 estimated payment threshold in 8 Del. C. Section 504(a), this company pays once a year with no June, September, or December installments.


### Example C. The reversal, Authorized Shares wins by $40,035


This is the case almost every startup guide gets wrong. Assumed par value capital scales with total gross assets, so a company that closes a large round and parks the cash can flip the answer.


**Facts:** 10,000,000 authorized shares, the same charter as Example A. 8,000,000 issued. $0.0001 stated par. $250,000,000 total gross assets after a Series D.


**Authorized Shares Method:** unchanged, because authorized shares didn’t change. $250 plus 999 blocks at $85 equals $85,165.


**Assumed Par Value Capital Method**


Step Arithmetic Result


1. Assumed par $250,000,000 / 8,000,000 issued $31.250000


2. Compare to stated par $31.250000 vs $0.0001 use assumed par


3. Assumed par value capital $31.250000 x 10,000,000 authorized $312,500,000


4. Whole millions, rounded up 312.5, round up 313


5. Tax 313 x $400 $125,200


6. Cap test $125,200 vs $200,000 under the cap, no relief


Pay $85,165 under the default method. Electing Assumed Par Value here would cost $125,200, so running both methods saves $40,035. On a 10,000,000-share charter with 8,000,000 issued, the two methods flip just under $170,000,000 of total gross assets. At $169,600,000 the assumed par is $21.20, capital is $212,000,000, and the tax is $84,800. At $170,000,000 the assumed par is $21.25, capital is $212,500,000, rounds to 213, and the tax is $85,200.


### Six cap tables, two methods, one comparison


Scenario Authorized Issued Stated par Total gross assets Authorized Shares Assumed Par Value Pay


Shell or holdco, minimal shares 5,000 3,000 $0.0001 $50,000 $175 $400 floor $175, Authorized Shares


Pre-seed, post-incorporation 10,000,000 8,000,000 $0.00001 $1,500,000 $85,165 $800 $800, Assumed Par Value


Series A (Example A) 10,000,000 5,000,000 $0.0001 $5,000,000 $85,165 $4,000 $4,000, Assumed Par Value


Series B, two classes 16,000,000 10,500,000 $0.0001 $14,000,000 $136,165 $8,800 $8,800, Assumed Par Value


Series D, cash-heavy (Example C) 10,000,000 8,000,000 $0.0001 $250,000,000 $85,165 $125,200 $85,165, Authorized Shares


Bootstrapped mid-market, low share count 5,000 5,000 $0.01 $20,000,000 $175 $8,000 $175, Authorized Shares


Two of those six rows favor the default method. A 5,000-share corporation with a real balance sheet should never elect Assumed Par Value, because $20,000,000 over 5,000 issued shares gives $4,000.000000 assumed par, times 5,000 authorized is $20,000,000 of capital, which is $8,000 of tax against a $175 alternative. Add the $50 annual report fee to every figure in the Pay column to get the total March 1 remittance.


## Federal Tax Obligations


Delaware franchise tax touches the federal return in exactly three places, and one of them supplies the number Delaware needs.


**It’s deductible on Form 1120, line 17.** Delaware franchise tax is a privilege tax measured on capital structure, not on income, so it’s an ordinary and necessary business expense under IRC Section 164(a). It belongs on[Form 1120](https://www.irs.gov/instructions/i1120) line 17, “Taxes and licenses,” and the $50 annual report fee rides along on the same line. The line 17 instructions exclude federal income taxes and state and local income taxes. Delaware franchise tax is neither of those.


**Schedule L, line 15 is the bridge.** The Assumed Par Value input is total assets at end of tax year, Schedule L line 15, column (d). One trap sits here. Under Schedule K, question 13, a corporation whose total receipts and total year-end assets are both under $250,000 isn’t required to complete Schedule L at all. Those corporations still owe Delaware a defensible total gross assets figure. No Schedule L does not mean no reporting obligation.


**The calendars don’t line up.** Delaware wants the report and the money by March 1. Form 1120 for a calendar-year C-corp isn’t due until April 15, extendable to October 15 on Form 7004. Your Delaware filing depends on a closed December balance sheet roughly six weeks before the federal return exists. Companies that close late estimate in March and find the mismatch in October.


One more point that isn’t a federal filing rule but gets treated like one. The tax is owed whether or not you’re profitable. No income-based relief, no de minimis exemption for pre-revenue companies, and no offset against a net operating loss. A company sitting on a $4,000,000 NOL still wires Delaware by March 1.


Everything past this point, the full annual C-corp calendar, estimated tax, Form 7004 mechanics, and multi-state nexus, belongs to our companion Delaware C-corp requirements guide. For the wider picture of[what Delaware incorporation costs in taxes](https://www.indinero.com/blog/delaware-incorporation-taxes/) , start there. This page owns the calculation.


## State Tax Obligations


The Delaware annual report and franchise tax payment are due March 1 for the prior calendar year, under 8 Del. C. Section 504(a).


- **Filing and payment mechanics.** Both happen online through the Division of Corporations e-filing system, available daily from 8:00 am to 11:45 pm Eastern Time. Payment is by electronic ACH debit, which is required above $5,000, or by Visa, MasterCard, American Express, or Discover.
- **The $5,000 installment trigger.** A corporation owing $5,000 or more in franchise tax pays estimated installments the following year: 40% by June 1, 20% by September 1, 20% by December 1, and the balance with the annual report by March 1. Miss them and interest runs at 1.5% per month on tax and penalty.
- **Delaware corporate income tax, usually not owed.** Delaware imposes an 8.7% corporate income tax under 30 Del. C. Chapter 19 on corporations doing business in Delaware. The[Delaware Division of Revenue](https://revenue.delaware.gov/frequently-asked-questions/corporate-income-tax-faqs/) states plainly that corporations incorporated in Delaware but not conducting business there are not subject to it under 30 Del. C. Section 1902(b)(6), while still owing franchise tax to the Department of State.
- **The intangible holding company exemption.** Corporations whose Delaware activity is confined to maintaining and managing intangible investments, including stocks, bonds, notes, patents, trademarks, and trade names, plus collecting income from them, are exempt under Section 1902(b)(8). Exempt corporations still file an annual information return, Form CIT-HIC, reporting income sources and services provided inside and outside Delaware.
- **Gross receipts tax.** Applies to companies actually selling goods or providing services in Delaware. Most non-operating charters never touch it. Delaware also has no state sales tax, per the[Tax Foundation](https://taxfoundation.org/location/delaware/) . Our[Delaware tax rates guide](https://www.indinero.com/state-tax-guides/delaware-tax-rates/) has the full rate picture.
- **A registered agent.** Required under 8 Del. C. Section 132, billed by the agent rather than the state. Since the December franchise tax notice goes to the agent, a stale agent relationship is how companies miss March 1.


A Delaware charter is not a Delaware tax return.


What a Delaware charter also doesn’t do is displace your home state. A Delaware C-corp with employees in California, New York, or Texas still owes foreign qualification, state income or franchise tax, payroll registration, and often a state minimum tax in each. That multi-state layer is where a Delaware charter stops being cheap, and it starts with knowing[where your startup creates tax nexus](https://www.indinero.com/blog/startup-tax-nexus/) .


## Entity-Specific Considerations


Delaware taxes entity types under different chapters, on different deadlines, at different amounts, and the failure is almost never arithmetic.


Entity type Annual obligation Amount, 2026 Deadline Annual report


Domestic C-corporation Franchise tax plus annual report $175 to $200,000, plus $50 report fee March 1 Yes


Domestic S-corporation Franchise tax plus annual report Identical to C-corp March 1 Yes


Exempt domestic corporation Annual report only $25 report fee March 1 Yes


Foreign corporation registered in DE Annual report only $250 report fee June 30 Yes


Domestic or foreign LLC Flat annual tax $400 June 1 No


Domestic or foreign LP Flat annual tax $400 June 1 No


General partnership registered in DE Flat annual tax $400 June 1 No


LLP or LLLP Per-partner annual tax $300 per partner June 1 Yes


Registered series of an LLC or LP Flat annual tax $100 per series June 1 No


**An S election changes nothing here.** An S election is a federal tax classification made on Form 2553. A Delaware franchise tax C-corp computation and an S-corp computation are identical, because Delaware still applies Section 503 to authorized shares or assumed par value capital, still requires the annual report, and still wants payment by March 1. Owners weighing the[difference between a C-corp and an S-corp](https://www.indinero.com/blog/c-corp-vs-s-corp/) should treat the Delaware bill as a constant across both.


**The June 1 split is the real trap.** Delaware LLCs, LPs, and general partnerships owe a flat $400 annual tax due June 1 with no annual report to file, per Delaware’s[LLC, LP, and GP franchise tax instructions](https://corp.delaware.gov/alt-entitytaxinstructions/) . Founders running an operating C-corp alongside a real estate LLC or an IP holding LLC routinely put every Delaware entity on the March 1 calendar and blow June 1. The penalty is the same $200 plus 1.5% per month.


It’s a calendar error, not a math error.


**Proration runs one way only.** Corporations prorate under 8 Del. C. Section 503(d) when they haven’t existed for the whole year. Alternative entities don’t, and Delaware assesses the full annual tax if the entity was active in its records anytime between January 1 and December 31. That asymmetry bites in the[LLC to C-corp conversion](https://www.indinero.com/blog/avoid-the-tax-penalty-converting-your-business-from-llc-to-c-corp/) nearly every venture-backed company runs before a priced round. Convert in July and you owe the full unprorated $400 LLC tax for that calendar year on June 1 of the next, plus a prorated corporate franchise tax by March 1. Two entities, two deadlines, one calendar year.


## Common Pitfalls


1. **Paying the December notice as printed.** The notice is computed on authorized shares because that’s the only variable the state has. For a 10,000,000-share startup it reads $85,215. Paying it without recalculating is the most expensive unforced error in startup compliance.
2. **Using net assets, equity, or cash instead of total gross assets.** Section 503(i) points to the total assets line on Schedule L. Don’t subtract liabilities, deferred revenue, or convertible notes. Understating the figure to lower the bill is a misstatement on a signed report.
3. **Using the wrong balance sheet date.** The statute specifies the fiscal year ending in the calendar year prior to filing. Not the current month, not the last return you filed. A March 1, 2027 filing uses December 31, 2026 total assets.
4. **Setting par value too high at incorporation.** Take Example A, where assumed par is $1.000000. At $0.0001 stated par the capital base is $10,000,000 and the tax is $4,000. At $5.00 stated par the base becomes $50,000,000 and the tax is $20,000. Same company, same balance sheet, five times the tax, decided by a number typed into the certificate on day one. It’s one reason[entity setup decisions](https://www.indinero.com/blog/how-to-choose-your-small-business-entity/) deserve a tax review before filing.
5. **Authorizing more shares than the plan requires.** Every additional 10,000 authorized shares, or any fraction of a block, adds $85 under the default method and inflates the capital base under the other. Authorizing 10,000,001 shares instead of 10,000,000 costs $85 more. Rounding up “for headroom” has a price tag.
6. **Blowing the $5,000 installment threshold.** Cross $5,000 and you owe 40% by June 1, 20% by September 1, 20% by December 1, and the balance by March 1. Teams that treat this as a single annual event accrue 1.5% monthly interest on three missed installments before anyone notices.
7. **Missing the 30-day amendment reporting window.** If a charter amendment changed authorized stock or par value during the year, Delaware requires issued shares and total gross assets within 30 days of the amendment, for each portion of the year each capital structure was in effect. The tax then prorates by days in effect over 365, or 366 in a leap year.
8. **Closing a round in December instead of January.** Gross assets are measured at fiscal year end, so a Series B closing December 20 puts the whole round into that year’s assumed par value calculation. The same round closing January 5 doesn’t hit until the following year’s report.
9. **Assuming Assumed Par Value always wins.** See Example C. Once assumed par value capital passes roughly $212.9 million on a 10,000,000-share charter, the default method becomes the cheaper one.
10. **Letting it lapse.** Under 8 Del. C. Section 510, a corporation that neglects for one year to pay franchise tax or file a complete report has its charter declared void and its corporate powers declared inoperative. No charter means no Certificate of Good Standing, which means a stalled financing, a stalled acquisition, and a broken customer contract with a good-standing rep. Revival requires a Certificate of Revival under Section 312 plus all back taxes, penalties, and interest.


## How Indinero Handles Delaware Franchise Tax


Delaware franchise tax fails at the seam between bookkeeping and tax. The bookkeeper closes December. The tax preparer files the 1120 in April. Nobody owns the number due March 1.


When those two functions sit at two vendors, the December close slips, the March filing gets estimated, and the estimate stops matching Schedule L six months later. Indinero bundles bookkeeping, accounting, tax, and fractional CFO advisory under one monthly engagement, with pricing that starts at $750/mo, so that seam doesn’t exist. What that looks like on this specific filing:


- **We close December before March 1.** The Schedule L line 15 figure on your Delaware report is the same figure that lands on the Form 1120 in April, because the same team produces both.
- **We run both methods every year, not once.** The right election changes as the balance sheet grows. We compute Authorized Shares and Assumed Par Value side by side each cycle and document the arithmetic in case the state asks.
- **We track the $5,000 installment threshold forward.** Cross $5,000 in one year and you owe June, September, and December installments the next. That becomes a cash calendar entry, not a February surprise.
- **We map every entity in the group to its own deadline.** March 1 for the corporations. June 1 for the LLCs, LPs, and GPs at $400 each. June 30 for foreign corporation annual reports. One calendar, not three vendors’ calendars.
- **We flag par value and authorized share decisions before they cost money.** A charter amendment adding authorized shares carries a franchise tax price. So does closing a round in December. We model both before the board votes.


You’re not just looking for someone to file the report. You’re looking for a finance partner who owns the number the report depends on.


LLC, S-corp, multi-entity, and non-Delaware companies sit inside the same engagement here, not as an exception to it. Indinero has maintained continuous operations since 2009, serves 500+ regular customers, brings 100+ years combined team experience, is SOC 2 compliant as of 2026, and holds a 5-star Clutch rating. If your March 1 filing currently depends on a December close nobody owns,[business tax services](https://www.indinero.com/services/business-tax-services/) is where that gets fixed, and[fractional CFO advisory](https://www.indinero.com/services/cfo-services/) is in the same engagement when the charter decisions get bigger. Reach out for a free consultation. We’d love to learn about your business.


## Frequently asked questions


Delaware franchise tax questions cluster around the same handful of mechanics. Who owes it, how each method computes, where the floor and the ceiling sit, and what happens when March 1 passes. Here are the ones founders and finance leads ask most.


### What is the Delaware franchise tax and who has to pay it?


Delaware franchise tax is an annual privilege tax on the charter itself under 8 Del. C. Section 501, not a tax on income. Every domestic Delaware corporation owes it regardless of revenue, profit, headcount, or whether it operates inside Delaware, so a pre-revenue startup still pays. Corporations also file an annual report carrying a $50 fee, while Delaware LLCs, LPs, and GPs pay a flat tax due June 1 instead. At indinero every entity in a group sits on one calendar inside the same monthly engagement.


### How is the Authorized Shares Method calculated?


The Authorized Shares Method charges $175 for 5,000 or fewer authorized shares, $250 for 5,001 to 10,000, then $85 per additional 10,000-share block. Any fraction of a block counts as a full block, and the total caps at $200,000. A 10,000,000-share charter produces $85,165, which is the figure Delaware prints on the December notice because authorized shares are the only input the state holds. Indinero recomputes both methods before that notice ever reaches accounts payable.


### How is the Assumed Par Value Capital Method calculated?


The Assumed Par Value Capital Method divides total gross assets by issued shares, then multiplies each class’s authorized shares by the greater par value. Sum the classes, round up to the next whole $1,000,000, and multiply by $400, with a $400 minimum. Total gross assets is the Form 1120 Schedule L total-assets figure for the fiscal year ending in the prior calendar year. That number comes from a closed December book, which is why indinero closes December before the March 1 filing.


### Why do VC-backed startups always elect Assumed Par Value?


VC-backed startups usually elect Assumed Par Value because a large authorized share count with a tiny par value punishes the default method. Always is the wrong word. On a 10,000,000-share charter the two methods cross just under $170,000,000 of total gross assets, after which the Authorized Shares Method is cheaper. Indinero runs both every year rather than defaulting to the heuristic.


### What is the minimum and maximum Delaware franchise tax?


The Delaware franchise tax minimum is $175 under the Authorized Shares Method or $400 under Assumed Par Value, and the maximum is $200,000. Add the $50 annual report fee to whichever figure applies, so the true floor is $225 or $450. A separate flat $250,000 applies to Large Corporate Filers, which requires stock listed on a national securities exchange, so no private venture-backed company qualifies. Where you land between those bounds is a charter decision, and indinero models par value changes before the board votes.


### When is the Delaware franchise tax due and where is it paid?


Delaware franchise tax is due March 1 for domestic corporations, for the prior calendar year, filed online through the Division of Corporations e-filing system. Corporations owing $5,000 or more also pay estimated installments, 40% by June 1, 20% by September 1, 20% by December 1, and the balance by March 1. The December notice goes to your registered agent, not to your office, which is how companies miss the date. Indinero tracks that installment threshold forward as a cash calendar entry.


### What happens if I miss the deadline?


Missing the March 1 Delaware franchise tax deadline triggers a $200 penalty plus 1.5% interest per month on both the tax and the penalty. Neglect it for a year and 8 Del. C. Section 510 lets the state void the charter and make corporate powers inoperative. No Certificate of Good Standing means a stalled financing, acquisition, or bank facility. Revival under Section 312 requires all back taxes, penalties, and interest. Indinero keeps the close and the filing in one engagement so the date holds.
