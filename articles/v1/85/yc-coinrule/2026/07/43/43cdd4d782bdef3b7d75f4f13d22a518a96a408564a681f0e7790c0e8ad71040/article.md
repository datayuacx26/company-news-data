---
schema_version: "1.0.0"
document_id: "43cdd4d782bdef3b7d75f4f13d22a518a96a408564a681f0e7790c0e8ad71040"
company_key: "yc-coinrule"
company: "Coinrule"
source_id: "yc-coinrule-rss-98629e270b11"
canonical_url: "https://coinrule.com/blog/ai/coinrule-mcp-ai-trading-agent/"
published_at: "2026-07-10T16:41:21+00:00"
first_seen_at: "2026-07-20T23:23:45.191933+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:25de5f5bd1d5eb8ddbcbb46e20d31fa56924c891255f18dabfb89216d010bd47"
---

# Coinrule MCP, Trade on ChatGPT, Claude, Gemini and Grok

### **Coinrule MCP is the official Model Context Protocol server that connects Coinrule Cloud to ChatGPT, Claude, Grok and other MCP-compatible AI assistants. It lets users read portfolio balances, inspect trading agents, create and manage strategies, run backtests, and launch multi-asset baskets through natural-language conversations.**


For years, traders have had to adapt their ideas to software: learning where every setting lives, translating an investment thesis into rigid fields, and moving between dashboards before a strategy can be tested or launched.


