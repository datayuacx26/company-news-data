---
schema_version: "1.0.0"
document_id: "a5b15f407f483ce7bdb414af1ee7661d9f74c21702cb5065a64a34fa6edba68f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/sccache-in-github-actions"
published_at: "2025-03-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:4752b277d4a55cd4924a5016c626b6b2161bf27799a5679626ded30fa9466b6b"
---

# Fast Rust builds with sccache and GitHub Actions

You know the story. You start an innocent project in Rust and start using GitHub Actions as your CI provider. The project accumulates complexity and evolves into a workspace with several crates. Its compilation time grows in kind, and you're finding it harder and harder to maintain flow.


In response, you've added caching to your workflow using[GitHub’s example config](https://github.com/actions/cache/blob/main/examples.md#rust---cargo) :


```text
-   uses  :   actions/cache@v4
with  :
path  :   |
~/.cargo/bin/
~/.cargo/registry/index/
~/.cargo/registry/cache/
~/.cargo/git/db/
target/
key  :   ${{ runner.os }}-cargo-${{ hashFiles('**/Cargo.lock') }}
```


It’s been doing some heavy lifting, but recently results have been underwhelming. Oftentimes small code changes cause wholesale recompilation and, to make matters worse, the ballooning cache payload itself is taking significant time to handle.


You grind your teeth and wonder if it’s all downhill from here; after all, you chose Rust intentionally, knowing you were trading fast execution for slow compilation. Are you doomed to push small changes and wait an eternity while the world is rebuilt, ad infinitum?


No, friend. Just Use Sccache.


