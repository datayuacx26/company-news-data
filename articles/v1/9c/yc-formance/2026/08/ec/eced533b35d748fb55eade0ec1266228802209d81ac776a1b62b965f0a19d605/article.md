---
schema_version: "1.0.0"
document_id: "eced533b35d748fb55eade0ec1266228802209d81ac776a1b62b965f0a19d605"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/immutable-ledgers-append-only-data-models"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T00:40:26.713435+00:00"
fetched_at: "2026-08-18T00:40:28.310152+00:00"
content_hash: "sha256:13461d67adb3b4a878d51d83362a53cf5ee826c7c91a14c62b6d59b10bacdae1"
---

# Immutable Ledgers Explained: How Append-Only Data Models Work

In February 2016, cyber criminals tried to make fraudulent transfers totaling $951 million using malware inside Bangladesh Bank's SWIFT terminal. By the time investigators reconstructed the sequence,[$81 million had left](https://www.reuters.com/article/world/exclusive-bangladesh-bank-hackers-compromised-swift-software-warning-issued-idUSKCN0XM0DW) the bank's account, and the local database that should have shown the truth had been edited to hide it.


This is the type of attack that an immutable ledger is built to prevent. An immutable ledger is a financial record where every posting is written once, ordered by sequence, cryptographically linked to the postings before it, and never altered or deleted after commit. Corrections can only happen as new postings are appended later in the sequence, so the full history remains reconstructable by anyone holding the raw log.


When payment rails, partner banks, and internal systems disagree, the only defense is a record that cannot be quietly rewritten. It is an ordered, replayable history of every posting, preserved for auditors, operators, finance, and compliance teams.


The case for immutability starts with what a mutable ledger cannot do, no matter how carefully it is instrumented.


### Why mutable ledgers cannot preserve financial history


A mutable ledger shows whatever the most privileged process last wrote, which may have nothing to do with what actually happened. Three issues highlight why mutable ledgers cannot preserve financial records: adversarial edits to the underlying database, routine in-place updates from application code, and soft-delete patterns that regulators explicitly reject.


#### Adversarial edits to the underlying database


The Bangladesh Bank attackers understood this precisely. Custom malware intercepted SWIFT confirmation messages and rewrote the local database so that the fraudulent transfers never appeared in balance reports or reconciliation queries.


Because the client-side database accepted in-place edits from a sufficiently privileged process, the malware could delete evidence of the fraudulent instructions, adjust balances to hide the missing $81 million, and suppress the printer output that would have alerted operators.


No breach of SWIFT's central network was required because a mutable ledger gives any attacker who reaches it the same power the application has, to overwrite history so the record and reality diverge with no artifact left behind.


#### In-place UPDATEs from application code


Malice is not required for the same failure. The most common case is a balance column updated in place by application code:


UPDATE accounts SET balance = balance + 100 WHERE id = 42;


The statement is atomic, cheap, and indistinguishable from any other write. It also destroys the sequence of postings that produced the new balance: the prior value is gone, no row records the increment, and nothing links the change to the business event that caused it.


If the balance later turns out to be wrong, from a duplicate credit or a race between two writers, there is no artifact to replay. The same failure hides behind ORM save-back patterns and admin-panel "edit balance" buttons that let support agents adjust totals directly.


#### Soft deletes and updated_at timestamps


Soft deletes and updated_at timestamps do not restore overwritten ledger history. A soft delete is an UPDATE that sets a flag, which means the same write path that flips the flag can silently alter any other field in the row. An updated_at column records only *when* the last write happened, not *what* it replaced. The prior value is not preserved, and successive writes overwrite the timestamp itself, so a row modified ten times looks identical to one modified once. Neither pattern gives an auditor a way to reconstruct what the record used to say.


