---
schema_version: "1.0.0"
document_id: "5c7ef2212e1b7044a980a1924f428cb85824f41d7f413e9b6b343591a3aa738a"
company_key: "zeta-global-holdings-corp-class-a-common-stock"
company: "Zeta Global Holdings Corp."
source_id: "zeta-global-holdings-corp-class-a-common-stock-rss-c6d0a6efafe0"
canonical_url: "https://www.zeta.tech/us/resources/blog/stablecoins-and-the-redistribution-of-banking-economics/"
published_at: "2025-11-13T07:21:49+00:00"
first_seen_at: "2026-07-20T23:17:32.671692+00:00"
fetched_at: "2026-07-28T22:25:48.708473+00:00"
content_hash: "sha256:cdf3fafb1fa2170d695d48b20ce9fb93d736f7b02bea7d8313ad6b1392c76a57"
---

# Stablecoins and the Redistribution of Banking Economics

### Contents:


- **The Stablecoin Base Layer: More Than Just Issuance**
- **The Infrastructure Layer: Where Value Compounds**
- **What Shifts for Banks as Stablecoins Scale**
- **A Practical Operating Pattern for Banks**
- **Redistribution, Not Disruption**


Stablecoins are quietly changing the economics of money. Not just how it moves, but who gets paid when it does.


