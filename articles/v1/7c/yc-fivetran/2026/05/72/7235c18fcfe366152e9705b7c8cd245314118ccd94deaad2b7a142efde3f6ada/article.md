---
schema_version: "1.0.0"
document_id: "7235c18fcfe366152e9705b7c8cd245314118ccd94deaad2b7a142efde3f6ada"
company_key: "yc-fivetran"
company: "Fivetran"
source_id: "yc-fivetran-news-import-dafa91bc199f"
canonical_url: "https://www.fivetran.com/blog/modern-data-stack-vs-open-data-infrastructure"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-23T09:42:24.979492+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:237268c6235088f26c3e45e3c29a4cdfcaaa71ac32c80e2e92b71ee29924944a"
---

# Modern data stack vs. Open Data Infrastructure

The modern data stack solved a real problem 10 years ago: get data into one place, model it, let analysts query it. That was the job, and warehouses did it well.


The job has changed. Most teams I talk to haven't fully grappled with how much. If you're putting AI into production on top of a warehouse-centric stack, you're paying for it — in compute bills that don't make sense, in latency you can't explain to the product team, and in the slow drift of data copies that nobody quite owns. The architecture isn't broken; it's just doing a job it wasn't designed for.


Here's the case for moving to[Open Data Infrastructure](https://www.fivetran.com/blog/what-is-open-data-infrastructure) , written around the questions skeptics actually ask and not the easy ones.


\[CTA_MODULE\]


## "Snowflake and Databricks are building AI features directly into their platforms. Why would I leave?"


You don’t have to, and that’s the point. These platforms are still best-in-class for compute, analytics, and AI. Cortex, Mosaic, AI Functions — they’re real, and for many workloads, they’re exactly what you want. The shift isn’t about replacing them. It’s about changing where your data lives so you can use them more effectively.


The limitation isn’t the tools; it’s the architecture. When data is tightly coupled to a single platform, every new AI use case that falls outside of it creates friction. The moment you need to use another model, another tool, or another system, you’re back to copying data, building pipelines, and managing inconsistencies across environments. That’s how costs rise, latency creeps in, and context breaks.


