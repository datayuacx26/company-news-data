---
schema_version: "1.0.0"
document_id: "a1c612032f751e6501446c95a2e8ca7901dd457b6e0c5ba69738064b5a5f0a72"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/what-stood-out-at-iclr-2026-criteo-papers-and-research-highlights-3b54dba09447"
published_at: "2026-05-28T06:31:00+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-08-20T03:05:58.983824+00:00"
content_hash: "sha256:485c7fbae7e5b9e97ec7b0018a77cbafce38e358e0c4fa06e43f703faf1dea02"
---

# What Stood Out at ICLR 2026: Criteo Papers and Research Highlights

#### Authors:[Ahmed Ben Yahmed](https://www.linkedin.com/in/ahmed-ben-yahmed-a590ab203/) ,[Antoine Schnepf](https://www.linkedin.com/in/antoine-schnepf-603a3b16a/) ,[Karim Kassab](https://www.kkassab.com/) , and[Mélissa Tamine](https://www.linkedin.com/in/melissa-tamine/) .


The 14th International Conference on Learning Representations ([ICLR 2026](https://iclr.cc/) ) was held from April 23 to 27, 2026, at the Riocentro Convention and Event Center in Rio de Janeiro, Brazil. It was the first time the conference made its way to South America. As one of the three flagship venues in machine learning, ICLR gathers each year several thousand researchers and practitioners from academia and industry around the latest advances in representation learning.


This edition was particularly vibrant, with a strong emphasis on reasoning and agentic LLMs, world models, multimodal foundation models, diffusion-based generation, and modern 3D and 4D scene representations. The setting itself, sitting between the conference center, the beach, and the Pão de Açúcar, added a uniquely warm and collaborative atmosphere to the technical program.


### Our Contributions


#### Contextual Multi-Armed Bandits with Minimum Aggregated Revenue Constraints


***Authors:*** *Ahmed Ben Yahmed, Marc Abeille, Hafedh El Ferchichi, Vianney Perchet.*


Our paper addresses an open question in the literature on the exploration–exploitation dilemma for contextual bandits with minimum revenue guarantees, where the learner must simultaneously maximize cumulative reward and ensure a minimum aggregated revenue for each arm across contexts. Prior work shows that in the non-contextual case, multi-armed bandits with revenue constraints can achieve fast rates for both regret and constraint violation; however, in the contextual case, existing methods rely predominantly on primal-dual approaches that yield only √T guarantees on both metrics. We establish a sharper trade-off between regret and constraint violation through two algorithms, **OLP** and **OPLP** , which target two opposite points on the Pareto frontier, one prioritizing regret, the other prioritizing constraint satisfaction. Our results are essentially tight, matching the lower bounds we derive.


*Ahmed Ben Yahmed*


**Main takeaways**


- **Pareto trade-off:** **OLP** achieves polylogarithmic regret with O(√T) constraint violation, while **OPLP** flips this, polylogarithmic constraint violation with O(√T) regret, giving practitioners two operating points on the performance/constraint-satisfaction frontier.
- **Novel gap notion:** We define the sub-optimality gap **ρ*** via the LP’s active constraint set rather than reward differences, providing a refined complexity measure that generalizes naturally to other globally-constrained bandit problems.
- **Lower bound rules out free exploration:** We establish a matching Ω(√T) lower bound, and prove that whenever K > 2 and |C| > 1, the optimal allocation assigns zero probability to at least one (arm, context) pair. This refutes the “free exploration” property exploited in non-contextual settings (Baudry et al., 2024), restoring the classical exploration-exploitation tradeoff.


[Contextual Multi-Armed Bandits with Minimum Aggregated Revenue...](https://openreview.net/forum?id=qbhPULMkuQ)


#### Fused-Planes: Why Train a Thousand Tri-Planes When You Can Share?


***Authors:*** *Karim Kassab*, Antoine Schnepf*, Jean-Yves Franceschi, Laurent Caraffa, Flavian Vasile, Jeremie Mary, Andrew Comport†, Valérie Gouet-Brunet† (*,† equal contribution).*


Tri-Planar NeRFs have become the go-to representation for large-scale 3D modeling, as their 2D planar structure makes them directly compatible with standard image-based architectures. Yet, the prerequisite step of training a Tri-Plane *per object* is computationally wasteful: each scene is fit in isolation, ignoring the rich structural similarities that exist within an object class. Our paper, *Fused-Planes* , addresses exactly this inefficiency.


We introduce a novel planar representation that decomposes each object into two complementary components: a *micro* plane that captures object-specific details, and a *macro* plane obtained as a learned linear combination over a small set of globally shared *base planes* .


*Antoine Schnepf & Karim Kassab*


**Main takeaways**


- **State-of-the-art efficiency among planar representations:** 7.2× faster training and 3.2× lower per-object memory footprint than Tri-Planes, while improving rendering quality.
- **Ultra-lightweight variant (Fused-Planes-ULW):** 1875× smaller per-object footprint than Tri-Planes, with only minor quality degradation, opening the door to truly large-scale Tri-Plane datasets.
- **Design insight:** the latent space and the micro/macro decomposition are *both* essential. Each in isolation underperforms; together, they provide a framework with similar or better quality than Tri-Planes.


[Fused-Planes](https://fused-planes.github.io/)


#### On the Impact of the Utility in Semivalue-based Data Valuation


***Authors:*** *Mélissa Tamine, Benjamin Heymann, Maxime Vono, Patrick Loiseau*


Our paper studies a practical but often overlooked question in semivalue-based data valuation: *how much do the resulting data rankings depend on the utility function chosen by the practitioner?* In data valuation, each data point is assigned a score reflecting its contribution to a downstream task. These scores are then typically used to rank examples, for instance, to remove harmful samples, select a smaller training subset, or reason about the contribution of different data providers.


The issue is that semivalue-based methods, such as Data Shapley, Beta Shapley, or Data Banzhaf, do not produce values in isolation: they depend on a utility function. If small changes in the utility lead to completely different rankings of the data points, then the valuation is hard to interpret and may not be reliable enough for downstream decisions.


We address this problem by introducing a geometric view of utility dependence. For a fixed semivalue, we embed each data point into a low-dimensional space, which we call the dataset’s **spatial signature** . In this space, choosing a utility corresponds to choosing a direction, and the value of a data point is obtained by projecting its embedding onto that direction. Therefore, changing the utility becomes a geometric rotation, and robustness becomes the question: *how far can we rotate the utility direction before the ranking of data points changes significantly?*


Building on this representation, we define a robustness metric that quantifies how stable the data-value ranking is under changes in the utility. Empirically, we show that this robustness metric agrees with classical rank-correlation analyses: when rankings under different utilities have low Kendall or Spearman correlation, our robustness score is also low. We also observe that Banzhaf often yields more robust rankings than Shapley or Beta Shapley, and we provide geometric and analytical insight into why this happens.


*Mélissa Tamine*


**Main takeaways**


- **Data valuation should not be used blindly.** Even when the valuation method is fixed, changing the utility function can substantially change the ranking of data points.
- **Robustness to utility choice is a necessary diagnostic.** Before using data values for cleaning, subset selection, or partner-level compensation, practitioners should check whether the ranking is stable or mostly an artifact of the chosen metric.
- **The spatial signature gives an interpretable geometric picture.** It turns utility dependence into a projection problem: data points are embedded in a space, utilities are directions, and rankings are induced by projections.
- **The proposed robustness metric makes this diagnostic operational.** It yields a normalized score between 0 and 1, making it easy to identify contexts where semivalue-based data valuation is reliable and those where it is too sensitive to trust.
- **Banzhaf appears more robust in many settings.** Across several datasets and utility families, Banzhaf often yields more stable rankings, which can be explained by the greater collinearity among its spatial signatures.


[On the Impact of the Utility in Semivalue-based Data Valuation](https://medium.com/criteo-engineering/on-the-impact-of-the-utility-in-semivalue-based-data-valuation-fd40b59a813b)


### Papers Highlighted By The Team


#### Can Small Training Runs Reliably Guide Data Curation? Rethinking Proxy-Model Practice?


***Authors:*** *Jiachen T. Wang, Tong Wu, Kaifeng Lyu, James Zou, Dawn Song, Ruoxi Jia, Prateek Mittal*


This paper studies a very practical question in large-scale model training: *can small proxy models reliably guide data curation decisions for larger target models?* In many industrial and frontier AI pipelines, data teams cannot train a full-scale model for every candidate data recipe (i.e., candidate training datasets resulting from different data curation choices). Instead, they train smaller proxy models on different curated datasets, compare their validation performance, and use this comparison to decide which data recipe should be used at scale.


The paper argues that the standard proxy-model protocol has a subtle but important flaw. Most proxy experiments train all candidate datasets with the same fixed hyperparameters, often in the name of fairness. However, in realistic large-scale training, the final model is not trained with a fixed generic configuration: hyperparameters are tuned for the selected dataset. Therefore, the right objective is not to find the dataset that performs best under one arbitrary proxy configuration, but the dataset that performs best after data-specific hyperparameter tuning. The authors show that proxy rankings can be fragile: even small changes in learning rate can flip the relative ranking of data recipes. This means that a data recipe that looks best under one proxy configuration may not be the one with the best potential after tuning at the target scale. To address this, they propose a simple patch: train proxy models with tiny learning rates. Their intuition is that, in this regime, training dynamics are dominated by first-order gradient alignment between the training data and the validation distribution, while higher-order effects that can scramble rankings are reduced.


Empirically, they evaluate 23 data recipes covering domain composition, filtering, deduplication, and major pretraining corpora across the GPT-2, Pythia, and OPT model families, ranging from 70M to 1B parameters. The results are strong: using tiny learning rates significantly improves the agreement between proxy-model rankings and tuned target-model rankings.


**Main takeaways**


- **Proxy-model data curation is not automatically reliable.** Small training runs can give misleading recommendations if the proxy configuration is fixed arbitrarily.
- ***Fair* fixed hyperparameters can be unfair to datasets.** Different data recipes may require different optimal learning rates or training configurations, so comparing them under a single fixed setup can favor the dataset that happens to match it.
- **Learning rate is a major source of ranking instability.** Small changes in learning rate can change which data recipe appears best, even at the same proxy scale.
- **Tiny learning rates provide a simple, practical fix.** Training proxy models with learning rates one or two orders of magnitude below standard choices makes rankings more stable and more transferable to tuned larger models.
- **This is highly relevant for data-centric AI at scale.** The paper gives a concrete warning: when using small models to guide expensive data decisions, the evaluation protocol matters as much as the data recipe itself.


[Can Small Training Runs Reliably Guide Data Curation? Rethinking Proxy-Model Practice](https://arxiv.org/abs/2512.24503)


#### Convergence of an Actor-Critic Gradient Flow for Entropy-Regularised MDPs in General Spaces


***Authors:*** *Denis Zorba, David Šiška, Lukasz Szpruch.*


This paper proves the stability and global convergence of a two-timescale actor-critic gradient flow for infinite-horizon, entropy-regularised Markov Decision Processes (MDPs) with **continuous (Polish) state and action spaces** under linear function approximation. The critic is updated via Temporal Difference (TD) learning on a faster timescale, while the actor is updated via Policy Mirror Descent, which in the continuous-time limit becomes an approximate Fisher–Rao gradient flow. The key technical challenge is that in general action spaces the KL divergence is unbounded (unlike the finite-action case where it is bounded by log|A|), making finite-time blow-up a real risk. The authors develop a Lyapunov-based framework combining convex analysis over both Euclidean and measure spaces to rule this out and establish exponential convergence to the optimal regularised value function.


**Main takeaways**


- **Timescale separation is essential:** The critic must run on a faster timescale than the actor for both stability and convergence; the authors show ηₜ = η₀·exp(k₁t) suffices.
- **Entropy regularisation is necessary, not optional:** In general action spaces, the KL term in standard mirror-descent bounds is typically infinite without regularisation, since optimal unregularised policies are mixtures of Diracs. Regularisation forces the optimal policy to have full support, making the analysis tractable.
- **Convergence rate:** The actor-critic flow achieves exponential convergence to the optimal regularised policy for all discount factors γ ∈ (0,1), up to an error term that decays exponentially when the critic runs sufficiently fast.
- **Stability result:** Under mild assumptions, KL(πₜ(·|s)|µ)² grows at most as a₁·exp(a₂t), preventing finite-time blow-up of either the policy or the critic parameters.


[Convergence of actor-critic for entropy regularised MDPs in general action spaces](https://arxiv.org/abs/2510.14898)


#### Best-of-N through the Smoothing Lens: KL Divergence and Regret Analysis


***Authors:*** *Gholamali Aminian, Idan Shenfeld, Amir R. Asadi, Ahmad Beirami, Youssef Mroueh*


This paper provides a theoretical analysis of **Soft Best-of-N (SBoN)** , a smoothed variant of Best-of-N (BoN) sampling for LLM alignment. Instead of deterministically picking the highest proxy-reward response from N samples, SBoN samples probabilistically via a softmax with inverse temperature β (recovering BoN as β → ∞, and the reference policy as β → 0). The authors derive finite-sample upper bounds on KL divergence between SBoN and the reference policy, and both upper and lower bounds on the regret gap versus the optimal policy. A novel *tilted error* metric ε_β,r is introduced to track reward misspecification, interpolating between MSE and L∞ error.


**Main takeaways**


- **Smoothing mitigates reward hacking:** When the proxy-reward model is weak, BoN overoptimizes as N grows, while SBoN’s stochasticity guards against this failure.
- **Optimal finite β exists:** Under overoptimization, there is a sweet spot β* where SBoN provably achieves a tighter regret bound than BoN.
- **Improved theory:** Unlike Huang et al. (2025), the BoN regret bound here stays finite when overoptimization vanishes, and the framework uses calibrated rewards.
- **Empirical validation:** Experiments with Olmo-2 1B on the Attaq harmfulness dataset confirm that SBoN outperforms BoN when paired with weak proxy reward models (Beaver 7B, Tulu-v2.5), while both perform similarly with a strong reward model (ArmoRM 8B).
- **Practical edge:** SBoN matches the performance of more complex methods like InferenceTimePessimism while requiring only a single hyperparameter.


[Best-of-N through the Smoothing Lens: KL Divergence and Regret Analysis](https://arxiv.org/abs/2507.05913)


#### Depth Anything 3: Recovering the Visual Space from Any Views


***Authors:*** *Haotong Lin*, Sili Chen*, Jun Hao Liew*, Donny Y. Chen*, Zhenyu Li, Yang Zhao, Sida Peng, Hengkai Guo, Xiaowei Zhou, Guang Shi, Jiashi Feng, Bingyi Kang*†.*


Depth Anything 3 (DA3) pursues a *minimal modeling* strategy for 3D visual perception. Stepping back from task-specific designs (monocular depth, SfM, MVS, SLAM), the authors ask two central questions: is a single set of prediction targets enough, and is a plain transformer enough?


The paper answers yes to both. DA3 predicts geometry from any number of input views, with or without known camera poses, using a plain pretrained DINO backbone (no architectural specialization) and a single depth-ray prediction target that sidesteps the orthogonality constraints of rotation matrices by representing camera pose implicitly through a per-pixel ray map. Multi-view consistency is obtained through an input-adaptive cross-view self-attention mechanism, and diverse training data is unified via a teacher-student paradigm in which a synthetic-data-trained monocular teacher generates high-quality pseudo-depth labels for real-world captures.


On the paper’s newly proposed visual geometry benchmark, DA3 sets a new state of the art, improving over VGGT in camera pose accuracy and in geometric accuracy, while also surpassing Depth Anything 2 on monocular depth. The paper further introduces a feed-forward novel view synthesis (FF-NVS) benchmark and reports two findings: (i) fine-tuning DA3 with an additional DPT head for FF-NVS substantially outperforms highly specialized task-specific models, and (ii) enhanced geometric reconstruction capability directly correlates with improved FF-NVS performance, establishing DA3 as a strong backbone for this task.


[Depth Anything 3: Recovering the Visual Space from Any Views](https://arxiv.org/abs/2511.10647)


#### Sharp Monocular View Synthesis in Less Than a Second


***Authors:*** *Lars Mescheder, Wei Dong, Shiwei Li, Xuyang Bai, Marcel Santos, Peiyun Hu, Bruno Lecouat, Mingmin Zhen, Amaël Delaunoy, Tian Fang, Yanghai Tsin, Stephan R. Richter, Vladlen Koltun.*


SHARP (Single-image High-Accuracy Real-time Parallax) addresses photorealistic view synthesis from a single image. The paper targets three problems: sub-second synthesis on a standard GPU, real-time photorealistic rendering from nearby views, and a metric 3D representation with absolute scale that can be coupled to a physical headset. Given an input image, SHARP regresses a high-resolution 3D Gaussian representation (around 1.2M Gaussians, parameterized over a 768×768 grid with two layers) via a single feed-forward pass.


The architecture consists of four learnable modules: a pretrained Depth Pro encoder, a depth decoder that outputs two distinct depth layers, a learned depth adjustment module, and a Gaussian decoder, and is trained end-to-end for view synthesis fidelity. Two additional contributions are emphasized: a carefully designed loss configuration that prioritizes view synthesis quality while suppressing common artifacts, and the depth adjustment module, which resolves depth ambiguities during training and is identified by the authors as a fundamental challenge for regression-based view synthesis.


Across multiple held-out datasets in the zero-shot regime, SHARP reduces LPIPS and DISTS relative to the strongest prior method, synthesizes three orders of magnitude faster than that baseline (and two to three orders of magnitude faster than diffusion-based systems more broadly), and supports rendering at more than 100 FPS on a standard GPU.


[SHARP](https://apple.github.io/ml-sharp/)


#### Lyra: Generative 3D Scene Reconstruction via Video Diffusion Model Self‑Distillation


***Authors:*** *Sherwin Bahmani, Tianchang Shen, Jiawei Ren, Jiahui Huang, Yifeng Jiang, Haithem Turki, Andrea Tagliasacchi, David Lindell, Zan Gojcic, Sanja Fidler, Huan Ling, Jun Gao and Xuanchi Ren.*


Lyra addresses the challenge of generating realistic 3D scenes from very limited input. Traditional 3D reconstruction methods usually require many camera views and precise camera positions, while video diffusion models can generate realistic videos but do not produce explicit 3D structures.


The main idea behind Lyra is to transfer the 3D understanding learned by a video diffusion model into a 3D Gaussian Splatting (3DGS) representation using a teacher–student training setup. Instead of relying on real multi-view datasets, the model is trained entirely on synthetic views generated by the diffusion model itself.


One of the most interesting aspects of Lyra is its flexibility: it can generate 3D scenes from a text prompt, a single image, or even a short video, and it also supports dynamic scene generation over time. The paper reports state-of-the-art performance on both static and dynamic 3D scene generation tasks.


[Lyra: Generative 3D Scene Reconstruction via Video Diffusion Model Self-Distillation](https://research.nvidia.com/labs/toronto-ai/lyra/)


#### EA3D: Event‑Augmented 3D Diffusion for Generalizable Novel View Synthesis


***Authors:*** *Wangbo Yu, Chaoran Feng, Jianing Li, Aofan Zhang, Zhenyu Tang, Mingyi Guo, Wei Zhang, Zhengyu Ma, Li Yuan and Yonghong Tian.*


EA3D focuses on generating new camera views in challenging situations with fast camera motion, where traditional NeRF and 3D Gaussian Splatting methods often fail because they rely on sparse RGB images.


The paper combines standard RGB frames with data from event cameras, which capture rapid brightness changes and motion information much more effectively. Instead of optimizing each scene independently, EA3D uses a diffusion-based framework that learns to combine appearance information from RGB images with geometric information extracted from event streams.


A key contribution is the **EA-Renderer** , which builds 3D scene features from both RGB and event data, allowing the model to generate realistic and temporally consistent novel views. The authors also introduce a new large-scale dataset, **Event-DL3DV** , designed specifically for this task.


What is particularly interesting is that EA3D achieves strong generalization across different scenes while handling difficult motion conditions much better than previous approaches.


[EA3D: Event-Augmented 3D Diffusion for Generalizable Novel View...](https://openreview.net/forum?id=YwawhlWdtm)


---


[What Stood Out at ICLR 2026: Criteo Papers and Research Highlights](https://medium.com/criteo-engineering/what-stood-out-at-iclr-2026-criteo-papers-and-research-highlights-3b54dba09447) was originally published in[Criteo Tech Blog](https://medium.com/criteo-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
