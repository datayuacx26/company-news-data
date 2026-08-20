---
schema_version: "1.0.0"
document_id: "8f28c055d690ccb127eedffe89e3446af45c0b07518767a143e4a659804e3f42"
company_key: "yc-slash"
company: "Slash"
source_id: "yc-slash-news-import-9ec40849fc98"
canonical_url: "https://www.slash.com/engineering/blog/banking-grade-stablecoin-rails"
published_at: "2026-01-26T18:45:49.275+00:00"
first_seen_at: "2026-07-24T01:09:43.930358+00:00"
fetched_at: "2026-07-28T22:23:07.215004+00:00"
content_hash: "sha256:11e73de531f30af04867e2f6fa92a377929afa93adb8e9ad083f5553bfe2f2d7"
---

# Building Banking-Grade Stablecoin Rails at Slash

Earlier this year, we crossed $1B in total stablecoin payment volume. For context, we processed our first stablecoin transaction less than 12 months ago. It started with a handful of customers asking for something that should be simple: the ability to receive stablecoin payments from their clients around the world and have those funds **settle in US dollars** - quickly and predictably, all without getting crushed on fees. Existing off-ramp solutions meant 1-3% fees, multi-day settlement, and incredibly long and frustrating compliance processes. We saw an opportunity and built an MVP in two weeks. Within a month, we had real and significant volume.


The challenge was scaling it.


Stablecoins settle in seconds, anywhere, any time. Traditional banking rails weren't built for that- they run on batch processing, business-day cutoffs, and multi-day settlement windows. A customer in Singapore sends you $50,000 in USDC at 2am on a Saturday. On-chain, it settles in seconds. The ACH to your bank account won't initiate until Monday, won't arrive until Wednesday, and might sit in review somewhere in between. Bridging these two worlds means coordinating across systems that were never designed to talk to each other, each with its own state.


This post is about the two infrastructure primitives we built to make stablecoin movement behave like banking-grade money movement:


**1. Flow of Funds** : a declarative orchestration engine for long-running, multi-system financial workflows


**2.** Our **On-chain execution framework** : A lifecycle model for reliable on-chain operations


Everything else—off-ramps, on-ramps, and our non-custodial Global USD accounts—is built by composing these primitives. If you take one thing from this post: stablecoins are easy; stablecoin *banking* isn't.


## Flow of Funds


When you're moving money through multiple external systems, you need something more than ad-hoc coordination. You need a way to express the entire flow declaratively: what should happen, in response to what events, with what guarantees. And you need that flow to be auditable, resumable, and correct even when steps fail mid-flight. That's what Flow of Funds gives us.


### The Problem with Money Movement


Most orchestration challenges in software are about handling failure gracefully. Financial orchestration has a harder constraint: **money is already in motion** .


Consider an instant deposit flow. A customer receives $10,000 USDC. We credit their account immediately (before the underlying ACH settles) so they can deploy that capital right away. Behind the scenes, we've issued a loan. When the ACH arrives days later, we collect repayment and fees.


That's four operations across two external systems: crypto liquidation, loan disbursement, ACH settlement, fee collection. Each step depends on the last, and any step can fail. You can't manage this with scattered state and ad-hoc handlers.


### The Core Abstraction


Flow of Funds is a declarative, rule-based orchestration system. The core abstraction has three parts:


**Events** are signals that something happened—an ACH settled, funds were received, a card authorization was captured, etc. Events can come from external systems or be emitted internally.


**Rules** define what should happen in response to events. Each rule specifies a list of triggering events and a sequence of side effects to execute.


**Side Effects** are the actions we take in response to the event: initiate a transfer, create a hold, disburse a loan, collect a fee. A rule fires once. The first time a matching event arrives, the side effects execute in order, and the rule is consumed. This guarantees idempotency within the flow context.


### Why Declarative?


The alternative is imperative orchestration: handlers calling handlers, state scattered across tables, the "flow" existing only in the implicit coordination between pieces of code.


That works for simple flows. For multi-day, multi-system financial operations with compliance holds and partial failures, it becomes unmaintainable. Error handling is ad-hoc. Recovery paths are implicit. Six months later, nobody can confidently answer "what happens if step 3 fails after step 2 succeeds?"


Declarative rules flip the model. You define the state machine explicitly: *these* events trigger *these* actions. The orchestration engine handles execution, persistence, and recovery. The flow *is* the documentation.


### Guarantees


FoF gives us four invariants we can rely on:


**1. Idempotency -** a rule fires exactly once per flow context, regardless of duplicate events or retries


**2. Deterministic reconciliation -** given the same events, the flow resolves to the same state


**3. Full auditability -** every side effect execution is tracked with lineage back to the triggering event


**4. Composability -** complex flows are built from simple rules that compose without becoming monolithic


### Execution Tracking


We track every side effect execution via Node records—each linked to its parent, forming a complete execution tree. When compliance needs an audit trail, we can trace the exact path through the system.


