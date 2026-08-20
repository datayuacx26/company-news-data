---
schema_version: "1.0.0"
document_id: "b7de92800eacde58a62ffbb9d22c81dde4d65e5eb8717b3872259c04fcff5c8a"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/bitbucket-pipelines-docker-build-limitations"
published_at: "2024-04-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:a6295e0030093292ac1278568e2851127590896ad8d1a2383f1d538e5ecf7f8b"
---

# Building and Caching Docker Images in Bitbucket Pipelines

This post explains how to build Docker images in Bitbucket Pipelines. First, we will take a look at the limitations of building Docker images in Bitbucket Pipelines, like lack of multi-platform support, limited caching, and the inability to use large portions of BuildKit. Then we will briefly introduce how Depot removes those limitations.


[Depot is a remote Docker image building service that works locally and integrates with any CI provider, such as Bitbucket Pipelines, GitHub Actions, and more. Depot provides cloud builders for both Intel and ARM, plus fast Docker layer caching on SSDs. Try using Depot's builders in your current Bitbucket Pipelines workflow to get a up to a 40x increase in speed! Get Started →](https://depot.dev/start)


## Building and Caching Docker Images in Bitbucket Pipelines


Bitbucket Pipelines is a CI/CD service built into Bitbucket. To build Docker images with Bitbucket Pipelines, we can add a` bitbucket-pipelines.yml` to the root of our repository with the following contents.


```text
pipelines  :
branches  :
master  :
-   step  :
name  :   Build with Docker
script  :
-   docker build .
services  :
-   docker
```


This builds a Docker image inside your pipeline by enabling the Docker service on the individual step. Note that you don't need to declare Docker as a service inside your Bitbucket pipeline because it's one of the default services. Underneath the hood, this is mounting the Docker CLI into the container running your pipeline, allowing you to run any Docker command you want inside your pipeline.


If you want to leverage more advanced features, like those found inside BuildKit, you can enable that by adding the` DOCKER_BUILDKIT=1` environment variable to your pipeline.


```text
pipelines  :
branches  :
master  :
-   step  :
name  :   Build with Docker
script  :
-   export DOCKER_BUILDKIT=1
-   docker build .
services  :
-   docker
```


With the new environment variable, the Docker build will use BuildKit. This workflow is functional and will work to at least build a Docker image inside of Bitbucket Pipelines.


Unfortunately, this Docker image build will be slow and painful due to several limitations of using Docker on Bitbucket Pipelines. Let's look at what these limitations are and what you can do about them.


## Limitations of building Docker images in Bitbucket Pipelines


On Bitbucket Pipelines, building Docker images is more limited by default than on many other CI/CD providers. The key difficulties are lack of multi-platform and buildx support, significant caching limitations that make the cache practically unusable for Docker image builds, and multiple disabled Docker features.


### Limitation 1: No multi-platform or buildx support


Bitbucket Pipelines doesn't support multi-platform builds. So, for example, you can't build an image for multiple CPU architectures simultaneously, like a multi-platform image for both Intel and ARM.


BuildKit supports multi-platform builds, and they are available in other CI providers like GitHub Actions, Google Cloud Build, and GitLab CI. They aren't necessarily performant in those providers, but they are supported.


In Bitbucket Pipelines, you can't even *attempt* a multi-platform build. Here is the` bitbucket-pipelines.yml` from before, but with the added buildx build for a multi-platform image to build an image for both Intel and ARM.


```text
pipelines  :
branches  :
master  :
-   step  :
name  :   Build with Docker
script  :
-   export DOCKER_BUILDKIT=1
-   docker buildx build --platform linux/amd64,linux/arm64 .
services  :
-   docker
```


But, if you attempted to run this multi-architecture image build in Bitbucket Pipelines, you'd get the following error:


```text
+   docker   buildx   build   --platform   linux/amd64,linux/arm64   .
unknown   flag:   --platform
```


Why? Because` buildx` is **completely disabled** in Bitbucket Pipelines, which means multi-platform builds are disabled and unavailable in Bitbucket Pipelines.


### Limitation 2:` RUN --mount=type=ssh` is disabled


When building images with Bitbucket Pipelines, you can't leverage the SSH mount inside your Dockerfile. You need to use the` --ssh` flag instead.


### Limitation 3: Caching limitations


You can enable a Docker cache in Bitbucket Pipeline by specifying the` cache` option in your config file.


```text
pipelines  :
branches  :
master  :
-   step  :
name  :   Build with Docker
script  :
-   docker build .
services  :
-   docker
caches  :
-   docker
```


The Docker cache allows you to leverage the Docker layer cache across builds. However, there are several limitations to this cache.


First, **the cache is limited to 1 GB in size** . Docker layers are often significantly larger than 1 GB, so you rarely get to cache all the layers of your build. This drastically slows Docker builds down because you can't reuse previous build results to skip repeated steps, as explained in[How to use Docker layer caching in GitHub Actions](https://depot.dev/blog/docker-layer-caching-in-github-actions) .


Second, the Docker cache in Bitbucket Pipelines **won't work when using BuildKit** , so you can't use this default cache when you enable BuildKit. However, you can work around this limitation by using a registry cache approach, as shown in[faster Docker image builds in Google Cloud Build](https://depot.dev/blog/docker-layer-caching-in-google-cloud-build) . During the build, specify the` --cache-from` flag to pull the cache from a registry.


```text
pipelines  :
branches  :
master  :
-   step  :
name  :   Build with Docker
script  :
-   export DOCKER_BUILDKIT=1
-   docker build --cache-from $IMAGE:latest .
services  :
-   docker
```


Registry caches tend to be slower than having a persistent cache across builds that is immediately available.


## Faster Docker image builds in Bitbucket Pipelines with Depot


These limitations don't prevent us from building a Docker image, but they do prevent us from building a Docker image quickly. Depot provides a drop-in replacement for` docker build` that allows you to work around these limitations.


Depot is a remote Docker image building service. When using Depot with Bitbucket Pipelines, the Docker image build is offloaded to optimized BuildKit builders running on much faster machines, with 16 vCPUs, 32GB memory, and 50GB+ NVME cache SSD storage, and with multiple natively-supported CPU architectures such as Intel and ARM.


To add Depot to your Bitbucket Pipelines, you need to install the Depot CLI. Here is an updated` bitbucket-pipelines.yml` file that does exactly that.


```text
pipelines  :
branches  :
master  :
-   step  :
name  :   Build Docker image with Depot
script  :
-   curl -L https://depot.dev/install-cli.sh | DEPOT_INSTALL_DIR=/usr/local/bin sh
-   depot build --platform linux/amd64,linux/arm64 .
services  :
-   docker
```


You can install Depot via curl and put it in the` /usr/local/bin` directory. Then, you can swap out` docker build` for` depot build` , as it's a drop-in replacement that takes all the same parameters.


### Overcoming limitation 1: multi-platform and buildx support


Depot not only has all the buildx features available, including multi-platform builds, but also has multiple CPU architectures available *natively* . As we explained in our[article about ARM and multi-architecture builds](https://depot.dev/blog/building-arm-containers) , by default Docker images are built for the CPU architecture matching that of the machine running the build. So building an image on an ARM machine will give you an image that's built for ARM, and building it on an Intel machine will give you one that's built for Intel.


It's also possible to build a Docker image for multiple architectures simultaneously: a multi-platform or multi-architecture image. For example, to build a multi-platform Docker image that supports both ARM and Intel, you can specify ARM and Intel in the` --platform` flag, as shown in the config example above.


A multi-platform Docker image build kicks off one build per architecture, resulting in a single image that works on both platforms. However, to build that image,[qemu](https://www.qemu.org/) emulation must be used to emulate one of the platforms. If the multi-platform image is being built in CI that supports the` --platform` flag, like GitHub Actions or other Bitbucket Pipelines alternatives, the ARM part (` linux/arm64` ) will be emulated.


Depot supports architectures such as Intel and ARM *natively* , meaning that builds will run on machines with Intel and ARM CPUs respectively. Running builds natively makes them much faster, with speedups reaching 40x (for example, an ARM image that takes 40 minutes to build on Intel in CI would take 1 minute with Depot).


To take advantage of native builds on Depot, you only need to specify the relevant architectures in your pipeline configuration. For example, the config section in **bold** is what you can use to build a multi-platform Docker image for both Intel and ARM on Bitbucket Pipelines:


```text
# bitbucket-pipelines.yml
pipelines  :
branches  :
master  :
-   step  :
name  :   Build Docker image with Depot
script  :
-   depot build --platform linux/amd64,linux/arm64 .
services  :
-   docker
```


### Overcoming limitation 2: availability of RUN --mount=type=ssh


This one is quite simple: Depot doesn't disable any Docker features, so everything that works for you locally will also work in Depot builds. You can't use` --mount-type=ssh` or BuildKit cache mounts like` RUN --mount=type=cache,target=/var/cache/apt` on Bitbucket Pipelines. But, with Depot, you get all of that functionality out of the box.


### Overcoming limitation 3: restricted caching


As we already explained, Bitbucket Pipelines Docker cache is limited to 1 GB, which is insufficient for many Docker image builds, and alternative solutions such as registry-based caching are slow. Depot, in contrast, uses fast SSDs to store your Docker layers and makes them available instantly for every Docker image build that you launch.


### Bonus: native BuildKit


By swapping` docker build` for` depot build` in your Bitbucket Pipeline, you get a complete native BuildKit environment for both Intel and ARM CPUs, and this gives you the possibility to use cool BuildKit features that further speed up your Docker image builds, such as[BuildKit cache mounts](https://depot.dev/blog/how-to-use-buildkit-cache-mounts-in-ci) for fine-grained caching.


## Try building Docker images on Bitbucket Pipelines with Depot


Building Docker images in Bitbucket Pipelines works for the initial use case but is almost never performant or flexible enough for a fast-moving enterprise team.


If you're looking for a long-term solution with Bitbucket Pipelines, try Depot. You can sign up for our[7-day free trial](https://depot.dev/sign-up) , and we're always available in our[Community Discord](https://discord.gg/MMPqYSgDCg) to answer any questions you may have.


## FAQ


Why can't I build multi-platform Docker images in Bitbucket Pipelines?


Bitbucket Pipelines has buildx completely disabled, so you can't use the` --platform` flag or attempt multi-platform builds. If you try to run` docker buildx build --platform linux/amd64,linux/arm64` , you'll get an "unknown flag: --platform" error. It's a hard limitation of how Bitbucket implements the Docker service.


What's the Docker cache size limit in Bitbucket Pipelines?


The Docker cache in Bitbucket Pipelines is limited to 1 GB. Docker layers are often significantly larger than 1 GB, so you rarely get to cache all the layers of your build. Additionally, this cache won't work when using BuildKit, forcing you to use slower registry-based caching approaches instead.


How do I set up Depot authentication in my Bitbucket Pipeline?


You'll need a Depot project token from your Depot dashboard. Add it as a secured environment variable (like` DEPOT_TOKEN` ) in your Bitbucket repository settings. The Depot CLI will automatically use this token when running` depot build` in your pipeline.


How much faster are Docker builds with Depot compared to Bitbucket Pipelines?


Depot can provide up to a 40x speedup for Docker builds. An ARM image that takes 40 minutes to build with emulation in standard CI takes around 1 minute with Depot's native ARM builders. You get faster machines (16 vCPUs, 32GB memory), native architecture support, and persistent SSD caching, which all contribute to significantly faster builds.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
