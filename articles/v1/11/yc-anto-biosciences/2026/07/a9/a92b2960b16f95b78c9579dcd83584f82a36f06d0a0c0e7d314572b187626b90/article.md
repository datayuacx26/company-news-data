---
schema_version: "1.0.0"
document_id: "a92b2960b16f95b78c9579dcd83584f82a36f06d0a0c0e7d314572b187626b90"
company_key: "yc-anto-biosciences"
company: "Anto Biosciences"
source_id: "yc-anto-biosciences-news-import-c516c9aa21d4"
canonical_url: "https://anto.bio/publications/ai-controlled-metagenomics"
published_at: null
first_seen_at: "2026-07-24T23:08:26.325303+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:fb15f2e64ed0df2757c153c463d2c7e151fd76156c05fb43ceba794dd36d151a"
---

# AI-controlled Metagenomics

## High-Throughput Taxonomic Profiling for Clinical Pathogen Detection and Early-Stage Cancer Screening and Type Classification


### Abstract


Searching for genomic sequences belonging to pathogens or cancer-promoting microbes is an essential and fundamental task in biomedical research and most genomic analyses. State-of-the-art metagenomic pipelines performing such computations fail to cope with the exponential growth of genomic sequencing data. Current computational metagenomic methods for clinical applications, such as cancer diagnostics and pathogen detection, indiscriminately process all genomic sequences, irrespective of their relevance to specific diseases. This approach incurs substantial resource and runtime overhead due to the computationally intensive procedures applied to sequences irrelevant to clinical diagnosis. We introduce the novel concept of AI-controlled metagenomics. As metagenomic data advances through the computational pipeline, an AI control unit dynamically prioritizes sequences based on their relevance to achieving a clinical diagnosis. Irrelevant sequences are processed using less computationally demanding algorithms or are discarded entirely. Upon identifying a specific pathogen/disease or excluding its presence, the AI control unit enables early termination of the computational process.


We introduce HighClass, a new metagenomic profiling pipeline designed for clinical early-stage cancer screening and type classification, exemplifying the concept of AI-controlled metagenomics. HighClass incorporates an AI/ML control unit, which directs the bioinformatic analysis of individual reads in real-time based on their estimated pathogenicity and relevance to the cancer microbiome. This control model deprioritizes irrelevant reference genomes and read sequences by analyzing previously determined mapping locations and assessing the improbability of various cancer types and pathogens. Consequently, it employs faster and less precise algorithms for reads irrelevant to cancer diagnosis while using accurate algorithms for relevant sequences, resulting in rapid, accurate, and memory-frugal analyses. Our AI/ML control unit enables early termination when improbable cancer types dominate, accurately identifying healthy individuals by processing, on average, less than 10% of the metagenomic dataset. Our evaluation demonstrates that HighClass achieves, on average (geo. mean), an 8x speedup in cancer and pathogen detection compared to MetaTrinity while maintaining comparable accuracy. For healthy patients, HighClass achieves a 60x speedup over MetaTrinity.
