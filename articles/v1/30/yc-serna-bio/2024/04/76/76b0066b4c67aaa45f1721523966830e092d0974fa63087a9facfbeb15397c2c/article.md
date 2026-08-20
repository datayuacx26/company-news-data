---
schema_version: "1.0.0"
document_id: "76b0066b4c67aaa45f1721523966830e092d0974fa63087a9facfbeb15397c2c"
company_key: "yc-serna-bio"
company: "Serna Bio"
source_id: "yc-serna-bio-news-import-10fe1061b09a"
canonical_url: "https://www.serna.bio/blog-post/predicting-rna-binding-molecules-using-machine-learning"
published_at: "2024-04-18T00:00:00+00:00"
first_seen_at: "2026-07-25T23:35:45.687739+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:59b7e67d65467a2b1209a8b659f74bd34c869626388b087cabb9b49f7ab067af"
---

# Predicting RNA Binding Molecules Using Machine Learning

‍


## Introduction


Developing a new modality such as[PROTACs](https://www.nature.com/articles/s41573-021-00371-6) or developing small molecules to target a new protein class- (such as[kinase](https://www.nature.com/articles/s41573-022-00418-2) inhibitors) has historically taken decades. Although small molecules targeting kinases are a major drug class now, these were not long ago, considered[undruggable](https://www.nature.com/articles/s41573-021-00252-y?fromPaywallRec=false) .


> “The development of protein kinase inhibitors that bind to the ATP site was initially viewed as an unsurmountable challenge because of the high concentration of ATP in the cell, the poor understanding of the regulation of kinase activity and the conserved ATP-binding pocket.”


Decades of research in the kinase targeting space have resulted in a good understanding of the chemical scaffolds that make selective kinase binders and the assays to study their off-target profiles.


At Serna Bio, we believe a data-first approach that involves generating large datasets and novel assays will accelerate our ability to discover drug-like starting points for small molecule discovery programs targeting RNA.


A classical approach to generate these datasets is to[screen diverse chemistry](https://journals.sagepub.com/doi/full/10.1177/2472555219885373) , yet these[HTS screens](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6314024/) have historically had low hit rates (typically between[0.01 and 1%](https://journals.sagepub.com/doi/full/10.1177/2472555219885373) ). Yet, unlike the 1990s, we now have advances in both HTS (for example[ALIS](https://pubmed.ncbi.nlm.nih.gov/31701793/) and[SMM](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6314023/) ) and methods and machine learning to accelerate the timelines to achieve this task.


We set out to accelerate timelines and costs to discover drug like medicinal chemistry starting points to modulate RNA function by targeting specific RNA motifs.


### In summary - we set out to answer the following questions:


- Can we predict whether a small molecule is likely to bind to RNA?
- Are RNA binders fundamentally different from protein binders?
- How much data is required to make a good predictive model for RNA binders?


## RNA binding versus protein binding - are we asking the right question?


A central question in our field is:


> “Do RNA-binding small molecules lie in chemical space covered by publicly available compound libraries, which are geared towards protein-binding chemicals?”.


This question stems from the hypothesis that small molecules that lead to functional interactions with RNA are fundamentally different from protein-binding small molecules.


To test the hypothesis that computational methods can differentiate between RNA-binding small molecules, and protein-binding compounds,[Yazdani et al. 2023](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) and[Deng et al. 2022](https://www.mlsb.io/papers_2022/Predicting_Ligand_RNA_Binding_Using_E3_Equivariant_Network_and_Pretraining.pdf) used computational methods to predict if a model can differentiate between RNA-binding molecules as compared to FDA-approved drugs as a proxy for protein binding molecules. This task is a useful benchmark in the context of identifying a chemical space separation between protein and RNA binders, and the models presented by[Yazdani et al. 2023](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) and[Deng et al. 2022](https://www.mlsb.io/papers_2022/Predicting_Ligand_RNA_Binding_Using_E3_Equivariant_Network_and_Pretraining.pdf) statistically differentiate between compounds in these categories well. That being said, are RNA binders different from FDA-approved drugs?


Recent work by[Fang et al. 2023](https://pubmed.ncbi.nlm.nih.gov/37653232/) shows that FDA-approved drugs bind to RNA, in a cellular context, and recent work by our team,[Allen et al 2024](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.abstract) , ** shows that one can define physicochemical rules (known as STaR rules) that suggest a molecule is likely to bind to RNA as shown in Figure 1.


*Figure 1: A box and swarm plot showing the hit rate variation between different compound classes each screened against 35 RNA targets. Each point represents the hit rate (the percentage of compounds found to be binders) for that compound class against a specific RNA target. Boxes and whiskers represent the distributions of these hit rates and the median hit rates are shown at the 50th percentile lines.*


Applying these rules to RNA targeting data and FDA-approved molecules shows an overlap, even suggesting that some FDA-approved molecules may be viable candidates for RNA targeting drug discovery.


Building upon this, we wanted to ask if we can differentiate RNA binders, not from FDA-approved compounds, but from RNA non-binders. A key constraint to effectively address this task was a data set that contained both RNA binders and non-binders. Until the publication of the ROBIN data set, there was a lack of RNA non-binder data in the public domain. The work by[Yazdani et al. 2023](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) provides the largest publicly available data set of small molecule binders to RNA ([ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) ) which is a vital resource for the development of machine learning (ML) models in this space.


## Can machine learning differentiate between RNA-binders and non-binders?


For our work, we trained ML models on both the[Serna Bio internal dataset](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.abstract) and the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) dataset. The task given to the models was to classify whether a new molecule is an RNA binder or a non-binder. (We also note the work of others to provide high-quality open data sets in this space:[Inforna](https://www.nature.com/articles/s41573-022-00521-4) ,[RBIND](https://pubs.acs.org/doi/abs/10.1021/acschembio.2c00224) and[DRTL](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10418101/) ).


**Here we present the best-performing models using:**


- Public data, the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) dataset ~26,000 small molecules (~900,000 total data points)
- Serna Bio’s large internal dataset ~181,000 small molecules (~2.4 million total data points)


We trained models on the two data sets above independently, trialing different ML model architectures and chemical representations, including chemical informatics fingerprints, such as[ECFP](https://pubs.acs.org/doi/abs/10.1021/ci100050t) , and machine learning representations from[graphs](https://arxiv.org/abs/1905.12265) and[language](https://dl.acm.org/doi/abs/10.1145/3307339.3342186) -based chemical embeddings. We found that the simple and efficient logistic regression models performed well on this task and trained them as an ensemble of five models on each data set. We then asked the following question of our models: *does the amount of data affect how well we can make such a prediction?*


The models were evaluated using the[Receiver Operating Characteristic](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) (ROC) and[Matthews correlation coefficient](https://en.wikipedia.org/wiki/Phi_coefficient) to assess the model’s performance. In this blog, we will focus on the more commonly used ROC metrics.


### What we discovered


#### TL/DR: We confirmed that the size of the dataset significantly impacts the quality of the model, a trend previously seen in other domains.


We have compared models trained on the larger[Serna Bio internal dataset](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.abstract) to those trained on the smaller public data set,[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) . Figure 2 shows the change in performance in the form of ROC curves for the two best-performing model ensembles based on training from either the[Serna Bio internal dataset](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.abstract) (left Figure 2) or the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) data set (right Figure 2). In each case, we display the ensemble of five models' independent prediction results and their mean in turquoise. The means are surrounded by a two-standard deviation band calculated across the five models in grey


*Figure 2: A comparison of ROC curves for the best logistic regression models trained on the Serna Bio proprietary data (left) where the model achieves an MCC of 0.152 ∓ 0.006 on the test set and*[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) *data (right) where the model archives an MCC of 0.105 ∓ 0.013. We see clearly that there is an overall improvement in the Serna Bio data model ROC curve and a tighter two-standard deviation band around the ROC curve showing a much smaller variability over bootstrap samples of the training data.*


Although expected, it is clear that the different data sets are resulting in ML classifiers with different levels of statistical performance. In all aspects, apart from the dataset, the methods presented here are equivalent. Therefore, we conclude the improvement, shown in the increased ROC AUC score, using the larger[Serna Bio internal dataset](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.abstract) (mean ROC AUC = 0.769) compared to[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) (mean ROC AUC = 0.632) is due to the data set size. This has been seen in other fields previously, however, the RNA small molecule targeting field is relatively data-poor therefore it is important to establish the trend in improving model quality with data set size and quality is also found in our domain.


This provides evidence that, as an industry, we need to continue to generate datasets to improve our understanding and modeling of RNA-small molecule interactions, as this work shows that larger data sets improve the statistical performance of ML models in this domain.


## Can we improve the hit rate of a primary screen against RNA targets?


Based on the promising results in figure 2, which show the model trained on the Serna Bio data set - which we refer to as the Serna Bio ML model - can effectively differentiate between RNA-binders and non-binders, we set out to explore if we can use this model to effectively rank the “hits” from a novel library for new targets.


Here we used the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) library as the external validation set. This means that the ML models trained using the Serna Bio data were applied to each RNA target in the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) dataset. We asked how often the model correctly classifies the molecules in the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) data set as binders or non-binders for each RNA target in the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) data set.


### What we discovered


TL/DR: We demonstrate that the hit rate at an RNA target can be significantly increased compared to the original ROBIN results utilizing the Serna Bio ML model discussed above when applied to the previously unseen[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) data set.


Using the Serna Bio ML model, we have taken each molecule in the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) data set and predicted whether it is a binder or not. We then calculate the hit rates from these predictions at each RNA target in the ROBIN data set. We find that these hit rates are significantly higher, based on a Wilcoxon signed rank sum test compared to the original[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) results. This means that using the Serna Bio ML model the median per target hit rate is improved by 0.16%, which is approximately a 25% improvement on the original[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) per target hit rate. It should be noted that[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) : i) covers different chemical space than the Serna Bio dataset, ii) tests against different RNA targets than the Serna Bio dataset and iii) is collected using a different experimental technique to the Serna Bio dataset - making this a good generalizability test for our models.


