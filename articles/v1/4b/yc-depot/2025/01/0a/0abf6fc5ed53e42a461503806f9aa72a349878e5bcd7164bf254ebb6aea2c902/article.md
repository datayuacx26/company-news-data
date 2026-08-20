---
schema_version: "1.0.0"
document_id: "0abf6fc5ed53e42a461503806f9aa72a349878e5bcd7164bf254ebb6aea2c902"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/introducing-depot-cache"
published_at: "2025-01-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:9f458cefe7579bfdc0c0e1ee25bd37c606794a4975b6b35502c284fcbefd4a91"
---

# Introducing Depot Cache

We are kicking off the New Year with our latest product launch, Depot Cache!


Depot Cache is a remote cache service that allows you to leverage our high-performance remote cache architecture to speed up builds for Bazel, Go, Gradle, Turborepo, sccache, and Pants. We've seen speed-ups from 2x to 20x faster when using Depot Cache with one of the supported tools.


## Remote cache speeds up builds


We started building our core cache architecture back in 2022 with our original beta release of Depot. Back then, we were focused on speeding up Docker image builds, because we'd experienced first-hand the specific pain point of slow builds, often caused by slow networks and slow storage when it came to saving & loading the Docker layer cache. So, we built a remote cache orchestration service to automatically cache Docker layers and make them instantly available across builds.


Faster remote caching has been the core of our architecture since the beginning, and it's allowed us to speed up container image builds by up to 40x through making it easy to share cache across teams and CI/CD pipelines.


But Docker image builds aren't the only thing that can benefit from faster remote caching.


## Depot Cache


With Depot Cache, we are bringing that same performance and instant cache sharing to more build tools like Bazel, Go, Gradle, Turborepo, sccache, and Pants.


We've taken what we learned from building our[accelerated Docker build service](https://depot.dev/products/container-builds) to create a high-performance cache that is globally distributed and instantly available to all of your builds.


The magic of remote caching is the ability to save granular build outputs and test results and reuse them in any future build. As an API service, that cache can be accessed from anywhere, be that from any CI provider or any development machine.


Here are a few of the key features of Depot Cache:


1.


**Built for speed.** We are relentless about build performance. Depot Cache is optimized to make any build from the supported tools as fast as possible. Depending on which tool you currently use for caching, you'll see from 2x to 20x faster build times.


2.


**Globally distributed.** When you use it for your local builds, we retrieve the cache from the cache edge closest to you. When you use it in your CI/CD pipeline, we retrieve the cache from the closest cache edge to your build machine. This means that the cache is always close to you, and your builds are always fast.


3.


**Automatically integrated with Depot GitHub Actions runners.** When you use our faster[GitHub Actions runners](https://depot.dev/products/github-actions) we automatically configure remote caching in your runner when we detect that you're using one of the supported tools. So your builds just get faster without any extra work.


4.


**Available for local builds.** You can use Depot Cache for your local builds as well. This is great for sharing cache across your team or sharing the build cache from your CI environment. Configure your supported tool to point to Depot, and you're all set. We have instructions for all of the supported tools in our[Depot Cache documentation](https://depot.dev/docs/cache/overview) .


## Pricing details


All Depot plans include a block of storage that covers Depot Cache, Depot Ephemeral Registry, GitHub Actions cache, and Docker layer cache. Once you've used up your included storage, you'll be charged for the additional storage used at **$0.20/GB/month.**


By default, cache retention is **14 days** . We will automatically delete any cache that has not been used after two weeks. We are also actively working on custom cache retention policies that you can apply to all types of cache in Depot. We're aiming to launch that capability in early February.


What about data transfer? We don't charge for data transfer. We want you to use our cache to speed up your builds as much as possible. So we don't charge for data transfer in or out of the cache.


## What's next?


We're excited to take another step toward our future goal of making *all builds* exponentially faster. If there is a specific tool that you'd like us to support, hop into our[Discord](https://discord.gg/MMPqYSgDCg) and let us know!


If you're new to Depot, thanks for checking us out! You can get started with Depot Cache by[signing up for a free trial](https://depot.dev/start) (no credit card required).


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
