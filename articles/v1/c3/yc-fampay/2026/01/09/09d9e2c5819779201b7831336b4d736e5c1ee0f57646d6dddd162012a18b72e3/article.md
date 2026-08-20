---
schema_version: "1.0.0"
document_id: "09d9e2c5819779201b7831336b4d736e5c1ee0f57646d6dddd162012a18b72e3"
company_key: "yc-fampay"
company: "FamPay"
source_id: "yc-fampay-rss-25fb68ce5f72"
canonical_url: "https://blog.famapp.in/blog/building-a-50-ms-home-feed-part-1-design/"
published_at: "2026-01-02T10:29:08+00:00"
first_seen_at: "2026-07-20T23:23:51.041833+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:5817b01e29f51f07d7a1a3e70d929548334507260347230a18c4d5787b48da89"
---

# Building a 50ms Home Feed — Part 1: Design

**Designing the 50 ms Home Feed**


As Famapp's user base expanded to over 7 million monthly transacting users, we began to encounter significant performance bottlenecks in our infrastructure. These issues became particularly apparent during evening usage surges and UPI ecosystem degradations, when increased app reloads would cause traffic to spike uncontrollably.


The most resource-intensive component of our system was the home feed API—the critical API responsible for loading the app's first screen. This API was designed with maximum flexibility in mind, dynamically rendering different UI elements based on user segments. iOS users might see Apple Store codes while Android users would receive Google Play recharge options. High-spending users could receive subscription nudges while others wouldn't.


Furthermore, our home screen was designed to be highly responsive to user behaviour, potentially refreshing its elements after each user action. For every app open, our system would:


1. Check the user's current state
2. Fetch all possible UI elements
3. Determine which elements to display


This approach served us well in Famapp's early stages, but as we scaled, our monolithic Django service began showing signs of strain. Most concerning was the home screen's loading time, which had reached an unacceptable p95 latency 2-4 seconds—creating a poor first impression for users expecting a fluid transactional experience.


When your home feed takes 4 seconds but users reload 10 times because of failures in UPI


The key reasons why these latencies were very high:


- Every request rebuilt the entire home page from scratch, repeating the same database queries and filtering logic even when nothing had changed.
- Django’s ORM, while convenient early on, introduced hidden *n+1* query patterns that inflated network latency.
- And since our monolith also handled mission-critical flows like payments, these redundant rendering requests were eating into CPU time meant for golden paths.


After analysing these challenges, we established two key objectives for our solution:


- Reduce home screen loading time from 2-4 seconds to under 50ms
- Preserve our product team's ability to dynamically modify UI elements without technical constraints


This is the story of how we redesigned our system to meet these ambitious goals while maintaining the flexibility our product team relied on.


## Why? Why 50ms?


We set the 50 ms target based on established responsiveness benchmarks. Human perception studies show that interactions completing within 100 ms feel instantaneous, while delays beyond 300 ms start to feel sluggish, and anything over 1 second breaks the user’s sense of flow. Since the home feed is the first screen users see, we aimed for sub-perceptual latency—ensuring it loads faster than users can consciously notice.


## How?


Working with our talented engineering team (yeah the same best minds who are working on it 😅), we conducted a thorough analysis of our existing system challenges:


1. **Redundant Calculation Overhead** : Our home screen rendering process recalculated display elements on every request, evaluating user cohorts, recent activities, wallet balances, and other parameters. We discovered that many of these calculations were unnecessary, as users frequently refreshed without any meaningful state changes occurring.
2. **Cascading Failure Patterns** : During payment infrastructure degradations, we observed a problematic feedback loop: users would repeatedly reload the app when transactions appeared to stall, triggering a surge of redundant home screen calculations. This pattern created resource contention precisely when our system needed to prioritise processing critical payment flows.


These findings highlighted how our rendering approach not only consumed excessive resources during normal operation but also exacerbated system strain during partial outages—effectively creating an avalanche effect in the system


After thorough assessment, we determined that our customisable rendering system remained conceptually sound, but required significant efficiency improvements. We developed a comprehensive strategy targeting key performance bottlenecks:


