---
schema_version: "1.0.0"
document_id: "9b0f1c65e9aa130962328d614f60ca911a6de0bad15bbba90ff607861a302d7f"
company_key: "pacific-biosciences-of-california-inc-common-stock"
company: "Pacific Biosciences of California Inc."
source_id: "pacific-biosciences-of-california-inc-common-stock-rss-232046c7699f"
canonical_url: "https://www.pacb.com/blog/fda-draft-guidance-highlights-the-growing-importance-of-long-read-sequencing-for-genome-editing-safety-assessment/"
published_at: "2026-07-09T17:31:12+00:00"
first_seen_at: "2026-07-20T23:19:12.685645+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:d8240097ede77e8b1905216669a830796a7b837e22d4ec247639ac865ae84789"
---

# FDA draft guidance highlights the growing importance of long-read sequencing for genome editing safety assessment

Genome editing technologies are transforming the development of next-generation therapeutics. From CRISPR-Cas systems to base and prime editors, these approaches offer unprecedented opportunities to correct disease-causing mutations and engineer novel cellular therapies. As these technologies move closer to widespread clinical adoption, regulators are increasingly focused on ensuring that genome editing outcomes are thoroughly characterized and understood.


The FDA’s recent draft guidance,[Safety Assessment of Genome Editing in Human Gene Therapy Products Using Next-Generation Sequencing](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/safety-assessment-genome-editing-human-gene-therapy-products-using-next-generation-sequencing) , reflects this shift. This guidance emphasizes the use of sequencing-based methods to evaluate both intended and unintended editing outcomes and highlights the importance of assessing genome integrity throughout development.