```text
Root   Node   (flow.begin)
├──   Node:   CreateOffRampTransaction
├──   Node:   SendNotification
└──   Node:   CreateSettlementRule
└──   (child   rule,   fires   later  )
├──   Node:   CollectLoanRepayment
├──   Node:   CollectFees
└──   Node:   ResolveTransaction
```


### Composability: Nested Rules


Rules can spawn child rules. This is how complex, multi-step flows compose without becoming monolithic. When an off-ramp transaction is created, the initial rule doesn't try to handle everything. It sets up *future* rules—listeners waiting for events that will come later:


```text
createRule  ({ events: [  'flow.begin'  ] })
.  addSideEffect  (CreateOffRampTransaction)
.  addSideEffect  (SendNotification)
.  addSideEffect  (
CreateRuleSideEffect.  create  ({
rules:   createRule  ({
events: [  'inbound_ach.completed'  ,   'completed'  ],
})
.  addSideEffect  (CollectLoanRepayment)
.  addSideEffect  (CollectFees)
.  addSideEffect  (ResolveTransaction)
.  buildRule  (),
})
)
.  buildRule  ();
```


The settlement logic doesn't exist as dead code waiting to be called. It exists as a rule, waiting for its event. When the banking provider webhook arrives days later, the rule fires and the flow continues. The parent rule is consumed and the child rule carries the context forward.


This also means flows are arbitrarily composable. Want to implement instant deposits? Easy. Add a rule that disburses the loan and sets up repayment rules. Each concern is isolated, but they all compose into a coherent flow.


### Side Effect Execution Contexts


Not all side effects are equal. Some need to be atomic with the database transaction. Some call external APIs. Some are fire-and-forget.


Method


Execution


Use Case


addSideEffect


Synchronous, same DB transaction


Async, no transaction


addAsyncSideEffect


Async via Temporal


External API calls, long-running operations


addAsyncNonTransactionalSideEffect


Async, no transaction


Notifications, logging, analytics


### The Payoff


The biggest benefit is how FoF changes the way engineers write orchestration code at Slash. Without it, every engineer solves the same problems differently. Everyone reinvents the wheel, and the wheels are all slightly different shapes.


FoF raises the floor. You define *what* should happen, not *how* to handle every failure mode. The framework handles execution, persistence, and observability. New engineers can read a flow definition and understand it without tracing through layers of imperative code. And it's harder to write bad orchestration code when the abstraction forces you to be explicit about events, side effects, and state transitions.


## Ramps: FoF in Practice


With FoF as the foundation for composing financial flows, building our core products became a matter of defining the right rules for each flow. The abstraction doesn't care what systems are on the other end, it just orchestrates.


Off-ramps and on-ramps are "flows" orchestrated by this engine, introducing a new external system: a crypto provider (or OTC desk) that handles stablecoin/fiat conversions. Like any external system, they deliver state updates on their own terms- which we can use to trigger FoF events. From there, it's just flow composition.


### Off-Ramps


Off-ramps let customers receive stablecoin payments and settle them as USD in their Slash account. The flow is straightforward:


1. Customer receives USDC or USDT at a deposit address we generate via our crypto provider
2. Provider detects the deposit, liquidates to USD, and initiates an ACH or wire
3. Our banking provider receives the inbound transfer
4. We reconcile the transfer to the original transaction and credit the account


For instant deposits—where we credit the customer immediately and collect repayment when the ACH settles—the flow includes loan disbursement, repayment collection, and fee capture. Each concern is a separate rule, listening for its event, composing into a single coherent flow. The FoF definition looks something like this:


```text
const   offRampInstantDepositFlow   =   createRule  ({
name:   'instant_deposit'  ,
events: [  'provider.deposit_received'  ],
})
.  addSideEffect  (
DisburseLoan.  create  ((  ctx  )   =>   ({
accountId: ctx.accountId,
amount: ctx.providerDeposit.amount,
}))
)
.  addSideEffect  (
// The settlement logic doesn't run immediately
// it waits as a rule until the ACH actually settles
// which might be days later
CreateRule.  create  ((  ctx  )   =>   ({
traceId: ctx.deposit.ach_trace_number,


rule:   createRule  ({
events: [  'inbound_ach.settled'  ],
})
.  addSideEffect  (RecollectLoan.  create  ({ loanId: ctx.loanId }))
.  addSideEffect  (
CollectFees.  create  ({ depositId: ctx.providerDeposit.id })
),
}))
);
```


### On-Ramps


On-ramps are the inverse: customers send USD from their Slash account and receive stablecoins at an external wallet. The flow:


1. Customer initiates a transfer to a destination wallet address
2. We create holds on their account for the amount plus fees
3. We send an ACH or wire to a deposit instruction at our crypto provider
4. Provider receives the funds and delivers stablecoins to the destination


