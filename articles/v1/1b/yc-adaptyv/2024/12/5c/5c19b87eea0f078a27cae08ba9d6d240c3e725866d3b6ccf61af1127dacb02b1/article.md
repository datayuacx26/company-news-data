---
schema_version: "1.0.0"
document_id: "5c19b87eea0f078a27cae08ba9d6d240c3e725866d3b6ccf61af1127dacb02b1"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/po103"
published_at: "2024-12-05T00:00:00+00:00"
first_seen_at: "2026-07-24T14:21:15.816567+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:a857e39257a21220d5f63956d4fa7f9141f7723bfff535015412f6ac77a19682"
---

# Protein Optimization 103: Racing to the Top 100

/


TL;DR


- -


Designers fought hard for a spot in the top 100. Several were racing against each other, switching the top positions multiple times right before the deadline.


- -


We are in the Cambrian explosion of design techniques, but there are some clear preferences. RFdiffusion with ProteinMPNN and BindCraft or RSO hallucination claimed the top spots.


#### A quick recap of the second round


In case you missed out, we’re running a


[second round of the EGFR binder design competition](https://design.adaptyvbio.com/)


. This was in partnership with


[Polaris](https://protein.polarishub.io/)


and


[Dimension Capital](https://www.dimensioncap.com/)


, and with the help of


[Modal](https://modal.com/)


, who offered $500 of GPU credits, and


[Twist](https://twistbioscience.com/)


, who helped us cover the costs for DNA synthesis!


The rules stayed more or less the same: participants were tasked with designing a new binder to the extracellular domain of EGFR, a cancer-associated drug target. Every design had to be at minimum 10 amino acids away from known therapeutic binders. At the end of each day, we ranked the top designs via a computational score. We then selected the top 100 for experimental validation, with an additional selection for the most unique design approaches.


Last time, we filtered the top 100 designs using the interface PAE metric from AlphaFold2 as described in various papers from Baker lab (


[Bennett et al., 2023](https://www.nature.com/articles/s41467-023-38328-5)


;


[Watson et al., 2023](https://www.nature.com/articles/s41586-023-06415-8)


), to which we adhered much to the dismay of several protein designers. We listened and included two additional metrics after


[polling the community](https://x.com/adaptyvbio/status/1844456052705853643)


: AlphaFold2’s interface pTM (a


[predictor of binding](https://x.com/MartinPacesa/status/1841357650967122222)


as a binary label) and ESM2’s pseudolikelihood/log-likelihood (


[Rives et al., 2021](https://www.pnas.org/doi/10.1073/pnas.2016239118)


;


[Lin et al., 2023](https://www.science.org/doi/10.1126/science.ade2574)


), based on some


[initial evaluations](https://x.com/NikhilHaas/status/1844446275967779284)


done by Nikhil Haas. Many of you pointed out later the fact that


[we did not normalize our ESM2 score by the sequence length](https://x.com/deepsatflow/status/1853442284064530592)


, which led to some interesting shortcuts taken by savvy designers to race to the top of the leaderboard.


We did not just want to select designs that hyper-optimize some


*in-silico*


metrics. That’s why, this time, we picked


[300(!!) additional designs](https://x.com/adaptyvbio/status/1854620949108183089)


for screening. Each designer had at least one protein tested and we filled in the rest with methods we found interesting or “mini-experiments” the participants hinted towards in the design process notes. Again, we thank everyone who participated, but we particularly appreciate those who left descriptive and interesting comments about their design method or were actively engaged through social media or blog posts. We also encourage people to include hypotheses/rationales which we can test out as “mini-experiments” for future competitions!


This Saturday, December 7th, we'll reveal the results of the ultimate test: do they actually bind in the lab or not? In anticipation, we will highlight some of our observations about what people did for their designs, how close the race was, and other trends we saw during the 2 weeks of competition. Lots of things have changed since our


[first round’s analysis](https://www.adaptyvbio.com/blog/po102)


!


#### The Cambrian explosion of design models


Number of designs submitted and selected in the second round of the EGFR competition by model category.


We found an enrichment of model choices compared to the first round (figure above). This is only a glimpse of the actual model diversity: a lot of designers mentioned their in-house variational autoencoders, diffusion, flow-matching, or other non-descriptive generative models (all labelled as “custom generative”), or protein language models (under the “custom PLM” -


**P**


rotein


**L**


anguage


**M**


odel - umbrella term).


The top choice is still RFdiffusion + ProteinMPNN, followed by AlphaFold2 filtering and some additional ESM2 likelihood optimization steps. We think this is mainly due to:


1. this pipeline’s accessibility, with many platforms including its full implementation (


[Tamarind Bio](https://app.tamarind.bio/protein-design-pipelines)


,


[ColabDesign](https://colab.research.google.com/github/sokrypton/ColabDesign/blob/v1.1.1/rf/examples/diffusion.ipynb)


), although some people mention their own pipelines.


2. its high hit rates and potential affinities, as reported by


[us](https://www.adaptyvbio.com/blog/rfdiff_il7ra)


and


[others](https://portal.valencelabs.com/blogs/post/rfdiffusion-5-anecdotal-observations-over-a-year-IoUlWlHZRw3aybI#)


, or seen in the


[previous round’s results](https://foundry.adaptyvbio.com/egfr_design_competition)


.


What we found interesting is that many designers used the following pipeline for optimization: known binders from the literature were partially diffused and then enriched with ProteinMPNN, filtered down to the most promising backbones given our metrics, and partially diffused again. We categorized this as diversifying a binder rather than optimizing. It is similar to Adrian Tripp’s


[successful approach](https://www.adaptyvbio.com/blog/po102#:~:text=Third%20place)


we have seen before.


A common (and successful, with 22 designs in the top 100) technique involved redesigning the binding site of a known binder with ProteinMPNN, then using ESM2 to iteratively replace the rest of the protein while maximizing its likelihood. This fits into the fixed model optimization schema we defined in


[one of our blog posts](http://adaptyvbio.com/blog/po101)


, a form of


**M**


achine


**L**


earning


**D**


irected


**E**


volution (MLDE) for fitness maximization. Others included Rosetta for interface redesign, with or without ProteinMPNN diversification, followed by ESM2 MLDE. It was also super effective!


We encouraged you to


[give BindCraft (Pacesa et al., 2024)](https://www.adaptyvbio.com/blog/po102#:~:text=give%20bindcraft%20a%20try)


a try and so you did! Some of you used another pipeline -


**R**


elaxed


**S**


equence


**O**


ptimization (RSO,


[Frank et al., 2024](https://www.science.org/doi/10.1126/science.adq1741)


), also built on


[ColabDesign](https://colab.research.google.com/github/sokrypton/ColabDesign/blob/main/af/examples/RSO.ipynb)


and performing input sequence optimization in a continuous space. This means a random sequence is fed through AlphaFold2, and the gradient of the loss (which can include multiple terms!) is taken with regard to the input. Unfortunately, BindCraft was not as successful as expected with respect to the


*in-silico*


metrics, with only 3 designs making the top 100. This is due to users not optimizing the ESM2 likelihood and, moreso, the pipeline still yielding relatively lower iPAE and ipTM scores caused by the composite loss involving more terms (helicity, structural compactness), compared to methods trying to maximize only these two. We included other non-descriptive mentions of hallucination/backpropagation through AlphaFold in the “other hallucination” category.


With regards to models we found interesting and worthy representatives of their categories, the steady evolutionary leaps in ML meant some long-standing powerhouses were replaced: inverse folding using CARBonAra (


[Krapp et al., 2024](https://www.nature.com/articles/s41467-024-50571-y)


) or


[TIMED](https://www.linkedin.com/posts/marta-chronowska-35b772216_timed-design-flexible-and-accessible-protein-activity-7260640654668480514-M-XA/)


(


[Castorina et al., 2024](https://academic.oup.com/peds/article/doi/10.1093/protein/gzae002/7591701)


) instead of ProteinMPNN, sampling with EvoDiff (


[Alamdari et al., 2023](https://www.biorxiv.org/content/10.1101/2023.09.11.556673v1)


) instead of RFdiffusion, and function filtering with ProTrek (


[Su et al., 2024](https://www.biorxiv.org/content/10.1101/2024.05.30.596740v1)


) or conditional generation with ESM3 (


[Hayes et al., 2024](https://www.biorxiv.org/content/10.1101/2024.07.01.600583v1)


).


There has also been an increase in usage of optimization via active learning methods which we were previously


[raving about](https://www.adaptyvbio.com/blog/po101)


. We’ve seen more of


[Alex Naka’s approach](https://x.com/gottapatchemall/status/1827386015713260019)


of training a surrogate (finetuned ESM2 or any regression model) to distil the AlphaFold2 oracle and then integrating it with EvoProtGrad (


[Emami et al., 2023](https://arxiv.org/abs/2212.09925)


) for guided design, followed by retraining (


[sequential optimization with model-based sampling](https://www.adaptyvbio.com/1135ca69e7be800ca31cf6f6ad06de58)


). As expected, this was quite successful in landing in the top 100. Others used reinforcement learning, yielding a similar performance.


The hype for language model agents, from


[coding assistants](https://medium.com/@ashinno43/cursor-v0-43-3-with-composer-agent-is-insane-d770dc5b61ea)


, to “functioning” Minecraft societies (


[AL et al., 2024](https://arxiv.org/abs/2411.00114)


), or fully automated scientists (


[Skarlinski et al., 2024](https://arxiv.org/abs/2409.13740)


) and labs (


[Swanson et al., 2024](https://www.biorxiv.org/content/10.1101/2024.11.11.623004v1)


), has trickled down to our competition. One designer simply stated they used a “GPT agent” for their design. We’re interested to see how that performs and what the future holds for LM agent-based design with or without humans-in-the-loop. On this note, a self-serving shout-out to


[310.ai](http://310.ai/)


and


[basedlabs](https://x.com/adaptyvbio/status/1861458246407426357)


which recently integrated with our


[new API](https://beta.adaptyvbio.com/api)


.


#### *De novo*


was outclassed by improving an existing binder


Number of designs submitted and selected in the second round of the EGFR competition by design category.


As you might remember, last time we grouped all submissions into


[diversified binders, optimized binders, de novo, and hallucination](https://www.adaptyvbio.com/blog/po102#:~:text=The%20most%20popular%20options%20included:)


. This time, lines were blurred between diversified binder + filter and optimized


binder because


several designers used a combination of the two.


For comparison's sake, we kept it simple and categorized the "core" of the method using the same logic as in the first round:


1


.


diversified binders focus on sampling from a conditional generative model, usually conditioned on input structure with a temperature term, followed by filtering for metrics using other oracles


. If


*de novo*


is (often*) a completely random search, diversifying a binder resembles more of a localized random search (constrained search space).


2


. we specifically refer to optimization via backpropagation through the structure prediction model as hallucination, as commonly known by protein designers.


Hallucination is a special subset of optimization.


3


. optimized binders


involve either setting up an active learning or Bayesian optimization loop (


[sequential optimization](https://www.adaptyvbio.com/blog/po101#:~:text=sequentially-optimized%20models)


), performing MLDE with surrogates (


[fixed model optimization](https://www.adaptyvbio.com/blog/po101#:~:text=Supplementary%20-%20Fixed%20model%20optimization)


), or any other algorithm different from hallucination.


Unlike the previous round, we no longer have a distinct physics-based category. This is because any such approach (e.g., molecular dynamics for conformational landscape sampling or Rosetta ΔG predictions) was always combined with extensive additional AI-based methods. Before, these were employed standalone, with minimal AlphaFold2 or ProteinMPNN “retouching”. People wanted to land in that top 100 spot!


*De novo*


is still the most common method - but it has only 4 designs in the top 100. This simply reflects how unlikely it is to sample from the “protein slot machine” and max out 3 metrics, not just one. Going more in-depth into why


*de novo*


performed worse this time around, let’s do some simple statistics. Let’s assume the meta did not change (even though it did so, noticeably due to the prevalence of optimization methods pushing the score boundaries) compared to the first round and that people had the same sampling (computational) budget. We had a


[37.5% (30/80) success rate](https://www.adaptyvbio.com/1135ca69e7be800ca31cf6f6ad06de58)


for the first competition solely based on the iPAE threshold. Since we now have more metrics to optimize, one simple assumptions is that you need to make the top 100 on each of them to make the final cut. Optimistically, using the same probability to land in the top 100 for ESM2 and for ipTM (perfectly decoupled), that leads to 0.375^3 or a success rate of 5.3%, close to the 1% (4/384) we observe. To point out the obvious: random search performs exponentially worse when we increase the number of objectives. The meta also changed, so keep an eye out for our next blog post where we address how this influenced not just the leaderboard rankings but also the experimental results!


#### Racing by optimizing


Let’s now talk about the cutthroat contest of the final 48 hours. We noticed most people submitted their designs in the last 2 days before the deadline. Other than limit-testing our submission processing endpoints, this had some interesting consequences.


**People heavily opted for binder optimization**


after the peaceful


*de novo*


days we initially had. This still doesn’t represent a huge fraction of the total submissions but makes up more than 40% of the top 100 selections, as you can see in the animation above.


The number of diversified binders also increased, representing another 40% of the top 100 on the final day. We’ve noticed people still perform a more localized search (conceptually similar to


[Tabu search](https://en.wikipedia.org/wiki/Tabu_search)


) even in this case, be that starting from known binders integrated into a partial diffusion loop or slightly diversified with an inverse folding model.


The prevalence of these methods in the last few days, dethroning


*de novo*


, is quite unique. Did some people F5 the leaderboard daily as their pipelines were churning until it was time to launch their hyper-optimized designs into the final sprint to the top 100? Maybe… All we know is spending time on optimizing was a sound choice.


The predominance of diversification and optimization is also reflected in how the metrics evolved. In the animation above, we normalized the average scores of the top 100 and all submissions at the end of each day against the best overall score. The trends are quite interesting: the top 100 improved across all objectives, with a rush in the last couple of days. The average designer simply had some fun, with the metrics stabilizing throughout the competition.


iPAE seems to be the the tougher one to improve. It was only surging in the last 5 days for the top 100 submissions and the starting average was comparatively lower. We speculate this might be due to a few of the top designs having taken a considerable lead in this metric, while the rest stagnated. Some people might have also found optimizing ESM2’s score more straightforward (masking residues and replacing them with others that increase likelihood) than sampling and filtering for good iPAE or ipTM.


Looking at the best scores, we see that ipTM was maxed out early on. The best iPAE was reached on the last day. Some users maximized two of the metrics at given times (iPAE and ipTM or likelihood and ipTM), but no one was able to achieve the maximum for all 3 at the same time. This indicates the difficulty of our multi-objective optimization problem, especially if the scores are not correlated or if there are trade-offs. Some models could implicitly optimize two metrics: e.g., loss terms in BindCraft accounting for iPAE and ipTM.


Surprisingly, all the designers we see above used a form of ProteinMPNN-based sampling on either diffused or natural binding backbones. Some applied this to multiple conformations, implemented a smarter way to rank designs, or sampled plenty of designs. We will see if these front-runners are true binders or not!


#### There’s a clear sequence length preference


Some of you


[pointed out](https://x.com/design_proteins/status/1854744521227092275)


we were using the unnormalized ESM2 log-likelihood, which penalizes longer designs. We checked if this was noticeable throughout the competition.


And, indeed, up until a few days before the deadline, the correlation between the normalized and unnormalized ranks was


[pretty high.](https://x.com/TheGermanPole/status/1854837261793100264)


Boy, did that change in the last 48 hours though!


In the animation above, we see how the correlation between sequence lengths and average rank becomes more noticeable as the competition progresses for the top 100 and all designs. The pattern for the top 100 appears even more striking, with not a single sequence longer than 100 residues. One explanation for this could be that everyone trimmed unnecessary parts of their binders to top out likelihoods. This also checks out when looking at the evolution across submissions. The top 6 sequences are all around 30 amino acids, and their designer explicitly stated they fixed the binding site of TGFα and trimmed amino acids, then selected the ones adhering to the metrics.


However, we believe the leaderboard’s pattern is explained better by the fact that most of the top designs started from EGF or TGFα, both around 50 amino acids in length. People might have noticed these had good enough scores already and a minimal diversification followed by ESM2 MLDE pushes them to their limits.


In conclusion, do we believe some of the designers in the top 100 selected an existing binder and removed amino acids to make ESM2 happy? Maybe… Again, a very sound choice!


#### Fold composition differs for the top 100 designs


We wanted to see if there are some obvious biases in the secondary structure composition of the top 100 versus all submissions. We hypothesized that given some models’ preference for helical binders (AlphaFold2 unless explicitly penalized in a hallucination setup, RFdiffusion), this will be the over-represented structure.


It holds when looking at all designs, especially in the first part of the competition. We saw before this was dominated by


*de novo*


and diversified binder approaches, likely combining RFdiffusion with ProteinMPNN, so an abundance of helices makes sense. Overall, helices and loops are well-represented, followed by sheets - unsurprising, yet a good confirmation of the current state and biases of binder design tools.


The distinction becomes more clear when looking at the top 100 designs. Loops are way better represented early on and, by the competition’s deadline, comprise over 50% of the secondary structure distribution. They are followed by β-sheets, which we did not expect. This can be explained by the starting point binders: for example, EGF’s structure contains 58% loops and 27% β-sheets, while TGFα’s has 56% loops and 38% sheets. Ultimately, as most of the designs in the top 100 used a starting point binder (TGFα more frequently), the secondary structure distribution will replicate this.


As stated in the literature (


[Cao et al., 2022](https://www.nature.com/articles/s41586-022-04654-9)


), we also believe the next design challenge will involve complete


*de novo*


binders with an abundance of β-sheets, targeting hydrophilic surfaces. We hope the people who tried these in our competition will have plenty of high-affinity binders!


#### TGFα and EGF were the preferred starting points


ESM2 embeddings of all submissions, coloured by design category and clustered by similarity to common starting point binders.


We’ve established the main starting points for the optimized and diversified binder strategies: TGFα and EGF. We next checked how these clusters are represented in a sequence only and structure-injected embeddings space, from the ESM2 (


[Lin et al., 2023](https://www.science.org/doi/10.1126/science.ade2574)


) and SaProt (


[Su et al., 2024a](https://www.biorxiv.org/content/10.1101/2023.10.01.560349v1)


) language models, respectively.


Our homology assignment (50% sequence identity normalized by sequence length) was basic and quite restrictive, yet could miss out a lot of the homologues to the two targets, especially since most of them used backbone diversification with ProteinMPNN or had several amino acids trimmed. Thus, two given sequences could be drastically different, whereas their structures could be the same. Some designers fixed or scaffolded-in the binding motifs from these two proteins, which could also be missed by a homology threshold. We hope these caveats can be captured in the SaProt embedding space.


We were interested in seeing if any designs labelled as


*de novo,*


as we inferred from the design process description, would be similar to existing binders. As


[pointed out](https://x.com/design_proteins/status/1854744523466817626)


, only two designers in the top 100 went the complete


*de novo*


route.


In the plot above we see the UMAP and PCA projections of ESM2-650M’s embeddings, colored by the design category. The two EGF and TGFα-like clusters are quite distinct and encompass a lot of the diversified or optimized binder submissions. Interestingly, most of the optimized/diversified binder entries cluster separately from the de novo or hallucinated ones.


A couple of designers stated they used the previous round’s submissions, either diversifying them, making them more soluble and expressible, or simply adding poly-glycine linkers to proven binders. Our analysis does not capture these other starting binders, but it would be interesting to trace the “evolutionary history” or “pull requests” of a known design.


SaProt embeddings of all submissions, coloured by design category and clustered by similarity to common starting point binders.


Similar patterns emerge when analysing SaProt’s embeddings. For those unfamiliar, SaProt jointly projects sequences and structures (as structure-representative tokens from the


[FoldSeek](https://github.com/steineggerlab/foldseek)


vocabulary): its representations are seemingly richer (


[Su et al., 2024a](https://www.biorxiv.org/content/10.1101/2023.10.01.560349v1)


) and its likelihood correlates well with several properties like affinity, thermostability, or expression, per the ProteinGym benchmark (


[Notin et al., 2023](https://www.biorxiv.org/content/10.1101/2023.12.07.570727v1)


). We see how EGF and TGFα-derived designs overlap in the embedding space, indicating some resemblance between the two which we also observed in the secondary structure distribution.


*De novo*


designs and hallucinated ones form separate clusters as well.


#### Highlighting some cool approaches and community efforts


Yet again, the protein design community did not disappoint, even outdoing the first round of the competition. Almost all submissions included detailed explanations of their process, reasoning, and even some hypotheses or calls to action for why we should pick their designs. We thank you all for this!


There were also a lot more social media and blog posts about the participants’ submissions. We will showcase some posts and urge you to check them out. They are an incredible resource for any computational protein designer, be they a newbie or a Rosetta master.


1. [Jude Wells](https://x.com/_judewells/status/1853805775807758465)


went in-depth into his design philosophy, linking together known antibody therapeutics and validating them with Chai-1 (


[team et al., 2024](https://www.biorxiv.org/content/10.1101/2024.10.10.615955v1)


).


2. [Anthony Gitter](https://github.com/agitter/adaptyvbio-egfr/tree/main/round2#adaptyv-egfr-protein-design-competition-round-2)


shared an incredibly comprehensive overview of his process and corresponding code. You’ll find scripts to run BindCraft jobs and analyze the results, along with insights into the lessons he learned from the first round and how he changed his strategy throughout the competition.


3. Allon Goldberg documented his approach in the first and second rounds


[here](http://allonwork.glitch.me/egfr.html)


for a complete


*de novo*


Rosetta-based design. His plots are incredibly aesthetically pleasing as well - check them out!!


4. [@suzuki2001_](https://x.com/suzuki2001_/status/1854840776594993321)


also released their


[code](https://github.com/suzuki-2001/adaptyv-protein-comp)


, using both BindCraft and RSO combined with AlphaFold3 and ColabFold. You can also find


[Corey Howe](https://x.com/design_proteins/status/1849919279790997596?s=61&t=4LW4TKynjsiSiQ9kKfehMg)


’s


[Modal implementation of RSO](https://github.com/coreyhowe999/RSO)


.


5. [Aurelia Bustos](https://www.linkedin.com/pulse/our-jouney-adaptyvbio-protein-design-competition-bustos-md-phd-ykmaf/)


had a super successful approach: maintaining the TGFα binding site, partial diffusion and inverse folding for the scaffold, filtering, and then trimming the sequence to maximize ESM2’s log-likelihood. Her method yielded some of the best-scoring designs - you should try it as well!


6. [Young Su Ko](https://x.com/youngsuko9/status/1855066343407599846)


’s method was very creative and used some of the most recent protein design breakthroughs: magnifying EGFR with Raygun (


[Devkota et al., 2024](https://www.biorxiv.org/content/10.1101/2024.08.13.607858v1)


) and checking if the core functionality was maintained with


[ProTrek](http://search-protrek.com/)


’s inferred annotations (


[Su et al., 2024b](https://www.biorxiv.org/content/10.1101/2024.05.30.596740v1)


).


7. [Brian Naughton](https://x.com/btnaughton/status/1855746146934853944)


went the classic route: poly-glycine-linked previous binders. Check out his


[animation](https://www.youtube.com/watch?v=DJkY-Tkg8RE)


of this round’s top 100!


8. [Cianna Calia](https://github.com/ccalia/EGFR_Binders_Adaptyv_Round2)


combined two BindCraft-generated binding domains with custom disordered linker sequences, generated via diffusion. You can find her code, write-up, and linker sequences


[here](https://github.com/ccalia/EGFR_Binders_Adaptyv_Round2)


!


9. The people from the


[Wells Wood lab](https://www.linkedin.com/posts/marta-chronowska-35b772216_timed-design-flexible-and-accessible-protein-activity-7260640654668480514-M-XA/)


generated a lot of buzz around their TIMED model for sequence generation (


). They used it to diversify known binders and also combined it with BindCraft. If you’re looking for ProteinMPNN alternatives, integrated into an excellent


[end-to-end design pipeline](https://github.com/wells-wood-research/timed-design)


, you should try TIMED!


#### Waiting for the experimental results and speculation


We look forward to sharing the experimental results. What we can say already: lots of people inserted existing binding motifs, used tools like SolubleMPNN and NetSolP for improving solubility, and our new selection criteria had an impact on both expression and affinity.


To find out more, be sure to check out the


[release this Saturday, December 7th](https://design.adaptyvbio.com/)


. And also keep up with the latest


Adaptyv


news by following our


[Twitter](https://x.com/adaptyvbio)


and


[LinkedIn](https://www.linkedin.com/company/adaptyvbio/?originalSubdomain=ch)


! There will be exciting updates to our platform coming early next year.
