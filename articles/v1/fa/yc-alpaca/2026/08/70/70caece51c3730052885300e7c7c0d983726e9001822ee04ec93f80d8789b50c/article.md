---
schema_version: "1.0.0"
document_id: "70caece51c3730052885300e7c7c0d983726e9001822ee04ec93f80d8789b50c"
company_key: "yc-alpaca"
company: "Alpaca"
source_id: "yc-alpaca-rss-4f5e7678c8d4"
canonical_url: "https://alpaca.markets/blog/alpaca-rebuilds-its-node-js-sdk-with-broader-api-coverage-and-launches-its-first-supported-java-sdk/"
published_at: "2026-08-06T13:00:15+00:00"
first_seen_at: "2026-08-06T14:15:27.112231+00:00"
fetched_at: "2026-08-06T14:15:29.386133+00:00"
content_hash: "sha256:8e39f527cb65b8423eae6e150b6ba437312629b7d4df63aca674fdf5aba988c9"
---

# Alpaca Rebuilds Its Node.js SDK with Broader API Coverage and Launches Its First Supported Java SDK

[Node.js SDK 4.0.1 is the current stable release of Alpaca’s new 4.x client architecture:](https://github.com/alpacahq/alpaca-trade-api-js/releases/tag/v4.0.1?ref=alpaca.markets) *a complete TypeScript rewrite with typed interfaces, a smaller direct runtime dependency set, and a unified surface for Trading and Market Data.*[Java SDK 0.1.3](https://github.com/alpacahq/alpaca-java/releases/tag/v0.1.3?ref=alpaca.markets) *introduces generated clients for Alpaca’s Trading, Market Data, and Broker APIs for Java 17 and later.*


Both SDKs follow a shared architectural pattern. REST clients and models are generated from Alpaca’s OpenAPI specifications, while handwritten layers add higher-level clients, pagination helpers, request controls, error handling, and real-time interfaces.


Together, the releases materially expand the Alpaca capabilities available through typed SDK surfaces. Node.js 4.x brings option-contract reference data, corporate action data, crypto funding, locates, tokenization workflows, account-activity event streams, indices, stock auctions, and exchange and condition metadata into one client. Java launches with REST coverage across Trading, Market Data, and Broker, WebSocket clients for stocks, crypto, news, and trading updates, and SSE support for Broker trade events.


## **Why Upgrade to Node.js 4.0.1?**


**Node.js 4.x is a breaking release.** Applications built on 3.x must update their imports, client construction, method paths, and arguments before upgrading.


Version 4 materially expands the Alpaca capabilities available through one typed, discoverable client. Alongside core account, order, position, and market-data workflows, the SDK provides typed access to:


- Option-contract lookup and reference data
- Corporate action data
- Crypto funding workflows
- Locate requests and quotes
- Tokenization workflows
- Account-activity event streams over Server-Sent Events
- Expanded Market Data access across indices, forex, fixed income, screeners, logos, stock auctions, and exchange and condition metadata


The redesign gives that broader surface a more consistent structure:


- **Clearer client organization:** Trading and Market Data operations live under dedicated` alpaca.trading` and` alpaca.marketData` namespaces.
- **A consistent generated API surface:** REST operations, request types, response types, and models are generated from the versioned Alpaca OpenAPI specification snapshots pinned for the release.
- **Focused workflow helpers:** Order builders, pagination helpers, canonical market-data results, and selected trading workflows sit above the generated clients, while the underlying generated operations remain available.
- **Typed errors and request controls: The SDK includes typed error categories, configurable timeouts, client-side rate limiting, and automatic retries for safe, idempotent HTTP methods.** Order placement remains outside automatic retry behavior, and ambiguous submitAndWait placements can be reconciled through a stable client order ID.
- **Migration support:** A migration guide and codemod help applications move from the former 3.x client structure to the new Trading and Market Data namespaces.
- **Fewer runtime dependencies:** Direct runtime dependencies fall from 11 in version 3.1.3 to three in version 4.0.1.
- **Modern package support:** Version 4 ships ESM, CommonJS, and TypeScript declarations for Node.js 20 and later. REST-only exports are also available for supported edge runtimes.


Together, these changes make the upgrade larger than a TypeScript rewrite. Version 4 expands the Alpaca workflows available through the SDK and gives that broader surface a more consistent client structure.


The new structure makes the API group, resource, and operation explicit in each method path:


```text
import { Alpaca, TimeFrame } from "@alpacahq/alpaca-trade-api";
const alpaca = new Alpaca({
keyId: process.env.APCA_API_KEY_ID,
secret: process.env.APCA_API_SECRET_KEY,
paper: true,
});
const account = await alpaca.trading.account.getAccount();
const positions = await alpaca.trading.positions.getAllOpenPositions();
const bars = await alpaca.marketData.getStockBarsFor("AAPL", {
timeframe: TimeFrame.Day,
start: new Date("2026-07-01"),
});
```


The method path identifies the API group, resource, and operation. Generated REST methods use typed request objects, while SDK fields use camelCase throughout the TypeScript interface.


This gives developers a more consistent surface to navigate and exposes method signatures and request types clearly to editors, compilers, and coding agents.


### **Moving from Node.js 3.1.3 to 4.x**


Version 4 carries the established Trading, Market Data, and streaming capabilities from 3.1.3 into the new client structure. It also expands the generated surface represented in the 4.0.1 specification snapshot.


Area


Version 3.1.3


Version 4.x


Client structure


Flat methods


Generated resource clients under trading and marketData, plus ergonomic helpers


Trading coverage


Accounts and configurations, account activities, portfolio history, orders, positions, assets, calendar, clock, and watchlists


Core 3.1.3 coverage plus option-contract reference data, crypto funding, locates, tokenization, and account-activity event streams


Market Data coverage


Stocks, crypto, options, news, and corporate actions


Core 3.1.3 coverage plus indices, forex, fixed income, screeners, logos, stock auctions, and exchange and condition metadata


Client behavior


Transport-specific errors and application-managed controls


Typed errors, timeouts, rate limiting, and automatic retries for safe, idempotent methods


Precision


Millisecond dates and numeric identifiers


Additive timestampRaw and selected idRaw fields preserve nanosecond timestamps and exact large market-data identifiers


Paper trading is the default in version 4. Applications using live trading should pass` paper: false` explicitly. Environment-file loading also belongs to the application in version 4. Projects using` dotenv` can import` dotenv/config` at their entry point.


### **Upgrade from 3.x**


Review the[3.x to 4.0 migration guide](https://github.com/alpacahq/alpaca-trade-api-js/blob/v4.0.1/MIGRATION.md?ref=alpaca.markets) before upgrading.


```text
npm install @alpacahq/alpaca-trade-api@^4
```


### **Starting a new Node.js project**


Install the current 4.x release and follow the[Node.js SDK documentation](https://alpacahq.github.io/alpaca-trade-api-js/?ref=alpaca.markets) .


```text
npm install @alpacahq/alpaca-trade-api@^4
```


## **Why Build with Alpaca Java SDK 0.1.3?**


Alpaca Java 0.1.3 gives Java 17 developers a supported client across three Alpaca product areas.


Product area


Java SDK support


Trading API


Generated REST clients and trading-updates WebSocket


Market Data API


Generated REST clients and WebSockets for stocks, crypto, and news


Broker API


Generated REST clients and Broker trade events through Server-Sent Events


The SDK combines generated REST clients and models with handwritten clients for real-time interfaces. Developers can use the top-level` AlpacaClient` facade for common workflows or` AlpacaClientFactory` for direct access to configured generated clients. Pagination utilities, configurable HTTP behavior, and opt-in retries for idempotent operations are included. Handwritten WebSocket models use BigDecimal for price and fractional-size fields, preserving decimal values without binary floating-point conversion.


**Install version 0.1.3 with Gradle:**


```text
implementation("markets.alpaca:alpaca-java:0.1.3")
```


Or Maven:


```text
<dependency>
<groupId>markets.alpaca</groupId>
<artifactId>alpaca-java</artifactId>
<version>0.1.3</version>
</dependency>
```


The[Java getting started guide](https://alpacahq.github.io/alpaca-java/getting-started?ref=alpaca.markets) covers credentials, client construction, Trading, Market Data, Broker, and streaming. The SDK is available through Maven Central with source and Javadoc artifacts.


## **SDK Context for Coding Agents**


Both repositories include SDK-specific guidance for AI-assisted development: an installable Agent Skill for Node.js and an LLM usage guide for Java. These resources give coding agents explicit context for documented clients, methods, models, pagination, retries, streaming, and response handling. They support development-time use; runtime request counts and payload sizes still depend on the API operations selected by the application.


## **Additional Resources**


For additional AI or agentic tools, check out Alpaca’s[Skills Library for AI Agents](https://alpaca.markets/blog/alpaca-launches-skills-library-for-ai-agents/) ,[Trading API MCP Server](https://alpaca.markets/blog/alpaca-launches-mcp-server-v2/) ,[Broker MCP Servers](https://alpaca.markets/blog/alpaca-launches-mcp-servers-in-public-beta-for-broker-partners/) , and[CLI](https://alpaca.markets/blog/alpaca-introduces-cli-for-trading-api/) . If you’re looking to learn more about what other users are building in the AI and agentic space, here are a few examples:


- [How to Get Started with Machine Learning in Trading](https://alpaca.markets/learn/how-to-get-started-with-machine-learning-in-trading?ref=alpaca.markets)
- [From Value Investing to Systematic Trading: Building a Multi-Strategy Backtesting Dashboard with AI and Alpaca](https://alpaca.markets/learn/from-value-investing-to-systematic-trading-building-a-multi-strategy-backtesting-dashboard-with-ai-and-alpaca?ref=alpaca.markets)
- [Building a Multi-Agent AI Trading System on Alpaca](https://alpaca.markets/learn/building-a-multi-agent-ai-trading-system-on-alpaca?ref=alpaca.markets)
- [Building NightWatcher V2: A Multi-Agent Trading System with Alpaca](https://alpaca.markets/learn/building-nightwatcher-v2-a-multi-agent-trading-system-with-alpaca?ref=alpaca.markets)
- [Agent-M: An Autonomous Multi-Agent Trading Platform Using Alpaca](https://alpaca.markets/learn/agent-m-an-autonomous-multi-agent-trading-platform-using-alpaca?ref=alpaca.markets)


Still have questions? Contact us at[\[email protected\]](https://alpaca.markets/cdn-cgi/l/email-protection#d5a6a0a5a5baa7a195b4b9a5b4b6b4fbb8b4a7beb0a1a6) or visit our[documentation](https://docs.alpaca.markets/?ref=alpaca.markets) .


---


*The content of this article is for general informational purposes only. All examples are for educational and illustrative purposes only.*


*Options trading is not suitable for all investors due to its inherent high risk, which can potentially result in significant losses. Please read*[Characteristics and Risks of Standardized Options](https://www.theocc.com/company-information/documents-and-archives/options-disclosure-document?ref=alpaca.markets) *before investing.*


*Fractional share trading allows a customer to buy and sell fractional share quantities and dollar amounts of certain securities. Fractional share trading presents unique risks and is subject to particular limitations that you should be aware of before engaging in such activity. See Alpaca Customer Agreement at*[https://alpaca.markets/disclosures](https://alpaca.markets/disclosures?ref=alpaca.markets) *for more details.*


*All investments involve risk and the past performance of a security, or financial product does not guarantee future results or returns. There is no guarantee that any investment strategy will achieve its objectives. Keep in mind that while diversification may help spread risk, it does not assure a profit, or protect against loss. There is always the potential of losing money when you invest in securities, or other financial products. Investors should consider their investment objectives and risks carefully before investing.*


*Cryptocurrency is highly speculative in nature, involves a high degree of risks, such as volatile market price swings, market manipulation, flash crashes, and cybersecurity risks. Cryptocurrency regulations are continuously evolving, and it is your responsibility to understand and abide by them. Cryptocurrency trading can lead to large, immediate and permanent loss of financial value. You should have appropriate knowledge and experience before engaging in cryptocurrency trading. For additional information, please click*[here.](https://files.alpaca.markets/disclosures/library/CryptoRiskDisclosures.pdf?ref=alpaca.markets)


*Securities brokerage services are provided by Alpaca Securities LLC (dba "Alpaca Clearing"), member*[FINRA](https://www.finra.org/?ref=alpaca.markets) */*[SIPC](https://www.sipc.org/?ref=alpaca.markets) *, a wholly-owned subsidiary of AlpacaDB, Inc. Technology and services are offered by AlpacaDB, Inc.*


*Cryptocurrency services are made available by Alpaca Crypto LLC ("Alpaca Crypto"), a FinCEN registered money services business (NMLS # 2160858), and a wholly-owned subsidiary of AlpacaDB, Inc. Alpaca Crypto is not a member of SIPC or FINRA. Cryptocurrencies are not stocks and your cryptocurrency investments are not protected by either FDIC or SIPC. Please see the*[Disclosure Library](https://alpaca.markets/disclosures?ref=alpaca.markets) *for more information.*


*AlpacaDB, Inc., the parent company of Alpaca Securities LLC and Alpaca Crypto LLC, provides services and technology, including the brokerage infrastructure API supporting Alpaca’s financial services.*


*This is not an offer, solicitation of an offer, or advice to buy or sell securities or cryptocurrencies or open a brokerage account or cryptocurrency account in any jurisdiction where Alpaca Securities or Alpaca Crypto, respectively, are not registered or licensed, as applicable.*
