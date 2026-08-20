---
schema_version: "1.0.0"
document_id: "52f8b3145ffc46362b4a2115669fb6523c13d09708c83812760313544cbc0ba6"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/the-one-view-odyssey-architecture-aggregations-and-1-billion-rows-in-snowflake-21008dcca2fd"
published_at: "2026-03-18T09:25:07+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-08-20T02:06:38.087890+00:00"
content_hash: "sha256:219c7c5dd0440e507ef9566d02da52beb3be595ca933c1269f422e95a9f4d51c"
---

# The “One View” Odyssey: Architecture, Aggregations, and 1 Billion Rows in Snowflake

#### Breaking down the practical challenges of building one view architecture


In Data Engineering, there is a difference between writing a query and ensuring that a billion rows of data actually make sense. My goal in writing this post is to share the architectural decisions and practical “war stories” from a massive data consolidation project, offering a roadmap for others facing similar scale challenges.


Recently, I finished a project that was the perfect example of this: building a Merchant-Facing ‘One View’ Table. The goal sounded simple, almost innocent: create a single source of truth for our external customers, consolidating data from multiple sources into one clean, performant table. Sounds standard, right? But when you introduce over 1 billion rows, drastically different levels of granularity, and data that changes its mind about the past (retroactive updates), it turns into a masterclass in SQL modeling and pipeline orchestration.


This project was led by the Data Engineer team because of our unique ability to bridge the gap between complex data structures and business value. In this post, I’ll break down the practical challenges of building this architecture and how we saved our Dev team from having to manage this monster themselves.


### The Architecture of Complexity


When people outside the field hear “Data Modeling,” they think of a simple join between two tables. But when you are building a product for external merchants, the stakes change. If an internal company dashboard is slightly off, you fix it in the morning. If a merchant sees incorrect data regarding their money or performance, you lose their trust instantly.


The project stood on three pillars of difficulty:


- **Scale vs. cost:** How do you handle 1 billion rows in Snowflake without burning through the annual budget in a week?
- **The granularity puzzle:** How do you stitch together sources that live at different hierarchy levels (Single Store vs. Regional Cluster vs. National Parent Company vs. Industry Benchmarks)?
- **The moving target:** How do you handle retroactive data changes while maintaining a strictly incremental process?


### 1. The Incremental Nightmare: How to Update a Billion Rows


The first rule of the 1-billion-row club is: Never Full Refresh. In Snowflake, compute time is money. Re-loading a table of this size, involving heavy Window Functions and complex joins, is an expensive and unacceptably slow process. We knew we had to work incrementally.


But here was the catch: Our data isn’t just “Append-only.” We deal with retroactive changes. A record from two weeks ago might be updated in the source system today. If we only look at what is “new” based on a simple timestamp, we miss the updates to the historical data.


The solution: The Look-Back & Watermark Strategy. We designed orchestration logic that asks the source system: “What has changed in your records in the last X hours, regardless of when the event actually occurred?” By using this “modified at” approach, we could surgically identify only the “dirty” records and update only the necessary slices of our billion-row pie. This required meticulous planning in the orchestration layer, but it made our pipeline bulletproof and cost-effective.


### 2. Bridging the Granularity Gap (Comparing Apples to Airplanes)


One of the most grueling parts of the project was the Data Mapping. We had multiple data sources, and each one spoke a completely different language:


- Source A provided data at the Order level.
- Source B arrived at the Shop level.
- Source C provided data at the Entity level.


The challenge was that we needed to present all of this data across four different layers of aggregation: Single Store, Regional Cluster, National Parent Company, and Industry-wide Benchmarks.


To build a “One View” dashboard, you need a common denominator. You cannot join data at different levels without creating duplicates or skewed calculations. This is where the hard work of creating the “Common Ground layer” began. The strategy was to push everything down to the lowest possible grain (the Shop level) before even thinking about a Join. This required building complex mapping tables that could “break down” a network-level data point into its constituent shops. Only once all sources were standing on the same starting line and at the same granularity could we begin the actual aggregation process.


