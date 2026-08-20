---
schema_version: "1.0.0"
document_id: "127ba22b20431631e5468911f472ad65eebf5242bed2586c059e3144c2988856"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2025-12-18-design-ebay-using-icepanel"
published_at: "2025-12-18T00:00:00+00:00"
first_seen_at: "2026-07-21T23:29:04.459524+00:00"
fetched_at: "2026-07-28T22:24:55.411240+00:00"
content_hash: "sha256:8ab95d5f3d62867fae6bdde93a97bebfa07da864d7cae6c213902cfc346b97ec"
---

# Design eBay using IcePanel

## 📝 Introduction


In this post, we’ll share an example architecture for **eBay** – one of the largest online auction and marketplace platforms where users can buy and sell items or make direct purchases. We’ll design this as software architects and create four hierarchical diagrams: Context, Container, Component, and Code (not sure what these are? Read[this](https://icepanel.io/blog/2024-07-18-what-is-the-c4-model) ). We’ll annotate the building blocks and highlight user interaction (data flow) within the system using IcePanel[Flows](https://docs.icepanel.io/visual-storytelling/flows) .


Let’s first define the scope by writing the functional and non-functional requirements of the system. Afterwards, we’ll go through each layer of the C4 model.


You can view the final architecture at this link:[https://s.icepanel.io/DWnaysJ3cbCQqg/eYV5](https://s.icepanel.io/DWnaysJ3cbCQqg/eYV5)


---


## 🔎 Scope


For an online auction like eBay, users should be able to:


1. Sell an item or post it for auction with a starting price.
2. Buy an item or place a bid with a price higher than the current bid.
3. View an auction for an item with the current highest bid.


For non-functional requirements, the system should have:


- Strong consistency with bidding to make sure all buyers can see the same price.
- Real-time notifications and scalable to ~1 million concurrent auctions.
- Fault-tolerant and durable (i.e., we can’t drop any bids from buyers)


Let’s start with the first diagram, Context.


---


## Level 1 - Context


This context view shows the interaction between end users, our main system (eBay), and the external systems it integrates with. In this design, we have two actors:


- Seller: User who sells items on eBay in an auction or with a fixed price.
- Bidder: User who buys items or places bids on eBay.


Ebay also depends on two external systems:


- **Payment Provider (Stripe)** : Responsible for processing payment transactions and managing the complete payment lifecycle. When a winning bidder needs to complete payment or when a seller receives funds, they are redirected to the payment provider’s secure interface, where card details are collected and processed.
- **SMS Provider (Twilio)** : Manages fast SMS delivery to users, mainly bidders. Our system publishes new bid events to a Kafka topic, where our notification service consumes that message and then forwards it to the SMS provider for notifying users.


---


## Level 2 - Container


This diagram is where we model a collection of independently deployable or runnable applications or data stores that are essential for the overall software system to function. This could be a web application, server, datastore, or a streaming data platform like[Apache Kafka](https://kafka.apache.org/documentation/) .
This system is an **event-based** architecture. This approach is ideal for high-throughput bidding scenarios where services process messages asynchronously whenever an item receives a bid. Events are triggered using Kafka topics, allowing the system to handle traffic spikes gracefully while maintaining strong consistency guarantees.


Our system is composed of the following Containers:


1. **Auction Service** : Manages the complete auction lifecycle, including creating new auctions when sellers list items, reading auction details and current bid status, and updating auction metadata (start time, end time, status).
2. **Bid Producer** : Responsible for accepting and validating incoming bids. It validates bid amounts with atomic checks on Redis, publishes bids to Kafka for asynchronous processing, and atomically updates the bid cache.
3. **Bid Service** : Consumes bid events from Kafka and provides durable persistence. It consumes bid events from a Kafka topic, persists all bids to the Auction Database (bids table) for audit trail, and updates the auction’s current price in the database.
4. **Notification Service** : Manages all user communications. It consumes bid events and auction lifecycle events from Kafka, sends real-time notifications when users are outbid, and sends winning bid confirmations.


**Note:** Real-time solutions like WebSocket connections for live auction updates and publish/subscribe patterns are outside the scope of this design. This architecture focuses on the core bidding flow, auction management, and durable event processing.


In this architecture, we use the following technologies:


1. **AWS API Gateway** : A scalable and secure entry point for all API requests coming from the frontend. This object is represented using[connection via](https://icepanel.io/blog/2025-12-15-new-releases-nested-groups-connection-via) property.
2. **AWS EC2:** Web service that provides reliable and secure compute for the different microservices (Auction Service, Bid Producer, Bid Service, Notification Service, Payment Service).
3. **Apache Kafka** : A distributed event streaming platform that provides high throughput (millions of messages per second), durability (persists all messages to disk with replication), and ordering guarantees (partitioning by auction_id ensures sequential processing). Perfect for handling high-volume bidding periods while ensuring no bids are lost. This object is represented using a[connection via](https://icepanel.io/blog/2025-12-15-new-releases-nested-groups-connection-via) property.
4. **PostgreSQL** : An ACID-compliant relational database that ensures data integrity and strong consistency. Ideal for structured data like auctions and bids tables where transactional guarantees are critical.
5. **Redis** : A fast in-memory cache that improves response times (sub-millisecond latency) and reduces database load. Essential for serving current bid prices to 1 million concurrent auctions.


We’ve designed two common data flows using IcePanel. Check out these flows and play them step by step to see how our system works:


1. Create a new auction listing:[https://s.icepanel.io/DWnaysJ3cbCQqg/SpZW](https://s.icepanel.io/DWnaysJ3cbCQqg/SpZW)
2. Place a bid on an auction:[https://s.icepanel.io/DWnaysJ3cbCQqg/rmpv](https://s.icepanel.io/DWnaysJ3cbCQqg/rmpv)


---


## Level 3 - Component


In the C4 model, a component is a grouping of related functionality encapsulated behind a well-defined interface. For example, a collection of classes behind an interface. Let’s look at the Components in eBay’s auction system.


### 1. Bid Producer


The diagram below shows how a user interacts with the Bid Producer to place a bid on an auction. When a user sends a POST /bid request, the Bid API first validates the request and extracts the auction_id and bid_amount. It then retrieves the current highest bid from the cache (using[optimistic locking](https://medium.com/@hanifmaliki/concurrency-in-go-pessimistic-optimistic-and-redis-distributed-locks-explained-77125355594d) ) and checks if the new bid amount exceeds it. If the bid is too low, an error is returned to the bidder. This prevents race conditions when multiple bidders submit bids simultaneously.


Once the Redis lock is acquired, the Bid Kafka Publisher sends a new bid event to the bid-events topic in Kafka. The bid event includes auction_id, user_id, bid_amount, and timestamp. After publishing to Kafka, the service atomically updates the current highest bid in Redis (auction:{id}:price) before releasing the lock.


### 2. Auction Service


The Auction Service manages the complete auction lifecycle and provides read/write access to auction data. When a user requests auction information via GET /auctions/:auctionId, the Auction API first checks the Auction Cache (Redis) to retrieve hot auction data with sub-millisecond latency. If the data isn’t cached, the Auction Controller queries the Auction Repository, which fetches auction details from the Auction Database (PostgreSQL).


The Redis Client manages all cache operations, storing frequently accessed auction data with time-to-live (TTL) values to reduce database load. The Auction Repository handles all database interactions, including creating new auction listings when sellers post items, updating auction metadata (start time, end time, reserve price), and querying active auctions with filters.


When an auction ends, the Auction Controller determines the winning bidder by querying the highest bid from the database. It then triggers the Auction Payment Controller, which initiates the payment flow by calling the Payment Service. The Payment Controller handles payment confirmations, failures, and refund processing for cancelled auctions.


### 3. Bid Service


The Bid Service handles all bid processing in our online auction system. When a bid event is published to Kafka, the Bid Consumer polls messages from the bid topic and processes each bid event asynchronously. It interfaces with the Bid Repository, which serves as the data access layer for storing and retrieving bid information. The Bid Repository executes SQL queries against the Auction Database (PostgreSQL) to persist bids into the database and maintain bid history.


Let’s go one level deeper with the source code in the Code layer.


---


## Level 4 - Code


This is where we can view implementation details at the code level. We don’t recommend creating extensive diagrams for this; instead, we link directly to the code. However, here’s the general structure of these components for reference. We’ll briefly go over two Components: **Auction** and **Bid Producer** .


### Classes in “Auction Service”


This component manages auction lifecycle and real-time auction data access. The Auction API class exposes RESTful endpoints for retrieving auction details, current high bids, and auction status. The API first checks Redis cache for hot auction data before querying the database. The Auction Controller coordinates between cache, repository, and payment processing. The Auction Repository serves as the data layer for fetching auction information from the Auction Database.


#### Auction Controller


- GET /auctions/:auctionId - Retrieve auction details (item, description, current bid, etc.)
- GET /auctions/:auctionId/bids - Get bid history for specific auction
- POST /auctions - Create a new auction listing
- PATCH /auctions/:auctionId - Update auction details
- DELETE /auctions/:auctionId - Cancel auction


#### Redis Client


- getCachedAuction(auctionId) - Retrieve auction data and current high bid from cache
- cacheAuction(auctionId, auctionData, ttl) - Store auction data with time-to-live


#### Auction Repository


- getAuctionById(auctionId) - Fetch complete auction details from database
- getCurrentHighBid(auctionId) - Retrieve current winning bid amount and bidder
- updateAuctionStatus(auctionId, status) - Change auction state (active, ended, cancelled)
- getActiveAuctions(filters) - Query active auctions with filtering and pagination


#### Auction Payment Controller


- initiatePayment(auctionId, winnerId) - Start payment process for auction winner
- refundBid(bidId) - Process refunds for cancelled auctions or failed transactions


### Classes in “Bid Producer”


This component handles the bid submission workflow with strong consistency guarantees. The Bid API class provides RESTful endpoints for users to place bids and check bid status. The Bid Validator ensures bid amounts meet minimum requirements and auction eligibility rules. The Optimistic Lock Manager implements Redis-based locking to prevent race conditions during concurrent bidding. The Kafka Producer handles asynchronous event publishing for downstream bid processing and notifications.


These classes have roughly the following methods:


#### Bid API


- POST /bids - Place a new bid on an active auction
- GET /bids/:bidId - Retrieve details of a specific bid
- GET /auctions/:auctionId/bids - Get all bids for an auction


#### Cache Client


- getCurrentBid(auctionId) - Retrieve current bid for specific auction
- updateCurrentBid(auctionId, bidAmount) - Update cached high bid in real-time


#### Kafka Producer


- publishBidPlacedEvent(bidData) - Send bid creation event to Kafka topic
- publishBidOutbidEvent(userId, auctionId) - Notify when user is outbid


---


## Conclusion


In this post, we designed an online auction platform (eBay) using the C4 model on IcePanel. We began with the core requirements like strong consistency, real-time updates, and fault tolerance, and modeled the system from the top down, starting with the Context layer, followed by the Container, Component, and finally the Code layer.


If you enjoyed this post, check out our previous design deep dives using IcePanel:


1. [Design YouTube](https://icepanel.io/blog/2025-11-18-design-youtube-using-icepanel)
2. [Design Ticketmaster](https://icepanel.io/blog/2025-10-20-design-ticketmaster-using-icepanel)


## 📚 Resources


- [https://aws.amazon.com/msk/](https://aws.amazon.com/msk/)
- [https://kafka.apache.org/documentation/](https://kafka.apache.org/documentation/)
- [https://redis.io/open-source](https://redis.io/open-source/)
- [https://aws.amazon.com/microservices](https://aws.amazon.com/microservices)
- [https://pyemma.github.io/How-to-design-auction-system/](https://pyemma.github.io/How-to-design-auction-system/)
- [https://icepanel.io/blog/2025-11-18-design-youtube-using-icepanel](https://icepanel.io/blog/2025-11-18-design-youtube-using-icepanel)
- [https://icepanel.io/blog/2025-10-20-design-ticketmaster-using-icepanel](https://icepanel.io/blog/2025-10-20-design-ticketmaster-using-icepanel)
