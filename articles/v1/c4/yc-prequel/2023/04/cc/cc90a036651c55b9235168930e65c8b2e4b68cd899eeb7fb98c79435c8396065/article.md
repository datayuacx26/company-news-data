---
schema_version: "1.0.0"
document_id: "cc90a036651c55b9235168930e65c8b2e4b68cd899eeb7fb98c79435c8396065"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/sql-maxis-why-we-ditched-rabbitmq-and-replaced-it-with-a-postgres-queue/"
published_at: "2023-04-11T00:00:00+00:00"
first_seen_at: "2026-07-24T10:10:51.593417+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:b48e9a19acd5006c56719344203e9d05784a0e67e34fc8be60784cfc392baaa8"
---

# SQL Maxis: Why We Ditched RabbitMQ And Replaced It With A Postgres Queue

We recently ripped out RabbitMQ from our system and replaced it with a queue built on top of our Postgres db and written in SQL. The change took half a day to implement + test, and had a net delta of -580 LOC. Much more importantly, it drastically improved the reliability and resiliency of our system. This is a writeup of that change, the rationale behind it, and how we did it.


Note: this is not an indictment against RabbitMQ or other queuing systems. It’s quite possible (likely, really) that we were holding it wrong. If anything, it’s a push to keep things simple. And, of course, a very biased reminder that SQL is unbelievably powerful. In fact, it often really is all you need. Do we dream of a world where everything is SQL? Maybe.


## Some useful context


Let’s talk briefly about why we needed a queue in the first place. Prequel is a large-scale data pipeline: we help B2B SaaS companies sync data to or from their customer’s database. In other words, our business consists of running data transfers between various dbs and warehouses. Each of these transfers can be modeled as a job, which is put into a queue and processed by workers. One idiosyncrasy of these jobs is that they can be fairly long to process: it’s not unheard of for a data backfill to take several hours, and the fastest jobs take at least a few seconds. To make all of this run smoothly, we enqueue and dequeue thousands of jobs every day.


## RabbitMQ: good enough to start


When we initially designed our system, we settled on RabbitMQ as the right queuing solution for the job. It was widely adopted, based on an even more widely adopted protocol ([AMQP](https://www.amqp.org/) ), seemed mature and production-grade, and looked easy enough to add to our Helm chart. We wrote our own GoLang wrapper around an[existing client](https://pkg.go.dev/github.com/streadway/amqp) , didn’t think about it twice, and shipped it.


Our setup was fairly straightforward. We configured it such that any message could only be processed by one consumer: this allowed us to avoid having different workers running the same transfer, which would be a waste of everyone’s compute. We also set it up such that consumers would “manually” ACK messages. We tried to keep everything as lightweight as possible, and it worked well for us for a while.


## Issues started popping up


We started experiencing issues and challenges a few months in. We spent a lot of time tweaking our consumer reconnection behavior, aka what happens if somehow one of the workers loses its connection to the queue. We had a handful of multi-threading bugs where a consumer timing out from the queue would cause it to reconnect twice, effectively growing the number of queue threads exponentially. This led to some fun memory utilization graphs. We dealt with those and moved on.


Until one fateful evening, when we got paged by a customer. They were seeing some of their transfers arbitrarily delayed by up to a few hours, sometimes so much so that our system would mark them as stale and cancel them. There wasn’t any good explanation for this: our queue wasn’t showing any major backlogs, our workers looked healthy, and the majority of jobs were being processed just fine. Except for those occasional, seemingly non-deterministic stragglers. Of course, that didn’t make for a very satisfying answer for our customers: “sorry, we know a handful of your transfers are delayed by a matter of hours, but our system looks healthy so ¯\\_(ツ)_/¯”.


After *hours* of investigating and a bunch of banging our heads against our desks, we managed to root-cause the issue. Turns out each RabbitMQ consumer was prefetching the next message (job) when it picked up the current one. This prevented other consumers from acking the message, and thus from picking it up from the queue. See RabbitMQ documentation on this[here](https://www.rabbitmq.com/confirms.html#channel-qos-prefetch) .


Now, this is fine when most jobs take a roughly even and well-bounded amount of time. Where things get dicey is when some jobs can take hours (remember, we’re talking about data transfers here). What was happening in our case was: a worker would pick up a message, and prefetch the next one. It would process the current message, a multi-hour transfer, and hold the next one effectively hostage until it had completed the one it was chewing on.


Even worse, there was no way (that we could find) to disable the behavior. You could set the prefetch count to 1, which meant every worker will prefetch at most 1 message. Or you could set it to 0, which meant they will each prefetch an infinite number of messages. But there was seemingly no way to *actually* set the prefetch count to zero (ie disable prefetching).


## Earning our stripes as SQL maxis


We had a good grasp on what was happening, but we had no immediate way of fixing it. This was causing issues for production customers, so waiting it out wasn’t an option. We couldn’t figure out how to make the queue behave in the way that we wanted it to. We also couldn’t just rip out the queue entirely. Or could we…


It turns out that we could actually recreate the same queue functionality in our Postgres database, but tweak it to match our exact requirements. Better yet, that implementation would be *way* simpler and fully modifiable in the future if our requirements changed in any way.


So that’s what we built: a new queue backed by a single, and surprisingly simple, Postgres table. Our publishers write to it. Our consumers (workers) read from it. We maintain things like queue ordering by adding an` ORDER BY` clause in the query that consumers use to read from it (groundbreaking, we know). And we guarantee that jobs won’t be picked up by more than one worker through[simple read/write row-level locks](https://www.postgresql.org/docs/current/sql-select.html#SQL-FOR-UPDATE-SHARE) . The new system is actually kind of absurdly simple when you look at it. And that’s a good thing. It’s also behaved flawlessly so far.


It comes with several, significant upsides for our team. For one, the application state is no longer spread out over two systems (RabbitMQ storage and Postgres). It’s now centralized in our application database. This makes disaster recovery that much easier, and increases the resiliency of the overall system by removing moving pieces.


Better yet, we all understand this queuing system *really* well since we wrote it and are all fluent in SQL. This allows us to tweak it if we need to update its behavior, and makes monitoring and debugging easier. We already have plenty of tooling set up to deal with Postgres, and we don’t need to remember how to set up this one different piece of infra like we did with RabbitMQ. That may not sound like much, but the delta in time and complexity makes a world of difference during an outage.


One of our team members has gotten into the habit of pointing out that “you can do this in Postgres” whenever we do some kind of system design or talk about implementing a new feature. So much so that it’s kind of become a meme at the company. But there’s a good reason for it: he’s right. It turns out that, when your problem involves enforcing constraints on your data, you can do it in Postgres, and it’s probably way simpler than the mountains of code you were going to write for it otherwise. We’re proud SQL maxis.
