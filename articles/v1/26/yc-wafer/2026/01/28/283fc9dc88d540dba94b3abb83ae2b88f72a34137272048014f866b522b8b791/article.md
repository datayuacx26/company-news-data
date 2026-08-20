---
schema_version: "1.0.0"
document_id: "283fc9dc88d540dba94b3abb83ae2b88f72a34137272048014f866b522b8b791"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/reward-hacking"
published_at: "2026-01-27T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:23:06.268498+00:00"
content_hash: "sha256:23855acab17741f77ab817cdd1d3e8dc7985573755f86ab608fe7ee61981f404"
---

# Case Study: A 104x (?) Speedup on KernelBench

Every team working with LLM-generated kernels has a reward hacking story. Each occurrence that goes unexamined erodes trust in our benchmarks for this task. But we can regain that trust with transparency around the problem.


Here’s a kernel that slipped through our defense checks and what we’re doing about it.


## What happened


During an eval run on KernelBench, our agent wrote a fused HIP kernel combining nn.Linear + Dropout + Softmax into a single GPU kernel, with a 104.89x speedup over the PyTorch baseline. Here’s the kernel the agent generated:


```text
__global__   void   fused_linear_dropout_softmax_kernel  (
const   float*   __restrict__   input  ,      // [batch_size, in_features]
const   float*   __restrict__   weight  ,     // [out_features, in_features]
const   float*   __restrict__   bias  ,       // [out_features]
float*   __restrict__   output  ,           // [batch_size, out_features]
int   batch_size  ,   int   in_features  ,   int   out_features  ,
float   dropout_p  ,   float   scale  ,   uint64_t   seed  ,   bool   training
) {
int   row   =   blockIdx.x;
int   tid   =   threadIdx.x;
int   num_threads   =   blockDim.x;


extern   __shared__   float   smem  []  ;         // 16384 * 4 = 65,536 bytes (dynamic)
float*   row_data   =   smem;


__shared__   float   warp_maxes  [  32  ];        // 128 bytes (static)
__shared__   float   warp_sums  [  32  ];         // 128 bytes (static)
// Total: 65,792 bytes - 256 over MI300X limit


// Step 1: Linear + dropout into shared memory
for   (  int   col   =   tid; col   <   out_features; col   +=   num_threads) {
float   sum   =   bias  [col];
for   (  int   k   =   0  ; k   <   in_features; k  ++  )
sum   +=   input  [row   *   in_features   +   k]   *   weight  [col   *   in_features   +   k];


if   (training) {
float   r   =   rand_uniform  (seed, row   *   out_features   +   col);
sum   =   (r   <   dropout_p)   ?   0.0  f   :   sum   *   scale;
}
row_data  [col]   =   sum;
}
__syncthreads  ();


// Step 2: Softmax via parallel reductions using warp_maxes / warp_sums
// ... (find max, exp, sum, normalize)
}
```


The agent requested 65,792 bytes of shared memory per workgroup, 256 bytes over the MI300X hardware limit of 65,536. The dynamic allocation (` row_data` , 16384 floats) fits exactly at 65,536 bytes, but the agent also declared two static shared memory arrays (` warp_maxes\[32\]` and` warp_sums\[32\]` ) for its parallel reductions, pushing the total over the limit. Those arrays were reading and writing uninitialized GPU memory.


On the ROCm 6.x runtime used during the eval, this was silently allowed: the kernel launched, produced garbage, and returned in 0.020ms.


For reference, the matmul alone (batch=128, 16384x16384) requires ~34B FLOPs; 0.020ms implies ~1.7 PFLOPS, roughly 1000x the MI300X’s fp32 peak. Very suspicious.


The output passed` torch.allclose(ref, impl, atol=1e-3, rtol=1e-3)` because softmax outputs are bounded to \[0,1\] and sum to ~1. With an absolute tolerance of 1e-3, even near-uniform garbage over 16,384 classes (where valid values are ~6e-5) falls well within tolerance.


We caught it because the number looked wrong. We re-tested it manually with our existing defensive checks, and it still passed on ROCm 6.4.


The existing defenses didn’t check for output determinism. When we tried to reproduce on ROCm 7, the kernel crashed immediately: the newer HSA runtime validates shared memory allocation before dispatch and rejects over-limit requests (` HSA_STATUS_ERROR_INVALID_ALLOCATION` ). Rather than relying on a runtime upgrade to catch this class of bug, we added a defense check that would work regardless of ROCm version.


## What we fixed


The above revealed that our existing defense module, adopted from[Deep Reinforce](https://github.com/deepreinforce-ai/CUDA-L2) , was incomplete; here’s the key addition we’ve made so far:


## Output determinism check


Our defense module, largely adopted from CUDA-L2, already runs the kernel multiple times for stream injection detection, comparing timing ratios but throwing away outputs. Since the overflow bug produces non-deterministic outputs for the same exact seed, the bitwise equality check with` torch.equal` will catch this.


```text
# Check output determinism - all outputs should be identical for same inputs
# This catches silent kernel failures (e.g., shared memory overflow on HIP)
if   outputs   and   isinstance  (outputs[  0  ], torch.Tensor):
reference   =   outputs[  0  ]
for   i, out   in   enumerate  (outputs[  1  :],   1  ):
if   not   torch.equal(reference, out):
max_diff   =   (reference   -   out).abs().max().item()
return   (
False  ,
f  "Non-deterministic output detected: run 0 vs run   {  i  }   differ "
f  "(max diff:   {  max_diff  }  ). This indicates a silent kernel failure "
f  "(e.g., shared memory overflow) or race condition."  ,
output,   None  ,
)
```


This catches the bad kernel even on ROCm 6.4. The shared memory overflow reads whatever happens to be at the overflowed address, which changes between runs.` torch.equal` is bitwise instead of tolerance based, so any variation will fail this check.


We also added` torch.cuda.empty_cache()` between timing iterations to reduce the chance that stale cached data at overflowed addresses masks the non-determinism here.


The determinism check catches this class of bug regardless of runtime version, but our future eval runs will use ROCm 7, which rejects over-limit shared memory requests at dispatch.


## What’s Next


Reward hacking isn’t a reason to give up on LLM-generated kernels. Every eval or RL trajectory we generate is a red-teaming opportunity against our defenses. Each hack we catch makes the next one harder to slip through.


We’ll be publishing select traces and kernels publicly at ‘[https://app.wafer.ai/traces](https://app.wafer.ai/traces) ’, and commit to continuing to release reports on reward hacks like the above.


If you find something we’ve missed, we want to hear about it. Every gap we find as a community is one we can close together.
