---
schema_version: "1.0.0"
document_id: "c23fe3f57a12d52ecee3efb49bd3a895e8b97410e034845c63b17575e7ff64c2"
company_key: "yc-indinero"
company: "inDinero"
source_id: "yc-indinero-rss-c0cb308109a2"
canonical_url: "https://www.indinero.com/blog/intercompany-eliminations-explained/"
published_at: "2026-08-12T15:18:48+00:00"
first_seen_at: "2026-08-12T16:22:28.267940+00:00"
fetched_at: "2026-08-12T16:22:29.426031+00:00"
content_hash: "sha256:7c1927ca57960c7377519ab0b4bfeba7abeb1349273a0285d7b963ecc083d5e3"
---

# Intercompany Eliminations: What They Are and How to Book Them

## What Are Intercompany Eliminations?


Intercompany eliminations are consolidation entries that remove balances and transactions between commonly controlled entities, so the group reports as one economic entity per ASC 810-10-45-1. They exist because a receivable a company owes itself is not a real receivable, and revenue a company bills itself is not real revenue. When two or more legal entities sit under common ownership, the traffic between them is internal to the group. Consolidation collapses those separate entities into a single reporting entity, and anything that happened only inside that boundary has to come out.


The governing standard is FASB ASC 810, Consolidation. ASC 810-10-45-1 requires that intra-entity balances and transactions be eliminated in consolidated financial statements, including open account balances, security holdings, sales and purchases, interest, and dividends. The rationale is the single-economic-entity assumption. Consolidated statements are prepared as if the group were one company, so they cannot report gain or loss on transactions among the entities in the group. The Codification’s[consolidation presentation guidance](https://dart.deloitte.com/USDART/home/codification/broad-transactions/asc810-10/roadmap-consolidation/chapter-11-presentation-disclosures/11-1-presentation) sets out how this presents.


Two points matter for controllers. First, the elimination is complete. The full amount of intra-entity income or loss is eliminated, and that amount is not reduced by a noncontrolling interest. Second, the same logic extends to combined statements. ASC 810-10-45-10 requires that brother-sister entities under a common owner eliminate intra-entity transactions and profits the same way. A parent with operating subsidiaries, a holding company, and brother-sister entities all generate the same internal traffic that has to be reconciled, then eliminated before a clean consolidated statement exists.


## The Four Common Elimination Types


At controller depth, intercompany elimination journal entries fall into four buckets. In intercompany transactions accounting, the mechanics differ by type, and so does the tax exposure attached to each.


**Intercompany receivables and payables.** Entity A bills Entity B for shared services, rent, or a cash advance. A carries an intercompany receivable, B carries an intercompany payable of the same amount, and on consolidation the two net to zero. Nothing hits income. This is a balance-sheet-only elimination that recurs every period the balance stays open. ASC 810-10-45-1 names these open account balances explicitly.


**Intercompany revenue and expense.** Management fees, cost-plus service charges, and internal royalties create revenue on the biller’s books and expense on the payer’s books. Consolidated revenue must reflect only external sales, so intercompany revenue is eliminated against the matching expense. Skip it, and both consolidated revenue and consolidated operating expense are overstated by the same amount. For SaaS groups, this is where reported ARR gets misstated when an intercompany license or platform fee is left in the top line, the same top-line discipline that[ASC 606 revenue recognition](https://www.indinero.com/blog/asc-606-what-you-need-to-know/) demands for external contracts.


**Intercompany loans, interest, and accrued interest.** A parent or holding company lends an operating sub working capital. One side books a note receivable and interest income, the other a note payable and interest expense, plus accrued interest on both balance sheets. All four legs eliminate. The tax side carries a trap. Under[IRC Section 7872](https://uscode.house.gov/view.xhtml?req=%28title%3A26+section%3A7872+edition%3Aprelim%29) , a related-party loan charging less than the[Applicable Federal Rate (AFR)](https://www.irs.gov/applicable-federal-rates) is a below-market loan, and the IRS imputes interest at the AFR to the lender. The AFR is set monthly under IRC Section 1274(d). For August 2026, Rev. Rul. 2026-13 sets the annual AFR at 4.10% short-term for loans up to 3 years, 4.35% mid-term for 3 to 9 years, and 4.92% long-term for over 9 years.


**Unrealized profit in transferred inventory or fixed assets.** When one entity sells inventory or a fixed asset to another at a markup and the asset is still inside the group at period end, the profit is not yet real. It stays deferred until the asset is sold to an outside party or, for a fixed asset, consumed through depreciation. Under the ASC 810 consolidation model and the parallel elimination principle in equity-method accounting, intercompany profits are eliminated until they are realized through third-party transactions.


## Journal Entries for Each Elimination


These entries live in the consolidation layer. They do not touch entity ledgers, and every figure below assumes the entity-level close is done and intercompany balances are already reconciled to zero difference.


**Intercompany receivable and payable.** HoldCo bills OpCo $150,000 for shared back-office services during the month, and OpCo has not paid at close. The entity books already carry HoldCo’s intercompany receivable against intercompany revenue, and OpCo’s management fee expense against its intercompany payable. The balance-sheet elimination in the consolidation layer is:


Account Debit Credit


Intercompany payable, HoldCo $150,000


Intercompany receivable, OpCo $150,000


The receivable and payable net to zero, so consolidated assets and liabilities are no longer inflated by an amount the group owes itself.


**Intercompany revenue and expense.** Using the same $150,000 management fee, the income-statement elimination is separate from the balance-sheet entry above:


Account Debit Credit


Intercompany management fee revenue $150,000


Management fee expense $150,000


Consolidated revenue now excludes the internal fee, and consolidated operating expense drops by the same $150,000. Group net income is unchanged. The fee was always a wash at the group level.


**Intercompany loan and interest.** HoldCo lends OpCo $500,000 on a 5-year note at the mid-term AFR of 4.35%. Annual interest is $21,750, unpaid at close. Three eliminations clear it. First the principal:


Account Debit Credit


Intercompany note payable, HoldCo $500,000


Intercompany note receivable, OpCo $500,000


Then the interest income against interest expense:


Account Debit Credit


Interest income, HoldCo $21,750


Interest expense, OpCo $21,750


Then the accrued interest on both balance sheets:


Account Debit Credit


Accrued interest payable, OpCo $21,750


Accrued interest receivable, HoldCo $21,750


All four legs disappear on consolidation. The note needs a written agreement and a stated rate at least equal to the AFR, or IRC Section 7872 imputes the interest anyway.


**Unrealized profit in transferred inventory.** OpCo A sells inventory to OpCo B for $100,000. A’s cost was $80,000, so the intercompany markup is $20,000, and B still holds all of it at period end:


Account Debit Credit


Intercompany sales, A $100,000


Cost of goods sold, A $80,000


Inventory, B $20,000


This removes the internal sale, removes the intercompany cost, and writes ending inventory back to the group’s $80,000 cost basis. The $20,000 of profit stays deferred until B sells to a third party. For a fixed asset sold at a gain, the mechanics mirror this. Eliminate the gain, restore the asset to its original carrying value, and reverse the excess depreciation the buyer recorded on the marked-up basis. If the selling entity is a partially owned sub, the profit is still eliminated in full, then allocated between the controlling and noncontrolling interests.


## Common Pitfalls


Eliminating intercompany balances is mechanically simple. Keeping the entries, the reconciliation, and the tax treatment consistent is where consolidations break.


**Booking eliminations in the entity ledger.** Elimination entries belong in a consolidation layer or top-side group journal, applied after entity trial balances are aggregated and before consolidated statements are produced. Post them into an entity’s own general ledger and you corrupt that entity’s standalone financials, its tax-return support, and any entity-level covenant reporting. The eliminations should never change the books of any individual entity.


**Skipping reversal discipline.** Balance-sheet eliminations recur as long as the underlying balances exist, so the consolidation team re-derives them every period instead of assuming last period’s entry still holds. Income-statement and unrealized-profit eliminations are period-specific. Treating a one-time elimination as permanent, or forgetting to reverse a deferral once inventory sells externally, is a frequent source of consolidated misstatement.


**Tolerating out-of-balance intercompany accounts.** Entity A’s receivable from Entity B should equal Entity B’s payable to Entity A, dollar for dollar. In practice they drift from timing differences, a partial payment applied on one side, or an invoice booked in a different period. The tolerance at consolidation is zero. A mismatch left unresolved forces a plug, and a plug is an audit finding waiting to happen. Reconciling as part of[closing your company’s books each month](https://www.indinero.com/blog/business-bookkeeping-closing-your-companys-books/) catches these while the invoices are still fresh.


**Mispricing intercompany fees and loans.** A management fee with no agreement, or a loan with no stated interest, is clean to eliminate but exposed on the tax side. Under IRC Section 482, the IRS can reallocate income among commonly controlled entities to match arm’s-length pricing, and below-AFR loans draw imputed interest under IRC Section 7872. The accounting can be perfect and the tax treatment still wrong.


**Leaving intercompany revenue in a growth metric.** For subscription groups, an intercompany platform or license fee left in the top line inflates consolidated revenue and any ARR figure derived from it. Eliminating that revenue is what makes reported ARR reflect only external customers.


## The Audit-Ready Standard


Auditors want more than the elimination entries. They want the paper trail proving the intercompany activity was real, priced correctly, and reconciled to zero.


**Written intercompany agreements.** Every recurring arrangement, shared-service fee, management fee, license, cash pool, and loan, should have a signed agreement stating scope, pricing method, and terms. For loans, the agreement states principal, rate, and repayment schedule, and the rate should be at least the Applicable Federal Rate to satisfy IRC Section 7872. Documented pricing is also where the[management-fee tax treatment](https://www.indinero.com/services/business-tax-services/) holds up under review.


**Transfer-pricing support for cross-border fees.** When intercompany fees cross a border,[the arm’s-length standard under IRC Section 482](https://www.irs.gov/businesses/international-businesses/transfer-pricing) applies, and contemporaneous documentation is the penalty shield. Under[IRC Section 6662](https://uscode.house.gov/view.xhtml?req=%28title%3A26+section%3A6662+edition%3Aprelim%29) and Treasury Regulation 26 CFR 1.6662-6, a net Section 482 adjustment above the lesser of $5 million or 10 percent of gross receipts is a substantial valuation misstatement carrying a 20 percent penalty, and above the lesser of $20 million or 20 percent of gross receipts is a gross valuation misstatement carrying a 40 percent penalty. Contemporaneous transfer-pricing documentation can protect against both.


**Elimination schedules tied to entity trial balances.** Every elimination should trace to a matched pair of intercompany accounts on named entity trial balances. Dedicated intercompany accounts by counterparty, rather than one lumped account, make the matching auditable. The schedule reconciles from aggregated entity trial balances to the consolidated statements, with intercompany accounts netting to zero. That is what auditors mean when they talk about[audit preparation](https://www.indinero.com/blog/audit-preparation/) that holds up.


**Monthly intercompany reconciliation.** Reconcile intercompany balances monthly, not at year-end, so differences surface while the underlying invoices and payments are still fresh. Waiting for the annual close turns small timing differences into a cumbersome back-and-forth across every subsidiary. Monthly intercompany reconciliation is the cheapest insurance against a consolidation surprise.


## How Indinero Approaches Intercompany Eliminations


Indinero runs a CPA-led, GAAP-first monthly close, and intercompany reconciliation is built into that close instead of bolted on at year-end. Matched intercompany accounts by counterparty get reconciled to zero every month, elimination entries post in a consolidation layer with reversal discipline, and the supporting schedules tie back to each entity’s trial balance. Consolidation stops being a quarter-end scramble. It becomes audit-ready, not audit-painful.


Because one team owns bookkeeping, accounting, tax, and fractional CFO advisory, the accounting and the tax treatment stay consistent. Your bookkeeping team and your tax team are the same team, so the entries and the intercompany agreements don’t drift apart. The person booking the intercompany management fee is the person confirming the agreement exists and that the loan carries at least the Applicable Federal Rate, which keeps the eliminations clean and the IRC Section 482 and IRC Section 7872 exposure covered at once. That coordination is the difference between a defensible consolidation and a diligence cleanup.


This isn’t just bookkeeping across two entities. It’s a finance function that keeps parent-subsidiary, holding-company, and brother-sister structures reporting as one economic entity, month after month. Indinero has maintained continuous operations since 2009, holds a 5-star Clutch rating, and is SOC 2 compliant (2026), so the team that owns your intercompany layer this quarter still owns it next year. See how the[accounting services](https://www.indinero.com/services/accounting-services/) engagement handles multi-entity consolidation, and reach out for a free consultation. We’d love to learn about your business and find where we can help.


## Frequently asked questions


Below, the questions controllers ask most about booking intercompany eliminations, reconciling intercompany balances, and keeping the related tax treatment defensible.


### Why do intercompany balances have to be eliminated in consolidation?


Intercompany balances must be eliminated because ASC 810-10-45-1 requires the group to report as one economic entity. A receivable a company owes itself is not a real receivable, and revenue billed between entities is not real revenue. Consolidation collapses commonly controlled entities into a single reporting entity, so internal traffic like open account balances, sales, interest, and dividends has to come out first.


### What happens if intercompany accounts do not reconcile at close?


If intercompany accounts do not reconcile at close, the mismatch forces a plug, and a plug is an audit finding waiting to happen. Entity A’s receivable from Entity B should equal Entity B’s payable dollar for dollar, and the tolerance at consolidation is zero. Differences usually come from timing, a partial payment, or an invoice booked in the wrong period, so indinero reconciles matched accounts to zero every month while invoices are still fresh.


### How are intercompany management fees treated for tax purposes?


Intercompany management fees must be priced at arm’s length, because IRC Section 482 lets the IRS reallocate income among commonly controlled entities. A fee with no written agreement is clean to eliminate but exposed on the tax side, and cross-border fees need contemporaneous transfer-pricing documentation under IRC Section 6662 to avoid penalties. At indinero, the team booking the fee also confirms the agreement exists, so the accounting and tax treatment stay consistent.


### Do intercompany loans need written agreements and stated interest?


Yes, intercompany loans need a written agreement and a stated rate at least equal to the Applicable Federal Rate to satisfy IRC Section 7872. A related-party loan charging below the AFR is a below-market loan, and the IRS imputes interest to the lender at the AFR, which is set monthly. For August 2026, the mid-term AFR is 4.35% for loans of three to nine years, and indinero confirms each note carries at least that rate as the entries are booked.


### How does eliminating intercompany revenue affect reported ARR?


Eliminating intercompany revenue makes reported ARR reflect only external customers, because an internal platform or license fee left in the top line inflates it. When one entity bills another, that revenue is eliminated against the matching expense, so consolidated revenue shows only external sales. Skip it, and both consolidated revenue and any ARR derived from it are overstated by the internal fee, so indinero applies the top-line discipline ASC 606 demands across every subscription entity in the group.


### Should eliminations be booked in the ledger or in a separate consolidation layer?


Eliminations should be booked in a separate consolidation layer or top-side group journal, never in an individual entity’s general ledger. They apply after entity trial balances are aggregated and before consolidated statements are produced. Post them into an entity’s own ledger and you corrupt that entity’s standalone financials, its tax-return support, and any covenant reporting. Indinero posts elimination entries in a consolidation layer with reversal discipline, and the schedules tie back to each entity’s trial balance.


### How often should intercompany accounts be reconciled?


Intercompany accounts should be reconciled monthly, not at year-end, so differences surface while the underlying invoices and payments are still fresh. Waiting for the annual close turns small timing differences into a cumbersome back-and-forth across every subsidiary. Monthly reconciliation is the cheapest insurance against a consolidation surprise. Indinero builds intercompany reconciliation into a CPA-led monthly close, reconciling matched accounts by counterparty to zero every month rather than bolting it on at year-end.
