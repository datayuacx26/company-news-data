---
schema_version: "1.0.0"
document_id: "be504bb793932ad514de16d3c6fdfb55bcf130bca956c878bfdd7f080851e561"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-news-import-9b9c5ebd5b82"
canonical_url: "https://www.nutanix.com/blog/building-enterprise-ai-stack-with-nutanix-agentic-ai"
published_at: "2026-03-16T20:26:00+00:00"
first_seen_at: "2026-07-24T12:20:54.900145+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:d64bc8c8205e5c87c58b0fede804c2f73bcbf994eb7ce5c54c22b5da37865a84"
---

# AI Series: Building the Enterprise AI Stack with Nutanix Agentic AI

In the race to deploy GenAI, the last mile is often the hardest. Data scientists and developers don’t just need raw compute; they need a cohesive ecosystem of models, security guardrails, and data connectors to build enterprise-grade agents.


Today, we are bridging that gap. By introducing the AI Gateway, expanded Model-as-a-Service (MaaS) and MCP Server Access Management capabilities within Nutanix Enterprise AI (NAI), we are providing a comprehensive, Kubernetes-native AI services layer. Combined with the curated AI Catalog (coming soon) on Nutanix Kubernetes Platform (NKP), Nutanix is delivering a production-ready AI PaaS designed for the modern enterprise.


## **One Interface, Zero Vendor Lock-in: AI Gateway (TechPreview)**


As organizations scale, they often find themselves trapped in SDK sprawl, juggling different APIs for hosted providers, local models, and fine-tuned variants. The Nutanix AI Gateway **** addresses this by providing a unified, standardized API across model vendors.


This abstraction layer does more than just simplify code; it provides critical enterprise features:


- **High Availability:** The AI Gateway can be configured to connect to multiple upstream providers. If a primary provider fails or exceeds its budget, traffic automatically routes to a healthy fallback, helping ensure your agents stay online.The Gateway offers Unified Observability across all models, hosted and self-hosted, eliminating fragmented views that may result in misleading analytics and compromised decision making.
- **Distributed Scalability:** Remote NAI clusters can be registered as providers, allowing you to leverage GPUs distributed across your entire infrastructure to eliminate throughput bottlenecks.
- **Governance and Cost Control:** With global and granular token-based rate limits per user, the Gateway acts as a financial guardrail. It prevents runaway agents from exhausting budgets, allowing builders to focus on logic rather than bill-watching.


## **Expanded Model as a Service**


We are expanding our Model-as-a-Service (MaaS) capabilities to give teams more flexibility in how they deploy intelligence.


New models in the validated catalog include the Olmo and Ministral families, complete with automated resource calculation. For those building multimodal applications, we’ve integrated NVIDIA Whisper NIM for high-performance speech-to-text.


Performance is further optimized through KV-Cache Aware routing (Tech Preview) and Speculative Decoding, which are designed to reduce "Time To First Token" (TTFT) and inter-token latency. For teams needing specialized logic, we’ve introduced LoRa-based Supervised Fine-Tuning (Tech Preview). This allows for compute-efficient tuning on private data, with the resulting models managed directly within the NAI workflow for seamless deployment and observability.


## **Securing the Hands of the Agent: MCP Server Access Management (Tech Preview)**


Model Context Protocol (MCP) is designed to allow agents to securely interact with external tools and internal databases without custom-coded integrations.


NAI provides the following capabilities for MCP Server Access Management:


- **Unified Security & RBAC for MCP Servers:** Apply API key injection at the gateway interface rather than configuring it on every individual MCP server.
- **Tool-Level Filtering:** Control specific tool capabilities (e.g., "Read Only" vs. "Write") that can be accessed by agents.
- **Enterprise Observability:** All MCP requests including latency and the specific tools being called are recorded, providing a full audit trail for AI governance.


## The NKP AI Catalog: A Curated Fast-Track to Production


The biggest hurdle for agent builders is often “Day 2" operations: managing CVEs, validating version compatibility, and stitching together disparate tools.


The upcoming NKP AI Catalog will provide a curated, validated suite of tools spanning the entire AI lifecycle:


- **Developer Tools:** Ready-to-use Jupyter Notebooks for experimentation.
- **Vector Databases:** Enabling semantic search to ground agents in private data.
- **MLOps & Frameworks:** Validated stacks for building complex agentic workflows.
- **NVIDIA NeMo Services:** Specialized tools for evaluating and fine-tuning NIMs.


The journey from a simple prompt to a governed, secure AI agent in production shouldn't be a fragmented struggle. With these updates to NAI and NKP, Nutanix is providing a unified platform that is designed to scale with your ambition. Whether you are experimenting with open-source models or deploying global-scale agents, the Nutanix AI PaaS helps ensure your infrastructure is an accelerator, not a bottleneck.
