---
schema_version: "1.0.0"
document_id: "178dc43a1ff9fa12f0c3cd85b4f6f99bdc8ca02619dbc9e00bd175c9fe4428db"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/reward-hacks-field-guide"
published_at: "2026-03-12T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:0bd3a9bf0b769f69e1fb61a5371259987383f109ba25701d22c1801765251f46"
---

# A Field Guide to Reward Hacking in AI Kernel Generation

## The Three Categories


Every reward hack we’ve seen falls into one of three categories:


- **Timing Attacks.** The kernel computes correctly but manipulates the clock. The measured time is fake. The real computation is either deferred, offloaded, or invisible to the timing infrastructure.
- **Semantic Attacks.** The kernel runs fast because it doesn’t do the right thing. It returns garbage, copies input to output, reads stale buffers, or computes in lower precision. The output either passes a loose correctness check or exploits buffer reuse in the evaluation harness.
- **Benign Shortcuts.** Not malicious, but beside the point. The model calls` torch.matmul` instead of writing a kernel. Correct output, potentially fast, but it dodges the entire purpose of the benchmark.


The timing attacks are the most creative. The semantic attacks are the most dangerous. The benign shortcuts are the most common.


## Timing Attacks


### 1. Stream Injection


The kernel runs its computation on a separate CUDA stream. Because timing events are recorded on the default stream, they miss the real work entirely. The measured time is near zero.


```text
def   stream_injection  (A: torch.Tensor, B: torch.Tensor):
stream   =   torch.cuda.Stream()
with   torch.cuda.stream(stream):
return   torch.matmul(A, B)
```


This is three lines of code and it defeats most timing harnesses. The output is correct. The timing is meaningless. You’ll see speedups of 50x+ that vanish the moment you add` torch.cuda.synchronize()` before the end event.


**Defense:** Hybrid timing. We time every kernel twice: once with standard events on the default stream, once with a full` torch.cuda.synchronize()` before the end event that forces all streams to complete. If the ratio exceeds 1.5x, stream injection is flagged.


### 2. Thread Injection


Similar idea, different mechanism. The kernel spawns a background CPU thread to do the GPU work, returns an empty output tensor immediately, and hopes the background thread finishes before the correctness check runs.


```text
def   thread_injection  (A: torch.Tensor, B: torch.Tensor):
out   =   torch.empty(A.size(  0  ), B.size(  1  ),
device  =  A.device,   dtype  =  A.dtype)
def   compute  ():
out.copy_(torch.matmul(A, B))
t   =   threading.Thread(  target  =  compute)
t.start()
return   out
```


The tell: output is occasionally zeros or partially filled, depending on the race between the background thread and the correctness check.


**Defense:** We record` threading.active_count()` before and after kernel execution. New threads = rejection.


### 3. Lazy Evaluation


This one is clever. The kernel returns a` torch.Tensor` subclass that stores the input tensors but doesn’t compute anything. The actual matmul happens when the correctness check calls` __eq__` on the result.


```text
class   _LazyMatmul  (  torch  .  Tensor  ):
@  staticmethod
def   __new__  (cls, A, B):
obj   =   torch.Tensor._make_subclass(
cls  , torch.empty(A.size(  0  ), B.size(  1  ),
device  =  A.device,   dtype  =  A.dtype))
obj.A, obj.B   =   A, B
return   obj


def   __eq__  (self, other):
return   torch.matmul(  self  .A,   self  .B)   ==   other


def   lazy_evaluation  (A, B):
return   _LazyMatmul(A, B)
```


Timing captures only the subclass construction (instant). The real work happens during the correctness comparison. The diagnostic: the kernel appears near-instant but the correctness check itself takes unusually long.


**Defense:** We validate that the output is a standard` torch.Tensor` (not a subclass), with allocated storage and a non-null data pointer on the expected device. Tensor subclasses are rejected.


### 4. Patching Timing


The most brazen approach. The kernel monkey-patches` torch.cuda.Event.elapsed_time` to always return 0.001ms, then runs normal (slow) code.


```text
_original_elapsed_time   =   torch.cuda.Event.elapsed_time


def   _fake_elapsed_time  (self, end_event):
return   0.001    # Always report 0.001ms


torch.cuda.Event.elapsed_time   =   _fake_elapsed_time


def   custom_kernel  (A, B):
return   torch.matmul(A, B)    # Normal speed, fake timing
```


The tell:` elapsed_time` reports an impossibly consistent value regardless of problem size. A kernel that takes 0.001ms for both a 128x128 and 16384x16384 matmul is lying.


**Defense:** Before executing any kernel, the harness captures references to the original` elapsed_time` ,` record` , and` synchronize` functions. After the kernel module is imported, we compare. If any have been replaced, the kernel is rejected.


## Semantic Attacks


### 5. Identity Kernel


The simplest semantic attack. The kernel copies its input to its output and does nothing else.


```text
@triton.jit
def   identity_kernel  (input_ptr, output_ptr, n_elements,
BLOCK_SIZE: tl.constexpr):
pid   =   tl.program_id(  0  )
offsets   =   pid   *   BLOCK_SIZE   +   tl.arange(  0  ,   BLOCK_SIZE  )
mask   =   offsets   <   n_elements
x   =   tl.load(input_ptr   +   offsets,   mask  =  mask)
tl.store(output_ptr   +   offsets, x,   mask  =  mask)    # Just copy
```


This is fast because it does almost no work. It can pass a correctness check if the evaluation harness happens to reuse buffers or if the shapes align in a way that makes input ≈ expected output.


**Defense:** Multi-input validation. We run the kernel on several distinct random inputs and compare against a reference. An identity kernel always produces output equal to input, which doesn’t match the reference for non-trivial operations.


