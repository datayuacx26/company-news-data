---
schema_version: "1.0.0"
document_id: "a4b6ce1d30cbdec8ced5ab5f560e53080039921c6f89e9c7f7005094dd7e7deb"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/cvpr-2026-real-time-vision"
published_at: "2026-05-24T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:42f761b6d30b18e239d33dcb7bd1d3fd966bd8e759551a50236d2b3ebb3f3ccf"
---

# CVPR 2026: Papers on Real-Time Vision

CVPR 2026 accepted[roughly 4,000 papers](https://openaccess.thecvf.com/CVPR2026?day=all) . In our review of the accepted list, we found around 50 that touch on the problem Overshoot is building around: making vision-language models work in real time.


This shortlist focuses on the papers most relevant to real-time visual intelligence.


## 1. StreamingTOM: Streaming Token Compression for Efficient Video Understanding


By[Xueyi Chen](https://yige24.github.io/) ,[Keda Tao](https://kd-tao.github.io/) ,[Kele Shao](https://cokeshao.github.io/) ,[Huan Wang](https://huanwang.tech/) ·[arXiv ↗](https://arxiv.org/abs/2510.18269)


When Streaming Vision Language Models ingest a live video stream, the KV cache the model attends to grows with every new frame. This results in more compute and memory requirements over time. StreamingTOM is a training-free compressor with three mechanisms:


1. It caps the number of vision tokens for each new frame by identifying tokens that changed since the last frame. This results in lower prefill compute for each new frame and a smaller KV cache.
2. It also quantizes the KV cache to INT4, which further reduces the memory footprint and bandwidth (group-aligned quantization where group = frame).
3. For a given user query` q` , StreamingTOM only attends to the top-K relevant frames.


The result is 2× faster TTFT, 15.7× smaller KV cache, and 1.2× lower peak memory against LiveVLM, while reaching 63.8% average offline accuracy and 55.8% on RVS streaming.


**Why does this matter for Overshoot?** A bounded, quantized KV cache results in predictable latency for each user query, which is paramount for most real-time vision applications.


## 2. WeaveTime: Streaming from Earlier Frames into Emergent Memory in VideoLLMs


By[Yulin Zhang](https://zhangyl4.github.io/) ,[Cheng Shi](https://chengshiest.github.io/) ,[Sibei Yang](https://sibeiyang.github.io/) ·[arXiv ↗](https://arxiv.org/abs/2602.22142)


When a Video-LLM answers questions over a live stream, it has to know what is happening now versus what happened earlier. Offline-trained Video-LLMs often treat buffered frames like a bag of evidence/context, so they can confuse event order and let old frames distract from the current observation. This hurts temporal questions and also increases per-query latency as the stream history grows.


WeaveTime is a fine-tuning and retrieval framework that teaches temporal order, then uses history only when needed:


1. It adds Streaming Order Perception Enhancement (SOPE), a lightweight fine-tune with timestamp tokens and a temporal reconstruction task, so the model learns frame order without specialized streaming data.
2. It introduces Past-Current Dynamic Focus Cache (PCDF-Cache), which answers from the current observation by default and triggers past-frame retrieval only when model confidence is low.
3. When retrieval is triggered, it searches coarse-to-fine: first over broad historical segments, then over the most relevant frames, instead of attending to the full buffered stream.


The result is better streaming QA with lower latency: WeaveTime improves over StreamBridge/ReKV by up to +7.10% on OVO-Bench Real-Time and +3.74% on Streaming-Bench Real-Time, with the largest gains on temporal tasks.


**Why does this matter for Overshoot?** WeaveTime gives a query-time path for real-time video: answer from the live frame first, and only retrieve history when uncertainty says the current frame is not enough.


## 3. Think-as-You-See: Streaming Chain-of-Thought Reasoning for Large Vision-Language Models


By Jialiang Zhang, Junlong Tong, Junyan Lin, Hao Wu, Yirong Sun, Yunpu Ma, Xiaoyu Shen ·[arXiv ↗](https://arxiv.org/abs/2603.02872)


When a VLM reasons (CoT) over a live video, there are two main approaches: (1) batch-CoT watches the full clip at query-time and then starts reasoning. This approach does not work well for live streams since it is backwards-looking. (2) interleaved-CoT alternates "read a frame, emit a thought, read a frame, emit a thought" on a single KV cache (so ingestion stalls every time the model thinks, breaking real-time frame rates).


Think-as-You-See (TaYS) is a fine-tuning-based framework that lets the model decode reasoning tokens in parallel with frame ingestion. Instead of frame → thought → frame → thought, you get a continuous frame stream and a continuous thought stream from the same model on the same GPU.


It does that by splitting the KV cache into two regions, one for visual tokens and one for reasoning tokens. Visual tokens attend to the visual KV cache region, and reasoning tokens attend to both. TaYS also uses a modified RoPE as well as a streaming-aware attention mask.


The result is improved reasoning accuracy by 2.9% over batch-CoT and faster TTFT.


**Why does this matter for Overshoot?** Reasoning improves model performance significantly for vision tasks. However, this often comes at the cost of reaction time. This parallel ingest/think paradigm unlocks a lot of use-cases in robotics and manufacturing where a VLM must reason over a live feed to understand if an SOP is being followed correctly.


## 4. MuKV: Multi-Grained KV Cache Compression for Long Streaming Video Question Answering


By Junbin Xiao, Jiajun Chen, Tianxiang Sun, Xun Yang, Angela Yao ·[arXiv ↗](https://arxiv.org/abs/2605.22269)


When a VLM answers questions over a long live video stream, every new frame adds new vision tokens to the KV cache, resulting in compute and memory footprint growing with stream length. When a user asks a question, the relevant frames might get diluted by thousands of unrelated tokens.


A popular approach to address this is composed of three steps: (1) compress the KV cache, (2) store it offline, (3) at user-query time, retrieve the most relevant tokens and pass them through the LLM.


MuKV proposes a new KV cache compression and retrieval strategy.


1. Frames are grouped into segments of 4 frames. A segment-grain contains every frame/patch in a segment. A frame-grain contains a representative frame in the segment (middle frame). A patch-grain partitions the frame into fewer "super" patches.
2. When a segment fills, three parallel prefills run. During prefill, each granularity attends to other KV caches of similar granularity.
3. At the end of prefill, the KV cache of each granularity is compressed using self-attention scores and frequency-based distinctiveness.
4. At query time, blocks at all three grains are scored in parallel against the question. The top segment matches form a "scene filter" that is then used to "rerank" the patch and frame candidates.


The result is improved accuracy on streaming benchmarks and lower query latency.


**Why does this matter for Overshoot?** At the time of writing, Overshoot limits the stream length to 10 min. Solutions like MuKV can help us support an open-ended live stream for users.


## 5. DeDelayed: Deleting Remote Inference Delay via On-Device Correction


By Dan Jacobellis, Mateen Ulhaq, Fabien Racapé, Hyomin Choi, Neeraja J. Yadwadkar ·[arXiv ↗](https://arxiv.org/abs/2510.13714)


When a power-constrained device (e.g. a robot or a drone) needs real-time video prediction like semantic segmentation, it hits a compute wall with two ways out: (1) offload to a cloud GPU and eat the round-trip latency hit, (2) run a small local model on-device. Path (1) computes on a past frame` x_(t-a)` rather than the live frame` x_(t)` , then returns at` x_(t+b)` a prediction that can be spatially misaligned with the current visual state; path (2) trades time for accuracy with a small, low-res model. The misalignment is an issue for real-time use (e.g. collision avoidance, obstacle control), and it gets worse with both longer delay (` d=a+b` ) and faster scene motion.


DeDelayed is a co-inference framework: the cloud runs a heavy model that produces features that are then processed by a small on-device model on the fresh frame. There are three main contributions:


1. The remote model predicts the current frame features from a delayed frame, conditional on the estimated round-trip delay.
2. Local and remote features fuse by element-wise addition into one path; if remote features don't arrive, the local model can still run as a fallback.
3. The local model runs low-res on the current frame; the remote runs a 3D ViT over multiple high-res frames, supplying temporal and visual detail the local model alone may miss.


The result is higher segmentation accuracy than local-only or remote-only baselines under tested delays (0 to 167 ms).


**Why does this matter for Overshoot?** It could show us how to keep a delayed large model aligned with the live state by making its output delay-aware and correcting it with the current frame.


## 6. Color When It Counts: Grayscale-Guided Online Triggering for Always-On Streaming Video Sensing


By Weitong Cai, Hang Zhang, Yukai Huang, Shitong Sun, Jiankang Deng, Songcen Xu, Jifei Song, Zhensong Zhang ·[arXiv ↗](https://arxiv.org/abs/2603.22466)


When an edge or wearable device (e.g. smart glasses) needs always-on video understanding, continuous RGB becomes the bottleneck. RGB capture, ISP, encoding, wireless transport, and visual-token decoding all scale with how much full-color video is processed. Full RGB preserves visual context but drains battery quickly; sparse trigger-only capture saves power but can miss important moments and leave visual gaps.


ColorTrigger is a training-free framework built around a grayscale-always, color-on-demand stream. A low-power grayscale camera runs continuously, while RGB is triggered only when the grayscale stream suggests the current frame is informative.


It does three things:


1. A causal grayscale module extracts CLIP features from a sliding window of recent frames and compares them to the current frame to decide whether it is informative enough to request RGB.
2. A controller rate-limits RGB triggering based on a credit budget to keep color acquisition bounded over long streams.
3. Grayscale frames go through a low-resolution, low-token path and RGB frames go through a high-resolution, high-token path, while preserving temporal order and reducing decoder compute cost.


The result is 92% of full-color InternVL-3.5-8B performance using only 8% of RGB frames.


**Why does this matter for Overshoot?** It is an interesting view on lowering real-time video cost: keep most frames cheap, then spend high-token processing only when the stream becomes informative.


## 7. Streaming Video Instruction Tuning


By[Jiaer Xia](https://jiaerxia.github.io/) ,[Peixian Chen](https://scholar.google.com/citations?user=WRGF9B8AAAAJ&hl=zh-CN&oi=ao) ,[Mengdan Zhang](https://scholar.google.com/citations?user=C941xtsAAAAJ&hl=zh-CN&oi=ao) ,[Xing Sun](https://scholar.google.com/citations?user=IUtix9IAAAAJ&hl=zh-CN&oi=ao) ,[Kaiyang Zhou](https://kaiyangzhou.github.io/) ·[arXiv ↗](https://arxiv.org/abs/2512.21334)


When a video LLM ingests a live stream, the model needs a per-moment decision of whether to stay silent, wait, or respond. Existing online systems add a separate controller that decides when to fire. However, this adds latency and separates response timing from generation.


Streamo is a fine-tuning framework that puts the response decision inside the model itself:


1. It adds three state tokens:` <Silence>` ,` <Standby>` , and` <Response>` . Silence means nothing relevant has happened, Standby means the relevant event is happening but not finished, and Response means the model should answer now.
2. It trains these state tokens with focal loss and class balancing, since most stream moments are Silence and naive training can collapse into never responding.
3. It builds Streamo-Instruct-465K, a large multi-task dataset that teaches the model real-time narration, action/event captioning, event grounding, time-sensitive QA, and offline QA with consistent time labels.


The result is a streaming VLM that understands video and decides when to speak, with Streamo-7B beating Dispider (prior SOTA) by +13.83% on OVO-Bench while also improving over its Qwen2.5-VL-7B base across offline video benchmarks.


**Why does this matter for Overshoot?** Streamo gives a mechanism for proactive video outputs: the model learns when an event starts, when to wait, and when to respond.


---


Browse the full[CVPR 2026 accepted-paper list](https://cvpr.thecvf.com/virtual/2026/papers.html) . If you find a paper that pushes real-time vision or streaming inference forward, send it our way.


See you in Denver!
