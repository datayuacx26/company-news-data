---
schema_version: "1.0.0"
document_id: "02ac7be442f74e77e9f66c4a192377eafa55ec4182dc0fcbe0c255a0fbe9b7c3"
company_key: "yc-kilobaser"
company: "Kilobaser"
source_id: "yc-kilobaser-news-import-9f375b6e1738"
canonical_url: "https://kilobaser.com/post/the-pain-of-primer-dimer"
published_at: null
first_seen_at: "2026-07-25T10:46:06.691006+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:eaac3e42e3ac3ea379b4da9d39173daef2a56613b2f87bb4896f97f55a002af9"
---

# The Pain of Primer Dimer

# The Pain of Primer Dimer


Assay development has been a hot topic since the coronavirus outbreak triggered a huge demand for molecular diagnostic kits. In fact, various SARS-CoV-2 diagnostic kits, usually qPCRs, are offered by many companies and national health authorities. Numerous optimizations are required to ensure accurate diagnosis of the virus. One of the most important optimization steps is primer design, as non-optimal primer sets can lead to incorrect results.


## How to recognize and avoid primer dimers


What can go wrong with a „bad“ primer design?


You may think that in the worst case, a „bad“ primer design will result in your target DNA not binding and thus no amplification. However, the real nightmare of qPCR is when primers are binding primers. The result of this binding interaction is primer dimers that are recognized by polymerases as starting points for strand elongation. Repeated binding and elongation multiply the amount of this artifact, which has nothing to do with the actual target sequence.


#### How are primer dimers formed?


Complementarity of only a few nucleotides is sufficient to bind primers together. A primer can either bind another primer and form a cross-primer dimer (Figure 1) or bind itself and form a self-primer dimer (Figure 2).


*Figure 1: Cross primer-dimer resulting from annealing of two primers*


*Figure 2: Self primer-dimer resulting from annealing with itself*


These primer interactions are weak and require a low annealing temperature. Therefore, it is important to perform qPCR at a sufficiently high annealing temperature to avoid nonspecific interactions including primer dimers. However, the greatest chance of primer dimer formation occurs just prior to the start of PCR. After all components for a qPCR have been mixed together, this test mixture still needs to be heated to 95°C. By then, the temperature is low enough for primer dimers to form. These primer dimers were a major problem before the introduction of „hot start“ polymerases, which do not begin to work until activated at 95°C.


Primer binding is enhanced when primers are present in high concentrations. Therefore, it is recommended to optimize the concentration of the primer set during the development of the assay. However, in multiplex reactions where multiple primers are combined, high primer concentration cannot be avoided. Primer dimer formation is therefore a significant problem in multiplex qPCR design and is the main reason for the need for intensive primer optimization.


#### How do primer dimers affect a qPCR?


Depending on the setup, there are many ways in which primer dimers affect detection of qPCRs. In general, primer dimers:


-


inhibit binding to target DNA (since primers are already bound to themselves, they can no longer bind to the target).


-


occupy polymerases and thus prevent amplification of the target.


-


consume dNTDs from the reaction pool.


All this leads to a reduced amplification of the target sequence and thus to a lower efficiency of detection.


#### False positive result


An assay with primer dimers can lead to a false positive result due to the detection of the ascending non-target amplicon. As shown in Figure 3, with SYBR Green Real-Time PCR detect primer dimer amplification in the non-template control in addition to template amplification. Such false-positive results can be easily avoided by using, for example, target-specific TaqMan probes.


*Figure 3: SYBR Green Real-Time PCR - with template (violet) and without template (light blue)*


#### False negative result


However, primer dimers can also lead to a false negative result, as low concentrations of the target gene are not efficiently amplified due to the lack of amplification resources such as dNTDs, primers and polymerase. This effect can be seen in the increase in Ct value, as shown in Figure 4 – two reverse primers with different tendency to primer-dimer formation resulted in different Ct values under the same conditions.


*Figure 4: TaqMan probe Real-Time PCR using primer with low primer-dimer tendency (orange) and with high primer-dimer tendency (violet)*


#### How to avoid primer dimers?


Nowadays, several online and software tools can estimate the tendency of dimerization and thus improve primer design. However, most of these tools are limited in terms of the number of primers and the correct reproduction of experimental conditions. Therefore, it is still difficult to evaluate multiplexing reactions in-situ.


It is recommended to design and experimentally test different primer sets to find the optimal primer for an assay. The first step in assay design is to perform SYBR-green PCR with the different primer sets with and without template. The non-specific binding of SYBR-green makes any dimer formation visible in the non-template control (NTC). The lower the Ct value in the NTC, the higher the tendency of this primer set to form dimers.


#### How can Kilobaser help?


In summary, even with a supposedly perfectly designed primer set, a test analysis is still necessary. Optimized primer composition is essential, especially in assay development for the diagnostic industry.


In the test phase for the perfect primer composition, fast and flexible troubleshooting, many different constellations and rapid adjustments of the sequences and conditions are required. With Kilobaser you can synthesize many DNA oligos quickly and flexibly and complete your troubleshooting rapidly.


## You want to learn more about Kilobaser?


You are only 3 steps away from your ready-to-use DNA. Take a closer look and let us explain all the details in your exclusive demo!


[book your demo](https://kilobaser.com/demo)


Share this article:


[previous One Step RT-qPCR – Application Note](https://kilobaser.com/post/one-step-rt-qpcr-application-note)[next DNA Data Storage](https://kilobaser.com/post/dna-data-storage)
