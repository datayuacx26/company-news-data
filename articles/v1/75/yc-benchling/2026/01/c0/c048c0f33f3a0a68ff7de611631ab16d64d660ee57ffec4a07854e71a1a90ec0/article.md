---
schema_version: "1.0.0"
document_id: "c048c0f33f3a0a68ff7de611631ab16d64d660ee57ffec4a07854e71a1a90ec0"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/why-standardizing-antibody-therapeutic-nomenclature-matters"
published_at: "2026-01-20T07:00:00+00:00"
first_seen_at: "2026-08-03T22:55:04.861414+00:00"
fetched_at: "2026-08-05T03:48:44.436222+00:00"
content_hash: "sha256:01d428f682d5d129bc4aba12df86528af22f139dbe248abff2b25c6a1a7bb418"
---

# Standardizing antibody therapeutic nomenclature: Why it matters

## Moving away from classic Y shaped full-length antibodies: Why so many formats?


In the past decade, there has been a substantial increase of antibody therapeutics with unconventional format entering the clinic, reaching late-stage studies, and being granted marketing approval.1,2,3 Since 2021, non-canonical antibodies (e.g. bispecifics, multispecifics, antibody drug conjugates and immunoconjugates) account for nearly half of the antibodies entering first in human studies, and a third of those being granted marketing approval each year.3


Several factors contributed to the increased diversity of antibody therapeutic formats. Advances in structural biology and antibody engineering have enabled scientists to design therapeutic antibodies as modular systems. Functional elements can now be combined into antibody fragments (with or without Fc), appended immunoglobulins, or antibody fusion proteins, to generate monospecific or multispecific antibody formats tailored to fine-tune valency, specificity, biodistribution, and function.


These ad hoc designed antibody formats, tailored to achieve the desired biological properties, have been crucial for overcoming the limitations of traditional antibodies and for expanding the range of therapeutic applications.


Finally, the growing number of biotech and pharmaceutical companies developing antibody therapeutics, together with considerations around Freedom to Operate (FTO) and Intellectual Property (IP), has further driven the development of novel antibody formats.‍


## Why we need a universal nomenclature for antibody therapeutics


Currently, antibody-based formats are often referred to by names coined by their inventors, some of which are trademarks (e.g. BiTE, DART, Nanobody, CrossMab) often with no universal logic or structure information, while others (e.g. scFv-Fc, Fab-IgG, scFv2-Fc, VHH2-Fc) are often inconsistent and can vary even across labs or companies. As a result, understanding what a name actually means often requires digging through papers or patent filings, slowing down communication, collaboration, and discovery.


This lack of standardization not only affects scientific communication, but also complicates the annotation of antibody-related databases. Different databases use different strategies to describe formats, and in many cases, molecules are annotated using the names provided by their developers or using one of the various synonyms used in the literature to describe the molecule.


‍The consequences extend beyond databases: inconsistent or vague naming also hinders clarity in patent literature, INN submissions, and regulatory filings, where precise format descriptions are critical. Without a standardized nomenclature, it becomes difficult to reliably compare formats, track design innovations, or assess therapeutic equivalence across sources.


‍As we enter the era of machine learning and AI-driven antibody research, resources such as antibody databases, patent literature, regulatory documents, and prescription labels hold valuable information. However, without standardized terminology, extracting and integrating this data becomes a major challenge. This inconsistency limits the generation of structured and harmonized datasets which are a critical prerequisite for applying machine learning to antibody discovery, developability analysis, and format optimization.


*Adapted from VERITAS: Harnessing the power of nomenclature in biologic discovery* *9*


### A standardized antibody format nomenclature will:


-


Enable clearer scientific discussion


-


Save time by eliminating the need to research background details for each format


-


Streamline formal processes like patent filings, regulatory and INN applications


-


Support project continuity and data integrity across research teams


-


Improve the consistency and usability of antibody databases


-


Allow meaningful comparisons between formats, facilitating formats grouping for computational analyses, including machine learning


## Existing classification and emerging nomenclature systems


Several groups have recognized the growing complexity of antibody formats and proposed ways to classify or name them more systematically.


‍In 2017, Brinkmann and Kontermann presented the “zoo of bispecific antibody formats” with over 100 different formats.4 They represent bispecifics and multispecifics as modular molecules assembled using a toolbox comprising different building blocks (antigen-binding modules as well as homo- and heterodimerization modules). They classify bispecifics according to format and composition, distinguishing them according to the presence or absence of an Fc region and a symmetric or an asymmetric architecture. They then further divide bispecifics with an Fc into those with an IgG-like structure and those that contain additional binding sites (i.e. appended or modified Ig-like structure).


