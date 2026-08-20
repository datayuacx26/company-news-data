---
schema_version: "1.0.0"
document_id: "bf45645706b3ee26463103ec7c273d9f9694c3ee0f10ebd193394026bf96a55c"
company_key: "pacific-biosciences-of-california-inc-common-stock"
company: "Pacific Biosciences of California Inc."
source_id: "pacific-biosciences-of-california-inc-common-stock-rss-232046c7699f"
canonical_url: "https://www.pacb.com/blog/the-practical-value-of-long-range-pcr-for-targeted-hifi-sequencing/"
published_at: "2026-07-28T13:00:22+00:00"
first_seen_at: "2026-07-28T14:14:27.135733+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:1b4b4ee1cfea94074240649eb815ab27213b600ce8910e6af34f7d227b9bfacf"
---

# The practical value of long-range PCR for targeted HiFi sequencing

For clinical research laboratories evaluating long-read sequencing, the central question is often not whether longer reads are useful. The value is clearest in the places where conventional approaches have historically required a patchwork of assays: regions with high sequence similarity, repetitive elements, structural variation, repeat expansions, or haplotypes that cannot be resolved confidently from short fragments alone.


That is where long-range PCR (LR-PCR) can play an important role. By amplifying larger DNA fragments, typically greater than 5 kb, LR-PCR gives researchers a way to focus sequencing on regions where completeness matters most. Instead of interrogating a complex locus through many short, overlapping reactions, labs can design either one or a small number of long amplicons that preserve the broader context of the region of interest.


