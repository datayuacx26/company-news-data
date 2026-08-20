---
schema_version: "1.0.0"
document_id: "46c6a58f8784d668011abd0ab90e4fa82de9a4dbe007affd3fef89a43fff1263"
company_key: "yc-cactus"
company: "Cactus"
source_id: "yc-cactus-news-import-6d2738280b9a"
canonical_url: "https://cactuscompute.com/blog/turboquant-h"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-24T00:01:40.427517+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:c6ce86cce3a0c310d84857f0347f673018686c6f542c94be0506ae2c627d92aa"
---

# TurboQuant-H: Hadamard Rotation for 2-Bit Embedding Quantization

## Abstract


TurboQuant (Zandieh et al., ICLR 2026) compresses KV cache vectors to 1-3 bits via random orthogonal rotation, optimal scalar quantization, and QJL bias correction. We introduce **TurboQuant-H** , a simplified offline variant that replaces random rotation with Hadamard rotation, uses per-group Lloyd-Max codebooks, and drops the QJL correction stage. We apply TurboQuant-H to per-layer input (PLI) embedding tables in Gemma 4 E2B, where embeddings constitute 60.6% of total model weight. On Gemma 4 E2B, TurboQuant-H compresses PLI weights from 2,496 MB to 624 MB (4x) at 2.125 effective bits per dimension, reducing total LLM storage by 40% (4,790 MB → 2,918 MB) with a perplexity increase of 0.06 (1.85 → 1.91) and no measured speed regression.


## 1. Introduction


TurboQuant (ICLR 2026) compresses KV cache vectors to 1-3 bits with near-zero quality loss. The technique is elegant: rotate vectors with a random orthogonal matrix, exploit the resulting Beta distribution to apply optimal scalar quantizers per coordinate, then correct inner product bias with a 1-bit QJL residual. The paper demonstrates quality neutrality at 3.5 bits and marginal degradation at 2.5 bits on Llama-3.1-8B and Ministral-7B.


But TurboQuant was designed for KV cache, vectors generated at runtime during inference. There's a catch: mobile devices and wearables need small models, which we found to significantly degrade when KV cache goes below INT4. We in fact keep KV cache at INT8 on Cactus to ensure correctness. This makes applying TurboQuant to Cactus KV workloads tricky.


However, with the emergence of per-layer embedding architectures (each layer has its own embedding lookup), these embeddings dominate the parameter count of models like the Gemma E-series. For instance, Gemma E2B has 2.3B effective parameters but 5.1B total, because the per-layer embeddings alone account for the difference. That bloats memory and storage footprint by more than 2x. There is a need to re-visit embedding quantisation.


## 2. Background: Per-Layer Embeddings Dominate Model Storage


Most quantization research focuses on linear layer weights and activations. Embeddings are treated as untouchable lookup tables, typically kept at FP16 or at best INT8 while everything else goes to INT4. This made sense when embeddings were a small fraction of total parameters. That assumption broke with per-layer embedding architectures.


Gemma 4 E2B uses AltUp, a technique where each of the 35 transformer layers gets its own embedding projection from the 262K-token vocabulary. Instead of one shared embedding table, you have a shared table plus a per-layer table. The numbers on Cactus's current INT4 weights:


Component Size % of Model


` token_embeddings` (shared) 408 MB 8.7%


` embed_tokens_per_layer` (35 layers) 2,496 MB 52.1%


**Total embedding storage** **2,904 MB** **60.6%**


All other weights (attention, FFN, norms, encoders) 1,886 MB 39.4%


**Total model** **4,790 MB** 100%


The per-layer embedding table is 2.5 GB. More than half the model. This is not unique to Gemma 4. The AltUp design pattern, where per-layer vocabulary projections replace a single shared embedding, is becoming standard for models that need large vocabularies (262K tokens for multilingual coverage) without proportionally large hidden dimensions. Gemma 3n uses the same architecture. Any model that follows this pattern will be embedding-dominated.


