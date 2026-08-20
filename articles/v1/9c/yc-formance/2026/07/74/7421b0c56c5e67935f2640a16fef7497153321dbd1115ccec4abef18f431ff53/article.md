---
schema_version: "1.0.0"
document_id: "7421b0c56c5e67935f2640a16fef7497153321dbd1115ccec4abef18f431ff53"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/embedded-finance/intercompany-reconciliation-explainer-fintechs"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:89659d7f4d5c4be9aca27f7763f1797e716c2b96002ba11926bb07c1853ec511"
---

# Intercompany Reconciliation Explainer

A group-level audit surfaces a $5,000 discrepancy between two subsidiaries. The auditor's first question is not where the difference came from, but rather how long it has been there.


The issue is that finance and engineering teams had been disagreeing for weeks before anyone noticed where the $5,000 was. A subsidiary booked a $200,000 receivable, but the counterparty booked a $195,000 payable. The $5,000 survived the month-end, cleared into consolidation, and surfaced when the auditor pulled the trial balance.


Fintechs run into this more than most. By the time the close team opens the spreadsheet, the missing $5,000 is thirty days old, and the trail has gone cold.


However, when intercompany reconciliation is modeled correctly in the ledger, with both legs of every transaction posted in a single atomic operation and tagged with a shared identifier, it becomes a query against the tag. This guide explains how to get there.


## What is intercompany reconciliation, and why does it matter for fintechs?


Intercompany reconciliation is the process of matching and reconciling financial transactions and balances between entities within the same group to ensure accurate group-level financials and cash positions.


When the balance consolidates, internal transactions must not be double-counted. One entity's receivable must be offset by the corresponding payable of the counterparty entity. If both survive into consolidated books, the result is an inflated balance sheet and misleading financials.


