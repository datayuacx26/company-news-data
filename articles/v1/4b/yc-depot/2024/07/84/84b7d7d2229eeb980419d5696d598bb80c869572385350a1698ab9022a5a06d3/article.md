---
schema_version: "1.0.0"
document_id: "84b7d7d2229eeb980419d5696d598bb80c869572385350a1698ab9022a5a06d3"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/docker-layer-caching-in-github-actions"
published_at: "2024-07-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:ac7d019049eb2f4d6ccdffe1f211d6840bbbc199fd71cea9dff9a81404f66e2a"
---

# How to use Docker layer caching in GitHub Actions

One of the keys to building a Docker image quickly is making use of the layer cache as frequently as possible. More cache hits means faster build times. Here we show how you can harness the power of Docker's layer cache when working in GitHub Actions.


[Build your Docker images up to 40x faster with Depot by leveraging automatic persistent layer caching across builds & native multi-platform image builds for Intel & Arm. Try Depot free for 7 days (no credit card required) →](https://depot.dev/start)


## What is the Docker layer cache?


The Docker layer cache is responsible for caching a series of Docker layers, stacked on top of each other. A Docker layer is the output of running a step in your Dockerfile (like` RUN` ,` ADD` ,` COPY` , and so on).


When building a Docker image, you typically start with a base image, and then run a series of steps defined in your Dockerfile that add customization to the base image.


Dockerfiles ensure that your build steps are run in a particular order. Each step outputs a layer, which is used as an input to the next step. Every time Docker runs a build, it reuses the unchanged layers, but if one layer has changed, then that layer and all subsequent layers that depend on it need to be rebuilt. Layers at the top of a Dockerfile will often remain unchanged, whereas layers at the bottom will change more often.


In the example below, a file in the source working directory has changed, meaning that the` COPY . .` command has produced a slightly different output layer. As this layer has changed, it can no longer be reused — and neither can any subsequent layers.


The benefit of Docker layer caching is it allows you to avoid wasting time rebuilding layers that are exactly the same as those from your previous build, which will make your overall Docker image build faster.


For a deeper dive into how the different cache layers interact with each other, see our[Fast Dockerfiles: theory and practice](https://depot.dev/blog/fast-dockerfiles-theory-and-practice) post.


## How to use Docker layer caching in GitHub Actions


By default, GitHub Actions doesn't persist the Docker layer cache across builds, because each Docker build is executed on a new, ephemeral GitHub Actions runner. Consequently, each build starts with a fresh, empty cache.


By making some simple changes to your GitHub Actions workflow, it's possible to get around this issue and persist the cache between builds. The main change required is to export the layer cache off the ephemeral runner. In this guide, we'll explore two ways to do this: using the GitHub Actions cache exporter and using Depot.


### Using the GitHub Actions cache exporter


The[GitHub Actions cache exporter](https://docs.docker.com/build/cache/backends/gha/) is an experimental cache exporter for the[GitHub Actions cache](https://depot.dev/blog/docker-layer-caching-in-github-actions#docker-layer-caching-in-github-actions) provided by buildx and BuildKit. The exporter orchestrates the storage of layer cache directly via the GitHub Cache API. When a new build is initiated, the cache can be restored via the GitHub Cache API so that your subsequent build can reuse previous build results in the layer cache.


To set up Docker layer caching using the GitHub Actions cache exporter, you'll need to set up a standard GitHub Actions workflow that builds a Docker image, and then add the GitHub Actions cache exporter to this.


First, create a GitHub Actions workflow file called` ci.yml` inside the` .github/workflows` directory at the root of your repository. Below, we show a typical workflow file for building a Docker image, highlighting the changes you'll need to make to add the GitHub Actions cache exporter to your workflow.


Adding the GitHub Actions cache exporter is easy: update the` build-push-action` step in your workflow by adding` cache-from` and` cache-to` arguments, and set the cache type to` gha` . This will export the cache to object storage via the GitHub Cache API.


```text
name  :   Build Docker image


on  :
push  : {}


jobs  :
build  :
runs-on  :   ubuntu-20.04
steps  :
-   name  :   Checkout repo
uses  :   actions/checkout@v3
-   name  :   Set up Docker CLI
uses  :   docker/setup-buildx-action@v3
-   name  :   Build and push Docker images
uses  :   docker/build-push-action@v5
with  :
context  :   .
-   cache-from  :   type=gha
-   cache-to  :   type=gha,mode=max`
```


If you commit your new` ci.yml` file, you should see a Docker build completed via GitHub Actions.


By adding the` cache-from` and` cache-to` lines for remote cache saving and loading, you can reuse your Docker layers from previous builds — as we see with the` CACHED` hits below.


Although the GitHub Actions cache exporter solves the problem of being able to reuse the cache across builds, it introduces some new problems:


**Network transfer time negates any performance benefits:** Loading and saving the cache is network bound, meaning it often negates any performance benefits of using the cached layers, particularly for simple image builds.


**10 GB cache size limit:** The GitHub Actions exporter relies on the GitHub Actions Cache API, which only supports a[cache size of up to 10 GB](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy) for your entire repository. This is a problem for more complex images that need a larger cache.


**The cache can't be shared:** The cache is locked to GitHub Actions and can't be used in other systems or on local machines.


### A faster alternative: using Depot with GitHub Actions


We built Depot to eliminate the limitations above, not only in GitHub Actions but in all CI providers.


Depot is a build acceleration platform that started with making Docker image builds up to 40x faster. We do it by managing a fleet of optimized remote BuildKit builders that support both x86 and ARM architecture natively without the need for emulation during your Docker image build. Each builder comes with 16 CPUs, 32 GB of memory, and a persistent NVMe cache disk that is up to 500 GB.


We persist your Docker layer cache directly to fast NVMe storage backed by Ceph. So you never have to think about saving or loading your layer cache over networks; it's just immediately available across builds.


Another benefit of Depot is that, unlike the GitHub Actions cache exporter, Depot's cache isn't tied to GitHub Actions. This means it can be shared with other CI/CD systems or used on your local machine.


#### How to use Depot for GitHub Actions Docker layer caching


1.


You need to[configure a project in Depot](https://depot.dev/docs/container-builds/quickstart#creating-a-project) before getting started. A project is a cache namespace that provides a way to isolate separate Docker layer caches.


2.


Switch out` docker/setup-buildx-action` and` docker/build-push-action` for Depot's own versions:` depot/setup-action` and` depot/build-push-action` .


3.


Set up permissions to allow Depot to build your projects. If you're using OIDC authentication, set the` permissions` block as it's shown below. If not, you need to pass your Depot project token in the` with` block of your configuration file.


Below, we show a typical GitHub Actions YAML workflow configuration file with the above steps added to it:


```text
name: Build Docker image


on:
push: {}


jobs:
build:
runs-on: ubuntu-20.04
+     # Set permissions if you're using OIDC authentication
+     permissions:
+       contents: read
+       id-token: write
steps:
- name: Checkout repo
uses: actions/checkout@v3


-  	- name: Set up Docker CLI
-     uses: docker/setup-buildx-action@v3


+   - name: Set up Depot CLI
+     uses: depot/setup-action@v1


- name: Build and push Docker images
-     uses: docker/build-push-action@v5
+  	  uses: depot/build-push-action@v1
with:
context: .
+       # Pass project token
+       project: ${{ secrets.DEPOT_PROJECT_ID }}
+       # OR pass user access token instead
+       # (if you're not using OIDC authentication)
+       token: ${{ secrets.DEPOT_TOKEN }}
```


For more examples of how to configure Depot in your GitHub Actions workflow for different types of builds like building multi-platform images or pushing to ECR, see our[GitHub Actions integration guide](https://depot.dev/docs/container-builds/integrations/github-actions) .


## Faster GitHub Actions caching with Depot


Depot's combination of using a faster cache and running builds on bigger and faster machines results in up to 40x faster Docker builds.


In addition, if you use Depot to speed up your Docker builds in GitHub Actions, you don't need to think about saving and loading the cache, as we persist it for you across builds automatically. The cached layers are available instantly during builds, with no need to save or load cached layers from the network, saving lots of time in CI builds.


The Docker layer cache is even shared with anyone who has access to the project, so a developer who runs a build locally can just reuse the cached layers that CI computed.


If you're interested in trying out Depot in your GitHub Actions workflow, you can[try it out now with our 7-day free trial.](https://depot.dev/sign-up)


**Now also available** :[Depot-hosted GitHub Actions runners](https://depot.dev/blog/depot-github-actions-runners) . 30% faster compute than GitHub Actions' default runners, with 10x the cache performance, at half the cost.


## FAQ


How do I enable Docker layer caching in GitHub Actions?


Add` cache-from` and` cache-to` arguments to your` build-push-action` step and set the cache type to` gha` . This exports your layer cache to GitHub's Cache API so it can be reused across builds. However, the GitHub Actions cache has a 10 GB limit per repository and network transfer time often negates performance benefits, especially for simple builds.


What is Docker layer cache and why does it matter?


The Docker layer cache stores the output of each step in your Dockerfile (like RUN, COPY, and ADD commands). When you rebuild an image, Docker reuses unchanged layers instead of rebuilding them, which makes your builds faster. If a layer changes, that layer and all subsequent layers need to be rebuilt. By default, GitHub Actions doesn't persist this cache between builds because runners are ephemeral.


Why isn't the GitHub Actions cache speeding up my Docker builds?


Loading and saving cache over the network is often slower than the time saved by reusing cached layers, particularly for simple images. The 10 GB repository cache limit fills quickly when storing full layer caches, and the cache can't be shared outside GitHub Actions. For more complex builds or larger caches, you need a solution that persists cache to fast local storage instead of transferring it over the network.


Can I use the same Docker layer cache across different CI providers?


Not with the GitHub Actions cache exporter, which locks your cache to GitHub Actions. However, tools like Depot persist your layer cache to fast NVMe storage that's accessible from any CI provider or your local machine. This means you can reuse the same cache whether you're building in GitHub Actions, CircleCI, Jenkins, or on your laptop.


## Related posts


- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)
- [The ultimate guide to Docker build cache](https://depot.dev/blog/ultimate-guide-to-docker-build-cache)
- [We reverse-engineered the GitHub Actions cache so you don't have to](https://depot.dev/blog/github-actions-cache)
- [How to leverage GitHub Actions matrix strategy](https://depot.dev/blog/github-actions-matrix-strategy)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
