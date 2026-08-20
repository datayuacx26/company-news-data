---
schema_version: "1.0.0"
document_id: "3f31829dd4777dbe33e0cb4874a93c954ed211da5da73d4b7732ae1f061e5a98"
company_key: "yc-tensorfuse"
company: "Tensorfuse"
source_id: "yc-tensorfuse-news-import-97ab2b67de47"
canonical_url: "https://tensorfuse.io/docs/blogs/small_language_model"
published_at: "2025-07-07T00:00:00+00:00"
first_seen_at: "2026-07-22T16:02:52.953662+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:5f8a0b654720d9725ab119df21c1502841e17e9770feb278d5f3d0163ad28017"
---

# Small Language Models are the Future of Agentic AI

July 7, 2025


Agam


Founder


Nvidia recently published the paper[“Small Language Models are the Future of Agentic AI”](https://arxiv.org/pdf/2506.02153) , and it is quickly gaining attention from developers and ML engineers.


The core proposition is simple: Small Language Models (SLMs) are powerful enough and practically better for building AI agents compared to large language models (LLMs).


In this post, we’ll explore the practical aspects of the paper and discuss its relevance for your AI applications.


### ​


What is an SLM?


Nvidia defines it as a *Language Model (LM) that can fit onto a common consumer electronic device.* For practical purposes, any LM with fewer than 10B parameters can be considered an SLM.


**Tasks LMs perform in an agentic system**


A typical agent decomposes complex goals into modular sub-tasks. Most of these tasks are repetitive, scoped, and non-conversational (like function calling).


Insisting on using LLMs for all agentic subtasks reflects a misallocation of computational resources. Instead, specialized or fine-tuned SLMs can effectively handle these repetitive tasks at lower cost with similar accuracy.


**Practical Benefits of Using SLMs**


1. **Performance and Efficiency:**


- While scaling laws remain valid, SLMs like Phi2 (2.7b), Deepseek Distill series, and HF SmolLM2 series have shown performance comparable to 30b parameter models from the same generation and some of the larger LLMs from 2 years back (ex: GPT-3.5 etc)
- If your tasks are narrow and well-defined, fine-tuning an SLM is much easier, especially with techniques like LoRA or DoRA.


2. **Operational Advantages:**


- **Inference Efficiency:** Serving a 7bn SLM is 10–30× cheaper (in latency, energy, and FLOPs) than a 70–175bn LLM, enabling real-time agentic responses at scale.
- **Modular System Design:** You can easily fine-tune several models of different sizes to align well with the real-world heterogeneity of agentic tasks. This sense for modularity allows for the easy addition of new skills and the ability to adapt to changing requirements.
- **Edge Deployment:** SLMs easily run on consumer hardware, simplifying edge deployment.


3. **Cost Savings:**


- Smaller models consume fewer resources, significantly reducing operational costs compared to larger LLM deployments.


### ​


**Should you use SLMs?**


Transitioning from LLMs to SLMs for your AI agents is beneficial but requires careful consideration:


- If your agent’s growth isn’t limited by improving performance on long-tail use cases or optimizing cost, SLMs might not be necessary.
- However, if your application has found product–market fit and growth depends on:


- Optimizing performance for long-tail use cases
- Reducing costs
- Ensuring security, compliance, and privacy


Then investing time, money, and resources into SLMs could offer significant advantages.


### ​


LLM-to-SLM Agent Conversion Algorithm


1. **Define your tasks** : SLMs works well for narrow and specialised tasks. Use SLMs for clearly defined and specialized tasks. Keep LLMs for general-purpose requirements and overall planning.
2. **Data Collection and Logging:** Record detailed logs of inputs, outputs, tool interactions, and performance metrics to improve future model training.
3. **Selecting the right SLM:** For each identified task, select one or more candidate SLMs. Criteria for selection include the SLM’s inherent capabilities (e.g., instruction following, reasoning, context window size), its performance on relevant benchmarks
4. **Specialised SLM fine-tuning** : Off the shelf SLMs are still not there yet. Prepare task-specific datasets from your logs and fine-tune selected SLMs using efficient methods like LoRA or QLoRA.
5. **Evals and Iteration:** Setting the right evals and ****retraining the SLMs and the router model periodically with new data is the key to maintain performance. Very important to know what does “Good” looks like for you.


### ​


**Deployment Considerations**


SLMs offer significant performance and cost benefits over LLMs for agentic tasks. However, deploying them in production requires effort in three areas:


1. **MLOps:** Set up data curation, filtering pipelines, and evaluations.
2. **Core ML:** Choose the right SLM and fine-tuning techniques.
3. **Infrastructure:** Establish infrastructure for fine-tuning and inference.


These tasks require substantial resources and are beneficial primarily when running at scale.


---


If you’re looking to deploy SLMs for your AI agents, Tensorfuse provides infrastructure support for fine-tuning and inference. We would love to chat and help in any way possible. Reach out atagam@tensorfuse.io or[schedule a call](https://calendly.com/agam-jn/30min) .
