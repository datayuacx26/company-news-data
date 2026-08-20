---
schema_version: "1.0.0"
document_id: "e3a1ce76fbc3f094357611dd53ce0357005c7b5ce5a59fec5ae728c40390091a"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/announcing-diom/"
published_at: "2026-04-20T12:00:00+00:00"
first_seen_at: "2026-07-22T15:22:16.880049+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:e79bfcf26c594d7eb0950d2696b8d08aac51e5c5467bdc77095f03fc69bb6b6f"
---

# Diom: Components Platform for Robust Services

We are very excited to announce the release of[Diom](https://diom.svix.com/) , a backend components platform for building robust services.


Diom came out of our years of experience running a high-availability, high-concurrency public API and solves many of the challenges we faced along the way. We've been actively developing it over the last six months and are very excited to share it with the world.


The Components Platform is a new approach that makes it easier to build better services. It's a self-contained service you can run in your own infrastructure that implements all the common primitives you need when building backends such as cache, rate-limiting, idempotency, queue, and more.


You can read more about what it is, or you can try it out by:


- Checking out[the docs](https://diom.svix.com/docs)
- Trying out[the playground](https://diom.svix.com/playground)


It's also open-source, with the source code[available on Github](https://github.com/svix/diom) .


## Why we built Diom


The idea for Diom came from our own challenges when building Svix.


Every SaaS company, including Svix, needs to reinvent the same backend primitives, such as idempotency, rate limiting, caching, auth tokens, and more. Some primitives, such as streams and queues, already have existing solutions. In those cases there is a constant tension between running additional dedicated services (like RabbitMQ and Kafka), or building your own worse solutions over existing infra (like Redis and Postgres) in order to reduce infrastructure complexity and sprawl.


Building your own solutions often means dealing with incomplete solutions that lack key functionality, are fragile, hard to test, and put significant strain on the underlying infrastructure, such as the database, which was not meant to deal with these kinds of workloads.


On the other hand, adding additional infrastructure adds significant operational complexity. Every new service you add to your infrastructure requires effort around backups, monitoring, disaster recovery and high-availability. This holds even when using cloud-hosted solutions, though especially when hosting your own infrastructure, or offering your product on-prem in customer environments.


Having to deal with these made us realize that what we needed was a platform that has all the primitives that people commonly need. People have been building web applications for long enough that the common patterns are now well established, which means that a common set of primitives can fit most use-cases.


We also realized that many of the existing solutions sacrifice usability in favor of scale, often requiring specialized personnel to maintain, even though most people don't have multi-TB/s workloads.


## What is Diom


Diom (pronounced: dye-omm/daɪəm) is a backend components platform for building robust, idiomatic services.


It offers high-level APIs for commonly used primitives such as cache, rate-limiting, idempotency, queue, and more, with many more planned. It's self-contained, manages its own storage, and can be run as a single node or a highly-available cluster.


Diom can replace Redis, RabbitMQ, Kafka, and a lot of custom code for most use-cases, meaning that developers can run with only a database and Diom for many workloads. Having just one service, instead of many, means it's easier to maintain, monitor, backup (including testing backups), and configure; as well as reducing deployment costs and complexity.


Additionally, because Diom implements common backend patterns, in most cases people will be able to just re-use a robust, well-tested, and efficient pre-built solution instead of wasting time building their own ad-hoc solutions for these common problems.


Diom's design goals are: reliability, developer experience, ease of operation, and performance; in this order. Focusing on being the best choice of 95% of products and developers, who don't process multiple terabytes and billions of events per second.


While performance is not the main goal, Diom can achieve high performance for the target use-cases without sacrificing durability and availability due to the more constrained use-cases. For example, because the implementations live in the same place as the data store, there are fewer network round-trips, which lead to lower overall latency.


Diom supports deploying in highly-available (HA)[Raft](https://raft.github.io/) -powered clusters, meaning you'll stay up even when a node fails or a service is restarted. It also makes maintenance and operations easier as you can easily rotate nodes without downtime when doing version upgrades or other maintenance.


It offers a powerful permissions system that lets you share multiple separate workloads on the same internal cluster, and even give access to untrusted third parties: for example, letting the frontend have a user specific key-value storage, or letting a third party write events to a dedicated queue.


It's written in 100% safe[Rust](https://rust-lang.org/) , based on[fjall](https://crates.io/crates/fjall) (a fast LSM-tree-based storage similar to RocksDB), has libraries and SDKs for a variety of languages, and a CLI tool. It's open-source, with the source code[available on Github](https://github.com/svix/diom) .


## Current status


We have Svix fully ported and continuously running tests and simulated workloads on top of Diom in one of our staging environments, and it's currently under evaluation by one of the top AI labs.


With that being said, it's not yet GA (general availability). We are aiming for GA later in the year once we've moved over Svix production workloads to Diom. While we will try to minimize changes, some things may change between now and then. For example, the API and intra-cluster communication may change, and it's missing some functionality that we would like to be included like automatic Diom-managed backups, and more.


## Closing words


We are very excited about launching Diom publicly, and can't wait for people to try it out!


To get started, please check out[the docs](https://diom.svix.com/docs) or try it out using[the playground](https://diom.svix.com/playground) .


Please let us know if you have any feedback, or if there are any primitives that you'd like us to build next!


---


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
