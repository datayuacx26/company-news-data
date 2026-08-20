---
schema_version: "1.0.0"
document_id: "2deec3f12bdefb7fd87de45cc9ef786305afc73c8232e9dc2422c5af3defefdc"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-buildkit-parallelizes-your-builds"
published_at: "2025-10-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:9bf47d7ff8aa9188dd4a1128128ef56b42c6de0bcc36777460f9c2ac0e0f6389"
---

# How BuildKit Parallelizes Your Builds

When you run` docker build` , you might assume your Dockerfile instructions execute one after another, like a traditional script. But behind the scenes, BuildKit is doing something far more sophisticated. At the heart of BuildKit lies a DAG (Directed Acyclic Graph) solver that transforms your Dockerfile into an optimized execution plan, identifying all parallelizable operations while maintaining dependency order.


We've previously covered how BuildKit works[here](https://depot.dev/blog/buildkit-in-depth) , but today we're discussing how BuildKit parallelizes your builds to make them faster and more efficient.


## Understanding BuildKit's DAG graph solver


BuildKit parses build instructions into something called LLB (Low-Level Build) format, creating a dependency graph of all the operations needed to produce your final image:


The DAG solver examines each instruction in your build and determines what it depends on. If instruction B needs the output of instruction A, they run sequentially. But if instructions B and C both only depend on A, they can run at the same time once A completes. This dependency analysis happens before any actual building starts, allowing BuildKit to create the most efficient execution plan possible.


This graph-based approach is what enables BuildKit to be fully concurrent. Every node in the graph represents a build operation, and BuildKit can execute any nodes that don't have unmet dependencies. It's constantly looking for work it can parallelize, which is why modern container builds can be surprisingly fast when structured properly.


## Three levels of parallelism in action


Once BuildKit has built its execution graph, parallelism happens at three distinct levels, each contributing to faster builds in different ways.


### Stage parallelism


The most visible form of parallelism happens when you have multiple stages in a multi-stage Dockerfile that don't depend on each other. Consider a typical web application with both frontend and backend components:


```text
FROM   node:20   AS   frontend
WORKDIR   /app
COPY   frontend/ .
RUN   npm install && npm build


FROM   golang:1.21   AS   backend
WORKDIR   /app
COPY   backend/ .
RUN   go build -o server


FROM   alpine   AS   final
COPY   --from=frontend /app/dist /static
COPY   --from=backend /app/server /usr/bin/
```


BuildKit recognizes that the` frontend` and` backend` stages have no dependencies on each other. While your Node.js dependencies are installing and your React app is building, BuildKit is simultaneously compiling your Go backend on a separate thread or CPU core. The` final` stage waits for both to complete, but you've effectively cut your build time by running these independent workloads in parallel.


### Instruction parallelism


Even within a single stage, BuildKit finds opportunities for parallel execution. When you have multiple` COPY` instructions that don't depend on each other, or when different branches of your build graph can be resolved independently, BuildKit executes them concurrently.


This is particularly noticeable when you're copying multiple directories or files that will be processed separately later. BuildKit can fetch all these resources in parallel rather than sequentially, shaving precious seconds off your build time. The solver is smart enough to understand which operations can safely overlap and which must wait for their dependencies.


### Deduplication across concurrent builds


While not being very obvious, perhaps the most clever optimization is BuildKit's automatic deduplication. This isn't just about caching, but about recognizing when multiple parts of your build are trying to do the exact same thing and ensuring that work happens only once.


Imagine you're building multiple services that all start from the same base image and run` npm ci` with identical` package.json` files. Without deduplication, each service would run its own` npm ci` , even if they're building simultaneously. But BuildKit is smarter than that. It uses content-addressable storage and checksums to identify when different build contexts would produce identical layers.


When BuildKit detects this situation, something interesting happens. The first build starts computing the layer while the others wait. Once the first build completes that` npm ci` , all the waiting builds immediately use that result and move on. The same operation that would have run three times runs only once.


This deduplication happens automatically across concurrent builds on the same runner, whether they're triggered by different developers pushing to the same repository or a` docker bake` command building multiple targets at once.


