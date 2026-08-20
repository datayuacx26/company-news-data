---
schema_version: "1.0.0"
document_id: "28c68ced2a90b0a4303990e6497ee67a29d6c38fa66bcc8c0a96e4fef8254617"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/industry-analysis/an-engineering-buying-framework-for-a-payment-processor"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:0179bf023c0e1087c5fcb382ab17da70c857c1df10c80d15b6b74ec9fe0b9ad1"
---

# How to Evaluate a Payment Processor | Engineering Buying Framework

Your reconciliation job ran overnight. This morning, your settlement total from one PSP is off by $4,200 against your internal records. Nobody knows whether it's a timing difference, a fee category your normalization layer doesn't recognize, or a genuine drift. Three engineers will spend the day tracing it through application code, and the auditor's exam window opens in two weeks.


Engineering teams evaluating a payment processor are really choosing the failure model for money movement, reconciliation, and audit. The processor's architecture determines who explains the mismatch when records drift. Score the failure path above the demo and feature matrix.


Fees, transaction success rates, and supported card networks belong on every checklist. They affect margin and customer experience directly. Eighteen months from now, you will also be living with whether the processor's architecture forces correctness or leaves it to your application code. A processor you can build on differs from one that becomes your next migration project on five axes: idempotency, failover, reconciliation, ledger controls, and auditability.


A sixth axis sits underneath all of them: whether the processor leaves you exposed to *orphan-value failures* , where a crash mid-transaction debits the sender but never credits the receiver.


## The evaluation framework at a glance


Score every processor on the same axes. Weight the categories against your own risk profile, then sum the scores.


**Category** **What you're measuring** **Weight** **Score (1–5)**


Reliability & failover Contractual SLA tier, remediation clause, routing isolation 10%


Idempotency mechanics Key reuse, TTL window, retry safety, effectively-once delivery 15%


Ledger correctness Immutability, double-entry, atomicity (no orphan-value), idempotency 15%


Reconciliation Named component, N-to-M matching, normalization layer 15%


Operational checklist Rails, fees, PCI/fraud, developer experience 25%


Compliance fit PCI, FCA, DORA, MiCA, audit rights 10%


A downloadable version of the rubric and an RFP template sit at the end of the article.


## Reliability, idempotency, and failover


Start with the failure modes. They reveal the architecture faster than any feature list. Three sub-topics carry most of the weight: how the contractual reliability tier is structured, whether the failover design keeps detection, routing, and reconciliation as separate concerns, and how idempotency holds up under retries.


### Reliability tiers compound at scale


Higher contractual uptime tiers materially reduce permitted downtime, so ask which tier the processor contractually guarantees and what the remediation path looks like when the SLA is breached. Service credits, refunds, and termination rights matter more than the uptime number itself. A 99.9% uptime SLA across multiple availability zones with replication, an RTO measured in hours, and an RPO of zero are useful benchmarks when reading any processor's SLA fine print.


### Failover should keep concerns separable


Detection, decision, routing, and reconciliation are different problems. Collapse them and you increase the blast radius of a failure. If a processor's routing logic also owns reconciliation, a routing failure can become a settlement-record problem rather than an isolated availability problem.


### Idempotency is the control that prevents duplicate charges under retries


Many evaluations stop after confirming that the processor supports idempotency keys. A common idempotency failure starts when a payment API times out before the client knows whether processing succeeded. The client retries. If the first request already succeeded server-side, the customer can be charged twice. The timeout is ambiguous: the client cannot distinguish "failed before processing" from "failed after processing."


Watch three practical traps:


1. **Key generation on retry.** If the client generates a fresh UUID on each retry rather than reusing the original, the downstream service treats it as a new request and charges again. Verify that your integration reuses the same key across the full retry sequence.
2. **TTL versus retry window.** If TTL alignment is wrong and the key expires while the client is still retrying, the retry finds no record and processes again. Set the key TTL longer than your maximum retry path, with margin. Verify the processor's retention window against your longest retry path.
3. **Thundering herd.** When a server problem fails many clients at once, their retry schedules align and hammer the recovering server. Add jitter to each client's wait.


On event delivery, treat "exactly-once" guarantees cautiously. In practice, design for **effectively-once processing** : at-least-once delivery combined with strict idempotency on the receiving end, with the idempotency key store transactionally linked to the operation it guards. Look for producer-side keys, a transactional outbox, broker-level atomic writes, and consumer-side idempotent handlers with deduplication on payment ID.


Even with idempotent APIs, keep reconciliation in the score. An external processor can still diverge from your internal record, so you cannot assume the external system is always right. That divergence is where the next axis takes over.


## How to evaluate the ledger underneath the processor


A payment processor moves money. A core ledger records who owns it. Conflating the two is how attribution gets lost.


