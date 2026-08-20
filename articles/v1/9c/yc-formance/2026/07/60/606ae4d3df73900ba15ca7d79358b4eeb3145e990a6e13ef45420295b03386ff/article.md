---
schema_version: "1.0.0"
document_id: "606ae4d3df73900ba15ca7d79358b4eeb3145e990a6e13ef45420295b03386ff"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/event-sourcing-vs-event-driven-architecture"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T15:40:58.350186+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:1b647a9bf0f51fc5a6f8e47c1bde9a59152b35cf7431907007e666d2814e362e"
---

# Event sourcing vs event-driven architecture: what's the difference?

Financial systems use events in two ways: to store money movements as a durable history and to notify other services that something has happened. For engineering teams building ledgers or reconciliation pipelines, confusing those events causes audit issues.


For example, an auditor asks for a merchant's balance as of March 31. Your platform is event-driven, so the team assumes the answer is accurate. Then, engineering checked the broker and found that the events from March had expired months ago, because the broker was configured to expire old messages. The team built event-driven architecture and assumed it included event sourcing.


Confusing event sourcing with event-driven architecture is expensive in financial systems because both concerns are non-negotiable, but demand different design decisions. This article distinguishes between the two patterns, starting with what each is and where it belongs in a payment system.


## What is event-driven architecture?


Event-driven architecture is a way for services to communicate. It decouples producers from consumers by routing domain events through a message broker, like synchronous API calls, which remain for direct request/response flows.


The payment service that settles a transaction publishes PayoutSettled and does not need to know which downstream services care about it. Events are messages that trigger reactions in downstream consumers, while each service keeps its own state wherever it chooses. The producer/consumer separation supports loose coupling and independent deployment across interoperable services.


Financial systems commonly need to account for duplicate message delivery. In at-least-once delivery environments, the same event can arrive more than once.


A non-idempotent AccountDebited handler that subtracts on every delivery can result in an incorrect balance. Ledger-writing consumers should enforce idempotency, typically by tracking processed message IDs, or duplicate deliveries can become double-postings.


Even when brokers offer stronger delivery features, ledger-writing consumers are usually designed to make their own database writes idempotent.


The typical practitioner topologies balance consumer autonomy with their connection to producer data through three methods:


### 1. Event notification topology for lightweight signaling


The event carries a small signal, such as an ID, and the consumer asks the producer for the rest. Schema contracts remain minimal, but consumers rely on the source for the full state. Hence, a ledger reconciliation worker built on notifications depends on ledger availability at the time of reconciliation.


### 2. Event-carried state transfer topology for consumer autonomy


The event carries the payload a consumer needs to maintain its own copy, giving consumers greater resilience if the producer goes down. The cost is data replication, eventual consistency, and a heavier schema contract that makes producer evolution harder.


### 3. Event streaming topology for replayable consumption


