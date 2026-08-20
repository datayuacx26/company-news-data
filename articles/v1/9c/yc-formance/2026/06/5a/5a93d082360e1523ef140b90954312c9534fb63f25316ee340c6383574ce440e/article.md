---
schema_version: "1.0.0"
document_id: "5a93d082360e1523ef140b90954312c9534fb63f25316ee340c6383574ce440e"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/what-is-a-ledger"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:c6c1a81b6f3e8ef794a16fa24375b83a3bac3c18766953c063d1265e59822c83"
---

# What Is a Ledger? A Guide for Software Engineers

You're running a fintech product that moves customer money through a payment processor, and one morning, a reconciliation job flags a $42,000 gap between your internal balances and the processor's settlement report.


As engineers, your team spends three days grepping application logs, replaying webhook payloads, and diffing the transactions table against the payment processor's API because the wallet has to match what the processor says happened. The root cause of the $42,000 discrepancy: a webhook retry posted the same transfer twice because the debit and credit were written as two separate, uncoordinated database calls instead of a single atomic transaction.


That discrepancy comes from moving money without a real ledger. And "ledger" is one of those words engineers use loosely: sometimes it means a transactions table, sometimes an event log, or sometimes whatever code sits closest to the payment processor. Those things are not the same, and the difference determines whether bugs like the one above are even possible.


This guide is a working definition for software engineers: what a ledger actually is, the rules it must enforce, and what happens when those rules are missing.


## What is a Ledger?


A ledger is the system that records every movement of money in your product as a pair of postings (a debit and a matching credit) and computes balances by summing those postings.


This ledger infrastructure ensures auditability and makes every balance verifiable by reconstruction: **balance(account) = ∑ postings(account)**