‍This architectural lens is especially helpful given that most of the nomenclature challenges lie within the multispecific antibody space, where formats proliferate rapidly.


However, bispecific and multispecifics are not the only antibody categories with alternative formats. Monospecific antibodies can be assembled using building block-like strategies too, allowing optimization of valency and size. Furthermore, therapeutic antibodies, mono- or multispecific, can be conjugated to molecules (e.g. cytotoxic drugs, radioisotopes, PEG) or fused to non-antibody protein domains (e.g. cytokines, toxins, receptors).5


‍In a thorough systematic review, Wilkinson and Hale analyzed over 700 therapeutic antibodies with International Nonproprietary Names (INNs) and classified them in 57 antibody-related formats based on their domain assembly. These were grouped into five broad categories: monospecific antibodies, multispecific antibodies, Fc fusion proteins, antibody fusion proteins, and miscellaneous. They also highlighted the increase in complexity of therapeutic antibody design and number of formats, which continue to accelerate from early 2000s, moving away from classic IgGs.2


‍While these classification systems are informative and able to group antibodies into general format categories, others have gone a step further by proposing actual naming languages for antibody formats. These systems draw inspiration from established molecular notation frameworks such as SMILES (Simplified Molecular-Input Line-Entry System) for organic molecules and HELM (Hierarchical Editing Language for Macromolecules) for biologics, aiming to provide more tailored solutions for complex biologics.


‍SMILES has become a widely adopted standard for describing small molecules, though its application to biologics is limited.6 HELM, introduced in 2012 by the Pistoia Alliance, was designed to represent a broad range of complex biomolecules, including antibodies.7 However, while powerful, HELM is not ideally suited for multispecific antibodies due to its complexity, lack of support for annotating Fv specificities, and limited flexibility in representing fused domains.6


To address the unique challenges of next-generation antibody formats, two dedicated systems have emerged:


-


**Antibody Markup Language (AbML)** , a machine-readable format annotation system6


-


**Verified Taxonomy for Antibodies (VERITAS)** , a human-readable descriptive naming scheme8


Both aim to provide a standardized framework to communicate structural information about antibody therapeutics, paving the way for better collaboration, database integration, and even AI-assisted drug development.


## AbML (Antibody Markup Language)


AbML is a notation language for antibody formats. The language consists in describing antibody domains arranged in a string and separated by connectors. Software (abYdraw) is also provided. This can both generate and render AbML, converting the rather complex-looking notation into visual representations.6


### AbML – How it works


**Category**


**Description**


Allowed domains


- "VH", "VL", "VHH", "CH1", "CH2", "CH3", "CH4", "CL", "X" (other protein), "C" (chemical conjugation), "H" (hinge) and "L" (engineered linker). The system also allows domains to annotate T cell receptors.


Rules


- Antibody chains are represented from N-terminus to C-terminus.


- Each domain is separated by a dash "-" character.


- Chains are separated by the pipe "|" character.


- Each domain is followed by parentheses containing a unique identifying number, given according to the order of appearance in the string, followed by a colon ":" and the number of the domain(s) is interacting with. If the interacting domains have disulfide bonds, the number of bonds is indicated in curly brackets.


Supported annotations


- Each domain can be annotated with comments, written as a comma separated list within a set of square brackets.


- The system allows the annotation of modifications such as knob(">") and hole ("@"), positive ("+") and negative ("_") mutations for charge pairing, specific conjugation sites for Antibody Drug Conjugates ("^"), CH2 aglycosilation ("!"). Other types of modifications can be annotated using "*" followed by a comment in square brackets.


#### Pros


-


Very thorough notation language for antibody formats which allows to annotate fine details with precision.


-


Software to generate and render AbML is provided, facilitating a standardized annotation and generation of custom antibody diagrams.


-


Future proofed- suitable for annotating all current and currently conceivable antibody therapeutic formats.


-


Suitable for thoroughly annotating antibody therapeutic formats as required in patent applications and for international regulatory purposes, including INN applications.


#### Cons


-


Although AbML supports drug conjugation (random or site-specific), these are not currently rendered or supported in abYdraw.


-


The notation system produces rather complex strings and is therefore not suitable for use for common scientific discourse, or for grouping formats for computational analysis. ‍


## Verified Taxonomy for Antibodies (VERITAS)


VERITAS is a human-readable and standardized nomenclature system designed for multimeric antibody-based formats, relaying structural information about a format in a concise manner, without the need for diagrams. The system is based on the idea that formats can be broken down into various modules and format names are built around a main multimerization center, with N- and C-terminal appendages systematically described following specific rules.8