[Consumers read from a durable](https://docs.formance.com/manage/events/streaming?deployment=cloud&license=ee) , partition-ordered log and track their own offsets, which supports replay while records remain available but ties recovery depth to broker configuration.


While event-driven architecture handles communication, event sourcing handles the record-keeping aspect of a financial system.


## What is event sourcing?


Event sourcing is a way to persist state inside a service. It replaces mutable state records with an append-only history of discrete events that the service can apply to reconstruct past state.


With event sourcing, a service stores each meaningful change as a durable fact and derives an entity's state by applying those facts in sequence. The event history is the authoritative source of state, and auditors require that the durable event history be treated as authoritative while current balances are computed from it.


For a financial ledger using event sourcing, the append-only invariant must be absolute, and posted records must be treated as immutable. If an event was recorded incorrectly, the correction is a new compensating event appended to the log; the original stays in place.


The current balance is always derived by replaying the full event sequence from the beginning or by adding the events since a known snapshot. Snapshots improve read performance, but they do not replace the underlying history because the log remains the source of truth.


In a financial system, each state-change event maps directly to a double-entry posting. Expressed those postings using[Numscript](https://www.formance.com/blog/engineering/numscript) , Formance's purpose-built language for describing the intent of a financial transaction, and recorded financial transactions with regulatory-grade traceability.


A PaymentReceived event that releases $20,500.00 from an[omnibus account](https://www.formance.com/glossary/omnibus-account) (an account that holds funds on behalf of many underlying owners) into a merchant's available balance is one atomic posting pair. Acme's pending omnibus funds sit at @merchants:acme:omnibus:pending, and Acme's available balance sits at @merchants:acme:available:


```text
// PAYMENT_RECEIVED
// Event: release a $20,500.00 payment from Acme's pending omnibus balance to its available merchant balance
send   [USD/2 2050000]   (
source   =   @merchants:acme:omnibus:pending
destination   =   @merchants:acme:available
)
set_tx_meta  (  "event_type"  ,   "payment_received"  )
set_tx_meta  (  "payment_id"  ,   "pay001"  )


```


The debit to @merchants:acme:omnibus:pending and the credit to @merchants:acme:available post together or not at all. Because every such posting lands in an append-only log, reconstructing the merchant's balance at any point in time requires replaying the append-only log.


Event sourcing makes bi-temporal ledger modeling practical by tracking both when an event was recorded and when it became effective. A bi-temporal ledger carries separate recorded and effective timestamps for each posting so that you can answer two different questions independently: "what was the balance on date D?" and "what did we believe the balance was on date D, as of record date R?"


Backdated corrections append a new record-time entry that changes effective history without rewriting what the system knew before. Separate record time from effective time to answer those questions.


## Five differences between event sourcing and event-driven architecture


Five practical dimensions separate event sourcing and event-driven architecture: where they apply, what an event represents, how systems built on them are tested, how they recover from failure, and how their schemas evolve.


**Dimension** **Event sourcing** **Event-driven architecture**


Scope Inside one service boundary (e.g., the core ledger) Across service boundaries (rails, fraud, notifications, and reconciliation)


What an event is The database itself; deleting the log deletes the system A message; once consumed, its job is done; state lives per service


Testability Deterministic replay of the same sequence yields the same state Isolate consumers, simulate broker behavior, and guard against side effects


Failure recovery Rebuild the current state from the service's own log, independent of any broker Depends on broker durability, retention, and consumer group replay semantics


Schema evolution Temporal obligation, including version events with tolerant deserialization (optionally upcasting) Spatial obligation, with producers and consumers at different versions, must interoperate


### 1. Scope of event sourcing vs event-driven architecture


Event sourcing lives within a single service, while event-driven architecture spans services.


Event sourcing is applied inside a single service boundary, the core ledger, to enforce a durable, auditable state.


Event-driven architecture operates across service boundaries to coordinate the payment rail integrations, fraud scoring, notification, and reconciliation services that surround the ledger.


### 2. What an event is in each pattern


In event sourcing, the event is the database; deleting the log deletes the system.


In event-driven architecture, the event is a message; once consumed, the message has done its job, and each service's state lives in its own store.


### 3. Testability of event-sourced and event-driven systems


An event-sourced ledger supports deterministic replay. Engineers can apply the same event sequence to a fresh aggregate and obtain the same state every time, making it a reproducible operation to debug a past ledger state or answer an audit question.


Event-driven reaction chains are tested differently, as teams typically isolate consumers and simulate broker behavior. Tests guard side effects so they do not fire against live systems.


### 4. Failure recovery for event-sourced and event-driven systems


Recovery depth differs sharply between the two patterns. An event-sourced ledger reconstructs the current state from its own log after a crash, with recovery independent of broker replay.


An event-driven system's recovery depends on broker configuration, including durability and retention, plus consumer group replay semantics being configured correctly before the incident. It resets a consumer, but only helps if the events are still available.


### 5. Schema evolution burden in each pattern


Schema evolution creates different obligations in each pattern. Event sourcing typically creates a temporal obligation, which calls for event versioning with tolerant deserialization from day one.


Event-driven architecture creates a spatial obligation with producers and consumers at different versions to interoperate simultaneously, and the coupling only surfaces when they deploy out of sync.


In[Formance programmable ledgers](https://www.formance.com/modules/ledger) , immutable history and replayability at the ledger layer are correctness requirements. Immutability and replay are prerequisites for regulatory-grade traceability, and the retrofit problem applies with full force to money. You cannot add a complete history after the production incident that made you want one.


Those five differences shape when to reach for each pattern, and when to reach for both together.


## When to use event sourcing, event-driven architecture, or both


Use event sourcing when a service needs a durable, replayable history, and use an event-driven architecture when independent services need to react to financial events. You can combine both in regulated payment systems.


### When to use event-driven architecture


Use event-driven architecture without event sourcing when multiple services need to react to the same domain event, such as a payout being settled or a card being authorized. Still, each service manages its own mutable state and has no requirement to replay its full history for auditing.


Many production event-driven systems operate this way without event sourcing, and the pattern is fine for services that do not need audit-grade replay.


### When to use event sourcing


Use event sourcing without event-driven architecture when a single service, the core ledger, must maintain a complete and immutable history of every state change to satisfy point-in-time balance reconstruction and backdated correction under regulatory audit, even if no other service ever subscribes to those events. Event sourcing is often used for only part of a system, especially the accounting side.


### When to use both


The combination of an event-sourced ledger and an event-driven architecture becomes mandatory when a regulated entity operates across[multiple payment rails](https://www.formance.com/blog/financial-operations/payment-rails-explained-for-fintech-engineers) .


The ledger must be the authoritative, event-sourced system of record for every posting. The ledger records every posting, while the payment providers connected to it, including PSPs, banks and digital asset rails, are the ones actually moving funds.


The event-driven layer handles asynchronous fan-out to the reconciliation layer. Reporting pipelines and notification services consume the same ledger output to determine when a transaction has been posted, but do not own the ledger's state.


Combining both patterns raises an implementation question: where exactly does the ledger boundary sit?


## Drawing the boundary between event sourcing and event-driven architecture in one financial stack


[Formance Ledger](https://www.formance.com/modules/ledger) provides the event-sourced foundation that gives financial systems their durable, replayable history, while emitting domain events onto whatever message broker a team already runs to support downstream reactions. The ledger's append-only log is the system of record for every posting, and the broker is the delivery mechanism that carries ledger output to consumers. A broker configured with default retention is not a system of record.


The boundary sits between the ledger's append-only log and the messaging layer that carries its output. Downstream consumers, including reconciliation workers, reporting pipelines, and notification services, subscribe to ledger output and react to posted transactions without owning ledger state.


Formance secures regulatory-grade traceability inside the event-sourced ledger, while the event-driven layer outside it handles decoupled fan-out to the rest of the payment stack. The boundary between the ledger and the messaging layer is what a payment system needs to draw before its first audit, not after.
