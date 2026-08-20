---
schema_version: "1.0.0"
document_id: "fc3aa98a7066bc284e3da8565dd3d93be62d38ef062eff9f90757a019ab07af6"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/selecting-antibodies-with-phage-display-technology"
published_at: "2022-10-07T07:00:00+00:00"
first_seen_at: "2026-08-03T09:07:34.565009+00:00"
fetched_at: "2026-08-05T03:48:46.961840+00:00"
content_hash: "sha256:a7861c0a9bfeab2660b35c3c5e09c2d028876ad577db1ca0c5b3a94d08d9eaa8"
---

# Selecting antibodies with phage display technology

## Introduction


Phage display is a powerful technology widely used for identification and selection of peptides binding to a broad range of targets. Since its first demonstration in 1985, the method has been proven extremely successful in a variety of fields, such as protein engineering, drug discovery, medical diagnosis and therapeutic targeting. The principle of phage display is based on modifying the phage DNA, which allows the expression of specific peptides on the phage coat. By creating large libraries of phages expressing different peptides, the method allows the isolation of phages which specifically bind to the target. The importance of phage display was recognized by awarding George Smith and Sir Gregory Winter the Nobel Prize in Chemistry 2018.


Phage display has revolutionized antibody drug discovery by enabling the isolation of high affinity antibodies to antigens from large combinatorial libraries. Such antibodies can then be used to create antibody-based drugs against cancer, infectious and inflammatory diseases. In 2002, Adalimumab was the first human antibody derived from phage display approved by FDA. Since then, 14 monoclonal antibodies derived using phage display technology have been approved until 2020, with many others in preclinical stage or in clinical trials1.


In this blog post, we present an overview of phage display technology applied to antibody drug discovery. Moreover, we highlight the advantages of using the PipeBio cloud-based bioinformatics platform to complement phage display experiments and to identify the most promising sequences after biopanning.


## Antibody Phage Display


A multitude of bacteriophages can be used to express antibodies, and the filamentous M13 is one of the most used phages for antibody phage display2. It contains a circular single-stranded DNA genome with nine genes. Foreign DNA can be incorporated into the phage coat protein genes, usually into the pIII gene, which results in the expression of the peptide of interest on the bacteriophage surface. Hence, the technique allows to create a direct link between genotype and phenotype through the phage. Antibodies can be introduced into the phage DNA in multiple formats, with smaller recombinant forms such as single chain fragment variable (scFv), nanobody fragments and fragments of antigen binding (Fab) being easier to express in the bacteriophage compared to full antibodies. Usually, the infection of *E.coli* containing a phagemid vector with a helper phage is needed for producing functional phage particles3 (Fig. 1).


**Figure 1:** Use of a phagemid in phage display. The phagemid vector contains the required elements for the production of the DNA that will be inserted into the phage. A helper phage is used to infect E.coli with the phagemid vector to ensure functional phage particles with only one copy of each peptide (monovalent display). Adapted from Jaroszewicz et al. 2022.


### Phage display library


The creation of a phage display library that allows the isolation of specific and high-affinity antibodies against diverse antigens is a crucial step in phage display methodology. Ideally, the phage clone population in the library should be large, diverse and display antibody variants as functional fragments. Human antibody libraries are constructed from mRNA or RNA extracted from B cells of either immunized or naïve donors. Immune libraries come from immunized IgG-secreting plasma cells from immunized donors, and are often used to find antibodies against a specific antigen in medical research3. On the other hand, naïve libraries are generated using IgM-reconfigured V genes from non-immunized donors, and are useful to discover antibodies binding to any kind of antigen. Synthetic libraries are usually developed from a single antibody with randomized complementary-determining regions (CDRs). CDRs are parts of the antibody variable chains that play an important role in antigen recognition, and particularly CDR-H3 affects the specificity and affinity of the binding (Fig. 2).


**Figure 2:** Types of antibody libraries. Image from www.sinobiological.com‍


### Affinity selection


