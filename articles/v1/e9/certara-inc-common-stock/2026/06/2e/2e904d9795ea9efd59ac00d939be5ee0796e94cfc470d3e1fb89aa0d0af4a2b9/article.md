---
schema_version: "1.0.0"
document_id: "2e904d9795ea9efd59ac00d939be5ee0796e94cfc470d3e1fb89aa0d0af4a2b9"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-6fa26b21f70d"
canonical_url: "https://www.certara.com/blog/certainty-2026-discovery-ugm-recap/"
published_at: "2026-06-15T14:36:09+00:00"
first_seen_at: "2026-07-22T21:45:01.422888+00:00"
fetched_at: "2026-07-28T21:44:22.050751+00:00"
content_hash: "sha256:eaecec6b861e4b1a45022e41c5e7b68ee550b6e0603dc115fa7e14348cdd6103"
---

# Data, models, and modalities: Key themes from the Certainty 2026 Discovery UGM

ShareShareShare


Csaba Peltz, PhD, MSc Director of Chemistry


Jun 15, 2026


*Boston, April 14–15, 2026*


Drug discovery has always involved a series of risky decisions. Before the first human dose, before the first animal study, before a compound even leaves the design stage, scientists are making bets: that a molecule will behave predictably in the body, that it will avoid the targets it should not hit, that it can actually be made. The Discovery UGM at Certainty 2026 was, in large part, a two-day conversation about how to make those decisions more informed, and to make them earlier in the discovery process. Across a dozen presentations over two days, representatives from pharma, biotech, and technology companies — alongside Certara’s own scientists and product teams – examined what it takes to get that science right.


The track theme, “ *Building confidence early through intelligent lead optimization* ” framed a question that runs through every drug discovery program: what does it take to get the science right from the start (or at least as early as possible)? Across the sessions, three interconnected answers emerged: (1) the data underpinning predictive models in early discovery is often not good enough, and fixing that is the prerequisite for everything else; (2) mechanistic biosimulation has matured to the point where it adds genuine value in discovery, not just in the clinic; and (3) the informatics infrastructure supporting modern drug discovery is under growing pressure from expanding drug modalities and AI-driven workflows. Across all three, a recurring question was what kind of AI or modeling approach is best suited to each stage of the process.


## The avoidome: naming the problem that consumes most of medicinal chemistry


The keynote of the Discovery track opened with a framing that proved useful throughout both days. Pat Walters of OpenADMET introduced the concept of the avoidome, a term coined by Fraser and Murckoi to describe the collective map of off-target proteins and biological pathways that drug candidates must navigate around to be safe and effective. Potency optimization, despite attracting much of the computational effort in drug discovery, is rarely the primary bottleneck. The persistent challenges, like CYP-mediated drug-drug interactions, hERG-related cardiotoxicity, solubility, permeability, consume the majority of medicinal chemistry resources, and they remain stubbornly difficult to predict well.


The reasons are partly structural. The AI and machine learning community in drug discovery has prioritized algorithms and molecular representations over the one thing models depend on: high-quality, consistently generated experimental data.


OpenADMET addresses this through a three-pillar framework: consistent data generation, high-resolution structural biology, and predictive machine learning. Its findings are disseminated through open benchmarking challenges and open-source code. The scientific infrastructure being built is as significant as any individual result it produces.


## Mechanistic biosimulation in discovery: the evidence is accumulating


A second major theme was the upstream migration of mechanistic modeling into the drug discovery phase. The physiologically based pharmacokinetic (PBPK) modeling and quantitative systems pharmacology (QSP) methods are well-established in late-stage development: PBPK has featured in more than 80% of FDA drug applications involving pharmacokinetic modeling between 2019 and 2023, and a Pfizer analysis across 42 active clinical programs quantified average savings of ten months and five million dollars per program.ii The sessions delivered by Adrian Stevens from Certara asked whether the same approaches could add value earlier, before candidate nomination.


The evidence presented suggests they can. Retrospective analyses from Genentech, AstraZeneca, and Sanofi, among others, demonstrated that PBPK-based in vitro to in vivo extrapolation using only calculated physicochemical properties and standard preclinical data can capture human PK profiles with sufficient accuracy to rank-order compounds and inform first-in-human dose predictions. An AstraZeneca retrospectiveiii across 29 clinical candidates found that translational PK/PD modeling predicted target engagement in humans within threefold for 83% of compounds – based entirely on non-clinical datasets. A Sanofi proof-of-conceptiv showed that full rat PK profiles could be predicted from molecular structure alone using machine learning, with accuracy broadly comparable to PBPK benchmarks. Taken together, these retrospectives point in a consistent direction: the predictive gap between preclinical and clinical is smaller than it has historically been treated, and these mechanistic models are the primary tool for bridging it.


The key focus should be defining what success means for discovery-stage biosimulation. The question should not be whether a model achieves a particular absolute accuracy, but whether it increases the probability of advancing the right compound and fills knowledge gaps that would otherwise drive poor decisions. Integrating PBPK, QSP, and early safety modeling directly into DMTA workflows alongside empirical assay data and AI-assisted property predictions is where the practical opportunity lies. Beyond the applicability itself, there is also the question of acceptance: how will decision-making processes and the general fabric of R&D change in a world where these simulations are considered routinely at the very earliest stages of discovery?


## Sharing predictions without sharing data: a precompetitive model that works


A third thread addressed a different resource constraint: the cost and scarcity of experimental data for endpoints that are expensive to generate, such as volume of distribution at a steady state. Limited training data limits model generalizability, and therefore the scope for applying predictions earlier in the design cycle, exactly where they are needed most.


