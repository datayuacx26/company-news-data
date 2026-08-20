---
schema_version: "1.0.0"
document_id: "4079eab39d01e998d053c32d5c1bf2795ccee0ab074ced02640c294e807596a8"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/embedded-finance/what-is-bank-reconciliation"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:361571eac2cd19890ad326a753fd7657e7c59d6183e2e2fe47b536d10fbaae9f"
---

# What is Bank Reconciliation?

On August 11, 2020, Citibank meant to wire $7.8 million in interest on a Revlon loan. It[wired $894 million](https://www.nysd.uscourts.gov/sites/default/files/2021-02/20cv6539%20Citibank%20Opinion.pdf) instead, the full outstanding principal, three years early. Three employees reviewed the transaction before it was sent, and the verification process failed. The error surfaced the next day, when someone compared the internal records with what had actually left the bank and found nearly $900 million sitting with 315 lenders.


Reconciliation caught a nine-figure error after every other safeguard failed. From an engineering angle, building payment infrastructure, bank reconciliation is the control that proves internal cash records agree with external bank activity and explains every difference before it becomes operational risk.


Bank reconciliation depends on comparable records, deterministic matching, exception handling, and a ledger design that lets the control run continuously.


## What is a bank reconciliation?


Bank reconciliation is the process of comparing your internal cash records against your bank statement to confirm that the balances and the individual transactions match. The reconciliation proves that the two agree and explains every line where they temporarily don't.


In payment systems, the two sides rarely align automatically at a cutoff. Asynchronous settlement across rails with different cycles is how payment systems normally behave. Reconciliation turns that expected drift into a verified, explainable number.


## Key concepts engineers should understand


Bank reconciliation relies on three things: a definition of your internal records, a definition of the bank statement, and a clear taxonomy of the reasons the two diverge.


1. **Internal records as a source of truth**


Internal records are your system’s source of truth for cash, including a[general ledger](https://www.formance.com/blog/financial-operations/what-is-a-ledger) , a cash book, or a transaction log recording every receipt and disbursement as it happens.


In a payment system, internal records typically include your core ledger and settlement files from each payment service provider (PSP), with sufficient immutable transaction detail to reconstruct your balance at any point in time.


1. **The bank statement of all recorded transactions**


The bank statement is a periodic record of all transactions on the account, including deposits, withdrawals, fees, interest, and other activity. In practice, you receive it as a structured file.


It is formatted for end-of-day statements, with per-entry fields such as amount (<Amt>), credit/debit indicator (<CdtDbtInd>), booking date (<BookgDt>), and an optional bank-assigned reference (<AcctSvcrRef>). Older formats carry similar data but add parsing overhead per integration when banks expose different transaction code schemes.


1. **A taxonomy of why records diverge**


Internal records and bank statements diverge for five recurring reasons: timing differences, boundary fees, batched settlements, in-flight multi-stage transactions, and gross-versus-net misalignment.


1. **Timing differences:** A deposit recorded in your books hasn't cleared the bank yet. Cash in transit clusters around month-end cutoffs, weekends, and international settlements.
2. **Fees deducted at the boundary:** You recorded a $1,200 receipt, but the bank deposited only $1,000 because the PSP took its cut before settling.
3. **Batched settlements:** A card network may post multiple payments as one net entry, and batch booking can obscure individual transaction details.
4. **In-flight multi-stage transactions:** Authorization, capture, and settlement occur at different times, and refunds or chargebacks arrive days later.
5. **Gross-versus-net misalignment:** A ledger that posts each sale at gross and expects a gross bank receipt shows a per-transaction shortfall that compounds across a batch.


Each of these taxonomies is, at its core, a ledger modeling problem.


The bank statement is fixed and outside your control, so whether a divergence reads as an *explainable timing item* or an *unexplained break* depends entirely on whether your ledger captured enough structure to account for it.


A ledger built, like Formance’s open-source,[programmable core ledger](https://www.formance.com/modules/ledger) , on immutable double-entry postings provides the bank statement with a stable, queryable counterpart to match against, along with the timestamps, account paths, and transaction states needed to explain every taxonomy.


## How does bank reconciliation work? A step-by-step engineering guide


The reconciliation flow has five stages: normalize bank and internal data, match bank lines to internal records, post bank fees and PSP settlements as their own ledger legs, handle any remaining unmatched items, and confirm the reconciliation is closed with adjusted balances.


### Step 1: How do I normalize bank and internal data before matching?


Pull bank transactions and internal cash entries for the same period, normalized to consistent identifiers, time zones, and currencies, before you attempt any comparison. Normalization determines whether everything downstream works.


Settlement and reconciliation data from PSPs and banks vary by provider and model, so your reconciliation system parses settlement files and lines them up against your ledger.


Store an effective_at timestamp (when the transaction occurred in the external system) separately from your system clock. Reconciling against wall-clock time makes in-flight settlements read as missing; reconciling as of the effective date does not. Normalize currencies and store the provider's transaction ID alongside your internal record so the two sides share a join key.


### Step 2: How do I match bank lines to internal records, including batches?


Match transactions by amount, date window, and stable identifiers, then group internal records in batches until their sum equals a single bank line. Exact matching on amount and date clears the most deterministic cases; the rest need tolerance windows, grouping, or fuzzy matching.


Reconciliation has four match patterns, and your engine needs all of them:


**Pattern** **Cardinality** **When it applies**


One-to-one 1:1 Wire transfers, real-time payments (RTP), and single invoice payments with stable references


Many-to-one N:1 Card net settlements, Automated Clearing House (ACH) batches, batched PSP payouts


One-to-many 1:N Partial payments, installments, split settlements


Many-to-many N:M Complex business-to-business (B2B) netting, cross-currency multi-leg flows


For the N:1 case, query your ledger for candidate groups of pending transactions whose sum equals the bank's single net line. Date matching should use a tolerance window because a card settlement can arrive after the internal transaction.


A common matching pattern combines exact invoice matching, a small date-tolerance window, and exact amount as the balancing attribute. When identifiers drift across systems (Client_ID versus ClientID), exact matching fails, and you fall back to normalization and fuzzy matching for short reference codes.


### Step 3: How should I post bank fees and PSP settlements to avoid per-transaction shortfalls?


Post bank-specific items (fees, interest, batched settlements) as their own ledger legs at the moment of settlement, so the ledger already knows the bank line will arrive net. Teams that post a sale at gross and expect a gross bank receipt create a per-transaction shortfall that the bank cannot reconcile.


Consider a $10,000 merchant sale settled through a PSP that takes a 3% fee. Three parties touch the money.


**Real-world party** **Ledger account** **Holds**


The PSP settlement boundary @platform:psp:provider:settlement cash crossing in from the PSP


The merchant @merchants:acme:payable what the platform owes the merchant


The platform's fee income @platform:revenue:fees the fee the PSP deducts and the platform records


A $10,000 sale settles. The PSP keeps 3% ($300.00); the merchant is owed the remaining $9,700.00. The settlement boundary records the gross figure so that it can be reconciled against the PSP report, and the net $9700.00 is what the bank will actually deliver.


In[Formance’s Numscript language](https://www.formance.com/blog/engineering/numscript) , this transaction looks like this:


```text
// PSP_SETTLEMENT
// Event: a $10000.00 sale settles; PSP keeps a 3% fee, merchant owed the rest
send   [USD/2 1000000]   (
source   =   @platform:psp:provider:settlement   allowing   unbounded   overdraft
destination   = {
3  %   to   @platform:revenue:fees
remaining   to   @merchants:acme:payable
}
)
set_tx_meta  (  "event_type"  ,   "psp_settlement"  )
set_tx_meta  (  "settlement_id"  ,   "stl001"  )


```


**After it posts**


@platform:psp:provider:settlement −$10,000.00 · @platform:revenue:fees +$300.00 · @merchants:acme:payable +$9700.00


The fee now lives in its own account with a stable path, and the settlement boundary's balance is your running record of everything that crossed from the PSP. You can total @platform:psp:provider: to reconcile against the provider's report, and the merchant payable nets to the figure the bank actually moves.


### Step 4: What do I do with unmatched items after the matching run?


Surface every bank line and internal record that didn't match, tag each by cause, and decide which need an adjusting posting versus which will clear on their own. An unmatched item needs to be classified before you can call it an error.


Tag exceptions into a stable taxonomy, such as deposits in transit, outstanding payments, bank fees not yet recorded, interest income, duplicates, foreign exchange (FX) differences, and genuinely unexplained items.


Timing differences will reconcile themselves once they clear, but you must carry them forward into the next cycle rather than writing them off. Fees and interest get adjusted in journal entries. A duplicate processed twice internally gets a reversal posting; a duplicate that the bank created gets a phone call to the bank. Items you cannot identify go to a suspense account while you investigate, and stay out of the main balance.


A retried payout that reuses the same idempotency key books the transfer twice in your ledger. The PSP never created a matching record. The duplicate ledger entry appears here as an unmatched internal item with no bank counterpart, and the fix is upstream in your write path.


### Step 5 – How do I confirm that the reconciliation is closed and that the balances agree?


After the adjustments, confirm that the adjusted internal cash balance equals the adjusted bank balance, and log the reconciliation as a completed control with supporting evidence. After you account for interest, fees, returned items, deposits in transit, and outstanding payments, the adjusted book balance and adjusted bank balance must be equal.


Make corrections by appending postings and keep history unchanged. For a payment originally recorded incorrectly as $10,000 when it should have been $8000, post a correcting entry rather than overwriting the original.


Append-only corrections enable dispute handling and historical debugging months later, and preserve a closed reconciliation period as a verifiable snapshot.


## Where engineering decisions can streamline bank reconciliation


Engineers must consider stable identifiers, bi-temporal data handling, and comprehensive transaction tracking when building a bank reconciliation process to ensure accurate matching and resolution of discrepancies between internal records and bank statements.


### Stable identifiers on every internal record


Carry the provider's transaction ID on every internal record and use it as the primary match anchor. Stable globally unique identifiers make accidental collisions unlikely in practice. Without a shared ID, you are matching on amount and date alone, which limits automation to the most deterministic cases.


### Bi-temporal timestamps for financial settlements


Separate the effective date from the posted date with[bi-temporal timestamps](https://www.formance.com/blog/financial-operations/card-timeline-2) . Separating the two dates determines whether in-flight settlements read as missing or as expected timing items.


### Status fields that mirror the bank's state model


Mirror the state labels your bank or PSP actually exposes, rather than forcing every payment into a single generic "done" flag. If a webhook sends a success event, your ledger marks the payment as pending while the PSP has it paid, and the gap goes undetected until the next run. Model statuses explicitly and reconcile them.


### Double-entry enforcement at the database layer


Single-entry or inconsistently written ledgers are the primary source of reconciliation risk.[Double-entry](https://www.formance.com/blog/engineering/defining-double-entry) makes reconciliation mathematically verifiable: for every batch, the sum of debits equals the sum of credits. Double-entry pairs naturally pair with[an append-only design](https://www.formance.com/blog/engineering/money-movement-architecture-for-multi-rail-fintechs) , where immutable entries preserve the history of operations.


Derive balances from postings and keep mutable balance fields out of the cash source of truth. Cached balances updated with write-behind logic drift the moment one of those updates fails silently, and a drifted balance is a reconciliation break of your own making.


## How to think about automating bank reconciliation as an engineering team


Moving from manual processes to automated systems, incorporating lookback windows, and utilizing bi-temporal ledgers automates bank reconciliation.


### Move from spreadsheets to a scheduled engine


You start with manual matching in spreadsheets: workable at low volume, but hard to audit and easy to miskey. You move to rule-based matching, where exact and tolerance rules clear deterministic items and surface exceptions for human review.


A reconciliation engine running as a scheduled control against live bank feeds, structured as a four-layer pipeline: ingest and normalize data from every source, apply a matching rules engine across all four cardinality patterns, route unmatched items into an exception queue with defined ownership, and keep a full audit trail with rule versioning throughout.


### Lookback windows and exception events


Use a lookback window and emit exception events. A card settlement arriving after the internal transaction needs the engine to scan a rolling window long enough to cover expected settlement delays.


When internal and external records disagree, a human reviewer should get a structured event describing the break before the month-end spreadsheet review. The engineering team should drive the exception rate down over time by fixing the upstream causes that keep surfacing in the exception queue, starting with phantom records and dropped webhooks; gross/net mismatches need separate ledger modeling.


### Why the bi-temporal ledger matters


A[reconciliation](https://www.formance.com/modules/reconciliation) engine built on a bi-temporal ledger runs cut-offs as of a transaction's effective date, not your system clock. That is what prevents in-flight settlements from appearing missing. The effective-date-vs-posted-date separation is a ledger property, not a reconciliation configuration, which is why it has to be in the schema before reconciliation runs, not patched in afterward.


## The architectural takeaway on whether reconciliation is continuous or manual


Bank reconciliation is the control that catches what every other safeguard missed, as the Citibank $894 million wire showed. How it runs, say as a monthly spreadsheet exercise or as a continuous check, is decided upstream, in how you model identifiers, timestamps, statuses, and postings.


Build the ledger for reconciliation up front, and the daily exception queue shrinks to the genuinely ambiguous cases.


Stable identifiers, bi-temporal timestamps, status fields that mirror the bank's state model, and double-entry enforced at the database layer are not a reconciliation configuration. They can be fixed with a[programmable core ledger](https://www.formance.com/blog/engineering/what-is-programmable-money) that enforces double-entry and immutability, so you can retire brittle matching scripts that break every time a provider changes its file format and run reconciliation as a control that your system manages on its own.
