---
schema_version: "1.0.0"
document_id: "41bbe3f3d13d9f17f9531ed0758a00a919f499fd1a60d123df006d07f742a09c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-to-use-buildkit-cache-mounts-in-ci"
published_at: "2022-12-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:200705241e667788eb7ea1857f30f688f76620a584aebddf10ca4e27d1b5a249"
---

# How to use BuildKit cache mounts in CI providers

Making Docker image builds fast requires using as many previous build results as possible. The most common way to do this is to focus on the Docker layer cache, as we did in our blog post on[fast image builds in GitHub Actions](https://depot.dev/blog/docker-layer-caching-in-github-actions) .


But there is another cache we can leverage inside a` Dockerfile` to squeeze out even more performance from our image builds. It's often called the` RUN` cache, but you will also see it referred to as BuildKit cache mounts.


## What are BuildKit cache mounts?


It's helpful to think of BuildKit cache mounts as a specialized cache that allows us to do more fine-grained caching between builds. Here is a sample` Dockerfile` that shows how to use them:


```text
FROM   ubuntu
# Allow ubuntu to cache package downloads
RUN   rm -f /etc/apt/apt.conf.d/docker-clean
RUN   \
--mount=type=cache,target=/var/cache/apt \
apt update && apt-get --no-install-recommends install -y gcc
```


This special cache mount inside a build is helpful when the outer Docker layer cache has determined that this specific layer needs rebuilding. Why? Because the contents of the` target` directory get preserved across builds when this layer gets rebuilt. In the example above, the` apt` cache gets stored in` /var/cache/apt` . With that cache, we can avoid updating the entire` apt` package list and only fetch the ones that have changed.


So we have an outer Docker layer cache that determines if a given layer in an image build needs rebuilding. Then we have a fine-grained` RUN` cache we can leverage when the layer needs to be rebuilt.


## How much of a difference do they make?


So how much of a performance boost can BuildKit cache mounts give our image builds? We can get an idea of that using our example from above. The first time we run a build of that image, we see results similar to the ones below.


```text
[+] Building 12.0s (  7/7  ) FINISHED
=  > [internal] load build definition from Dockerfile-ex                                                                                                                                              0.0s
=  > =  >   transferring   dockerfile:   270B                                                                                                                                                                   0.0s
=  > [internal] load .dockerignore                                                                                                                                                                    0.0s
=  > =  >   transferring   context:   116B                                                                                                                                                                      0.0s
=  > [internal] load metadata   for   docker.io/library/ubuntu:latest                                                                                                                                     1.8s
=  > [stage-0   1/3]   FROM   docker.io/library/ubuntu@sha256:4b1d0c4a2d2aaf63b37111f34eb9fa89fa1bf53dd6e4ca954d47caebca4005c2                                                                                1.3s
=  > =  >   resolve   docker.io/library/ubuntu@sha256:4b1d0c4a2d2aaf63b37111f34eb9fa89fa1bf53dd6e4ca954d47caebca4005c2                                                                                        0.0s
=  > =  >   sha256:0509fae36eb0656f8bdb23f8ae64100d893bcea2563e97468d337e04d2d0410b   28.38MB   /   28.38MB                                                                                                       0.5s
=  > =  >   sha256:4b1d0c4a2d2aaf63b37111f34eb9fa89fa1bf53dd6e4ca954d47caebca4005c2   1.42kB   /   1.42kB                                                                                                         0.0s
=  > =  >   sha256:41130130e6846dabaa4cb2a0571b8ee7b55c22d15a843c4ac03fde6cb96bfe45   529B   /   529B                                                                                                             0.0s
=  > =  >   sha256:3c2df5585507842f5cab185f8ad3e26dc1d8c4f6d09e30117af844dfa953f676   1.48kB   /   1.48kB                                                                                                         0.0s
=  > =  >   extracting   sha256:0509fae36eb0656f8bdb23f8ae64100d893bcea2563e97468d337e04d2d0410b                                                                                                              0.7s
=  > [stage-0   2/3]   RUN   rm   -f   /etc/apt/apt.conf.d/docker-clean                                                                                                                                           0.2s
=  > [stage-0   3/3]   RUN       --mount=type=cache,target=/var/cache/apt       apt   update   &&   apt-get   --no-install-recommends   install   -y   gcc                                                                    8.2s
=  >   exporting   to   image                                                                                                                                                                                 0.3s
=  > =  >   exporting   layers                                                                                                                                                                                0.3s
=  > =  >   writing   image   sha256:5127068f27a0845f9d889f4a2428dbd0f2e2061b4d3f74d5bfbd5b6ba4bda24b                                                                                                           0.0s
```


The first build took ~12 seconds to finish, and the` RUN` statement where we have our cache mount accounted for 8.2 seconds. So what happens if we install another package like` curl` in addition to` gcc` ?


```text
FROM   ubuntu
# Allow ubuntu to cache package downloads
RUN   rm -f /etc/apt/apt.conf.d/docker-clean
RUN   \
--mount=type=cache,target=/var/cache/apt \
apt update && apt-get --no-install-recommends install -y gcc curl
```


The Docker layer cache will detect that we have changed our` RUN` statement and thus trigger the rebuild of that entire layer.


```text
[+] Building 5.0s (  7/7  ) FINISHED
=  > =  >   transferring   dockerfile:   275B                                                                                                                                                                   0.0s
=  > =  >   transferring   context:   34B                                                                                                                                                                       0.0s
=  > [internal] load metadata   for   docker.io/library/ubuntu:latest                                                                                                                                     0.4s
=  > [stage-0   1/3]   FROM   docker.io/library/ubuntu@sha256:4b1d0c4a2d2aaf63b37111f34eb9fa89fa1bf53dd6e4ca954d47caebca4005c2                                                                                0.0s
=  >   CACHED   [stage-0   2/3]   RUN   rm   -f   /etc/apt/apt.conf.d/docker-clean                                                                                                                                    0.0s
=  > [stage-0   3/3]   RUN       --mount=type=cache,target=/var/cache/apt       apt   update   &&   apt-get   --no-install-recommends   install   -y   gcc   curl                                                               4.3s
=  > =  >   writing   image   sha256:6550c3e629be8f9d61d9b62a41f08fc86ff791060500fc2db2e949f7a99f837f                                                                                                           0.0s
```


After running the build, we see that the build took 5 seconds to finish and the` RUN` statement with our cache mount took 4.3 seconds. So caching the` apt` packages via a BuildKit cache mount makes our` RUN` statement **twice as fast** when we have to rebuild the layer.


The power of BuildKit cache mounts and their performance improvements can vary depending on what we're caching. But in general, they provide a fine-grained approach for preserving` RUN` level results across builds where you want to avoid entirely rebuilding the layer when it is invalidated.


## Limitations of BuildKit cache mounts in CI providers


BuildKit doesn't support saving or loading cache mounts, so they can't be persisted across builds in CI providers. However, there is an[open issue](https://github.com/moby/buildkit/issues/1512) to allow changing the location of cache mounts so they can be exported and reused across builds.


## Depot supports cache mounts out of the box


Most CI runners are ephemeral and don't come with persistent volumes, so you are usually left with running your own image builders with a persistent volume if you want to use BuildKit cache mounts in your` Dockerfile` . But, with Depot, you get them for free 🎉


We run remote image builders on cloud VMs for both Intel and Arm architectures. Each builder has its own 50GB SSD cache that is persisted across builds automatically. Our builders are also ephemeral, but unlike generic CI providers, we persist your cache across builds on SSD volumes without any additional saving/loading of cache needed.


In practice, this means that **you can use BuildKit cache mounts in your generic CI providers** like GitHub Actions, CircleCI, etc., by routing your image build to remote Depot builders. Our[depot](https://github.com/depot/cli) CLI is a drop-in replacement for` docker build` , so getting faster builds with all of the BuildKit features (like cache mounts) is a one-line code change in your existing CI workflow.


## Conclusion


A fine-grain cache is a powerful feature for squeezing out more performance from Docker image builds. It allows us to avoid rebuilding expensive layers in their entirety because of small changes. Instead, we can use portions of precomputed work even if the Docker layer cache invalidates the layer. As we saw, this can make rebuilding a given layer at least twice as fast (even faster for more expensive operations).


Unfortunately, they don't work in CI providers like GitHub Actions, CircleCI, and Travis CI, where you need a persistent volume to store cache mounts across builds.


But they work right out of the box with Depot because of our remote builder infrastructure and fast SSD caches that are automatically shared across builds. You can drop Depot into your existing CI provider by installing our[CLI](https://github.com/depot/cli) and route your builds to our remote BuildKit builders via` depot build` instead of` docker build` .


Sign up today on our free tier and try things out in your existing workflows to see if we can make your image builds faster:[https://depot.dev/sign-up](https://depot.dev/sign-up) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
