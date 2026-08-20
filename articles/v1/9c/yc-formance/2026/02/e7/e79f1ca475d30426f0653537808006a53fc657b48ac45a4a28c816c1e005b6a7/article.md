---
schema_version: "1.0.0"
document_id: "e79f1ca475d30426f0653537808006a53fc657b48ac45a4a28c816c1e005b6a7"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/industry-analysis/that-time-a-400-giveaway-became-an-8m-mistake"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:70cc33fd1cf8c7b89c4ea97efb167e027bc2536d8e7ce132873baf7fa36bdb57"
---

# Bithumb’s $40B Ledger Error: The Case for Double-Entry Bookkeeping

[On 6 February 2026](https://feed.bithumb.com/notice/1651924) , a Bithumb employee distributing small promotional rewards worth roughly ₩2,000 per user, (roughly $1.50) selected BTC as the reward asset instead of KRW. The result: 695 users were each credited with approximately 2,000 BTC. In total, 620,000 BTC appeared across those accounts, resulting in aggregate balances worth in excess of $40 billion at the time. This figure represents nearly 3% of Bitcoin's entire circulating supply.


But, importantly, none of this Bitcoin actually existed. No on-chain transactions occurred. The credits were purely entries in Bithumb’s internal ledger, reflecting a state the exchange did not actually hold. There was no exploit or fraud in the conventional sense. No private keys were compromised, and no actual on-chain transactions appear to have been involved in the mistake. The event was not the result of bad actors, but of a system with insufficient safeguards like balance checks executing an incorrect instruction.


Their internal system behaved consistently with what the ledger said was true. Users who noticed their balances began to sell. Within minutes, BTC/KRW on Bithumb had fallen more than 15% below prices on other exchanges, briefly touching ₩81 million (~$55,000) while global markets held above $66,000. Bithumb identified the error twenty minutes after distribution and began freezing affected accounts fifteen minutes after that, and completing the process five minutes later. In that 40-minute window, 1,788 BTC were sold on Bithumb’s own exchange. No funds left the platform; all selling occurred against real KRW liquidity inside the exchange. The exchange recovered 93% of those trade proceeds in KRW and other assets, leaving a net realized loss of approximately 125.16 BTC—roughly $8 million at prevailing prices—which Bithumb covered from its own reserves.


What makes this incident worth analyzing is not the data-entry error itself but what resulted from it. The failure propagated through every downstream system precisely because the internal ledger treated an impossible state as valid.


## Where the Error Occurred


The important detail is that *this entire process took place off-chain* . The balances that users observed—and, in some cases, acted upon—were not the result of blockchain settlement. They were the product of an internal accounting system that had recorded an incorrect state and exposed it to the rest of the platform.


[Korean FSC Chairman Lee](https://www.mk.co.kr/en/stock/11957098) was clear that the verification system between the internal ledger and the actual assets did not work when the payment was executed. That is a structural description, not a judgment about the specific mistake. The data-entry error was the trigger; the absence of any reconciliation between ledger state and reserve state was the condition that allowed it to propagate. As a result, Korean authorities are now closely auditing how other centralized digital asset exchanges are managing their internal ledgers.


This is how centralized exchanges work by necessity. Bitcoin's blockchain processes roughly seven transactions per second; an exchange serving millions of active traders cannot route every order through on-chain settlement. Instead, it maintains its own internal ledger: deposits are recorded when funds arrive, withdrawals are settled on-chain when funds leave, and everything in between—trades, transfers, balance updates—happens as entries in a database the exchange controls entirely. The blockchain is consulted only at the edges. In the middle, the ledger is the only record that matters.


At Bithumb, that ledger accepted an instruction to credit 695 accounts with approximately 2,000 BTC each and recorded it without resistance. The system had no mechanism to ask whether the exchange actually held 620,000 BTC available to distribute. According to Korean financial press,[Bithumb's actual BTC reserves at the time of the incident were in the range of 50,000 BTC](https://www.mk.co.kr/en/stock/11956330) , an order of magnitude less than the credits it had just recorded. The ledger and the exchange’s actual holdings had diverged by a factor of twelve, and nothing in the system detected it.


## Diagnosis


The most direct explanation for how this happened is that Bithumb’s internal ledger appears to operate on a single-entry basis.[In a single-entr](https://www.formance.com/blog/engineering/how-not-to-build-a-ledger)[y system](https://www.formance.com/blog/engineering/how-not-to-build-a-ledger) , a credit to a user account is simply recorded as a positive balance change. There is no corresponding debit, no source account that must be drawn down, and therefore no structural check on whether the credited value actually exists anywhere in the system. The instruction “add 2,000 BTC to account X” succeeds as long as the write operation succeeds. Whether the exchange holds 2,000 BTC, 50,000 BTC, or zero BTC is irrelevant to the transaction.


Double-entry bookkeeping eliminates this class of error by requiring that every credit has a corresponding debit of equal value. To credit 695 user accounts with 2,000 BTC each, a double-entry system must debit a source—most naturally, the exchange’s own reserve account. That debit would either succeed, drawing down the reserve, or fail, because the reserve does not hold 620,000 BTC and no balance checks were in place. In either case, the system produces a visible, auditable signal: either a reserve account showing a deficit that risk systems can flag, or a rejected transaction that never reaches user balances. The error cannot silently become reality. This is not a theoretical property of double-entry accounting—it is precisely why double-entry has been the foundation of institutional bookkeeping for five centuries.


Beyond the fundamental accounting structure, the incident also exposes several more specific design weaknesses that compound the risk. The first is the absence of strict asset typing at write time. A system that enforces clear distinctions between assets ( *e.g* that KRW and BTC are not interchangeable units) would have rejected or flagged a transaction denominated in an unexpected asset before it was recorded. Mechanisms such as account typing and[asset coloring](https://www.formance.com/blog/talks/fintech_devcon-2025-recording) exist precisely to enforce this at the ledger level.


The second is the absence of a controlled transaction lifecycle. The erroneous credits moved directly from recorded to tradeable with no intermediate state. A system that routes large or anomalous credits through a review or holding state before making them available provides a window for detection and correction. In this case, that window would have been sufficient: Bithumb identified the error twenty minutes after distribution. Had the credits been held pending settlement, no selling would have been possible in that interval.


More generally, these failures reflect a pattern the FSC identified as industry-wide: The ledger treated as a passive database that records instructions, rather than as the mechanism that enforces what is financially possible. When the ledger does not enforce invariants, namely that value must come from somewhere, assets have types, large credits require review, then incorrect states are not prevented. They are merely observed after the fact, once the damage is done.


This dynamic is not unique to exchanges. It reflects a broader characteristic of modern financial systems, including those built around cryptocurrencies, including stablecoins. While blockchains provide a mechanism for settlement,[most user interactions occur within systems that maintain their own representation of balances](https://www.formance.com/blog/financial-operations/the-fiat-to-digital-asset-playbook) . Exchanges, payment processors, and stablecoin platforms all rely on internal ledgers to track positions, enforce constraints, and enable real-time activity. In many cases, users do not interact directly with settlement layers. They interact with balances presented by these systems. Likewise, trading, transfers, and risk management decisions are driven by the ledger’s view of the world, *not* by the underlying state of a blockchain at any given moment.


As a result, the ledger is not merely a record of activity. It is the operational source of truth from which the system derives all subsequent behavior.


## Conclusion


The events at Bithumb are best understood not as a failure of digital assets or blockchains, but as a failure of internal representation. A system recorded a false state and, in so doing, created a temporary reality in which that false state was treated as true.


This is a property shared by all ledger-based systems. They define what exists within their domain, and all other components operate accordingly. And for systems built on digital assets, this principle is particularly important: *The reliability of the underlying asset does not compensate for weaknesses in the system that governs its use* .


Ultimately, every financial system depends on the integrity of its ledger. When that integrity is compromised, even briefly, the consequences are immediate.


(Image source:[Seoul Institute](https://commons.wikimedia.org/wiki/File:Government_Complex_Seoul_Main_Building.jpg) ,[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.en) )
