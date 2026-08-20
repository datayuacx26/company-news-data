---
schema_version: "1.0.0"
document_id: "0248b9fa5686a9910224e85fda7a0d2ac65d50672ad6d5c36c14985e14997c6b"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/convert-llc-to-c-corp-when-and-how/"
published_at: "2026-08-16T01:25:55+00:00"
first_seen_at: "2026-08-16T03:33:31.835012+00:00"
fetched_at: "2026-08-16T03:33:33.358679+00:00"
content_hash: "sha256:00d1e1f95e58af5d3b76895a21cd17f349f971dbe0ee791f5cb430a921ce2517"
---

# Converting LLC to C-Corp: When and How to Do It Right

## What Is an LLC to C-Corp Conversion?


To convert LLC to C-corp is to change two things at once. The entity’s form under state law changes, and its classification under the Internal Revenue Code changes with it. Those two changes run on different rule sets, and the expensive mistakes happen when founders treat them as a single event.


Three routes get you there. All three land in the same federal analysis, an incorporation Section 351 event that turns on who transferred what and who holds control afterward. They differ in state-law continuity, document volume, and cost. If you’re still weighing the structures themselves, start with the[difference between an LLC and a corporation](https://www.indinero.com/faq/difference-between-llc-and-corporation/) before picking a route.


### Statutory conversion


The cleanest route, where the state offers one. Delaware allows it under 8 Del. C. Section 265, which covers conversion of other entities into a domestic corporation. The LLC files a Certificate of Conversion to Corporation alongside a Certificate of Incorporation. The statute treats the resulting corporation as the same entity as the converting LLC and says the conversion is not a dissolution. Contracts, leases, bank accounts, and IP assignments carry over without assignment, which is why counsel usually prefers this path. Delaware’s definition of “other entity” is broad enough that a California LLC can convert directly into a Delaware corporation under the same section, which is the most common startup fact pattern there is.


### Contribution under IRC Section 351


Members form a new corporation and contribute their LLC membership interests to it in exchange for stock. The LLC becomes a wholly owned subsidiary, then gets liquidated or left in place as a disregarded entity. This is the “interests-over” method blessed by Rev. Rul. 84-111, which recognized three ways to incorporate a partnership: assets-over, assets-up, and interests-over. Each carries its own mix of Subchapter C, Subchapter K, and Subchapter P consequences. The contribution route shows up when the parties want a holding structure from day one, or when the LLC holds assets that are awkward to move. It costs more documents than a one-page certificate.


### Merger into a new corporation


The LLC merges into a newly formed corporation under the merger statutes of both states. Mechanically it resembles the contribution route, and it becomes mandatory in states with no LLC-to-corporation conversion statute. New York is the example that catches founders mid-deal. New York’s LLC Law lets a domestic LLC merge or consolidate with a domestic or foreign business entity, but the state offers no statutory conversion or domestication path for an LLC becoming a corporation. A New York LLC that wants to be a Delaware C-corp forms the Delaware corporation first and merges into it. Same federal analysis, more filings, and a real risk of a multi-week delay if it surfaces during a financing.


Rev. Rul. 2004-59, published in[Internal Revenue Bulletin 2004-24](https://www.irs.gov/irb/2004-24_IRB) , governs the statutory route. It treats a formless conversion as a deemed contribution of all assets and liabilities to the corporation for stock, followed by a deemed liquidation of the partnership. So a founder filing a one-page certificate in Dover is executing an assets-over incorporation whether they realize it or not. Deciding when to convert LLC to C-corp is a separate question from deciding how, and our walkthrough on[converting your company to a C-corp](https://www.indinero.com/blog/converting-your-company-to-a-c-corp/) covers the document sequence.


## Federal Tax Obligations


The LLC C-corp tax conversion runs on three code sections. IRC Section 351 delivers nonrecognition, IRC Sections 358 and 362 set the basis that survives it, and IRC Section 357(c) is the exception that turns a tax-free deal into a taxable one.


### Section 351 and the 80% control test


[IRC Section 351(a)](https://www.law.cornell.edu/uscode/text/26/351) provides that no gain or loss is recognized when property is transferred to a corporation solely in exchange for stock and the transferors control the corporation immediately after the exchange. Three conditions, all required:


- **Property is transferred.** Under IRC Section 351(d), stock issued for services, for unsecured corporate debt, or for accrued interest on that debt isn’t treated as issued for property. Founder shares granted purely for future services don’t count toward control.
- **Solely in exchange for stock.** If transferors receive cash or other property, IRC Section 351(b) recognizes gain up to the money plus the fair market value of that other property. No loss is recognized.
- **Control immediately after.** IRC Section 368(c) defines control as at least 80% of combined voting power plus at least 80% of the total shares of every other class.


In a plain conversion where the same members hold all of the new stock in the same proportions, the 80% test is met by definition. It gets interesting only when the conversion is bundled with a financing.


### Carryover basis and the Section 357(c) trap


Nonrecognition preserves gain, it doesn’t erase it. IRC Section 358(a)(1) gives each shareholder a basis in the new stock equal to the basis of what was exchanged. IRC Section 362(a) gives the corporation a carryover basis in the assets. Where transferred property carries a net built-in loss, IRC Section 362(e)(2) caps the corporation’s aggregate basis at fair market value.


IRC Section 357(a) treats an assumed liability as something other than money, which is the helpful default. Section 357(c)(1) is the carve-out. Where assumed liabilities exceed the adjusted basis of the property transferred, the excess is gain. An asset-light SaaS LLC with venture debt, a drawn revolver, or years of member distributions can sit exactly there, and the conversion produces a tax bill with no cash attached. Our earlier piece on how to[avoid the tax penalty when converting from an LLC to a C-corp](https://www.indinero.com/blog/avoid-the-tax-penalty-converting-your-business-from-llc-to-c-corp/) walks the same trap from the owner’s side.


### The filings that follow the conversion


1. **Final Form 1065.** The partnership’s tax year closes on the conversion date. File a short-period return with the final-return box checked plus final Schedules K-1 to every member. The[Instructions for Form 1065](https://www.irs.gov/instructions/i1065) set the late-filing penalty at $255 for each month or part of a month, up to 12 months, multiplied by the number of people who were partners during any part of the year.
2. **First Form 1120.** The[Instructions for Form 1120](https://www.irs.gov/instructions/i1120) require the return by the 15th day of the 4th month after the tax year ends, so April 15 for a calendar-year filer. A corporation formed mid-2026 files from the conversion date through December 31, 2026. Form 7004 extends the time to file, not the time to pay.
3. **Quarterly estimates at the 21% rate.** The flat corporate rate applies from day one, and estimated payments are new discipline for a team used to K-1s and member draws.
4. **A new EIN.** IRS guidance requires one when a partnership incorporates and when an LLC terminates to form a corporation. An LLC that only files Form 8832 to elect corporate treatment, without changing its state-law form, does not need a new number. Every payroll registration, bank account, and vendor W-9 gets reissued against the new one.


## State Tax Obligations


The state workstream is where conversions quietly slip. The certificate is one filing. Everything downstream of it, in every state where you have people, customers, or property, is a separate cleanup project with its own calendar.


### Delaware and California, the two common paths


Delaware handles the LLC to C-corp conversion under 8 Del. C. Section 265. The company files a Certificate of Conversion to Corporation together with a Certificate of Incorporation, and the incorporation fee scales with authorized stock. Expedited processing is worth budgeting for when the conversion is a closing condition on a round.


The Delaware tax calendar then shifts. A Delaware LLC pays a flat annual tax due June 1 and files no annual report. A Delaware corporation owes franchise tax plus a $50 annual report fee, both due March 1, per the Division of Corporations’[annual report and tax instructions](https://corp.delaware.gov/paytaxes/) . Franchise tax runs a $175 minimum under the Authorized Shares Method and a $400 minimum under the Assumed Par Value Capital Method. Delaware bills the higher figure first, so recalculating is an annual habit rather than a one-time fix. More detail sits in our guide to[Delaware incorporation taxes](https://www.indinero.com/blog/delaware-incorporation-taxes/) .


California permits statutory conversion as well. Under the Secretary of State’s[conversion information](https://www.sos.ca.gov/business-programs/business-entities/conversion-information) , an LLC converting into a California general stock corporation files Articles of Incorporation containing a statement of conversion, at a $150 filing fee, with the authority sitting in Corporations Code Sections 1150 and 17710.01 and following.


### The registrations that don’t move on their own


Merger-only states come first. New York has no LLC-to-corporation conversion statute, so the New York LLC merges into a newly formed corporation instead. Plan for the extra weeks.


Then the cleanup list. None of this updates itself when the certificate is accepted:


- **Foreign qualifications** in every state where the LLC was registered to do business
- **Payroll withholding and unemployment insurance accounts** , which are tied to the EIN and have to be re-registered under the new one
- **Sales tax permits and economic nexus registrations** , which is where[state tax nexus](https://www.indinero.com/blog/startup-tax-nexus/) exposure resurfaces
- **Business licenses, city tax certificates, and registered agent appointments**


For a company with employees in eight states, this list usually runs longer than the federal tax work.


## Entity-Specific Considerations


This is where the 2026 answer diverges from most of what’s published. Section 1202 was rewritten last year, and a conversion happening now issues stock into the new regime automatically.


### QSBS under the rewritten Section 1202


Section 70431 of the One Big Beautiful Bill Act, P.L. 119-21, enacted July 4, 2025, rewrote the qualified small business stock rules. The statute now pivots on an “applicable date,” and that date is the enactment date. Here is how the two regimes compare under the current text of[26 U.S.C. Section 1202](https://uscode.house.gov/view.xhtml?req=granuleid%3AUSC-prelim-title26-section1202&num=0&edition=prelim) .


Feature Stock acquired on or before July 4, 2025 Stock acquired after July 4, 2025


Minimum holding period More than 5 years At least 3 years


Exclusion at 3 years None 50%


Exclusion at 4 years None 75%


Exclusion at 5 years or more 100% (post September 27, 2010 stock) 100%


Per-taxpayer, per-issuer cap Greater of $10,000,000 or 10x basis Greater of $15,000,000 or 10x basis


Cap indexed for inflation No Yes, beginning after 2026


Issuer aggregate gross-assets ceiling $50,000,000 $75,000,000, indexed after 2026


A founder converting in 2026 issues brand-new stock after the applicable date, so it lands in the new regime by default. That buys a first liquidity window at three years instead of five, a $15,000,000 floor on the per-issuer cap, and room to grow to $75,000,000 of aggregate gross assets before the issuer disqualifies itself.


The trade-off is the rate on the portion that isn’t excluded. Non-excluded Section 1202 gain is taxed at 28% under IRC Section 1(h)(4) rather than the standard long-term rate. Our explainer on[the QSBS exemption](https://www.indinero.com/blog/qsbs-exemption/) covers the qualification tests in more depth.


The clock starts at issuance.


### What changes by entity type


- **Multi-member LLC taxed as a partnership.** The base case. Rev. Rul. 2004-59 governs a formless conversion, Rev. Rul. 84-111 governs contribution and merger. Final Form 1065, final K-1s, capital accounts closed out, and a conversion-date basis schedule for every member.
- **Single-member LLC.** No partnership to terminate and no final Form 1065. One transferor means the 80% control test is satisfied automatically. The new EIN and the conversion-date basis figure still matter.
- **An LLC that already filed Form 8832.** It’s a C-corp federally but still an LLC under state law, and IRC Section 1202(c) requires stock. Investors’ counsel generally wants a real state-law corporation before a priced round rather than an argument about whether units count as stock.
- **An LLC with an S election.** Different transaction entirely. Moving from S to C is a revocation of the election, not a Section 351 incorporation, and it pulls in accumulated adjustments account mechanics and a possible short-year split. Start with[C-corp vs S-corp](https://www.indinero.com/blog/c-corp-vs-s-corp/) for the structural comparison.
- **Foreign members.** IRC Section 1446 withholding on effectively connected income ends. Dividend withholding under IRC Sections 1441 and 1442 begins, at a statutory 30% reduced by treaty. A C-corp carries no shareholder-eligibility restrictions at all, which is a strong argument for the form.
- **Outstanding profits interests.** Units granted under the Rev. Proc. 93-27 safe harbor are tax-free at grant because they have no liquidation value. Converting them into stock isn’t automatic. Depending on how units map to shares, holders can recognize compensation income and the corporation can pick up a withholding obligation on the same day.


Most conversion content assumes a Delaware C-corp with a term sheet already in hand. Plenty of the LLCs converting in 2026 are bootstrapped, PE-backed, or sitting inside a multi-entity group, and the analysis has to start with the entity you actually have.


## Common Pitfalls


Nine failure modes show up over and over. Each one is cheap to prevent and expensive to discover late.


1. **Liabilities above basis.** Venture debt, a drawn line of credit, accrued liabilities, or years of member distributions that pushed capital accounts negative can put assumed liabilities above aggregate adjusted basis. IRC Section 357(c) turns the excess into recognized gain in a transaction that produced no cash. Model the number before anything is filed.
2. **Bundling the conversion with the priced round.** Section 351 requires 80% control immediately after the exchange, and Treas. Reg. Section 1.351-1(a)(1)(ii) disregards property of relatively small value contributed mainly to qualify other transferors. Sequence instead. Close the conversion, let it settle, then close the financing as a separate documented transaction.
3. **Assuming the QSBS clock tacks.** It doesn’t. IRC Section 1202(i)(1)(A) starts the holding period on the exchange date, so four years of operating history buys zero months. The years the business spent as an LLC do not tack.
4. **Assuming the whole gain is excludable.** The IRC Section 1202(i)(1)(B) basis floor sets the stock’s Section 1202 basis at no less than the fair market value of the property contributed, which puts pre-conversion appreciation outside the exclusion. That same deemed basis raises the 10x-basis alternative cap, which can be worth more than the $15,000,000 floor for a company converting with real value on the books.
5. **Converting above the gross-assets ceiling.** IRC Section 1202(d)(2)(B) counts contributed property at fair market value as of the contribution. Cross $75,000,000 of aggregate gross assets and QSBS is off the table for that issuance. Converting late doesn’t just shorten the runway. It can close the door.
6. **Expecting QSBS in an excluded field.** IRC Section 1202(e)(3) rules out health, law, engineering, architecture, accounting, actuarial science, performing arts, consulting, athletics, financial services, brokerage, banking, insurance, farming, extraction, and hospitality, plus any business whose principal asset is the reputation or skill of its employees. A staffing agency or a design consultancy may still have excellent reasons to convert. QSBS shouldn’t be one of the numbers in that model.
7. **Missing the 30-day 83(b) window.** New corporation, new restricted stock, new vesting schedules. The election is due no later than 30 days after the transfer, filed on[IRS Form 15620](https://www.irs.gov/pub/irs-pdf/f15620.pdf) or as a written statement meeting Treas. Reg. Section 1.83-2, and there’s no relief for forgetting. The new stock plan also needs a current[409A valuation](https://www.indinero.com/services/409a-valuation-services/) before the first grant, and the[83(b) election basics](https://www.indinero.com/blog/83b-startup-taxes/) are worth a refresher for every founder receiving stock.
8. **Skipping the final Form 1065.** The partnership year closes on the conversion date whether anyone files or not. At $255 per partner per month for up to 12 months, a 10-member LLC that forgets the short-year return accrues $2,550 every month.
9. **Never building the basis and substantiation file.** Every QSBS claim is the taxpayer’s to prove. The proof package is the conversion-date balance sheet, the fair market value support, the stock issuance records, and the corporation’s gross-asset history from issuance forward. Nobody reconstructs a 2026 conversion in 2032 when a diligence team asks.


## How Indinero Handles LLC to C-Corp Conversion


A conversion is a two-desk job. Counsel drafts the plan of conversion, the charter, the bylaws, the stockholders agreement, and the state filings. The finance team owns everything that has to be true on the other side of the certificate. That second desk is the one that usually goes unstaffed.


Here’s what our tax team carries on a conversion:


- **Section 357(c) exposure, modeled before anything is filed** , from the LLC’s actual liability and basis position rather than from the balance sheet alone
- **The conversion-date basis schedule** , member by member under IRC Section 358, plus the corporation’s asset basis under IRC Section 362
- **The QSBS record that matters later** , meaning the conversion-date fair market value that fixes the basis floor and the issuance date that starts the tiered holding period
- **The final short-period Form 1065 and final Schedules K-1** , then the first Form 1120 cycle, quarterly estimates, and the accounting method election
- **The new EIN, payroll re-registration, and multi-state qualification cleanup**
- **The annual Delaware franchise tax recalculation** , so the March 1 bill reflects the Assumed Par Value Capital Method rather than the default
- **The QSBS substantiation file, maintained from year one** , including the gross-asset history IRC Section 1202(d) requires


Indinero serves bootstrapped, PE-backed, LLC, S-corp, and multi-entity growth companies, not only VC-backed Delaware C-corps. That breadth is the point here. Half of the conversion question is whether the entity you have today should become a corporation at all, and that answer looks different for a founder with no term sheet than for one holding a signed one.


Tax work is bundled with bookkeeping, accounting, and fractional CFO advisory under one monthly engagement, so the conversion-date balance sheet that fixes your QSBS basis is the same one your accountants closed. Continuous operations since 2009 also means the 2026 workpapers are still with the same team when an acquirer asks for them in 2032.


A conversion isn’t a filing you finish. It’s a tax position you maintain. If yours is coming up this year, our[business tax services](https://www.indinero.com/services/business-tax-services/) team can model it before the documents get drafted. Reach out for a free consultation. We’d love to learn about your business and where the timing lands.


## Frequently asked questions


Founders ask the same questions about conversion timing, tax exposure, and QSBS. Here’s what comes up most often.


### When does it make sense to convert an LLC to a C-Corp?


Converting an LLC to a C-corp makes sense at four moments: an institutional priced round, QSBS planning, a stock option program, or retained-earnings economics. LLCs can’t issue incentive stock options, and profits interests get awkward to administer as headcount grows. The harder question is whether your entity should convert at all, and indinero works with LLC, S-corp, C-corp, and multi-entity clients rather than assuming the answer.


### What are the three main conversion methods (statutory, contribution, merger)?


The three LLC to C-corp conversion methods are statutory conversion under a state formless-conversion statute, contribution of LLC interests to a new corporation, and merger. Delaware allows the statutory route under 8 Del. C. Section 265, and the contribution route is the interests-over method of Rev. Rul. 84-111. Merger is mandatory in states with no conversion statute, such as New York, and all three land in Section 351 territory federally, so the difference is state-law continuity, document volume, and cost.


### Does conversion trigger any immediate tax?


Converting an LLC to a C-corp generally triggers no immediate tax, because IRC Section 351 grants nonrecognition when members receive only stock. Three exceptions break that. Boot under IRC Section 351(b), liabilities assumed above aggregate adjusted basis under IRC Section 357(c), and compensation income where profits interests convert into stock. The 357(c) figure is the one worth modeling before anything gets filed, because it produces a tax bill with no cash attached.


### How does Section 351 protect the conversion from immediate tax?


IRC Section 351(a) grants nonrecognition when property is transferred to a corporation solely for stock and the transferors control it immediately after the exchange. IRC Section 368(c) defines that control as at least 80% of voting power and 80% of the shares of every other class. The gain isn’t erased, it’s preserved. Carryover basis under IRC Sections 358(a)(1) and 362(a) carries it into the new stock and the corporation’s assets, which is why somebody has to record conversion-date basis on day one.


### When does the QSBS 5-year clock start after conversion?


The QSBS clock starts on the conversion date, when the corporation issues stock, and time spent as an LLC does not tack. For a 2026 conversion the clock isn’t purely five years. Section 70431 of P.L. 119-21 rewrote IRC Section 1202, so stock issued after July 4, 2025 earns a 50% exclusion at three years, 75% at four, and 100% at five, with the per-issuer cap at the greater of $15,000,000 or 10x basis and a $75,000,000 gross-assets ceiling.


### Do investors typically require the conversion before a priced round?


Yes, in practice, because venture funds can’t accept the K-1 flow-through an LLC produces for their limited partners. Conversion is normally listed as a closing condition on the round. The reason to do it early rather than at signing is that rushed conversions are where Section 357(c) exposure gets missed and 83(b) deadlines get blown. That modeling is what indinero’s tax team runs before counsel starts drafting, well ahead of the closing calendar.


### What state-level filings does the conversion trigger?


Converting an LLC to a C-corp requires a Certificate of Conversion and a Certificate of Incorporation in the destination state, plus withdrawal filings where required. Then comes the cleanup nobody schedules. Updated foreign qualifications in every operating state, new payroll withholding and unemployment registrations tied to the new EIN, and updated sales tax permits. The Delaware calendar shifts too, from the LLC annual tax due June 1 to the corporate franchise tax and annual report due March 1.
