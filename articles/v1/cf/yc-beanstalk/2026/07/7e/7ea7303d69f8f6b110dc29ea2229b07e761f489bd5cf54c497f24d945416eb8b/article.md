---
schema_version: "1.0.0"
document_id: "7ea7303d69f8f6b110dc29ea2229b07e761f489bd5cf54c497f24d945416eb8b"
company_key: "yc-beanstalk"
company: "Beanstalk"
source_id: "yc-beanstalk-news-import-a3f4f49fe20d"
canonical_url: "https://bean.money/blog/bip-30-generalized-pipeline"
published_at: null
first_seen_at: "2026-07-24T19:21:00.865118+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:470e29ebe751ef0a04a3980743eee31287d53ade163f3a899ebfb3b3563d9527"
---

# Announcing BIP-30: Generalized Pipeline

Beanstalk Farms is pleased to announce that[BIP-30](https://snapshot.org/#/beanstalkdao.eth/proposal/0x724bbca47b55d42ec25f76c233846bdbbae1dd833618b938c84d58f53ffe7c3d) —Generalized Pipeline and ERC-20 and ERC-721 Permit Support—has passed by a vote of the DAO.


BIP-30 was proposed by Beanstalk Farms to improve the composability and the economic efficiency of Beanstalk.


**Generalized Pipeline**


BIP-30:


- Implements a version of[Clipboard](https://evmpipeline.org/pipeline.pdf#section.5) in the` farm` function such that Farmers can copy return values from any function call into the function calldata of subsequent functions; and
- Wraps Pipeline functionality in a Depot Facet such that Farmers can access Pipeline through the use of the` farm` function.


[Pipeline](https://evmpipeline.org/pipeline.pdf) is a sandbox contract which can be used to execute an arbitrary number of actions within the EVM from an externally-owned account (EOA) in a single transaction. Used in tandem with the Clipboard—a spec for copy-pasting calldata within Pipeline—Pipeline can bundle the following in a single transaction, for example:


1. Loading Pipeline with ETH;
2. Wrapping ETH → WETH via WETH9;
3. Swapping WETH → USDT → BEAN via tricrypto2 and the BEAN:3CRV pool, and;
4. Copying the amount of BEAN from the swap into a Deposit.


This is how the[Root UI](https://roottoken.org/) can mint Roots in a single transaction using Beans or another stablecoin.


The Depot is a wrapper for the Pipeline that supports:


1. Loading assets (ERC-20 / ERC-721 / ERC-1155 / Deposits) into Pipeline;
2. Using them; and
3. Unloading them, in a single transaction.


The combination of Pipeline, Depot and Clipboard allows EVM users to perform arbitrary valid actions, through arbitrarily many protocols, in a single transaction.


**ERC-20 and ERC-721 Permit Support**


BIP-30 adds[EIP-2612](https://eips.ethereum.org/EIPS/eip-2612) permit support for ERC-20 tokens and[EIP-4494](https://eips.ethereum.org/EIPS/eip-4494) permit support ERC-721 tokens in Circulating balances.


This streamlines the Beanstalk UX by allowing Farmers to perform approvals through permits without the need for a separate transaction.


**Sunrise Incentive Adjustment**


BIP-30 reduces the base` sunrise` incentive reward in order to minimize Bean sell pressure without sacrificing demand for` sunrise` calls.


Currently, the majority of the transaction fee from` sunrise` calls comes from the priority fee. Optimizing the base reward is expected to reduce the Beans that Beanstalk issues to pay for the` sunrise` call by up to 60,000 Beans per month.


**About Beanstalk**


Beanstalk is a decentralized protocol that allows anyone to realize the value of an open, permissionless fiat stablecoin. The Beanstalk community of lenders, borrowers and savers secures a protocol-native stablecoin, Bean, with the goal of creating the world’s most accessible digital money system. By eliminating collateral requirements, Beanstalk can be the catalyst for a trustless money that unlocks the potential of decentralized finance for everyone.