Public debate often casts stablecoins as a zero-sum threat to bank deposits, a view reinforced by their scale: issued value has climbed from $120 billion 18 months ago to $250 billion today, with a projection of $2 trillion by 2028[1](https://www.mckinsey.com/industries/financial-services/our-insights/the-stable-door-opens-how-tokenized-cash-enables-next-gen-payments) .


But that framing misses the deeper shift. Stablecoins aren’t only competing with deposits. They are redistributing the economics of deposits – custody, reserves, payments, float – and reallocating profit pools across the financial value chain.


This redistribution story is still unfolding, but its implications for banks, networks, and fintechs are already visible.


## The Stablecoin Base Layer: More Than Just Issuance


On the surface, issuing a stablecoin looks like a commodity utility: tightly regulated, operationally heavy, and thin-margin – take dollars into reserve, mint and redeem tokens, and run strict compliance. The token itself is largely interchangeable, so without added services, there’s little room to stand out.


But beneath that surface lies a more dynamic model. Stablecoin issuers monetize across multiple levers:


- Custody fees on reserves.
- Reserve management, often earning yield on Treasuries or cash equivalents.
- Brokerage and liquidity services for large holders or platforms.
- Foreign exchange (FX) when stablecoins are used globally.
- Float income when settlement lags behind underlying flows.


In this sense, the economics resemble traditional deposit-taking, but without the localized, relationship-driven footprint of banks. Revenues in stablecoin issuance are more centralized among a few large players, scale with the size of reserves rather than balance sheet lending, and are driven by infrastructure services like custody, payments, and reserve management.


This helps explain why banks like Bank of America and State Street have expressed interest in stablecoins, and why custodial giants such as BNY Mellon are expected to play outsized roles.


## The Infrastructure Layer: Where Value Compounds


The real transformation is happening at the infrastructure layer where networks, processors, and fintech platforms package stablecoins into usable building blocks. This is where complexity gets abstracted away and where economics begin to concentrate.


Consider a few examples:


- **Stablecoin accounts & card enablement:** Stripe’s Stablecoin Financial Accounts turn stablecoins into a practical money stack[2](https://stripe.com/in/newsroom/news/sessions-2025) : businesses can hold USDC/USDB, receive over crypto and ACH/SEPA, and send stablecoins globally without stitching together issuers, wallets, and off-ramps. Its acquired platform, Bridge, enables cards that draw from a stablecoin wallet and auto-convert to local currency at merchants.
- **Liquidity & chain abstraction:** Circle’s Cross-Chain Transfer Protocol lets apps move USDC by burning on one chain and minting on another, keeping funds in native USDC (not wrapped tokens)[3](https://www.circle.com/cross-chain-transfer-protocol) . That reduces fragmentation and quickly puts liquidity where a payment needs to land.
- **Programmable auth/capture:** Coinbase’s protocol mimics card flows—auth holds, captures, partial refunds—via smart contracts, so a token payment can reserve funds at order time and release them on shipment, with clean refunds if needed[4](https://www.finextra.com/blogposting/29130/deep-dive-coinbases-commerce-payments-protocol-how-to-use-it-integrate-it-and-win-with-it) .
- **On/off-ramps & merchant acceptance:** Worldpay enables merchants to accept crypto/stablecoin and settle in fiat (or sometimes in stablecoins), letting retailers keep their existing checkout and reconciliation while customers spend from token balances[5](https://fintechmagazine.com/articles/worldpay-partners-with-bvnk-for-stablecoin-payouts) .


What ties these together is abstraction. The platforms above hide the messy parts—keys, chains, liquidity, FX, and compliance—and expose clean APIs or familiar card rails. That’s why value is concentrating upstream: distribution plus orchestration wins. It’s no coincidence that large networks and fintech infrastructure providers are out in front—their scale, risk controls, and developer reach let them convert stablecoins from a promising asset into everyday payment utility.


## What Shifts for Banks as Stablecoins Scale


If the infrastructure layer is where stablecoin value is compounding, banks need a clear view of where that value might reroute, and where it can be recaptured.


- **Deposit substitution:** As stablecoin accounts become packaged with familiar UX—balances, cards, rewards—some rate-sensitive, mobile-first customers may keep more funds in token balances rather than traditional checking/savings.
- **BaaS and embedded finance, re-routed:** Many fintech programs that once required sponsor banks, FBO accounts, and deposit sweeps can now stand up on stablecoin rails with custody, compliance, and card acceptance abstracted by infra providers.
- **Reserve migration:** Most stablecoin reserves are expected to sit with custodial giants like BNY Mellon or State Street, not with regional or community banks. This moves liquidity “upstream,” shifting balance sheet economics toward institutions designed for scale custody.
- **Cross-border and FX:** Stablecoins are already proving useful in regions like Latin America, where Stripe and Visa are piloting stablecoin-linked cards[6](https://www.reuters.com/business/visa-bridge-partner-launch-stablecoin-linked-cards-2025-04-30/) . For banks with strong correspondent networks, this may mean both competition and partnership opportunities.


Stablecoins might rebalance where margin pools sit. The nearer a bank is to custody, liquidity, FX, settlement, and programmable controls, the more of that margin it can keep, even when the customer experience is orchestrated by networks and developer platforms.


## A Practical Operating Pattern for Banks


Two concrete frictions are driving near-term adoption:


- **Deposit substitution:** More value sits in fintech/stablecoin wallets instead of bank DDAs, pulling balances, and potentially downstream card spend, outside the bank.
- **Cross-border friction:** Paying freelancers/suppliers across borders remains slow and fee-heavy; platforms increasingly park working capital in USD-stablecoins because funds move quickly and settle on flexible schedules.


A pragmatic response is card-first orchestration with token funding sources:


- Keep the card as the everyday surface (physical/virtual, wallet-provisioned).
- Behind the scenes, allow the card to draw from multiple pots: a standard DDA/FBO and a partner-custodied stablecoin wallet.
- At authorization, apply policy (who/where/how much). If the stablecoin pot is selected, trigger an instant off-ramp to fiat and complete the purchase on normal card rails.
- Outcome: the customer spends anywhere cards are accepted; the bank retains card economics (interchange, fees, engagement) while custody/liquidity sit with upstream partners.


This pattern doesn’t require banks to issue a stablecoin or rebuild core systems. It reuses familiar levers like authorization policy, settlement, disputes, while letting specialized partners handle wallet custody and conversion.


Crucially, the control surface doesn’t change. The same card policies—who can spend, where, and how much—apply no matter which pot funds the purchase. Role-based limits, merchant and geography rules, velocity caps, simple budget pots for families or teams, and just-in-time funding all carry over. On the back end, refunds, partial captures, and disputes follow the standard card workflows ops teams already use.


## Redistribution, Not Disruption


Stablecoins are rerouting banking economics upstream toward custody, settlement, and infrastructure. We can already see this across the development of stablecoin accounts and card spend, reduction of chain friction, auth/capture on-chain, token balances to fiat settlement. As issued value climbs and potentially scales into the trillions, margin pools naturally follow the pipes: reserve custody, liquidity management, FX, and programmable controls. How far this shift runs will be set by rulemaking on reserves, yield, and AML, but the near-term signal is clear: value is concentrating where distribution and orchestration meet.


For banks, the practical takeaway is role clarity and operating discipline. Map where economics are accruing in this stack and decide, case by case, whether to participate as issuer, custodian, partner, or orchestrator. A card-first, multi-pot model—where fiat and token balances can both fund everyday spend—offers a near-term path to preserve engagement and capture margin without reinventing core banking.


### References:


1. McKinsey |[The stable door opens: How tokenized cash enables next-gen payments](https://www.mckinsey.com/industries/financial-services/our-insights/the-stable-door-opens-how-tokenized-cash-enables-next-gen-payments) | 2025
2. Stripe |[Stripe accelerates the utility of AI and stablecoins with major launches](https://stripe.com/in/newsroom/news/sessions-2025) | 2025
3. Circle |[Cross-Chain Transfer Protocol](https://www.circle.com/cross-chain-transfer-protocol) | 2025
4. Finextra |[Deep Dive: Coinbase’s Commerce Payments Protocol: How to Use It, Integrate It, and Win With It](https://www.finextra.com/blogposting/29130/deep-dive-coinbases-commerce-payments-protocol-how-to-use-it-integrate-it-and-win-with-it) | 2025
5. FinTech Magazine |[Worldpay Partners with BVNK for Stablecoin Payouts](https://fintechmagazine.com/articles/worldpay-partners-with-bvnk-for-stablecoin-payouts) | 2025
6. Reuters |[Visa, Bridge partner to launch stablecoin-linked cards](https://www.reuters.com/business/visa-bridge-partner-launch-stablecoin-linked-cards-2025-04-30/) | 2025


Tags:[#payments](https://www.zeta.tech/us/resources/blog/tag/payments/)


#### [Gary Singh](https://www.zeta.tech/us/resources/blog/author/garysingh/)


President, North America


#### About Author


Gary Singh is the President, North America at Zeta. A 20+ year silicon valley industry veteran, Gary has an extensive knowledge about the fintech industry and holds multiple patents in the mobile and wireless industry. At the core, Singh is a business and product guy, who understands how to build and take new and innovative products and services to disrupt status quo markets. Prior to joining Zeta, Singh was the Chief Revenue Officer at Ondot Systems. He has also held executive level positions at Obopay, Nokia Financials Services and Aruba Networks. He comes with over a decade of experience at Zebra (through multiple acquisitions — Motorola Solutions enterprise division and Symbol technologies), where he helped pioneer the WiFi market to automate supply chain operations. At Zeta, Singh is responsible for the company’s go-to-market, operations, growth and overall financial performance in North America.


[Learn More](https://www.zeta.tech/us/resources/blog/author/garysingh/)
