---
schema_version: "1.0.0"
document_id: "61ea3845cb124a9ffe7a7b95344431c18624c1f24b24450de5e198dc30cb63ae"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/native-heavy-light-chain-pairing-of-antibodies-with-ngs"
published_at: "2023-12-08T07:00:00+00:00"
first_seen_at: "2026-08-01T01:24:50.959811+00:00"
fetched_at: "2026-08-01T01:24:55.222184+00:00"
content_hash: "sha256:1b2841cbff68acc1226f841036fe6ed2e7863bf5369fbec2437c1e24d9a9c32c"
---

# Native heavy-light chain pairing of antibodies with NGS

## The importance of native VH/VL pairing in antibody repertoire sequencing


Antibodies play a critical role in the adaptive immune system's response to pathogens. They bind to specific antigen epitopes with their variable regions, composed of a heavy chain (VH) and light chain (VL) partner. Native pairing of heavy (VH) and light (VL) chains is critical for maintaining the specificity and functionality of antibodies. However, analyzing the highly diverse antibody repertoire encoded by human B cells with preserved VH/VL pairs has been a major challenge.


Next-generation DNA sequencing (NGS) of antibody repertoires has become a powerful approach for discovering novel antibodies, analyzing B cell development, and investigating immune responses. But most NGS only sequences VH and VL chains separately, whereas paired VH/VL data is required for complete antibody characterization[1](https://www.nature.com/articles/nm.3743) . This article discusses techniques to recover natively paired antibody sequences using NGS approaches.


## Emulsion encapsulation RT-PCR


Molecular and microfluidic strategies have unlocked the ability to re-link heavy and light chain sequences at the point of sample preparation and sequencing. A good example is the emulsion-based approach developed by Rajan et al.[2](https://www.nature.com/articles/s42003-017-0006-2) Their method compartmentalizes single B cells into tiny picoliter water-in-oil droplets and amplified by reverse transcription (RT) PCR. Early PCR cycles concatenate cognate VH and VL amplicons into a single scFv construct before the limiting reagents are consumed. In this way, native pairing is maintained for >96% of sequences.


**Figure 1.** Microfluidic encapsulation and single-droplet reactions to obtain native pairing of antibody VH and VL sequences


The scFv amplicon library can directly be incorporated into a phage display vector for functional screening, in addition to NGS. Using custom primers, this method allows paired-end sequencing to yield natively paired VH and VL fragments in the millions by using Illumina sequencers, such as the Illumina MiSeq, which was used in the experiment.[2](https://www.nature.com/articles/s42003-017-0006-2)


**Figure 2.** Sequencing of scFvs by Illumina paired-end NGS sequencing. Protocol from Rajan et al. for isolation and sequencing of natively paired antibody heavy and light chain sequences.


Using this workflow, Rajan et al.[2](https://www.nature.com/articles/s42003-017-0006-2) isolated rare broadly neutralizing antibodies against influenza hemagglutinin from healthy donors, demonstrating the potential of the technology.


## Cellular barcoding and molecular tagging


An alternative to emulsion encapsulation is labeling cDNA with unique barcodes during RT to tag sequences originating from single cells. The 10x Genomics Chromium platform is an example, compartmentalizing cells into gel beads that co-encapsulate viral RT primers bearing 16-bp random barcodes.[5](https://doi.org/10.3389/fcimb.2022.962945)


After barcoded cDNA amplification and emulsion dissolution, NGS reads sharing the same barcode can be assigned to their parent cell. Although stochastic shuffling of mRNA species between beads does occur, bioinformatics tools determine consensus VH and VL pairs from barcoded reads.


Specialized molecular tagging employs template switching to incorporate UMIs and cell barcodes during RT. McDaniel et al.[3](https://www.nature.com/articles/nprot.2016.024) demonstrated this approach by co-encapsulating splenocytes with uniquely barcoded beads bearing an oligo(dT) primer. Following RT, emulsion dissolution, and PCR, reads can be traced back to single cells via the bead barcode. By aggregating UMI counts, somatic variants of antibody lineages are also quantified with high confidence.


## ‍Long-read sequencing


Long-read sequencing holds particular promise for resolving native VH and VL pairs, with reads frequently spanning entire variable domain transcripts.[4](https://www.nature.com/articles/nbt.2782) For example, Oxford Nanopore sequencing was recently utilized to obtain full-length antibody sequences from rat hybridoma cell lines.[7](https://www.tandfonline.com/doi/full/10.1080/19420862.2022.2106621) Although throughput is currently lower than short-read platforms, ongoing advances in long-read sequencing and analysis will undoubtedly enhance these capabilities.


## Antibody clonotype assembly: Library preparation and computational pipeline


Realizing the potential of high-throughput, natively paired antibody sequencing requires specialized library construction protocols in concert with tailored computational analysis workflows. Fahad et.[6](https://link.springer.com/protocol/10.1007/978-1-0716-2609-2_25) al developed a protocol for processing millions of Illumina MiSeq 2x300 bp reads into high-confidence V(D)J clonotypes relating to complete VH/VL consensus sequences. Reads are first annotated and filtered before clustering clonally related lineages defined by junctional diversity. Finally, the highest quality full consensus sequence is called to represent each paired antibody lineage cluster.


**Figure 3.** Using bioinformatics analysis to for constructing consensus sequences of natively paired heavy light chain sequences of antibodies.


## The future of high-throughput native antibody sequencing


In summary, molecular barcoding techniques like emulsion RT-PCR, cellular indexing, and long-read sequencing have empowered the ability to resolve linkage of natively paired antibody chains. Coupling these approaches with streamlined computational pipelines provides a framework for functional interrogation of vast antibody repertoires. These technologies promise to transform antibody discovery and immune repertoire analysis to tackle challenges beyond the constraints of standard NGS.


## References


1.


[DeKosky et al. Nat Med 21, 86–91 (2015)](https://www.nature.com/articles/nm.3743)


2.


[Rajan et al. Communications Biology 1 (2018)](https://www.nature.com/articles/s42003-017-0006-2)


3.


[McDaniel et al. Nat Protoc 11, 429–442 (2016)](https://www.nature.com/articles/nprot.2016.024)


4.


[Georgiou et al. Nat Biotech 32, 158–168 (2014)](https://www.nature.com/articles/nbt.2782)


5.


[Hurtado et al. Front. Cell. Infect. Microbiol. (2023)](https://www.frontiersin.org/articles/10.3389/fcimb.2022.962945/full)


6.


[Fahad et al. Computer-Aided Antibody Design (2015)](https://link.springer.com/protocol/10.1007/978-1-0716-2609-2_25)


7.


[Satish et al. mAbs (2022)](https://doi.org/10.1080/19420862.2022.2106621)
