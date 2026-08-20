---
schema_version: "1.0.0"
document_id: "e279411d75b6b3837381f1156feb647416e743d811b0366bdb7bdbfca9f16a56"
company_key: "yc-harmonic-discovery"
company: "Harmonic Discovery"
source_id: "yc-harmonic-discovery-rss-ada272b06e31"
canonical_url: "https://news.harmonicdiscovery.com/presenting-at-mlsb-neurips-2022-1ea72594aa1"
published_at: "2023-01-10T22:50:31+00:00"
first_seen_at: "2026-07-25T07:36:23.550190+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:ed2d482867b77a7ca21486c2813e1d9454c26059374c02c04098f44ed766953b"
---

# Presenting at MLSB@NeurIPS 2022

# Presenting at MLSB@NeurIPS 2022


[Rayees Rahman](https://medium.com/@rayees_76660?source=post_page---byline--1ea72594aa1---------------------------------------)


3 min read


·


Jan 10, 2023


--


Press enter or click to view image in full size


At Harmonic Discovery, we strongly encourage all of our colleagues to present their work at top conferences. We are thrilled that science led by Harmonic Discovery Student Scientist Carmen Al Masri and Harmonic Discovery Scientist Francesco Trozzi, PhD was accepted at one of the top machine learning conferences: NeurIPS! Their work, **Investigating the conformational landscape of AlphaFold2-predicted protein kinase structures,** rigorously evaluates the utility of AlphaFold2 (AF2) to model kinases across several pharmacologically relevant conformational states and how we can use these models for structure-based drug discovery. Some highlights below.


Press enter or click to view image in full size


AF2 can model kinases across several conformational states at the same representation as observed in the PDB.


**AlphaFold2 can model kinases across several conformations**


When we design drugs for proteins such as kinases it’s important to understand the 3D spatial arrangement of amino acids in the binding site. Importantly, proteins are not just static objects, instead they are highly dynamic, changing their shape (or conformation) based on a variety of factors. While each conformation presents a unique physicochemical opportunity for therapeutic intervention, for most kinases we usually have access to the crystal structure describing just a single conformation. In the case of kinases we usually just have the ‘active’ conformation available for structure-based drug discovery. Thus, computational approaches that model the 3D shape of proteins, such as AF2, can illuminate new ways to design therapeutics if they can model different conformational states.


## Get Rayees Rahman’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


What Carmen and Francesco show is that AF2 can, indeed, model protein kinases across several pharmacologically relevant conformational states. Importantly however, the quality of those models is dependent on the number of training examples for those conformations observed in Protein Data Bank.


Press enter or click to view image in full size


AF2 model performance in virtual screening is highly dependent on the modeled conformation of the kinase.


**Utilizing AF2 kinase models for structure based drug discovery**


When we apply these models to drug discovery activities such as virtual screening we observe something extremely interesting. For kinases modeled in conformations with a large amount of training examples (the ‘active’ , CIDI, conformation, for instance) the AF2 structures perform as well, *if not better* , then the next best crystal structure in the same conformation for enriching known actives vs inactive and decoys. This is shocking since all AF2 structures are, by definition, apo structures, or structures without a ligand bound. In the literature, apo structures are known to perform poorly in virtual screening activities compared to their ligand-bound, holo, structures. Conversely we also note that AF2 models perform extremely poorly for structures in conformations less sampled in the PDB.


Taken together we see a huge opportunity to leverage AF2 in structure based drug discovery, especially for identifying conformation specific inhibitors of kinases. To learn more about this work we recommend reading our pre-print linked[here](https://www.biorxiv.org/content/10.1101/2022.12.02.518928v2) .


Huge congrats to Carmen and Francesco for presenting their work at NeurIPS!


Press enter or click to view image in full size