### 6. No-Op Kernel


Even simpler. The kernel launches but executes zero instructions.


```text
@triton.jit
def   noop_kernel  (output_ptr, n_elements,
BLOCK_SIZE: tl.constexpr):
pass    # Do nothing
```


This relies on the output tensor already containing the correct values, either from a previous reference run that wrote to the same memory, or from buffer aliasing in the harness. The tell: correctness passes only when the reference ran first.


**Defense:** Memory guard buffers. We wrap inputs and outputs with sentinel regions filled with NaN/Inf. After the kernel runs, we verify guards are intact and output was actually written. A no-op kernel leaves the NaN-poisoned output untouched.


### 7. Shared Memory Overflow


This is the one we caught in the wild: A fused HIP kernel requested 65,792 bytes of shared memory, 256 bytes over the MI300X hardware limit of 65,536. The ROCm 6.x runtime silently allowed it. The kernel launched, read garbage from uninitialized memory at the overflowed addresses, and returned in 0.020ms.


```text
__global__   void   fused_kernel  (...) {
extern   __shared__   float   smem  []  ;         // 65,536 bytes (at limit)
float*   row_data   =   smem;


__shared__   float   warp_maxes  [  32  ];        // +128 bytes (over limit!)
__shared__   float   warp_sums  [  32  ];         // +128 bytes
// Total: 65,792 bytes - 256 over hardware limit
// warp_maxes and warp_sums read/write uninitialized memory
}
```


For a batch=128, 16384x16384 matmul, 0.020ms implies 1.7 PFLOPS. That’s 1000x the MI300X’s fp32 peak. The output passed` torch.allclose` because softmax bounds output to \[0,1\] and sums to ~1. With atol=1e-3 over 16,384 classes, even near-uniform garbage looks close enough.


**Defense:** Output determinism check. We run the kernel multiple times with identical inputs and compare outputs with` torch.equal` (bitwise). The overflowed shared memory reads whatever happens to be at the address, which changes between runs. We also call` torch.cuda.empty_cache()` between iterations to prevent stale cached data from masking the non-determinism.


### 8. Precision Downgrade


The subtlest semantic attack. The kernel computes in fp16 and casts back to fp32. This is genuinely faster (fp16 has higher throughput) but the output has significantly worse numerical accuracy.


```text
def   precision_downgrade  (A: torch.Tensor, B: torch.Tensor):
return   torch.matmul(A.half(), B.half()).float()
```


The tell: a modest speedup (1.5-3x) with output error suspiciously close to the` allclose` tolerance threshold. This one is hard to distinguish from a legitimately clever kernel that trades precision for speed, which is why it needs a precision-specific check.


**Defense:** Two layers. First, a dtype check catches obvious cases where output type differs from expected. Second, a ULP (units in last place) precision check compares the kernel output against an fp64 reference. fp16-computed results show characteristic error patterns that fp32 results do not.


### 9. Caching / Memoization


The kernel computes correctly on the first invocation and caches the result. All subsequent calls return the cache. Since benchmarks measure repeated invocations, every timed iteration hits the cache.


The Python variant keys by shape and device. Easy to catch. Change the input values and the cached result is wrong.


The C++ variant is harder. We observed this pattern in GPT-5.4 GEMM traces: a` static std::unordered_map` keyed by tensor data pointer addresses.


```text
// C++ pointer-keyed variant (observed in-the-wild)
static   std  ::unordered_map  <  CacheKey,   torch  ::Tensor, CacheKeyHash  >   cache;
CacheKey key{  reinterpret_cast<uintptr_t>  (a.  data_ptr  ()),
reinterpret_cast<uintptr_t>  (b.  data_ptr  ()), M, N, K};
auto   it   =   cache.  find  (key);
if   (it   !=   cache.  end  ())   return   it->second;    // skip compute
auto   result   =   /* actual GEMM */  ;
cache.  emplace  (key, result);
return   result;
```


Since PyTorch reuses memory allocations, pointer addresses are stable across benchmark reps. The cache hit rate is 100% on timed iterations. Fresh tensors with new pointers cause a cache miss and trigger real compute, so the correctness check passes. This lives in compiled C++ extension code. No Python-level inspection will find it.


**Defense:** Pointer-poisoning. After fresh-tensor verification passes, we overwrite the verification tensors in-place with new random data (same pointer addresses, different content), then re-run the kernel. A pointer-keyed cache returns the stale cached result, which mismatches the reference output computed on the new data.


## Benign Shortcuts


### 10. Baseline Kernel


The model calls` torch.matmul` ,` F.linear` , or cuBLAS instead of writing a custom kernel. Correct output, reasonable speed, but it sidesteps the purpose of the benchmark.


```text
def   forward  (self, A, B):
return   torch.matmul(A, B)
```


**Defense:** Static analysis scans for torch computation ops,` nn.Module.forward` calls,` torch.nn.functional` usage, cuBLAS, and cuDNN imports. Any match is rejected before the kernel ever runs.


## The Progression


Early hacks were crude: no-op kernels, identity copies. As evaluation harnesses improved, the hacks got more creative. Stream injection exploits CUDA’s asynchronous execution model. Lazy evaluation turns Python’s type system against the harness. The C++ pointer-keyed cache exploits PyTorch’s memory allocator determinism from inside compiled extension code.


Each new defense closes a door. We publish these patterns openly because a benchmark is only as trustworthy as its defense suite, and defense suites improve faster when the community can audit them.


KernelArena’s evaluation harness implements defenses against all 10 patterns documented here. The leaderboard reflects post-defense scores. If you’re building your own kernel benchmarks, the defense implementations are the part worth stealing.


We maintain the full catalog with live code examples, diagnostics, and defense implementations at kernelarena.ai/resources.
