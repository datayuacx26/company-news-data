---
schema_version: "1.0.0"
document_id: "71e3a6209e671093686dbc7632f57ad7f3ec864479bd1862e86b461d9c23856e"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/speed-up-docker-builds"
published_at: "2024-06-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:6e643af1410386053ee78c011048c04e5c91b9af91cf702ae9018fc31512ca54"
---

# How to speed up your Docker builds

Slow container image builds can be a massive pain for an engineering team. A developer might need to shift focus to a new task while waiting for their 10-20-minute local Docker build to complete.


Depending on which Docker features you rely on, slowness can be even more pronounced in CI. If you need to build multi-platform Docker images, for example, on most CI platforms you have to use QEMU emulation to build images for different architectures, and this is notoriously slow — builds that previously took 10-20 minutes on the native architecture can now take over an hour when emulated!


But building Docker images doesn't have to be this slow or inconvenient. At Depot, we frequently help customers speed up their Docker builds by up to 40x, and in this post we've collected a series of recommendations for achieving faster Docker builds by effectively using Docker's layer cache, actually using that cache in CI, reducing the size of your Docker images, and parallelizing build steps as well as the builds themselves.


[Use Depot to speed up your Docker image builds. Fast machines hosted in AWS, with dedicated builders for Intel and ARM, plus a superfast caching system, resulting in up to 40x faster Docker builds. Try free for 7 days. To get started, create a Depot account and then visit our docs.](https://depot.dev/start)


## How to benefit from the Docker layer cache


During a Docker build, Docker caches each command in a Dockerfile as a separate layer. Each layer depends on the previous layer, allowing the state of the build to be captured at each step.


Each layer in the cache depends on the layer that was produced in the previous build step in your[Dockerfile](https://depot.dev/blog/docker-build-image) . The layers that haven't changed can be reused in future builds. Once Docker detects that one layer has changed, it must entirely rebuild that layer and any subsequent layers that rely on it.


An example of when a cache layer can't be reused is shown below. When a new file is added to the source working directory, the` COPY ..` command can no longer be cached as its output will change. All layers prior to this can be read from the cache, and all layers after this need to be recomputed as they depend on previous layers.


### Optimize the order of your build steps


Given Docker's caching mechanism, the order of steps in a Dockerfile matters when it comes to caching effectiveness. In the above example, every time a file changes in the source working directory, Docker will recompute *all subsequent layers* , including the layer that runs` npm install` . However, reinstalling packages every time a new file is added to the project directory is excessive, as most file changes won't affect the package set installed by` npm install` .


A more efficient approach would be to execute` npm install` before copying the files into the target directory. In this example, we copy only the` package.json` file — the only one that will affect the output of` npm install` — and copy the rest of the working directory later, *after*` npm install` . This change leads to a much faster Docker build, as the output of the most lengthy command, whose result doesn't change often, can now be reused across builds even when project files change.


```text
FROM   node:20
WORKDIR   /app
COPY   package.json package-lock.json /app/
RUN   npm install
COPY   . .
RUN   npm build
```


### Use multiple stages to build in parallel


Rewriting your Dockerfile so that your[build steps run in parallel](https://depot.dev/blog/buildkit-in-depth#buildkit-speeds-up-docker-builds-with-parallelization) is a great way to save time during your Docker builds. You can replace a linear Dockerfile with one that has multiple stages, which are a way of building sections of your Dockerfile in parallel.


In the example below, let's assume the first three` RUN` commands can be run independently, but` command4` depends on` command3` , so it must wait for` command3` to complete before it can start. If that's the case, you can divide the build into three stages,` stage1` ,` stage2` , and` stage3` , which can all execute in parallel, reducing the total time required to complete the build.


## Optimize: make your Docker images smaller


One easy way to speed up your Docker builds is to make your Docker images *smaller* . Most Docker images are full of unnecessary bloat that can easily be removed.


The two main ways to reduce your Docker image size are:


- **Use a` .dockerignore` file** to exclude files and directories you know you don't need to copy into your container (for example, the` node_modules` directory in a Node.js project). For help analyzing your build context (in order to understand which files and directories could be excluded), you can use[dive](https://depot.dev/blog/reducing-image-size-with-dive) to view details about the size and files contained within each individual layer of an image, or Depot's[Build Insights feature](https://depot.dev/blog/build-context) to easily visualize everything in your build context.
- **Use a smaller base image,** like[Alpine](https://hub.docker.com/_/alpine/) . If you can deal with the tradeoffs of having fewer libraries and binaries available in your images, and perhaps installing a library or two explicitly when needed, using a smaller base image can significantly reduce the size of your final Docker image.


## Docker build speed bottlenecks in CI


All of the above strategies are relevant for speeding up your CI Docker builds as well as local builds, but for CI Docker builds in particular there are three additional speed bottlenecks.


### Problem 1: maximum cache size is too small for a Docker image


The external caches that most CI providers offer are too small. For example, in GitHub Actions, the cache size is limited to around[10 GB](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#usage-limits-and-eviction-policy) , and in BitBucket the cache is only[1 GB](https://support.atlassian.com/bitbucket-cloud/docs/cache-dependencies/#When-does-a-cache-get-saved) .


You could run out of space before you're able to cache all your layers. And if the cache can't fit in all your layers, the caching mechanism will keep deleting the layers that you actually need in your builds, eliminating the benefits of caching.


### Problem 2: slow network transfer speeds between the cache and the CI runner


To persist your Docker cache across CI builds, given the cache size limitation, many CI providers recommend that you use a[registry](https://docs.docker.com/build/cache/backends/registry/) as a Docker cache. You can do this by passing` cache-from`` and`` cache-to` options to the` docker build` command in your CI workflow configuration files, as well as specifying` type=registry` .


Storing the cache to a registry leads to a big slowdown when you need to download the cache from the registry service and transfer it across a network to the CI runner, and then reupload it later. The speed improvements you'd get from caching will be almost entirely wiped out by the network transfer penalty.


### Problem 3: multi-platform builds will have to use QEMU emulation


This is relevant if you need to build a single project for multiple platforms (Intel and ARM, for example). Any machine responsible for a Docker build must match the architecture of the image being built, or use[QEMU emulation](https://depot.dev/blog/multi-platform-docker-images-in-github-actions#building-multi-platform-docker-images-in-github-actions) . Using emulation can be extremely slow, and might cause your builds to take upward of an hour when they only take minutes on the native architecture. CI providers usually only allow one type of machine to do builds, so assuming you need to build to multiple architectures, at least one will need to use QEMU emulation.


## Our solution with Depot


Depot is a drop-in replacement for` docker build` and can be used locally or easily integrated with CI providers. Depot gives much faster build times than standard CI builds due to its architecture.


- **Improved network latency:** Depot builders persist cache directly to fast NVMe SSDs automatically. As a result, there is no network latency when accessing your cache across builds: it's instantly available.


- **Faster machines:** Depot builders run on much higher-spec machines than most CI providers offer: 16 vCPUs, 32 GB memory, and a persistent 50 GB+ NVMe cache.
- **Ability to reuse local cache:** The Depot cache is shared between local and CI builds. This means that even on your first CI build, you can gain speed benefits from the cache, as someone on your team will have likely recently run a local build and persisted changes to the shared cache that you can just reuse.
- **Zero QEMU emulation:** Depot allows for separate Depot builders for different architectures with separate caches running simultaneously (for example, one on Intel, and one on ARM), so there is no need to use QEMU emulation when[building multi-platform Docker images in parallel](https://depot.dev/blog/docker-arm) .
- **Extra speed improvements for GitHub Actions:** If you're a GitHub Actions user, you can now pair Depot with our new[GitHub Actions Runners](https://depot.dev/products/github-actions) feature. It's a one-line configuration change that gives even faster builds, due to Depot's GitHub Actions runners having faster compute, faster cache, and less network transfer loss. All this allows you to increase your caching speeds further than by using Depot alone.


To see some benchmarks for recent multi-architecture builds done in Depot, and how they compare to builds without Depot, check out benchmarks for projects such as[Mastodon](https://depot.dev/benchmark/mastodon) or[Temporal](https://depot.dev/benchmark/temporal) .


## Conclusion


To speed up your Docker builds, you need to maximize the use of the cache, try to run build stages in parallel, and run builds on native architectures rather than using QEMU virtualization.


Even with this taken care of, there are additional challenges to speeding up Docker builds in CI. For CI environments, Depot provides a purpose-made solution that runs Docker builds on fast machines, with fast cache storage, as well as support for native multi-platform builds.


Using Depot for your Docker builds can cut image build time of **1 hour or more** to **less than 3 minutes** through effective use of the layer cache combined with avoiding QEMU emulation. On top of this, your local builds can easily be configured to use the same remote cache, meaning your entire team can share the same cache.


[Sign up](https://depot.dev/sign-up) today for our free 7-day trial. As Depot is a drop-in replacement for` docker build` it's easy to use and requires minimal configuration.


## Related posts


- [BuildKit in depth: Docker's build engine explained](https://depot.dev/blog/buildkit-in-depth)
- [Best practice Dockerfile for speedy Rust builds](https://depot.dev/blog/rust-dockerfile-best-practices)
- [Building Docker Images in GitLab CI with Depot](https://depot.dev/blog/build-docker-images-in-gitlab-ci)
- [How to build multi-platform Docker images in GitHub Actions](https://depot.dev/blog/multi-platform-docker-images-in-github-actions)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