## 3. TurboQuant-H


TurboQuant-H shares the core insight from TurboQuant; rotation concentrates coordinates into a well-behaved distribution, enabling aggressive scalar quantization, but simplifies the pipeline for offline weight quantization.


### 3.1 Comparison with TurboQuant


TurboQuant (Zandieh et al.) TurboQuant-H (this work)


**Target** KV cache (runtime activations) Embedding weight tables (offline)


**Rotation** Random orthogonal matrix via QR of Gaussian, O(d2)O(d^2)


O


(


d


2


)


Normalized Hadamard matrix, O(Nlog⁡N)O(N \\log N)


O


(


N


lo g


N


)


, symmetric = self-inverse


**Quantizer** Per-coordinate scalar quantizer (precomputed for Beta distribution) Per-position Lloyd-Max codebook (trained on actual weight distribution)


**Codebook** Implicit (quantization levels derived from Beta CDF) Explicit FP16 centroids per position group (0.125 bits overhead at group-128)


**Bias correction** Two-stage: MSE quantizer at b−1b-1


b


−


1


bits + 1-bit QJL residual Single-stage: no QJL correction


**When it runs** Every forward pass during inference Once during weight conversion


**Bit width** 2.5-bit and 3.5-bit 2-bit (+0.125 codebook overhead ≈ 2.125 effective)


### 3.2 Formal Description


Let E∈RV×D\\mathbf{E} \\in \\mathbb{R}^{V \\times D}


E


∈


R


V


×


D


be the PLI embedding matrix with vocabulary size V=262,144V = 262{,}144


V


=


262


,


144


and embedding dimension D=8,190D = 8{,}190


D


=


8


,


190


. We partition each row into P=⌈D/G⌉=64P = \\lceil D/G \\rceil = 64


P


=


⌈


D


/


G


⌉


=


64


positional groups of G=128G = 128


G


=


128


contiguous elements. The pp


p


-th positional group of vocabulary row vv


v


is denoted xv,p∈RG\\mathbf{x}_{v,p} \\in \\mathbb{R}^G


x


v


,


p


​


∈


R


G


.


**Quantization** (offline, during weight conversion):


**Step 1: Hadamard rotation.** For each row vv


v


and position pp


p


, rotate:


x^v,p=HˉG⋅xv,p\\hat{\\mathbf{x}}_{v,p} = \\bar{\\mathbf{H}}_G \\cdot \\mathbf{x}_{v,p}


x


^


v


,


p


​


=


H


ˉ


G


​


⋅


x


v


,


p


​


where HˉG=1GHG\\bar{\\mathbf{H}}_G = \\frac{1}{\\sqrt{G}} \\mathbf{H}_G


H


ˉ


G


​


=


G


​


1


​


H


G


​


is the G×GG \\times G


G


×


G


normalized Hadamard matrix, satisfying HˉGTHˉG=I\\bar{\\mathbf{H}}_G^T \\bar{\\mathbf{H}}_G = \\mathbf{I}


H


ˉ


G


T


​


H


ˉ


G


​


=


I


and HˉG=HˉGT\\bar{\\mathbf{H}}_G = \\bar{\\mathbf{H}}_G^T


H


ˉ


G


​


=


H


ˉ


G


T


​


. The 1G\\frac{1}{\\sqrt{G}}


G


​


1


​


normalization ensures the transform is its own inverse.


**Step 2: Codebook training.** For each positional group p∈{1,…,P}p \\in \\{1, \\ldots, P\\}


p


∈


{


1


,


…


,


P


}


, collect the rotated values from all VV


V


vocabulary rows and train a Lloyd-Max codebook Cp={c1,c2,c3,c4}\\mathcal{C}_p = \\{c_1, c_2, c_3, c_4\\}


C


p


​


=


{


c


1


​


,


c


2


​


,


c


3


​


,


c


4


​


}


(at b=2b = 2


b


=


2


bits, 4 centroids) by minimizing:


