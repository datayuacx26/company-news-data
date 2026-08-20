---
schema_version: "1.0.0"
document_id: "7ec7385c4f65406e9a3db4d6b4a5d13e140454c6bc751723bce02eb1399c1e9f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-to-use-cache-mount-to-speed-up-docker-builds"
published_at: "2025-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:243f68846e2d3105af414e93dd7b1333b6ed3783dfab0bf35050aac2d897d82b"
---

# How to use cache mounts to speed up Docker builds

Caching build steps is one of the most reliable ways to speed up Docker builds, but not all caching works the same way. Many teams start with` --cache-from` to reuse build layers by restoring your layer cache from a local filesystem, or most often from a container registry. It works well in theory, but in most CI systems the cache gets wiped between runs making the local filesystem solution not possible and the registry cache tends to be slow because of the network latency of transferring large layer caches.


Depot solves this problem by giving you a real persistent cache that is written to a NVMe drive, so your layer cache is always available when your build starts. Meaning you can often drop` --cache-from` entirely and just use the Depot cache directly.


Because it's a real NVMe drive, Depot also allows you to use real cache mounts in any environment, including CI. This is a powerful feature that can make your builds even faster by allowing you to reuse partial state between runs, rather than having to rebuild everything from scratch.


## Why cache mounts matter


Structuring your Dockerfile to maximize reuse of the layer cache is a tried and true method for speeding up your builds. Average build times decrease sharply when expensive build steps can be skipped entirely. The` --cache-from` build flag can help you configure this properly in traditional CI with ephemeral runners, but oftentimes this alone is insufficient.


For various reasons, maybe layers were evicted from remote cache, or build context changed – docker may still decide to execute a build step. If layer cache is all you’ve got, you’re stuck waiting for that step to run from scratch. More advanced CI providers like Depot equip you with another tool for these scenarios, cache mounts.


## ` docker build --cache-from` is all or nothing


The` --cache-from` flag tells BuildKit to load it's layer cache from a type of cache backend like a local file system, registry, or even the GitHub Actions cache. Similarly, the` --cache-to` flag can be used to persist the layer cache to a remote backend after the build completes.


```text
docker   build   --cache-from=type=registry,ref=myregistry/myimage:cache   \
--cache-to=type=registry,ref=myregistry/myimage:cache,mode=max
...
```


The` --cache-from` flag is a powerful tool that allows you to reuse previously built layers from a remote cache. In theory, it's great because you can reload your layer cache (i.e. previous build results) across builds in ephemeral environments like CI. In practice, it's quite painful to use effectively because of network latency, registry limits, and GitHub Actions cache size limits.


Once the cache is loaded from the remote backend, BuildKit will use it to determine whether a layer can be reused or needs to be rebuilt. If a layer is found in the cache, it will be reused, otherwise, BuildKit will execute the build step and save the resulting layer to the cache for future use.


Crucially,` cache-from` is all or nothing, and it does not permit partial restores of a layer. This can be particularly painful, for example if your build step is installing dependencies or attempting an incremental compilation:


```text
# Add a dependency here
COPY   package.json pnpm-lock.yaml ./
# Ouch - now we need to rebuild this layer from scratch
RUN   pnpm install --frozen-lockfile
```


Consider a one-line change to` package.json` to add a dependency. You might expect this to result in an incremental build, which downloads a single package and reuses everything else from a previous build.


However, whether your diff was large or small, all BuildKit sees is a different checksum for` package.json` . BuildKit will invalidate your layer cache for all subsequent steps and run` pnpm install` from scratch, as if you're installing a totally new set of dependencies.


This is where cache mounts can really shine.


## Cache Mounts


**Cache mounts** are complementary to the layer cache. They act like a second line of defense, activating only when a layer needs to be rebuilt. Consider this modified Dockerfile:


```text
# Add a dependency here
COPY   package.json pnpm-lock.yaml ./
# Rebuild this layer, but not from scratch! :tada:
RUN   --mount=type=cache,target=/root/.local/share/pnpm/store pnpm install --frozen-lockfile
```


