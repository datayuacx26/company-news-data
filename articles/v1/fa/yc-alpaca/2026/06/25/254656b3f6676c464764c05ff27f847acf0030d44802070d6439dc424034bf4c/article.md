---
schema_version: "1.0.0"
document_id: "254656b3f6676c464764c05ff27f847acf0030d44802070d6439dc424034bf4c"
company_key: "yc-alpaca"
company: "Alpaca"
source_id: "yc-alpaca-rss-4f5e7678c8d4"
canonical_url: "https://alpaca.markets/blog/htb-trading-api-locates/"
published_at: "2026-06-24T13:25:51+00:00"
first_seen_at: "2026-07-20T23:23:37.146111+00:00"
fetched_at: "2026-07-28T20:48:18.651537+00:00"
content_hash: "sha256:8805a35c4f64c617affc9598c2921ba52570f40d02622e91ccdf08df20bfbe4c"
---

# Alpaca Launches Hard-to-Borrow Short Selling Featuring Full API Support for Locates

We’ve launched Hard-to-Borrow (HTB) trading, featuring full API support for locates for traders. This launch expands our short-selling infrastructure beyond[easy-to-borrow (ETB)](https://alpaca.markets/blog/zero-borrow-fees-on-short-selling-etb-stock-shares-alpaca-trading-api/) shorting into a more flexible, fully-automated workflow, bringing advanced short-selling functionality directly into automated trading workflows.Now, traders can locate, execute, and track advanced short-selling workflows through a unified API experience.


For active traders, quantitative teams, algorithmic strategies, and trading operations teams, HTB short selling has historically introduced operational complexity. Locate workflows have often depended on manual broker touchpoints, separate locate desks, or tools that sit outside the core trading system.


Alpaca’s HTB capabilities and API support for locates workflow into the same[Alpaca Trading API](https://alpaca.markets/algotrading?ref=alpaca.markets) ecosystem traders already use for research, positions, orders, account activity, and execution. The result is a programmatic workflow to check borrow availability, control locate costs, and move from signal to short-sale order.


"Bringing HTB locates into Alpaca's trading infrastructure significantly expands and enhances Alpaca's short-selling capabilities for active traders, algorithmic strategies, and platforms building modern short-selling workflows," said Michael Fonesca, Alpaca's VP of Securities Finance.


## **What Is Short Selling, and Where Do Locates Fit?**


Short selling involves selling a security the seller does not own, which requires locating and borrowing stock. In a typical short sale, a trader borrows shares, sells them in the market, and later buys shares back to close the position.


Because short sales depend on the ability to deliver borrowed shares, US market structure includes rules designed to address delivery and locate requirements. Under Regulation SHO, before accepting or effecting a short sale order in an equity security, a broker-dealer generally must have borrowed the security, entered into a bona fide arrangement to borrow it, or have reasonable grounds to believe the security can be borrowed and delivered when delivery is due.


That “reasonable grounds” concept is where locate workflows become important. A locate helps establish whether shares are available to borrow before a short sale is opened.


## **ETB vs. HTB: When Are Locates Required?**


Easy-to-borrow (ETB) securities are securities where borrow availability is generally accessible. Alpaca supports ETB shorting with $0 borrow fees, and Alpaca’s existing 5,000+ ETB inventory remains available with $0 borrow fees.


Hard-to-borrow (HTB) securities are different. Borrow supply may be limited because of existing short-selling demand, market events, security-specific constraints, or limited inventory availability. When a symbol is HTB, a locate is required before opening a short position.


This distinction matters, especially for automated trading systems.


**Alpaca’s Design: Locates Built Into the Trading Workflow**


HTB shorting at many other brokerages rely on manual touchpoints, separate locate desks, or platform-specific tools. That can create friction for traders monitoring many symbols, reacting to market-moving events, or managing short-selling workflows across internal systems.


Alpaca’s design brings HTB locate functionality into the same API-based workflow as quotes, orders, account activity, and asset data. This helps traders and platforms evaluate borrow availability, manage locate costs, and prepare short-sale orders without switching between disconnected systems.


### **Automation and Scale for Programmatic Trading**


Market dynamics shift fast. A symbol that is ETB today can easily become HTB tomorrow. Traders, platforms, and trading operations teams can monitor symbols, evaluate borrow status, request locates, check pricing, and prepare order workflows programmatically.


This can reduce reliance on manual locate desks or platform-specific tools, especially for strategies that monitor broad watchlists, respond to signals intraday, or need to adapt as borrow conditions change.


### **Transparent Pricing with Programmatic Controls**


Alpaca provides upfront transparency by delivering exact availability, fee-per-share, and total-fee information directly in the API response. Traders can pass optional` limit_price` and` all_or_none` parameters to ensure a strategy never executes a short sale that is out of the cost thresholds or risk parameters.


### **Coverage for Overnight and Event-Driven Workflows**


Market-moving events don’t stick to standard market hours. With locate available across 24/5 trading windows (8:00 PM ET Sunday night to 8:00 PM ET Friday night), traders and their automated strategies can respond to overnight trading, earnings releases, macro headlines, and global market shifts during overnight trading sessions.


### **Built for Developer-First and Agent-Enabled Trading Systems**


Structured API responses can power automated strategies, risk dashboards, trading tools, and agent-assisted workflows. Systems can interpret borrow status, prepare locate requests, explain rejections, track total fees, and execute short selling.


The workflow is designed to keep execution controls with the trader, platform, or approved automation layer while giving applications the data they need to reason about HTB short-selling decisions. With the launch, HTB functionality has also been added to[Alpaca’s MCP Server](https://alpaca.markets/mcp-server?ref=alpaca.markets) and[CLI](https://alpaca.markets/blog/alpaca-introduces-cli-for-trading-api/) , making it easy for agents to get started.


## **How Alpaca’s Locates Workflow Works**


Alpaca’s API support for locates is designed to support the core HTB workflow, from checking borrow status to evaluating locate pricing, reserving inventory, and tracking locate records. The locate flow is direct: quotes provide pricing and availability signals, while a create request records and reserves a locate when successful.


### **1. Check Borrow Status**


Before entering the locate workflow, systems can use the Assets endpoint to determine whether a symbol is easy-to-borrow or hard-to-borrow.


```text
GET /v2/assets/{symbol}
```


Example (for illustration purpose only):


```text
GET /v2/assets/TSLA
```


Sample response:


```text
{
"id": "b6d1aa75-5c9c-4353-a305-9e2caa1925ab",
"class": "us_equity",
"exchange": "NASDAQ",
"symbol": "TSLA",
"name": "Tesla, Inc.",
"status": "active",
"tradable": true,
"shortable": true,
"borrow_status": "hard_to_borrow"
}
```


The` borrow_status` field helps systems branch between workflows.` easy_to_borrow` indicates that the security is` easy-to-borrow` and can generally follow the existing shorting path where supported.` hard_to_borrow` indicates that the security is` hard-to-borrow` and requires a locate before opening a short position.


**2. Preview Locate Quotes**


For HTB symbols, traders can preview current account-specific availability and pricing.


```text
GET /v1/locates/quotes?symbols=TSLA,AAPL
```


Sample response:


```text
{
"quotes": [
{
"symbol": "TSLA",
"available_qty": 1000,
"price": "0.0123",
"quoted_at": "2026-01-02T15:04:05Z"
}
],
"errors": [
{
"symbol": "AAPL",
"code": "easy_to_borrow",
"message": "symbol is easy to borrow"
}
]
}
```


In this example, TSLA returns HTB quote information, while AAPL is identified as easy-to-borrow and does not require the HTB locate workflow.


Quote responses can include both quote objects and per-symbol errors, making it easier for systems to handle mixed ETB and HTB symbol lists in one workflow.


### **3. Request a Locate**


When a quote satisfies the trader’s availability and cost criteria, the trader can request a locate.


```text
POST /v1/locates
```


```text
{
"symbol": "TSLA",
"qty": 100,
"limit_price": "0.05",
"all_or_none": true
}
```


Locate requests must be submitted in round lots of 100 shares. The optional` limit_price` field lets traders set a maximum acceptable fee per share, while all_or_none can help enforce quantity requirements.


Sample active response:


```text
{
"id": "550e8400-e29b-41d4-a716-446655440000",
"symbol": "TSLA",
"requested_qty": 100,
"limit_price": "0.05",
"all_or_none": true,
"status": "active",
"created_at": "2026-01-02T15:04:05Z",
"located_qty": 100,
"located_price": "0.05",
"total_fee": "5.00",
"expires_at": "2026-01-03T01:00:00Z"
}
```


A recorded locate request returns either` active` or` rejected` . Active locates include fields such as` located_qty` ,` located_price` ,` total_fee` , and` expires_at` . If a locate cannot be completed, the response returns a rejected status with the relevant reason.


### **4. Track Locate Status and History**


Traders and platforms can retrieve locate records for monitoring, reconciliation, and auditability.


```text
GET /v1/locates?status=active&symbol=TSLA&limit=100
```


Sample response:


```text
{
"locates": [
{
"id": "550e8400-e29b-41d4-a716-446655440000",
"symbol": "TSLA",
"requested_qty": 100,
"limit_price": "0.05",
"all_or_none": true,
"status": "active",
"created_at": "2026-01-02T15:04:05Z",
"located_qty": 100,
"located_price": "0.05",
"total_fee": "5.00",
"expires_at": "2026-01-03T01:00:00Z"
}
],
"next_page_token": null
}


```


Systems can also retrieve a single locate by ID:


```text
GET /v1/locates/{locate_id}
```


### **Common Workflow Patterns**


The same API surface supports several HTB trading patterns:


**Borrow Status to Locate:** Check whether a symbol is ETB or HTB through the Assets endpoint. Route ETB symbols through the existing shorting workflow where available, and route HTB symbols into the locate workflow.


**Quote to Locate:** Fetch an indicative locate quote, evaluate availability and price, then submit a locate request with a` limit_price` when the quote satisfies the strategy’s cost controls.


**Locate to Short-Sale Order:** Once a locate is` active` , use the locate details, including` located_qty` ,` located_price` ,` total_fee` , and` expires_at` , to support the next step in the short-sale workflow.


Availability and pricing can change quickly. Quotes are advisory and do not reserve shares. A request may be rejected if inventory is unavailable, the available price exceeds` limit_price` , or another locate condition prevents completion.


### **Example Agent-Assisted Workflow**


The same workflow can support agent-assisted trading operations. For example, a trader or operations team could instruct an approved internal agent to prepare locate decisions using predefined controls:


```text
You are my trading operations agent. For a symbol in my short-selling watchlist, check whether it is easy-to-borrow or hard-to-borrow using Trading API’s Assets endpoint.


If the symbol is easy-to-borrow, execute a short sell order.


If the symbol is hard-to-borrow, first check the current locate quote and availability. Only continue if there is enough availability and the locate fee is below $0.10 per share. Then prepare a locate request.


Ask for my approval before submitting any locate request or short-sale order. After the short-sale order is executed, check the active locate record and report the remaining available locate quantity, located price, total fee, expiration time, and any relevant status details.
```


In this workflow, the agent can review structured API responses, explain rejections, prepare locate requests based on user-defined constraints, and keep the human or system of record in control of execution permissions.


## **Get Started**


With hard-to-borrow trading and API support for locates, Alpaca expands short selling beyond its 5,000+ easy-to-borrow securities with $0 borrow fees. Active traders can now build both ETB and HTB workflows within the same Alpaca ecosystem, programmatically through APIs or with AI agents.


Ready to integrate HTB into your trading systems?


- [Create a Trading API account](https://app.alpaca.markets/signup?utm_source=blog&utm_medium=blog&utm_campaign=q226&utm_content=htb-trading-api-locates)
- [Explore the HTB and locate documentation](https://docs.alpaca.markets/us/docs/margin-and-short-selling?ref=alpaca.markets)
- [Set up Alpaca's MCP Server](https://docs.alpaca.markets/us/docs/alpaca-mcp-server?ref=alpaca.markets)
- [Get started with Alpaca's CLI](https://docs.alpaca.markets/us/docs/alpacas-cli?ref=alpaca.markets)


---


*Orders placed outside regular trading hours (9:30 a.m. – 4:00 p.m. ET) may experience price fluctuations, partial executions, or delays due to lower liquidity and higher volatility.*


*Orders not designated for extended hours execution will be queued for the next trading session.*


*Additionally, fractional trading may be limited during extended hours. For more details, please review*[Alpaca Extended Hours & Overnight Trading Risk Disclosure](https://files.alpaca.markets/disclosures/library/ExtHrsOvernightRisk.pdf?ref=alpaca.markets) *.*


*Margin trading involves significant risk and is not suitable for all investors. Before considering a margin loan, it is crucial that you carefully consider how borrowing fits with your investment objectives and risk tolerance.*


*When trading on margin, you assume higher market risk, and potential losses can exceed the collateral value in your account. Alpaca may sell any securities in your account, without prior notice, to satisfy a margin call. Alpaca may also change its “house” maintenance margin requirements at any time without advance written notice. You are not entitled to an extension of time on a margin call. Please review the Firm’s*[Margin Disclosure Statement](https://files.alpaca.markets/disclosures/library/MarginDiscStmt.pdf?ref=alpaca.markets) *before investing.*


Short selling involves significant risk, including the potential for losses that exceed the initial investment. HTB availability, locate availability, borrow availability, and pricing can change quickly. Locate requests may be rejected if inventory is unavailable, pricing exceeds a submitted limit price, or another locate condition prevents completion. Locate fees are not credited back if a locate is not used to short the stock. Active locates are single use, and a new locate is required before entering a new short position after covering.


*Past hypothetical backtest results do not guarantee future returns, and actual results may vary from the analysis.*


*All investments involve risk and the past performance of a security, or financial product does not guarantee future results or returns. There is no guarantee that any investment strategy will achieve its objectives. Please note that diversification does not assure a profit, or protect against loss. There is always the potential of losing money when you invest in securities, or other financial products. Investors should consider their investment objectives and risks carefully before investing.*


*The content of this article is for general information only and is believed to be accurate as of posting date but may be subject to change. Alpaca does not provide investment, tax, or legal advice. Please consult your own independent advisor as to any investment, tax, or legal statements made herein.*


*This is not an offer, solicitation of an offer, or advice to buy or sell securities or open a brokerage account in any jurisdiction where Alpaca Securities is not registered or licensed, as applicable.*


*Please see the*[Disclosure Library](https://alpaca.markets/disclosures?ref=alpaca.markets) *for additional information and disclosures from Alpaca Securities.*
