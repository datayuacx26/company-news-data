---
schema_version: "1.0.0"
document_id: "fe67690bc3277fb9ef53c995b6152d8019cd3b261a4b4368ca46985811e4a6ee"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/where-did-my-microseconds-go"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:0d8fd6c20ae812701a9f247a5a1b4fa3323dae2774b49ac45ada4d762bce92a4"
---

# Where Did My Microseconds Go?

## What We Noticed


We were running[WaferBench NVFP4](https://www.kernelarena.ai/eval?suite=waferbench-nvfp4) , a suite of six fused inference kernels (Add+RMSNorm, SiLU+Mul, NVFP4 Quantize) benchmarked against[FlashInfer](https://github.com/flashinfer-ai/flashinfer) on NVIDIA B200. Four frontier models, 24 kernel submissions, measured with the[ThunderKittens 2.0](https://hazyresearch.stanford.edu/blog/2026-02-19-tk-2) benchmarking convention: 500 warmup iterations, 100 timed reps, L2 cache cycling, CUDA event timing.


While reviewing the harness, we noticed something:` compare.py` benchmarks the FlashInfer reference *first* , then calls` cpp_extension.load()` to JIT-compile the custom kernel, then benchmarks the custom kernel. On the surface this is fine; both use the same warmup budget and run on the same GPU. But we started wondering: what if they’re not actually running in the same CPU state?


## The Problem


` cpp_extension.load()` shells out to` nvcc` to compile your` .cu` file. On a B200 with a non-trivial kernel, that takes ~30 seconds. During that time the Linux scheduler migrates your Python process to a different CPU core.


You can watch it happen:


```text
import   ctypes, os
_libc   =   ctypes.CDLL(  "libc.so.6"  )


cpu_before   =   _libc.sched_getcpu()
silu_fp4   =   cpp_extension.load(  ...  )
cpu_after   =   _libc.sched_getcpu()


# core 255 → 236
```


Here’s why that matters. Every time CUDA dispatches a kernel, the CPU reads a set of internal data structures to set up the launch. Those structures live in the CPU cache of whatever core you’re running on. When the scheduler moves you to a different core, that core’s cache is empty; every kernel launch pays the cost of pulling those structures back from main memory until the cache warms up.


Load a second extension (another migration: core 236 → 221) and the working set gets evicted again.


Our NVFP4 suite uses 500 warmup iterations, which is enough to recover once the L3 is hot. But the pattern is common in kernel development and automated evaluation, and it’s easy to get wrong. Here’s a concrete example.


We ran our SiLU+Mul+FP4Quant kernel (B=8, M=256, K=7168) on a B200 in the typical “reference-first” pattern: measure a reference kernel, then` cpp_extension.load()` the custom kernel, then measure it.


```text
# Scenario: reference-first benchmark pattern
Reference (warm, before load):     16.39 µs   core 255
-- cpp_extension.load() --                   core 255 → 118 (MIGRATED)
Custom kernel (warmup=0):          23.64 µs   ← 28% inflated
Custom kernel (warmup=10):         18.45 µs   ← recovered
Custom kernel (warmup=100):        18.45 µs
```


With zero warmup after loading, the custom kernel appears 44% slower than the reference, even though at steady state it’s actually 12% *faster* . With just 10 warmup iterations the effect vanishes. But if your harness uses low warmup, or if you’re iterating during development and benchmarking right after` cpp_extension.load()` , every number you see is wrong.


The effect gets worse with multiple extensions. In a multi-kernel suite that loads several` .cu` files before benchmarking, each load is another potential migration, another cache eviction:


```text
# Two extensions loaded, then benchmark first kernel
Loaded ext A:   core 118 → 226 (MIGRATED)
Loaded ext B:   core 226 → 205 (MIGRATED)
Custom (warmup=0):   19.20 µs   ← still inflated
Custom (warmup=10):  18.46 µs   ← recovered
```


Pinning the core eliminates it entirely:


```text
# Same kernel, core pinned during load
Extension loaded:   core 118 → 118  (no migration)
Custom (warmup=0):  18.89 µs   ← only 2% over steady state
Custom (warmup=10): 18.45 µs
```


## What To Call This


Is this a reward hack? Not in the traditional sense; no model is gaming anything. We’re calling it a **benchmark blind spot** : something the harness doesn’t account for that quietly inflates or deflates scores without any adversarial intent.


It belongs in the same catalog as our other documented benchmark failure modes, because the effect on the leaderboard is the same: wrong numbers produce wrong rankings. We’ve added it to the[WaferBench NVFP4 reward hacks catalog](https://www.kernelarena.ai/eval?suite=waferbench-nvfp4) on KernelArena.


## The Fix


Pin your CPU core before loading extensions:


```text
import   ctypes, os
_libc   =   ctypes.CDLL(  "libc.so.6"  )


# pin before ANY compilation starts
cpu   =   _libc.sched_getcpu()
os.sched_setaffinity(  0  , {cpu})


# load everything: no migrations during nvcc
silu_fp4   =   cpp_extension.load(
name  =  "silu_fp4"  ,
sources  =  [  "silu_mul_fp4.cu"  ],
extra_cuda_cflags  =  [  "-O3"  ,   "-gencode=arch=compute_100a,code=sm_100a"  ],
)
add_rmsnorm_fp4   =   cpp_extension.load(
name  =  "add_rmsnorm_fp4"  ,
sources  =  [  "add_rmsnorm_fp4.cu"  ],
)


# unpin: let the scheduler work normally again
os.sched_setaffinity(  0  ,   set  (  range  (  256  )))
```


With pinning, the dispatch working set never goes cold:


```text
unpinned    pinned
warmup=0       36.07µs   19.34µs     ← cold-start goes from 2x to 5% overhead
warmup=100     18.49µs   18.46µs     ← both at steady state
warmup=500     18.48µs   18.47µs
```


If you can’t pin (shared cluster, container with restricted permissions), use at least 5000 warmup iterations after loading all extensions before you start measuring.


## Check Your Harness


We updated` compare.py` in the NVFP4 suite to pin the CPU core around all` cpp_extension.load()` calls and increased the default warmup to 5000 iterations. The fix is three lines. The investigation was 27 experiments.


The broader point: if you’re building kernel benchmarks or evaluating LLM-generated kernels at scale, the models might not be cheating. Your infrastructure might be. The most dangerous blind spots aren’t the ones that produce obviously wrong numbers. They’re the ones that produce numbers that are wrong by just enough to shift rankings without anyone noticing.


We’ve added this to the[WaferBench NVFP4 reward hacks catalog](https://www.kernelarena.ai/eval?suite=waferbench-nvfp4) on KernelArena as a documented benchmark blind spot. If you find something similar in your own setup, we’d love to hear about it.


## Files


The kernel and reproduce scripts used in this post. Drop them on any Linux machine with a GPU and` torch.utils.cpp_extension` available.


- silu_mul_fp4quant_kernel.cu - Fused SiLU+Mul+NVFP4Quant kernel (Blackwell)
- benchmark_warmup.py - Reproduce the warmup experiment
- benchmark_gpu_events.py - Prove the overhead is host-side
