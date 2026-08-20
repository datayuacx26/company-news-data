---
schema_version: "1.0.0"
document_id: "bd3b56ec45e6d3d8f54b808e6e9f45b45e7874474abf393075ff86a0be7f54fb"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/the-telecom-engineers-who-went-small-the-origin-story-of-tinybird"
published_at: "2026-03-23T14:32:49+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:5179ae3dfaf98e8d75dbaa7674927939ba25cfb78e8ab82f3327df98ff6a939c"
---

# The Telecom Engineers Who Went Small: The Origin Story of Tinybird

## The Ticket That Should Have Taken an Afternoon


Madrid, 2018. A backend engineer is staring at a ticket that should take an afternoon. Product wants a new analytics endpoint: show users their usage stats, by day, by feature, filtered by plan type, in real time. Nothing exotic. Every SaaS product has this. The engineer opens a blank file and starts thinking through what "real time" actually requires.


First, the database. The existing PostgreSQL instance is not built for this — analytical aggregations over millions of rows will kill query times. So you need a separate OLAP store. ClickHouse, maybe. Except now you have to provision it, configure it, manage replication, think about schema design for columnar storage, learn the specific syntax.


Then the ingestion layer. Data is arriving from the app constantly — events, sessions, updates. You need a pipeline that captures it, batches it, writes it to the new OLAP store reliably. Kafka, probably. Which means Kafka infrastructure, consumer groups, topic management, failure handling.


Then the API. Once data is in ClickHouse and queries are working, you still need to expose that as an HTTP endpoint. Build a small service, define the route, handle query parameters, add caching, add rate limiting, add authentication, deploy it, monitor it.


The afternoon ticket has turned into a three-week project. Maybe four. The engineer picks up a cup of coffee and wonders why, in 2018, this is still the way.


Somewhere in Madrid, four engineers are wondering the same thing.


---


## Four Engineers, One City, One Very Obvious Problem


The people who founded Tinybird are not, by biography, obvious startup founders. They are not Stanford dropouts or ex-Google PMs hunting for their unicorn moment. They are engineers who got tired of the same problem showing up everywhere they worked.


Javier Santana, Jorge Sancha, Sergio Álvarez Leiva, and Javier Álvarez had each spent years in the trenches of data-heavy applications. The problem they kept running into had a specific shape: teams had good data, they had capable engineers, and still it took weeks — sometimes months — to build a feature that let users *see that data* in their product in real time. The bottleneck was never the data itself. It was the infrastructure stack required to serve it fast enough to be useful.


Every team they knew had solved this problem the same way: custom. Build your own ingestion pipeline. Build your own transformation layer. Build your own API. Deploy your own ClickHouse cluster. Write your own schema migration scripts. Repeat this for every new analytics feature, at every company, forever.


No one had abstracted it. No one had said: this is a solved problem. Let's make it a primitive.


Madrid's startup scene in the late 2010s was not San Francisco. There was no culture of "build a feature, raise a round, build another feature." The city had engineering talent — good engineering talent — but the default career path ran through large European corporations or the Madrid offices of US tech companies. Founding something was not the assumed path. It was the unusual one.


That context matters. Because when you start a company in an ecosystem that doesn't assume you'll succeed, you tend to build more carefully. You solve real problems rather than theoretical ones. You find customers early because you have to. You do not have the luxury of building for five years in stealth on a16z's money.


Tinybird was founded in 2019. The product they built was not a vision — it was a prescription. They had each personally experienced the pain. They knew exactly who the customer was because they had been the customer.


---


## ClickHouse, Developer Experience, and a Category Nobody Had Named


The first decision was the most important: what to build on top of.


The founders chose ClickHouse, and the choice was not obvious at the time. ClickHouse had been open-sourced by Yandex in 2016, and by 2019 it was not yet mainstream. Most Western data engineers had not heard of it. It had a reputation as brutally fast but technically demanding — a database built for people who had already read the entire documentation twice, knew its column-oriented internals cold, and were comfortable tuning MergeTree configurations at the command line.


