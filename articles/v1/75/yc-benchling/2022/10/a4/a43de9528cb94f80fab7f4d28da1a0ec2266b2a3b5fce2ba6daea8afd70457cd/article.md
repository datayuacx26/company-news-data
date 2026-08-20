---
schema_version: "1.0.0"
document_id: "a43de9528cb94f80fab7f4d28da1a0ec2266b2a3b5fce2ba6daea8afd70457cd"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/antibody-numbering"
published_at: "2022-10-12T07:00:00+00:00"
first_seen_at: "2026-07-29T00:41:24.192337+00:00"
fetched_at: "2026-07-29T00:41:27.899557+00:00"
content_hash: "sha256:97ce9409ac59981e88204578f580a199939f0d1482f5c59d21b7d8d00782b751"
---

# Antibody numbering schemes and CDR definitions

**Engineered monoclonal antibodies** (mAbs) play an increasingly important part in novel immunotherapeutics and to date, they constitute a large proportion of approved therapeutic antibodies.


The humanization of monoclonal antibodies involves conserving the IgG variable regions of the animal antibody and replacing the IgG constant region (Fc) with those from human immunoglobulins. Approved therapeutic antibodies include a majority of humanized full-length IgGs and other formats such as


-


sdAb (single-domain antibody), e.g. envafolimab


-


Fab (Fragment antigen-binding), e.g. certolizumab, idarucizumab and ranibizumab


-


scFv (single-chain fragment variable), e.g. brolucizumab


-


Bispecific antibodies, e.g. emicizumab, faricimab and amivantamab


With the plethora of innovative engineered antibody formats, a challenging aspect from an engineering and comparability perspective remains standard numbering and definitions of antibody regions. Accurate definitions of the crucial antigen-binding regions – the complementarity determining regions (CDRs) – is therefore paramount.‍


The strength of a specific paratope-epitope binding interaction depends on several characteristics of the binding sites, including the shape (CDR length, orientation) of the regions as well as biophysical properties such as the hydrophobicity and electrostatic forces of the interacting residues1. Framework regions (FRs) and the Fc may also affect the binding affinity or specificity of an antibody2.


While definitions of antibody variable regions may vary between different standard schemes, these definitions and numbering schemes serve as indispensable tools for properly distinguishing CDRs from FRs in antibody engineering and development.


## Standard definitions of antibody variable regions


Different antibody numbering schemes have been developed for accurate identification and subsequent comparison of variable regions of different antibodies. The most commonly used numbering schemes include IMGT, Kabat, Chothia, Martin (Enhanced Chothia or AbM) and Honneger’s numbering scheme (AHo).


The largest differences between the numbering schemes arise from where insertion points of amino acids are placed in regions. These insertion points allow amino acids to be inserted or deleted which occur as a result of somatic hypermutation. Below we will briefly describe the most commonly used antibody numbering schemes.


‍ **Figure 1.** IMGT, Kabat and Chothia amino acid positions for a subset of CDR-H3s of varying lengths. The figure shows the Cys and Trp anchor residues, marking the start and end of the CDR-H34


### **Kabat**


Building on earlier work in the 1970s, the Kabat numbering system (first published for immunoglobulins by Kabat et al. in 19793) was originally derived from observations from sequence alignments of light chain λ, κ and heavy chain sequences for a number of antibodies and for the α, β, γ, δ chain sequences for a number of TCRs.


The scheme defines specific positions where insertions and gaps may occur in CDRs and FRs. In the system, additional amino acid insertions are annotated with letters1.


As the original dataset was fairly limited and with standard-length variable regions, the numbering and definitions in Kabat are less flexible. Therefore, antibodies of unconventional length or with unconventional insertions or deletions may be overlooked when they do not match the more stringent regions defined in Kabat.


Another caveat with the Kabat numbering scheme is that it fails to take into account the topology or 3d structure of the binding domains.


### **IMGT**


The IMGT numbering scheme was originally based on alignment of germ-line V genes, spanning from FR1 to the beginning of the CDR3.The scheme was later extended to cover the entire variable region. The numbering runs from 1 to 128 based on the V-gene sequence alignment, with an insertion point only between positions 111 and 112 in the CDR3 for lengths exceeding 13 amino acids.


**Figure 2.** Amino acid bar charts on PipeBio showing the difference between linear numbering and IMGT numbering for the CDR-H3 region in a VHH


‍The sequence alignments behind the numbering scheme are based on a complete reference gene database, encompassing the entire immunoglobulin superfamily. This makes IMGT a widely applicable and widely applied standard scheme.


However, the flexibility and structural correlation of the scheme is limited when it comes to insertions of new amino acids, as they are appended to the CDRs.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


### **Chothia**


Chothia is a structure-based scheme, created (by Chothia and Lesk5) by aligning the variable region crystal structures forming CDRs instead of a sequence-based alignment. Differences between Kabat and Chothia can be found in amino acid insertion points, for example for CDR-L1 and CDR-H1, as well as the loop lengths in CDRs.


In short, Chothia numbering corresponds with the three-dimensional structures of hypervariable regions of typical length antibodies.


### **Martin**


Martin is a scheme that is essentially an updated version of Chothia, including proposed corrections to certain positions, which are based on analysis of a database of both sequences and structures.


