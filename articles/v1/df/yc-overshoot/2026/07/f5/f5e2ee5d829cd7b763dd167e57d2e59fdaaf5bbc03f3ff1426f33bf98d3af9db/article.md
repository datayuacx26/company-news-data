---
schema_version: "1.0.0"
document_id: "f5e2ee5d829cd7b763dd167e57d2e59fdaaf5bbc03f3ff1426f33bf98d3af9db"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/from-attention-to-diversity"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-30T07:35:23.073828+00:00"
fetched_at: "2026-07-30T07:35:24.294045+00:00"
content_hash: "sha256:7dbaa7e18ed9de943a00d07a4abbfedd4ddabf8ee904203b4ddbc73515f5d364"
---

# From Attention to Diversity: How Visual Token Pruning Methods Decide What to Keep

Visual token pruning examines how the visual representation passed to a vision-language model can be compressed during inference to reduce computation, memory usage, and latency.


The central challenge is deciding which visual tokens can be removed without discarding information the model needs. Existing methods generally approach this through importance-based or diversity-based selection, using either prompt-aware signals that adapt to the user’s question or prompt-agnostic signals derived from the image alone.


This survey looks at nine representative methods and how each decides what visual information to keep.


## 1. FastV: An Image is Worth 1/2 Tokens After Layer 2 (2024)


