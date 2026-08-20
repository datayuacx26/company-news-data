---
schema_version: "1.0.0"
document_id: "f27edc2cf614497e58b82740f0b9206959ae8314ef00fbdf1d4568091748de13"
company_key: "yc-lightly"
company: "Lightly"
source_id: "yc-lightly-news-import-b9754ffe9935"
canonical_url: "https://lightly.ai/blog/nvidia-b200-vs-h100"
published_at: null
first_seen_at: "2026-07-24T02:26:12.148445+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:85fa0f90db7dc6acfe0c2900ec79ee816aad268ee71ec323ee51f90c31a9cf0d"
---

# NVIDIA Blackwell B200 vs H100: Real-World Benchmarks, Costs, and Why We Self-Host

‍


We got early access to NVIDIA's new Blackwell B200 GPUs and benchmarked them against cloud-based H100s ([Nebius](https://nebius.com/) ) on real-world AI tasks: computer vision pretraining (YOLOv8+DINOv2) and LLM inference ([Ollama](https://ollama.com/) with Gemma/DeepSeek).


We were among the first startups in Europe to get our hands on the new B200s. Instead of relying on synthetic benchmarks, we ran the workloads that actually matter to us:


- Computer Vision Pretraining on ImageNet-1k using[LightlyTrain](https://www.lightly.ai/lightlytrain) (YOLOv8 + DINOv2)


- LLM Inference using Ollama (Gemma 27B, DeepSeek 671B)


We’re running it in a self-hosted setup at[GreenMountain](https://greenmountain.no/) in Norway, powered entirely by renewable energy. This post covers the full benchmark data, cost comparisons, power draw, and why we decided to build our own cluster.


**Note:** This post is not sponsored by or in any way affiliated with[NVIDIA](https://nvidia.com/) .


**Figure 1:** Ollama realtime inference using Deepseek-R1 on the DGX B200. By Author.


## B200 vs H100 — Performance Summary


Before diving into benchmarks, here's the configuration of our self-hosted, power-efficient B200 cluster used for testing. It's designed for continuous, heavy-duty AI workloads 24/7:


**Self-Hosted Cluster Configuration:**


- GPUs: 8x NVIDIA B200


- Memory per GPU: 192 GB HBM3e


- CPU cores: 112


- RAM: 2 TB


- Storage: 33.28 TB NVMe SSD


- Hosting: GreenMountain (Norway, renewable-powered)


We chose GreenMountain for its commitment to sustainability (100% renewable energy) and its rock-solid infrastructure, providing the stability needed for pushing high-end GPUs around the clock.


**Figure 2:** Screenshot of nvidia-smi when we finally managed to bring the GPUs close to max power. By Author.


**Note:** The H100 comparison benchmarks were run using **8x H100 GPUs on Nebius Cloud instances** (specific instance details in the cost comparison section).


Here’s how the B200 stacks up against the H100 at a glance.


**Table 1:** B200 vs H100 at glance. By Author.


## Architecture Comparison: B200 vs H100


The performance gains aren’t just anecdotal—they’re grounded in major architectural upgrades. Below is a head-to-head comparison of the B200 and H100 on specs that actually move the needle for ML workloads: memory, bandwidth, and tensor throughput.


**Table 2:** B200 vs H100 Architecture Comparison. By Author.


The B200 boasts **over double the memory capacity** , roughly **2.4 times the memory bandwidth** , and **more than double the peak FP16/BF16 compute throughput** compared to the H100.


## What We Benchmarked and Why


We focused on two distinct, real-world tasks representing different ends of the ML compute spectrum. These stress GPUs in different ways and reflect common production workloads:


- **Computer Vision Pretraining (YOLOv8-x + DINOv2 on ImageNet-1k):** This is a **GPU-bound, throughput-sensitiv** e task. It benefits significantly from high parallelism, large batch sizes, and massive memory capacity/bandwidth for processing the ~1.28 million images in the ImageNet-1k dataset.


- **LLM Inference (Ollama with Gemma 27B & DeepSeek 671B):** This task is often **latency-sensitive and memory-bandwidth constrained** , especially with large models. We tested with quantized models (Q4_K_M) and a batch size of 1 to simulate interactive chatbot or Q&A scenarios where quick responses are crucial.


Testing both heavy training and latency-critical inference helps reveal where the B200's architectural improvements provide the most significant advantages.


## Computer Vision Benchmarks: YOLOv8 + DINOv2


For our computer vision test, we pretrained a YOLOv8-x model on the full ImageNet-1k dataset (~1.28 million images) using DINOv2 for distillation, leveraging our new LightlyTrain framework. We compared our 8x B200 setup against an 8x H100 setup on Nebius Cloud.


We trained YOLOv8-x models using DINOv2 distillation on the full ImageNet-1k training set (1.28M images). This workload benefits from larger batch sizes and memory bandwidth.


**Table 3:** Computer Vision benchmarking. By Author.


The B200 is around 33% faster for AI model training than the H100 for the same workload. If we make use of the larger memory to increase the batch size we can increase the speedup to 57%.


**Results:** Our self-hosted B200 consistently trained **up to 57% faster** than the cloud-based H100 at the same batch size (2048). Furthermore, the B200's larger 192GB memory allowed us to easily double the batch size to 4096, achieving even higher throughput. This clearly demonstrates the B200's raw performance advantage for scaling GPU-bound training tasks, even comparing self-hosted next-gen to cloud current-gen.


> **💡Pro tip:** For understanding how dataset volume impacts on-device throughput, our[Too Much Data on the Edge? How to Build Data Pipelines for Edge AI](https://www.lightly.ai/blog/too-much-data-on-the-edge-how-to-build-data-pipelines-for-edge-ai) blog outlines strategies to reduce edge-processing load.


## LLM Inference Benchmarks: Gemma 27B + DeepSeek 671B


We evaluated LLM inference speed using **Ollama** , comparing our B200s against H100s on Nebius. Ollama was chosen as it was one of the more accessible frameworks providing early support for the Blackwell architecture. We ran models in 4-bit quantized format (Q4_K_M) with batch size = 1.


**Table 4:** H100 vs B200 on Gemma 27B and DeepSeek 671B. By Author.


For the mid-sized Gemma 27B model, the B200 showed a clear **~10% speedup** in token generation. However, for the massive DeepSeek 671B model, B200 performance was roughly on par (or slightly slower) than the H100 using Ollama.


Note that at the time of testing, optimized inference frameworks like vLLM were not easily available or stable on the new Blackwell hardware. Running such a large model with Ollama (batch size 1) likely introduced overheads that masked the B200's potential hardware advantages. We expect B200 inference performance, especially for large models, to improve significantly as the software ecosystem (drivers, CUDA libraries, frameworks like vLLM, TensorRT-LLM) matures. We plan to revisit these benchmarks as the software evolves.


## Power and Utilization Observations


Raw speed is only part of the story; power consumption is crucial for operating costs and sustainability, especially in a 24/7 self-hosted setup. We monitored power draw directly using \`nvidia-smi\` during our tests.


**Table 5:** Power consumption analysis. By Author.


**Full Cluster Power:** Our entire 8xB200 node (GPUs only) drew approximately **4.8 kW** under heavy training load. Including CPUs, RAM, storage, etc., the total system power draw was roughly **6.5–7 kW** at the wall. During some tests we managed to bring the power consumption to over 900W per GPU. We split the node into two 4 GPU nodes. You can treat a single B200 node as two H100 nodes. You get almost the same performance and even more memory with 4x B200 vs 8x H100.


## Why We Self-Host Our GPUs (And Maybe You Should Too)


Running our own hardware isn't just about costs (though, as we'll show, the savings are substantial). For teams like ours running continuous, heavy AI workloads, the public cloud model for high-end GPUs started showing limitations. We switched to self-hosting because we needed:


- **Guaranteed, Consistent Performance:** No virtualization overhead, no "noisy neighbors." We get the full power of the hardware every time.
- **24/7 Availability:** No waiting for spot instances, no fighting for quota on the latest GPUs. Our cluster is ready when we are.
- **Cost Control & Predictability:** Fixed monthly costs for colocation and power, rather than variable hourly rates. We can maximize utilization without runaway bills. Idle time doesn't cost us premium GPU rates.


By colocating our 8xB200 cluster at GreenMountain, we get these benefits *plus* reliable power, cooling, and networking, all powered by 100% renewable energy. For our constant training and experimentation needs, self-hosting provides faster iteration and predictable operations.


## Cost Analysis: Cloud GPU Rental vs. Self-Hosting B200s


Let's talk numbers. How does the cost of buying and hosting B200s compare to renting equivalent H100 capacity (since B200 cloud pricing isn't available yet, but likely higher) from cloud providers? The difference is stark, especially under continuous use (approx. 730 hours/month).


**Table 6:** Cost analysis on GPU Rental vs Self-Hosting B200s. By Author.


**Key Takeaway:** Even *assuming* future cloud B200 pricing matches current H100 rates (a big assumption!), **self-hosting our B200s costs roughly $0.51 per GPU per hour** in operating expenses, **compared to $2.95-$16.10 per hour for cloud H100s** . That makes self-hosting **~6x to over 30x cheaper on an operational basis** .
If B200 cloud instances cost **more** than H100s (which is likely), the self-hosting advantage becomes even greater. We wouldn't be surprised to see B200 instances priced upwards of $3-5+ per GPU-hour on major clouds.


## **When Does Self-Hosting Pay Off? (ROI Calculation)**


Of course, self-hosting involves upfront capital expenditure (CapEx). Let's estimate the break-even point:


- **One-time CapEx:** Approx. **$400,000** for the 8x B200 GPUs themselves (server infrastructure costs are additional but smaller). *Note: This is a market estimate. Actual acquisition costs can vary, and programs like NVIDIA Inception may offer discounts to eligible startups, potentially lowering this initial investment.*
- **Monthly Operating Cost (OpEx):** Approx. **$3,000** (our cost for colocation space, power, cooling). *Note: Our actual monthly operating costs are lower than this, but we use $3,000/month as a conservative and simplified figure for these ROI calculations.*
- **Equivalent Cloud Cost (H100):** **$17,000/month** (Nebius) to **$70,000+/month** (AWS/GCP on-demand).


**Table 7:** ROI calculations. By Author.


If you’re spending $10K+ monthly on GPUs and training full time, self-hosting offers a clear path to lower total cost of ownership.


***A Note on Cloud Discounts:*** *It's important to highlight that the cloud costs used in this ROI calculation (especially for AWS/GCP) are based on* ***on-demand pricing*** *. Major cloud providers offer significant Sustained Usage Discounts (SUDs) or Reserved Instances (RIs) for long-term commitments (typically 1-3 years). These discounts can substantially lower the monthly cloud expenditure (often in the range of 30-60%), which would naturally* ***extend the calculated break-even period*** *for the self-hosted hardware compared to committed cloud usage.*


## What Surprised Us During B200 Testing


While we had high expectations, a few things stood out:


- Actual power draw was ~600W, well below the 1000W spec


- LLM inference gains were smaller than expected. Likely the software stack is not mature yet.


- YOLOv8 + DINOv2 ran at high throughput with zero tuning


- Cloud remains useful for fast hyperparameter sweeps


- Idle power draw was high at 140W/GPU—something we hope NVIDIA addresses


## What’s Next


We’re actively benchmarking more workloads:


1. **Stable Diffusion Inference** Measuring latency and throughput for SD v1.5 and SDXL.


2. **LLM Fine-Tuning** Using LoRA and QLoRA on models like Mistral and Gemma. Early results coming soon.


## Final Thoughts


The NVIDIA Blackwell B200 GPU demonstrably delivers on its promise of a significant performance leap over the H100, particularly for compute-heavy CV training and inference on mid-to-large LLMs based on our real-world tests. The **up to 57% faster training** speeds are compelling generational gains.


We're excited to continue pushing these GPUs to their limits and will share more benchmarks (like Stable Diffusion and LLM Fine-Tuning) soon. Stay tuned!


We want to express our sincere gratitude to several partners who made this testing possible. Thank you to **Nebius** for providing the cloud credits used for our H100 comparison benchmarks. A huge thank you also goes to[Amber](https://amber-ai.bg/) for their support in the procurement and setup process, and to the entire **GreenMountain** team for providing an excellent, reliable, and 100% renewable-powered environment for hosting our self-hosted B200 cluster.
