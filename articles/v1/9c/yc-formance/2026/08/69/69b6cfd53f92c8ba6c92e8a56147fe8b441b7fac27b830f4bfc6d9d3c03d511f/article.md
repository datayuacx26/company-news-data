---
schema_version: "1.0.0"
document_id: "69b6cfd53f92c8ba6c92e8a56147fe8b441b7fac27b830f4bfc6d9d3c03d511f"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/industry-analysis/payment-orchestration-vs-payment-gateway"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-06T23:28:18.502414+00:00"
fetched_at: "2026-08-07T00:00:01.615228+00:00"
content_hash: "sha256:7b7bc9c5ddec6ae567fdedfe94b93d17f9b7b9fca95994149fa03021589951e2"
---

# Payment Orchestration vs. Payment Gateway: Which Does Your Stack Need?

Engineering integrates one gateway, authorization rates hit a ceiling, someone signs a second acquirer, and routing logic starts living in if-statements scattered across the checkout service. But nobody designed a routing layer, so it accreted inside application code, where every change requires a deploy.


Once a second acquirer is in the stack, the reconciliation window widens too. Suddenly, finance teams inherit a second settlement file format, and engineering inherits the hidden dependencies between routing logic and the ledger those routes are supposed to update.


A payment gateway sits at the authorization edge because it connects your checkout to a single acquirer and carries the approve-or-decline message. Payment orchestration sits one level up, above multiple gateways, and decides which one handles each transaction. Naming the layer each component occupies is the first step, while the second is recognizing what neither layer does.


At Formance, we believe both layers are stateless by design. A gateway performs authorization, while an orchestration layer makes a routing decision. Yet, neither maintains a double-entry system of record of what those transactions did to your balances. The most expensive failures in payment infrastructure occur when there is no record of them.


### What a payment gateway does, and where its responsibility ends


A payment gateway is a stateless authorization channel. It validates a transaction request, routes it to an acquirer, returns approval or decline and exits the flow.


In card-network terms, the gateway is front-end technology built into checkout and carries authorization messages. The ledger records how the payment affected your balances.


The gateway participates in the authorization-to-settlement flow in five simple steps:


1


During authorization, the gateway forwards the request through the processor to the acquiring bank, the card network, and the issuing bank, then carries the response back.


2


The issuer places a hold on funds without withdrawing them; the hold can expire if it is not captured within the applicable network, issuer, or processor window.


3


During capture, the merchant or payment service provider (PSP) collects the reserved funds. Timing varies by implementation: some stacks capture automatically, while others use delayed or manual capture.


4


The acquirer owns the settlement and now calculates the net amount after interchange, assessment, and gateway fees.


5


Fund availability then depends on the acquirer's payout process, the merchant's bank, weekends, holidays, and local settlement cutoffs.


When settlement reporting lands later, the gateway's job is complete. Reconciliation exposure begins with that file. The settlement report arrives asynchronously, in a provider-specific format: providers differ in how they expose settlement, accounting, batch, and fee data. The transaction identifiers inside are provider-native.


A card transaction's acquirer reference is assigned within the card-processing chain; it does not automatically map to your internal order ID. Some provider workflows also distinguish order-level reconciliation references from payment- or capture-level investigation references. Matching those files against internal records requires custom parsing logic per provider, and that work belongs entirely to your team.


### What payment orchestration does, and where its responsibility ends


Payment orchestration is the routing layer above multiple gateways. It selects which gateway or acquirer handles each transaction based on configurable rules for cost, geography, success rate, or payment rail.


The orchestration platform examines routing parameters such as Bank Identification Number (BIN), currency, transaction amount, customer location, current acquirer status, and historical approval patterns. It then ranks the available providers and sends the payment to the preferred route.


Orchestration adds routing controls that a single gateway cannot provide. When an acquirer declines or times out, waterfall fallback can retry eligible failures using the next-best acquirer while maintaining the same checkout experience. In multi-acquirer setups, waterfall fallback may recover some soft declines that a single static PSP path would lose.


Transactions can also be routed by fee, typically toward a domestic debit scheme over an international network when the transaction is eligible. The cheapest route can harm approval probability, so cost-based routing has limits.


For cross-border payments, local acquiring routes transactions through in-market acquirers, so the issuer sees an in-country authorization request. Cross-border transactions typically face higher decline rates than domestic transactions, and in-market acquiring can improve issuer recognition in the target market.