Figure 3 highlights the impact of using the Serna Bio ML model compared to other screens that have been run internally at Serna Bio. We show on the x-axis three different screening scenarios: diversity screening, a commercial RNA targeting library and the Serna Bio ML model applied to ROBIN as discussed above. The y-axis plots the hit rate at each RNA target considered in the screens. We display three box plots, one per screen, which highlight the distribution of hit rates across RNA targets within a screen.


1. The first box plot on the left-hand side represents a standard diversity screen. Here we screen a diverse set of small molecules against two RNA targets.
2. The second box plot in the center represents the application of a commercial RNA targeting library to 30 RNA targets.
3. The third box plot on the right-hand side represents the hit rate distribution when the Serna Bio ML model is applied to the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) data set


We have overlaid a scatter plot highlighting each RNA target’s hit rate (the points) within this distribution. We see a boost in the median hit rate from the application of the Serna Bio model.


*Figure 3: Displays the improved median hit rate performance across the initial Serna Bio screen, commercial RNA targeting screen and Serna Bio model. We see that the Serna Bio model on average achieves a higher hit rate compared to the other screening scenarios. The median hit rate is displayed as a thick black line in the boxes. The whiskers represent 1.5 times the interquartile range.*


The median hit rate of the commercial RNA targeting library displayed here is 0.15%, applying our ML method to ROBIN we achieve a median hit rate of 0.85%, this means that on average we can increase the hit rate by 0.7%. If we rank the molecules based on the probability score from the Serna Bio ML model we find 50% of the binders in the ROBIN dataset in the top 40% of the molecules.