```text
const   providerAccountId   =   '...'  ;
const   userAccountId   =   '...'  ;


const   onRampFlow   =   createRule  ({
name:   'on_ramp'  ,
events: [  'flow.begin'  ],
})
.  addSideEffect  (
InitiateTransfer.  create  ((  ctx  )   =>   ({
type:   'wire'  ,
source: userAccountId,
destination: providerAccountId;
// ...
}))
)
.  addSideEffect  (
CreateRule.  create  ((  ctx  )   =>   ({
traceId: ctx.transaction.id,
rule:   createRule  ({
events: [  'outbound_wire.completed'  ],
})
.  addSideEffect  (CaptureFees.  create  ({ transactionId: ctx.transaction.id }))
}))
)
.  addSideEffect  (
CreateRule.  create  ((  ctx  )   =>   ({
traceId: ctx.transaction.id,
rule:   createRule  ({
events: [  'provider.delivery_failed'  ],
})
.  addSideEffect  (CancelPendingFees.  create  ({ transactionId: ctx.transaction.id }))
.  addSideEffect  (InitiateTransfer.  create  ({
type:   'book'  ,
from: operatingAccountId,
destination: userAccountId,
description:   'Refund'  ,
// ...
})),
}))
);
```


What's notable is how little new infrastructure this required. The FoF framework and reconciliation logic we built for off-ramps carried over directly. On-ramps are different rules listening for different events—but the same underlying machinery.


## On-Chain Lifecycle


FoF solved coordination on the fiat side—banking rails, providers, compliance. But when we started building Global USD, we hit a new surface area: the chain itself. Getting a transaction on-chain, confirming it actually landed, handling failures and chain reorganizations, and deriving accurate state from the results—that's a different coordination problem. We needed the same guarantees we had with FoF, but for on-chain execution.


### The Pattern: Intent → Execute → Reconcile


We use a consistent pattern across all blockchain operations:


**1.** **Intent** : Declare what we're trying to do


**2.** **Execute** : Submit the transaction and shepherd it to block inclusion


**3** **Reconcile** : Process confirmed blocks, update internal state, trigger downstream flows


If you're from traditional finance, the analogy is straightforward:


- Intent ≈ payment order
- Execute ≈ pending
- Reconcile ≈ posted


Each phase has distinct responsibilities and failure modes. The rest of this section walks through how we built each layer.


Before we can execute anything, we need to define *what* we're executing. A blockchain transaction is fundamentally an instruction (a function invoked with parameters), but the chain doesn't understand human-readable instructions. Everything gets encoded into **calldata -** a blob of hex bytes that specifies the function to call and the arguments to pass.


For example, a simple USDC transfer—"send 500 USDC to address X"—becomes:


` 0xa9059cbb0000000000000000000000007e2f5e1fd4d79ed41118fc6f59b53b575c51f182000000000000000000000000000000000000000000000000000000001dcd6500`


Raw calldata like this is opaque and tells you nothing about *why* money moved. And when you're building systems that need to track business context—not just that assets transferred, but that this was a fee collection for invoice #1234—you need to preserve that context.


We solve this with a registry of typed call definitions:


```text
```  tsx
export   const   erc20Transfer   =   blockchainCallRegistry  .  define  (
defineBlockchainCall(  {
key:   'erc20.transfer',
abi:   'function transfer(address to, uint256 amount)',
request:   {
input:   tbox.obj({
contract:   tbox.hex(),
recipient:   tbox.hex(),
amount:   tbox.bigIntStr(),
}),
categories:   [
'outbound_wire_transfer',
'outbound_ach_credit',
'outbound_transfer_fee',
],
tags:   [],
context:   tbox.obj({
relatedAuthorizationId:   tbox.opt(  tbox.id('authorizationRequest'  )),
}),
},
encode:   ({ input, encodeFunctionData }) => ({
to:   input.contract,
data:   encodeFunctionData([input.recipient, BigInt(  input  .  amount  )]),
value:   bigIntStr(  0n  ),
}),
})
);


```
```


Engineers work in domain terms—contract, recipient, amount—instead of hex strings. The registry validates inputs, handles encoding, and preserves the metadata we'll need later: category, tags, and business context.


Creating a call becomes straightforward:


```text
const   call   =   erc20Transfer.  create  ({
walletId:   'wallet_12345'  ,
chain:   'base'  ,
request: {
input: {
contract:   '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913'  ,
recipient:   BASE_USDC_FEE_ACCOUNT  ,
amount:   bigIntStr  (  25_000_000  n  ),   // 25 USD
},
category:   'outbound_transfer_fee'  ,
tags: [],
description:   'USDC Fee Transfer - SLASH FINANCE'  ,
context: {}
},
})
```


Each call becomes a BlockchainCall record:


```text
interface   BlockchainCall   {
id  :   string  ;
type  :   string  ;                        // 'erc20.transfer'
chain  :   Chain  ;
walletId  :   string  ;
to  :   Hex  ;                             // Target contract
value  :   BigIntStr  ;                    // ETH value (usually 0)
data  :   Hex  ;                           // Encoded calldata
request  :   BlockchainCallRequest  ;      // Original typed request with metadata


// Intent association
intentId  :   string  ;
indexInBatch  :   number   // Position in the batched transaction
}
```


