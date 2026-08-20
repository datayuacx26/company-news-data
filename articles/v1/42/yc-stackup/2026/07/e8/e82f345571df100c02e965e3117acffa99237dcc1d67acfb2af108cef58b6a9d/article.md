---
schema_version: "1.0.0"
document_id: "e82f345571df100c02e965e3117acffa99237dcc1d67acfb2af108cef58b6a9d"
company_key: "yc-stackup"
company: "Stackup"
source_id: "yc-stackup-news-import-4adfae6a34ed"
canonical_url: "https://www.stackup.fi/resources/what-is-eip-4337"
published_at: null
first_seen_at: "2026-07-27T05:25:45.190590+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:616fc919c08d113b723bce289b2f6a0f75f1cc75c76c23d10ae76ed36e9dba99"
---

# What is ERC-4337?

‍


ERC-4337 is an Ethereum standard that enables[Account Abstraction](https://www.stackup.fi/resources/what-is-account-abstraction) , allowing users to interact with the Ethereum network through smart contract wallets without needing to directly manage private keys or native ETH for gas fees.


## **What are the components of ERC-4337?**


The ERC-4337 standard enables account abstraction by creating an alternative transaction pool (also called amempool) for account abstraction transactions. It has four main components: UserOperations, Bundlers, EntryPoint, and Contract Accounts.


- **UserOperations** are pseudo-transaction objects that are used to execute actions through a smart contract wallet.
- **Bundlers** package UserOperations and send them to the EntryPoint contract on the blockchain for verification and execution.
- **EntryPoint** is a singleton smart contract that handles the verification and execution logic for transactions.
- **Contract Accounts** are owned by a user and can be supplemented with features from other smart contracts like aggregators and paymasters.


‍


## **How ERC-4337 Enables Account Abstraction**


The alternative mempool, managed by Bundlers, gets past the restriction of most EVM blockchains that only an EOA (externally owned account with a private key) can initiate transactions.


Here is the process is a diagram:


# **How ERC-4337 Enables Account Abstraction**


First, a user creates a UserOperation object and submits it to the mempool. The Bundler then receives the UserOperation and submits it to the EntryPoint for verification and execution. The EntryPoint verifies the UserOperation is valid by checking the Contract Account and if needed the Paymaster and Aggregator, and then sends the call data to the Wallet Contract for execution. Any unused gas fees are refunded to the Wallet Contract or a function is called on the Paymaster Contract to run any post-execution logic.


The ERC-4337 standard has the potential to revolutionize the way users interact with the Ethereum network and could make it easier for non-technical users to participate in the Ethereum ecosystem. However, it is important for all implementations of the ERC-4337 standard to be thoroughly audited to ensure security and reliability.
