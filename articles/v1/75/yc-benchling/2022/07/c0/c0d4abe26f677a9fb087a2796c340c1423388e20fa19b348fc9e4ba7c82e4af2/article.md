---
schema_version: "1.0.0"
document_id: "c0d4abe26f677a9fb087a2796c340c1423388e20fa19b348fc9e4ba7c82e4af2"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/single-cell-sequencing-technologies-in-antibody-discovery"
published_at: "2022-07-12T07:00:00+00:00"
first_seen_at: "2026-08-03T22:55:04.861414+00:00"
fetched_at: "2026-08-05T03:48:56.843789+00:00"
content_hash: "sha256:25f08615ac382c98894a8b022e086cb50b8572f4a1b54c431b2b4a0d1b0e8b8f"
---

# Single-cell sequencing technologies in antibody research

## Introduction


With the current advancements in antibody drug discovery, there is an increasing demand for sequencing technologies to provide efficient tools able to characterize single immunological cells.


Single-cell methods offer a deep insight into the complex biology of the immune system and enable the precise study of cell secretion, migration, cell-to-cell interaction and mRNA sequencing. Ultimately, these approaches accelerate the development of antibody-based therapeutics by directly isolating and sequencing antibody-secreting cells (ASCs) to select antigen-specific antibodies.


One of the first technologies used to study gene expression in single cells was qPCR. qPCR can measure multiple genes simultaneously from hundreds of cells, but the process is tedious and lacks whole-transcriptome analysis. For RNA sequencing of the whole transcriptome, Fluorescence-activated Cell Sorting (FACS) has been used to sort distinct immune populations and to isolate single ASC based on cell surface markers or staining. The sorted cells are subsequently sequenced using Sanger sequencing or NGS.


In the past years, several transcriptomic technologies based on microfluidic assays have emerged, enabling accurate and high-throughput sequencing of single cells. These methods use special miniaturized platforms which enable the compartmentalization of single cells in either wells or droplets.


The isolated RNA is labeled with cell-specific barcodes, so the different sequences can be traced back to the original cell after being sequenced. Microfluidic-based assays are able to dissect major immune populations in unprecedented speed, retrieve antibody chain pairs accurately and recover the processed sequences for downstream analysis.


