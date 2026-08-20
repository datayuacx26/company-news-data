---
schema_version: "1.0.0"
document_id: "73309f7895f4741baff2ef16f0e1d4f3a1aab98122dcccffa0644c4c2ee4fd94"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/how-to-know-when-you-need-ledger-as-a-service"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:8f91b8bae8bb855a8820afed022ac02a52552580bbbc352e6bdd3f15921fcee1"
---

# Ledger as a Service: When You Need One

A U.S. fintech launches on one payment processor, with a homegrown ledger that operates on a few SQL tables, stores balances as columns and writes postings in application code.


Then the engineering team adds a second processor for cards, a banking partner for ACH, and a cross-border rail for international payouts. Now the same balance is stored across four external systems, with four different formats and four different definitions of "settled." The lack of a unified ledger causes this miscalculation, regulatory issues and delayed reconciliation.


When fintechs move money across multiple providers, the missing ledger is the single internal layer that tracks all money movement across every rail at once. Whether that ledger is homegrown and kept internal or hosted externally is how fintechs discuss adopting Ledger as a Service (LaaS).


## What is Ledger as a Service?


Ledger as a Service is an API-accessible operational ledger infrastructure for modeling accounts, postings, balances, fund flows, custom rules, and reconciliation workflows, without maintaining the ledger in-house.


A LaaS ledger serves as a transaction-level system embedded in the product or operational stack, serving as the source of truth for the financial state. It is distinct from your accounting system because a LaaS ledger captures every individual posting in real time as money moves, while the accounting system consumes summarized positions for reporting, tax, and close.


In the stack, LaaS sits between your core product logic and your external financial providers:


- **Above LaaS:** Your product (wallets, marketplaces, and neobank features) and your accounting/GL system
- **LaaS layer:** Accounts, postings, balances, and reconciliation primitives
- **Below LaaS:** Payment service providers (PSPs), banking partners, card networks, and cross-border rails


The core primitives a LaaS platform exposes are accounts (named containers for value), postings (the double-entry movements between accounts), and balances (the derived state of any account at any point in time). Every fund flow, no matter how complex, is expressed as a set of postings against accounts.


LaaS is most relevant for organizations whose product is money movement, including fintechs, neobanks, marketplaces, PSPs, crypto platforms, embedded finance providers and any B2B platform that handles customer funds across multiple rails.


Many early-stage fintech teams skip implementing a ledger and resort to common workarounds, such as tracking funds in Excel or trusting their payment processors' transaction logs. However, these outdated models fail in modern fintech organizations.


## Why Ledger as a Service matters in modern fintech


LaaS gives fintechs a single internal view of accounts and balances across all providers they integrate with, so all transactions are tied to the same record.


Every external provider operates with its own formats, timelines and definitions of correctness. Fragmentation occurs when a transfer appears "completed" at the PSP but "pending" at the bank, and the internal record must determine which state's rules govern the balance. A unified ledger merges fragmented provider views into one authoritative record from initiation through settlement.


