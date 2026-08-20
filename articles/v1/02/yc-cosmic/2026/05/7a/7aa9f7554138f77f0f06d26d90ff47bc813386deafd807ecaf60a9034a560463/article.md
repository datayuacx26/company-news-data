---
schema_version: "1.0.0"
document_id: "7aa9f7554138f77f0f06d26d90ff47bc813386deafd807ecaf60a9034a560463"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/bun-rust-rewrite-javascript-runtime"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:ddd7c123b3e54b1056e359476d641bbbcb930e625c1967b5e7afbf71c0e7157e"
---

# Why Bun is Rewriting in Rust (And What It Means for JavaScript Developers)

Bun started as a bet that JavaScript tooling could be much faster. That bet paid off. Now, Bun is making another one: rewriting its runtime in Rust. Here's everything you need to know about why, where things stand, and what it means if you're building JavaScript applications today.


## What Is Bun and Why It Matters


Bun is an all-in-one JavaScript toolkit. It combines a fast JavaScript runtime, a bundler, a package manager, a test runner, and a transpiler into a single binary. If you've been frustrated by Node.js startup times, slow runs, or juggling separate tools for bundling and testing, Bun was built for exactly those pain points.


The numbers are striking. According to benchmarks published on[bun.sh](https://bun.sh/) , Bun handles 59,026 Express.js "hello world" HTTP requests per second on Linux x64, compared to 25,335 for Deno and 19,039 for Node.js. For WebSocket throughput, Bun clocks 2,536,227 messages per second against Deno's 1,320,525 and Node's 435,099.


Bun also bundles 10,000 React components in 269ms. Rolldown completes the same job in 495ms. esbuild takes 572ms.


These aren't hypothetical gains. Companies including X (Twitter) and Midjourney run Bun in production. Tailwind's standalone CLI is compiled with Bun. Claude Code, Anthropic's AI coding assistant, ships as a Bun executable to millions of users. That last point matters a lot for where Bun is headed (more on that shortly).


## Why Bun Was Originally Written in Zig


Bun's creator, Jarred Sumner, started the project in 2021. He was building a voxel game and got frustrated with Next.js hot reload times. He began porting a JSX and TypeScript transpiler from Go to Zig, a systems programming language that compiles to native binaries and offers fine-grained memory control without a garbage collector.


Zig was a pragmatic choice for a small team obsessed with performance. It gave Bun low-level control over memory layout and system calls, which directly contributed to the benchmark advantages the runtime is known for. Bun shipped its 1.0 release in September 2023 and its 1.2 release added built-in PostgreSQL, S3, and Redis clients.


But Zig is a young language with a small ecosystem. Finding contributors who know Zig well is harder than finding Rust developers. The Rust ecosystem, by contrast, is large, mature, and has become the dominant language for systems-level rewrites across the industry (see: the Linux kernel, Android, Firefox, and much of the cloud-native toolchain).


## The Decision to Rewrite in Rust


The Rust rewrite is still described as experimental. Jarred Sumner[announced the milestone on Hacker News](https://news.ycombinator.com/item?id=48073680) in what became one of the more closely-watched threads in the JavaScript runtime community this year.


The core reasons for the rewrite come down to three things:


**Contributor accessibility.** Rust has a vastly larger developer community than Zig. More contributors means faster bug fixes, more platform support, and a healthier open-source ecosystem around Bun. As Bun's usage grows, this matters more.


**Maintainability at scale.** Bun's codebase has grown substantially. Rust's ownership model and strong type system provide compile-time safety guarantees that help catch bugs before they reach users. Zig's memory safety story, while improving, isn't at the same maturity level.


**Long-term stability signals.** Bun was acquired by Anthropic in late 2025. As[Jarred wrote on the Bun blog](https://bun.sh/blog/bun-joins-anthropic) , Claude Code ships as a Bun executable to millions of users. If Bun breaks, Claude Code breaks. That stakes profile demands a codebase that's as battle-hardened as possible. Rust fits that requirement better than Zig does today.


None of this means Zig was the wrong choice early on. Zig enabled the team to move fast and ship a highly performant runtime. The rewrite is a maturation decision, not a repudiation of the original approach.


## Where the Rewrite Stands Today


The headline number: the Rust rewrite has achieved **99.8% test compatibility on Linux x64 glibc** .


This figure comes from Jarred Sumner's own announcement[on Hacker News](https://news.ycombinator.com/item?id=48073680) and was also reported in[Cosmic's developer rundown](https://www.cosmicjs.com/blog/cosmic-rundown-bun-rust-rewrite-aws-frustrations-github-decline) when the milestone was first reached.


99.8% compatibility is a meaningful threshold. It means the Rust rewrite, running Bun's existing test suite, behaves identically to the Zig implementation in the overwhelming majority of cases. The 0.2% gap represents edge cases and platform-specific behaviors, not fundamental incompatibilities.


The rewrite is currently scoped to Linux x64 glibc. macOS, Windows, and ARM support come later. This is a standard approach for large runtime projects: nail the primary server deployment target first, then expand.


Bun's current stable release series (v1.3.x at the time of writing) continues to ship from the Zig codebase. The Rust rewrite is parallel, experimental work. When it reaches feature and compatibility parity across all supported platforms, it will become the default.


---


Want to build AI-powered content workflows? Cosmic gives your agents a structured, versioned content store with a REST API, TypeScript SDK, and built-in analytics. See what your agents produce and whether it worked.[Start for free, no credit card required.](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=in-article-signup-cta)


---


## Performance Implications: What Rust Means for Speed, Memory, and Maintainability


The most common question is whether Rust will be faster or slower than Zig. The honest answer is: it depends, and the gap is smaller than most people assume.


Both Zig and Rust compile to native machine code. Neither has a garbage collector. Both allow the same class of low-level optimizations. The performance difference between a well-written Rust program and a well-written Zig program is typically within a few percent.


The real performance benefits of the rewrite are likely to come from:


**A larger contributor pool finding and fixing performance regressions.** More eyes on the code means hot paths get optimized faster.


**Rust's ecosystem of performance-focused crates.** Libraries like Rayon for data parallelism and Tokio for async I/O are battle-tested in production systems. The Bun team can leverage these rather than building equivalent infrastructure from scratch in Zig.


**Memory safety eliminating a class of bugs.** Rust's ownership model prevents use-after-free, double-free, and data race bugs at compile time. These are the kinds of bugs that cause random slowdowns and crashes in production. Fewer bugs in the runtime means more consistent performance for your applications.


What won't change: Bun's use of JavaScriptCore (WebKit's JavaScript engine) for actually executing your JavaScript code. The runtime rewrite is the layer that wraps and orchestrates JavaScriptCore, handles I/O, implements Node.js APIs, and runs the bundler and package manager. The JS execution engine itself remains the same.


## What Changes for JavaScript Developers Using Bun


For most JavaScript developers, the Rust rewrite won't require any changes to your code or workflow. Bun's explicit goal is 100% backward compatibility. The command-line interface, the API surface, and the Node.js compatibility layer will all remain the same.


What you can expect to improve over time:


- **Fewer edge-case bugs.** Especially on non-Linux platforms where the Zig implementation has historically had more rough edges.
- **Faster patch releases.** A more accessible codebase means contributors can ship fixes sooner.
- **Better cross-platform parity.** Rust's cross-compilation story is mature and well-tooled.
- **Increased confidence for production adoption.** The combination of 99.8% test compatibility and Anthropic's backing makes Bun a more defensible choice for teams evaluating runtimes.


If you're already using Bun, you don't need to do anything differently. If you've been holding off because you weren't sure Bun was production-stable, the rewrite milestone and Anthropic acquisition are meaningful evidence that the project has a credible long-term trajectory.


### A quick example: using Bun with Cosmic


If you're building with Bun today, integrating Cosmic is straightforward. Install the TypeScript SDK:


```text


```


Then initialize your client:


```text


```


This runs identically on Bun, Node.js, and Deno. Cosmic's REST API and TypeScript SDK don't depend on any runtime-specific APIs, so you get the same behavior regardless of which runtime you're targeting.


## What's Next: Timeline, Open Questions, and Community Reaction


The timeline for the Rust rewrite becoming the default Bun binary is not publicly committed. The team has said they'll continue work in parallel with the existing Zig-based releases. The 99.8% compatibility milestone on Linux x64 is the first major public checkpoint. The likely next steps are:


1. Closing the remaining 0.2% compatibility gap on Linux x64
2. Expanding platform coverage to macOS (both x64 and ARM)
3. Windows support
4. A public beta period before the Rust binary becomes the default release


Community reaction has been largely positive, though not without questions. Zig has its own dedicated community, and some contributors to Bun have expressed interest in keeping the Zig codebase maintained as a fork. Whether that happens at any meaningful scale remains to be seen.


The bigger context is Anthropic's acquisition. Bun now has institutional backing from one of the best-funded AI labs in the world. Anthropic has direct incentive to keep Bun excellent because Claude Code depends on it. That financial and organizational support changes the risk calculus for anyone evaluating Bun for long-term production use.


For a broader look at how this fits into the wider developer ecosystem news, see[Cosmic's original Bun Rust rewrite rundown](https://www.cosmicjs.com/blog/cosmic-rundown-bun-rust-rewrite-aws-frustrations-github-decline) .


## The Bottom Line


Bun's Rust rewrite is a deliberate maturity move by a team that has already proven it can execute. The 99.8% test compatibility milestone is real progress, the Anthropic acquisition provides genuine long-term stability, and the move to Rust opens Bun's development to a much larger contributor community.


For JavaScript developers, the message is straightforward: Bun was already fast. The Rust rewrite is about making it more reliable, more maintainable, and more accessible to contributors, while keeping the performance characteristics that made it worth paying attention to in the first place.


The runtime wars aren't over, but Bun just made a significant move.


---


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)
