---
schema_version: "1.0.0"
document_id: "5032ed82f5b2314bdf468be2b88752215d9f7717103a33d99d87203fa929e87f"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/antibody-library-design"
published_at: "2022-09-15T07:00:00+00:00"
first_seen_at: "2026-07-29T00:41:24.192337+00:00"
fetched_at: "2026-07-29T00:41:27.899557+00:00"
content_hash: "sha256:00e935758ce01405088aecd0735cb06c54766152c70674ae9467c00099688a44"
---

# Antibody library design

## What are antibody libraries?


Antibody libraries are collections of unique antibody clones derived from B-lymphocytes of naïve or immunized donors. Upon exposure to antigens, antibodies undergo *in-vivo* or *in-vitro* affinity maturation and variable gene (V-gene) hypermutation to produce specific clones.1


These clones are inserted into a vector and expressed in their genotype through molecular techniques for therapeutic purposes.2 Display methods are used to expose antibody libraries to specific immobilized antigens followed by multiple cycles of elution and subsequent binding of antibody clones. This allows obtaining higher levels of specificity against target antigens.2, 3


Different sources and specific purposes dictate the basis of the construction of antibody libraries. They include immune, naïve, semi-synthetic and synthetic libraries. Immune and naïve libraries are generated only from naturally occurring sequences, whereas synthetic and semi-synthetic libraries are generated using computational and chemical methods.2


## Key considerations in antibody library design


Key factors for designing mAbs and constructing antibody libraries include biophysical and chemical properties such as solubility, viscosity, glycosylation, amino-acid modifications, stability, and binding specificity when formulated against specific molecular targets. 4 Apart from decoding the genetic basis of antigen-antibody interactions, knowledge of molecular pathways, and the exact time and space for initial target interaction (extracellular space vs. cell membrane) are also critical in developing efficient mAbs. 5 Moreover, while developing antibody libraries, ensuring diversity also drives its success in clinical applications. 6


The main bottlenecks while designing diverse antibody libraries come from their sources, especially when natural sources are used. 8 A closer look at all these factors will elucidate the complexities involved in antibody library design and understand its underlying opportunities. Antibodies selected from an efficiently designed library with stringent biopanning conditions have higher affinities for their targets and are highly efficient. 3


**Figure 1.** Creating diversity in antibody libraries and some key design aspect of library design


## Design approaches for antibody libraries


Choosing the target (antigen) product profile (TPP) and the generation strategy are two main approaches behind the successful design of therapeutic antibody libraries. A rational approach to antibody library design involves prior knowledge of the antibody structure (from either X-ray crystallography, nuclear magnetic resonance, or cryo-electron microscopy data), antibody sequence, antibody-antigen interactions, and their epitope-binding probabilities.7


Comprehensive knowledge about the complementarity determining regions (CDRs/paratopes), an idea of somatic hypermutations, the interactions between the Fab fragment and the epitopes, different amino-acid sequences, and their correlations to their locations in the antibody genes is also required. Minute structural details help in refining the efficacy and developability profile of the antibodies.8, 9


## Types and sources of antibody libraries


Naïve libraries are constructed from B-cell populations of non-immunized donors. In naïve libraries, antibody fragments (specific areas on the heavy and light chains) from donor B-cells are amplified using PCR in a series of reactions following which they are stored in plasmid vectors with their diversity assessed using NGS and validated using control antigens to assess their functionality against specific targets.10


Immune libraries are synthesized after immunization or exposure to an antigen, with knowledge of the specific target antigen. For example, the keyhole limpet hemocyanin (KLH) coupled p-nitrophenyl phosphonamidate (NPN) antigen for murine antibodies and specific viruses (e.g., Rabies) and tumor-specific sera for human antibodies.10


Once target-specific antibodies are synthesized using *in-vivo* and *in-vitro* methods, they can be mutated to create a new set of antibodies.


These antibodies can be genetically re-engineered and screened for potential binding properties against novel target antigens. This cycle is repeated multiple times to get a diverse and efficient antibody library. 8 Prevalent methods for construction of antibody libraries are combinatorial in nature and apply molecular engineering of antibody fragments from both human and animal sources where antibody genes are inserted into vectors. Antibodies can then be expressed and selected through display methods.


