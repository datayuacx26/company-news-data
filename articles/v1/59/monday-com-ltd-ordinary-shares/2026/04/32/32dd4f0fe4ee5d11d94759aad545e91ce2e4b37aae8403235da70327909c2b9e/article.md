---
schema_version: "1.0.0"
document_id: "32dd4f0fe4ee5d11d94759aad545e91ce2e4b37aae8403235da70327909c2b9e"
company_key: "monday-com-ltd-ordinary-shares"
company: "monday.com Ltd."
source_id: "monday-com-ltd-ordinary-shares-rss-78444ad48313"
canonical_url: "https://engineering.monday.com/redesign-authorization-to-monday-com-scale/"
published_at: "2026-04-26T14:06:03+00:00"
first_seen_at: "2026-07-20T23:22:14.841151+00:00"
fetched_at: "2026-07-28T20:52:06.558365+00:00"
content_hash: "sha256:718ad924995851adb2e2f897f01d844460a0fb175131b3569292098ce7b1e443"
---

# Redesign Authorization to monday.com Scale

Over the last year, we changed one of the most important building blocks of monday.com – the authorization system. We didn’t simply rewrite it or update tools. We changed the basics: where we fetch data from, how it is saved, and how the decision logic itself works. As our legacy MySQL databases approached their scaling limits, we redesigned the system to meet monday.com’s scale and standards, unlocking support for larger and more demanding customer use cases.


This post walks through the pressures that made the old model unsustainable, the new data and decision flow we designed, and what it unlocked for monday.com going forward.


## **Where We Started and the Pain**


Authorization sits on the critical path of almost every request at monday.com. Every page load, mutation, or background job goes through it, often multiple times. That makes it both a security boundary and a performance bottleneck by default. A wrong decision is a security issue, and a slow one degrades the entire product.


As monday.com scaled and customer configurations grew more complex, authorization stopped being a simple gate and became a core runtime dependency that had to be consistently correct while operating under strict latency constraints.


### **The cost of enrichment on the critical path**


Our original decision engine was built around real-time data enrichment. For every request, the flow queried multiple internal and external data sources, aggregated their responses, and only then executed its decision logic.


In theory, this approach ensured that every decision was made using the most up-to-date information. In practice, it meant the engine was only as fast and reliable as the slowest dependency.


Waiting on these data sources added roughly 200ms to the request path. Worse, this latency was highly variable. A single slow response or transient failure could cascade into timeouts, retries, and could cascade into timeouts and retries, risking customer experience.


### **MySQL as a bottleneck**


Behind the scenes, the engine relied heavily on a MySQL database to fetch the data we owned. While MySQL served us well in earlier stages, it became a limiting factor as traffic grew:


- Horizontal scaling – supporting more request load- was complex and operationally expensive, requiring constant assistance from our DBA team.


- A few heavily used tables caused uneven performance, and even normal data updates could impact the system’s responsiveness.


- Dependencies on data stored in shared databases owned by other services increased coupling and widened the blast radius during incidents.


More importantly, the engine depended on tier 2 and 3 microservices that shared this data. These services were not designed with the same availability or latency guarantees as our core systems, yet they sat directly on the critical path.


Combined, these challenges pushed the system beyond its limits, forcing a re-architecture.


### **Our goal**


We set three clear goals for the projects:


1. Increase stability by reducing reliance on non-tier-1 services or single points of failure.


2. Be ready for a 10X scale.


3. Maintain correctness and customer experience, with minimal visible change during migration.


The key idea was simple: instead of fetching data at decision time, we would replicate the data ahead of time and make decisions using local, in-memory state.


## **Designing the New Backbone**


### **Replication instead of enrichment**


At the heart of the new architecture was a shift from pull to push.


Rather than querying data sources on demand, we replicated relevant data into the decision system asynchronously. This allowed the engine to operate independently while maintaining a locally consistent view of the world.


This required building reliable data pipelines with near-real-time data propagation ensuring consistency while delivering major gains in latency and resilience.


### **WAL-based CDC and flush mechanism**


We implemented a WAL-based Change Data Capture (CDC) pipeline.


- We consumed changes when they were written to our data sources and duplicated to our DynamoDB table.


- While listening to the DynamoDB stream, we could send updates to serving pods with new changes.


- To load tenant data, a pod retrieved a data “snapshot” from S3. These snapshots were generated periodically by a flush mechanism to reduce DynamoDB costs.


This approach gave us:


- Reliable replication


- Close to real-time updates


- Fast recovery after failures or restarts


### **The stickiness layer**


To achieve sub-millisecond latency, tenant data was loaded into pod memory, with updates from the WAL-CDC system providing a complete view. The previous round-robin routing randomly distributed requests, forcing each pod to constantly preload new tenants and evacuate old ones to prevent out-of-memory errors.


To avoid it, we introduced a stickiness layer that ensures requests from the same tenant are consistently routed and evaluated in the same pod. This minimizes data load, cost, and latency.


## **Migration Strategy**


### **One data source at a time**


Rather than attempting a huge single migration, we moved data sources incrementally.


Each source was:


1. Replicated to our DynamoDB.


2. Validated for completeness and correctness.


3. Gradually introduced into the decision flow.


This allowed us to increase impact safely while limiting risk.


### **Integrity checks everywhere**


Throughout the migration, integrity checks were non-negotiable.


We continuously compared:


- Raw data between old and new sources.


- Decisions made by the old engine vs. the new engine


- Data freshness and completeness


Any discrepancy was investigated before traffic was increased.


## **Impact and Results**


### **Minimal customer impact**


From the outside, the change was almost invisible.


- No API changes


- Minimal required customer action


- No degradation during rollout


This was a key success metric for us.


### **Independence from Tier 2–3 providers**


While we continue extending this architecture to additional components, the ones we’ve migrated achieved complete independence from other data sources.


Failures or latency spikes in downstream services no longer directly affect these flows. This has strengthened fault isolation and improved overall system stability.


### **Latency improvements**


By calculating decisions entirely in memory, we removed the most expensive part of the request path.


- No synchronous enrichment calls


- No database reads on the critical path


- In memory-only calculations for over 99% of the requests


The result was a significant reduction in latency, both P50 and P99, with far more consistent response times.


We lowered our P50 latency from 240ms to 6ms, and P99 latency from 720ms to 80ms.


### **A stronger foundation**


Beyond the immediate gains, the new architecture gives us a much stronger foundation for future work:


- Fully automated horizontal scaling.


- Clear data ownership boundaries.


- Split between configuration definitions and evaluation logic


## **Closing Thoughts**


This project was not about chasing novelty. It was about accepting that a system which once served us well no longer matched our scale or reliability requirements.


By moving replication upstream, simplifying the decision path, and migrating carefully, we were able to make a big architectural change with minimal customer impact.


While the journey continues, the core foundation is now in place. Customers are already benefiting from faster performance, improved stability, and support for new capabilities. In large-scale systems, laying that foundation is often the most difficult milestone.
