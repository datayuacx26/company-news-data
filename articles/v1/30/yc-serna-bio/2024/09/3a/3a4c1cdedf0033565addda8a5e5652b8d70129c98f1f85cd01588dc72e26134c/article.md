---
schema_version: "1.0.0"
document_id: "3a4c1cdedf0033565addda8a5e5652b8d70129c98f1f85cd01588dc72e26134c"
company_key: "yc-serna-bio"
company: "Serna Bio"
source_id: "yc-serna-bio-news-import-10fe1061b09a"
canonical_url: "https://www.serna.bio/blog-post/star-small-molecules-targeting-rna---one-year-on"
published_at: "2024-09-04T00:00:00+00:00"
first_seen_at: "2026-07-25T23:35:45.687739+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:1b8d31e8cef7d813038751a07a71f8295c5e9d93fffdaa2820e959fd5efd198a"
---

# STaR: Small Molecules Targeting RNA - One Year On

**STaR: Small Molecules Targeting RNA - One Year On**


By Tim Allen - Director of ChemAI


<iframe style="border-radius:12px" src="https://open.spotify.com/embed/episode/1iSTh6HvbNMKOJhrSEnvC3?utm_source=generator" width="100%" height="352" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>


‍


One year ago we released the blog[‘STaR: Small Molecules Targeting RNA: Defining the Chemical Rules that Govern RNA Binders’](https://www.serna.bio/post/star-small-molecules-targeting-rna) and subsequently released[a more detailed manuscript as a pre-print on bioRxiv](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1) .[We also published a blog on using this data to develop machine-learning models to differentiate RNA binders from non-binders](https://www.serna.bio/post/predicting-rna-binding-molecules-using-machine-learning) . As a follow-up, this blog aims to address some of the questions we’ve been asked over the last year about this work and reveal some more of the insights we have found on working small molecules targeting RNA at Serna Bio.


‍


1. **Can you comment on the drug-likeness of the compounds passing the STaR rules, particularly with reference to the rule of five and oral drug development?**


‍


The challenges of developing a repeatable and scalable process for the discovery of drug-like RNA targeting small molecules[have been discussed before](https://www.nature.com/articles/nrd.2018.93) .[As part of our pre-print](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1) , we showed that 97% of the Serna Bio identified RNA Binders in our dataset pass[Lipinski’s Rule of Five (Ro5)](https://www.sciencedirect.com/science/article/abs/pii/S0169409X00001290) suggesting they have physicochemical properties favourable for development into orally bioavailable drugs. Below, we have extended this analysis by identifying the compounds in different RNA-binding datasets which pass the STaR rules and showing that high percentages of those compounds also pass the Ro5 (Table 1)


‍


*Table 1 - A table showing the results when applying the STaR Rules for RNA binders to Serna*


*Bio, R-BIND, DRTL, ROBIN, Inforna, and Patent Data (*[as defined in our pre-print](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.full.pdf) *). The second column shows the proportion of RNA binders in each dataset passing the STaR rules and the third column the percentage of these STaR rules passing RNA binders that subsequently pass the Ro5. Please note that for Patent Data - binding to RNA is not confirmed but assumed based upon patent protection.*


‍


Furthermore, the STaR and Lipinski rules can be applied to Risdiplam, an orally bioavailable FDA-approved gene splicing modifier for the treatment of spinal muscular atrophy, showing it passes both sets of rules (Figure 1).


‍


*Figure 1 - Risdiplam and its calculated physicochemical properties relevant to the Ro5 (left) and STaR rules (right). Risdiplam passes both rulesets. MW = molecular weight, HBA = number of H-bond acceptors, HBD = number of H-bond donors, Mol. Ref. = molar refractivity, Aro. Rings = number of aromatic rings, Rel.PSA = relative polar surface area*


‍


In conclusion, high percentages of STaR-rules compliant RNA Binders in the Serna Bio dataset and publicly available data are also Ro5 compliant, and Risdiplam complies to both the STaR rules and the Ro5. This evidence suggests that STaR-rules compliant RNA binding small molecules are not at a physicochemical disadvantage for development into orally bioavailable, approved drug compounds.


‍


1. **Is it odd that some of the datasets analysed in the STaR rules work show opposite directions for the same physicochemical properties? Can you explain this?**


‍


While most of the calculated physicochemcial properties are moving in a consistent direction for RNA Binders or are found to be not statistically significant (17/26, ~65%), some move in opposite directions when considering different RNA binder datasets (Figure 2). Some examples of these differences include:


- Molecular weight, which increases for RNA Binders in Inforna, R-BIND, the Patent compounds and Serna Bio but decreases for RNA Binders in ROBIN
- cLogP, which increases for RNA Binders in ROBIN and Serna Bio but decreases for RNA Binders in R-BIND
- The number of aromatic nitrogens, which increases for RNA Binders in R-BIND, the Patent compounds and Serna Bio but decreases for RNA Binders in ROBIN


‍


‍


*Figure 3 - Radar plots showing the change in physicochemical properties between RNA binders and non-binders in the datasets Serna Bio (upper left, red), Inforna (upper right, blue), R-BIND 2.0 (middle left, yellow), DRTL (middle right, turquoise), ROBIN (lower left, purple), and Patent compounds (lower right, pink). In these plots, an extension of the line to the edge of the circle indicates a statistically significant increase in that property in RNA binders compared to non-binders, while a contraction to the center indicates a statistically significant decrease in that property in RNA binders compared to non-binders. Here, statistically significant changes must involve a p-value < 0.01 and a change in median value for the physicochemical property in question using Mood’s median test with Benjamini-Hochberg correction. Figure from our preprint:*[Physicochemical Principles Driving Small Molecule Binding to RNA](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.full.pdf)


‍


It should be noted that all the datasets examined have been collected in different ways, often relying on different experimental techniques for the determination of compounds as binders to RNA. For example, all compounds in[ROBIN](https://onlinelibrary.wiley.com/doi/abs/10.1002/anie.202211358) were assayed against RNA and DNA targets using small molecule microarray screening, and a binding outcome against any of the RNA targets resulted in a compound being labelled as a Binder. Conversely,[R-BIND](https://www.biorxiv.org/content/10.1101/2022.03.14.484334v1.full) consists of a curated and classified list of bioactive RNA binding compounds.[The Serna Bio dataset](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.full.pdf) was collected similarly to ROBIN, but used ASMS technology for experimental data collection.[Inforna](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4912454/) combined experimental data collection with scientific literature searches.


‍


When comparing the physicochemical properties of compounds in datasets collected using different methods and experimental techniques it is not surprising that they all do not align. Given the variety of the publicly available RNA-binding datasets, this highlights the challenge in identifying generalizable rules for their physicochemistry, highlighting the value in the generalizable ruleset we have constructed.


‍


1. **Have you considered using the STaR rules in machine learning models (such as decision trees) to improve their prediction capacity?**


‍


Using our RNA-binding data for the development of machine learning models[is a subject we have blogged on previously](https://www.serna.bio/post/predicting-rna-binding-molecules-using-machine-learning) . In that work, we have shown that:


- The Serna Bio dataset enables the training of machine learning classifiers that can classify molecules as RNA Binders or RNA Non-Binders
- The Serna Bio dataset trained models outperform those trained on publicly available data (ROBIN) for the same task
- The Serna Bio dataset trained classifiers can enrich existing datasets for RNA Binders in an external validation task (by applying them to ROBIN)


‍


In that exercise a number of representations of chemistry were investigated, including the use of high dimensionality calculated chemical descriptors using[Mordred](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-018-0258-y) . When considering the roles of machine learning classifiers and physicochemical rules of thumb in drug discovery, they can each be used in different ways that play to their specific advantages. In the case of the STaR rules and the Ro5, the advantage they have is that they are transparent, easy to calculate, and easy to understand. Chemists can intuitively understand why a compound passes or does not pass the rules and potentially modify its chemical structure to move it into more favourable physicochemical space. This can be more challenging when considering the predictions of a machine learning classifier, and how exactly to modify chemical structures to obtain the desired outcome.


‍
