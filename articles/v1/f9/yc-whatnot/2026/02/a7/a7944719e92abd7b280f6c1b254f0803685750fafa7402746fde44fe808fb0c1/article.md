---
schema_version: "1.0.0"
document_id: "a7944719e92abd7b280f6c1b254f0803685750fafa7402746fde44fe808fb0c1"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/scaling-whatnot-behind-the-largest-live-shopping-stream-in-us-history-040a458f538c"
published_at: "2026-02-24T14:33:16+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-07-28T22:19:12.535128+00:00"
content_hash: "sha256:6c2429712dc8226765e2649210021dc13a2563a74643813695c9d6412e9fc90e"
---

# Scaling Whatnot: Behind the Largest Live Shopping Stream in US History

Featured


# **Scaling Whatnot: Behind the Largest Live Shopping Stream in US History**


[Whatnot Engineering](https://medium.com/@whatnotengineering?source=post_page---byline--040a458f538c---------------------------------------)


11 min read


·


Feb 24, 2026


--


Press enter or click to view image in full size


On February 8, 2026, over a half million viewers tuned in to watch MrBeast give away 1 million dollars in prizes on Whatnot.


On Big Game Sunday 2026, MrBeast went live on Whatnot for a giveaway show that would become the largest live shopping event in US history. At peak, 583,000 concurrent viewers were watching a single show on our platform. Over 555k people entered a single giveaway. We drove hundreds of thousands of new signups in 24 hours. If any one of a dozen systems buckled, it would have happened live on camera.


We pulled it off with zero major incidents. But that outcome was never guaranteed. It took months of preparation, 60+ engineers across every major engineering org, and some of the most significant infrastructure investments we’ve ever made. In this post, we’ll walk through the biggest technical challenges we faced and how we solved them, not with throwaway scaffolding, but with durable platform improvements that raise our scalability ceiling for every seller and buyer on the platform.


We’ll cover the work in three parts. First, the key architectural investments we made to handle this scale: admission control, connection pooling, feed resilience, and video infrastructure. Then, how we validated it all through progressive production load testing. Finally, what happened on event day, what we learned, and what we’re carrying forward.


## Setting the Stage


If you’ve followed our blog, you might remember our[Post Malone “Post-Poned” post](https://medium.com/whatnot-engineering/open-letter-on-scalability-reliability-34ca347dcad) from 2022 or our[three-part series on preparing for the 2024 Big Game](https://medium.com/whatnot-engineering/whatnots-data-driven-approach-to-scalability-reliability-for-big-on-platform-events-166c0cceb171) . Each of those events pushed us to improve, and each one revealed new limits. As our community has grown, scaling our infrastructure to match has been a consistent priority. The MrBeast event was on a different order of magnitude entirely, but it accelerated work that was already underway.


Our target was to support 1 million concurrent viewers on a single stream and 1.35 million across the platform. To put that in perspective, our previous largest event had been a fraction of that. Every layer of our stack — from the edge to the database — would face traffic levels we had never seen in production. And we had about 10 weeks to get ready.


We kicked off in early December 2025 with a planning process that cut across every engineering org: SRE, Platform Engineering (our Elixir-based Live Service and Python-based main backend), Mobile, Web, Security, Discovery, Data, and Notifications. Twice-weekly readiness reviews ran for the full 10-week period. Every week, we ran production load tests at progressively higher concurrency, and each test surfaced new bottlenecks that drove the following week’s priorities.


One thing we knew going in: this was a live, largely unscripted event. We had a run of show, but MrBeast could go off-script at any moment, and hundreds of thousands of viewers would act on it simultaneously. We couldn’t predict exactly what would happen, so we designed our protection mechanisms — admission control, rate limiters, feed fallbacks — around the assumption that we’d face unpredictable traffic patterns, not just high volume.


By late January, we had validated our systems at 1.46 million total concurrent users with roughly 970,000 in a single stream — well beyond our targets.


## The Bouncer: Building Platform Admission Control


The single highest-risk technical challenge was a deceptively simple question: what happens when more users show up than our infrastructure can safely handle?


Without admission control, a sudden surge of over a million concurrent users would overwhelm backend services and trigger cascading failures — not just for event viewers, but for every seller running a live show and every buyer making a purchase across the platform. We needed a way to cap active sessions while giving excess users a clear, friendly experience rather than a broken app.


We built the **Client Admission Service (CAS)** : a Go-based service that enforces a global session cap, rate limits how quickly new users can join the platform, and implements deterministic identity-based load shedding.


A core design choice was that admission control should be invisible to anyone already on the platform. If you were watching a live show or mid-purchase when the event peaked, your session was protected — CAS would never interrupt an existing user to make room for a new one. This meant sellers could run their shows, and buyers could complete transactions without any awareness of the capacity event happening at the edge, while newcomers received a clear, friendly explanation instead of a broken experience.


To include CAS in all requests, we decided to use Envoy’s ext_authz filter for inline authorization, which in turn drove the introduction of **Istio** as our new ingress infrastructure, replacing previously used ingress-nginx. The resulting traffic path looked like this:


Press enter or click to view image in full size


We shipped CAS support across iOS, Android, and Web. Each platform handles the “over capacity” state, so users who can’t be admitted see a clear explanation instead of errors or hangs. Production load tests validated CAS performs at sub-millisecond latency with 99.9999% availability while handling 1M+ admitted users, with the remainder cleanly rejected via special error responses.


On event day, CAS performed exactly as designed — admitting our target concurrency and rejecting excess traffic, protecting the experience for hundreds of thousands of viewers already in the stream. And it remains in production as reusable admission control for any future high-traffic event.


## Breaking Through the Connection Ceiling


As we scaled our main backend environment (MBE) horizontally during load tests, we hit a wall that no amount of additional pods could solve: connection limits on downstream data stores.


At a certain scale, three things started breaking simultaneously. Redis connections approached capacity across our clusters. OpenSearch coordinator nodes started failing, cascading errors to all direct clients. And Kafka brokers neared their connection ceiling.


The root cause was structural. Every MBE pod maintained its own connections to every downstream data store. More pods meant linearly more connections, and data stores have hard limits that don’t scale the way compute does.


We introduced proxy layers to decouple pod scaling from connection capacity:


- **Envoy proxies for Redis and OpenSearch.** We deployed Envoy in front of 27 Redis/Valkey clusters and 4 OpenSearch clusters for connection pooling, health checking, and automatic failover. Instead of tens of thousands of pods each maintaining direct connections, the Envoy fleet maintains a fixed, manageable connection pool regardless of how many MBE pods are running behind it. We completed the Redis cutover by mid-January, which unblocked the load test where we first crossed 1 million concurrent users.
- **kafka-proxy for Kafka.** Kafka brokers were our tightest constraint. We built kafka-proxy, a custom Go DaemonSet that communicates with MBE pods via Unix Domain Sockets and pools Kafka connections per Kubernetes node. The team built and shipped it in about a month, enabling us to run over 50,000 MBE pods in production.


These proxy layers are permanent infrastructure. Before this work, scaling MBE beyond 15,000 pods was effectively impossible. Now it’s routine.


## Protecting the Feed: Doomsday Feed


There’s a moment in any massive live event where a huge number of users hit your app at the same time: a push notification goes out, the stream ends, someone opens the app to see what all the buzz is about. At Whatnot, that means they all hit the feed simultaneously.


Our primary feed pipeline was not designed to handle a million concurrent feed requests. Under that kind of spike, it would have buckled — meaning new users wouldn’t see any content at all, including the MrBeast stream they came for.


We built what we call **Doomsday Feed** : a hardened fallback feed path purpose-built for extreme load. It uses in-memory caching backed by Redis, serves a curated set of content, and can sustain over 20,000 queries per second, validated through dedicated load testing ahead of the event. We added a rate limiter that automatically sheds feed load above a certain request rate to the Doomsday Feed, a forced killswitch for instant activation, along with admin tooling for manual cache refresh. For the MrBeast event, we statically inserted the Beast stream into the Doomsday Feed so that every user hitting the fallback path would see the stream front and center, and curated it with after-party shows for stream exit, where 95% of feed requests ended up flowing through the fallback path.


On event day, Doomsday Feed activated as designed, serving over 708,000 feed responses during the 90-minute main event window and absorbing load that would have otherwise overwhelmed the primary feed pipeline. This wasn’t a degraded experience — it was the system doing exactly what it was built to do.


Like everything else described in this post, Doomsday Feed is permanent infrastructure. The caching layers, rate limiter, killswitch, and refresh tooling are durable improvements to feed resilience that protect against any future traffic spike.


## Scaling Video with Agora


Scaling our application infrastructure was only part of the challenge. Delivering real-time video to over half a million concurrent viewers was another. Whatnot uses WebRTC to deliver video with just milliseconds of latency. This is ideal for auctions with up to a few thousand buyers, but its connection model is fundamentally not designed to scale to the levels we required.


For the MrBeast event, we partnered with[Agora](https://agora.io/) based on their ability to support the extreme single-channel concurrency we were targeting. The joint preparation started roughly 10 weeks before the event. Together, we designed a dedicated streaming configuration optimized for ultra-high single-channel concurrency, provisioned isolated capacity to prevent cross-impact to other live streams on the platform, and ran progressive load tests validating the video path up to 1.3 million concurrent viewers. Testing covered rapid traffic ramp-ups, sustained high-concurrency plateaus, and regional failover scenarios, all without advance warmup of the infrastructure.


On event day, Agora’s real-time media pipeline performed reliably at peak, supporting 582,225 concurrent viewers within a single RTC channel. Time to first frame stayed under 1 second, latency remained consistently low, and video quality held stable throughout the stream, even as we pushed systems to their limits at extreme load. Agora’s delivery network provided resilience against packet loss up to 70–80%, with adaptive bitrate and congestion control applied dynamically. A 30-person cross-functional war room between Whatnot and Agora maintained real-time telemetry monitoring across 72 hours of continuous on-standby support.


This kind of collaboration, where each team owns the layer they’re experts in, is what makes operating at this scale possible. We focused on the application layer while Agora ensured the video stream reached hundreds of thousands of viewers without degradation.


## Validating Everything: Load Testing in Production


Load testing was the connective tissue across all of this work. Every major issue we fixed before the event was first surfaced by a load test.


Starting in early December, we ran weekly load tests against production. Not a staging environment. Production. Real traffic patterns against real infrastructure to find real bottlenecks.


The load testing framework itself required significant investment. We added cookie persistence across restarts, dynamic user scaling, giveaway participation flows, onboarding and signup scenarios, and configurable interarrival rates. Each test simulated realistic user behavior, not just raw connection counts.


Press enter or click to view image in full size


*Concurrent simulated users by load test in the weeks leading up to the event.*


The progression told a story. Each week’s test surfaced new bottlenecks: connection pool exhaustion, read replica saturation, worker queue contention. Each issue was resolved before the next test. This cadence of test, find issues, fix, retest is what gave us confidence grounded in data rather than hope.


One of the most impactful discoveries was the need for a dedicated analytical read replica cluster. Heavy queries (listings, offers, order history) were saturating the primary read replicas under peak load, which would have caused elevated latency for all user-facing reads. We stood up a new Aurora cluster in a separate AWS region to isolate these queries, built a new database routing layer with automatic fallback, and systematically migrated the heaviest query families. That cluster remains in production, continuing to protect user-facing performance.


## Event Day: What Worked and What Was Stressed


On Big Game Sunday, the main event went live. Here’s what happened.


**CAS** cleanly managed admission, holding platform concurrency within our target envelope. **The giveaway system** processed 555,000 entries at peak — 554,631 on the Lamborghini giveaway alone — without data integrity issues. **Push notifications** delivered at scale without queue backlogs. **Cloudflare** handled traffic spikes with no WAF-triggered incidents. **Doomsday Feed** activated and absorbed feed traffic as designed.


Press enter or click to view image in full size


*MrBeast gives away a Lamborghini at peak 583K stream viewership*


Not everything ran smoothly. During a 15-minute window around peak viewership, our Live Service experienced significant pressure: distributed Erlang proxy calls between nodes timed out, DynamoDB throttled on a hot key, and viewer count reporting temporarily stopped. Rate limiting engaged earlier than planned to protect users already in the stream.


Later, on-stream calls to action (“go follow me”) triggered two profile-visit stampedes as hundreds of thousands of viewers simultaneously loaded MrBeast’s profile page. Our rate limiters caught these surges and protected the backend, working exactly as intended.


We share these details because we think transparency matters. No system at this scale runs perfectly. The difference between a successful event and a failed one isn’t the absence of pressure — it’s whether the system degrades gracefully or cascades into failure. Our systems degraded gracefully. Users in the stream stayed in the stream. The giveaways worked. The video kept playing.


The biggest lesson was about the shape of the load, not just the size. Our load tests ramped users gradually and distributed actions over realistic intervals. But when MrBeast says “enter now” to 500,000+ viewers, the thundering herd arrives in under a second. Our protection mechanisms handled these surges, but we underestimated just how instantaneous they would be. For future events, we’re introducing more chaotic, bursty patterns into our load tests to better simulate what a live, unscripted event actually looks like.


## What We’re Taking Forward


From the outset, a stated goal was that this preparation should improve the platform broadly, not just serve a single event. Roughly 75–80% of our engineering investment went toward durable improvements that remain in production today:


- **Istio gateway** replaces our deprecated ingress controller, with circuit breaking, outlier detection, and connection pooling.
- **Envoy proxy layers** in front of Redis, Valkey, and OpenSearch clusters, decouples horizontal scaling from data store connection limits.
- **Kafka proxy** decouples backend pod scaling from Kafka broker connection capacity.
- **Client Admission Service** is a reusable platform for admission control, ready for any future high-traffic event.
- **Doomsday Feed** with in-memory caching, Redis cache layer, and operational tooling acts as feed resilience infrastructure.
- **Analytical read replica cluster** isolates heavy queries from latency-sensitive user-facing reads.
- **Load-testing framework** with realistic user flows is reusable for all future capacity validation.


The event-specific work — custom splash screens, deeplinks, temporary feature flags, manual scaling overrides — was a small fraction of the total effort and is straightforward to replicate or automate for future events.


## Come Build With Us


The MrBeast stream was the largest event in Whatnot history, but it won’t be the last. Every improvement we made raises the ceiling for what comes next. If building systems at this scale sounds interesting to you — live commerce, real-time infrastructure, the intersection of community and technology —[we’re hiring](https://www.whatnot.com/careers) .
