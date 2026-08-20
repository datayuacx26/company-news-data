---
schema_version: "1.0.0"
document_id: "cfe288a396df6cbdd13dbbdb3f12c74763ffed7aba4dcd3feb794acb5685de09"
company_key: "yc-ditto-biosciences"
company: "Ditto Biosciences"
source_id: "yc-ditto-biosciences-news-import-785de67d49e0"
canonical_url: "https://www.dittobio.com/blog/2026-03-02-mapping-parasite-molecules"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-26T08:12:12.958989+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:657abe56d8ccccecdaa3d82f579672b5481e6a547be0544d161b3f2cff27baf3"
---

# Mapping parasite molecules to treat autoimmune disease

*By Adair Borges, Dennis Sun, and Emily Weiss*


The world’s best drug developers live inside of you.


For millions of years, parasites have been evolving, optimizing, and stress-testing molecules in the most rigorous setting imaginable: the human body. In our first Ditto Bio blog post, we share an example of how we use parasite data and our MoleculeMapper platform to inform target selection and drive molecule discovery. We analyzed ~10,000 proteins from all known human-infecting viruses using our platform. We then used this data to improve target selection for autoimmune diseases and highlight a viral molecule that has invented an identical mechanism to control inflammation as a blockbuster autoimmune drug.


---


## We develop technologies for treating disease by harnessing parasite biology


Parasites like viruses, worms, ticks, and many more have been evolving with humans for millions of years. Their survival hinges on their ability to control the human immune system, which exists to destroy foreign, harmful organisms. We hypothesize that parasites produce molecules that by necessity have solved many of the major challenges of drug discovery and development:


-


Selecting targets that can powerfully modify human biology in the context of the entire human system


-


Designing molecules with the required mechanisms to control target activity


-


Simultaneously optimizing proteins to be soluble, stable, and low-immunogenicity


-


Engineering novel delivery mechanisms to transport biomolecules into the human body


This isn’t a new insight. For decades, scientists have posited that parasitic organisms may be the key to understanding how to control the human body. In fact, many of the foundational discoveries in human immunology have come from studying the organisms that have evolved to subvert and manipulate human immunity. Viruses themselves have even been used as drugs – adeno-associated viruses enable gene therapies like Zolgensma, and oncolytic viruses are the basis of anti-cancer medicines such as Imlygic.


What has changed is that over the last decade, decreasing sequencing technology costs have led to an explosion in genomic resources, from parasite genomes and transcriptomes to large-scale datasets of human health and disease, including GWAS, bulk tissue transcriptomes, and single-cell transcriptomes. With the development of AI systems like AlphaFold, which allow us to interrogate parasite-human protein-protein interactions at an incredible scale, we have a newfound opportunity to discover parasite molecular innovations in a way that was previously impossible. In this post, we demonstrate how viruses—the simplest parasites—can reveal clinically relevant drug targets and effective therapeutic molecules for autoimmune disease.


---


## Mapping 10k viral proteins to their targets with MoleculeMapper


We know that viruses produce proteins that modulate the human immune system, and we wanted to discover how these molecules map to high-priority immune targets at scale. Our hypothesis is that parasites like viruses have identified the targets that are best for controlling human immunity. Our goal is to identify those targets and the viral molecules that manipulate them.


