---
schema_version: "1.0.0"
document_id: "5266b1622b81a0bcfef7a418416072880dc070ce080938bac17287adebedc140"
company_key: "wealthfront-corporation-common-stock"
company: "Wealthfront Corporation"
source_id: "wealthfront-corporation-common-stock-rss-9fbe6ef385e3"
canonical_url: "https://eng.wealthfront.com/2026/04/21/putting-the-tech-in-fintech-how-we-use-fix-to-execute-trades-at-scale/"
published_at: "2026-04-22T00:27:03+00:00"
first_seen_at: "2026-07-20T23:18:22.615485+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:da65b2c33365c0f14a6b337fcfb17e5de31fe7bdf40593bbe7432e4142a101a2"
---

# Putting the Tech in Fintech: How We Use FIX to Execute Trades at Scale

At Wealthfront, the “tech” in financial technology isn’t just a buzzword—it’s the foundation of everything we build. Beneath the intuitive frontend our clients interact with lies a complex ecosystem of distributed systems. One of the most critical pieces of that backend architecture is the engine that enables us to route massive volumes of trades efficiently.


Thanks to advances in financial technology, trading on the United States stock market has come a long way from the early days of verbal orders and paper tickets on Wall Street. For decades, the telephone was the primary scaling tool, decentralizing the trading floor and opening the market to anyone with access to a phone line. Today, however, human intervention is a bottleneck. With[over $500 billion in daily US equity volume as of 2023](https://www.finra.org/media-center/reports-studies/2024-industry-snapshot/market-data#table-3-1-1-2) , modern market scale requires software that can evaluate, route, and execute thousands of transactions in less than a second, without directly involving people in every trade.


In this post, we’ll pull back the curtain and do a deep dive into how Wealthfront leverages technology to communicate with our executing brokers at scale. We’ll explore the layers of infrastructure and the systems that are designed to work together to help ensure every trade is executed safely and efficiently.


### The Infrastructure Supporting Trade Execution


To achieve a high level of automation and reliability, there are four main pieces of technology that intertwine to form the infrastructure supporting trade execution at scale.


If you think of this system like making a traditional phone call, **networking** is the physical telephone line that connects us to our brokers, the **FIX protocol** is the language spoken over that line, **Quickfix/j** is the operator who connects the call and keeps it in order, and our **internal application layer** is the person on the phone who decides what to say and translates messages as they come in and go out.


### 1. The Networking Layer (The Telephone Line)


Before Wealthfront can send a single order, we need a live connection to each broker that we trade with. These connections follow a layered networking stack that can be mapped closely to the TCP/IP model:


- **Network Layer:** We maintain a dedicated network connection to each broker to help ensure fast, low-latency, and secure transport.
- **Internet Layer:** Built upon that physical connection, we use standard IP routing. Each broker connection targets a specific, provisioned IP address.
- **Transport Layer:** Leveraging that internetwork path, we establish a persistent TCP connection. This connection stays open for the entire trading day. FIX does not natively support multiplexing, so every broker session requires its own dedicated TCP socket—one connection per broker, per session type.
- **Application Layer:** Sitting on top of this TCP connection is a FIX session, which standardizes all communication into the FIX protocol. The session defines who’s talking to whom (via unique identifiers, like WLTH), tracks message sequence numbers, and exchanges periodic heartbeat messages to confirm the connection is still alive.


Most of our broker connections are configured with multiple connection targets. If the primary path goes down, engineers can switch to a backup host in real time. The system tears down the existing TCP connection, rebuilds it against the backup endpoint, and reestablishes the FIX session.


### 2. The FIX Protocol (The Language)


The Financial Information eXchange (FIX) protocol is the industry-standardized messaging protocol used to electronically communicate trade orders, executions, and market data between financial institutions in real time. Think of FIX as a unique language made specifically for trading, similar to how Morse code was a language created for communication over the telegraph.


The FIX protocol consists of a series of numeric tags that represent different fields. Each FIX message is sent as a sequence of **Tag=Value** pairs separated by a SOH (Start of Heading) character (ASCII code 01). These fields are standardized and can be found in any FIX dictionary. While the protocol has been updated over time, we primarily use version 4.2.


Each defined FIX tag is pertinent for order placement, fulfilment, or session management. Here are a few examples:


- Tag 39 (OrdStatus): holds the order status (e.g., new, filled, rejected)
- Tag 40 (OrdType): holds the order type (e.g., market, limit)
- Tag 55 (Symbol) holds the ticker symbol for the order


A FIX session is an ongoing conversation between two parties. Wealthfront sends messages to place orders, and receives incoming messages from brokers with updates on those orders.


Here is what a real conversation might look like when we initiate a trade:


**7:00 AM EDT**


Wealthfront (WLTH) initiates the session by sending a logon message.


```text
8=FIX.4.2|9=64|35=A|34=1|49=WLTH|56=BRKR|52=20250819-11:00:00|...|
```


The broker (BRKR) acknowledges the logon message. The session is now active.


```text
8=FIX.4.2|9=64|35=A|34=1|49=BRKR|56=WLTH|52=20250819-11:00:00|...|
```


Tag 35=A denotes a Logon message. Tag 34 represents the Message Sequence Number. In the above messages, 34=1 because this is the very first message of the day for both parties.


**8:32 AM EDT**


Wealthfront sends an order to buy 500 shares of AAPL. 35=D denotes that the message type is an order (single).


```text
8=FIX.4.2|...|35=D|34=2|49=WLTH|56=BRKR|52=20250819-12:32:15.421|...|38=500|40=2|44=175.20|54=1|55=AAPL|...|
```


The broker immediately acknowledges the order.


```text
8=FIX.4.2|...|35=8|34=2|49=BRKR|56=WLTH|52=20250819-12:32:15.530|...|150=0|39=0|55=AAPL|...|
```


Shortly after, the broker fills it, sending an execution report back.


```text
8=FIX.4.2|...|35=8|34=3|49=BRKR|56=WLTH|52=20250819-12:32:16.012|...|150=2|39=2|55=AAPL|32=500|31=175.20|...|
```


Notice how tag 34 (the sequence number) increments as the conversation continues. This ensures that every message is interpreted in the exact order it was sent, allowing the receiver to detect dropped packets or duplicate trades. These sequence numbers are reset every time a new session is started, which, in our systems, happens once a day around market open.


### 3. QuickFIX/J (The Operator)


QuickFIX/J is a robust, open-source, Java-based FIX engine. We use a customized fork of QuickFIX/J to manage TCP connections, parse pipe-delimited FIX strings, and track sequence numbers from scratch. It acts as our automated operator, maintaining the persistent conversation between Wealthfront and our executing brokers while handling the low-level protocol details so our application layer doesn’t have to.


Out of the box, QuickFIX/J abstracts away a significant amount of complex, stateful infrastructure:


- **TCP Management:** The FIX protocol doesn’t natively support multiplexing, meaning every distinct session requires a dedicated TCP connection. QuickFIX/J establishes, monitors, and maintains these socket connections automatically.
- **State & Sequence Management:** QuickFIX/J tracks message sequence numbers in real-time. If it detects a gap on the wire (for example, if we receive message 5 immediately after message 3), it will automatically pause processing and issue a ResendRequest for message 4 before resuming.
- **Database Persistence:** By configuring a few simple properties, QuickFIX/J automatically logs incoming messages, outgoing messages, and session events directly into our database tables. This provides an immediate audit trail for debugging.


```text
settings  .setString  ( JdbcSetting  .SETTING_LOG_INCOMING_TABLE  , " incoming_fix_messages  ");
settings  .setString  ( JdbcSetting  .SETTING_LOG_OUTGOING_TABLE  , " outgoing_fix_messages  ");
settings  .setString  ( JdbcSetting  .SETTING_LOG_EVENT_TABLE  , " fix_events  ");   Code language:    CSS    (  css  )
```


Once the core infrastructure is handled, QuickFIX/J also gives us the flexibility to adapt legacy protocols to modern product requirements via an internal XML data dictionary. This lets us parse and validate custom, “unofficial” tags bilaterally agreed upon with our brokers.


For example, in the standard FIX 4.2 specification, tag 38 represents OrderQty, which is defined as a quantity type that does not allow decimals. However, we now need a way to specify fractional shares. To do this, we could agree with a broker to use a custom tag, like 3800, to represent FractionalOrderQuantity. As long as both sides have configured their dictionaries to interpret this new tag, QuickFIX/J will serialize and validate it just like any standard field.


### 4. Internal Application Layer (The Person on the Phone)


QuickFIX/J handles the protocol transport, but it has no concept of Wealthfront’s internal business logic. We still need to decide what to say and what to do with what we hear back. Our internal trade execution engine handles this semantic translation: mapping our internal **FixOrder** objects into outgoing FIX messages, and translating incoming execution reports into **FixOrderEvent** objects that track the order’s lifecycle.


Let’s look at what happens when we want to place a buy order. Our execution engine generates relevant internal objects, one of which is represented in our database as a **FixOrder** :


```text
+-----------+---------+------------+------------+-------------+------------+--------+--------------+--------+
| id        | version | open_state | fill_state | trans_state | account_id | action | instrumentid | symbol |
| 8765432   | 4       | OPEN       | UNFILLED   | STEADY      | 2          | BUY    | 12345        | SYMBL  |
```


Because there is no automatic mapping from our Java domain objects to FIX messages, we handle this translation manually using the Builder pattern. Our core **Fix42MessageBuilder** translates domain enums (like mapping Action.BUY to FIX Side.BUY) and uses a Visitor pattern to safely unpack order parameters, ensuring that a Market order and a Limit order map to their correct respective tags. We also utilize broker-specific subclasses to inject custom venue-specific tags.


Once mapped, it serializes into the outgoing FIX message:


```text
8=FIX.4.2|9=204|35=D|34=20720|49=WLTH|52=20260330-15:16:45.862|56=BRKR|115=WLTH|11=1_8765432_2|12=0|13=3|15=USD|18=5|21=1|22=4|38=1.000000|40=2|44=142.12|47=P|48=US54321X9876|54=1|55=SYMBL|59=0|60=20260330-15:16:45.860|10=082|
```


###


Reading raw FIX strings manually is time consuming – not only is the delimiter is an unprintable character (simplified to a pipe (|) for readability here), knowing every tag by memory is unrealistic. To improve developer velocity, we built an internal tool (GetOutgoingFixMessage) that parses these strings and replaces tags with their English names, making debugging significantly easier:


```text
BeginString  : FIX.4.2
BodyLength  : 204
MsgSeqNum  : 20720
MsgType  : NewOrderSingle (D)
SenderCompID  : WLTH
SendingTime  : 20260330-15:16:45.862
TargetCompID  : BRKR
OnBehalfOfCompID  : WLTH
ClOrdID  : 1_8765432_2
Commission  : 0
CommType  : ABSOLUTE (3)
Currency  : USD
ExecInst  : HELD (5)
HandlInst  : AUTOMATED_EXECUTION_ORDER_PRIVATE_NO_BROKER_INTERVENTION (1)
IDSource  : ISIN_NUMBER (4)
OrderQty  : 1.000000
OrdType  : LIMIT (2)
Price  : 142.12
Rule80A  : PRINCIPAL (P)
SecurityID  : US54321X9876
Side  : BUY (1)
Symbol  : SYMBL
TimeInForce  : DAY (0)
TransactTime  : 20260330-15:16:45.860
CheckSum  : 082   Code language:    HTTP    (  http  )
```


When the broker responds over the FIX session, QuickFIX/J parses the strings and provides typed dispatching back to our application. We route these to specific handler classes (e.g., **FillHandlerImpl** ), which read the fields and generate **FixOrderEvent** records. This provides a completely auditable, real-time lifecycle of the trade:


```text
+--------+---------------+----------+-----------------+---------------------+----------+----------+-----------------+------------------+
| id     | type          | order_id | sequence_number | event_time          | price    | quantity | leaves_quantity | executing_broker |
|  432      | create        |  8765432    |  0                 |  2026  -03  -30    11  : 16  : 45   | < null  >   | < null  >   | < null  >          | < null  >           |
|  433      | sendattempted |  8765432    |  1                 |  2026  -03  -30    11  : 16  : 45   | < null  >   | < null  >   | < null  >          | < null  >           |
|  434      | sent          |  8765432    |  2                 |  2026  -03  -30    11  : 16  : 45   | < null  >   | < null  >   | < null  >          | < null  >           |
|  435      |  new             |  8765432    |  3                 |  2026  -03  -30    11  : 16  : 45   | < null  >   | < null  >   |  1.0000            | BRKR             |
|  436      | fill          |  8765432    |  4                 |  2026  -03  -30    11  : 16  : 45   |  139.2650   |  1.0000     |  0.0000            | BRKR             |
+--------+---------------+----------+-----------------+---------------------+----------+----------+-----------------+------------------+   Code language:    PHP    (  php  )
```


By strictly separating the transport layer from our semantic translation layer, we ensure that our trading system remains modular, highly testable, and capable of adapting to the quirks of any new executing broker we integrate with.


### Safety First: Integrating with New Brokers


Because this infrastructure directly handles client money and interacts with the live market, we take a highly defensive approach when integrating a new executing broker. Our rollout process follows an intentional, graduated sequence:


1. **UAT Environment:** We begin by connecting locally to the broker’s User Acceptance Testing (UAT) server. We manually send test orders to verify that our QuickFIX/J implementation and the broker’s system are translating and acknowledging messages correctly.
2. **The ZVZZT Ticker:** When moving to production, our first real trades are placed using ZVZZT. This is a special non-reportable test ticker which ensures there are no serious implications should an issue arise.
3. **Firm Accounts:** Once test tickers are trading cleanly, we execute small, manual trades on real tickers using internal firm accounts.
4. **Gradual Client Rollout:** Finally, we open the connection to client trades starting with a single ticker, eventually ramping up to a previously determined percentage of all trading volume.


This meticulous testing strategy helps to ensure that our automated investing infrastructure remains fast, resilient, and most importantly, safe.


### The Intersection of Tech and Finance


Building a modern automated investing platform requires more than just smart financial algorithms; it requires rock-solid infrastructure capable of talking to the broader financial ecosystem. The US stock market is, at its core, a massive distributed system.


By combining industry-standard protocols like FIX with robust open-source libraries like QuickFIX/J and our own carefully designed internal abstractions, we bridge the gap between finance and technology. This infrastructure allows us to abstract away the immense complexity of market execution. Our engineers don’t have to worry about dropped packets or raw string parsing; they can focus on what matters most: building the resilient, scalable software that delivers on our promise of effortless investing.


---


### Disclosures


Investment management and advisory services are provided by Wealthfront Advisers LLC (“Wealthfront Advisers”), an SEC-registered investment adviser, and brokerage related products are provided by Wealthfront Brokerage LLC (“Wealthfront Brokerage”), a Member of[FINRA](http://finra.org/) /[SIPC](http://sipc.org/) . Financial planning tools are provided by Wealthfront Software LLC (“Wealthfront Software”).


The information contained in this communication is provided for general informational purposes only, and should not be construed as investment or tax advice. Nothing in this communication should be construed as a solicitation or offer, or recommendation, to buy or sell any security.


Wealthfront Advisers, Wealthfront Brokerage, and Wealthfront Software are wholly-owned subsidiaries of Wealthfront Corporation.
© 2026 Wealthfront Corporation. All rights reserved.