[Coinrule MCP](https://cloud.coinrule.com/docs/mcp) reverses that relationship. Instead of starting with a form, you start with intent. You describe what you want to understand, test or automate, and your AI assistant selects the relevant Coinrule tools on your behalf.


> *“Show my active Coinrule strategies, summarise today’s PnL and tell me which strategy most recently generated a signal.”*


The assistant handles the conversation. Coinrule remains the execution, validation, portfolio and market-connectivity layer underneath it. Strategies created through chat appear in the standard Coinrule Agents dashboard, so users keep a single view of their automation across chat and web.


# What is the Coinrule MCP?


MCP stands for Model Context Protocol, an open standard that allows an AI assistant to connect to external software and use authorised tools. Coinrule operates an official remote MCP server for Coinrule Cloud accounts.


Once connected, a compatible assistant can call Coinrule functions on the user’s behalf. The user does not need to know the tool names or API structure. They can ask in plain English, and the assistant chooses the correct action.


> Coinrule MCP turns ChatGPT, Claude, Grok and other compatible assistants into conversational interfaces for Coinrule trading automation.


The official MCP server address is:


[https://cloud.coinrule.com/mcp](https://cloud.coinrule.com/mcp)


# What can you do with Coinrule MCP?


Coinrule MCP covers the full workflow from account visibility to strategy execution. Its tools are grouped across strategy monitoring, portfolio data, strategy creation, templates, baskets and backtesting.


## 1. Read balances, holdings and portfolio allocation


Ask your assistant for cash balances, holdings, total portfolio value and allocation across connected exchanges and brokers. The MCP can also identify connected venue accounts, which are required when launching live strategies.


> *“Show my current holdings across every connected account, rank my five largest allocations and tell me how much cash is available.”*


## 2. Inspect live strategies and trading activity


The assistant can list strategies and retrieve configuration, status, open positions, PnL, paper balance, trigger proximity, activity logs, trades and PnL history. It can also read a combined activity feed across all strategies.


> *“What have my Coinrule bots done in the last 24 hours? Summarise signals, completed trades, open positions and PnL.”*


## 3. Create trading strategies from natural language


With Read + Write permission, users can describe a trading idea in ordinary language and ask Coinrule to validate, test and launch it. Coinrule supports both a staged workflow and a one-step workflow.


- Staged workflow: validate the prompt or script, generate tests, review the result, then create the strategy on a selected venue.
- One-step workflow: convert a plain-English instruction into a validated, tested and launched strategy in one request.


> *“On paper trading, create a BTC/USDT strategy using $500. Buy when RSI(14) falls below 30 on the one-hour timeframe. Take profit at 7% and stop loss at 4%. Validate and backtest it before launch.”*


## 4. Update, pause, resume or stop strategies


The[MCP](https://coinrule.com/mcp) can update a strategy configuration, redeploy it, resume a stopped strategy, or stop and pause a running agent. A stop request can optionally close open positions, so users should state their intent clearly.


> *“Pause my ETH strategy but keep any open positions unchanged.”*


## 5. Browse Coinrule strategy templates


The Script Library can be explored through chat. Users can browse pre-built approaches such as RSI, DCA, TradingView and basket strategies, filter them by level, and use a template as the basis for a new agent.


> *“Show me beginner-friendly DCA templates, explain the differences and launch the best fit on paper trading with $300.”*


## 6. Build and launch multi-asset baskets


[Coinrule MCP](https://mcpmarket.com/server/coinrule-mcp-agentic-trading) can browse curated baskets, search the supported asset universe, create custom baskets containing two to 20 assets and assign target weights. A basket can be launched on one venue or across several venues.


For multi-venue baskets, Coinrule maps each basket component to the matching connected exchange or broker account rather than assuming every asset belongs on one platform.


> *“Build an equally weighted basket of BTC, ETH and SOL, rebalance it weekly and launch it with $1,000 on paper trading.”*


## 7. Run and compare backtests


The[MCP](https://medium.com/coinrule/3-best-trading-mcp-servers-for-ai-agents-coinrule-vs-binance-vs-coingecko-90ec3a3f70ee) can list saved backtests, retrieve a backtest summary, access raw trade exports, run a new historical simulation and compare several parameter scenarios in one pass.


> *“Backtest a BTC RSI mean-reversion strategy on the one-hour timeframe over the last year, then compare 2%, 4% and 6% stop-loss variants.”*


# Coinrule MCP capabilities at a glance


**Capability** **Example action** **Read only** **Read + Write**


Strategies View status, activity, trades and PnL Yes Yes


Portfolio Read balances, holdings and allocations Yes Yes


Signals Review recent buy and sell signals Yes Yes


Templates Browse the Script Library Yes Yes


Strategy creation Validate and launch a new bot No Yes


Strategy control Update, start, pause or stop a bot No Yes


Baskets Create and launch multi-asset baskets Browse only Yes


Backtests Review saved runs or start new tests Review only Yes


# Supported venues and asset types


Through the MCP, an assistant can trade on supported venues connected to the user’s Coinrule account, including Binance, Coinbase, OKX, Kraken spot, Kraken perpetuals, Hyperliquid, KuCoin and Trading 212, Webull, Alpaca, Public. Coinrule supports crypto spot, perpetuals, stocks and paper-trading equivalents within the relevant connected-account setup.


Venue availability may expand over time, so the live Coinrule documentation should remain the source of truth for the latest integrations.


# How to connect ChatGPT, Claude, Grok or another MCP client


1. Create or sign in to a[Coinrule Cloud](https://cloud.coinrule.com/) account.
2. Open the apps, connectors or integrations area inside your AI assistant.
3. Add a custom or remote MCP server using https://cloud.coinrule.com/mcp.
4. Complete the Coinrule sign-in flow. Authentication happens through Coinrule using OAuth 2.1.
5. Choose Read only or Read + Write access.
6. Test the connection by asking the assistant to list your Coinrule strategies or show your balances.


A connected exchange or broker is required for live execution. Paper trading can be used without a live exchange connection.


# Read only vs Read + Write permissions


Coinrule uses scoped access. The user approves the permission level on Coinrule’s own consent screen.


**Access level** **OAuth scope** **What it allows**


Read only coinrule:read Inspect strategies, balances, holdings, trades, signals, PnL and backtests. It cannot place or change trades.


Read + Write coinrule:read + coinrule:write Everything in Read only, plus create, launch, update, start and stop strategies, launch baskets and run backtests.


**Important**


Read + Write can launch strategies that trade real funds on a connected live account. Start with Read only or paper trading while testing the workflow.


# How Coinrule MCP security works


- OAuth 2.1 authentication: the AI assistant does not receive the user’s Coinrule password or exchange API keys.
- Scoped permissions: write tools are hidden unless coinrule:write was explicitly granted.
- API-only access: the MCP acts through the Coinrule API and has no direct database, Redis or exchange-key access.
- Expiring and rotating tokens: access tokens are short-lived and refresh tokens rotate.
- Per-user rate limits: requests are rate-limited to help protect the account.
- Instant revocation: connections can be reviewed and revoked from Settings → Integrations in Coinrule.


# The best workflow for AI-assisted trading


AI can make strategy creation faster, but it should not remove review and risk control. A strong process keeps the trader in the loop at each consequential stage.


**Define the objective:** State whether the strategy is designed for accumulation, momentum, mean reversion, hedging or rebalancing.


**Specify the details:** Name the asset, venue, timeframe, capital, entry, exit, position sizing and risk limits.


**Ask for an explanation:** Request a plain-language summary of the strategy logic and its main assumptions.


**Validate the strategy:** Check that the generated structure matches the original intent.


**Run backtests:** Test historical performance and compare several parameter variants.


**Use paper trading:** Observe behaviour in current conditions without risking real capital.


**Move gradually:** Only consider live execution after review, starting with limited capital and ongoing monitoring.


# How to write better[Coinrule MCP](https://github.com/coinrule-com/coinrule-mcp-ai-trading) prompts


Good prompts are specific enough to remove ambiguity while leaving room for the assistant to explain and validate the result. Include the following whenever relevant:


- Asset or trading pair
- Exchange, broker or paper venue
- Live or paper execution
- Capital allocation
- Timeframe and indicators
- Entry and exit conditions
- Position sizing
- Stop-loss and take-profit rules
- Rebalancing frequency for baskets
- A request for validation and backtesting


**Weak prompt**


Make me a Bitcoin bot.


**Stronger prompt**


Create a paper-trading BTC/USDT strategy using $500. Buy when RSI(14) falls below 30 on the one-hour timeframe and price remains above the 200-period moving average. Take profit at 7%, stop loss at 4%, validate the logic and backtest it over the last 12 months.


# Copy-and-paste Coinrule MCP prompts


## Portfolio analysis


*“Summarise my portfolio across all connected accounts.”*


*“What are my five largest holdings and how much cash is available?”*


*“Show my current allocation by asset and by venue.”*


## Strategy monitoring


*“Show all active strategies and their current PnL.”*


*“Which bot most recently generated a signal?”*


*“Summarise strategy activity from the last seven days.”*


## Strategy creation


*“Create a weekly ETH DCA strategy using $100 per purchase on paper trading.”*


*“Create a BTC momentum strategy that buys after a 20/50 moving-average crossover and exits on the opposite crossover. Backtest it first.”*


## Backtesting


*“Backtest this strategy over the last two years and summarise return, drawdown and number of trades.”*


*“Compare three take-profit variants and explain the trade-off between return and drawdown.”*


## Baskets


*“Create an equally weighted basket of BTC, ETH, SOL and LINK.”*


*“Show curated AI-related baskets and explain their composition.”*


## Strategy management


*“Pause my BTC strategy without closing its position.”*


*“Update my ETH strategy to take profit at 8% instead of 6%.”*


# Common Coinrule MCP connection problems


**ChatGPT shows invalid_client:** Remove the Coinrule connector and add it again. This forces a fresh OAuth registration.


**The server URL is rejected:** Use exactly https://cloud.coinrule.com/mcp with no extra path or query string.


**The sign-in flow loops:** Confirm the correct Coinrule account, allow pop-ups and redirects, then remove and reconnect if the client cached an incomplete attempt.


**The assistant can read but cannot trade:** Reconnect and select Read + Write. Write tools are intentionally hidden on a read-only connection.


**Access stopped working:** Check whether the connection was revoked. If token refresh failed, remove and reconnect.


**A multi-venue basket reports a missing account:** Connect an account for each exchange or broker used by the basket, then retry.


**The assistant chooses the wrong action:** State the venue, amount, asset and whether the request is paper or live.


# Frequently asked questions


## What is Coinrule MCP?


Coinrule MCP is the official Model Context Protocol server that connects Coinrule Cloud to compatible AI assistants. It allows users to inspect accounts, create and manage strategies, run backtests and launch baskets through chat.


## Can ChatGPT trade through Coinrule?


Yes. When ChatGPT is connected to Coinrule MCP with Read + Write permission, it can use authorised Coinrule tools to create, launch and manage strategies. Live execution requires a connected exchange or broker.


## Does Coinrule MCP work with Claude and Grok?


Yes. Coinrule provides[connection guidance for ChatGPT](https://cloud.coinrule.com/docs/mcp/connecting-your-ai-assistant) , Claude and Grok, and it supports other clients that can connect to a remote MCP server using OAuth.


## Can Coinrule MCP trade crypto and stocks?


Coinrule MCP can access supported crypto spot, perpetual and stock venues connected to the user’s Coinrule account, as well as paper-trading equivalents, It is a fully-fledged offering, as explained on the “[Best MCPs for Trading](https://medium.com/coinrule/3-best-trading-mcp-servers-for-ai-agents-coinrule-vs-binance-vs-coingecko-90ec3a3f70ee) ” article


## Can I use Coinrule MCP without risking money?


Yes. Add “on paper” or “use paper trading” to launch prompts. Paper trading can be used without connecting a live exchange.


## Is Coinrule MCP safe?


Coinrule MCP uses OAuth 2.1, scoped permissions, expiring tokens and per-user rate limits. The assistant does not receive the user’s Coinrule password or exchange API keys, and access can be revoked instantly.


## What is the difference between Read only and Read + Write?


Read only can inspect strategies, balances, holdings, trades, signals, PnL and backtests. Read + Write can also create and manage strategies, launch baskets and run new backtests.


## Can an AI assistant withdraw funds?


**No** ,[Coinrule MCP](https://github.com/coinrule-com/coinrule-mcp-ai-trading) is designed around Coinrule strategy and portfolio functionality. Users should configure exchange API permissions securely and **keep withdrawal permissions disabled.**


## Where do strategies created through chat appear?


They appear in the standard Coinrule Agents dashboard, where they can be reviewed and managed alongside strategies created in the web app.


## What is the Coinrule MCP server URL?


The official server URL is[https://cloud.coinrule.com/mcp](https://cloud.coinrule.com/docs/mcp)


# The beginning of conversational execution


[Coinrule MCP](https://medium.com/coinrule/3-best-trading-mcp-servers-for-ai-agents-coinrule-vs-binance-vs-coingecko-90ec3a3f70ee) is more than a new integration. It changes the interface between a trader’s intent and automated execution.


The traditional workflow begins with menus, forms and configuration. The conversational workflow begins with a question or an idea: inspect an account, describe a strategy, test it, improve it, launch it and monitor it—all through the AI assistant the user already works with.


Coinrule remains the controlled infrastructure beneath that conversation: validating strategies, connecting to supported markets, tracking activity and giving users permissioned control over their agents.


**[Coinrule MCP](https://mcp.so/servers/coinrule-mcp) is now live for ChatGPT, Claude, Grok and other compatible AI assistants. Connect your assistant, begin with Read only access or paper trading, and turn your next trading idea into a conversation.**


Trading involves substantial risk and may result in the loss of capital. Backtests, simulations and past performance do not guarantee future returns. AI-generated strategies and analysis may contain errors and should be independently reviewed before use. Nothing in this article constitutes financial, investment or trading advice.


**Learn more:**[Coinrule MCP documentation](https://cloud.coinrule.com/docs/mcp)