Engineers often mistake a ledger for transaction tables, event logs, and payment processors, but none of those enforce double-entry or own the financial truth the way a[core ledger](https://www.formance.com/glossary/core-ledger) does.


A transactions table can be edited and has no rule that total debits equal total credits, an event log is append-only but enforces no financial rules, and a payment processor moves money over external rails while the ledger records what those events did to your balances.


## How a Ledger Works: The Mechanics Engineers Need to Understand


A properly built ledger rests on four things working together: double-entry balance validation checked on every write, a chart of accounts naming scheme, balances calculated from financial postings, and two timestamps on every entry. Each one prevents a specific failure that occurs during ledger management and execution.


### Double-Entry as a Data Integrity Rule


[Double-entry](https://www.formance.com/blog/engineering/defining-double-entry) is checked by the system before it writes the transaction, not after. Every transaction must produce financial postings where the total amount of debits equals the total amount of credits, and the system rejects any write that doesn't balance.


### The Chart of Accounts: Your Schema for Money


The chart of accounts is a naming scheme in which account numbers indicate the account type and its position in the hierarchy.


A prefix system like 1xx, 2xx, 3xx, 4xx, and 5xx lets you query ranges and roll up totals without storing the type as a separate column on every row.


Each account type has a sign rule enforced by the database. Asset and expense accounts have debit-normal balances (balance = debits_posted − credits_posted). Liability, equity, and income accounts have credit-normal balances.


### Derived Balances to Calculate All Postings


A ledger never stores a balance as a number you can update. Instead, it adds up all the entries for an account whenever you ask: balance = sum of postings.


The moment you save a running total in its own column, that number can fall out of sync with the actual history, and you won't know which one is correct without adding the entries up again.


### Recorded Time vs. Effective Time


Every financial posting needs two timestamps:


- Effective time is when the event happened in the real world ("valid time")
- Recorded time is when the system wrote it down ("transaction time")


A card transaction authorized on Monday but settled on Wednesday has an effective time of Monday (when the cardholder authorized the spend) and a recorded time of Wednesday (when settlement was confirmed and your system recorded it).


A ledger with only one timestamp can't answer "What was the balance on Tuesday?" accurately, because the transaction was committed on Monday but only recorded on Wednesday — single-timestamp systems will either miss it or backdate everything.


## The Three Ledger Types and Which One You Actually Need


Most teams calling their system a "core ledger" have actually built a sub-ledger. It's a mislabel that costs them the moment a second product line ships or finance asks a cross-product question.


Most teams don't need a general ledger at all because their finance team or ERP already owns one. What they've built and labeled a "core ledger" is usually a sub-ledger for one product domain (wallets, payouts, cards) quietly elevated to source-of-truth status.


Three distinctions worth keeping straight:


- A[general ledger](https://www.formance.com/glossary/general-ledger) is the company-wide book of record for regulatory reporting and financial statements; engineers feed it, they don't own it.
- A core ledger is defined by scope — every product domain that moves money, under one set of invariants and one chart of accounts.
- A[sub-ledger](https://www.formance.com/glossary/sub-ledger) is the detail behind a single line of the core ledger or GL, scoped to one product domain. If your "core ledger" only knows about one domain, it's a sub-ledger with ambitions — and that's fine, as long as you know that's what you've built.


Open-source ledgers, which are core ledgers published under permissive licenses such as MIT or Apache 2.0, let you read the code that handles your money, avoid vendor lock-in, and run them on your own infrastructure.


## Four Invariants That Separate a Correct Ledger From a Broken One


Four rules must hold for a ledger to produce the right balances: immutability, idempotency, consistency, and atomicity. They depend on each other, so breaking any one of them weakens the infrastructure of the remaining.


### 1. Immutability That Never Updates or Deletes a Transaction


A financial posting, once written, can't be changed or deleted. The right way to amend a wrong posting is a reversal: a new entry that cancels out the original. That keeps history from being rewritten, but it doesn't prevent the same writing from being recorded twice, leading to[integrity issues during audits](https://www.formance.com/blog/financial-operations/ensuring-ledger-integrity) .


### 2. Idempotency to Prevent Double-Posting on Retries


Safe retries stop a request from being processed twice when a client retries a failed call. Consider a payment that goes through, but the confirmation gets lost on the way back. The client retries, and without safe retries, both charges run.


For the client to generate a stable key for each business event and send it with every request, the server must check whether it has already processed that key and return the saved result if so.


A common mistake is generating a fresh UUID on each retry, which defeats the whole thing. Safe retries handle duplicates from a single client, but writes occurring simultaneously from different clients still need to be coordinated.


### 3. Consistency so Balances Always Add Up


When two requests read the same balance, change it separately, and write it back without coordinating, one update overwrites the other. One common approach is optimistic locking: a version number that goes up every time the row changes. The last failure mode lives between the two sides of a single transaction.


### 4. Atomicity so Debits and Credits Post Together


A partial transaction occurs when a debit is recorded, but the matching credit never reaches its destination, usually because the two legs were processed by different services.


The requirement is that the smallest unit the ledger writes is a transaction with at least two postings that balance, and there's no API for writing just one side.


## The Negative Impact of a Broken Ledger


A broken ledger costs you in three escalating ways: silent balance drift, reconciliation debt that eats engineering time, and homegrown systems that fail outright under production load. The CFPB action against Synapse is the public case study for what happens when those problems pile up.


### Balance Drift


Balance drift happens when a ledger write succeeds, but a matching downstream update fails. For example, when a cache update fails, when a distributed saga partially completes, or when an outbox pattern fails to publish an event. The cached balance and the real ledger value disagree.


One contributing architectural pattern is to store a running balance as a column in a user account table, without an append-only log as the real source of truth; however, drift can occur in distributed systems even with proper append-only ledgers if downstream propagation fails. Drift between systems eventually shows up somewhere, and that somewhere is the reconciliation queue.


### Reconciliation Debt


Reconciliation debt builds up when mismatches require a human to investigate them rather than being matched automatically. As a ledger collects design debt, more mismatches need a human, and each one pulls engineers away from product work.


### Homegrown Ledgers Break At Scale


Homegrown ledgers fail in three predictable ways under production load:


1. **Concurrency failures:** A client times out, retries, and the ledger processes both the original and the retry. Money moves twice, and that's the default behavior of any system without retry keys.
2. **Hot account bottlenecks:** A common settlement or float account appears on nearly every transaction, creating a write bottleneck that serializes what should be parallel work.
3. **Bi-temporality gaps:** Late-arriving transactions, such as ACH returns, are recorded at processing time and corrupt the historical balance at event time, making it impossible to answer the question "What was the balance on date X?"


The real-world consequences are documented. The[CFPB filed an action](https://www.consumerfinance.gov/enforcement/actions/synapse-financial-technologies-inc/) against Synapse Financial Technologies after its internal records of individual customer balances were found to exceed the actual funds in the pooled accounts supporting them.


With no source of truth to figure out who owed what, Synapse filed for bankruptcy, and customers lost access to their money.


## Treat Your Ledger as Infrastructure


Do not consider your ledger as a feature inside the application. Treat your ledger as its own piece of infrastructure, sitting between your payment rails and your general ledger. The issue is that the application code says what a transaction is supposed to do, while the ledger handles writing it atomically, calculating balances, and keeping the audit history.


[Formance Ledger](https://www.formance.com/modules/ledger) is one way to build this solid infrastructure foundation. It's open-source and MIT-licensed, with the four rules built in as native primitives. Transactions are written in[Numscript](https://www.formance.com/blog/engineering/numscript) — Formance's language for moving money, readable by both engineers and finance — so instead of juggling debits, credits, and balance checks in application code (where race conditions and partial writes live), you describe what the transaction should do, and the ledger writes it all in one go and won't let you change it after.


Teams that build a ledger from scratch end up with correctness bugs that are hard to spot and expensive to amend. The build-versus-buy question is whether your team can build a ledger while maintaining one, or if it is the best use of your engineering time.


If you want to go deeper, explore the Formance Ledger open-source[repo on GitHub](https://github.com/formancehq/ledger) to read the code, run it locally, and see how the four invariants are enforced as native primitives.
