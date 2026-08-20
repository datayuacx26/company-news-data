---
schema_version: "1.0.0"
document_id: "c948051a9c1be661f45a3dd2e2f779fa5c5c64452af2b8c78ee8742f37e5d95e"
company_key: "10x-genomics-inc-class-a-common-stock"
company: "10x Genomics Inc."
source_id: "10x-genomics-inc-class-a-common-stock-news-import-0e59b1db97d8"
canonical_url: "https://www.10xgenomics.com/blog/what-is-the-difference-between-bulk-and-single-cell-rna-sequencing"
published_at: null
first_seen_at: "2026-08-05T02:52:34.709441+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:937b330ffd33c49b67b0f3ca2b85dc3e7677c8df00bda2c9f2ba1e7d4394b221"
---

# What is the difference between bulk and single cell RNA sequencing?

How do you *understand* a biological sample? Among many other features to explore, gene expression can reveal the composition and quality of a sample by focusing on its cellular makeup.


Two approaches to detect gene expression levels from cells in a complex biological sample dominate the research world: bulk RNA sequencing (bulk RNA-seq) and single cell RNA sequencing (scRNA-seq). Before you can weigh their strengths, you need to know what each one actually does.


### What is bulk RNA-seq?


Bulk RNA-seq is a next-generation sequencing (NGS)-based method to measure the whole transcriptome across a population of cells. Bulk pools every cell together and hands you one profile for the whole sample as an average. That means you get average expression levels for individual genes across all cells that compose the sample.


#### Steps in a bulk experiment


You start a bulk experiment by digesting your sample to extract its RNA. That could be total RNA from the sample, or you can enrich mRNA or deplete ribosomal RNA. Then you convert the RNA into cDNA and follow more processing steps to prepare a sequencing-ready gene expression library. After sequencing and data analysis, you can review gene expression levels across the tissue sample.


*This video provides an overview of several common methods to measure gene expression, and gets you ready to go deeper into the differences between bulk RNA-seq and single cell RNA-seq:*


### What is single cell RNA-seq? (Seeing every tree vs. the forest)


scRNA-seq is a method for studying the whole transcriptome gene expression profile of each *individual cell* from a sample. While a bulk readout is like the view of a forest, a single cell readout is like seeing every tree. As such, there are a few key experimental differences.


#### Sample preparation for single cell RNA-seq


scRNA-seq measurements come from individual cells, as opposed to a pool of cells. First, you need to generate viable single cell suspensions from whole samples that you digest through an enzymatic or mechanical process, cell sorting, or other cell isolation technique. Then you perform cell counting and quality control steps to ensure your samples have an appropriate concentration of viable cells and are free of clumps and cell debris.


You can also stain your sample with antibodies to label proteins and other biological analytes, or perform FACS enrichment for cell types of interest.


These steps for dissociating your sample will vary based on your starting material and your experimental goals. Additional preparation steps may be necessary depending on the quality of the tissue, sample abundance, cell size, or if you need to extract nuclei for chromatin accessibility profiling. Regardless of these specific considerations, the same basic principle of generating a high-quality single cell suspension applies across sample types.


#### Instrument-enabled cell partitioning


Single cell RNA-seq, like bulk RNA-seq, seeks to isolate RNA and convert it to cDNA, enabling measurement of gene expression across the whole transcriptome through next-generation sequencing. Because every readout traces back to its cell of origin, you can use the same workflow to capture more than RNA—protein, chromatin, and even immune receptor sequences, all from the same cell.


In the 10x Genomics scRNA-seq workflow, you isolate single cells into individual micro-reaction vessels (Gel Beads-in-emulsion, or GEMs) before isolating RNA. This partitioning step happens in an automated, controlled environment within a microfluidic chip loaded onto a Chromium X series instrument.


Then, within the GEM, Gel Beads get dissolved, releasing oligos containing unique barcodes. The cell is also lysed in order to capture the RNA molecules, which are barcoded with cell-specific barcodes. This ensures analytes from each cell can be traced back to their cell of origin. Those barcoded products are then used to create a sequencing library, allowing measurement of gene expression across the entire transcriptome of each individual cell.


Figure 1. An illustration of cells or nuclei moving through a microfluidic channel and generating GEMs within a Chromium X series instrument. These are the essential steps of a single cell workflow, and the end result is a sequencing-ready gene expression library.


#### A beginner’s guide to single cell RNA-seq


Want more helpful resources to learn about single cell RNA-seq? Explore what it can do, advantages over other methods, available products, and common barriers to using the technology in our learning hub.


[Start here](https://www.10xgenomics.com/what-is-single-cell-rna-seq)
