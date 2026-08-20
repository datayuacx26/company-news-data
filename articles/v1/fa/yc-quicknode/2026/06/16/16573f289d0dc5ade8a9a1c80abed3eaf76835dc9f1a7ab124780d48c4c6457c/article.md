---
schema_version: "1.0.0"
document_id: "16573f289d0dc5ade8a9a1c80abed3eaf76835dc9f1a7ab124780d48c4c6457c"
company_key: "yc-quicknode"
company: "Quicknode"
source_id: "yc-quicknode-news-import-d0b2f3935987"
canonical_url: "https://www.quicknode.com/blog/introducing-shredtransactionsubscribe"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T20:23:53.195520+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:4a591ce11d0c681ab5ed78d3f3bd3a4499e77b97903df83a5a3753181399d0df"
---

# shredTransactionSubscribe: Solana Pre-Execution Data | Quicknode

#


Introducing shredTransactionSubscribe: Solana Transaction Visibility Before Execution


Blazar's shredTransactionSubscribe brings Solana transaction visibility before execution as a WebSocket JSON-RPC subscription. Learn the filters, notification shape, and example requests.


[Quicknode](https://www.quicknode.com/blog/author/quicknode)


June 30, 2026 — 9 min read


Solana moves fast because blocks do not travel through the network as single monolithic objects. Validators break block data into smaller pieces called shreds and propagate them through the network before the block has been fully replayed and before normal RPC subscriptions can expose execution metadata.


Blazar, Quicknode's Solana WebSocket engine, now includes[shredTransactionSubscribe](http://quicknode.com/docs/solana/shredTransactionSubscribe) to turn that early signal into a developer-facing WebSocket method. It subscribes to transactions observed from shred/entry ingestion before execution.


*shredTransactionSubscribe is in beta, and what is described in this post may change.*


## What Is a Shred?


A shred is a fixed-size piece of a Solana block transmitted across the network while the block is being produced. Consuming shreds lets you observe transaction data as it's transmitted, before the finalized block becomes queryable through post-execution RPC paths.


Those shreds can be reconstructed into entries. Entries contain Solana` VersionedTransaction` values: signatures, messages, static account keys, instructions, recent blockhashes, references to Address Lookup Tables for v0 transactions.


The key distinction is timing:


-


Shred/entry ingestion happens before replay/execution.


-


Normal transaction metadata is available only after execution.


Shred-time transaction data can be earlier than execution data, but it cannot include results such as meta, err, logs, token balances, compute units, or inner instructions.


## How Blazar Obtains and Normalizes Data


The raw shred data path has two gaps most developers cannot easily fill.


First, a raw SubscribeEntries stream requires gRPC, bincode decoding, transaction parsing, Address Lookup Table resolution, and a clear understanding of what is and is not known before execution. Most application developers want a WebSocket JSON-RPC subscription, not that infrastructure.


Second, account filtering needs ALT awareness. Most Solana v0 transactions use Address Lookup Tables. Without local ALT resolution, a transaction that touches an account only through a lookup table can be missed by account-based filters. Blazar resolves static keys plus loaded ALT addresses for account filtering.


Blazar receives an early Solana transaction feed from shred-streaming infrastructure and turns that low-level network signal into a standard WebSocket JSON-RPC subscription experience. Blazar can observe transactions before normal post-execution RPC paths expose them, then emit a structured` shredTransactionNotification` with the transaction body, slot, signature, version, and resolved loaded addresses when available.


Treat the stream as an early, best-effort signal: notifications arrive in near-upstream order but without a strict ordering guarantee, and the same transaction can occasionally be observed more than once. Dedupe on signature if your use case needs exactly-once.


Blazar also enhances the shred-derived transaction data before delivery. Instead of forwarding a raw early signal and leaving every client to do the same enrichment work, Blazar normalizes the transaction shape and resolves Address Lookup Table loaded addresses when possible. That gives subscribers a more complete pre-execution account view without operating specialized Solana networking infrastructure or building their own shred parsing stack.


## Shreds on the Solana Transaction Timeline


Shred-time transaction visibility creates a new point on the Solana data timeline:


Terminal


```text
Transaction broadcast -> shreds observed -> entries decoded -> replay/execution -> processed/confirmed/finalized RPC data
```


Most Solana WebSocket methods operate after execution or around validator state transitions.` shredTransactionSubscribe` exposes the earlier entry-decoding point.


The result is not a replacement for` transactionSubscribe` . It is an earlier, pre-execution companion for workflows that benefit from seeing transaction intent as soon as possible.


## Example Use Cases


shredTransactionSubscribe supports use cases that post-execution methods cannot cover and improves on others by delivering transaction data before execution.


### Early Transaction Intent


Trading systems, DEX analytics, routing engines, and market infrastructure can observe transactions before execution. This identifies account pressure, program activity, or incoming order flow earlier than standard post-execution streams.


### Account-Aware Pre-Execution Monitoring


A client can subscribe to transactions that touch specific accounts, including accounts loaded through Address Lookup Tables. Use it to monitor hot accounts, pool accounts, token accounts, vaults, or protocol-specific state accounts.


### Signature-Level Lifecycle Tracking


Paired with` signatureSubscribe` and` enableReceivedNotification: true` , a client can observe when a signature was first seen pre-execution and later observe whether it landed successfully or failed.


### Operational Diagnostics


Support and infrastructure teams can answer questions such as: Was the transaction observed before execution? Did it later land? Was it never seen? Did it rely on lookup-table addresses? This enables better transaction lifecycle diagnostics.


### Vote And Slot Signals


Blazar also uses the same shred/entry ingestion path for pre-execution vote notifications and new-slot signals. shredTransactionSubscribe is the transaction-specific surface built on top of that same early data source.


## Method Overview


shredTransactionSubscribe subscribes to pre-execution transaction notifications from shred/entry ingestion. It is intentionally not a config variant of transactionSubscribe. The payload is different because the data stage is different.


It accepts one optional filter object as the first and only parameter.


Terminal


```text
{
"jsonrpc": "2.0",
"id": 1,
"method": "shredTransactionSubscribe",
"params": [
{
"vote": false,
"signature": "optional-signature",
"accounts": {
"include": ["optional-pubkey"],
"exclude": ["optional-pubkey"],
"required": ["optional-pubkey"]
}
}
]
}
```


All fields are optional. Each accounts list (include, exclude, required) accepts up to 50,000 entries.


## Supported Filters


### ` vote`


-


true: only simple vote transactions.


-


false: exclude simple vote transactions.


-


omitted: include both vote and non-vote transactions.


The vote filter matches Agave's simple-vote transaction classification. It does not mean "any transaction that contains a Vote program instruction somewhere." Mixed transactions with a vote instruction are not classified as simple vote transactions. voteSubscribe is a different method with its own gossip-vote parsing behavior.


### ` signature`


-


Delivers only the transaction with the given signature.


### ` accounts.include`


-


Delivers transactions that touch at least one listed account.


-


Matches static account keys plus resolved ALT-loaded addresses.


### ` accounts.exclude`


-


Drops transactions that touch any listed account.


-


Matches static account keys plus resolved ALT-loaded addresses.


### ` accounts.required`


-


Delivers only transactions that touch all listed accounts.


-


Matches static account keys plus resolved ALT-loaded addresses.


## Unsupported Parameters


commitment, encoding, transactionDetails, showRewards, maxSupportedTransactionVersion, and failed are not supported because shred-derived transactions are pre-execution JSON payloads with no execution outcomes or commitment levels.


## Example Requests


### Subscribe To All Pre-Execution Non-Vote Transactions


Terminal


```text
{
"jsonrpc": "2.0",
"id": 11,
"method": "shredTransactionSubscribe",
"params": [
{ "vote": false }
]
}
```


### Subscribe To A Specific Signature


Terminal


```text
{
"jsonrpc": "2.0",
"id": 12,
"method": "shredTransactionSubscribe",
"params": [
{
"signature": "5VERv8NMvzbJMEkV8xnqLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW"
}
]
}
```


### Subscribe To Transactions Touching An Account


Terminal


```text
{
"jsonrpc": "2.0",
"id": 13,
"method": "shredTransactionSubscribe",
"params": [
{
"accounts": {
"include": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"]
}
}
]
}
```


### Require Multiple Accounts And Exclude Another


Terminal


```text
{
"jsonrpc": "2.0",
"id": 14,
"method": "shredTransactionSubscribe",
"params": [
{
"accounts": {
"required": [
"AccountA11111111111111111111111111111111111",
"AccountB22222222222222222222222222222222222"
],
"exclude": [
"AccountC33333333333333333333333333333333333"
]
}
}
]
}
```


## Subscription Response


On success, the server returns a subscription ID. Use this ID to unsubscribe.


Terminal


```text
{
"jsonrpc": "2.0",
"result": 123,
"id": 14
}
```


## Unsubscribe


Terminal


```text
{
"jsonrpc": "2.0",
"id": 15,
"method": "shredTransactionUnsubscribe",
"params": [123]
}
```


## Notification Shape


A notification uses the standard JSON-RPC PubSub envelope, but the inner transaction value is pre-execution data.


Terminal


```text
{
"jsonrpc": "2.0",
"method": "shredTransactionNotification",
"params": {
"result": {
"context": { "slot": 418437508 },
"value": {
"slot": 418437508,
"signature": "5VERv8NMvzbJMEkV8xnqLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW",
"transaction": {
"signatures": [
"5VERv8NMvzbJMEkV8xnqLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW"
],
"message": {
"header": {
"numRequiredSignatures": 1,
"numReadonlySignedAccounts": 0,
"numReadonlyUnsignedAccounts": 2
},
"accountKeys": [
"FeePayer11111111111111111111111111111111111",
"Program111111111111111111111111111111111111"
],
"recentBlockhash": "Eit7...",
"instructions": [
{
"programIdIndex": 1,
"accounts": [0],
"data": "3Bxs4NN8M2Yn4TLb",
"stackHeight": 1
}
]
}
},
"version": "legacy"
}
},
"subscription": 123
}
}
```


## Versioned Transaction Example With Loaded Addresses


For v0 transactions, transaction.message.addressTableLookups preserves the lookup-table references, and value.loadedAddresses carries the resolved pubkeys.


Terminal


```text
{
"jsonrpc": "2.0",
"method": "shredTransactionNotification",
"params": {
"result": {
"context": { "slot": 418437509 },
"value": {
"slot": 418437509,
"signature": "7rYg...",
"transaction": {
"signatures": ["7rYg..."],
"message": {
"header": {
"numRequiredSignatures": 1,
"numReadonlySignedAccounts": 0,
"numReadonlyUnsignedAccounts": 1
},
"accountKeys": [
"FeePayer11111111111111111111111111111111111",
"Program111111111111111111111111111111111111"
],
"recentBlockhash": "AbCd...",
"instructions": [
{
"programIdIndex": 1,
"accounts": [0, 2, 3],
"data": "3Bxs4NN8M2Yn4TLb",
"stackHeight": 1
}
],
"addressTableLookups": [
{
"accountKey": "LookupTab1e11111111111111111111111111111111",
"writableIndexes": [12],
"readonlyIndexes": [3]
}
]
}
},
"version": 0,
"loadedAddresses": {
"writable": ["Writab1eLoaded11111111111111111111111111111"],
"readonly": ["Readon1yLoaded11111111111111111111111111111"]
}
}
},
"subscription": 124
}
}
```


If Blazar cannot resolve one or more lookup tables from cache, v0 notifications set:


Terminal


```text
"loadedAddresses": null
```


Even with startup warmup and live Solana gRPC updates, a just-extended table can race the cache. In that case, Blazar keeps the notification honest by setting loadedAddresses: null for v0 transactions whose loaded addresses are incomplete.


Legacy transactions omit loadedAddresses.


## What The Payload Does Not Include


Because this is pre-execution data, the notification does not include:


-


meta


-


err


-


logs


-


inner instructions


-


pre/post balances


-


pre/post token balances


-


compute units consumed


-


return data


-


rewards


-


final transaction index in the block


shredTransactionSubscribe is an early observation method, not an execution result method.


## Getting Started


1.


Log in to your[Quicknode dashboard](https://dashboard.quicknode.com/) .


2.


Select your[Solana endpoint](https://www.quicknode.com/chains/sol) .


3.


Copy your WSS endpoint URL (wss://your-endpoint-name.solana-mainnet.quiknode.pro/abc123) and connect using your existing WebSocket client code.


4.


See the Example Requests above to send a shredTransactionSubscribe request with an optional filter.


5.


Process shredTransactionNotification messages as they arrive.


New to Solana WebSockets? The[How to Create Solana WebSocket Subscriptions guide](https://www.quicknode.com/guides/solana-development/getting-started/how-to-create-websocket-subscriptions-to-solana-blockchain-using-typescript) covers everything you need.


For teams that need lower-latency Solana streaming beyond WebSockets, Quicknode's[Solana gRPC](https://www.quicknode.com/solana-grpc) product provides Yellowstone-compatible real-time streams from the same endpoint ecosystem.


## FAQs


### Is shredTransactionSubscribe the same as transactionSubscribe with an earlier commitment?


No. transactionSubscribe is post-execution and carries transaction status metadata. shredTransactionSubscribe is pre-execution and does not have logs, meta, err, balances, or compute units.


### Can a shred transaction fail later?


Yes. A transaction observed from shreds may later fail execution, expire, or not appear in the final ledger. This method is an early observation signal, not a finality signal.


### Do account filters include Address Lookup Table accounts?


Yes. Account filters match static account keys plus resolved ALT-loaded addresses from Blazar's local lookup-table cache.


### What happens when an ALT is deactivated?


A deactivated ALT can remain usable during its SlotHashes cooldown window. Blazar resolves deactivated tables during that valid cooldown period and treats clearly expired deactivated tables as unresolved.


### Who should use this method?


Use it when early transaction intent matters more than execution outcome: low-latency analytics, account activity monitoring, transaction lifecycle diagnostics, trading infrastructure, and pre-execution observability.


### Who should not use this method?


Do not use it if you need final transaction status, logs, balances, token balance changes, compute units, rewards, or finalized block membership. Use post-execution RPC/WebSocket methods for that.


## The Bottom Line


Pre-execution transaction data has historically required custom Solana networking infrastructure: gRPC clients, bincode decoding, and shred parsing pipelines. Blazar's shredTransactionSubscribe removes that requirement and delivers transaction intent before execution as a standard WebSocket JSON-RPC subscription.


It is available on every Quicknode Blazar WebSocket endpoint today. Log in to your[Quicknode dashboard](https://dashboard.quicknode.com/) to get started, or[create a Quicknode account](https://www.quicknode.com/signup) if you do not have one yet.


Since shredTransactionSubscribe is currently in beta, we invite you to share any feedback or suggestions you have. Please email us atsolana@quicknode.com or reach out to us on[Discord](https://discord.gg/quicknode) .


### About Quicknode


Founded in 2017, Quicknode deploys institutional-grade blockchain infrastructure for developers and enterprises. With 99.99% uptime and support for 80+ chains, teams build and scale onchain applications without compromise.


[Start building today](https://www.quicknode.com/)
