---
schema_version: "1.0.0"
document_id: "24c8252a2bc9f20b20453d187dfe9bcb18ef54094adae439a5776e591d5c1285"
company_key: "yc-anto-biosciences"
company: "Anto Biosciences"
source_id: "yc-anto-biosciences-news-import-c516c9aa21d4"
canonical_url: "https://anto.bio/publications/microbiome-sparsification"
published_at: null
first_seen_at: "2026-07-24T23:08:26.325303+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:d158a5ecaaa5a5f6a0ff2e1fa9e6c74d3a8e5d4a4978f27c52a7229e147987cc"
---

# Microbiome Sparsification

## tl;dr


Anto is building a multimodal foundation model for microbial communities, making the gut microbiome computable for the first time. We predict drug toxicity and efficacy across diverse populations and optimize molecules for universal efficacy — addressing the microbiome-driven causes of drug response and failures.


## The Key Problem


We caught a cancer drug that worked in China but failed in U.S. trials. Same molecule, different microbiomes. $1B lost.


This isn't an isolated story; more than a billion people are currently on medications where the microbiome dictates success or failure. The gut microbiome drives drug toxicity and efficacy, but pharma lacks the tools to understand and act on it \[10,11,13\]. Current methods to simulate the gut take months, cost millions, and rely on animal models that are too different from humans to be useful.


Biological data is inherently noisy. Microbiome data, in particular, contains 99% noise and only 1% signal that carries predictive power. Machine learning models trained on noisy microbiome data produce unreliable predictions. The data is too sparse, too high-dimensional, and too confounded. Drug development proceeds without understanding how the microbiome will influence outcomes, and billions are spent developing drugs that fail in diverse populations.


## About Anto


We predict drug toxicity and efficacy across diverse populations and optimize molecules for broader efficacy—addressing the hidden, microbiome-driven causes of drug response and failures.


Input any drug molecule. We tell you how people will respond. We show where it works and where it fails—by geography, age, or diet.


We predict:


- Population-specific efficacy (geographic, age-based, vegans vs. meat eaters, etc.)
- Failure mechanisms (specific microbial metabolic pathways) \[9,11\]
- Treatment response profiles across diverse populations \[10,14\]


And we don't just flag risk; we identify failure mechanisms down to microbial metabolic pathways and generate treatment response profiles across populations. Then we optimize the molecule for broader efficacy. Turning 20% efficacy into 80% becomes an optimization problem we can actually solve.


## Our Research


Our approach combines two breakthrough innovations:


1. Multimodal Foundation Models


We've pioneered quality-aware, goal-directed sparsification algorithms \[5,9,15\]. We built the first multimodal foundation model for microbial communities. Instead of treating different data types (genomics, metabolomics, clinical outcomes) separately, our model learns the deep relationships between:


- Microbial community composition and structure
- Metabolic pathways and biochemical transformations
- Drug-microbe interactions at molecular resolution
- Patient outcomes and clinical phenotypes


By integrating multiple data modalities, we capture the causal mechanisms linking microbiome composition to drug response. This isn't correlation—it's mechanistic understanding at scale.


2. Quality-Aware Sparsification


Microbiome data is 99% noise. We developed computational sparsification algorithms that:


- Identify the 0.1-1% of signal that determines outcomes
- Remove noise while preserving predictive power
- Achieve 10-100× performance gains in training and inference
- Enable models to generalize across diverse populations


That's how we make the microbiome computationally tractable. \[6,7,8,14\].


## References


1. N. M. Ghiasi et al., "GenStore: A high-performance in-storage processing system for genome sequence analysis," in Proc. 27th ACM Int. Conf. Architectural Support for Programming Languages and Operating Systems, 2022, pp. 635-654.
2. N. M. Ghiasi et al., "GenStore: In-storage filtering of genomic data for high-performance and energy-efficient genome analysis," in 2022 IEEE Computer Society Annual Symposium on VLSI (ISVLSI), 2022, pp. 283-287.
3. N. M. Ghiasi et al., "GenStore: A high-performance and energy-efficient in-storage computing system for genome sequence analysis," arXiv preprint arXiv:2202.10400, 2022.
4. M.-D. Rumpf, M. Alser, A. E. Gollwitzer et al., "SequenceLab: A comprehensive benchmark of computational methods for comparing genomic sequences," arXiv preprint arXiv:2310.16908, 2023.
5. A. E. Gollwitzer et al., "MetaTrinity: Enabling fast metagenomic classification via seed counting and edit distance approximation," arXiv preprint arXiv:2311.02029, 2023.
6. N. M. Ghiasi et al., "MetaStore: High-performance metagenomic analysis via in-storage computing," arXiv preprint arXiv:2311.12527, 2023.
7. N. M. Ghiasi et al., "MetaStore: High-performance metagenomic analysis via in-storage computing," arXiv e-prints, pp. arXiv-2311, 2023.
8. N. M. Ghiasi et al., "MegIS: High-performance, energy-efficient, and low-cost metagenomic analysis with in-storage processing," in 2024 ACM/IEEE 51st Annual Int. Symp. Computer Architecture (ISCA), 2024, pp. 660-677.
9. A. Gollwitzer et al., "MetaFast: Enabling fast metagenomic classification via seed counting and edit distance approximation," arXiv preprint arXiv:2311.02029, 2023.
10. A. E. Gollwitzer, D. A. Subramanian, I. Tucker, and G. Traverso, "Steering the evolutionary game: Hierarchical control of therapeutic resistance in cancer treatment," in NeurIPS 2025 AI for Science Workshop, 2025.
11. A. E. Gollwitzer, D. A. Subramanian, I. Tucker, and G. Traverso, "MetaOmics-10T: The foundational dataset to unlock causal modeling of microbial ecosystems," in NeurIPS 2025 AI for Science Workshop, 2025.
12. N. M. Ghiasi et al., "MegIS: High-performance, energy-efficient, and low-cost metagenomic analysis with in-storage processing," arXiv preprint arXiv:2406.19113, 2024.
13. H. J. Haiser and D. de Gruijl, "Emerging tools and technologies for microbiome-aware drug development," Clinical Pharmacology & Therapeutics, 2025, doi:10.1002/cpt.70026.
14. A. Gollwitzer et al., "AI-controlled metagenomics: High-throughput taxonomic profiling for clinical pathogen detection and early-stage cancer screening and type classification," arXiv preprint arXiv:2311.02029, 2025.
15. A. Gollwitzer et al., "The Thinking Microscope: A reinforcement learning framework for the co-optimization of computational and generative data sparsification in metagenomics," arXiv preprint arXiv:2311.02029, 2025.
16. A. E. Gollwitzer, "Decoding the human microbiome through AI-controlled metagenomics: Promises and ethical implications," presented at ETH Zurich ETHix Series, Dec. 2024. \[Online\]. Available[here](https://ethz.ch/en/the-eth-zurich/education/innovation/launchpad/ETHix.html) .
