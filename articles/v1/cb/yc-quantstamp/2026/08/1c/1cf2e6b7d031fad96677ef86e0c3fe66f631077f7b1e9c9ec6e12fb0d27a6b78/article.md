---
schema_version: "1.0.0"
document_id: "1cf2e6b7d031fad96677ef86e0c3fe66f631077f7b1e9c9ec6e12fb0d27a6b78"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/july-security-beat-2026"
published_at: "2026-08-12T16:33:16+00:00"
first_seen_at: "2026-08-13T09:47:54.408585+00:00"
fetched_at: "2026-08-13T09:47:56.358728+00:00"
content_hash: "sha256:78d7875da41846c6851b8e76fbbe544caf1ba56ac1b52fddeae41b0b277d91a7"
---

# July Security Beat: Keys Over Code (Again?)

Crypto lost more than $240 million across 29 incidents in July 2026, up roughly 216% from June's $75.87M, even as the number of attacks fell from 40 to 29. For the second month running, the damage didn't come from clever contract exploits. The same failure class ran through Web2, from a $12.3M rail-industry ransom to a 1TB bank breach that started with one employee's email.


Here's the month in security 👇


Category Loss Incidents


Private key / entropy / hot-wallet compromise ~$149.3M 4


Price-oracle manipulation ~$36.1M 5


Malicious governance proposal ~$22.1M 2


Bridge signature / verification bypass ~$14.0M 2


Other protocol logic, flash-loan, AMM ~$18.5M 16


**Total (Jul 1 to 31)** **~$240M** **29**


### **Hack of the month: Coldcard, ~$115M.**


In a sweep during the final days of July, an attacker drained roughly $115M in BTC from Coldcard hardware wallets. A firmware defect caused key generation to fall back from the dedicated hardware random-number generator to predictable software inputs (the chip's serial number and clock registers), which cut the seed space down to roughly four billion possibilities. The attacker regenerated candidate seeds on their own hardware, derived the resulting addresses, and matched them against the public chain. Early reporting put the loss near $70M while the drain was still in progress; the final tally reached about $115M, and there is no self-test an owner can run to check exposure. It is the single largest event of the month, about 48% of July's total losses.


‍ *Source |*[CoinDesk](https://www.coindesk.com/tech/2026/08/01/how-bitcoin-cold-wallets-lost-usd70-million-in-an-attack-that-never-touched-the-devices) *·*[Bitcoin Magazine](https://bitcoinmagazine.com/news/coldcard-wallet-exposed-after-bitcoin-hack)


### **AFX Bridge, ~$24.15M (Jul 22).**


The Arbitrum-based perpetuals venue lost about $24.15M when its bridge signing keys were compromised, and withdrawals were authorized with no matching deposits.


‍ *Source |*[CoinDesk](https://www.coindesk.com/tech/2026/07/23/arbitrum-based-afx-trade-drained-of-usd24-million-after-bridge-keys-compromised) *·*[The Block](https://www.theblock.co/post/409482/arbitrum-protocol-afx-trade-exploit)


### **Ostium, ~$23.75M (Jul 15).**


The Arbitrum trading protocol lost about $23.75M after an attacker seized an oracle key and pushed future-dated price data into the protocol's own feed, minting artificial profits. Ostium paused trading.


S *ource |*[CoinDesk](https://www.coindesk.com/business/2026/07/15/ostium-suffers-usd18-million-exploit-as-oracle-attack-wave-continues-to-hit-defi) *·*[Halborn](https://www.halborn.com/blog/post/explained-the-ostium-hack-july-2026)


### **BonkDAO, ~$21.3M (Jul 6).**


An attacker pushed a malicious proposal through governance and executed it against the treasury, draining about $21.3M and pressuring the BONK token.


‍ *Source |*[The Crypto Times](https://www.cryptotimes.io/2026/07/07/bonkdao-hit-by-20m-treasury-drain-in-governance-attack-bonk-slides/)


## **Off-chain: Web2 broke the same way.**


July's biggest traditional-security incidents traced back to the same root cause, a single compromised credential or key. The Bank of Baroda lost roughly 1 TB of customer and internal data after one employee email account was compromised, and Swiss rail manufacturer Stadler Rail was breached by the Everest gang through a supplier's data-exchange platform, refused a $12.3M ransom, and had about 271,000 files leaked. Paidwork exposed roughly 23M users, and KDDI up to 12M customers. In a preview of where this is heading, attackers compromised the Hugging Face AI model repository through an autonomous AI agent, an attack surface that barely existed a year ago.


‍ *Source |*[BleepingComputer](https://www.bleepingcomputer.com/news/security/swiss-rail-giant-stadler-rejects-123m-ransom-demand-after-cyberattack/) *·*[Reuters](https://www.reuters.com/business/media-telecom/customer-data-indias-bank-baroda-leaked-online-source-researcher-say-2026-07-27/) *·*[Hugging Face](https://huggingface.co/blog/security-incident-july-2026)


## **The pattern across the month.**


July's most expensive lessons, onchain and off, were on the security of keys and the quality of randomness. This is the same failure class that ran through June, only larger. Treat randomness sources, key custody (especially hot signers on bridges and oracles), governance execution paths, and third-party access as first-class parts of the review, and monitor as if a key will eventually leak, because in July many did.


## **Disclaimer**


This report aggregates publicly reported information as of the publication date and may be revised as investigations evolve and post-mortems are released. Recommendations are general guidance. Verify against primary sources before acting on any specific claim.


## **About This Series**


Quantstamp publishes the Security Beat monthly. We've conducted 1,300+ audits and secured $500B+ in digital assets across 250+ clients, including Ethereum Foundation, Aave, Polymarket, Ethena, Visa, OpenSea, Maker, Curve, Compound, and Lido. If you'd like to chat about anything security or request an audit, check out[quantstamp.com](https://quantstamp.com/) .


‍