We computationally mapped ~10,000 viral proteins to their human targets using our MoleculeMapper platform. After connecting each viral protein with its human target, we next wanted to understand the relevance of these targets to human autoimmune diseases. In this example, we used data from[Fang et al., 2019](https://www.nature.com/articles/s41588-019-0456-1) to generate an autoimmune disease target landscape. That study ranked genes as autoimmune targets using diverse data types, including genetic association with disease, genomics, and network proximity to disease drivers. We took that useful landscape and added the number of viral molecules predicted to hit each target, building a map of associations between viral molecules, their targets, and the diseases to which they are linked. At Ditto, we use maps like this to explore the applications of parasite molecules to different indications, highlight targets of interest, and surface promising cohorts of molecules to move forward to experiments(Figure 1) .


**Figure 1.** A UMAP of disease targets for 10 autoimmune diseases and their incidence of viral targeting. Candidate therapeutic targets for ten immune-mediated diseases were identified by integrating genetics-led target prioritization from[Fang et al., 2019](https://www.nature.com/articles/s41588-019-0456-1) with Ditto’s identification of human proteins that are targeted by viral molecules. Fang et al. integrated genomic evidence from GWAS, functional immunogenomics, ontology annotations, and protein interaction networks to rank > 15,000 candidate genes per disease. Only targets ranked in the top 10% for at least one disease are shown in this plot. Point position is determined by UMAP, where targets prioritized in similar diseases are placed closer together. Fill color indicates rank from Fang et al. within the selected disease or mean rank across all diseases (darker blue = higher priority target). When viewed by individual disease, only targets in the top 10% for that disease are colored; remaining targets are shown in grey. Point size reflects the number of unique viral molecules interacting with the target (log scale). Diseases highlighted in the dropdown: Vitiligo, Sjögren's disease, rheumatoid arthritis, psoriasis, ulcerative colitis, ANCA-associated vasculitis, multiple sclerosis, Crohn's disease, gout, systemic lupus erythematosus.


---


## Viruses pick winning targets


We next asked whether viral targeting frequency could help prioritize clinically successful immune targets. As a case study, we analyzed the top 100 genes ranked by Fang et al. as high-priority targets for rheumatoid arthritis. Of the 19 rheumatoid arthritis targets with FDA-approved drugs[(Open Targets)](https://platform.opentargets.org/) , 10 were included in this top 100 set.


We rescored these 100 targets, this time incorporating the frequency of viral targeting as an additional feature. We then evaluated how known, clinically validated targets performed under this revised ranking.


Clinically approved targets rose by an average of 38% when viral targeting data were included(Figure 2a) . For example, TNF (tumor necrosis factor)—a pro-inflammatory cytokine targeted by several blockbuster drugs—moved from rank #82 to #12, reflecting the large number of viral proteins that inhibit TNF signaling. We also identified novel targets that increased substantially in rank, suggesting they may represent potent, underexplored regulators of immune function(Figure 2b) .


**Figure 2.** Frequency of viral targeting predicts clinical success for rheumatoid arthritis targets. **A)** Targets with clinically approved drugs for rheumatoid arthritis when scored using human-only data from[Fang et al., 2019](https://www.nature.com/articles/s41588-019-0456-1) (grey) or with the frequency of viral targets (our data) included as a data type in target ranking (blue). **B)** Top 100 targets for rheumatoid arthritis from Fang et al., rescored by including Ditto’s viral data. Each arrow represents a target, with the bottom of the arrow starting at the target’s original rank (human only data from Fang et al.) to new rank (human data + frequency of viral targeting). Targets whose ranking improved with when frequency of viral targeting was included are shown in blue, and targets whose standing decreased are in red. Targets with clinically approved drugs have bolded arrows, and TNF is highlighted in gold.


---


## Parasite molecules have evolved clinically relevant drug mechanisms


We can use our platform to rapidly map parasite molecules to their targets and also surface targets likely to be clinically relevant. Traditional drug discovery focuses on identifying high-leverage targets that can modulate disease and then on finding drugs for those targets. An exciting aspect of our approach is that when we use our platform to identify an interesting target, we also simultaneously identify molecules that have evolved to modulate the activity of that target. As an example of how our approach can do this successfully with a known target and drug pairing, let’s look into the viral molecules that have evolved to bind and control TNF.


TNF is an incredibly potent proinflammatory cytokine, and molecules that block TNF activity were the first approved biologics. TNF inhibitors are used in rheumatoid arthritis, Crohn’s disease, and many other autoimmune diseases and have grossed billions in lifetime sales.


Remarkably, one of these blockbuster biologics, Etanercept (aka Enbrel), uses the exact same mechanism of action as viral TNF inhibitors(Figure 3) but was created using traditional drug development approaches. Viral TNF inhibitors are well studied, and these proteins have been shown to bind TNF with picomolar (drug-like) affinities[(Alejo et al., 2006)](https://pmc.ncbi.nlm.nih.gov/articles/PMC1458686/) . Our platform rediscovers these known viral TNF inhibitors, as well as many other uncharacterized viral proteins against other targets.


**Figure 3.** Etanercept and viral TNF inhibitors use the same mechanism. **A)** Structural prediction of Enbrel TNF-binding domain (orange) bound to TNF (green). **B)** Structural prediction of viral TNF inhibitor AXN74999.1 (blue) bound to TNF (green). **C)** Alignment of a structural prediction of Enbrel TNF-binding domain (orange) and a structural prediction of poxviral TNF inhibitor AXN74999.1 (blue).


---


## Looking forward


We are incredibly excited about the potential to use parasites to unlock grand challenges in drug discovery and development. With a single computational search using our MoleculeMapper, we surface exciting targets more effectively than approaches that use human-only data, and we discover molecules with therapeutic potential that equals that of blockbuster drugs on the market today. So far at Ditto, we’ve analyzed three very different classes of parasites — viruses, ticks, and helminth worms — and each is a genuine goldmine of new therapeutic biology. We look forward to sharing more in our future posts!


Please reach out to learn more about our platform athello@dittobio.com .
