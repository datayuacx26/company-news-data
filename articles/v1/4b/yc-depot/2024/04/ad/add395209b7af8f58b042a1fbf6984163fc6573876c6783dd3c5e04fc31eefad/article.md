---
schema_version: "1.0.0"
document_id: "add395209b7af8f58b042a1fbf6984163fc6573876c6783dd3c5e04fc31eefad"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/rust-dockerfile-best-practices"
published_at: "2024-04-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:625bddb4a1bdcd6b163226864fca222e4f6b5781ed5e221e613c0753e9b86a25"
---

# Best practice Dockerfile for speedy Rust builds

Rust builds in Docker tend to be slow. This article shows how to drastically speed them up using Docker layer caching and more advanced techniques like sccache.


[Looking to speed up Rust Docker builds in CI? Sign up for a 7-day free trial of Depot today and use the steps in this tutorial to cut your Rust Docker build time by up to 40x. Get Started→](https://depot.dev/start)


Rust projects can be notoriously slow to compile. We've worked on projects that took over 40 minutes to build! It's actually the slowness of these builds that inspired Depot in the first place.


The secret to fast container image builds for Rust projects is crafting your Dockerfile to make good use of caching.[In our Rust documentation](https://depot.dev/docs/container-builds/optimal-dockerfiles/rust-dockerfile) , we provide a best-practice Dockerfile with various levels of caching applied.


In this tutorial, we'll create that Dockerfile step by step using an open-source Rust project as an example, and show three specific optimizations that you also can use to speed up your Rust builds.


## Setup


For this tutorial, we're building off the[open-source repository](https://github.com/TheAlgorithms/Rust) from *The Algorithms,* implementing various algorithms in Rust for educational purposes. We've forked this repository, made some minor modifications, and added a Dockerfile so that we could show how to optimize the build process step by step.


## The simple (but slow) way to build a Rust project


To start, let's take a look at an unoptimized Rust Dockerfile.


```text
FROM   rust:1.75


WORKDIR   /app
COPY   . .
RUN   cargo build
```


This Dockerfile uses a Rust v1.75 base image, then installs all libraries and builds all source code with` cargo build` . On our system, this unoptimized build comes in at 1m 4s, with most of this time spent pulling the Rust base image. The good news is that if you run the build again, it will finish almost instantly, as Docker has cached all intermediate build artifacts.


But what happens if we make a minor modification to the source code and rerun the build? After adding a comment in the source code, the build now takes around 34s on our setup. But why is this?


First, the base image` rust:1.75` is unchanged and already exists in the layer cache, so we don't need to repull it. But, we changed the source code and thus the` COPY . .` invalidates the cache, forcing the` cargo build` to happen again.


So the cargo build step has to reinstall **all** of the packages when the source code has changed. The reason is because the source code changes are happening above the` cargo build` step, so any source code change requires that step to be recomputed.


So that` RUN` statement will execute that command again (re-downloading and installing all packages) every time you make a change to the source code. This is extremely inefficient, because the unchanged artifacts from previous builds should be reused here to skip steps and speed up the build.


Let's see how we can create a more optimal Dockerfile for our Rust project.


## Optimization 1: Use cargo-chef to cache third-party dependencies


Below, we've added two more` FROM` statements to create a[multi-stage build](https://docs.docker.com/build/building/multi-stage/) . We've added separate stages to install[cargo-chef](https://github.com/LukeMathWalker/cargo-chef) , generate the cargo-chef recipe, and finally build the source code. The` base` stage only has to be rebuilt when a new version of cargo-chef is released, the` planner` stage only has to be rebuilt when any of the dependencies change, and only the` builder` stage is rebuilt whenever the source code changes.


```text
FROM   rust:1.75   as   base
RUN   cargo install cargo-chef --version ^0.1


FROM   base   AS   planner
WORKDIR   /app
COPY   . .
RUN   cargo chef prepare --recipe-path recipe.json


FROM   base   as   builder
WORKDIR   /app
COPY   --from=planner /app/recipe.json recipe.json
RUN   cargo chef cook --release --recipe-path recipe.json
COPY   . .
RUN   cargo build
```


Initially, the build will take longer due to the one-time penalty of downloading and installing extra packages for cargo-chef. However, subsequent builds with only source code changes take around 15 seconds because we're no longer installing packages and only rebuilding the changed source code.


This is **more than a 50% reduction** from the initial 34 seconds! However, even though this is a major optimization, there is still duplicate work: if **any** of the Rust dependencies changes, **all** dependencies need to be recompiled, which is not optimal.


## Optimization 2: Caching your Rust project compilations with sccache


Cargo-chef can drastically speed up Rust builds in Docker as it generates a recipe of all the dependencies in your project, much like a` requirements.txt` in Python. But compiling and downloading third-party dependencies is still considered one operation. If a single dependency changes, there will be a cache miss, and all dependencies will have to be re-downloaded and compiled, even though they haven't changed.


Enter[sccache](https://github.com/mozilla/sccache) , which caches individual compilation artifacts so that they can be reused at a more granular level during future compilations. This allows you to recompile individual dependencies only when needed, rather than everything or nothing.


Here's how an updated Dockerfile looks when using` sccache` :


```text
FROM   rust:1.75   AS   base
RUN   cargo install sccache --version ^0.7
RUN   cargo install cargo-chef --version ^0.1
ENV   RUSTC_WRAPPER=sccache SCCACHE_DIR=/sccache


FROM   base   AS   planner
WORKDIR   /app
COPY   . .
RUN   --mount=type=cache,target=$SCCACHE_DIR,sharing=locked \
cargo chef prepare --recipe-path recipe.json


FROM   base   as   builder
WORKDIR   /app
COPY   --from=planner /app/recipe.json recipe.json
RUN   --mount=type=cache,target=$SCCACHE_DIR,sharing=locked \
cargo chef cook --release --recipe-path recipe.json
COPY   . .
RUN   --mount=type=cache,target=$SCCACHE_DIR,sharing=locked \
cargo build
```


Now, subsequent builds take around 7 seconds. That's **more than a 75% reduction** from the 34 seconds in the unoptimized Dockerfile!


## Optimization 3: Caching the Cargo registry


At this point, you've gained the vast majority of the speedup from caching and reusing whatever you can in the build. But there's still one more thing you could reuse between builds: the Cargo registry directory. The Cargo registry stores the packages that have already been downloaded, and caching it removes the need to re-download those packages again unless they have changed.


To cache the Cargo registry, you can use an additional BuildKit cache mount that stores the Cargo registry directory. Here's what the resulting Dockerfile looks like:


```text
FROM   rust:1.75   AS   base
RUN   cargo install sccache --version ^0.7
RUN   cargo install cargo-chef --version ^0.1
ENV   RUSTC_WRAPPER=sccache SCCACHE_DIR=/sccache


FROM   base   AS   planner
WORKDIR   /app
COPY   . .
RUN   --mount=type=cache,target=/usr/local/cargo/registry \
--mount=type=cache,target=$SCCACHE_DIR,sharing=locked \
cargo chef prepare --recipe-path recipe.json


FROM   base   as   builder
WORKDIR   /app
COPY   --from=planner /app/recipe.json recipe.json
RUN   --mount=type=cache,target=/usr/local/cargo/registry \
--mount=type=cache,target=$SCCACHE_DIR,sharing=locked \
cargo chef cook --release --recipe-path recipe.json
COPY   . .
RUN   --mount=type=cache,target=/usr/local/cargo/registry \
--mount=type=cache,target=$SCCACHE_DIR,sharing=locked \
cargo build
```


You can also check out our post to see how to leverage these[BuildKit cache mounts in CI](https://depot.dev/blog/how-to-use-buildkit-cache-mounts-in-ci) builds, more generally.


## Pair cargo-chef, sccache, and Depot for even faster builds


Using cargo-chef and sccache provides drastic speedups to our Docker build by more effectively leveraging the Docker cache, but these tools can only offer speed improvements if the cache is persisted between builds.


Depot[persists cache](https://depot.dev/blog/depot-magic-explained#technique-2-persistent-cache-shared-between-local-and-ci-builds) to a fast NVMe SSD during the build, so that later builds can automatically make use of previous Docker layer cache without the large network transfer penalty of saving and loading cache over networks.


Depot's remote builders come with 16 vCPUs, 32 GB memory, a 50+ GB NVMe cache disk, and support both Intel and ARM architectures. To access these remote builders, you can use the Depot CLI, which serves as a drop-in replacement for` docker build` ,` docker buildx build` , and` docker buildx bake` commands.


In order to get the full benefits from the Rust Dockerfile that we covered in this article in your CI environment,[sign up](https://depot.dev/sign-up) for a 7-day free trial today and use Depot to accelerate your Docker builds.


You can also check out our[docs page for Rust builds](https://depot.dev/docs/container-builds/optimal-dockerfiles/rust-dockerfile) .


## FAQ


What does cargo-chef do to speed up Rust Docker builds?


Cargo-chef generates a recipe of all the dependencies in your Rust project, similar to a requirements.txt in Python. It allows you to separate the installation of third-party dependencies from building your source code. This means when you make source code changes, you only rebuild the changed source code instead of reinstalling all packages. Using cargo-chef can reduce build times by more than 50% when only source code changes.


How does sccache improve on cargo-chef for Rust builds?


While cargo-chef treats downloading and compiling third-party dependencies as one operation, sccache caches individual compilation artifacts. This means if a single dependency changes, sccache only recompiles that one dependency rather than recompiling everything. With cargo-chef alone, any dependency change causes all dependencies to be re-downloaded and compiled. Adding sccache on top of cargo-chef can reduce build times by more than 75% from an unoptimized Dockerfile.


Can I use sccache without cargo-chef in my Rust Dockerfile?


Yes, but you'll miss out on significant optimization. Cargo-chef handles the separation of dependency installation from source code building, which is crucial for leveraging Docker's layer cache effectively. Sccache then adds more granular caching on top of that. Using both together gives you the best results because they optimize different parts of the build process. Cargo-chef optimizes the Docker layer cache usage, while sccache optimizes individual compilation artifacts.


Do cargo-chef and sccache work in CI environments with ephemeral runners?


They work, but only if you have a way to persist the cache between builds. In CI environments like GitHub Actions, runners are ephemeral and don't keep state between builds. The BuildKit cache mounts that sccache relies on need to be persisted somewhere, otherwise you lose the cache after each build and these tools provide no benefit. You either need to save and load cache over the network (which is slow) or use a service like Depot that persists cache to fast NVMe SSDs automatically.


## Related posts


- [Guide to faster Rust builds in CI](https://depot.dev/blog/guide-to-faster-rust-builds-in-ci)
- [Fast Rust builds with sccache and GitHub Actions](https://depot.dev/blog/sccache-in-github-actions)
- [How to use cache mounts to speed up Docker builds](https://depot.dev/blog/how-to-use-cache-mount-to-speed-up-docker-builds)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
