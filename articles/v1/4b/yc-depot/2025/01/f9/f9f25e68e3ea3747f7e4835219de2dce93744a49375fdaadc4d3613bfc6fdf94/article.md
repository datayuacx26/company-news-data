---
schema_version: "1.0.0"
document_id: "f9f25e68e3ea3747f7e4835219de2dce93744a49375fdaadc4d3613bfc6fdf94"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/larger-builders-for-faster-builds"
published_at: "2025-01-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:91eaac04be39baee3ecbb50b0a01545b984b4e7f544c4b87703210e3713f8663"
---

# Now available: Larger builder sizes for Depot

We're excited to announce that you can now configure your Docker image builds to leverage larger builders! You can now choose between our default 16 CPUs with 32 GB memory, 32 CPUs with 64 GB of memory, and 64 CPUs with 128 GB of memory.


## Larger builders for ... larger builds


Our default builder configuration of 16 CPU cores and 32GB of memory, dates back to when we launched[Turbo Builders](https://depot.dev/blog/turbo-builders) . This setup typically works well for a broad set of builds. But, with the rise of more intensive builds, folks need more power for complex and compute-heavy workloads. So, we've expanded to offer two additional builder sizes: Large and Extra Large.


- Default: 16 CPUs with 32 GB of memory
- Large: 32 CPUs with 64 GB of memory
- Extra Large: 64 CPUs with 128 GB of memory


When would you might need larger builders for building your container images? Larger builders are particularly valuable for builds that can take advantage of increased parallelization and memory. Some examples include:


- Building large Rust codebases
- Creating binaries for multiple architectures in parallel
- Compiling multiple services inside a large monorepo
- Building or testing LLMs inside of your container builds


## How can I start using larger builders?


Larger Docker image builders are available on our **Startup** and **Business** plans. If you're on one of those plans, you can visit your Project settings page and select your preferred builder size. Your choice will apply to all subsequent builds in that project.


[Choose your builder size from your Project Settings](https://depot.dev/images/bigger_builders_screenshot.webp)


## Pricing


Pricing for our larger builders scales with CPU count, charged per minute. The full breakdown of builder pricing is as follows:


Builder Size CPUs Memory Cost per Minute Multiplier


Default 16 32 GB $0.04 1x


Large 32 64 GB $0.08 2x


Extra Large 64 128 GB $0.16 4x


You only pay for the time your builds are running; we never charge you for startup or queue times and we bill by the minute but track per second.


A quick pricing example for **Large** and **Extra Large** builders, we normalize down to the default builder per-minute rate of $0.04/minute and then multiply your build time by the multiplier. For example, a 5-minute build on a 32 CPU builder is charged as follows: 5 x 2 x $0.04 = $0.40.


## Try it out and let us know what you think!


We're working on several enhancements behind the scenes to make your builds even faster! We're excited to see what your build times look like with these new larger builders. If you have any thoughts or feedback about this or any future features, feel free to reach out in our[Community Discord](https://discord.gg/MMPqYSgDCg) .


Billy Batista


Software Engineer at Depot