This was, in a sense, a feature, not a bug. ClickHouse is extraordinary at what it does: OLAP queries over billions of rows in milliseconds. It can scan trillions of rows per second on modest hardware. Its columnar storage format and vectorized execution engine produce query speeds that feel almost offensive to engineers who have spent their careers nursing PostgreSQL through analytical queries. Nothing else at the time came close for the specific problem of "query very large amounts of data, very fast, returning results in time for a user to see them."


If you wanted to build a real-time analytics API platform, ClickHouse was the only serious answer. The founders knew this. What they also knew is that ClickHouse required a kind of expertise that most product teams — good teams, capable teams — simply did not have. The very thing that made ClickHouse powerful made it inaccessible.


So the bet was: abstract the complexity. Build a managed layer on top of ClickHouse that handles everything product engineers don't want to deal with. Schema management. Ingestion pipelines. Query optimization. Replication. Scaling. And then — crucially — expose the transformed data as an HTTP API automatically, with no backend code required.


This is what became Tinybird's core architecture. You define a Data Source — the shape of your incoming data. You build Pipes — chains of SQL nodes, each one transforming the data from the previous step. You publish the final node of a Pipe as an API Endpoint. Tinybird handles everything underneath: the ClickHouse cluster, the ingestion, the caching, the API serving, the authentication, the scaling. You write SQL. Tinybird turns it into production infrastructure.


The "Pipes" metaphor is not accidental. It is one of those product naming decisions that captures an architecture more precisely than any technical diagram. Data flows in. SQL nodes reshape it — filter, aggregate, join, enrich. The output flows out as an endpoint. You can inspect every node individually. You can chain them. You can branch them. You can test them with live data before you publish. The whole thing feels like UNIX pipes applied to modern data infrastructure: small, composable primitives that combine into powerful workflows.


The internal architecture went deeper than just a developer experience wrapper. Tinybird built its own ClickHouse fork — not to compete with ClickHouse, but to optimize for their specific use case. They implemented compute-storage separation before ClickHouse's own cloud product made it mainstream: data lives in S3 or GCS (object storage), while local SSDs on each cluster replica serve as a high-speed cache layer. Each Tinybird workspace is effectively its own ClickHouse database on a multi-replica cluster, isolated but sharing infrastructure. They built custom packed-part formats and zero-copy replication on top of the fork. These are not cosmetic changes. They are the engine-room decisions that let Tinybird promise sub-second query performance at scale while keeping operational costs sane.


This is also why the "Tinybird is to ClickHouse what Supabase is to Postgres" comparison — while directionally right — understates what Tinybird actually built. Supabase wraps Postgres with a clean API and an auth layer. Tinybird rewrote significant internals to make their specific use case work at production scale. The abstraction runs much deeper.


The fundraising story has a European texture to it. Most of the serious infrastructure venture capital sat in San Francisco. The firms that had the conviction to write large checks into data infrastructure companies — the ones who had funded Snowflake, Databricks, Confluent — were on the other side of an ocean. Getting in the door from Madrid required either warm introductions through the US tech diaspora or the kind of traction that makes investors reach out.


The Tinybird team ground through it. They shipped. They found early customers. They demonstrated what the product could do — not in a deck, but in a live demo where a backend engineer went from raw data to a live API endpoint in front of an investor's eyes, in under a minute. The "30 seconds to an API" pitch is not a marketing line. It is something you have to see once to understand. The moment it clicks for a developer — *I can do in 30 seconds what took my last team three weeks* — is the moment Tinybird converts a skeptic.


By 2022, they had raised their Series A: $29.5 million, led by CRV and Singular VC, with participation from Balderton Capital and Crane VC. This was not a trivial round for a Madrid-based infrastructure company in 2022. It was a statement that the market they were building was real, and that building from Europe was a solvable problem, not a disqualifying one.


---


## When "SQL to API in 30 Seconds" Became the Category


There is a specific moment in every successful infrastructure company's story when the product stops being something you explain and starts being something that spreads on its own. For Tinybird, that moment came when developers started describing it to each other the same way, without prompting, without reading a press release.


