---
schema_version: "1.0.0"
document_id: "834bc0a0bff7c69258166e1c289b619fa5652b0aad399ae3f5c17b44aefd6df2"
company_key: "nextdoor-holdings-inc-class-a-common-stock"
company: "Nextdoor Holdings Inc."
source_id: "nextdoor-holdings-inc-class-a-common-stock-rss-10247875dc01"
canonical_url: "https://engblog.nextdoor.com/typeahead-search-at-nextdoor-1875e70c67e8"
published_at: "2022-07-06T19:49:06+00:00"
first_seen_at: "2026-07-20T23:17:36.074163+00:00"
fetched_at: "2026-07-28T22:26:31.203349+00:00"
content_hash: "sha256:f84b0403f1b0a047c3ce238264041f8c563db90715eaa762fa741811047b7c28"
---

# Typeahead Search at Nextdoor

# Typeahead Search at Nextdoor


[Jerry Tian](https://medium.com/@tianjerry?source=post_page---byline--1875e70c67e8---------------------------------------)


5 min read


·


Jul 6, 2022


--


## Background


In a thriving community, people are connected to their friends and local businesses. Nextdoor is the hyperlocal platform that mirrors these offline relationships. Every day, through active discussions on the platform, new relationships are formed and existing ones strengthened.


For example, a Nextdoor user can create a post like “I really like @ **XYZ cafe** . @ **John** is a hard working business owner and we should all support him by buying a cup of delicious latte!” Here, the post is created by[at-mentioning](https://techcrunch.com/2022/02/15/nextdoor-revamps-with-new-profiles-feed-and-more-community-building-features/?guccounter=1#:~:text=Neighborhood%20members%20will%20also%20be%20able%20to%20connect%20and%20%40mention%20one%20another%20in%20posts%20and%20comments%2C%20similar%20to%20other%20social%20networks%2C%20like%20Twitter%20or%20Facebook.) (via the @ symbol) nearby businesses and users. From this post, users in the neighborhood can contribute by at-mentioning others to be part of the comment threads. As a result, John’s cafe thrives and acts as a neighborhood hub where new friends are made.


Every month, millions of these mentions are created in various discussions (including[lost dogs](https://youtu.be/PYfT7d3ZSw4?t=801) !). In addition to posts and comments, a user can type into the search box and see, among other things, nearby users and businesses. All these features are powered by the same autocomplete service — a set of APIs to ingest data and handle typeahead search of different entity types (businesses, users, keywords etc) on Nextdoor.


Press enter or click to view image in full size


This post focuses on how we built a proximity-based typeahead service to power typeahead use cases at Nextdoor.


## **Proximity-Based Typeahead Search as a Service**


Any good search experience can be boiled down to two core components:


1. Relevance: Given a search query, whether the user sees relevant results or not. As a hyperlocal social network, relevancy is heavily weighted by geo proximity.


2. Low latency.[Google Search](http://radar.oreilly.com/2009/07/velocity-making-your-site-fast.html) found that


> *a 400 millisecond delay resulted in a -0.59% change in searches/user. What’s more, even after the delay was removed, these users still had -0.21% fewer searches, indicating that a slower user experience affects long term behavior.*


For a good autocomplete experience, as users type, relevant results should show up instantaneously.


To meet the product requirements, we set out to build a service with the following design goals:


1. Low latency. There are hundreds of millions of entities on Nextdoor. The search latency at the service level should be less than 50ms.
2. Horizontally scalable to meet future scaling needs (we scale by adding more nodes).
3. Extensible. Typeahead search is a foundational API that enables other product features, so it should be easy to add other types of entities in the future.
4. High throughput for writes. We want to be able to index hundreds of millions of entities in a matter of hours.
5. Ease of operation and maintenance. When we index records, we should not impact production traffic.


## **Implementation**


We landed on an in-memory-based solution that leverages geohash. At a high level, geohashing divides the earth into multiple zones based on latitude and longitude. It provides a good way to shard a large data set into buckets based on a[zoomed-in level](https://h3geo.org/docs/core-library/restable/) . Entities in the same bucket are in close proximity.


We used Uber’s open source geohashing library called[H3](https://h3geo.org/) .


## Get Jerry Tian’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


For handling typeahead search, we decided to use[sorted sets](https://redis.io/docs/manual/data-types/#sorted-sets) . This gives a set of benefits:


1. In-memory storage gives us the best possible latency characteristics for handling typeahead search.
2. It is easy to maintain. We can rely on[redis persistence](https://redis.io/topics/persistence) without having to handle durability ourselves.
3. By following the Command Query Responsibility Segregation ([CQRS](https://martinfowler.com/bliki/CQRS.html) ) pattern, we are able to index hundreds of millions of entities in a matter of hours with no impact to the serving of production traffic. Ingestion is handled by redis primary nodes in the cluster, and updates are then replicated to the read-only nodes which handle the typeahead search. Replication lag is less than 10ms.


With these two core pieces in place, we built a set of APIs that work together to handle all aspects of typeahead search:


* indexing_api (ingestion)


* typeahead_api (search)


* ranking_api (ranking by entity types)


## **Ingestion Path**


Here is an example of the ingestion flow for businesses (Starbucks with id: 5, latitude: 47, and longitude -122):


Press enter or click to view image in full size


For users, the typeahead search API works for both first- or last-name prefixes. Here is an example of the ingestion flow for users (Steve Jobs with id 4, latitude 47, and longitude -122):


Press enter or click to view image in full size


## **Query Path**


With the above structure in place, typeahead search retrieves results with a simple look-up using entity type, geohash key, and prefix. We then hydrate and rank the results before returning them to the client.


Press enter or click to view image in full size


## **What we have today**


The service has been running since August 2021. Every month we are handling hundreds of millions of typeahead search requests, and millions of comments with at-mentions are created. The service level for P95 search latency is less than 30ms.


## **Future work: typeahead for 1+ degree connections**


To handle typeahead search with 1+ degrees (friends of friends), we can


1. Get a list of 1st degree connections.
2. For each user in the connection from step 1, get their connections.
3. Aggregate these connections and perform typeahead by prefix.


To reduce the network round trip between the first and successive calls, we can leverage[Lua](https://redis.io/docs/manual/programmability/eval-intro/) for edge computing.


## Acknowledgement


It takes a team to move mountains! I would like to take the opportunity to give a shoutout to all the dedicated Nextdoor folks behind this endeavor:


Shivam Bhalla, Stephen Cheng, Yuki Mizuno, Rajesh Balasa, Siva Pandeti, Uzair Khan, Sharvil Parekh, Hung Dao, Josh Sibelman, Bojan Babic, Jane Wang, Sudhanshu Siddh, Omer Palaz, Kristy Duong, Tristan Eastburn, Paul Meng, Cory Dolphin, Andrew Munn, Tim Wong, Chintan Shah, Rahul Sureka, Madeline Neveaux, Murali Krishna Hosabettu Kamalesha, Glen Tona, and Avinash Chukka.


And by the way, we are[hiring](https://about.nextdoor.com/careers/) !
