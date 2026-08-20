---
schema_version: "1.0.0"
document_id: "b177bf1d33c341ebe810b44655f4ec237354335caa40778ea6bf58c4945e84f6"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/wasm-clouds-the-world-after-containers"
published_at: "2026-05-20T13:33:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:04665c9c2fc9e7fca82ce7b7ba003553e9f77857316b7858beb802d9d3979731"
---

# WebAssembly Clouds: The world after Containers

Wasmer is a computing platform built on WebAssembly. Instead of isolating applications using virtual machines or containers, we run them using WebAssembly.


The practical consequence is that executable code can be shared across applications, while memory and state remain fully isolated. This significantly reduces per-application memory overhead and startup latency compared to container and VM-based platforms.


Modern cloud workloads look very different from the services containers were designed for. AI inference, agents, and API-driven workloads are bursty, short-lived, and often dominated by I/O rather than CPU. They need to **scale from zero to thousands** of instances globally, while safely running untrusted code side by side.


This post explains how a **multi-tenant WebAssembly runtime with shared executable pages** becomes a foundational primitive after containers for the upcoming AI age.


## Wasmer


We started Wasmer with the aim of creating an ecosystem for running applications securely, fast and in any platform and operating system.


Over the last years, we reached a key realization: the technology we created is critical for AI-generated applications, due to the increasing needs for sandboxed execution with high density and fast startup times. But, why is this now possible?


*Note: This approach comes with real tradeoffs, particularly around ecosystem compatibility and native binaries, which we’ll discuss later.*


### How Containers improved Virtual Machines


Containers improved VMs by moving the Kernel one level up. By doing that they were able to **reuse the Kernel** across all containers, and thus, they delivered much better performance than VMs.


However, Containers still need to make use of an hypervisor to allow proper sandboxing on the kernel, and they still include the OS code inside of it in most cases (other than scratch images or unikernels).


### How Wasmer improved Containers


Wasmer has taken things one step further: it also completely removes all OS code from the containers.


But more importantly, It allows **reusing the binaries across different apps** .


Containers replicate binary and runtime state per tenant, Wasmer shares executable pages across isolated tenants.


For example, all applications using a specific version of Python will reuse exactly the same Python binary. Thanks to the properties of WebAssembly (where executable code and data are in different memory regions), the Python runtime binary will be shared across all apps, but each one will have different data for it.


> Nowadays, modern OSes do a similar optimization when doing consuming a shared library across different processes. However, this capability is lost when running through containers, as it breaks their sandboxing model.


Memory is often the highest cost of running a customer’s code (even higher than the CPU), lowering it by an order of magnitude dramatically changes the economics.


By using a Wasm-native architecture we enabled something pretty amazing:


1. All binary files can be reused between apps
2. No hardware virtualization is needed
3. And thus, much higher density of compute is unlocked


## Startup times


Things are sometimes better explained with a graph.


Cold start latency is mostly a function of how much state needs to be initialized before user code runs: kernel boot, container startup, runtime initialization, and dependency loading.


The chart below shows median cold-start response times for a minimal Python HTTP server across several platforms. The code the apps run[is incredibly simple](https://github.com/wasmerio/examples/tree/main/python-http) , as we wanted the **numbers to reflect platform overhead rather than application logic** .


> You can see a benchmark of AWS Lambda vs Cloudflare Workers here:[https://cold.picheta.me/#bare](https://cold.picheta.me/#bare) . The Python App is benchmarked in Wasmer and Fly.io using OpenStatus from their` ams` location.


The key takeaway is not the exact numbers, but the story they tell: removing per-app OS and runtime initialization reduces cold-start latency by **an order of magnitude** .


On the brighter side, we even haven’t spent yet time fully optimizing startup time… we are aiming to could reduce our current startup times even further by at least a 2-5x factor!


By using[Instaboot we can keep startup times extremely low](https://wasmer.io/posts/announcing-instaboot-instant-cold-starts-for-serverless-apps) , even for apps that are very large (like WordPress, LangFlow, …).


## Compute Density


Thanks to this architecture, we have been able to run tens of thousands of applications per server without having issues scaling them from zero to millions of requests. As of today, more than **half million apps are running in a handful of servers** .


Given that the overhead of the runtime is minimal, it’s now easy to relate density and app size (and memory consumption of that app). So the scaling is mainly bounded to the app intrinsics, with minimal overhead from the runtime.


Now you’ll be able to have an extremely high density of apps run per server. At least an order of magnitude higher than what we’ve been able to achieve with container or VM-based platforms. All this, without requiring to rewrite your cloud apps to a proprietary API (like Lambda or Cloudflare Workers do require).


## Billing


Most serverless platforms bill by wall-clock time, often rounded to 100ms. This worked for CPU-bound services, but it breaks down for modern AI workloads.


AI inference and agents are largely **I/O-bound** : they spend most of their time waiting on network calls to models, databases, or APIs. Even when no CPU is used, users are still billed for that idle time.


Because Wasmer dramatically reduces memory overhead and runs workloads at high density, we can bill based on **CPU cycles actually consumed** , not how long an application waits for external input. For AI workloads, this aligns cost with real resource usage… and it makes a meaningful difference.


## Security


Wasmer runs untrusted workloads in a multi-tenant WebAssembly runtime with shared executable pages.


Each application is isolated in its own WebAssembly instance, with **separate memory and execution state** , while compiled code pages are shared read-only across tenants.


All access to the host (networking, filesystem, and I/O) is **explicitly mediated by the runtime** , minimizing the attack surface.


Wasmer approach takes advantage of the sandboxing properties of Wasm and stops relying on hardware virtualization, enabling higher density and faster startup without sacrificing isolation.


# Tradeoffs


Not everything is unicorns and rainbows in Wasmland.


Adopting this new architecture has its downsides:


- Shared executable pages improve density and latency but require compilation to Wasm. This required building manually each of the packages for Wasmer (we do it, so you don’t have to worry):


- PHP
- Python
- Go (stay tuned!)
- …


- Small hit on performance: Right now we have seen a hit of 5-10% of runtime performance compared to native binaries (Python, PHP). However, due to the 32-bit function pointer nature of Wasm, and with some optimization hints… we could eventually have faster-than-native execution!
- Not a fit for (yet)


- Workloads that depend on kernel modules or some POSIX goodies that are not yet supported in WASIX
- JITted runtimes (like V8) would need to create new backends that emit WebAssembly code instead of x86 or aarch64 assembly for native-like performance
- Applications requiring arbitrary native binaries (if you require native binary execution without recompiling it to Wasm)


Even with the downsides, we believe that this change introduces a new paradigm for Cloud Computing. It will be hard for any other technology to make a bigger dent than Wasmer will do to the clouds.


## Your help


I would love for you to try Wasmer and let us know your thoughts and experience.


There’s still a lot that we have to work on to make it ready for any language ecosystem, and for that we would love your feedback!
