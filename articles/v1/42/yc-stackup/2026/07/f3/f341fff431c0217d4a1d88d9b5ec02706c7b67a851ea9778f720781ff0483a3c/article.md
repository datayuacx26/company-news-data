---
schema_version: "1.0.0"
document_id: "f341fff431c0217d4a1d88d9b5ec02706c7b67a851ea9778f720781ff0483a3c"
company_key: "yc-stackup"
company: "Stackup"
source_id: "yc-stackup-news-import-4adfae6a34ed"
canonical_url: "https://www.stackup.fi/resources/stackup-community-update-may-2025"
published_at: null
first_seen_at: "2026-07-27T05:25:45.190590+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:738744066bc8cb2310f3828348fcf939da8dfc8edc96934d5537160d4e8bbb8b"
---

# Stackup Community Update: May 2025

Since our[March update](https://www.stackup.fi/resources/stackup-community-update-march-2025) , we've shipped several key features that address the most common friction points in crypto business operations: connecting to traditional banking, managing assets across multiple chains, and ensuring reliable transaction tracking.


# Connect your Bank


You can now connect your bank account directly to your Stackup wallet. ACH dollars in, and they appear as stablecoins in your wallet. Transfer stablecoins out, and they arrive in your bank account as dollars.


This eliminates the need to use separate exchanges or maintain parallel treasury systems. Your client pays your invoice via ACH, and those funds are immediately available for onchain operations. When you need to pay US-based employees or vendors, you can transfer directly back to your bank account.


We're using Bridge as our exchange partner, which has proven reliable for both ACH in and out flows.


# Cross-Chain Swaps


Our new cross-chain swap feature lets you move assets between different blockchains in a single transaction. You can now rebalance your treasury across multiple chains without managing separate wallets or tracking gas tokens on each network.


‍


The feature automatically handles gas fees and finds optimal routing for any token swap. If you need to pay a contractor on Polygon but your funds are on Ethereum mainnet, you can complete that payment directly without intermediate steps.


# Rebuilt Transaction Indexer


We've replaced our transaction indexing infrastructure with a custom-built solution. The previous system, based on third-party APIs, had trouble parsing ERC-4337 transactions correctly and occasionally missed or delayed transaction updates.


Our new indexer ensures transactions appear in your dashboard immediately and accurately. This was essential infrastructure work that delayed some other features but provides a more reliable foundation for everything else.


# Platform Improvements


We've made several refinements based on user feedback:


- ‍ **Unified Payouts Page** : Combined our transaction queue and invoicing features into a single interface that handles all outgoing payments. **‍**
- **Enhanced Transaction History** : Better display for batched transactions and cross-chain operations, making it easier to track what happened. **‍**
- **Expanded Token Support** : Added more tokens across additional networks. **‍**
- **Help Center and Security Page** : Launched comprehensive[documentation](https://docs.stackup.fi/) and a[page showing our security practices](https://trust.stackup.fi/) . **‍**
- **Batched Transaction API** : New endpoints that allow you to queue multiple transactions programmatically from your internal systems.


# What's Next


We're working on a USD yield product to help businesses earn returns on treasury balances. We're also developing enhanced reporting features and expanding our API capabilities for deeper integrations with existing business tools.


These updates address many of the operational friction points we've heard about from customers. If you haven't tried the new features yet, they're available for all users today.


[Create your Stackup account](https://app.stackup.fi/) to try them out.


‍