The Synapse collapse showed what that costs. Synapse Financial Technologies, a banking-as-a-service middleware provider, filed for Chapter 11 bankruptcy on 22 April 2024. More than 100,000 fintech end users were locked out of their funds. Its internal records could not be reconciled against the balances held across its four partner banks: Evolve Bank & Trust, American Bank, AMG National Trust, and Lineage Bank.


The Chapter 11 trustee's[first interim status report](https://www.consumerfinance.gov/enforcement/actions/synapse-financial-technologies-inc/) found that the banks held roughly $180 million while end users were owed approximately $265 million. Subsequent trustee filings estimated the aggregate shortfall at $65–95 million. Funds existed in theory; the records no longer proved attribution.


Processor evaluations often miss this distinction. If a processor hands you settlement records, identify who builds the system of record and what enforces correctness in the layer you control.


At[Formance](https://www.formance.com/) , we've built the open-source core ledger that sits underneath these axes. Four properties have to hold for any system of record under a processor to work: double-entry invariants enforced in the storage layer, append-only immutability, write idempotency, and per-entity account isolation. Application-layer enforcement is not enough — and those four properties map directly to the invariants below.


Four[core ledger invariants](https://www.formance.com/blog/financial-operations/what-is-a-ledger) must hold for that layer to produce correct balances:


1. **Immutability.** A posting, once written, cannot be changed or deleted. To amend a wrong posting, write a reversal: a new posting that cancels the original. A $500 credit posted to the wrong user is corrected by a second, equal-and-opposite send. Both events stay visible in the history, and the original posting is never overwritten.
2. **Idempotency.** Safe retries stop a request from being processed twice. Again, generating a fresh UUID on each retry is the common mistake.
3. **Consistency.** Double-entry enforcement where every debit has an equal credit, so the sum of every transaction's postings is zero. Money cannot be created or destroyed, only moved.
4. **Atomicity.** An *orphan-value failure* starts when a server crashes mid-transaction after debiting the sender but before crediting the receiver. Without wrapping both writes in one atomic transaction, value is orphaned in the system. The $4,200 drift in the opener is the delayed symptom; an orphan-value failure mid-transaction is the cause.


Check whether the processor enforces these rules as structural storage constraints. Application-code conventions are weaker. A relational database gives you ACID guarantees on rows, but financial-domain invariants (account semantics, balance semantics, and a language for[programmable money](https://www.formance.com/blog/engineering/what-is-programmable-money) movement) must come from the ledger layer itself.


Once the ledger is sound, the next question is what happens when its records meet the outside world.


## Give reconciliation product-level ownership


Reconciliation breaks when transaction metadata is missing, inconsistent, or arrives across providers in different shapes. At high event volumes, double-entry systems can generate many ledger rows per payment and make reconciliation a continuous architectural requirement.


Multi-rail compounds the problem. When payments flow across multiple gateways, reconciliation benefits from a normalization layer that standardizes transaction IDs, timestamps, currencies, and fee categories before matching begins. The goal is to ingest transactions and balances from PSPs, banks, open banking providers, exchanges, and custodians into a single, uniform data model so matching can run against one shape. Without that layer, gateway-confirmed payments can go missing in the ledger, others can get double-counted, and treasury can end up making liquidity decisions from wrong numbers.


When evaluating a processor, ask whether reconciliation is a distinct, named component that compares internal ledger state against settlement records, and whether reconciliation rules handle more than 1-to-1: a single bank deposit matched against several invoices, and multiple card payments matched back to one bulk invoice. A processor that only emits a settlement file and offloads normalization to your team can leave you with recurring engineering work.


Make ownership, rail, and state visible in the account model so reconciliation stays tractable. One fragile pattern mirrors the bank's reality (one big account) in the internal ledger, then maintains a separate user-balance table that needs constant reconciliation. A better virtual segregation pattern uses hierarchical account paths that encode Entity:Location:State directly: who owns the funds, which rail holds them, and whether they've arrived.


An ACH settlement and a card settlement arrive in different formats, but the ledger should see the same posting shape:


```text
send   [USD/2 50000]   (
source   =   @world:ach:nacha
destination   =   @users:1234:available
)
send   [USD/2 50000]   (
source   =   @world:card:visa
destination   =   @users:1234:available
)
```


Both postings move $500 into the same user's available balance. The only difference is the source account, @world:ach:nacha versus @world:card:visa, which preserves the rail of origin for reconciliation and audit without leaking rail-specific logic into your core.


With the architectural axes scored (failover, idempotency, ledger, reconciliation), the rest of the evaluation is operational.


## Operational checklist


The architectural axes above decide whether a processor can be a system of record. The operational items below decide what it costs to live with day to day. Four items carry the weight: total cost of acceptance, rails and geographic acquiring, PCI posture and fraud tooling, and developer experience. Score them, but don't let them outweigh correctness.


**Fees and total cost of acceptance.** Score fees as total cost of acceptance. Interchange-plus is transparent and usually cheaper at scale; flat-rate (e.g., 2.9% + 30¢) is simpler but more expensive above modest volume. Watch hidden line items (monthly account fees, gateway fees, PCI compliance fees, batch fees, minimum-volume penalties), chargeback economics (per-dispute fees of $15–$50 for standard merchants, up to $100+ for high-risk verticals), FX markups of 1–3% on top of interchange, and settlement timing trade-offs. At renewal, the number to compare is total fees ÷ total processed volume over a representative month.


**Rails and geographies.** Local acquiring is the single highest-leverage geographic decision; everything else here is downstream of it. Verify each card network (Visa, Mastercard, Amex, Discover, JCB, UnionPay, Diners) is acquired, not just displayed. Confirm coverage for the bank rails you need (ACH, SEPA, Faster Payments, RTP, FedNow, PIX, UPI) and wallets/APMs your customers actually use. Local acquiring typically lifts authorization rates 5–10 percentage points versus cross-border; map the processor's acquiring footprint to your top-10 revenue countries, and check which currencies settle without forced conversion.


**PCI DSS, fraud, and chargebacks.** Optimize for scope reduction first, fraud sophistication second. A processor that keeps you on SAQ-A saves more engineering and audit time than one with the cleverest ML model. Beyond scope: native 3-D Secure 2.x with selective exemption handling, network tokens, built-in fraud tooling with the option to plug in Sift, Forter, or Signifyd, and API-driven dispute lifecycle. Manual dispute portals are a hidden labor tax.


**Developer experience.** Sandbox fidelity and webhook reliability are the two items here that compound for years; everything else is table stakes. Measure idempotency-key support, predictable error codes, SDK breadth and maintenance cadence, sandbox simulation of declines and 3DS challenges and disputes, webhook retry semantics and signature verification, and test card coverage for every rail you'll use.


The operational items live or die against the regulatory regimes the processor has to operate inside.


## Map compliance requirements to technical constraints before you sign


Compliance requirements impose architectural constraints that are hard to retrofit onto a mutable-balance architecture. Three regimes dominate processor selection in EU and UK markets: FCA safeguarding, DORA, and MiCA. They differ in scope. The technical pattern they each demand is the same: per-client fund or asset attribution, daily reconciliation against external counterparties, and immutable or tamper-evident transaction history with point-in-time balance queries. A processor that satisfies one regime architecturally tends to satisfy the others; one that fails any of them tends to fail all three.


### EMI safeguarding (FCA)


Under[FCA CP24-20](http://www.fca.org.uk/publication/consultation/cp24-20.pdf) , firms using the segregation method must:


- Keep relevant funds segregated **from receipt** .
- Segregate relevant funds into a designated safeguarding account by the end of the business day following receipt ( **D+1 placement** ).
- Perform both an **internal reconciliation** (comparing the required segregation amount against the safeguarded balance) and an **external reconciliation** (against third-party bank or custodian confirmations) at least every business day. Where funds are held in a different currency, the internal reconciliation must adjust to at least the original currency amount at the prior day's closing spot rate.
- File safeguarding reports to the FCA within **15 business days of month-end** .


Translated to architecture: per-client fund attribution within omnibus accounts, daily automated internal and external reconciliation with currency adjustment, D+1 placement workflows, and exportable monthly artifacts.


### DORA


On 17 January 2025,[DORA became fully applicable](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/digital-operational-resilience-act-dora) . It requires financial entities to maintain incident logging and review incident detection, response, forensic analysis, escalation, and communication capabilities. When outsourcing critical functions, evaluate whether the processor's contracts support audit rights and document resilience expectations and exit strategies. A black-box processor that won't grant audit rights can create DORA liability concerns.


### MiCA


MiCA (Regulation (EU) 2023/1114) has been fully applicable to CASPs since 30 December 2024. Under **Article 70** , CASPs must hold client crypto-assets and funds separately from their own, reconcile records daily, and ensure customer holdings remain identifiable and recoverable in insolvency. In architectural terms, that means per-client asset attribution at the ledger level, not a pooled wallet with an internal balance table bolted on top.


Across these regimes, verify that the processor and your ledger expose immutable or tamper-evident transaction histories and point-in-time balance queries. Atomic linked transfers need to sit alongside them.


## How to score a payment processor


A payment processor moves money across rails. The system of record underneath it should enforce correctness as a structural guarantee, so application code that one engineer understands isn't the last line of defense. Score processors on idempotency mechanics, failover layering, ledger correctness, reconciliation as a first-class component, the operational checklist (fees, rails, PCI posture, developer experience), and whether the processor's regulatory-grade traceability satisfies the regimes you operate under.


The $65–95M Synapse shortfall was not a rail failure. It was a recordkeeping failure. A programmable core ledger with double-entry enforced at the storage layer makes that failure structurally impossible.


Clone the[Formance Ledger](https://www.formance.com/modules/ledger) on GitHub (MIT-licensed, open-source, programmable core ledger with the four invariants built in as native primitives) and run it locally.
