---
schema_version: "1.0.0"
document_id: "d35524adef7472c2be9d92ef3f80cf963ddd18817d1ed5c67917c611e82d6a5e"
company_key: "yc-alpaca"
company: "Alpaca"
source_id: "yc-alpaca-rss-4f5e7678c8d4"
canonical_url: "https://alpaca.markets/blog/alpaca-introduces-index-options-paper-trading/"
published_at: "2026-07-23T13:56:50+00:00"
first_seen_at: "2026-07-23T13:58:29.241375+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:ff2cae313b0e3474d25c9eec62a72ac153cf2602716c00977c7c6f5aed3b4831"
---

# Alpaca Introduces Index Options in Paper Trading for Trading API Users

We've added index options in[paper trading](https://alpaca.markets/learn/start-paper-trading?ref=alpaca.markets) to[Alpaca's Trading API](https://alpaca.markets/algotrading?ref=alpaca.markets) . This release lets builders and traders test strategies on Cboe-listed US index options in a simulated environment before trading in a live environment. Initial coverage includes SPX, SPXW, VIX, VIXW, DJX, and XSP.


Alpaca has long focused on API-first infrastructure for builders. We’re extending that coverage to index options, so you can test strategies, confirm order handling, and validate expiration logic in paper before trading live.


## **What Are Index Options?**


Index options are contracts on the value of a market index, such as the S&P 500, VIX, or Dow Jones Industrial Average, rather than a single stock. Because there is no underlying share to deliver, these contracts settle in cash. The primary difference between equity options and index options is that index options are cash-settled and European-style, which changes how expiration and assignment work.


- **Cash-settled** : At expiration, positions settle in cash based on an official reference value. There is no share delivery.
- **European-style** : Exercise happens only at expiration, which removes early assignment during the life of the contract.
- **AM or PM expiration** : Each contract settles against a published Cboe reference value, depending on whether it is an AM-settled or PM-settled contract.
- **Same-day expiration (0DTE):** SPXW weeklies use PM settlement and can expire the same trading day, supporting[zero-days-to-expiration (0DTE)](https://alpaca.markets/learn/0dte-options?ref=alpaca.markets) strategies on the S&P 500 index.


## **How Index Options Work on Alpaca**


The index options workflow includes a few key differences:


- **Cash Settlement** : Profit or loss settles in cash, so you do not take delivery of shares or hold an unwanted position after expiration.
- **No Early Assignment** : European-style exercise means a position cannot be assigned before expiration, so you do not need to monitor for overnight assignment.
- **Tax Considerations:** Certain broad-based index options are generally taxed 60% long-term and 40% short-term, regardless of holding period **. ***


## **Built for Builders and Active Traders**


Index options provide exposure to a broad index in a single contract, without managing positions across many individual stocks. That structure is commonly used by market participants for hedging, income generation strategies, or expressing views on broader market movements.


Paper trading lets you build and test these workflows first before executing in live environments. You can wire index options into an existing strategy, check how cash settlement and European-style exercise affect your logic, and review fills in a simulated account[before trading with real money](https://alpaca.markets/learn/paper-trading-vs-live-trading-a-data-backed-guide-on-when-to-start-trading-real-money?ref=alpaca.markets) .


## **Risks and Considerations**


Options trading is not suitable for all investors and can result in significant losses. A few points are specific to index options and to this release:


- **Paper results differ from live** : Paper trading is a simulated environment. Fills, liquidity, latency, and order handling can differ from live trading.
- **AM settlement timing** : AM-settled contracts reference a value published after the open, so the final settlement value is not known at the close of the prior session.
- **Order cutoffs** : AM-settled contracts may have a morning cutoff for new orders on expiration day.
- **Exchange fees** : Index options carry exchange fees that differ from standard equity options. Review[current pricing](https://files.alpaca.markets/disclosures/library/BrokFeeSched.pdf?ref=alpaca.markets) before trading live.
- **Availability at launch:** Index options are initially available through the Trading API. Dashboard support will follow shortly. Alpaca does not currently provide index data through its Market Data offering, which will be supported in the coming months.


## **Get Started**


If you have an Alpaca account,[sign in](https://app.alpaca.markets/account/login?ref=alpaca.markets) , switch to a paper account, and enable options to begin testing index options in a simulated environment.


New to Alpaca’s Trading API? See[How to Connect to Alpaca's Trading API for paper API keys and setup](https://alpaca.markets/learn/connect-to-alpaca-api?ref=alpaca.markets) .


Both new and existing traders can review the[Trading API documentation for endpoint details](https://docs.alpaca.markets/us/reference/postorder?ref=alpaca.markets) . For step-by-step order placement and API workflows for both equity and index options, see[How to Trade Options with Alpaca.](https://alpaca.markets/learn/how-to-trade-options-with-alpaca?ref=alpaca.markets)


Still have questions? Learn more in the[docs](https://docs.alpaca.markets/us/docs/trading-api?ref=alpaca.markets) or contact us at[\[email protected\]](https://alpaca.markets/cdn-cgi/l/email-protection#5f2c2a2f2f302d2b1f3e332f3e3c3e71323e2d343a2b2c) .


---


**Tax information provided is general and for informational purposes only. It is not tax advice. Section 1256 treatment depends on your individual circumstances. Consult a qualified tax professional.*


*Any symbols or examples shown are for demonstration purposes only and do not constitute investment advice or a recommendation to buy or sell any security.*


*All investments involve risk, including the possible loss of principal. Past performance does not guarantee future results. No investment strategy can guarantee a profit or achieve its intended objectives. Diversification does not ensure a profit or protect against loss. Investors should carefully consider their investment objectives, risks, charges, and expenses before investing.*


*The Paper Trading API is offered by AlpacaDB, Inc. and does not require real money or permit a user to transact in real securities in the market. Providing use of the Paper Trading API is not an offer or solicitation to buy or sell securities, securities derivative or futures products of any kind, or any type of trading or investment advice, recommendation or strategy, given or in any manner endorsed by AlpacaDB, Inc. or any AlpacaDB, Inc. affiliate and the information made available through the Paper Trading API is not an offer or solicitation of any kind in any jurisdiction where AlpacaDB, Inc. or any AlpacaDB, Inc. affiliate (collectively, “Alpaca”) is not authorized to do business.*


*Options trading is not suitable for all investors due to its inherent high risk, which can potentially result in significant losses. Please read*[Characteristics and Risks of Standardized Options](https://www.theocc.com/company-information/documents-and-archives/options-disclosure-document?ref=alpaca.markets) *before investing in options.*


*This material is for informational purposes only and is not an offer, solicitation, or recommendation to buy or sell any security. Brokerage services are provided by Alpaca Securities LLC, member FINRA/SIPC, a wholly-owned subsidiary of AlpacaDB, Inc.*


*For additional information and disclosures, see*[Alpaca's Disclosure Library](https://alpaca.markets/disclosures?ref=alpaca.markets) *.*
