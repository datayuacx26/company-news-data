---
schema_version: "1.0.0"
document_id: "fc537dcf7d0c66d44a4149f98fece20a122554c120abc875244d85531eedd031"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/multi-platform-docker-images-in-github-actions"
published_at: "2023-10-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:d869977b7364f1e34b409a4e6ad25abe80222704877e11a37b22fff847bb1751"
---

# How to build multi-platform Docker images in GitHub Actions

In this post, we will focus on building multi-platform Docker images, as well as Arm images, in GitHub Actions.


[Depot launches cloud builders for both Intel and Arm — allowing you to build multi-platform Docker images with native CPUs and instant caching across builds. No more slow emulation or saving/loading Docker layer cache over slow networks. Get Started →](https://depot.dev/start)


## Multi-platform images and Arm support


By default, Docker images are built for the architecture of the machine running the build. If you build an image on an Intel machine, the image will be built for Intel. If you build an image on an Arm machine, the image will be built for Arm.


If you want to build an image for a different architecture than the machine you are building on, you can specify the` --platform` flag during a Docker build. For example, if you are building on an Intel machine and want to build an Arm image, you can specify` --platform linux/arm64` .


```text
docker   build   --platform   linux/arm64   .
```


A Docker image can also be built for multiple architectures simultaneously. This produces what is often referred to as a multi-platform or multi-architecture image. If you want to build a multi-platform Docker image for both Intel and Arm, you can specify multiple platforms in the` --platform` flag.


```text
docker   build   --platform   linux/arm64,linux/amd64   .
```


A multi-platform Docker image build triggers two builds, one for each architecture, and produces a single image that supports both platforms. But, to build that image, one of the architectures must be emulated using[qemu emulation](https://www.qemu.org/) . If this is a multi-platform image built in CI, like GitHub Actions, the Arm portion (i.e.,` linux/arm64` ) will be emulated.


Alternatively, there is the option to configure` docker buildx build` to use multiple builders, one for each platform. This method removes the need to emulate the non-host machine architecture. But, in exchange, you have to run your own native builders. For more details, we have a blog post on[running your own builder instances](https://depot.dev/blog/building-arm-containers#option-3-running-your-own-builder-instances) .


## Building multi-platform Docker images in GitHub Actions


This section assumes you know the basics of building Docker images in GitHub Actions. If you are new to building Docker images in GitHub Actions, we have a[blog post](https://depot.dev/blog/docker-layer-caching-in-github-actions#building-docker-images-in-github-actions) that covers the basics of building Docker images in GitHub Actions.


To build a multi-platform Docker image in GitHub Actions, we must configure QEMU emulation and` buildx` in our workflow.


```text
name  :   Build multi-platform Docker image


on  :
push  : {}


jobs  :
build-with-docker  :
name  :   Build multi-platform Docker image
runs-on  :   ubuntu-20.04
steps  :
-   uses  :   actions/checkout@v3
-   uses  :   docker/setup-qemu-action@v3
-   uses  :   docker/setup-buildx-action@v3
-   uses  :   docker/build-push-action@v5
with  :
context  :   .
platforms  :   linux/amd64,linux/arm64
cache-from  :   type=gha
cache-to  :   type=gha,mode=max
```


Three steps in our workflow are worth noting:


1.


` docker/setup-qemu-action` configures QEMU emulation for the Arm portion of our multi-platform Docker image build. This is required for multi-platform Docker image builds in GitHub Actions, as the hosted runners are Intel machines.


2.


` docker/setup-buildx-action` configures` buildx` for our workflow. It's required for multi-platform Docker image builds in GitHub Actions that use` docker/build-push-action` .


3.


` docker/build-push-action` builds and pushes our Docker image. We specify the` platforms` argument to build our image for both Intel and Arm architectures. If we wanted to push our image to a registry, we could add an additional step above` docker/build-push-action` to login to our registry and then specify the` push` argument. We specify the` cache-from` and` cache-to` parameters to store the Docker layer cache via the GitHub Cache API.


```text
-   uses  :   docker/login-action@v3
with  :
username  :   ${{ secrets.DOCKERHUB_USERNAME }}
password  :   ${{ secrets.DOCKERHUB_TOKEN }}
-   uses  :   docker/build-push-action@v5
with  :
context  :   .
platforms  :   linux/amd64,linux/arm64
push  :   true
tags  :   <your-repo>:<your-tag>
cache-from  :   type=gha
cache-to  :   type=gha,mode=max
```


If you wanted to build a Docker image for only Arm, you would change the` platforms` argument to just` linux/arm64` but keep the rest of the workflow to use QEMU emulation.


This is a basic example of building multi-platform Docker images in GitHub Actions with QEMU emulation. It's functional, but it has limitations that are worth noting:


1. The Arm portion of the build is emulated using QEMU. We've seen up to 30x speedups when building on native Arm machines vs emulating Arm in CI via our benchmarks like[Temporal's multi-platform build](https://depot.dev/benchmark/temporal) .
2. The GitHub Cache API only supports a[maximum size of 10 GB](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy) for the entire repository, each architecture will have to share this 10 GB limit.
3. Loading and saving cache over networks is slow, meaning the loading and saving could negate any performance benefits of using the cached layers for simple image builds
4. The cache is locked to GitHub Actions and can't be used in other systems or on local machines


An alternative to emulation is to run your own[GitHub Action runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/using-self-hosted-runners-in-a-workflow) on Arm instances. This comes with a significant increase in infrastructure and complexity you must manage.


## A managed solution


We built Depot to eliminate the pain of emulation and other limitations above, not only in GitHub Actions but in all[CI providers](https://depot.dev/integrations) .


Depot is a remote container build service that runs an optimized version of BuildKit on native cloud instances for Intel and Arm. We manage your Docker layer cache on fast NVMe SSDs that make your cache instantly available across builds. The layer cache can be used from your CI build in GitHub Actions and your local machine when you use` depot build` .


Each build runs on a dedicated single-tenant BuildKit machine, using native Intel or Arm CPUs. Each instance includes 16 CPUs, 32GB memory, and the persistent layer cache can be expanded up to 500 GB.


The combination of faster machines, native CPUs, and instant caching across builds results in faster builds. We've seen up to 30x speedups when building on native Arm machines vs emulating Arm in CI via our benchmarks like[Temporal's multi-platform build](https://depot.dev/benchmark/temporal) or[Mastodons multi-platform build](https://depot.dev/benchmark/mastodon) .


Depot effectively provides on-demand access to one or more remote BuildKit builders and automatically routes each build platform to the machine best suited to build for that platform.


If you are interested in trying out Depot in your GitHub Actions workflow, check out our[GitHub Actions integration guide](https://depot.dev/integrations/github-actions) and[get started with Depot](https://depot.dev/start) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
