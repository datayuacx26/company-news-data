---
schema_version: "1.0.0"
document_id: "187467078b305438203934690a5fac64c76a7c8744ea753600e8e80e18b7f062"
company_key: "yc-rivet"
company: "Rivet"
source_id: "yc-rivet-news-import-b9b70e7746c3"
canonical_url: "https://rivet.dev/blog/2026-07-26-sandboxes-vs-webassembly-lambda-vs-workers-round-two/"
published_at: "2026-07-26T00:00:00+00:00"
first_seen_at: "2026-07-31T19:11:06.802677+00:00"
fetched_at: "2026-07-31T19:11:08.509085+00:00"
content_hash: "sha256:5f6e5b47035c913a5de5384351231157028464eba7a51a9857b1d2c981e82c0b"
---

# Sandboxes vs WebAssembly → Lambda vs Workers, Round Two

##


The great serverless wars of the 2010s


The two front runners leading the serverless wars were Cloudflare Workers and AWS Lambda. They were powered by fundamentally different technologies:


- Lambda with[microVMs](https://dev.to/koyeb/what-is-a-microvm-1a2b)
- Cloudflare Workers with[V8 isolates](https://developers.cloudflare.com/workers/reference/how-workers-works/#isolates)


AWS optimized Lambda for the wrong bottleneck: backends spend most of their time idle, waiting on databases and API calls, but a **Lambda serves one invocation at a time** , reserving lots of unused memory and CPU. The entire microVM sits idle while it waits on the database.


Workers made serverless efficient by changing what that idleness costs. An idle isolate is a few megabytes of heap in a shared process, **not a reserved VM for each request** . The CPU it isn’t using goes to other isolates in the same process.


This way, one host holds thousands of isolates serving requests instead of dozens of microVMs.


Cloudflare made the same comparison in[Cloud Computing without Containers](https://blog.cloudflare.com/cloud-computing-without-containers/) .


##


Sandboxes are repeating history


It turns out that **agents and sandboxes behave a lot like traditional backends** .


They **sit mostly idle** while waiting on inference, user input, or running simple file operations and scripts. Just like how backends sit idle waiting for databases and API requests.


The mainstream solution today for running agents is using sandboxes like Modal, Daytona, and Vercel sandboxes, each of which has a **container-per-agent architecture** , exactly like AWS Lambda.


With[agentOS](https://agentos-sdk.dev/) , we’re building an open-source Cloudflare Workers-like architecture by using WebAssembly and V8 isolates to provide a Linux-compatible OS.


You’re able to use **significantly fewer resources by not reserving an entire container per agent** . Instead, you’re only using resources when the agent itself is busy.


For example, a simple shell workload costs about 22 MB of RAM in agentOS, against the 1 GiB a sandbox reserves whether the agent is actively working or waiting on inference.


##


Linux-compatible OS in WebAssembly


The innovation behind agentOS is that it **compiles native Linux software to WebAssembly** . If it works in a sandbox, it probably works in agentOS.


agentOS supports native bash, git, duckdb, sqlite, and more, all cross compiled to WASM. The agentOS kernel supports **POSIX-compliant filesystems, process trees, and sockets** . You can run dev servers on it, data analytics, Node.js (full JIT), Python, the whole gamut with significantly fewer resources.


It turns out that few production agents do niche dev work that requires true x86 or memory-intensive workloads that require an entire Linux container. Think about ChatGPT Work and Claude: **most work is just spreadsheets, data processing, and API calls** . All of that is a cakewalk for WebAssembly and V8.


You can also run native Claude Code, Codex, OpenCode, and Pi inside of agentOS, or bring your own harness.


##


An OS as a library, not as extra infrastructure


By leveraging WebAssembly, agentOS is able to run as a library in your existing backend, the same way you’d drop in Vercel’s AI SDK, which simplifies infrastructure and cuts cost.


agentOS as a library also lets you have much more control over permissions, outbound middleware, and limits on what your agents can do.


##


Is it secure?


Yes.


agentOS is built on the same isolation as Chrome and Cloudflare Workers, both in terms of how we use V8’s isolation, Spectre mitigations, process isolation, and resource limits.


Read more about our[security model](https://agentos-sdk.dev/docs/security-model/) .


##


Sandboxes and the long tail of workloads


There are some workloads where you do need a heavy x86 Linux sandbox.


For example, if you’re doing “heavy” dev work like compiling Rust or using legacy software, sandboxes are still key.


agentOS provides[sandbox mounting](https://agentos-sdk.dev/docs/sandbox/) which lets agents escalate to a heavy Linux VM on demand if it detects it has one of these workloads.


In practice, this lets you default to using a lightweight WebAssembly OS with agentOS for the majority of cheap workloads and upgrade to a sandbox only when needed, without compromise.


##


Is your use case supported?


Probably.


Our[DMs](https://x.com/nathanflurry) ,[Discord](https://rivet.dev/discord) , and[GitHub](https://github.com/rivet-dev/agentos) issues are open. Shoot us a message if you have any questions or find a workload that we can help support better.


##


Getting started


We’re seeing public companies and startups swapping out sandboxes for agentOS for flexibility and cost, for everything from AI-generated apps to personal assistants to RL environments.


```text
npm   install   @rivet-dev/agentos


```


- **GitHub** :[github.com/rivet-dev/agentos](https://github.com/rivet-dev/agentos)
- **Documentation** :[agentos-sdk.dev](https://agentos-sdk.dev/)
- **Discord** :[rivet.dev/discord](https://rivet.dev/discord)
