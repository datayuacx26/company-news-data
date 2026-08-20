---
schema_version: "1.0.0"
document_id: "250a7e8ba6e9e431a7efb07d7fecad9ab2644d42e009acee8d1410f6cff84021"
company_key: "yc-stackup"
company: "Stackup"
source_id: "yc-stackup-news-import-4adfae6a34ed"
canonical_url: "https://www.stackup.fi/resources/optimising-eth-senduseroperation-with-native-tracers"
published_at: null
first_seen_at: "2026-07-27T05:25:45.190590+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:7994a8c5946fae180fbdf8159409b5b485a78523c68aba688ffdf1481d82f65c"
---

# Optimising eth_sendUserOperation with native tracers

The biggest performance bottleneck to submitting a UserOperation is the work required for a bundler to verify the validation phase of a transaction. With the release of ERC-7562, the list of rules that a compliant bundler must check for when accepting a new UserOperation is long.


These rules cover everything from the context in which opcodes are used, how storage is accessed, and how various entities are handled based on their reputation. Each rule may also have multiple variations to consider. At the time of writing, the[compliance test suite](https://github.com/eth-infinitism/bundler-spec-tests) for bundlers already has more than 100 cases that require validation of opcode and storage. Understanding the rationale of each rule is best left for another series of articles. But we can start to understand the issue regarding performance. **When someone sends a transaction via eth_sendUserOperation, the bundler has to synchronously check all these rules before responding with a UserOp hash or error.**


## Enter debug_traceCall


A majority of these rules cannot be verified by just inspecting the UserOperation or running a quick simulation with eth_call. To check for the exact access patterns of things like opcodes and storage, we require that the bundler collect information at every step of a UserOperation's validation phase. To do this we need to trace the simulation with debug_traceCall.


The debug_traceCall method comes with a number of[built-in tracers](https://geth.ethereum.org/docs/developers/evm-tracing/built-in-tracers) for common use cases. However none of these can be applied entirely for bundlers. This is why all compliant standalone bundlers have to rely on a[custom EVM tracer](https://geth.ethereum.org/docs/developers/evm-tracing/custom-tracer) that is purpose built to collect the information needed to verify a UserOperation. We call this the "bundlerCollectorTracer" and most bundlers are currently reliant on a[JavaScript implementation](https://github.com/stackup-wallet/stackup-bundler/blob/main/pkg/tracer/BundlerCollectorTracer.js) .


## The pros and cons of JS tracing


The first type of custom tracer can be written entirely in JS and passed as an option to debug_traceCall. The big advantage here is that any standard Ethereum node can handle it without customisation assuming its debug APIs are available. This in turn allows developers to have a much faster iteration cycle. Although convenient, JS tracers are not performant enough for more intensive tracing. This includes our bundler use case.


To see why, we should look at the *step* function of bundlerCollectorTracer.js. This function is called at each step of the EVM as the UserOperation is being simulated. On a high level it has to do a few different things from checking for out of gas conditions to collection information on storage access, opcode use, and contract interactions. The data collected can also be very specific, such as only counting the use of *GAS* opcodes if it's not immediately followed by a *CALL* or counting precise opcode patterns like " ** EXTCODESIZE ISZERO* ". But opcode access is not the only data we need to collect. Additionally, we also need slot information from all *SLOADs* and *SSTOREs* plus all *KECCAK* inputs which is used to detect associated storage access patterns. These are just a few examples of specific things collected by the tracer. All these seemingly disparate data points are passed back to the bundler to help it deterministically decide wether a UserOperation is safe to be included in the mempool or not.


For accounts with relatively straightforward validation this is not an issue. The[SimpleAccount](https://github.com/eth-infinitism/account-abstraction/blob/develop/contracts/accounts/SimpleAccount.sol) implementation, which relies on an ECDSA signature, won't have a large number of opcodes to step through. On the other hand, accounts with more complex validation techniques will. For example, accounts with WebAuthn will typically require Sepc256r1 signatures and base64 encoding. **This results in an outsized number of opcodes to step through and can lead to hitting the 5 second timeout when using the JS tracer.**


## Porting the tracer from JS to Go


The most straightforward way to solve this timeout issue is to directly port the bundlerCollectorTracer from its JS implementation to native Go. A native tracer is significantly more performant than its JS counterpart. However it comes with a more complex deployment process. Unlike JS tracers, a native tracer cannot be passed as an option to debug_traceCall. Instead we have to build Geth from source along with the additional Go tracer file. This means that you need to not only self-host the bundler, but the underlying node too. On top of that, your build environment must be setup to compile Geth (or the relevant execution client) from source.


The upside of dealing with the additional deployment overhead is significantly faster validation. Re-compiling Geth with the following bundler_collector.go tracer results in a **~40% faster execution** of the bundler spec test for storage rules when run in a local devnet.


A comparison of JS vs Native tracer on bundler spec test


Native tracing also translates to significantly lower latency of eth_sendUserOperation in production with much less variability. From the graph below, we can observer that JS tracing results in a baseline P95 latency of 100ms and spikes quiet often. The intensity of the spike tends to correlate with network activity and the account implementation used. After making the switch to native tracing, we can see a clear decrease in the baseline from 100ms down to 50ms, a **~50% faster response time** . In addition, P95 values are also much more stable over time with less pronounced spikes.


P95 latency of eth_sendUserOperation before and after native tracing


## Streamlining the deployment process


At Stackup, all clusters of bundlers and nodes are provisioned and maintained using automated CD pipelines. Prior to native tracing it was sufficient for us to use the official pre-built packages for all execution clients. But as mentioned above, this is not feasible for native tracing and we needed to figure out a way to compile each network's execution client from source with the relevant Go tracer. Since[stackup-bundler](https://github.com/stackup-wallet/stackup-bundler) is written in Go, our build system was already setup to compile these clients from source. So there wasn't any issue there.


The main complexity is building an automated process for cloning the execution client repository at a specific stable release, copying the Go tracer to the correct directory, and compiling the final package. This process must also be repeatable within a CD pipeline and can account for multiple variations for each network that is supported.


Our current solution has been consolidated to an open source repository at[erc-4337-execution-clients](https://github.com/stackup-wallet/erc-4337-execution-clients) . We encourage anyone who is self hosting a bundler to check it out for much faster tracing performance!


## Wrapping up


At Stackup, we are all in on native tracing and plan to maintain the Go bundlerCollectorTracer by running it in production for all our fully managed instances. As the number of UserOperations scale up, it is likely that more entities will require advance validation logic that isn't just native ECDSA. At such a scale this becomes a clear performance bottleneck for bundlers with JS tracing to securely serve a growing mempool of UserOperations.
