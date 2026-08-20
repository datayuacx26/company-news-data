---
schema_version: "1.0.0"
document_id: "8d6c16df93c0081f83798526e7c534016714563bebc3f6cbef0721708b19606d"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/marketplaces/formance-vs-tigerbeetle"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T23:38:15.740107+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:aacba428ad52fa50cfbb7c8f049e8dd6bf3a1d1b58ef10152400b8c7731760aa"
---

# Formance vs TigerBeetle

Formance and TigerBeetle get benchmarked against each other in payments infrastructure, but the two platforms sit at different layers of the stack.


Formance is an open-source, programmable core ledger that unifies fiat and digital assets with regulatory-grade traceability, covering ledgering, orchestration, connectivity, and reconciliation above the storage layer. TigerBeetle is a specialized transaction database and not an operational ledger platform; it uses database software built for the hot path of financial transaction processing.


Picking between them is a category error until you know which layer you are actually buying.


## Formance vs TigerBeetle: side-by-side comparison


Formance and TigerBeetle differ across ten properties, including abstraction layer, multi-asset support, built-in reconciliation, payment rail connectivity, and regulatory-grade traceability.


**Property** **Formance** **TigerBeetle**


Abstraction layer Operational core ledger platform with an open-source Ledger and Connectivity; Enterprise Flows and Reconciliation Storage and consensus tier; low-level transaction database, paired with a general-purpose control-plane database


Account identifiers Readable multi-segment strings, e.g. @wallet:polygon:0xabc Numeric 128-bit identifiers for accounts and transfers


Schema model Multi-segment namespace, arbitrary first-class metadata, Numscript-defined postings Specialized transaction database; three numeric metadata fields per record, so text lives outside the hot path


Multi-asset support Native; all assets in one ledger, customer-defined Cross-asset movement handled outside the core; a separate ledger per asset


Metadata queryability First-class queryable, server-side Requires client-side join against the sidecar


Built-in reconciliation Enterprise Reconciliation: policy-based matching, drift detection, balance verification Application-layer responsibility


Payment rail connectivity Enterprise pre-built connectors plus a generic connector framework Application-layer responsibility


Fund flow orchestration Enterprise Flows with retries and fallbacks Application-layer responsibility


Regulatory-grade traceability Immutable hash-chained log, bi-temporality; SOC 2 Type II, ISO 27001; supports DORA, GENIUS, MiCA Immutable, checksummed, hash-chained data; ships no certifications out of the box


Open-source posture MIT-licensed Core Ledger and Connectivity; Enterprise products (Flows, Reconciliation, Wallets, Web Console, pre-built connectors) are subscription-based Open source


Deployment options Open Source / Community Edition; Enterprise Self-Hosted; Enterprise Private Cloud Self-operated or commercial support; specialized operations planning required


## Formance is a double-entry ledger with Numscript and orchestration


Formance Ledger is an operational core ledger built on four properties: double-entry enforcement at the storage layer, Numscript as a purpose-built transaction language, native multi-asset accounts with bi-temporality, and four modules (Connectivity, Flows, Reconciliation, and Wallets) layered on top of the ledger.


### Double-entry enforcement at the storage layer


Double-entry records every transaction as equal and opposite postings across two accounts, which is what makes ledger balances auditable and lets discrepancies be traced to a specific entry


