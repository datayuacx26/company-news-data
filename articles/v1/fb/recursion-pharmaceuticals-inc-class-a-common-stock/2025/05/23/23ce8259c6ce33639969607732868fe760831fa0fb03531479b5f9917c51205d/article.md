---
schema_version: "1.0.0"
document_id: "23ce8259c6ce33639969607732868fe760831fa0fb03531479b5f9917c51205d"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/accelerating-ai-drug-discovery-with-open-source-datasets"
published_at: "2025-05-28T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:0546c4ab58fc4d9e6a2c2be68342ea25552e0e084202f14adfb12a133dd45df7"
---

# Accelerating AI Drug Discovery with Open Source Datasets

Biology has a data problem. To accurately train machine learning models to simulate what will happen to our cells, tissues, and organs in response to disease or medications, we need standardized biological data that captures those changes. We also need data that represents different layers of biology, including different types of cells and key cellular processes across animal and real-world patient data. Only then can machine learning models find connections between how the introduction of one element – a genetic mutation, or a specific drug – can drive changes across the entire body.


But public datasets in biology and chemistry have fallen short when it comes to reliably training AI models for drug discovery. That’s why we’ve not only been building massive fit-for-purpose datasets at Recursion, but releasing smaller open source versions of these datasets to accelerate the field. Our most recent,[RxRx3-core](https://www.rxrx.ai/rxrx3-core) , has been downloaded over 6,000 times since its release in November 2024.


Even though these public datasets represent less than 1% of our total dataset, they are nonetheless part of the critical data infrastructure that powers everything we do. Our proprietary dataset is the reason that, on average, we’re able to advance potential medicines to the clinic[faster and at lower costs](https://www.recursion.com/mission#data) than industry averages. It’s also enabled us to discover[previously unknown targets](https://youtu.be/92tLRpKYaWo) and design potentially first-in-disease and best-in-class[molecules](https://www.recursion.com/pipeline) that are rapidly advancing into and through clinical development. Our data is our differentiator.


### Filling in Public Datasets’ Missing Pieces


A good portion of biological and chemical data today comes from public datasets like GenBank, the Cancer Genome Atlas, the UK Biobank, ChEMBL, and the Protein Data Bank as well as scientific literature in resources like PubMed and PLOS ONE. These sources provide important insights into genetic drivers, chemical properties, and clinical data around specific diseases, but can be contaminated, lack standardization, and be biased toward certain populations or protein types.


At Recursion, we’ve been building our own proprietary biological datasets for over a decade to help fill in those missing pieces – and we share a subset of those fit-for-purpose datasets with the broader scientific community via[publicly available datasets](https://www.rxrx.ai/) in order to accelerate AI drug discovery research.


“This is really important in our industry because we are 100% data driven,” says[Nicola Richmond](https://www.linkedin.com/in/nicola-richmond-92973a3/) , Chief Scientist, AI at Recursion.


Our in-house generated data provides a strong training basis for building foundation models which we use to solve problems in early drug discovery. In our automated wet lab, which is outfitted with robotic equipment, microscopy, and other advanced technology, we run millions of experiments per week with HUVEC (human umbilical vein endothelial cells), taking pictures of cells that have been perturbed via CRISPR-Cas9 editing along with reagents and compound treatments at different points along the assay. This gives us insights into which genes radically alter the cell when knocked out and which molecules via incubation do similar things to the cell. All this data is collected in a highly controlled and standardized way, generating what we call “fit-for-purpose” data that is ideally suited for training AI.


### High Content Microscopy Data for All


We released our first open-source dataset – RxRx1 – in 2019, with more than 100,000 images and 300-plus gigabytes of data. The latest versions include[RxRx3](https://www.rxrx.ai/rxrx3) – a more than 100 Tb dataset which spans more than 17,000 genes (CRISPR knockouts of most of the human genome) and 2.2 million images of HUVEC cells. RxRx3 is one of the largest public collections of cellular screening data generated using a common experimental protocol within a single lab, although it represents less than 1% of Recursion’s total dataset.


Most recently, we released[RxRx3-core](https://www.rxrx.ai/rxrx3-core) on Hugging Face – a more manageable 18Gb version for researchers to benchmark their own microscopy vision models. RxRx3-core contains 222,601 microscopy images spanning 736 CRISPR knockouts and 1,674 compounds at 8 concentrations and image embeddings computed with[OpenPhenom-S/16](https://www.rxrx.ai/phenom) , Recursion’s public foundation model. This dataset was shared in[a recent preprint](https://arxiv.org/abs/2503.20158) – and as a poster at the recent[ICLR](https://iclr.cc/) conference in Singapore – and released with an associated drug-target interaction benchmark.


### Better Data, Better Benchmarks


High quality public datasets – and robust benchmarks – are critical to advancing AI drug discovery. They allow pharma companies and academic researchers alike to explore our high quality microscopy cell data, and use it to make new discoveries.


“There’s a lack of accessible datasets and meaningful benchmarks,” says[Oren Kraus](https://www.linkedin.com/in/oren-kraus-5a681b69/) , associate director of machine learning at Recursion. “While there have been previous efforts to release smaller HCS datasets for benchmarking, they had various challenges.” As noted in the paper, these challenges included datasets with too small a number of perturbations or confounders introduced by the experimental design.


“Unlike these previous datasets, RxRx3-core is specifically designed to drive research and discovery in representation learning for HCS data,” Kraus says. Its compact size makes it more accessible, and the related, well-defined benchmarks make it ideal for, as the paper notes, “evaluating zero-shot drug-target interaction prediction directly from HCS images.”


“If we think about some of the successes in AI, for example big models that generate text and images, they are so good because we have a ton of text and digital images sitting in the public domain and those data are really diverse,” says Richmond. “The AI community wouldn’t be able to advance the field without access to lots of data.”


‍


*Author:*[Brita Belli](https://www.linkedin.com/in/brita-belli/) , *Senior Communications Manager at Recursion.*


‍
