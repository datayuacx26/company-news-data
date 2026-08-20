---
schema_version: "1.0.0"
document_id: "de43dd82c25b991b8bbc80d0fef2fd70d4797c8101960f02385aad825d4c5358"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/core-banking-system-architecture"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-14T15:05:55.153996+00:00"
fetched_at: "2026-08-14T15:05:57.282861+00:00"
content_hash: "sha256:a958057c5a47d9696779ddfda5763dddb92014f106467ea3428972c088d4ffdf"
---

# Core Banking System Architecture: A Reference Model For Modern Stacks

Most banking platforms rely on multiple systems to track a single payment, such as a payment provider, a bank, and internal tools, and these systems often disagree on whether the payment is pending, finalized, or settled.


Core banking system architecture is how you resolve those disagreements. It determines which system holds the truth, and whether your platform can add new markets and products without drowning in reconciliation errors.


When statuses disagree across systems, the root cause is usually architectural rather than a bug in the ledger itself. The most common mistake is mixing the ledger with payment processing or compliance logic in the same layer, which means every new market, product, or payment rail forces another painful rebuild.


This article covers how core banking system architecture is vital to modern stacks, focusing on the four architectural layers, the trade-offs among monolithic, SOA, and event-driven microservices, and a nine-step guide to building the architecture.


### The four functional layers of a modern core banking architecture


Four layers make up a modern core banking architecture: the core ledger, the money movement infrastructure, the compliance and risk layer, and the integration and API surface. Each layer owns its own state and communicates with adjacent layers through defined contracts.


#### Layer 1: Core ledger, the double-entry system of record


The core ledger is the double-entry system of record. Every other layer derives its view of financial state from it.


Four invariants must be enforced at the storage layer:


•


