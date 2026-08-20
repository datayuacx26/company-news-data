---
schema_version: "1.0.0"
document_id: "3795f2687fd3f7935705431c144f66f8dceb36d0b6abe4f666d9ab62cad1b95c"
company_key: "yc-stackup"
company: "Stackup"
source_id: "yc-stackup-news-import-4adfae6a34ed"
canonical_url: "https://www.stackup.fi/resources/stackup-keystore"
published_at: "2025-09-23T00:00:00+00:00"
first_seen_at: "2026-07-27T05:25:45.190590+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:9956b74ade131acc964ab880e40e2e96099adc934c5fdee598126afd018aeecc"
---

# Scaling Access for Multi-Chain Smart Accounts

Smart accounts offer huge advantages over traditional externally owned accounts (EOAs), with one of them being true flexibility over authentication and authorization.


An EOA can only be authenticated with a` secp256k1` signature with no granular controls over account actions. On the other hand, a smart account is not technically restricted in its methods for determining the type of signers and what each signer can do. For instance, we have seen smart accounts authenticated with passkeys or multisig schemes while also implementing granular policies from spending limits to timed sessions.


The current account abstraction ecosystem has many production grade smart account implementations from Safe to Kernel, so why did we build another one?


At Stackup, we've spent the last four years building at every layer of the smart account stack from protocol and infrastructure to application. During this time we continuously ran into three distinct problems that we felt were not adequately solved.


## Problem 1: Awkward key rotations and cross chain deployments


It is a hard fact that accounts today don't just live on one chain but many. For an EOA with static and stateless verification, this is not an issue.


However, smart accounts are complete opposites. Verification can be dynamic and backed by some critical state. This inevitably leads to downstream issues with key rotations.


In the simplest case where a smart account is owned by one public key, this key will be hard coded in the initialization data during its onchain deployment.


Once deployed the owner is free to do a key rotation to a second key for whatever reason. The issue emerges when it becomes time to for the account to be deployed on a second chain.


Since the first public key was hard coded in the initialization data, it directly influences the account's deterministic address. Therefore, to deploy the same account on a new chain, the owner must always maintain access to the first public key. Only after the deployment can they initiate a key rotation to the second key.


This is the unfortunate meta of current smart accounts which has implications in both security and user experience. It breaks best practices in key rotations if a outdated key can still lead to potential loss on a subset of user funds. Additionally, it breaks our expectations that if I rotate my keys then that state should be the same regardless of the network.


## Problem 2: High gas cost and messy cross-chain sync


Of course, single-owned-accounts are not the only viable setup. Other schemes exist in the real world from N/M multi-sig thresholds or multiple individual signers with varying degrees of authorization.


Although powerful, these setups require state that grows with the number of signers and policies. Let's take the example of an account that can be owned by` N` number of users. The account then needs to store` N` public keys. This is not a gas friendly approach as storage costs grow linearly with` N` .


Also consider that storage is just for a single chain. Assuming a multi-chain account, we then have to replay this update on` M` chains that the account is active on.


‍


We could argue that gas cost on L2s are cheap and we only need to optimize on mainnet by restricting the storage size.


While this might be acceptable for some limiting use cases, there is also a burden on the user to sign a transaction for` M` chains in order to replay the update.


This is a bad experience that only gets worse as teams operate across more and more chains. We've seen that teams using these kinds of wallets avoid updating configuration across chains, leading to configuration differences and ultimately security vulnerabilities.


## Problem 3: No privacy by default


Even if gas is not an issue, we should also consider the trade-offs in privacy from storing validation state onchain.


Let's imagine our smart account has a recovery mechanism where we delegate a group of known entities (e.g. friends, family, or services) to assists us in an emergency recovery. For best practice in operational security, it would be highly recommended to not have this information public until the time it is required.


By storing the entire access configuration onchain, we inadvertently give up the option to have such state private by default.


## Solution: A singleton contract for storing access configuration


