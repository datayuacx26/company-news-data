---
schema_version: "1.0.0"
document_id: "73fa7ae265786bae10855fbca53d6888d3bf28d3c0490f4875b1d4358c87338b"
company_key: "yc-harmonic-discovery"
company: "Harmonic Discovery"
source_id: "yc-harmonic-discovery-rss-ada272b06e31"
canonical_url: "https://news.harmonicdiscovery.com/presenting-at-the-american-chemical-society-fall-2022-meeting-f3ed26e75f23"
published_at: "2022-09-08T02:09:35+00:00"
first_seen_at: "2026-07-25T07:36:23.550190+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:0481ca02f98b31662e0f5f6ecfda0b328e2992c1ef1a517d69f8ea94398fecc3"
---

# Presenting at the American Chemical Society Fall 2022 Meeting

# Presenting at the American Chemical Society Fall 2022 Meeting


[Rayees Rahman](https://medium.com/@rayees_76660?source=post_page---byline--f3ed26e75f23---------------------------------------)


5 min read


·


Sep 8, 2022


--


Press enter or click to view image in full size


Harmonic students Annalise Schweikart, Katalina Biondi and Jason Lin at ACS Fall 2022 Conference in Chicago


At Harmonic Discovery, we strongly believe in not only performing rigorous science but also presenting and sharing our discoveries with broader community. That’s why we were excited to present three talks at the American Chemical Society Fall 2022 meeting!


The talks shared several key components of our drug discovery platform. Most importantly the work was led and presented by three Harmonic Discovery Student Interns.


We’ve included the abstracts and pictures of their talks here.


Press enter or click to view image in full size


[Leveraging multiple data types for improved compound-kinase bioactivity prediction](https://acs.digitellinc.com/acs/live/28/page/905/1?eventSearchInput=ryan+theisen&eventSearchDateTimeStart=&eventSearchDateTimeEnd=#sessionCollapse442502)


Presented by Ryan Theisen


Compound activity against a kinase is typically determined either from a single dose of compound (e.g., percent inhibition or activity readouts) or more comprehensive and costly multi-dose profiling (e.g., Kd, Ki or IC50 readouts). Standard approaches to compound-kinase activity modeling utilize primarily multi-dose data, ignoring a large space of compounds with single-dose data points only (~40% of kinase activities in ChEMBL).


To overcome these limitations, we developed a two-stage machine learning framework for compound-kinase activity prediction that integrates both single- and multi-dose experimental readouts. In the first stage, we use a random forest to learn a mapping from multiple single-dose to multi-dose activity values. This model is then used to generate proxy multi-dose activity labels for compounds with multiple single-dose measurements only. The predictions from the first-stage model are then integrated along with measured multi-dose activities to predict compound-kinase interactions from chemical structures, using ECFP4-based Tanimoto kernel ridge regression. The two-stage model incorporates sample weighing to prioritize experimentally determined labels over predicted labels.


## Get Rayees Rahman’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Models were fit using 5-fold cross validation. Here, we restrict each kinase to 100 labeled multi-dose compound examples for the baseline model (using no single-dose examples), and an additional 100 single-dose compound examples for the two-stage model. We observed that our two-stage model performs significantly better (p-value = 5.96e-18) than a model trained on multi-dose labels alone (Fig. 1, RMSE on a held-out test set). Taken together, our results indicate benefits of incorporating a diverse array of data types for predicting compound-kinase bioactivity. We anticipate that these results will enable more efficient and economical development of activity datasets for kinase drug discovery.


Press enter or click to view image in full size


[Accurate prediction of adenine pocket kinase inhibitor substructures by integrating machine learning and expert curation](https://acs.digitellinc.com/acs/live/28/page/905/1?eventSearchInput=Katalina+Biondi&eventSearchDateTimeStart=&eventSearchDateTimeEnd=#sessionCollapse442624)


Presented by Katalina Biondi


Kinase inhibitors (KI) are a proven therapeutic class in oncology. However, the clinical efficacy of KIs is often limited by off-target interactions that can cause adverse events. KIs are a well-defined chemotype that have critical interactions in the active site of their on- and off-targets. For instance, adenine pocket (AP) fragments can be identified by having specific hydrogen bond donor and acceptor features. Other components of a KI often include solvent-exposed (SE), front-pocket (FP), gate keeper (GA) and back pocket (B1/2) regions that contribute to provide ligand-receptor interactions. Development of KIs with optimized binding profiles can be improved by labeling inhibitor fragments and their connectivity.


In this work, we train a classifier to categorize KI fragments as one of seven labels based on the KinFragLib dataset, prioritizing AP fragments. We first identify unique fragments from the KinFragLib dataset using the dice similarity score based on Morgan fingerprints. We then classify the fragments to one of the seven labels by using two separate machine learning models: random forest and logistic regression. Both models were trained on chemical fingerprints of each fragment. We then evaluate if an expert-curated set of descriptors improves the performance of each model in distinguishing AP fragment labels. These descriptors capture key AP features such as lipophilicity, number of fragment exit vectors, and the number of aromatic rings.


Our results show that the expert descriptors alone perform comparably to chemical fingerprints. Importantly, integration of the descriptors with fingerprints improves the F1-score (.74) for AP binders. The most important features for the model are clogP, the sum of hydrogen bond donors and acceptors, and the number of heavy atoms. These favorable results motivate future work to construct curated descriptors capturing specific characteristics of the other subpocket pools.


Press enter or click to view image in full size


[Common Scaffold Visualizer (CSViz): A computational framework to enable interactive analysis and design of multi-targeted kinase inhibitors](https://acs.digitellinc.com/acs/live/28/page/905/1?eventSearchInput=common+scaffold+visualizer&eventSearchDateTimeStart=&eventSearchDateTimeEnd=#sessionCollapse442672)


Presented by Jason Lin


Agents targeting multiple proteins are known to circumvent efficacy and drug resistance-related limitations in comparison to highly selective drugs. On the other hand, these therapeutics often have unintended off-target interactions giving rise to adverse drug effects. Rational design of agents with targeted polypharmacology requires a detailed understanding of both the chemical space of the targets under investigation and the landscape of the anti-targets. Though there exists a plethora of computer-aided drug design (CADD) tools geared towards creating selective inhibitors, the rational design of multi-targeting agents calls for a novel suite of tools that best accommodates the polypharmacological rationale.


To address this unmet need, we present CSViz, a computational tool that interactively analyzes the chemogenomic landscape of targets and anti-targets to 1) identify scaffolds shared across targets and 2) exclude scaffolds implicated in anti-target binding. CSViz thereby provides a knowledge-driven compound moiety for designing multi-target inhibitors. Given a set of targets and their inhibitors, CSViz estimates pairwise compound similarity and finds the maximum common substructure (MCS). These MCSs are first filtered based on connectivity and ring-based criteria, and later subjected to topological-based clustering. Representative MCSs are identified for each cluster and their enrichment scores are estimated. The resulting MCSs, along with their enrichment scores, enable the identification of initial scaffolds for the design of ligands with multi-targeting activity.


The present case study focuses on identifying polypharmacological agents for kinase targets p38α and EGFR. Representative MCSs shared between inhibitors of p38α and EGFR were identified (Fig. 1). These MCSs were used as seed molecules to virtually screen various vendor libraries. The compounds were found to show favorable docking poses amongst the selected targets. We anticipate that this tool will accelerate the discovery of kinase inhibitors with targeted polypharmacology.
