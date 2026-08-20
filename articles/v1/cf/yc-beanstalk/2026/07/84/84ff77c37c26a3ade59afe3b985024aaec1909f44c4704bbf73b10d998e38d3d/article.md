---
schema_version: "1.0.0"
document_id: "84ff77c37c26a3ade59afe3b985024aaec1909f44c4704bbf73b10d998e38d3d"
company_key: "yc-beanstalk"
company: "Beanstalk"
source_id: "yc-beanstalk-news-import-a3f4f49fe20d"
canonical_url: "https://bean.money/blog/bip-29-pod-market-price-functions"
published_at: null
first_seen_at: "2026-07-24T19:21:00.865118+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:65ccefb2a046d8a6fd936cab4ed871fa4bc79d24284522c92c0f2ce426071642"
---

# Announcing BIP-29: Pod Market Price Functions

Beanstalk Farms is pleased to announce that[BIP-29: Pod Market Price Functions](https://snapshot.org/#/beanstalkdao.eth/proposal/0x53c358af0fae50f888795c5f2272d50f8759b7702bf7dc2255a03f9fb22ccf45) has passed by a vote of the DAO.


## Pod Market Price Functions


BIP-29 proposed the following changes:


- Implementing V2 Pod Orders and Listings such that the price per Pod is priced as a function of place in the Pod Line (via piecewise cubic polynomials);
- Allowing Farmers to delegate use of their Farm balances to other contracts and adding EIP-2612 permit support to Farm balances; and
- Adding EIP-2612 permit support for Silo Deposits.


The Pod Market used to limit Farmers to creating Pod Orders and Listings with a single Fill price per Pod independent of place in Line. Pod Orders and Listings with a single Fill price failed to maximize overall marketplace liquidity by requiring the placing or updating of multiple Orders/Listings in order to create a non-flat pricing curve. This would be highly expensive for Farmers who tried to do this.


Now that Pod Orders and Listings can be dynamically priced, market efficiency and depth should improve. Pod Market V2 functionality isn't yet live on the[Beanstalk UI](https://app.bean.money/) but will be rolled out over the course of the next several weeks and months.


With EIP-2612 permit support for Silo Deposits and Farm balances, the number of transactions required to interact with Beanstalk decreases. This will enable projects like[Root](https://roottoken.org/) to allow users to interact with their protocols and Beanstalk in a single transaction.


## About Beanstalk


Beanstalk is a decentralized protocol that allows anyone to realize the value of an open, permissionless fiat stablecoin. The Beanstalk community of lenders, borrowers and savers secures a protocol-native stablecoin, Bean, with the goal of creating the world’s most accessible digital money system. By eliminating collateral requirements, Beanstalk can be the catalyst for a trustless money that unlocks the potential of decentralized finance for everyone. *