We treat the BlockchainCall as the atomic unit of work. A transaction might batch multiple calls, but each call represents a single accountable operation. The request field preserves the full typed input in addition to the encoded bytes- including any metadata and arbitrary context. This metadata is what lets us answer "what was this $500 transfer for?" when we reconcile chain state back to business operations.


### Execution: Shepherding Transactions to Finality


Submitting a transaction sounds simple. In practice, there's a minefield between "send this" and "it landed."


When you submit a transaction, it doesn't go directly into a block. It enters the **mempool-** a waiting area where transactions sit until a block producer picks them up. While waiting, transactions can be dropped (mempool is full), outbid (someone paid higher fees), or stuck (gas price too low for current network conditions).


**Gas** is how Ethereum-based networks price computation. Every operation costs gas, and you pay for gas with the network's native token. When you submit a transaction, you specify the maximum gas price you're willing to pay. If network congestion spikes after you submit, your gas price might no longer be competitive- your transaction sits in the mempool, waiting, potentially forever.


Even after a transaction lands in a block, it's not truly final. Blockchains can experience **reorganizations (reorgs)-** situations where the network discards recent blocks and replaces them with a different chain of blocks. A transaction you thought confirmed can disappear. This is rare on mature networks, but "rare" isn't "never" when you're moving real money.


Each of these failures happens at a different layer: gas estimation, signing, submission, confirmation. And recovering from each requires different remediation- a stuck transaction needs resubmission with higher gas, an invalid signature needs re-signing, a reorg needs the whole flow to retry.


We handle this by modeling the execution lifecycle as a hierarchy of 4 entities. At the top is the business outcome we're trying to achieve. Below it, increasingly specific layers handle preparation, signing, and submission. Each layer owns its failure domain and can retry independently before escalating to the layer above:


```text
BlockchainIntent   (what   we're trying to achieve)
└── PreparedCall (unsigned transaction, gas estimates locked)
└── PreparedCallExecution (signing attempt)
└── PreparedCallExecutionNode (submission attempt)
```


**` BlockchainIntent`** represents the business outcome: "transfer 500 USDC to this address as payment for invoice #1234." It's the top-level orchestrator that tracks the full lifecycle and can spawn retries if needed.


**` PreparedCall`** is an immutable, unsigned transaction with gas estimates locked in. If gas estimates expire (network conditions changed), we create a new PreparedCall.


**` PreparedCallExecution`** represents a signing attempt. For server-side operations, we sign automatically. For user-facing operations (like Global USD), the user approves via OTP. Either way, once signed, we're ready to submit.


**` PreparedCallExecutionNode`** is a single submission attempt. We send the transaction to the network and poll for inclusion. If it fails for retryable reasons (network timeout, dropped from mempool), we create a new node and try again.


Each layer handles its own failure domain:


Failure Layer Resolution


Network timeout


ExecutionNode


Retry with new node


Retry with new node


Re-org after confirmation


Retry with new node


Invalid signature


PreparedCallExecution


Require new signature


Gas underestimate


ExecutionNode → Intent


Escalate after N retries


Expired gas estimates


PreparedCall → Intent


Escalate, create new PreparedCall


Re-org after confirmation


BlockchainIntent


Spawn child intent


The key insight is that failures escalate to the parent layer when a layer exhausts its remediation options. Consider persistent gas underestimation. Network congestion spikes, and the gas params locked in our PreparedCall are no longer competitive. The execution node retries a few times, and maybe congestion clears. After N failures, it can't do more. The failure escalates to Execution, which reaches a terminal state and escalates to Intent. The Intent spawns a child intent with higher gas multipliers, constructs a fresh PreparedCall, and the cycle begins again.


Each layer handles its own failure domain, but escalation is explicit. The parent intent preserves the full history; the child intent gets a fresh attempt with adjusted parameters. We never lose context about *why* we're retrying.


### Reconciliation: From Chain Events to Product State


A transaction is included in a block. Now what?


The blockchain doesn't tell us directly a 1000 USDC transfer happened. It tells us a transaction executed and emitted some **event logs** . We need to parse those logs, figure out what they mean, and update our internal state accordingly.


Event logs are how smart contracts communicate what happened during execution. When you call the` transfer` function on a USDC contract, the contract emits a` Transfer` event with three pieces of data: who sent it, who received it, and how much. This event gets recorded in the transaction receipt as a log entry.


But logs are encoded as topic and data fields containing hex-encoded values. Parsing them requires knowing the event signature and decoding the parameters. A raw Transfer log looks something like:


