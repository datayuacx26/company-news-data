---
schema_version: "1.0.0"
document_id: "406ca6cb8b3f1d1dfb5d12f19a41669bc9557ac537e22dbfc12a75824e261f6f"
company_key: "yc-tamarind-bio"
company: "Tamarind Bio"
source_id: "yc-tamarind-bio-rss-96b2a54954a5"
canonical_url: "https://tamarindbio.substack.com/p/zero-shot-mutations-for-antibody"
published_at: "2024-08-29T18:08:04+00:00"
first_seen_at: "2026-07-24T03:17:16.497895+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:3f41166959ed9d88ff8db81ce72c755fa5e97d83fe4bd40dd3c3c7442e45a615"
---

# Zero-Shot AI-Guided Mutations For Antibody Fitness Optimization

# Zero-Shot AI-Guided Mutations For Antibody Fitness Optimization


### Two Stanford papers demonstrate improved antibody binding affinity, enzyme activity, and antibiotic resistance solely using AI models trained on general protein sequences.


[Deniz Kavi](https://substack.com/@denizkavi)


Aug 29, 2024


Antibody discovery campaigns typically start by collecting a set of binders to an antigen using an experimental screen such as a phage display. Then, the task becomes optimizing affinity to the antigen of interest while maintaining developability properties. An experimental approach would include directed evolution, i.e. iteratively testing variants produced through mutagenesis. This blogpost investigates two papers that massively increase the throughput of evolving fitter proteins by using protein language models.


Reading


[Shanker et. al.](https://www.science.org/stoken/author-tokens/ST-1968/full) ’s Editor’s summary, I was impressed by the comment “In experimental screens of virus-neutralizing antibodies, the authors observed substantial improvement in binding affinity and neutralization for their predicted sequences”, especially since their ML model hadn’t been trained on any data outside of publicly available, general protein sequences.


The implication here being that language model confidence(or an ensemble of multiple) alone is sufficient to not only recommend biologically sound mutations, but also improve the fitness of a protein on tasks without specifically being trained on it. Through only 2 rounds of evolution no less.


Another paper by a subset of the authors of


[Shanker et. al.](https://www.science.org/stoken/author-tokens/ST-1968/full) ,


[Hie et. al.](https://www.nature.com/articles/s41587-023-01763-2) , finds that language model-guided substitutions “improved the binding affinities of four clinically relevant, highly mature antibodies up to sevenfold and three unmatured antibodies up to 160-fold”. This suggests that training language models on all proteins available can lead to emulating evolutionary pressures as seen in mutagenesis.


And while the authors focus antibody affinity maturation and neutralization tasks, the approach should by definition be applicable any loosely defined “fitness” optimization task.


[Hie et. al.](https://www.nature.com/articles/s41587-023-01763-2) show the example of enzyme kinetics improving from 3 to 20% for a alkaline phosphatase.


Thanks for reading Tamarind Bio Blog! Subscribe for free to receive new posts on protein design, engineering and easy-to-use structural bioinformatics.


Interestingly, the majority of mutations produced through the language modeling approach were in framework regions, where traditional approaches almost always focus on CDRs. This is a promising result in that AI-guided approaches may not necessarily replace experimental approaches, but rather unlock alternatives that traditional methodologies can’t feasibly attempt.


Both approaches discussed are easily accessible through the


[Tamarind](https://www.tamarind.bio/) web platform for any type of protein inputs. Take a look below and get in touch at info@tamarind.bio to learn more on how to securely optimize your proprietary sequences!


Evolution starting from structure:


[https://www.tamarind.bio/structural-evolution](https://www.tamarind.bio/structural-evolution)


Evolution starting from sequence:


[https://www.tamarind.bio/antibody-evolution](https://www.tamarind.bio/antibody-evolution)


A guest post by


[Deniz Kavi](https://substack.com/@denizkavi?utm_campaign=guest_post_bio&utm_medium=web)


Co-founder at Tamarind Bio


[Subscribe to Deniz](https://denizkavi.substack.com/subscribe?)
