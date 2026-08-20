---
schema_version: "1.0.0"
document_id: "a1c43fd8ca1564476404fdf2a3ea983a9f406b7194538fc6cf8a5d05a4c6d2b0"
company_key: "yc-namecard-ai"
company: "Namecard.ai"
source_id: "yc-namecard-ai-rss-f3518cbae1fb"
canonical_url: "https://chainsigtweb3.medium.com/what-happened-to-ankr-protocol-hack-1a584bf43ecb"
published_at: "2022-12-05T10:37:36+00:00"
first_seen_at: "2026-08-10T00:54:06.811990+00:00"
fetched_at: "2026-08-20T03:26:10.503825+00:00"
content_hash: "sha256:ae509be0905196f8d07509f464ec881deb113c63ced8e81bde0a55313df20444"
---

# What happened to ANKR Protocol Hack?

Ankr admitted a former employee caused a $5M exploit and vowed to improve security through a review and update process. *(Towfiqu barbhuiya/Unsplash)*


1. Summary


The hacker exploited Ankr’s smart contact and minted 60T aBNBc to their address 0xf3a465C9fA6663fF50794C698F600Faa4b05c777. aBNBc is a cryptocurrency issued by Ankr that should be pegged to BNB.


The aBNBc and BNB have depegged after the hacker dumped all the aBNBc.


The hacker then uses PancakeSwap, Celer cbridge, debridge, SushiSwap, Uniswap, 1inch and Paraswap to swap and send assets from the BSC chain to ETH and Polygon.


2. Current asset distribution


The hacker has profited approximately $5.4M from this incident. The hacker sent assets worth almost $4M to Tornado cash and 1 ETH to MEXC exchange deposit address 0x06aea65f82e225ec718b5dd0b803df7022d16d7d.
There is about $1.4M worth of assets remaining in the wallets.


Current stolen assets distribution.


3. Transaction details on BSC


**aBNBc**
Mint 60T aBNBc.
Swap 27k aBNBc for 5.5k BNB and 4.08M USDC via PancakeSwap.


**BNB**
Swap 3.6k BNB for 1M USDC via PancakeSwap
Swap 900BNB for 262k USDC via 1inch.
Sent 900 BNB to Tornado Cash.


**USDC**
Sent 3M USDC to ETH and 700M USDC to Polygon via Celer cbridge.
Sent 1M USDC to ETH via debridge.
Swap 100k USDC for 90k MATIC and send it to Polygon via debridge.


4. Transaction details on Polygon


**USDC**
Swap 700k USDC to 755k Matic via Paraswap.


**MATIC**
Sent 332k Matic to Tornado Cash.


5. Transaction details on ETH


**USDC** Swap 2.7M USDC for 2.1k ETH via PancakeSwap.
Swap 1.3k USDC for 1 ETH via SushiSwap.
Swap 100k USDC for 78 ETH via UniSwap.
Swap 1.7M USDC for 1.3k ETH via 1inch.


**ETH** Sent 2659 ETH to Tornado Cash.
Sent 1 ETH to 0xa2bf6C6…, then sent to MEXC.


The cash flow of the hacker’s ETH wallet address. (Source: Breadcrumbs)


6. Follow up


Owner of Address 0x8d11f5b4d351396ce41813dce5a32962aa48e217 has arbitraged 15.5M BUSD out of 10 BNB.


The arbitrageur made $15.5M with only 100BNB, which is worth about $30k. (Source:Debank)


Here is how the arbitrageur does it:


1. Swap 10 BNB for 184k aBNBc via 1inch.
2. Convert 184aBNBc to 191k hBNB via Helio. (aBNBc is still considered worth around 1 BNB on Helio)
3. Deposit 191k hBNB as collateral to borrow 16.4M HAY via Helio. (HAY is an algorithmic stablecoin issued by Helio that should be worth 1 USD.)
4. Swap 16.4M HAY for 15.5 BUSD via 1inch.


Some believe the arbitrageur is also the hacker because some test transactions happen within 1 hour after the hack.


The first test transactions were made by the arbitrageur. (Source:Debank)The hack starts at Dec-02–2022 12:43:18 AM (UTC)The arbitrageur made the first test transactions on Dec-02–2022 at 01:22:31 AM (UTC)


15.5M BUSD and 200k USDC have been sent to a Binance deposit address 0x4c7f5513894a99260bbfcf88311b544d6ca12757 which CZ from Binance claimed that they had frozen the account with $3M assets left.


### CZ 🔶 Binance on Twitter: "Possible hacks on Ankr and Hay. Initial analysis is developer private key was hacked, and the hacker updated the smart contract to a more malicious one. Binance paused withdrawals a few hrs ago. Also froze about $3m that hackers move to our CEX. / Twitter"


Possible hacks on Ankr and Hay. Initial analysis is developer private key was hacked, and the hacker updated the smart contract to a more malicious one. Binance paused withdrawals a few hrs ago. Also froze about $3m that hackers move to our CEX.
