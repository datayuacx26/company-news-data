---
schema_version: "1.0.0"
document_id: "fe01afffc1978e37368bd0f292cf4fa741a61a05111f908cd7439549cc0f7cf8"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/how-ai-coding-is-breaking-synthetic-data-generation/"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:20:47.930048+00:00"
content_hash: "sha256:27a8f47a2927ef80cf4a98fe775ac13f1d7baa3659c1edebcecbd6dc1f4e174f"
---

# How AI Coding Is Breaking Synthetic Data Generation

# Executive Summary


Traditional synthetic data generation approaches, still called “Test Data Management” (TDM) by legacy vendor, were designed for a world where applications were monolithic, databases were the center of gravity and change happened slowly. The world looks a lot different now.


Modern systems are distributed, often times event-driven, and increasingly powered by streaming data and AI agents. In this environment, **batch-oriented synthetic data generation fails to capture how systems actually behave** . Worse, they reinforce observability gaps created by PII concerns, leaving teams with data that is technically “safe” but practically useless.


This post explains why traditional synthetic data generation struggles in modern architectures, why AI agents are accelerating this breakdown, and why safe streaming access to production behavior, instead of static datasets, is the missing ingredient for quality testing.


---


# 1. A quick recap: the PII problem


In[Part 1](https://speedscale.com/blog/the-pii-testing-dilemma/) , we established a core tension:


- Production data is the best test data
- Production data is full of PII and sensitive context
- Teams respond by locking data down entirely


We also discovered that PII is often hidden in Base64-encoded fields, JWTs, nested JSON objects, and binary formats like gRPC and Protobuf. This means teams often don’t know where PII exists until it’s already leaked, making “just mask the data” an increasingly impossible task.


Traditional synthetic data generation, or what legacy vendors still call “Test Data Management” (TDM), emerged as a way to *work around* this problem, but it was never able to solve it fully. As we saw in Part 1, AI coding agents are stochastic systems that compound uncertainty when working with synthetic data. Now, with AI coding agents generating code at unprecedented speed, the limitations of batch-oriented synthetic data generation are being exposed faster than ever.


---


# 2. What synthetic data generation was built to do


At its core, traditional synthetic data generation (still called “Test Data Management” or TDM by legacy vendors) focuses on:


- Replicating production databases into test environments
- Masking or tokenizing known sensitive fields
- Subsetting large datasets to manageable sizes
- Refreshing test data on a periodic schedule


Leading platforms that traditional vendors still market as “Test Data Management” like[Delphix](https://www.perforce.com/products/delphix) ,[Broadcom Test Data Manager](https://www.broadcom.com/products/software/test-data-management) ,[Informatica TDM](https://www.informatica.com/products/data-integration/test-data-management.html) ,[IBM InfoSphere Optim](https://www.ibm.com/products/infosphere-optim) ,[K2view](https://www.k2view.com/) ,[GenRocket](https://www.genrocket.com/) ,[Tonic.ai](https://www.tonic.ai/) , and[Redgate Test Data Manager](https://www.red-gate.com/products/dba/test-data-manager/) were built around these batch-oriented workflows.


This works reasonably well when:


- The database is the system of record
- Data models are stable
- Workloads are predictable
- Tests care more about schema correctness than behavior


Those assumptions no longer hold — especially as AI coding agents generate code that interacts with systems in ways that traditional synthetic data generation can’t anticipate or capture.


💡 Tip


Classic TDM failed because it centralized control in a world that was moving toward developer autonomy, automation, and disposable infrastructure.


---


# 3. The batch-processing assumptions baked into synthetic data


Traditional synthetic data generation (what vendors still call TDM) is fundamentally **batch-oriented** :


- Extract data at a point in time
- Transform it (mask, subset, anonymize)
- Load it into a test environment
- Repeat on a schedule


This creates several problems:


- Data is stale the moment it’s created
- Rare edge cases are often filtered out
- Cross-service interactions are either flattened or ignored


As we established in Part 1, **traffic shape and sequence matter more than individual records** in modern systems. Batch snapshots are easy to manage but they don’t represent how systems actually behave under real traffic. When AI coding agents generate code that depends on real-world data patterns, sequences, and edge cases, batch-oriented synthetic data generation leaves them testing against a false reality. This compounds the stochastic uncertainty we discussed in Part 1, where synthetic data introduces artificial patterns that don’t match real-world distributions.


---


# 4. Why stream processing changes the rules


Modern systems don’t just store data — they *process flows of events* .


Examples include:


- API request/response streams
- gRPC and Protobuf message flows
- Event buses and message queues
- AI agents reacting to sequences of inputs over time


In these systems:


- Ordering matters
- Timing matters
- Context matters
- Payload shape evolves continuously


Stream processing captures **behavior** , not just state. Batch processing captures state without behavior — which is precisely what makes it insufficient for modern testing.


---


# 5. Why AI systems expose these weaknesses faster


As we saw in Part 1, AI coding agents are inherently **stochastic** — they produce non-deterministic outputs even with identical inputs. They don’t interact with systems in neat, repeatable ways:


- Explore edge cases aggressively
- Chain multiple calls together
- Depend on subtle data correlations
- Fail in ways that don’t show up in static tests


Batch-based synthetic data strips away the very signals AI coding agents rely on — the rare edge cases, real user behavior patterns, long-tail distributions, and sequential decision-making context we identified in Part 1. This leads to:


- Tests that pass while production fails
- Hard-to-reproduce incidents
- Slower iteration due to fear-driven releases


As we established in Part 1, when you combine non-deterministic AI behavior with synthetic or sanitized test data, you’re compounding uncertainty in ways that make failures more frequent and harder to predict. AI doesn’t break testing — it simply reveals how brittle it already was, and how **we’ve been testing with inadequate data all along** .


---


# 7. What modern testing actually needs


**How do we safely observe and reuse real production behavior to improve software quality?**


To test modern systems effectively — especially when AI coding agents are involved — teams need:


- Realistic request and response streams (not static snapshots)
- Accurate payloads and sequences (preserving the traffic shape and sequence that matters more than individual records)
- Coverage of rare and emergent behaviors (the edge cases AI agents explore aggressively)
- Safe handling of PII and sensitive data (including the hidden PII in JWTs, Base64 fields, and binary formats)
- Continuous alignment with production reality (not stale batch snapshots)


Static datasets can’t deliver this no matter how well masked. AI coding agents amplify this problem because they depend on realistic data distributions and sequences that batch-oriented synthetic data generation systematically removes. The stochastic nature of AI agents means they need the grounding that real production data provides — the same grounding that PII concerns have made inaccessible.


---


# 8. Coming Next: How DLP Unlocks Safe Traffic Replay


In Part 3, we’ll explore how **Data Loss Prevention applied to live traffic** enables a fundamentally different approach: capturing real production behavior, making it safe, and replaying it to test systems the way they actually operate.


This is where traditional synthetic data generation (what legacy vendors still call TDM) ends — and where traffic replay begins.
