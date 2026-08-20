---
schema_version: "1.0.0"
document_id: "a9b05abbfb6c68473ad502f2466a1cf41cb2c87c93bb188f83569e9733c0afe8"
company_key: "yc-serna-bio"
company: "Serna Bio"
source_id: "yc-serna-bio-news-import-10fe1061b09a"
canonical_url: "https://www.serna.bio/blog-post/real-world-use-case-of-ai-in-drug-discovery"
published_at: "2025-02-05T00:00:00+00:00"
first_seen_at: "2026-07-25T23:35:45.687739+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:1348dc6d2a55a6ac36026e4ea5071a962fcbde096ff2bc894a47042e871080e5"
---

# Real World Use Case of AI in Drug Discovery: Using GenAI to design small molecules targeting RNA.

### **Real World Use Case of AI in Drug Discovery:**


### **Using GenAI to design small molecules targeting RNA.**


By Tim Allen, Anton Petrov, Matt Ivler


A demonstration of the Serna Bio Gen AI Platform - Polaris.


At Serna Bio, our vision is to unlock the potential of RNA as a drug target for small molecules. Developing new modalities involves overcoming various novel challenges, and[in the case of targeting RNA with small molecules, these challenges have been well-documented previously](https://www.nature.com/articles/s41573-022-00521-4) .[Designing chemical structures exhibiting selective RNA binding resulting in a functional response (for example, modulation of protein expression) has remained a challenge](https://www.nature.com/articles/s41587-023-01790-z) .


‍


*Figure 1 - The challenges of RNA-targeting drug discovery*


To date,[there is only one example of an FDA-approved small molecule with a known mechanism of action (MOA) by modulating RNA function](https://pubs.acs.org/doi/10.1021/acs.jmedchem.8b00741) . That being said, there are other compounds in clinical trials, such as the MYB-targeting compounds from[Remix](https://www.remixtx.com/press-june13-2024/) and[Rgenta](https://www.prnewswire.com/news-releases/rgenta-therapeutics-announces-first-patients-dosed-in-phase-1ab-clinical-trial-of-rgt-61159-an-oral-small-molecule-targeting-myb-in-adenoid-cystic-carcinoma-acc-and-colorectal-cancer-crc-302269345.html) .


At Serna Bio, we hypothesize that by using large data sets, **we can train machines to design molecules; unconstrained by the limitations of the human mind.** Using this approach, we aim to rapidly accelerate the rational design of small molecules that can modulate RNA function, opening up the transcriptome as a drug target.


> ***We train machines to design molecules; unconstrained by the limitations of the human mind.***


However, the foundations of medicinal chemistry have been constructed on the targeting of proteins, as[the vast majority of approved drug compounds to date target the proteome](https://europepmc.org/article/MED/27910877) .


We asked a simple question - if targeting RNA with small molecules requires different chemical solutions to targeting proteins - will we be able to discover those solutions without **changing the way we think about chemistry?**


In order to do this,[we have generated a dataset we have written about previously](https://www.serna.bio/blog-post/star-small-molecules-targeting-rna-defining-the-chemical-rules-that-govern-rna-binders) , and in this blog, we will speak to **how we have used this dataset to develop Generative Chemistry Pipelines to accelerate our hit-to-lead campaign.**


#### **The Power of AI in Drug Discovery: Chemical Optimisation**


[In a traditional drug discovery campaign](https://bpspubs.onlinelibrary.wiley.com/doi/full/10.1111/j.1476-5381.2010.01127.x) , program chemistry is driven via the design of compounds by medicinal chemists. Designed compounds are synthesised, assayed and evaluated in an iterative design-make-test (DMT) cycle to improve molecular properties including biological activity, pharmacokinetics and safety. This process is heavily influenced by the chemists designing the compounds in each iterative cycle and aims to discover entirely novel chemical material with desirable properties to suit the therapeutic area, such as blood-brain barrier penetration.


Generative chemistry algorithms (such as those powered by[large language models (LLMs)](https://pubs.acs.org/doi/10.1021/acscentsci.7b00512) and[variational autoencoders (VAEs)](https://pubs.acs.org/doi/10.1021/acscentsci.7b00572) ) can provide a tool to design novel chemistry and optimize molecules across multiple desirable parameters (such as predicted on-target activity,[Synthetic Accessibility](https://jcheminf.biomedcentral.com/articles/10.1186/1758-2946-1-8) , and[drug-likeness](https://www.nature.com/articles/nchem.1243) ). **These algorithms learn the underlying rules of chemical structures from large datasets of known molecules, enabling them to generate entirely novel new chemical entities (NCEs).** ** Examples of publicly available, state-of-the-art chemical generators include[REINVENT, designed by AstraZeneca](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-024-00812-5) and[MolMIM, designed by Nvidia](https://build.nvidia.com/nvidia/molmim-generate) .


> *These algorithms learn the underlying rules of chemical structures from large datasets of known molecules, enabling them to generate entirely novel new chemical entities (NCEs).*


*Figure 2 - The DMT cycle powered by generative chemistry*


***Importantly, because these algorithms can be trained or fine-tuned using chemical data we select, we can influence their learning away from protein-targeting drug space and towards RNA-targeting small molecules.*** Generative chemistry models can then explore chemical space in a manner that is ***unconstrained by the human biases of a medicinal chemist who has learned their trade through years of focusing on protein-targeting drug molecules*** , potentially discovering novel scaffolds required to move an RNA-targeting drug discovery campaign into the clinic.


Our goal at Serna Bio has been to build a generative chemistry platform that was not available publicly and avoids human biases, with objectives to:


- Outperform state-of-the-art publicly available algorithms[REINVENT](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-024-00812-5) and[MolMIM](https://build.nvidia.com/nvidia/molmim-generate) in RNA-binding chemical space
- Design compounds different to those designed by human chemists
- Optimize compounds over several parameters concurrently, and
- Be experimentally validated: producing RNA-targeting small molecules with performance similar to or better than a human expert


#### ‍ **AI-enabled generation of small molecules to target RNA**


**‍** We set out to construct and train a generative chemistry platform to design a focused set of RNA-targeting small molecules to advance molecular optimization for RNA-targeting drug discovery. Using our internal dataset of 2.4 million data points (mentioned in[our preprint on the physicochemical rules of RNA binding small molecules](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1.abstract) ) we have developed generative chemistry architectures and hypothesised that these architectures are more suited to generate RNA-targeting small molecules. We tested this hypothesis using[Risdiplam](https://pubs.acs.org/doi/10.1021/acs.jmedchem.8b00741) , the only[FDA-approved RNA-small molecule splicing modulator for the treatment of spinal muscular atrophy](https://www.fda.gov/news-events/press-announcements/fda-approves-oral-treatment-spinal-muscular-atrophy) and have compared the compounds generated by our GenAI platform (Polaris) to those generated by[AstraZeneca’s REINVENT](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-024-00812-5) and[Nvidia’s MolMIM](https://build.nvidia.com/nvidia/molmim-generate) . We are looking for:


- A small focused set of compounds (~50)
- Compounds that were similar to Risdiplam as measured by[Tanimoto similarity](https://link.springer.com/article/10.1186/s13321-015-0069-3) . In a standard hit-to-lead campaign, a medicinal chemist would be designing compounds with a score of ~0.7 and higher
- Synthetically tractable compounds to ensure ease of synthesis and testing.[Cheminformaticians sometimes use a synthetic accessibility score of less than 4 to indicate a compound is synthesizable](https://chemrxiv.org/engage/chemrxiv/article-details/6764938b6dde43c908ac68f0)
- Compounds that would rank highly in a docking study with the target of Risdiplam ([Risdiplam has been shown to stabilize a specific residue in the 5’ splice site of SMN2 exon 7, strengthening the splice site and resulting in the promotion of exon 7 inclusion](https://www.nature.com/articles/s41589-019-0384-5) )


‍


#### ‍ **The results - improving generated compounds for molecular optimization**


**‍** A summary of the metrics described in the previous section is included in the table below:


*Table 1 - A summary of the performance of REINVENT, MolMIM and Polaris when using Risdiplam as a seed compound*


On compound numbers, REINVENT generated ~11,000 compounds, which would not be reasonable to synthesize in a drug discovery campaign. Both Polaris and MomMIM generated a more focused set. It is worth noting that the Serna Bio Polaris Platform generated compounds not generated by the other two methods. Searching purchasable chemical space showed that none of the Polaris-generated compounds can be purchased off-the-shelf or through building block synthesis. This set of 61 compounds is our focused chemical set with novelty.


*Figure 3 - Numbers of compounds generated by REINVENT, MolMIM and Polaris when using Risdiplam as a seed compound*


Next, we assessed if the compounds would be useful in a drug discovery campaign, where the aim is to make small molecular changes to explore how changes in chemical structure relate to changes in molecular activity. Below are plotted the distributions of the Tanimoto Scores across the three methods, and a vertical dashed line is included at a similarity value of 0.8 - around which a medicinal chemist considers the chemistry of the new compound informative.


*Figure 4 - The Tanimoto similarity of compounds generated by REINVENT, MolMIM and Polaris to Risdiplam shown in a histogram. 0.8 is indicated as a vertical grey dotted line*


Polaris generates compounds with high similarity to Risdiplam - with all 61 generated compounds having Tanimoto similarity > 0.6, while REINVENT and MolMIM generate a much more diverse chemical set - where only a handful of compounds have Tanimoto similarity > 0.3, which would be unsuitable for a hit to lead campaign.Finally, to assess the ability of the models to generate compounds with on-target activity, we used a docking study using[the PDB structure of the SMN2-U1 duplex target](https://www.rcsb.org/structure/6HMO) of Risdiplam. We have shown the model was able to dock Risdiplam with a high degree of overlap to the experimentally determined structure.


*Figure 5 - An image of the SMN2-U1 duplex target of Risdiplam used to build our docking study. The experimentally determined position of Ridiplam is shown in red and the docked pose of Risdiplam by our docking model is shown in green*


We used this model to calculate docking scores for each compound generated by REINVENT, MolMIM and the Serna Bio Polaris platform. Compounds with higher docking scores indicate a preferable compound for drug discovery.


*Figure 6 - The docking scores of compounds generated by REINVENT, MolMIM and Polaris shown in a histogram*


Comparing the distribution of the docking scores of the generated compounds, it is evident that the Polaris-generated compounds (blue) are shifted to higher docking scores when compared to the distributions for REINVENT (purple) or MolMIM (green). We consider this a useful proxy to show that Polaris can generate compounds with on-target activity.It should be noted that none of the above results indicate that REINVENT or MolMIM are not good molecular generators for RNA-binding chemical space. **‍**


#### **Proof in experiment** ‍


At Serna Bio, we believe AI is only valuable if it can provide a perspective that is novel, demonstrable in a biological experiment, and does not focus on in silico benchmarks. We deployed our Polaris GenAI platform, in support of our internal drug discovery program to develop translational enhancers. Four Hit compounds were passed to Polaris and 15 Polaris-generated compounds were tested. We were looking for the platform to:


- Generate compounds not designed by human-chemists
- Generate compounds with a high hit rate in our assays
- Show a clear distribution of changes that both decrease and increase molecule potency


Polaris designed a total of 411 chemical structures, only one of which was found to also be designed by our human expert.


Top-ranked compounds designed by Polaris were synthesised and tested in an experiment to assess the increase in protein expression. 47% of the tested Polaris-designed compounds were designated as hits, being found to increase the expression of the target gene as desired It is also clear that the changes have developed compounds that both increase and decrease potency, therefore, the molecular changes were informative for the next round of chemical design.


> ‍ *"The Polaris GenAI analogs are very useful to guide the next round of SAR with impact comparable to human-designed analogs"- Medicinal Chemistry Expert* ‍


Serna Bio’s Polaris platform has been constructed to assist RNA-targeting drug discovery campaigns through the molecular optimization of RNA-targeting small molecules.


> ‍ **We have shown here that Serna Bio GenAI platform is capable of designing novel chemistry suitable for the exploration of compound structure-activity relationships and designing compounds without human bias.**


We believe that tools like Polaris have a major role to play in our ability to design RNA-targeting chemistry and unlock the potential of RNA as a drug target.
