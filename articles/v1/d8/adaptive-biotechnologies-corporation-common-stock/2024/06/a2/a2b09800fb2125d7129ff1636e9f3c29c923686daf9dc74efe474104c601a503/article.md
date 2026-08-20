---
schema_version: "1.0.0"
document_id: "a2b09800fb2125d7129ff1636e9f3c29c923686daf9dc74efe474104c601a503"
company_key: "adaptive-biotechnologies-corporation-common-stock"
company: "Adaptive Biotechnologies Corporation"
source_id: "adaptive-biotechnologies-corporation-common-stock-rss-662b961e0510"
canonical_url: "https://www.adaptivebiotech.com/2024/06/07/decoding-public-t-cell-signatures-of-disease-with-tens-of-thousands-of-t-cell-receptor-repertoires/"
published_at: "2024-06-07T21:36:29+00:00"
first_seen_at: "2026-07-20T04:36:00.414288+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:e4c38a612c8bae45242806849d026913e47f2732ecabbadb15a8d35dca0b7c36"
---

# Decoding Public T-cell Signatures of Disease with Tens of Thousands of T-cell Receptor Repertoires

Damon May, PhD, Associate Director, Computational Biology, Adaptive Biotechnologies


In the ever-evolving field of immunology, the study of T-cell responses to various exposures is pivotal for unraveling the complexities of immune health and advancing new therapies that use these natural defenses. T cells, through their unique receptors, record the history of an individual’s immune challenges, such as infections, vaccinations or various chronic conditions, referred to as T-cell signatures. Collectively, these signatures are comprised of both T-cell responses that are individualistic as well as those that are public across populations with overlapping exposures and shared Human Leukocyte Antigens (HLAs).


Understanding these signatures is crucial as they offer a window into the body’s adaptive immune responses, revealing how T cells differentiate and mobilize in the face of diverse threats. This knowledge is instrumental in developing targeted therapies and vaccines. By mapping the intricate relationship between T-cell responses and exposures, researchers can identify immune system malfunctions and craft interventions that bolster our defense mechanisms against diseases.


At Adaptive, we have spent over a decade generating the largest collection of immune receptor data from tens of thousands of donors. Together with our partners at Microsoft Research, we have developed computational models to infer the HLA type of a donor from their T-cell receptor (TCR) repertoire. We’ve also associated millions of “public” TCRs (TCRs that occur in many people) with the HLAs that present the antigens that the TCRs respond to. The combination of these two capabilities – to infer a donor’s HLA type and to catalog the public TCRs associated with HLAs – enables new kinds of discoveries.


Adaptive and Microsoft have released three manuscripts on bioRxiv, shedding light on this complex relationship and offering new avenues for research and clinical applications.


**Large-scale mapping of TCRs to HLAs**


In the first manuscript, “[Large-scale statistical mapping of T-cell receptor β sequences to Human Leukocyte Antigens](https://www.biorxiv.org/content/10.1101/2024.04.01.587617v1) ,” we analyze Adaptive Immunosequencing data from >4,000 subjects with known HLA type to statistically associate a million public TCRs with hundreds of HLAs. This in turn allows us to build accurate models for imputing HLA genotypes from TCR repertoires. The reason that these TCRs associate with an HLA is that each TCR responds to some HLA-presented antigen that is part of a prevalent immune exposure, and so these HLA-associated TCRs collectively represent the public responses to many different exposures.


**Constructing ECOclusters: the public responses to individual immune exposures**


The second manuscript, “[Identifying immune signatures of common exposures through co-occurrence of T-cell receptors in tens of thousands of donors](https://www.biorxiv.org/content/10.1101/2024.03.26.583354v2) ,” leverages HLA-associated TCRs and immune repertoires from more than 30,000 donors to construct “ECOclusters” (Exposure-associated Co-Occurrence clusters), groups of HLA-associated TCRs that tend to co-occur in the same people. Each ECOcluster putatively represents the “public” T-cell response to some virus, bacterium, or other prevalent exposure. We use T-cell repertoires from donors of known serological status for 7 exposures (Cytomegalovirus, SARS-CoV-2, HSV-1, HSV-2, EBV, Parvovirus and *T. gondii* ) to identify a single ECOcluster associated with each exposure.


**Exploring the Cytomegalovirus (CMV) ECOcluster, a new public resource**


A third manuscript, “[A catalog of the public T-cell response to Cytomegalovirus](https://www.biorxiv.org/content/10.1101/2024.05.08.593237v1) ,” makes the 26,106-TCR ECOcluster associated with Cytomegalovirus (CMV) publicly available and explores it in detail. We use TCR sequence similarity within ECOclusters to identify groups of TCRs that appear to respond to the same antigen, and we find suggestions of different subgroups of CMV-exposed donors responding to different antigens. CMV has clinical relevance in certain settings, like transplant rejection. More broadly, the CMV ECOcluster represents the first comprehensive catalog of the public T-cell response to a virus with a profound impact on the T-cell repertoire.


Combined, these three manuscripts demonstrate important new capabilities but also highlight a great enigma still to be solved. We’ve defined the public T-cell responses to many prevalent immune exposures, and we can use them to construct a ledger of an individual’s historical exposures. However, most of the entries in that ledger remain a mystery: currently, we only know the identity of seven of them. Through future work with collaborators, and by experimentally associating more TCRs with their HLA-presented antigens, we will learn the exposures associated with more and more ECOclusters, adding labels to this new catalog of public immune responses.


These capabilities have applications beyond infectious disease. Adaptive trains AI/ML models to help accelerate our target and drug discovery efforts. For example, in autoimmune disorders, we can infer the HLA types of affected individuals, to isolate the HLA presentation of specific autoimmune responses and help identify their target antigens. We can also use our catalog of TCR responses to public exposures to explore the intersection between pathogen response and autoimmunity, building our knowledge of the mechanism of disease.


These new capabilities extend Adaptive’s reach as a key player in an era of novel target discovery and development of immune-mediated therapeutics to help tackle devastating diseases. We look forward to decoding more and more of the public responses to prevalent immune exposures, so we can realize a future where we fully leverage diagnostic and therapeutic power of the adaptive immune system.
