---
schema_version: "1.0.0"
document_id: "3b8d32831c760af0a3f81010a65850bec5d62dd21b5ea5645ef3d9a03f4cade7"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/postgres-to-clickhouse-migration"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-24T05:02:31.373763+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:6baff65f1b9adfdffa6a8d8fec02bb1f7a8413df43670455d0d5a62e18919e09"
---

# How We Ditched Postgres for ClickHouse to Process 12 Billion Caches Per Day

*This is part 2 of our series on how Momentic scaled from 80k to 1B caches and successfully migrated from Postgres → Clickhouse. Read part 1:[Most browser agents "see" the DOM. Ours had to understand user intent.](https://momentic.ai/blog/teaching-browser-agents-user-intent)*


## **Growing pains using Postgres**


Adding more values to the cache key fixed many of our consistency problems, but it also meant that we went from having around 80k active cache entries to now approximately 1B.


Our previous setup was simple: we stored the caches in a single table in Postgres, but this started to show cracks pretty quickly. Because of the high-write, high-read workload, we were running into both elevated resource usage as well as lock contention from queries trying to read and write to the cache concurrently, and as we increased the number of entries by several orders of magnitude, this only became worse.


Old system (Postgres only)


## The decision to move to ClickHouse


We decided to move our storage backend to ClickHouse in order to improve the performance of the cache lookup query. As Momentic grew, we were running it 600k times per day. Because caches are such a critical piece of our infrastructure, we needed to make sure that we could serve them with 100% uptime and sub-second latency. In theory, because we essentially only had a single query (select caches where test ID, CLI version and branch name and commit timestamp match some filters) we could leverage ClickHouse’s primary key to make that query super performant.


In order to understand this decision, it’s important to understand what Momentic’s cache data looks like. The cache is keyed by test ID, step ID, Momentic version, git branch, and commit timestamp. For a given test ID, Momentic version, git branch, and commit, there are essentially a fixed number of values (the number of steps in the test, most often between 10 and 100). Postgres indexes are B-tree based, which means that the query cost will always increase with the amount of data. On the other hand, ClickHouse uses a sparse primary index which means that if we know the four key values for every query, we can very efficiently narrow down the search space to just a few granules.


## How we optimized our ClickHouse architecture


### Choosing the right primary key


ClickHouse’s primary key got us 90% of the way there.


On **non-main branches** , we were able to easily identify the data part containing the cache entries for our test, read it into memory, and compute the matching entries (10k-30k rows).


Queries on a feature branch


But on **main branches** , we weren’t so lucky. We were still searching over all entries with a lower commit timestamp than the current commit’s (potentially 500k+ rows). This meant that most queries were reading 1-2 data parts, but some outliers were reading almost all of the parts on every query, leading to spiky memory usage and disk operations.


This also made it very difficult to serve traffic reliably since query performance was heavily dependent on the specific customer’s data and usage pattern, and a few rogue queries could send our infra into a tailspin.


Queries on main before optimization


‍


To address this problem, we used a **materialized view** to precompute all of the available commit timestamps for a given test ID. This view was significantly smaller than the main table and could trivially be used to determine the best available commit timestamp.


Once we had that we could search the main table for entries with that specific timestamp, thus narrowing it back down to one or two parts.


## Replacing <updates> with <inserts> for extending TTL


In Postgres, we would make three queries for every test run:` SELECT` to get the caches,` UPDATE` to increase the TTL for the caches that were used (this was debounced using Redis), and` INSERT/UPDATE` to store the updated caches.


This didn’t play super well with ClickHouse because potentially 2/3 queries are updates, which aren’t very performant. Instead we switched to using only` INSERTS` combined with ClickHouse’s` ReplacingMergeTree` :` SELECT` to get the caches, re-` INSERT` the caches that were used to extend TTL,` INSERT` the new caches after the test run, and let ClickHouse take care of deduplicating entries asynchronously. This was such an improvement that we were able to fully eliminate the Redis layer, which at this point had limited value due to the higher cache key cardinality.


From this:


Old system using Postgres and Redis


To this:


New system using ClickHouse


‍


## How we migrated from Postgres → ClickHouse


### Double write


In order to ensure that no data was lost during the migration and to validate our new approach, we started by double writing caches to both Postgres and ClickHouse. The 14 day stale-cache expiration guaranteed that after two weeks, the two databases contained exactly the same values.


### Double read + consistency check


Once the same data was stored in both systems, we began validating the correctness and performance of our ClickHouse queries. During this phase, users were served caches from Postgres, but in the background we executed the same query against ClickHouse and compared the results, flagging any discrepancies.


This allowed us to:


1. Make sure that our results were consistent between the two databases, and
2. Validate that our ClickHouse architecture was performant and held up at scale


It was during this phase that we were able to iterate and improve performance by creating the materialized view of commit timestamps.


## The switch to ClickHouse


Once we were confident in our ClickHouse setup, we gradually cut over production traffic from Postgres to ClickHouse, still keeping the double write in case we needed to roll back. After an initial validation period, we stopped writing to Postgres in the background.


New system (Clickhouse only)


## The results


Migrating to ClickHouse has allowed us to handle over two million cache queries per day, processing almost 20 billion cache entries every day while maintaining ~250ms resolution latency on average. This new infrastructure allowed up to roll out the accuracy improvements we wrote about in our[previous article](https://momentic.ai/blog/teaching-browser-agents-user-intent) at scale.


Cache queries per day in ClickhouseCache resolution latency by client configuration


## Next steps


Going forward, some challenges that we still want to solve include stronger relativity checks and better validation for SVGs:


- While we currently capture information about related elements, we still don’t do a good job encoding how they need to be related to the target. In an ideal world, we would also enforce things like how close the elements are, where they’re positioned relatively, or how they’re related in the DOM.
- Many of our customers’s sites contain a lot of icons or icon buttons. Often, these buttons are positioned closely together and are easy to mistake for each other. In some cases, when there are aria attributes or other labels, we can handle these well. We are working towards the ability to validate that our cache resolves to the same icon every time using the SVG itself, even if the site isn’t accessible.


‍


*Thanks for reading! Read part 1 of our series on caching:[Most browser agents "see" the DOM. Ours had to understand user intent.](https://momentic.ai/blog/teaching-browser-agents-user-intent)*


*If this kind of problem interests you: connect with us on[LinkedIn](https://www.linkedin.com/in/hungweiwu/) ,[X](https://x.com/MomenticAI) , or check out our open opportunities at[momentic.ai](https://momentic.ai/) .*


‍
