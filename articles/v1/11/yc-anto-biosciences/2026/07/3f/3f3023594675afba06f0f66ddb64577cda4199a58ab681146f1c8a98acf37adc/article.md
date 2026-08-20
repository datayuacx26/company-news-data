---
schema_version: "1.0.0"
document_id: "3f3023594675afba06f0f66ddb64577cda4199a58ab681146f1c8a98acf37adc"
company_key: "yc-anto-biosciences"
company: "Anto Biosciences"
source_id: "yc-anto-biosciences-news-import-c516c9aa21d4"
canonical_url: "https://anto.bio/publications/steering-the-evolutionary-game"
published_at: null
first_seen_at: "2026-07-24T23:08:26.325303+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:46484ee740a3d30988821e2dce4d94e8503cb97f15916d1b2b980c37ae6057eb"
---

# Steering the Evolutionary Game

## Hierarchical Control of Therapeutic Resistance in Cancer Treatment


### Abstract


failure in cancer often arises from Darwinian selection for drug resistance. We introduce a hierarchical ecological control framework that reshapes the evolutionary fitness landscape on a slow timescale (microbiome modulation) to render the fast tumor--immune--drug dynamics curative. We prove existence and uniqueness of a Markov-perfect Nash equilibrium for the LQ fast game and formalize a conservative robust immune-influx threshold \\(s_{crit,\\max}^{rob}\\le\\tfrac{d}{n}a\\). All nonlinear results are stated relative to \\(s_{crit,\\max}^{rob}\\). Maintaining \\(s(\\mathbf{M})\\ge s_{crit,\\max}^{rob}+\\delta\\) renders the tumor-free state globally exponentially stable with rate \\(\\lambda=\\min\\{\\tfrac{n\\delta}{2d}, \\tfrac{d-g_{\\max}}{2}\\}\\) and explicit gain bounds. Robustness is established via: (i) spatial sufficiency using domain eigenvalues precluding sanctuaries, (ii) global asymptotic stability in probability under stochastic perturbations when \\(L_z<\\sqrt{\\lambda/K}\\) (and almost-sure convergence under additional recurrence conditions), (iii) quantitative tolerance to clonal heterogeneity when \\(\\Delta_a+\\Delta_\\kappa u_{d,\\max}<\\tfrac{n}{d} \\delta\\), (iv) delay and observer robustness under precise small-gain and observability conditions, and (v) an \\(\\epsilon\\)-accurate Fenichel decomposition of the two-timescale game.


Using TCGA-derived priors, AI-synthesized policies enforce the stability margin along trajectories and achieve high efficacy with lower cytotoxic exposure. In Skin Cutaneous Melanoma (SKCM) the controller achieves **89%** eradication (95% CI \\(\\pm6\\)), maintains time-above-threshold at **92%** ( \\(\\pm5\\)), and reduces peak tumor burden by **57%** ; first response occurs in 28--35 days with a lower dose index. In colorectal cancer (CRC) the controller achieves **76%** eradication ( \\(\\pm7\\)) with **88%** time-above-threshold ( \\(\\pm6\\)). Across structured perturbations (5-clone heterogeneity, 14-day delay, stochastic noise, partial observability), individual eradication rates exceed 80% and remain 73% under combined stressors, aligning with the theory. These results establish ecological landscape engineering as a principled, general strategy for mitigating resistance in oncology.


### Key Results


By eliminating the evolutionary pathways that lead to resistance, this new method shows transformative clinical potential when tested with real cancer genomic data:


#### Melanoma (Skin Cancer):


- 89% complete tumor eradication (with high confidence)
- 92% of treatment time maintaining therapeutic effectiveness
- 57% reduction in peak tumor burden


#### Colorectal Cancer:


- 76% eradication rate
- 88% time above therapeutic threshold
- Consistently effective with reduced toxicity


Current cancer treatment is reactive, but this approach is proactive and preventive, controlling the evolutionary rules that made cancer so deadly.


‍
