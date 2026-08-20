---
schema_version: "1.0.0"
document_id: "ecf8da53732fdd1b96f1bbfc84b1ba4228fa864f2a095ec822616a0de46d59ac"
company_key: "mntn-inc-class-a-common-stock"
company: "MNTN Inc."
source_id: "mntn-inc-class-a-common-stock-rss-3c333be474a8"
canonical_url: "https://mountain.com/tech/unleashing-performance-how-mntns-bidding-system-delivers-superior-results/"
published_at: "2026-03-23T16:28:08+00:00"
first_seen_at: "2026-07-25T01:12:17.961662+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:c517c3a4f32e1a4c9408d6ab7f922f940c5726089e75c74a7e6d7bbcf4db4cd6"
---

# Unleashing Performance: How MNTN’s Bidding System Delivers Superior Results

# Unleashing Performance: How MNTN’s Bidding System Delivers Superior Results


Mar. 2026


At MNTN, our mission is simple: help you achieve your marketing goals through the power of Connected TV (CTV). We call it Performance TV™ (PTV). At the heart of this mission is our bidding system, built to deliver the lower-funnel outcomes you expect from channels like search and social.


This post breaks down how our bidding system, which we built from the ground up, works and why it drives better results for you.


### **A Winning Combination**


Three things separate MNTN’s bidder from other solutions:


1. **Advanced ad buying settings:** MNTN leverages multiple algorithms to automate media buying and campaign management, including:


- **Dynamic daily pacing:** Our system adjusts your daily budgets based on real-time performance, ensuring optimal resource allocation and maximizing visits and return on ad spend (ROAS).
- **Automated auction management:** We use predictive models that consider factors like recency, frequency, and streaming networks to optimize bids in real-time, minimizing wasted ad spend and maximizing profitability.


- **Recency:** We intelligently allocate your budget to your most engaged customers. This precise targeting is based on their recent interactions with your brand, ensuring we focus on those who have most recently demonstrated interest.
- **Frequency:** The model determines the ideal ad frequency by analyzing your customers’ shopping patterns and applies frequency caps proven to be the most performant. This approach ensures sustained customer engagement while preventing ad fatigue.


2. **High-quality PTV inventory:** We prioritize access to premium, high-quality TV advertising inventory across a diverse range of streaming networks. By securing 98% of our ad inventory through direct relationships with streaming networks, our collective buying power helps us negotiate competitive pricing and pass those savings directly to you, resulting in lower CPMs and increased ROAS.
3. **Audience:** Our system evaluates every ad opportunity in real time, automatically filtering out low-value devices so your budget is strictly focused on the audiences most likely to engage with your brand.


All of this happens on our platform, which runs on robust, scalable infrastructure that processes millions of auction requests per second. That speed allows us to connect your ads with the right viewers at the exact moment of maximum engagement.


### **Why This Matters**


By seamlessly integrating advanced ad buying settings with access to premium streaming TV inventory, MNTN delivers a better advertising experience, one that delivers lower-funnel outcomes of the kind you expect from paid search and social but on the biggest screen in the home.


### **Key Takeaways:**


- Our bidding system combines advanced automation with premium inventory.
- Dynamic pacing and real-time optimization help you maximize ROI.
- Direct streaming network relationships give you lower CPMs and higher ROAS.
- Our infrastructure is built for speed and scale, ensuring your ads reach and engage the right people and deliver impact.


We built our bidding system to give you more control, efficiency, and performance from your CTV campaigns.


### **How Does Real-Time Bidding Work for CTV**


About 45 seconds before you see a commercial on CTV, a complex set of events unfolds to sell that commercial spot. It starts with someone watching a show. Within that show are pods of ads. The number of ads varies by network, but three to five ads per commercial break is typical. Each ad is auctioned off by the streaming network just before the viewer of the show sees the ad.


Step one is to initiate the sale of the ad spot via an API call from the app the consumer is watching to an ad server. For example, if you’re watching Peacock or Disney+, you are using the Peacock or Disney+ app on your TV, phone or tablet. You can also do this from a computer. Each streaming network app decides when you as a viewer are due to see a pod of ads. The app calls the ad server and things begin.


