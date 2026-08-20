---
schema_version: "1.0.0"
document_id: "8dc2f69778c6cf17d01680a747d4663bec2fd28680c4cc9dd7221bc3dc30a8f5"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/diffusion-autoregressive-performance"
published_at: null
first_seen_at: "2026-08-08T03:56:15.906765+00:00"
fetched_at: "2026-08-08T03:56:17.231059+00:00"
content_hash: "sha256:8a3f46981d3734f9ca69f438a1e9e91a61674d59e449792a9bb9329f8aefe844"
---

# Beyond Next-Token Prediction: A Performance Characterization of Diffusion versus Autoregressive Language Models

[View publication](https://arxiv.org/abs/2510.04146)


Large Language Models (LLMs) have achieved state-of-the-art performance on a broad range of Natural Language Processing (NLP) tasks, including document processing and code generation. Autoregressive Language Models (ARMs), which generate tokens sequentially conditioned on all previous tokens, have been the predominant paradigm for LLMs. While these models have achieved high accuracy across a range of downstream tasks, they exhibit low arithmetic intensity due to the inherent sequential dependency in next-token prediction. Recently, Diffusion Language Models (DLMs) have emerged as a promising alternative architecture. DLMs generate output tokens in parallel, mitigating the limitations of sequential decoding. However, the performance implications of DLMs relative to commonly deployed ARMs are not fully understood. In this work, we present a comprehensive study of the performance characteristics of ARMs and DLMs, combining theoretical analysis with empirical profiling to characterize the trade-offs between these approaches. We show that although DLMs can achieve higher arithmetic intensity than ARMs by leveraging parallelism across token positions, they fail to scale effectively with longer contexts. We then explore block-wise decoding for DLMs, which decouples arithmetic intensity from sequence length and enables better scaling to long contexts (similar to ARMs). We also examine batched inference and find that ARMs exhibit superior throughput as they benefit more from parallelism across sequences in the batch. Finally, we highlight opportunities for accelerating DLM inference, emphasizing that reducing the number of sampling steps is key for open-source DLMs to achieve lower latency relative to ARMs.


- † Seoul National University
- ‡ University of California, Berkeley
- § ICSI
- ¶ LBNL
- †† University of Texas at Austin
- * Advisory role
