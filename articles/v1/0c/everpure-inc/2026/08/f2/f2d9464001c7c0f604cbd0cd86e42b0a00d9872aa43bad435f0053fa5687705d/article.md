---
schema_version: "1.0.0"
document_id: "f2d9464001c7c0f604cbd0cd86e42b0a00d9872aa43bad435f0053fa5687705d"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/purely-technical/building-a-gpu-native-data-path-for-pure-kva/"
published_at: "2026-08-18T13:00:00+00:00"
first_seen_at: "2026-08-18T14:56:55.255720+00:00"
fetched_at: "2026-08-18T14:56:57.826681+00:00"
content_hash: "sha256:fe2b2328119e7c1f83bd1bce85494a56b955bf6cd4233dc32850de7d6313bbc5"
---

# Building a GPU-Native Data Path for Pure KVA

### Summary


Pure KVA accelerates scalable LLM inference by efficiently reloading KV caches with NVIDIA GPUDirect Storage, CUDA scatter kernels, and lease-managed GPU staging buffers.


LLM inference is increasingly a data movement problem.


As context windows grow, the key-value (KV) cache becomes one of the largest and most valuable memory objects in the inference stack. KV cache is expensive to compute and store in GPU HBM, but it’s valuable to reuse when prompts overlap across requests, users, agents, or workflows.