Ultimately this translates into a reduction in time and cost to locate an RNA binder. It is important to note that binding does not imply function, however, it is an important step in identifying active molecules that target RNA for drug discovery.


At Serna Bio, we will be pursuing the use of machine learning and AI models together with physicochemical rules,[known as the STaR rules](https://www.biorxiv.org/content/early/2024/02/02/2024.01.31.578268) , from our previous work to guide our selection of molecules.


## Other Work:


Although we do not cover all the work in this space, as a resource for the community, we have shared a selection of recent papers that have been published in this space:


**Paper**


**Key points**


[Wicks et al. 2023: Probing Bioactive Chemical Space to Discover RNA-Targeted Small Molecules](https://www.biorxiv.org/content/10.1101/2023.07.31.551350v1)


Probing Bioactive Chemical Space to discover RNA-targeting small molecules. The work shows a method for selecting small molecules as RNA binders using similarity/dissimilarity measures and a kNN model.


[Deng et al. 2022: Predicting Ligand – RNA Binding Using E3-Equivariant Network and Pretraining](https://www.mlsb.io/papers_2022/Predicting_Ligand_RNA_Binding_Using_E3_Equivariant_Network_and_Pretraining.pdf)


Uses the[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/ange.202211358) data set to fine-tune a[uni-mol](https://docs.google.com/document/d/1kmE185p1t3_-kMrZJBQqDuwHPoSL53BPJagIQI-T8cA/edit#heading=h.vghuv8kja68m) neural network model(s) aiming to predict RNA binding and other properties.


[Grimberg et al. 2022: Machine learning approaches to optimize small-molecule inhibitors for RNA targeting](https://d-nb.info/125730173X/34) :


This work showed the use of ML models for the optimization of small-molecule inhibitors targeting RNA using docking


[Sun et al . 2020: RLDOCK: A New Method for Predicting RNA–Ligand Interactions](https://pubs.acs.org/doi/abs/10.1021/acs.jctc.0c00798)


A method for RNA docking, which takes structures and input parameters to estimate docking free energy


[Yan et al. 2020: Graph neural representational learning of RNA secondary structures for predicting RNA-protein interactions](https://academic.oup.com/bioinformatics/article/36/Supplement_1/i276/5870515)


RNA graph embedding code used to predict RNA Binding Protein (RBP) interactions


[Kazan et al 2010: RNAcontext: A New Method for Learning the Sequence and Structure Binding Preferences of RNA-Binding Proteins](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000832)


This work trains a model on RNA-protein interaction strengths based on the mutation of RNA sequences


[Oliver et al . 2020: Augmented base pairing networks encode RNA-small molecule binding preferences](https://academic.oup.com/nar/article/48/14/7690/5870337)


Augmented base pairing networks encode RNA-small molecule binding preferences outputting a fingerprint representation describing molecules likely bind at a given RNA structure


[Rizvi et al. 2020: Targeting RNA with Small Molecules: Identification of Selective, RNA-Binding Small Molecules Occupying Drug-Like Chemical Space](https://journals.sagepub.com/doi/full/10.1177/2472555219885373)


This work describes the experimental screening of a set of molecules for RNA binding propensity. It follows with a detailed data analysis and the generation of Gaussian Naive Bayes models for binding predictions.


*Table 1: This table provides links to recent papers in the RNA targeting domain and provides a short very high-level summary.*
