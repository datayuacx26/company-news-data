---
schema_version: "1.0.0"
document_id: "cdaf398f05c7d9264e51ad54236de8e8268c0bffda0a0472f92ad3aed495d4b2"
company_key: "yc-pgdog"
company: "PgDog"
source_id: "yc-pgdog-rss-662afb283f74"
canonical_url: "https://pgdog.dev/blog/scaling-postgres-listen-notify"
published_at: "2025-08-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:49.478310+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:343043f6eba0f540fcafbc296b41fec5b506316ab34e2b7828373f8e609d7169"
---

# Scaling Postgres LISTEN/NOTIFY

Postgres has a cool, albeit lesser known, feature: pub/sub. You can listen for and send arbitrary messages, in real time, using Postgres as the broker. Unfortunately, it was only usable by directly connecting to Postgres. For those of you who have 100+ containers across several services, that isn’t really an option.


That limitation seemed a bit arbitrary, so we added support for` LISTEN` /` NOTIFY` to PgDog. Now, you can have thousands of listeners and just as many publishers, without adding specialized message brokers to your stack.


### Pub/sub basics


If you’re familiar with how this works, you can skip straight toarchitecture . For everyone else, a quick introduction.


This game has two players: *pub* lishers and *sub* scribers. Publishers are sending messages while subscribers wait for them and can perform actions once messages are received.


Any Postgres client can send a message using the` NOTIFY` command. They only need to specify the name of a channel and an optional payload. The channel, also known as “topic”, separates messages into their own queues.


Subscribers are Postgres clients that declare their interest, by using the` LISTEN` command, in receiving messages from a particular channel. Each channel subscriber receives one copy of each message sent to that channel.


*How pub/sub works.*


Subscribers can listen on more than one channel. Publishers can send messages to multiple channels as well. This is the foundation for decoupled software architectures. Publishers don’t need to know who’s listening and subscribers don’t need to care where the messages are coming from. They just need to know what to do with them or when to send them.


For example, if you send a notification every time a user signs up for your app, one listener can send your user a welcome email while another can send a notification to your Slack. Neither of them need to know about each other.


Postgres works great as the central communication hub. The problem arises once you start pushing its` max_connections` limit.` LISTEN` /` NOTIFY` must share one database instance, so traditional scaling techniques, like adding replicas, don’t work.


We fixed that by implementing pub/sub in our proxy, while using Postgres as the broker between its multiple instances. PgDog itself doesn’t have a connection limit, so you can basically an have unlimited number of publishers and subscribers.


### Architecture


PgDog is written in Rust, using the Tokio runtime. Tokio has many features, like multithreading, a fast async task scheduler, and async I/O. It also comes with powerful synchronization primitives, like the[broadcast](https://docs.rs/tokio/latest/tokio/sync/broadcast/index.html) channel. Each client connected to PgDog runs inside its own task, and passing messages between them is a solved problem.


So, by virtue of using Tokio, we already have a pub/sub system in place. We just needed to hook it into our parser and to make sure commands are forwarded to Postgres, so multiple instances of PgDog would receive all messages. Since we’re using[pg_query](https://pgdog.dev/blog/hacking-postgres-wire-protocol) , we had no trouble parsing Postgres commands and extracting arguments.


*Background task handles LISTEN and NOTIFY commands.*


To limit the number of connections we’re using to talk to Postgres, PgDog processes commands in a background task. The task talks to Postgres, while clients talk to it through Tokio channels.


When a client sends a notification to a topic, we forward it to the task which, in turn, sends it over to Postgres using the same` NOTIFY` command. The database sends it to all instances of PgDog that registered themselves, on the clients behalf, to listen on that topic. Once we receive the message, we copy and forward it to all registered clients on our side.


Postgres only has to send a message as many times as there are instances of PgDog. PgDog can have thousands or even millions of clients, and it’ll forward messages without putting that load on Postgres.


*Multiple instances of PgDog receive all messages.*


#### Performance, guarantees & trade-offs


Both` LISTEN` and` NOTIFY` commands are acknowledged by PgDog immediately. We don’t wait for an answer from Postgres. This removes the latency penalty of using a proxy between your app and the database. However, since the commands are queued up and executed in the background, it’s important to understand the trade-offs.


The first client is guaranteed to be, *eventually* , subscribed to a topic after sending the` LISTEN` command. All subsequent clients are added immediately. Since PgDog executes it on the client’s behalf, there is a short delay between when the first command is acknowledged and messages start to arrive. This is usually acceptable, since there are often multiple instances of the same listener in production. If you want to guarantee that someone is always listening for messages, you can use the blue/green deployment strategy with a short delay between cutovers.


Distributed systems tend to choose either *at least once* or *at most once* strategies for communication. *At least once* will attempt to deliver a message until it’s successfully read by the receiver. This can result in the same message received multiple times. Application code needs to be aware of this and deduplicate messages or their end, or write idempotent code.


*At most once* , on the other hand, will attempt delivery just one time. If it doesn’t succeed, the message is lost. This counts on the fact that networks are generally reliable and the vast majority of messages will be delivered. Clients don’t need to build complex deduplication logic or ensure idempotency, which often isn’t trivial.


PgDog’s pub/sub system is *at most once* .` NOTIFY` messages are immediately acknowledged and forwarded in the background. We make every effort to deliver all of them, but intermittent connectivity errors in production cannot be avoided. If we lose the connection to Postgres during message delivery, we don’t attempt to deliver that message again. This prevents the same message from being delivered twice.


If you’re building systems that require 100% message delivery guarantees, consider storing messages in a Postgres table and using a secondary polling mechanism to fetch the ones you haven’t received yet. This is a typical pattern for job queues where losing jobs can be disruptive. Real time notifications help jobs run immediately, while occasionally polling for abandoned entries ensures they are all eventually processed.


##### Sharding topics


Since PgDog is primarily built for sharding Postgres, we should mention that we also sharded` LISTEN` /` NOTIFY` . For simple deployments, we use the primary database as the message broker. If you have a sharded Postgres cluster, you have multiple primaries. To avoid a single point of failure, PgDog hashes the channel name, using the Postgres’[partition by hash](https://pgdog.dev/blog/hacking-postgres-wire-protocol) function, and routes commands relevant to that channel to the matching shard.


This is similar to how clustered RabbitMQ or clustered Redis works, except it all happens inside Postgres (and PgDog, of course) so you don’t need to manage yet another database.


### Conclusion


PgDog is scaling Postgres, one feature at a time. While sharding is our north star, other features we use and love are being taken care of as well. If you like what you just read,[get in touch](https://pgdog.dev/cdn-cgi/l/email-protection#a5cdcce5d5c2c1cac28bc1c0d3) . We’re happy to help you deploy PgDog to your production stack.
