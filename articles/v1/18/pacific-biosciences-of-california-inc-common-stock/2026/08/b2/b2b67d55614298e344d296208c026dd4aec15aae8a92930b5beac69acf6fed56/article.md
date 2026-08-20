---
schema_version: "1.0.0"
document_id: "b2b67d55614298e344d296208c026dd4aec15aae8a92930b5beac69acf6fed56"
company_key: "pacific-biosciences-of-california-inc-common-stock"
company: "Pacific Biosciences of California Inc."
source_id: "pacific-biosciences-of-california-inc-common-stock-rss-232046c7699f"
canonical_url: "https://www.pacb.com/blog/measuring-not-guessing-the-true-path/"
published_at: "2026-08-10T13:05:57+00:00"
first_seen_at: "2026-08-10T14:54:49.507555+00:00"
fetched_at: "2026-08-10T14:54:50.539629+00:00"
content_hash: "sha256:39deec87221670bcacd83841445359003ddb1c8b60acc736dac2640f14362cd7"
---

# Measuring – not guessing – the true path

Jonas Korlach#


Over the past ~15 years, several attempts were made to adapt short-read sequencing to capture longer-distance information, including 10X Genomics linked reads, Illumina TruSeq synthetic long reads, and Illumina Infinity Reads (later branded Illumina Complete Long Reads). Comparisons with contemporaneous PacBio data revealed significant errors and shortcomings in every case[1](https://www.pacb.com/blog/the-hifi-difference-true-long-reads-vs-synthetic-long-reads/) ,[2](https://www.pacb.com/blog/the-hifi-difference-getting-the-right-answer/) ,[3](https://www.pacb.com/blog/hifi-difference-not-being-clr/) , and eventually all these methods were discontinued.


Recently, Illumina introduced another iteration of this concept with a whole genome sequencing (WGS) approach called the TruPath Genome, and made data available for a well-characterized, HG002 Genome-in-a-Bottle truth benchmark human sample, along with DRAGEN pangenome-based mapping and variant calls[4](https://basespace.illumina.com/s/KhNiNfb4KYE0) . So let’s again take a look at this new data type and how it compares to PacBio HiFi WGS for the same sample (also publicly available[5](https://downloads.pacbcloud.com/public/2026Q2/HG002-SPRQ-Nx/) ). Four initial observations make it apparent that similar errors compared to previous approaches are present in TruPath data, reflecting the inherent limitations of deriving long-range information from short sequence reads:


1. Some variants, previously missed by standard Illumina sequencing, are still missing in TruPath data.
2. Some variants are called by TruPath without any read support.
3. Some variants called by TruPath have conflicting read support.
4. Methylation information is absent in TruPath data.


A representative example for each category is described below.


---


##


## 1. Some variants, previously missed by standard Illumina sequencing, are still missing in TruPath data.


An example for variant calls still missed by the TruPath genome is shown below for the *NPIPB3* gene, member of a highly polymorphic, primate-specific gene family involved in immune responses. The HiFi reads support the correct calling and phasing of three deletions: 87 bp and 252 bp on allele 1, and 126 bp on allele 2. In contrast, none of the TruPath reads (as previously with standard Illumina sequencing; not shown) detected these deletions.


##


## 2. Some variants are called by TruPath without any read support.


Conversely to missed calls, instances occur for which variants are seemingly called in the TruPath genome despite not showing any supporting reads. As one example below, the size of a (CGG)n repeat at the *FMR1* gene plays an important role in a person’s risk of Fragile X-associated diseases. The HiFi reads correctly measure eleven additional repeat units (33 bp insertion) relative to the GRCh38 reference (this is in the healthy range; HG002 is male so there is only one X chromosome allele).


In contrast, in the TruPath data there is not a single read containing this insertion. Despite this lack of sequence support, the same 33 bp insertion variant call is inferred by the DRAGEN pangenome-assisted pipeline.


##


## 3. Some variants called by TruPath have conflicting read support.


In some cases, it appears that the TruPath sequence reads are at odds with the pangenome-assisted inference, leading to erroneous variant calls. The figure below shows an intronic region in *NCS1* , a gene which has been linked to neurodevelopmental disorders. HiFi reads allow for straightforward variant calling and phasing: a heterozygous insertion of 368 bp and 129 bp, respectively, followed by a one-base heterozygous A deletion that is in phase with the 368 bp insertion allele – again consistent with the benchmark truth set.


In contrast, the TruPath genome apparently calls this region as simultaneously containing a heterozygous 8 bp deletion and a homozygous 129 bp insertion, which are both incorrect. In addition, the heterozygous 368 bp insertion and one-base A deletion are missed.


Further, the TruPath variant calls that were made are surprising in view of the underlying conflicting read support. First, out of 48 mapped TruPath reads that span this region, 45 reads contain a 7 or 8-bp deletion error; however despite nearly all reads (94%) containing this deletion, the 8 bp deletion is called as *heterozygous* . Second, there are only three reads that contain the 129 bp insertion, and they are low quality (two with mapping quality zero and one with mapping quality 3; only one read is phased). Despite such low-frequency and low-quality support, the 129 bp insertion is called, but wrongly designated as *homozygous* in the variant file.


## 4. Methylation information is absent in TruPath data.


Methylated cytosine is a critical determinant of gene regulation, aging, and associated with many diseases. Genome-wide positions of 5-methylcytosine (5mC) are inherently output with every HiFi WGS run, and can be visualized in IGV (right-click: Color alignments by > base modification (5mC)). The figure below shows the imprinted *PEG10* gene (important in mammalian placental development, and as an oncogenic driver in several cancers), with red denoting 5mC and blue for unmethylated cytosine in CpG contexts in the HiFi reads, resolving the different methylation status between the two alleles across the beginning of the gene, and phasing the differential methylation with nearby heterozygous SNVs, indels, and a 74 bp intronic deletion:


Information about 5mC is not available in the TruPath genome, hence no coloring appears upon activation of the methylation IGV visualization, and thus missing the allelic imprinting, extent and phasing of differential methylation (TruPath also fails to detect the 74 bp deletion).


From the above, it is apparent that similar errors compared to previous approaches are present in TruPath data, reflecting the inherent limitations of deriving long-range information from short sequence reads. Pangenome references are an important advance (incidentally they are built with HiFi data[6](https://www.nature.com/articles/s41586-023-05896-x) ,[7](https://www.biorxiv.org/content/10.64898/2026.07.21.739710v1) ,[8](https://www.nature.com/articles/s41586-023-06173-7) ,[9](https://www.nature.com/articles/s41467-025-61645-w) ,[10](https://www.biorxiv.org/content/10.1101/2024.12.16.628631v2) ,[11](https://www.biorxiv.org/content/10.1101/2024.08.07.606392v2) ,[12](https://humanpangenome.org/hprc-data-release-2/) ,[13](https://www.nature.com/articles/s41586-025-08998-w) ,[14](https://www.nature.com/articles/s41586-026-10315-y) ,[15](https://www.nature.com/articles/s41586-025-09140-6) ,[16](https://www.medrxiv.org/content/10.64898/2026.07.06.26357367v1) ,[17](https://www.biorxiv.org/content/10.1101/2024.12.16.628631v1.full) ), but it appears that they are used very differently between the TruPath genome and PacBio HiFi WGS, e.g.:


- HiFi WGS: “HiFi reads *measured* a variant against hg38 that I don’t know about. The pangenome says that this is a common variant in several populations, so it’s less likely to be pathogenic.”


- TruPath genome: “TruPath sees conflicting or no read evidence for a variant against hg38 that however in the pangenome is common, so the variant is *inferred* as present in my sample as well.”


Indirectly inferring variants from the pangenome carries risks of bias and errors for resolving the underlying causes of rare and inherited diseases (inherently differing from the pangenome), *de novo* variation in an individual, somatic variants and rearrangements in cancer, infrequent population-specific variants, and more. While the genome-wide benchmarking stats of a TruPath genome may look improved, it does not necessarily translate into confidence for rare and personal variants with no or conflicting read support in real-world samples. In contrast, HiFi WGS directly measures the genetic and epigenetic variation in a diploid human genome, as a foundation for pangenome-leveraged interpretation and understanding.


---


To learn more about HiFi sequencing and its applications across genomics, explore the HiFi starter kit, a collection of technical resources, educational content, and expert insights.


[Explore the HiFi starter kit](https://programs.pacb.com/l/1652/2026-05-18/45j7wb)


* *Jonas Korlach is an inventor of single molecule, real-time sequencing (on which PacBio sequencing is based) while he was a graduate student at Cornell University. He joined PacBio as the company’s eighth employee in 2004, was its Chief Scientific Officer from 2012–2023, and since then has served as a scientific advisor. He is a co-author on over 100 peer-reviewed publications (including *Nature* , *Science* , and *PNAS* ) in various fields of genomics. He received both his PhD and MS degrees in Biochemistry, Molecular and Cell Biology from Cornell University, and MS and BA degrees in Biological Sciences from Humboldt University in Berlin, Germany. In 2013, Dr. Korlach was honored by the Obama White House as an Immigrant Innovator “Champion of Change.”*


*No AI was used in conjunction with this article. Research use only. Not for use in diagnostic procedures. © 2026 Pacific Biosciences of California, Inc. (“PacBio”). All rights reserved. The author and PacBio assume no responsibilities for any errors or omissions in this document. Pacific Biosciences, the PacBio logo, and PacBio are trademarks of PacBio. All other trademarks are the sole property of their respective owners. All comparisons are deemed accurate as of 08/03/2026 based on Illumina data accessed on 05/01/2026.*