The description was always some variation of: "You write SQL. It becomes an API. It handles the scale."


This convergence of language is not accidental. It means the product did exactly what it advertised, and it did it simply enough that users could describe it in a single sentence to a colleague. In developer tooling, that sentence-length description is the difference between a product that grows through word-of-mouth and one that requires a sales team to explain.


What Tinybird unlocked for product teams is something that sounds incremental but is actually structural. Before Tinybird, building a real-time analytics feature into a product required two things: a data engineering team to build the pipeline, and a backend engineering team to build the API. These two teams spoke different languages, worked on different timelines, and had different definitions of "done." The data engineer thought they were done when the data was in the warehouse. The backend engineer thought they were done when the endpoint returned the right number. Neither controlled the full stack. Feature requests moved through both queues.


Tinybird collapses this into a single workflow. A data engineer — or a backend engineer with SQL fluency, which is most of them — can own the entire process: data in, pipes to transform it, endpoint to serve it. No handoff. No queue. No "we'll get to the analytics API in Q3." The cycle time from idea to live endpoint drops from weeks to hours.


At Canva, engineers reported achieving in days what would have taken weeks with any other tooling. At Factorial, a team shipped twelve new user-facing dashboards in a matter of months. At Dub — the link analytics platform — the entire analytics infrastructure that shows users their link performance data, updating in real time, runs on Tinybird. The team reports not having to think about scaling, traffic spikes, or ingestion management at all. They just write the queries.


This last detail matters. Dub's product is link analytics. That *is* the product. The real-time data API is not a supporting feature — it is the thing users came for. Tinybird handling that infrastructure is not a convenience; it is the reason the company's data engineering team can stay small while serving customers at enterprise scale.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## Global from Madrid, ClickHouse Tension, and the AI Era


By 2024, Tinybird had raised a $30 million Series B at a $240 million valuation — a figure that would have seemed impossible from a Madrid office in 2019. They had opened a presence in New York. Their customers ranged from two-person startups to Canva, one of the world's most valuable design platforms.


The competitive landscape had grown more complex. ClickHouse, Inc. — the company spun out of the open-source project — had launched ClickHouse Cloud, its own managed offering. On the surface, this looks like Tinybird's worst nightmare: the company providing your engine becoming your direct competitor.


In practice, the competition runs on different planes. ClickHouse Cloud is a managed database. You still have to build everything on top of it: the ingestion pipeline, the transformation layer, the API. Tinybird is the full stack above the database — ingestion, pipes, API, developer experience, CLI workflow, branch environments, schema migration tooling. The two products are not substitutes. ClickHouse Cloud is a better raw database. Tinybird is a platform for teams that want to skip straight to the outcome.


The CLI workflow is worth describing. Tinybird ships a command-line tool that makes the development experience feel like working with code. You define data sources and pipes as files. You run` tb push` to deploy changes. You use` tb branch` to create isolated environments with production data. You can run` tb test` against your pipes before they hit production. The mental model is familiar to any engineer who has used modern deployment tooling — it feels like deploying a web application, not managing a database. This is intentional. Tinybird wants engineers to think of data APIs as software, not infrastructure.


The Madrid question never fully disappears. Building a global developer tool from Spain means serving US customers from a timezone that is six to nine hours behind the US coasts. It means hiring in a market where data infrastructure talent is thinner than in San Francisco or New York. It means the cultural references, the conference circuit, the investor relationships, and the press all default to American companies as the baseline. Tinybird has navigated this by going fully remote while maintaining Madrid as the cultural and engineering heartbeat of the company. The founders are still there. The engineering identity — meticulous, deep, solutions-first — still carries the flavor of a team that built something because they needed it, not because they identified a market.


