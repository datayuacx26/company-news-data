---
schema_version: "1.0.0"
document_id: "b3a1a35764abef5b70808d4db4bd610bd41763f6377b5b0c802d0d335f33e55c"
company_key: "yc-namecard-ai"
company: "Namecard.ai"
source_id: "yc-namecard-ai-rss-f3518cbae1fb"
canonical_url: "https://chainsigtweb3.medium.com/where-is-my-money-a-fund-tracing-report-for-steaker-users-7681d19b67c9"
published_at: "2022-12-13T12:32:01+00:00"
first_seen_at: "2026-08-10T00:54:06.811990+00:00"
fetched_at: "2026-08-20T03:26:10.503825+00:00"
content_hash: "sha256:a703413c74135169f75f8a9f0ae335f7876e62a18b1328ff552c4acb9dbf5fec"
---

# Where is my money? A fund tracing report for Steaker users.

Thomas Lefebvre/Unsplash


*Disclaimer: The data resource is from EtherScan, TronScan and BscScan. We also used the investigation tool Breadcrumbs to assist our research. This report doesn’t constitute forensic accounting evidence.*


Update: 18:30, December 15, 2022
1. Update data.
2. Add source of cash flow for the Steaker withdrawal wallet (user withdrawal)
3. Provided findings about the amount that cannot be withdrawn due to FTX’s crash supported by on-chain data.


### Intro


Steaker, a Taiwanese crypto investment service provider, has stopped withdrawals for users who purchased Portfolios affected by the FTX crash on 2022/11/11 at 19:00:00 (UTC+8).


Our report will first focus on the cash flows of stablecoins supported by Steaker. This includes USDT, USDC, BUSD and DAI, equivalent to 1 USD.


> What is stablecoin?
> Stablecoin is a cryptocurrency that’s pegged to fiat. The most common stablecoin is usually pegged to USD.


Many users are strongly affected by this unfortunate incident and have questions about their money and how Steaker takes care of their assets.


Steaker users have voluntarily provided Chainsight with their Steaker wallet addresses. We have found 3 interesting results through on-chain data analysis.


### 1. Steaker didn’t manage their risk properly. 83.1% of the funds were deposited to FTX.


Based on the data provided by Steaker users, we have identified the following addresses (hereinafter “Deposit wallets”) belonging to Steaker and we believe these are Steaker’s hot wallets to receive user’s funds:
0x5425A0f2633B60B9F2400D82ed6596b5A4B41523BSC (ETH) 0xf4Ad3ba2be7089092AbB55766E07953a50952139 (BSC)
TAXBLYJUYDokkUQLscuSfAAh6VGK1HPCV1 (TRX)


> What is hot wallet?
> Hot wallet is an address to receive the funds from user’s wallet addresses provided by the service provider (e.g., Steaker). This benefits the service provider as it enables batch transactions, decreasing transaction fees.


Based on the transaction history, amounts and pattern, we believe the following addresses (hereinafter “Withdrawal wallets”) are Steaker’s wallets to handle users’ withdrawal requests:
0x9680Eb0a1eca7a81AbAA0e923Decb3C0b4FA0E2a(ETH)
0x9680eb0a1eca7a81abaa0e923decb3c0b4fa0e2a(BSC)
TGXoNjiQrVUsNLaZQu3CakkxDyPAAmkryL (TRX)


We gathered the information and found that 48.2 million USD were deposited to Deposit wallets (User’s deposit) and 40.1 million USD were deposited to Withdrawal wallets (User’s withdrawal) since February 2022. The balance of funds Steaker owes to the users should be around $8 million (interest excluded).


The cashflows of stablecoin deposited to Steaker’s Deposit and Withdrawal wallets.


Among $48.2 million user funds, $40.1 million (83.1%) were deposited into FTX. $6.6 million (13.6%) was deposited into Steaker’s withdrawal addresses. $1.4 million (3%) was deposited to Binance and 0.1% was withdrawn to another multisig wallet possibly controlled by Steaker.


> What is multisig wallet?
> Multisig wallet means that this wallet requires multiple keys/signatures to access instead of one single private key. This increase the safety of the wallet.


Most of the funds have gone to FTX.The relevant trace of funds on 0x5…BSC (ETH). (Source: Breadcrumbs)


$40.1 million went into the Withdrawal wallet, which handles users’ withdrawals. $27.6 million was from FTX. $5.6 million was from Binance. $6.6 million was from the Deposit wallet directly. $220k was from other wallets.


The sources of funds deposited into the Withdrawal wallet


The on-chain data leads us to 2 key points：


- Since February 2021, $40.1 million of users’ funds has been deposited into FTX and $27.6 million has been withdrawn from FTX to Steaker’s Withdrawl wallet. The result of subtraction is $12.5 million dollar. This result should be the theoretical balance of Steaker’s FTX account (interest and withdrawals to other wallets excluded)
- Only $1.4 million funds were deposited to Binance, but $5.6 million was deposited to the Withdrawal wallet from Binance. This shows that Steaker has at least deposited $4.2 million to Binance from their FTX account or other wallets.


According to Steaker’s announcement on 2022/11/13, approximately 10.6 million USD was affected by FTX’s crush.


If we use the theoretical balance of Steaker’s FTX account ($12.5 M) minus the funds deposited into Binance ($4.2 M) and then add interest, the on-chain data seems to support Steaker’s claim about the number of user assets affected by FTX’s crush.


We could also use the number of user assets affected by FTX’s crush ($10.6M) minus the Total balance ($8M), which leads us to the result that Steaker at least made a profit of $2.5 million for the last two years.


Screenshot provided by Steaker in their announcement. (Source: Steaker) and the transaction between Steaker and FTX.


### 2. The founder of Steaker mixed part of the funds from Steaker and funds from his own address.


Binance address 0x901532F40941962BE4525497376e40513E0E8d16 has shown signs of mixing the funds from Deposit wallets and the founder’s personal wallets after the FTX crash.


We suspect this address might be from the founder’s personal Binance account since it only engaged with the founder’s other personal wallets until Nov-09–2022, 11:03:59 AM (UTC).


After FTX suspended users from withdrawal, 160,000 USDC that was supposed to be the user’s funds were sent to 0x9…d16 from Deposit wallets.


Address 0x9…d16 has engaged with Steaker’s Deposit wallet and Steaker founder’s wallet. (Source: EtherScan)


### 3. Where did the funds go after the FTX crash?


Approximately 822k USD of user funds were withdrawn from Deposit wallets after Nov-09–2022(UTC). $160k went to address 0x9…d16, which we believe is the founder’s personal Binance account. $30k went to another Binance account. $53k went to a multisig wallet recently created on Dec-06–2022(UTC). The rest went to Withdrawal wallets.


If you enjoy Chainsight’s article, don’t forget to follow us on social media or contact us via:


- [Twitter](https://twitter.com/ChainsightWeb3)
- [Facebook](https://www.facebook.com/ChainsightOfficial)
- [Discord](https://discord.gg/sfABMBCN)
- [Mastodon](https://cryptodon.lol/@ChainsightWeb3)
- Email


We provide the following services:


- [Web3Check](https://www.web3check.com/) - Browser Extention to detect crypto/NFT scams and secure your digital assets (Waitlist available)
- **Chainsight investigation service** - Illicit crypto tracing and loss retrieval consulting
