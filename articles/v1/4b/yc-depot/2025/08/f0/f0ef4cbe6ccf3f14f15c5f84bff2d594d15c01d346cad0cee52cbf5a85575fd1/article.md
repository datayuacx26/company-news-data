---
schema_version: "1.0.0"
document_id: "f0ef4cbe6ccf3f14f15c5f84bff2d594d15c01d346cad0cee52cbf5a85575fd1"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/guide-to-faster-rust-builds-in-ci"
published_at: "2025-08-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:7e1d0f25ebaeb9d77a823750c559b113da83ae553b4afbc7d7e7aa3b7163e920"
---

# Guide to faster Rust builds in CI

We'll be building the[Zed](https://github.com/zed-industries/zed) project, a code editor written in Rust. Zed is a large codebase with many dependencies, making it a good candidate for exploring build optimizations. We'll also be starting off using Depot's GitHub Actions runners, which have a ton of[optimizations already implemented](https://depot.dev/blog/introducing-github-actions-ultra-runners) . I would know, I[worked on some of them!](https://depot.dev/blog/accelerating-builds-improve-ec2-boot-time)


Just for fun, I built Zed with GitHub’s equivalent runner. We ran the same[baseline workflow](https://github.com/depot/zed-test/actions/runs/16666944005/job/47175083235) on` ubuntu-latest-8` . The[GitHub runner workflow](https://github.com/depot/zed-test/actions/runs/16679327806) took 43m 16s - a staggering 61.1% slower than Depot’s 26m 51s baseline. The easiest optimization you can make to your CI workflows is to use Depot!


## Toolchain optimizations


### Mold linker


First, let’s work on optimizing the Rust toolchain itself. The fewer changes we need to make to this codebase, the better. We can start by replacing the default linker (lld on Linux) with[mold](https://github.com/rui314/mold) . This linker is designed to be really fast, especially for projects with large build outputs.


#### Results


The mold linker showed negligible results for the Zed codebase. When applied only to release builds (not tests), the[mold workflow](https://github.com/depot/zed-test/actions/runs/16679565239) completed in 28m 49s - just 0.7% slower than baseline.


Test execution took 15m 27s and release builds took 11m 35s. The minimal difference doesn't justify the added complexity of configuring a different linker for the Zed codebase.


### Nightly features


The nightly Rust compiler has several features that can help to speed up builds, if you’re willing to switch Rust compiler versions. The two that we’ll use in this example are:


- ` -Z share-generics` : This feature allows the compiler to share generic code across different compilation units, which can significantly reduce build times for large projects.
- ` -Z threads=8` : This feature allows the compiler to parse files and expand macros in parallel


#### Results


Switching to Rust nightly provided significant benefits. The[nightly workflow](https://github.com/depot/zed-test/actions/runs/16731664175) completed in 26m 30s - a 7.3% improvement over baseline.


- Total time: 26m 30s (7.3% faster than baseline)
- Test execution: 15m 43s (similar to baseline)
- Build time: 9m 1s (22.7% faster than baseline)


The nightly features (` -Z threads=8` and` -Z share-generics=y` ) dramatically improve build times by allowing the compiler to do more work concurrently and avoid redundant compilation of generic code.


## Caching strategies


### Caching dependency downloads


Let’s get into the real meat of speeding up Rust builds: caching. Cargo, the Rust package manager, can download hundreds of dependencies before compiling them. This can be slow, especially for large projects with many dependencies. By caching these downloads, we can speed up build times significantly.


#### Results


Caching Cargo dependencies showed good improvements when combined with nightly features. The[cargo caching workflow with nightly](https://github.com/depot/zed-test/actions/runs/16731664369) completed in 26m 18s - 8.0% faster than baseline.


By caching the` ~/.cargo` directories between runs, we avoid re-downloading and re-indexing hundreds of crates.


### Sccache


[sccache](https://github.com/mozilla/sccache) is a compiler cache for Rust that can significantly speed up builds by caching compiled artifacts. It works by storing the output of compilation in a local or remote cache, so that subsequent builds can reuse these artifacts instead of recompiling everything from scratch.


[Depot Cache](https://depot.dev/docs/cache/integrations/sccache) has built-in support for sccache storage, allowing you to share cached artifacts across different CI runs and local development environments. Using the` depot` CLI, you can easily set up sccache to use Depot as its cache backend by running the` depot cargo` command.


#### Results


Using` depot cargo` with sccache showed promising results with a warm cache:


**Cold cache** :[33m 23s](https://github.com/depot/zed-test/actions/runs/16731664252) (16.8% slower than baseline, 26.8% slower than previous optimization)


- Test execution: 13m 18s (12.3% faster)
- Build time: 14m 54s (27.7% slower)


**Warm cache** :[25m 18s](https://github.com/depot/zed-test/actions/runs/16732584997) (11.5% faster than baseline, 4.0% faster than previous optimization)


- Test execution: 13m 12s (12.9% faster)
- Build time: 10m 48s (7.4% faster)


With a warm cache, sccache provides meaningful speedups for both test and build times.


## Compiler backend alternatives


### Compiler backends?


For those of you who aren't compiler nerds,` rustc` uses LLVM as its backend for machine code generation. You can think of LLVM as a library that the Rust compiler uses to generate machine code for different architectures. However, LLVM can be slow to compile large projects, since it’s designed for optimized machine code first, and compile speed second. This is where alternative backends come in.


### Cranelift for debug builds and tests


[Cranelift](https://cranelift.dev/) is an alternative backend for the Rust compiler, designed to compile fast enough to be used in development iterations. It trades off some runtime performance for faster compile times, making it a great choice for debugging and testing builds.


#### Results


Cranelift fails to compile the Zed codebase on both stable and nightly Rust. The[cranelift workflow](https://github.com/depot/zed-test/actions/runs/16730115341) shows:


```text
error:   asm!   and   global_asm!   sym   operands   are   not   yet   supported
error:   could   not   compile   `  wasmtime-fiber  `   (  lib  )   due   to   1   previous   error
```


This is a known limitation of Cranelift - it doesn't support all LLVM features, particularly inline assembly. Many Rust projects that depend on low-level crates (like wasmtime in Zed's case) cannot use Cranelift as a drop-in replacement. This limitation exists regardless of whether you use stable or nightly Rust.


For now, we'll[revert this change](https://github.com/depot/zed-test/actions/runs/16727448022) . While Cranelift can provide dramatic speedups for projects it supports, it won't work for our example codebase.


## CI-specific optimizations


### Cargo nextest with matrix builds for massive parallelization


[cargo-nextest](https://nexte.st/) is a next-generation test runner for Rust that can bring significant performance improvements over the default` cargo test` . The main advantage is that it runs each test in its own separate process, allowing us to run many tests in parallel.


#### Results


Cargo nextest showed excellent performance with a warm sccache. The[nextest workflow](https://github.com/depot/zed-test/actions/runs/16733819334) completed in 18m 34s - a 35.0% improvement over baseline. Test execution took 10m 46s, which is 28.9% faster than baseline.


Nextest delivers substantial performance improvements when combined with warm sccache, making it an excellent choice for CI pipelines.


### Architecture optimization


The single biggest impact on build times is often the hardware you’re running on. More CPU cores generally mean faster builds, especially for highly parallelizable tasks like Rust compilation. Doubling your CI costs isn’t always an option, of course.


#### Results


To test the impact of more CPU cores, we doubled the runner size from 8 to 16 cores. The[nextest-16-cores workflow](https://github.com/depot/zed-test/actions/runs/16733860911) results were interesting:


- Total time: 21m 25s (25.1% faster than baseline)
- Test execution: 7m 31s (50.3% faster test execution than baseline)
- Build time: 11m 54s (slower than 8-core due to cache warmth differences)


The 16-core configuration showed excellent test parallelization (7m 31s vs 10m 46s on 8 cores), but the overall time was impacted by sccache variability. This demonstrates that raw CPU scaling isn't always the answer - cache effectiveness can be more important than core count.


## Platform-specific optimizations


### Linux I/O optimizations with RAM disks


[RAM disks](https://en.wikipedia.org/wiki/RAM_drive) provide a filesystem backed by memory instead of persistent storage, offering dramatically faster I/O operations. For Rust builds, this can significantly reduce the time spent reading and writing intermediate compilation artifacts.


Our runners reserve some memory for a RAM disk. When a job starts, VMs will automatically create and mount this RAM disk to the runner, which helps to make every I/O operation, including reading and writing files for compilation, much faster.


For self-hosted runners or other CI providers:


```text
sudo   mkdir   -p   /mnt/ramdisk
sudo   mount   -t   tmpfs   -o   size=16G   tmpfs   /mnt/ramdisk


# Alternative: ramfs (pure RAM, no size limits - use with caution!)
# sudo mount -t ramfs ramfs /mnt/ramdisk


# Configure Rust to use it
export   CARGO_TARGET_DIR  =  /mnt/ramdisk/target
export   TMPDIR  =  /mnt/ramdisk/tmp
```


## Summary


After testing various Rust build optimizations on the Zed codebase, here are the key findings:


**Optimization** **Total** **Tests** **Build** **vs Baseline**


GitHub Runner 43m 16s ~30m ~13m +51.2%


Baseline (Depot) 28m 36s 15m 10s 11m 40s -


Mold linker 28m 49s 15m 27s 11m 35s +0.7%


Nightly features 26m 30s 15m 43s 9m 1s -7.3%


Cargo caching 26m 18s ~14m ~10m -8.0%


Depot cargo (warm) 25m 18s 13m 12s 10m 48s -11.5%


Cranelift Failed - - N/A


Nextest 18m 34s 10m 46s 5m 46s -35.0%


Nextest (16 cores) 21m 25s 7m 31s 11m 54s -25.1%


## Fin


The biggest wins came from using nextest with warm sccache (35.0% improvement on 8 cores) and combining multiple optimizations. Depot cargo with warm sccache provided an 11.5% improvement, while properly configured nightly features added 7.3%.


Ready to speed up your own Rust builds? Start with[Depot's optimized runners](https://depot.dev/github-actions) and work your way through these techniques - your future self (and your CI bill) will thank you.


## FAQ


What's the single most impactful optimization for Rust CI builds?


Cargo nextest with warm sccache provides exceptional performance. We achieved a 35.0% speedup on standard 8-core runners.


Should I use sccache for all my Rust builds?


It depends on your use case. sccache speeds up test compilation (11-14% faster) but can slow down release builds by up to 50%. Consider a hybrid approach: use sccache for tests and regular cargo for release builds.


Is switching to Rust nightly worth it for build performance?


Yes, if configured properly. Nightly features like` -Z share-generics=y` and` -Z threads=8` provided a 7.3% overall speedup with 22.7% faster build times in our testing. Make sure to actually pass these flags via RUSTFLAGS, not just install the nightly toolchain.


Why did Cranelift fail to compile the Zed codebase?


Cranelift doesn't support all LLVM features, particularly inline assembly. Projects with low-level dependencies (like wasmtime) often can't use Cranelift as a drop-in replacement, despite its potential for dramatic speedups.


What's the easiest optimization to implement right now?


Using cargo nextest. With warm sccache, this single change delivered a 35.0% speedup. It's a drop-in replacement for` cargo test` that runs tests in parallel more efficiently.


## Related posts


- [Fast Rust builds with sccache and GitHub Actions](https://depot.dev/blog/sccache-in-github-actions)
- [Best practice Dockerfile for speedy Rust builds](https://depot.dev/blog/rust-dockerfile-best-practices)
- [Introducing Ultra Runners — Up to 3x faster GitHub Actions jobs](https://depot.dev/blog/introducing-github-actions-ultra-runners)
- [How to reduce CI/CD costs: Complete optimization checklist](https://depot.dev/blog/how-to-reduce-cicd-costs-complete-optimization-guide)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)


Billy Batista


Software Engineer at Depot
