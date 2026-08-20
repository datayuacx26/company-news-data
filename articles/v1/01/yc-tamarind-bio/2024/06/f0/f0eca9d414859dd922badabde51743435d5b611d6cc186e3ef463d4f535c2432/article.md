---
schema_version: "1.0.0"
document_id: "f0eca9d414859dd922badabde51743435d5b611d6cc186e3ef463d4f535c2432"
company_key: "yc-tamarind-bio"
company: "Tamarind Bio"
source_id: "yc-tamarind-bio-rss-96b2a54954a5"
canonical_url: "https://tamarindbio.substack.com/p/benchmarking-the-universe-of-protein"
published_at: "2024-06-04T00:12:38+00:00"
first_seen_at: "2026-07-24T03:17:16.497895+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:d299bc7b781cf8dee7ee0a276c16e3ad211957d45f35bb4989548b88a6d4cf63"
---

# Benchmarking The Universe of Protein-Ligand Docking Tools

# Benchmarking The Universe of Protein-Ligand Docking Tools


### An overview of machine learning-based approaches for small molecule docking, and which to pick per task (last updated June 24)


[Deniz Kavi](https://substack.com/@denizkavi)


Jun 04, 2024


The prediction of a ligand’s binding affinity to a protein and the pose in which it binds is highly important for drug discovery. With this comes a growing number of approaches to computationally predict these attributes.


This article draws from the


[POSEBENCH](https://www.arxiv.org/abs/2405.14108) ,


[PoseBusters](https://arxiv.org/abs/2308.05777) , and


[Astex Diverse](https://pubs.acs.org/doi/abs/10.1021/jm061277y) benchmark among others to evaluate the efficacy of recent approaches to predicting protein-ligand complexes.


**Known target structure + known binding site**


Conventional approaches to protein-ligand docking typically involve providing the tool a binding site location (typically in terms of coordinates) along with the structures of the protein and ligand.


Until recently, physics based approaches such as


[AutoDock-Vina](https://www.tamarind.bio/autodock-vina) were the gold standard for docking to a known binding site, far outperforming ML based approaches such as


[DiffDock](https://www.tamarind.bio/diffdock) . However, the release of


[Uni-Mol Docking v2](https://www.tamarind.bio/unimol2) in May 2024 changed this, improving upon conventional approaches significantly.


Docking tools benchmarked on PoseBusters by percentage of predictions within an RMSD of 2Å of known pose. Image credit: DP Technology


**Blind docking (No binding site information)**


In cases where the binding site is unknown,


[DiffDock-L](https://www.tamarind.bio/diffdock) , the most recent version of the DiffDock model appears to be the most performant. However, this subtask in general shows much more room to improve, with all cases performing below 50%.


Blind(no known binding site) docking performance on PoseBusters and Astex Diverse, image credit:


[Morehead et. al.](https://arxiv.org/abs/2405.14108)


**Using predicted structures for docking**


There is a not insignificant body of work in evaluating whether docking ligands to protein structures produced from


[AlphaFold](https://www.tamarind.bio/alphafold) and similar is practically useful.


[Scardino](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9852548/) et. al. find that “performance of as-is AF models is significantly lower compared with PDB structures” for high throughput docking scenarios. Simultaneously


[Lyu](https://www.science.org/doi/10.1126/science.adn6354) finds the opposite.


My advice around approaching a set of proteins that don’t have crystal structures is to try a known subset of your dataset if possible. Otherwise, it seems feasible enough to evaluate docking tools on predicted protein structures.


A guest post by


[Deniz Kavi](https://substack.com/@denizkavi?utm_campaign=guest_post_bio&utm_medium=web)


Co-founder at Tamarind Bio


[Subscribe to Deniz](https://denizkavi.substack.com/subscribe?)
