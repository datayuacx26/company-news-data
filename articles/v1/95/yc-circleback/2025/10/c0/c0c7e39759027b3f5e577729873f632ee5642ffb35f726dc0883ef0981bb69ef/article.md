---
schema_version: "1.0.0"
document_id: "c0c7e39759027b3f5e577729873f632ee5642ffb35f726dc0883ef0981bb69ef"
company_key: "yc-circleback"
company: "Circleback"
source_id: "yc-circleback-news-import-b22aa3eada49"
canonical_url: "https://circleback.ai/blog/how-we-fixed-postgres-connection-pooling-on-serverless-with-pgdog"
published_at: "2025-10-09T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:40.934837+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:48bf7e64faa0a2b1a24860003c985a255742e4e83ac4ea02cf0c766326797ab0"
---

# How we fixed Postgres connection pooling on serverless with PgDog

We were afraid to deploy during business hours. Not because of bad code, but because our connection pooler couldn't handle the spike. Every deploy to Vercel spun up a flood of new serverless functions, each one grabbing a database connection. The pooler would buckle, users would see errors, and we'd wait a few minutes for things to settle.


For a startup that needs to ship fast, "don't deploy during peak hours" is not a viable strategy.


We never ran out of database CPU or memory. We ran out of connections. That matters, because the fix wasn't a bigger database. It was a better pooler.


## The journey to get there


We started with Supabase's default pooler, Supavisor. It worked until it didn't. Connections between the pooler and database would stop being recycled, the database would hit its connection limit, and the app would go down. As traffic grew this happened more often. We worked with Supabase support but couldn't find the root cause, so they deployed PgBouncer on our Postgres hosts instead.


PgBouncer was better. The stuck connection problem went away. But under bursty serverless traffic, a different failure showed up. During deploys we'd see client connections spike and then stay high for minutes, well after the deploy finished. The pooler-to-database connections were nowhere near their limit. Postgres could handle plenty more. The bottleneck was PgBouncer itself.


The core issue: PgBouncer is single-threaded. When hundreds of serverless functions spin up at once, a single thread can't assign connections fast enough. This is a known limitation, but most guidance glosses over it. If your traffic is steady, PgBouncer works great. If your traffic comes in spikes, which is the nature of serverless, it struggles.


We tried tuning everything on the Postgres side: session timeouts, query optimization, better indexing, memory settings. These improved database health but didn't touch the pooler problem. We added read replicas to spread the load. That helped at the margins but was another bandaid. The root cause was the pooler's inability to handle connection spikes.


Since PgBouncer was managed by Supabase, every config change required a multi-day email loop. Vercel's gradual deployments feature smoothed the spikes somewhat, but it was treating the symptom rather than fixing the problem.


## Finding PgDog


I'd been looking at PgCat, a multi-threaded pooler built at Instacart that seemed well-suited to bursty traffic. Before we could properly test it, the project was sunset. Its main contributor, Lev, had started a fork called PgDog.


By chance, Ali, our founder, met Lev at a YC event. The next day I got on a call with him and walked through our issues. Two things stuck with me. First, you shouldn't have to scale up your database just to raise connection limits. A pooler should solve that. Second, keep the pooler close to your database and runtime. A 10-50ms difference in round-trip latency is fine for most apps, but putting a pooler on a different cloud than your database is not.


I set PgDog up on AWS using EKS and connected it to our dev database. I ran pgbench with a range of queries, then simulated real traffic by hitting a staging deployment with requests of varying complexity while redeploying mid-test. PgDog handled everything. The connection spikes showed up in the metrics, but the pooler never struggled.


When I moved to testing against our real app, I hit a snag: Prisma's use of prepared statements conflicted with how PgDog caches them. We were one of the first PgDog users running Prisma, so this wasn't surprising. The PgDog team found and fixed it fast, pushing new releases to add logging and get to the root cause within days. Later we found a memory leak. Same story, quick fix. Coming from months of slow debugging cycles, the speed was refreshing.


Since then, we've had zero pooler issues.


## What else PgDog gave us


Beyond fixing the connection spike problem, PgDog improved our setup in ways we didn't expect.


Before PgDog, we used Prisma's replica feature to spread reads across database instances. This is purely client-side with no awareness of database health. It picks a host at random. If Supabase was resizing or restarting an instance, queries sent to the inactive host would fail and the app would go down.


With PgDog in between, we get health-aware load balancing. PgDog checks each Postgres instance and routes queries away from unhealthy hosts. During Supabase resizes we see zero read downtime. Only writes pause while the primary is down, which is hard to avoid without a more complex setup. Compared to the whole app going down, this is a big step forward.


PgDog also sends out metrics in OpenMetrics format, which we scrape into Prometheus and show in Grafana. We now have real-time visibility into client connections, waiting clients, query latency, and transaction latency. Before this, we were mostly flying blind during incidents.


## The outcome


Because of PgDog, we removed a replica, scaled our Supabase hosts from 12xl down to 4xl, and kept pooler-to-database connections well under limits even during deploy spikes. We stopped sizing the database for connection problems and started sizing it for actual resource use.


Running PgDog on EKS costs a fraction of what we were paying for an overprovisioned database. The total savings are real, but the bigger win is operational. We deploy to prod during peak hours without thinking twice.


We're building the central source of truth for getting work done, powered by context from every conversation a team has. If that sounds interesting, we're[hiring](https://circleback.ai/jobs) .