The AI era has added a new dimension to what Tinybird does. LLMs need data. Agents need real-time signals. Product teams building AI features into their applications need to serve fresh data to models — user histories, behavioral signals, content embeddings — through APIs that respond fast enough to be useful inside a model inference loop. This is exactly the infrastructure problem Tinybird was built to solve, applied to a new category of product. The company has leaned into it: "AI-focused DevEx, built for agents and developers" is now part of their core positioning. The product that started as the answer to "why does building an analytics endpoint take three weeks" has become an answer to "how do I give my AI agent access to live data at scale."


The thread connecting 2019 to 2026 is the same thread. Data moves fast. Users expect to see it in their products in real time. Building the infrastructure to make that happen from scratch is expensive and unnecessary. Tinybird's bet has always been: there should be a primitive for this. You should be able to write SQL, and have the infrastructure follow.


That bet has compounded.


---


## Five Things Most People Don't Know About Tinybird


**1. Madrid is not an accident — it is the whole story.**
Tinybird's identity as a European company is not a limitation they overcame. It is the reason the product is what it is. The founders were not hunting for a market. They were solving a problem they had personally spent years inside. That kind of problem-solving — grounded in real engineering pain, not market analysis — produces different products than the ones built from pitch deck hypotheses. The Madrid engineering culture is meticulous, infrastructure-serious, and allergic to bullshit abstractions. You can feel it in the product. The pipes metaphor is not a marketing invention. It is a working architecture.


**2. Tinybird built a ClickHouse fork — which means they are not simply "Supabase for ClickHouse."**
The comparison to Supabase is the most common shorthand for Tinybird. It captures the right vibe — managed layer, developer experience, takes away the hard parts — but it obscures something important. Supabase wraps Postgres with minimal changes. Tinybird built their own ClickHouse fork with compute-storage separation (data in S3/GCS, SSDs as cache), custom packed-part formats, and zero-copy replication. They did not inherit the engine. They rewrote parts of it. This is a much harder technical bet, and it is the reason they can make promises about performance at scale that a simpler wrapper product could not.


**3. ClickHouse, Inc. launching ClickHouse Cloud is not Tinybird's death sentence — it is clarification.**
When ClickHouse spun out of Yandex and raised hundreds of millions of dollars to build a managed cloud product, the obvious read was: Tinybird's underlying engine just became their biggest competitor. The less obvious read is that ClickHouse Cloud validated the market and drew a product boundary. ClickHouse Cloud is a managed database. Tinybird is everything above the database: ingestion, SQL-to-API transformation, developer workflow, branch environments, CLI tooling, API authentication. The customer who uses ClickHouse Cloud still has to build all of that. The customer who uses Tinybird never thinks about any of it. The two products serve engineers at different moments in their journey.


**4. The "pipes" metaphor encodes an entire philosophy about how data should move through a system.**
Most data platforms think of data as a thing you store and query. Tinybird thinks of data as a thing that flows. This is not a semantic difference — it changes what the product looks like. A "pipe" in Tinybird is a chain of SQL nodes: each one receives data, transforms it, and passes it forward. The final node is published as an API endpoint. You can test each node individually. You can trace a row of data through every transformation step. You can branch a pipe to create a new variant without touching the production version. The metaphor forces composability. It forces testability. It forces you to think about your data transformations the same way a software engineer thinks about a function — small, single-purpose, readable, combinable. That is a radical idea in an industry that has historically thought of SQL as something you write in a BI tool and never version-control.


**5. Tinybird's real opportunity in the AI era is not analytics — it is the data layer for intelligent products.**
When Tinybird explains what it does, the word "analytics" comes up constantly: real-time analytics APIs, user-facing analytics, product analytics dashboards. But the underlying capability is more general than analytics. It is: *take a large stream of structured data, transform it with SQL, and serve the result as a fast HTTP endpoint* . In the AI era, every intelligent product needs this. You want to serve the last 90 days of a user's behavior to a recommendation model? That is a Tinybird pipe. You want to give an AI agent real-time inventory data to make pricing decisions? That is a Tinybird pipe. You want to power a natural-language interface with live database queries behind it? That is a Tinybird pipe. The company was built for analytics. But the product is actually a real-time data API primitive. Those are very different markets — and the second one is significantly larger.
