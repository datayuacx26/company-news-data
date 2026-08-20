---
schema_version: "1.0.0"
document_id: "06210f0f0b0f2a5011d1dc0081a65ab2e862d8a085f36c31cc458c98608ed878"
company_key: "mntn-inc-class-a-common-stock"
company: "MNTN Inc."
source_id: "mntn-inc-class-a-common-stock-rss-3c333be474a8"
canonical_url: "https://mountain.com/tech/part-two-inside-mntns-bidding-architecture/"
published_at: "2026-03-23T16:28:37+00:00"
first_seen_at: "2026-07-25T01:12:17.961662+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:8e0d3b6709bf6436cc0748634d38235465e590ad64d252b653e6b6e442b23aea"
---

# Part Two: Inside MNTN’s Bidding Architecture

# Part Two: Inside MNTN’s Bidding Architecture


Mar. 2026


In our last installment, we shared how MNTN’s bidding system helps you maximize performance on Connected TV (CTV) by blending automation with premium inventory. Now, let’s pull back the curtain on the technology powering those outcomes, from the microservices architecture that handles millions of bids per second to the audience tools that help you reach the households most likely to convert.


### **MNTN Bidding System – Overview**


At the core of Performance TV is our bidding system’s microservice architecture. The MNTN bidding system’s roster of microservices runs in Kubernetes, backed by data stores that include Redis and Aerospike.


The microservices are written in Kotlin, Python, or Rust, depending on which language best fits the use case and requirements of each service.


Events for audience/scores updates, ad spend, impressions, etc., are piped through Kafka, picked up by consumer services, and ultimately loaded into the data stores, creating a real-time feedback loop that enables the bidding system to have the most up-to-date data to make its bid decisions.


Though you don’t see it directly, this architecture gives you the ability to fine-tune ad delivery, reach the right audience, and maximize your budget.


### **Integration with multiple SSPs**


Another key feature of the MNTN bidding system is its integration with multiple supply-side platforms (SSPs). Built with extensibility in mind, a flexible abstraction layer allows for the addition of new SSPs as business requirements grow.


The bidding system natively supports inventory buying from private marketplace (PMP) deals. Since MNTN has strong partnerships with numerous networks, the ability to bid on that network inventory into PMP deals was a pillar of the system’s architecture from its inception.


### **Multi-region deployments / scalability**


To improve responsiveness and bolster availability, our bidder system runs on both the West Coast and East Coast of the United States. Traffic is automatically routed to the nearest data center via a cluster of scalable load balancers.


As web traffic patterns and television view habits change throughout the day (think of the transition from early fringe — 4-7p — to prime time) our load balances and the underlying services also scale up and down alongside that traffic.


Our system is also 100% dockerized, allowing for easy extensibility into other regions in the future.


## **Audience**


Our audience service aggregates shopper data, event data, intent graphs, and pixel and impression data, combining disparate sources into cohesive audience segments.


When you create a campaign, those rules determine which households belong in the audience, ensuring your ads reach the right people.


## **Campaign metadata caching**


Although the metadata for an ad campaign lives in a relational database, we’ve found that retrieving that data directly is slow and not particularly scalable.


Instead, we have a cache loader service that periodically loads that metadata into Redis caches. The microservices retrieve that data directly from the caches, yielding much faster response times.


One tradeoff of such an approach is that changes to that data need to propagate into the cache. In a future blog post, we’ll examine how we tackle cache invalidation.


## **CTV frequency capping**


You understand that while it’s important to engage your audience with a consistent message, it’s equally important not to overwhelm them and cause ad fatigue. To that end, our bidding system supports a flexible set of frequency capping rules.


For instance, you might configure rules like:


- No more than three ads from the same campaign over the last five days
- No more than five ads from your brand over the past two weeks


Frequency capping in CTV advertising campaigns presents unique challenges compared to display ads, primarily due to a significant time delay between an auction win and the ad’s actual playback. Unlike the near-instantaneous impression confirmation for display ads, CTV ad delivery and playback can take several minutes after an auction is won in milliseconds. An impression is only confirmed upon successful playback; if the TV is turned off or the viewer switches apps, the ad may not play.


These delays make real-time impression tracking difficult, often leading to exceeding frequency caps. To address this, our system employs a proactive strategy: a provisional impression is recorded immediately after an auction win. These provisional impressions are counted towards the cap and are later cross-referenced with video completion playback events. If a playback event is received, the provisional impression’s time-to-live (TTL) is canceled. Conversely, if no playback event occurs within a specified period (the TTL is set based on the expected upper bound for a playback event), the impression is discarded. This ensures precise impression counts and prevents breaches of frequency caps.


## **Recency**


You can also customize ad delivery behavior via recency rules. In this case, recency refers to a customizable period within which a household has had a touchpoint with your brand (e.g., visited their website, viewed an ad impression).


## **Spend capping**


When you set up a campaign, you also can specify a certain amount of budget over a certain period (or flight).


Our real-time spend services will take that budget and determine pacing rules — the rate at which we spend — for that budget, taking into account auction wins as they come in and honoring your daily spend caps.


## **Dynamic performance adjustments**


Our bidding system incorporates a variety of inputs to make real-time bid decisions, driving your performance. Those inputs include household affinity scores calculated by an advanced AI engine, event data, and budget data.


As bids occur, MNTN uses AI to collect feedback on those bids and other proprietary factors to improve the flow, adjusting internal levers (thresholds) that go into the bid decision process.


Among the data points for the decision engine are affinity scores. Our AI engine analyzes your ad campaign and households to determine how likely that household will find that ad campaign relevant, ultimately leading to conversions and increasing performance.


### **Why this matters:**


Every piece of our architecture is designed with your outcomes in mind. From microservices and caching to AI-driven affinity scores, our system ensures your ads are delivered to the right people, at the right time, all while staying within your budget and maximizing performance.
