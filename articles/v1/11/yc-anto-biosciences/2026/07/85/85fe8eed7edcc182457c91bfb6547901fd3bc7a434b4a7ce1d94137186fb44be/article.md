---
schema_version: "1.0.0"
document_id: "85fe8eed7edcc182457c91bfb6547901fd3bc7a434b4a7ce1d94137186fb44be"
company_key: "yc-anto-biosciences"
company: "Anto Biosciences"
source_id: "yc-anto-biosciences-news-import-c516c9aa21d4"
canonical_url: "https://anto.bio/publications/from-noise-to-knowledge"
published_at: null
first_seen_at: "2026-07-24T23:08:26.325303+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:aa58163cd148a9164293d8a5cd925d1764f4488d18bbc399dc32e64f3f21a90f"
---

# From Noise To Knowledge

## Enabling Foundation-Model Pretraining on Noisy, Real-World Corpora via Quality-Aware Tokenization


### Abstract


Current tokenization methods process sequential data without accounting for signal quality, limiting their effectiveness on noisy real-world corpora. We present *QA-Token (Quality-Aware Tokenization)* , which incorporates data reliability directly into vocabulary construction. Our framework introduces three technical contributions: (i) a bilevel optimization formulation that jointly optimizes vocabulary construction and downstream performance (proven NP-hard), (ii) a reinforcement learning approach that learns merge policies through quality-aware rewards with convergence guarantees, and (iii) an adaptive parameter learning mechanism via Gumbel-Softmax relaxation for end-to-end optimization.


We show that QA-Token achieves information-theoretic optimality under noisy conditions, with convergence guarantees for both policy and parameter learning. Experiments demonstrate consistent improvements: *genomics* (8.9% absolute F1 gain in variant calling, Hedges' \\(g=8.2\\)), *finance* (30% Sharpe ratio improvement). At foundation scale, re-tokenizing METAGENE-1's 1.7 trillion base-pair corpus achieves state-of-the-art pathogen detection (94.53 MCC) while reducing token count by 15%. A 1.2B parameter financial model trained with QA-Token shows 12-27% improvements across forecasting tasks. These results demonstrate that quality-aware tokenization enables effective training on noisy corpora that standard methods cannot handle.


### Introduction


Tokenization serves as the interface between raw data and neural computation. Current methods such as Byte-Pair Encoding (BPE) rely exclusively on frequency statistics, assuming that occurrence frequency correlates with semantic importance. This assumption fails when data quality varies significantly—from sequencing errors in genomics to microstructure noise in financial markets . Models trained on noisy corpora using frequency-based tokenization inherit these errors, resulting in degraded performance.


The problem is substantial: error rates in third-generation sequencing exceed 10% , yet current tokenizers treat high-confidence and error-prone regions identically. In finance, over 40% of high-frequency data contains microstructure noise , but tokenization methods do not distinguish signal quality. This limitation constrains foundation model training on real-world data.


We present **Quality-Aware Tokenization (QA-Token)** , a framework that incorporates data quality into vocabulary construction. QA-Token introduces three technical contributions:


**1. Bilevel Optimization with Complexity Analysis:** We formalize tokenization as a bilevel optimization problem (Definition ) that jointly optimizes vocabulary construction and downstream performance. We show this problem is NP-hard (Theorem ) and develop a principled approximation scheme with theoretical guarantees.


**2. Reinforcement Learning with Convergence Guarantees:** We cast vocabulary construction as a Markov Decision Process (Definition ) and employ reinforcement learning to discover optimal merge policies. Our approach includes formal convergence analysis (Proposition ) and achieves \\((1-1/e)\\)-approximation to the optimal adaptive policy.


**3. Differentiable Parameter Learning:** Through Gumbel-Softmax relaxation (Theorem ), we enable end-to-end learning of quality sensitivity parameters, with proven consistency and bounded gradients (Proposition ).


We show that QA-Token achieves information-theoretic optimality under noisy conditions (Theorem ), providing formal justification for quality-aware tokenization. Experiments show 30% higher Sharpe ratios in algorithmic trading, 8.9% absolute improvement in genomic variant calling F1 score, and state-of-the-art performance when integrated into 7B-parameter foundation models.


**Core Contributions:** (i) We derive a quality-aware merge score (Theorem ) balancing frequency, quality, and domain constraints with learnable sensitivity \\(\\alpha\\) (Appendix ). (ii) We formulate vocabulary construction as an MDP (Definition , Appendix ) achieving \\((1-1/e)\\)-approximation through adaptive submodularity. (iii) Gumbel-Softmax relaxation enables end-to-end parameter learning with \\(O(1/\\sqrt{T})\\) convergence rate (Proposition , Appendix ). (iv) Domain-specific instantiations achieve state-of-the-art performance across 15+ benchmarks.


Our analysis shows that incorporating quality signals into tokenization enables training on noisy corpora where frequency-based methods fail, expanding the range of usable training data for foundation models.


### Key Results


The model solves how to learn effectively from imperfect data. It explicitly handles noise and provides dramatic improvements in data processing across all applications:


- **8.9% boost** in identifying genetic variants
- **94.53% accuracy** pathogen detection on a massive dataset of 1.7 trillion genetic building blocks
- **15% more efficient** processing with less computational power
