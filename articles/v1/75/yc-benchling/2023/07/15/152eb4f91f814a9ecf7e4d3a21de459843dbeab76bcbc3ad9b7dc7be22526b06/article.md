---
schema_version: "1.0.0"
document_id: "152eb4f91f814a9ecf7e4d3a21de459843dbeab76bcbc3ad9b7dc7be22526b06"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/developability-predictions-in-therapeutic-antibody-discovery"
published_at: "2023-07-10T07:00:00+00:00"
first_seen_at: "2026-07-29T00:41:24.192337+00:00"
fetched_at: "2026-07-29T00:41:27.899557+00:00"
content_hash: "sha256:585626ac926bc8deb61e0a0cb658eb587ab5db2a1f2c5af10ca4bede82e03b10"
---

# Developability predictions in therapeutic antibody discovery

Antibody therapeutics are hard to make. In order to bring a candidate to market, there are several ‘developability’ factors that need to be considered, more broadly this includes characteristics such as, safety, immunogenicity, solubility, specificity, stability, manufacturability, and storability.1–3


Specifically,[liabilities](https://pipebio.com/blog/antibody-liabilities) such as post-translational modifications (PTMs),4 deamidation, oxidation and isomerization,5,6 can cause serious issues in downstream development.


To complicate matters, as all of these factors are physically linked, trying to fix one issue alone (increasing specificity) may cause another factor to change (increasing aggregation).


Solving this multivariate optimization problem is not straightforward, the non-linear associations and high dimensionality make for a uniquely challenging task. Therefore, predicting and fixing these properties (quickly and cheaply) at an early stage is critical to avoid wasting resources on failed candidates.


One issue that affects many of the factors listed above that needs to be balanced when developing biologics for clinical usage is aggregation. As part of the bioinformatics liability identification toolbox, PipeBio’s aggregation analysis pipeline has been developed based on state-of-the-art techniques which include deep learning models for predicting tertiary structure combined with per-residue aggregation scoring.


This tool can help scientists dealing with information overload de-risk their therapeutic antibody discovery workflows by rapidly analyzing antibody, nanobody or TCR sequences for aggregation prone regions.


## Antibody developability: The challenges of antibody aggregation


Aggregation is a major problem in antibody developability.7 Not only is aggregation dangerous (think amyloidosis), it can impact therapeutic efficacy and immunogenicity. Furthermore, aggregation is also a practical manufacturability problem8: what good is a therapy that can’t be stored and shipped to patients?


Understandably, to obtain FDA approval for a clinical usage, it is likely that proof must be given that a newly developed antibody will not aggregate. Combined with the high costs associated with testing for antibody stability, aggregation issues can dramatically slow the rate of bringing new therapeutics to market.


There are, however, many tools which can be used to support the development of stable and effective antibody therapeutics that range from experimentation, physico-chemical computation, and machine learning.‍


## The science of antibody aggregation


Antibody aggregation occurs when it is energetically favorable for monomers of proteins to come together and interact. This is dictated by factors such as pH, temperature, isoelectric point (pI), ionic strength, protein concentration, and secondary structure.9 Additionally, surface exposed hydrophobic regions pose a real danger for aggregation, as it is not energetically favorable to have hydrophobic regions facing water.


Simply put, “the inherent hydrophobic interaction of VH and VL domains limits the stability and solubility of engineered antibodies, often causing aggregation…”.10


A similar issue occurs with single-chain variable fragments (scFv), where the hydrophobic surfaces can dissociate and interact with other hydrophobic regions causing aggregation. This further explains why the smaller *Camelid* nanobodies (VHH)11,12 are so appealing, as instead of the typical hydrophobic regions of VH domains, they have a hydrophilic region which does not bind light chains and increases solubility.10


However, these VHHs can still suffer from misfolding and aggregation,13 so detecting and preventing aggregation can not be ignored while developing therapeutic nanobodies either.


## Detecting aggregation and aggregation prone regions - experiment vs computation


‍Methods for detecting or predicting aggregation fall into the categories of experiment or computational prediction. Though experimentation is the gold standard for determining antibody aggregation, it would be massively cost prohibitive to perform experiments on the entire antibody or nanobody space.


Though they are less accurate, computational techniques are high-throughput and can be significantly more cost effective. Computational models can be physics based and rely solely on physico-chemical properties or used machine learning algorithms where experimental data is used as inputs for training models to predict aggregation propensity.


## Experimentation


There are several experimental techniques which are industry standard, such as size exclusion chromatography (SEC), hydrophobic interaction chromatography (HIC), affinity-capture self-interaction nanoparticle spectroscopy (AC-SINS) and stand-up monolayer adsorption chromatography (SMAC). SEC is the standard method for measuring protein aggregation along with HIC as a complementary tool.14


Additionally, AC-SINS is useful for measuring antibody or nanobody propensity to self-associate.15 To increase confidence, multiple experimental values can be used in tandem, such as the consensus of SMAC, SINS and HIC.16


## Computation


Computational methods for predicting aggregation typically take sequence or structure data as inputs, and can be physics or ML-based. In some cases, they are designed to predict aggregation per sequence, which is particularly useful for protein developers who have a lot of data to analyze and short deadlines. Or they can be designed to predict scores at the per residue level which is more explainable.


An example of a *sequence-property* model is the *Zyggregator* method, which outputs aggregation scores per residue from sequence data inputs, such as alpha and beta-helix propensity, hydrophobicity, charge, hydrophobic patterns, gatekeeper residue, and local stability into account.17‍


Another example is[SSH2.0](http://i.uestc.edu.cn/SSH2/) (SSH stands for **S** MAC, **S** GAC-SINS, and **H** IC), a *sequence-ML method* , which uses aggregation data to train a support vector machine (SVM) ensemble model to predict aggregation.18,19


With respect to *structural-property* models, which require 3-dimensional atom positions as input, a popular example is Schrodinger's[BioLuminate](https://www.schrodinger.com/products/bioluminate) Package (AggScore20). AggScore is based entirely on tertiary structure inputs which measure the hydrophobic and electrostatic surface patches. Alternatively,[Therapeutic Antibody Profiler (TAP)](https://opig.stats.ox.ac.uk/webapps/sabdab-sabpred/sabpred/tap) 21,22 compares developmental antibodies to clinical-stage therapeutic antibodies using five developability values, such as CDR length, surface charge and hydrophobicity, and structural Fv charge symmetry (SFvCSP).


Note that TAP only requires sequence data as input, as the atom positions used in TAP are generated by the deep learning structural prediction tool[ABodyBuilder2](https://opig.stats.ox.ac.uk/webapps/sabdab-sabpred/sabpred/abodybuilder2/) .


## Avoiding aggregation


It isn’t enough to simply detect aggregation liabilities, they must be removed.


This is by no means a *solved problem* and a complete set of attributes determining aggregation resistance are not well mapped. However, some heuristics have been developed; for example, it has been shown that aggregation of nanobodies can be circumvented by simply adding a positive charge to the CDRs.23


Other strategies include inferring stability by combining sequence and thermostability data,24 retrofitting variables domains,25 the introduction of “artificial aggregation gatekeeper residues”,26 or “camelization” of human VH domains.10


There are also in vivo platforms for evolving aggregation resistant proteins, such as the tripartite β-lactamase enzyme assay (TPBLA), which can screen and evolve ‘manufacturable’ biopharmaceuticals as well as rank *innate* aggregation prone peptides.27


Computational methods can also be used to engineer antibodies. For instance, to help in the selection of possible mutations for conferring aggregation resistance, the open source[Aggrescan3D](http://biocomp.chem.uw.edu.pl/A3D2/) (A3D) tool can be used.28


A3D calculates aggregation scores per residue for a static (or dynamic using CABSs29) structure, as well as performs what-if scenarios for point mutations (using energy minimized structures in FoldX30). The key to this calculation is estimation of the solvent accessible surface areas (SASA)31 32 where the topological character of antibodies are accounted for in aggregation scoring.


Similarly to Aggrescan3D, a software tool called SolubiS attempts to optimize stability by introducing mutations that reduce aggregation.


We have performed a thorough review of available aggregation prediction servers and have provided a summary table with links below, additionally a further review of the state-of-the-art computational techniques can be found by Navarro and Ventura.33


## Bioinformatics software with open access servers


### Second generation algorithms (post-2016)


The second generation algorithms tend to take either secondary or tertiary data into account, employ machine learning or a combination of different advanced techniques.


Name


Inputs


Outputs


DOI


Year


Notes


[SSH2.0](http://i.uestc.edu.cn/SSH2/)


Sequence


Aggregation Score Per Sequence


[doi.org/10.3389/fgene.2022.842127](https://doi.org/10.3389/fgene.2022.842127)


2022


ML Based


[Therapeutic Antibody Profiler (TAP)](https://opig.stats.ox.ac.uk/webapps/newsabdab/sabpred/tap)


Sequence


5 Metrics


[doi.org/10.1073/pnas.1810576116](https://doi.org/10.1073/pnas.1810576116)


2019


Deep Learning Based


[Aggrescan3D 2.0](http://biocomp.chem.uw.edu.pl/A3D2/)


PDB File


Aggregation Score Per Residue


[doi.org/10.1093/nar/gkz321](https://doi.org/10.1093/nar/gkz321)


2019


Multiple combined algorithms


[Solubis](https://solubis.switchlab.org/node/add/solubis-job)


PDB File


Aggregation nucleating regions


[doi.org/10.1093/protein/gzw019](https://doi.org/10.1093/protein/gzw019)


2016


Multiple combined algorithms


[Waltz](https://waltz.switchlab.org/)


Sequence


Amylogenic regions


[doi.org/10.1093/nar/gkz758](https://doi.org/10.1093/nar/gkz758)


2019


Amyloid focussed


[Cordax](https://cordax.switchlab.org/dashboard)


Sequence


Amylogenic score per residue (binary estimation)


2020


ML Based and uses multiple combined algorithms


[ANuPP](https://web.iitm.ac.in/bioinfo2/ANuPP/homeseq1/)


Sequence


APRs


[doi.org/10.1016/j.jmb.2020.11.006](https://www.sciencedirect.com/science/article/abs/pii/S0022283620306252)


2021


ML Based


[AmyloGram](http://biongram.biotech.uni.wroc.pl/AmyloGram/)


Sequence


Amyloid probability per sequence


[doi.org/10.1038/s41598-017-13210-9](https://www.nature.com/articles/s41598-017-13210-9)


2017


Ml Based


[SOLart](http://babylone.ulb.ac.be/SOLART/k_query.php)


PDB File


Scaled solubility score %


[doi.org/10.1093/bioinformatics/btz773](https://pubmed.ncbi.nlm.nih.gov/31603466/)


2022


ML Based


[PON-Sol2](http://139.196.42.166:8010/PON-Sol2/input/sequence/)


Sequence


Identification of solubility affecting substitutions


[doi.org/10.3390/ijms22158027](https://www.mdpi.com/1422-0067/22/15/8027)


2021


ML Base


### First generation algorithms (prior to 2016)


The *first generation* algorithms exclusively deal in sequence data, are mainly amyloid focussed and do not employ machine learning.‍


Name


Inputs


Outputs


DOI


Year


Notes


[TANGO](http://tango.crg.es/)


Sequence


Aggregation nucleating regions


[doi.org/10.1038/nbt1012](https://pubmed.ncbi.nlm.nih.gov/15361882/)


2004


Amyloid focussed


[AGGRESCAN (1.0)](http://bioinf.uab.es/aggrescan/)


Sequence


Aggregation Score Per Residue


[doi.org/10.1186/1471-2105-8-65](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-8-65)


2007


[FoldAmyloid](http://bioinfo.protres.ru/fold-amyloid/)


Sequence


Amylogenic regions


2009


Amyloid focussed


[ArchCandy (2.0)](https://bioinfo.crbm.cnrs.fr/index.php?route%3Dtools%26tool%3D7)


Sequence


Amylogenic regions


[doi.org/10.1016/j.jalz.2014.06.007](https://alz-journals.onlinelibrary.wiley.com/doi/10.1016/j.jalz.2014.06.007)


2014


Amyloid focussed


[BetaScan](https://cb.csail.mit.edu/cb/betascan/)


Sequence


Beta strand prediction


[doi.org/10.1371/journal.pcbi.1000333](https://journals.plos.org/ploscompbiol/article?id%3D10.1371/journal.pcbi.1000333)


2009


Amyloid focussed


[GAP](https://www.iitm.ac.in/bioinfo/GAP/)


Sequence


Amyloid probability per residue


[doi.org/10.1093/bioinformatics/btu167](https://academic.oup.com/bioinformatics/article/30/14/1983/2391051)


2014


Amyloid focussed


[CamSol](https://www-cohsoftware.ch.cam.ac.uk/index.php)


Sequence


Aggregation Score Per Sequence


[doi:10.1016/j.jmb.2014.09.026](http://www-vendruscolo.ch.cam.ac.uk/camsolmethod.html)


2014


Not open access


[PASTA 2.0](http://old.protein.bio.unipd.it/pasta2/)


Sequence


APRs


[doi.org/10.1093/nar/gku399](https://pubmed.ncbi.nlm.nih.gov/24848016/)


2014


Amyloid focussed


### Other models and algorithms


‍There are also published papers and methods that boast strong predictive power, though you will need to either set them up yourself or pay a license fee.


Name


Inputs


Outputs


DOI


Year


Notes


Support Vector Machine (SVM)


Sequence


Aggregation Prediction Probability


[doi.org/0.1080/19420862.2022.2026208](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8794240/)


2022


ML Based


Random Forest + Logistic Regression


Sequence


Aggregation Prediction (retention time)


[doi.org/10.1093/bioinformatics/btx519](https://academic.oup.com/bioinformatics/article/33/23/3758/4083264)


2017


ML Based


Spatial Aggregation Propensity (SAP)


PDB File


Location & size APRs


[doi.org/10.1073/pnas.0904191106](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2715526/)


2009


ML Based


Zyggregator


Sequence


Aggregation Score(s) per Residue


[doi.org/10.1039/b706784b](https://pubs.rsc.org/en/content/articlelanding/2008/CS/b706784b)


2008


Physics Based


Random Forest Model


Sequence


Amyloidogenesis score


[doi.org/10.1371/journal.pone.0053235](https://pubmed.ncbi.nlm.nih.gov/23308169/)


2013


ML Based


APPNN


Sequence


amyloidogenicity propensity


[R Package](https://cran.r-project.org/web/packages/appnn/index.html)


2015


ML Based


AggScore


PDB File


APRs


[doi.org/10.1002/prot.25594](https://doi.org/10.1002/prot.25594)


2018


License Required


‍Note there are further models not reviewed by PipeBio listed[here](https://en.wikipedia.org/wiki/Protein_aggregation_predictors) .


## Predicting developability issues in PipeBio


Lets look at an example which demonstrates the utility of some of the tools available at PipeBio for developing therapeutic biologics:


### Hydrophobicity and aggregation score tracks


Let’s say you have a nanobody sequence that has shown promise with respect to selectivity for a target of interest, though it also suffers from poor solubility. To improve the sequence you perform a bio-panning assay and collect enriched sequences with increased selectivity. After panning, you have a new improved set of sequences, however, their stability and solubility are still uncertain.


Using the PipeBio toolkit, you can easily investigate the potential solubility of your selected sequences for further downstream development. The first step in the developability analysis is to upload the sequences, align and annotate them.


Then, the hydrophobic patches in different regions can be compared across the aligned sequences using the hydrophobicity track. The hydrophobicity track calculates a windowed average hydropathicity score of residue hydrophobicity (Kyte-Doolittle scale).34


In this case red is hydrophobic and blue is hydrophilic. To simplify, here are two example candidates, Seq1 and Seq2:


**Figure 1.** Hydrophobicity tracks (hydrophobic patches) for Seq1 and Seq2


In the first iteration, based on the calculation of hydrophobicity alone it isn’t clear which of the two sequences would be more aggregation resistant.


In the CDR-H1 of Seq2 there exists a more soluble hydrophilic region, however there also appears to be a less soluble hydrophobic region in CDR-H3.


This is more clearly seen in the zoomed and cropped hydrophobicity track of the CDR-H1 and CDR-H3.‍


**Figure 2.** CDR-H1 and CDR-H3 of Seq1 and Seq2 displaying hydrophobic and hydrophilic patches


The method above does not take into account the tertiary structure of the sequences. As the nanobodies are 3-dimensional structures, what really matters are the hydrophobic regions that are exposed to the solvent.


To get a more nuanced understanding of the sequences, a method which takes into account the tertiary structure can be applied in PipeBio. First the 3D structure of a nanobody (or antibody) is predicted from sequence data using Immune Builder.21 (Alternatively, raw crystal structure data can be uploaded).


Secondly, this structure is used to calculate the per-residue aggregation score (A3D) score,35 which indicates aggregation prone patches in red and aggregation resistant patches in blue28.


Here is a zoomed and cropped view of the hydrophobicity and A3D scoring track for CDR-H1 and CDR-H3:


**Figure 3.** Aggregation scores and hydrophobicity scores mapped for each amino acid residue of the nanobody sequences.


Now it is easier to see that the (blue) negative patch of A3D scores in the CDR-H1 region of Seq2 increases our confidence in the aggregation resistance of Seq2, and the (red) positive patches of A3D scores appear in the CDR-H3 region of Seq1 which decreases our confidence in the aggregation resistance of Seq1.‍


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## 3-Dimensional structural view of aggregation scores


Using the PipeBio protein structure display tools, a 3-dimensional surface plot of aggregation propensity scores illuminates how aggregation resistant sequences can be selected. In this case aggregation *prone* regions are shaded red and aggregation *resistant* regions are shaded blue, as shown in view of CDR-H1 (Figure 4).‍


Here, it is visually clear that the aggregation resistant patch in the left is smaller in Seq1 relative to Seq2, and the aggregation prone region in the right is larger in Seq1 relative to Seq2; the F at IMGT position 115 (F115), which has a high aggregation score of 1.95, is particularly prominent. This is strong evidence that indicates Seq2 will be less likely to aggregate.


**Figure 4.** 3-dimensional surface plot with aggregation scores for sequence 1 (Seq1) and sequence 2 (Seq2) showing CDR-H1. For reference, both sequences share an S at IMGT position 83 (S83) and an F at IMGT position 28 (F28) in CDR-H1. Note that, due to the effect of the surrounding hydrophobic residues, the aggregation score at F28 decreases from -0.21 to -1.13 from Seq1 to Seq2.


If the structures are rotated about the vertical axis by 45 degrees, the right side of the structure in **Figure 4** is more easily seen – including the CDR-H3 region. In **Figure 5** , CDR-H3 (view 1) F115 is shown more clearly along with the surrounding surface exposed patches, such as Y108 with a score of 1.43 in Seq1.


Conversely, at IMGT 115 in Seq2, there is a leucine with a smaller score of 1.30, and the surrounding surface exposed patches are aggregation resistant with a score of -2.1 for P114; further evidence for choosing Seq2 over Seq1.


**Figure 5** . CDR-H3 (view 1). 3-dimensional surface plot with aggregation scores for sequence 1 (Seq1) and sequence 2 (Seq2) showing CDR-H3 facing front (view 1). For reference, both sequences share an W at IMGT position 118 (W118).


If the structures are now rotated so the CDR-H3 regions are facing the left ( **Figure 6** ), it can be seen that the aggregation prone regions around R115 stick out in Seq1, where the same region is more concave.


Based on this conformational and residue difference, it can be seen that the aggregation score at F115 is 1.95 for Seq1 but drops to 1.30 for Seq2 (at L115).


Furthermore, the addition of an E at 113 significantly changes the aggregation scoring in that region where the E113 has a score of -1.27 and A114 has a score of 1.14.


**Figure 6** . CDR-H3 (view 2). 3-dimensional surface plot with aggregation scores for sequence 1 (Seq1) and sequence 2 (Seq2) showing CDR-H3 facing to the left (view 2). For reference, both sequences share an W at IMGT position 118 (W118).


This theoretical example consists of real nanobody structures, namely the insoluble Dp47d (Seq1) and the soluble version HEL4 (Seq2).36


HEL4 is an isolated human VH domain with similar properties to camelid VHH domains, which has been shown to be resistant to aggregation.27 HEL4 was obtained from HEL biopanning with a phage-display library of VH dAbs - Dp47D being the template.


These nanobodies illustrate the utility of tertiary structure prediction combined with the aggregation scoring. As was clear form the first iteration, the sequentially calculated hydrophobicity was insufficient to predict the aggregation propensity of two related sequences.


Only after the tertiary structural information was added and the scores were updated to account for surface exposure was the difference in aggregation propensity more easily seen.


## Conclusions


Developability characterizations for antibody discovery workflows are important as they are key to preventing downstream issues that cause delays and drive up costs. Aggregation propensity, for instance, can prevent the successful application of new therapeutics by decreasing stability and efficacy.


We have reviewed many web based tools to both predict *and* design structures with improved aggregation resistance.


We furthermore have integrated aggregation and 3D structure prediction tools in PipeBio which combines state of the art technology consolidated in a single platform.


‍Though the utility of the aggregation analysis at PipeBio is clear, some challenges ( *solutions still under development* ) still persist. Firstly, in theory the A3D scoring can take into account the dynamic nature of antibodies using software such as CABs to capture different conformations that may exist in solution.


However, this does not take into account the dynamics of the antibody when it is bound to its antigen, or correct for environmental factors such as pH. Complementary techniques will be required in the future to account for these limitations.


‍A second limitation is overall prediction. Though having a per-residue score is insightful, given many scientists have a large number of sequences to analyze at a time, a simple “go/no-go” score would dramatically increase throughput. This is a difficult challenge as the current state-of-the-art models, though boast strong predictive power on the training data, do not perform well on new datasets.


This brings about further challenges of handling false positives, where sequences are selected as *stable* when in reality will aggregate; and false negatives, where the end user loses out on potentially valuable sequences that were predicted to aggregate but would have been stable in practice.


In the future, as more experimental data is collected, deep learning models can be leveraged to help make stronger predictions that are more accurate and generalizable across domains.


# References


1. Raybould, M. I. J. & Deane, C. M. The Therapeutic Antibody Profiler for Computational Developability Assessment. *Methods Mol. Biol. Clifton NJ* **2313** , 115–125 (2022).


2. Fernández-Quintero, M. L. *et al.* Assessing developability early in the discovery process for novel biologics. *mAbs* **15** , 2171248 (2023).


3. Zhang, W. *et al.* Developability assessment at early-stage discovery to enable development of antibody-derived therapeutics. *Antib. Ther.* **6** , 13–29 (2023).


4. Ramazi, S. & Zahiri, J. Posttranslational modifications in proteins: resources, tools and prediction methods. *Database J. Biol. Databases Curation* **2021** , baab012 (2021).


5. Lu, X. *et al.* Deamidation and isomerization liability analysis of 131 clinical-stage antibodies. *mAbs* **11** , 45–57 (2019).


6. Gupta, S., Jiskoot, W., Schöneich, C. & Rathore, A. S. Oxidation and Deamidation of Monoclonal Antibody Products: Potential Impact on Stability, Biological Activity, and Efficacy. *J. Pharm. Sci.* **111** , 903–918 (2022).


7. Li, W. *et al.* Antibody Aggregation: Insights from Sequence and Structure. *Antibodies* **5** , 19 (2016).


8. Vázquez-Rey, M. & Lang, D. A. Aggregates in monoclonal antibody manufacturing processes. *Biotechnol. Bioeng.* **108** , 1494–1508 (2011).


9. Lee, S., Choi, M. C., Al Adem, K., Lukman, S. & Kim, T.-Y. Aggregation and Cellular Toxicity of Pathogenic or Non-pathogenic Proteins. *Sci. Rep.* **10** , 5120 (2020).


10. Bannas, P., Hambach, J. & Koch-Nolte, F. Nanobodies and Nanobody-Based Human Heavy Chain Antibodies As Antitumor Therapeutics. *Front. Immunol.* **8** , (2017).


11. Rahbarizadeh, F., Ahmadvand, D. & Sharifzadeh, Z. Nanobody; an Old Concept and New Vehicle for Immunotargeting. *Immunol. Invest.* **40** , 299–338 (2011).


12. Pain, C., Dumont, J. & Dumoulin, M. Camelid single-domain antibody fragments: Uses and prospects to investigate protein misfolding and aggregation, and to treat diseases associated with these phenomena. *Biochimie* **111** , 82–106 (2015).


13. Kunz, Patrick *et al.* The structural basis of nanobody unfolding reversibility and thermoresistance | Scientific Reports.


14. Baek, J. HIC as a Complementary, Confirmatory Tool to SEC for the Analysis of mAb Aggregates.


15. Bailly, M. *et al.* Predicting Antibody Developability Profiles Through Early Stage Discovery Screening. *mAbs* **12** , 1743053 (2020).


16. Zhou, Y. *et al.* SSH2.0: A Better Tool for Predicting the Hydrophobic Interaction Risk of Monoclonal Antibody. *Front. Genet.* **13** , 842127 (2022).


17. Tartaglia, G. G. & Vendruscolo, M. The Zyggregator method for predicting protein aggregation propensities. *Chem. Soc. Rev.* **37** , 1395–1401 (2008).


18. Chen, Z. *et al.* iFeature: a Python package and web server for features extraction and selection from protein and peptide sequences. *Bioinformatics* **34** , 2499–2502 (2018).


19. Chen, K., Jiang, Y., Du, L. & Kurgan, L. Prediction of integral membrane protein type by collocated hydrophobic amino acid pairs. *J. Comput. Chem.* **30** , 163–172 (2009).


20. Sankar, K., Krystek Jr, S. R., Carl, S. M., Day, T. & Maier, J. K. X. AggScore: Prediction of aggregation-prone regions in proteins based on the distribution of surface patches. *Proteins Struct. Funct. Bioinforma.* **86** , 1147–1156 (2018).


21. Abanades, B. *et al.* ImmuneBuilder: Deep-Learning models for predicting the structures of immune proteins. *Commun. Biol.* **6** , 1–8 (2023).


22. Raybould, M. I. J. *et al.* Five computational developability guidelines for therapeutic antibody profiling. *Proc. Natl. Acad. Sci.* **116** , 4025–4030 (2019).


23. Zhong, Z. *et al.* Positive charge in the complementarity-determining regions of synthetic nanobody prevents aggregation. *Biochem. Biophys. Res. Commun.* **572** , 1–6 (2021).


24. Kunz, P. *et al.* Exploiting sequence and stability information for directing nanobody stability engineering. *Biochim. Biophys. Acta* **1861** , 2196–2205 (2017).


25. Dudgeon, K. *et al.* General strategy for the generation of human antibody variable domains with increased aggregation resistance. *Proc. Natl. Acad. Sci.* **109** , 10879–10884 (2012).


26. Kant, R. van der *et al.* Prediction and Reduction of the Aggregation of Monoclonal Antibodies. *J. Mol. Biol.* **429** , 1244 (2017).


27. Ebo, J. S. *et al.* An in vivo platform to select and evolve aggregation-resistant proteins. *Nat. Commun.* **11** , 1816 (2020).


28. Kuriata, A. *et al.* Aggrescan3D (A3D) 2.0: prediction and engineering of protein solubility. *Nucleic Acids Res.* **47** , W300–W307 (2019).


29. Jamroz, M., Kolinski, A. & Kmiecik, S. CABS-flex predictions of protein flexibility compared with NMR ensembles. *Bioinformatics* **30** , 2150–2154 (2014).


30. Schymkowitz, J. *et al.* The FoldX web server: an online force field. *Nucleic Acids Res.* **33** , W382–W388 (2005).


31. Voynov, V., Chennamsetty, N., Kayser, V., Helk, B. & Trout, B. L. Predictive tools for stabilization of therapeutic proteins. *mAbs* **1** , 580–582 (2009).


32. Chennamsetty, N., Voynov, V., Kayser, V., Helk, B. & Trout, B. L. Design of therapeutic proteins with enhanced stability. *Proc. Natl. Acad. Sci. U. S. A.* **106** , 11937–11942 (2009).


33. Navarro, S. & Ventura, S. Computational methods to predict protein aggregation. *Curr. Opin. Struct. Biol.* **73** , 102343 (2022).


34. Kyte, J. & Doolittle, R. F. A simple method for displaying the hydropathic character of a protein. *J. Mol. Biol.* **157** , 105–132 (1982).


35. Kuriata, A., Iglesias, V., Kurcinski, M., Ventura, S. & Kmiecik, S. Aggrescan3D standalone package for structure-based prediction of protein aggregation properties. *Bioinformatics* **35** , 3834–3835 (2019).


36. Jespers, L., Schon, O., James, L. C., Veprintsev, D. & Winter, G. Crystal Structure of HEL4, a Soluble, Refoldable Human VH Single Domain with a Germ-line Scaffold. *J. Mol. Biol.* **337** , 893–903 (2004).
