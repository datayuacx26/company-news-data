---
schema_version: "1.0.0"
document_id: "823c4d61c9117facebfa494c22fe2c84684746f4e321297f87a35b349d7a9bb0"
company_key: "yc-nabla-bio"
company: "Nabla Bio"
source_id: "yc-nabla-bio-news-import-60629cd68eb4"
canonical_url: "https://www.nabla.bio/news/test-time-scaling-de-novo-antibodies"
published_at: null
first_seen_at: "2026-07-24T11:57:23.102708+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:88dc0e7f9f438a108c07072f6c9a20d3050004d2040a803ff3dd05a2a3327cf9"
---

# Test-Time Scaling Unlocks a Leap Forward in Antibody Design

Announcing JAM-2: Drug-like antibodies straight from the computer


[Technical Report](https://nabla-public.s3.us-east-1.amazonaws.com/2025_Nabla_JAM2.pdf)


[Platform](https://www.nabla.bio/platform)[Partner](https://www.nabla.bio/partner)[News](https://www.nabla.bio/news)[Team](https://www.nabla.bio/team)[Careers](https://www.nabla.bio/careers)


Announcing JAM-2: Drug-like antibodies straight from the computer


Do you want to be part of the next step in drug design? Get in touch


Partner


bd@nabla.bio


hiring


hiring@nabla.bio


Copyright 2025 Nabla Bio, Inc


Test-Time Scaling Unlocks a Leap Forward in De Novo Antibody Design


Blog, 05.12.25


At Nabla Bio, we've developed a breakthrough approach to antibody design using test-time scaling in our AI system called JAM (Joint Atomic Modeling). By allowing our AI to "think longer" through a process we call "introspection," we've created antibodies de novo targeting notoriously difficult receptors (GPCRs) with therapeutic-grade properties—achieving binding success rates 20-70 times higher than previous methods. Most remarkably, we discovered the first antibody activators for a receptor called CXCR7, then used a single successful example to steer JAM to design hundreds more diverse and improved variants, demonstrating how AI can efficiently learn from minimal experimental data to solve complex drug design challenges.


## Where We Are Today


Protein design has seen remarkable advances in recent years. AlphaFold2 revolutionized our ability to predict protein structures, while generative AI models have begun creating novel proteins with specific functions. However, designing antibodies—especially ones targeting complex membrane proteins like GPCRs—has remained a significant challenge.


GPCRs (G protein-coupled receptors) are involved in numerous physiological processes and represent about one-third of all drug targets. Yet most GPCR-targeting drugs are small molecules or peptides, which often suffer from limited selectivity and off-target effects. Antibodies would offer superior specificity and longer half-lives, but they've been largely restricted from the GPCR therapeutic landscape due to technical hurdles with traditional discovery approaches.


In our earlier work at Nabla, we demonstrated the feasibility of computational antibody design against GPCRs, but fell short of therapeutic relevance. Our system produced a single GPCR-binding antibody from 20,000 designs, but it had modest affinity, poor developability properties, and no functional activity—interesting scientifically, but far from clinically useful.


## Our Advances


### Test-Time Scaling Through "Introspection"


Inspired by recent advances in large language models, we applied the concept of "test-time scaling" to protein design. Rather than making antibody designs in a single computational pass, we developed an approach we call "introspection," where our JAM system generates multiple design proposals, evaluates them, selects the most promising candidates, and repeats this process through multiple rounds of refinement—all computationally, before any wet lab testing.


This approach fundamentally changes what's possible in antibody design. For the SARS-CoV-2 spike protein, six rounds of introspection improved our success rate of nanomolar binders by 22-fold compared to a single design pass. For the more challenging GPCRs CXCR4 and CXCR7, the improvements were even more dramatic.


The resulting antibodies don't just bind—they have properties comparable or superior to clinical-stage molecules:


- Sub-nanomolar affinities (over 10x stronger than a clinical-stage benchmark for CXCR4)
- High selectivity between similar targets
- Strong functional modulation of receptor activity
- Favorable developability profiles (production yield, stability, low polyreactivity)


### Experiment-Guided Steering


Our most significant discovery came when testing the function of our CXCR7-targeting antibodies. While most designs inhibited receptor activity as expected, two activated it—making them the first antibody activators ever reported for CXCR7.


Instead of treating this as merely an interesting finding, we demonstrated a powerful new design paradigm: experiment-guided steering. We took just one of these rare activator antibodies and used it to "steer" JAM to generate semantically similar designs.


The results were remarkable. Without retraining our model, this approach generated 700+ CXCR7-binding antibodies, with 348 showing activator function—a success rate thousands of times higher than our initial discovery. Our top new activators exhibited further improved properties, with some rivaling the potency of CXCR7's natural ligand (which evolution refined over 400 million years).


This demonstrates how AI systems can learn from minimal experimental data (N=1) to solve complex design challenges without expensive retraining.


## The Bigger Picture


### Unlocking New Target Space


GPCRs represent just one example of challenging targets that have been difficult to address with antibodies. Multipass membrane proteins—including ion channels and transporters—comprise approximately two-thirds of cell surface proteins but are targeted by less than 10% of current biologics.


By enabling atomic-precision engineering of binding interfaces with high success rates, our approach could open new therapeutic territories. The ability to not just bind these targets but modulate their function in precise ways (inhibition or activation) creates opportunities for controlling disease biology.


As our de novo hit rates improve, direct screening against native proteins on cells becomes feasible, eliminating the need for artificial screening reagents that introduce false negatives. This would accelerate discovery against challenging membrane proteins.


### Faster Antibody Development


Traditional antibody discovery against complex targets often takes months to years, with multiple iterative campaigns that frequently fail altogether. Our computational-first approach could compress timelines and increase success rates.


For example, discovering antibody activators has historically been challenging, usually requiring extensive engineering after initial discovery, if possible at all. Between our de novo design round and experiment-guided steering round, we generated hundreds of diverse activators in about 8 weeks of work, culminating in 300+ activators with minimal experimental effort.


This acceleration enables us to explore biological hypotheses and mechanisms quickly.


### Looking Ahead


We believe that test-time scaling will become a fundamental "scaling law" for biomolecular design, similar to how it's transforming language model capabilities. By giving generative protein design systems more time to "think" and refine their outputs, increasingly complex design challenges should become solvable without requiring larger models or more training data.


Our future directions include targeting more challenging membrane proteins like ion channels, exploring designs with specific signaling profiles (biased activation), and developing antibodies that can recognize specific receptor conformational states.


As our de novo hit rates continue to improve, we anticipate designing smaller sets of antibodies with high confidence of success, enabling performance assessment in ultra-relevant settings like patient primary cells or potentially even direct to animal models—further accelerating the path to clinically viable therapeutics.


Just as test-time reasoning has transformed what language models can accomplish, test-time scaling in biological design offers a path toward designing therapeutics for previously intractable targets. This emerging paradigm could be the critical piece to advancing AI-guided drug discovery from interesting scientific demonstrations to clinical impact.