Our previous blogs on Pure Key Value Accelerator (KVA) covered[KV caching in detail](https://blog.everpuredata.com/purely-technical/cut-llm-inference-costs-with-kv-caching/) , how Everpure provides[scalable external storage for KV tensors](https://blog.everpuredata.com/perspectives/pure-kva-1-solution/) , and how[Pure KVA accelerates inference](https://blog.everpuredata.com/purely-technical/20x-faster-inference-first-kv-cache-for-s3-and-nfs/) .


In this blog, we’ll go one level deeper to focus on the GPU data path: how Pure KVA uses NVIDIA GPUDirect Storage (GDS) to move cached KV tensors from storage into GPU memory, and what else is needed to make that data usable by an inference runtime.


The main idea is simple: GDS can bring KV tensor bytes into GPU memory, but Pure KVA still has to turn those bytes into active KV cache rows in the layout expected by the inference runtime. That requires careful handling of memory ownership, data placement, and runtime-specific layouts.


## **GDS solves the first hop, not the whole problem**


NVIDIA GPUDirect Storage enables data to move directly between storage and GPU memory, bypassing traditional CPU staging buffers. For data-intensive workloads, this removes unnecessary data movement and lowers CPU involvement in the I/O path.


At a high level, GDS changes the data path from:


1


2


3


4


5


Storage


→


CPU


memory


→


GPU


memory


to


:


Storage


→


GPU


memory


*Figure 1: Comparison of traditional I/O path vs. GPUDirect Storage data path.*[Source](https://developer.nvidia.com/blog/gpudirect-storage/) *.*


For KV cache reloads, this is a significant improvement. Cached KV tensors can be read directly from Everpure™[FlashBlade](https://www.everpuredata.com/products/unstructured-data-storage.html) ® into GPU memory without first passing through host memory.


However, getting data into GPU memory is only the first step.


A KV tensor stored on disk is typically organized in compact token order. The inference runtime, on the other hand, manages its KV cache using its own internal layout, often based on pages, blocks, or slots.


As a result, a successful GDS read does not automatically make the KV cache usable by the runtime. The reload problem for the runtime becomes:


1


2


3


4


5


6


7


8


9


Read


KV


tensor


into


GPU


memory


↓


Place


KV


rows


into


the


runtime


’ s


KV


cache


layout


↓


Make


them


available


for


attention


This is why the Pure KVA reload path has two stages:


1. Storage-to-GPU transfer using GDS
2. GPU-side placement into the runtime-managed KV cache


GDS solves the first stage. The rest of this post focuses on the second stage: how Pure KVA makes GPU-resident KV data safe, correctly placed, and ready for the inference runtime.


## **The GDS staging region: Necessary, but not the final destination**


GDS gives Pure KVA a way to move KV tensor bytes directly from FlashBlade into GPU memory. But GDS still needs a GPU memory region that is suitable for direct storage I/O.


In Pure KVA, this region acts as a GPU staging buffer for cuFile transfers. It’s allocated by Pure KVA, aligned for direct I/O, and registered with cuFile using *cuFileBufRegister* .


The staging buffer is optimized for moving bytes between storage and GPU memory. The inference runtime’s KV cache is optimized for attention execution. These are not the same thing.
At a high level, the data path becomes:


*Figure 2: Data path from FlashBlade KV-cache storage to attention inference.*


The staging buffer solves the storage I/O problem, but it introduces a new system problem: Once the tensor lands there, Pure KVA still has to make it usable by the inference runtime.


### **Why the staging buffer cannot simply be the KV cache**


The first idea was the most direct one: Once GDS has read the KV tensor into GPU memory, let the inference runtime use that tensor directly.


The tensor is already on the GPU, so why move it again?


The problem is that the GDS staging buffer is transfer memory, not long-lived runtime memory. Pure KVA reuses this registered GPU region across requests, chunks, and layers. A slice of the staging buffer that contains KV data for one request may later be reused for another GDS transfer.


If the inference runtime were holding pointers into that staging region, those pointers could later refer to memory that has been overwritten by a different request.


So the issue is not whether the tensor is in GPU memory. It’s whether the memory remains stable for the lifetime expected by the runtime.


Another subtlety is that framework-managed GPU tensors are not a good lifecycle boundary. PyTorch, for example, aggressively recycles GPU allocations. The same address may be reused for another tensor after the original object is released, and Python object lifetime does not necessarily line up with CUDA execution.


All of the above makes direct pointer-based handoff fragile. The staging buffer is a good place to land data from storage, but it cannot safely become the runtime’s KV cache.


### **Alternative: Clone the staged tensor**


The next approach was to clone the tensor after it landed in the GDS staging buffer.


This made the handoff safer. A cloned tensor has its own allocation and is no longer tied to the reusable staging region. That means the runtime can hold the cloned tensor without worrying that a later GDS read will overwrite the same staging slice.


But cloning works against the performance goal.


The GDS path is intended to reduce unnecessary data movement. If every reload does the following:


1


2


3


4


5


6


7


8


9


GDS


read


into


GPU


staging


buffer


↓


clone


into


another


GPU


allocation


↓


runtime


consumes


cloned


tensor


Then the system has added another GPU-to-GPU copy on the critical path. That increases HBM pressure, adds latency, and reduces the benefit of avoiding the CPU staging path in the first place.


## **Epiphany: Make the handoff a CUDA operation**


The key realization was that the handoff from the GDS staging buffer to the runtime KV cache is itself a GPU data movement problem.


The data is already in GPU memory. The destination is also GPU memory. The transformation required is well-defined: move KV rows from the compact tensor read by GDS into the physical locations expected by the inference runtime.


Instead of relying on framework-level tensor operations or cloning the whole tensor, Pure KVA could use CUDA directly for this handoff.


That gives us a cleaner path:


1


2


3


4


5


6


7


8


9


GDS


read


into


registered


GPU


staging


buffer


↓


CUDA


moves


data


into


runtime


KV


cache


↓


runtime


attends


over


its


own


KV


memory


This keeps the staging buffer as reusable transfer memory while leaving the runtime KV cache as runtime-owned memory.


This introduced one more challenge: While the CUDA transfer is happening, the staging slice must not be reused by another request.


## **Leases: Tracking staging buffer lifetime**


The staging buffer is a shared pool. Multiple requests may be reading or writing KV tensors. Pure KVA needs to know when a slice of that pool is safe to reuse.


A Python object going out of scope is not enough. Tensors and views are passed by reference, and CUDA work is asynchronous. The host program may move on while the GPU is still reading from a memory region.


If Pure KVA returned a staging slice to the free pool too early, a later GDS read could overwrite data that a CUDA kernel is still consuming.


So the lifecycle cannot be tied only to Python object lifetime. It needs to be tied to GPU execution.


Pure KVA treats the registered staging region as a reusable pool. Each GDS operation receives a lease over a slice of that pool.


A lease means:


*This staging slice is owned by this transfer until the consuming GPU work is complete.*


The lifecycle becomes:


*Figure 3: Data flow for leasing a staging slice, loading a KV tensor via GDS, copying it to the runtime KV cache, and releasing the slice.*


The staging buffer is not released because Python dropped a reference. It’s released because the CUDA stream that consumes the staged tensor has completed the transfer into runtime-owned memory.


Once released, the slice can be coalesced back into the free space of the staging pool and reused by later requests.


## **The placement problem**


The lease model solves lifetime. It does not solve the layout.


The tensor read from storage is compact. It’s usually ordered by token:


1


2


3


4


5


src


\[


0


\]


=


KV


row


for


token


0


src


\[


1


\]


=


KV


row


for


token


1


src


\[


2


\]


=


KV


row


for


token


2


The runtime KV cache is different. It may be paged or block-based. The runtime decides where each token’s KV row should live.


So the destination looks more like:


1


2


3


4


5


dst


\[


slot_mapping


\[


0


\]


\]


=


src


\[


0


\]


dst


\[


slot_mapping


\[


1


\]


\]


=


src


\[


1


\]


dst


\[


slot_mapping


\[


2


\]


\]


=


src


\[


2


\]


The *slot_mapping* tells Pure KVA where each logical token’s KV row belongs in the runtime-owned KV cache.


## **CUDA scatter kernel: Copy and transform in one step**


Pure KVA uses a CUDA scatter kernel to move KV rows from the leased GDS staging slice into the runtime KV cache.


At a high level, the kernel does this:


1


2


3


4


5


for


each


token


t


in


parallel


:


destination


=


slot_mapping


\[


t


\]


copy


KV


row


from


staging


memory


into


destination


slot


For standard key-value cache layouts, the same mapping is applied to both K and V rows.


This is why we call it a scatter operation. The source tensor is compact and token-ordered. The destination is runtime-owned and slot-ordered. The kernel scatters each row into the correct destination location.


The optimized path becomes:


1


2


3


4


5


6


7


8


9


GDS


read


into


leased


staging


memory


↓


CUDA


scatter


into


runtime


KV


cache


↓


release


staging


lease


after


CUDA


stream


completion


This avoids the clone-based handoff and CPU staging. It also avoids splitting the operation into separate copy and layout-conversion steps.


The kernel expresses the exact handoff Pure KVA needs:


*Figure 4: Scatter-based placement of compact tensor rows into selected runtime KV-cache slots using* *slot_mapping* *, while leaving other slots unchanged.*


**
Together, the registered staging pool, lease lifecycle, and CUDA scatter kernel form the optimized GDS reload path.


## **The save path follows the same boundary**


The reload path is the more interesting direction, but the save path follows the same separation in reverse.


When Pure KVA saves KV cache, the runtime-owned KV data has to be written out to storage:


1


2


3


4


5


6


7


8


9


10


11


12


13


runtime


KV


cache


↓


GPU


staging


memory


↓


GDS


write


↓


FlashBlade


The key correctness rule is that metadata should become visible only after tensor data is durable. A future request should not discover a cached KV entry whose tensor bytes are still incomplete.


So Pure KVA treats data durability and metadata visibility as separate steps:


1


2


3


4


5


6


7


8


9


write


tensor


data


↓


confirm


data


is


durable


↓


publish


metadata


This keeps the GDS fast path compatible with the broader cache correctness model.


## **GDS fast path and mmap fallback**


GDS is an acceleration path, not a separate cache format.


When the cuFile fast path is available, Pure KVA reads tensor data directly from storage into registered GPU staging memory:


1


Storage


→


GDS


/


cuFile


→


registered


GPU


staging


memory


If that path is unavailable, Pure KVA falls back to the standard filesystem path. This fallback uses host memory through mmap and then copies data between host memory and GPU memory:


1


Storage


→


host


page


cache


/


mmap


→


cudaMemcpy


→


GPU


tensor


The fallback bypasses the GDS staging pool and lease model. It’s useful for compatibility and correctness, but it reintroduces the CPU/host-memory bounce that GDS is designed to avoid.


The key point is that the cache object model stays the same. Only the data movement path changes.


## **Conclusion**


KV cache reload sits at the boundary of storage, GPU memory management, and inference runtime layout.


GDS removes the CPU staging hop, but it does not make GPU-resident data automatically usable by the runtime. Pure KVA still has to control where the data lands, how long that memory remains valid, and how compact tensor rows become runtime-owned KV cache rows.


The design that emerged is simple:


- **GDS:** Moves KV bytes from storage into registered staging memory
- **Leases:** Protect staging memory until CUDA has consumed it
- **CUDA scatter:** Places KV rows into the runtime-owned cache layout


This is the core engineering lesson: Efficient KV reuse needs both a fast storage path and a runtime-aware GPU placement path.


Together, the registered staging buffer, lease lifecycle, and CUDA scatter kernel turn GDS from a fast transfer mechanism into a practical KV cache reload path for inference.


This deep dive shows that GDS-ready requires more than a certification badge.


## **Accelerate Your LLM Inference**


See how Pure KVA uses storage-backed KV caching to reduce recomputation, improve GPU efficiency, and speed inference reloads.


[Learn More](https://www.everpuredata.com/events/vmware-explore-las-vegas-2026.html)
