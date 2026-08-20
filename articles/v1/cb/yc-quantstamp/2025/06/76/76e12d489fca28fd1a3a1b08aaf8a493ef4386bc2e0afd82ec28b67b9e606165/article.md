---
schema_version: "1.0.0"
document_id: "76e12d489fca28fd1a3a1b08aaf8a493ef4386bc2e0afd82ec28b67b9e606165"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/economics-of-the-quantstamp-security-network"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:91830e9daff48de951c64815d18d8440f4061e0996b50f8fb7b2f1b449caaadb"
---

# Economics of the Quantstamp Security Network

This post will explain the economics of the Quantstamp Security Network which allow trustworthy scan results to be achieved with a decentralized network. The two major influences to the economics of the Quantstamp Security Network are the price floor, and supply and demand.


### The Price Floor


If a user wants to scan a smart contract, the network sets a minimum price of 1000 QSP. Any scan offered below this threshold will automatically be rejected by the network. Users can choose to offer to pay more than 1000 QSP for a scan, and we will explain why they might do that later in this post.


‍


### Comparing to the Economics of an Ethereum Base Transaction


The number of pending transactions on Ethereum impacts gas prices. Similar economics influence the price of Quantstamp Security Network scans. source:[https://etherscan.io/chart/pendingtx](https://etherscan.io/chart/pendingtx)


‍


Aside from the minimum price floor, the economics of the Quantstamp Security Network is similar to how gas prices work on other decentralized networks like Ethereum. When you pay for an Ethereum transaction directly on the blockchain level, you are making a bid to miners to include your transaction in a block with limited capacity. Only so much data can be stored in each block.


When there is not enough space to include all pending transactions in the next block, miners prioritize transactions that pay more per unit of data because it increases their profit. Consequently, Ethereum users can increase the likelihood of including their transaction in the next block by paying a higher gas price.


### Economics of the Quantstamp Security Network


After 1000 QSP, the price is controlled by the user. Like an Ethereum transaction, users can choose to increase the amount they are willing to pay for a scan. If the demand for Quantstamp Security Network scans is high, users can increase the amount of QSP they are offering to pay in order to get their transactions processed by Quantstamp Security Network node operators before other users.


Although users control the price they are willing to pay, the Quantstamp Security Network prioritizes their transaction depending on availability and demand. Node operators prioritize higher paying scan requests first. Also, although 1000 QSP is the current price floor, individual nodes can set a personal price floor above 1000 QSP.


Also, in order for someone to be eligible to run a Quantstamp Security Network node, they are required to stake 50,000 QSP. This mechanism helps to ensure that node operators are good actors. Currently, node operators are not slashed for malicious behavior. This may change in the future.


‍


### What does this mean for node operators and scan requestors?


When demand for scans is low, users can offer to pay close to the price floor (1000 QSP) and expect their scan to get completed fairly quickly. When demand for scans is high, if they want their scans processed quickly, they should increase the amount they are willing to pay.


For node operators, they only get paid when their node processes a scan. The more scans that a node processes, the more QSP they will earn depending on the amount of QSP scan requestors offer to pay.


As a decentralized network, the Quanstamp Security Network does not have centralized actors controlling the behavior on the network. Instead, economic incentives drive beneficial behavior from decentralized actors, creating trustworthy results.
