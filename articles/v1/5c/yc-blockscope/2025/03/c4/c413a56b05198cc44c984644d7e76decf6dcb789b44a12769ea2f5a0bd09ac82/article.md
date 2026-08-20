---
schema_version: "1.0.0"
document_id: "c413a56b05198cc44c984644d7e76decf6dcb789b44a12769ea2f5a0bd09ac82"
company_key: "yc-blockscope"
company: "Blockscope"
source_id: "yc-blockscope-rss-6562ab22cedc"
canonical_url: "https://medium.com/@blockscope.co/tracing-cross-chain-asset-movements-with-blockscope-9860243e7399"
published_at: "2025-03-08T07:14:56+00:00"
first_seen_at: "2026-07-27T08:09:19.809826+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:6f287423021d951c7d6190cd335a9b0cc89a6ef21b91140cabdc24580dbb5582"
---

# Tracing Cross-Chain Asset Movements with Blockscope

# Tracing Cross-Chain Asset Movements with Blockscope


## Written By: Tushar Tiwari, Analyst at[Blockscope](https://medium.com/u/e3c579ea4506?source=post_page---user_mention--9860243e7399---------------------------------------)


[Blockscope](https://medium.com/@blockscope.co?source=post_page---byline--9860243e7399---------------------------------------)


4 min read


·


Mar 8, 2025


--


With more than **$1.6 billion lost to DeFi exploits in 2025** , tracking stolen assets across blockchains has become an increasingly complex challenge. Attackers leverage cross-chain bridges to obscure fund trails, making traditional tracing methods ineffective.


Bridges like[THORChain](https://x.com/THORChain) **,**[DeLiquidity Network Bridge](https://x.com/DLN_Trade) **(** DLN Bridge **), and**[Stargate Protocol](https://x.com/StargateFinance) (built on LayerZero) facilitate seamless asset transfers, but they also provide exploiters with tools to launder stolen funds. For example, in the **Bybit exploit** , exploiters used **THORChain** to swap stolen ETH into BTC, significantly complicating recovery efforts.


Security firms like[Blockscope](https://www.blockscope.co/) tackle these challenges using advanced blockchain analytics tools to track fund flows across chains with precision. To illustrate how cross-chain tracing works, let’s analyze the[Moby Trade](https://x.com/Moby_trade) **exploit** and demonstrate how Blockscope’s forensic tools enabled precise tracking of stolen assets.


## Case Study: The Moby Trade Exploit (January 2025)


Moby, a decentralized options trading platform on Arbitrum, suffered a **$2.5 million exploit** due to a **compromised private key** . The attacker, using address *0x2a566D111d0a5Be888FEC5F3834434Af3245Bb1b* , stole:


- **3.77 wBTC**
- **207.76 wETH**
- **30,179 USDC** *(* later recovered by the protocol *)*


## Tracing the Cross-chain Movement of Stolen Funds


Following the initial theft, the exploiter **swapped all wBTC and wETH for native ETH** using[Uniswap](https://x.com/Uniswap) , consolidating **312 ETH** at address *0x6A92D4840309f447922114a349984a1d09a51470* . The next step was **cross-chain movement to evade detection and launder funds** .


Press enter or click to view image in full size


Tracer tool showing Exploiter swapping all the various stolen assets into ETH


### Bridging ETH from Arbitrum to Ethereum


To obfuscate the funds, the exploiter leveraged **Stargate Protocol** , making five transactions to its native pool contract.


Press enter or click to view image in full size


Funds are getting bridged on Arbitrum using Stargate Protocol


Using **Blockscope’s Transaction Decoder** , we identified these as **OFT** (Omnichain Fungible Token) transfers **** on[LayerZero](https://x.com/LayerZero_Core) , which typically maintain the same recipient address across chains unless explicitly changed by the sender. By decoding the transaction logs, we found:


- **GUID** (a unique identifier for cross-chain transactions)
- **Destination Chain ID:** 30101 (Ethereum Mainnet)


Press enter or click to view image in full size


Transaction Decoder displaying the decoded log for the OFTSent event, revealing the destination chain ID, transferred amount, and sender’s address


A quick search using **Blockscope’s Tracer tool** on Ethereum confirmed that 311.8 ETH was received at the same exploiter-controlled address.


Press enter or click to view image in full size


Tracer showing funds bridging from Arbitrum to Ethereum using Stargate Protocol


### Bridging ETH to Polygon via DLN Bridge


Further investigation revealed another movement — **3 ETH was bridged to Polygon Mainnet via DLN Bridge** . By examining the transaction hash *0x35031f2fca5558df30bbcca228c06027357a4481f8a413d782c568c9e0a475e9* , we confirmed that Polygon address **** *0x36d137d85a8a0c8d30cec57aeda82b4eff1ebade* received **21,339 POL** , transferred from one of the exploiter’s side wallets.


Press enter or click to view image in full size


Press enter or click to view image in full size


ETH bridged to Polygon mainnet via DLN Bridge; transaction logs display chain IDs, sender, and recipient details, along with amount being transferred


This pattern of swapping, bridging, and moving funds across chains is a common obfuscation tactic among exploiters.


## How Blockscope Enables Cross-Chain Investigations


Blockscope’s advanced tools streamline **cross-chain tracing** , enabling analysts to track stolen funds with precision. Key features include:


- **Transaction Decoding & Log Analysis** — Extracting blockchain logs to analyze transactions, GUIDs, and destination IDs.
- **Automated Tracing** — Tracking address activity across multiple chains using the **Tracer tool** .
- **Bridging Analysis** — Identifying and verifying cross-chain fund movements.


These tools help law enforcement, exchanges, and DeFi protocols **** mitigate risks and recover stolen assets efficiently.


## Conclusion


With DeFi exploits growing in complexity, cross-chain fund tracing is no longer optional — it’s essential. Attackers increasingly use bridges and multi-chain strategies to obscure stolen funds, but **Blockscope’s advanced forensic tools** help investigators unravel even the most sophisticated laundering methods.


By leveraging our tools, businesses can enhance transparency, identify vulnerabilities, and improve asset recovery.


For more insights into the Moby Exploit, check out our full investigation:[https://research.blockscope.co/moby-trade-and-the-breached-private-key](https://research.blockscope.co/moby-trade-and-the-breached-private-key)
