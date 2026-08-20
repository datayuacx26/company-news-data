---
schema_version: "1.0.0"
document_id: "5591637a198543542b6026484cf124c97e1773b1a5f1c8a7ab58d831b6f419e3"
company_key: "yc-tamarind-bio"
company: "Tamarind Bio"
source_id: "yc-tamarind-bio-rss-96b2a54954a5"
canonical_url: "https://tamarindbio.substack.com/p/high-throughput-computational-protein"
published_at: "2024-07-08T23:41:47+00:00"
first_seen_at: "2026-07-24T03:17:16.497895+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:c611150f62628a8e9fb3baa078134c8ef9b3a239244177941514e88bb2d0a545"
---

# High Throughput Computational Protein-Protein Interaction Screening with AlphaFold

# High Throughput Computational Protein-Protein Interaction Screening with AlphaFold


### Predict binding affinities between protein sequences in a massively parallel manner, no programming required!


[Deniz Kavi](https://substack.com/@denizkavi)


Jul 08, 2024


DeepMind’s AlphaFold series of models has revolutionized protein structure prediction for both complexes and monomers. One emergent consequence of the architecture of the AlphaFold is that the metrics used to score the quality of the prediction often strongly correlate with biologically relevant metrics as well.


Most important for the purposes of this article, the


[interface predicted template modeling (ipTM) score](https://www.ebi.ac.uk/training/online/courses/alphafold/inputs-and-outputs/evaluating-alphafolds-predicted-structures-using-confidence-scores/confidence-scores-in-alphafold-multimer/) produced for each complex is an indicator for binding affinity. This way, chains can be paired together into complexes (e.g. “chainA:chainB“) fed into AlphaFold, then along with the predicted structure for this complex, AlphaFold will produce metrics corresponding to its confidence in the prediction.


Per the AF2 authors, ipTM values above 0.8 are understood to be high confidence and those below 0.6 are failed predictions. This confidence is usually correlated with binding affinity, and the feasibility of this complex structure prediction. Results in this 0.6-0.8 range warrant separate inspection. It’s also important to note false positives can arise from using AlphaFold in this way, where domains may be inaccurately swapped or the structures may be predicted to bind somewhere where they are known not to interact. Feel free to contact us at info\[at\]tamarind.bio if you’d like help with this process!


This approach of using AlphaFold as an in silico screen has seen a sizeable amount of papers, including


[discovering targets for peptides](https://pubmed.ncbi.nlm.nih.gov/36993313/) ,


[antibody affinity maturation](https://www.sciencedirect.com/science/article/pii/S0141813023026272) , and


[predicting cross-kingdom interactions at the plant-pathogen interface](https://www.nature.com/articles/s41467-023-41721-9) .


**Quickly, Easily and Securely Do an In Silico PPI Screen on Tamarind**


Each AlphaFold prediction can take up to a few hours running on a cloud computing environment. Tamarind supports running each prediction in parallel, reducing PPI screen times by up to a hundred fold.


Simply upload a fasta file, then get binding affinities for all of your complexes as an ipTM score.


Tamarind Bio supports up to tens of thousands of structure/binding predictions using AlphaFold. Try it out for free at a small scale:


[https://www.tamarind.bio/batch](https://www.tamarind.bio/batch) or get in touch at info\[at\]tamarind.bio to learn more for larger use cases. We support both complex structure predictions and monomers!


A guest post by


[Deniz Kavi](https://substack.com/@denizkavi?utm_campaign=guest_post_bio&utm_medium=web)


Co-founder at Tamarind Bio


[Subscribe to Deniz](https://denizkavi.substack.com/subscribe?)
