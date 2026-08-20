---
schema_version: "1.0.0"
document_id: "2e1518d90a790093470ab5f39d245cc6248320ddcc90090758e13c884f90b6e9"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/annotated-zanzibar-launch"
published_at: "2022-12-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:93e52e2276bffcbca63f7a5b088d9e96cc5327a52671d4fb59e8b17ba2343d5c"
---

# Google Zanzibar Through Our Eyes

You can skip ahead and read the paper[here](https://authzed.com/zanzibar)


In the past I have[written](https://authzed.com/blog/what-is-zanzibar/) and[spoken](https://www.youtube.com/watch?v=1nbSbe3kw2U) quite frequently about how incredible I find Google’s Zanzibar paper. Battle scars from my past experience building and running permissions systems for Quay and CoreOS taught me that it was compelling enough to form a new company implementing the concepts and features explained in the paper. I originally read the paper on the first day that it[hit Hacker News](https://news.ycombinator.com/item?id=20132520) , June 8, 2019. I knew then that it was something that I wanted to pursue together with the others in our founding team: Joey and Jimmy. It took a little while to get the ball rolling, but today SpiceDB is powering permissions in hundreds of applications, serving all of their respective end users.


A while back Evan, one of our engineers, had the idea to host an annotated copy of the Zanzibar paper so that people experiencing the paper for the first time could see it through our eyes. Who better to help a reader explain and understand the concepts presented in the paper than those who have put it all on the line bringing the paper to life through a variety of[projects](https://github.com/authzed) and[products](https://authzed.com/) ? So today, we’re unveiling a brand new experience built to help the reader understand and interpret the Zanzibar paper, including both the model and its implementation.


> Quite a few companies and projects today are claiming to “do Zanzibar”, but many support only some fraction of the Zanzibar programming model...


Quite a few companies and projects today are claiming to “do Zanzibar”, but many support only some fraction of the Zanzibar programming model, and often implement few to none of the engineering best-practices that Google has included in the paper based on their years of running and improving the service; refinements such as[relation-rewrites](https://zanzibar.tech/2SsR9DA-h7:0:L) ,[hotspot sharding](https://zanzibar.tech/2SQLmEVo5y:2d._PANC-Vk:I) ,[caching](https://zanzibar.tech/2RymZp8Vn5:4O:5R) , and[request hedging](https://zanzibar.tech/25BZCydh9r:1F:2N) get left out. To be fair, each of those features could practically be a research paper on their own, and in fact[hotspot sharding is](https://research.google/pubs/pub46921/) !


We’ve spent the last few years using the Zanzibar paper as our architectural north star for the design and implementation of[SpiceDB](https://github.com/authzed/spicedb) , and consequently all of the products and services that we’ve built on top of and around it. Closely adhering to the paper gives us a solid bedrock from which to build a system with some quite lofty[availability](https://zanzibar.tech/2JsohM16rk:y:2H) ,[scalability](https://zanzibar.tech/2SuZL2BPQU:0:15) , and[latency](https://zanzibar.tech/244-S0qtcs:t:2W) goals. Google has paid handsomely for the knowledge and experience baked into the Zanzibar paper: we thank them for the opportunity to follow in their footsteps and extend their work to meet the needs of our diverse set of users and customers.


Without further ado,[The Zanzibar Paper, annotated by Authzed](https://zanzibar.tech/) .


We invite you to read or re-read the paper with some sections highlighted with additional information, thoughts, and open questions from our team. If you want to discuss anything in particular, our team is always available on our[Discord Server](https://authzed.com/discord) . Finally, if this is your first time experiencing Zanzibar, you can try out the concepts and our implementation using our[Playground](https://play.authzed.com/) or[SpiceDB](https://github.com/authzed/spicedb) open source.


In keeping with our open source roots, we're also[open sourcing the repository](https://github.com/authzed/zanzibar-annotated) behind this annotated paper experience. If you're interesting in contributing your own annotations,[open a PR](https://github.com/authzed/zanzibar-annotated/pulls) . If you want to use this to annotate another paper,[make a fork](https://github.com/authzed/zanzibar-annotated/fork) !


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


## FAQ


### What is Request Hedging in Google Zanzibar?


Request hedging is a tail-latency technique built into Google Zanzibar that sends a duplicate of a slow backend request to another replica, then uses whichever response arrives first. The hedge only fires after a dynamic per-call delay threshold, typically around the 95th percentile of recent latency for that request class, so only true outliers trigger extra work. AuthZed's[annotated Zanzibar paper walkthrough](https://authzed.com/zanzibar) covers how this fits into Zanzibar's broader latency strategy.


Zanzibar applies hedging specifically to reads against Spanner and its Leopard indexing service, not to inter-server permission check RPCs, since those vary too much in cost to duplicate safely. In production, roughly 1% of Spanner reads benefited from hedging while keeping median latency around 0.5ms. AuthZed's[blog post on the HTTP API](https://authzed.com/blog/authzed-http-api) notes that request hedging appears as an enabled behavior in SpiceDB startup logs.


SpiceDB exposes experimental dispatch-layer flags, including` --experimental-dispatch-secondary-upstream-addrs` and related hedging delay options, documented in the[SpiceDB commands reference](https://authzed.com/docs/spicedb/concepts/commands) . These target internal dispatch paths rather than the client-facing API. Teams enabling them should validate under load and review[AuthZed best practices](https://authzed.com/docs/best-practices) alongside the[Zanzibar overview](https://authzed.com/blog/what-is-google-zanzibar) for full context.


On this page


- Additional Reading
- FAQ
- What is Request Hedging in Google Zanzibar?


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