Orchestration can complicate your reconciliation because each acquirer added for routing purposes produces its own settlement files, ID schemas, settlement cycles, and dispute workflows. The orchestration layer contributes its own event log of routing decisions and retries, a log that nothing automatically reconciles against your core ledger or against each provider's books.


The new failure modes include routing logic errors and provider outages affecting specific transaction types;[multi-source reconciliation](https://www.formance.com/blog/financial-operations/automate-reconciliation) shifts from back-office cleanup to the payment architecture.


### Payment gateway vs payment orchestration comparison


**Payment gateway** **Payment orchestration**


**What it does** Carries the authorization message between checkout and one acquirer Routes each transaction across multiple gateways or acquirers based on rules


**What it records** Event log of authorization requests and responses Event log of routing decisions and retries


**Stateful?** No (stateless authorization channel) No (stateless routing layer)


**Reconciliation surface** One provider's settlement file Every provider's settlement file plus the orchestration layer's own event stream


### Why neither gateways nor orchestration platforms record the financial state


Neither gateways nor orchestration platforms record postings, so financial state drifts from provider reports whenever provider data is treated as the system of record. Provider reports conflict with internal state, especially when prior-period refunds and chargebacks net into current batches out of order.


Consider what stateless execution layers cannot catch.


A payment operator intends to send an interest payment to lenders, but instead wires the full outstanding principal years before it is due. Because the loan system defaults, it sends the entire balance unless several override boxes are left unchecked.


A maker, checker, and approver workflow lets the same mistake propagate through every human gate. From the payment rails' perspective, the wire executes correctly. Every control upstream of irrevocable settlement fails because the only gates were a user interface (UI) and a manual approval workflow.


Gateways and orchestration platforms leave the recording gap because they log events, not postings. An event log captures what was requested and whether it succeeded. They enforce no rule that debits equal credits, so a discrepancy can hide in it indefinitely. A[double-entry posting](https://www.formance.com/glossary/posting) records the movement of funds between named accounts in your system of record, and because every movement has a matched pair, the sum of all entries is either zero or an alarm.


#### How a payment orchestration layer misses financial postings


An orchestration layer executes the posting below without recording it. A $5,000.00 card settlement routed through PSP A arrives; $140.50 of it is processing cost, and the rest belongs to the customer's available balance.


The accounts are @counterparties:paymentServiceProviders:a:settlement for the PSP settlement boundary, @platform:fees:processing:paymentServiceProviders:a for the processing cost, and @customers:alice:wallet:available for the customer's available balance.


In[Numscript](https://www.formance.com/blog/engineering/numscript) , Formance's purpose-built language for describing financial transactions, the settlement is shown below:


```text
// CARD_SETTLEMENT
// Event: settle a $5000.00 card payment routed through PSP A; book a $140.50 processing fee
send   [USD/2 500000]   (
source   =   @counterparties:paymentServiceProviders:a:settlement   allowing   unbounded   overdraft
destination   = {
max   [USD/2 14050]   to   @platform:fees:processing:paymentServiceProviders:a
remaining   to   @customers:alice:wallet:available
}
)
set_tx_meta  (  "event_type"  ,   "card_settlement"  )
set_tx_meta  (  "settlement_id"  ,   "stl001"  )


```


The postings commit atomically: the customer is never credited $4850.50 unless the $140.50 fee is booked in the same transaction. The gateway saw an authorization succeed, and the orchestration saw a route complete. Only the core ledger holds the financial state, which includes the balance growth, the costs incurred and the settlement against which.


### When to use a payment gateway, payment orchestration, or both


Four signals decide whether your stack needs a gateway alone, orchestration on top of it, both together, or a core ledger beneath them.


#### When a payment gateway works alone


A payment gateway alone fits a narrow payment stack. One acquirer relationship keeps authorization and reconciliation tied to a single provider, with a single settlement file format to parse. A single market also keeps the case simple. Domestic transactions through a domestic acquirer already get the approval treatment that local acquiring exists to recover.


When routing failures or provider outages do not materially affect revenue, and provider-specific declines stay limited, a single PSP usually handles peak load without meaningful friction and fallback logic buys little.


For a narrow single-acquirer stack, the gateway remains the execution layer, and the reconciliation surface stays narrow enough to be managed directly.


#### When payment orchestration becomes necessary


Payment orchestration becomes useful once a single static path limits payment performance. Authorization rates may plateau on one acquirer. Improvement then requires another provider or market-specific acquiring, often backed by routing experiments.


Expansion into markets that require local acquisition has the same effect. Each in-market acquirer is a new integration project, and orchestration centralizes them under a single layer.


Hardcoded routing is visible in the codebase. Failover requires manual intervention or does not exist, and payment logic stays distributed across payment services.


Once routing logic is scattered across services, teams should make the routing layer explicit and remove it from the payment services.


#### When to use a payment gateway and orchestration together


Gateway plus orchestration is the correct architecture for any team processing across more than one acquirer.


The layers perform different jobs: orchestration handles routing decisions, and each underlying gateway handles the authorization protocol with its acquirer.


Adding orchestration removes none of the underlying gateways. Orchestration sits above the gateways and processors, routing between them.


#### When the stack also needs a core ledger


The moment your team holds customer funds in an[omnibus account](https://www.formance.com/glossary/omnibus-account) , payment gateway and orchestration logs are no longer adequate, because they cannot prove an individual customer's balance at a point in time.


Regulators in major jurisdictions increasingly require firms to reconcile customer-level balances in an internal ledger against balances held in external safeguarding or custodial accounts, often on a daily cadence, with the balance attributable to each beneficial owner traceable at any point in time.


Recent failures among fintech intermediaries holding pooled customer funds show the cost of failing this test, with shortfalls in the tens of millions that cannot be attributed to individual account holders when the underlying records are gone.


[Formance Ledger](https://www.formance.com/modules/ledger) sits beneath the orchestration and gateway layers as the double-entry system of record for exactly this case. It attributes every cent in the omnibus account to a named account at any point in time, with regulatory-grade traceability that neither execution layer maintains.


### The three-phase migration path from a single payment gateway to an orchestrated multi-rail


Payment stacks often move from a single gateway to multi-acquirer routing and then to dedicated orchestration, and reconciliation complexity compounds along the way.


#### Phase 1: Single-gateway integration


One PSP with one authorization path. Each night, the PSP sends a settlement file, and your reconciliation job parses it against internal orders. Even single-gateway reconciliation is not free. Data preparation, file normalization, and exception review can consume finance time before reconciliation begins.


#### Phase 2: Multi-acquirer expansion


A second acquirer arrives for redundancy or market coverage. Routing logic appears in application code, and reconciliation complexity grows. You need two ID schemas, two settlement cycles, two file formats and two dispute workflows. The second acquirer pays off when recovered soft declines, market coverage, and processor resilience outweigh its cost.


#### Phase 3: Dedicated orchestration layer


Routing logic moves out of application code into a configurable orchestration platform. The reconciliation problem now spans every provider's settlement reports plus the orchestration layer's own event stream, and maintaining a unified view of financial state across all rails requires a central ledger rather than per-provider scripts.


#### Where the ledger fits in the migration sequence


Instrument the core ledger at Phase 2. Retrofitting a system of record onto an already-orchestrated multi-rail stack means reconciling historical state across every provider first, once payments, refunds, disputes, fees, and provider-specific identifiers have already accumulated across multiple rails, which is the problem the ledger was supposed to prevent.


With early instrumentation, product boundaries matter. You need to pull settlement and event data from[connected providers](https://www.formance.com/connectors) into a single data model, with pre-built connectors for Stripe, Adyen, Mangopay, Wise, Banking Circle, Modulr, and Fireblocks, plus a generic connector for anything else.


### Payment orchestration vs payment gateway is a routing complexity question


Most scaling teams need both gateway and orchestration. The more consequential architectural decision is whether a core ledger sits beneath them as the system of record. Multi-provider payment stacks are already common, and the multi-provider condition that makes orchestration necessary is normal for scaling teams.


Teams running orchestration without a core ledger risk treating provider event logs as the financial-state substrate. A regulator may request per-customer balances that the logs cannot provide. Finance and engineering may spend hours each month on reconciliation, and each additional rail can compound data drift.


Formance maintains the open-source, double-entry system of record across fiat and digital assets beneath the gateway and orchestration layers, so your team stops relying on parsing scripts and mutable balance tables to stand in for a system of record.
