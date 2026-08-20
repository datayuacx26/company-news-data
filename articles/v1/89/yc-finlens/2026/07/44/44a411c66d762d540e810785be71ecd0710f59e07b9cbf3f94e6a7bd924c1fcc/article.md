---
schema_version: "1.0.0"
document_id: "44a411c66d762d540e810785be71ecd0710f59e07b9cbf3f94e6a7bd924c1fcc"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/1099-k-reporting-cpa-guide"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T22:39:39.250313+00:00"
fetched_at: "2026-07-31T22:39:40.641602+00:00"
content_hash: "sha256:0416ca0bfba27c85b0b953fa28288b76de8fe544cd70856381ed742a84df69b6"
---

# 1099-K (2026) CPA Firm Guide to Form 1099-K Threshold, Reporting, and Stripe/PayPal/Venmo

## **Key takeaways**


- 2026 federal threshold: $20,000 AND 200 transactions OBBBA-restored permanent, per Avalara/IRS
- 1099-K reports GROSS payments CPA firm must reconcile to net revenue after fees, refunds, chargebacks
- All business income is reportable whether or not a 1099-K was issued
- State thresholds vary MD, MA, NJ, VT, VA, DC use $600–$1,000 (much lower than federal)
- IRS computer-matches 1099-K to return; mismatches trigger CP2000 letters within months of filing
- [Finlens](https://www.finlens.app/accountants) automates Stripe/QBO gross-to-net reconciliation that feeds 1099-K matching workpaper


## The 2026 1099 K threshold what actually applies


Layer


Threshold


Source


Federal (2026, post-OBBBA)


$20,000 AND 200 transactions


OBBBA 2025 (P.L. 119-21) reinstated pre-ARPA threshold permanently


American Rescue Plan (attempted, 2021)


$600


(no transaction floor)


Never fully enforced; IRS delayed each year 2022–2024; OBBBA repealed


State — Maryland


$600


State law overrides federal on state filings


State — Massachusetts


$600


State law


State — New Jersey


$1,000


State law


State — Vermont


$600


State law


State — Virginia


$600


State law


State — District of Columbia


$600


State law


State — Illinois


$1,000 + 4 transactions


State law


*Verify current thresholds at*[irs.gov/businesses/understanding-your-form-1099-k](https://www.irs.gov/businesses/understanding-your-form-1099-k) *and each state DoR before filing. State thresholds change frequently.*


The take-away: even after OBBBA restored $20K/200 federal threshold, several states continue to issue 1099-Ks at $600. A client that stays under $20K federally can still receive multiple state-issued 1099-Ks CPA firm workpaper needs to catch both.


*Figure 1. Federal restored to $20K/200 under OBBBA; several states still on $600. Multi-state clients need both layers on reconciliation workpaper.*


## What Form 1099-K actually reports and why "gross" is trap


Form 1099-K reports gross payments in Box 1a. That is total dollar amount of payments settled through payment network for calendar year before any of following are subtracted:


- **Processing fees** (Stripe fees, PayPal fees, credit card interchange)
- **Refunds** to customers
- **Chargebacks** and disputes
- **Adjustments** and reserves held by PSE
- **Sales tax collected** on behalf of buyer
- **Tips** paid through platform but distributed to employees
- **Personal payments** that shouldn't have hit business account (e.g., Venmo/PayPal cross-use)


For a business processing $500,000 gross through Stripe in a year with $14,500 in Stripe fees, $8,000 in refunds, and $1,500 in chargebacks 1099-K Box 1a will show **$500,000** . The actual net business revenue is **$476,000** . Filing $500,000 as revenue on return overstates income by $24,000 and misses deductible expense line for fees, refunds, and chargebacks.


**The right workpaper flow:**


1. Pull 1099-K gross amount from every PSE client uses (Stripe, PayPal, Venmo, Square, Cash App, Etsy, eBay, Amazon)
2. Pull corresponding platform reports showing fees, refunds, chargebacks, adjustments
3. Reconcile 1099-K gross to platform net (bank-deposited amount) to book net revenue
4. Book fees and refunds separately as deductible expenses on return
5. Match total reported gross across all PSEs against client's QBO revenue accounts to catch any missing sources
6. Document reconciliation for client file this is workpaper that survives an IRS CP2000 inquiry


Related:[Section 174 R&D capitalization](https://www.finlens.app/blogs/section-174-r-and-d-capitalization) covers same upstream-reconciliation principle for engineering payroll workpapers.


## Who issues a 1099-K and who does not


The Payment Settlement Entity (PSE) files 1099-K. That is party that settles payment to seller not necessarily party that processed transaction.


**PSEs that issue 1099-K:**


- **Payment processors** Stripe, Square, PayPal, Cash App, Venmo (business), Amazon Pay, Adyen
- **Third-party settlement organizations (TPSOs)** marketplaces that settle payments to sellers: eBay, Etsy, Amazon, Shopify (via Shopify Payments), Uber (to drivers), Lyft, DoorDash, Airbnb, Vrbo
- **Credit card networks and merchant acquirers** Visa, Mastercard, First Data (via acquiring banks) for direct card acceptance


**Not usually issuing 1099-K:**


- Bank wire transfers (no PSE role)
- ACH transfers between business accounts (unless routed through a payment app)
- Cash payments
- Checks
- Cryptocurrency payments (unless platform is a PSE most exchanges are not)


**Personal payments through Venmo, PayPal, Cash App:** designed as gifts or reimbursements between friends should NOT trigger a 1099-K. When Venmo asks "goods & services" or "friends & family," selecting "friends & family" places payment outside 1099-K reporting stream. This is where firms see errors a client who accidentally receives business payments as "friends & family" underreports revenue AND may face constructive receipt questions.


## 1099-K Stripe, PayPal, Venmo platform specific quirks


### 1099-K Stripe


Stripe issues Form 1099-K to every US-based business account that meets federal $20K/200 threshold. Stripe's Dashboard shows 1099-K under Reports → Tax Documents. The Box 1a gross matches sum of all successful charges settled to account in calendar year before Stripe fees, refunds, and disputes.


For a Stripe-heavy client, reconciliation is:


- Stripe 1099-K Box 1a = gross charges settled
- Stripe monthly fee reports = Stripe fees deducted
- Stripe disputes report = chargebacks (both winning and losing)
- Stripe refunds report = refund count and amount
- Net bank deposit total = Box 1a − fees − refunds − disputes (approximately)


The gross-to-net reconciliation is one of highest-frequency workpapers a Finlens-supported firm builds. Where numbers usually diverge from Stripe's reports: reserves held by Stripe, ACH payouts spanning year-end, adjustments for prior-year disputes settled in current year.


### 1099-K PayPal


PayPal issues a 1099-K for business account portion of activity payments received as "goods and services." Personal transfers received as "friends and family" are NOT reported on 1099-K by PayPal. This is a common client error source: business payments accidentally sent as "friends and family" (usually to avoid PayPal fees) underreport client's revenue.


PayPal's 1099-K is under Statements → Tax Statements in account. Reconciliation follows same gross-fee-refund-net pattern as Stripe.


### 1099-K Venmo


Venmo (owned by PayPal) issues 1099-Ks for business profile activity only personal profile payments are not reported. Clients using their personal Venmo for business transactions frequently receive no 1099-K at all, which does NOT relieve them of obligation to report income.


Venmo's split between "friends and family" (personal) and "goods and services" (business) is reporting boundary. Firms should ask every gig-economy or side-hustle client whether their Venmo activity is on a business or personal profile.


Related:[tax resolution CPA firm process and fees](https://www.finlens.app/blogs/tax-resolution) covers what happens when an unreported 1099-K triggers a CP2000 inquiry.


‍ *Figure 2. 1099-K Box 1a is gross. Fees, refunds, chargebacks, and personal payments reduce it to actual taxable revenue. The workpaper shows delta line by line.*


## 1099-K vs 1099-MISC vs 1099-NEC which one applies


The classification matters because filing wrong form (or duplicating income across forms) creates same computer-match exposure that ignoring form does.


Form


Reports


Issued by


Threshold


1099-K


Payments settled through payment cards / apps / marketplaces


PSE (Stripe, PayPal, Amazon, etc.)


$20K + 200 transactions (federal, 2026)


1099-NEC


Nonemployee compensation for services


Payer directly (business, not through a PSE)


$600 per payee per year


1099-MISC


Rents, royalties, prizes, medical/health payments, attorney fees


Payer directly


$600 (most boxes); $10 (royalties)


**The no-double-report rule:** if a payment is reported on a 1099-K by PSE, payer should NOT also issue a 1099-NEC for same payment. The IRS explicitly instructs payer to check "was this reportable on a 1099-K" before issuing a 1099-NEC. A payment made through a payment card or third-party network gets one 1099 1099-K from PSE.


For CPA firms preparing 1099s for clients: every payment routed through Stripe, PayPal, Square, or a marketplace is 1099-K territory (issued by PSE, not payer). Only direct-payment vendor invoices paid via check, ACH, or bank wire go on 1099-NEC.


Related:[best collaborative accounting software for CPA firms](https://www.finlens.app/blogs/best-collaborative-accounting-software-for-cpa-firms) covers how firms manage 1099 preparation cycle across dozens of clients.


## The IRS-official rule you'll find nowhere else must report regardless of form


Direct quote from IRS's[Understanding your Form 1099-K](https://www.irs.gov/businesses/understanding-your-form-1099-k) page (accessed 2026-07-30):


**"You may receive a Form 1099-K even when total payments or transactions are less than reporting threshold. No matter amount of reported payments, if you receive payments for selling goods or services, you must report all income on your tax return."**


Three practical consequences most 1099-K blogs miss:


1. **A client under $20K/200 federal threshold can still receive a 1099-K.** PSEs frequently issue 1099-Ks below federal threshold either voluntarily, to comply with lower state threshold, or in error. The client cannot "ignore" a below-threshold 1099-K just because federal rule didn't require issuance.
2. **A client with zero 1099-Ks can still owe tax.** Cash payments, bank wires, checks, and ACH receipts don't trigger 1099-Ks. The obligation to report underlying income is separate from whether a form was issued.
3. **Multi-platform aggregation is per-PSE, not aggregate.** A client with $18K on PayPal + $15K on Stripe + $12K on Square in same year receives **three separate 1099-Ks** even though no single platform crossed $20K threshold because each PSE evaluates threshold independently for its own settlements. Wrong: no 1099-K. Right: three 1099-Ks; check each platform separately.


## The double-reporting trap a real worked example


The most expensive 1099-K error CPA firms see is duplicate reporting between 1099-K (from PSE) and 1099-NEC (from paying business). NerdWallet's Tina Orem laid out canonical scenario:


Client freelanced $23,000 worth of work for one customer. At year-end, customer sends a **1099-NEC** for $23,000 (payer's obligation on services). But customer paid client via PayPal. PayPal, having settled $23,000 through its network, also issues a **1099-K** for $23,000.


Two 1099s. Same $23,000 of income. The IRS Automated Under Reporter (AUR) system will compare sum of 1099s against client's return and if return only reports $23,000 (correct, not double-counted), AUR flags $23,000 of "underreported" income because it expected $46,000 from two forms.


**The correct fix:** paying customer should NOT have issued a 1099-NEC on a payment routed through a payment card or third-party network. IRS instructions to filers of 1099-NEC explicitly exclude payments where a 1099-K would apply. The CPA firm workflow:


1. Confirm payment method was it through a PSE (PayPal, Stripe, Square)? If yes, 1099-NEC was issued in error.
2. Contact payer, request they withdraw 1099-NEC (file corrected zero-dollar 1099-NEC with IRS).
3. If payer refuses or is unresponsive, report income once on client's return (matching either 1099-K OR 1099-NEC amount, not both). Attach a statement to return explaining duplicate reporting, with documentation of payment method.
4. When CP2000 letter arrives, respond within 30 days with same explanation and any correspondence with payer.


## Additional IRS resources for 1099-K workpapers


The IRS.gov 1099-K hub links to two authoritative publications every 1099-K workpaper should reference:


- [IRS Publication 5951 Form 1099-K MYTHS vs FACTS (PDF)](https://www.irs.gov/pub/irs-pdf/p5951.pdf) explicitly addresses "I received a 1099-K, does that mean I owe tax?" myths. Useful to hand to confused clients.
- [IRS Publication 5731-B Are you making money selling things or providing a service? (PDF)](https://www.irs.gov/pub/irs-pdf/p5731b.pdf) hobby-vs-business distinction that determines whether a 1099-K represents Schedule C income or occasional-sale non-business income.
- [Form 1099-K FAQs (IRS newsroom)](https://www.irs.gov/newsroom/form-1099-k-faqs) kept current with latest threshold guidance.
- [Gig Economy Tax Center](https://www.irs.gov/businesses/gig-economy-tax-center) for clients on Uber/Lyft/DoorDash/Airbnb who receive 1099-Ks from platform.


## The workflow a CPA firm actually runs on 1099-K reconciliation


For every client with material payment-processor activity:


**Step 1 Inventory PSEs.** Ask client: what payment platforms did you receive money through this year? Cover Stripe, PayPal, Venmo, Square, Cash App, Zelle (usually no 1099-K), Amazon, eBay, Etsy, Shopify Payments, Uber, DoorDash, Airbnb, Vrbo anything that could produce a 1099-K.


**Step 2 Pull each 1099-K.** Log in to every platform's tax document center. Save PDF to client file. If a platform issued no 1099-K (below threshold), note that.


**Step 3 Cross-check against client's QBO revenue.** Sum all 1099-K Box 1a amounts. Compare to client's gross revenue recognized in QBO. Investigate any material gap:


- Under-reporting on return? (revenue booked below 1099-K gross)
- Timing mismatch? (year-end settlements crossing calendar year)
- Non-1099-K revenue sources missed? (bank wires, checks, cash)
- Personal payments improperly deposited?


**Step 4 Build gross-to-net reconciliation per PSE.** For each platform, list gross (1099-K Box 1a), less fees, less refunds, less chargebacks, less any sales tax collected, less any personal payments, equals net taxable revenue. Book fees, refunds, chargebacks as separate deductible expense lines.


**Step 5 Match state 1099-Ks separately.** For clients with activity in MD, MA, NJ, VT, VA, DC, IL pull state-issued 1099-Ks even when federal $20K threshold wasn't met.


**Step 6 Address 1099-K vs 1099-NEC overlap.** If client also received 1099-NECs from payers, verify none duplicate 1099-K amounts (payment made through a payment card should be 1099-K, not 1099-NEC payer shouldn't have issued both).


**Step 7 Document workpaper.** This is what survives a CP2000. The reconciliation showing gross-per-1099-K, less fees, less refunds, equals net taxable = client's audit defense.


**Step 8 File return matching 1099-K totals reported to IRS.** The IRS will computer-match. Any mismatch requires a documented explanation.


## Common 1099-K CP2000 letter scenarios and how firms respond


The IRS's Automated Under Reporter (AUR) program computer-matches every 1099-K against recipient's tax return. Mismatches trigger a CP2000 letter within 12–18 months of filing.


**Scenario 1: Return reports less than 1099-K gross.**


- Cause: return reported net revenue (after fees) and IRS matched to gross
- Response: reconciliation workpaper showing gross 1099-K, less deductible fees/refunds, equals net reported revenue. Attach as CP2000 response within 30 days.


**Scenario 2: Client received a 1099-K they don't recognize.**


- Cause: identity mistake, wrong TIN, or payment aggregator misattribution
- Response: contact PSE to correct and reissue. IRS accepts corrected 1099-K along with CP2000 response.


**Scenario 3: 1099-K includes personal Venmo/PayPal payments.**


- Cause: business account used for gift/reimbursement payments, or personal account misclassified
- Response: reconciliation showing personal payments (with counterparty documentation where possible) reducing taxable portion. If PSE issued 1099-K in error, request a corrected form.


**Scenario 4: Duplicate reporting 1099-K AND 1099-NEC for same payment.**


- Cause: payer erroneously issued 1099-NEC on a payment routed through a payment card
- Response: statement identifying duplicate, request payer withdraw 1099-NEC.


Related:[tax resolution CPA firm process and fees](https://www.finlens.app/blogs/tax-resolution) covers broader CP2000-response workflow when mismatch has compounded into a resolution engagement.


## Where 1099-K workpaper goes wrong upstream


Every 1099-K reconciliation depends on clean gross-vs-net records at source. The failure points on messy books:


- **Stripe revenue booked net of fees** P&L shows $486K when 1099-K shows $500K; a $14K mismatch that requires spreadsheet rebuilding at filing time
- **PayPal fees blended into a single "Payment processing" line** separating Stripe fees from PayPal fees from Square fees isn't possible without going back through bank statements
- **Refunds booked as expense reductions instead of contra-revenue** misclassifies reconciliation trail
- **Personal Venmo/PayPal payments deposited into business bank** inflates 1099-K gross without corresponding tax liability, but requires documentation to explain
- **Cross-year settlements** a January 2 bank deposit from a December 30 Stripe charge gets timing-mismatched


Finlens automates Stripe payout decomposition, QBO categorization with per-client rules, and month-by-month reconciliation between platform reports and general ledger. For clients where 1099-K reporting is a material return item, that upstream cleanup is what makes CP2000 defense trivial instead of a fire drill. Related:[bookkeeping services fees in 2026](https://www.finlens.app/blogs/bookkeeping-services-fees) covers delivery-cost math on underlying reconciliation work.


## Conclusion


**Pick one Stripe- or PayPal-heavy client whose 2025 or 2026 return still needs 1099-K reconciliation done bring client's Stripe/PayPal 1099-K PDF and three months of QBO, and we'll walk gross-to-net workpaper live before return goes to tax preparer.**


$20K


+ 200 tx federal threshold


$600


in 6 states


CP2000


in 12–18 months if mismatched


## 1099-K matches to your return.
Does your reconciliation?


Finlens decomposes Stripe / PayPal / Venmo payouts in QBO so gross-to-net reconciles month-by-month, not at filing time.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


‍


## Frequently asked questions


### What is 1099-K threshold for 2026?


For 2026 federal threshold is $20,000 AND 200 transactions per calendar year reinstated permanently under OBBBA in 2025. This reversed American Rescue Plan's attempt to lower threshold to $600 (which IRS delayed enforcing every year from 2022 through 2024). Several states (MD, MA, NJ, VT, VA, DC, IL) impose lower state thresholds ($600–$1,000).


### Do I have to pay taxes on a 1099-K?


Yes, if payments represent business or profit income. Personal payments (gifts from family or friends, splitting a meal, reimbursements) are not taxable and should not trigger a 1099-K if sender selected "friends and family" on Venmo or PayPal. All business income must be reported on your tax return whether or not you received a 1099-K.


### What is new rule for 1099-K under OBBBA?


The One Big Beautiful Bill Act (P.L. 119-21) signed in 2025 reinstated pre-ARPA 1099-K threshold of $20,000 AND 200 transactions per calendar year on a permanent basis. The $600 threshold from American Rescue Plan is no longer in effect at federal level.


### Can I ignore a 1099-K?


No. The IRS's Automated Under Reporter (AUR) system computer-matches every 1099-K against recipient's tax return. Mismatches including missing income trigger a CP2000 or similar inquiry letter within 12–18 months. Response is typically required within 30 days. Ignoring CP2000 leads to a Notice of Deficiency and eventual collections. Address it with a reconciliation workpaper.


### What's difference between 1099-K and 1099-MISC?


Form 1099-K reports payments settled through payment cards/apps/marketplaces (issued by PSE, e.g., Stripe or PayPal); threshold is $20K + 200 transactions. Form 1099-MISC reports rents, royalties, prizes, medical/health payments, attorney fees paid directly by payer (not through a PSE); threshold is $600 for most boxes. A payment reported on a 1099-K should NOT also be reported on a 1099-NEC by same payer.


### Does a 1099-K count as earned income?


For a business owner, yes underlying revenue that generated 1099-K is earned income subject to income tax and self-employment tax. Selling personal items at a loss on eBay is not taxable even if a 1099-K is received. Reselling or "flipping" items for profit is taxable, and 1099-K reflects gross proceeds; CPA firm reconciles to net taxable via reconciliation workpaper.


### How does Stripe report 1099-K?


Stripe issues a 1099-K to every US business account meeting federal $20K + 200 threshold. The Box 1a gross equals total successful charges settled to account in calendar year before Stripe fees, refunds, or disputes. Stripe's Dashboard shows 1099-K under Reports → Tax Documents. Reconciliation to net revenue subtracts Stripe fees, refunds, and chargebacks from Box 1a gross.


### How does PayPal report 1099-K?


PayPal issues a 1099-K on business-account portion of activity (payments received as "goods and services"). Personal "friends and family" transfers on PayPal are not reported on 1099-K. The 1099-K is under Statements → Tax Statements in PayPal account. Business payments accidentally sent as "friends and family" underreport recipient's revenue and should be caught in reconciliation workpaper.


### How does Venmo report 1099-K?


Venmo (owned by PayPal) issues 1099-Ks for business profile activity only. Personal Venmo activity is not reported. Clients running business payments through their personal Venmo receive no 1099-K, which does NOT relieve them of obligation to report income. Ask every gig-economy client whether their Venmo is a business or personal profile.


### What if I received a 1099-K that includes personal payments?


Common when a business account is used for gift/reimbursement payments. On return, reconcile: 1099-K gross, less personal payments (with counterparty documentation), equals actual business revenue. Document each personal payment. If PSE issued 1099-K in error (e.g., misclassified transactions), request a corrected 1099-K from platform.


### Does Finlens automate 1099-K reconciliation?


Not form filing itself PSE files 1099-K, and tax software (Drake, Lacerte, ProSeries, UltraTax) reports it on return. What Finlens automates is upstream reconciliation: Stripe payout decomposition so gross-to-net is defensible, transaction categorization so fees, refunds, and chargebacks are separately tracked, and per-client rules so reconciliation workpaper builds automatically each month instead of being rebuilt at filing time.


*1099-K thresholds, state variation, and PSE reporting rules change frequently. This article reflects guidance current as of 2026-07-30 based on P.L. 119-21 (OBBBA) and IRS guidance publicly available at that date. Verify current thresholds at*[irs.gov/businesses/understanding-your-form-1099-k](https://www.irs.gov/businesses/understanding-your-form-1099-k) *and each state Department of Revenue before filing. Nothing in this article is legal or tax advice engage a licensed CPA, EA, or attorney for actual 1099-K reconciliation and return preparation. Third-party trademarks (QuickBooks®, Stripe®, PayPal®, Venmo®) belong to their respective owners.*


‍
