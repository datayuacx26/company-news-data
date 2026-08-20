---
schema_version: "1.0.0"
document_id: "6d63d7a2ef1f4c04516567cd1602b7c3c6a14e2a06d9a3a67c263916bbcf2a88"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/predicting-antibody-structure-and-developability-tools-opportunities-and-challenges"
published_at: "2022-08-04T07:00:00+00:00"
first_seen_at: "2026-07-29T00:41:24.192337+00:00"
fetched_at: "2026-07-29T00:41:27.899557+00:00"
content_hash: "sha256:3342172fca559d0de481d8411a1100be6118090dd3eae2517e84110eed855186"
---

# Predicting antibody structure and developability: Tools, opportunities and challenges

## Why is structure prediction important?


‍Knowing the molecular conformation and the shape of a certain protein enhances our understanding of its function, biophysical properties ([developability](https://pipebio.com/blog/ab-developability-prediction) ) and the potential targets that it might bind to 1–3. Traditionally, protein structure determination relies on time-consuming and expensive methods such as X-ray crystallography, cryo-electron microscopy (cryo-EM) and nuclear magnetic resonance (NMR) 4,5.


For these reasons, protein structure prediction has emerged as an interdisciplinary research field that attracted scientists from multiple disciplines, including biochemistry, statistics, physics, and computer science 6, promising to decrease the cost and time involved in experimental protein structure determination. Thus, the increasing interest in the field (Figure 1) motivated the development of *in silico* tools and softwares that predict protein structures starting from the simple input of their amino acid sequences.


‍ **Figure 1.** The publication count per year that contains the keyword “protein structure prediction” has been increasing annually. Source: PubMed library.[Accessed Feb 2024](https://pubmed.ncbi.nlm.nih.gov/?term=protein%20structure%20prediction&timeline=expanded) .


## A brief history of structure prediction


Due to the increased interest in structure prediction, the field witnessed the establishment of the Protein Structure Prediction Centre 7 which conducts a community-wide experiment to assess the quality of these tools and softwares, famously known as the Critical Assessment of Methods in Protein Structure Prediction ([CASP](https://predictioncenter.org/) ), every two years.


Among other benchmarking criteria, CASP examines the performance of newly-developed structure prediction tools by checking the atomic deviation between predicted and experimental structures (Root Mean Square Deviation, known as **RMSD** ) on either the full protein structures or specific domain(s) 8. The lower the RMSD between the predicted and the experimental (ground truth) structures, the better the tool is.


Recently, CASP14 highlighted the drastic improvement that can be achieved on protein structure prediction accuracy when the principles of machine learning (ML) are integrated within these tools 9.


Among these tools, AlphaFold 10 and RoseTTAFold 11 have achieved impressive accuracy when trained on all the experimental structures (by their release date) in the publicly available Protein Data Bank (PDB) 12 . ‍


Despite these advancements, the accurate prediction of antibody structures, and therefore their structure-based developability parameter values, is still considered more challenging than other types of proteins 13,14.‍


## Why is predicting the structure of antibodies difficult?


Accurate antibody structure models are important to estimate or calculate structure-based developability parameters. There are two main reasons that make predicting antibody structures difficult:


### 1. The structural uniqueness of antibodies as a class of proteins


Briefly, the complete antibody molecule is formed by two identical heavy chains (H) and two identical light (L) chains, and both chains have constant ( C ) and variable (V) regions (Figure 2A) . While the constant regions of both chains are highly conserved among antibodies, and hence easier to predict their structures, the variable regions (Fv) are much more changeable in sequence which makes predicting their structures a challenging task (Figure 2B).


More specifically, and among the six CDR loops that gives each antibody molecule its unique antigen-recognition and binding properties 15, the conformation of the CDR3H has proven tricky to get right 16. This is because of its central interdomain orientation between the heavy and the light chains which plays a key role in shaping the antigen-binding domain of the antibody 17,18 (Figure 2C). ‍


**Figure 2.** The antibody structure. A) Schematic representation of an antibody molecule. B) The genetic composition of VH and VL domains. C) Structure of the variable domain (Fv). CDR: complementarity-determining region; FWR: framework. Adopted and inspired from19,20‍


### 2. The bottleneck of numbers


Relatively speaking, antibodies form a tiny proportion of the general protein population with experimentally-validated structures. Speaking numbers, out of more than 200,000 structures available in the PDB (Figure 3), only around 8,000 are antibodies 21,22, forming only around 4% of the total pool of proteins in the PDB. Such a minimal ratio results in antibodies (as a class of proteins) to be under-represented in the training datasets used to develop general machine-learning based structure prediction models (like AlphaFold). ‍


**Figure 3.** By the end of 2023, 214,118 general protein structures were experimentally determined and deposited in the protein data bank (PDB).[Accessed: Feb. 2024](https://www.rcsb.org/stats/growth/growth-released-structures) .‍


## The emergence of antibody-specific structure prediction tools


To overcome the above hurdles, scientists started developing antibody-specific structure prediction tools to better capture the conformations of antibody loops1 (some of which are summarised below in Table 1).


These tools implement either 1) template-based strategy where a CDR loop with a known structure is chosen as a template structure based on its sequence similarity to the query CDR loop and grafted on the antibody structure 23,24 or 2) ML approaches, inspired by AlphaFold and its competitors 14, where the principles of artificial intelligence and knowledge obtained from training on antibody structural data are implemented to perform structure prediction.


ML-based tools (some of which summarised below) showed better performance when compared to general-purpose structure prediction methods in predicting antibody structures because they better capture the conformation of the CDR loops. For instance, ABlooper, AbFold and IgFold achieved lower RMSD measures on average, and hence better structural prediction of the CDR3H, when benchmarked against AlphaFold 1,24–26.


Figure 4 provides examples that were documented in scientific literature. It is worth noting that an[updated version of AlphaFold](https://deepmind.google/discover/blog/a-glimpse-of-the-next-generation-of-alphafold/) has been announced in October 2023 and yet to be evaluated for its prediction of the CDR loop structures in antibodies.34‍


Recently, the[OpenFold Consortium](https://openfold.io/) also[announced](https://www.businesswire.com/news/home/20240219658831/en/OpenFold-Biotech-AI-Research-Consortium-releases-SoloSeq-and-Multimer-an-integrated-protein-Large-Language-Model-with-3D-structure-generation) two new open source structure prediction models: SoloSeq and OpenFold-Multimer. The two models (alongside the[existing OpenFold tool](https://github.com/aqlaboratory/openfold) ) are designed for prediction of single proteins and protein/protein complexes.


**Figure 4.** Examples showing that antibody-specific structure prediction tools achieve higher accuracy for CDR3H loop structure prediction. Adopted from 24, 25


## What are the limitations in regards to structural antibody developability?


Antibody prediction tools provide rigid structure output that we can use to measure structure-based developability parameters. But, several studies have shown that even with close resemblance to the ground truth structures, developability measures vary across structure prediction tools 14,27.


As antibody loops are very flexible, it is important to measure their developability on their dynamic structures rather than on their rigid ones. Indeed, only when implementing molecular dynamics (MD) to measure the developability parameters of antibodies, it is possible to reach higher agreement with developability measurements of those reported on experimental structures 26–28.


Currently, running molecular dynamics and generating developability data on the structural ensemble of antibodies are still considered lengthy and computationally-intensive procedures. However, ML-based MD may offer a high-throughput solution to this bottleneck to enhance the accuracy of computational structural antibody developability assessments 29.


## Brief of summary of antibody structure prediction tools


### **Table 1:** Summary of structure prediction tools that can be used to predict antibody structures


Tool


Type


Link


Group


Predicted molecule


ABodyBuilder


30


template-based


N/A (Tool has been deprecated)


Oxford Protein Informatics Group (OPIG), Oxford University, UK


Antibody variable regions (Fv)


ABodyBuilder-ML (incorporates ABlooper


1


)


ML: deep learning


[Link](https://opig.stats.ox.ac.uk/webapps/sabdab-sabpred/sabpred/abodybuilder/)


Oxford Protein Informatics Group (OPIG), Oxford University, UK


Antibody variable regions (Fv)


AbodyBuilder2 (a part of ImmuneBuilder2


31


)


ML: deep learning


[Link](https://opig.stats.ox.ac.uk/webapps/sabdab-sabpred/sabpred/abodybuilder2/)


Oxford Protein Informatics Group (OPIG), Oxford University, UK


Antibody and TCR variable regions (Fv), Nanobodies


Repertoire Builder


23


template-based


[Link](https://sysimm.org/rep_builder/)


Standley Lab, Osaka University, Japan


Antibody variable regions (Fv)


IgFold


24


ML: deep learning


[Link](https://github.com/Graylab/IgFold)


Gray Lab, John Hopkins University, USA


Antibody variable regions (Fv)


DeepAb


32


ML: deep learning


[Link](https://github.com/RosettaCommons/DeepAb)


Gray Lab, John Hopkins University, USA


Antibody variable regions (Fv)


AbFold


25


ML: deep learning


N/A


Multiple contributors (Fudan University, China; Hong Kong Graduate School of Advanced Studies; Palindromic Labs Limited, Hong Kong)


Antibody variable regions (Fv)


AlphaFold multimer


13


ML: deep learning


[Link](https://github.com/google-deepmind/alphafold)


Google DeepMind, London, UK


Protein-protein complexes and multimeric proteins (including antibody Fv).


H3-OPT


33


ML: deep learning


[Link](https://github.com/chdcg/H3-OPT)


Tsinghua TianLab, Tsinghua University, China.


Antibody variable regions (Fv)


OpenFold, OpenFold-Multimer and SoloSeq


ML: deep learning


[Link](https://github.com/aqlaboratory/openfold)


OpenFold Consortium


Protein structures and complexes


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## References


‍1. Abanades, B., Georges, G., Bujotzek, A. & Deane, C.M. ABlooper: Fast accurate antibody CDR loop structure prediction with accuracy estimation. Bioinformatics (2022) doi:10.1093/bioinformatics/btac016.


2. Bertoline, L. M. F., Lima, A. N., Krieger, J. E. &Teixeira, S. K. Before and after AlphaFold2: An overview of protein structure prediction. Front Bioinform 3, 1120370 (2023).


3. Fernández-Quintero, M. L. et al. Assessing developability early in the discovery process for novel biologics. MAbs 15, 2171248 (2023).


4. Alberts, B. et al. Analyzing Protein Structure and Function. (Garland Science, 2002).


5. Carroni, M. & Saibil, H. R. Cryo electron microscopy to determine the structure of macromolecular complexes. Methods 95, 78–85(2016).


6. Huang, B. et al. Protein Structure Prediction: Challenges, Advances, and the Shift of Research Paradigms. Genomics ProteomicsBioinformatics (2023) doi:10.1016/j.gpb.2022.11.014.


7. Moult, J., Pedersen, J. T., Judson, R. & Fidelis, K.A large-scale experiment to assess protein structure prediction methods. Proteins 23, ii–v (1995).


8. Kryshtafovych, A., Schwede, T., Topf, M., Fidelis, K.& Moult, J. Critical assessment of methods of protein structure prediction (CASP)-Round XIV. Proteins 89, 1607–1617 (2021).


9. Pearce, R. & Zhang, Y. Toward the solution of the protein structure prediction problem. J. Biol. Chem. 297, 100870 (2021).


10. Jumper, J. et al. Highly accurate protein structure prediction with AlphaFold. Nature 596, 583–589 (2021).


11. Baek, M. et al. Accurate prediction of protein structures and interactions using a three-track neural network. Science 373,871–876 (2021).


12. Berman, H. M. et al. The Protein Data Bank. NucleicAcids Res. 28, 235–242 (2000).


13. Evans, R. et al. Protein complex prediction with AlphaFold-Multimer. bioRxiv 2021.10.04.463034 (2022)doi:10.1101/2021.10.04.463034.


14. Fernández-Quintero, M. L. et al. Challenges in antibody structure prediction. MAbs 15, 2175319 (2023).


15. Marks, C. & Deane, C. M. Antibody H3 StructurePrediction. Comput. Struct. Biotechnol. J. 15, 222–231 (2017).


16. Teplyakov, A. et al. Antibody modeling assessment II.Structures and models. Proteins 82, 1563–1582 (2014).


17. Dunbar, J., Fuchs, A., Shi, J. & Deane, C. M.ABangle: characterising the VH–VL orientation in antibodies. Protein Eng. Des.Sel. 26, 611–620 (2013).


18. Bujotzek, A. et al. Prediction of VH-VL domain orientation for antibody variable domain modeling. Proteins 83, 681–695 (2015).


19. Schroeder, H. W., Jr & Cavacini, L. Structure and function of immunoglobulins. J. Allergy Clin. Immunol. 125, S41–52 (2010).


20. Kovaltsuk, A. et al. How B-Cell Receptor RepertoireSequencing Can Be Enriched with Structural Antibody Data. Front. Immunol. 8,1753 (2017).


21. Dunbar, J. et al. SAbDab: the structural antibody database. Nucleic Acids Res. 42, D1140–6 (2014).


22. Raybould, M. I. J. et al. Thera-SAbDab: the Therapeutic Structural Antibody Database. Nucleic Acids Res. 48, D383–D388 (2020).


23. Schritt, D. et al. Repertoire Builder: high-throughput structural modeling of B and T cell receptors. Mol. Syst. Des. Eng. 4, 761–768(2019).


24. Ruffolo, J. A., Chu, L.-S., Mahajan, S. P. & Gray,J. J. Fast, accurate antibody structure prediction from deep learning on massive set of natural antibodies. Nat. Commun. 14, 2389 (2023).


25. Peng, C., Wang, Z., Zhao, P., Ge, W. & Huang, C.AbFold -- an AlphaFold Based Transfer Learning Model for Accurate Antibody Structure Prediction. bioRxiv 2023.04.20.537598 (2023) doi:10.1101/2023.04.20.537598.


26. Bashour, H. et al. Biophysical cartography of the native and human-engineered antibody landscapes quantifies the plasticity of antibody developability. bioRxiv 2023.10.26.563958 (2023) doi:10.1101/2023.10.26.563958.


27. Park, E. & Izadi, S. Molecular Surface Descriptors to Predict Antibody Developability. bioRxiv 2023.07.18.549448 (2023) doi:10.1101/2023.07.18.549448.


28. Jain, T., Boland, T. & Vásquez, M. Identifying developability risks for clinical progression of antibodies using high-throughput in vitro and in silico approaches. MAbs 15, 2200540 (2023).


29. Noé, F., Tkatchenko, A., Müller, K.-R. & Clementi,C. Machine Learning for Molecular Simulation. Annu. Rev. Phys. Chem. 71,361–390 (2020).


30. Leem, J., Dunbar, J., Georges, G., Shi, J. & Deane,C. M. ABodyBuilder: Automated antibody structure prediction with data–driven accuracy estimation. MAbs 8, 1259–1268 (2016).


31. Abanades, B. et al. ImmuneBuilder: Deep-Learning models for predicting the structures of immune proteins. bioRxiv 2022.11.04.514231(2022) doi:10.1101/2022.11.04.514231.


32. Ruffolo, J. A., Sulam, J. & Gray, J. J. Antibody structure prediction using interpretable deep learning. Patterns (N Y) 3,100406 (2022).


33. Chen, H. et al. H3-OPT: Accurate prediction of CDR-H3loop structures of antibodies with deep learning. eLife (2023) doi:10.7554/elife.91512.1.


34. Google DeepMind AlphaFold Team and Isomorphic Labs Team: Performance and structural coverage of the latest, in-development AlphaFold model (2023).[Online source](https://deepmind.google/discover/blog/a-glimpse-of-the-next-generation-of-alphafold/) . Accessed on 26/02/2024.
