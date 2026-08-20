---
schema_version: "1.0.0"
document_id: "6b7a31ba0ef10d8c21991192b1981babb47a3c64ffcb0aab14c22cf27c6bee5c"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/testing-production-ml-configurations-safely-how-we-replay-millions-of-decisions-b965c5aa178b"
published_at: "2026-06-16T19:55:42+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:85f8f6096ad68623df59cd2f86f1f5f3c381581713b5ea238854fa835af101cb"
---

# Testing Production ML Configurations Safely: How We Replay Millions of Decisions

Press enter or click to view image in full size


Software Engineering


Machine Learning


Databricks


Spark


# **Testing Production ML Configurations Safely: How We Replay Millions of Decisions**


## Shared libraries, precomputed data, and distributed execution for large-scale ML validation.


[Guillermina Cledou](https://medium.com/@guillecledou?source=post_page---byline--b965c5aa178b---------------------------------------)


8 min read


·


Jun 16, 2026


--


Riskified is the fraud analysis solution that has your back if it makes a boo-boo. We analyze e-commerce orders in real-time to detect payment fraud and tell merchants whether to approve or decline them. If we approve an order by mistake and it is charged back, we pay for it ourselves — that’s our chargeback guarantee.


Press enter or click to view image in full size


As you can imagine, we need to be fast and sharp when making decisions. Fraudsters are constantly evolving, so we need to continuously adapt our machine learning models and configurations to keep pace. But here’s the challenge: how do you safely test new models before deploying them to production? You can’t experiment on live orders when reputation and real money are at stake.


This is where our simulation system comes in. It lets us replay millions of historical orders offline using updated machine learning models, with the exact same code that runs in production. In this post, we’ll walk through how we architected this system and the tradeoffs we made to balance consistency, scale, speed, and cost.


We rely on many supporting tools, including Kafka, Databricks, dbt (data build tool), CockroachDB, Spark, AWS S3, and Airflow. So if you are wondering how we architected all these tools together to simulate live order analysis, or you are building ML systems where decisions have real consequences, this post is for you.


## The requirements


Let’s talk about what this system needed to do.


**Maintain code consistency:** The simulation must use the same decisioning logic as in production. Even tiny differences would make results meaningless. In addition, it would be hard to maintain two codebases — they’d inevitably drift apart.


**Handle massive scale:** Process anywhere from thousands to millions of orders per simulation. In the past 30 days alone, our simulations ranged from 720 orders to 6 million orders, averaging about 493,000 orders per run.


**Be fast enough to iterate:** Data scientists need results in hours, not days. A 24-hour simulation kills productivity. Since our decisioning logic takes ~100ms per order, a single-threaded approach would mean 1 million orders = 28 hours.


**Stay cost-effective:** Our live system enriches orders by calling external data providers — e.g., email age, address services, and IP geolocation APIs. These charge per call. Re-enriching millions of orders for every simulation would cost a fortune, and it wouldn’t make sense to run it twice for the same order.


These requirements pointed us toward a solution: extract the decisioning logic into a shared library, reuse enrichments from live analysis, and distribute the work with Spark.


## The main idea: Shared Library + Pre-Computed Data


Our live analysis system, Swan (SWift ANalysis), is built in Scala. Here’s what happens when a merchant sends us an order:


- Enrich the order with external data (~400ms)
- Generate features based on enrichments (~200ms)
- Calculate a fraud score using our models + make a an approve/decline decision (~100ms)
- Respond to the merchant and publish everything to Kafka


Total time: under 700ms (p99). Fast, but expensive — those enrichment API calls add up.


### **Extracting the Decisioning Logic**


To enable offline simulation without duplicating code, we extracted Swan’s core analysis logic into a standalone Scala library. This library is stateless and contains only the decision-making logic — no API calls.


**Our current architectural choice:** both our live system and simulation code live in the same monorepo and use the same compiled JAR artifact. When we deploy a new version of Swan, the simulation is automatically updated. There’s no separate “simulation version” that could drift from production.


### The Tradeoffs


The simulation can only test models and configurations — it can’t test brand new enrichments or features. To add a new data source, we must first deploy it to production, let it run on live orders, and only then we can simulate with it. This is fine because we change features less frequently than we iterate on models.


### **Pre-Computing Enrichments and Features**


Here’s where we made another critical tradeoff: pre-computation over real-time generation.


Every time Swan analyzes a live order, it publishes the enriched data and computed features to Kafka. We stream this to our data lake and use dbt to build a gold table — a denormalized view where each row has everything needed to simulate an order:


- Original data sent by the merchant
- All enrichments (the expensive API results)
- All computed features
- The live analysis result


This approach eliminates the cost and time of re-calling external services. Instead of 400ms enrichments + 200ms feature calculation, the simulation reads pre-computed values and spends only ~100ms on decisioning.


**Query optimization is critical here.** The gold table has billions of rows. We use Databricks’ liquid clustering feature to organize data by date, merchant ID, and order ID — the dimensions analysts most often filter by. Liquid clustering is basically a smarter way to lay out data than traditional partitioning, and it lets us change clustering keys without rewriting data.


## The Tools That Make It Work


We orchestrate several tools to implement this architecture.


- **dbt** — Defines daily data transformations to build and maintain our gold table
- **Databricks** — Our data lake platform where simulations run, with liquid clustering for query optimization
- **Scala + Spark** — Distributes simulation work across 500 partitions for parallel processing
- **CockroachDB** — Stores simulation metadata that needs to be immediately accessible and consistent
- **S3** — Houses large model and configuration files that get downloaded at runtime
- **Airflow** — Orchestrates the pre-simulation setup, triggers Databricks jobs, and handles post-processing
- **Kafka** — Our data backbone: carries enrichments and features from live analysis to our data lake, and later carries simulation status updates back to analysts


Each tool plays a specific role, but the key is how they work together: Kafka streams live data, dbt transforms it into our gold table, Airflow orchestrates everything, and Databricks executes the actual simulations.


## **How It All Works Together**


Let’s walk through what happens when an analyst wants to test a new model.


### Setting Up the Data


Press enter or click to view image in full size


An Airflow DAG runs an incremental dbt model every day. This model:


- Pulls in orders from the past two days (to catch any late arrivals)
- Combines the order data + enrichments + features + live results into our gold table
- Updates incrementally, so we’re only processing new data


### Triggering a Simulation


Analysts use internal interfaces to kick off simulations. They can:


- Filter by date range, specific merchants, or order IDs
- Test different models or configuration changes
- Compare new vs. old model performance side-by-side. Since the simulation outputs the same response format as the live system, analysts can see what was decided in production vs in the simulation


We also have automated daily jobs that simulate orders flagged as chargebacks. This helps analysts understand if these orders would now be classified as fraud.


### Behind the Scenes: Orchestration


Press enter or click to view image in full size


When a simulation is triggered:


1. **Backend — Simulation Service** stores metadata in CockroachDB, generates a configuration file, uploads it to S3 (files can sometimes exceed 15 MB), and triggers an Airflow DAG.
2. **Simulation DAG** retrieves model files, stores them in a volume accessible by the simulation, and launches the Databricks Spark job.
3. **Spark job** (Scala) reads the configuration, queries the gold table for matching orders, and distributes work across 500 partitions.
4. **Swan library** processes each order using the broadcasted models (broadcast caching avoids passing huge files to each executor)
5. **Results** get written to a Databricks table.


Since large simulations can take hours, we don’t block and wait. The DAG publishes status updates to Kafka, a consumer reads them, and the analyst gets notified directly when results are ready.


## **The Numbers: What We Achieved**


Over the past 30 days, we ran 93 successful simulations with a 97% success rate, processing 46 million orders total. Here’s how performance scales:


Press enter or click to view image in full size


One of our best metrics: 6 million orders processed in 224 minutes (3.7 hours) at 27,000 orders per minute.


For a typical 1-million-order simulation, we complete it in about 40 minutes. Remember that single-threaded projection of 28 hours? **We’re 42x faster with Spark** .


In terms of cost, we’re paying about **$0.0006 per order in Databricks compute** , or roughly $18 per simulation on average. By reusing pre-computed enrichments instead of re-calling external APIs, we avoid thousands of dollars in data provider fees per simulation.


It’s worth noting that failing simulations are usually due to misconfigured models, so it’s a good thing we simulated them.


## **What This Enables for Our Team**


This architecture transformed how our data science team works.


**Faster iteration:** Model changes that used to take weeks now happen in hours. Analysts can run multiple simulations per day, comparing results side-by-side to understand which model performs best.


**Continuous validation:** Our daily chargeback reclassification runs automatically and tests whether recent chargebacks would now be correctly declined with our latest models. This continuous feedback loop has caught multiple misconfigurations before they hit production.


**Confidence to experiment:** When a new fraud pattern emerges, we can quickly validate potential solutions against historical data before deploying to production.


**Safety + velocity:** The system acts as both a safety net and an accelerator. It prevents bad changes from reaching production while enabling good changes to deploy faster.


## **The Tradeoffs We Made**


**Pre-computation constraint:** We can only simulate with features that already exist in production. Testing brand-new enrichment logic requires deploying to live first.


**Liquid clustering limits:** Four-column maximum means we had to carefully prioritize which dimensions to optimize for query performance. This has made us re-imagine how we store information in the dbt table.


**Non-optimized Spark code** : The Scala code in our shared library wasn’t written with Spark optimization in mind. This means we’re not leveraging some of Spark’s performance optimizations. But this tradeoff is worth it: using the exact same code gives us complete confidence that simulation results match what would happen in production.


These constraints are acceptable because the benefits — code consistency, cost savings, and iteration speed — far outweigh them.


## Wrapping Up


If you’re building ML systems where decisions matter, consider this pattern: extract your core logic into shared libraries, pre-compute expensive operations, and use distributed processing for scale. The combination gives you both safety (production code in testing) and speed (iterate quickly without breaking the bank).


For us, it means responding to fraud faster, refining models with confidence, and sleeping better knowing we’ve tested changes thoroughly before they go live.


### Some Future Improvements


While the system works well, there’s room to optimize.


**Smarter limits:** Right now, we cap simulations at 60 days of data, but this constraint isn’t meaningful. A better approach would be to limit by order count instead.


**Simulating enrichments and features:** Although it might increase costs, we would like to simulate changes to feature calculation and enrichments. Both can provide new information during analysis that can affect the decision.