Perhaps most importantly, the guidance acknowledges that different classes of editing outcomes require different sequencing approaches. While short-read sequencing may be sufficient for evaluating small edits less than ~50 bp, larger and more complex genomic alterations, including large insertions, deletions, rearrangements, translocations, and chromosomal abnormalities, may require[long-read sequencing technologies](https://www.pacb.com/technology/long-read-sequencing/) for comprehensive characterization.


**This distinction is significant because many of the genomic events most relevant to safety are also among the most difficult to detect and characterize using conventional sequencing methods.**


---


## Looking beyond editing efficiency


Historically,[genome editing](https://www.pacb.com/gene-editing/) studies often focused on measuring editing efficiency at the intended target site. Did the desired edit occur? What percentage of cells were successfully edited?


While these metrics remain important, they provide only part of the picture.


A growing body of researchi, ii has demonstrated that genome editing can produce a diverse range of unintended outcomes. DNA repair processes can generate genomic changes that extend well beyond the intended edit site, including:


- Small insertions and deletions (indels)
- Large indels >50 bp
- Chromosomal rearrangements and translocations
- Vector integration events


Many of these events occur infrequently, but they may have important implications for product safety. As a result, regulators are increasingly asking developers to demonstrate not only that editing occurred as intended, but also that unintended genomic consequences have been appropriately assessed.


Large indels >50 bp make up circa 6% of edits in this study by Hoijer et al (Fig 5)iii


The FDA draft guidance reflects this broader perspective. Throughout the document, there is a clear emphasis on evaluating off-target effects and assessing loss of genome integrity, including structural genomic alterations that may be difficult to capture using traditional approaches.


---


## Why long-read sequencing matters for genome editing safety


Many of the genomic events highlighted in the guidance are fundamentally structural in nature.


A large deletion may remove thousands of bases surrounding an edit site. A translocation may connect DNA segments from different chromosomes. A vector integration event may create a novel genomic junction that is not represented in the reference genome or could pose a safety risk. Complex rearrangements may involve multiple breakpoints and repair outcomes occurring within the same molecule.


These events can be challenging to reconstruct from short sequencing fragments.


[Long-read sequencing](https://www.pacb.com/technology/long-read-sequencing/) provides a fundamentally different view. Rather than piecing together structural changes from fragmented reads, researchers can directly observe edited molecules and their genomic context.


This capability is particularly important when evaluating the exact classes of genomic alterations emphasized in the FDA guidance.


Long reads enable researchers to:


- Resolve large deletions and insertions
- Characterize chromosomal rearrangements
- Identify translocation breakpoints
- Detect vector integration events
- Assess chromosomal integrity
- Evaluate complex editing outcomes at single-molecule resolution


As the FDA’s recommendations make clear, understanding genome editing safety increasingly requires both sensitivity and structural resolution.


Detecting off-target effects and characterizing unintended editing outcomes at this level of resolution is an area where long-read sequencing offers real advantages over short-read methods.


---


## HiFi sequencing delivers accuracy, read length, and off-target detection


PacBio[HiFi sequencing](https://www.pacb.com/technology/hifi-sequencing/) is uniquely positioned to address these challenges because it combines long reads with greater than 99.9% accuracy.


Historically, researchers often faced a tradeoff between read length and accuracy. Short-read technologies provided highly accurate base-level information but limited structural context, whereas long-read technologies offered structural insight but often sacrificed accuracy.


HiFi sequencing eliminates this compromise.


By combining long-read coverage with exceptional accuracy, HiFi sequencing enables researchers to directly observe the genomic outcomes that matter most. Rather than choosing between accuracy and structural resolution, genome editing developers can obtain both in a single dataset.


### *By delivering long, highly accurate reads, HiFi enables comprehensive characterization of both small and large editing outcomes within a single workflow.*


Researchers can accurately quantify intended edits, detect low-frequency off-target effects, and characterize the structural, unintended editing outcomes using the same technology platform.


This combination makes HiFi sequencing particularly well suited for off-target detection and characterizing unintended editing outcomes, the two areas where genome editing safety assessment most often runs into blind spots. These capabilities align closely with the FDA’s emphasis on comprehensive assessment of genome editing outcomes and genome integrity.


---


## Flexible long-read sequencing workflows for genome editing analysis


As part of the[NIST genome editing consortium](https://www.nist.gov/programs-projects/nist-genome-editing-consortium) , PacBio is an active participant in developing and comparing workflows and technologies to help characterize gene editing outcomes with confidence.


### **Whole genome sequencing for unbiased discovery**


For broad assessment of genome integrity, HiFi[whole genome sequencing](https://www.pacb.com/products-and-services/applications/whole-genome-sequencing/) (WGS) provides an unbiased view of genomic changes across the entire genome.


This approach helps enable researchers to identify structural variants, chromosomal rearrangements, translocations, and other unintended editing outcomes without being limited to predefined target regions.


As regulatory expectations increasingly emphasize comprehensive characterization, WGS provides a powerful tool for evaluating potential safety risks and discovering unexpected genomic alterations.


### **Amplicon-based HiFi sequencing for targeted characterization**


When researchers need deep characterization of known on-target or off-target sites, amplicon-based HiFi sequencing provides highly accurate analysis of editing outcomes.iv


Unlike conventional short-read amplicon sequencing, long HiFi reads can span larger genomic regions surrounding an edit site. This enables detection of not only small indels, but also larger insertions, deletions, and complex repair outcomes that may otherwise be missed.


For developers seeking detailed characterization of candidate editing sites,[amplicon sequencing](https://www.pacb.com/wp-content/uploads/Application-Brief-Targeted-sequencing-for-amplicons-best-practices.pdf) offers a targeted and scalable approach while preserving the advantages of long-read sequencing.


### **PureTarget panels for amplification-free analysis**


The FDA guidance also notes the importance of minimizing technical artifacts that could complicate interpretation of editing outcomes.


PacBio[PureTarget](https://www.pacb.com/technology/puretarget/) enrichment technology enables targeted, amplification-free sequencing of genomic regions of interest. By eliminating PCR amplification, researchers can reduce amplification bias and preserve the native structure of edited molecules.


This provides a particularly valuable approach for evaluating complex structural outcomes, insertion events, and genomic rearrangements where accurate representation of the original DNA molecule is critical.v


*Frequencies of reference (blue) and alternative (red) alleles at single nucleotide variant (SNV) positions in heterozygous target regions for F1 zebrafish. The bars to the left show allele frequencies for long-range PCR (LR-PCR) while the bars to the right show allele frequencies for PureTarget in the same samples. The minor allele frequencies obtained by PureTarget are close to 50% while LR-PCR results show skewed frequencies of the two haplotypes. The colors below the bars indicate which of the target sites were examined. The target sites have been detailed below each bar. Reproduced with permission from *[Höijer et al. (2025)](https://www.biorxiv.org/content/10.1101/2025.09.08.674810v1) , Figure 2b.**


PureTarget combines the benefits of targeted analysis with the confidence that comes from directly sequencing native DNA, making it a powerful option for genome editing characterization and safety assessment.


---


## Characterizing genomic variants across editing outcomes


The FDA guidance makes clear that genome editing safety assessment should encompass a wide range of potential outcomes.


PacBio HiFi sequencing helps enable researchers to characterize this full spectrum, including:


- Small insertions and deletions
- Precise on-target edits
- Low-frequency off-target edits
- Large indels
- Complex structural variants
- Chromosomal rearrangements and translocations
- Vector integration events


Importantly, these analyses can be performed using a combination of whole-genome, targeted, and amplification-free workflows on a single sequencing platform.


Other long-read technologies are less well-suited to detect small indels and edits due to their lower accuracy compared to HiFi. HiFi sequencing allows you to confidently characterize editing outcomes using one technology.


---


## A new standard for genome editing characterization


The FDA’s draft guidance represents an important milestone for the genome editing field. The agency is signaling that comprehensive genomic characterization is becoming a core component of genome editing safety assessment.


The guidance also acknowledges an important reality: different editing outcomes require different sequencing tools. While short-read sequencing may be sufficient for characterizing small edits, larger structural alterations and chromosomal changes increasingly require long-read approaches.


These are precisely the types of genomic events where HiFi sequencing provides its greatest value.


With flexible workflows that include whole-genome sequencing, amplicon-based analysis, and amplification-free PureTarget enrichment, PacBio enables researchers to detect and characterize genome editing outcomes ranging from single-base changes to chromosome-scale alterations, **including off-target effects and other unintended editing outcomes that short-read methods can miss.**


As genome editing therapies advance toward broader clinical adoption, the ability to generate comprehensive, high-confidence genomic data will become increasingly important. HiFi sequencing provides the accuracy, read length, and workflow flexibility needed to help meet the evolving expectations of regulators and help accelerate the development of safer genome editing therapies.


Visit the PacBio[gene editing page](https://www.pacb.com/gene-editing/) to learn more or[connect with a PacBio expert](https://www.pacb.com/ask-a-question/) .


---


I Tei, C., Hata, S., Mabuchi, A., et al. (2023). Comparable analysis of multiple DNA double-strand break repair pathways in CRISPR-mediated endogenous tagging. *bioRxiv* 2023.06.28.546861[https://doi.org/10.1101/2023.06.28.546861](https://doi.org/10.1101/2023.06.28.546861)


II Kosicki, M., Tomberg, K., Bradley, A. (2018). Repair of double-strand breaks induced by CRISPR–Cas9 leads to large deletions and complex rearrangements. *Nature Biotechnology* . 36(8):765–771.[https://doi.org/10.1038/nbt.4192](https://doi.org/10.1038/nbt.4192)


III Höijer, I. et al. (2022). CRISPR-Cas9 induces large structural variants at on-target and off-target sites in vivo that segregate across generations. *Nature Communications.* 13(1):627.[https://doi.org/10.1038/s41467-022-28244-5](https://doi.org/10.1038/s41467-022-28244-5)


IV Arnson, B. et al. (2025). Efficacious genome editing in infant mice with glycogen storage disease type Ia. *JCI Insight* . 10(18):e181760.[https://doi.org/10.1172/jci.insight.181760](https://doi.org/10.1172/jci.insight.181760)


V Höijer, I., Ameur, A. (2025). Accurate characterization of CRISPR-Cas9 genome editing outcomes and mosaicism with near-perfect long reads. *bioRxiv* .[https://doi.org/10.1101/2025.09.08.674810](https://doi.org/10.1101/2025.09.08.674810)