The ad server request triggers a waterfall of decisions. The first decision is whether the network has a direct commitment to an advertiser that they need to fill. If the network has a direct commitment, then no auction occurs and the ad spot is sold through what’s called a Programmatic Guarantee deal.


If the ad request gets past that step, then the ad request calls a Supply Side Platform (SSP). The role of the SSP is to conduct an auction of the ad impression on behalf of the streaming network among Demand Side Platforms (DSP). Each auction represents a single viewer watching a specific TV show on any device. SSPs are effectively the Sotheby’s of ads, except the auction is occurring approximately 4 million times per second. That amounts to 240 million auctions per minute, 14 billion auctions per hour, and 345 billion per day. It’s a lot of volume.


There are two layers of auctions: private marketplace (PMP) auctions or open market auctions. They operate the same way, but private marketplace auctions occur before the open market auction, but after the programmatic guarantee. The PMP auction generally involves customers of the streaming network that buy in significant volumes and as a result they get to look at ad impressions before the open market does. These advertisers / agencies are essentially getting VIP treatment by having the first opportunity to bid on ad impressions after up front commitments are met.


The auction itself is conducted by an SSP or a handful of streaming networks that sometimes act as their own SSP. The auction process is standardized using the[Open Real Time Bidding Standard (OpenRTB)](https://www.iab.com/guidelines/openrtb/) . This standard was released in December 2010 by the Open RTB Consortium, which later became part of the Interactive Advertising Bureau (IAB).


The standard itself is fairly simple. Each request from an SSP to a DSP represents one ad impression. So if a single consumer is watching a TV show and a commercial break containing five ads occurs, that will generate five RTB requests. That is multiplied by the number of people watching. The RTB request contains information like which streaming network is selling the ad impression, anonymous information about the device making the request, the time, the size of the ad unit (e.g., a 30-second or 15-second commercial), broad location information of the device and other information a bidder will need to decide if it wants to bid on the request and how much it’s willing to pay.


All bidding is fully automated based on any set of rules that the DSP is employing, and the advertisers and DSP can employ complex strategies to get the ad impression they want at the price they want. For example, some advertisers may be focused on getting the lowest price across a broad range of entertainment consumers, while others are focused on advertising to a small group of consumers with less price sensitivity. Regardless of the strategy, the RTB request goes to all bidders participating in the auction simultaneously, and those bidders are given fewer than 100 milliseconds to make a decision and respond with a bid or skip the auction. If the bidder misses the deadline, that is treated the same as skipping the auction.


This process is first done for the bidders with private marketplace deals, and they provide a key (ID) to participate in the auction. If the ad impression is not sold via private marketplace, the process is repeated in an open market auction where any advertiser can participate via whichever DSP platform they are using.


After an auction is won, the ad still has to be streamed into the app the consumer is watching during the commercial break, so a new process is triggered after the auction win to stream the commercial. The process to stream the TV ad impressions uses the[Video Ad Serving Template (VAST)](https://www.iab.com/guidelines/vast/) . This standard was released by the IAB in 2008 to provide a standardized method of streaming video ads.


Per the VAST standard, after a programmatic auction is won by a DSP, a request is sent from the streaming network to the DSP’s ad server to get the ad that will be streamed. This is typically a 30-second video, which is the TV commercial the user sees in the streaming app. The DSP responds to the ad impression request with a VAST file. This file includes the location of the video at various bit rates so that consumer devices on high speed internet connections and low speed internet connections can both be supported. For some streaming networks, the request for the ad comes from their servers and then is forwarded to the streaming app the consumer is using and for others the request comes directly from the streaming app. Once the app receives the URL of the video in either method, the video is streamed for the user in the streaming app and the user sees the commercial.


The VAST standard also supports notifying the DSP when the video starts playing, when it stops, and when it reaches various points along the way. This enables calculating the video completion rate, which is what percentage of people watching the video watched it entirely. For CTV, video completion rates are typically 97% or higher.


Data from every request to our bidding, ad, and vast servers is continuously logged, aggregated, and analyzed to power advanced targeting, attribution, and ad reporting. This ecosystem relies on an elastic technology stack designed to operate at an immense scale, capable of processing 350 billion auctions daily across our East and West Coast data centers.