When those amplicons are sequenced with PacBio HiFi reads, the result is a[targeted workflow](https://www.pacb.com/products-and-services/applications/targeted-sequencing/) that combines the depth and efficiency of PCR-based enrichment with the accuracy, read length, and haplotype resolution of HiFi sequencing. For defined clinical research questions, that combination can make difficult loci more accessible without requiring teams to move immediately to whole genome sequencing for every application.


Explore our targeted sequencing resources, including practical guidance on LR-PCR design and workflow development.


[Explore targeted resources](https://programs.pacb.com/l/1652/2026-07-27/45lhyz)


## Overcoming short-read limitations in complex loci


Many clinically relevant genes are challenging because the variant of interest is not always a simple single-nucleotide change in an easy-to-map region. These[complex regions](https://www.pacb.com/blog/why-long-reads-are-changing-how-researchers-approach-targeted-sequencing/) may contain segmental duplications, pseudogenes, GC-rich sequence, mobile elements, repeat tracts, or structural changes that span beyond the length of conventional short-read fragments. In those settings, the technical problem is not just generating sequence. It is keeping enough genomic context together to understand what is actually present on each allele.


Traditional Sanger sequencing can be useful for small, defined targets, but complex loci often require many overlapping primer pairs and manual reconstruction across the region. Short-read panels can scale efficiently, but they can also lose physical linkage across variants, creating ambiguity in phasing, breakpoint interpretation, or repeat characterization.


LR-PCR paired with HiFi[long-read sequencing](https://www.pacb.com/technology/long-read-sequencing/) changes that starting point. A well-designed long amplicon can span the region of interest so that SNVs, indels, structural changes, repeat sequences, and nearby variants are read in the same molecule. That physical linkage is especially valuable for clinical research teams asking whether variants occur on the same chromosome or opposite chromosomes, whether a repeat expansion is associated with a particular haplotype, or whether a structural change alters the broader architecture of a locus.


## Consolidating Sanger, MLPA, and repeat-primed PCR into a single workflow


The operational case for LR-PCR is closely tied to consolidation. In many targeted workflows, different variant classes have historically required different methods: Sanger sequencing for sequence-level changes, MLPA or arrays for copy number, repeat-primed PCR or Southern blotting for repeat expansions, and statistical approaches for haplotyping. Each method can answer part of the question, but each also adds handoffs, interpretation steps, and opportunities for uncertainty.


A targeted HiFi workflow built on LR-PCR offers a more integrated path. For a given locus, the same amplicon can provide full-length sequence, direct read-backed phasing, breakpoint-level context for structural variation, and sequence-level information across repetitive or low-complexity regions. While it does not make assay design automatic or eliminate the need for validation, it does give laboratories a practical way to ask more complete questions from a single targeted experiment.


This is the same kind of implementation question many labs are asking as long-read sequencing moves from evidence generation into operational planning. Where does a targeted long-read assay fit within an existing menu? Which applications benefit from a focused LR-PCR approach, and which are better served by HiFi whole genome sequencing? How can teams reduce complexity without sacrificing analytical confidence?


## Long-range PCR primer design best practices for HiFi sequencing


The value of LR-PCR depends heavily on what happens before the sample reaches the sequencer. Primer placement, template quality, polymerase choice, and cycling conditions all influence whether the final HiFi dataset reflects the intended biology appropriately and reproducibly.


For clinical research laboratories, that makes assay design a critical planning step. Primer binding sites should be selected outside repetitive sequences and away from common or clinically relevant polymorphisms where possible. Forward and reverse primers should be closely matched in melting temperature, screened for hairpins and primer-dimers, and checked for genome-wide specificity. High-molecular-weight DNA input is important because a broken template cannot support long-range amplification across the full target.


Polymerase selection is equally important. Long amplicons require high processivity and high fidelity, particularly when the amplified material will be used for variant analysis. Gradient PCR, gel confirmation of a single clean band, and careful quantification before library preparation are practical quality checkpoints that can reduce downstream ambiguity.


For detailed guidance on primer design, polymerase selection, and workflow optimization, visit our targeted sequencing hub to download resources including our LR-PCR Application note.


[Explore targeted resources](https://programs.pacb.com/l/1652/2026-07-27/45lhyz)


## When to use LR-PCR for targeted sequencing


Despite its advantages, LR-PCR is not the answer to every clinical research sequencing question. If the goal is to discover variation broadly across the genome, HiFi[whole genome sequencing](https://www.pacb.com/products-and-services/applications/whole-genome-sequencing/) may be the better entry point. Or if the priority is native methylation detection for your targets, amplification-free workflows that preserve base modifications, like[PureTarget](https://www.pacb.com/technology/puretarget/) , should be considered. But when the question is defined, the locus is difficult, and deep targeted interrogation is needed, LR-PCR can be a highly practical front end for HiFi sequencing.


This makes it particularly relevant for teams evaluating targeted approaches in areas such as inherited disease, carrier screening, pharmacogenomics, and other research applications where specific genes are known to be challenging for conventional methods. In these contexts, the value of LR-PCR is not simply that it produces a longer amplicon. The value is that it enables a more complete, phased, and interpretable view of a region that may otherwise be fragmented across multiple tests.


## Building a clinical research LR-PCR workflow with HiFi sequencing


As clinical research labs consider how to adopt long-read sequencing, the practical details matter. A successful LR-PCR workflow starts with clear target definition, thoughtful primer design, high-quality DNA, the right polymerase system, and up-front optimization. HiFi sequencing can then convert that focused amplicon strategy into highly accurate long-read data to support more comprehensive analysis of difficult loci.


The new PacBio LR-PCR Application note walks through this workflow step by step, from selecting target boundaries and screening primer sites to choosing polymerases, optimizing thermal cycling, preparing libraries, and troubleshooting common amplification issues.


For labs moving from the idea of targeted long-read sequencing to practical assay development, LR-PCR offers a clear path forward with HiFi reads in a single assay.


Access PacBio targeted sequencing resources, including the LR-PCR Application Note and guidance for other targeted sequencing approaches


[Explore targeted resources](https://programs.pacb.com/l/1652/2026-07-27/45lhyz)[Connect with a PacBio scientist](https://www.pacb.com/ask-a-question/)
