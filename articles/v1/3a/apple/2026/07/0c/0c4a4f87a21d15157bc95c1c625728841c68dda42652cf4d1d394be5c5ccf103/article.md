---
schema_version: "1.0.0"
document_id: "0c4a4f87a21d15157bc95c1c625728841c68dda42652cf4d1d394be5c5ccf103"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/length-value-model"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:3d4f7deeebb562d38004396a730fe82804cc270ceea2f51f6ab56b09b21f6995"
---

# Length Value Model: Scalable Value Pretraining for Token-Level Length Modeling

[View publication](https://arxiv.org/abs/2604.27039)


Token serves as the fundamental unit of computation in modern autoregressive models, and generation length directly influences both inference cost and reasoning performance. Despite its importance, existing approaches lack fine-grained length modeling, operating primarily at the coarse-grained sequence level. In this paper, we introduce the Length Value Model (LenVM), a token-level framework that models the remaining generation length at each decoding step. By formulating length modeling as a value estimation problem and assigning a constant negative reward to each generated token, LenVM predicts a bounded, discounted return that serves as a monotone proxy for the remaining generation horizon. This formulation yields supervision that is annotation-free, dense, unbiased, and scalable. Experiments on LLMs and VLMs demonstrate that LenVM provides a highly effective signal at inference time. On the LIFEBench exact length matching task, applying LenVM to a 7B model improves the length score from 30.9 to 64.8, significantly outperforming frontier closed-source models. Furthermore, LenVM enables continuous control over the trade off between performance and efficiency. On GSM8K at a budget of 200 tokens, LenVM maintains 63 percent accuracy compared to 6 percent for token budget baseline. It also accurately predicts total generation length from the prompt boundary. Finally, LenVM’s token-level values offer an interpretable view of generation dynamics, revealing how specific tokens shift reasoning toward shorter or longer regimes. Results demonstrate that LenVM supports a broad range of applications, including length control, prediction, and interpretation of generation dynamics. They suggest that generation length can be effectively modeled as a token-level value signal, highlighting the potential of LenVM as a general framework for length modeling and as a length-specific value signal that could support future RL training.


- † University of California, Santa Barbara
- ‡ Carnegie Mellon University
- § LMSYS Org
- ¶ University of Wisconsin–Madison
