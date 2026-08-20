---
schema_version: "1.0.0"
document_id: "3b5692be98be3b46e7e7ba6931a21a033bd2f1636306d3e7ee632e1ac5c42204"
company_key: "yc-stackup"
company: "Stackup"
source_id: "yc-stackup-news-import-4adfae6a34ed"
canonical_url: "https://www.stackup.fi/resources/what-is-an-eoa"
published_at: null
first_seen_at: "2026-07-27T05:25:45.190590+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:2f59caa67c85ebc5eded3c9c77ef1811ad0bf82cf895adaf289b0da31763578f"
---

# What is an EOA?

An Externally Owned Account (EOA) is a type of Ethereum account controlled by a private key, allowing users to send transactions, hold tokens, and interact with smart contracts on the blockchain. Unlike smart contract accounts that are controlled by code, EOAs are controlled directly by users through their private keys, making them the fundamental way individuals interact with Ethereum and similar blockchains. Every transaction on Ethereum must originate from an EOA, as only EOAs can initiate transactions by signing them with their private key.


## Understanding the Two Types of Ethereum Accounts


Ethereum has two distinct account types that serve different purposes in the ecosystem:


**Externally Owned Accounts (EOAs)** are user-controlled accounts that represent individual users or entities on the blockchain. These accounts are created when someone generates a private key and derives a public address from it. The most common examples are MetaMask wallets and hardware wallets.


**Contract Accounts** are smart contracts deployed on the blockchain. Unlike EOAs, they don't need private keys and can execute complex logic, and may require additional infrastructure like ERC-4337 to initiate transactions.


## Key Characteristics of EOAs


### Private Key Control


The defining feature of an EOA is its control mechanism through a private key. This cryptographic key is a very large number (256-bit or 78 digits) that serves as the ultimate proof of ownership. Sometimes this number is generated from a seed phrase, 12 to 24 words that can be more easily stored by the user to recover the account if needed.


Anyone with access to the private key has complete control over the account and its assets. The corresponding public address is derived from this private key with cryptography, allowing users to send assets to the EOA without knowing the private key.


### Transaction Initiation


