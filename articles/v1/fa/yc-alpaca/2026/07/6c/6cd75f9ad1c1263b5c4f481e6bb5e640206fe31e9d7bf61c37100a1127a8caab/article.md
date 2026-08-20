---
schema_version: "1.0.0"
document_id: "6cd75f9ad1c1263b5c4f481e6bb5e640206fe31e9d7bf61c37100a1127a8caab"
company_key: "yc-alpaca"
company: "Alpaca"
source_id: "yc-alpaca-rss-4f5e7678c8d4"
canonical_url: "https://alpaca.markets/blog/lamatrader-launches-on-alpaca-connect-marketplace/"
published_at: "2026-07-09T14:54:38+00:00"
first_seen_at: "2026-07-20T23:23:37.146111+00:00"
fetched_at: "2026-07-28T20:43:05.676547+00:00"
content_hash: "sha256:beb8adcf5254088527e24554786016dcdf765c8352e98180958e6a9c1d40218e"
---

# LamaTrader Launches on Alpaca Connect Marketplace with their Integrated Trader Environment

We’re excited to welcome[LamaTrader](https://lamatrader.com/?ref=alpaca.markets) to the[Alpaca Connect Marketplace](https://alpaca.markets/oauth?ref=alpaca.markets) . LamaTrader is an independent trading workspace that connects to a user's existing Alpaca brokerage account through Alpaca Connect (OAuth).


Connecting with Alpaca’s[Trading API](https://alpaca.markets/algotrading?ref=alpaca.markets) and[Market Data API](https://alpaca.markets/data?ref=alpaca.markets) , LamaTrader introduces what it calls an Integrated Trader Environment (ITE): a customizable workspace that brings research, charting, portfolio management, AI-powered analysis, and access to trading functionality available through a connected Alpaca account into a single interface by their users.


Additionally, using Lama AI, their users can create custom trading skills, generate widgets, and design dashboards that adapt to their workflow. The platform is intended to serve a range of users, from those placing their first trades to those running algorithmic strategies.


## **Engineering An Integrated Trader Environment**


Many traders work across a patchwork of tools: a charting platform in one tab, a broker’s order form in another, a spreadsheet for tracking P&L, and a separate app for news. Every context switch costs time and focus.


LamaTrader addresses this with a customizable workspace built from configurable dashboards. After securely connecting an Alpaca brokerage account through OAuth, users can access their account information, portfolio, Alpaca market data, charts, widgets, and AI tools within a single workspace.


A trader new to the markets can begin in a simulated paper trading account, while a quantitative trader can describe a strategy in plain language and receive generated code to review. The same environment is designed to support both workflows. Instead of moving between disconnected applications, LamaTrader allows authorized account information, Alpaca market data, charts, dashboards, and AI tools to operate together within one interface.


"Traders shouldn't have to juggle five disconnected tools to make one decision. We brought AI, Alpaca market data, account information, and order management into one environment that works together and adapts to each trader. LamaTrader is a complete Integrated Trader Environment, built for the age of AI, and Alpaca's APIs made it possible to build it right," said Dr. Huseyn Guliyev, Founder of LamaTrader


## **What You Can Do with LamaTrader**


### **Trade Many Asset Classes**


Through a connected Alpaca account, LamaTrader provides a unified interface for viewing and trading supported US stocks, ETFs, options, and cryptocurrency to their users.


- **Stocks & ETFs** : US equities and ETFs with fractional share support and extended hours trading
- **Options:** Multi-expiry options chains with live Greeks (Delta, Gamma, Theta, Vega, Rho), plus a visual multi-leg strategy builder that auto-detects iron condors, iron butterflies, straddles, credit spreads, debit spreads, calendar spreads, and others
- **Cryptocurrency:** BTC, ETH, USDC, and other supported cryptocurrencies and stablecoins
- **Paper Trading** : A simulated account environment for learning the platform and testing strategies without using real capital


Caption: Example of options trading within the LamaTrader interface. For illustrative purposes only. Not investment advice.


### **Advanced Order Management**


LamaTrader provides an interface for submitting orders through Alpaca's brokerage infrastructure using the user's connected Alpaca account. Available functionality includes market, limit, and trailing-stop orders, brackets, OCO, and OTO, multi-leg options orders, time-in-force selections, and extended-hours trading coverage where supported by Alpaca. Newer users may use a simplified order ticket, while experienced users can access additional order functionality.


### **Lama AI: An Assistant for Your Connected Account**


With a user’s authorization through Alpaca Connect, Lama AI can access permitted account information, including positions, open orders, watchlists, balances, and market data, to provide responses based on the connected account. Users may choose among multiple AI models and providers, such as Google, Anthropic, and OpenAI.


Because Lama AI can read the connected account, a trader can give it instructions in plain language:


- "Build me a risk dashboard showing my current positions, sector exposure and P&L."
- "Calculate a position size that would risk approximately 1% of my account based on a hypothetical entry and stop."
- "Help me prepare a collar strategy for my existing MSFT position."
- "Create a skill that scans my watchlist for unusual volume and ranks securities by RSI."


Caption: Example of Lama AI skill-building interface. For illustrative purposes only. Not investment advice.


Beyond answering questions, Lama AI can prepare workflows, generate code, create widgets, and skills, and prepare orders for their user’s review before submission through the connected Alpaca account. Every order must be reviewed and approved by the user before submission.


A note on working with AI: these models can generate a wide range of suggestions quickly. Like any financial information, the assistant's output should be understood before it is acted on, and is not a recommendation to follow without review. The platform is designed to keep the trader informed and in control. Orders are reviewed and approved before they are placed, visuals present the underlying data clearly, Code Mode turns one-off suggestions into repeatable, reviewable logic, and Alpaca's paper trading allows strategies to be tested with simulated funds before any real capital is used.


### **Adaptive by Design**


LamaTrader is designed to be extended by the user.


#### **AI Code Mode**


With Code Mode, users can describe a workflow in plain language and Lama AI generates a sandboxed JavaScript skill for review before execution. Approved skills may access Alpaca APIs, interact with LamaTrader dashboards, retrieve authorized account information, and prepare orders for user review through the connected Alpaca account. Because approved skills are saved, they execute consistently each time they are run.


For example, a user might request: "Display the top 10 trending stocks and prepare an order to purchase $1,000 of the highest-ranked security.”


The generated code can be reviewed, tested in Alpaca Paper Trading, and modified before any live order is submitted.


Caption: Example of stock trading interface. This is not investment advice.


#### **Dashboards**


Users can build custom workspaces from more than 50 configurable widgets, including analytics, charts, screeners, options chains, news, and portfolio views, and account information available through their connected Alpaca account.


#### **AI-Generated Widgets**


When the built-in functionality is not sufficient, Lama AI can generate additional widgets that connect to Alpaca APIs to display market data and authorized account information within custom dashboards.


#### **Review Custom Development.**


Generated skills and widgets can be inspected, edited, and reviewed before use. LamaTrader also offers custom widget and skill development services for users requiring additional functionality.


Over time, users can build up a personal library of reviewed skills, custom dashboards, and added widgets.


## **Why Alpaca**


Alpaca Connect enables third-party applications like LamaTrader to allow their users to securely connect to their existing Alpaca brokerage account using OAuth.


Alpaca continues to provide:


- Brokerage accounts
- Custody of customer assets
- Market data
- Order execution
- Access to supported stocks, ETFs, options, and cryptocurrency through Alpaca's APIs
- Paper trading
- Fractional shares
- Extended-hours trading


LamaTraders builds on those services by providing the user interface, dashboards, AI tool, analytics and workflow automation. Customer brokerage accounts, custody of assets, market data, and order execution remain within Alpaca's regulated infrastructure.


“When we set out to rethink the trading experience from the ground up, we needed an infrastructure that would let us build it right. Alpaca was the clear choice: developer-first, and equipped with a rich Market Data API that lets us offer stocks, ETFs, options, and crypto through one unified interface without stitching together multiple brokers. Your assets stay with Alpaca. We just build the experience on top,” said Dr. Huseyn Guliyev, Founder, LamaTrader.


**LamaTrader is an independent third-party application available through the Alpaca Connect Marketplace. Customers who choose to use LamaTrader may connect their existing Alpaca brokerage account using Alpaca Connect (OAuth). Alpaca does not operate or control the LamaTrader platform.**


To connect an Alpaca account:


1. Visit[LamaTrader.com](https://lamatrader.com/?ref=alpaca.markets) .
2. Sign in to LamaTrader via your email
3. Link your paper/trading accounts using Alpaca Connect (OAuth), if you choose to authorize the connection


Interested in learning more about building an app for the Alpaca Marketplace? Contact our team by filling out the form below.


## **Contact Alpaca**


---


*This content is provided for informational purposes only.. Any examples or illustrations are provided for informational and illustrative purposes only and do not constitute investment, tax, legal, or a recommendation to buy or sell any security or cryptocurrency. Please consult your own professional advisors regarding your individual circumstances.*


*LamaTrader is an independent third-party application available through the Alpaca Connect Marketplace. Alpaca and LamaTrader are not affiliated. Alpaca does not control or operate the LamaTrader platform and is not responsible for LamaTrader's products, services, security, or content.*


*Inclusion in the Alpaca Connect Marketplace does not constitute a recommendation, endorsement, or certification by Alpaca of any third-party application. Customers are responsible for evaluating whether a third-party application is appropriate for their needs before choosing to connect their Alpaca account.*


*Options trading is not suitable for all investors due to its inherent high risk, which can potentially result in significant losses. Please read*[Characteristics and Risks of Standardized Options](https://www.theocc.com/company-information/documents-and-archives/options-disclosure-document?ref=alpaca.markets) *before investing.*


*The Paper Trading API is offered by AlpacaDB, Inc. and does not require real money or permit a user to transact in real securities in the market. Providing use of the Paper Trading API is not an offer or solicitation to buy or sell securities, securities derivative or futures products of any kind, or any type of trading or investment advice, recommendation or strategy, given or in any manner endorsed by AlpacaDB, Inc. or any AlpacaDB, Inc. affiliate and the information made available through the Paper Trading API is not an offer or solicitation of any kind in any jurisdiction where AlpacaDB, Inc. or any AlpacaDB, Inc. affiliate (collectively, “Alpaca”) is not authorized to do business.*


*Fractional share trading allows a customer to buy and sell fractional share quantities and dollar amounts of certain securities. Fractional share trading presents unique risks and is subject to particular limitations that you should be aware of before engaging in such activity. See Alpaca Customer Agreement at*[https://alpaca.markets/disclosures](https://alpaca.markets/disclosures?ref=alpaca.markets) *for more details.*


*The testimonials, statements, and opinions presented in this material and on the website are applicable to the specific individuals. It is important to note that individual circumstances may vary, and may not be representative of the experience of others. There are no guarantees of future performance or success. The testimonials are voluntarily provided and are not paid, nor were they provided with free products, services or any other benefit in exchange for said statements.*


*All investments involve risk and the past performance of a security, or financial product does not guarantee future results or returns. There is no guarantee that any investment strategy will achieve its objectives. Keep in mind that while diversification may help spread risk, it does not assure a profit, or protect against loss. There is always the potential of losing money when you invest in securities, or other financial products. Investors should consider their investment objectives and risks carefully before investing.*


*Cryptocurrency is highly speculative in nature, involves a high degree of risks, such as volatile market price swings, market manipulation, flash crashes, and cybersecurity risks. Cryptocurrency regulations are continuously evolving, and it is your responsibility to understand and abide by them. Cryptocurrency trading can lead to large, immediate and permanent loss of financial value. You should have appropriate knowledge and experience before engaging in cryptocurrency trading. For additional information, please click*[here.](https://files.alpaca.markets/disclosures/library/CryptoRiskDisclosures.pdf?ref=alpaca.markets)


*This is not an offer, solicitation of an offer, or advice to buy or sell securities or cryptocurrencies or open a brokerage account or cryptocurrency account in any jurisdiction where Alpaca Securities or Alpaca Crypto, respectively, are not registered or licensed, as applicable.*
