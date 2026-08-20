---
schema_version: "1.0.0"
document_id: "dab58c582a966e976c76ae48c35a110faa33020646ba243db74689accddf545a"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/sanger-sequence-analysis-of-antibodies-and-tcrs"
published_at: "2022-10-07T07:00:00+00:00"
first_seen_at: "2026-08-03T09:07:34.565009+00:00"
fetched_at: "2026-08-05T03:48:46.961840+00:00"
content_hash: "sha256:3c9bb8ee736eff791909866f76166a7a1e71885250407ff6c2d04d461131e940"
---

# Sanger sequence analysis of antibodies and TCRs

Sanger sequencing has been routinely used to characterize immune cell populations and analyze antibody repertoires for more than twenty years. Despite the development of new NGS methods such as Illumina sequencing or[microfluidic systems](https://www.benchling.com/blog/single-cell-sequencing-technologies-in-antibody-discovery) , Sanger sequencing still remains relevant and in wide use for its high accuracy and longer DNA sequence reads.


PipeBio offers a wide range of built-in tools for analyzing and editing Sanger data. Once the DNA sequences are imported into the platform, the user can choose from a variety of options to examine the Sanger reads, including:


-


Quality control (QC), including trimming


-


Secondary peak detection


-


Assembling forward and reverse Sanger reads


-


Annotation


-


Editing and correcting nucleotide bases of sequences


-


Identifying duplicate clones


-


Multiple sequence alignment


-


Aligning Sanger sequences to parent clone


-


Automate all this with in-app workflows‍


Typical Sanger analysis workflow used for antibody and TCR sequences on PipeBio: From QC to phylogenetic trees


‍In order to automate repetitive tasks when analyzing large volumes of Sanger data, our customers are able to run fully configurable workflows that connect tools together for an optimized and repeatable analysis of the data. Here we showcase some of the most useful tools in our Sanger sequencing analysis workflow using VHH sequences from alpaca.


### Secondary peak detection


Heterozygote base calls or secondary peaks might appear in Sanger reads after sequencing a PCR product derived from diploid DNA with polymorphic positions or SNPs, resulting in a double fluorescence peak in the chromatogram.


Thus, the first step in the workflow performs **secondary peak calling** to correct mixed clones in the dataset using the desired parameters. This tool identifies double peaks in the sequences and annotates them accordingly, providing information about which nucleotides were detected and at which percentage in a given position.


You can move through each individual secondary peak using the navigation tools on the right-hand side panel. Moreover, it is possible to manually edit the sequence to correct wrong base calls by replacing the erroneous nucleotide with the desired one.


The navigation tools allow you to move through individual secondary peaks and edit the sequence if desired.


### Assemble forward and reverse strands of Sanger reads


If you are using forward and reverse primers, you'll want to assemble the F and R reads to obtain a longerconsensus sequence. You can also assemble multiple reads of a single clone to construct a consensus sequence from that.


This would be the case when individual Sanger reads have low sequencing quality and need to be assembled into a longer high-quality contig.


You are able to 1) inspect mismatches for individual alignments and 2) obtain a document with all consensus sequences when running in bulk‍


Another example where assembly is necessary is when sequencing scFvs, where the length of the sequence can’t be covered by a single read. ScFv sequencing is often done from both ends, and assembly of the forward and reverse reads is required to obtain the full length scFv sequence.


Depending on the type of imported data, assembly of Sanger reads can be incorporated as a first step of the workflow to achieve a longer consensus sequence.


### Annotation


Once we have ensured the quality of the sequences by using the secondary peak detection tool, the next step in the workflow is the **annotation** of the sequences. Here we use a VHH scaffold including warnings for secondary peaks and the alpaca germline database to annotate our Sanger reads.


In the output file we can inspect the annotation results, check which genes and regions are present in the sequences, identify mutations and filter the sequences according to warnings. Through the status chart we can get an overview of the annotation results as well as a detailed list of the errors found.‍


### How to detect duplicate clones from Sanger sequencing?


If you wish to identify duplicate clones, you can do this easily by sequence **clustering.** Clustering is also useful when handling large amounts of data in order to reduce the number of sequences to consider for further analyses, avoid data redundancy and group similar sequences into families sharing functional characteristics.


PipeBio offers a powerful tool for clustering Sanger sequences and it is also integrated in this automated workflow. In this example we cluster our reads on the VHH region using a 100% identify cutoff. The algorithm identifies different clusters in our data and also choses one single sequence to represent each cluster.


In this interactive graph, antibody sequence clustering shows identical clones grouping together. User-assigned labels are also displayed in the diagram.‍


### Multiple sequence alignment and Phylogenetic tree


After clustering, we have configured the workflow to **extract** the IgG-H region of the most representative sequence of each cluster and perform a Multiple Sequence Alignment (MSA) of the full variable domain sequences. If we sort the sequences by the cluster ID we can display the phylogenetic tree next to the aligned sequences. Sorting lets us observe how the different families of sequences group together in the tree. We can manually assign different labels to each sequence for better visualization of the data.


### Alignment-based diversity analysis


Another option in our Sanger sequencing analysis workflow is to perform an **alignment based on sequence diversity** that reduces the amount of sequences in our dataset to a smaller, yet diverse subset. The tool calculates a distance between the selected regions of the sequences based on the[ScoreDist](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-6-108) method and ranks them in terms of the amount of diversity they represent.


The alignment-based diversity analysis tool also outputs a phylogenetic tree based on the calculated distance between the sequences on the selected regions.‍


### Aligning Sanger reads to reference parent clone


If you have one or multiple reference sequences or a parent clones that you want to align sequences to, you are able to do this in bulk.


You can just use the clone name or a similar identifier to run the alignment and see the numbers and positions of mismatches between the clones.


## Run PipeBio’s Sanger sequencing workflow today


In this article we have summarized some of the most useful tools designed to analyze Sanger reads on our platform – and PipeBio has a lot more to offer. The PipeBio platform has been carefully designed to help scientists run powerful bioinformatic analysis in a simple, intuitive and interactive way. We are also constantly developing new tools adapted to our customer needs.


[Request a demo](https://www.benchling.com/demo-request) and try the Sanger sequencing analysis workflow or explore the rest of the platform.


‍‍
