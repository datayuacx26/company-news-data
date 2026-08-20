---
schema_version: "1.0.0"
document_id: "72086c2df1542ac778ae2f345228bc285da1f14f99a2b2fba15b4432c09400a0"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/antibody-liabilities"
published_at: "2022-10-14T07:00:00+00:00"
first_seen_at: "2026-07-29T00:41:24.192337+00:00"
fetched_at: "2026-07-29T00:41:27.899557+00:00"
content_hash: "sha256:17acda318869bab0411954378f0dd18e617fab2a78d7f95502a5bde712abf8cb"
---

# Accurate detection of liabilities to de-risk antibody development

The accurate annotation, definition and characterization of antibody sequences is a crucial step in antibody drug discovery with direct consequences in the final stages of the development process. The slightest oversight in the annotation of the sequences, detection of liabilities and identification of mutations will undoubtedly lead to a reduction in the efficacy of any promising lead candidates. Thus, efficient and exact annotation tools are needed to ensure the developability\[1\] of antibodies and decrease the risk of producing non-effective therapeutics.


PipeBio’s annotation pipeline is an extremely powerful and configurable tool to precisely analyze various kinds of antibody sequences and identify liabilities that impact the antibody’s affinity and specificity to its target. The customizable scaffold system allows annotation of traditional and custom sequence regions with a wide variety of germlines, annotation definitions and liabilities to choose from.


## Identify CDRs, framework regions and germline genes accurately


PipeBio’s annotation engine is able to efficiently detect and label CDRs and framework regions in antibody sequences from both the heavy and light (kappa and lambda) chains. The variable (V), joining (J), diversity (D) and constant (C) gene segments are also identified thanks to our manually-curated germline databases (Fig. 1). Furthermore, custom databases can be created to identify and annotate additional regions in the sequence, such as linkers, tags, loops or any others. We provide germline databases for human, mouse, alpaca, rat, rabbit and chicken, and can easily generate databases for other species not mentioned.‍


**Figure 1.** PipeBio’s sequence viewer displaying the CDRs, framework regions and germline genes identified by the annotation pipeline.‍


## Detect liabilities and flag incorrect sequences


One of the most powerful and advantageous characteristics of our annotation tool is its ability to detect a wide range of liabilities in the sequences that can affect the binding of the antibody with the relevant antigen. These include post-translational modifications\[2\] (PTMs), quality validations and structural verifications that can be applied to specific regions of the sequence as needed. These validations are specified in the scaffold system with a designated liability score (Fig. 2). Additionally, custom validations and liabilities can be easily added to the whole sequence or to desired regions.


**Figure 2.** a) The validations for CDR-H1 are defined in the scaffold system which will be used by the annotation engine to detect liabilities (errors and warnings) in the sequences analyzed. b) A list of all the errors found in the annotated sequences is displayed in the output document


‍Here we will list and briefly describe some of the different validations and liabilities that can be incorporated in your analysis using PipeBio to optimize the developability of antibodies.


## Sequence quality validations


Somatic hypermutations (or SHMs) increase antibody diversity and are essential for the adaptation of the immune system against new foreign elements. It might become difficult to differentiate between those mutations and sequencing errors caused by low quality base calls of nucleotides. We compute the Phred quality score (Q) system as an indicator of sequence quality (Fig. 3), which is logarithmically related to the probability of a wrong base call during DNA sequencing. For instance, a Q30 is equivalent to the probability of 1 incorrect base call in 1000 (99.9% accuracy), while a Q20 indicates 1 wrong base call in 100 (99% accuracy). To ensure high sequence quality, we provide the following validations:


-


Ambiguous bases: nucleotides different from A, C, G, T and U are flagged.


-


Nucleotide quality: marks individual bases with a Phred score lower than a specified threshold.


-


Average quality region: if a certain % of a region in the sequence has a Phred score lower than the specified threshold, the region will be flagged.


-


Secondary peaks: highlights heterozygote base calls in Sanger reads.


**Figure 3.** The Phred quality score is calculated for each region of the sequence. Typically, sequences with a high amount of detected liabilities (warnings) will have lower Phred scores, as seen in the table


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Sequence structure validations


We examine sequence modifications in multiple regions to guarantee the integrity of an antibody (Fig. 4).


-


Missing regions: A sequence will be flagged if any of the CDRs, frameworks or other custom regions defined in the scaffold are missing


-


Truncation: The annotation engine is able to detect truncated framework regions in the beginning and in the end of the sequence. The frameworks can be reconstructed using the germline gene as reference


-


Frameshifts: Detects sequences with regions containing frameshifts due to mutations


-


Region length: Useful when regions are shorter or longer than a specified length


**Figure 4.** Example sequence flagged as INCORRECT due to a TAA stop codon, out of frame and missing regions


## Post-translational modifications (PTMs)


PTMs refer to the modifications that affect the amino acid chain after the biosynthesis of an antibody (Fig. 5). This process provides additional sequence variation and alters the antibody binding properties, which might be troublesome when developing antibodies designed against a specific target. PTMs include the addition of functional groups in the amino acid chain, the modification of existing amino acids in the sequence or the cleavage of bonds, among others \[3, 4\]. PTM liabilities can be either added to specific regions of the antibody cover the whole sequence. PipeBio can detect the following PTMs based on the amino acid sequences:


-


Asparagine deamidation


-


Aspartate isomerization


-


N-linked glycosylation


-


Lysine glycation


-


Methionine oxidation


-


Tryptophan oxidation


-


Aspartic acid – Proline cleavage


-


Hydrolysis


**Figure 5.** Post-translational modifications can occur in both the constant and variable regions of IgGs and can be risk factors that decrease the stability of monoclonal therapeutic antibodies


## Additional sequence validations


The scaffold system offers the flexibility to add custom validations and detect PTMs in order to support a wide range of structural needs, additional motifs and quality checks. Other liabilities that we can include during the annotation of sequences are, among many others:


-


Stop codons: Depending on the codon table used, the annotation tool will detect stop codons in the sequences.


-


Cysteine count: Different options are available for flagging sequences depending on their C count. Free sulfhydryls (odd cysteines or unpaired cysteines) may result in lower stability, structural changes and a higher risk of aggregation in monoclonal antibodies. We provide support for both antibody and non-antibody configurations with scaffolds that do not flag sequences with a large number of cysteines.


-


Hydrophobic patches: which may aggregate during production.


-


Amino acid repeats: sequences will be marked whenever an amino acid repeat is present that does not correspond to a specific pattern (e.g., “GSS” in a linker region).
