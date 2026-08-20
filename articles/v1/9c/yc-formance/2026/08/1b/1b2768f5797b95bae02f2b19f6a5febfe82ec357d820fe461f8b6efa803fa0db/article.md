---
schema_version: "1.0.0"
document_id: "1b2768f5797b95bae02f2b19f6a5febfe82ec357d820fe461f8b6efa803fa0db"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/what-is-payment-orchestration"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T14:15:27.660602+00:00"
fetched_at: "2026-08-03T16:10:47.640296+00:00"
content_hash: "sha256:b84e3f42fcdc8ba1b16d0ce99c08357ffdde71394c630568502e7bc56948798e"
---

# What is Payment Orchestration? A Technical Primer

Visa Europe showed what single-rail dependency costs on June 1, 2018, when a component within a switch in[Visa's primary UK data center](https://www.americanbanker.com/payments/news/visa-blames-european-outage-on-systems-it-had-been-phasing-out) suffered a partial failure, preventing the backup switch from activating. The malfunctioning switch created a message backlog that slowed processing at both data centers.


The outage lasted roughly 10 hours, during which[5.2 million transactions failed](https://www.theguardian.com/money/2018/jun/19/visa-admits-5m-payments-failed-over-a-broken-switch) across Europe, including 2.4 million in the UK, leaving merchants on the Visa network with no automatic fallback path. That single point of failure is what payment orchestration is designed to eliminate.


### What is payment orchestration?


Payment orchestration is the layer that, per transaction, decides which payment service provider (PSP) receives the request and what happens when that PSP says no. It turns processor choice from hard-coded application logic into an operational layer that can be inspected, modified, and linked to reconciliation records.


Architecturally, payment orchestration depends on four components:


1


**Routing logic** that selects a PSP per transaction based on configurable rules.


2


**PSP connectors** that normalize each provider's API into a single internal data model.


3


**Reconciliation records** that match every orchestrated transaction to its settlement.


4


**A**[double-entry system of record](https://www.formance.com/blog/engineering/defining-double-entry) underneath that makes every routing decision auditable.


Orchestration controls where a payment goes, but cannot record what happened afterward. Without the fourth component, the routing decision is unreconcilable. A $15,000 charge succeeded somewhere, but the orchestrator cannot tell you which PSP's clearing account holds the funds, or whether the first failed attempt left a residue.


### How payment orchestration operates in four steps


Four steps carry the engineering load of payment orchestration: routing, connector normalization, retry safety, and event-driven flow control.


#### 1. PSP selection and smart routing


The routing engine ranks PSPs per transaction inside the payment request path, weighing fixed attributes (BIN, currency, amount, geography, and MCC) against live signals (success rates, fees, fraud flags, PSP health). Cost and acceptance ranking both work at BIN granularity.


The biggest gains appear in cross-border routing, where a local acquirer cuts cross-border fees and lifts domestic authorization rates because issuers treat cross-border traffic as higher risk. Most teams start with static rules and add ML models optimizing for approval rate once volume justifies it.


#### 2. Unified connector and API normalization


PSP integrations differ in SDKs, API shapes, and HTTP status code handling. A connector framework hides that heterogeneity behind a single internal data model, handling request transformation, response translation, per-provider retry semantics, and circuit breakers.


Normalization also covers settlement data; one provider's transaction_id is another's pspReference, and fees may arrive as a single processing_fees or split into interchange_fee, scheme_fee and markup. Mapping everything to a single taxonomy is why later PSP integrations become configuration rather than new codebases.


#### 3. Retry logic, fallback chains, and idempotency


Retry correctness is hard because failure is ambiguous. A decline is definitive, but a timeout leaves the outcome unknown, and retrying without an idempotency key that the PSP honors turns a network blip into a double charge.


Idempotency guarantees vary by provider (key scope, retention, length limits, and regional behavior), and multi-server deployments without a shared duplicate-check store further weaken the guarantee.


Fallback chains define ordered PSPs per region with health-based routing and circuit breakers, because sustained latency spikes and retry storms cascade through dependent services. Every attempt should be traceable as its own object (PSP name, reference, state, normalized error, and timestamps), giving retries a reconcilable history instead of a collapsed status field.


#### 4. Payment flow automation and event-driven triggers


Orchestration platforms turn PSP lifecycle transitions into triggers, consuming webhook events through an asynchronous queue so handlers return quickly. Flow builders move coordination out of application code into declarative configuration, chaining steps such as hold funds, await KYC, capture at end-of-day, trigger a payout and write the posting.


For multi-step flows across services, the saga pattern is well-suited, as each step publishes an event that triggers the next, with compensating transactions rolling back on failure. When the sequence changes, you edit a flow definition instead of redeploying three services.


### Four layers of a production payment orchestration stack


A production orchestration stack separates four responsibilities to stay auditable: Connectivity, Routing, Reconciliation, and Ledger.


We call this the **CRRL stack** . The top two layers move money while the bottom two prove where it went.


#### 1. Connectivity adapters that normalize PSP APIs


[Connectivity adapters](https://www.formance.com/connectors) translate each PSP's proprietary request and response schema into a single internal data model. The adapter normalizes PSP-specific account, payment, and settlement data into a common internal model before writing ledger postings, so product code never sees the raw rail format. Adding a rail changes only the adapter; the checkout flow stays untouched.


Adapters are also where reconciliation bugs hide. When a PSP changes a file format or status code, the adapter[quietly mismaps a field](https://www.formance.com/blog/financial-operations/account-reconciliation-patterns-for-high-volume-fintech) , and the mismap only surfaces when balances stop matching. Isolating each PSP behind its own adapter keeps schema changes localized.


#### 2. Routing and logic engine for waterfall and retry rules


The routing and logic layer houses the rules engine, where waterfall sequences, geographic split ratios, A/B PSP tests, and retry policies reside. Version control safeguards production.


For auditability, teams should store a reference to the rule version used for every transaction, along with the decline codes, the PSP and merchant account that handled the attempt, and the legal entity. Without version-controlled rule references, you cannot explain last month's routing decisions to an auditor.


#### 3. Reconciliation across PSPs and bank settlements


The reconciliation layer ties each orchestrated transaction to its settlement record from each PSP and detects drift when PSP settlement reports diverge from internal records.


The reconciliation topology is often asymmetric: incoming payments reconcile 1:1 on the first leg (order to PSP, by transaction reference), then N:1 on the second leg, where many PSP transactions map to one settlement credit at the bank.


#### 4. Core ledger as the double-entry system of record


The[core ledger](https://www.formance.com/blog/financial-operations/what-is-a-ledger) is the double-entry system of record that makes routed transactions auditable through retries and settlement, and represents reversals as new balancing entries.


[Numscript, Formance's language for financial transactions](https://www.formance.com/blog/engineering/numscript) , expresses a gross-to-net settlement as one atomic multi-posting. Take this example of a $10,000.00 card settlement netting $9,700.00 after $120.00 markup, $50.00 scheme fees, and $130.00 interchange:


```text
// CARD_SETTLEMENT
// Event: record a $10,000.00 card settlement net of markup, scheme, and interchange fees
send   [USD/2 1000000]   (
source   =   @counterparties:acquirers:a   allowing   unbounded   overdraft
destination   = {
970000/1000000   to   @merchants:acme:available
12000/1000000   to   @platform:expenses:fees:markup
5000/1000000   to   @platform:expenses:fees:scheme
13000/1000000   to   @platform:expenses:fees:interchange
}
)
set_tx_meta  (  "event_type"  ,   "card_settlement"  )
set_tx_meta  (  "settlement_id"  ,   "set001"  )


```


All four postings are committed together or not at all, so the fee is preserved per transaction rather than buried in a PSP invoice.


The other three layers depend on the core ledger as routing writes intent, connectivity writes PSP events, and reconciliation compares against external reality.


### Why single-PSP payment stacks break at scale


A single-PSP payment stack breaks for structural reasons: vendor concentration, integration sprawl per rail added, and routing decisions scattered across application code.


#### Vendor availability becomes your revenue ceiling


If the PSP goes down, rate-limits traffic, or declines a geography-specific segment, the stack has no alternative route and limited visibility into the cause. Every outage window, regional decline pattern, and unannounced rate limit reaches checkout unfiltered. There is no fallback path and no operational lever to pull while the incident is live.


#### Every added rail brings its own schema, webhooks, and settlement format


Each added PSP or payment method, including buy now, pay later (BNPL) providers and[local payment methods](https://www.formance.com/blog/financial-operations/payment-rails-explained-for-fintech-engineers) , brings its own request schema, webhook semantics, and settlement report format. Each addition creates a separate data model and reconciliation process, and integration work grows disproportionately as your team maintains a bespoke translation layer for each provider.


#### Routing decisions scatter across application code with no audit trail


Without a central routing layer, "which PSP handles this transaction" is determined by application code, spread across checkout logic, retry handlers, and per-region branches. There is no central place where the rule is defined or audited, no way to change it without a deploy, and no reliable answer when an auditor asks why a specific transaction went to a specific processor last month.


### What new failure modes does orchestration introduce


Adding orchestration resolves single-PSP failures but introduces new ones: partial-capture risk after failover, reconciliation drift at high transaction volumes, and broken network token portability.


#### Partial-capture risk after failover


Partial-capture risk arises when an authorization succeeds at PSP A, the connection drops before the response reaches PSP A, and the capture routes to PSP B. A ghost authorization now sits open at PSP A: the customer's issuer holds funds against the authorization, the customer can be charged twice, and the reconciliation record never closes cleanly.


Partial-capture exposure widens across geography. If a retry routes to a different data center during a regional failover, the idempotency state may need to be visible across processing locations to reduce the risk of duplicate charges. Failover logic should also avoid assuming uniform capture semantics across processors and instruments.


#### Reconciliation drift compounds at high transaction volume


Small per-transaction discrepancies across PSPs compound as volume rises. A 0.1% per-transaction discrepancy across three acquirers becomes a five-figure month-end exception queue; each unresolved line blocks the settlement close.


Timing differences across acquirers and later refund or chargeback events make those exceptions harder to trace. Reconcile at the transaction level rather than in daily batches. Batch reconciliation surfaces the drift only after it has compounded.


#### Network token portability across PSPs


Network token portability breaks when teams using processor vaults try to move volume across PSPs. Re-tokenizing cardholder data creates migration work and may require customers to re-enter payment details.


Network-level tokens can reduce processor-vault lock-in when the relevant issuer, network, acquirer, and processor support the token flow. However, support and performance still vary by issuer, processor, and transaction context.


### Five signals your architecture needs payment orchestration


Five signals confirm that you need payment orchestration within your financial architecture: manual multi-PSP routing, geographic authorization variance, single-PSP outage exposure, cross-border expansion, and inconsistent retry logic.


#### 1. You route across three or more PSPs manually in the application code


Routing rules embedded in if statements require a code deployment for every change, and the operational drag compounds as the number of providers and payment methods grows. If your PSP count has moved beyond a simple primary-and-backup setup, hand-rolled routing is already the bottleneck.


#### 2. Your authorization rate varies by geography or card network, and nothing exploits that variance


A blended authorization rate can hide strong domestic debit performance and weak international credit performance. Local acquiring can help narrow the gap in some cases. Without signal-based routing, that variance is a report you read.


#### 3. A single PSP outage takes your payment stack down


Partial outages can affect a specific region, rail, or payment method rather than the entire provider. You cannot integrate your way out mid-incident; onboarding a new acquirer directly can take many months.


#### 4. You are expanding into markets where no single PSP has strong local acquiring coverage


Cross-border expansion introduces differences in authentication, acquisition, currency, and compliance across markets. Multi-market coverage means multi-acquirer by construction, and multi-acquirer without orchestration means one bespoke integration per market.


#### 5. Your retry logic is bespoke, inconsistently idempotent, and owned by whoever wrote the original integration


Each direct PSP integration carries its own retry implementation with its own assumptions about key retention, scopes, and timeout semantics. Centralizing the taxonomy can reduce duplicated engineering work, as multi-PSP fallbacks may recover transactions that would otherwise fail.


### Adopting a programmable core ledger beneath the payment orchestrator


A programmable core ledger beneath the orchestrator records every fund movement the orchestrator triggers and makes those movements auditable across PSPs.


An orchestration platform routes payment instructions: a[programmable core ledger](https://www.formance.com/modules/ledger) records the resulting fund movements, while a reconciliation layer compares them with external statements across connected rails. If you collapse the orchestrator and the ledger into one layer, external PSP APIs become your source of financial truth by default, which is exactly the failure mode a separate ledger prevents.
