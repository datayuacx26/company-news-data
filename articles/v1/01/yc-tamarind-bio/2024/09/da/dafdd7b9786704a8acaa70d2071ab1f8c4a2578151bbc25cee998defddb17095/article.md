---
schema_version: "1.0.0"
document_id: "dafdd7b9786704a8acaa70d2071ab1f8c4a2578151bbc25cee998defddb17095"
company_key: "yc-tamarind-bio"
company: "Tamarind Bio"
source_id: "yc-tamarind-bio-rss-96b2a54954a5"
canonical_url: "https://tamarindbio.substack.com/p/experiment-quality-relative-binding"
published_at: "2024-09-23T01:32:10+00:00"
first_seen_at: "2026-07-24T03:17:16.497895+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:5c41ac766900ffbe3051636b4b693aa048059b55ed901ee5cfc2f5a52d74e7c6"
---

# Experiment Quality Relative Binding Free Energy (RBFE) Calculation with Tamarind Bio

# Experiment Quality Relative Binding Free Energy (RBFE) Calculation with Tamarind Bio


### No-code RBFE calculation in only a few hours, validated experimentally for a TYK2-ligand system.


[Deniz Kavi](https://substack.com/@denizkavi)


Sep 23, 2024


Accurate computational prediction of protein-ligand binding affinities is a useful task in hit-to-lead and lead optimization processes. Relative Binding Free Energy maps well to these scenarios as an accurate and time-efficient method to test virtual ligands against a target to prioritize compounds for synthesis. At Tamarind, we use our RBFE tool, based on


[OpenFE](https://github.com/OpenFreeEnergy/openfe) , to prioritize hits in our virtual screening workflow.


**TYK2 Case Study with Experimental Results**


Below we’ll walk through what happens under the hood when using the Tamarind RBFE tool for a set of ligands against TYK2. As a user, the experience will be uploading a protein structure and a list of compounds!


Binding energy is the change in the free energy associated with the binding of a ligand to a protein. However, since it is highly computationally challenging to model the transition of a ligand to a protein-ligand complex, we can model bond and atom changes of the complex to compute which small changes produce the highest affinities instead. Since we have multiple ligands to test, we can form a network of the relation between ligands, computing free energy changes when transforming from ligand A to B.


We run our 9 ligands of interest using the Tamarind interface and receive a relative ranking of each against others below. The ddG values match the experimental results well!


The whole RBFE computation process takes 1-5 hours for one “edge” on a cutting edge GPU, i.e. one pair in the perturbation network of 9 ligands. Through Tamarind’s high performance computing infrastructure. the entire process finishes in 4 hours, instead 45 hours when running on a single machine.


**About Tamarind Bio**


Tamarind is a user-friendly web application and programmatic API for in silico drug discovery tools. Get in touch to learn more about our Absolute Binding Free Energy workflow, and our virtual screening services: info@tamarind.bio or book a call with me


[here](https://calendly.com/founders-cji/tamarind-bio) .


Thanks for reading the Tamarind Bio Blog! Subscribe for free to receive new posts.


A guest post by


[Deniz Kavi](https://substack.com/@denizkavi?utm_campaign=guest_post_bio&utm_medium=web)


Co-founder at Tamarind Bio


[Subscribe to Deniz](https://denizkavi.substack.com/subscribe?)