Open Data Infrastructure solves this by separating storage from compute. Instead of ingesting data into each platform, you store it once in an open, governed lake and let Snowflake, Databricks, and other engines read from the same source of truth. This eliminates duplicated pipelines,[reduces ingestion and compute costs](https://www.fivetran.com/blog/why-an-open-data-infrastructure-means-reliability-and-low-cost) , and keeps data consistent across teams and tools.


You can see this clearly in real organizations. It’s common for different teams to use different compute tools — finance in Snowflake, data science in Databricks, maybe a new AI team experimenting with something else entirely. Or after an acquisition, you inherit an entirely different stack. Without a shared foundation, you end up running parallel pipelines into each system, paying for ingestion multiple times, and trying to reconcile slightly different versions of the same data. With a centralized data lake, that complexity disappears. You maintain one source of truth, govern it once, and every tool operates on the same, up-to-date context.


So the question isn’t whether to leave; it’s whether you want your data tied to one system or accessible to all of them. Keeping these platforms as compute layers while moving to an open data foundation gives you flexibility now and optionality later, without forcing a full rebuild when your needs inevitably change.


## "We tried a data lake in 2018. It was a swamp. Why is this different?"


Fair, and a lot of teams have this baggage. The 2018 data lake failed for specific reasons that have since been addressed.


The old lakes were file dumps with no transactional guarantees. Schema drift was constant, governance was an afterthought, and the query engines that read those files were slow and brittle compared to a warehouse. So teams abandoned them and consolidated back into Snowflake or Databricks, and that was the right call at the time.


What's actually different now: open table formats (Iceberg, Delta) provide ACID transactions, schema evolution, and time travel on top of object storage. Catalog standards have matured. Query engines that read these formats are competitive, sometimes faster, than warehouse-native performance for many workloads.


But those capabilities don’t deliver value on their own.[You still need to get data into those formats](https://www.fivetran.com/blog/the-business-case-for-upgrading-to-a-data-lake) , keep it continuously up to date, manage schema changes, and maintain metadata across systems — which is where most teams hit complexity.


[Fivetran Managed Data Lake Service](https://www.fivetran.com/blog/governing-your-lakehouse-with-fivetran-managed-data-lake-service) handles that for you. It delivers data into open formats, keeps your lake continuously synced from source systems, manages schema evolution automatically, and publishes metadata to your catalog so every engine can access a consistent, governed view of the data. Instead of stitching these pieces together yourself, it gives you the benefits of an open data lake without the operational overhead that traditionally came with it.


## "Databricks created Delta Lake and now owns Tabular, which developed Iceberg. How is this 'open'?"


Openness is on a spectrum, and it matters which axis you mean. Format openness — can multiple engines read and write the data — is real. Iceberg has working implementations from Snowflake, AWS, Google, Trino, DuckDB, and others. Delta has broad support too. Whichever vendor sponsors the spec, your data lives in object storage you control, in a format multiple engines can read. That's a meaningfully different position than proprietary warehouse storage.


[Governance openness](https://www.fivetran.com/governance) , who controls the spec, is messier. Iceberg is in the Apache Foundation but Databricks now owns Tabular. Delta is Databricks-led with Linux Foundation governance. Neither is fully neutral. What matters operationally is if your storage and catalog are open enough that you could swap query engines without re-ingesting petabytes of data, you have meaningful leverage. That's the bar. ODI clears it. Proprietary warehouse storage doesn't.


## "Won't I just trade one vendor for 5? My platform team can't manage that."


This is where a lot of "modular stack" pitches fall apart in practice. ODI gives you flexibility, and flexibility has operational cost. If you assemble it yourself from raw parts — a catalog here, a query engine there, orchestration glued in — you'll need a platform team that can hold it together. For a lot of organizations, that's not realistic.


Two things make this manageable. First, the layers that matter most, storage and catalog, are increasingly standardized, so you're not gluing together five proprietary systems, you're configuring layers that actually interoperate. Second, managed offerings now exist for most pieces, so you can adopt the architecture without running every component yourself.


The real question isn't "modular vs. consolidated." It's "where do I want my optionality?"[If you consolidate inside one vendor](https://www.fivetran.com/blog/saps-latest-api-policy-raises-the-stakes-for-your-ai-strategy) , you trade operational simplicity today for switching cost tomorrow. If you adopt ODI, you trade some operational complexity today for the ability to route workloads as they evolve. Both are valid. The wrong move is pretending there's no tradeoff.


## "My warehouse bill is fine. Show me where the AI workload actually breaks the math."


It doesn't break for the early experiments. It breaks at the point most teams haven't reached yet.


A single agent task, say generating a recommendation, looks like one output. Underneath, it's a user-behavior retrieval, a product lookup, an inventory check, a model inference, and often a re-ranking pass. That's not one query, it's a dozen, sometimes more. Run it for one user, no problem. Run it across thousands of concurrent agents, and the per-task cost on warehouse-priced compute starts to look ridiculous compared to what the same operations would cost on a routing-aware setup.


There's also another problem: latency. Warehouses are tuned for throughput on big queries, not low-latency lookups on small ones. When agents are doing chained retrievals, every hundred milliseconds compounds. Some workloads will run on the warehouse and you won't notice. Others will quietly become unusable, and the answer the team reaches for is "spin up a different system," which is how you end up with the patchwork problem.


If you want to see this for yourself, instrument one real agent workflow end-to-end and look at the cost per task and the p95 latency.[The numbers tend to be more persuasive than the argument.](https://www.fivetran.com/blog/why-an-open-data-infrastructure-means-reliability-and-low-cost)


We saw this internally. As agent-driven usage increased, query volume and costs rose much faster than expected. The initial response was to add guardrails, but that only made the underlying issue clearer: if every new AI workflow depends on the same high-cost path, scaling AI becomes a cost problem, not a capability one. That’s what pushed us to shift toward a data lake architecture, where high-volume workloads could run on lower-cost engines instead of defaulting to the warehouse.


## "Isn't this just a vendor pitch dressed as architecture advice?"


Every architecture argument has someone who benefits from it. The vendors building ODI components — open table formats, query engines, catalogs, and data movement platforms — benefit as this model grows. But importantly, warehouse vendors are part of this ecosystem as well. In an ODI approach, they continue to play a critical role as powerful compute layers — just no longer as the only place your data lives.


What's harder to dismiss is that the underlying shifts: open table formats becoming the storage standard, compute decoupling from storage, agents becoming a primary consumer of data are happening regardless of which vendor wins. Snowflake supports Iceberg. Databricks bought Tabular. AWS, Google, and the major catalogs are all converging on this layer. The question isn't whether the architecture is moving in this direction. It is. The question is how aggressively you reposition for it, and how much of your current stack you're willing to rebuild on the way.


A reasonable read: if the vendors who'd lose from this shift are themselves moving toward it, the shift is probably real.


## Modern data stack vs. Open Data Infrastructure


Modern data stack (MDS) Open data infrastructure (ODI)


Warehouse-centric Lake-centric (open storage)


Proprietary data formats Open formats (Iceberg, Delta)


Compute tightly coupled to storage Compute decoupled from storage


Single query engine Multiple engines (Snowflake, Databricks, Trino, DuckDB, etc.)


Optimized for batch queries and dashboards Built for continuous, real-time access and automation


Human-driven workloads Human + agent-driven workloads


Cost scales with queries Cost optimized via flexible compute routing


Limited interoperability across tools Interoperable across tools and systems


Data movement is costly and complex Data is portable and reusable


Vendor lock-in risk No vendor lock-in


Hard to scale AI workloads Designed for agent-scale workloads


## Where to start


You don't need to rip out the warehouse. You need to stop treating it as the center of the universe. Move the storage layer to open formats first: Iceberg or Delta on object storage you actually control. Everything else assumes this foundation. Most warehouses can read these formats now, so this isn't a migration off your existing stack;[it's repositioning the data so other engines can reach it too.](https://www.fivetran.com/blog/85-of-enterprises-are-running-agentic-ai-on-a-data-foundation-that-isnt-ready)


Pull one workload off the warehouse next. Pick something the warehouse serves badly: a high-frequency retrieval, a feature pipeline for an ML use case, an agent workflow that's been generating surprising bills. Route it through a different engine against the same storage. Measure the difference. That's your proof point internally.


Invest in the context layer last but not least. Metadata and semantics feel like back-office work until you watch an agent take action on a stale definition. They aren't optional for AI; they're the difference between agents that work and agents that don't.


\[CTA_MODULE\]
