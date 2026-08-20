---
schema_version: "1.0.0"
document_id: "67c9e2260fa60fa0a3e6182badf5e7d06193128de50f253dae4616e978cd6477"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/what-is-a-context-warehouse"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-21T19:35:42.143614+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:cfe2bfd79d00468e322c89053e50b14ffacf5d8b3ec2fd0b7f00a93fa4066f9d"
---

# What is a context warehouse?

# What is a context warehouse?


- [Lizzie Epton](https://posthog.com/community/profiles/43387)


Jul 21, 2026


- [Data warehouse](https://posthog.com/blog/data-warehouse)


,
- [Infrastructure](https://posthog.com/blog/infrastructure)


,
- [PostHog news](https://posthog.com/blog/posthog-news)


#### Contents


-
-
-
-
-
-
-
-
-
-


Data warehouses are optimized for analytics, business intelligence (BI), and data engineers. What would they look like if they optimized for AI agents instead?


They'd look like context warehouses: a single tool that stores your business and product data, ingests and models it, and makes it directly available to agents that want to reference and use it as context.


This isn't just an agent bolted onto a data warehouse, or a smarter semantic layer sitting on top of one. Plenty of data warehouses have that. A context warehouse is different because the ingestion pipeline and the modeling that make raw data mean something are part of the same tool as the storage and the querying, not separate pieces you assemble and keep in sync yourself. Nothing has to be piped from one system to another to be useful.


Here's how we're thinking about the differences between a data and context warehouse, and how we're building one at PostHog.


##


Why "data warehouse" stopped being the right word


A data warehouse's job is simple: store structured data and let you query it. It doesn't know what a "customer" is, what a feature flag is, or what an experiment variant is; someone has to teach it, by building the modeling and pipelines to wire that meaning in. Traditionally that's a data engineer's job.


As agents increasingly become the consumer of data, they need both the access and the tools to make sense of this data themselves. It becomes their job to know the meaning of the data so they can decide what to fix next. An agent working the[self-driving product loop](https://posthog.com/blog/self-driving-product)


needs product signals (errors, funnel drops, slow sessions) and business context (revenue, plan tier, support history) in the same place so it can query directly without waiting for your daily data sync.


That's what a context warehouse is: a data warehouse plus ingestion pipeline and data modeling, with direct access to your agents in one place. It’s what you get without having to build and maintain it.


##


Data warehouse vs. context warehouse


The difference isn't the storage engine, it's what's allowed to touch the data without a pipeline standing in between.


Data Warehouse Context Warehouse


**What data is included?** Whatever you pipe in, on tables you define, including via your own batch export jobs Product events (already there, no export job to build) + data you pipe in


**What can query it without extra glue?** Your data warehouse's query engine and AI agent Every tool built on top of it, natively. For PostHog that's Analytics, Experiments, Feature Flags, the SQL editor, PostHog AI, and MCP-connected agents


**Who owns it?** A dedicated data team Product engineers, with or without a data team


**It's for you if you need…** Scale and governance for structured data The full context layer your product and your agents run on


##


Who a context warehouse is for (and who it isn't, yet)


**It's a good fit if:**


- You're AI-pilled and ship an increasing amount of work through agents and loops, or you want to make your product self-driving.
- You haven't built a data stack yet, and want product data and business context queryable by an agent from day one instead of assembling a data engineer's worth of tooling first.
- You're an engineer-led team tired of maintaining ETL pipelines whose only job is keeping two systems in sync so a human (or now, an agent) can ask a question that spans both.


**It's not the right fit, yet, if:**


- You run a data org with mature dbt pipelines and advanced modeling needs we don't match yet.
- You want a warehouse and nothing else. The integrated platform is the point, if that's not what you're looking for, a standalone warehouse might be a better fit.


##


How our context warehouse works at PostHog


###


Get all your data in


Your context warehouse starts with data that's already there. Once enabled, PostHog mirrors your product event data to S3, partitioned by organization, as it's captured with no pipeline to build.


Everything else comes in through[Warehouse Sources](https://posthog.com/data-stack/sources)


: 200+ integrations that sync external systems including Stripe, HubSpot, Salesforce, Postgres, and more. Connecting each source is no-code and takes under two minutes, or even more hands-off with the PostHog Wizard.


Compare that to an ETL pipeline: writing and maintaining extraction jobs, hosting them somewhere, wiring up a destination, and debugging it every time a schema changes upstream. Warehouse Sources allows you to skip all of that.


###


Store your data


Under the hood, we store all your data in our[rebuilt Managed Warehouse](https://posthog.com/blog/why-we-rebuilt-our-data-warehouse)


. It's a DuckDB instance per organization sitting on an S3 data lake that you have full access and ownership over your data through.


The main ingredients:


- **Storage:** S3, partitioned per organization. Once enabled, product events land here automatically; Warehouse Sources syncs external systems into the same bucket.
- **Compute & isolation:** A DuckDB instance per organization, running in its own Firecracker MicroVM (the same technology AWS uses for Lambda), so no query competes with anyone else's.
- **Lifecycle:** A lifecycle service sleeps instances when idle and wakes them when a query arrives, in roughly 300ms. You're billed when a worker is running, and they automatically shut down when idle after 60s.
- **Access:** A Postgres wire protocol endpoint. Connect with psql, point a BI tool at it, or wire it into an agent over MCP – no proprietary connector to build against.
- **Local iteration:** DuckHog, a DuckDB extension that lets local compute pull subsets of warehouse data into pandas, polars, or DuckDB itself, iterate quickly, and write results back. For an agent that wants fast local loops instead of round-tripping every query to the cluster, this is the better pattern.


###


Agents read your data and drive development


None of this matters to an agent unless the agent can actually query it. PostHog exposes the context warehouse over[MCP](https://posthog.com/mcp)


,[PostHog Web](https://posthog.com/self-driving)


,[Slack](https://posthog.com/slack)


, or[PostHog Code](https://posthog.com/code)


. An agent working the[self-driving product](https://posthog.com/blog/self-driving-product)


loop reads product events and business context through the identical interface a human would use in the SQL editor.


That's what makes this a context warehouse rather than a data warehouse with an AI feature bolted on: the context an agent needs to make a good decision is already there, and it can use it to drive development autonomously or answer your questions.


##


What's in the pipeline


The context warehouse today is deliberately not a full data stack: land your events and your business data in one place, let the tools you already use in PostHog query it directly to make better decisions about the direction of your product. Here's what we're already building toward:


Direction What it means


Making our Managed Warehouse generally available Right now, we're in a closed beta that will be open very soon, you can[get on the waitlist](https://posthog.com/data-stack/managed-warehouse)


and be the first to hear about it.


Deeper modeling Closing the gap with dbt-style transformation maturity for teams with more advanced modeling needs.


More sources Expanding[Warehouse Sources](https://posthog.com/data-stack/sources)


coverage so fewer teams need a custom sync to get their systems in.


More of the loop exposed to agents As the[self-driving product](https://posthog.com/blog/self-driving-product)


loop matures, more of what an agent can see in the context warehouse becomes something it can act on directly, not just read.


##


Try it yourself


[Join the waitlist](https://posthog.com/data-stack/managed-warehouse)


for the managed warehouse beta, or just[connect your first source](https://posthog.com/docs/data-warehouse/start-here)


and get a head start, no need to wait around for us. Once you're in, the warehouse lives inside PostHog, same as everything else: sync a source through Warehouse Sources, poke at it in the SQL editor, or hand an agent the keys and see what it does with it.


##


FAQ


**Is a context warehouse the same as a data lakehouse?**


No. A lakehouse combines warehouse and data-lake storage patterns. A context warehouse adds the ability for your product tools and agents to query that storage natively, without a separate BI or reverse-ETL layer.


**Can I use a context warehouse if I already have a data warehouse?**


Yes. Sync your data into PostHog via Warehouse Sources, and that data becomes queryable alongside your product events in analytics, experiments, and agents. No migration required.


**Does a context warehouse replace Fivetran, dbt, and Looker?**


For most product teams, yes. Ingestion (Warehouse Sources), modeling, and querying are built in. Teams with advanced modeling needs may still want dedicated tooling for now.


**What's the difference between a context warehouse and a CDP?**


A CDP moves customer data between tools. The context warehouse is the storage and query layer those tools run on top of, which can include a CDP.


**Is my data still mine, can I export it?**


Yes. It's stored in S3 in your organization's partition, accessible over a standard Postgres wire protocol connection, not locked behind a proprietary format.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
