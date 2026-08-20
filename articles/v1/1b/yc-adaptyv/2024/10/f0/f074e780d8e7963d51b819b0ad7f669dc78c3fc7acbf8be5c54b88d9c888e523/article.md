---
schema_version: "1.0.0"
document_id: "f074e780d8e7963d51b819b0ad7f669dc78c3fc7acbf8be5c54b88d9c888e523"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-rss-523875a8e4a3"
canonical_url: "https://adaptyvbio.substack.com/p/protein-optimization-101-insights"
published_at: "2024-10-09T13:02:07+00:00"
first_seen_at: "2026-07-24T14:21:19.055100+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:321f6a0f7506e39d941abd190d77ad06a850be9f4b4b444492f296634c20e106"
---

# Protein Optimization 101: Insights from the literature

# Protein Optimization 101: Insights from the literature


### For this technical blog post, we surveyed the state of the art of using ML for protein optimization. We focused on adaptive, cost constrained, and multi-objective methods and have summarized the best


[Adaptyv Bio](https://substack.com/@adaptyvbio)


Oct 09, 2024


> TL;DR
>
>
> -
>
>
> Protein Optimization is tricky, as evidenced by our recent
>
>
> [protein design competition](https://design.adaptyvbio.com/) .
>
>
> -
>
>
> If you don’t have a large budget, you need to be strategic about how you optimize your proteins. Especially if you want to compete in our next round.
>
>
> -
>
>
> For this blog post, we surveyed the state of the art (SOTA) of using ML protein optimization. We focused on adaptive, cost constrained, and multi-objective methods and have summarized the best papers for you (some of which yielding as much as 7.5-fold improvement, see below!).
>
>
> -
>
>
> In the next blogpost, we will dive deeper and give concrete, actionable recommendations based on the survey and the learnings from our competition.
>
>
> -
>
>
> If you already know the field and just want to see our takes a selection of recent papers skip to
>
>
> [“Insights from the literature”](https://www.adaptyvbio.com/cc8248e855f4479f9df207938d2db6be#1185ca69e7be800ca9e8cea02dfd1881) , or for the full survey with all details, click
>
>
> [here](https://link.adaptyvbio.com/po_papers) .
>
>
> -
>
>
> If you want to understand them a bit better and need an intro to the underlying concepts, read on.


### Introduction


Imagine you have 20 or so experimentally validated EGFR-binding proteins, maybe from our recent


[protein design competition](https://design.adaptyvbio.com/) . None of them are fit for your application yet, so you want to further optimize them before you submit them to round two. Or maybe you got some decent binders, but need to re-optimize to find a trade-off between their binding strength and their expression in a high-yield bioreactor for commercialization.


You can of course re-use tools like RFdiffusion or the newly released


[AlphaProteo](https://deepmind.google/discover/blog/alphaproteo-generates-novel-proteins-for-biology-and-health-research/) . You can have the model propose hundreds of promising variants of your designs, and then use heuristics (for example other ML models) to select candidates for another round of validation. However, you might not have the budget of DeepMind or the capacity of the


[Baker lab](https://www.bakerlab.org/) to filter out and screen hundreds of candidates playing this “


*de novo* slot machine” in hopes of improvement.


In this blog post, we will give a brief introduction to optimization techniques aiming to minimize the number of real world measurements required, i.e. aiming for "sample efficiency" to use the technical term. We will use a hypothetical lab budget constrained protein engineering scenario, characteristic of an individual designer or small company which just got some angel investment to build a proof of concept, but the same techniques can be used purely


*in silico* , for example to optimize the score function used in your favourite EGFR binding competition.


This series of blog posts is aimed at practitioners or those aiming to become one, so while we try to avoid obscure jargon, if you are completely new to this your favorite LLM or search engine might be required for some more technical sections and concepts.


### Problem setting


To ground the discussion and constrain the survey at the end of the post, we will assume that


1.


you have ample, but not infinite compute (so detailed molecular dynamics simulations are out).


2.


your total lab budget in this optimization campaign is around 200 validated designs (or only around $ 24’000 using


[Adaptyv’s data generation capabilities](https://beta.adaptyvbio.com/) ) - these should include both the variants you put back in your optimization loops and the chosen leading candidates.


As we will see, most techniques aiming at sample efficient protein


**optimization** fall under the framework of


**model-based optimization** (MBO,


[Hie & Yang, 2022](https://www.sciencedirect.com/science/article/pii/S0959440X21001457?via%3Dihub=) ). Here a


**surrogate** model is trained to predict a fitness score (e.g., protein folding stability, binding affinity, expressibility, etc.) from a set of labelled training points. This model is then used to either:


1.


find promising new variants by directly optimizing the seed sequence.


2.


or to trade off exploration (with the goal of further improving the surrogate model) and exploitation (picking the best final candidate given the model) when sampling candidates from a


**generator** .


We distinguish optimization from single-shot


*de novo* /


** conditional design with filtering, the key difference being that are trying to improve a seed sequence using a surrogate model trained on true fitness values (from a small labelled dataset - low-N regime, or entire Deep Mutational Scanning/ DMS datasets).


22For those in the know, think Bayesian Optimization (BO) using deep learning models applied to protein engineering.


Main themes in sequential and fixed model protein optimization.


### Training your model before or during the optimization campaign


Following


[Hie & Yang, 2022](https://www.sciencedirect.com/science/article/pii/S0959440X21001457?via%3Dihub=) , we can distinguish between


**sequentially-optimized models** and


**fixed model approaches** . Sequentially-optimized means we are retraining the surrogate used for scoring and/ or the generator used to propose new candidates


*online,* as we obtain newly validated data. The generator can be a generative model such as a VAE or a GAN, but also a simple mutation and recombination scheme, although it generally won't be as adaptable.


Thus, the optimization algorithm acts in a self-guided manner: it proposes new sequences, receives feedback and then learns to either generate better sequences in the same sequence space (if updating the generator) and/ or improves its fitness estimates (if updating the surrogate).


Sequentially-optimized models need to balance exploration (learning the fitness landscape) and exploitation (samping from regions with the highest expected fitness). Some models can adaptively transition between these two modes by directly using the surrogate, while others might balance them with a fixed parameter or avoid exploration completely and only suggest high fitness variants (exploit-only).


We will refer to the former category as


**model-based adaptive sampling methods** , and to the latter as


**greedy heuristics** . You can also check out this


[resource](https://mybinder.org/v2/gh/tudorcotet/bayesopt_sandbox/HEAD?urlpath=%2Fvoila%2Frender%2Fbayesopt_sandbox.ipynb) to get a better feel for the explore-exploit trade-off and Bayesian optimization.


Visualizing the explore-exploit trade-off in a simple Bayesian optimization loop with a Gaussian Process surrogate.


In contrast, within the


**fixed model approach** , all training happens before the optimization campaign. A model can be conditioned, guided, or simply trained once on the initial set of training points and used to validate randomly sampled designs (”sample and filter”).


This might be done because adaptive sampling is too computationally demanding, because it's too complex to reason about, implement or justify to stakeholders, or for other reasons.


Although this approach is not the focus of this blog post, it is very popular, so we will briefly give it some consideration in the appendix.


### Model-based versus heuristic sampling


As mentioned, we will split sequentially-optimized models into explicit adaptive models — which rely on the trained surrogate (or the generative model generating the candidates, in some cases) to balance between exploration and exploitation — and greedy/ heuristic methods, which either select the top predicted sequences to be evaluated (exploit-only) or


*a priori* establish how the fitness landscape should be parsed (fixed parameter).


Or, to make the distinction clear:


**models either are told how to navigate the landscape beforehand, or they adaptively learn how to do it** . In both cases, our final goal is to keep the number of experimental evaluations within our budget while maximizing fitness, in our example binding affinity to EGFR.


### Model-based adaptive sampling: Bayesian Optimization or Active Learning?


For the model-based sampling methods, an uncertainty-aware surrogate (a Gaussian Process, for example) models a distribution over a set of fitness scores given an initial pool of sequences.


If you bear with us through some mathematical notation, we can write the true fitness of a protein pp as f∗(p)


*f* ∗(p) and our estimate as random function f(p)∼N(μ(p),σ(p))


**f** (p)∼N(


*μ* (p),


*σ* (p)), making μ(p)


*μ* (p) our expected value of the protein fitness and σ(p)


*σ* (p) out measure of uncertainty.


Some sequences might have a high expected fitness μ(p)


*μ* (p) with low uncertainty, indicating a region in the fitness landscape from which it might be desirable to sample to optimize binding (exploitation). Some might have a slightly lower but still good μ(p)


*μ* (p) but also high uncertainty σ(p)


*σ* (p), and we might want to look into these to see if they lead to regions with even better binding affinity (exploration).


The strategy that determines which regions the model should explore next, i.e. which sequence should be validated next, is captured in what is called an acquisition function α(p)


*α* (p). A popular choice is called the


**Upper Confidence Bound (** UCBβUCB


*β* ​


**)**[(Srinivas 2009)](https://arxiv.org/abs/0912.3995) , which is simply α(p)≔UCBβ(p)≔μ(p)+βσ(p)


*α* (p):=UCB


*β* ​(p):=


*μ* (p)+


*β* ​


*σ* (p), with β≥0


*β* ≥0 being a hyperparameter that lets you choose your risk appetite. For a finite space, with ∣D∣ ∣


*D* ∣ options, for a confidence level δ


*δ* , the theory developed in


[(Srinivas 2009)](https://arxiv.org/abs/0912.3995) suggests choosing β=βt∗=2log⁡(∣D∣t2π2/6δ)


*β* =


*βt* ∗​=2log(∣


*D* ∣


*t* 2


*π* 2/6


*δ* ), increasing the importance of exploration at each round t


*t* as we learn more about the domain. With this, the model will act conservatively, making sure to keep exploring uncertain areas about as much as highly promising areas, while β=0


*β* =0 means we are directly improving the target property in a greedy manner.


[(Srinivas 2009)](https://arxiv.org/abs/0912.3995) found that the sweet spot lies between the two. Choosing a smaller, but non-zero value than their theory suggests, e.g β=0.2βt∗


*β* =0.2


*βt* ∗​ yielded the best results for them, but this will depend on the exact problem you are tackling.


Visualizing things, it will look like this:


How model-based sampling methods explore a 2D simulated fitness landscape.


-


**A)** a


**simulated fitness landscape** with input dimension (X1, X2) and a fitness dimension (color), with the previously sampled points shown in red and newly acquired points in yellow.


-


**B)** The Gaussian Process


**surrogate’s prediction** as a function of the input (X1, X2), showcasing its generalization when trained on a low number of data points.


-


**C) T** he


**surrogate’s model predicted uncertainty** (std) as a function of the entire input space - regions with lower uncertainty have been already explored.


-


**D)** The


**acquisition function’s values** (UCB - Upper Confidence Bound) when calculated for the entire input space, indicating the region that will get sampled from the most given the explore-exploit trade-off.


You might encounter the terms Bayesian Optimization and Active Learning as separate concepts while digging through the literature. Both fit into this model-based sampling category, the former being more common when tasked for optimizing a protein’s property, the latter for obtaining a more accurate fitness predictor or better training data.


Historically, Bayesian optimization involved using a budget to make a single, close to optimal decision as measured by a completely black-box fitness function, while active learning was independently developed for the efficient acquisition of a training set in hopes of learning a better predictive surrogate model. However, recent research has


**blurred the distinction between the two** , and mathematically they can be treated similarly (


[Kirsch, 2024](https://arxiv.org/abs/2401.04305) ). We therefore recommend thinking of them as a single approach and taking ideas from both communities as you embark on your protein optimization journey.


### Conceptual components


All methods we will discuss have the following key components:


1.


a (possibly uncertainty-aware)


**surrogate model** , mapping sequences to fitness estimates.


2.


an


**acquisition function** that considers both predicted values and possible variants, balancing exploration and exploitation to suggest candidates to be validated.


3.


an optional


**generative model** to sample candidates for fitness prediction, or alternatively a method to modify the input sequence to generate candidates.


4.


a


**ground-truth oracle** (computational or experimental) for candidate validation.


Active Learning and Bayesian optimization loop overview and key elements (surrogate, generator, acquisition, oracle).


We start the optimization by pre-training our models and generating candidates. Then the surrogate scores each candidate, with the acquisition function providing the final decision on which candidate to evaluate. Finally, we validate the chosen sequences, retrain the models on the extended dataset and repeat the cycle. The algorithm converges when it reaches a fitness threshold, no longer improves after a set number of iterations, or when we can no longer validate new candidates because our sampling budget of 200 sequences is exhausted.


### Sequence versus latent optimization


In addition to choosing the way we incorporate information during optimization, we also need to decide what space we want to operate in.


In the literature, optimization commonly occurs directly at the


**sequence level** : new mutations are sampled, fed through the surrogate, and selected if they reach a fitness threshold. However, since sequences are discrete objects, i.e. there is no residue that is halfway between


` S` and


` I` *,* optimizing them directly is often noisy and non-smooth due to the drastic fitness changes due to single-point mutations (landscape roughness).


Even worse, fitness values can be greatly influenced by


**epistasis** , where several sites in the protein bear long-range interactions. In this case combinations of mutations can have an additive effect on the overall activity, whilst most of the landscape bears a similar fitness (landscape sparsity). This landscape roughness and sparsity problem has been addressed in two major ways:


**latent optimization** and


**landscape smoothing techniques** .


Latent optimization involves replacing sequences with embedding vectors. These vectors come from pre-trained model, usually variational autoencoders or protein language models.


Several studies have found them to be meaningful, evolutionary-informed representations (


[Lin et al., 2023](https://www.science.org/doi/10.1126/science.ade2574) ;


[Detlefsen, Hauberg & Boomsma, 2022](https://www.nature.com/articles/s41467-022-29443-w) ;


[Ding, Zou & Brooks, 2019](https://www.nature.com/articles/s41467-019-13633-0) ).


Assuming our representations are good enough, optimization can then take place in a continuous vector space. Such a space allows for arbitrarily small (i.e. careful) exploration steps. Together with other aspects of representational embeddings, this makes task more amenable to classic methods of Bayesian optimization like Gaussian Processes using RBF kernels (


[Maus et al., 2022](https://arxiv.org/abs/2201.11872) ;


[Gómez-Bombarelli et al., 2018](https://pubs.acs.org/doi/10.1021/acscentsci.7b00572) ;


[Tripp, Daxberger & Miguel Hernández-Lobato, 2020](https://proceedings.neurips.cc/paper/2020/file/81e3225c6ad49623167a4309eb4b2e75-Paper.pdf) ).


Moreover, we can combine gradient-based optimization with Bayesian methods on continuous inputs to find the global optimum faster and more robustly (


[Stanton et al., 2022](https://arxiv.org/pdf/2203.12742) ;


[Khang Ngo et al., 2024](https://www.biorxiv.org/content/10.1101/2024.04.13.589381v2.full.pdf) ;


[Gómez-Bombarelli et al., 2018](https://pubs.acs.org/doi/10.1021/acscentsci.7b00572) ). You should check out the references above to get more familiar with latent optimization.


[Kirjner, Yim, and colleagues](https://arxiv.org/pdf/2307.00494) addressed the roughness problem with a different approach.They converted the fitness landscape into a graph. In this graph, node attributes were based on fitness values and edges were based on the Levenshtein distance between sequence. They then further regularized the resulting graph Laplacian and used it to train their fitness surrogate. Under the crucial assumption that single-point mutants likely have an incremental effect on the fitness,this landscape smoothing ensures more similar sequences have similar fitness values.


### Insights from the literature


Now, let's say you want to use one of these methods for your protein design campaign. Where should you start?


We have performed a review of the state of the art protein ML models used for optimization and summarized our findings for you. We have focused on design and property targets, costs for optimization, fold-change improvements versus the wild-type, availability of good implementations and other aspects practitioners might care about to help you easily find only the papers that are of interest to you Untitled.


In the following, we highlight some of these, mainly recent adaptive models (Bayesian optimization/ Active learning/ Greedy), that we consider SOTAs or just good baselines. Any of these should be a decent starting point for budget constrained optimization campaigns.


Relative improvement of best variants vs cost (= number of labeled samples required) for optimization campaigns found in the literature. Up and left is better.


### Evolutionary Algorithms


Evolutionary algorithms like AdaLead (


[Sinai et al., 2020](https://arxiv.org/abs/2010.02141) ) and PEX (


[Ren et al., 2022](https://www.biorxiv.org/content/10.1101/2022.04.12.487986v2) ) generate new candidates by mutating and/ or recombining previous ones, followed by selection. In our framework they are sequentially-optimized models, with greedy/ heuristic candidate propositions, as sampling does not explicitly depend on the learned model and its acquisition function.


Code:


[https://flexs.readthedocs.io/en/latest/](https://flexs.readthedocs.io/en/latest/)


**AdaLead** is a greedy search optimization method, used in the literature as a robust benchmark. At each step, a model predicts fitness values for a selected batch of variants, with fixed parameter rates for recombination and mutation. The explore-exploit trade-off is not learned as the optimization proceeds. Instead, it is instilled a priori via a parameter κ that controls the fitness threshold for selecting new variants (within 1 − κ of the maximum fitness observed previously


*)* .


The top variants within this threshold are then selected (greedy algorithm). The paper argues this is still an adaptive sampling approach, since exploration is encouraged on flat fitness landscapes, followed by a switch to exploitation as the maximum observed fitness increases, with fewer potential candidates being proposed. It is relatively robust, with minimal assumptions, high interpretability, and easy to implement, thanks to the


[FLEXS](https://flexs.readthedocs.io/en/latest/index.html) landscape simulation environment and Python package (


[Sinai et al., 2020](https://arxiv.org/abs/2010.02141) ). Moreover, FLEXS offers implementations of other landscape explorers like


[DbAS](https://github.com/samsinai/FLEXS/blob/dd409167b5575b51967d325b4e48aef3577a505f/flexs/baselines/explorers/cbas_dbas.py#L14) ,


[CbAS](https://github.com/samsinai/FLEXS/blob/dd409167b5575b51967d325b4e48aef3577a505f/flexs/baselines/explorers/cbas_dbas.py#L14) , and reinforcement learning ones like


[DyNA-PPO](https://github.com/samsinai/FLEXS/blob/master/flexs/baselines/explorers/dyna_ppo.py) .


Code:


[https://github.com/HeliXonProtein/proximal-exploration/tree/main](https://github.com/HeliXonProtein/proximal-exploration/tree/main)


Next,


**PEX** adapted this idea to focus more on local search. The acquisition function now includes an edit distance regularization term, controlled by a parameter λ, which penalizes candidates with high edit distance from wild-type sequences. This algorithm also deviates from the greedy selection approach. It instead selects candidates along the “proximal frontier”, defined as all maximal points at various λ. With this, PEX optimization yielded a great fitness improvement on an avGFP landscape. Maximum fitness (fluorescence intensity) was improved by 2-fold over the maximum value in the starting pool of sequences after 10 cycles with 100 sequences each, with a relatively lower mutation count (<10). On the same data, AdaLead reports a slightly worse performance (3.12 fitness score for PEX, 2.61 for AdaLead), with < 15 mutation count for the best performing variant as well. On an adeno-associated viruses VP1 protein dataset, AdaLead’s best variant had a mutation count of over 20 and a fitness score of 3.58, while PEX maintained it at below 5 and 4.45 score. The paper above offers an implementation of the


[PEX explorer](https://github.com/HeliXonProtein/proximal-exploration/blob/main/algorithm/pex.py) .


Both PEX and AdaLead can achieve relatively similar maximum fitness scores with the same number of oracle evaluations (1000). However, this is clearly beyond our constraint of 200. We recommend you use these 2 as benchmarks for a custom sequentially-optimized adaptive model you’re building, on either simulated fitness landscape (using the


[NK model](https://en.wikipedia.org/wiki/NK_model) for example) or available ones. Ensure your method is robust by testing it on several ground-truth landscapes. If it can beat these two evolutionary algorithms while reaching your fitness fold-change threshold within 200 total candidates, it could be used in a real active learning setting.


Code:


[https://github.com/churchlab/low-N-protein-engineering](https://github.com/churchlab/low-N-protein-engineering)


[Biswas and colleagues](https://www.nature.com/articles/s41592-021-01100-y) engineered a pre-training method using unlabeled data which can be fine tuned using small enough number of samples (N) that it can qualify as a heuristics guided sequential optimization scheme.


Evolutionary-related sequences to the optimization target are first mined into an unsupervised training dataset for a language model and used for ”evotuning”. In this case UniRep (


[Alley et al., 2019](https://www.nature.com/articles/s41592-019-0598-1) ) was used, but it could easily be adapted to more recent ones like ESM2 (


[Lin et al., 2023](https://www.science.org/doi/10.1126/science.ade2574) ) or 3 (


[Hayes et al., 2024](https://www.biorxiv.org/content/10.1101/2024.07.01.600583v1) ).


The model is subsequently fine-tuned given a small amount of labelled data (N = 24 or 96 random seed sequences) and used to navigate a fitness landscape with a Markov-chain Monte Carlo sampling scheme and greedy selection.


Finally, following ranking and a threshold of at most 15 mutations distance from the wild-type, 300 optimized designs are selected for a single round. From these, 10% were hits, with a fitness higher than the wildtype. Further restrictions on the mutation distance (to 7) enabled a hit rate of 18% with N = 96 and 1.8% with N = 24. We believe this approach could be plausibly be employed in a sequential optimization platform given decent hit rates in the low-N training regime, however it doesn’t have the theoretical interpretability of “proper” BO methods and might be sensitive to initialization.


Despite this, when scaled to a greater screening budget (larger batches than normally seen in classical BO), such “almost fixed” model loops with a surrogate-guided random search have been shown to effectively find high fold-change variants.


For example,


[Li and colleagues](https://www.nature.com/articles/s41467-023-39022-2) achieved a 28.7-fold improvement in antibody binding affinity over the training set following MLDE (Machine Learning-assisted Directed Evolution) using an ensemble of fitness predictors, a protein language model and a


[greedy hill climb sampling strategy](https://www.nature.com/articles/s41467-023-39022-2#:~:text=Optimization%20strategies%20via%20sampling) . This required pre-training on a library of


[22,188 heavy chain sequences with binding affinities](https://www.nature.com/articles/s41597-022-01779-4/tables/3) (


[Engelhart et al., 2022](https://www.nature.com/articles/s41597-022-01779-4) ). We can imagine how these models could be employed in sequential optimization loops given large enough batches or number of iterations. Another option would be likelihood-based directed evolution -


[Hie and colleagues](https://www.nature.com/articles/s41587-023-01763-2) leveraged this to obtain 5.1-fold affinity increase antibody for the Omicron BA.1 receptor-binding domain. You can play with the same technique


[here](https://www.tamarind.bio/antibody-evolution) , or with a more recent ones employing inverse folding (structure to sequence,


[Shanker et al., 2024](https://www.science.org/doi/full/10.1126/science.adk8946) ) models


[here](https://www.tamarind.bio/structural-evolution) .


If your screening budget is a bit larger than our assumptions or you already have a substantial screened library, you could fine-tune some PLMs. With model repositories like


[HuggingFace](https://huggingface.co/) and exhaustive online docs, it is now quite straight-forward to do so.


Code:


[https://github.com/idmjky/EvolvePro](https://github.com/idmjky/EvolvePro)


[Jiang and colleagues](https://www.biorxiv.org/content/10.1101/2024.07.17.604015v1.full.pdf) achieved impressive results with their EVOLVEpro active learning framework which makes use of ESM2 embeddings and iterative optimization of a random forest (RF) surrogate.


They managed to evolve a miniature CRISPR nuclease with 2.2-44-fold increase in the indel%, an antibody with 63% better binding to the SARS-CoV-2 spike protein, a 4-fold improved integrase and even a T7 polymerase with a 7-fold increase expression and 2-fold decrease in the product immunogenicity.


Starting from a set of seed sequences, an initial round of experimental validation is performed (N = 10-16 variants), with the surrogate later trained on this data. This is followed by embedding all single point mutants of the seed sequences, using these to predict the fitness with the RF surrogate, followed by ranking and selection.


For the CRISPR nuclease evolution, 12 mutants were screened for 5 rounds, yielding the impressive fold-change improvement with a minimal experimental burden of 60 total variants. Similar number of total variants were screened across the other optimization campaigns (e.g., 96 variants across 8 iterations for the Bxb1 integrase). If these impressive results hold up — which one might hope for given the solid experimental validation in the paper — EVOLVEpro appears well suited for our budget constrained use case.


The code is accessible


[here](https://github.com/idmjky/EvolvePro) and is easy to understand if you are in the field. We think EVOLVEpro, accompanied by AdaLead and PEX (all of them sequentially optimized models with greedy selection), will become a the baseline to beat for any optimization endeavour.


Code:


[https://github.com/jsunn-y/ALDE](https://github.com/jsunn-y/ALDE)


Within their optimization framework ALDE (Active Learning-assisted Directed Evolution),


[Yang and colleagues](https://www.biorxiv.org/content/10.1101/2024.07.27.605457v2.full.pdf) compared active learning loops with Bayesian methods (acquisition functions, uncertainty-aware surrogates) with simple machine learning-assisted directed evolution.


They tackled a epistatic fitness landscape for a biocatalyst based on a protoglobin


*Par* Pgb. Epistasis reflects a great challenge in protein engineering: fitness cannot be decomposed as a simple sum of each mutation’s effect and some combinations might greatly increase it or drastically reduce it (


[Lipsh-Sokolik & Fleishman, 2024](https://www.pnas.org/doi/abs/10.1073/pnas.2314999121?trk=feed_main-feed-card_feed-article-content) ).


ALDE benchmarked several sequence encoding schemes (AAIndex, Georgiev, one-hot, ESM2), fitness surrogates (boosting ensemble, Gaussian Processes, deep neural network ensemble/ DNN, deep kernel learning), acquisition functions (greedy, upper confidence bound, Thompson sampling) on two ground-truth landscapes - GB1 (


[Wu et al., 2016](https://elifesciences.org/articles/16965) ) and TrpB (


[Johnston et al., 2024](https://www.pnas.org/doi/10.1073/pnas.2400439121) ). The most successful combination was one-hot encodings, a DNN ensemble, and Thompson sampling.


Adapted to the biocatalyst evolution task, two rounds of optimization increased the cyclopropanation yield of the lead variant from 12% to 99%, with better cis:trans selectivity (14:1). Overall, the entire campaign required only 396 experimental calls (216 seed sequences and 90 per round), effectively giving rise to an 8.5-fold fitness increase variant.


ALDE identified a promising recipe for parsing epistatic landscapes. The paper’s


[code](https://github.com/jsunn-y/ALDE) is accessible and well-documented, and you should find it easy to implement it from scratch if you are familiar with active learning and Bayesian optimization. The these validations requirements are moderately beyond our budget - one could explore reducing the number of seed variants, but this could have a detrimental effect on the overall convergence. As stated in the paper, zero-shot fitness predictors (PLMs, mutation effect predictors like EVmutation,


[Hopf et al., 2017](https://www.nature.com/articles/nbt.3769) ) could be used to design the starting library if the target property you want to optimize is captured by evolutionary (as predicted by EVmutation) or stability-based (as predicted by ESM2 due to its training data) metrics. Similarly, a recent pre-print from JURA Bio showed that variational synthesis models can generate a more informative library compared to random mutagenesis/ NNK libraries (


[Weinstein et al., 2024](https://static1.squarespace.com/static/66db3a552002432ff6a39dae/t/66e45d8b7ed2bf1a517e03c1/1726242191877/JURA_VariationalSynthesis.pdf) ).


We hope this blog post clarified some similarities and key differences between the classes of machine learning models in protein desing, offered insights into the currently used approaches, and whetted your appetite to explore model-based sampling.


**Now what are you waiting for? Start optimizing those binders! → [adaptyvbio.com](https://adaptyvbio.com/)**


---


### Supplementary - Fixed model optimization


Fixed model approaches typically employ a, well,


*fixed* pre-trained model. This can be a surrogate to map out the fitness landscape on


*in silico* generated mutants or a generator conditionally sampling binders for a target. Training or fine tuning these models generally takes a lot more data than our assumed budget allows.


Some fine-tuning approaches can operate in a low-N (= training samples) setting (e.g., by using a regression layer on top of a protein language model,


[Biswas et al., 2021](https://www.nature.com/articles/s41592-021-01100-y) ), which would allow tuning the 200 heuristically chosen data points initially and use that to test out and select new candidates for measurements, further retraining the model if you have the budget (see above).


Types of fixed* (given the constraint) models for protein optimization and design, categorized by training budgets. Often, these are trained only once on the starting dataset, and no longer adapted on newly acquired batches.


### Surrogate-only or machine learning-assisted directed evolution:


Until very recently, the standard approach involved training a surrogate fitness predictor on available labelled data, then generating


*in silico* mutational libraries and selecting top variants from the predictor’s output. This is a typical directed evolution approach, but with the surrogate model replacing the laborious experimental validation. These methods have been grouped together as machine learning-assisted directed evolution (MLDE -


[Yang, Wu & Arnold, 2019](https://www.nature.com/articles/s41592-019-0496-6) ;


[Wittmann, Yue & Arnold, 2021](https://www.sciencedirect.com/science/article/pii/S2405471221002866) ;


[Qiu, Hu & Wei, 2021](https://www.nature.com/articles/s43588-021-00168-y) ;


[Wu et al., 2019](https://www.pnas.org/doi/full/10.1073/pnas.1901979116) ), with some being trained sequentially given new batches of data


*.*


They mainly differ in details, such as


1.


how many labelled training points they demand (


[Wittmann, Yue & Arnold, 2021](https://www.sciencedirect.com/science/article/pii/S2405471221002866) ,


[Biswas et al., 2021](https://www.nature.com/articles/s41592-021-01100-y) ).


2.


how they can extrapolate to combinatorial mutations after being trained on single-point mutants (


[Widatalla, Rafailov & Hie, 2024](https://www.biorxiv.org/content/10.1101/2024.05.20.595026v1) ;


[Freschlin et al., 2024](https://www.nature.com/articles/s41467-024-50712-3) ).


3.


the sequence features: one-hot or embeddings from a large language model, with several systematic benchmarks (


[Greenman, Amini & Yang, 2023](https://www.biorxiv.org/content/10.1101/2023.04.17.536962v1) ;


[Shanehsazzadeh et al., 2020](https://arxiv.org/abs/2011.03443) ;


[Schmirler, Heinzinger & Rost, 2024](https://www.nature.com/articles/s41467-024-51844-2) ).


4.


the architectures, with 1D CNN and CNN ensembles generally preferred for prediction and ability to extrapolate to higher-order mutants (


[Shanehsazzadeh et al., 2020](https://arxiv.org/abs/2011.03443) ;


[Freschlin et al., 2024](https://www.nature.com/articles/s41467-024-50712-3) ;


[Sinai et al., 2020](https://arxiv.org/pdf/2010.02141) ).


### Pre-trained generative models:


The explosion of diffusion and protein language models (PLMs) had a significant impact in the field of protein optimization. It allowed practitioners to efficiently optimize a property in the absence of data, only starting from a wild-type sequence or simply from noise.


One approach involved masking out residues in a starting sequence, then querying the PLM to predict the most likely one (with variations in the sampling/ masking procedure and type or number of PLMs used). In this way a sequence can be optimized to exhibit the highest fitness/naturalness, known as unsupervised evolution. This has been correlated with desirable properties such as improved antibody binding affinity (


[Hie et al., 2023](https://www.nature.com/articles/s41587-023-01763-2) ;


[Shanker et al., 2024](https://www.science.org/doi/full/10.1126/science.adk8946) ).


Other types of autoregressive models can also generate proteins with a desired function by starting from a single conditioning token (


[Madani et al., 2023](https://www.nature.com/articles/s41587-022-01618-2) ;


[Munsamy et al., 2024](https://www.biorxiv.org/content/10.1101/2024.05.03.592223v1) ). Several PLM studies explored the influence of additional modalities such as text description (


[Xu et al., 2023](https://arxiv.org/abs/2301.12040) ;


[Liu et al., 2023](https://arxiv.org/abs/2302.04611) ;


[Qiu et al., 2024](https://www.biorxiv.org/content/10.1101/2024.04.17.589642v1) ) or structure (


[Heinzinger et al., 2024](https://www.biorxiv.org/content/10.1101/2023.07.23.550085v2) ;


[Su et al., 2024a](https://www.biorxiv.org/content/10.1101/2023.10.01.560349v5) ,


[2024b](https://www.biorxiv.org/content/10.1101/2024.05.30.596740v1) ), preference-optimization (


[Widatalla, Rafailov & Hie, 2024](https://www.biorxiv.org/content/10.1101/2024.05.20.595026v1) ), retrieval-augmentation (


[Shaw et al., 2024](https://www.biorxiv.org/content/10.1101/2024.05.30.596539v1) ), and different pre-training schemes (


[Truong et al., 2023](https://arxiv.org/abs/2306.06156) ).


One caveat for unsupervised PLM evolution is that the correlation of sequence likelihood with protein function is highly dependent on the target property. Thus using it as an optimization proxy does not ensure we always optimize the desired property. Deep mutational scanning datasets like the ones provided ProteinGym (


[Notin et al., 2023](https://www.biorxiv.org/content/10.1101/2023.12.07.570727v1) ), FLIP (


[Krishna et al., 2021](https://www.biorxiv.org/content/10.1101/2021.11.09.467890v1) ), and FLAb (


[Chungyoun, Ruffolo & Gray, 2024](https://www.biorxiv.org/content/10.1101/2024.01.13.575504v1.full) ) are important to benchmark language models’ ability to predict the effect of mutations on various experimentally measured properties and see which one can generalize better.


Shifting model class completely, denoising diffusion models are trained to sample valid designs directly from noise (


[Anand & Achim, 2022](https://arxiv.org/abs/2205.15019) ). Various extensions have been explored, allowing for ligand diffusion (


[Krishna et al., 2024](https://www.science.org/doi/10.1126/science.adl2528) ), conditioning generation on a target binding partner (


[Watson et al., 2023](https://www.nature.com/articles/s41586-023-06415-8) ) or evolutionary information (


[Alamdari et al., 2023](https://www.biorxiv.org/content/10.1101/2023.09.11.556673v1) ), and gradient-guidance via externaly trained surrogates and classifiers (


[Gruver et al., 2023](https://arxiv.org/abs/2305.20009) ;


[Ingraham et al., 2023](https://www.nature.com/articles/s41586-023-06728-8) ).


For example, partial diffusion with RFdiffusion enables the diversification of a known working design’s structure (


[Watson et al., 2023](https://www.nature.com/articles/s41586-023-06415-8) ). We can then attempt to sample correctly folding sequences with ProteinMPNN (


[Dauparas et al., 2022](https://www.science.org/doi/10.1126/science.add2187) ) or LigandMPNN (


[Dauparas et al., 2023](https://www.biorxiv.org/content/10.1101/2023.12.22.573103v1) ), resulting in a larger library of potential candidates to be screened. This has been done for


[enzyme design](https://meilerlab.org/wp-content/uploads/2024/07/ml_enzyme_design.pdf) .


The main disadvantages of these


*de novo* models are the complex — in terms of hyperparameters and setup — and computationally demanding design filtering pipelines. They often require structure folding with AlphaFold2, selecting top pLDDT and PAE candidates, and external property predictors depending on the optimization campaign. This resembles a semi-random search process more than an iterative optimization approach: designs are constantly sampled from a “valid” generator trained on natural proteins from PDB or UniRef, and fitered based on thresholds from


*in silico* oracles.


The difficulty in obtaining a high rate of successful candidates with these methods has been demonstrated in the recent AlphaProteo preprint (


[Zambaldi et al., 2024](https://arxiv.org/abs/2409.08022) ), a diffusion model and RFdiffusion rival from Google DeepMind. Compared to RFdiffusion, when designing an interleukin-7 receptor-𝛼 binder, AlphaProteo had a considerable “


*in silico* success rate” . This was defined as interchain AF2 pAE < 10, binder-aligned binder RMSD < 1 Å, and plDDT > 80 and was approx. 30% for AlphaProteo. The experimental success rate was 24.5% versus RFdiffusion’s 16.8%.


While impressive, the limited edge a billion dollar company achieved over an academic lab shows the challenge of achieving reliable and predictable results with this type of non-iterative protein design based on


*in silico* filters alone.


### References


\[1\] Alamdari, S., Thakkar, N., Berg, R. van den, Lu, A.X., Fusi, N., Amini, A.P. & Yang, K.K. (2023) Protein generation with evolutionary diffusion: sequence is all you need. bioRxiv. 2023.09.11.556673. doi:10.1101/2023.09.11.556673.


\[2\] Alley, E.C., Khimulya, G., Biswas, S., AlQuraishi, M. & Church, G.M. (2019) Unified rational protein engineering with sequence-based deep representation learning. Nature methods. 16 (12), 1315. doi:10.1038/S41592-019-0598-1.


\[3\] Anand, N. & Achim, T. (2022) Protein Structure and Sequence Generation with Equivariant Denoising Diffusion Probabilistic Models.


[https://arxiv.org/abs/2205.15019v1](https://arxiv.org/abs/2205.15019v1) .


\[4\] Biswas, S., Khimulya, G., Alley, E.C., Esvelt, K.M. & Church, G.M. (2021) Low-N protein engineering with data-efficient deep learning. Nature Methods 2021 18:4. 18 (4), 389–396. doi:10.1038/s41592-021-01100-y.


\[5\] Chungyoun, M., Ruffolo, J. & Gray, J. (2024) FLAb: Benchmarking deep learning methods for antibody fitness prediction. bioRxiv. 2024.01.13.575504. doi:10.1101/2024.01.13.575504.


\[6\] Dauparas, J., Anishchenko, I., Bennett, N., Bai, H., Ragotte, R.J., et al. (2022) Robust deep learning–based protein sequence design using ProteinMPNN. Science. 378 (6615), 49–56. doi:10.1126/SCIENCE.ADD2187/SUPPL_FILE/SCIENCE.ADD2187_SM.PDF.


\[7\] Dauparas, J., Lee, G.R., Pecoraro, R., An, L., Anishchenko, I., Glasscock, C. & Baker, D. (2023) Atomic context-conditioned protein sequence design using LigandMPNN. bioRxiv. 2023.12.22.573103. doi:10.1101/2023.12.22.573103.


\[8\] Detlefsen, N.S., Hauberg, S. & Boomsma, W. (2022) Learning meaningful representations of protein sequences. Nature Communications 2022 13:1. 13 (1), 1–12. doi:10.1038/s41467-022-29443-w.


\[9\] Ding, X., Zou, Z. & Brooks, C.L. (2019) Deciphering protein evolution and fitness landscapes with latent space models. Nature Communications 2019 10:1. 10 (1), 1–13. doi:10.1038/s41467-019-13633-0.


\[10\] Engelhart, E., Emerson, R., Shing, L., Lennartz, C., Guion, D., Kelley, M., Lin, C., Lopez, R., Younger, D. & Walsh, M.E. (2022) A dataset comprised of binding interactions for 104,972 antibodies against a SARS-CoV-2 peptide. Scientific Data 2022 9:1. 9 (1), 1–8. doi:10.1038/s41597-022-01779-4.


\[11\] Freschlin, C.R., Fahlberg, S.A., Heinzelman, P. & Romero, P.A. (2024) Neural network extrapolation to distant regions of the protein fitness landscape. Nature Communications 2024 15:1. 15 (1), 1–13. doi:10.1038/s41467-024-50712-3.


\[12\] Gómez-Bombarelli, R., Wei, J.N., Duvenaud, D., Hernández-Lobato, J.M., Sánchez-Lengeling, B., Sheberla, D., Aguilera-Iparraguirre, J., Hirzel, T.D., Adams, R.P. & Aspuru-Guzik, A. (2018) Automatic Chemical Design Using a Data-Driven Continuous Representation of Molecules. ACS Central Science. 4 (2), 268–276. doi:10.1021/ACSCENTSCI.7B00572/SUPPL_FILE/OC7B00572_LIVESLIDES.MP4.


\[13\] Greenman, K.P., Amini, A.P. & Yang, K.K. (2023) Benchmarking Uncertainty Quantification for Protein Engineering. bioRxiv. 2023.04.17.536962. doi:10.1101/2023.04.17.536962.


\[14\] Gruver, N., Stanton, S., Frey, N., Rudner, T.G.J., Hotzel, I., Lafrance-Vanasse, J., Rajpal, A., Cho, K. & Wilson, A.G. (2023) Protein Design with Guided Discrete Diffusion. Advances in Neural Information Processing Systems. 36.


[https://arxiv.org/abs/2305.20009v2](https://arxiv.org/abs/2305.20009v2) .


\[15\] Hayes, T., Rao, R., Akin, H., Sofroniew, N.J., Oktay, D., et al. (2024) Simulating 500 million years of evolution with a language model. bioRxiv. 2024.07.01.600583. doi:10.1101/2024.07.01.600583.


\[16\] Heinzinger, M., Weissenow, K., Gomez Sanchez, J., Henkel, A., Mirdita, M., Steinegger, M., Rost, & B., Heinzinger, M., Weissenow, K., Gomez Sanchez, J., Henkel, A., Mirdita, M., Steinegger, M. & Prostt5, R. (2024) Bilingual Language Model for Protein Sequence and Structure. bioRxiv. 2023.07.23.550085. doi:10.1101/2023.07.23.550085.


\[17\] Hie, B.L., Shanker, V.R., Xu, D., Bruun, T.U.J., Weidenbacher, P.A., Tang, S., Wu, W., Pak, J.E. & Kim, P.S. (2023) Efficient evolution of human antibodies from general protein language models. Nature Biotechnology 2023 42:2. 42 (2), 275–283. doi:10.1038/s41587-023-01763-2.


\[18\] Hie, B.L. & Yang, K.K. (2022) Adaptive machine learning for protein engineering. Current Opinion in Structural Biology. 72, 145–152. doi:10.1016/J.SBI.2021.11.002.


\[19\] Hopf, T.A., Ingraham, J.B., Poelwijk, F.J., Schärfe, C.P.I., Springer, M., Sander, C. & Marks, D.S. (2017) Mutation effects predicted from sequence co-variation. Nature Biotechnology 2017 35:2. 35 (2), 128–135. doi:10.1038/nbt.3769.


\[20\] Ingraham, J.B., Baranov, M., Costello, Z., Barber, K.W., Wang, W., et al. (2023) Illuminating protein space with a programmable generative model. Nature 2023 623:7989. 623 (7989), 1070–1078. doi:10.1038/s41586-023-06728-8.


\[21\] Jiang, K., Yan, Z., Bernardo, M. Di, Sgrizzi, S.R., Villiger, L., Kayabolen, A., Kim, B., Carscadden, J.K., Hiraizumi, M., Nishimasu, H., Gootenberg, J.S. & Abudayyeh, O.O. (2024) Rapid protein evolution by few-shot learning with a protein language model. bioRxiv. 2024.07.17.604015. doi:10.1101/2024.07.17.604015.


\[22\] Johnston, K.E., Almhjell, P.J., Watkins-Dulaney, E.J., Liu, G., Porter, N.J., Yang, J. & Arnold, F.H. (2024) A combinatorially complete epistatic fitness landscape in an enzyme active site. Proceedings of the National Academy of Sciences of the United States of America. 121 (32), e2400439121. doi:10.1073/PNAS.2400439121/SUPPL_FILE/PNAS.2400439121.SAPP.PDF.


\[23\] Khang Ngo, N., T Tran, T. V, Thanh Duy Nguyen, V. & Son Hy, T. (n.d.) LATENT-BASED DIRECTED EVOLUTION ACCELERATED BY GRADIENT ASCENT FOR PROTEIN SEQUENCE DESIGN 1 Latent-based Directed Evolution accelerated by Gradient Ascent for Protein Sequence Design. doi:10.1101/2024.04.13.589381.


\[24\] Kirjner, A., Yim, J., Samusevich, R., Bracha, S., Jaakkola, T., Barzilay, R. & Fiete, I. (n.d.) IMPROVING PROTEIN OPTIMIZATION WITH SMOOTHED FITNESS LANDSCAPES.


[https://github.com/kirjner/GGS](https://github.com/kirjner/GGS) .


\[25\] Kirsch, A. (2024) Advancing Deep Active Learning & Data Subset Selection: Unifying Principles with Information-Theory Intuitions. doi:10.5287/ora-koewoxvaw.


\[26\] Krishna, R., Wang, J., Ahern, W., Sturmfels, P., Venkatesh, P., et al. (2021) FLIP: Benchmark tasks in fitness landscape inference for proteins. bioRxiv. 2021.11.09.467890. doi:10.1101/2021.11.09.467890.


\[27\] Krishna, R., Wang, J., Ahern, W., Sturmfels, P., Venkatesh, P., et al. (2024) Generalized biomolecular modeling and design with RoseTTAFold All-Atom. Science. 384 (6693). doi:10.1126/SCIENCE.ADL2528/SUPPL_FILE/SCIENCE.ADL2528_MDAR_REPRODUCIBILITY_CHECKLIST.PDF.


\[28\] Li, L., Gupta, E., Spaeth, J., Shing, L., Jaimes, R., Engelhart, E., Lopez, R., Caceres, R.S., Bepler, T. & Walsh, M.E. (2023) Machine learning optimization of candidate antibody yields highly diverse sub-nanomolar affinity antibody libraries. Nature Communications 2023 14:1. 14 (1), 1–12. doi:10.1038/s41467-023-39022-2.


\[29\] Lin, Z., Akin, H., Rao, R., Hie, B., Zhu, Z., Lu, W., Smetanin, N., Verkuil, R., Kabeli, O., Shmueli, Y., dos Santos Costa, A., Fazel-Zarandi, M., Sercu, T., Candido, S. & Rives, A. (2023) Evolutionary-scale prediction of atomic-level protein structure with a language model. Science. 379 (6637), 1123–1130. doi:10.1126/SCIENCE.ADE2574/SUPPL_FILE/SCIENCE.ADE2574_SM.PDF.


\[30\] Lipsh-Sokolik, R. & Fleishman, S.J. (2024) Addressing epistasis in the design of protein function. Proceedings of the National Academy of Sciences. 121 (34), e2314999121. doi:10.1073/PNAS.2314999121.


\[31\] Liu, S., Li, Y., Li, Z., Gitter, A., Zhu, Y., Lu, J., Xu, Z., Nie, W., Ramanathan, A., Xiao, C., Tang, J., Guo, H. & Anandkumar, A. (2023) A Text-guided Protein Design Framework.


[https://arxiv.org/abs/2302.04611v3](https://arxiv.org/abs/2302.04611v3) .


\[32\] Madani, A., Krause, B., Greene, E.R., Subramanian, S., Mohr, B.P., Holton, J.M., Olmos, J.L., Xiong, C., Sun, Z.Z., Socher, R., Fraser, J.S. & Naik, N. (2023) Large language models generate functional protein sequences across diverse families. Nature Biotechnology 2023 41:8. 41 (8), 1099–1106. doi:10.1038/s41587-022-01618-2.


\[33\] Maus, N.T., Moore, J.S., Bradshaw, J., Jones, H.T., Kusner, M.J. & Gardner, J.R. (2022) Local Latent Space Bayesian Optimization over Structured Inputs. Advances in Neural Information Processing Systems. 35.


[https://arxiv.org/abs/2201.11872v2](https://arxiv.org/abs/2201.11872v2) .


\[34\] Munsamy, G., Illanes-Vicioso, R., Funcillo, S., Nakou, I.T., Lindner, S., Ayres, G., Sheehan, L.S., Moss, S., Eckhard, U., Lorenz, P. & Ferruz, N. (2024) Conditional language models enable the efficient design of proficient enzymes. bioRxiv. 2024.05.03.592223. doi:10.1101/2024.05.03.592223.


\[35\] Notin, P., Kollasch, A.W., Ritter, D., Niekerk, L. van, Paul, S., Spinner, H., Rollins, N., Shaw, A., Weitzman, R., Frazer, J., Dias, M., Franceschi, D., Orenbuch, R., Gal, Y. & Marks, D.S. (2023) ProteinGym: Large-Scale Benchmarks for Protein Design and Fitness Prediction. bioRxiv. 2023.12.07.570727. doi:10.1101/2023.12.07.570727.


\[36\] Qiu, J., Xu, J., Hu, J., Cao, H., Hou, L., et al. (2024) InstructPLM: Aligning Protein Language Models to Follow Protein Structure Instructions. bioRxiv. 2024.04.17.589642. doi:10.1101/2024.04.17.589642.


\[37\] Qiu, Y., Hu, J. & Wei, G.W. (2021) Cluster learning-assisted directed evolution. Nature Computational Science 2021 1:12. 1 (12), 809–818. doi:10.1038/s43588-021-00168-y.


\[38\] Ren, Z., Li, J., Ding, F., Zhou, Y., Ma, J. & Peng, J. (2022) Proximal Exploration for Model-guided Protein Sequence Design. bioRxiv. 2022.04.12.487986. doi:10.1101/2022.04.12.487986.


\[39\] Schmirler, R., Heinzinger, M. & Rost, B. (2024) Fine-tuning protein language models boosts predictions across diverse tasks. Nature Communications 2024 15:1. 15 (1), 1–10. doi:10.1038/s41467-024-51844-2.


\[40\] Shanehsazzadeh, A., Belanger, D., Research, G. & Dohan, D. (2020) Is Transfer Learning Necessary for Protein Landscape Prediction?


[https://arxiv.org/abs/2011.03443v1](https://arxiv.org/abs/2011.03443v1) .


\[41\] Shanker, V.R., Bruun, T.U.J., Hie, B.L. & Kim, P.S. (2024) Unsupervised evolution of protein and antibody complexes with a structure-informed language model. Science (New York, N.Y.). 385 (6704), 46–53. doi:10.1126/SCIENCE.ADK8946/SUPPL_FILE/SCIENCE.ADK8946_MDAR_REPRODUCIBILITY_CHECKLIST.PDF.


\[42\] Shaw, P., Gurram, B., Belanger, D., Gane, A., Bileschi, M.L., Colwell, L.J., Toutanova, K., Parikh, A.P. & DeepMind, G. (2024) ProtEx: A Retrieval-Augmented Approach for Protein Function Prediction. bioRxiv. 2024.05.30.596539. doi:10.1101/2024.05.30.596539.


\[43\] Sinai, S., Wang, R., Whatley, A., Slocum, S., Locane, E. & Kelsic, E.D. (2020) AdaLead: A simple and robust adaptive greedy search algorithm for sequence design.


[https://arxiv.org/abs/2010.02141v1](https://arxiv.org/abs/2010.02141v1) .


\[44\] Stanton, S., Maddox, W.J., Gruver, N., Maffettone, P., Delaney, E., Greenside, P. & Wilson, A.G. (n.d.) Accelerating Bayesian Optimization for Biological Sequence Design with Denoising Autoencoders.


\[45\] Su, J., Han, C., Zhou, Y., Shan, J., Zhou, X. & Yuan, F. (2024) SaProt: Protein Language Modeling with Structure-aware Vocabulary. bioRxiv. 2023.10.01.560349. doi:10.1101/2023.10.01.560349.


\[46\] Su, J., Zhou, X., Zhang, X. & Yuan, F. (2024) ProTrek: Navigating the Protein Universe through Tri-Modal Contrastive Learning. bioRxiv. 2024.05.30.596740. doi:10.1101/2024.05.30.596740.


\[47\] Tripp, A., Daxberger, E. & Miguel Hernández-Lobato, J. (n.d.) Sample-Efficient Optimization in the Latent Space of Deep Generative Models via Weighted Retraining.


\[48\] Truong, T.F., Ny, O.A., Ai, U.T. & Bepler, T. (2023) PoET: A generative model of protein families as sequences-of-sequences. Advances in Neural Information Processing Systems. 36.


[https://arxiv.org/abs/2306.06156v3](https://arxiv.org/abs/2306.06156v3) .


\[49\] Watson, J.L., Juergens, D., Bennett, N.R., Trippe, B.L., Yim, J., et al. (2023) De novo design of protein structure and function with RFdiffusion. Nature 2023 620:7976. 620 (7976), 1089–1100. doi:10.1038/s41586-023-06415-8.


\[50\] Weinstein, E.N., Gollub, M.G., Slabodkin, A., Gardner, C.L., Dobbs, K., Cui, X.-B., Amin, A.N., Church, G.M. & Wood, E.B. (2024) Manufacturing-Aware Generative Model Architectures Enable Biological Sequence Design and Synthesis at Petascale.


\[51\] Widatalla, T., Rafailov, R. & Hie, B. (2024) Aligning protein generative models with experimental fitness via Direct Preference Optimization. bioRxiv. 2024.05.20.595026. doi:10.1101/2024.05.20.595026.


\[52\] Wittmann, B.J., Yue, Y. & Arnold, F.H. (2021) Informed training set design enables efficient machine learning-assisted directed protein evolution. Cell Systems. 12 (11), 1026-1045.e7. doi:10.1016/J.CELS.2021.07.008.


\[53\] Wu, N.C., Dai, L., Olson, C.A., Lloyd-Smith, J.O. & Sun, R. (2016) Adaptation in protein fitness landscapes is facilitated by indirect paths. eLife. 5 (JULY). doi:10.7554/ELIFE.16965.


\[54\] Wu, Z., Jennifer Kan, S.B., Lewis, R.D., Wittmann, B.J. & Arnold, F.H. (2019) Machine learning-assisted directed protein evolution with combinatorial libraries. Proceedings of the National Academy of Sciences of the United States of America. 116 (18), 8852–8858. doi:10.1073/PNAS.1901979116/SUPPL_FILE/PNAS.1901979116.SAPP.PDF.


\[55\] Xu, M., Yuan, X., Miret, S. & Tang, J. (2023) ProtST: Multi-Modality Learning of Protein Sequences and Biomedical Texts. Proceedings of Machine Learning Research. 202, 38749–38767.


[https://arxiv.org/abs/2301.12040v2](https://arxiv.org/abs/2301.12040v2) .


\[56\] Yang, J., Lal, R.G., Bowden, J.C., Astudillo, R., Hameedi, M.A., Kaur, S., Hill, M., Yue, Y. & Arnold, F.H. (2024) Active Learning-Assisted Directed Evolution. bioRxiv. 2024.07.27.605457. doi:10.1101/2024.07.27.605457.


\[57\] Yang, K.K., Wu, Z. & Arnold, F.H. (2019) Machine-learning-guided directed evolution for protein engineering. Nature Methods 2019 16:8. 16 (8), 687–694. doi:10.1038/s41592-019-0496-6.


\[58\] Zambaldi, V., La, D., Chu, A.E., Patani, H., Danson, A.E., et al. (2024) De novo design of high-affinity protein binders with AlphaProteo.


[https://arxiv.org/abs/2409.08022v1](https://arxiv.org/abs/2409.08022v1) .


\[59\] Srinivas, N., Krause, A., Kakade, S. M., & Seeger, M. (2009). Gaussian process optimization in the bandit setting: No regret and experimental design.


*arXiv preprint arXiv:0912.3995* .


[https://arxiv.org/abs/0912.3995](https://arxiv.org/abs/0912.3995)
