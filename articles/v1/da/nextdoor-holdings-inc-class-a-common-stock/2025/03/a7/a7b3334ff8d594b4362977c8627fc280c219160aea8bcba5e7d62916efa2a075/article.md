---
schema_version: "1.0.0"
document_id: "a7b3334ff8d594b4362977c8627fc280c219160aea8bcba5e7d62916efa2a075"
company_key: "nextdoor-holdings-inc-class-a-common-stock"
company: "Nextdoor Holdings Inc."
source_id: "nextdoor-holdings-inc-class-a-common-stock-rss-10247875dc01"
canonical_url: "https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-2-513922e4b4b1"
published_at: "2025-03-19T15:08:32+00:00"
first_seen_at: "2026-07-20T23:17:36.074163+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:dea1549a1e227b44d3b56ee606d865279914b11969e1cc2002523cf396950897"
---

# Scaling Nextdoor’s Datastores: Part 2

# Scaling Nextdoor’s Datastores: Part 2


[Tushar Singla](https://medium.com/@singla1998?source=post_page---byline--513922e4b4b1---------------------------------------)


4 min read


·


Mar 19, 2025


--


In the second installment of Nextdoor’s “Scaling Nextdoor’s Datastores” blog series, the Core-Services team discusses challenges faced after implementing database read replicas.


Adding read replicas to an existing database is a very common pattern as applications or products evolve to handle increased demand. Typically, the implementation details are hand waved and it’s assumed that this strategy will work. However, that is rarely the case, and we’ll dive into some more of the intricacies around the implementation.


## Initial Attempt


When replicas were first introduced in the Nextdoor stack, we gave the product engineers latitude to choose when they wanted to have their query routed to a read replica or to the primary. This was done by leveraging the existing routing mechanism in our ORM, Django.


This seemed like the right idea at the time because the product engineers had the most context around consistency requirements within their changes and load characteristics of their product feature. Therefore, they would have the best ability to judge which node to send their query to. However, as our business logic evolved and became more feature-rich, product engineers began to add abstraction layers to help abstract complex operations away from business logic.


Press enter or click to view image in full size


In this design evolution there is a high frequency read, followed by a low frequency conditional write, followed by a read. The read performed after the write should be routed to the primary, but that may get buried in abstractions and this requirement regressed.


The explicit routing decisions engineers made became buried and subsequently created a serious problem for users of these abstractions. If one abstraction method was performing a write and another a read, they could not safely be used together due to read-after-write consistency issues. Due to replication lag between the primary and replica databases, a race condition arises when the application attempts to read data from a replica after performing a write.


We had created a system where engineers had to be aware of *the entire call stack* and be able to determine if this situation would apply to them or not. The easiest way to handle this situation? Always use the primary…


## The Band-aid


A common solution engineers employed was to wrap higher-order business logic in database transactions because within the context of a transaction, all queries are routed to the primary.


Press enter or click to view image in full size


Shows that the choice to route to primary is made in the abstraction layer so functions A, B, and C all use the primary whether they need to or not.


> Author’s Note: Our default isolation level for transactions, repeatable read, gave engineers a false sense of security, as it only guarded against replication lag and not racing writes with concurrent transactions. We have since improved this to ensure read-your-own-write semantics.


This strategy had a negative effect on database load because it indiscriminately caused all queries intended to be sent to replica databases to be sent to the primary database. The impact of this problem increased as:


1) more business logic leveraged the database


## Get Tushar Singla’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


2) business logic increased its use of existing abstractions


3) query performance decreased as more data was added


What transpired was a years-long erosion of the capacity benefits the additional read replicas initially provided. As a result, the load on the primary database node became one of the most pressing issues with Nextdoor’s database stacks.


It was clear that the initial approach of exposing routing choices to engineers was no longer tenable and the team embarked on a way of making the replica vs. primary decision for the product engineers.


## Reimagining


Using ORMs (Object Relation Mappings) is controversial. There are many pros and cons and we won’t debate all of them here. However, one of the advantages of an ORM is that there is a consistent layer of abstraction between the database and the application. This allowed us to inject a simple piece of custom logic to keep track of which tables have been written to while processing a web request. Why is it helpful to keep track of modified tables? By doing this we could automatically make informed decisions of where to route subsequent read queries, regardless of where they were buried in the business logic stack.


Press enter or click to view image in full size


This simple strategy, coupled with our already high read-to-write ratio, allowed us to shift much of the read traffic to the replica databases and substantially reduce our reliance on the primary database.


> Author’s Note: While the strategy was simple, we did have to cleanup up all of the manual routing decisions along with inappropriate usages of transactions.


While this naive approach was rather effective, we realized that this strategy was actually too conservative. We noticed that our average database replication lag was around 20ms while our web requests lasted an order of magnitude longer. That means that even after the update had been replicated to the primary, we were still disallowing queries for that table to the read replica. This provided an opportunity to use a timing based system that marked tables as re-eligible after p99.9 replication lag had elapsed. With this additional optimization, we were able to re-route most of the queries from our primary to the read replicas.


Press enter or click to view image in full size


## Takeaway


Each application will have its own eccentricities that make RDBMS scalability a challenge for platform teams but we hope this post provides a cautionary tale, as well as a potential solution for similar cases.


In the[next post, Appropriately serializing data for caching](https://medium.com/@ronakts/e9b4dd8a9393) , we’ll cover how we serialize database data for caching and some of the pitfalls to be aware of when introducing caching to an application.


Authors:


[Tushar Singla](https://medium.com/u/e734698562ba?source=post_page---user_mention--513922e4b4b1---------------------------------------)


,[Slava Markeyev](https://medium.com/u/d8db977cf129?source=post_page---user_mention--513922e4b4b1---------------------------------------)


and[Ronak Shah](https://medium.com/u/3d01db79be59?source=post_page---user_mention--513922e4b4b1---------------------------------------)