By Liang Chen, Haozhe Zhao, Tianyu Liu, Shuai Bai, Junyang Lin, Chang Zhou, Baobao Chang ·[arXiv ↗](https://arxiv.org/abs/2403.06764)


Prompt-aware · Attention-based importance


FastV prunes visual tokens inside the language-model decoder. After allowing the complete visual sequence to pass through the first few decoder layers, it ranks each visual token using the average attention it receives from all other tokens at the selected pruning layer. Tokens with lower scores are removed from the remaining decoder layers, reducing computation without requiring retraining.


**Reported result:** FastV reduced theoretical FLOPs by 45% on LLaVA-1.5-13B without sacrificing aggregate performance across the evaluated image- and video-understanding tasks.


## 2. FasterVLM: \[CLS\] Attention is All You Need for Training-Free Visual Token Pruning: Make VLM Inference Faster (2024)


By Qizhe Zhang, Aosong Cheng, Ming Lu, Zhiyong Zhuo, Minqi Wang, Jiajun Cao, Shaobo Guo, Qi She, Shanghang Zhang ·[arXiv ↗](https://arxiv.org/abs/2412.01818v1)


Prompt-agnostic · Attention-based importance


FasterVLM argues that text-to-visual attention inside the language model is not always a reliable indicator of patch importance. Instead, it ranks image tokens using the attention they receive from the vision encoder’s \[CLS\] token and removes low-ranked tokens before they enter the language model. This makes the method prompt-agnostic, while pruning earlier allows it to eliminate more downstream computation than decoder-side methods such as FastV.


**Reported result:** FasterVLM pruned 95% of the visual tokens while retaining 90% of LLaVA-1.5-7B’s original performance in the paper’s evaluation.


## 3. SparseVLM: Visual Token Sparsification for Efficient VLM Inference (2024)


By Yuan Zhang, Chun-Kai Fan, Junpeng Ma, Wenzhao Zheng, Tao Huang, Kuan Cheng, Denis Gudovskiy, Tomoyuki Okuno, Yohei Nakata, Kurt Keutzer, Shanghang Zhang ·[arXiv ↗](https://arxiv.org/abs/2410.04417)


Prompt-aware · Attention-based importance


SparseVLM identifies the text tokens in the instruction that are most visually relevant and uses them as raters for visual-token importance. Rather than pruning once, it progressively reduces the visual sequence across multiple decoder layers, adapts the pruning ratio by layer, and compresses some discarded information into recycled tokens. This provides a more sophisticated prompt-aware signal than FastV, although its progressive selection and recycling operations add complexity.


**Reported result:** SparseVLM reduced LLaVA’s FLOPs by 54% and CUDA latency by 37% while retaining 97% of the model’s original accuracy.


## 4. DART: Stop Looking for Important Tokens in Multimodal Language Models: Duplication Matters More (2025)


By Zichen Wen, Yifeng Gao, Shaobo Wang, Junyuan Zhang, Qintong Zhang, Weijia Li, Conghui He, Linfeng Zhang ·[arXiv ↗](https://arxiv.org/abs/2502.11494)


Prompt-agnostic · Redundancy and duplication-based


DART challenges the idea that visual tokens should be selected according to individual importance scores. It chooses a small set of pivot tokens and measures how similar the remaining tokens are to those pivots. Tokens whose representations are highly duplicated are removed, while tokens that contribute less-represented information are retained. DART therefore shifts visual-token pruning from identifying the most important patches to eliminating repeated visual evidence.


**Reported result:** DART pruned 88.9% of visual tokens while maintaining comparable performance, producing a 1.99× speedup in total inference time and a 2.99× speedup during prefilling.


## 5. DivPrune: Diversity-based Visual Token Pruning for Large Multimodal Models (2025)


By Saeed Ranjbar Alvar, Gursimran Singh, Mohammad Akbari, Yong Zhang ·[arXiv ↗](https://arxiv.org/abs/2503.02175)


Prompt-agnostic · Diversity-based


DivPrune formulates token selection as a max-min diversity problem. It progressively builds a retained subset by selecting tokens that are maximally dissimilar from those already chosen, allowing a limited token budget to cover a broader range of visual representations. Unlike DART, which identifies duplication relative to a small set of pivots, DivPrune directly seeks diversity across the complete retained subset.


**Reported result:** On LLaVA-NeXT-Video-7B, DivPrune reduced end-to-end latency by approximately 22%, prefill latency by 55%, and GPU memory usage by roughly 400 MB. On ActivityNet, it scored 45.90%, compared with 48.10% for the unpruned model.


## 6. CDPruner: Beyond Attention or Similarity: Maximizing Conditional Diversity for Token Pruning in MLLMs (2025)


By Qizhe Zhang, Mengzhen Liu, Lichen Li, Ming Lu, Yuan Zhang, Junwen Pan, Qi She, Shanghang Zhang ·[arXiv ↗](https://arxiv.org/abs/2506.10967)


Prompt-aware · Prompt-conditioned diversity


CDPruner addresses the weakness of purely diversity-based pruning by conditioning token relationships on the instruction. It uses the prompt to determine which visual differences matter for the current task, then applies a determinantal point process to select a subset intended to be both relevant and non-redundant. This bridges attention-based relevance and diversity-based coverage.


**Reported result:** CDPruner reduced LLaVA’s FLOPs by 95% and CUDA latency by 78% while retaining 94% of the model’s original accuracy.


## 7. HiPrune: Hierarchical Attention for Efficient Token Pruning (2025)


By Jizhihui Liu, Feiyi Du, Guangdao Zhu, Niu Lian, Jun Li, Bin Chen, Weili Guan, Yaowei Wang ·[arXiv ↗](https://arxiv.org/abs/2508.00553)


Prompt-agnostic · Hierarchical attention-based importance


HiPrune selects visual tokens using attention patterns from different layers of the vision encoder. It preserves object-centric anchor tokens from intermediate layers, spatially neighboring buffer tokens that maintain local context, and globally informative register tokens from deeper layers. Rather than relying on one attention score from one layer, HiPrune uses the hierarchy of the vision encoder to preserve both local objects and broader scene information.


**Reported result:** HiPrune preserved up to 99.3% of task performance using one-third of the visual tokens while reducing inference FLOPs by 58.7%.


## 8. HiPrune++: Prompt-Conditioned Hierarchical Token Pruning (2026)


Prompt-aware · Prompt-conditioned hierarchical importance


HiPrune++ extends HiPrune by coupling its hierarchical vision-encoder attention signals with similarity to the text tokens. This allows the selected subset to preserve object-level and global visual information while adapting more directly to the user’s instruction.


**Reported result:** HiPrune++ maintained up to 99.7% of full-token performance while retaining only two-ninths, or approximately 22.2%, of the visual tokens.


## 9. AnchorPrune: Relevance-Anchored Contextual Expansion for Visual Token Pruning (2026)


By Kyuan Oh, Bumsoo Kim ·[arXiv ↗](https://arxiv.org/abs/2607.07033)


Prompt-aware · Relevance-and-diversity hybrid


AnchorPrune first creates a protected set of tokens that are strongly relevant to the instruction. After securing this query-critical evidence, it uses the remaining token budget to select context that is both important and novel relative to the protected set. This ordering prevents diverse but less relevant patches from displacing indispensable evidence while avoiding the redundancy that can result from relevance-only selection.


**Reported result:** On LLaVA-NeXT-7B, AnchorPrune preserved 97.6% of full-token performance while retaining only 160 of the original 2,880 visual tokens, equivalent to approximately 5.6% of the sequence.
