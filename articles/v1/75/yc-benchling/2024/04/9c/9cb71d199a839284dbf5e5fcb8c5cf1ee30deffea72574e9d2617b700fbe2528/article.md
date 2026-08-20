---
schema_version: "1.0.0"
document_id: "9cb71d199a839284dbf5e5fcb8c5cf1ee30deffea72574e9d2617b700fbe2528"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/bruker-cellular-analysis-beacon"
published_at: "2024-04-04T07:00:00+00:00"
first_seen_at: "2026-07-29T00:41:24.192337+00:00"
fetched_at: "2026-07-29T00:41:27.899557+00:00"
content_hash: "sha256:569dafeddda3e9dbfa4eaa4f3bb30875eb609d8f28a38d1b10c97c55772efd70"
---

# Bruker Cellular Analysis Beacon® Data in PipeBio Software

## Introduction


We have been analyzing some single cell Next-generation sequencing (NGS) and multi-target cross-reactivity assay data from the Beacon B-cell workflow in PipeBio. In this post, we will summarize some of the analyses performed in PipeBio.


The data is from a study to detect antigen-specific and cross-reactive antibodies against vascular endothelial growth factor receptor (VEGFR) antigens in an immunized mouse. These are important cell surface protein receptors that mediate cellular responses to VEGF signaling proteins, most often in endothelium tissue. VEGF-VEGFR signals play an important role in physiological and pathological angiogenesis in diseases such as preeclampsia, aged macular degeneration, nephrotic syndrome, and cancer[1](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3936422/) .


This is a perfect example of how DNA sequence data analysis can be more powerful when integrated with functional assay data, and visa-versa.


## The Beacon Optofluidic System


