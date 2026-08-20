---
schema_version: "1.0.0"
document_id: "cdba7e04f2ca75715d32eb22ef077a2f27b96a59241f0d69133d43e49e64a330"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-gocache-v2-faster-improved-golang-build-performance"
published_at: "2025-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:1fc31247141ce568661620cd97e93a59bf33ba82f10ca6cd8dfefd710cc1e4f9"
---

# Now available: Gocache v2 for improved Golang build performance

Earlier this year, we introduced Gocache support in Depot Cache, allowing Golang builds to automatically reuse previously generated artifacts via` GOCACHEPROG` . This delivered a modest boost to build performance. However, we knew there was room to push things even further.


Today, we’re excited to share Gocache v2, which significantly reduces network traffic and unlocks even faster builds. With this update, Golang build times in CI can improve by up to 4x, and you can expect even bigger gains when paired with[Depot Runners](https://depot.dev/products/github-actions) .


[Want to dig deeper into how Gocache V2 works and learn how to get started? Read the Gocache docs →](https://depot.dev/docs/cache/integrations/gocache)


## How Gocache works: Initial implementation of Golang build caching


We implemented the` cacheprog` protocol inside our cli with a` depot gocache` subcommand, which allows golang to spawn it as a subprocess and issue simple gets and puts. Our cli then translates those requests into traffic to Depot Cache, where artifacts will be durably stored and made globally available. Those artifacts can be reused to save time in any future build, run by you, your teammates, or CI. You can read more about the` cacheprog` protocol and our implementation in our[initial launch post](https://depot.dev/blog/introducing-depot-cache) .


## Real-World impact: Analyzing Golang build cache performance


The vast majority of traffic post-launch came from jobs running on our ephemeral GitHub Actions runners. And boy, was it a lot of traffic. We knew the cacheprog protocol was granular and expected it to be chatty, but we didn't anticipate just how chatty it could be.


The result of every repeatable computation -- incremental and final compilation artifacts, test results, vet results, etc -- is stored and retrieved from the configured` cacheprog` . Many of these cache entries are less than 1kb in size. Some are simply a cache key with a 0-byte value. Altogether, we've observed that this workload is *over an order of magnitude* chattier than even our heavier[bazel](https://depot.dev/blog/faster-bazel-builds-with-remote-cache) workloads.


This level of granularity is great for the end user: smaller increments mean a smaller blast radius when invalidating cache entries, which in turn make cache entries more reusable overall. This no doubt contributes to golang's impressive build performance. However, a problem emerges when each cache operation resolves to an individual network call to S3, where our cache resides. The network between EC2 and S3 is extremely fast, so any single request is not inherently problematic. But each request has overhead which, especially when accounting for small and 0-byte files, altogether adds up to significant downward pressure on our ability to accelerate builds, to say nothing of the obviously negative cost implications.


[Ready to unlock faster builds and supercharge your Go projects? Check it out free for 7-days. Get started →](https://depot.dev/sign-up)


## Next iteration: Bundled Golang build cache for faster builds


Our goal, then, was to decrease overall network activity, and we began with a relatively small code change: bundling cache entries together. When a PUT request arrives, rather than immediately turning it around to S3, we append it to an in-memory buffer and mark its offset and length. Once that buffer reaches a target size, the entire buffer is submitted to Depot Cache as a bundle.


On the theory that adjacent cache requests originate from proximate sections of the build graph, GETs locate and download their segment as well as its whole corresponding bundle and segment index. In other words, a request for one segment will download hundreds of sibling segments with high likelihood of being used in the immediate future. Only the first segment in a bundle pays the cost of a round trip in milliseconds -- a subsequent GET for another segment is served from disk often in tens of microseconds. The net effect? A small code change with an outsized impact.


Gocache V2 build time comparisons


Whereas in our launch post we saw Tailscale builds completing 22% faster, we're now seeing runtime for cached builds decrease by almost a factor of 4. Tests are running more than twice as fast. Both the experiment and the control were performed within a Depot Runner, so you can imagine the delta over a stock GitHub runner is even larger still. Doing this work generated a whole host of other ideas for how to improve Depot Cache performance, but the next-most impactful change is likely teaching our cache client to bundle cache entries for other supported build tools.


## Boost your CI speed: Try Depot runners with Gocache v2


If you’re building Go services and haven’t optimized caching in CI, you’re likely leaving performance on the table. Try Depot Runners with Gocache v2 for free during your first 7 days and see how much faster your builds can be.[Get started today](https://depot.dev/sign-up) .


## Related posts


- [Introducing: Depot Gocache](https://depot.dev/blog/introducing-depot-cache)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Luke Morris


Staff Software Engineer at Depot