Cp∗=arg⁡min⁡C∑v=1V∑i=1Gmin⁡c∈C(x^v,p,i−c)2\\mathcal{C}_p^* = \\arg\\min_{\\mathcal{C}} \\sum_{v=1}^{V} \\sum_{i=1}^{G} \\min_{c \\in \\mathcal{C}} \\left( \\hat{x}_{v,p,i} - c \\right)^2


C


p


∗


​


=


ar g


min


C


​


∑


v


=


1


V


​


∑


i


=


1


G


​


min


c


∈


C


​


(


x


^


v


,


p


,


i


​


−


c


)


2


This trains each codebook on V×G=262,144×128≈33.6MV \\times G = 262{,}144 \\times 128 \\approx 33.6\\text{M}


V


×


G


=


262


,


144


×


128


≈


33.6


M


data points, giving the Lloyd-Max algorithm sufficient statistics for accurate centroid placement. Each positional group gets its own codebook because the weight distributions vary across positions in the embedding dimension. We tried a single joint codebook shared across all positions; per-position was consistently better.


The codebook values are stored at FP16 (16 bits per centroid), contributing:


2b⋅16G=4⋅16128=0.125 bits/element overhead\\frac{2^b \\cdot 16}{G} = \\frac{4 \\cdot 16}{128} = 0.125 \\text{ bits/element overhead}


G


2


b


⋅


16


​


=


128


4


⋅


16


​


=


0.125


bits/element overhead


**Step 3: Quantize by proximity.** Each rotated element maps to its nearest centroid:


qv,p,i=arg⁡min⁡j∈{1,…,2b}∣x^v,p,i−cj∣q_{v,p,i} = \\arg\\min_{j \\in \\{1,\\ldots,2^b\\}} \\left| \\hat{x}_{v,p,i} - c_j \\right|


q


v


,


p


,


i


​


=


ar g


min


j


∈


{


1


,


…


,


2


b


}


​


∣


x


^


v


,


p


,


i


​


−


c


j


​


∣


Store the 2-bit indices qv,p,iq_{v,p,i}


q


v


,


p


,


i


​


and the P=64P = 64


P


=


64


FP16 codebooks {C1,…,CP}\\{\\mathcal{C}_1, \\ldots, \\mathcal{C}_P\\}


{


C


1


​


,


…


,


C


P


​


}


.


**Dequantization** (at inference):


x~v,p=HˉG⋅scatter(Cp,qv,p)\\tilde{\\mathbf{x}}_{v,p} = \\bar{\\mathbf{H}}_G \\cdot \\text{scatter}(\\mathcal{C}_p, \\mathbf{q}_{v,p})


x


~


v


,


p


​


=


H


ˉ


G


​


⋅


scatter


(


C


p


​


,


q


v


,


p


​


)


where scatter(Cp,qv,p)i=cqv,p,i\\text{scatter}(\\mathcal{C}_p, \\mathbf{q}_{v,p})_i = c_{q_{v,p,i}}


scatter


(


C


p


​


,


q


v


,


p


​


)


i


​


=


c


q


v


,


p


,


i


​


​


maps indices back to centroids. Since HˉG\\bar{\\mathbf{H}}_G


H


ˉ


G


​


is symmetric and orthogonal, the inverse rotation is the same forward transform. No transpose is needed.


**Effective bit rate:**


beff=b+2b⋅16G=2+0.125=2.125 bits/elementb_{\\text{eff}} = b + \\frac{2^b \\cdot 16}{G} = 2 + 0.125 = 2.125 \\text{ bits/element}


b


eff


​


=


b


+


G


2


b


⋅


16


​


=


2


+


0.125


=


2.125


bits/element


### 3.3 The Quantization Pipeline