**Pro tip:** Never try to aggregate and join in the same step. First, flatten everything to a single level, clean it, and only then build your One View on a stable foundation.


### 3. Beyond the CTE Skyscraper: Managing Complexity with Staging Tables


Snowflake is a powerful tool, but your SQL can quickly become a readability nightmare when you need to show data across shifting hierarchies: **Store -> Cluster -> National -> Industry.**


We needed to calculate complex metrics at every one of these levels. While many resort to a “CTE Skyscraper,” the correct solution for this scale is to create physical staging tables. This approach helps to read, understand, and debug problems by ensuring each table contains a specific calculation that creates a pipeline chain.


To keep the code readable (and to keep my sanity during debugging), we built a logical chain:


- **Base Staging Table:** Data cleaning and type standardization.
- **Granularity Staging Table:** Bringing all sources to the Shop level.
- **Metric Staging Tables:** Layer upon layer, where each table handles exactly one level of aggregation to ensure the logic is isolated.
- **Final Select:** The unification of all these layers into the final table.


This approach made QA much simpler. If a calculation error appeared at the National level, we knew exactly which staging table in the pipeline to investigate.


### 4. The Round Process: Solving for Retroactive Changes


This is perhaps the most interesting technical part. How do you update data that has already been published? We developed a workflow called the “Round Process.” Instead of just appending rows, our pipeline performs a multi-step dance:


- **Identify:** Finding every record that changed retroactively in the source.
- **Insert:** Injecting the new versions of these rows into the system.
- **Flagging:** Running a post-process script that flags the old, outdated rows as “irrelevant”. This allowed us to maintain a history of changes (for internal auditing) while ensuring the merchant only sees the most “truthful” and updated version of their data at any given second.


**5. Business Value: Why a Data Engineer should lead this project**


At some point, the question arose: Why shouldn’t the backend developers just pull this data directly from the operational DB? The answer is **Architecture and Efficiency** . While the Dev team is more than capable of handling complex logic, asking them to manage massive aggregations and hierarchy mapping within the application code isn’t the best use of their time or the system’s resources.


The Data Engineering team was the right choice because we bring a unique perspective to the table: a deep understanding of the data catalog, the overall data architecture, and how this data interacts with other products across the organization. By handling the heavy lifting in the Data Warehouse, we kept the application backend lean and fast. We managed billion-row joins and retroactive updates so that Devs could focus on core product features. They received a clean, performant, and accurate table, while we ensured the business logic was solid and scalable. It was about each team owning the layer where they provide the most value.


### Conclusion: Sisyphus Work, Not Magic


If you are a junior or mid-level engineer, here is the honest truth: there was no “Eureka!” moment. There wasn’t one single line of genius code that solved everything. The success of this project was the result of systematic, tedious, and often exhausting work:


- Running a query, finding 500 mismatched rows, and digging through the source to find the edge case.
- Fixing a bug in a staging **** table, only to realize it broke an aggregation at a different level.
- An endless cycle of Plan -> Implement -> QA -> Fix -> Repeat.


We built this so our merchants could make smarter business decisions. When I see the dashboard load in seconds, showing perfectly synced data across four levels of hierarchy — I know that every hour of QA and every staging table was worth it.


### Final takeaways:


- If you are a professional Data Engineer, you know that the work is about much more than just generating reports. Early involvement in the data design and requirements gathering ensures that architectural decisions are rooted in the data context, which is key to a successful product.
- Pre-planning saves weeks of work: don’t rush into writing SQL. Spend your time on the mapping.
- Embrace the QA: In a table of a billion rows, edge cases aren’t “rare” — they are guaranteed.


**What is the most complex project you’ve built in Snowflake? Let’s swap “horror stories” from the field in the comments!**


---


[The “One View” Odyssey: Architecture, Aggregations, and 1 Billion Rows in Snowflake](https://medium.com/riskified-technology/the-one-view-odyssey-architecture-aggregations-and-1-billion-rows-in-snowflake-21008dcca2fd) was originally published in[Riskified Tech](https://medium.com/riskified-technology) on Medium, where people are continuing the conversation by highlighting and responding to this story.