*Adapted from VERITAS: Harnessing the power of nomenclature in biologic discovery* *9* ‍


### **VERITAS – How it works**


**Category**


**Description**


Type of modules


- Multimerization center (e.g., IgG, heteroIgG, Fc, heteroFc, Fab).


- N- or C-terminal appendages (e.g., VHH, VH, VL, CH1, CH2, CH3, CL, Fd, LC, scFv, Fab, scFab, protein). Modules written before the multimerization centre are N-terminal appendages, while those written after the multimerization centre are C-terminal appendages.


Rules


- Formats are written from the N to C terminal of the molecule.


- Modules are separated by a dash ("-") when they are on the same amino acid chain, whereas modules that are on separate chains are separated by an asterisk ("*").


- Unless specific domains have appendages, modules are usually described by the most comprehensive descriptor. Light chains (VL-CL) are usually described by LC, Fab domains (Fd:LC) are usually described as Fab (similar logic is applied for Fc, heteroFc, IgG).


- In cases where specific chains of a domain have appendages, a colon ":" is used to represent noncovalent interaction between two chains which are part of the N- or C-terminal appendages (for example, Fab can be written Fd:LC), and appendages can be written next to the appropriate chain separated by "-" (the appendage will be reported before or after the chain depending on whether is attached to the N- or C-terminal).


- Symmetric formats (where appendages on both chains of the multimerization centre are the same): square brackets and asterisks are omitted.


- Asymmetric formats (where the chains of multimerization center do not have the same appendages): appendages information is reported in square brackets and appendages of different chains are separated by an asterisk "*". If one chain doesn't have appendages, no text will be reported after "*". If the number of appendages is the same, the chains are reported in alphabetical order, otherwise, the chain with the higher number of appendages is reported first.


Supported annotations


- The level of details can be customized including information in parentheses after the appropriate domain (e.g. targets, heterodimerization method, valency, and point mutations).


#### Pros


-


Suitable for use in conversations, databases, clinical trial registries, research and patent literature, as well as generic name to use in regulatory documents and INN application documents.


-


Suitable for standardizing antibody therapeutic format in new and historical data – to be used for analyses on trends and success rates and as inputs to machine learning algorithms.


-


Future proofed- suitable for annotating all current and currently conceivable multimeric antibody therapeutic formats.


-


Customizable: flexibility to include or omit information about specificity, valency, and point mutations.


-


Relays the structural information in a concise manner without the need for a diagram – important for improving accessibility (suitable as alternative text for schematics representing the structure of the antibody).


#### Cons


-


Format annotation rules for molecules without a multimerization centre not yet described (antibody fragments like scFv, diabodies, scFab, VHH, VH, VL and combinations of these).


-


Format annotation rules for Antibody Drug Conjugates not yet described.


Despite the growing complexity of therapeutic antibody formats, the scientific community has yet to adopt a universal system for their classification and naming. AbML and VERITAS represent two promising and complementary approaches to addressing this gap.


AbML offers a precise and detailed syntax ideal for annotating fine structural features and is associated with the abYdraw software for antibody diagrams generation, making it especially suited for regulatory submissions, patent filings, and antibody schematics generation. VERITAS, on the other hand, emphasizes human readability and inclusion of structural information in the name, which makes it better suited for scientific communication, data analysis, and accessibility. While VERITAS currently focuses on multimeric antibody formats, its framework could be expanded to include fragments and conjugates, completing its utility across the full range of therapeutic designs.


Rather than being competing systems, AbML and VERITAS can serve different, and often complementary, purposes. A combined or interoperable approach may offer the best of both: the technical rigor of AbML alongside the clear and concise structural information of VERITAS.


While naming frameworks continue to evolve, teams still need robust internal systems to consistently capture, organize, and interpret the growing diversity of antibody formats. As the industry continues to struggle with the lack of a universal antibody therapeutic nomenclature, Benchling is building the infrastructure that helps teams impose clarity and consistency on an increasingly complex space. Benchling Biologics unifies biologics discovery end-to-end, connecting sequence, functional, and predictive data so teams can design, register, and analyze diverse antibody formats with shared context. Today, tools like PipeBio and Benchling Research already streamline hit identification, in-silico design, and experimental capture within one connected design–make–test–learn (DMTL) loop. And with this year’s Biologics Registry, introducing an antibody-aware data model, custom formats, and intelligent registration tools, scientists will be able to register antibodies of any format, assemble ordered domains into chains, and capture domain-level metadata for richer analysis and AI-readiness, all using a consistent Benchling annotation framework. Together, these capabilities help standardize how teams describe, organize, and work with modern antibody therapeutics, even in the absence of a universal naming system.


