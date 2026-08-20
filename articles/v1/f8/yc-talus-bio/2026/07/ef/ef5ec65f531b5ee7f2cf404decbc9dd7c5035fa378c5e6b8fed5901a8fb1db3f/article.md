---
schema_version: "1.0.0"
document_id: "ef5ec65f531b5ee7f2cf404decbc9dd7c5035fa378c5e6b8fed5901a8fb1db3f"
company_key: "yc-talus-bio"
company: "Talus Bio"
source_id: "yc-talus-bio-rss-3371821d42a6"
canonical_url: "https://blog.talus.bio/p/welcome-to-the-structure-free-future"
published_at: "2026-07-31T18:16:46+00:00"
first_seen_at: "2026-07-31T18:59:38.380379+00:00"
fetched_at: "2026-07-31T18:59:39.643366+00:00"
content_hash: "sha256:19a9315a5b6581bb828010beeab9f75783c88d6ec4d5839a0628713656951739"
---

# Welcome to the structure-free future of drug discovery

# Welcome to the structure-free future of drug discovery


### Meet our newest model, Ptarmigan-1, built for massive-scale virtual screening and extending to poorly-structured protein targets


[Will Fondrie](https://substack.com/@wfondrie)


,[Alex Federation](https://substack.com/@afederation)


, and[Lindsay Pino](https://substack.com/@lkpino)


Jul 31, 2026


*This post accompanies our [preprint introducing Ptarmigan-1](https://www.biorxiv.org/content/10.64898/2026.07.28.741295v1) .*


As visual creatures, humans love to see see a protein structure. Nobel-prize winning models like


[AlphaFold](https://www.nature.com/articles/s41586-024-07487-w) and the open-weight


[Boltz](https://www.biorxiv.org/content/10.1101/2025.06.14.659707v1) have democratized structure prediction across a wide range of proteins. These predictions opened new


[possibilities](https://www.biorxiv.org/content/10.64898/2026.07.04.736485v1) for drug discovery, helping to quickly identify drug molecules that can fit into these structures like hand-in-glove.


Subscribe for future Ptarmigan updates


The reality though is that these models work best on the roughly


[half](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0034687) of all proteins that have well-defined, previously observed structures.


But do our AIs models actually need to predict protein structures to find drugs?


We already know that the 3D structures we as humans cling to are only a snapshot of the vast range of possible conformations a protein might assume. We also have observed only a small fraction of possible protein structures experimentally, biasing all AI models trained on such data. And from a practical perspective, forcing models to generate structures for our 3D brains is expensive in today’s GPU-limited world.


We set out to develop a model that would give us access to that other half of protein targets that exist beyond the well-defined structures that we already know.


Meet our model, Ptarmigan-1 (ptarmigan, like pterodactyl), that extends small-molecule drug discovery beyond the reach of structure-based modeling.


## Ptarmigan-1 maps molecules to the sites they bind without structure


A protein never stops moving, and a 3D structure is a photograph of something in motion. So rather than force Ptarmigan-1 to reason about proteins in the 3-dimensional space of our world, we give it the freedom of 256 dimensions to learn in a space that makes sense to a machine.


Taking a 3D protein and letting the machine render it in 256 dimensions


Think about how your maps app finds you dinner. Sure, the app finds resturants physically close to you, but it also considers a much larger space, considering the cuisine and price and a hundred things it learned from millions of reviews, and "nearby" in that space means "you'll probably like this." Nobody finds a 256-dimensional taste space strange when it's picking out Teryaki.


Ptarmigan-1 builds that kind of map for molecular recognition. Every residue of every protein gets an address. Every compound gets an address in the same space. We train the model so that a molecule lands closest to the sites it actually binds.


Screening a library against a target stops being a simulation and becomes a lookup. “What molecules are closest to this pocket?”


## A lookup instead of an experiment


Today’s frontier co-folding methods score a target and a library from scratch, every time, so each screen is a one-off computation whose reach you fix in advance by the chemistry you choose to enumerate and the compute you can afford. Nobody rebuilds Google Maps for each search. Ptarmigan-1's map is built once and queried endlessly, and it gets better as data accumulate. Every new measurement is another review, sharpening every future query.


Which changes what a virtual screen even is.


Co-folding is like eating your way through every teriyaki joint in Seattle to find the best one


(shout out


[Kenji](https://www.kenjilopezalt.com/where-im-eating) ).


Ptarmigan is ranking the best-reviewed teriyaki on earth and flying straight to the top of the list. For drug discovery, that means querying trillions of compounds against the entire human proteome and getting a shortlist in days, without massive compute.


In our preprint, we show Ptarmigan-1 is competitive or exceeds SOTA on standard and custom benchmarks, show its ability to find cryptic pockets, and show that it is orders of magnitude faster than co-folding methods, and quite a bit more. But for now, let’s take a look at one vignette exploring recently disclosed compounds targeting STAT6.


## Ptarmigan-1 unlocks druggable sites outside of traditional pockets


We compared Ptarmigan-1 head-to-head against a leading co-folding model, Boltz-2, at trying to find the 40 active molecules that bind STAT6 from two recent Pfizer patents from within a haystack of similar-looking molecules.


This gave a fair comparison (which is surprisingly hard to find in this rapidly evolving field), as the molecules from these patents are absent from the training sets for both Ptarmigan-1 and Boltz-2.


Ptarmigan-1 identifies recent active molecules targeting STAT6. (A) Ptarmigan-1 beats other leading methods for finding these active molecules against a library of property-matched decoy molecules. (B) Ptarmigan-1 correctly localizes the active molecules to a shallow groove on the SH2 domain. (C) Ptarmigan-1 correctly localizes these molecules, while Boltz-2 does not.


We found that for this target that was new to both models, Ptarmigan-1 was highly capable at retrieving the active compounds from the haystack compared to Boltz-2 or a more traditional docking method, which were pretty much random guessing (A). The really cool part though is that Ptarmigan-1 was the only model that localizes these active compounds to the residues on a shallow groove of the SH2 domain binding site.


Let’s take a look at another story - in this case an example of emergent behavior that didn’t make it into the manuscript, involving KRAS.


## Ptarmigan-1 learned to rank KRAS G12C-targeted compounds by generation


Just 10 years ago, KRAS was an “


[undruggable](https://www.nature.com/articles/s41392-021-00780-4) ” white whale in cancer, so we intentionally withheld it from training so we can do experiments with it.


While putting Ptarmigan-1 through it’s paces, we saw that it learned to do something we did not expect: when we look at a series of KRAS G12C molecules and their evolution, Ptarmigan-1 learns to rank them by generation, including the early hits the opened up this entire thread of research.


Figure 2. Ptarmigan-1 learns to rank KRAS G12C-targeted ligands by generation, despite KRAS G12C and the corresponding molecules being withheld during training (A-C). Furthermore, reverting the G12C mutation ablates the predicted activity of these molecules (D).


During training, Ptarmigan-1 sees only binary labels for a compound and the residues of a protein or the protein as a whole. It is not trained to predict binding affinities or other biophysical properties. Learning to rank these molecules by generation is an emergent property of the model that signals the training process is forcing it to learn about the underlying physical process of the interaction rather than just memorizing patterns for active molecules.


Furthermore, when we un-mutate the G12C mutation to wildtype, the predicted engagement for these molecules evaporate.


## Talus’ platform data is what gives Ptarmigan-1 its power for poorly structured proteins


Although Ptarmigan-1 does not use any protein structures to train, it does benefit from labels that specify where a compound binds a protein. In recent years, chemoproteomics approaches have become a power tool for probing ligandable residues with covalent molecules.


At Talus, we’ve measure how thousands of covalent small molecules bind across the poorly structure proteins of the regulome, including high value targets like transcription factors. This dense chemoproteomic map that is enriched for proteins with poorly characterized structures are what allow Ptarmigan-1 to generalize beyond the structured protein domain seen by other models.


Not familiar with our platform? Check out


[our earlier preprint describing our unique assay to profile the regulome](https://www.biorxiv.org/content/10.1101/2025.06.14.659727v2) .


## Bonus: What is a Ptarmigan?


A ptarmigan (the “p” is silent) is a bird that one might describe as mountain chicken. In fact, near our Seattle HQ, you might find


[a ptarmigan among the talus fields (rock/boulder fields) of Mount Rainier](https://www.fws.gov/species/mt-rainier-white-tailed-ptarmigan-lagopus-leucura-rainierensis) .


*We are working with researchers to provide early access to Ptarmigan-1. Reach out and let’s work together:*


Email Talus Bio


Thanks for reading Talus Bio Blog! Subscribe for free to receive new posts and support my work.
