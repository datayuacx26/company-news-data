---
schema_version: "1.0.0"
document_id: "a94350042f0c243760cfe0c5f4d2257be9f0b2fbcb8a3517620968aa19fdfdad"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/nordlys-case-study"
published_at: "2026-02-01T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:816a67ff3e3bff764760ac6b6527a279baddb066d59a11e43d41498f667106e3"
---

# Nordlys Labs: 8x Faster Routing with Wafer-Guided Kernel Optimization

A non-kernel expert hit 8× on latency-critical CUDA clustering with Wafer profile-guided optimization.


[View the Pull Request on GitHub](https://github.com/Nordlys-Labs/nordlys/pull/821)


[Nordlys Labs](https://nordlyslabs.com/) builds Hypernova, a Mixture of Models router that dynamically selects the optimal LLM for each coding task. Their insight: no single model wins everywhere. On SWE-bench, they found 65 tasks that Opus failed were solved by other models. So instead of betting on one model, Hypernova embeds incoming prompts, clusters them, and routes to whichever model performs best for that cluster.


The clustering kernel runs on every single request. Routing latency adds directly to user-perceived inference time. Starting point: ~180 microseconds per clustering operation. After optimization with Wafer: 21.7 microseconds. That’s an **8x speedup** .


The most interesting part: the engineer who did this optimization described themselves as “not a kernel dev in the slightest.”


## The Workflow


The optimization loop was simple:


> “optimize the kernel and keep consulting wafer and don’t stop until wafer is happy”


This is profile-guided optimization in practice. Instead of guessing which optimizations might help, Wafer’s NCU profiling pinpointed exactly what was bottlenecking the kernel at each iteration. The agent kept iterating until the profiler showed no obvious remaining issues.


## What the Profiler Found


Each profiling run revealed specific problems that mapped to specific fixes:


**Low memory bandwidth utilization** (` dram__throughput` < 50%) pointed to uncoalesced memory access. The fix: float4 vectorized loads.


```text
int vec_n = dim / 4;
const float4* q4 = reinterpret_cast<const float4*>(query);
const float4* c4 = reinterpret_cast<const float4*>(cent);
for (int i = 0; i < vec_n; ++i) {
float4 qv = __ldg(&q4[i]);
float4 cv = __ldg(&c4[i]);
dot += qv.x*cv.x + qv.y*cv.y + qv.z*cv.z + qv.w*cv.w;
}
```


**Kernel launch overhead** visible in the timeline showed gaps between kernels. The fix: fuse the distance calculation and argmin into a single kernel.


```text
__global__ void __launch_bounds__(256, 2)
find_nearest_centroid(
const T* __restrict__ query,
const T* __restrict__ centroids,
const T* __restrict__ centroid_norms,
int* __restrict__ out_idx,
T* __restrict__ out_dist,
int n_clusters, int dim
)
```


**Shared memory bank conflicts** (` l1tex__data_bank_conflicts_pipe_lsu` high) indicated the shared memory layout was causing serialization. The fix: padding to avoid bank conflicts.


```text
inline constexpr int kSharedMemPadding = 33;  // 32 + 1 for bank conflicts
```


**Poor occupancy** (` sm__warps_active` low) suggested the compiler needed hints. The fix: explicit launch bounds.


The kernel also adopted cooperative groups for modern warp-level reductions:


```text
auto warp = cg::tiled_partition<32>(cg::this_thread_block());
return cg::reduce(warp, val, cg::plus<T>());
```


## Results


On an RTX 3070 with 100 clusters and 1536-dimensional embeddings:


- **Single query latency:** 21.7 microseconds (down from ~180 microseconds)
- **Batch throughput:** 8.5 million queries per second (180x improvement over naive batching)


The final kernel includes NVTX profiling markers, comprehensive error handling, and a full test suite. Production-ready code, not a benchmark artifact.


## Why This Matters


The kernel expert shortage isn’t going away. But the workflow demonstrated here shows a path forward: profile-guided iteration with an AI agent, where the profiler acts as the oracle for whether optimizations are working.


The key insight from Wafer’s perspective: raw NCU counters are data, but translating them into “what to fix next” is where the value lies. When the profiler said memory bandwidth was underutilized, the agent knew to try vectorized loads. When it showed bank conflicts, padding was the answer. Each metric mapped to a known optimization pattern.


This is about making kernel expertise accessible. Someone who knows the patterns can encode them into tools. Someone who doesn’t can still apply them through guided iteration.


## Try It Yourself


Wafer integrates with Claude Code and other agentic coding environments. The same profile-guided workflow that achieved 8x on this clustering kernel works on any CUDA or HIP kernel.


- [Book a Demo](https://cal.com/wafer/20min)
- [Express Interest](https://www.wafer.ai/cdn-cgi/l/email-protection#7e16173e091f181b0c501f17)
- [Follow our research](https://www.linkedin.com/company/wafer-ai/)