## References


1.


**Antibodies to watch in 2023.** Kaplon H, Crescioli S, Chenoweth A, Visweswaraiah J, Reichert JM. MAbs. 2023 Jan-Dec;15(1 ):2153410. doi: 10.1080/19420862.2022.2153410.


2.


**Systematic analysis of the varied designs of 819 therapeutic antibodies and Fc fusion proteins assigned international nonproprietary names.** Wilkinson I, Hale G. MAbs. 2022 Jan-Dec;14(1 ):2123299. doi: 10.1080/19420862.2022.2123299.


3.


**Antibodies to watch in 2025.** Crescioli S, Kaplon H, Wang L, Visweswaraiah J, Kapoor V, Reichert JM. MAbs. 2025 Dec;17(1 ):2443538. doi: 10.1080/19420862.2024.2443538.


4.


**The making of bispecific antibodies.** Brinkmann U, Kontermann RE. MAbs. 2017 Feb/Mar;9(2 ):182-212. doi: 10.1080/19420862.2016.1268307.


5.


**The Antibody Society's antibody therapeutics database.** Rawat P, Crescioli S, Prabakaran R, Sharma D, Greiff V, Reichert JM. YAbS: MAbs. 2025 Dec;17(1 ):2468845. doi: 10.1080/19420862.2025.2468845.


6.


**Antibody markup language (AbML) - a notation language for antibody-based drug formats and software for creating and rendering AbML (abYdraw).** Sweet-Jones J, Ahmad M, Martin ACR. MAbs. 2022 Jan-Dec;14(1 ):2101183. doi: 10.1080/19420862.2022.2101183.


7.


**Pistoia Alliance. Hierarchical Editing Language for Macromolecules.** \[accessed on August 6, 2025\] https://www.pistoiaalliance.org/project/helm-project/


8.


**VERITAS: Harnessing the power of nomenclature in biologic discovery.** Biswas R, Belouski E, Graham K, Hortter M, Mock M, Tinberg CE, Russell AJ. MAbs. 2023 Jan-Dec;15(1 ):2207232. doi: 10.1080/19420862.2023.2207232.


‍9. **Figure references** a. Bispecific antibodies: from research to clinical application. Ma J, et al. Front Immunol. 2021. doi: 10.3389/fimmu.2021.626616.b. Bispecific antibodies: a mechanistic review of the pipeline. Labrijn AF, et al. Nat Rev Drug Discov. 2019. doi: 10.1038/s41573-019-0028-1.c. Principles and current clinical landscape of multispecific antibodies against cancer. Elshiaty M, et al. Int J Mol Sci. 2021. doi: 10.3390/ijms22115632.d. Alternative molecular formats and therapeutic applications for bispecific antibodies. Spiess CJ, et al. Mol Immunol. 2015. doi: 10.1016/j.molimm.2015.01.003.e. A robust heterodimeric Fc platform engineered for efficient development of bispecific antibodies of multiple formats. Moore GL, et al. Methods. 2019. doi: 10.1016/j.ymeth.2018.10.006.f. Development of human-machine language interfaces for the visual analysis of complex biologics and RNA modalities and associated experimental data. Kunz RK, et al. AAPS Open. 2023. doi: 10.1186/s41120-023-00073-w.37073302.g. Immunoglobulin Fc Heterodimer platform technology: from design to applications in Therapeutic antibodies and proteins. Ha JH, et al. Front Immunol. 2016. doi: 10.3389/fimmu.2016.00394.h. The making of bispecific antibodies. Brinkmann U, Kontermann RE. MAbs. 2017 Feb/Mar;9(2):182-212. doi: 10.1080/19420862.2016.1268307i. Dual targeting strategies with bispecific antibodies. Kontermann RE. MAbs. 2012. doi: 10.4161/mabs.4.2.19000.j. Bispecific antibodies. Kontermann RE, Brinkmann U. Drug Discov Today. 2015. doi: 10.1016/j.drudis.2015.02.008.k. Rapid Novor. The Landscape of Bispecific and Multispecific Antibodies. \[accessed on August 4, 2-25\] https://www.rapidnovor.com/bispecific-and-multispecific-antibodies/ l. Enhancing antibody Fc heterodimer formation through electrostatic steering effects: applications to bispecific molecules and monovalent IgG. Gunasekaran K, et al. J Biol Chem. 2010;285(25):19637–46. PMID: 20400508. doi:10.1074/jbc.M110.117382.


‍
