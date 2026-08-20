---
schema_version: "1.0.0"
document_id: "18bd61cefa7ded5543a252c33e5b3fb6c7ae23a47c85c181f5f5050b120d1c95"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/topk-sigmoid-optimization"
published_at: "2026-01-29T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:22:56.687510+00:00"
content_hash: "sha256:4770f82047a44929f97a4f4d15be71202849ed5f7efda19e6933d5d74e0edb45"
---

# The Year of the LLM GPU Kernel Engineer

Optimizing GPU kernels is hard. The number of people who can do it well is small. Hardware companies update their architectures every year, and with hyperscalers, chip makers, and even AI labs building custom silicon, the demand for kernel engineers keeps growing while the supply stays flat. Efforts from the GPU engineering community such as[GPUMODE](https://www.gpumode.com/v2/home) have played a massive role in using LLMs to 10x the productivity of GPU engineers.


In a similar fashion, we used an AI agent to optimize AMD’s` topk_sigmoid` kernel in its Aiter library, achieving a **9x speedup** over PyTorch and 1.6x speedup over AITER. The broader point is that this isn’t a one-off exception. We’ve been building the pieces for this, architecture docs, ISA analysis tools, iterative benchmarking, and this is our first real demonstration of the workflow we think will define GPU performance engineering going forward: AI agents doing the optimization work, while humans set the goals and constraints. This will enable GPU engineers to 10x in productivity, and solve the supply constraint.


---


## The Problem: LLMs lack a fundamental understanding of the parallel programming model GPUs operate with.


We wanted to answer: could an agent do better using architecture-specific documentation and tools?


---


## What the Agent Had Access To


Two things


1. Docs (context)


Specifically, we gave the agent access to:


- AMD GCN/CDNA ISA reference manuals
- DPP (Data Parallel Primitives) instruction specs
- MI300X architectural details (wavefront size, register files, memory hierarchy)
- GFX9-specific features like` row_bcast` instructions


1. ISA analysis (tool)


- Compile HIP kernels to code objects
- Disassemble and analyze generated ISA
- Count register usage (VGPRs, SGPRs, AGPRs)
- Identify instruction patterns (DPP moves, LDS operations, MFMA counts)
- Detect spills and stalls


With these two in hand, we put the agent in an optimization loop with the following steps:


1. Profile
2. Analyze ISA
3. Come up with hypothesis on how to get speedup
4. Implement (write code)
5. Correctness: is the solution correct? If not, give error as context and retry in a loop.
6. Benchmark: is the solution faster than baseline?
7. Repeat N times until > optimization threshold


---


## 7 Kernel Variants in One Session


Step Technique Time (μs) vs PyTorch Agent’s Rationale


0 Naive 899 0.03x “Establish correctness baseline”


1 Parallel + Atomics ~15 ~2x “Parallelize across experts”


2 Shared Memory 7.56 3.7x “Replace atomics with reduction”


3 Warp Shuffle 7.23 3.9x “Eliminate explicit shared memory”


4 DPP Basic 4.67 6.0x “ISA shows` ds_bpermute` latency; use DPP”


5 DPP Broadcast 3.33 **8.4x** “GFX9 has` row_bcast` ; eliminates` readlane` ”


6 Hand-tuned ASM 3.12 **9.0x** “Fuse max+DPP, reduce instruction count”


The key insight here came at step 4. The agent looked at the generated ISA and noticed the shuffle implementation was still going through LDS via` ds_bpermute` . Because of the context we gave it about the hardware, it proposed and implement DPP as a faster alternative.


---


## How the Agent Found DPP Broadcast


At step 4, the ISA analyzer reported:


```text
Kernel: topk_sigmoid_dpp_basic
Architecture: gfx942
VGPRs: 24, SGPRs: 16, LDS: 0 bytes


Key instructions:
v_mov_b32_dpp (quad_perm): 8
v_mov_b32_dpp (row_shr): 8
v_readlane_b32: 8        ← Agent flagged this
v_cmp_gt_f32: 6
v_cndmask_b32: 12
```


The agent’s analysis:


> “The kernel uses 8` readlane` instructions to gather results across rows. Each` readlane` is a scalar operation that serializes the wavefront. According to the GFX9 ISA documentation,` row_bcast:15_` and` row_bcast:31` can broadcast a single lane’s value to an entire row without serialization. This should eliminate 6 of the 8` readlane` calls.”


The optimization the agent implemented:


```text
// Before: 4 readlanes to gather row results
float   row0   =   __builtin_amdgcn_readlane  (val,   15  );
float   row1   =   __builtin_amdgcn_readlane  (val,   31  );
float   row2   =   __builtin_amdgcn_readlane  (val,   47  );
float   row3   =   __builtin_amdgcn_readlane  (val,   63  );


// After: DPP broadcast (GFX9 only)
val   =   __builtin_amdgcn_mov_dpp  (val, DPP_ROW_BCAST_15, ...);
val   =   __builtin_amdgcn_mov_dpp  (val, DPP_ROW_BCAST_31, ...);
```


Result: **4.67 μs → 3.33 μs** (1.4x from a single architectural insight)


---


## The Final Optimization: Fused Max Reduction


At step 6, the agent analyzed AMD’s own optimized kernels and found an interesting pattern:


> “Reference kernels use` v_max_f32_dpp_` (fused max+move) instead of separate` v_mov_b32_dpp` +` v_cmp_` +` v_cndmask` . This reduces the argmax reduction from 5 instructions to 1 instruction per step.”


```text
; Fused max reduction
v_max_f32_dpp v1, v1, v1   quad_perm:  [  1  ,  0  ,  3  ,  2  ]
v_max_f32_dpp v1, v1, v1   quad_perm:  [  2  ,  3  ,  0  ,  1  ]
v_max_f32_dpp v1, v1, v1   row_shr:  4
v_max_f32_dpp v1, v1, v1   row_shr:  8
v_max_f32_dpp v1, v1, v1   row_bcast:  15
v_max_f32_dpp v1, v1, v1   row_bcast:  31


; Find which lane had the max
v_readlane_b32 s0, v1,   63
v_cmp_eq_f32 vcc, v1, s0
s_ff1_i32_b64 s1, vcc
```


**30 instructions → 9 instructions** per reduction.


> Note: the final fused reduction was not part of the PR submitted to AITER for code clarity. Please contact the Wafer team for the source code.


---


## Production Results


We integrated the step 5 kernel into AITER with architecture-aware dispatch:


```text
void   topk_sigmoid  (...) {
if   (  isGPUArch  ({  "gfx9"  })) {
topk_sigmoid_gfx9  (...);    // DPP-optimized
}   else   {
topk_sigmoid_ck  (...);      // CK fallback
}
}
```


## Headline Performance


Across 40 benchmark configurations (64 / 128 experts, fp16 & bf16, multiple token sizes and Top-K values), the DPP kernel delivers a consistent and substantial improvement over the existing CK implementation:


#### Full Benchmark Results (64 Experts)


Tokens Top-K DType CK (μs) DPP (μs) Speedup


256 4 fp16 4.19 2.57 1.63×


256 8 bf16 5.44 3.03 1.80×


512 4 fp16 3.98 2.52 1.58×


1024 8 bf16 5.71 2.97 1.92×


2048 4 fp16 3.93 3.01 1.31×


4096 8 bf16 7.33 4.61 1.59×


#### Full Benchmark Results (128 Experts)


Tokens Top-K DType CK (μs) DPP (μs) Speedup


256 4 fp16 5.04 2.80 1.80×


256 8 bf16 6.62 3.59 1.84×


512 4 fp16 4.64 2.73 1.70×


1024 8 bf16 6.66 3.80 1.75×


2048 4 bf16 4.76 3.10 1.54×


4096 8 fp16 8.48 5.53 1.53×


---


## Code


The optimized kernel is fully open source:


[View the Pull Request on GitHub](https://github.com/ROCm/aiter/pull/1909)


---


## What This Means


The journey from naive hip to hand-tuned GCN assembly took hours for the agent to find and implement. This is a first discovery in what we argue is a complete revolution in how performance engineering is done. The agent found architecture-specific optimizations (DPP broadcast, fused max reduction) by reading ISA docs and analyzing generated assembly, the same process a human expert would follow, just 10-100x faster.


This is the workflow we’re building toward: agents that can navigate the GPU optimization search space while humans set the goals. The kernel engineer shortage isn’t going away, but agents can multiply what each engineer can accomplish.


If you want to try agent-assisted kernel optimization on your workloads, talk to us:


- [Book a Demo](https://cal.com/wafer/20min)
- [Express Interest](https://www.wafer.ai/cdn-cgi/l/email-protection#e48c8da49385828196ca858d)


[Follow our research](https://www.linkedin.com/company/wafer-ai/)
