---
schema_version: "1.0.0"
document_id: "f9a2cd8a650b77d3e213fcc4bfb4de1ccb25e18fd4b523ea4785494698d69534"
company_key: "absci-corporation-common-stock"
company: "Absci Corporation"
source_id: "absci-corporation-common-stock-rss-3a87fd054c09"
canonical_url: "https://www.biorxiv.org/content/10.1101/2023.12.08.570889"
published_at: "2024-12-18T20:32:02+00:00"
first_seen_at: "2026-07-20T23:22:57.028146+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:dfad3d4a462b8e89af9cf7945674ab28d8ab2c745bb76fa295e94a1b8b4bf13e"
---

# IgDesign: In vitro validated antibody design against multiple therapeutic antigens using inverse folding

## Abstract


Deep learning approaches have demonstrated the ability to design protein sequences given backbone structures \[1 ,2 ,3 ,4 ,5 \]. While these approaches have been applied *in silico* to designing antibody complementarity-determining regions (CDRs), they have yet to be validated *in vitro* for designing antibody binders, which is the true measure of success for antibody design. Here we describe *IgDesign* , a deep learning method for antibody CDR design, and demonstrate its robustness with successful binder design for 8 therapeutic antigens. The model is tasked with designing heavy chain CDR3 (HCDR3) or all three heavy chain CDRs (HCDR123) using native backbone structures of antibody-antigen complexes, along with the antigen and antibody framework (FWR) sequences as context. For each of the 8 antigens, we design 100 HCDR3s and 100 HCDR123s, scaffold them into the native antibody’s variable region, and screen them for binding against the antigen using surface plasmon resonance (SPR). As a baseline, we screen 100 HCDR3s taken from the model’s training set and paired with the native HCDR1 and HCDR2. We observe that both HCDR3 design and HCDR123 design outperform this HCDR3-only baseline. IgDesign is the first experimentally validated antibody inverse folding model. It can design antibody binders to multiple therapeutic antigens with high success rates and, in some cases, improved affinities over clinically validated reference antibodies. Antibody inverse folding has applications to both *de novo* antibody design and lead optimization, making IgDesign a valuable tool for accelerating drug development and enabling therapeutic design. The data generated in this study serve as a useful benchmark of diverse antibody-antigen interactions. We use this data to benchmark self-consistency RMSD (scRMSD), using ABodyBuilder2 \[6 \], ABodyBuilder3 \[7 \], and ESMFold \[8 \], as a metric for assessing binding. We open source the code for IgDesign and the SPR datasets.


Code and datasets can be found at[https://github.com/AbSciBio/igdesign](https://github.com/AbSciBio/igdesign) .


### Competing Interest Statement


The authors are current or former employees, contractors, interns, or executives of Absci Corporation and may hold shares in Absci Corporation.


## Footnotes


-


Open sourcing data and code. Analysis of self-consistency RMSD (scRMSD) as a metric for predicting binding.


-


[https://github.com/AbSciBio/igdesign/tree/main](https://github.com/AbSciBio/igdesign/tree/main)


-


↵ 8


[https://github.com/BytedProtein/ByProt/issues/3](https://github.com/BytedProtein/ByProt/issues/3)


-


↵ 9


[https://github.com/dauparas/ProteinMPNN](https://github.com/dauparas/ProteinMPNN)


-


↵ 10


[https://github.com/dauparas/ProteinMPNN/blob/main/examples/submit_example_3.sh](https://github.com/dauparas/ProteinMPNN/blob/main/examples/submit_example_3.sh)