With greater sequence variability in the reference dataset, also unconventional CDR lengths and deletions are taken into account in the numbering scheme.


## **CDR definitions from sequences**


Accurately predicting the loop-region and structure of the CDRs from amino acid sequences is an extremely useful tool.


Studies on the canonical classes have indeed indicated that CDR-L 1,2 and 3 as well as CDR-H1 and 2 have a more limited amount of conformations, with often preserved residues in certain positions5, 6 . However, regions outside the CDRs, e.g. the non-canonical “DE-loop”7, may also affect the ultimate structure of the loops.


Several computational tools have also been developed recently to predict loop structure from large amounts of sequences, including the ABlooper8, SCALOP9, AlphaFold210.


These deep learning-based tools have the potential to bring increasing insights for designing and optimizing antibody structures in antibody engineering and represent a new wave of computational research in the field.


Some open-source tools, such as ANARCI11, allow you to apply numbering to a given translated amino acid sequence with different numbering schemes.


PipeBio’s annotation tools allow annotation of DNA or protein sequences by aligning them to the closest reference germline gene sequence, adding definitions for FRs, CDRs the Fc-region, and applying a numbering scheme, such as IMGT, Kabat or AbM.


## **Conclusion**


While standard antibody numbering schemes exist, deciding which numbering scheme to use depends on the premise of the research objective.


-


Sequence alignment-based schemes (Kabat, IMGT) offer the benefit of large reference databases and


-


A large number of derivative tools that can be used with the schemes to accurately define a wide array of antibody regions.


-


IMGT is widely applicable scheme for standard numbering of antibodies and TCRs, regardless of the chains or species.


-


Numbering systems with CDR definitions that correspond to antibody CDR loop structure (Chothia and Martin) can be good alternatives for antibody engineering efforts where structure and the interacting residues are the focus.


In the end, the final objective and needs determine what type of a scheme is best for each use case, since using custom numbering and derivatives of the most widely used systems is also an option.


## References


1.


Dondelinger M, Filée P, Sauvage E, Quinting B, Muyldermans S, Galleni M, Vandevenne MS. Understanding the Significance and Implications of Antibody Numbering and Antigen-Binding Surface/Residue Definition. Front Immunol. 2018 Oct 16;9:2278. doi: 10.3389/fimmu.2018.02278. PMID: 30386328; PMCID: PMC6198058.


2.


Sela-Culang I, Kunik V, Ofran Y. The structural basis of antibody-antigen recognition. Front Immunol. 2013 Oct 8;4:302. doi: 10.3389/fimmu.2013.00302. PMID: 24115948; PMCID: PMC3792396.


3.


Kabat EA, Te Wu T, Bilofsky H, (U.S.) NI of H. Sequences of Immunoglobulin Chains: Tabulation and Analysis of Amino Acid Sequences of Precursors, V-regions, C-regions, J-Chain and BP-Microglobulins, 1979. Department of Health, Education, and Welfare, Public Health Service, National Institutes of Health (1979). Available online at: https://books.google.com/books?id= OpW8- ibqyvcC


4.


Figure adapted from IMGT (accessed 2022-10-28): https://www.imgt.org/IMGTScientificChart/Numbering/IMGTnumberingCDR_VH.html


5.


Chothia C, Lesk AM. Canonical structures for the hypervariable regions of immunoglobulins. J Mol Biol (1987) 4:901–17. doi:10.1016/0022-2836(87)90412-8


6.


North B, Lehmann A, Dunbrack RL Jr. A new clustering of antibody CDR loop conformations. J Mol Biol. 2011 Feb 18;406(2):228-56. doi: 10.1016/j.jmb.2010.10.030. Epub 2010 Oct 28. PMID: 21035459; PMCID: PMC3065967.


7.


Kelow SP, Adolf-Bryfogle J, Dunbrack RL. Hiding in plain sight: structure and sequence analysis reveals the importance of the antibody DE loop for antibody-antigen binding. MAbs. 2020 Jan-Dec;12(1):1840005. doi: 10.1080/19420862.2020.1840005. PMID: 33180672; PMCID: PMC7671036.


8.


Brennan Abanades, Guy Georges, Alexander Bujotzek, Charlotte M Deane, ABlooper: fast accurate antibody CDR loop structure prediction with accuracy estimation, Bioinformatics, Volume 38, Issue 7, 1 April 2022, Pages 1877–1880, https://doi.org/10.1093/bioinformatics/btac016


9.


Wing Ki Wong, Guy Georges, Francesca Ros, Sebastian Kelm, Alan P Lewis, Bruck Taddese, Jinwoo Leem, Charlotte M Deane, SCALOP: sequence-based antibody canonical loop structure annotation, Bioinformatics, Volume 35, Issue 10, 15 May 2019, Pages 1774–1776, https://doi.org/10.1093/bioinformatics/bty877


10.


Jumper, J., Evans, R., Pritzel, A. et al. Highly accurate protein structure prediction with AlphaFold. Nature 596, 583–589 (2021). https://doi.org/10.1038/s41586-021-03819-2


11.


James Dunbar, Charlotte M. Deane, ANARCI: antigen receptor numbering and receptor classification, Bioinformatics, Volume 32, Issue 2, 15 January 2016, Pages 298–300, https://doi.org/10.1093/bioinformatics/btv552
