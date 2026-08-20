---
schema_version: "1.0.0"
document_id: "d60dcafc3eb7ed2197643a4519a3eeccc1e586d080d3770227fb73d473a2edcd"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/ga"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:0b94eade720c9efad999bb4b4f20bb2753d562ab48eba2069b0c4d88f80d6e6f"
---

# General availability & our seed round

The s2.dev cloud service is now **Generally Available** !


We're also thrilled to share that we raised **$3.85M** in a seed round led by **Accel** , with participation from **Y Combinator** , a terrific group of angel investors like Theo Browne (t3.gg), Charles Zedlewski (Together AI), Paul Masurel (Quickwit), and more. This brings our total capital raised to **$5.5M** .


## What we do


S2 is making real-time, serverless data infra for AI builders. We give users an API for durable streams, unlimited in number and storage.


The need for real-time infra is everywhere today: token streaming, live observability, agent communication. This has only reinforced our[original thesis](https://s2.dev/blog/intro) that streams need to be as simple, reliable, and scalable as object storage.


Our customers are now creating millions of durable streams and pushing terabytes of data, each week.


> S2 is such a useful primitive for building realtime features. We use it to stream telemetry data so that users can see in realtime things like logs and metrics. It’s been rock solid and reliable—the kind of infra that just works™.
>
>
> — Rafael Garcia, CTO,[Kernel](https://www.kernel.sh/)


> S2 solved our problem with reliably streaming long-running AI sessions. Before, connection drops could cause lost data and broken streams. S2's bottomless storage means our customers can stream for hours and network hiccups don’t matter.
>
>
> — Matt Aitken, CEO,[Trigger.dev](https://trigger.dev/)


## The gap we fill


Companies have been stitching complex systems together to try to acommodate the demands of real-time AI applications.


LLMs return token streams. Sandboxed execution involves remote I/O streams. Agent sessions evolve as a sequence of events. How do you ensure real-time visibility with long-term history? What does distributed plumbing for multi-agent architectures look like?


With traditional infra like Kafka, NATS, or Redis Streams, you are[forced to choose](https://s2.dev/blog/agent-sessions#landscape) between cardinality of streams and durability.


What if one serverless resource could persist every agent action at the session-level — and make it instantly visible to any number of readers?


That's S2.


## General availability


GA reflects our confidence in the maturity of the service, and our commitment to stay highly available, durable, and consistent. As a data infra company, this is existential for us.


So many aspects of our work have given us this confidence: we gained experience running production workloads for our customers; focused on clarifying, simplifying, and hardening[our architecture](https://s2.dev/docs/platform/architecture) ;[invested](https://s2.dev/blog/dst)[heavily](https://s2.dev/blog/linearizability) in verification through simulation.


## Where we are going


Reliability, scalability, performance, and security will always be a priority.


What we have in mind as we evolve the product, shaped by what we have heard from users —


- **Features for agent builders:** first-class support for forking, integrations
- **Security:** bring-your-own-key encryption, stateless[access tokens](https://s2.dev/blog/access-control)
- **Expansion:** additional cloud regions, and customer VPCs
- **Higher layers:** queuing, large messages, and more to come


We are so excited to continue the work of **making streams a cloud storage primitive** . Thank you to all of our customers, testers, and[open source](https://github.com/s2-streamstore/s2) adopters.


---


If you are new to S2,[try it out](https://s2.dev/docs/quickstart) with free credits! You can also connect with us onemail or[Discord](https://discord.gg/JfTWJ5xxZ6) .


## All our investors


Accel, Adam Suskin, Alessandro Puppo, Aman Sidhant, Amitav Chakravartty, Andrew Stanton, Benjamin Bryant, Brian Kim, Chalmers Brown, Charles Zedlewski, Deep Kapur, Dennis Beatty, Eight Capital, Fredrik Björk, Grayscale Ventures, Hemanth Soni, James Wu, Jeff Ling, Jeremy Hindle, JJ Fliegelman, Kevin Li, Keyur Govande, Li Sabhaya Capital, Lyon Wong, Manju Rajashekhar, Materialized View Capital, Micah Wylde, Michael Shimeles, Mokhtar Bacha, Nikitha Suryadevara, Nimit Maru, Orange Collective, Paul Masurel, Pioneer Fund, Race Capital, Rafael Garcia, Ritual Capital, Rush Sadiwala, Satish Talluri, Shane Barratt, Spot VC, Theo Browne, Transpose Platform, Twenty Two Ventures, Uncorrelated Ventures, Victor Mota, Y Combinator