```text
QUANTIZATION (offline, during cactus convert)
==============================================


PLI Matrix E  (262K x 8190)
|
v
+-----------------------+
| Partition into        |  Each row -> 64 positional groups
| groups of G=128       |  of 128 contiguous elements
+-----------+-----------+
|
v
+-----------------------+
| Hadamard rotation     |  x_hat = (1/sqrt(G)) * H_128 * x
| per group             |  O(G log G) butterfly
+-----------+-----------+
|
v
+-----------------------+
| Lloyd-Max codebook    |  Train 4 centroids (2-bit) per position
| per position          |  across all 262K vocab rows
|                       |  C_p = {c1, c2, c3, c4} in FP16
+-----------+-----------+
|
v
+-----------------------+
| Quantize by           |  q = argmin_j |x_hat_i - c_j|
| proximity             |  Store 2-bit indices per element
+-----------+-----------+
|
v
Output: 2-bit index tensor + 64 FP16 codebooks
Effective: 2.125 bits/element


DEQUANTIZATION (at inference, per token)
=========================================


Token IDs
|
v
+-----------------------+
| Gather 2-bit indices  |  Look up row from compressed table
| + codebook per pos.   |  ~3.8x less bandwidth than INT8
+-----------+-----------+
|
v
+-----------------------+
| Scatter codebook      |  Replace 2-bit indices with FP16
| values                |  centroid values from C_p
+-----------+-----------+
|
v
+-----------------------+
| Hadamard rotation     |  x_tilde = (1/sqrt(G)) * H_128 * scatter(...)
| (same as forward,     |  H_bar is symmetric: H_bar = H_bar^T = H_bar^-1
|  no transpose needed) |  O(G log G) butterfly per group
+-----------+-----------+
|
v
FP16 embedding -> feed to transformer layer
```


### 3.4 Design Decisions


**Why Hadamard instead of random orthogonal?** The normalized Hadamard matrix HˉG=1GHG\\bar{\\mathbf{H}}_G = \\frac{1}{\\sqrt{G}} \\mathbf{H}_G


H


ˉ


G


​


=


G


​


1


​


H


G


​


is deterministic, O(Nlog⁡N)O(N \\log N)


O


(


N


lo g


N


)


to apply via the butterfly factorization (same structure as the FFT), and its own inverse ( Hˉ=HˉT=Hˉ−1\\bar{\\mathbf{H}} = \\bar{\\mathbf{H}}^T = \\bar{\\mathbf{H}}^{-1}


H


ˉ


=


H


ˉ


T


=


H


ˉ


−


1


). For offline weight quantization we don't need the data-oblivious guarantees of a random rotation, we have full access to the weight data at conversion time. The Hadamard rotation still concentrates coordinates, which is all we need to make low-bit scalar quantization work. Note: the unnormalized Hadamard satisfies HHT=G⋅I\\mathbf{H}\\mathbf{H}^T = G \\cdot \\mathbf{I}


H


H


T


=


G


⋅


I


, so the 1G\\frac{1}{\\sqrt{G}}


G


​


1


​


factor is essential for the self-inverse property.


**Why no QJL correction?** TurboQuant's second stage exists because MSE-optimal quantizers introduce multiplicative bias in inner product estimation. At 1-bit, E\[⟨y,Q(x)⟩\]=2π⟨y,x⟩\\mathbb{E}\[\\langle \\mathbf{y}, Q(\\mathbf{x}) \\rangle\] = \\frac{2}{\\pi} \\langle \\mathbf{y}, \\mathbf{x} \\rangle


E


\[⟨


y


,


Q


(


x


)⟩\]


=


π


2


​


⟨


y


,


x


⟩


, a 36% shrinkage. The QJL residual corrects this at the cost of 1 additional bit per dimension. But we're quantizing at 2 bits with a trained codebook, not a precomputed scalar quantizer. The per-position Lloyd-Max codebook already minimizes distortion over the actual weight distribution, and the Hadamard rotation ensures the codebook sees well-spread inputs. At 2 bits with group-128, the inner product bias is small enough that the downstream perplexity impact is negligible (PPL 1.91 vs 1.85). Adding QJL would cost an extra bit per dimension for a correction that isn't needed at this operating point.


