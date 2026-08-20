---
schema_version: "1.0.0"
document_id: "c90695a64bdce7eeb0aa1a9a3cc3ee910ba2d307b9ebe6362d6068395d053608"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/iglm-a-generative-language-model-for-antibody-design"
published_at: "2024-12-13T07:00:00+00:00"
first_seen_at: "2026-07-29T22:49:49.320956+00:00"
fetched_at: "2026-07-29T22:49:54.237682+00:00"
content_hash: "sha256:281a1e401ace9bee81dfd8ff1e0600002a7de70a5fe36a404070a9a510eee38f"
---

# A model for generating, infilling and diversifying antibody sequences

As promised in our[previous blog post](https://pipebio.com/blog/protein-language-models) , we intend to introduce our readers to a number of antibody-specific language models (AbLMs) in a series of articles. In this article we present the first one – Immunoglobulin Language Model (IgLM).


## Model definition – In Brief


IgLM is a **** generative antibody-specific language model that leverages bidirectional context for generating, infilling and diversifying antibody sequences, conditioned on chain type and species of origin. It was published in Cell Systems in 2023.


## Training data


IgLM was trained on **558M** non-redundant unpaired antibody Fv sequences from human, mouse, rat, rabbit, rhesus and camel that belong to both heavy and light chains. These sequences are sourced from the Observed Antibody Space (OAS) 1.


Overall, IgLM uses a combination of three strategies during training (Figure 1):


1.


‍ **Sequence masking and rearrangement:** For each antibody, sequence segments of random-length are masked (hidden) during training and appended to the end of the sequence, creating a rearranged sequence. This enables IgLM to learn and eventually predict the masked segments depending on the surrounding amino acid tokens.


2.


‍ **Bidirectionality:** Sequences are presented to IgLM in both directions during training. This enables learning both the left-to-right and the right-to-left contexts and dependencies, while maintaining the autoregressive nature of the model to eventually generate antibody sequences in the correct left-to-right direction.


3.


**Conditioning tags:** Training data is presented to the model accompanied with two tags that inform the model about the species of origin and the chain type (Heavy chain ;VH or Light chain VL) of each antibody sequence in the training set.


**Figure 1.** Overview of IgLM training strategies including sequence masking, bidirectionality and conditioning tags for antibody species and chain type.


## What Can IgLM be used for?


IgLM can:


1.


**Generate** antibodies conditioned to chain type and species of origin.


2.


**Evaluate** the likelihood of an antibody sequence to belong to a specific chain type and species of origin.


3.


**Infill and/or diversify** a certain region of the antibody sequence that is missing or needs improvement.


The infilling and diversification function showed to improve the in silico developability 4,5 and humanness 6 of the original antibody input. Also, the training strategies (Figure 1) helped IgLM to achieve good performance in infilling the CDRs being the most diverse, and hence hardest to fill regions of the antibody sequence. For example IgLM achieved higher certainty (lower perplexity scores) for generated or infilled CDR loops when compared to ProGen2-OAS[7](https://paperpile.com/c/Kkhmgb/2FuA) . Of note, ProGen2-OAS is an antibody-specific language model of similar training dataset size and transformer architecture to IgLM, but it does not support bidirectional training and conditioning tags.8


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Model Inventors and code availability


Richard Shuai, Jeffrey Ruffolo, and Jeffrey Gray at John Hopkins University (USA). The commercial use of IgLM requires a licensing agreement. Model code and installation is provided via this[github repository](https://github.com/Graylab/IgLM?tab=readme-ov-file) . Model publication is available from[Cell Press](https://www.cell.com/cell-systems/abstract/S2405-4712(23)00271-5) .


## References


1. Kovaltsuk, A. et al. Observed Antibody Space: A Resource for Data Mining Next-Generation Sequencing of Antibody Repertoires. The Journal of Immunology 201, 2502–2509 (2018).


2. Radford, A. et al. Language Models are Unsupervised Multitask Learners. (2019).


3. Donahue, C., Lee, M. & Liang, P. Enabling Language Models to Fill in the Blanks. arXiv \[cs.CL\] (2020).


4. Chennamsetty, N., Voynov, V., Kayser, V., Helk, B. & Trout, B. L. Prediction of Aggregation Prone Regions of Therapeutic Proteins. J. Phys. Chem. B 114, 6614–6624 (2010).


5. Sormanni, P., Aprile, F. A. & Vendruscolo, M. The CamSol method of rational design of protein mutants with enhanced solubility. J. Mol. Biol. 427, 478–490 (2015).


6. Prihoda, D. et al. BioPhi: A platform for antibody design, humanization, and humanness evaluation based on natural antibody repertoires and deep learning. MAbs 14, (2022).


7. Shuai, R. W., Ruffolo, J. A. & Gray, J. J. IgLM: Infilling language modeling for antibody sequence design. Cell Syst 14, 979–989.e4 (2023).


8. Nijkamp, E., Ruffolo, J., Weinstein, E. N., Naik, N. & Madani, A. ProGen2: Exploring the Boundaries of Protein Language Models. arXiv \[cs.LG\] (2022).