Financial correctness is enforced at the storage and data model layer, beneath the application logic, so no service-layer code can bypass[double-entry](https://www.formance.com/blog/engineering/defining-double-entry) . Every transaction's postings sum to zero, and multiple send statements in one script commit as one atomic transaction.


### Numscript as a purpose-built transaction language


[Numscript](https://www.formance.com/blog/engineering/numscript) is Formance's purpose-built transaction language that describes transaction intent in terms readable by both engineers and finance teams. A send statement names an asset, a source account, and a destination account, while multiple sends in a single script commit atomically.


A USD-to-USDC conversion through an[omnibus account](https://www.formance.com/glossary/omnibus-account) (a single omnibus or FBO account holding funds on behalf of many underlying owners) shows the multi-asset case that TigerBeetle's transfer-oriented model cannot express natively without a custom application layer:


```text
// STABLECOIN_ONRAMP
// Event: Alice converts $1000.00 to 100 USDC through the platform omnibus via Circle
send   [USD/2 100000]   (
source        =   @users:alice:wallet
destination   =   @platform:treasury:omnibus
)
send   [USD/2 100000]   (
source        =   @platform:treasury:omnibus
destination   =   @counterparties:liquidityProviders:circle
)
send   [USDC/6 1000000000]   (
source        =   @counterparties:liquidityProviders:circle   allowing   unbounded   overdraft
destination   =   @users:alice:wallet
)
set_tx_meta  (  "event_type"  ,   "stablecoin_onramp"  )
set_tx_meta  (  "conversion_id"  ,   "cnv001"  )


```


The postings carry explicit asset identifiers, USD/2 and USDC/6, and execute as one atomic transaction. Alice's USD moves from @users:alice:wallet through the platform omnibus to @counterparties:liquidityProviders:circle, and the delivered USDC is sourced from that named liquidity-provider counterparty.


### Native multi-asset accounts and bi-temporality


Formance accounts are multi-asset by default, so the same account structure works across USD, EUR, stablecoins, or any asset the customer defines. Postings carry explicit asset identifiers, so a single ledger tracks fiat and digital assets without spawning a separate ledger per asset.


Formance Ledger also carries bi-temporality, which tracks both when an event was recorded and when it was effective. Separate recorded and effective timestamps keep historical balance queries accurate when transactions arrive out of order, and support point-in-time views and backdated corrections without rewriting history.


### The four modules of Connectivity, Flows, Reconciliation, and Wallets


Four modules can be added on top of Ledger: Connectivity, Flows, Reconciliation, and Wallets.


1. [Connectivity](https://www.formance.com/modules/connectivity) ingests and unifies data from external financial providers into a single data model and can initiate payments through connected providers.
2. [Flows](https://www.formance.com/modules/flows) orchestrates multi-step fund lifecycles (for example, holding funds until a payout event arrives) with retries and fallbacks defined declaratively.
3. [Reconciliation](https://www.formance.com/modules/reconciliation) runs policy-based matching of ledger balances against external statements, and provides drift detection and settlement-error alerts at configurable intervals.
4. [Wallets](https://docs.formance.com/modules/wallets?deployment=cloud&license=ee) supports multi-currency balances, holds, and expiring balances.


Together, the four modules mean the operational work most payments teams would otherwise build themselves (provider integration, workflow orchestration, matching, and balance management) sits inside the platform.


## TigerBeetle is the purpose-built OLTP for the storage layer


TigerBeetle is a specialized transaction database for financial transfers, with a scope intentionally narrow to the hot path of financial transaction processing and payment-system context kept outside that path.


Four properties define TigerBeetle's architecture: a purpose-built OLTP design optimized for double-entry transfers, deterministic simulation testing (DST), a mandatory sidecar database, and a narrow scope that leaves everything above storage to the application team.


### Purpose-built OLTP for double-entry transfers


TigerBeetle is built for high-volume, low-latency double-entry transfers between numeric accounts. Every design choice serves that workload. Accounts and transfers use fixed-size 128-bit numeric identifiers instead of variable-length strings. Records carry three numeric metadata fields and nothing else, so the hot path avoids variable-width text entirely. Transfers commit in batches against a custom storage engine tuned for sequential writes, and the operation set is limited to a small, fixed vocabulary of transfer types.


A general-purpose OLTP database has to accommodate arbitrary schemas, joins, secondary indexes, and ad hoc queries; TigerBeetle deliberately does none of that. In exchange, it targets 1M+ transfers per second on batched workloads and predictable tail latency at that ceiling. However, anything outside the transfer hot path (human-readable text, reconciliation state, or provider-specific metadata) has to live somewhere else.


### Deterministic simulation testing (DST) with the VOPR


TigerBeetle's durability results come from deterministic simulation testing (DST), a technique conventional integration tests cannot match. TigerBeetle's simulator, the Viewstamped Operation Replication Simulator (VOPR),[runs an entire cluster](https://jepsen.io/analyses/tigerbeetle-0.16.11) with mocked clock and mocked disk/network behavior and injects storage and network faults across simulated executions.


Deterministic simulated time lets the VOPR explore many more execution paths than ordinary integration tests. An integration test observes one execution of real time as the DST explores the state space. However, there is a technique limit: the VOPR's pre-registered query workload can create a blind spot that hides a bug.


### Why TigerBeetle requires a sidecar database


In a TigerBeetle-based payments system, textual records such as reconciliation status, counterparty names, leg descriptions, and external reference hashes have to live in a second or sidecar database beside TigerBeetle, because the engine does not carry them.


The team owns, backs up, and keeps consistent a second database alongside the cluster. Because the sidecar sits outside TigerBeetle's replicated cluster, the sidecar claws back much of the engine-level disaster-recovery advantage at the system level. The cluster protects the transfers, but the metadata that reconciliation depends on has whatever durability the sidecar has.


### What TigerBeetle's narrow scope excludes


TigerBeetle's excluded scope follows from the product boundary. Teams still need operational layers around the storage engine for multi-asset ledgering, plus reconciliation against bank or PSP statements and connectivity to payment service providers or custodians.


More complex movement patterns become application-design concerns. TigerBeetle has added query capabilities, but payment systems usually still need a separate control-plane database. Operationally, teams should also plan around a specialized database.


## Formance vs TigerBeetle comparison: 1 client and 49 events


A client ran an identical 49-event stream through Formance and TigerBeetle behind a swappable ledger port in a single payments system, changing only the ledger implementation. Every client-specific number in the sections that follow (line counts, timings, and operational-question outcomes) comes from that run.


Both engines came out correct and safe: balances reconciled, no partial writes, idempotency behaved as documented, and records were immutable on both sides.


Correctness was not the differentiator, but within the three layers around the core ledger: how much scaffolding the team had to build for a working integration, how well the data model fit a multi-asset payments business, and what it took to answer operational questions after the fact.


## Where Formance outperforms TigerBeetle: scaffolding, data model, and operational questions


Formance outperformed in all three layers: scaffolding (integration code and pre-built connector), data model fit (atomic multi-asset postings and bi-temporality), and operational questions (server-side reporting vs client-side sidecar joins).


### Scaffolding: 283 vs 473 lines of integration code and pre-built rail connectivity


The client's 49-event run put concrete numbers behind the scaffolding argument. The Formance adapter came in at 283 lines with no additional infrastructure to stand up. The TigerBeetle adapter came in at 473 lines, of which 113 existed only to build and maintain the mandatory sidecar database. The line-count gap traces directly to the data model.


With Formance, an account is a readable string such as @wallet:polygon:0xabc, multiple assets live in a single ledger, and metadata is a first-class queryable field on transactions and accounts.


TigerBeetle, by design, uses numeric 128-bit identifiers for accounts and transfers, requires a separate ledger per asset, and exposes only three numeric metadata fields per record, so any text the business actually needs (reconciliation status, leg names, counterparty descriptions, or external reference hashes) has to live outside the engine, in the second datastore the team owns and keeps consistent with the cluster.


Connectivity closes the rail-adapter gap. The pre-built[Formance connector catalog](https://www.formance.com/connectors) includes Stripe, Adyen, Wise, Fireblocks, Kraken, Banking Circle, Modulr, Mangopay, Tink, and Powens. The generic connector framework handles any provider without native support.


Customers build their own connector for unsupported providers as Formance provides the framework, and the customer implements the translation layer. On TigerBeetle, each of those integrations is a bespoke adapter plus its share of the sidecar schema.


### Data model: atomic multi-asset postings and bi-temporality in Formance Ledger


With Numscript, double-entry enforcement, bi-temporality, and atomic multi-posting across fiat and digital assets are properties of Formance Ledger rather than conventions the service layer must uphold.


The USD-to-USDC Numscript block above is the ledger-side atomic posting model for a two-asset conversion. Actual money movement still executes through connected providers.


### Operational questions: server-side reporting vs client-side sidecar joins


Formance answered the operational questions the client cared about (balances by asset, transaction history filtered by metadata, reconciliation status, and human-readable leg descriptions) server-side, in a single API call per question.


TigerBeetle natively answers only balances and transfer history against its numeric identifiers. Anything human-readable, filterable by text, or joined against reconciliation state required a client-side join between the engine and the sidecar. The client-side join follows directly from the "text lives outside the engine" data model. The application layer still inherits that work every time a new operational question comes up.


## Where TigerBeetle outperforms Formance: batched throughput and engine-level DR


TigerBeetle has two advantages over Formance: batched throughput and engine-level disaster recovery, with the caveat of a sidecar database.


### Batched throughput: 1M+ transfers per second (and the per-leg caveat)


On throughput, TigerBeetle targets 1M+ transfers per second on batched workloads with a custom storage engine, and predictable performance is a first-class design goal over a broad general-purpose database surface. Teams should still benchmark that ceiling against their own workload and batch sizes before treating it as their result.


In the 49-event run, Formance completed the stream in 1,099ms and TigerBeetle in 1,673ms. The result does not mean Formance is faster than TigerBeetle in general. The client's workload was per-leg rather than heavily batched, which is TigerBeetle's worst case. TigerBeetle's throughput advantage applies to extreme batched volume, and most teams should measure the ceiling against their actual workload and batch sizes rather than assume either result generalizes.


### Engine-level disaster recovery with three-replica consensus


On disaster recovery,[TigerBeetle's replication quorum](https://www.amplifypartners.com/blog-posts/why-tigerbeetle-is-the-most-interesting-database-in-the-world) can help a failed node self-heal from the cluster. With three-replica consensus, the engine-level DR model gives zero data loss on single-node failure by default, an edge over Formance self-hosted deployments, where replication has to be configured and does not guarantee zero data loss out of the box.


TigerBeetle's narrow operation set also helps prevent an entire class of accidental misuse. The committed-financial-transfer model limits what application code can do to committed financial records. The narrow operation set plus engine-level DR make TigerBeetle a defensible choice for a team that wants to compose its own financial stack from low-level primitives and has the staffing to build everything on top.


### Why the sidecar database weakens system-level disaster recovery


The sidecar database caveat still applies because the second datastore sits outside TigerBeetle's replicated cluster. Therefore, the system-level recovery story is weaker than the engine-level one, and any team relying on the guarantee has to plan the sidecar's durability with equal care. Some teams accept the sidecar burden to gain the engine-level guarantee.


## Formance vs TigerBeetle: how to choose between them


Choose TigerBeetle for a proprietary storage layer serving a narrow, extreme-volume transaction workload with the engineering staff to build the application layers above it. Choose Formance for multi-rail financial products that need regulatory-grade traceability.


### Choose TigerBeetle for high-volume proprietary transaction storage


Evaluate TigerBeetle if you are building a proprietary financial database layer for a narrow, high-volume transaction-processing workload measured against your actual ceiling. A TigerBeetle build also requires the engineering staff to build and maintain every application-layer primitive above storage, including the sidecar database, the batching layer, reconciliation pipelines, and a rail adapter per provider.


A narrow, extreme-volume workload with a strong infrastructure team can justify the tradeoff, so you should plan for specialized production operations and a specialized codebase the team will debug.


### Choose Formance for multi-rail fintech and regulated financial products


Evaluate Formance if you are building financial products as a fintech or platform company and need to track fiat and digital assets across multiple[payment rails](https://www.formance.com/blog/financial-operations/payment-rails-explained-for-fintech-engineers) and produce regulatory-grade traceability for auditors.


Formance fits teams that want to ship financial product features while offering traceability backed by SOC 2 Type II, ISO 27001, and support for[DORA compliance](https://www.formance.com/blog/financial-operations/dora-compliance) , GENIUS, and MiCA.


Formance does not hold or move funds (every payout executes through a connected PSP, bank, or custodian), and integration is API-first work for an engineering team.