Accounting standards make this requirement explicit. Under[US GAAP, ASC 810-10-45-1](https://dart.deloitte.com/USDART/home/codification/broad-transactions/asc810-10/roadmap-noncontrolling-interests/chapter-4-intercompany-matters-with-noncontrolling/4-3-transactions-between-parent-subsidiary) requires that intra-entity balances and transactions be eliminated in consolidated financial statements. Under[IFRS 10 paragraph B86(c)](https://www.ifrs.org/content/dam/ifrs/meetings/2023/january/iasb/ap13b-perceived-conflict-between-ifrs-10-and-ias-28-feedback-summary-of-the-outreach-activities-undertaken.pdf) , consolidated statements must eliminate all intercompany assets and liabilities, equity, income, expenses, and cash flows associated with transactions among group entities.


Fintechs feel the requirement harder because the same flow crosses licensed entities, local reporting calendars, and settlement boundaries, often across multiple settlement rails between entities that hold customer money.


## The five intercompany flows that matter most in fintechs


Settlement, foreign exchange (FX), and licensing boundaries drive most of the reconciliation risk. The flows worth modeling explicitly cluster around a few high-frequency, high-value movements:


1. **Cross-entity settlement and safeguarding funding** : moving customer-related funds between entities to meet safeguarding obligations.
2. **Card program prefunding** : one entity funding another's card scheme balance ahead of authorizations.
3. **Treasury liquidity sweeps** : pooling and redistributing cash across entities to manage working capital.
4. **Internal FX between entities** : converting currency for one entity through another, where the rate and spread must agree on both sides.
5. **Cross-entity revenue and fee sharing** : splitting platform fees or revenue between the entity that earned it and the entity that booked it.


Each flow needs a named owner, an agreed direction, and pre-agreed FX, fee, and timing rules before the transaction posts.


## Five steps on how intercompany reconciliation works


An intercompany reconciliation workflow breaks down into five concrete steps: inventory the relationships, align the data, match both sides, resolve breaks, and eliminate matched balances.


### Step 1: Inventory intercompany accounts and relationships


List every entity, its intercompany accounts, and the types of intercompany transactions in use. Review[core ledgers and subledgers](https://www.formance.com/glossary/sub-ledger) to identify transactions flagged with intercompany codes.


For fintech groups, the relevant categories are funding transfers, shared service charges, internal FX, and revenue or fee shares. Use standardized transaction coding, such as dedicated vendor or customer codes for group entities or specific account categories, so identification follows a repeatable system.


### Step 2: Align data, identifiers, and cut-offs across entities


Standardize shared IDs, currency rules, and cut-off times so that both sides of each transaction can be reliably matched. Data extraction gets harder when entities use different ERPs, data structures, and levels of transaction detail.


Start with shared account codes, common transaction identifiers, and aligned cutoff dates. Once the data is consistent, automated matching can clear the routine work.


### Step 3: Match "side A" and "side B" and find breaks


Match transactions between entities and flag every break. If Subsidiary A records a $10,000 intercompany receivable, Subsidiary B must record a matching $10,000 intercompany payable. Matching can run at two granularities, including company-to-company, where aggregate seller and buyer balances are compared, and invoice level, where individual transactions are matched across entities.


**Break type** **Example**


Missing side Entity A posted; Entity B has no record


Amount difference $250,000 vs. $249,800 from timing, rounding, or FX


FX mismatch Different rates are applied at posting vs. reporting


Timing difference Payment in transit: Entity A sent, Entity B has not yet received


### Step 4: Model a simple intercompany posting in a programmable ledger


Prevent miscalculation by creating both sides of the intercompany transaction in a single atomic operation. Then, tag both legs with the same identifier so reconciliation can pair them on read.


The root cause of most intercompany miscalculations is that the two sides of a transaction are posted separately, by different systems, at different times. By the time the close team runs reconciliation, the break is already 30 days old.


The fix is to post both legs in a single atomic operation. Atomic means either both sides commit, or neither does. There is no state where Entity A's receivable exists without Entity B's corresponding payable, because the transaction cannot partially succeed.


In[Numscript](https://playground.numscript.org/?template=simple-send) , you can record multiple send statements within the same script executed as a single transaction. The UK entity's outbound bank movement and the EU entity's inbound bank movement post together, tagged with the same intercompany_id. Reconciliation then runs as a query against that tag, not as a search through 30 days of unmatched postings.


**Real-world party** **Ledger account** **Holds**


UK entity's claim on the EU entity @entities:ukEmi:intercompany:receivable what the UK entity is owed


Cash leaving the UK entity's bank @entities:ukEmi:banks:main UK entity bank boundary


Cash entering the EU entity's bank @entities:euSub:banks:main EU entity bank boundary


EU entity's obligation to the UK entity @entities:euSub:intercompany:payable what the EU entity owes


The UK entity sends €2,000,000 to the EU entity. Both legs post in a single transaction with the same transfer ID:


// INTERCOMPANY_FUNDING


// Event: UK entity funds EU entity card program with EUR 2,000,000; both sides booked together


```text
send   [EUR/2 200000000]   (
source   =   @entities:ukEmi:banks:main   allowing   unbounded   overdraft
destination   =   @entities:ukEmi:intercompany:receivable
)


send   [EUR/2 200000000]   (
source   =   @entities:euSub:intercompany:payable   allowing   unbounded   overdraft
destination   =   @entities:euSub:banks:main
)


```


set_tx_meta("event_type", "intercompany_funding")


set_tx_meta("intercompany_id", "ic0001")


set_tx_meta("counterparty_pair", "ukEmi:euSub")


Both legs carry the same intercompany_id and counterparty_pair metadata, and matching on read is a filter, not a reconstruction.


### Step 5: Post corrections and eliminate intercompany in consolidation


Post agreed adjustments first, then eliminate the matched balances when producing group financials. Reconcile at the line level, and then park unresolved differences in a clearing account while they are reviewed.


Settle the difference by offset, netting, or a separate payment whose remittance advice points back to the original transaction. The elimination adjustment offsets the intercompany transaction so values are not double-counted at the consolidated level. Run reconciliations on a regular close cadence so large adjustments do not build up at year-end.


## How should you architect a ledger for intercompany reconciliation?


Multi-entity fintechs should model intercompany transactions directly in their operational ledgers, with per-entity ledgers that include explicit intercompany accounts, plus a shared mapping layer that feeds the core ledger and consolidation tools. When intercompany is a first-class structure in the system of record, the receivable-and-payable pairing is enforced at the point of posting.


Generate intercompany entries at the point of transaction. When the funding moves, ledger postings for both sides are created together, tagged with the same identifier, and immediately available for automated reconciliation. External banks or payment providers still move the actual funds, while the ledger records the postings for those movements.


If the only place both sides come together is the consolidation tool, breaks will accumulate for thirty days before anyone looks. Formance is an[open-source, programmable core ledger](https://www.formance.com/) that unifies fiat and digital assets with regulatory-grade traceability.


Both legs of an intercompany transaction post in a single atomic operation, so the paired receivable and payable move together from the first posting event, and reconciliation becomes a confirmation step.


## What are the best practices for intercompany governance in multi-entity fintechs?


Atomic posting handles the mechanics while policy handles the inputs. Define a group-wide intercompany policy that sets the chart of accounts, intercompany codes, FX rate sources, pricing formulas, and cut-off rules for each flow.


Both entities should apply the same rate from the same source at the same cut-off, so FX mismatch and timing differences disappear before reconciliation runs.


Three operational practices carry most of the weight:


1. **A shared calendar** so both sides of every flow post within the same cut-off window.
2. **Ownership by entity and by flow,** so every intercompany relationship has a named owner who is accountable for both legs.
3. **A dispute-resolution process** so that mismatches are investigated and cleared before consolidation.


When policies, prices, and cutoffs are agreed upon before transactions occur, the close team verifies the result.


## Which systems do you need for intercompany reconciliation?


Policy, atomic postings, and automation work as a system: clear policies,[atomic ledger postings](https://www.formance.com/blog/engineering/ledgering-all-the-way-up) , and automated exception matching. Good policy with manual matching still buries the team in spreadsheets. A strong reconciliation tool on top of single-sided postings cannot reconstruct connections that the ledger never recorded.


Locate the weakest layer. If intercompany is modeled with single-sided entries, or if breaks appear weeks after posting, adopt or strengthen the ledger to post both legs atomically. If the ledger is already solid but matching is manual, add a reconciliation engine that plugs into the ledger and the consolidation stack.


The[Formance Reconciliation module](https://www.formance.com/modules/reconciliation) is designed for verifying that your internal Ledger balances match what your payment providers and banks actually hold.


For intercompany matching specifically, the right tooling path starts with the Ledger itself. Because both legs of every intercompany transaction carry the same intercompany_id and counterparty_pair metadata tags, reconciliation runs as a query against those tags. No separate matching engine is needed when the pairing key is baked into the posting at the time of the transaction.