```text
// A 5 USDC transfer on Base
{
"address"  :   "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913"  ,
"topics"  : [
"0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"  ,
"0x0000000000000000000000007e2f5e1fd4d79ed41118fc6f59b53b575c51f182"  ,
"0x000000000000000000000000a6dbc393e2b1c30cff2fbc3930c3e4ddfc9d1373"
],
"data"  :   "0x00000000000000000000000000000000000000000000000000000000004c4b40"  ,
"transactionIndex"  :   "0x34"  ,
"logIndex"  :   "0x15d"  ,
}
```


How can we tell that this is a Transfer? Each log's` topics\[0\]` is the keccak256 hash of the event signature- in this case,` 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef` is the hash of` Transfer(address indexed from, address indexed to, uint256 value)` - the standard ERC-20 transfer event. Parameters marked as` indexed` are stored in the topics array in declaration order, following the signature hash. For this Transfer event,` from` is in` topics\[1\]` and` to` in` topics\[2\]` . Non-indexed parameters like` value` are ABI-encoded in` data` .


Extracting the transfer details from this log:


- **from** :` topics\[1\]` (32 bytes, zero-padded) →` 0x7e2f5e1fd4d79ed41118fc6f59b53b575c51f182`
- **to** :` topics\[2\]` (32 bytes, zero-padded) →` 0xa6dbc393e2b1c30cff2fbc3930c3e4ddfc9d1373`
- **value** :` data` decoded as uint256 →` 0x4c4b40` = 5,000,000 (5 USDC, since USDC has 6 decimals).


The mental overhead of knowing how to parse every type of log seems like a nightmare- which is why we’ve built out log processors that know how to parse specific event types and transform them into domain state:


The processor ingests the raw log data, parses it into typed fields (instead of hex strings), and produces domain entities.


```text
const   transferLogProcessor   =   createLogProcessor  ({
abi:   'event Transfer(address indexed from, address indexed to, uint256 value)'  ,
requires  : ({   log  ,   chain   })   =>   ({
token: TokensDependency.  args  ({ chain, contracts: [log.address] }),
}),
process  : ({   log  ,   tx  ,   parsed  ,   deps   })   =>   {
return   {
erc20Transfers: [
{
from: parsed.from,
to: parsed.to,
amount: parsed.value,
contract: log.address,
decimals: deps.token.decimals,
txHash: tx.hash,
},
],
};
},
});
```


**Inclusion vs. Confirmation**


We handle two distinct lifecycle stages:


1. **Inclusion -** Transaction first appears in a block. State is tentative- the block could still be reorged away.
2. **Confirmation -** Block reaches sufficient depth (enough subsequent blocks to eliminate the possibility of a reorg). State is final.


This distinction matters. We might update the UI to show a pending status at inclusion, but we wouldn’t trigger downstream FoF flows until confirmation. The cost of acting on tentative state is unbounded.


Log processors handle individual events, but we often need to coordinate across them or add transaction-level state. Transaction processors wrap this up: they receive the merged output from all log processors and can transform it, add to it, or trigger additional downstream effects. This is also where the two-phase lifecycle shows up.` processTransaction` runs at inclusion- we produce tentative state.` processConfirmation` runs once the block is final- this is typically where we complete lifecycles for financial operations.


```text
const   processor   =   createBlockchainTransactionProcessor  ()
.  requires  (({   transaction   })   =>   ({
// Transaction-level dependencies
}))
.  processTransaction  (  async   ({   logResults   })   =>   {
const   transfers   =   logResults.erc20Transfers   ??   [];


// ...


return   {
// ...
erc20Transfers:
transfers.  map  ((  t  )   =>   ({ type:   'insert'  , entity: t })),
};
})
.  processConfirmation  (  async   ({   logResults   })   =>   {
// Runs when block reaches finality


const   flowOfFundsRulesToTrigger   =   await   Promise  .  all  ([
findFlowOfFundsRules  ({
for: logResults.erc20Transfers.  map  ((  t  )   =>   t.fromAddress),
events: [  'on_chain_transfer.source.confirmed'  ],
}),
findFlowOfFundsRules  ({
for: logResults.erc20Transfers.  map  ((  t  )   =>   t.toAddress),
events: [  'on_chain_transfer.destination.confirmed'  ],
}),
]);


// ...


return   {
// ...
flowOfFundsRulesToTrigger: flowOfFundsRulesToTrigger.  flat  (),
erc20Transfers: logResults.erc20Transfers.  map  ((  t  )   =>   ({
type:   'update'  ,
entity: t,
changes: {
status:   'confirmed'  ,
},
})),
};
})
.  build  ();
```


### Connecting Logs Back to Calls


When our log processor produces a transfer record, it needs to link back to the originating BlockchainCall. The log tells us *what* happened—assets moved from A to B. The BlockchainCall tells us *why* —this was a fee collection, or a payment to a vendor, or a refund. For simple transactions with one call, this is straightforward. For batched transactions—where we bundle multiple operations into a single on-chain transaction to save gas—it gets harder. The receipt gives us a flat list of all logs emitted during execution, with no indication of which call produced which log. We solve this with call-frame tracing, which we cover in the advanced section below.


### Advanced: Attributing Batched Logs to Individual Calls


