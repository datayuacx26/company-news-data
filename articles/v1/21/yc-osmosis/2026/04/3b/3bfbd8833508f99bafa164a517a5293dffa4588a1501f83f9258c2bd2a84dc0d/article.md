---
schema_version: "1.0.0"
document_id: "3bfbd8833508f99bafa164a517a5293dffa4588a1501f83f9258c2bd2a84dc0d"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/testing-data-overlap-between-sft-and-grpo-on-autoformalization"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:94d0114fc7497603a1ae209237bf3f19370dab376c66c1c71e54b3805f48026b"
---

# Testing Data Overlap Between SFT and GRPO on Autoformalization

**TL;DR: We investigated the impact of data overlap between SFT and GRPO by training models on Lean 4 autoformalization. We discovered that keeping training data disjoint between the two stages is critical. At 0% data overlap, SFT+GRPO boosts semantic accuracy by +10.4pp over SFT alone, while at 100% data overlap it adds no performance gain.[Read the full paper on arXiv](https://arxiv.org/abs/2604.13515) .**


### Overview


SFT followed by GRPO is the standard post-training recipe for reasoning models. You collect labeled data, fine-tune, then run reinforcement learning with a reward signal. But there's a design decision that's unexplored: how much of your GRPO data should overlap with your SFT data?


Most published pipelines either draw GRPO prompts from the same pool as SFT, or use identical data across both stages. To investigate, we ran a controlled ablation study on Qwen3-8B post-trained for Lean 4 autoformalization (i.e. the process of translating math statements into Lean 4 theorem declarations), isolating data overlap as the sole variable, which we ablated at 0%, 30%, and 100% data overlap.


### Autoformalization & Reward Design


Autoformalization is the process of translating math statements into Lean 4 theorem declarations that typecheck against Mathlib.


Given a problem like:


> *Determine the value of the infinite series ∑ k²/2ᵏ. Show that the answer is 6.*


The model should produce:


```text
theorem lean_workbook :
∑' k : ℕ, (k ^ 2 : ℝ) / 2 ^ k = 6 := by sorry
```


But just because a formalized statement is valid (i.e. passes compilation check) does not mean it's correct. More specifically, models can produce Lean 4 that typechecks but is mathematically wrong. For example, one model formalized a Putnam problem about great circles dividing a sphere as:


```text
theorem putnam_1964_b4 : ∀ n : ℕ, 0 < n →
(n ^ 2 - n + 2 : ℕ) = n ^ 2 - n + 2 := by sorry
```


It compiles, but the statement is a tautology with zero mathematical content. This is why we measure both **compile pass@k** (does it typecheck?) and **semantic pass@k** (does it typecheck *and* faithfully capture the original math, as judged by Gemini Flash 3 with a ≥ 0.7 threshold).


### Experiment


We trained five model variants on Qwen3-8B (thinking disabled), all sharing the same training dataset and hyperparameters, and evaluated them alongside the base model. Data overlap is the only variable:


- **Base** : Qwen3-8B with no post-training
- **SFT** : Supervised fine-tuning on 20K (NL, Lean 4) pairs
- **GRPO** : GRPO applied directly to base, no SFT
- **SFT+GRPO-0%** : SFT then GRPO with fully disjoint data
- **SFT+GRPO-30%** : SFT then GRPO with 30% data overlap
- **SFT+GRPO-100%** : SFT then GRPO with identical data


We evaluated on Gaokao-Formal (495 competition math problems) and PutnamBench (672 Putnam problems), using n=8 rollouts per problem.


### Results


Through our training experiments, we observed that performance improves as overlap decreases.


Model Gaokao C@1 Gaokao S@1 Gap Δ Putnam C@1 Putnam S@1 Gap Δ


Base 19.9% 10.2% 9.7% 11.3% 3.3% 8.0%


SFT 61.8% 41.0% 20.8% 28.5% 14.3% 14.2%


GRPO-only 50.9% 28.1% 22.8% 36.1% 11.9% 24.2%


**SFT+GRPO-0%** **77.6%** **51.4%** 26.2% **47.9%** **23.6%** 24.3%


SFT+GRPO-30% 76.4% 48.6% 27.8% 46.4% 22.9% 23.5%


SFT+GRPO-100% 62.9% 40.6% 22.3% 29.1% 14.7% 14.4%


Lower overlap improved both reward metrics and both benchmarks. At 0% overlap, GRPO adds +10.4 pp semantic accuracy over SFT alone on Gaokao and +9.3 pp on Putnam. At 100% overlap, both C@1 and S@1 are flat relative to SFT alone, and the entire GRPO stage is effectively redundant.


### Observations


SFT teaches the model to formalize specific problems through imitation. When GRPO subsequently trains on those same problems, the reward signal is already near-saturated. Both the compiler gate and the semantic judge give high scores on familiar data, leaving no room for policy improvement. The training dynamics confirm this: higher-overlap variants exhibit lower policy entropy throughout training, constraining the model toward memorized solutions rather than exploring novel outputs.


The 30% overlap case is telling. Training reward was actually the *highest* for the 30% overlap variant, but it didn't translate to better eval performance. Our hypothesis is that partial familiarity inflates reward without forcing genuine exploration.


### Conclusion


When SFT and GRPO share data fully, the RL stage has little left to learn. Keeping the two stages on disjoint data lets GRPO push the model beyond what imitation alone can teach. When designing a post-training pipeline, partitioning your data is a straightforward way to get more out of RL at no additional compute cost.


For full details, see[our paper on arXiv](https://arxiv.org/abs/2604.13515) .


---


### Experiment Details


*The sections below cover data curation, reward design, and training configuration.*


#### Data Experiment


We drew from four publicly available corpora, each vetted by compiling 500 samples against Mathlib v4.27.0:


Dataset Pool Size Pass Rate


Leanabell-Prover 1.13M 93.0%


Lean Workbook 13.5K 92.0%


NuminaMath 104K 90.6%


HERALD 580K 86.6%


The curation pipeline: ingestion, compilation, SHA-256 deduplication (Leanabell contains 15 to 20% duplicates from NuminaMath and Lean Workbook), quality filtering, topic stratification, answer injection, and formatting. Final output: 20K SFT pairs (40% NuminaMath, 35% Leanabell, 15% HERALD, 10% Lean Workbook) and 16K GRPO prompts from the same sources.


#### Answer Injection


Many competition problems require the model to "find" or "determine" a value that only appears in the ground truth. During GRPO, the model generates from prompts alone. Without the answer, it can't formalize a correct statement because it doesn't know what the answer is. Our pipeline extracts answers from ground-truth Lean 4 using 12 pattern types and appends them to prompts:


- **Without** : *"Determine ∑ k²/2ᵏ."* → Model must compute 6 *and* formalize.
- **With** : *"Determine ∑ k²/2ᵏ. Show that the answer is 6."* → Model just formalizes.


This bridges SFT datasets (designed for supervised training) to RL training.


#### Reward Design


The reward couples a hard compiler gate with a continuous semantic judge. Non-compiling outputs get r=0. Compiling outputs are scored by Gemini Flash 3 on a 0.0 to 1.0 scale for semantic faithfulness against the ground truth. The model never sees the ground-truth output; it only receives graded feedback on its own generations.


#### Training Setup


Stage Framework Configuration


SFT Axolotl Full-parameter fine-tuning, cosine LR (peak 2×10⁻⁵), 2 epochs


GRPO Slime / Megatron-LM 8 rollouts/prompt, constant LR 1×10⁻⁶, 4 actor GPUs (TP=1) + 4 rollout GPUs (SGLang)


Evaluation vLLM on H200s Gaokao-Formal (495 problems) + PutnamBench (672 problems), n=8 rollouts
