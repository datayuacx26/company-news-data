---
schema_version: "1.0.0"
document_id: "94c65c3f094570747cd591013e01a05395f3b233fad40f68e963ef5ed8e9e677"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/fly-builds-powered-by-depot"
published_at: "2024-07-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:b6e7dcf2b25bbd673a74795ce35a2fda7c2a93201c0ce2fd539d6971289a6694"
---

# Fly builds, now with Depot

We're excited to announce that you can now **build your[Fly.io applications](https://fly.io/) with Depot** , directly from` flyctl` ! This partnership with Fly brings the speed and performance of Depot's container image builds to the Fly platform, making it the fastest way to build and deploy your Fly apps.


The beta integration is available to use now, and you don't even need a Depot account to get started. We're excited to see what you build!


## How to get started


As of` flyctl` version 0.2.102, you can use the` --depot` flag to enable Depot as the builder when deploying your application:


```text
flyctl   deploy   --depot
```


Alternatively, if you are running Fly machines directly, you can use the` --build-depot` flag to route the build through Depot as well:


```text
flyctl   machines   run   --build-depot
```


That's it!` flyctl` connects to a Depot builder running within Fly's infrastructure, and you can watch the build progress in your terminal. Once the build is complete, your image is automatically pushed into Fly's registry and then deployed, ready to serve traffic.


## Why Depot for Fly builds?


Depot is the fastest way to build container images — for the past two years, we've been focused on optimizing and accelerating every step of the build process. We run a specialized build engine based on BuildKit, the technology that powers` docker build` . Our builders are highly optimized for the build server use-case, taking full advantage of the host resources to build and push images as efficiently as possible. Depot builders automatically manage layer cache, ensuring that all builds are perfectly incremental.


Fly provides a compute platform for running your applications. Fly transforms container images into Firecracker VMs, running on their own hardware, that launch instantly in any of their 35 regions worldwide. This compute platform includes global networking, with global anycast load-balancing and zero-configuration private networking, block and object storage, GPUs, and everything else you need to run your applications at scale.


Depot's fast builds and Fly's global infrastructure are a perfect match. With Depot-powered Fly builds and Fly's lightning-quick VMs, you can deploy your applications faster than ever before.


## How it works


When you run` flyctl deploy --depot` ,` flyctl` connects directly to a Depot builder running within Fly's infrastructure. That builder processes the build and then pushes the resulting image directly to Fly's internal registry. Once the image is in the registry, Fly deploys it to their infrastructure, and you're ready to go.


The Depot builders are running as[Fly machines](https://fly.io/docs/machines/) . Each Depot builder is single-tenant, meaning that your Fly organization's builds are isolated and use the full performance of the underlying hardware to accelerate your builds. All cache data is persisted to Fly volumes, so future builds can instantly reuse the results of previous builds.


For this initial beta, Depot builders can be launched into 18 of Fly's global regions. This means that your builds can be processed geographically near the machine or CI provider that executes the` flyctl deploy --depot` command, speeding up the transfer of your application files into Fly's infrastructure.


## What's next?


This is just the initial release, and we have several improvements planned for the future, including:


-


**Integration with[Tigris Global Object Storage](https://fly.io/docs/reference/tigris/)** - Tigris is an S3-compatible object storage service, running on Fly. Integrating Tigris with Depot's caching infrastructure will allow build cache to be shared across different Fly regions, allowing us to dynamically launch builders in different Fly regions for each build request.


-


**Advanced build telemetry and debugging** - Depot builders provide deep instrumentation into each step of the container build process, making it easier to understand what's happening during a build and to debug any failures. We plan to further integrate this data with Fly, to provide a richer build experience for your Fly builds.


-


**GPU support** - Depot builders can access GPUs during the container build, allowing you to build images that perform validation checks or run tests that require GPU acceleration. And[Fly has GPUs](https://fly.io/docs/gpus/) , so we plan to explore integrating with Fly's GPU infrastructure.


We're excited to see what you build with Depot and Fly. If you have any feedback or questions, please reach out to us on[Twitter](https://twitter.com/depotdev) or[Discord](https://depot.dev/discord) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