In order to isolate specific antibodies binding to a target, the phage library is presented to the antigen, where an affinity selection process known as biopanning begins (Fig. 3). This process has five steps and starts with the antigen presentation, typically done on a 96- or 384-well microtiter plate. The desired antigen is often immobilized on a solid surface but can it also be in a solution. Binding of the phages with the highest affinity antibodies to the epitopes of the antigen is the second step of biopanning. Step three is a washing step that removes the phages that are not bound or weakly bound. In the fourth step, the bound phages are eluted by enzymatic cleavage using trypsin, pH changes or other denaturant agents. Lastly, the high-affinity phages are amplified by infecting *E.coli* to obtain a new enriched phage library. These five steps are repeated 3 to 5 times in order to significantly increase the affinity of the selected antibodies. The specificity of the phage clones with the antigen after the last round of biopanning is analyzed using ELISA, and the DNA of each individual phage is often analyzed with Sanger sequencing or Next-Generation Sequencing (NGS).


**Figure 3:** Phage display biopanning process. Adapted from Jaroszewicz et al. 2022.


## PipeBio and Phage Display


The[PipeBio Bioinformatics platform](http://www.pipebio.com/) provides built-in tools designed to accelerate antibody drug research by optimally analyzing large amounts of NGS and Sanger sequence data. Phage display technology can be complemented with the PipeBio platform in order to identify the most promising candidate sequences from a biopanning experiment.


In a recent[example](https://pipebio.com/blog/identify-sequence-candidates-from-biopanning/) , we used PipeBio’s tools to re-analyze the original NGS data from Hanke et al. (2020), where an alpaca antibody able to neutralize the SARS-CoV-2 spike protein was identified4. After annotating and clustering the NGS data from the immunized alpaca, we performed differential enrichment analysis to find the most differentially enriched clusters after each round of biopanning (Fig. 4). Differential enrichment analysis allows the identification of sequence clusters that are significantly overexpressed in one group compared to another.


**Figure 4:** Volcano plot after performing differential enrichment analysis on the immunized alpaca NGS data. Each point in the volcano plot represents a cluster, and the most differentially enriched clusters are highlighted in red.


Furthermore, we selected sequences from the most enriched clusters and aligned them (Fig. 5). Lastly, we picked the best 8 candidates from each of the main families, one of them being the Ty1 sequence shown to neutralize the SARS-CoV-2 spike protein.


**Figure 5:** Alignment of the selected sequences from the most differentially enriched clusters.‍


The described bioinformatics analysis was performed using the[PipeBio platform](http://www.pipebio.com/) and managed to identify a shortlist of novel sequence candidates from a biopanning experiment. PipeBio provides an user-friendly interface and is able to perform powerful computational analysis without the need of writing your own scripts. If you think that such kind of analysis could help your research, or would like to know more details about the different applications of PipeBio, don’t hesitate to[contact](http://www.pipebio.com/contact-us/) the team or[sign up](http://www.pipebio.com/sign-up/) for a free trial.


## References


1.


Alfaleh MA, Alsaab HO, Mahmoud AB, et al. Phage Display Derived Monoclonal Antibodies: From Bench to Bedside. *Front Immunol* . 2020;11:1986. Published 2020 Aug 28. doi:10.3389/fimmu.2020.01986


2.


Ledsgaard L, Kilstrup M, Karatt-Vellatt A, McCafferty J, Laustsen AH. Basics of Antibody Phage Display Technology. *Toxins (Basel)* . 2018;10(6):236. Published 2018 Jun 9. doi:10.3390/toxins10060236


3.


Węgrzyn G. Phage display and other peptide display technologies. *FEMS Microbiol Rev* . 2022;46(2):fuab052. doi:10.1093/femsre/fuab052


4.


Hanke L, Vidakovics Perez L, Sheward DJ, et al. An alpaca nanobody neutralizes SARS-CoV-2 by blocking receptor interaction. *Nat Commun* . 2020;11(1):4420. Published 2020 Sep 4. doi:10.1038/s41467-020-18174-5
