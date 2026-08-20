---
schema_version: "1.0.0"
document_id: "9cd17cb198f0969ae6e1fc72a2831ef2ed147475bab90edc595c2ff9c7bc3ec5"
company_key: "yc-sid"
company: "SID"
source_id: "yc-sid-news-import-30f4e157198f"
canonical_url: "https://www.sid.ai/research/sid-1-technical-report"
published_at: null
first_seen_at: "2026-07-25T23:59:57.623768+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:c1f7dc2d89f297679b28ad3a36bd5e85e11cc1010d668e1a0ac9efa180c370a7"
---

# SID-1 Technical Report: Test-Time Compute for Retrieval

Current retrieval practice relies on a single-step pipeline where multiple small, specialized models prompt search tools and rerank documents. In agentic retrieval, a single model performs all these tasks iteratively. We introduce SID-1, the first model trained end-to-end with reinforcement learning solely for agentic retrieval. SID-1 achieves 0.84 recall on a difficult, diverse retrieval benchmark, outperforming GPT-5.1 (0.78) and Sonnet 4.5 (0.64), at 3-4 orders of magnitude lower cost and 1-2 orders of magnitude lower latency. SID-1 nearly doubles recall compared to traditional reranking pipelines (from 0.45 to 0.84). Our model is drop-in compatible with existing retrieval systems and works as a subagent with frontier AI models. This technical report explores our infrastructure, reward design, synthetic data, and issues with common GRPO formulations.