- **Caching Card Configurations** : Implemented caching on client side to eliminate repeated database calls to get the same data. If the client has the home feed and there is no change in the response then no extra database calls! Problem solved!
- **Request Consolidation** : Introduced a mechanism so that concurrent requests for identical card configurations would trigger only a single database operation with shared results.
- **Eliminating N+1 Query Patterns** : Refactored our code to prevent hidden database calls caused by Django's ORM in our object-oriented architecture. We implemented request-level caching to ensure any resource fetched once would be reused throughout the request lifecycle.
- **Architectural Shift** : Began migrating from our monolithic Django service to a lightweight Golang implementation with direct SQL queries instead of ORM, reducing overhead and improving response times. Independent[TechEmpower benchmark](https://www.techempower.com/benchmarks/?ref=blog.famapp.in#section=data-r23) results have shown that optimized Go services consistently outperform the fastest Django setups—typically delivering **5–10× higher request throughput** and **2–4× lower p95 latency** under equivalent load. In our case, using Go’s native` pgx` driver, strict type system, and efficient JSON serialisation reduced memory allocations and parsing overhead, resulting in a leaner and faster service.


This multi-pronged approach allowed us to address performance bottlenecks at each layer of our stack while maintaining the flexibility our product team required.


## Home Screen Architecture


Our home screen follows a hierarchical structure designed for maximum flexibility and personalisation:


1. **Primary Home Section** : The main container that encompasses all interactive elements visible to the user upon app launch.
2. **Contextual Groups** : Within the primary section, content is organised into logical groups based on functionality or user relevance (e.g., "For you section”, “Quick actions” etc.)
3. **Contextual Cards** : Each group contains individual cards that serve as the atomic units of our interface. These cards dynamically display:


- Customised imagery tailored to user preferences
- Personalised text reflecting the user's current state
- Interactive elements that adapt based on usage patterns and eligibility


This nested structure allows our product team to precisely target content delivery at multiple levels, ensuring each user receives a home screen optimised for their specific context and needs.


## Why so dynamic? Why a backend driven architecture?


Our home feed is entirely **backend-driven** , allowing the product team to dynamically control what each user sees without requiring app releases. Every card’s content, layout, and eligibility logic are generated on the server based on real-time user attributes—such as transaction history, verification status, and feature access. This design enables us to experiment rapidly, run targeted campaigns, respond instantly to ecosystem changes and most importantly provide a personalised experience to our users.


## Balancing Dynamism and Performance


Our challenge was clear — we couldn’t compromise on the flexibility that made the home feed valuable, yet we needed to bring its performance closer to that of a static page. A fully cached or precomputed home feed would have been extremely fast, but it would also strip away the personalisation that defines the product experience. On the other hand, fully dynamic rendering ensured contextual relevance but came with significant computational cost.


We needed an approach that could **retain the home feed’s dynamism while avoiding redundant recomputation** .


The key insight was that not every user attribute changes frequently. If we could identify exactly *which variables* a home screen depends on — and determine *when* those variables last changed — we could safely reuse previously rendered results instead of recalculating everything from scratch.


We also realised that the home screen didn’t need to be real-time to the millisecond. Allowing a **small relaxation window of around one second** would still feel instantaneous to users while drastically reducing backend load.


This thought process led us to design a **conditional caching mechanism** , where responses could be reused as long as none of their dependent variables had changed. That reasoning ultimately formed the foundation for our **ETag-based caching system** — a way to preserve full dynamism while achieving near-static response times.


---


---


## **Implementing ETag-Based Caching**


We adopted an ETag-based caching strategy inspired by frontend systems and applied the principle of disaggregation—keeping heavy home-feed computation out of the request path. With ETags, the server does minimal work unless data changes, because *the fastest operation is the one you don’t do* .


### **What is Etag?**


An **ETag (Entity Tag)** serves as a **fingerprint** for an API response, uniquely identifying its current version. When the underlying data changes, the ETag changes too, helping the client know whether it can safely reuse cached data or needs to fetch fresh content


### **Fundamental Working**


- **Server generates an ETag** for a response (usually a hash of the content).
- **Client caches** the response along with the ETag.
- On the next request, the **client sends the ETag** in the` If-None-Match` header.
- The **server compares** the client's ETag with the latest version:


- If **unchanged** , server replies with **304 Not Modified** (no data transfer needed).
- If **changed** , server sends the new data with a new ETag.


This pattern significantly reduces server load by eliminating unnecessary rendering and data transfer when a user's state remains unchanged between requests. The approach is particularly effective during system degradations when users frequently reload the app, as it converts potentially resource-intensive rendering operations into lightweight validation checks.


After deciding on an ETag-based caching strategy, we faced a critical implementation question: *how could we efficiently determine when cached content should be invalidated?*


## The Invalidation Vector Approach


We developed a system based on " **invalidation vectors** " - specialised data structures that represent specific conditions under which a cached response becomes invalid. The elegance of this approach lies in its precision:


A visual representation:


**What is a vector?**


A **vector** is a lightweight data structure that tracks the validity of a cached response. Each vector stores:


- **Key →** A unique identifier (e.g., user ID, home contextual card ID).
- **Value →** A timestamp derived from the` date_modified` column in our database. If this timestamp is **greater than** the one associated with the cached response, the vector is considered **invalid** .
- **TTL →** The duration for which the vector remains valid before it must be refreshed.


Each vector represents a specific component or state that, if changed, would necessitate a fresh rendering of the responses that depend on this vector.


1. **Composite Validation Logic** : The entire response validity depends on AND logic across all invalidation vectors. If any single vector becomes invalid, the entire cached response must be regenerated.
2. **Hierarchical Dependency Mapping** : We mapped dependencies between different vectors to understand cascading invalidation effects. For example:


- If a user's KYC status changes, it invalidates the user vector
- If a home contextual card's image mapping changes, it invalidates the corresponding card vector


3. **Targeted Invalidation** : Each invalidation vector is associated with specific IDs (such as user IDs or card IDs), ensuring that changes only invalidate relevant cached responses.


So we ended up making two kinds of vectors:


- **Primary Invalidation Vectors** : Directly linked to a specific response and determine its validity
- **Secondary Vectors** : Can trigger invalidation of primary vectors through defined dependency relationships. Example: KYC table linked to user, Card image map table which invalidates the card itself.


This two-tier approach creates a invalidation cascade system, where changes propagate through our dependency graph, ensuring we only regenerate responses when truly necessary.


- Red: Primary Invalidation Vectors
- Green: Secondary Vectors


As we can see above, any change in the User group map table for example will invalidate the user vector (the primary invalidation vector)


Once the invalidation vectors were in place, the next challenge was determining how to effectively track the changes that should trigger a cache invalidation. We evaluated two approaches:


### Option 1: Manual Event Emission


In this method, specific sections of our monolithic service would be modified to emit change events to a message broker (such as Kafka). Consumers would then process these events in line with our dependency graph to invalidate the appropriate vectors. While this method offers direct control over which events trigger an invalidation, it comes with significant downsides:


- **Increased Code Complexity** : Embedding event emissions into the existing codebase adds extra lines of code, making the monolith more cumbersome to maintain.
- **Consistency challenges:** Ensuring event emissions perfectly match database state changes introduces a **consistency nightmare** , especially when failures, retries, or partial rollbacks occur. Even minor mismatches can lead to stale or prematurely invalidated cache entries.
- **Scalability Concerns** : Every new feature would need to incorporate this mechanism, raising the risk of inconsistencies and oversight as the system evolves.


### Option 2: Change Data Capture (CDC) from PostgreSQL (Preferred)


Our preferred solution leverages PostgreSQL's Change Data Capture (CDC) capabilities. Here’s how it works:


- **Automatic Change Detection** : PostgreSQL logs every transaction in its Write-Ahead Log (WAL). By consuming these WAL logs, we can automatically capture all changes made to tables that either contain or affect invalidation vectors.
- **Minimal Code Modification** : This approach operates independently of the application logic. It requires little to no modifications to the existing codebase, ensuring that new features integrate seamlessly without additional overhead.
- **Dynamic Decision Making** : Based on the type of operation (such as UPDATE or INSERT) and the specific table affected, our CDC system evaluates which invalidation vectors need to be refreshed. This ensures that the caching system remains accurate and up-to-date with minimal manual intervention.


The CDC-based system thus provides a robust and scalable solution, significantly reducing the need for manual code changes and allowing us to operate entirely outside the core application code. This not only simplifies maintenance but also ensures that our caching mechanism stays aligned with the dynamic nature of our data.


**From planning to execution**


Part 1 brought us to a clear, battle-tested plan: reduce redundant computation, eliminate N+1 patterns, introduce ETag-based conditional caching, and track data changes precisely using invalidation vectors and CDC. With the design in place, the next challenge was turning this blueprint into a real, production-ready system.


In[Part 2](https://blog.famapp.in/blog/rebuilding-the-home-feed-how-homelander-learned-to-respond-in-50ms-part-2/) , we dive into how we built Homelander—the service that made sub-50 ms home feed responses possible.


### Tags


- [Tech](https://blog.famapp.in/blog/tag/tech/)
