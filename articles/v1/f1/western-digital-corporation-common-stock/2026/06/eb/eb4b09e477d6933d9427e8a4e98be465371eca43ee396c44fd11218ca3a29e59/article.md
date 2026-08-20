---
schema_version: "1.0.0"
document_id: "eb4b09e477d6933d9427e8a4e98be465371eca43ee396c44fd11218ca3a29e59"
company_key: "western-digital-corporation-common-stock"
company: "Western Digital Corporation"
source_id: "western-digital-corporation-common-stock-rss-a1b03adbb2f1"
canonical_url: "https://blog.westerndigital.com/ai-storage-architecture-flash-hdd/"
published_at: "2026-06-05T21:25:44+00:00"
first_seen_at: "2026-07-20T23:21:38.045244+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:f510ee65095ab451d8772e254c17ce9d70f5ec65d8d58a483566c1ee96174a69"
---

# Flash Handles the Moment. HDDs Handle the Lifetime.

June 5, 2026


9 min read


[Technology](https://blog.westerndigital.com/category/technology/)


# Flash Handles the Moment. HDDs Handle the Lifetime.


[Ahmed Shihab](https://blog.westerndigital.com/?guest_author=ahmed-shihab)


*How training, inference, and the data inference produces have together reshaped cloud storage, and why both flash and HDDs are winning.*


## Compute digests demand. Data compounds.


That distinction, simple as it sounds, explains why the flash-replaces-everything narrative is wrong in a way that matters. Over the past several years, much of the industry’s focus has centered on optimizing the read side of AI systems—including model weights, KV cache, embeddings. But almost no one was watching what inference was producing. Every inference call is also a write, and every interaction adds to a pool that doesn’t shrink. Most people weren’t watching that side of the equation—and it’s where the real story is.


So, in fact, AI didn’t collapse the storage stack—it made it deeper. Training put enormous new demands on the bulk capacity tier. Inference invented a genuinely new flash tier that didn’t exist before. And the data that inference itself produces keeps the capacity-optimized HDD tier growing faster than almost anyone expected.


Understanding why requires treating three workloads as distinct: training, inference consumption, and inference output. Blend them together and you’ll miss the architecture—especially the biggest driver of HDD growth in the system. A core misunderstanding drives the flash-replaces-everything narrative: treating storage like compute. Compute digests demand—jobs complete, hardware refreshes, software gets more efficient. But data doesn’t work that way. Data compounds. Every training run, every inference, every interaction adds to a pool that doesn’t shrink, and AI makes this dramatically more pronounced. A short AI-generated video creates logs, traces, intermediate outputs, and metadata that can rival the output itself in size. Multiply that across billions of interactions per day, and data growth becomes structural—not incidental.


The question “Does flash replace HDDs?” That’s a compute framing. The better question is: “What does compounding data require from the system, and where does it naturally live?”


## Training: large footprint, familiar pattern


Training looks like a high-performance storage problem: very large datasets, models reading through them repeatedly. The pattern is more familiar than it first appears. Most data is accessed sequentially in large blocks, a workload HDDs handle well. For many large-scale training environments, storage is not typically the primary constraint on GPU utilization once data is staged close to compute.1


The architecture reflects this. A small workload is staged close to compute on fast flash, and everything else lives in bulk storage on HDDs. Meta’s AI Research SuperCluster illustrates the ratio: 46 petabytes of NVMe cache storage serving 6,080 GPUs—roughly 7.5TB of cache per GPU against a total storage footprint measured in exabytes.1


Checkpointing is the exception: bursty, high-throughput writes that flash handles at the moment, before those checkpoints migrate to capacity-optimized storage infrastructure for long-term retention.


That’s training—a workload the industry knows how to reason about. Inference is different.


## Inference: where everything changed


Inference now dominates AI systems—according to MIT Technology Review, roughly 80 to 90 percent of all AI computing power goes to inference, not training.2 Every response generated, every API call, every automated workflow. Inference runs continuously, not episodically like training, and it wants fundamentally different storage.


Inference reads from four categories. Model weights—loaded into GPU memory—are flash, small footprint.


KV cache: a structured representation of everything the model has processed in a session, roughly 1MB per token. A 100,000-token conversation generates around 100GB of KV cache. At millions of concurrent sessions, the aggregate is hundreds of petabytes of ephemeral context that needs to be accessible in milliseconds.


Vector databases and RAG embeddings are random access, read heavy, latency critical, drawing from flash. The source documents behind those indexes are large and read occasionally, pulling from HDDs. Interestingly, inference is used to generate these vectors.


The industry converged on the same solution independently. What emerged was not simply a new product category, but a fundamentally new infrastructure layer created specifically to sustain inference at scale: a petabyte-scale flash tier between GPU memory and bulk storage, built specifically for KV cache and vector databases. NVIDIA calls it the Inference Context Memory Storage platform.3 WEKA calls it the Augmented Memory Grid.4 Pure Storage released KVA.5 Three vendors, building the same layer. That’s not a product trend—that’s an architectural necessity nobody had a name for three years ago. But all of that is still only the read side of inference. The write side is where the storage conversation usually stops—and where it shouldn’t.


## The part everyone forgets: inference writes


Here’s where the storage conversation usually stops—with what inference reads. But every inference call also writes, continuously. At hyperscale, those writes don’t disappear, they become operational history, compliance records, retrieval context, synthetic training data, and future model inputs. It’s this inference write stream that is arguably becoming the single biggest driver of HDD growth in the industry.


There are four output streams from inference, each with different retention requirements.


The first is the response itself—the chatbot reply, the generated image, the code completion, or the new molecule AI designed. From an infrastructure standpoint, this is largely ephemeral for the provider. It goes to the user’s storage.


The second is session state and application data—conversation history, intermediate computations, agent context that needs to persist between turns, new vectors for generated data. This lives on flash while active, then cools and migrates to bulk storage.


The third is compliance and audit data. As AI moves deeper into regulated environments, retention requirements can expand significantly. OpenAI’s documentation notes default abuse monitoring logs retain prompts and responses for up to 30 days.6 Some regulations like the EU AI Act can have retention windows of up to 10 years for certain documentation of high-risk AI systems.7 At hyperscale, that retention lands on whatever medium is cheapest per TB, which is HDDs.


The fourth is synthetic training data. Outputs generated today increasingly become part of tomorrow’s training corpus. Meta has documented using 405B model outputs to improve smaller models in the Llama family8—a feedback loop where inference output continuously contributes to the next training cycle. That retained output goes straight to HDD bulk storage.


> Every inference call is also a write. At hyperscale, most of those writes end up on spinning disks—forever.


Two of the four streams go straight to HDD and stay there, often for years. One is flash temporarily, then HDD. Only one largely disappears from the provider’s storage concerns. This is genuinely new behavior: traditional workloads don’t have a self-reinforcing output-to-training feedback loop. AI inference generates data that feeds the next training cycle, which produces better models, which serve more inference, which generates more training data. Every cycle writes to the HDD bulk tier. And much of this retained data remains operationally valuable long after the original inference cycle completes.


## Two tiers became four


Traditional cloud storage was two tiers: fast cache for hot data, HDDs for everything persistent. AI expanded that into four. At the top: flash for model weights and GPU memory overflow—tiny footprint, latency-critical. Below that: the KV cache tier—petabytes per pod, ephemeral, entirely new. Below that: vector databases and RAG embeddings on flash. At the bottom—the largest tier by far, growing fastest—bulk retention on HDDs: training corpora, checkpoints, session history, compliance archives, inference logs, and accumulating synthetic training data.


The new flash tiers didn’t replace the bulk tier. Flash grew significantly, but above the HDD tier, not in place of it. And the bulk tier kept growing faster than expected, because inference output created a massive new write stream that no previous architecture had.


> Training made the bulk tier bigger. Inference consumption made the flash tier deeper and bigger. Inference output made the bulk tier bigger again, faster than before.


## Both tiers are winning


The flash-replaces-everything narrative made sense if you were only watching the read side of AI. But inference also writes—continuously, at hyperscale, into retention windows that can be measured in years. That stream lands on bulk storage and stays there, feeding the next training cycle, which produces more inference, which writes more data. It has no analog in traditional workloads. This is a structural change in data storage.


Flash grew significantly. The HDD bulk tier grew faster than expected. The cost multiple widened because the workload demanded both—and still does.9


The reads made flash indispensable. The writes made HDDs irreplaceable. Combined, the AI data system delivers performance and cost efficiency, at scale.


The companies that scale AI most successfully over the next decade will not simply be those with the most compute, but those that architect data systems capable of sustaining persistent growth economically, reliably, and at global scale.


1. Meta AI. “Introducing the AI Research SuperCluster.” Meta AI Blog, January 2022.[https://ai.meta.com/blog/ai-rsc/](https://ai.meta.com/blog/ai-rsc/)
2. Hao, K. and Woodard, C.. “We did the math on AI’s energy footprint.” MIT Technology Review, May 2025.[https://www.technologyreview.com/2025/05/20/1116327/ai-energy-usage-climate-footprint-big-tech/](https://www.technologyreview.com/2025/05/20/1116327/ai-energy-usage-climate-footprint-big-tech/)
3. NVIDIA Newsroom. “NVIDIA BlueField-4 Powers New Class of AI-Native Storage Infrastructure for the Next Frontier of AI.” NVIDIA Press Release, January 2026.[https://nvidianews.nvidia.com/news/nvidia-bluefield-4-powers-new-class-of-ai-native-storage-infrastructure-for-the-next-frontier-of-ai](https://nvidianews.nvidia.com/news/nvidia-bluefield-4-powers-new-class-of-ai-native-storage-infrastructure-for-the-next-frontier-of-ai)
4. WEKA. “Demystifying the BlueField-4 & Inference Context Memory Storage Announcement.” WEKA Blog, February 2026.[https://www.weka.io/blog/ai-ml/demystifying-the-bluefield-4-inference-context-memory-storage-announcement/](https://www.weka.io/blog/ai-ml/demystifying-the-bluefield-4-inference-context-memory-storage-announcement/)
5. Pure Storage. “Breaking Enterprise AI Bottlenecks: 20X Faster Inference with the First KV Cache for S3 and NFS.” Pure Storage Blog, July 2025.[https://blog.purestorage.com/purely-technical/20x-faster-inference-first-kv-cache-for-s3-and-nfs/](https://blog.purestorage.com/purely-technical/20x-faster-inference-first-kv-cache-for-s3-and-nfs/)
6. OpenAI. “Data controls in the OpenAI platform.” OpenAI Developer Documentation, 2025.[https://developers.openai.com/api/docs/guides/your-data](https://developers.openai.com/api/docs/guides/your-data)
7. European Parliament. “Article 18: Documentation Keeping,” EU Artificial Intelligence Act (Regulation 2024/1689). EUR-Lex, 2024.[https://artificialintelligenceact.eu/article/18/](https://artificialintelligenceact.eu/article/18/)
8. Meta AI. “Introducing Llama 3.1: Our most capable models to date.” Meta AI Blog, July 2024.[https://ai.meta.com/blog/meta-llama-3-1/](https://ai.meta.com/blog/meta-llama-3-1/)
9. Mellor, Chris. “VDURA says enterprise SSDs now 16 times more expensive than disk drives.” Blocks and Files, January 20, 2026.[https://www.blocksandfiles.com/disk/2026/01/20/vdura-says-enterprise-ssds-now-16-times-more-expensive-than-disk-drives/4090513](https://www.blocksandfiles.com/disk/2026/01/20/vdura-says-enterprise-ssds-now-16-times-more-expensive-than-disk-drives/4090513)


[Artificial Intelligence](https://blog.westerndigital.com/tag/ai/)[Cloud](https://blog.westerndigital.com/tag/cloud/)[Flash](https://blog.westerndigital.com/tag/flash/)[HDD](https://blog.westerndigital.com/tag/hdd/)
