---
schema_version: "1.0.0"
document_id: "dc06bf75f82d3126c942dcbfa89a57f8f160ea6688e97677f029c00119aaf9b7"
company_key: "yc-volta-labs-inc"
company: "Volta Labs, Inc."
source_id: "yc-volta-labs-inc-news-import-34ea91a0655a"
canonical_url: "https://voltalabs.com/post/automating-the-last-mile-of-targeted-sequencing-workflows-on-callisto"
published_at: "2025-06-05T00:00:00+00:00"
first_seen_at: "2026-07-26T04:56:32.866801+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:8873ca530c2f2ac3e603aa2a2d65a84a992127e556466e57a0715632950b5919"
---

# Automating the Last Mile of Targeted Sequencing Workflows on Callisto

# Automating the Last Mile of Targeted Sequencing Workflows on Callisto


‍


## Hybridization Capture in Next-Generation Sequencing


In the world of genomics, researchers often need to focus their sequencing efforts on specific regions of interest rather than sequencing entire genomes. This targeted approach enables focused, deeper coverage of specific genomic regions that are most relevant to research or clinical questions. In many contexts, particularly clinical applications, this focused analysis provides a more manageable dataset with fewer variants of unknown significance, reducing the computational burden while delivering the most actionable insights.


Hybridization capture represents one of the essential approaches for targeted sequencing applications, particularly when working with large panels or when discovery of novel variants is important. Common implementations of hybridization capture include DNA and RNA "bait" strategies where short oligonucleotide sequences complementary to regions of interest are used to selectively enrich target DNA, whether that's a handful of genes or the entire protein-coding portion of the genome (the exome). Various methods are used to separate and typically amplify target-bound baits from the rest of the input sample, resulting in a highly enriched pool of targeted DNA fragments that can then be sequenced more deeply and efficiently than would be practical for an entire genome.


Overall, this approach offers higher discovery power for novel variants, better coverage uniformity across target regions, including those with challenging GC content, greater scalability with larger panels, and higher analytical sensitivity. Collectively these advantages of targeted sequencing have driven diverse applications like cancer genomics, including somatic variant detection and liquid biopsy analysis, minimal residual disease (MRD) monitoring, hereditary disease testing, and carrier screening.


‍


## Challenges of Implementing Hybridization Capture Workflows


Despite its advantages, implementing hybridization capture workflows in laboratories remains challenging. The process is complex, requiring temperature sensitive heated wash steps involving magnetic beads, and is typically lengthy and time-consuming, often requiring a full day or more to complete. Traditional manual methods demand constant attention and precision from skilled technicians, which can lead to significant variability between operators and runs. Current pipetting-based lab automation platforms also fall short, as they are not built to handle the complexity of hybrid capture workflows that require extensive method development, complex programming, and dedicated engineering resources to implement and maintain effectively.


These challenges translate directly to real laboratory pain points: high failure rates requiring costly and time-consuming sample prep and sequencing re-runs, unpredictable performance across different sample types and operators, complex protocols that limit who can perform the work, and workflows that tie up valuable personnel for extensive periods. As genomic applications continue to expand into clinical and applied settings, these limitations have become increasingly problematic.


‍
