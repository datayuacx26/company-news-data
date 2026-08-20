---
schema_version: "1.0.0"
document_id: "9319e472e87d38d39196941165f4ed2a777eda48abed1b321d8616443e2e87a5"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/wafer-cli"
published_at: "2026-01-20T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:308552be8aad2bddb2fcffea72e0732700571be8de94cd6576112266b43f7cc7"
---

# wafer-ai CLI: GPU Superpowers for Your Coding Agent

## The Problem


AI coding assistants are increasingly responsible for more and more lines of code output, and kernels are no different. But users typically run their coding agents on their local development environment or laptop, or inside a sandbox, not attached to a GPU.


So you end up having to:


1. Write the kernel optimization
2. Sync file to GPU box
3. Run it, profile it, sync results back
4. Read the profiler output yourself, or paste a mess into the chat
5. Correct your agent about its out-of-date knowledge
6. Repeat…


And if you don’t have your own always-on GPU, you’re stuck paying for idle time or waiting for the startup time of GPU providers.


## The Solution


**wafer-ai CLI** is a command-line tool that gives your coding agent direct access to GPU development primitives. With the right onboarding, your agent can:


### 1. Query GPU documentation with citations


```text
wafer   agent   -t   docs   "What causes shared memory bank conflicts?"
```


Instead of “Claude thinks it remembers something about 32-byte accesses,” you get answers grounded in the actual CUDA documentation, with citations you can verify.


### 2. Analyze performance traces


```text
wafer   agent   -t   trace-analyze   --args   trace=./profile.ncu-rep   "Why is this kernel slow?"
```


Point it at an NCU report, NSYS trace, or PyTorch profiler output. The agent reads the trace data and tells you where the bottlenecks are: memory bound, compute bound, warp divergence, whatever it finds.


Your agent can also query traces directly and write its own SQL:


```text
wafer   tool   perfetto   query   trace.json   \
"SELECT name, dur/1e6 as ms FROM slice WHERE cat='kernel' ORDER BY dur DESC LIMIT 10"
```


### 3. Evaluate kernels on remote GPUs


```text
wafer   tool   eval   \
--impl   ./kernel.py   \
--reference   ./reference.py   \
--test-cases   ./tests.json   \
--target   my-gpu   \
--benchmark
```


This runs your kernel on a real GPU, checks correctness against a reference implementation, and measures speedup. Your agent can use this in a loop: write code, evaluate, see results, iterate.


We support GPUMode and Kernelbench formats for specifying kernels and test cases.


**wafer tool eval doesn’t need a GPU on your local machine.** wafer-ai handles the remote execution.


## Talk to the Team


- [Book a Demo](https://cal.com/wafer/20min)
- [Contact the team](https://www.wafer.ai/cdn-cgi/l/email-protection#620a0b2215030407104c030b)


We can help your team get set up and map wafer-ai workflows into your existing agent stack.


## Why This Matters


The future of kernel development is AI-assisted. But today’s agents are flying blind: no access to real hardware, no grounding in documentation, no ability to profile and iterate.


wafer-ai bridges that gap. Your agent gets the same tools you use, automated and accessible from the command line.


**Give your coding agent GPU superpowers.**


- [Book a Demo](https://cal.com/wafer/20min)


## We’d love your feedback!


Would love to hear from the community. What features would help your workflow? What’s missing? What’s broken?


Reach out to us at[\[email protected\]](https://www.wafer.ai/cdn-cgi/l/email-protection#9bf3f2dbecfafdfee9b5faf2) or find us on[Twitter/X](https://x.com/wafer_ai) .