A single internal view also makes new products easier to ship. A wallet or an[omnibus account](https://www.formance.com/glossary/omnibus-account) structure can be modeled on top of one ledger that tracks any asset you define, with no new schema or reconciliation pipeline per feature.


Regulation further influences the importance of a unified ledger. Every new payment rail and market adds to your reconciliation surface, and all regulated money movement requires contemporaneous, immutable records of every state change. Once you are operating across multiple providers under regulatory scrutiny, the ledger becomes infrastructure you have to defend.


This was evident in 2021, when the Commodity Futures Trading Commission (CFTC)[fined Tether](https://www.cftc.gov/PressRoom/PressReleases/8450-21) $41 million after finding the company tracked reserves manually and could not show their true status in real time. The Tether failure shows what happens when ledger correctness is treated as an application-layer concern rather than a storage-layer guarantee.


## Why modern fintechs move from homegrown ledgers to LaaS


Most homegrown ledgers start as a few SQL tables, with balances stored as columns on user records and posting rules scattered across services. Without financial invariants enforced in the storage layer, every rule governing accounts, transfers, and balances becomes a code convention that can be bypassed under pressure.


Complexity compounds every time you add a new payment rail, region, or product. Each provider brings its own formats and timelines, every feature demands another schema change, and teams spin up shadow ledgers in spreadsheets or side services to keep shipping. The result is one customer’s balance represented in several systems, with no single ledger that can explain how it got there.


LaaS offloads that complexity onto a purpose‑built operational ledger that enforces double‑entry, immutability, and bi‑temporality at the storage layer, so correctness becomes a property of the data model instead of a convention in application code.


## Capabilities you get with Ledger as a Service


LaaS gives you platform‑level guarantees, including atomic posting, bi‑temporal records, idempotency, immutability, multi‑currency support, real‑time events, and an audit‑ready history.


### Atomic posting


Every transaction succeeds or fails as one unit, with a double-entry balance validated on every write. A purpose-built ledger that rejects unbalanced writes at the API layer is safer than application-level validation, which is exposed to bugs and race conditions.


### Bi-temporal records


Tracking both when an event was recorded and when it was effective keeps historical balances accurate when transactions arrive out of order. For card- and settlement-lagged systems, missing the recorded-versus-effective distinction can make cash flow statements wrong and turn reconciliation into detective work.


### Idempotency


Repeating an operation should produce the same result every time, because distributed systems routinely retry after network failures or lost responses. Without idempotency, a POST /payments called twice charges $20,000 instead of $10,000.


### Immutability and append-only history


Immutability and append-only history turn the ledger into a tamper-evident audit log. Once a transaction is recorded, it is never edited or deleted. Instead, corrections are modeled as compensating entries, so you can always reconstruct how a balance changed over time without relying on mutable running totals.


### Multi-currency and multi-asset support


Multi-currency and multi-asset support enable a single LaaS ledger to track all the value your platform holds. Fiat, stablecoins, and other asset types share the same posting model and account structure, so you do not re-implement ledger logic every time you add a new currency or product line.


### Real-time events and integration


Real-time events and integration let the ledger sit at the center of your fund flows rather than serving as passive storage. Each change in ledger state can emit events into your product and operations stack, keeping internal workflows, notifications, and reconciliation logic aligned with what actually moved across external rails.


### Audit-ready history for regulators


Audit-ready history by design means the ledger produces exam-grade evidence without extra work. Combining double-entry accounting, immutability, and precise timestamps makes every balance traceable back through a clear chain of postings, so regulators and auditors can follow the story directly from the ledger.


## When to move to Ledger as a Service


Implementing a LaaS usually stems from trigger events, including a regulatory deadline, a payment rail expansion, manual work consuming engineering capacity, or a compounding incident.


### Regulation deadline


A regulator is coming, and the homegrown ledger won't pass scrutiny. DORA, MiCA, and the GENIUS framework all impose reserve traceability and audit requirements that an in-house ledger built before compliance was on the roadmap can't meet without months of rework.


### Payment rails expansion


You started with two bank accounts and a PSP. Now, 10 to 20 rails all need to talk to one central ledger, and every new integration breaks reconciliation in a different way.


### Manual work as an engineering tax


Engineers stop shipping new products because they're buried in reconciliation scripts and exception workflows. Month-end close takes a day of fixes and alignment instead of hours.


### A compounding incident


A double-payout bug, a settlement discrepancy nobody can trace, and a fund-attribution gap caught by an audit are compounding incidents that trigger a downward spiral for fintechs.


For example, in February 2026,[Bithumb credited 695 users](https://www.ccn.com/education/crypto/bithumb-620000-fake-bitcoin-error-explained/) with 2,000 BTC each instead of 2,000 Korean won, which was a data-entry error that resulted in an impossible ledger state, which every downstream system then treated as real. The exchange's internal ledger had no layer that could reject a posting it couldn't actually cover.


One incident like that reframes the ledger conversation from a theoretical concern into a board-level question about reliability and auditability.


## The LaaS vendor dependency


Adopting LaaS shifts complexity from your team to a provider, but it also introduces a dependency you need to understand.


LaaS providers can limit flexibility when your workflows need deep adaptation, costs grow as modules and usage expand, and a deeply integrated, proprietary subledger slows product velocity when every new feature requires the vendor's involvement.


### Integration and migration realities


Migrating a live ledger is the part of the LaaS decision most teams underestimate, and your evaluation should account for migration cost. Keep the old and new systems in parallel for a defined validation period until exit criteria are met.


A safer pattern is to treat the legacy ledger as the source of record while discrepancies are investigated and corrected in the target system. A conservative migration plan may include one close cycle in parallel to surface discrepancies that only appear under real operating conditions.


Decide whether you are migrating GL-level data or rebuilding subledger detail for audits. A pilot subset in a sandbox can expose mapping issues before a full backfill. With Formance Ledger, for example, the[bulk processing API](https://docs.formance.com/modules/ledger/working-with-the-ledger/bulk-processing) accepts a custom timestamp to backdate transactions while still validating timeline consistency on every insert. That lets teams rebuild subledger detail without rewriting history or compromising bi‑temporal correctness.


Before going live, compare trial balances and transaction traceability records across systems and address any inconsistencies before the cutover.


## How open-source changes the LaaS equation


LaaS platforms are proprietary subledgers that live between your product and your data. Formance takes a different path: an open‑source, programmable core ledger you can deploy on your own infrastructure. The[core ledger is MIT‑licensed](https://www.formance.com/pricing) , so the storage model, concurrency guarantees, and double‑entry enforcement are inspectable end-to-end by your engineers, your security team, and the auditors who will eventually ask how balances are reconstructed. The lock-in risk with LaaS is closer to that of an open-source database dependency than to a black-box SaaS contract.


Around the Formance Ledger, Formance's Numscript DSL gives financial logic a shared language readable by both engineers and finance teams. A $10,000 marketplace transaction that pays the seller 85%, takes a 10% platform fee, and routes 5% to a reserve account is one transaction in one place:


```text
send   [USD/2 10000]   (
source   =   @users:alice:wallet
destination   = {
85  %   to   @sellers:bob:wallet
10  %   to   @platform:fees
5  %   to   @platform:reserves
}
)
```


Account paths like @users:alice:wallet and @platform:reserves are first-class names in the ledger. Holds, settlement, fee captures, reserve movements, and marketplace splits all live in the same multi-party, multi-step posting model.


## A ledger-first path forward


LaaS is one way to solve the unified-ledger problem. The same correctness guarantees are also available from an open-source ledger you deploy yourself


You can start by cloning the open-source Formance Ledger on[GitHub](https://github.com/formancehq/ledger) , running it locally, and posting your first double-entry transaction against the same data model you would migrate onto.
