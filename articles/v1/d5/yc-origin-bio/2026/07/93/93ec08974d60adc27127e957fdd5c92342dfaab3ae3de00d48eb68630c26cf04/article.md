---
schema_version: "1.0.0"
document_id: "93ec08974d60adc27127e957fdd5c92342dfaab3ae3de00d48eb68630c26cf04"
company_key: "yc-origin-bio"
company: "Origin"
source_id: "yc-origin-bio-news-import-f8108680889a"
canonical_url: "https://origin.bio/blogs/switch/"
published_at: null
first_seen_at: "2026-07-24T07:50:20.443926+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:4970bf4cd7dad4e15d76654d0bf9cb9f79943be4ad42eec7b03584da61b27ffb"
---

# 10,000 AI-Designed Regulatory DNA Sequences, Open for Research

Your browser does not support the video tag.


## 10,000 AI-Designed Regulatory DNA Sequences, Open for Research


March 13, 2026


### For each sequence, we provide


- Predicted activity across all three cell types using[Malinois](https://www.biorxiv.org/content/10.1101/2023.03.05.531189) , an independently validated model from the Broad Institute that predicts MPRA activity from DNA sequence alone.
- Sequence quality metrics including worst hairpin stability (ΔG), GC content, nucleotide composition, and longest homopolymer length.
- Transcription factor binding site annotations that can be filtered and visualized.
- 3D double-stranded DNA structure, using[Protenix](https://github.com/bytedance/protenix) .


**Explore all sequences at[switch.origin.bio](https://switch.origin.bio/)**


Today, we’re releasing 10,000 AI-designed proximal enhancer-like sequences (pELS) across three cell types: SK-N-SH (neuroblastoma), HepG2 (hepatocellular), and K562 (erythroleukemia). Proximal enhancer-like sequences are short DNA elements positioned near gene promoters (~2kb) that boost transcription.


We hope the research community will find these sequences useful in experiments such as MPRA studies and functional assays like ATAC-seq and ChIP-seq. As we experimentally evaluate the activity of these sequences, we’ll share those results on[Switch](https://switch.origin.bio/) — and we’ll enable others to do the same. We aim to continue contributing sequences to this repository, including for primary cells and tissues. The breadth of experiments to run, findings to uncover, and impact to be made go far beyond any single organization.


### Why are we designing and testing these libraries at scale?


We see regulatory elements — nature’s DNA switches and dials — as a means to encode spatiotemporal control over gene expression, making medicines more programmable. The implications will be medicines with increased specificity and the ability to respond to changes in cell-state.


Beyond applications in cell and gene therapies, we see these sequences as a fundamentally new tool for perturbation biology. Traditional perturbation approaches — gene knockdowns, knockouts — are largely binary. Chemical treatments offer dosage control but lack target specificity, hitting multiple pathways simultaneously. Cell fate decisions are driven by the dosage of gene expression: how much of a protein is made, in which cells, and when.


By designing libraries of regulatory elements with graded transcriptional strengths, we can titrate a gene’s expression across a continuous range and measure how the transcriptome reshapes in response. This moves perturbation biology from asking *“what happens when we remove gene X?”* to asking *“what happens as we dial gene X from low to high expression in disease cells?”*


### Consider the questions this enables.


How does varying IL-10 dosage reprogram tumor-infiltrating lymphocytes toward stemness? Recent work has shown that T cell fate in the tumor microenvironment is governed by a[metabolic-epigenetic axis](https://www.science.org/doi/10.1126/science.aau0135) : elevated extracellular potassium in necrotic tumors triggers functional caloric restriction, depleting nucleocytosolic acetyl-CoA and stripping activating histone marks from effector and exhaustion gene loci — thereby preserving the stem-like T cell state most associated with durable anti-tumor responses.


> IL-10 appears to work through a parallel route — reprogramming terminally exhausted CD8+ T cells to restore anti-tumor function.


But while we know both signals can shift T cell fate, we don’t know the dose-response landscape for either.


> Is there an expression threshold at which IL-10 flips exhausted TILs toward a more favorable state without overshooting into immunosuppression? Graded regulatory elements let you map that landscape systematically rather than guessing with a single overexpression construct.


Or consider the broader challenge of T cell therapy. Clinical data show that the two strongest predictors of efficacy are T cell stemness — measured by markers like TCF7 expression and the[CD39⁻CD69⁻ phenotype](https://www.sciencedirect.com/science/article/pii/S0952791521001278?via%3Dihub) — and polyclonal tumor reactivity across multiple neoantigens. Yet during ex vivo expansion, both properties erode: tumor-reactive clonotypes are selectively lost and the remaining cells terminally differentiate.


> What if you could use titrated expression of stemness-associated transcription factors like TCF7 or BACH2 during expansion to find the precise dosage that preserves self-renewal without sacrificing effector potential?


These are the kinds of questions that become experimentally tractable when you have libraries of regulatory elements spanning a continuous range of transcriptional activity in specific cell types — and that’s exactly what we’re building.


### Explore the sequences at[switch.origin.bio](https://switch.origin.bio/) .
