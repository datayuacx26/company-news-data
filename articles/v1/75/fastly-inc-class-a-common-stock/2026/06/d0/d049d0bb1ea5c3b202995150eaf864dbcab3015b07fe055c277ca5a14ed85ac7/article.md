---
schema_version: "1.0.0"
document_id: "d049d0bb1ea5c3b202995150eaf864dbcab3015b07fe055c277ca5a14ed85ac7"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/fastly-c-sdk-is-now-generally-available/"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:79c744765a7ef763d6ed82eac06f9bb53f2a78a720a81af3b5a4b5a7e4326820"
---

# The Fastly C++ SDK is Now Generally Available

At first, C++ at the edge feels like an odd mix, but as you dive deeper the utility becomes obvious. Even before its standardization in 1998, C++ had been a backbone of performant computing systems and it remains so to this day. Gaming engines, financial systems, and so many pieces of glue holding the web together all rely on it. And as an ecosystem C++ has a pretty unparalleled array of libraries and tools for developers to use.


So we built an SDK for C++ at the edge, and I’m thrilled to say it’s generally available as of today.


## Native C++ Performance on Fastly Compute with WebAssembly


We focused on a few key use cases for this SDK:


-


**Unlocking and securing legacy code:** Many teams have valuable legacy code they want to utilize at the edge, particularly those moving away from older architectures (e.g., developers who previously used Apache Traffic Server (ATS) or even cgi-bin scripts).


-


**Powering modern gaming logic:** The modern gaming landscape is still fundamentally powered by C++. This SDK lets game developers extend latency-sensitive gaming features to the edge — think low-ping server selection, multiplayer matchmaking. Gamers know the difference.


-


**Agentic access to the C++ ecosystem:** Agentic systems need flexibility and access to specific tools, and the C++ ecosystem provides them. No matter what you’re needing your agentic programs to accomplish, the performance of C++ with the breadth of its libraries and solutions will be able to tackle it.


You probably know this already but[Fastly Compute](https://www.fastly.com/products/edge-compute) is built for native execution of Wasm binaries. Those binaries retain the performance profile of the language they’re built and compiled in — meaning when you write C++ for Fastly it’s idiomatic, feels natural, and can easily handle even the most demanding edge workloads. Wasm even provides an isolated, memory-safe sandbox so your C++ code can be memory-safe.


## Benefits of Running C++ at the Edge


-


**Ultra-low latency:** When you run C++ on Fastly Compute, your code compiles down to some of the smallest and most efficient Wasm bytecode available. This gives you the performance required for the most critical parts of your applications; whether that’s your new anti-cheat solution, clipping audio, or just parsing datasets.


-


**Security and polyglot advantages:** Running C++ on the edge shouldn't mean sacrificing security or locking your entire team into a single language. Because our Wasm isolation provides built-in memory safety, you get C++ speed with built-in security. And it plays nice with all the other languages Compute supports, meaning your teams can collaborate even across language boundaries.


-


**Even more with Object Storage and Image Optimization:**


-


**Object Storage:**[Fastly Object Storage](https://www.fastly.com/blog/getting-started-fastly-object-storage-cdn-integration-serverless-computing) can help you deliver everything from install data and patches, to in-game assets. Everything is delivered efficiently and reliably while you benefit from zero egress fees. For example, customers like[iRacing](https://learn.fastly.com/storage-webinar.html) use Fastly Object Storage for all of their asset delivery.


-


**Image Optimization:** Because modern gaming relies heavily on rich web-based ecosystems like companion apps, leaderboards, and digital game stores, using[Fastly Image Optimizer](https://www.fastly.com/products/image-optimization) can help build intelligent workflows. Transform your images on the fly, whether they need to be served in-game or from the web, all from a single source of truth.


Bringing C++ to the edge just makes sense. You get to keep the incredible speed and vast library ecosystem you already rely on with the added performance and security benefits that only Wasm can enable.


## Get Started with Fastly C++ SDK Today


Get started today. Our C++ SDK is generally available and if you want some help getting started, be sure to check out our[code examples and docs](https://www.fastly.com/documentation/guides/compute/developer-guides/cpp/) . As always, connect in[our forum](http://community.fastly.com/) and let us know what you are building!