Only EOAs can initiate transactions on Ethereum. This is a fundamental rule of the network that remains true even with advanced wallet technologies, though[the Ethereum Foundation is working on changing this rule](https://eips.ethereum.org/EIPS/eip-7701) as contract accounts are becoming more common. In the meantime,[ERC-4337](https://docs.erc4337.io/) is often used to overcome this limitation.


### Gas Payment


EOAs must hold the native token of the network, usually ETH, to pay for gas fees. If an account is out of ETH it can’t initiate a transaction, even if it holds other tokens.


### No Code Execution


Unlike smart contracts, EOAs cannot contain or execute code. They can only sign and send transactions. This simplicity is both a limitation and a security feature. There's no complex code that could contain bugs or vulnerabilities at the account level, but they also cannot contain additional safety features like access control policies. This makes EOAs particularly vulnerable to phishing.


### Upgrading EOAs to Contract Accounts


Until recently, there was no way to convert an EOA into a contract account. However, a new change to the Ethereum network called[EIP-7702](https://eip7702.io/) now allows EOAs to upgrade to contract accounts with some limitations.


## Best Practices for EOA Management


Because EOAs provide no customization, they rely on manual organizational controls to use them safely. To safely manage EOAs, follow these guidelines:


**Secure Key Storage** : Never store private keys or seed phrases digitally. Use hardware wallets for significant holdings and write seed phrases on paper or metal backup solutions.


**Address Verification** : Always double-check addresses before sending transactions. Consider sending a small test transaction first for large transfers.


**Regular Security Audits** : Periodically review your account's transaction history and revoke unnecessary token approvals using tools like Revoke.cash.


**Multiple Accounts** : Use different EOAs for different purposes: one for DeFi interactions, another for NFTs, and a separate cold storage account for long-term holdings. The cold storage account should use a hardware wallet and never make smart contract interactions.


In practice, organizations struggle to adhere to these best practices because they are too burdensome. We recommend organizations use smart contract accounts with built-in access control policies, like Stackup.


## How EOAs Work: Technical Deep Dive


When you create an EOA, you're essentially generating a random private key and deriving a public address from it. Here's the process:


1. **Private Key Generation** : A random 256-bit number is generated
2. **Public Key Derivation** : The public key is calculated using elliptic curve multiplication
3. **Address Creation** : The Ethereum address is derived by taking the last 20 bytes of the Keccak-256 hash of the public key


Every transaction from an EOA follows this flow:


- The user constructs a transaction specifying the recipient, amount, data, gas price, and gas limit
- The transaction is signed with the private key using the ECDSA algorithm
- The signed transaction is broadcast to the network
- Nodes verify the signature matches the sender's address
- If valid and the account has sufficient balance, the transaction is included in a block


## Common EOA Wallets and Tools


Most crypto wallets you interact with are interfaces for managing EOAs:


**Browser Wallets** like MetaMask, Rabby, and Rainbow create and manage EOAs directly in your browser. They store encrypted private keys locally and provide a user-friendly interface for signing transactions.


**Hardware Wallets** such as Ledger and Trezor generate and store private keys on specialized hardware devices, offering enhanced security by keeping keys offline and requiring physical confirmation for transactions.


**Mobile Wallets** provide EOA management on smartphones, often incorporating additional features like biometric authentication and cloud backup options.


**MPC Wallets** like Fireblocks, Qredo, and ZenGo use Multi-Party Computation (MPC) to split private key control across multiple parties. Instead of a single private key, cryptographic key shares are distributed among different servers or devices. These shares must work together to sign transactions, reducing the risk of the private key becoming inaccessible.


‍[Learn more about MPC Wallets](https://www.stackup.fi/resources/mpc-wallets-a-complete-technical-guide)


MPC wallets still manage regular EOAs, they just use a different method to control the private key. The transactions they produce are indistinguishable from standard single-signature EOA transactions. This approach removes the need for users to manage seed phrases, though users must trust the MPC infrastructure providers and do not have the accessibility features of contract accounts.


## Security Considerations for EOAs


The security model of EOAs is straightforward but unforgiving. According to Chainalysis' 2024 Crypto Crime Report, over $2 billion was lost to private key compromises, highlighting the critical importance of proper EOA security.


**Single Point of Failure** : If someone gains access to your private key, they have complete control over your account. There's no password reset, no customer service, and no way to reverse unauthorized transactions. In fact, 80% of the $3.1 billion lost in crypto during early 2025 was attributed to access control failures rather than smart contract bugs, with EOA compromises being a significant contributor.


**Seed Phrase Management** : Most wallets generate private keys from a seed phrase (12-24 words). This seed phrase must be stored securely offline. If lost, access to the EOA is permanently lost. If stolen, the account is compromised.


**Phishing Risks** : Users must carefully verify transaction details before signing. Malicious websites can request signatures for transactions that drain your wallet. Always verify the transaction data and the website's authenticity.


**No Recovery Mechanism** : Unlike traditional accounts, there's no "forgot password" option. The blockchain's immutability means that once assets are sent from an EOA, the transaction cannot be reversed.


## Limitations of EOAs


While EOAs are fundamental to Ethereum, they have several limitations:


**Poor User Experience** : Managing private keys and seed phrases is complex for non-technical users. One mistake can lead to permanent loss of funds.


**Limited Functionality** : EOAs cannot implement custom logic, multi-signature requirements, spending limits, or other advanced features without external smart contracts.


**Gas Dependency** : Users must always maintain ETH balance for gas, creating friction when trying to use other tokens or when onboarding new users.


**No Batching** : Each action requires a separate transaction, leading to multiple approvals and gas payments for complex operations.


## Limitations of Multisigs


The first contract accounts were called “multisigs” because they require multiple EOAs to sign a transaction. While this improves the security of accounts, this approach adds additional friction. All transactions require multiple signatures, no matter how small, one of the EOAs has to pay to initiate the transaction, and every transaction has complete control over the wallet with no additional guiderails.


This clunky user experience was exploited in the[$1.5 billion ByBit hack](https://www.bbc.com/news/articles/c2kgndwwd7lo) , where a malicious transaction was queued up for approval in a Safe (formerly Gnosis Safe) multisig. The signers thought they were approving a routine small transaction, but in reality it drained the entire wallet.


## Account Abstraction and Smart Accounts


Contract accounts can address all of the limitations of EOAs, but require additional infrastructure like ERC-4337 to provide these benefits. ERC-4337 enables smart contract wallets to function more like EOAs while adding programmable features:


**Smart Contract Wallets** can implement features like social recovery, spending limits, multisig requirements, and gas payment in any token.


**Session Keys** allow temporary permissions for specific actions, improving UX for gaming and frequent interactions without compromising security.


**Bundled Transactions** enable multiple operations in a single transaction, reducing gas costs and improving user experience.


Stackup helped develop ERC-4337 and has worked with it full time since 2021.


> EIP-4337 is LIVE on Ethereum
>
>
> Here's what it means and why it's a big deal 👇
>
>
> — John Rising (@johnrising_)[March 2, 2023](https://twitter.com/johnrising_/status/1631087378642206720?ref_src=twsrc%5Etfw)


‍


## Conclusion


EOAs are the gateway for users to interact with blockchain networks, providing direct, permissionless access to the decentralized web. While they have limitations in terms of user experience and functionality, they remain the most common way users interact with blockchains. Understanding how EOAs work, their security implications, and best practices for managing them is essential for anyone participating in the blockchain ecosystem.


As the technology evolves with innovations like account abstraction and EIP-7702, the distinction between EOAs and smart contract accounts may blur, offering users more flexibility and better experiences while maintaining the security and decentralization that make blockchains valuable.


For organizations operating onchain, we strongly recommend a contract account with access control policies. This approach not only adds safeguards, but also makes it easier to maintain internal policies.


While we were building the account abstraction infrastructure that powered Coinbase Wallet (now Base App) and Trust Wallet, we couldn’t find a wallet that did this, so we built one ourselves. Now you can[control your onchain finances at stackup.fi](https://www.stackup.fi/) .


‍