We view these problems as high priority blockers to reaching the full potential of smart accounts, and without it, we will continue to see[multi-billion dollar losses](https://www.stackup.fi/resources/the-bybit-lesson-rethinking-treasury-wallet-security) due to access control issues.


To solve it, we've built a novel, permissionless, and pragmatic solution in the form of a Keystore contract.


‍


Many of the problems with smart contract wallets are rooted in a tight coupling between the account's code and its configuration. By decoupling the configuration into a Keystore entity that lives independent of the account state, we find that many of the above problems become a lot more manageable.


More specifically, the account delegates validation to the Keystore using an immutable pointer we call a reference hash. Although the pointer cannot change, the value it points to can. Furthermore, this value can change irrespective of whether or not the account code is deployed.


In the following sections, we'll walk through how we solve the issues with account deployment, cross-chain sync, and privacy using the Keystore contract.


## Fixing problem 1 with immutable pointers


Lets get more concrete and circle back to the key rotation and cross chain sync problem but now through the lens of a Keystore.


First we can see that instead of hard coding the public key in the account's initialization data, we now hard code the reference hash which we know can never change. This immediately solves the cross chain deployment problem whereby the user must always have access to the initial key.


The big asterisk here is that this is only true if the configuration that the reference hash points to can be reliably synced across` M` chains. Fortunately, this can be much more manageable if the Keystore is designed in a way where updates can be securely created and replayed permissionlessly.


From a user's perspective, this looks like a single signature to create an update action which then gets sent to a delegated relayer for broadcasting across` M` chains.


Since configuration is fully decoupled from the account, the delegated relayer can broadcast updates on all chains even if the account is not deployed. By the time a user makes their first transaction on a new chain, they can expect to use their latest key right away.


## Fixing problem 2 and 3 by making the pointer a Merkle Tree Root


We are still left with the problem of gas cost and privacy. By picking the right data structure for the pointer value, we can potentially solve both at once.


In the Keystore solution, we use a Merkle Tree data structure to represent the account's configuration. In the protocol this is called the User Configuration Merkle Tree (UCMT).


The leaves of the tree represent a single access rule. For example, each node could be an individual owner with their unique key.


Through several rounds of hashing, we derive a root hash that is unique for a specific combination of nodes. This dynamic root hash is what the immutable reference hash is pointing to onchain.


This data structure has two advantages:


1. We can represent configuration efficiently onchain using a single storage slot.
2. Because the actual leaves are not stored onchain, we are private by default.


When a specific leaf is required for validation, a Merkle Tree proof can be efficiently verified in` O(log N)` space and time complexity where` N` is the number of leaves.


## How validation works


Let's now look at a concrete validation flow when an account makes a transaction.


During validation, the account makes a` staticcall` to` Keystore.validate()` with the following inputs:


- Reference hash
- Message (e.g. userOp hash)
- Node (e.g. ECDSA verifier address || owner public key)
- Proof that node is included in the root hash
- Data (e.g. ECDSA signature)


The leaf (also called a` node` ) is a concatenation of a standardized verifier contract address and arbitrary config bytes.


The Keystore will verify the proof to ensure that the node is genuinely apart of the account's configuration. It then does a` staticcall` to` Verifier.validateData` with the following inputs:


- Message (e.g. userOp hash)
- Data (e.g. ECDSA signature)
- Extracted config from the node (e.g. owner public key)


The verifier will attest that a signature is valid and the validation results are returned back up to the account.


## How configuration is synced across chains


The final flow to consider is how a reference hash value is updated. This value always starts as the zero hash, in which case the reference hash itself is used as the value.


When updating the reference hash value, the user (or relayer) makes a call to` Keystore.handleUpdates` with the following inputs:


- Reference hash
- Next hash (i.e. the root hash to update to)
- Nonce (managed by the Keystore for replay protection)
- Proof that node is included in the current root hash
- Data (i.e. a signature for this update)


The downstream actions then follow a similar path to validation whereby the proof is validated and a` staticcall` to the verifier is made for signature attestation. Assuming all steps are ok, the Keystore will update the reference hash pointed value to the next hash.


## Keystore V1 release


A lot of effort has been put into evaluating the Keystore problem space and developing the following solution based on what our team has experienced from working deeply within the account abstraction ecosystem.


We are also excited to launch V1 of the protocol to mainnet with an[audit by Spearbit](https://github.com/stackup-wallet/keystore/blob/main/audits/spearbit-july2025.pdf) . While this article serves as a primer to the Stackup Keystore protocol, we also encourage users to deep dive into the[full specification](https://github.com/stackup-wallet/keystore/blob/main/doc/spec.md) for a complete picture.