**Why per-position codebooks instead of per-coordinate?** TurboQuant can use a single precomputed quantizer because random rotation makes all coordinates identically distributed (each coordinate of a uniform unit-sphere vector follows a known distribution that converges to N(0,1/d)\\mathcal{N}(0, 1/d)


N


(


0


,


1/


d


)


in high dimensions). Hadamard rotation concentrates coordinates but doesn't make them identically distributed — there are structured patterns from the butterfly network. Per-position codebooks (one codebook per group of 128 dimensions, trained across all 262K vocabulary rows at that position) adapt to these patterns. We tried a single joint codebook shared across all positions; per-position was consistently better.


**Why group size 128?** We swept group sizes from 32 to 512. The qualitative tradeoffs:


Group size Codebook overhead (bits/elem) Hadamard cost Quality


32 0.500 fastest degraded (high overhead eats bit budget)


64 0.250 fast good


**128** **0.125** **fast** **best (sweet spot)**


256 0.063 moderate slightly worse (less uniform within group)


512 0.031 slow worse (distribution spreads too thin)


Group-128 gives 0.125 bits overhead, a fast butterfly, and the best quality. Detailed per-group-size PPL numbers are a target for future work.


## 4. Results


### 4.1 Perplexity


Evaluated on Gemma 4 E2B. Evaluation set: 128 self-generated WildChat-1M completions from our` trajectories.jsonl` calibration set, completion-only NLL, 24,438 scored tokens.


Variant Avg bits PPL


HuggingFace BF16 16 1.2892


Cactus default (INT4 linears + INT8 PLI + INT8 token-emb) ~6.3 1.8547


**Cactus + TurboQuant-H PLI** **~3.8** **1.9111**


Perplexity moves from 1.85 to 1.91. A delta of 0.06 PPL on a 24K-token eval set, within noise for practical use. No measured speed regression.


### 4.2 Disk Footprint


Variant Size (MB) Factor


HuggingFace FP16 snapshot ~10,240 1.00×


Cactus default (INT4 linears + INT8 PLI + INT8 emb) ~4,790 0.47×


**Cactus + TurboQuant-H PLI** **2,918** **0.29×**


The PLI table specifically: **2,496 MB → 624 MB, a 4× reduction.**


Total LLM weight reduction: **40%** from the Cactus baseline. Including the vision and audio encoders (untouched by this change), the overall model reduction is **30%** .


For Gemma 4 E2B, that's the difference between a 4.8 GB model and a 2.9 GB model. On a 4 GB RAM Android device, that's the difference between fitting and not fitting.


For Gemma-270m, where the embedding table (295M params) is larger than all other weights combined, we expect the same technique to cut total model size roughly in half. Validation on Gemma-270m is planned future work.


## 5. Inference Path


### 5.1 Before (Cactus default)


```text
token_ids → gather from INT8 table → dequantize to FP16 → feed to transformer
```


### 5.2 After (Cactus TurboQuant-H)


```text
token_ids → gather 2-bit indices → scatter codebook → Hadamard rotation → FP16 → transformer
```


### 5.3 Overhead Analysis


The Hadamard butterfly on a group of G=128G = 128


G


=


128


elements has log⁡2(G)=7\\log_2(G) = 7


lo g


2


​


(


G


)


=


7


stages. Each stage performs G/2=64G/2 = 64


G


/2


=


64


add/subtract pairs (the butterfly operations). Over 7 stages that is 7×64=4487 \\times 64 = 448


7


×


64


=


448


add/sub operations per group. On ARM NEON (128-bit SIMD, 8 FP16 lanes), each stage processes the group in G/(2×8)=8G / (2 \\times 8) = 8


G


/


(


2


×


8


)


=


8


vector instructions, giving 7×8=567 \\times 8 = 56


7


×


8


=


56


vector operations per group.


For a single PLI embedding row of D=8,190D = 8{,}190


D


=