*This section covers a specific technical challenge with batched transactions. If you're not working with ERC-4337 or batched execution, feel free to skip to Global USD.* Earlier we mentioned that connecting logs back to their originating BlockchainCall is straightforward for simple transactions. For batched transactions, it's not.


### The Problem


When we batch multiple operations into a single transaction—say, a $500 payment plus a $1 fee—both execute atomically. The transaction receipt gives us a flat list of every log emitted during execution:


```text
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"result"  : {
"type"  :   "0x2"  ,
"status"  :   "0x1"  ,
"logs"  : [
{
"address"  :   "0x0000000071727de22e5e9d8baf0edac6f37da032"  ,
"topics"  : [   /* ... */   ],
"data"  :   "0x"  ,
"logIndex"  :   "0x3db"  ,
},
{
"address"  :   "0xcc7b940e22e1eef83c0170608aed6d5fd386cdad"  ,
"topics"  : [  "0xddf252ad..."  ,   /* ... */   ],
"data"  :   "0x000000000000000000000000000000000000000000000000000000001dcd6500"  ,
"logIndex"  :   "0x3dc"  ,
},
{
"address"  :   "0xcc7b940e22e1eef83c0170608aed6d5fd386cdad"  ,
"topics"  : [  "0xddf252ad..."  ,   /* ... */   ],
"data"  :   "0x000000000000000000000000000000000000000000000000000000000010c8e0"  ,
"logIndex"  :   "0x3dd"  ,
},
{
"address"  :   "0x0000000071727de22e5e9d8baf0edac6f37da032"  ,
"topics"  : [  "0x49628fd1..."  ,   /* ... */  ],
"data"  :   "0x000000000000000000000000000000000000000000000001000000000000000400000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000009206e5fb300000000000000000000000000000000000000000000000000000000000036f83"  ,
"logIndex"  :   "0x3de"  ,
}
],
"logsBloom"  :   "0x..."  ,
"transactionHash"  :   "0x95aaaa96f3ccefbfcedb1fe5e41d817bc663fcac305ebfc3861f2a189f664cda"  ,
"transactionIndex"  :   "0x18e"  ,
"blockHash"  :   "0xfdc6c014e387c5b6f95edc17a25e81f694a5ad46fc75a247aeb0ad3f5dcd8004"  ,
"blockNumber"  :   "0x25b1de6"  ,
"gasUsed"  :   "0x28c6d"  ,
"effectiveGasPrice"  :   "0x1b3ed0"  ,
"blobGasUsed"  :   "0x24078"  ,
"from"  :   "0x8dc6004dcbfdceeff98f16e7d58b0fde5fab1412"  ,
"to"  :   "0x0000000071727de22e5e9d8baf0edac6f37da032"  ,
}
}
```


We get a flat array of every log emitted during execution. Looking at this receipt, we can identify two Transfer events at log indices 1 and 2 (both sharing the` 0xddf252ad...` transfer event signature we discussed earlier).


But which one was the payment and which was the fee? The receipt doesn't tell us—logs are attributed to the top-level transaction, not to individual calls within a batch. You might think: just match logs to calls in order. But that only works if each call emits exactly one log. A simple transfer emits one; a swap might emit five. Without knowing the boundaries, you can't map them reliably.


### Call Frame Tracing


The solution turned out to be` debug_traceTransaction` - a Geth archive node RPC method that most people use for debugging failed transactions. But it does something else: it replays the transaction and returns the complete call frame tree, with logs attached at the correct depth in the call hierarchy.


```text
{
"jsonrpc"  :   "2.0"  ,
"method"  :   "debug_traceTransaction"  ,
"params"  : [
"0x..."  ,
{
"tracer"  :   "callTracer"  ,
"tracerConfig"  : {
"withLog"  :   true
}
}
],
"id"  :   1
}
```


The result is a recursively nested structure of call frames (simplified for readibility)


```text
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"result"  : {
"from"  :   "0x8dc6004dcbfdceeff98f16e7d58b0fde5fab1412"  ,
"to"  :   "0x0000000071727de22e5e9d8baf0edac6f37da032"  ,
"input"  :   "0x765e827f..."  ,
"calls"  : [
{   /* ... */   },
{   /* ... */   },
{
"from"  :   "0x0000000071727de22e5e9d8baf0edac6f37da032"  ,
"to"  :   "0x0000000071727de22e5e9d8baf0edac6f37da032"  ,
"input"  :   "0x0042dc53..."  ,
"calls"  : [
{
// ...
"calls"  : [
{
"from"  :   "0xcc7b940e22e1eef83c0170608aed6d5fd386cdad"  ,
"to"  :   "0xe1452ff880739d26f341c2510222e808def38d10"  ,
"input"  :   "0xa9059cbb..."  ,
"logs"  : [
{
"address"  :   "0xcc7b940e22e1eef83c0170608aed6d5fd386cdad"  ,
"topics"  : [   /* ... */   ],
"data"  :   "0x..."  ,
"position"  :   "0x0"  ,
"index"  :   "0x1"
}
],
"value"  :   "0x0"  ,
"type"  :   "DELEGATECALL"
}
]
},
{
// ...
"calls"  : [
{
"from"  :   "0xcc7b940e22e1eef83c0170608aed6d5fd386cdad"  ,
"to"  :   "0xe1452ff880739d26f341c2510222e808def38d10"  ,
"input"  :   "0xa9059cbb..."  ,
"logs"  : [
{
"address"  :   "0xcc7b940e22e1eef83c0170608aed6d5fd386cdad"  ,
"topics"  : [   /* ... */   ],
"data"  :   "0x..."  ,
"position"  :   "0x0"  ,
"index"  :   "0x2"
}
],
"value"  :   "0x0"  ,
"type"  :   "DELEGATECALL"
}
]
}
]
}
],
"logs"  : [],
"value"  :   "0x0"  ,
"type"  :   "CALL"
}
}
```