## Scaling beyond a single builder with Depot's auto-scaling


While BuildKit's parallelism features are powerful, they're ultimately limited by the resources of the machine they're running on. A single builder can only do so much, even with perfect parallelization. This is where Depot's[autoscaling](https://depot.dev/docs/container-builds/how-to-guides/autoscaling) feature changes the game.


By default, all builds for a Depot project run on a single BuildKit instance per architecture. This maximizes cache utilization and deduplication benefits, as all builds share the same cache and can deduplicate work across each other. But when you have resource-intensive builds or high build volume, that single instance can become a bottleneck.


With auto-scaling enabled, Depot automatically provisions additional BuildKit builders when your concurrent build limit is reached. Each new builder starts with a clone of the main builder's cache, so they're not starting from scratch. This means you can handle sudden spikes in build demand, like when multiple developers push changes simultaneously or when your CI/CD pipeline triggers numerous builds at once.


The relationship between BuildKit's parallelism and Depot's auto-scaling is complementary. BuildKit optimizes execution within each builder, finding every opportunity to parallelize and deduplicate work. Depot's auto-scaling ensures you have enough builders to handle your workload, preventing any single builder from becoming overwhelmed.


## What this means for your builds


The combination of BuildKit's intelligent DAG solver, multi-level parallelism, automatic deduplication, and Depot's autoscaling creates a build system that's both fast and efficient. It's not just about throwing more compute at the problem, but rather being smart about how that compute is used, ensuring that work is never duplicated unnecessarily and that every available CPU cycle is put to good use.


Every build you run benefits from BuildKit's intelligent parallelism, with the DAG solver constantly finding ways to save time without sacrificing correctness. Depot's auto-scaling takes it further by ensuring you always have enough builders to handle the load. Together, they make "waiting for builds" a thing of the past.


For more details on configuring build parallelism and auto-scaling in your Depot projects, see our[build parallelism documentation](https://depot.dev/docs/container-builds/build-parallelism) .


## FAQ


How does BuildKit's DAG solver determine what can run in parallel?


BuildKit parses your Dockerfile into LLB (Low-Level Build) format and creates a dependency graph of all operations. It examines each instruction to see what it depends on. If instruction B needs the output of instruction A, they run sequentially. But if instructions B and C both only depend on A, they can run simultaneously once A completes. This dependency analysis happens before any actual building starts, allowing BuildKit to create the most efficient execution plan possible.


What's the difference between BuildKit's stage parallelism and instruction parallelism?


Stage parallelism happens when you have multiple stages in a multi-stage Dockerfile that don't depend on each other. For example, a frontend build and backend build can run simultaneously on separate threads or CPU cores. Instruction parallelism occurs within a single stage, where BuildKit executes independent operations concurrently, like multiple COPY instructions that don't depend on each other. Both levels work together to speed up your builds.


If I have multiple services with the same dependencies, will BuildKit run npm install multiple times?


No, BuildKit's deduplication prevents that. It uses content-addressable storage and checksums to identify when different build contexts would produce identical layers. When multiple builds try to do the exact same thing, like running npm ci with identical package.json files, the first build computes the layer while the others wait. Once that first build completes, all waiting builds immediately use that result and move on. The work happens only once.


Does Depot's auto-scaling split work across multiple builders or does each builder run the full build?


Each builder runs full builds, not split work. When your concurrent build limit is reached, Depot provisions additional builders, with each new builder starting with a clone of the main builder's cache. This handles sudden spikes in build demand, like when multiple developers push simultaneously. The relationship between BuildKit's parallelism and Depot's auto-scaling is complementary: BuildKit optimizes execution within each builder, while auto-scaling ensures you have enough builders to handle your workload.


## Related posts


- [BuildKit in Depth: How Modern Container Builds Work](https://depot.dev/blog/buildkit-in-depth)
- [Now available: Build autoscaling for everyone](https://depot.dev/blog/build-autoscaling-now-generally-available)


Pedro Guerra


Enterprise Support Lead at Depot