8


,


190


elements:


Groups per row=⌈8190/128⌉=64\\text{Groups per row} = \\lceil 8190 / 128 \\rceil = 64


Groups per row


=


⌈


8190/128


⌉


=


64


Total vector ops=64×56=3,584\\text{Total vector ops} = 64 \\times 56 = 3{,}584


Total vector ops


=


64


×


56


=


3


,


584


At one vector add/sub per cycle on a 2 GHz A15 NEON unit, this completes in under 2 microseconds per embedding lookup. The gather from a table that is 8/2.125≈3.8×8 / 2.125 \\approx 3.8\\times


8/2.125


≈


3.8


×


smaller in memory more than compensates for the rotation cost in bandwidth savings.


## 6. Related Work


**TurboQuant** (Zandieh et al., ICLR 2026) introduced data-oblivious vector quantization with random rotation and QJL correction for KV cache compression, achieving near-optimal distortion rates within a constant factor of ≈2.7\\approx 2.7


≈


2.7


from information-theoretic lower bounds.


**QuIP#** (Tseng et al., 2024) uses the randomized Hadamard transform for weight quantization of linear layers, but does not address embedding tables or per-layer embeddings.


**GPTQ, AWQ** focus on linear layer weight quantization with calibration data. These methods do not handle embedding tables, which are pure lookup operations with no gradient flow during inference.


To our knowledge, TurboQuant-H is the first application of rotation-based vector quantization specifically to per-layer embedding tables, which is where the technique yields the largest storage benefit due to the embedding-dominated weight distribution in AltUp architectures.


## 7. Next Steps


This is a research preview of ongoing work at Cactus. Here's what's coming:


1. **Downstream task validation.** The 0.06 PPL increase is promising, but we're running full evals (MMLU, IFEval, GPQA) across Gemma-270m, Gemma 3n, and Gemma 4 E4B to confirm TurboQuant-H holds up on real tasks, not just perplexity.
2. **Extend to shared` token_embeddings` .** TurboQuant-H currently targets PLI tables only. Applying it to the shared embedding table (408 MB) is straightforward and would push total compression further.
3. **Quantitative group size sweep.** Section 3.4 reports qualitative tradeoffs. We're collecting per-group-size PPL numbers to give a rigorous recommendation.
4. **1-bit with QJL correction.** At 1-bit, the theoretical compression reaches 8x on PLI tables. We're evaluating whether reintroducing the QJL residual stage at this extreme bit width recovers enough quality to be practical.
5. **Ship TurboQuant-H weights.** Integrate the quantization path into` cactus convert` and publish pre-quantized weights on HuggingFace for all supported Gemma E-series models.


## Try It


Run Gemma 4 today on Cactus:


```text
brew   install   cactus-compute/cactus/cactus
cactus   run   google/gemma-4-E2B-it
```


TurboQuant-H PLI weights will ship in an upcoming release. If you're working on embedding quantization or have thoughts on extending this to the shared token embedding table, open an issue on[GitHub](https://github.com/cactus-compute/cactus) .


## Citation


If you use TurboQuant-H in your research, please cite:


```text
@article  {  turboquant-h  ,
title       =   {  TurboQuant-H: Hadamard Rotation for 2-Bit Embedding
Quantization in Embedding-Dominated Models  }  ,
author      =   {  Mosoyan, Karen and Ndubuaku, Henry  }  ,
year        =   {  2026  }  ,
url         =   {  https://docs.cactuscompute.com/latest/blog/turboquant-h/  }  ,
note        =   {  Cactus Compute Research Preview  }
}
```


## References


- Zandieh, Daliri, Hadian, Mirrokni.[TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate](https://arxiv.org/abs/2504.19874) . ICLR 2026.
- Tseng et al. QuIP#: Even Better LLM Quantization with Hadamard Incoherence and Lattice Codebooks. 2024.
- Google Gemma Team. Gemma 3n / Gemma 4 Technical Reports. 2026.
