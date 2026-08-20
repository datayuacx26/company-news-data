---
schema_version: "1.0.0"
document_id: "0dd55c30a29894890a6293a182232a176030ec70bd943c094f7c6c6d2aafa3b0"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/build-autoscaling-now-generally-available"
published_at: "2025-07-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:b7d52e6615d20825a67c74aaa5041525151e1006b047344d3fb2ea3962115033"
---

# Now available: Build autoscaling for everyone

When we first launched Depot, our goal was to make Docker image builds exponentially faster. Why? Because we experienced the absolute drudgery of waiting for container builds locally and in CI. The modern day equivalent of watching paint dry because saving and loading layer cache over networks negated all performance benefits of caching, and building multi-platform images required emulation, bringing builds to a crawl.


So, we built the solution we had always wanted: a fast, shareable, and reliable container build service that could be used from any existing CI workflow or anywhere you were using` docker build` . Today, Depot's container build service is used by thousands of developers to build Docker images faster than ever before. **Saving engineering teams tens of thousands of hours in build time every week.**


[Your builds don't have to suck. Follow our docs to turn 20-minute nightmares into 2-minute wins. Read the docs →](https://depot.dev/docs)


We don't bullshit about benchmarks,[here is our benchmark for building PostHog's container images.](https://depot.dev/benchmark/posthog)


Depot benchmark for PostHog container builds


## What is the container build architecture?


Before we get into the details of how autoscaling works, it's worth understanding how Depot's container build service works **without** it. When you run a container build, Depot runs an optimized version of BuildKit to process the build and cache layers to a persistent NVMe drive.


Depot container build architecture without autoscaling


Behind the scenes, the flow looks like this:


1. You run` depot build` , which informs our control plane that you'd like to run a container build. We infer the architecture of the build based on the client architecture, or we look at the` --platform` flag if it is specified.
2. The control plane informs our provisioning system that a request for a container builder has been made for the specified platforms.
3. The provisioning system spins up one or more BuildKit builders to process the build. The number of builders is determined by the number of platforms in the build request (i.e., a multi-platform build will spin up a builder for each platform, such as` linux/amd64` ,` linux/arm64` , etc.).
4. The authentication details and IP of the builders are returned to the client, and the` depot build` command connects directly to the builder to run the build.


In this flow, each BuildKit builder is responsible for processing the build and caching layers to a persistent NVMe drive for the architecture it supports.


By default, these BuildKit builders can process multiple jobs concurrently on the same host. This is a feature of BuildKit that enables deduplication of work across builds that share similar steps and layers.


So in this model, multiple` depot build` commands can run concurrently on the same BuildKit builder, and the builder will process them in parallel. This is great for most use cases, but it does have a limitation: **the number of concurrent builds is limited by the resources of the single BuildKit builder** .


## But one limitation has remained


But one limitation has existed since the beginning: **container builds could only run on a single BuildKit builder** . Today, we are excited to announce the general availability of **container build autoscaling** , which removes that limitation, and also explain why you may not always want to use it.


### How does container build autoscaling work?


With container build autoscaling, we can now automatically scale out your container builds to multiple BuildKit builders based on the number of concurrent builds you want to process on a single builder. This means that if you have a large number of concurrent builds, Depot will automatically spin up additional BuildKit builders to process them in parallel.


Depot container build architecture with autoscaling


We've added a step 5 to our previous flow:


1. The control plane and provisioning system automatically scale out additional BuildKit builders based on the number of concurrent builds you want processed on a single builder. The authentication details and IPs of the additional builders are returned to the client, and the` depot build` command connects directly to all of the builders to run the build.


### When should I use container build autoscaling?


This is by far the most important question to ask because of the tradeoffs involved. Container build autoscaling is a powerful feature that can significantly speed up your container builds. Still, it does come with some tradeoffs, like cache cloning and losing the ability to deduplicate work across builds that share similar steps and layers.


That said, container build autoscaling is particularly useful when a single build can consume all of the resources of a single BuildKit builder, or you have a large number of concurrent builds that chew through all of the resources as well.


In these cases, we recommend starting with **sizing up your container builder** , which you can see the full sizes available on our[pricing page](https://depot.dev/pricing) . This allows you to run larger builds on a single builder without needing to scale out to multiple builders.


However, for instances where you have a large number of concurrent builds or a single build that consumes all the resources of a single builder, container build autoscaling is a great way to speed up your builds.


### How do I enable container build autoscaling?


To turn on container build autoscaling, you will need to navigate to your **Depot project settings** for the container build project you want to enable it for. From there, you can enable the autoscaling feature in the settings tab and specify the number of concurrent builds you want to process on a single builder.


Enabled horizontal autoscaling on a Depot project


Once enabled, all new builds will automatically scale out to multiple BuildKit builders based on the number of concurrent builds you specified. You can also adjust the number of concurrent builds at any time in the project settings.


## What about the layer cache?


Autoscaling does come with tradeoffs, and one of those is that the layer cache the additional builders operate on is a clone of the main builder's layer cache. This means that the additional builders operate on a copy of the layer cache, and that copy is **not written back to the main builder's layer cache** . This means that the additional builders will not be able to share layers with the main builder, and you will not be able to take advantage of the deduplication of work across builds that share similar steps and layers.


## Billing details


Container build autoscaling is available on **all plans** . Any cache clones created by the autoscaling feature are not persisted beyond the lifetime of the builder. This means that when the autoscaled builders are terminated, their layer cache clones are also deleted. Thus, cache clones do not count towards storage billing.


## What's next?


We're excited to make this feature generally available to everyone using Depot for container builds. We believe that this is going to help folks get even faster Docker image builds, and we can't wait to see the new use cases for build performance that this enables.


We're working on some additional features here to make this feature available on our Build API as well as add some additional logic around how the cache clones are managed.


If you have any questions or feedback about this feature, please reach out to us on[Discord](https://depot.dev/discord) and let us know.


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Gocache v2 for improved Golang build performance](https://depot.dev/blog/now-available-gocache-v2-faster-improved-golang-build-performance)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
