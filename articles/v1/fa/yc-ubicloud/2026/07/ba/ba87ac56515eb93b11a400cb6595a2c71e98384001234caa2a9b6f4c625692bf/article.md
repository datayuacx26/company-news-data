---
schema_version: "1.0.0"
document_id: "ba87ac56515eb93b11a400cb6595a2c71e98384001234caa2a9b6f4c625692bf"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/life-of-an-inference-request-vllm-v1"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:108124a8a8a2f7411e7139e287eebab601dac39ee0c6e28237ed4910102fadd0"
---

# Life of an inference request (vLLM V1): How LLMs are served efficiently at scale

## **Life of an inference request (vLLM V1): How LLMs are served efficiently at scale**


June 27, 2025 · 10 min read


Junhao Li


Senior Software Engineer


*Ubicloud is an open source alternative to AWS. We offer managed cloud services that build on top of PostgreSQL, Kubernetes, vLLM, and others.* ‍


‍[vLLM](https://github.com/vllm-project/vllm) is an open-source inference engine that serves large language models. We deploy multiple vLLM instances across GPUs and load open weight models like Llama 4 into them. We then load balance traffic across vLLM instances, run health checks, and do upgrades. Our customers consume our managed service by sending their prompts to our API endpoints. This endpoint also determines the vLLM instance that serves their prompt.


vLLM sits at the intersection of AI and systems programming, so we thought that diving into its details might interest some of our readers. In this blog post, we describe how an inference request travels through vLLM’s OpenAI-compatible API server and core engine. We also provide key code pointers.


We assume readers are already familiar with the[transformer architecture](https://arxiv.org/pdf/1706.03762) and large language models. If you're not, we highly recommend this[video](https://www.youtube.com/watch?v=kCc8FmEb1nY) by OpenAI co-founder Andrej Karpathy. We will focus on the new V1 architecture of vLLM and how it achieves state-of-the-art text generation performance. If you're looking for the V0 behavior or multi-modal inference, please refer to other vLLM documentation.


### Terminology


We use the following terms throughout this blog. These terms also align with what is used in vLLM’s codebase and documentation:


- Request: An incoming chat completion message from the client, formatted in the[OpenAI-compatible format](https://platform.openai.com/docs/api-reference/completions/create) .
- Sequence: The combined stream of prompt and response tokens associated with a request. Except in special cases, a single request typically corresponds to one sequence. Sequence is sometimes used interchangeably with Request in both the codebase and this blog post.
- Batching: The process of grouping multiple requests together into a single forward pass through the model to maximize GPU utilization.
- Prefill: The stage in which the model processes the prompt tokens of a new request. It computes the transformer attention and stores the Key (K) Value (V) tensors of the attention for each token and each layer into the KV cache blocks.
- Decode: The stage in which the model generates the next output token or the output tensor for intermediate layers repeatedly for ongoing requests. It reuses the stored KV cache of all preceding tokens and computes the Query (Q) tensors based on the most recent token.
- KV Cache: The GPU memory region used to store the transformer attention keys and values for each token in a request. It is managed globally across all requests and devices in vLLM.
- KV Block: A fixed-size memory unit in the KV cache that holds keys and values for a fixed number of tokens. KV cache is segmented into blocks to minimize memory reads/writes.


### High Level Architecture


Before diving into the request flow, let’s first take a look at the main components of vLLM V1’s serving architecture:


- [AsyncLLM](https://github.com/vllm-project/vllm/blob/94d8ec8d2bcb4ec55e33022b313c7e978edf05e1/vllm/v1/engine/async_llm.py) : An asynchronous wrapper around the core engine. The OpenAI-compatible server uses AsyncLLM to submit requests to and retrieve outputs from the EngineCore asynchronously. It communicates with the EngineCore via[AsyncMPClient](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core_client.py#L729) using asynchronous inter-process communication (IPC). It also handles tokenization and detokenization.
- [EngineCore](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py) : The core engine that carries out the inference. It is responsible for scheduling, which means batching and allocating tokens for requests. It also executes the model, processing prompt tokens or generating new ones. It’s the heart of vLLM where most of the logic resides. At the core, there is a[busy loop](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L501-L509) that pulls data from an internal input queue and runs an engine[step](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L211-L226) . An engine step includes the scheduling and execution of one forward pass of the model. The results are first pushed to an internal output queue, and then pushed back to the AsyncLLM and the API server.[Two additional background threads](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L592-L650) manage the asynchronous data transfer between the two internal input/output queues and the AsyncLLM over IPC.
- [Scheduler](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/core/sched/scheduler.py) : A component within the engine that handles how multiple requests are batched. The scheduler maintains queues of requests and decides at each step how many tokens to process for each request.
- [ModelExecutor](https://github.com/vllm-project/vllm/blob/980a172474fa0f32433dda87ae1fa4aadba24c51/vllm/executor/ray_distributed_executor.py) : A component that coordinates the loading and execution of the model across one or more GPU worker processes. Internally, it uses the[ray](https://docs.ray.io/) distributed computing library and launches one worker process per GPU device.
- [ModelRunner](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/worker/gpu_model_runner.py) : Each GPU worker has a ModelRunner that[loads the model](https://github.com/vllm-project/vllm/blob/980a172474fa0f32433dda87ae1fa4aadba24c51/vllm/v1/worker/gpu_model_runner.py#L1502-L1524) and[executes the forward passes](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/worker/gpu_model_runner.py#L1094-L1416) . The ModelRunner takes requests in batches from the scheduler. It processes these requests through the model on all CUDA cores at the same time. It takes care of assembling input tensors and can use optimizations like CUDA graph capture for repeated executions.
- [KVCacheManager](https://github.com/vllm-project/vllm/blob/980a172474fa0f32433dda87ae1fa4aadba24c51/vllm/v1/core/kv_cache_manager.py) : vLLM’s GPU memory management modules, responsible for managing space for the transformer KV tensors on GPU for all requests. It treats GPU memory as a paging system. KV cache is stored in fixed-size blocks, or pages. This allows for efficient dynamic allocation and avoids needing large contiguous memory reservations. The Scheduler uses the KVCacheManager to[allocate these blocks](https://github.com/vllm-project/vllm/blob/980a172474fa0f32433dda87ae1fa4aadba24c51/vllm/v1/core/sched/scheduler.py#L231-L234) and[attaches the block IDs](https://github.com/vllm-project/vllm/blob/980a172474fa0f32433dda87ae1fa4aadba24c51/vllm/v1/core/sched/scheduler.py#L501-L523) to each request. The ModelRunner then[uses](https://github.com/vllm-project/vllm/blob/980a172474fa0f32433dda87ae1fa4aadba24c51/vllm/v1/worker/gpu_model_runner.py#L353-L364) these IDs when preparing the execution context.


This idea of a paged KV cache was proposed by the vLLM authors in the[PagedAttention paper](https://arxiv.org/pdf/2309.06180) and was the genesis of the vLLM project.


### Receiving the Request – API Server and Async IPC


The journey begins when an HTTP request arrives at the vLLM server (e.g. a POST to /v1/chat/completions). The[OpenAI-compatible API server](https://github.com/vllm-project/vllm/blob/94d8ec8d2bcb4ec55e33022b313c7e978edf05e1/vllm/entrypoints/openai/api_server.py) handles the HTTP communication and authentication (via the API key, if configured). This server is often launched by running the vllm serve


command defined by[vllm/entrypoints/cli/serve.py](https://github.com/vllm-project/vllm/blob/94d8ec8d2bcb4ec55e33022b313c7e978edf05e1/vllm/entrypoints/cli/serve.py) .


After validation, the server invokes the AsyncLLM engine to handle the request. In code, this is done by calling the AsyncLLM engine method[generate()](https://github.com/vllm-project/vllm/blob/94d8ec8d2bcb4ec55e33022b313c7e978edf05e1/vllm/v1/engine/async_llm.py#L263-L272) . The method is given the prompt and a freshly generated request_id to track this request. When the generate() is called, the AsyncLLM performs[tokenization](https://github.com/vllm-project/vllm/blob/94d8ec8d2bcb4ec55e33022b313c7e978edf05e1/vllm/inputs/preprocess.py#L199-L219) with the model’s tokenizer (often from Hugging Face) to convert text to token IDs. Some multimodal input processing also happens at this stage, but for simplicity, we’ll focus on text prompts here. It then[sends the request](https://github.com/vllm-project/vllm/blob/94d8ec8d2bcb4ec55e33022b313c7e978edf05e1/vllm/v1/engine/async_llm.py#L253) to the EngineCore via[AsyncMPClient](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core_client.py#L729) using asynchronous IPC calls.


Then, a[background loop](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L601-L610) at the EngineCore picks up the request from the IPC channel and places it into an internal input queue for scheduling.


Note that the AsyncLLM and EngineCore run in different processes. This bypasses Python’s Global Interpreter Lock (GIL). It allows CPU-intensive tasks, like tokenization and HTTP communication, to run alongside the GPU-intensive task, the model execution. This maximizes overall throughput.


### Scheduling and Continuous Batching


The[Scheduler](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/core/sched/scheduler.py) module is central to vLLM’s ability to achieve high throughput. It keeps track of all requests and orchestrates their progress. The scheduler maintains a[waiting deque](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/core/sched/scheduler.py#L91) for requests that are ready to be processed (new or resumed ones) and a[running list](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/core/sched/scheduler.py#L92) for requests currently in active generation. In each iteration (cycle) of the engine, it first[examines the internal input queue](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L526-L528) and[adds new requests](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L190) into the scheduler’s waiting deque. It then[picks](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/core/sched/scheduler.py#L151) a set of requests to advance, looking at the requests from the running list first, and then the requests from the waiting deque.


vLLM uses a continuous batching algorithm. This helps maximize GPU utilization by keeping a full workload within a fixed token budget, a.k.a[max_num_batched_tokens](https://github.com/vllm-project/vllm/blob/6b6d4961147220fb80f9cc7dcb74db478f9c9a23/vllm/v1/core/sched/scheduler.py#L65-L66) . It also ensures fairness in request order and limits the batch to just one forward pass of the model.


Here’s a continuous batching example:


Suppose we have three requests with 3, 5, and 12 prompt tokens, respectively, and a token budget of 10. In a traditional server, these might be handled sequentially or in fixed-size batches. In vLLM, however, the scheduler could decide in one iteration (Step 0) to feed, for example, 3 prompt tokens from R1, 5 from R2, and 2 from R3 in a single forward pass. Note that it only picks two prompt tokens from R3 due to the token budget of 10. In the next iteration (Step 1), it can continue with the remaining 8 prompt tokens from R3. At the same time, it will start generating 1 token each for R1 and R2. They have finished filling their prompt tokens and are now in the decoding phase.


Note that during the prefill phase, all prompt tokens from a request can be processed in one batch. This is possible because the query (Q) tensors, calculated from the tokens immediately before them, are available for each prompt token position. In the decoding phase, however, the immediately preceding token must be decoded iteratively, so we can only process one token at a time per request. The scheduler ensures that all requests complete the prefill phase before entering the decoding phase.


During the scheduling phase, the KV cache blocks are also determined. The KV Cache Manager groups tokens into fixed-size chunks and either[allocates a new KV block](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/core/sched/scheduler.py#L231-L234) or[retrieves an existing KV block](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/core/sched/scheduler.py#L353-L355) for each chunk of the tokens. Note that the actual GPU memory allocation occurs during vLLM initialization. The KV Cache Manager determines the block IDs for the KV caches of each request and attaches these IDs to the request, so they can later be used by the ModelRunners when executing the model.


### Model Execution – Forward Pass on the GPU


Once the scheduler has determined which requests to advance, it[invokes](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L203) the ModelRunners via the ray-based[ModelExecutor](https://github.com/vllm-project/vllm/blob/980a172474fa0f32433dda87ae1fa4aadba24c51/vllm/executor/ray_distributed_executor.py) . This[executes](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/worker/gpu_model_runner.py#L1094-L1098) the model’s forward pass, either processing the prompt tokens or generating new tokens.


All tokens from the selected requests are combined into a single large tensor, a.k.a a batch, and are processed layer by layer through the transformer's weight matrices. At each layer, the three attention tensors, Key (K), Value (V), and Query (Q), of each attention head are computed. Then a final attention output tensor for the layer is computed from these tensors. The K and V tensors are stored for future use, and the output tensor becomes the input for the next layer. This is where GPUs come into play, as they are great at large matrix computations. Each tensor includes data from all batched requests. This allows each matrix operation of these requests to be processed on the GPU in parallel using SIMD(T) (single instruction, multiple data, and multiple threads) across all CUDA cores.


For requests in the decoding phase, the output tensor from the final transformer layer produces the logits, which represent the predicted probabilities for the next token. vLLM then[applies the sampling or decoding strategy](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/worker/gpu_model_runner.py#L1224-L1255) . For each sequence, it looks at the logits and either selects the top token (greedy or deterministic) or samples according to the provided parameters, e.g. temperature. This process produces the next token for each batched request. These tokens are[placed](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L537) into an internal output queue, where a[background loop](https://github.com/vllm-project/vllm/blob/dc1440cf9f8f6233a3c464e1a01daa12207f8680/vllm/v1/engine/core.py#L629-L650) picks them up and sends them back to the AsyncLLM via IPC for output processing.


This timeline diagram shows a portion of the forward pass for the[DeepSeek R1 Distilled Qwen 32B model](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B) that we serve.


Within the busy loop of the EngineCore, the model's forward method runs. This method calls the forward methods of each transformer layer. Then, the forward method of each layer calls the FlashAttention module. FlashAttention is a highly optimized function for computing transformer attention. It uses the[FlashAttention-3](https://arxiv.org/abs/2407.08608) algorithm. This model contains a total of[64 transformer layers](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B/blob/main/config.json#L13) and this timeline diagram shows 3 of them at the bottom. The model runner repeats the forward pass through all 64 transformer layers at each engine step.


### Output Processing – Streaming Tokens Back


The AsyncLLM gets the new tokens from the IPC channel. Then, it[processes](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/engine/output_processor.py#L297) these tokens by detokenizing them and[placing](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/engine/output_processor.py#L362) them into an internal output queue. The original caller, the AsyncLLM’s generate() function, will then[pick them up](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/engine/async_llm.py#L310) and[pass them back](https://github.com/vllm-project/vllm/blob/1f079540db5f1080a2f61a730da50d3009934c5a/vllm/v1/engine/async_llm.py#L315) to the API server.


In non-streaming mode, the[API handler](https://github.com/vllm-project/vllm/blob/20bd6f4d2ee10d6187d93e4bc6a64f78195243a2/vllm/entrypoints/openai/serving_chat.py#L890-L899) would accumulate tokens internally until the request is finished and then return the final output. In streaming mode, the[handler](https://github.com/vllm-project/vllm/blob/20bd6f4d2ee10d6187d93e4bc6a64f78195243a2/vllm/entrypoints/openai/serving_chat.py#L394-L403) will send each partial output immediately to the client in[chunks](https://github.com/vllm-project/vllm/blob/20bd6f4d2ee10d6187d93e4bc6a64f78195243a2/vllm/entrypoints/openai/serving_chat.py#L850) like data: {...}


### Conclusion


In this blog post, we’ve gone through the entire life of a vLLM inference request. The journey starts with the API server handing the request to the AsyncLLM, which tokenizes and sends requests to the EngineCore. At EngineCore, the scheduler batches requests. The executor works with the ModelRunner to run forward passes through the attention layers on GPUs. Then, tokens are streamed back to clients. Each component plays a distinct role in this lifecycle, and together they allow vLLM to achieve state-of-the-art serving performance.


For AI engineers looking to deploy LLMs, we hope understanding this lifecycle helps in tuning and customizing vLLM. For others, we hope this breakdown provides insight into vLLM's inner workings. By doing so, we aim to demystify how large language models are served efficiently and at scale.


### References


-


[https://github.com/vllm-project/vllm](https://github.com/vllm-project/vllm)


-


[https://blog.vllm.ai/2025/01/27/v1-alpha-release.html](https://blog.vllm.ai/2025/01/27/v1-alpha-release.html)