Regulators have already ruled on the pattern. The U.S. Securities and Exchange Commission's (SEC's) amendments to Rule 17a-4, effective May 3, 2023, require broker-dealer records to be kept either in write once, read many (WORM) format or in a system that[permits recreation](https://www.sec.gov/rules-regulations/staff-guidance/trading-markets-frequently-asked-questions/rule-amendments-broker) of an original record if it is modified or deleted, alongside a complete time-stamped record of all modifications. A table that stores only final state plus a timestamp cannot recreate an original, so it fails that test by construction.


### How immutable ledgers preserve financial history


Immutable ledgers close each of the failures in a mutable ledger with three combined mechanisms:


1


Append-only writes refuse in-place edits, so no UPDATE path exists to overwrite history.


2


Hash chaining links every posting to the one before it, so any change to a historical record breaks the chain and surfaces on the next verification pass.


3


Four invariants that turn the tamper-evident log into a record an auditor can reconstruct without trusting the application that wrote it.


Each of those mechanisms rests on a specific structural property of the underlying store, starting with what "append-only" means at the data layer.


### What does "append-only" mean at the data layer?


Append-only storage accepts new records only at the tail of the log and offers no write path to overwrite, modify, or physically delete an existing record. It is an ordered, immutable sequence of records that is continually appended to, where each record's offset fixes both its content and its position. A record's position matters because a commit log captures facts, such as events that happened at a given point in time.


An append-only structure does not guarantee permanent immutability. Apache Kafka discards records after a[configurable retention period](https://kafka.apache.org/10/getting-started/introduction) , and PostgreSQL's multiversion concurrency control (MVCC) keeps old row versions after an UPDATE until VACUUM eventually[reclaims dead tuples](https://www.postgresql.org/docs/current/routine-vacuuming.html) , so old versions can still be removed. An append-only interface also depends on the permissions enforcing it; a process with sufficient operating system (OS) privileges can still truncate or overwrite the underlying storage. For stronger immutability guarantees, enforcement has to sit beyond the log structure itself.


Append-only and immutable differ. Append-only describes the structural property of the write path, while immutability describes the system-level guarantee that written data cannot be altered or deleted. The structure delivers the guarantee only when access control on the write path prevents every process, including privileged ones, from updating an existing record, or when cryptographic chaining makes any such update detectable.


### How hash chaining turns an append-only log into a tamper-evident record


Hash chaining makes tampering detectable by construction. Each new posting stores a cryptographic hash computed from its own data combined with the previous posting's hash. Modify any historical record and its hash changes, which invalidates the chain hash stored in every subsequent record.


An attacker who alters posting *n* must recompute every hash from *n* to the current head, and that recomputed head will not match any digest stored or distributed independently. Verification then depends on a digest that represents the ledger's history as of a point in time.


Where the chain is enforced determines who it protects against.[Formance Ledger](https://www.formance.com/modules/ledger) uses hash chaining to make transaction history tamper-evident. Database-level protections defend against application bugs. A machine administrator with direct storage access can still bypass database checks and tamper with underlying files, but detection hinges on where digests are stored.


**Mechanism** **The adversarial condition where it breaks**


Database ledger tables (append-only at the application programming interface (API) level) Ledger verification detects tampering by recomputing hashes and comparing them with database digests; to preserve that tamper evidence, digests should be stored in protected external storage with immutability controls.


Database ledger with external digest storage A privileged user can redirect or replace digest storage with unprotected storage, which can undermine tamper detection if verification does not inspect the digest locations.


WORM immutable tables without hash chaining Any privileged actor with direct storage access; access controls alone do not provide cryptographic tamper evidence.


Managed ledger journal The central trusted authority itself; the service operator sits outside the stated threat model.


Hash chain enforced at the ledger layer, keys outside the database Requires compromising both database storage and the application-held key material.


A database administrator (DBA) who edits storage files under a ledger-layer chain cannot make tampering pass integrity checks when verification is run against previously generated digests, so the tampering surfaces on the next verification pass.


Every database-layer alternative in the table collapses to a single question, what we can call the **digest-reachability test** : can the attacker reach the digests too?


### Four invariants immutable ledgers must enforce


A tamper-evident log becomes a trustworthy financial ledger only when four invariants hold at write time: gapless sequence ordering, zero-sum postings, non-destructive correction, and independent verifiability.


#### 1. Gapless sequence numbers


Every[ledger posting](https://www.formance.com/glossary/posting) must carry a sequence number that increases without gaps, because replaying the sequence then exposes every class of positional tampering. Replay detects missing records and positional tampering. Any issue exposes a missing record, and duplicate or lower-than-current sequence numbers expose insertion at an existing position or backdating.


Sequence allocators can fail here if numbers are allocated before commit. A rolled-back transaction can leave a permanent gap that only becomes visible under concurrency. Wall-clock timestamps cannot substitute for sequence numbers either.


#### 2. Zero-sum balance across postings


[Double-entry accounting](https://www.formance.com/blog/engineering/defining-double-entry) requires every posting to net to zero across all accounts, which turns tampering into an arithmetic alarm. Every movement of value records a source and a destination. An injected or altered record that lacks a perfectly matching counter-entry pushes the sum of all postings away from zero.


The deviation surfaces on any balance verification pass regardless of how the record was changed. Use layered checks because the application can assert before commit. In contrast, the database can use a CHECK constraint requiring each entry to be strictly a debit or a credit. An asynchronous audit job can scan for transactions whose lines fail to sum to zero.


#### 3. Non-destructive correction


Corrections must be appended as new compensating postings, ideally linked to the original posting, so both the error and its resolution remain immutable, ordered facts in the log. Event sourcing treats undoing a change as a new compensating event, while the original event remains unchanged.


In[Numscript](https://www.formance.com/blog/engineering/numscript) , a purpose-built language for describing financial transactions, a wrong $2,500.00 fee posted against customer 1234 looks like this:


```text
// Original (incorrect) fee posting
send   [USD/2 250000]   (
source   =   @customers:1234:wallet
destination   =   @platform:revenue:fees
)
set_tx_meta  (  "event_type"  ,   "fee_charge"  )
set_tx_meta  (  "fee_id"  ,   "fee1234"  )


```


The correction is a new posting appended later in the sequence, moving the same amount back and referencing the original by ID:


```text
// Compensating posting, appended with a reference to the original
send   [USD/2 250000]   (
source   =   @platform:revenue:fees
destination   =   @customers:1234:wallet
)
set_tx_meta  (  "event_type"  ,   "fee_reversal"  )
set_tx_meta  (  "reversal_id"  ,   "rev1234"  )
set_tx_meta  (  "adjusted_posting_event_id"  ,   "fee1234"  )


```


Both postings are immutable, sequenced entries. The net effect on balances is zero, and the history explains why: a charge happened, then a correction happened.


Bi-temporality, tracking both valid and transaction time, keeps the correction from distorting history. The compensating posting carries a recorded timestamp of today while its effective timestamp matches the original charge, so point-in-time queries before the correction return the original state and queries after return the corrected state.


#### 4. Independent verifiability


The log must be verifiable without trusting the application layer that wrote it. An auditor holding only the raw log and its hash chain can recompute every chain hash and replay the postings to confirm integrity, and can confirm balances if the log contains all information needed to derive them without additional application state.


Regulators are converging on this exact demand for reconstruction. The[Markets in Crypto-Assets Regulation (MiCA)](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica) requires crypto-asset service providers to arrange for records to be kept. Those records must cover all services, orders, and transactions sufficient to allow competent authorities to fulfill their supervisory tasks, and must be retained for five years.


The Financial Conduct Authority's (FCA's) Client Assets Sourcebook (CASS) 15 safeguarding rules are in force from 7 May 2026. They require[records to be maintained](https://api-handbook.fca.org.uk/files/instrument/Glossary-GEN-CASS-SUP/FCA%202025/38-2026-05-07.pdf) to demonstrate "their correspondence to the relevant funds held for clients." Each framework asks the same architectural question: can you prove history from the record itself?


### Benefits of an immutable ledger for financial infrastructure


Immutable ledgers give you a financial record you can prove from the raw log, without trusting the application that wrote it. Every posting is a permanent, hash-anchored fact, and provider confirmations, reversals, and reconciliations arrive as new appended entries rather than edits to settled history.


Formance Ledger delivers these properties natively: hash-chained transactions, immutable, tamper-evident entries once posted, programmatically enforced double-entry accounting, and bi-temporality that supports point-in-time views and backdated corrections without rewriting history.


The decision in front of you is whether to keep building bespoke verification infrastructure around a database that permits the exact writes your auditors prohibit, or to run a ledger that treats immutability as a structural guarantee.