**Sum-to-zero:**[Every transaction](https://www.formance.com/blog/engineering/double-entry-accounting-for-engineers-building-financial-products) balances debits and credits, so no money is created or lost.


•


**Postings as source of truth:** Balances are derived from postings and are never stored separately, so state can't drift.


•


**Append-only with reversal postings:** You never edit or delete a posting. All corrections are posted as offsetting entries, preserving a full audit trail.


•


**Idempotent writes:** Every write is deduplicated explicitly, because a duplicate posting can still balance internally and slip past double-entry checks.


Product logic such as interest accrual, fee splits, and reserve allocations should be encoded directly as[Numscript](https://www.formance.com/blog/engineering/numscript) transactions rather than in application code. A fee split in the application code executes as multiple writes, which can partially fail. However, the same split as a Numscript transaction either commits in full or is rejected, and the posting documents the logic.


#### Layer 2: Money movement infrastructure for normalizing payment rails


The money movement layer normalizes inbound and outbound events from PSPs, bank APIs, and digital asset rails into a single data model the ledger consumes without rail-specific branching.


Without it, teams maintain a separate adapter per provider, and those[adapters are where reconciliation](https://www.formance.com/blog/financial-operations/account-reconciliation-patterns-for-high-volume-fintech) bugs hide. When a provider changes a file format or status code, the adapter quietly mismaps a field, and the issue only surfaces when balances no longer match.


This layer also absorbs the weak delivery guarantees providers offer. Webhooks can overload endpoints and trigger retries; without idempotent processing, duplicate or out-of-order handling follows. Deduplication, ordering, and normalization must occur here before the ledger sees anything, which is what[Formance Connectivity](https://www.formance.com/platform/connectivity) encapsulates through pre-built connectors.


#### Layer 3: Compliance and risk layer for bi-temporal audit and regulatory reporting


The compliance and risk layer produces the immutable, point-in-time evidence regulators and auditors require, drawing directly from ledger postings rather than reconstructing state from application logs. It relies on immutable hash-linked records, point-in-time balance queries, separate records and the two-timestamp model known as bi-temporality.


An ACH return for a $5,000 credit posted March 3 arrives on March 8. You record it on March 8 with an effective date of March 3, and when an examiner asks, "What was the balance on March 5?" the[ledger database answers correctly:](https://www.formance.com/blog/product/what-is-a-ledger-database) $5,000 lower than the naive view, without rewriting the original posting.


Regulations codify this requirement:


•


[DORA's logging RTS](https://ec.europa.eu/finance/docs/level-2-measures/dora-regulation-rts--2024-1532_en.pdf) requires documented logging with defined retention periods


•


[MiCA Article 70](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32023R1114) requires a register of client positions, with every movement evidenced by a registered transaction


•


The FCA's[CASS 15](https://handbook.fca.org.uk/handbook/cass15) rules require internal safeguarding reconciliation at least once each reconciliation day.


Reconstructing point-in-time balances from application logs during an examination is exactly the failure mode this layer exists to prevent.


#### Layer 4: Integration and API surface for translating events into postings


The API surface translates external events into ledger postings while the financial state and replicated balances remain outside this layer.


When the API takes on ledger responsibilities, a PSP webhook hits your endpoint, the handler posts the transaction, and the process dies before the acknowledgment reaches the provider. The processor retries, and the payment posts twice unless the ledger boundary enforces idempotency.


Balance replication fails the same way in slow motion. Any balance the API layer caches and mutates independently will eventually disagree with the ledger. Balances belong in the ledger, and the API surface only reads them.


### Architecture patterns and what monolithic, service-oriented architecture, and microservices each lock in


Each architectural pattern in a core banking system serves a different operational constraint, and each choice locks in a failure domain, a scaling model, and a consistency model.


**Pattern** **Failure domain** **Scaling model** **Consistency guarantee**


**Monolithic** Single unit; one component's failure can propagate across the whole system All-or-nothing; the whole deployment scales together Strong ACID; every posting inside one database transaction


**SOA (shared database)** Domain services isolated, but the shared database and enterprise service bus are shared failure points Services scale independently, but the shared database throttles them Strong ACID at the database, degraded by contention on shared resources


**Event-driven microservices** Isolated per service; the ledger is the canonical event log Each service scales independently against its own datastore Eventual consistency via distributed sagas; each consumer needs idempotency


#### Monolithic core banking with a single failure domain and simple consistency


A monolithic core puts the ledger, payment engine, product logic, and compliance reporting in a single deployment unit and a single failure domain so that a component failure can spread across the financial system.


Scaling is all-or-nothing, so a payday spike in payment volume means upgrading capacity for idle modules too. Change velocity suffers most because even simple updates to a tightly coupled core can require long development and release cycles.


A monolith keeps transactional consistency simple because every posting lives within a single database transaction.


#### Service-oriented architecture with a shared database as bottleneck


Service-oriented architecture (SOA) decomposed the monolith into domain services, but many implementations retained a shared central database, which became both a throughput bottleneck and a shared point of corruption.


Services compete for the same resources, and a single poorly optimized query can degrade every service on the shared database, and the enterprise service bus adds its own failure mode. If the central bus fails, every service behind it can be impacted.


#### Event-driven microservices with sagas, idempotency, and consumer discipline


Event-driven microservices resolve the shared-state problem by making the core ledger the canonical event log. Therefore, all downstream services subscribe to postings and replay them, and direct shared-database queries are removed from the architecture.


Multi-leg transactions need distributed sagas, which trade strict ACID guarantees for availability. IBM describes this as a way to handle[distributed transaction management](https://developer.ibm.com/articles/use-saga-to-solve-distributed-transaction-management-problems-in-a-microservices-architecture) when a single ACID transaction is not available. A client can observe an intermediate state that a compensating transaction later reverses, and compensations themselves might not succeed.


Exactly-once delivery is narrower than the marketing suggests: once a consumer performs non-idempotent work outside the broker, the risk of duplication returns. Every consumer therefore needs its own inbox-style deduplication.


Any service that consumes events out of order or with duplicates the ledger did not detect opens a gap that eventually shows up in the reconciliation queue. The ongoing bill for event-driven microservices is operational discipline, including dashboards, incident response, replay tooling, and explicit ownership for every consumer.


### How to build a core banking system architecture


Build the ledger first, enforce its invariants at the storage layer, then add the account hierarchy, rails, reconciliation, and compliance reporting in that order. The build sequence has nine steps, and every step depends on the ones before it.


#### Step 1: Start with the double-entry ledger


The ledger is the source of financial truth, while the core banking system is the environment where that truth is created and consumed. Treating the two as the same layer is one of the more common architectural mistakes in fintech.


Building a correct double-entry ledger from scratch can take many months of engineering effort; budget accordingly, or don't build one.[Formance Ledger](https://www.formance.com/platform/ledger) can be deployed as a sidecar ledger alongside existing core banking systems, which is how most enterprise customers adopt it.


#### Step 2: Enforce invariants in the storage layer and not the application code


Enforce four invariants at the storage layer: a uniqueness constraint on a client-supplied idempotency key, a sum-to-zero check at write time, append-only postings, and integer amounts in minor units.


Application-layer balance checks leave race windows: concurrent withdrawal requests can both read the same starting balance before either write is committed. Separating transaction time from commit time is another storage-layer property worth enforcing here, because without it, reconstructing a balance as of a past date has no reliable reference point.


#### Step 3: Control who can write


Control write access with fine-grained role-based access control (RBAC) scoped to account paths and transaction types, with audit logs on every permission check.


Storage-layer invariants govern what a write must satisfy. Access control governs who is allowed to attempt one, and both belong at the storage layer for the same reason: an application-layer permission check is a race condition and a bypass risk.


#### Step 4: Design the account hierarchy


Hierarchical paths like @users:carlos:providers:providerA:custody let you query at any level or drill into a single provider leg. Stored value needs separate ledger, available, and pending balance views because reading one while acting on another constitutes balance conflation, a common source of production bugs.


That hierarchy is logical, and regulated deployments typically also need physical data isolation between tenants or entities.


For semi-fungible cases such as separating restricted from unrestricted balances of the same currency,[Formance Ledger's Assets Coloring](https://docs.formance.com/v3.2/modules/numscript/reference/asset-colors?deployment=cloud&license=ee) tags units with provenance or ownership constraints without creating a new asset type.


#### Step 5: Encode product logic as ledger transactions


Encode product logic (fee splits, interest accrual, and reserve allocations) directly as ledger transactions so each execution commits atomically and documents its own logic.


In Numscript, a $482,000 PSP settlement lands in a pending treasury account, then splits between the merchant, fee revenue, and reserves:


```text
// PSP_SETTLEMENT_SPLIT
// Event: settle $482,000 from a PSP; split merchant proceeds, fee revenue, and reserves
send   [USD/2 48200000]   (
source        =   @counterparties:paymentServiceProviders:providerA   allowing   unbounded   overdraft
destination   =   @platform:treasury:pending
)


send   [USD/2 48200000]   (
source        =   @platform:treasury:pending
destination   = {
97  %   to   @merchants:8451:available
remaining   to   {
80  %   to   @platform:revenue:fees
remaining   to   @platform:reserves:available
}
}
)
set_tx_meta  (  "event_type"  ,   "psp_settlement_split"  )
set_tx_meta  (  "settlement_id"  ,   "set001"  )


```


The script moves $467,540 to the merchant's available balance and allocates the remaining $14,460 as $11,568 in fee revenue and $2,892 in reserves.


Every posting commits as a single atomic transaction, and if any leg fails, none are posted. The PSP boundary account path preserves which rail the money came from for later reconciliation.


#### Step 6: Put payment rails behind normalization adapters


Isolate each payment rail behind a normalization adapter so that provider changes are contained within the adapter and never reach the product code. When a rail's behavior is buried in your checkout flow, every provider change becomes a product change. When it lives in an adapter behind a normalization layer, adding a provider changes only the adapter. Every incoming payment, settlement, fee, and conversion across every rail posts to the ledger.


#### Step 7: Emit events for downstream systems


Emit an event on every committed posting so downstream systems (accounting tools, notification services, and internal dashboards) stay current without polling the ledger.


A committed posting should notify the systems that depend on it. It shouldn't just sit in the ledger waiting to be queried. Event emission is what lets reconciliation, reporting, and product surfaces track the ledger in near real time.


#### Step 8: Build reconciliation as a separate concern


Design reconciliation as a parallel, stateless worker pool that reads from an event queue and writes match results to separate Findings and Audit tables. Changed matching logic can then regenerate results by replaying the stream.


Under an omnibus account structure, reconciliation is a three-party problem: your sub-ledger reconciles to the transaction source data, and the source data reconciles against the bank's core.


#### Step 9: Derive compliance reporting from the ledger


Regulator reports should pull from the same ledger that feeds customer statements, and both should reconcile against actual rail settlements. Keep compliance modules isolated so that a regulation change updates one service, and never just the ledger.


The same feed also supplies the corporate general ledger at month-end close. In this case, the core ledger tracks money in motion in real time while the general ledger closes the books, and neither should require a separate reconciliation to agree with the other.


### Building a core banking system architecture that holds


A modern core banking system architecture comes down to four layers held together by a single decision: where the ledger boundary lies.


Put the double-entry ledger at the center as the source of financial truth, keep money movement, compliance, and the API surface as separate layers around it, and enforce the invariants at the storage layer.


At Formance, we've built the open-source programmable ledger, connectors, and reconciliation module that occupy these four layers, so teams write product logic in Numscript instead of rebuilding posting and reconciliation infrastructure from scratch.