Major[display methods](https://pipebio.com/blog/antibody-display-technologies) include *in-vitro* phage display, ribosomal display, yeast display, and mammalian cell-surface display. 7, 11 Displayed antibody formats include single-chain variable fragments (scFv) or fragment antigen binding (Fab) regions from, for example. murine, camelid, rabbit or human hosts on the surface of bacteriophages, ribosomes, yeast cells or mammalian cell lines like HEK293 or CHO.‍


Generic Name


Product / Company


Format


Antigenic Target


Year of Approval


Adalimumab


Humira / Abbvie


IgG1


TNFα


2002


Ranibizumab


Lucentis / Novartis, Roche / Genentech


Fab-IgG1


VEGFA


2006


Belimumab


Benlysta / Human genome Sciences (HGS), GlaxoSmithKline (GSK)


IgG1


BLyS


2011


Ramucirumab


Cyramza / Lilly, Imclone


IgG1


VEGFR2


2014


Necitumumab


Portrazza / Eli Lilly


IgG1


EGFR


2015


Ixekizumab


Taltz / Eli Lilly


IgG4


IL-17a


2016


Avelumab


Bavencio / Merck Serono, Pfizer


IgG1


PD-L1


2017


Lanadelumab


Takhzyro / Dyax, Shire


IgG1


pKal


2018


Inebilizumab


Uplizna / AstraZeneca, Medimmune, Viela Bio


IgG1


CD19


2020


Tralokinumab


Adbry / AstraZeneca, Medimmune, Leo Pharma


IgG4


IL-13


2021


Faricimab


Vabysmo / Roche


Bi-Fab VEGFA


Ang2


2022


**Table 1.** Commercial therapeutic antibodies derived from Antibody Libraries16‍


## Designing diverse in-vitro antibody libraries


The most common way to introduce diversity in an antibody library is to have a larger pool of unique sequences by carefully randomising amino-acid sequences of either an existing repertoire of pre-engineered proteins or from immunized donors.9 Diversity also depends on the number of donors, the type of donor tissue, the types of variable regions of an antibody from which these amino-acid sequences are amplified and the choice of V-gene frameworks used.17 Different display methods can then be used to express these diverse antibody libraries of different sizes.


Bacterial systems like *E. coli* can typically yield 109 colony forming units (CFU), and up to 1012 scFv of IgG using a thousand transformations, whereas ribosome concentration in cell-free systems can yield up to 1015 ribosomes.9


One can enhance the functional diversity of the antibody library by limiting the sequence randomization to only specific parts of the CDR region, introducing degenerate nucleotides along these selected CDR regions, using pre-defined CDR sequences, or even recombining naïve heavy and light chains.9


**Figure 2.** The design and creation of a humanized VHH library by Moutel et al., 2016 (adapted from figure 1 in publication)29


### Non-targeted methods


Non-targeted methods for introducing mutations in an antibody library include error-prone PCR, chain shuffling, use of mutator *E-coli* , DNA shuffling by random fragmentation and site-saturation mutagenesis.10 Error-prone PCR can be used to induce mutations across the entire antibody gene thus producing a ready mutagenized library.18, 19


Error-prone PCRs are used in combination with ribosomal display methods since a PCR-based amplification is the first step of construction of ribosomal libraries and that it can generate clones of strengths 1012~15 without needing the transformation step.10


Using mutator bacterial strains like *E-coli* with phage display libraries can also be a viable method for inducing antibody diversity but it also mutates the vector backbone and thus needs subsequent re-cloning of just the antibody gene fragment.18, 20


‍Another method to induce diversity is to first fragment existing DNA pools and then introduce PCR (can be error-prone PCR or mutator bacterial strains) to amplify these fragments building a diverse library.10


Chain shuffling refers to the sequential shuffling of sequences in one of the heavy and light chain variable regions owing to their compatibility with multiple antigens and their poly-functionality. The shuffling is done with repertoires of V genes from unimmunized donors.21, 22


The main limitation with random mutagenesis is that it becomes difficult to locate the exact mutation responsible for binding affinity which makes deciding on the next steps for further mutagenesis to further enhance binding affinity, challenging.


### Targeted methods


Targeted methods for introducing mutations include site-specific CDR mutagenesis and CDR walking applied to shorter, known target sequences. As suggested by the name, site-specific or site-directed mutagenesis refers to inducing mutations in known, target regions. Since the CDR regions are known to have the highest binding affinity, these regions have been traditionally targeted in site-directed mutagenesis.


However, some have preferred to preserve the V(H) CDR3/ L(H) CDR3 regions intact due to risks of losing binding affinity, and have instead targeted other CDR regions that might be useful in removing or reducing ‘low contact / repulsive residues’ with better kinetics and binding affinities.24 Site-directed CDR-specific mutagenesis is also less likely to induce immunogenicity in comparison to mutations in the more conserved regions.24


Site-saturation mutagenesis refers to the substitution of a single amino acid in any of the other 19 substituents which then leads to the formation of a library with a different set of mutated codons in the target positions.23 When done with alanine, it is also known as alanine scanning mutagenesis.


CDR walking helps in optimizing the antibody binding sites by sequentially mutating the CDRs in a stepwise manner. After every round of mutation, the best mutant is used as the template for the subsequent round of mutagenesis and selection. This creates a more diverse and high-affinity batch of antibodies for libraries.25[Yang et al.](https://link.springer.com/chapter/10.1007/978-3-662-04605-0_38) developed a high affinity anti-HIV gp 120 Fab by the CDR walking strategy with a 420-fold increase in affinity (Kd=1.5x10-11 M) whereas Schier et. al. isolated an anti-c-erbB-2 scFv with picomolar affinity (Kd=1.3x10-11M) using CDR walking methods.25


Despite being more targeted and complimentary to immunization methods, the experimental setups and multiple iteration assays for *in-vitro* assays are tedious and require considerable amounts of resources for a marginal increase in antibody library diversity, and limited transformation efficiencies, especially in eukaryotic display vectors.10, 11


‍Another drawback of *in-vitro* designs of antibody libraries is the absence of somatic hypermutations (SHM) which is the key to adaptive, antibody-mediated immunity in host systems.10


## Computational methods for antibody library design


The most modern methods applied in antibody library designs comprise *in-silico* computational techniques and more recently, implementing an *ab initio* approach, both of which are time and resource-efficient ways to get past the initial hurdles of antibody library design. Semi-synthetic and synthetic libraries are generated using computational methods.


Owing to decades of research on antibody generation, there are multiple data sources for information on antibodies in different domains in the present day and bioinformatic engineering techniques like homology modelling, protein–protein docking and interface prediction are useful in developing therapeutic antibodies.7 The most important of these are the Structural Antibody Database or the SAbDab, the Database for ImmunoGlobulins with Integrated Tools or the DIGIT, the Immune Epitope Database or the IEDB, and the International IMmunogGeneTics Information System or the IMGT.26


These databases provide crucial information to select the heavy chain and light chain variable region, optimize their orientation, selection of the CDR-H3 and non-CDR-H3 loops, and their optimization as well.8[Antibody numbering](https://pipebio.com/blog/antibody-numbering) schemes like Kabat (number-based), Chothia (structure-based), and Aho (structure-based) help in annotating and reflecting structurally equivalent residue positions within an antibody sequence when performing sequence analysis.8


Four methods, namely OptCDR, OptMAVEn, AbDesign, and RosettaAntibodyDesign are used predominantly for *ab initio* design of antibodies based on antigen-antibody interface prediction. Antigen-antibody interfaces (CDR-paratopes) are determined using statistical approaches like Antibody i-Patch, Paratome or machine learning algorithms like proABC, Parapred, and Antibody Interface Prediction. Antibody-specific epitopes are identified using programs like ASEP, BEPAR, ABEpar, EpiPred, PEASE, and MabTope among others.7


Similarly, programs like ClusPro, SurFit, FRODOCK, and SnugDock are used for antibody-specific docking.7 ‘Hot-spot grafting’, a process wherein binding site motifs from existing protein–protein complexes are transferred directly onto an antibody and ‘re-epitoping’, where existing antibodies are tested for binding capacities towards target epitopes, are novel approaches to select best candidates (binders) for constructing the semi-synthetic and synthetic libraries, albeit needing further optimization.


Implementing machine learning (ML) algorithms and conducting computational mutagenesis of CDR3 regions have been used for optimization of designed antibodies for further iterations in *in-silico* modelling for antibody library synthesis. Such methods improve antibody stability and affinity through a combination of conformational and free energy change optimization upon modification of specific residues using programs like OptCDR, OptMAVEn, AbDesign, and RosettaAntibodyDesign.7


In order to identify potential binders and evaluate antibodies from experiments incorporating antibody libraries, PipeBio’s bioinformatic analysis platform can be used to effectively analyze B-Cell and T-Cell receptors (BCR/TCR), VHH, scFv and peptide sequences. There are comprehensive tools to automate workflows, identify sequences from large repertoires, identify potential liabilities and screen antibodies to screen therapeutic antibody libraries and identify antibodies with optimal binding affinity. All in one secure, centralized cloud platform.


Attribute


Non-Targeted Methods


Targeted Methods


Computational Methods


Methodology


Randomly generated sequences along the antibody gene


Specific targeted sequences, especially around the CDR regions


Targeted methods aided by computer algorithms with known sequences from existing antibody databases


Library Size


Larger due to more randomized mutations in sequences ~ (1011 – 1015)


Moderate due to selective mutations along specific regions on the gene (~107 – 109)


Depends on the sources of libraries in database (combinatorial) models (epitope-paratope pairs) and optimization (in-vitro) methods


Diversification Techniques


Random mutagenesis, DNA shuffling, error-prone PCR, Site-saturation mutagenesis


Site-directed Mutagenesis, CDR mutagenesis, CDR walking


Molecular modelling, docking, protein properties, combinatorial mutation of key positions (computational mutagenesis)


Selection Methods


Biopanning, NGS, FACS


Biopanning, NGS, FACS, CIC, AC-SINS, SMAC


ML-based docking-simulations, developability prediction (statistical) scores


Developability


Naturally occurring sequences or optimized synthetic scaffold


Optimized for specific targets depending on the target protein profiles


Rational design based on databases or ML algorithms


Purpose


Provide a diverse library to be screened against target antigens later


Provide a specific set of antibodies for targeted and novel antigens


Resource-efficiency in initial phases of antibody-library design


Limitations


Difficulty in locating exact site for further mutation and optimization given its diversity


Specific antigenic targets might present specific challenges and biases which make further developability steps challenging; extensive experimental set-up required for marginal increase in diversity


Accurate modelling of the delicate equilibrium between biophysical and physiochemical properties of antibodies; need for in-vitro methods for downstream processing


**Table 2.** Comparison between different methods for creating Diverse Antibody Libraries7, 8, 9, 10‍


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Developability & optimization in computational antibody library design


The ultimate challenge in antibody discovery and development is to identify developability factors in order to optimize mAbs for desired binding affinity, high specificity, excellent stability, and other favorable physicochemical properties for therapeutic applications, given that these traits are often conflicting from an evolutionary perspective, and inducing mutations to improve one of them often tends to worsen the others.11, 27


Excluding antibody sequences with liabilities like unpaired cysteines, deamidation hotspots or motifs related to non-specific binding, high viscosity and low solubility, is one way to improve developability of high-quality therapeutic antibody libraries.27 For instance,[Teixeira et.al.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8654478/) embedded only pre-existing CDRs from natural antibodies into a genetically diverse panel of developable clinical antibody scaffolds to reduce liabilities.6, 27


**‍** Since the heavy chain CDR3 (CDR-H3) is known to be the most important in terms of binding affinity and specificity, these CDR-H3s were generated by PCR from B-cells from healthy donors, embedded with paired frameworks from previously validated therapeutic antibodies.6, 28


Another example is from a paper published in 2016 by Moutel et al.29, where the design and creation of one of the first synthetic libraries of humanized nanobodies is described (see Figure 2).


The library was constructed by using a strategy that included selecting for robust folding, controlled variability of CDR regions and low aggregation. First, a VHH scaffold displaying desired robustness was identified through screening of a naïve llama VHH library.


Subsequently, the scaffold was humanized by reducing the sequence distance between the camelid sequence and frequently observed motifs in human sequences. CDR grafting experiments were performed to validate the synthetic scaffold.


Computational methods with statistical scoring can be used to induce humanization and reduce immunogenicity in the resulting antibody sequences. The Humanness Score and Human String Content (HSC) are such methods. They are based on the sequence similarity between short, overlapping peptide sequences in animal-derived antibodies and the closest human antibody germline sequences.7 However, such scoring systems would need to be combined with structure-based design methods like re-surfacing (identifying solvent-exposed positions) and (ML) algorithms to predict T-cell epitope binding to MHC II complexes, to mitigate immunogenicity risks.7


Further optimization and risk mitigation procedures involve the identification of potential immune epitopes and aggregation prone regions (APRs) around the CDRs of the designed therapeutic antibodies. This approach also solves issues with the biophysical properties of the antibody like colloidal stability, solubility, viscosity, and pharmacokinetics.


## Conclusion


Generating diverse antibody libraries with high binding affinities takes a combination of computational and *in-vitro* methods through multiple cycles of screening and optimization. Generation of monoclonal antibodies has come a long way from hybridoma technology, to display methods and now in their third generation, through computational assays.


On one hand, *in silico* methods make the initial design and ideation process time and resource efficient with known sequences and scaffolds for designing antibodies whereas advanced molecular techniques like cross interaction chromatography (CIC), hydrophobic interaction chromatography (HIC), stand-up monolayer adsorption chromatography (SMAC), affinity-capture self-interaction nanoparticle spectroscopy (AC-SINS), aid in assessment of the developability and optimization of generated antibodies.


With the immense amounts of data accrued from NGS of antibodies and emerging techniques like data mining, machine learning and high-throughput screening, antibody libraries can be designed, screened, and adequately interrogated paving a faster and more efficient path to generate novel, effective, therapeutic antibodies.


# References


1. Antibody Library - an overview | ScienceDirect Topics \[Internet\]. \[cited 2023 Aug 8\]. Available from: https://www.sciencedirect.com/topics/medicine-and-dentistry/antibody-library


2. Lim BN, Tye GJ, Choong YS, Ong EBB, Ismail A, Lim TS. Principles and application of antibody libraries for infectious diseases. Biotechnol Lett. 2014 Dec;36(12):2381–92.


3. Lin CW, Lerner RA. Antibody Libraries as Tools to Discover Functional Antibodies and Receptor Pleiotropism. Int J Mol Sci. 2021 Apr 16;22(8):4123.


4. Sifniotis V, Cruz E, Eroglu B, Kayser V. Current Advancements in Addressing Key Challenges of Therapeutic Antibody Design, Manufacture, and Formulation. Antibodies. 2019 Jun 3;8(2):36.


5. Lu ZJ, Deng SJ, Huang DG, He Y, Lei M, Zhou L, et al. Frontier of therapeutic antibody discovery: The challenges and how to face them. World J Biol Chem. 2012 Dec 26;3(12):187–96.


6. Azevedo Reis Teixeira A, Erasmus MF, D’Angelo S, Naranjo L, Ferrara F, Leal-Lopes C, et al. Drug-like antibodies with high affinity, diversity and developability directly from next-generation antibody libraries. mAbs. 13(1):1980942.


7. Norman RA, Ambrosetti F, Bonvin AMJJ, Colwell LJ, Kelm S, Kumar S, et al. Computational approaches to therapeutic antibody design: established methods and emerging trends. Brief Bioinform. 2020 Sep 25;21(5):1549–67.


8. Krawczyk K, Dunbar J, Deane CM. Computational Tools for Aiding Rational Antibody Design. In: Samish I, editor. Computational Protein Design \[Internet\]. New York, NY: Springer New York; 2017 \[cited 2023 Jul 25\]. p. 399–416. (Methods in Molecular Biology; vol. 1529). Available from: http://link.springer.com/10.1007/978-1-4939-6637-0_21


9. Zhao Q, Buhr D, Gunter C, Frenette J, Ferguson M, Sanford E, et al. Rational library design by functional CDR resampling. New Biotechnol. 2018 Oct 25;45:89–97.


10. Ponsel D, Neugebauer J, Ladetzki-Baehs K, Tissot K. High Affinity, Developability and Functional Size: The Holy Grail of Combinatorial Antibody Library Generation. Molecules. 2011 May 3;16(5):3675–700.


11. Sormanni P, Aprile FA, Vendruscolo M. Third generation antibody discovery methods: *in silico* rational design. Chem Soc Rev. 2018;47(24):9137–57.


12. Mondon P. Human antibody libraries: A race to engineer and explore a larger diversity. Front Biosci. 2008;13(13):1117.


13. Alfaleh MA, Alsaab HO, Mahmoud AB, Alkayyal AA, Jones ML, Mahler SM, et al. Phage Display Derived Monoclonal Antibodies: From Bench to Bedside. Front Immunol \[Internet\]. 2020 \[cited 2023 Aug 23\];11. Available from: https://www.frontiersin.org/articles/10.3389/fimmu.2020.01986


14. 제품 \[Internet\]. \[cited 2023 Aug 23\]. Available from: http://www.insung.net/web/goods/goods.view.php?encData=Z2lkeD0xNjEwJmNhdGVnb3J5PTAwMjAxNiZjYXRlZ29yeTI9JmNhdGVnb3J5Mz0mY29kZT0wMDIwMTY=


15. Kramer RA, Marissen WE, Goudsmit J, Visser TJ, Clijsters-Van der Horst M, Bakker AQ, et al. The human antibody repertoire specific for rabies virus glycoprotein as selected from immune libraries. Eur J Immunol. 2005;35(7):2131–45.


16. Zhang Y. Evolution of phage display libraries for therapeutic antibody discovery. mAbs. 2023 Dec 31;15(1):2213793.


17. Schwimmer LJ, Huang B, Giang H, Cotter RL, Chemla-Vogel DS, Dy FV, et al. Discovery of diverse and functional antibodies from large human repertoire antibody libraries. J Immunol Methods. 2013 May 31;391(1):60–71.


18. Unkauf T, Hust M, Frenzel A. Antibody Affinity and Stability Maturation by Error-Prone PCR. In: Hust M, Lim TS, editors. Phage Display \[Internet\]. New York, NY: Springer New York; 2018 \[cited 2023 Aug 9\]. p. 393–407. (Methods in Molecular Biology; vol. 1701). Available from: http://link.springer.com/10.1007/978-1-4939-7447-4_22


19. Simons JF, Lim YW, Carter KP, Wagner EK, Wayham N, Adler AS, et al. Affinity maturation of antibodies by combinatorial codon mutagenesis versus error-prone PCR. mAbs. 2020 Jan 1;12(1):1803646.


20. Low NM, Holliger P, Winter G. Mimicking Somatic Hypermutation: Affinity Maturation of Antibodies Displayed on Bacteriophage Using a Bacterial Mutator Strain. J Mol Biol. 1996 Jul;260(3):359–68.


21. Marks JD, Griffiths AD, Malmqvist M, Clackson TP, Bye JM, Winter G. By–Passing Immunization: Building High Affinity Human Antibodies by Chain Shuffling. Nat Biotechnol. 1992 Jul;10(7):779–83.


22. Cheng M, Chan SYW, Zhao Q, Chan EYM, Au SWN, Lee SST, et al. Construction and Characterization of Single-Chain Variable Fragment Antibody Library Derived from Germline Rearranged Immunoglobulin Variable Genes. PLOS ONE. 2011 Nov 11;6(11):e27406.


23. Siloto RMP, Weselake RJ. Site saturation mutagenesis: Methods and applications in protein engineering. Biocatal Agric Biotechnol. 2012 Jul 1;1(3):181–9.


24. Lou J, Marks JD. Affinity Maturation by Chain Shuffling and Site Directed Mutagenesis. In: Kontermann R, Dübel S, editors. Antibody Engineering \[Internet\]. Berlin, Heidelberg: Springer Berlin Heidelberg; 2010 \[cited 2023 Aug 10\]. p. 377–96. Available from: http://link.springer.com/10.1007/978-3-642-01144-3_25


25. Takkinen K, Hemminki A, Söderlund H. Affinity and Specificity Maturation by CDR Walking. In: Kontermann R, Dübel S, editors. Antibody Engineering \[Internet\]. Berlin, Heidelberg: Springer Berlin Heidelberg; 2001 \[cited 2023 Aug 10\]. p. 540–5. Available from: http://link.springer.com/10.1007/978-3-662-04605-0_38


26. Zhao J, Nussinov R, Wu WJ, Ma B. In Silico Methods in Antibody Design. Antibodies. 2018 Sep;7(3):22.


27. Svilenov HL, Arosio P, Menzen T, Tessier P, Sormanni P. Approaches to expand the conventional toolbox for discovery and selection of antibodies with drug-like physicochemical properties. mAbs. 15(1):2164459.


28. D’Angelo S, Ferrara F, Naranjo L, Erasmus MF, Hraber P, Bradbury ARM. Many Routes to an Antibody Heavy-Chain CDR3: Necessary, Yet Insufficient, for Specific Binding. Front Immunol \[Internet\]. 2018 \[cited 2023 Jul 28\];9. Available from: https://www.frontiersin.org/articles/10.3389/fimmu.2018.00395


29. Moutel, Sandrine, et al. "NaLi-H1: A universal synthetic library of humanized nanobodies providing highly functional antibodies and intrabodies." Elife 5 (2016): e16228.
