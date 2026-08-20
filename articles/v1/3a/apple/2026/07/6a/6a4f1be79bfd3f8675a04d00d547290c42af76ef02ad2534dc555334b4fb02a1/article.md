---
schema_version: "1.0.0"
document_id: "6a4f1be79bfd3f8675a04d00d547290c42af76ef02ad2534dc555334b4fb02a1"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/environment-free"
published_at: null
first_seen_at: "2026-07-21T19:16:24.438497+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:be49703b89e13fa36a09a82e8fa315d0ac189907d3d28aaf85d92c62bb4d8a74"
---

# Environment-free Synthetic Data Generation for API-Calling Agents

[View publication](https://arxiv.org/abs/2607.16900)


Training API-calling large language model (LLM) agents demands massive amounts of high-quality trajectories. However, collecting such data at scale typically requires fully implemented environments with executable APIs and realistic, pre-populated backend databases, creating a major bottleneck for scalability. To overcome this, we propose an environment-free synthetic data generation approach that leverages LLMs as on-the-fly digital world models. Given only API specifications, our method generates trajectories mimicking interactions between an agent and a stateful environment. Specifically, an LLM first generates diverse tasks solvable with the provided APIs. A teacher agent then iteratively solves each task while an LLM simulator generates coherent synthetic API responses conditioned on the task context and simulation history. Finally, an LLM judge filters the trajectories to ensure the quality of the resulting dataset. We evaluate our approach on the challenging AppWorld and OfficeBench benchmarks, which include both information-retrieval and state-changing tasks. Fine-tuning models on our synthetic data yields significant performance gains, demonstrating that effective supervision for API-calling agents can be generated without any executable environment. Our results establish LLM-based API simulation as a practical, scalable solution for training agents across diverse API ecosystems.