The Beacon Optofluidic System from Bruker Cellular Analysis (BCA; formerly Berkeley Lights) is an exciting technology that enables workflows for single cell analysis and functional characterization. Applications range from cell line development to T-cell profiling and antibody discovery. See[Bruker’s webpage on the Beacon Optofluidic System](https://brukercellularanalysis.com/products/instruments/the-beacon-optofluidic-system/) for a more detailed overview of its capabilities.


This technology offers an alternative to lengthy B-cell and hybridoma workflows for antibody discovery, dramatically reducing timelines and complexity (Fig. 1). Early adopters have proven its efficacy for ​​rapid VHH discovery directly from natural camelid immune repertoires[2](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10481890/) and neutralizing antibody discovery against Covid-19[3,](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8444149/)[4,](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6771990/)[5](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8194108/) .‍


**Figure 1.** Drastically reduce antibody discovery timelines and the cost of immunization and screening campaigns with the Beacon Optofluidic System. Image from[www.brukercellularanalysis.com](http://www.brukercellularanalysis.com/) .


## Functional and NGS sequencing data from the Beacon B-cell workflow at single cell resolution


Researchers at BCA designed an antibody discovery campaign to investigate the ability to immunize a mouse with multiple related antigens (VEGFR1, VEGFR2, and VEGFR3) to yield both antigen-specific and cross-reactive antibodies. This study investigated the specificity and sensitivity of the system, as well as demonstrated a method to decrease immunization and screening campaign costs. This could help to validate the accuracy of the Beacon workflow when posed with a challenging cross-reactive multiplex assay, while also demonstrating the need for both functional assay and sequencing data to provide better insight about recovered sequences to carry forward for lead characterization and optimization.


In brief, optoelectrical positioning was used to move single mouse B-cells into individual Nanopen® chambers on OptoSelect® chips, on the Beacon platform (Fig. 2). Secreted antibodies were diffused from the Nanopen chambers into a common channel and their antigen binding specificity and cross-reactivity against VEGFR1, VEGFR2, and VEGFR3 were measured using time-lapse fluorescent imaging assays. As a positive control, fluorescence against mouse IgG was also tested. All screening assays were performed on cells across three chips, totaling ~35,000 cells screened.


‍ **Figure 2.** Using the Beacon system for B-cell screening and down-selecting candidates for antibody discovery. Image from[www.brukercellularanalysis.com](http://www.brukercellularanalysis.com/)


A total of 3,106 Nanopen chambers showed antigen specific binding activity (Table 1), most pens showing specificity for one VEGFR antigen.‍


Mouse IgG


VEGFR1


VEGFR2


VEGFR3


Number of pens


Hit rate


+


+


-


-


826


2.40%


+


-


+


-


1558


4.53%


+


-


-


+


634


1.84%


+


+


+


-


72


0.21%


+


+


-


+


6


0.02%


+


-


+


+


7


0.02%


+


+


+


+


3


0.01%


**Table 1.** Antigen-specific pens identified with for each phenotype on the Beacon platform


After the cellular assays, 1,142 B-cells were randomly selected for NGS. The mRNA encoding the variable heavy (VH) and light (VL) chains of IgG were sequenced using Illumina NGS (Fig. 3). Paired-end 150bp reads were assembled into full-length heavy and light variable regions (contigs) for each single cell using Bruker’s proprietary PrimeSeq™ BCR software.


**Figure 3.** OptoSeq Barcoded BCR enables accurate sequencing of full-length, paired heavy/light chains (VH/VL) from >1000 antibodies from a single run on standard Illumina NGS sequencing platforms. A) mRNA capture and cDNA synthesis are performed on-chip and cDNA amplification is performed off-chip. B) The OptoSeq Barcoded BCR NGS Prep kit and Nextera XT DNA Library Prep kit are used to prepare NGS libraries for 150x150 sequencing compatible with standard Illumina sequencing platforms. Image taken from[https://brukercellularanalysis.com/resource/opto-plasma-b-discovery-4-0-workflow/](https://brukercellularanalysis.com/resource/opto-plasma-b-discovery-4-0-workflow/)


## Analysis of Beacon Data in PipeBio


An overview of the analysis pipeline performed is shown in Figure 4.


**Figure 4.** An overview of the analysis steps performed in PipeBio


A total of 2,466 assembled contig sequences (1,041 VH and 1,425 VL) were imported into PipeBio along with the binding assay data, which was associated with each cell. Sequences were annotated using an IgG scaffold and the mouse antibody germline database with IMGT definitions as a reference. Almost all sequences (2,422 in total, 98.2%) were correctly annotated, and only two cells had no correct sequences.


VH and VL chains were paired together using a unique single cell identifier, resulting in 1,166 VH-VL pairs. Cells with only VH or VL sequences only were excluded from downstream analysis (199 cells). An additional filtering step further excluded cells with more than one VH-VL pair, resulting in 781 cells with a single VH-VL pair. Note, the cells with multiple VH-VL pairings can also be retained for further re-expression analysis. Figure 5 shows a summary of the number of cells and sequences at each step of the analysis.


**Figure 5.** A schematic summary of the number of cells, VH & VL sequences and VH-VL pairs at various stages of the analysis.


To explore the chain pairing, a chain network diagram was generated for the 1,562 sequences (781 VH + 781 VL) found in the 781 cells (Fig. 6).


**Figure 6.** Chain network of VH (dark purple) and VL (light purple) DNA sequences found in 781 cells with one VH-VL pair. Each node is a unique sequence and the size indicates how often the sequence is observed. The grouping indicates how sequences are paired. The grouping in Figure 7 is highlighted with a red square


Most heavy and light chains form unique pairs, as shown by the sparse grouping pattern in the plot. Some larger nodes indicate the same VH and/or VL chains are used by multiple cells, but the grouping indicates variation in how specific sequences are being paired. For example, the grouping in Figure 7 shows a VH chain that appears in seven cells. In those seven cells, it is paired with different VL chains: the same two VL chains in two cells, another two VL chains in two other cells, and three additional VL chains in three other cells.


**Figure 7** . A close up on one grouping in the chain network with one VH sequence (dark purple) in seven cells and five VL sequences.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Clustering


The 1,562 sequences found in the 781 cells with a single VH-VL pair were clustered based on VH and VL independently with a 100% identity threshold, resulting in 627 VH clusters and 591 VL clusters (Fig. 8). The independent cluster assignments were combined to give 682 unique VH-VL clusters. This means that all sequences belonging to the same combined (union) cluster all have the same VH and VL sequence pairs. Most VH-VL clusters had one VH-VL sequence pair and 76 had more than one (Fig. 9).


‍ **Figure 8.** Schematic summary of the clustering steps and subsequent filtering


‍ **Figure 9.** Bar chart of VH-VL cluster sizes


## Combining antibody sequences with binding assay data to identify reliable candidates


We aimed to find reliable antibody sequences that could be good candidates for further testing. If the same VH-VL sequence pair in different cells show the same binding profile, we can be more confident about the sequence and binding result. If, however, the same sequence shows different binding results in different cells, the sequence (or binding data) may not be so reliable.


To look for variability in binding profiles between cells with identical VH-VL sequence pairs, the clusters with one VH-VL sequence pair were filtered out. The remaining 76 clusters (175 VH-VL sequences) were used to generate honeycomb plots to group cells with the same sequences together and inspect their binding for the three antigens (Fig. 10).‍


**Figure 10.** Each node is a VH-VL sequence pair (175 pairs in total). Because there is one pair per cell, we can think of each node as being a single cell. They are grouped together by their VH-VL cluster group of identical sequences. The colors indicate positive (TRUE, orange) and negative (FALSE, blue) binding for the three antigens.


The vast majority (88% on average) of VH-VL clusters show a consistent binding profile for cells with identical sequences (Table 2). The largest cluster of 5 identical VH-VL sequence pairs (toward the center of the honeycomb plots) shows a consistent positive binding for VEGFR2 and negative binding for VEGFR1 and VEGFR3 for all five member cells.‍


VEGFR1


VEGFR2


VEGFR3


Average


Number of clusters where all cells agree


64


67


70


67


Number of clusters where not all cells agree


12


9


6


9


**Table 2.** Summary of binding profile variation across cells belonging to the same cluster


‍The clusters that show a discrepancy in binding profiles (12, 9, and 6 clusters for VEGFR1, VEGFR2, and VEGFR3, respectively) can be considered as either unreliable sequences and/or unreliable antigen-specific calling on the Beacon system. It may be prudent to examine those pens again in more detail and/or possibly exclude these sequences from downstream analyses. Focusing downstream lead discovery efforts on the clusters with more consistent and reproducible binding profiles may be better.


## Combining antibody sequences with binding assay data to examine cross reactivity


Eighty-eight of the original set of B-cells showed evidence of cross-reactivity (Table 1). If the cross-reactivity can be verified in independent cells with the same VH-VL sequence pair, we can be more confident that the cross reactivity is a genuine biological signal. If, however, cells with the same sequence show different cross reactivity profiles, it could be a technical artifact.


To examine cross-reactivity in some more detail, a multiple alignment of the VH sequences from the 76 clusters with more than one VH-VL sequence pairs (i.e. 175 VH sequences) was performed using the MAFFT algorithm[6](https://pubmed.ncbi.nlm.nih.gov/12136088/) . and a phylogenetic tree was generated for visualization (Fig. 11). The binding assay data was plotted against the sequences in the tree.


**Figure 11.** Phylogenetic tree of 175 VH sequences. The green shading indicates the cluster size and the orange and blue colors represent the positive/true and negative/false binding activity for each antigen. The two cells that show evidence of cross-reactivity between VEGFR1 and VEGFR2 are highlighted in red. The scale bar shows the branch length, which can roughly be thought of as a fraction of residue changes between pairs of sequences.‍


Only two cells (D82744_13464 and D82744_614) showed evidence of cross reactivity between VEGFR1 and VEGFR2, in this subset of sequences. These two cells belong to clusters 132 and 656, respectively. All of the sequences in these clusters were extracted and re-aligned for a closer look (Fig. 12).


**Figure 12.** Phylogenetic tree of VH sequences in VH-VL clusters 134 and 656, each with one potentially cross-reactive cell (D82744_13464 and D82744_614, respectively). Coloring is the same as in Figure 11. The CDR-H3 and CDR-L3 sequences are also shown.


The cross-reactive cell D82744_13464 is part of a cluster of three cells (cluster id = 134), all with identical VH and VL sequences. The cross-reactivity is not observed in the other two cells. Indeed, all three cells differ in their binding profiles. D82744_9959 is negative for all three antigens and D82749_8272 is positive for VEGFR2 only.


The cross-reactive cell D82744_614 is part of a cluster of two cells (cluster id = 656). The identical VH and VL sequences are showing different binding profiles, with D82747_11281 showing positive binding for VEGFR2 only.


The lack of concordance between cells of the same sequence and their cross-reactive binding profiles suggests the cross-reactive signal might be a technical artifact in these specific cases. For example, we might need to look again at the choice of assay reagents, fluorophores, and/or how the Beacon binding assay activity was assessed.


## Conclusion


In this post, we demonstrate that the Beacon system can efficiently recover high quality variable heavy and light chain sequences from cells that have been pre-screened for functional binding. Furthermore, the sequence and assay data can be brought together and analyzed in tandem in the easy-to-use PipeBio platform.


Relying on single cell assay data alone for calling antigen specificity and/or cross-reactivity can be challenging due to cryptic technical artefacts. Likewise, relying on sequence data alone can be equally challenging, because of a lack of phenotypic context. When analyzed together, there are new analyses that can be performed to accelerate decision making toward more promising leads and allowing us to dive more deeply into the accuracy of the initial assay scoring.


This post demonstrates some of those new analyses. In some of the clusters listed above where identical sequences were recovered from multiple cells/pens, but assay results did not align, this could prompt deeper investigation into those assay results, which may not have been the case without the sequence data.


## References


1.


Shibuya M. VEGF-VEGFR Signals in Health and Disease. Biomol Ther (Seoul). 2014 Jan;22(1):1-9. doi: 10.4062/biomolther.2013.113. PMID: 24596615; PMCID: PMC3936422.


2.


Shapiro MB, Boucher J, Brousseau A, *et al* . Alpaca single B cell interrogation and heavy-chain-only antibody discovery on an optofluidic platform. Antib Ther. 2023 Aug 17;6(3):211-223. doi: 10.1093/abt/tbad018. PMID: 37680350; PMCID: PMC10481890.


3.


Mullen TE, Abdullah R, Boucher J, *et al* . Accelerated antibody discovery targeting the SARS-CoV-2 spike protein for COVID-19 therapeutic potential. Antib Ther. 2021 Aug 28;4(3):185-196. doi: 10.1093/abt/tbab018. PMID: 34541454; PMCID: PMC8444149.


4.


Jorgolli M, Nevill T, Winters A, Chen I, *et al.* Nanoscale integration of single cell biologics discovery processes using optofluidic manipulation and monitoring. Biotechnol Bioeng. 2019 Sep;116(9):2393-2411. doi: 10.1002/bit.27024. Epub 2019 Jun 24. PMID: 31112285; PMCID: PMC6771990.


5.


Zost SJ, Gilchuk P, Chen RE, *et al* . Rapid isolation and profiling of a diverse panel of human monoclonal antibodies targeting the SARS-CoV-2 spike protein. Nat Med. 2020 Sep;26(9):1422-1427. doi: 10.1038/s41591-020-0998-x. Epub 2020 Jul 10. PMID: 32651581; PMCID: PMC8194108.


6.


Katoh K, Misawa K, Kuma K, *et al* . MAFFT: a novel method for rapid multiple sequence alignment based on fast Fourier transform. *Nucleic Acids Res* . 2002;30(14):3059-3066. doi:10.1093/nar/gkf436.
