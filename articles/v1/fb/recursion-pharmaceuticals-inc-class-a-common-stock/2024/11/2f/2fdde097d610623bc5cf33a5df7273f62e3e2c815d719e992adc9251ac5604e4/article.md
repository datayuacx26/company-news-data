---
schema_version: "1.0.0"
document_id: "2fdde097d610623bc5cf33a5df7273f62e3e2c815d719e992adc9251ac5604e4"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/introducing-mole-a-new-model-for-predicting-molecular-properties-for-ai-drug-design-and-beyond"
published_at: "2024-11-13T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:e33d70ce6f0e320b02100264b46dd7d36872a084917b388e19ce27523d4a56f3"
---

# Introducing MolE: A New Model for Predicting Molecular Properties for AI Drug Design and Beyond

When medicinal chemists are engaged in drug discovery tasks like hit identification, hit-to-lead, and lead optimization, they need to predict properties about the molecules they want to synthesize in order to optimize features like potency and binding affinity while preventing toxicity across a number of different dimensions. The most efficient way to do this is using a trained AI model that can predict the molecule’s biological effects and propose structural changes.


There are a number of these chemical modeling efforts underway, however most of public and private training sets are typically small compared to other fields – in the tens of thousands, versus millions for image recognition labeled examples as with the public image database[ImageNet](https://www.image-net.org/) .


Many existing chemical modeling efforts also use a type of molecular representation known as simplified molecular input line-entry system (SMILES) sequences for training. These sequences rely on a linguistic construct, with a series of characters representing the molecules’ atoms and bonds.


In a[new paper in Nature Communications](https://www.nature.com/articles/s41467-024-53751-y) , researchers[Oscar Méndez-Lucio](https://www.linkedin.com/in/oscarmendezlucio/) ,[Berton Earnshaw](https://www.linkedin.com/in/bertonearnshaw/) , and[Christos Nicolaou](https://www.linkedin.com/in/nicolaouchristos/) offer a new foundation model for chemistry called MolE that relies instead on molecular graphs – visual depictions of the molecules with nodes and edges as opposed to a linear string of characters. MolE was first trained on over 842 million molecular graphs using a self-supervised approach – meaning it didn’t need experimental results since it learns entirely from the chemical structure – and then further fine-tuned on a set of downstream absorption–distribution–metabolism–excretion–toxicity (ADMET) tasks. Ultimately, they demonstrated that MolE outperforms earlier approaches, ranking first in 10 of the 22 ADMET tasks included in the Therapeutic Data Commons (TDC) leaderboard.


MolE initially learns from unlabeled data, allowing researchers to build models with fewer data points, generalizing better, and producing better benchmarks in downstream models than prior methods.


MolE self- supervised approach in which an input atom is masked and the task is to predict the corresponding atom environment of radius 2, i.e. the masked atom plus all the neighboring atoms separated by no more than two bonds.


### Building and Testing the MolE Foundation Model for Chemistry


MolE relies on atom identifiers as input tokens and graph connectivity for positioning information from the molecular graphs. Researchers used this data to pretrain the model – transferring from large unlabeled datasets to smaller labeled datasets by randomly masking atoms and having the model predict the corresponding atom environment of all neighboring atoms separated by no more than 2 bonds. They used both a self-supervised approach with approximately 842 million molecules, followed by supervised pre-training with about 456,000 molecules.


Next, they fine-tuned the model by assessing the quality of its predictions using a set of 22 ADMET tasks included in the TDC benchmark. The[Therapeutic Data Commons](https://tdcommons.ai/) is a resource providing tools, libraries and other resources accessible via an open Python library, that allows researchers to evaluate AI capability across a variety of therapeutic modalities and stages of discovery. This benchmark provided a standard way to compare MolE against other established models, such as those using precomputed fingerprints like[RDKit](https://www.rdkit.org/) or the Morgan fingerprint, convolutional neural networks using SMILES, and versions of graph neural networks like[ChemProp](https://pubs.acs.org/doi/10.1021/acs.jcim.3c01250) .


When compared to the best models in the TDC leaderboard, MolE achieved state-of-the-art performance in 10 of the 22 tasks and was the second-best model on another 4 tasks. The tasks where it performed best included 6 regression tasks and 4 classification tasks, primarily those related to CYP inhibition. CYP enzymes play a major role in metabolizing drugs, and CYP inhibition often leads to drug-drug interactions. While the CYP results are tied to larger datasets, MolE also achieved top performance on tasks with just a few hundred training examples, including predicting half-life and CYP substrates.


The next-best model after MolE, ZairaChem, achieved top performance on only 5 of the 22 tasks.


### What They Learned – And What’s Next


The paper demonstrates that it's possible to use a transformer-based model, MolE, to predict chemical and biological properties directly from molecular graphs using a pre-trained model. Using a two-step pretraining approach – self-supervised followed by supervised – they successfully trained models that outperformed earlier approaches.


So what does this mean for the future of AI drug discovery?


MolE can serve as an important tool in our understanding of how new molecules will perform in the body – and in guiding the design of highly optimized molecules. By training the model to understand atom environments and their relationship to each other, it can also help avoid problems of classical fingerprints such as sparsity and clashes when using bit vectors.


And while only drug-like molecules were used in this research, the researchers anticipate that larger and more diverse datasets can only improve the model’s performance. This work, they believe, represents an important first step towards establishing a foundation model for chemical property prediction.


*Note to researchers: The code to use the model reported in this study is available under the Attribution-Non Commercial 4.0 International License (CC-BY-NC 4.0) in*[https://github.com/recursionpharma/mole_public](https://github.com/recursionpharma/mole_public) *.*


‍


*Author:*[Brita Belli](https://www.linkedin.com/in/brita-belli/) *, Senior Communications Manager at Recursion*


‍
