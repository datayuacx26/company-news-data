---
schema_version: "1.0.0"
document_id: "dd646b459cca813f1246ec902b582526deceb0063fd092d864c249b78f85e57c"
company_key: "yc-pgdog"
company: "PgDog"
source_id: "yc-pgdog-rss-662afb283f74"
canonical_url: "https://pgdog.dev/blog/our-funding-announcement"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:49.478310+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:4c50c04f9fe3366e44e0d86243dd59be9faf745cec1149de18c58baa4375251d"
---

# Our funding announcement

Postgres is the only database you need.


The reason DBs like Mongo or Dynamo exist is because Postgres has a scaling problem. If you could make it just work, with 100 TB+ tables and 1M queries per second, we don’t think you would use anything else.


This is why we are building PgDog. Same old Postgres, just with a proxy in front of it, to make it horizontally scalable.


You can deploy PgDog anywhere, including on-prem and in your cloud account: pull our Docker image, change your` DATABASE_URL` , and make us do the work.


## Our status


PgDog is serving more than 2M queries per second, in production, across dozens of deployments. We sharded over 20 TB that we know about.


PgDog is open source and anyone can just deploy it, and they do: we have over 1.4M Docker pulls on GitHub.


A new version comes out every week, on Thursdays. Our[Discord](https://discord.gg/CcBZkjSJdd) community is growing. We are there, every day, to answer questions and provide support.


## Why us


PgDog is a small, three-person startup. So, why use our stuff and trust us with your data?


We are infrastructure engineers, application engineers and generalists. We built apps on Postgres before it was cool and made it work at massive scale.


I ran Postgres at Instacart, where we scaled the company 5x in April of 2020. The biggest problem we had was making Postgres serve 100,000s of grocery delivery orders per minute.


We sharded Postgres on RDS, Aurora and EC2. We fixed the actual problem, using first principles (and a lot of code).


The same technology is now available as an open source product.


Building PgDog is not a pivot. For us, scaling Postgres has been, and is, the only goal.


We built PgDog to run in your cloud, in your colo rack, on-prem, or on your laptop. Wherever you need it, PgDog works, with no dependencies or hidden serverless costs. If you can provide CPUs, our multithreaded code will use them all.


Postgres adoption is only going to increase. With $5.5M from Basis Set, YC, Pioneer Fund and other great investors, we have years of runway, and we are going to make Postgres just work, for everyone, at any scale.


– Lev


P.S. We are building an[Enterprise edition](https://pgdog.dev/enterprise) of PgDog to make it easier to run in AWS. It comes with SLA-backed support from our team. Give us a[call](https://calendly.com/lev-pgdog/30min) if you want to try it out.


## More info


- [Read our docs](https://docs.pgdog.dev/) to get started with PgDog
- [Star our repo](https://github.com/pgdogdev/pgdog) and follow it for weekly releases
- [Join our Discord](https://discord.gg/CcBZkjSJdd) to get to know us better