We flatten this recursive structure into a schema that preserves the tree relationships:


```text
export   type   EvmCallType   =
|   (  'CALL'   |   'DELEGATECALL'   |   'STATICCALL'   |   'CREATE'   |   'CREATE2'  )
|   (  string   &   {});


/**
* Represents a single frame in an EVM transaction's call stack trace.
*
* Each call frame captures the execution of one contract call, including:
* - Its position in the call tree (parent, depth, index)
* - Call details (type, from, to, value, input, output)
* - Gas metrics (gas available, gas used)
*
* Call frames form a tree structure via the parentId relationship, where:
* - Root frame (depth 0): The initial transaction call
* - Child frames: Calls made by the parent contract (CALL, DELEGATECALL, etc.)
*/
export   interface   IEvmCallFrameSchema   {
id  :   Id  <  'evmCallFrame'  >;
transactionHash  :   string  ;
parentId  :   Id  <  'evmCallFrame'  >   |   undefined  ;
depth  :   number  ;
index  :   number  ;
type  :   EvmCallType  ;
from  :   `0x${  string  }`  ;
to  :   `0x${  string  }`   |   undefined  ;
value  :   string  ;
input  :   `0x${  string  }`  ;
output  :   `0x${  string  }`   |   undefined  ;
gas  :   string   |   undefined  ;
gasUsed  :   string   |   undefined  ;
logs  :   {
address  :   `0x${  string  }`  ;
topics  :   `0x${  string  }`  [];
data  :   `0x${  string  }`  ;
position  :   number  ;
index  :   number  ;
}[];
}
```


```text
const   root   =   debugTraceTransactionResp.result;


const   recursivelyBuildSchemas   =   (
frame  :   typeof   root,
index  :   number  ,
parent  ?:   InsertSchema  <  IEvmCallFrameSchema  >
)  :   InsertSchema  <  IEvmCallFrameSchema  >[]   =>   {
const   schema  :   InsertSchema  <  IEvmCallFrameSchema  >   =   {
id:   generateId  (  'evmCallFrame'  ),
transactionHash,
parentId: parent?.id,
depth: (parent?.depth   ??   -  1  )   +   1  ,
index,
type: frame.type,
from: frame.from,
to: frame.to,
value: (frame.value   ?   BigInt  (frame.value)   :   BigInt  (  0  )).  toString  (),
input: frame.input,
output: frame.output,
gas: frame.gas   ?   BigInt  (frame.gas).  toString  ()   :   undefined  ,
gasUsed: frame.gasUsed   ?   BigInt  (frame.gasUsed).  toString  ()   :   undefined  ,
logs: (frame.logs   ??   []).  map  ((  log  )   =>   ({
...  log,
position:   Number  (log.position),
index:   Number  (log.index),
})),
};


return   [
schema,
...  (frame.calls   ??   []).  flatMap  ((  call  ,   index  )   =>
recursivelyBuildSchemas  (call, index, schema)
),
];
};


const   schemas   =   recursivelyBuildSchemas  (root,   0  );


const   trace  :   InsertSchema  <  IEvmTransactionTraceSchema  >   =   {
transactionHash,
chain,
rootFrameId: schemas[  0  ].id,
};


await   persistTransactionTrace  ({
trace,
callFrames: schemas,
});
```


Consider a batched UserOp with two USDC transfers: a $500 payment and a $1.10 fee, represented by the execution trace above. The trace gives us:


```text
EntryPoint.handleOps  ()
...
└──   SmartWallet.executeBatch  ()
├──   Call   0:   USDC.transfer  (  500.00   USDC   to   0x...  )
│     └──   Transfer  (  from,   to,   500000000  )
├──   Call   1:   USDC.transfer  (  1.10   USDC   to   Slash  )
│     └──   Transfer  (  from,   to,   1100000  )
└──   ...
```


The entire transaction can now be represented as a tree. This reframes the entire problem: instead of inferring structure from a flat log array, we reconstruct the execution tree- where the call hierarchy is explicit and logs are attached to the frames that emitted them.


