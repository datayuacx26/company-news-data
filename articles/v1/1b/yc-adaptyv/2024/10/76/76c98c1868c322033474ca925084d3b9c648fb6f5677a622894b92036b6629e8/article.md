---
schema_version: "1.0.0"
document_id: "76c98c1868c322033474ca925084d3b9c648fb6f5677a622894b92036b6629e8"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-rss-523875a8e4a3"
canonical_url: "https://adaptyvbio.substack.com/p/protein-optimization-102-lessons"
published_at: "2024-10-18T16:56:10+00:00"
first_seen_at: "2026-07-24T14:21:19.055100+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:14154b8baa5c777284795862395e3b1f7afd35ac18ed5b59942b4b8fc3c8646f"
---

# Protein Optimization 102: Lessons from the protein design competition

# Protein Optimization 102: Lessons from the protein design competition


### In this blog post, we analyze the most common model and design choices, take a closer look at the strategies that yielded successful binders, and provide recommendations for your future designs


[Adaptyv Bio](https://substack.com/@adaptyvbio)


Oct 18, 2024


> TL;DR
>
>
> -
>
>
> We analyzed the submissions for the first EGFR competition (results
>
>
> [here](https://foundry.adaptyvbio.com/egfr_design_competition) ) for you!
>
>
> -
>
>
> Most people explored RFdiffusion for backbone generation, ProteinMPNN for obtaining sequences, with some other interesting approaches we’ve highlighted.
>
>
> -
>
>
> **Winning combination:** AlphaFold2 hallucination and SolubleMPNN inverse-folding.
>
>
> -
>
>
> We’ve provided actionable
>
>
> [recommendations](https://www.adaptyvbio.com/blog/po102#:~:text=Our%20recommendations) on design algorithms based on our
>
>
> [previous literature review](https://www.adaptyvbio.com/blog/po101) and the learnings from our competition. You can find out which is the right method for you with a protein engineer’s “
>
>
> [personality test](https://www.adaptyvbio.com/blog/po102#:~:text=The%20protein%20engineer%E2%80%99s%20personality%20test) ”.
>
>
> -
>
>
> We also include some domain knowledge-derived tips that should help participants with a purely computational background to avoid some pitfalls when it comes to the in-silico → in-vitro domain shift


In our


[last blog](https://www.adaptyvbio.com/blog/po101) post, we looked at the different ways you can optimize a protein for a given task using machine learning models.


We made the distinction between


**[fixed](https://www.adaptyvbio.com/blog/po101#:~:text=Supplementary%20-%20Fixed%20model%20optimization)** and


**[sequential](https://www.adaptyvbio.com/blog/po101#:~:text=sequentially-optimized%20models)** model optimization and further split the latter into


**[model-based sampling](https://www.adaptyvbio.com/blog/po101#:~:text=Model-based%20adaptive%20sampling:%20Bayesian%20Optimization%20or%20Active%20Learning?)** and


**[greedy/heuristic](https://www.adaptyvbio.com/blog/po101#:~:text=Model-based%20versus%20heuristic%20sampling)** methods. Model-based methods use a learned…well…


*model* to explicitly sample novel candidates. This model tries to balance between exploitation and exploration using the


*knowledge* that it incorporates from observations via training. Greedy/heuristic methods often use the fitness model to rank randomly proposed candidates, but generally don’t adapt to the observations as much as model-based methods.


In the first half of this post, we will use this taxonomy to take a look at the methods employed by designers in the first round of our EGFR competition. Different approaches come with different theoretical trade-offs, and we will see whether we can see those in the submissions’ practical performance.


In the second half, we will give you some actionable advice on how to choose an approach for the


[second round](https://design.adaptyvbio.com/) , summarized with two handy decision charts.


### **Overview of Adaptyv’s EGFR binder design competition**


Competition overview and winners.


In our competition, creative protein engineers were tasked with designing a new binder to the extracellular domain of EGFR, a cancer-associated drug target.


We selected 200(+1) of the most promising sequences for screening. The top 100 were based on the AlphaFold2 interface pAE as a proxy for binding (


[Bennett et al., 2023](https://www.nature.com/articles/s41467-023-38328-5) ), whereas the rest were selected across a wide range of design techniques. AlphaFold2 iPAE as a binding metric has its problems, which we will briefly discuss, but we chose it because it has been exhaustively used in the communities, including luminaries such as the Baker lab (


[Bennett et al., 2023](https://www.nature.com/articles/s41467-023-38328-5#Fig1) ;


[Watson et al., 2023](https://www.nature.com/articles/s41586-023-06415-8) ;


[Zhang et al., 2024](https://www.biorxiv.org/content/10.1101/2024.08.29.610300v3) ). Most successful designs (in terms of leaderboard placement) aimed to directly optimize this objective. For a brief overview of protein design competitions, check out this


[Nature article](https://www.nature.com/articles/d41586-024-03335-z) we have been featured in!


### Design and model categories


We had requested competitors to include a brief description of their submissions’ strategy. Some wrote very brief stubs (which we still thank them for!), and some wrote detailed descriptions and blog posts (you guys are awesome!).


The figure below shows the diversity of techniques designers used, categorized into certain themes by us.


You can browse through all the categorized (tested) designs


[here](https://foundry.adaptyvbio.com/egfr_design_competition) .


Number of designs submitted and selected in the first round of the EGFR competition by design and model category.


The most popular options included:


-


sampling from a generative model like RFdiffusion (


[Watson et al., 2023](https://www.nature.com/articles/s41586-023-06415-8) ), sometimes conditioned on EGFR’s structure, followed by inverse-folding with ProteinMPNN (


[Dauparas et al., 2022](https://www.science.org/doi/10.1126/science.add2187) ), then using AlphaFold’s predicted metrics or others for filtering (


**de novo + filter** ). We called this playing the “


*de novo* slot machine” in our


[previous post](https://www.adaptyvbio.com/blog/po101) . In a typical experimental setting, these approaches can be prone to relatively low hit rates, although recent models have made great leaps to address this (e.g., Google DeepMind’s AlphaProteo,


[Zambaldi et al., 2024](https://arxiv.org/abs/2409.08022) , which has a 24.5% hit rate for interleukin-7 receptor-𝛼 binders).


-


diversifying known binders from the literature with ProteinMPNN (


[Dauparas et al., 2022](https://www.science.org/doi/10.1126/science.add2187) ) and subsequent selection after re-folding with AlphaFold and computing selection metrics (


**diversified binder + filter** ). This category includes partial diffusion of a known binder with RFdiffusion. In our last post’s taxonomy, this would be a fixed model optimization or local search strategy. Other designers chose a rational approach (


[Korendovych, 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5912912/) ) for diversifying: starting from known sequences (such as EGF), they mutated regions (often not in the binding site!) using methods such as BLOSUM62 substitutions (


[Eddy, 2004](https://www.nature.com/articles/nbt0804-1035) ). Diversified binder + filter was the most common design strategy.


-


gradient-based input space optimization via AlphaFold2 (aka


**hallucination)** (


[Anishchenko et al., 2021](https://www.nature.com/articles/s41586-021-04184-w) ;


[Goverde et al., 2023](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10204179/) ;


[Frank et al., 2023](https://www.biorxiv.org/content/10.1101/2023.02.24.529906v1.abstract) ). For this, a random sequence is fed through AlphaFold2, then a custom loss is computed (often including terms like pLDDT, pAE, and other constraints) and its gradient with regards to the input is taken by backpropagating through the folding network. Several iterations of input optimization can achieve a structurally stable design. Feeding a random sequence and its desired binding target through AlphaFold-Multimer enables the design of binders, with losses that can take into account the interaction pLDDT and pAE, the number of interface residues, etc.


-


directly optimizing a starting sequence using


[unsupervised protein languge model directed evolution](https://www.adaptyvbio.com/blog/po101#:~:text=unsupervised%20evolution) (fixed model optimization), and building custom active learning loops (sequential model optimization with greedy/heuristic sampling). We included both in the


**optimized binder** category. This is very similar to input space optimization


*which hallucination is also a part of, yet we decided it should have its separate category to highlight its [“resurgence” in protein design](https://x.com/DdelAlamo/status/1837216020836278540) , as we will see from our winners.*


-


strategies that mainly employed molecular dynamics, docking, or Rosetta design protocols were labelled as


**physics-based** . Most of these still filtered the final candidates using AlphaFold2’s iPAE to ensure they made it on the leaderboard.


Submissions with missing or unclear descriptions are labelled as “not mentioned” and excluded from this analysis (398 out of 726 total submissions, with 65 of these being selected for validation).


Looking more in-depth at the models represented, we found that RFdiffusion for backbone design followed by ProteinMPNN diversification and filtering is by far the most common strategy (176 total submissions).


Here RFdiffusion is used either for target-conditioned generation or partial diffusion/scaffolding of known binder motifs. This is a completely valid choice given the iPAE objective and lack of fully characterized EGFR binder datasets: people opted to sample a large number of potential designs and simply select the top candidates. But, as Brian Naughton


[reports](http://blog.booleanbiotech.com/adaptyv-binder-design-competition.html) , it is often hard to get both RFdiffusion and ProteinMPNN to reliably sample high-confidence designs. For validation, this would entail screening massive designed libraries and it might not be the best choice if you’re working with a limited experimental budget.


### Expression rates


Only 146 sequences (excluding the Cetuximab control) expressed, leaving some people surprised and some disappointed. This is the reality of translating


*in silico* designs into experimentally-valid candidates. A 73% (146/201) expression rate is about on par with


[Wicky and colleagues](https://www.science.org/doi/10.1126/science.add1964) , who achieved a 74% (71/96) expression rate for their AlphaFold2 hallucinated + ProteinMPNN-designed symmetric assemblies.


[Goverde and colleagues](https://www.nature.com/articles/s41586-024-07601-y) compared the designs obtained from the standard ProteinMPNN for an AlphaFold2-hallucinated backbone (with the


[AF2Seq](https://github.com/bene837/af2seq) pipeline) with biasing ProteinMPNN’s sampling towards hydrophilic amino acids, and with their fine-tuned version on soluble proteins (SolubleMPNN): for a single


[redesigned protein](https://www.rcsb.org/structure/6ffi) , normal ProteinMPNN has a 0% expression rate (0/12), the biased version 75% (6/8), and SolubleMPNN 93.1% (27/29). Recently, a paper from the Baker lab (


[Glögl et al., 2024](https://www.biorxiv.org/content/10.1101/2024.09.13.612773v1) ) achieved a 98% (94/96) expression rate for TNFR1 binders.


With this context, 73% could be considered on the lower side


*compared to the [Baker](https://www.nobelprize.org/prizes/chemistry/2024/baker/facts/) lab* . However, only half of the participants we could verify via their socials had protein design experience (defined as PhD/postdoc/professor or working in a therapeutics biotech). This combined with the wide range of methods employed and the fact that not many people optimized for expression, only binding/iPAE, makes us at Adaptyv relatively happy with the outcome.


**For the second round, [we are including an expression proxy in the metrics](https://x.com/adaptyvbio/status/1846623958646312978) . We also recommend using SolubleMPNN as a final check before submitting your sequences!**


### Hit rates


Out of these, 5 were considered strong binders, with a KD


*KD* ​ between 3e-8 M to 2.3e-5 M, with 2 labelled as weak binders (KD


*KD* ​ above 1e-5 M). This yields a 2.5% total hit rate (5/201), considerably higher than past EGFR-targeting design campaigns (0.01% previously reported using a Rosetta protocol,


[Cao et al., 2022](https://www.nature.com/articles/s41586-022-04654-9) ). We should emphasize that hit rates are highly dependent on the design target, with highly accessible, hydrophobic (


[Cao et al., 2022](https://www.nature.com/articles/s41586-022-04654-9) ;


[Pacesa et al., 2024](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v1) ), and reduced flexibility epitope regions (


[Kim, Choi & Kim, 2021](https://pubs.acs.org/doi/abs/10.1021/acs.jcim.0c01397) ) often yielding better results. To ensure you get a lot of binders,


**we recommend you target hydrophobic surface regions or the EGFR [epitope region we provided](https://design.adaptyvbio.com/)** , especially if you want to succeed in the EGF neutralization assay we are doing!


### iPAE as a proxy for KD


*KD* ​


Seven binders (3 true binders, with 2 disqualified due to similarity to other therapeutics, and 2 weak) are too few data points to test the quality of iPAE as a competition proxy. Brian Naughton recently looked at 55 entries from PDBBind in his


[blog post](http://blog.booleanbiotech.com/adaptyv-binder-design-competition.html) , showing a Pearson’s correlation coefficient of -0.25 (p-value << 0.05). iPAE might not be an ideal score for binding affinity.


We recently


[asked for input](https://x.com/adaptyvbio/status/1844456050726174751) from the protein design community regarding which metric should be used as a binding affinity proxy. This sparked quite an interesting conversation:


-


Nikhil Haas conducted an extensive analysis of the competition sequences and other data ordered from Adaptyv, looking into expression and binding correlations. He showed that ESM2 log-likelihoods correlate well with expression. You can watch his video explanation


[here](https://x.com/NikhilHaas/status/1844446275967779284) .


-


[Martin Pacesa](https://x.com/MartinPacesa/status/1844631283005108452) says ipTM could be used for binary binding predictions.


-


[Sergey Ovchinnikov](https://x.com/sokrypton/status/1844761659505594584) recommends using no filters for the ranking process or, at best, looking at iPAE from AlphaFold2’s predictions with initial guess and also selecting designs that did not pass the filters.


We hope to see


**more filtering and optimization proxies suggested and experimented with in the second round** !


### The champion binders


**First place 🥇- [Bindcraft](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v1) : [Martin Pacesa](https://x.com/MartinPacesa) and [Lennart Nickel](https://de.linkedin.com/in/lennart-nickel-aba84a203)**


Our undisputed winners are


[Martin Pacesa](https://x.com/MartinPacesa) and


[Lennart Nickel](https://de.linkedin.com/in/lennart-nickel-aba84a203) , with a 4.91e-7 M KD


*KD* ​. This was achieved with their custom AlphaFold2 hallucination pipeline called BindCraft (


[Pacesa et al., 2024](https://colab.research.google.com/github/martinpacesa/BindCraft/blob/main/notebooks/BindCraft.ipynb) ). As opposed to others, it hallucinates the binding interface instead of the binder’s structure only. It uses four stages of optimization: first gradient-based optimization on the continuous sequence logits, then on the softmax matrix, followed by one-hot encoding without and with randomly sampled mutations. It is also highly customizable via the combined loss function: some terms aim to optimize the binding interface’s prediction confidence, the number of residues, as well as a radius of gyration for the binder (to prevent “spaghetti”-like long binding stretches) and a “helicity” loss (as AlphaFold2 hallucination is biased towards helices, this promotes more non-helical designs).


You can try it out


[here](https://colab.research.google.com/github/martinpacesa/BindCraft/blob/main/notebooks/BindCraft.ipynb) using Google Colab. As the authors pointed out, it is super straightforward to adapt it to your own design campaign and implement additional loss terms (


[see here](https://github.com/martinpacesa/BindCraft/blob/main/functions/colabdesign_utils.py#L369) !).


**For the second round, give BindCraft a try!**


**Second place 🥈 - [Khondamir Rustamov](https://x.com/KhRRustamov)**


The second spot was taken by a binder designed using the


[AF2Seq](https://github.com/bene837/af2seq) protocol for backbone hallucination and SolubleMPNN for inverse-folding, as initially explored by


[Goverde and colleagues](https://www.nature.com/articles/s41586-024-07601-y) (see the section on expression rates). It had a KD


*KD* ​ of 4.77e-6 M and ranked 54th for iPAE.


**We can now see a common theme: hallucinated backbones with AlphaFold2, followed by SolubleMPNN inverse-folding!**


The methods we have seens so far indirectly optimized multiple objectives and their designs are likely on the


[Pareto-optimal front](https://en.wikipedia.org/wiki/Pareto_front) between experimental binding, iPAE, and expression.


**Third place 🥉- [Adrian Tripp](https://www.linkedin.com/in/adrian-tripp-9ab231294/) and [Sigrid Kaltenbrunner](https://www.linkedin.com/in/sigrid-kaltenbrunnner/)**


This binder had a KD


*KD* ​ of 2.29e-5 M, ranked 89th in the iPAE leaderboard, and had a high expression rate. Sigrid and Adrian used the


[ProtFlow pipeline](https://github.com/mabr3112/ProtFlow) (not to be confused with Adaptyv Bio’s


[ProteinFlow](https://github.com/adaptyvbio/ProteinFlow) - which you should check out for


[processing protein structures](https://www.adaptyvbio.com/blog/proteinflow) !) to orchestrate their complex workflow. First, RFdiffusion tackled the binder backbone generation, targeting EGFR’s residues 18, 39, 41, 108 and 131, followed by initial filtering, then


[LigandMPNN](https://github.com/dauparas/LigandMPNN) inverse-folding on Rosetta-relaxed structures, and sequential folding and filtering with


[ESMFold](https://huggingface.co/facebook/esmfold_v1) and


[ColabFold](https://github.com/sokrypton/ColabFold) . The ESMFold step accounted only for the binder structures and selected for pLDDT+TMscore, whereas the ColabFold step predicted the entire complex and filtered based on pLDDT, TMscore, iPAE, ipTM, and the number of hotspot contacts. The resulting designs were once again fed through the entire pipeline, for 3 cycles in total. We can see how they took the


*de novo* design + filter approach to its limit!


### Special mentions


Some approaches were not as successful, but were extensively documented and so deserve being highlighted as well.


In his


[blog post](http://blog.booleanbiotech.com/adaptyv-binder-design-competition.html) , Brian Naughton tried out a couple of methods, including ESM2 directed evolution with an iPAE oracle, RFdiffusion, ProteinMPNN, and Bayesian optimization. He provides the Modal commands for several of these tools


[here](https://github.com/hgbrian/biomodals) and you should definitely check this out. It is a great way to get started in protein design. And you can make use of your $30 free Modal credits as well!!


[Anthony Gitter](https://x.com/anthonygitter/status/1827760228122738689) found that a


*de novo* , language-instructed binder from ProTrek (


[Su et al., 2024](https://www.biorxiv.org/content/10.1101/2024.05.30.596740v2) ) performed quite well, despite not ranking in the top 100. He tested the “non-biological/domain unadapted” language model Llama 3.1’s ability to design proteins, which still suggested


[antibody-like EGFR inhibitors](https://x.com/anthonygitter/status/1827760253351723390) . We are pretty hopeful for what language-instructed, chat-based protein design might look like in the future!


Alex Naka’s


[strategy](https://x.com/gottapatchemall/status/1827386015713260019) was the only one that fits the definition of sequentially-optimized, model-based sampling we established in the


[last blog post](https://www.adaptyvbio.com/blog/po101) . For his


*in silico* oracle, he first opted for a


[simple AlphaFold2 implementation using Modal](https://github.com/whitead/minimalaf) with


[PepMLM](https://github.com/programmablebio/pepmlm) (


[Chen et al., 2023](https://arxiv.org/abs/2310.03842) ) as his EGFR-conditioned sequence generator. He trained a surrogate (an ensemble of 1D CNNs on one-hot encodings) on a starting dataset mined from this in-silico oracle, then continued training it during optimization. He used


[EvoProtGrad](https://github.com/NREL/EvoProtGrad) (


[Emami et al., 2023](https://iopscience.iop.org/article/10.1088/2632-2153/accacd) ) as his surrogate-conditioned generator, iterating between scoring new candidates and then retraining. A complete active learning loop! Now we know why all his designs were at the top of the leaderboard (the ”custom active learning” category is entirely comprised of these).


**This is an interesting strategy you could use for the second competition round, but make sure you also co-optimize for expressibility!**


The top 5 submitted designs ranked by AlphaFold2’s iPAE in the first design competition round.


We thank


[Brian Naughton](http://blog.booleanbiotech.com/adaptyv-binder-design-competition.html) ,


[Anthony Gitter](https://x.com/anthonygitter/status/1827760228122738689) , and


[Alex Naka](https://x.com/gottapatchemall/status/1827386015713260019) for their contributions, both to the competition, and especially for these highly detailed writeups. Make sure you read their posts!


We thank all other design competition participants as well - it was a great experience seeing so many creative protein engineers’ solutions and, even beyond


[round two](https://design.adaptyvbio.com/) , we plan to organize more competitions in the future!


### Recommendations for ML based protein optimization


Main considerations for a protein engineering campaign. For simplicity, we grouped the starting data, lab budget, and risk-reward trade-off as business goals - willingness to spend on your campaign’s lab validation. ML power includes computational resources and ML experience - available expertise and resources to use ML models.


**A) Questions to ask yourself before an optimization campaign**


When selecting a model for your protein optimization project, ask yourself the following questions:


1.


**Starting data:** How much labeled data do you have available to start with (low-N vs. large-N regime)? Do you plan to obtain more data given your model’s suggestions?


2.


**Lab budget:** What is your total budget for screening? How many variants do you think you can reliably test out (consider replicates as well!)? Does your lab offer the necessary screening platforms and machines?


3.


**Risk-reward trade-off:** Are you trying to be more conservative and incrementally improve an existing lead? Or are you trying to discover the next moonshot therapeutic?


4.


**Computational resources:** How many computational resources (GPUs, available time to implement or train) do you have?


5.


**ML experience:** What is your ML experience? Are you able to implement novel architectures, simply fine-tune a model, or are you familiar with tools like ColabFold, 310.ai, tamarind.bio?


**B) The protein engineer’s personality test**


The answers to these questions determine in which of a couple of profiles a protein engineer might fall into, and which ML models likely to achieve their aims during a binder optimization campaign.


Decision flow chart for your next optimization campaign.


If you only have a


**single known binder or no starting data** , you might be forced to


*de novo* design using


[ProTrek](https://x.com/anthonygitter/status/1827760241598939416) or


[RFdiffusion](http://blog.booleanbiotech.com/adaptyv-binder-design-competition.html##:~:text=RFdiffusion) , or mine some data from the literature and maybe replace some residues in the binding site using a


[BLOSUM62 scheme](https://x.com/anthonygitter/status/1827760235039055884) . If you have a


**small starting set, you might try low-N fine-tuning techniques** and carefully plan to test what your model will suggest. If you’re lucky or proficient enough in the lab, you could have a


**large dataset** , mapping both single point and combinatorial mutations to their binding affinities. In this case, custom architectures and from-scratch training become feasible for you.


Next, when it comes to the


**lab resources for validation** , most at-home protein engineers have a


**small to non-existent** screening budget: they would validate at most their final best binder. This was the case for our EGFR competition, where completely


*in silico* oracles are necessary.


[Alex Naka](https://x.com/gottapatchemall) really leaned into this constraint,


[building a small dataset of low iPAE binders](https://x.com/gottapatchemall/status/1827386028640051523) , training


[CNN ensembles](https://x.com/gottapatchemall/status/1827386032301715571) for prediction, and finally creating a


[completely](https://x.com/gottapatchemall/status/1827386034923172305) *[in silico](https://x.com/gottapatchemall/status/1827386034923172305)*[active learning loop](https://x.com/gottapatchemall/status/1827386034923172305) .


If you can afford to validate about 200 designs, we recommend sequential optimization using either explicit model-based sampling or greedy/heuristics. We prefer Bayesian optimization/Active learning with simple, uncertainty-aware surrogates (Gaussian Processes or ensembles), as argued for in our


[first blog post](https://www.adaptyvbio.com/blog/po101) ! The


[ALDE](https://www.adaptyvbio.com/blog/po101#:~:text=Yang%20and%20colleagues) (


[Yang et al., 2024](https://www.biorxiv.org/content/10.1101/2024.07.27.605457v2) ) or


[EVOLVEpro](https://www.adaptyvbio.com/blog/po101#:~:text=Jiang%20and%20colleagues) (


[Jiang et al., 2024](https://www.biorxiv.org/content/10.1101/2024.07.17.604015v1) ) frameworks are great starting points. If you want to do some input optimization via AlphaFold2 backpropagation, take a look at


[BindCraft](https://colab.research.google.com/github/martinpacesa/BindCraft/blob/main/notebooks/BindCraft.ipynb) (


[Pacesa et al., 2024](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v1) ) - use the default settings or even implement your own design campaign-tailored loss function!


If you are fortunate enough, your validation budget could be


**large to limitless:** your best bet would be to take the largest protein foundation model you can find and sequentially fine-tuning it on new batches of data - you can retrain any fixed model. Be as greedy as you want: generate combinatorial libraries (


[Wu et al., 2019](https://www.pnas.org/doi/full/10.1073/pnas.1901979116) ) and select the top-performing variants at each step!


Another axis of consideration is the


**risk-reward trade-off** you are willing to make. Running more iterations of active learning, exploring more of the sequence space, or validating a larger batch at each step means higher investment, but also a higher chance of discovering moonshot binders, if you think those should exist. The alternative is to simply take an existing binder and do some local search that


*just* gets you away from patent protections while improving enough to be worth it. These areas of consideration (starting sequences, lab resources, risk-reward trade-off) ultimately depend on your


**business goals.**


Compute is rarely, if ever the bottleneck in protein design. Most protein engineers can make do with a single GPU


**** on their local machine. Some use


[Modal](https://modal.com/) or AWS, with a


**low resource consumption** . With very limited resources it’s even possible to set up an active learning loop (as seen


[here)](https://x.com/gottapatchemall/status/1827386036386984371) and only run it for a couple hours or days. We recommend using simple surrogates (1D CNN ensemble or Gaussian Processes, specifically). Simple one-hot encodings can also suffice, In fact, they perform about as well as embeddings from a protein language model like ESM2 for fitness prediction and optimization (


[Shanehsazzadeh et al., 2020](https://arxiv.org/abs/2011.03443) ;


[Greenman, Amini & Yang, 2023](https://www.biorxiv.org/content/10.1101/2023.04.17.536962v1) ;


[Yang et al., 2024](https://www.biorxiv.org/content/10.1101/2024.07.27.605457v2) ).


---


### Brief detour: cloud-GPUs


In the past, the way to do "budget" ML was to buy a used gaming PC and run stuff locally (getting you started on a few hundred USD with high FLOP/USD ROI). This is still the highest ROI if you will be running experiments 24/7 and don't pay for electricity (shoutout to


[Tim Dettmers](https://timdettmers.com/2023/01/30/which-gpu-for-deep-learning/) ), yet the hype around chatGPT created a very competitive cloud computing environment.


We therefore recommend that you use


[Modal](https://modal.com/) ,


[Google Colab](https://colab.research.google.com/) , or other cloud-GPU platforms to get started.


Most of them now offer generous free tiers (in the case of Modal, 30 USD recurrent per month) and it means you can get started for as low as 0 USD (with credits) and for about 5 USD per designed protein, following


[Brian’s approach.](http://blog.booleanbiotech.com/adaptyv-binder-design-competition.html)


Click


[here](https://cloud-gpus.com/) for a cost comparison between different cloud-computing platforms.


**And, lucky for you, we have partnered with Modal to offer free credits worth 500 USD on a first come first serve basis: you can get them from [here](https://design.adaptyvbio.com/) . Make sure you mention which computing platform you used when you submit your binders. Good luck designing!!**


---


That being said, more is in fact better. If you have a


**high computational resource budget -** you can fine-tune any large language model (


[Li et al., 2023](https://www.nature.com/articles/s41467-023-39022-2) ) or diffusion models (


[Bennett et al., 2024](https://pubmed.ncbi.nlm.nih.gov/38562682/) ). While these larger models will also require more data, your limitless computational budget means you could collect data from


*in silico* oracles as a proxy, or augment your sequence features for model training. For example, you could train on energy scores from Rosetta, perform molecular dynamics simulations, observe multiple blind-docking instances, run AlphaFold2 on your variants and extract structural features to train your surrogates on, and many more options.


Finally, you should consider your ML experience. This, combined with compute resources, describes your


**ML power** . Lucky for you, you live in the best times to do computational protein design because the barrier of entry has been lowered so much.


Use platforms like


[310](http://310.ai/) ,


[BioLM](https://biolm.ai/) ,


[Lab.Bio](http://lab.bio/) ,


[Tamarind](https://www.tamarind.bio/) ,


[Latch](https://latch.bio/) , or


[many more](https://design.adaptyvbio.com/tools) if you lack any ML experience. For example, 310’s Copilot lets you


[chat with your personal protein design assistant](https://310.ai/copilot/e10dd124-ba9e-4232-96fd-a368875e8ecf) : you can ask it to find functionally annotated proteins, fold them, diversify them with ProteinMPNN, compare their structures, then export.


Then, for someone with an intermediate level of experience in ML, there is the option of fine-tuning an existing language model from HuggingFace and performing


*[in silico](https://huggingface.co/blog/AmelieSchreiber/esm-interact)*[directed evolution](https://huggingface.co/blog/AmelieSchreiber/esm-interact) . Implement your own architectures if you’re an experienced ML practitioner, or loss functions, optimization objectives, try out a bunch of acquisition functions, participate in design competitions, publish on arXiv, write Substack articles and


[post on X](https://x.com/i/communities/1838705263672431012) to get involved in the scene! You know what to do!


Platforms for protein design and optimization


**We hope to see you in the [second round of the EGFR competition](https://design.adaptyvbio.com/) !**


---


### References


\[1\] Anishchenko, I., Pellock, S.J., Chidyausiku, T.M., Ramelot, T.A., Ovchinnikov, S., Hao, J., Bafna, K., Norn, C., Kang, A., Bera, A.K., DiMaio, F., Carter, L., Chow, C.M., Montelione, G.T. & Baker, D. (2021) De novo protein design by deep network hallucination. Nature 2021 600:7889. 600 (7889), 547–552. doi:10.1038/s41586-021-04184-w.


\[2\] Bennett, N.R., Coventry, B., Goreshnik, I., Huang, B., Allen, A., Vafeados, D., Peng, Y.P., Dauparas, J., Baek, M., Stewart, L., DiMaio, F., De Munck, S., Savvides, S.N. & Baker, D. (2023) Improving de novo protein binder design with deep learning. Nature Communications 2023 14:1. 14 (1), 1–9. doi:10.1038/s41467-023-38328-5.


\[3\] Bennett, N.R., Watson, J.L., Ragotte, R.J., Borst, A.J., See, D.L., et al. (2024) Atomically accurate de novo design of single-domain antibodies. bioRxiv : the preprint server for biology. doi:10.1101/2024.03.14.585103.


\[4\] Cao, L., Coventry, B., Goreshnik, I., Huang, B., Sheffler, W., et al. (2022) Design of protein-binding proteins from the target structure alone. Nature 2022 605:7910. 605 (7910), 551–560. doi:10.1038/s41586-022-04654-9.


\[5\] Chen, T., Dumas, M., Watson, R., Vincoff, S., Peng, C., et al. (2023) PepMLM: Target Sequence-Conditioned Generation of Therapeutic Peptide Binders via Span Masked Language Modeling.


[https://arxiv.org/abs/2310.03842v3](https://arxiv.org/abs/2310.03842v3) .


\[6\] Dauparas, J., Anishchenko, I., Bennett, N., Bai, H., Ragotte, R.J., et al. (2022) Robust deep learning–based protein sequence design using ProteinMPNN. Science. 378 (6615), 49–56. doi:10.1126/SCIENCE.ADD2187/SUPPL_FILE/SCIENCE.ADD2187_SM.PDF.


\[7\] Eddy, S.R. (2004) Where did the BLOSUM62 alignment score matrix come from? Nature Biotechnology 2004 22:8. 22 (8), 1035–1036. doi:10.1038/nbt0804-1035.


\[8\] Emami, P., Perreault, A., Law, J., Biagioni, D. & St. John, P. (2023) Plug & play directed evolution of proteins with gradient-based discrete MCMC. Machine Learning: Science and Technology. 4 (2), 025014. doi:10.1088/2632-2153/ACCACD.


\[9\] Frank, C., Khoshouei, A., Stigter, Y. de, Schiewitz, D., Feng, S., Ovchinnikov, S. & Dietz, H. (2023) Efficient and scalable de novo protein design using a relaxed sequence space. bioRxiv. 2023.02.24.529906. doi:10.1101/2023.02.24.529906.


\[10\] Glögl, M., Krishnakumar, A., Ragotte, R.J., Goreshnik, I., Coventry, B., Bera, A.K., Kang, A., Joyce, E., Ahn, G., Huang, B., Yang, W., Chen, W., Sanchez, M.G., Koepnick, B. & Baker, D. (2024) Target-conditioned diffusion generates potent TNFR superfamily antagonists and agonists. bioRxiv. 2024.09.13.612773. doi:10.1101/2024.09.13.612773.


\[11\] Goverde, C.A., Pacesa, M., Goldbach, N., Dornfeld, L.J., Balbi, P.E.M., Georgeon, S., Rosset, S., Kapoor, S., Choudhury, J., Dauparas, J., Schellhaas, C., Kozlov, S., Baker, D., Ovchinnikov, S., Vecchio, A.J. & Correia, B.E. (2024) Computational design of soluble and functional membrane protein analogues. Nature 2024 631:8020. 631 (8020), 449–458. doi:10.1038/s41586-024-07601-y.


\[12\] Goverde, C.A., Wolf, B., Khakzad, H., Rosset, S. & Correia, B.E. (2023) De novo protein design by inversion of the AlphaFold structure prediction network. Protein Science : A Publication of the Protein Society. 32 (6). doi:10.1002/PRO.4653.


\[13\] Greenman, K.P., Amini, A.P. & Yang, K.K. (2023) Benchmarking Uncertainty Quantification for Protein Engineering. bioRxiv. 2023.04.17.536962. doi:10.1101/2023.04.17.536962.


\[14\] Jiang, K., Yan, Z., Bernardo, M. Di, Sgrizzi, S.R., Villiger, L., Kayabolen, A., Kim, B., Carscadden, J.K., Hiraizumi, M., Nishimasu, H., Gootenberg, J.S. & Abudayyeh, O.O. (2024) Rapid protein evolution by few-shot learning with a protein language model. bioRxiv. 2024.07.17.604015. doi:10.1101/2024.07.17.604015.


\[15\] Kim, D.G., Choi, Y. & Kim, H.S. (2021) Epitopes of Protein Binders Are Related to the Structural Flexibility of a Target Protein Surface. Journal of Chemical Information and Modeling. 61 (4), 2099–2107. doi:10.1021/ACS.JCIM.0C01397/SUPPL_FILE/CI0C01397_SI_002.ZIP.


\[16\] Korendovych, I. V. (2018) Rational and Semirational Protein Design. Methods in molecular biology (Clifton, N.J.). 1685, 15. doi:10.1007/978-1-4939-7366-8_2.


\[17\] Li, L., Gupta, E., Spaeth, J., Shing, L., Jaimes, R., Engelhart, E., Lopez, R., Caceres, R.S., Bepler, T. & Walsh, M.E. (2023) Machine learning optimization of candidate antibody yields highly diverse sub-nanomolar affinity antibody libraries. Nature Communications 2023 14:1. 14 (1), 1–12. doi:10.1038/s41467-023-39022-2.


\[18\] Pacesa, M., Nickel, L., Schmidt, J., Pyatova, E., Schellhaas, C., et al. (2024) BindCraft: one-shot design of functional protein binders. bioRxiv. 2024.09.30.615802. doi:10.1101/2024.09.30.615802.


\[19\] Shanehsazzadeh, A., Belanger, D., Research, G. & Dohan, D. (2020) Is Transfer Learning Necessary for Protein Landscape Prediction?


[https://arxiv.org/abs/2011.03443v1](https://arxiv.org/abs/2011.03443v1) .


\[20\] Su, J., Zhou, X., Zhang, X. & Yuan, F. (2024) ProTrek: Navigating the Protein Universe through Tri-Modal Contrastive Learning. bioRxiv. 2024.05.30.596740. doi:10.1101/2024.05.30.596740.


\[21\] Watson, J.L., Juergens, D., Bennett, N.R., Trippe, B.L., Yim, J., et al. (2023) De novo design of protein structure and function with RFdiffusion. Nature 2023 620:7976. 620 (7976), 1089–1100. doi:10.1038/s41586-023-06415-8.


\[22\] Wicky, B.I.M., Milles, L.F., Courbet, A., Ragotte, R.J., Dauparas, J., Kinfu, E., Tipps, S., Kibler, R.D., Baek, M., DiMaio, F., Li, X., Carter, L., Kang, A., Nguyen, H., Bera, A.K. & Baker, D. (2022) Hallucinating symmetric protein assemblies. Science. 378 (6615), 2024. doi:10.1126/SCIENCE.ADD1964/SUPPL_FILE/SCIENCE.ADD1964_SM.PDF.


\[23\] Wu, Z., Jennifer Kan, S.B., Lewis, R.D., Wittmann, B.J. & Arnold, F.H. (2019) Machine learning-assisted directed protein evolution with combinatorial libraries. Proceedings of the National Academy of Sciences of the United States of America. 116 (18), 8852–8858. doi:10.1073/PNAS.1901979116/SUPPL_FILE/PNAS.1901979116.SAPP.PDF.


\[24\] Xue, L.C., Rodrigues, J.P., Kastritis, P.L., Bonvin, A.M. & Vangone, A. (2016) PRODIGY: a web server for predicting the binding affinity of protein–protein complexes. Bioinformatics. 32 (23), 3676–3678. doi:10.1093/BIOINFORMATICS/BTW514.


\[25\] Yang, J., Lal, R.G., Bowden, J.C., Astudillo, R., Hameedi, M.A., Kaur, S., Hill, M., Yue, Y. & Arnold, F.H. (2024) Active Learning-Assisted Directed Evolution. bioRxiv. 2024.07.27.605457. doi:10.1101/2024.07.27.605457.


\[26\] Zambaldi, V., La, D., Chu, A.E., Patani, H., Danson, A.E., et al. (2024) De novo design of high-affinity protein binders with AlphaProteo.


[https://arxiv.org/abs/2409.08022v1](https://arxiv.org/abs/2409.08022v1) .


\[27\] Zhang, J.Z., Li, X., Liu, C., Jiang, H., Wu, K. & Baker, D. (2024) De novo design of Ras isoform selective binders. bioRxiv. 2024.08.29.610300. doi:10.1101/2024.08.29.610300.