We've asked BuildKit to mount a persistent directory when it cannot outright skip a build step. This directory is injected into the container at build time, and its contents can be read and mutated by the corresponding command.


Here we chose` /root/.local/share/pnpm/store` as a mount location to target where pnpm stores its cache by default. And voila -- we've effectively implemented a multilevel cache, making our builds properly incremental.


A change to` package.json` still invalidates the layer cache, but now pnpm downloads only the new dependency, pulling the rest from disk in the warm cache mount.


[Ready for builds that never forget your cache? Depot gives you persistent NVMe storage. Read the docs →](https://depot.dev/docs/cache/overview)


## Caveat: Use in Ephemeral CI Environments


Cache mounts are simple to implement and effective at stabilizing build times. Despite the benefits, they're rarer than you might expect, simply because they're difficult to use in traditional CI. Why? Because cache mounts can't be exported and expect to reuse the file system state from a previous build (i.e. the cache mounts don't get exported with things like` cache-from` ).


For example, the default GitHub Actions runners are ephemeral by nature, so any work you've done to populate the cache directory is discarded when the workflow completes and the machine is terminated.


Most workflows we've seen in the wild use options like` cache-from` instead and hope for the best, but we covered above how that's just leaving performance on the table.


Alternatively,[it is possible](https://github.com/reproducible-containers/buildkit-cache-dance) with some effort to hack the cache mount directory into GitHub Actions's cache. But to take full advantage of the feature, it's more straightforward and performant to use a modern CI provider like Depot with native support.


## Caveat: Concurrent Access


The cache directory lives on the Docker host itself and is made available to any build step that needs it in the future. This is regardless of timing -- if two builds arrive simultaneously, BuildKit will very happily mount the directory (not a copy) to both, processing the build steps concurrently.


```text
COPY   . .
# Simultaneous builds will share access to this directory
RUN   --mount=type=cache,target=/root/.cache/go-build \
go build ./cmd/
```


It's also possible for BuildKit to mount the directory to separate build stages in the same build, e.g. for dev and prod dependencies.


```text
# Build assets in the presence of dev dependencies
FROM   base   AS   build
COPY   package.json pnpm-lock.yaml ./
# (1) Likely run in parallel:
RUN   --mount=type=cache,target=/root/.local/share/pnpm/store \
pnpm install --frozen-lockfile


# Prep production dependencies in isolation
FROM   base   AS   deps
COPY   package.json pnpm-lock.yaml ./
# (2) Likely run in parallel:
RUN   --mount=type=cache,target=/root/.local/share/pnpm/store \
pnpm install --frozen-lockfile --prod
```


Both of these scenarios are likely in CI, so it's important to consider whether concurrent access to the cache directory is safe with our specific build tool.


In our case,` pnpm` is designed with lock-free concurrent cache access in mind, and the same can be said for many other common package managers. Since we can assume multiple builds can coexist without clobbering each other, we can proceed with the default sharing mode of` shared` .


However, there are tools like` apt` that won't tolerate concurrent access, first acquiring a pessimistic file lock before overwriting system files in-place.


If two` apt` commands compete for the same build cache, the loser will fail to acquire the apt lock and exit with an error. We can solve for this by changing the mount sharing mode:


```text
# sharing=locked exclusively locks the cache mount
# Build B now blocks and waits until Build A is complete
RUN   --mount=type=cache,sharing=locked,target=/var/cache/apt \
--mount=type=cache,sharing=locked,target=/var/lib/apt \
apt update && \
apt-get -y --no-install-recommends install build-essential
```


Here we ask BuildKit to acquire an exclusive lock to our cache mount when the build step begins and release it when the step is complete. Other builds will block and wait until the lock is released, so there's no risk of any two commands running simultaneously, avoiding errors or even corruption in the worst case.


## Caveat: Lock Contention


Locked sharing can make builds more consistent and correct at the cost of serialized access to the cache mount. This is a pretty easy tradeoff to make, especially when starting out, but this can quickly become a performance bottleneck as your build volume scales.


Builds will begin to queue, average build duration will climb, and so too will your bill if you're paying by the build minute. This scenario commonly arises in monorepos that contain a multitude of apps with similarly structured Dockerfiles.


Imagine five apps whose Dockerfiles all include the following cache mount:


```text
RUN   --mount=type=cache,target=/usr/local/cargo/registry \
--mount=type=cache,target=/usr/local/cargo/git \
# All five apps compete for this exclusive lock
--mount=type=cache,target=/sccache,sharing=locked \
cargo build -p myapp --release
```


This instructs BuildKit to serialize access to the` /sccache` mount for *all concurrent builds* . That means a cross-cutting commit could trigger a build for all apps and inadvertently create a build queue, with only one app allowed to compile at a time.


If we wanted to keep these builds running on the same host, the simplest change is to add an explicit namespace with the` id` field. The namespace was previously derived from the` target` itself, but we can delineate cache and serialize access per app as follows:


```text
-    --mount=type=cache,target=/sccache,sharing=locked \
+    --mount=type=cache,target=/sccache,sharing=locked,id=myapp \
```


Alternatively, it may be more appropriate to dispatch each app to a different builder with dedicated resources. Since the cache lives on the builder host itself, it can act as an effective namespace so long as app-host assignment is sticky.


In Depot, you can achieve this today with projects, where each app gets its own project:


```text
depot   build   -t   app1   --project   0194ae0c   .
depot   build   -t   app2   --project   a850b922   .
depot   build   -t   app3   --project   6603246f   .
```


## TLDR


Traditional caching in Docker relies on a fragile mix of remote registries, ephemeral runners, and best-effort CI hacks. It works, until it doesn't.


Depot solves the real bottleneck by persisting your layer cache and it's cache mounts automatically while giving you instant access across builds. By giving you a fast, persistent NVMe volume for every build, your layer cache and your cache mounts are always warm and ready.


The result is faster, more resilient builds that scale cleanly with your team.


## FAQ


What are cache mounts in Docker and how do they work?


Cache mounts are persistent directories that BuildKit injects into your container during specific build steps. Unlike layer cache which is all or nothing, cache mounts let you preserve partial state between builds. For example,` RUN --mount=type=cache,target=/root/.local/share/pnpm/store pnpm install` mounts a directory that pnpm can read from and write to, so when your package.json changes, pnpm only downloads new dependencies instead of reinstalling everything from scratch.


What is the difference between --cache-from and cache mounts?


` --cache-from` reuses entire build layers from previous builds, but if a layer changes at all, BuildKit rebuilds it completely from scratch. Cache mounts work at a different level, preserving partial state within a build step. So when layer cache misses, cache mounts kick in as a second line of defense, allowing tools like package managers or compilers to reuse most of their cached data even when the layer itself needs to be rebuilt.


When should I use sharing=locked vs sharing=shared for cache mounts?


Use` sharing=locked` when your build tool can't handle concurrent access to the cache directory, like` apt` which acquires a file lock before overwriting system files. Use` sharing=shared` (the default) when your tool supports concurrent access, like pnpm, npm, or go build. With locked sharing, only one build can use the cache mount at a time, so other builds will queue and wait for the lock to be released.


Can I use cache mounts in GitHub Actions?


Technically yes, but they're difficult to use effectively because GitHub Actions runners are ephemeral. The cache mount directory lives on the Docker host and gets wiped when the runner terminates. You can hack it into GitHub Actions cache with extra tooling, but it's more straightforward to use a CI provider like Depot that gives you persistent NVMe storage where cache mounts just work automatically.


## Related posts


- [Optimize your Dockerfile for faster builds](https://depot.dev/blog/optimize-your-dockerfile)
- [How to reduce your Docker image size](https://depot.dev/blog/how-to-reduce-your-docker-image-size)
- [Fast Rust builds with sccache and GitHub Actions](https://depot.dev/blog/sccache-in-github-actions)
- [Remote build caching: The secret to lightning-fast builds](https://depot.dev/blog/remote-build-caching-secret-to-software-builds)


Luke Morris


Staff Software Engineer at Depot