From there, attribution is straightforward. Find the node corresponding to the` executeBatch()` call, iterate through its children at indices` 0..N-1` , and recursively collect logs from each subtree. Each child index` 0..N-1` maps directly to its corresponding BlockchainCall` indexInBatch` . We now know exactly which call produced which logs.


Since virtually every transaction needs this attribution, we built it directly into our log processor. It reconstructs the full call tree, matches logs to their originating frames, and resolves all BlockchainCalls in the batch. Each log processor then receives the specific call and frame context for the log it's handling:


```text
const   transferLogProcessor   =   createLogProcessor  ({
requires  : ({   call  ,   callFrame   })   =>   ({
// ...
}),
process  : ({   call  ,   callFrame   })   =>   {


return   {
// ...
};
},
});
```


The full attribution chain:


```text
```
Transfer   log
→   EvmCallFrame (index   0   under executeBatch)
→   BlockchainCall (indexInBatch:   0  )
→   type: 'erc20.transfer'
→   category: 'outbound_ach_credit'
→   context: { recipientName: 'Acme Corp' }
```
```


## Global USD: The Capstone


Off-ramps and on-ramps solved a problem for our existing customers—businesses with US bank accounts who wanted to move between fiat and crypto. But we kept hearing from a different segment: international businesses that need access to US dollar rails but can't easily get them.


If you're a software contractor in Argentina, an e-commerce merchant in Nigeria, or a SaaS company in Southeast Asia, opening a US bank account often requires US entity formation—lawyers, registered agents, and months of overhead. Many legitimate businesses are effectively locked out of the dollar economy, not because of anything they've done, but because of where they're incorporated.


Stablecoins change this. A USDC balance is a dollar balance. Global USD is our attempt to build banking infrastructure on top of that premise.


### Non-Custodial by Design


We built Global USD as a non-custodial system. The decision was driven by two factors: regulatory complexity and trust.


Holding customer funds introduces licensing requirements that vary by jurisdiction. A non-custodial architecture simplifies our licensing posture in many of these markets. On the trust side, customers control their own keys—by design, Slash cannot initiate transfers without cryptographic authorization from the account signers.


The core primitive is the **smart wallet** : a smart contract that acts as a wallet but with programmable access control.


Each Global USD account is a smart wallet governed by a multi-sig. Every authorized member of the business holds a key. Transfers require their approval before executing. Slash can *prepare* a transaction, but we cannot *execute* it without signer authorization.


### Signing Without Custody


This raises a UX question: if users control keys, don't they need to manage seed phrases and sign transactions manually?


We use embedded wallet infrastructure from Privy and Alchemy. When a user creates an account, a private key is generated inside hardware-isolated memory (a “trusted execution environment”, or TEE). The key exists, but it's designed to be inaccessible to Slash or anyone else directly. When a user initiates a transfer, they approve via OTP, which authorizes the TEE to sign on their behalf. The signed transaction is then submitted to the network.


From the user's perspective, it feels like approving a bank transfer. From a custody perspective, we never touch private keys.


### What This Unlocks


A business in Lagos can now hold dollars, receive payments from US clients, and pay international vendors—all without a US bank account, without custody risk, and with the same audit trail and compliance workflows we'd apply to any Slash customer.


That's what stablecoins can actually be: not just a payment method, but foundational infrastructure for a more accessible financial system.


## What's Next


The primitives we've built aren't just for moving money between fiat and crypto. They're the foundation for everything we're building at Slash. We're expanding our global account offerings—giving more businesses access to USD rails regardless of where they're incorporated. And we're building out our global card: a high-cashback, stablecoin-backed card that lets customers spend their balances anywhere. Both rely heavily on the same orchestration and execution frameworks we've described here. If you made it this far and you're an engineer who wants to solve hard infrastructure problems for real customers at a company that's growing fast, we're hiring.


## Read more from us


### [One Engineer, Three Months, Zero Compromises](https://www.slash.com/engineering/blog/new-mobile-app)


≈ 17 minutes read


### [Building Card Transaction Processing at Slash](https://www.slash.com/engineering/blog/card-transaction-processing)


≈ 12 minutes read


### [My Internship with Slash](https://www.slash.com/engineering/blog/intern-blog)


≈ 13 minutes read


### [The Facelift](https://www.slash.com/engineering/blog/facelift)


≈ 15 minutes read


### [Winning & Culture](https://www.slash.com/engineering/blog/winning-and-culture)


≈ 4 minutes read


### [Scaling 1M lines of TypeScript: Registries](https://www.slash.com/engineering/blog/scaling-1m-lines-of-typescript-registries)


≈ 5 minutes read


### [Health Checks](https://www.slash.com/engineering/blog/health-checks)


≈ 7 minutes read


### [Type challenge: Datadog Logs Preview](https://www.slash.com/engineering/puzzles/datadog-preview-type-challenge)


≈ 2 minutes read
