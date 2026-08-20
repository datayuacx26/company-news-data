---
schema_version: "1.0.0"
document_id: "b1bfc2f6e6c3e93efd2819a9e8e05e2422d51a5d39fb19ded98e3db6f4fdff85"
company_key: "yc-ctgt"
company: "CTGT"
source_id: "yc-ctgt-news-import-358f70d55d44"
canonical_url: "https://www.ctgt.ai/research/a-feature-level-approach-to-mitigating-bias-and-censorship-in-deepseek-r1"
published_at: "2025-03-14T00:00:00+00:00"
first_seen_at: "2026-07-27T01:23:44.579296+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:7f726ca7b6aba238ab00608d8af800c2991d0fc613837830f7677f16e2c0a6b1"
---

# A Feature-Level Approach to Mitigating Bias and Censorship in DeepSeek-R1

Bias and censorship in AI models restrict valid responses and reduce their overall usefulness, as seen in DeepSeek-R1 and similar models. Post-training fine-tuning and reinforcement learning aim to improve safety but frequently result in over-censorship, adding inefficiencies and limiting adaptability.


Perplexity’s R1 1776 demonstrated a dataset-based post-training approach to override embedded restrictions in models like DeepSeek-R1. However, these solutions remain static and require extensive manual intervention for further refinements.


A feature-level intervention framework modifies internal activations responsible for censorship in` DeepSeek-R1-Distill-Llama-70B` , providing a more efficient and adaptable alternative. Unlike retraining-based approaches, this method dynamically adjusts censorship behavior at inference time while preserving the model’s reasoning capabilities with minimal computational overhead.


#### Methodology


We aim to locate and intervene in internal features that drive high-level behaviors. If a model refuses a request, it is because certain latent variables (neurons or directions in the feature space) have activated. By isolating and modifying these variables, we can alter model behavior dynamically without modifying its weights.


The method consists of three core steps:


##### Feature identification:


A structured set of prompts triggers known censorship behaviors. These prompts span different topics (political, social, historical) and include control prompts that should not be refused, allowing differentiation between general difficulty and censorship.


##### Feature isolation:


We analyze hidden-layer activations and extract a subspace correlated with refusals. A distinct pattern (fcensor) emerges across censorship-triggering inputs.


##### Dynamic feature modification:


By modulating fcensor at inference time, we adjust the model’s censorship behavior dynamically, restoring responses without degrading factual accuracy.


#### Experimental results


To test this approach, we evaluated the modified model against a benchmark of 100 sensitive queries and measured its accuracy across reasoning and coding tasks. The baseline DeepSeek-R1-Distill-Llama-70B fully answered 32% of prompts. Our modified version achieved a **100% response rate.** Runtime overhead was negligible.


We evaluated performance on neutral tasks, including reasoning, mathematics, and coding. Accuracy remained statistically unchanged across modes. Perplexity variations were below 0.5%, confirming minimal impact on general language modeling capability.


#### Case studies


The effectiveness of this approach is demonstrated through several key examples:


These results highlight the potential of feature-level intervention to refine model behavior, allowing for nuanced, context-aware responses while preserving the integrity of the model’s core reasoning.


#### Future applications


While this study was conducted on` DeepSeek-R1-Distill-Llama-70B` , the methodology is designed to be adaptable across different LLM architectures.


Modifying internal representations linked to censorship allows for real-time, precise control over model behavior without retraining. Beyond censorship mitigation, this method offers a structured way to refine outputs for broader model adjustments, making it applicable across different deployments.


Feature-level intervention moves beyond theory into real-world application. Instead of treating AI models as static systems that require costly retraining to adjust behavior, this method enables dynamic refinements at inference time. Enterprises gain a new level of control, shaping model responses to align with policy guidelines without compromising reasoning capabilities.


#### What's next?


This work is part of a broader effort to move beyond rigid, static AI systems. Rather than relying on post-training fixes that introduce inefficiencies, feature-level intervention provides a more flexible and computationally efficient alternative. As AI adoption grows, models need to be adaptable in real-world environments while maintaining accuracy and reliability.


With the momentum from[our latest funding round](https://www.ctgt.ai/ctgt-raises-7m-to-help-enterprises-break-through-the-limits-of-ai-compute) , we are continuing to develop methods that make AI systems more responsive in production, giving enterprises new tools to shape model behavior in ways that were not previously possible. Feature-level intervention is one step toward building AI that is not just more scalable, but fundamentally more adaptable to real-world needs.


Read the paper[here](https://hal.science/hal-04992348v1/file/r1bias.pdf) .


This work was made possible with support from[Etched](https://www.etched.com/) and[Nebius.](https://nebius.com/)