[Use Depot Cache with sccache to unlock faster builds with distributed remote caching that's shared with your entire team and CI environment. Try it free for 7 days →](https://depot.dev/sign-up)


## GitHub as a suboptimal default


On your laptop, you may be used to incremental builds out of the box, with` cargo` saving granular building blocks to disk for later reuse. In a CI environment like GitHub Actions, however, runners are ephemeral by nature and don’t include the benefit of a persistent disk. Artifacts from one job will not carry over to the next, and it’s up to you to reconstitute important context before you begin your build.


It’s natural to reach for` actions/cache` here: list the directories that are important to keep, and GitHub will manage their propagation across builds. But, although it's straightforward to install, it doesn’t take long for a Rust project to feel the limitations of this setup. The` target/` directory will hoard artifacts from prior builds and grow uncontrollably without your intervention. Even if you cull stale artifacts, the whole collection is handled as a coarse unit, and builds will regularly download a cache entry containing a subset of artifacts that aren’t useful.


Beyond that, GitHub network transfer is[notoriously slow](https://depot.dev/blog/github-actions-cache) , and each repo is limited to a total cache size of 10GB, which fills quickly when you’re saving whole copies of the` target/` directory at a time. In short, this cache is operational but far from optimal.


## Enter sccache


` cargo` struggles to capitalize on its aggressive caching strategy without a persistent disk in CI and is particularly hamstrung in GitHub Actions. In contrast,` sccache` was designed with ephemeral environments in mind and sidesteps GitHub's limitations with an alternative approach.


It wraps the rust compiler (` rustc` ) and functions like a shim, intercepting all compilation requests inbound from` cargo` . Deriving a cache key[from the request and its environment](https://github.com/mozilla/sccache/blob/main/docs/Caching.md#rust) ,` sccache` then checks for its presence in its cache. A hit means the compilation task was previously completed, so` sccache` simply returns the cached result. With a miss,` sccache` forwards the call to` rustc` and caches the result for later.` sccache` will store this cache on disk by default but, crucially, it also has native support for remote content-addressable storage (CAS).


` sccache` is straightforward to test locally. After installing the` sccache` binary to your system, you can run your first` sccache` -enhanced build like so:


```text
RUSTC_WRAPPER  =  sccache   cargo   build   --release
```


Your first build may take some time as` sccache` warms the cache, but rerun the build and your second should be much faster:


```text
sccache   --stop-server   # to isolate stats for the next build
cargo   clean
RUSTC_WRAPPER  =  sccache   cargo   build   --release
```


` sccache -s` will ask for build statistics, which in this case should reveal all cache hits.


```text
$   sccache   -s


Compile   requests                        45
Compile   requests   executed               33
Cache   hits                              33
Cache   hits   (Rust)                     33
Cache   misses                             0
Cache   hits   rate                     100.00   %
Cache   hits   rate   (Rust)            100.00 %
…
Non-cacheable   calls                     11
Non-compilation   calls                    1
…
```


` sccache` is similarly straightforward to adopt in CI with the help of the official[sccache GitHub Action](https://github.com/marketplace/actions/sccache-action) . Add the following to your workflow to install and activate` sccache` . Note the line enabling the GitHub Actions cache as the default remote backend for now.


```text
- runs-on: ubuntu-latest
- steps:
+   - name: Run sccache-cache
+     uses: mozilla-actions/sccache-action@v0.0.7


- name: Compile project
+     env:
+       SCCACHE_GHA_ENABLED: "true"
+       RUSTC_WRAPPER: "sccache"
run: cargo build --release
```


With just a bit of effort, you now have functional, incremental compilation in your CI pipeline. As expected, your first build will populate the cache, and successive builds should be much faster as those cache contents are utilized.


## Taking stock


This represents an improvement over the status quo. Whereas` cargo` alone must wait for the whole cache blob to arrive upfront (and then also later depart),` sccache` allows the build to begin immediately and concurrently fetches only what’s necessary for the current build. However, the GitHub backend still comes with many of the aforementioned caveats – slow network transfer, a 10GB maximum, etc – and its direct (ab)use as a CAS unfortunately limits us in a new and important way.


For each invocation of` rustc` ,` sccache` will ask the cache backend if the corresponding artifact exists. If your project is large and contains a lot of dependencies, this could end up being too chatty for GitHub’s liking. To its credit,` sccache` gracefully treats a` 429 Too Many Requests` response as a cache miss, as opposed to failing your build midway. But this is indeed a false miss, and the corresponding compilation time during periods of high activity could result in worse overall build performance.


## Enter Depot Cache


All of these issues are alleviated by choosing a backend designed specifically for this workload.` sccache` is built on top of the awesome OpenDAL storage adapter and thus supports a number of backends with alternative protocols. We’ve added WebDAV support to Depot Cache, and so it plugs right into` sccache` (in addition to other popular WebDAV-based build clients like` gradle` and` bazel` ).


You can use Depot Cache by configuring an endpoint and a token:


```text
- runs-on: ubuntu-latest
- steps:
- name: Run sccache-cache
uses: mozilla-actions/sccache-action@v0.0.7


- name: Compile project
env:
-       SCCACHE_GHA_ENABLED: "true"
+       SCCACHE_WEBDAV_ENDPOINT: 'https://cache.depot.dev'
+       SCCACHE_WEBDAV_TOKEN: DEPOT_ORG_TOKEN
RUSTC_WRAPPER: "sccache"
run: cargo build --release
```


And with that,` sccache` is free to manage its artifacts without restriction. Artifacts from one branch are available to any other as soon as they’re built, and its cache can grow to any size you need. This alone is a great milestone, but if you want ultimate performance, there’s one final low-hanging fruit to harvest.


## Extra Credit: Depot Runners


We've written at length before about GitHub's CI runners. In sum, they're expensive, underpowered, and often unreliable. At Depot, we're working hard to improve this experience for everyone, and that includes smoothing out rough UX edges wherever possible. In addition to being[30% faster and 50% cheaper](https://depot.dev/blog/introducing-github-actions-ultra-runners) , our Runners have fast network access to Depot Cache, so` sccache` can run at optimum speed.


On a Depot Runner,` sccache` will automatically authenticate to Depot Cache, so all told, your workflow file can simplify to the following:


```text
- - runs-on: ubuntu-latest
+ - runs-on: depot-ubuntu-latest
- steps:
- name: Run sccache-cache
uses: mozilla-actions/sccache-action@v0.0.7


- name: Compile project
env:
-       SCCACHE_WEBDAV_ENDPOINT: 'https://cache.depot.dev'
-       SCCACHE_WEBDAV_TOKEN: DEPOT_ORG_TOKEN
RUSTC_WRAPPER: "sccache"
run: cargo build --release
```


With a more powerful runner and a more capable cache backend, your builds should finish significantly faster. The time you recover can be better spent iterating on your code and making your users happier.


Compile time will certainly continue to demand your attention as your project gathers steam, but removing your CI pipeline as your main bottleneck allows you to direct your energy toward[optimizations with uniform benefit](https://matklad.github.io/2021/09/04/fast-rust-builds.html) to localhost and CI alike.


## FAQ


What is sccache and how does it speed up Rust builds?


` sccache` wraps the Rust compiler and intercepts compilation requests from cargo. It generates a cache key from each compilation request and checks if that exact compilation was done before. On a cache hit, it returns the cached result instead of recompiling. On a miss, it forwards the request to rustc and caches the result. Unlike cargo's built-in caching, sccache stores artifacts in content-addressable storage that works in ephemeral CI environments.


How do I use sccache with Rust in GitHub Actions?


Add the` mozilla-actions/sccache-action` to your workflow, then set two environment variables before your build:` RUSTC_WRAPPER: "sccache"` to make cargo use sccache as a compiler wrapper, and` SCCACHE_GHA_ENABLED: "true"` to use GitHub Actions cache as the backend. Your first build will populate the cache, and subsequent builds will be much faster as sccache reuses previously compiled artifacts.


Why does sccache work better than actions/cache for Rust builds?


` actions/cache` saves and loads the entire` target/` directory as a coarse unit, which grows uncontrollably and regularly contains artifacts that aren't useful for the current build. Network transfer is slow and the 10GB cache limit fills quickly. sccache starts building immediately and fetches only what's needed for the current build concurrently. It's designed specifically for ephemeral environments and sidesteps GitHub's limitations with content-addressable storage.


Can I use Depot Cache as a backend for sccache?


Yes, sccache supports WebDAV backends through OpenDAL, and Depot Cache provides a WebDAV interface. Set` SCCACHE_WEBDAV_ENDPOINT: 'https://cache.depot.dev'` and` SCCACHE_WEBDAV_TOKEN` with your Depot org token. This eliminates GitHub's 10GB limit and slow network transfer, and artifacts from one branch are immediately available to any other. If you use Depot Runners, authentication happens automatically.


## Related posts


- [Remote build caching: The secret to lightning-fast builds](https://depot.dev/blog/remote-build-caching-secret-to-software-builds)
- [How we launch GitHub Actions runners in 5 seconds](https://depot.dev/blog/github-actions-breaking-five-second-barrier)
- [How we cut GitHub Actions queue times by 4x](https://depot.dev/blog/reducing-queue-time-with-cached-schemas)


Luke Morris


Staff Software Engineer at Depot
