---
schema_version: "1.0.0"
document_id: "4677e2a1fbf3f6e299143f5ddf6f383934315ed2766b631867fb8334cce486a0"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/the-quest-for-better-protein-ligand-interaction-modeling---improving-ml-methods-offer-promise-for-ai-drug-discovery"
published_at: "2025-08-25T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:218d9cb77b26622aa7b1c0f2421630a1fe1293ba113745d2fbf80acdf6767d4c"
---

# The Quest for Better Protein-Ligand Interaction Modeling – Improving ML Methods Offer Promise for AI Drug Discovery

Machine learning models are helping researchers predict how a potential drug molecule might "dock" with a target protein in the body. But[a recent paper in the Journal of Cheminformatics](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-025-01011-6) from researchers at Recursion finds that these models often focus on how a molecule is positioned while overlooking its ability to make precise protein-ligand interactions. Missing this critical information leads to a gap between academic benchmarks and real-world drug design.


While the paper highlights the need for improved protein-ligand interaction modeling in AI models, the field is rapidly evolving in that direction, says[David Errington](https://www.linkedin.com/in/david-errington-a0040374/) , Senior AI Research Scientist at Recursion, and one of the study’s authors. New models like[Boltz-2](https://www.rxrx.ai/boltz-2) – developed by[Massachusetts Institute of Technology](https://www.linkedin.com/school/mit/) in partnership with Recursion – are already making enormous progress in modeling not only protein structure, but protein binding affinity, and catching up to traditional physics-based modeling.


“This is new, exciting technology,” says Errington, “and we’ve already seen huge progress in the past 18 months – but there’s still work to do.”


### Where Existing AI Models Fall Short


The study highlights that while newer ML cofolding models perform well at predicting the 3D position (or "pose") of a drug molecule in a protein pocket, they often fail to replicate key chemical interactions, like hydrogen bonds, that are an essential part of structure-based drug design.


The researchers used[PoseBusters](https://github.com/maabuu/posebusters/blob/main/docs/source/index.rst) to evaluate several pose-prediction tools – including both classical methods and newer ML methods. By considering the recovery of protein-ligand interactions, they demonstrate that conventional docking metrics can often overestimate model performance, particularly in ML methods.


Specifically, they found that:


- **Classical docking algorithms** , like the well-established GOLD program, consistently outperformed newer ML-based methods in recovering these crucial interactions, because their scoring functions are inherently designed to seek out and reward these connections.
- **ML-based docking models** , like DiffDock-L, often found physically plausible poses with low RMSD (root-mean-square deviation) but frequently missed key interactions that classical methods successfully identified.
- **Cofolding models** , which simultaneously predict both the protein and the ligand, are a nascent technology and are improving rapidly. While early cofolding models perform poorly on this benchmark, Errington says, “it is exciting to see a significant improvement with more modern approaches.”


### Improving ML Model Performance – and their Advantages in AI Drug Discovery


Although ML methods still lag behind classical methods in terms of their understanding of protein-ligand interactions, they have the capacity to offer significant improvements to traditional approaches as their performance continues to improve.


Unlike traditional methods, ML models can enable structure-based drug design in the absence of a crystal structure. Creating a crystal structure of one protein is enormously time- and cost-intensive, says Errington – often “an entire PhD’s worth of work.” Furthermore, he adds, with ML models, the protein is able to fully adapt to accommodate very different ligands – something that would be difficult to achieve in a classical docking campaign.


“This is definitely the direction of travel in the field,” he says, “we just need to ensure we encode the physics correctly."


Boltz-2 was designed to tackle the “binding affinity problem” that plagues early stage AI drug discovery – providing a means to quickly and efficiently estimate the absolute binding free energies (ABFE) of small molecules to proteins without relying on experimental crystal structures. Until now, determining binding affinity computationally has been time- and cost-intensive – requiring atomistic, physics-based simulations like free-energy perturbations, which has represented a major bottleneck in early stage drug discovery.


“Boltz-2 shows real progress when examined under this lens of protein-ligand interaction recovery,” says Errington.


‍


-------------------------


‍


*Author:*[Brita Belli](https://www.linkedin.com/in/brita-belli/) *, Senior Communications Manager at Recursion.*


‍
