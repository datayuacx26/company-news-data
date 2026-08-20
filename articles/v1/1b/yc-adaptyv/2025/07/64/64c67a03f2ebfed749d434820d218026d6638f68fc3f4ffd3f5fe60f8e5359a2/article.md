---
schema_version: "1.0.0"
document_id: "64c67a03f2ebfed749d434820d218026d6638f68fc3f4ffd3f5fe60f8e5359a2"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-rss-523875a8e4a3"
canonical_url: "https://adaptyvbio.substack.com/p/designer-spotlight-protrl-reinforcement"
published_at: "2025-07-29T13:10:06+00:00"
first_seen_at: "2026-07-24T14:21:19.055100+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:737a5bcfddeb69a36793a7206ca91dd5bd61cad75f9c771962d45f172860fce8"
---

# Designer Spotlight: ProtRL - Reinforcement learning and the Move 37 of protein engineering

# Designer Spotlight: ProtRL - Reinforcement learning and the Move 37 of protein engineering


### We’re taking a look at ProtRL: a framework for aligning protein language models to your desired distributions using reinforcement learning. And we test proteins in our lab!


[Adaptyv Bio](https://substack.com/@adaptyvbio)


Jul 29, 2025


> TL;DR
>
>
> -
>
>
> [ProtRL](https://github.com/AI4PDLab/ProtRL) - a framework for
>
>
> **aligning (protein) language models with the distribution or score function you want** using reinforcement learning (RL). You can use it for binders, enzymes, novel folds, and anything you can imagine.
>
>
> -
>
>
> In this blog post, we’ll give you a brief review of reinforcement learning and how it has been applied to protein engineering - we hope this will get you up-to-speed with what ProtRL is doing!
>
>
> -
>
>
> After, Filippo Stocco from the
>
>
> [Ferruz Lab](https://www.aiproteindesign.com/) will be telling us all about ProtRL: why reinforcement learning is critical in protein design, how it works, how he’s currently using it to improve his initial EGFR binders with multiple rounds of experimental validation using
>
>
> [Adaptyv’s binding characterization platform](https://adaptyvbio.com/) , and what’s next for ProtRL.
>
>
> -
>
>
> We’ll finally speculate on some interesting reinforcement learning trivia: the infamous AlphaGo
>
>
> [Move 37](https://en.wikipedia.org/wiki/AlphaGo_versus_Lee_Sedol) - could this be possible in protein engineering? What would this look like? Are we there yet?


### Protein language models are quite limited to their training data


[ProtRL](https://arxiv.org/abs/2412.12979) starts from a simple observation: protein language models (pLMs) capture the distribution of their training dataset. This condition is far to be ideal in protein engineering, where we aim to


[sample high fitness variants](https://www.science.org/doi/10.1126/sciadv.adr7338) . For example, if that dataset is more biased towards highly stable, alpha-helical proteins (such as the PDB), then we should expect the same bias in the model when when we use a pLM for sampling new proteins (this has been a


[known](https://arxiv.org/abs/2202.07206)


[problem](https://arxiv.org/abs/2502.18326) for pre-training). But what if we want to sample


**something that is not well-represented in the training set, maybe a new enzyme** ?


This is where reinforcement learning comes into play: by biasing the model’s learned distribution in a variant of fine-tuning generally called


**“alignment”** , we can increase the likelihood of sampling unlikely events.


Take for example the


[ZymCTRL](https://huggingface.co/AI4PD/ZymCTRL) model: a GPT-like pLM trained to generate new enzyme sequences starting from a simple


[enzyme commission number](https://en.wikipedia.org/wiki/Enzyme_Commission_number) (EC) input in an autoregressive manner. When


[asked to sample 40,000 new carbonic anhydrases](https://arxiv.org/pdf/2412.12979) , the sequences captured the training dataset distribution. We can measure this by looking at metrics such as structural confidence (pLDDT) from


[ESMFold](https://huggingface.co/facebook/esmfold_v1) , the increased proportion of topologies like β (there are at least 5 families of carbonic anhydrases - α, β, γ, δ and ζ), and sequence lengths being closely matched. After aligning to a representative α carbonic anhydrase with the


[TM-score](https://en.wikipedia.org/wiki/Template_modeling_score) as an oracle (measuring structural similarity), ProtRL completely shifted ZymCTRL’s distribution: 95% of the generated sequences had the desired fold by the 6th round of reinforcement learning.


We see that ProtRL can be incredibly powerful for alignment!


Aligning ZymCTRL to generate more carbonic anhydrase (CA) alpha variants with ProtRL.


### But what is reinforcement learning?


In nature, goal-directed behaviours are shaped by rewards and punishments. RL in machine learning (ML) operates in a similar manner: an


**agent** (model) interacts with its


**environment** with a set of


**actions** given its current


**state** , refining its decisions (


**policy** ) over the set of actions based on the feedback it receives (


**reward** ). RL has successfully been applied to a wide range of challenges, from beating human players in


[Go](https://en.wikipedia.org/wiki/AlphaGo_versus_Lee_Sedol) and


[chess](https://arxiv.org/abs/1712.01815) , to being world-class


[video](https://openai.com/index/openai-five-defeats-dota-2-world-champions/)


[game](https://deepmind.google/discover/blog/alphastar-grandmaster-level-in-starcraft-ii-using-multi-agent-reinforcement-learning/) competitors.


For a deep dive into RL concepts, we recommend this


[OpenAI resource](https://spinningup.openai.com/en/latest/spinningup/rl_intro.html#) .


More recently, Reinforcement Learning has successfully been applied to Large Language Models (LLMs), following Human Feedback (RLHF). If you are familiar with Gemini, ChatGPT, Claude, or DeepSeek, you have experienced the power of RLHF first-hand! For instance, users can vote on


[ChatGPT responses](https://openai.com/index/instruction-following/) , and sometimes the model presents them with two different options from which they can select the one that better aligns with their expectations. User feedback is then used to make the model more performant.


[ProtRL](https://github.com/AI4PDLab/ProtRL) is a framework where RL algorithms are implemented to be easily applied to any biological language model, now focusing on autoregressive pLMs like ZymCTRL. Currently, weighted directed preference optimization (wDPO) and group relative policy optimization (GRPO) are implemented, which we will explain in more detail later!


Glossary of key RL terms: Action, Policy, Agent, Reward, and Environment.


### The landscape of protein RL


In the last years, RL innovations have also flourished in the protein engineering field. Broadly speaking, current efforts for protein RL can be abstracted into two main approaches:


1.


**Planning-based RL** (seach-centric), which leverage a search algorithm (for example


[Monte Carlo Three Search](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search) ) to explore the space of possible actions at each state. In this case, the possible actions are usually discrete. For instance, we can consider introducing a mutation in a sequence. The possible action space is defined as 20, the number of possible amino acids.


2.


**Policy-based RL** (generative-centric), which learns the best way to explore the protein space by explicitly learning a policy (e.g. LLM) that maps from states → to optimal actions. This actions can be then queried directly to pick the best actions for each of the states, without passing through a search. This paradigm is the current hallmark for large scale modern generative AI.


We’ll briefly describe these and point you to some relevant papers.


The two main RL for proteins categories and model examples.


### Search-centric RL


In the search centric RL, the main objective is explore the space applying a search algorithm and applying a discrete set of possible actions in a very vast, yet constrained, space. For example, with the game of Pac-Man, we can decide between 4 different actions: up, down, left, and right. From this set of actions, we can explore the space applying a search algorithm and find the best combination of actions that will result in the higher outcome (e.g final score).


To better understand this, let's directly jump in one example of these approaches in protein design, that yet can generalize to other trends in RL.


[Monte Carlo Tree Search (MCTS)](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search) has been popularized by


[AlphaGo](https://en.wikipedia.org/wiki/AlphaGo) and


[AlphaZero](https://en.wikipedia.org/wiki/AlphaZero) to guide exploration (although strictly speaking they result in a kind of hybrid approach between the two categories here described). In this framework, the protein is modelled as a discrete sequence with each residue position as a potential mutation site. The policy network assigns probabilities for single-site changes across the sequence. Mutations are made and each of these "actions" spawns a new candidate sequence, and the search tree branches as we iteratively apply further mutations. Finally, each branch is evaluated to identify the mutation paths yielding the highest functional performance. This strategy was applied in


[EvoPlay](https://www.nature.com/articles/s42256-023-00691-9) , where the model "takes actions" generating several of these mutate-score trajectories to improve both its policy and value network (representing the mutation’s effect on fitness). As a result, EvoPlay generates luciferase mutants with 7.8x higher luminescence than the wild-type.


The agent can also operate on larger sections of a protein: for example, it can propose new


[secondary motifs](https://arxiv.org/html/2405.01983v1#:~:text=structural%20scores,nuanced%20aspects%20of%20protein%20design) (its domain being the protein backbone rather than the sequence) or monomers that should correctly form a


[nanocage](https://www.science.org/doi/10.1126/science.adf6591) . These still fit the search-centric RL category: the algorithm explores an exhaustive set of possible combinations, gets rewards for these, and can learn from the trajectories (


[self-play](https://huggingface.co/learn/deep-rl-course/en/unit7/self-play) ).


### Generative-centric RL


Generative-centric RL is founded on a base generative model, which can be fine-tuned using reward or preference data to update model's parameters (θ).


Very recently, the field of research on aligning models with reinforcement learning algorithms has gained significant traction. This trend is particularly evident in the recent success of applying this technique to generative LLMs. Algorithms such as


[proximal policy optimization](https://arxiv.org/abs/1707.06347) (PPO),


[direct preference optimization](https://arxiv.org/abs/2305.18290) (DPO), and


[group relative policy optimization](https://arxiv.org/pdf/2402.03300) (GRPO) have emerged as powerful tools for aligning models to generate outputs that


[adhere to human preferences](https://www.tylerromero.com/posts/2024-04-dpo/) (e.g., generate correct answers to our questions, avoid toxic words), modelled as a


[preference probability](https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model) . This field experienced remarkable success, particularly considering the widespread adoption of such technologies by the general public, emerging from research and academic circles, making it even more important to ensure an ethical alignment for these powerful tools.


### PPO, DPO, and GRPO explained


One of the first


[policy gradient methods](https://en.wikipedia.org/wiki/Policy_gradient_method) in RL is the


**REINFORCE algorithm** . In REINFORCE, the agents interacts with the environments and the policy is updated directly by multiplying the reward. Intuitively, the higher the rewards of that specific action, the more likely that action will be in the updated model.


In the case of LLMs and


***PPO*** , a


**policy model** generates a set of candidate sentences, which are then


**scored.** The score itself represents a value that a human annotator would assign to a given output. However, directly using humans to score every model output is extremely costly and impractical. To address this, a


**reward model** is first trained on a dataset of human-labeled examples. Once trained, this model can automatically evaluate and assign scores to new outputs, mimicking human judgment.


In addition to the reward model, a


**value model** is trained, which has the role to predict the long term cumulative reward. Essentially, it estimates how good a partially completed output is, considering the future potential of the full sentence.For example, if we're training a model to talk about animals, the sentence


*"In the savanna"* would be assigned a higher value than


*"In the pub"* . This is because


*"In the savanna"* is more likely to lead to future content about animals, resulting in a higher expected cumulative reward.


This method helps the system optimize for


**long-term success** , not just immediate reward, which reduces the risk of the model getting stuck in local optima. Finally, the reward and value outputs are combined to compute the


**advantage** , which is used to update and improve the policy model (e.g LLM).


captioPPO training overview: step 1 involves training a reward model on human preference data, then used in step 2 for LLM alignment.n...


The need to train a reward model and a value model on top of the policy model makes PPO quite challenging to be applied to large scale alignments. To alleviate these issues,


***DPO*** allows the alignment of LLMs without explicitly learning and approximating a reward function, by learning directly from a preference dataset. In other words, compared to training a reward model where we need to connect the "


*inner knowledge of the model to a scalar value with limited data (compared to the training), with DPO we can directly tune the implicit reward* ", resulting thus in a potentially better approximation of the reward function (see this


[thread](https://x.com/rm_rafailov/status/1729208972476059785) discussing the differences between DPO and PPO). DPO shifts the “preference burden” from the reward model to the training set, making the preferences itself align the model directly. This is a


[great resource](https://www.tylerromero.com/posts/2024-04-dpo/) if you want to understand the mathematical derivation from classic RLHF to DPO.


There is also some debate


[if DPO is RL or not](https://arxiv.org/abs/2502.03095) but we leave that debate to the philosophers.


More recently,


***GRPO*** has gained great popularity. GRPO makes PPO more efficient by removing the


**value** model and simply drawing multiple samples from its policy, then applying for each token the same advantage - computed by subtracting the sequence reward to the group mean reward and normalizing for the standard distribution of the group. Compared to DPO, GRPO provides greater flexibility making it potentially more adaptive to any type of distribution of the dataset - not just preference data.


Recent protein engineering applications of RL, including ProtRL, have been motivated by the success of PPO, DPO, and GRPO. For example, one of the earlier works -


[ProteinDPO](https://www.biorxiv.org/content/10.1101/2024.05.20.595026v1) - trained an inverse folding model (


[ESM-IF](https://www.biorxiv.org/content/10.1101/2022.04.10.487779v2) ) with a DPO objective on pairs of proteins with high and low thermostability. This yielded improved correlations between the log-likelihoods and stability - thus shifting the generative model’s distribution to a higher stability one. Other works applied DPO for redesigning sequences with


[lower immunogenicity](https://academic.oup.com/peds/article/doi/10.1093/protein/gzaf003/8082933?login=true) ,


[better designability](https://arxiv.org/html/2506.00297v1) (as AlphaFold2 confidence), or


[more diverse peptides](https://openreview.net/forum?id=VY96NfQRIo) . PPO has been applied in the


[RLXF](https://www.biorxiv.org/content/10.1101/2025.05.02.651993v1.full.pdf) framework (reinforcement learning from experimental feedback) where experimental data and the PPO objective are used to align the


[ESM2](https://www.science.org/doi/10.1126/science.ade2574) pLM to high fitness regions. It produced multiple variants of a fluorescent protein (CreiLOV), with one variant outperforming the best-known variant until then (1.7-fold improvement over wild-type versus 1.2). Other generative-centric RL models applied to protein engineering include:


[ProteinRL](https://openreview.net/forum?id=sWCsSKqkXa&utm_source=chatgpt.com) ,


[LatProtRL](https://arxiv.org/abs/2405.18986) ,


[ProteinZero](https://arxiv.org/abs/2506.07459) . Recent works have also been aligning discrete diffusion models instead of pLMs with an RL objective:


[RL-DIF](https://arxiv.org/pdf/2410.17173) for inverse folding,


[DRAKES](https://arxiv.org/abs/2410.13643) ,


[VIDD](https://arxiv.org/pdf/2507.00445) .


Overview of the main RL algorithms: proximal policy optimization (PPO), group relative policy optimization (GRPO), and direct preference optimization (DPO). Figure adapted from


[here](https://arxiv.org/pdf/2402.03300)


### ProtRL : align your auto-regressive protein language mode


To explore the power of reinforcement learning in steering pLMs toward functional phenotypes, Filippo


*et al.* developed ProtRL: a general framework that iteratively refines any


[decoder-only](https://huggingface.co/learn/llm-course/en/chapter1/6) pLM against an arbitrary oracle. In each ProtRL iteration, the authors sample a batch of candidate sequences (e.g. 200), score them with their chosen oracle, and feed back the resulting phenotype–genotype pairs as reward signals to update the model.


In a progressively more complex tasks, they used ProtRL to align the model to generate a specific fold of carbonic anhydrase with higher probability than another one (beta to alpha). Over just six iterations, more than 95 % of generated sequences adopted the intended α-topology. Filippo


*et al.* have also applied it to many other bounded and unbounded scores such as


[ESM1v](https://www.biorxiv.org/content/10.1101/2021.07.09.450648v2) ,


[predicted enzymatic activity](https://www.science.org/doi/10.1126/science.adf2465) , and


[ProteinMPNN](https://www.science.org/doi/10.1126/science.add2187) .


Yet, the most informative and real trustworthy oracle is the real-world itself. To demonstrate a full lab-in-the-loop campaign, Filippo


*et al.* turned to binder design against the


[Epidermal Growth Factor Receptor](https://www.uniprot.org/uniprotkb/P00533/entry) (EGFR). They first fine-tuned ZymCTRL on 600 EGFR-related sequences (BLAST‐retrieved with wild-type


[EGF](https://www.uniprot.org/uniprotkb/Q6QBS2/entry) ), generating 10,000 candidates and filtering them by


[perplexity](https://huggingface.co/docs/transformers/en/perplexity) and length (FT method). Six designs were tested experimentally in round 1 (coinciding with round 2 of the


[Protein Design Competition](https://foundry.adaptyvbio.com/competition) ), yielding three binders with affinities of 328–819 nM. They then applied DPO (RL method), using measured Kd, sequence length, TM-score, and expression level as reward components. In round 2, tested again with the


[Adaptyv platform](https://adaptyvbio.com/) , four of nine tested sequences outperformed the best round 1 binder, including a top candidate with Kd = 27.4 nM. Remarkably, diversity naturally increases across rounds, with mean sequence identity dropping from 88.6% → 73.1%. Experimental results are summarized in the figure below.


EGFR binders submitted to the Protein Design Competition using a fine-tuning (FT) method (blue) versus a subsequent ProtRL campaign (purple). Whereas 3 initial FT designs were binders, the RL campaign yielded 9/20 binders (45% hit-rate). The highest affinity binder was achieved with the RL method, at a K_D of 27.4 nM (see structure of the binder RL_01 bound to EGFR).


ProtRL combines several powerful advantages in a single, seamless workflow: it can be driven entirely by synthetic data, needs only a handful of reinforcement learning iterations to deliver large fitness gains, and anything can serve as the feedback signal. Given these properties, ProtRL can be used to guide unconditional pLMs to explore vast, high-fitness regions of the sequence space while tailoring the sequences toward a property, producing entirely


*de novo* proteins (such as


[BindCraft](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v3) ). In this scenario, the pLM is aligned to exhaustively sample vast, high fitness regions of the protein space, while meeting the reward function’s criteria. This paradigm has the potential to generate


*de novo* binders or enzymes, all while avoiding intellectual property constraints.


### Limitations


ProtRL and, in general, RL for protein design face several key challenges. First, reward hacking is pervasive: when the model over-optimizes a poorly specified reward, it can drift toward artifacts that score well but lack real biological function. Designing robust reward functions becomes therefore the most critical component in every RL campaign and requires many passes of hyper-parameter engineering.


[Here](https://diffuse.one/p/m1-000) is an interesting blog post about this. Second, oracles (whether


*in silico* predictors or wet-lab assays) are imperfect, and in the second case, often expensive to run. Training or integrating multiple orthogonal predictors (e.g. stability, expression, off-target effects) can increase computational and experimental cost, limiting throughput. Third, real biological systems are high-dimensional and context-dependent, far more complex than the closed environments of games. Generalizing across folds, functions, and host contexts remains an open problem. That said,


**we remain optimistic!** There are many exciting opportunities to be explored in the field, that are advancing at breakneck pace!


### Going further: Move 37 in protein design?


` Are we still in Plato’s (pLM) cave?`


Move 37 is a famous move played by AlphaGo against the Go champion Lee Sedol in March 2016 (Netflix also made a


[documentary](https://www.netflix.com/ch-en/title/80190844) about this!). This move represented a play so non-intuitive that it left even one of the best players in the world speechless and, in the end, was a winning move. This move was described as an emergent phenomenon from RL training. AlphaGo was trained so that the model was able to self-play countless games and compute the score for each play, thus the set of decisions was made based on the outcomes of the match (winning or losing).


RLHF has revolutionized NLP by aligning language models to human-annotated data. Yet, the nuances and the underlying workings of the biological “grammar” are quite challenging to grasp. For this, relying on human annotation in pLM is not feasible. Additionally, aligning outputs with existing human knowledge risks confining generative models to familiar evolutionary motifs or research bias/trends rather than pioneering truly novel solutions.


In a recent


[substack](https://sergeylevine.substack.com/p/language-models-in-platos-cave) , Prof. Levine describes LLMs as projections of human capabilities, distilled from the processing of the enormous amount of information of data present on the internet. Yet these models are still far from achieving the generality and flexibility of human learning processes, which can extrapolate and connect multiple subjects and concepts. In other words, LLMs have just learned to mimic our behaviour without implementing/exploiting the underlying learning processes we use. In a way, LLMs are constrained to a world of shadows, exactly how Plato, in his


[Allegory of the Cave](https://en.wikipedia.org/wiki/Allegory_of_the_cave) , described humans able to see only the mere shadows of the "


*real world"* , chained in the cave of the ignorance. For this reason, according to Prof. Levine, AI systems will not acquire the flexibility and adaptability of human intelligence until they can actually


*learn* like humans do, shining with their


*own light* rather than observing a


*shadow* from ours, and finally escape from the Plato’s Cave.


RL, especially in protein design, can play a different role compared to the NLP field, as each generation can be assessed and scored, things that we cannot easily do with language (how can we computationally rank and compare Virginia Woolf with Cervantes?). For this reason, as interestingly pointed out by


[Max Jaderberg in an interview](https://www.sequoiacap.com/podcast/training-data-max-jaderberg/) **,** protein design problems may be seen more from an AlphaGo point-of-view rather than the ChatGPT one, where the model can autonomously learn the best policies / moves to achieve a determined objective, unconstrained by our knowledge.


Move 37 explained.


But what would Move 37 look like in biology? Just as AlphaGo’s Move 37 emerged as a counterintuitive, unexpected strategy, a true biological Move 37 will be a mutation or fold no expert anticipated yet one that increases functional performance. For a protein engineering campaign, it could be a mutation that drastically reduces fitness for several rounds, until it yields a way better variant (e.g., the model anticipating a “fitness valley” that needs to be traversed to reach a peak). Translating this paradigm to proteins will require hybrid architectures integrating pLMs with reward functions that explicitly value novelty (there are some works to robustly assess


[protein](https://arxiv.org/abs/2505.08041)


[novelty](https://arxiv.org/abs/2410.17173) ), and high-throughput testing or assemblies of oracles to navigate vast mutation landscapes, paving the way for real unconstrained exploration of the space and discovering the first Move 37 in protein design. Simply put,


**a Move 37 needs more search / exploration** - AlphaGo achieved that through self-play in a constrained environment with a clear reward feedback. For biology, we either need better experimentally-aligned oracles (to create a constrained environment similar to AlphaGo’s), more experimental testing (to search and validate), or better methods to enforce diversity, novelty, and going “out-of-distribution”. Or all of the above!


### Conclusion


ProtRL tries to lay the groundwork for iterative, oracle-driven protein design, but the ultimate frontier lies in discovering “blind spots” where neither nature nor existing models nor experiments have ventured. By combining synthetic datapoints, wet-lab feedback, and autonomous learning,


**we would like to get close to what Move 37 was for Go: a design leap so unexpected it rewrites our understanding of what proteins can do!**


### Resources and links


-


Try out ProtRL


[here](https://github.com/AI4PDLab/ProtRL) and read the preprint


[here](https://arxiv.org/pdf/2412.12979) .


-


Check out what the


[Ferruz Lab](https://www.aiproteindesign.com/) is working on!


-


Say hi to Filippo:


[X](https://x.com/Filippo_Stocco_) ,


[LinkedIn](https://www.linkedin.com/in/stoccofilippo/) .


-


**Have some novel proteins you want to test in the lab? Come talk to us** — we’d like to run many more of these protein designer spotlights, so if you have a cool new hypothesis or model to test we’d love to hear from you!


### Acknowledgments


We are grateful for the invaluable feedback and insightful discussions provided by


[Noelia Ferruz](https://x.com/ferruz_noelia) and


[Michele Garibbo](https://michele1993.github.io/) .