Rajarshi Guha of Vertex Pharmaceuticals, co-chair of the IQ Consortium’s In Silico ADME Working Group, presented a precompetitive solution to this problem: the Student-Teacher model framework. Each participating company trains an internal teacher model on its own proprietary data and generates predictions for a shared public compound set: a diverse 133,000-compound subset drawn from ChEMBL. An honest broker, in this case the IQ Consortium, anonymizes and aggregates those predictions to train a single student model. No proprietary structures, assay data, or model architectures are exchanged at any point. The student model consistently outperformed every individual teacher model, and performance improved as more participants contributed. The approach is deliberately simple, low-cost, and model-agnostic.


## AI across the DMTA cycle: from generative design to agentic workflows


These first themes – data quality, mechanistic modeling, precompetitive collaboration – share a common feature: they are about doing the scientific groundwork that makes AI or modeling work trustworthy, rather than deploying AI for its own sake. Against that backdrop, the conference also examined where AI is actively accelerating discovery, and what the next generation of AI-driven workflows might look like.


At the level of the DMTA cycle itself, the integration of generative AI, combined with physics-based computational chemistry, and continuously updated ML predictive models is already delivering some results. Structure-based modeling has been around for quite a while now. Generative design tools, retrosynthetic AI, and real-time multi-parameter optimization scoring are all shortening the path from hit identification to candidate nomination. The picture that emerged is of AI as an amplifier of human expertise rather than an immediate replacement for it: the value comes from the quality of the scientific judgment embedded in the workflow, not from automation alone.


Looking further ahead, a session delivered by Nicholas Rioux from Labviva on agentic AI introduced a concept worth watching closely: intelligence density, defined as how much useful, domain-relevant capability a model delivers per unit of compute. The argument is that the future of AI in drug discovery is not a single large general-purpose model, but orchestrated crews of specialist agents. In regulated environments, this architecture has particular appeal: specialist agents produce full audit trails, map onto GxP and 21 CFR Part 11 compliance frameworks, and can be individually qualified. Building toward this model requires governance-first thinking, with AI literacy developed across scientific and operational teams before agents are deployed.


## Modalities, data foundations, and developer tooling


A final angle is the informatics layer that all the above depends on: the rise of peptide therapeutics (including the GLP-1 receptor agonist success and improved delivery technologies) and several other modalities necessitates changes in a (chem)informatics infrastructure built primarily for small molecules. Sessions covered work underway to close those gaps: new peptide sequence representation and structure-activity analysis capabilities in D360; the extension of Compound Registration to handle biologics at enterprise scale; and real-world case studies from Takeda and Cellarity in building connected data platforms from scratch.


## Takeaways


If there is a single conclusion based on both days, it is that algorithms can only go as far as the data allows. Whether the context is ADMET prediction, precompetitive PK modeling, or enterprise compound registration, the sessions returned repeatedly to data quality as the non-negotiable prerequisite. It is possible and necessary to improve it through deliberate investment in consistent data generation, open benchmarking, and cross-industry collaboration.


A second theme is that the boundaries between discovery and development are becoming more porous in the right direction. The retrospective evidence supporting mechanistic biosimulation in the DMTA cycle, and the growing suite of tools that make it practical, represent a genuine shift in how early-stage decision-making can be done.


The conference reinforced that the most durable competitive advantage in drug discovery informatics is not any individual tool or model, but the quality of the scientific and data infrastructure that sits underneath them. That infrastructure is what the field is actively building right now.


The data, the models, and the modalities are converging.


## Make scientifically grounded decisions, faster


By combining advanced models with your data and external sources, Chemaxon helps you predict what lies ahead — success or potential challenges — so every decision is better informed.


[Learn More About the Chemaxon portfolio](https://www.certara.com/chemaxon/)


## References


i Fraser, J.S. & Murcko, M.A., “Structure is beauty, but not always truth,” *Cell* , 187(3), 517–520, February 2024. DOI: 10.1016/j.cell.2024.01.003


ii Sahasrabudhe et al., *Clinical Pharmacology & Therapeutics* , 118(2), August 2025. DOI: 10.1002/cpt.3636


iii **AstraZeneca:** Jansson-Löfmark et al., *Drug Discovery Today* , 30(7), July 2025. DOI: 10.1016/j.drudis.2025.104417


iv **Sanofi:** Pillai et al., *Clinical and Translational Science* , 17(5), May 2024. DOI: 10.1111/cts.13824


## Author


[Csaba Peltz, PhD, MSc](https://www.certara.com/teams/csaba-peltz-phd-msc/) Director of Chemistry


Csaba spent 11 years in pharma R&D specializing in mass spectrometry and NMR spectroscopy. In 2012, he joined Chemaxon’s product development team, where he held various roles, including product owner, product manager, and product director, overseeing portfolio strategy. Recently, his focus has shifted toward scientific and market trends as Director of Chemistry. He holds an MSc in chemistry and computer science and earned a PhD in theoretical mass spectrometry.


## You May Also Like


AllDiscovery


[Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)


[Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)[Publication](https://www.certara.com/category/publication/)


### [Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)[Poster](https://www.certara.com/category/poster/)


### [A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)


[Certara Brings Chemaxon Cheminformatics Natively to Python with New Python API](https://www.certara.com/announcement/certara-brings-chemaxon-cheminformatics-natively-to-python-with-new-python-api/)


[Certara Brings Chemaxon Cheminformatics Natively to Python with New Python API](https://www.certara.com/announcement/certara-brings-chemaxon-cheminformatics-natively-to-python-with-new-python-api/)[Announcement](https://www.certara.com/category/announcement/)


### [Certara Brings Chemaxon Cheminformatics Natively to Python with New Python API](https://www.certara.com/announcement/certara-brings-chemaxon-cheminformatics-natively-to-python-with-new-python-api/)
