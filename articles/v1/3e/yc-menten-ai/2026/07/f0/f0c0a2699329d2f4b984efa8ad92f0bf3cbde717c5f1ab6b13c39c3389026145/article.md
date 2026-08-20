---
schema_version: "1.0.0"
document_id: "f0c0a2699329d2f4b984efa8ad92f0bf3cbde717c5f1ab6b13c39c3389026145"
company_key: "yc-menten-ai"
company: "Menten AI"
source_id: "yc-menten-ai-news-import-4a0f36dd5218"
canonical_url: "https://menten.ai/news/xenet-paper"
published_at: null
first_seen_at: "2026-07-22T04:02:30.999653+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:9c66ea26246c789e3896a088440509a615590f6b0a42394fd356c1a14a8fbf68"
---

# XENet paper

#### XENet: Using a new graph convolution to accelerate the timeline for protein design on quantum computers


‍


#### Abstract


Graph representations are traditionally used to represent protein structures in sequence design protocols where the folding pattern is known. This infrequently extends to machine learning projects: existing graph convolution algorithms have shortcomings when representing protein environments. One reason for this is the lack of emphasis on edge attributes during massage-passing operations. Another reason is the traditionally shallow nature of graph neural network architectures. Here we introduce an improved message-passing operation that is better equipped to model local kinematics problems such as protein design. Our approach, XENet, pays special attention to both incoming and outgoing edge attributes.


We compare XENet against existing graph convolutions in an attempt to decrease rotamer sample counts in Rosetta’s rotamer substitution protocol. This use case is motivating because it allows larger protein design problems to fit onto near-term quantum computers. XENet outperformed competing models while also displaying a greater tolerance for deeper architectures. We found that XENet was able to decrease rotamer counts by 40% without loss in quality. This decreased the problem size of our use case by more than a factor of 3. Additionally, XENet displayed an ability to handle deeper architectures than competing convolutions.


**Author summary** Graphs data structures are ubiquitous in the field of protein design and are at the core of the recent advances in artificial intelligence brought forth by graph neural networks (GNNs). GNNs have led to some impressive results in modeling protein interactions, but are not as common as other tensor representations.


Most GNN architectures tend to put little to no emphasis on the information stored on edges; however, protein modeling tools often use edges to represent vital geometric relationships about residue pair interactions. In this paper, we show that a more advanced processing of edge attributes can lead to considerable benefits when modeling chemical data.


We introduce XENet, a new member of the GNN family that is shown to have improved ability to model protein residue environments based on chemical and geometric data. We use XENet to intelligently simplify the optimization problem that is solved when designing proteins. This task is important to us and others because it allows larger proteins to be designed on near-term quantum computers. We show that XENet is able to train on our protein modeling data better than existing methods, successfully resulting in a dramatic decrease in protein design sample space with no loss in quality.


‍


#### Competing Interest Statement


I have read the journal's policy and the authors of this manuscript have the following competing interests: VKM and HM are cofounders and shareholders in Menten AI, Inc. JBM is employed by Menten AI with granted stock options. The content of this manuscript is relevant to work performed at Menten AI.


‍


##### Abbreviations


AUC *Area Under Curve (used with respect to ROC)*


*‍* ECC *Edge-Conditioned Convolution*


*‍* FGN *Filter-Generating Network*


*‍* GCN *Graph Convolutional Network*


*‍* GNN *Graph Neural Network*


*‍* RAM *Random Access Memory*


*‍* REU *Rosetta Energy Units*


*‍* ROC *Receiver Operating Characteristi* ‍ *c*


‍


[https://www.biorxiv.org/content/10.1101/2021.05.05.442729v1](https://www.biorxiv.org/content/10.1101/2021.05.05.442729v1)