Here, we highlight two of the main single-cell sequencing technologies based on microfluidics available nowadays: the 10x Genomics Chromium system and the Beacon system. Furthermore, we discuss some of the challenges that arise when analyzing single-cell data, and we feature how the[PipeBio Bioinformatics platform](http://www.pipebio.com/) supports and processes data exported from single-cell sequencing platforms.


## 10x Genomics Chromium


The 10x Genomics Chromium system makes use of droplet microfluidics to sequence mRNA of single cells. A gel bead in emulsion (GEM) with unique 10x barcodes, sequencing adapters and primers is used to encapsulate each cell.


The cell inside a GEM undergoes lysis and reverse transcription to generate 10x barcoded cDNA, which is then transferred to a tube for amplification, library construction and parallel sequencing (Fig. 1). All generated cDNA from a single cell share common 10x barcodes.


**‍Figure 1.** 10x Genomics Chromium workflows, from sample to barcoded cDNA. Image from www.10xgenomics.com


This technology allows transcriptome sequencing of thousands of individual cells, making it possible to study immune cell populations and identify cellular subtypes in the sample. Immune profiling of full-length V(D)J sequences for paired B-cell or T-cell receptors is also possible, as shown in Goldstein et al. (2019).


In this study, single B-cells from rat, mouse and human were sequenced using the 10x Genomics Chromium system to obtain paired full-length antibody variable regions, demonstrating that the method is incredibly effective for rapid antibody discovery \[1\].‍


## Beacon (Berkeley Lights)


The Beacon system developed by Berkeley Lights combines optics and nanofluidics for processing and deep profiling of single cells. The cells are deposited into nanopen chambers where they proliferate and can be tested using multiple assays. The system is capable of capturing bright field and fluorescent images of each nanopen to track single cells over time.


For further analysis, single cells are moved using light patterns into well plates and exported.


The Beacon technology offers automated workflows designed to facilitate and speed-up antibody discovery. For instance, thousands of single B cells can be cloned into the nanopen chambers and characterized using the desired assays, e.g., antigen-specific bead assay and multiplex IgG capture assay (Fig. 2).


After identifying promising candidates, the antibody sequences can be recovered and sequenced. The genetic barcodes ensure that each antibody sequence can be linked to its corresponding bead and functional profile.


**Figure 2:** Using the Beacon system for B-cell screening and down-selecting candidates for antibody discovery. Image from[www.berkeleylights.com](http://www.berkeleylights.com/) **


‍In light of the COVID-19 pandemic, the Beacon system was used to rapidly select human monoclonal antibodies that bind and block the interaction of SARS-CoV-2 Spike protein with the human ACE-2 receptor \[2\]. Individual B cells recovered from patients were functionally analyzed and the most promising candidates were selected.


Two of them were selected to create AZD7442, an antibody cocktail which quickly entered Phase III clinical trials and has been recently authorized in the US for pre-exposure prophylaxis of COVID-19 \[3\].


## Challenges when analyzing single-cell data


Single-cell sequencing and modern microfluidic systems have undoubtedly revolutionized immunology research. The high-throughput nature of such technologies creates large amounts of data from a single experiment, which needs to be analyzed carefully. Researchers face several challenges when analyzing single-cell data, and in PipeBio we wanted to highlight some of the most prominent ones.


### High dimensionality of the data


By increasing the amount of cells and features measured, single-cell data becomes more noisy, sparse and high-dimensional, which might complicate the implementation of modeling algorithms.


Hence, dimensionality reduction is an important step when analyzing such amount of data, and methods such as Principal Component Analysis (PCA), Uniform Manifold Approximation and Projection (UMAP) and t-distributed Stochastic Neighbor Embedding (t-SNE) are commonly used \[4\].


### Correct handling of multiplexed data


**‍** Cell multiplexing is the process of mixing different barcoded single-cells together in a GEM well. The molecular tag or barcode ensures that after sequencing, each sequence can be assigned to the original cell. In order to secure a precise retrieval, several bioinformatics tools have been designed, such as Cell Ranger from 10x Genomics.


The software processes single-cell data to align reads, demultiplex and cluster the data, among others. The[PipeBio platform](http://www.pipebio.com/) has specific tools designed to analyze 10x Genomics data, allowing the user to associate specific 10x data to each sequence and run QC analysis (Fig. 3).


Additionally, PipeBio provides pipelines for pairing the heavy and light chain together, clustering VH/VL pairs to define clonotypes, and aligning sequences by region to visualize clone diversity (Fig. 4).


**Figure 3:** Using the Beacon system for B-cell screening and downselecting candidates for antibody discovery. Image from www.berkeleylights.com


### **Antibody sequence diversity**


**‍** The B cell receptor (BCR) repertoire comprises the full set of BCRs expressed by a given individual (108-1010 unique BCRs in human adult). The antibody repertoire of a given individual is defined as a subset of the BCR repertoire containing all the secreted antibodies with a unique sequence.


CDR3 is the most variable region of an antibody and it is greatly responsible for the high diversity of the antibody repertoire. Hence, the proper identification of an antibody is dependent on the correct sequencing of CDR3, which can differ by only a few nucleotides.


For this reason, it is crucial that single-cell technologies provide accurate analysis with minimal sequencing errors, and computational approaches must be developed to identify biological mutations from sequencing errors.


**Figure 4:** VH alignment obtained using PipeBio to examine clone diversity.


## Summary


Single-cell sequencing is an extraordinarily potent technology for expanding our knowledge of the heterogeneity of the immunological system, facilitating the study of individual immune cells and identifying promising candidates for the rapid development of therapeutic antibodies. Microfluidic approaches have been developed to optimize single-cell sequencing with high sensitivity and high throughput, creating large data sets that require accurate analysis. The[PipeBio Cloud Platform](http://www.pipebio.com/) offers specific tools designed to analyze single-cell data, making it possible for scientists to import the data directly to the platform and start the analysis with just a few clicks.


## References


1.


Goldstein LD, Chen YJ, Wu J, et al. Massively parallel single-cell B-cell receptor sequencing enables rapid discovery of diverse antigen-reactive antibodies. *Commun Biol* . 2019;2:304. Published 2019 Aug 9. doi:10.1038/s42003-019-0551-y


2.


Zost SJ, Gilchuk P, Chen RE, et al. Rapid isolation and profiling of a diverse panel of human monoclonal antibodies targeting the SARS-CoV-2 spike protein. *Nat Med* . 2020;26(9):1422-1427. doi:10.1038/s41591-020-0998-x


3.


https://www.astrazeneca.com/media-centre/press-releases/2021/evusheld-long-acting-antibody-combination-authorised-for-emergency-use-in-the-us-for-pre-exposure-prophylaxis-prevention-of-covid-19.html


4.


Xiang R, Wang W, Yang L, Wang S, Xu C, Chen X. A Comparison for Dimensionality Reduction Methods of Single-Cell RNA-seq Data. *Front Genet* . 2021;12:646936. Published 2021 Mar 23. doi:10.3389/fgene.2021.646936
