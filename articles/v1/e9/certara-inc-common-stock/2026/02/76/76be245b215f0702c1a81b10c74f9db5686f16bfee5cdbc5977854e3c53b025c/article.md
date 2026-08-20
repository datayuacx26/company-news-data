---
schema_version: "1.0.0"
document_id: "76be245b215f0702c1a81b10c74f9db5686f16bfee5cdbc5977854e3c53b025c"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-6fa26b21f70d"
canonical_url: "https://www.certara.com/blog/how-quantitative-systems-pharmacology-addresses-key-challenges-in-alzheimers-drug-development/"
published_at: "2026-02-02T20:10:50+00:00"
first_seen_at: "2026-07-30T08:43:47.512613+00:00"
fetched_at: "2026-07-30T08:43:49.249261+00:00"
content_hash: "sha256:1e229ed19613b2fe7c5057784a342adc17c2ecc1b7b7998b541217c0dbf4f2b2"
---

# How quantitative systems pharmacology addresses key challenges in Alzheimer’s drug development

ShareShareShare


Hugo Geerts, PhD Head of Neuroscience Modelling, QSP


February 2, 2026


The recent regulatory approval of anti-amyloid antibodies has led to an increased optimism in the Alzheimer’s community and rejuvenated interest from pharma companies. At the same time, recent clinical trial readouts of the anti-tau therapies bepranemab, zagotinemab, and ceperognastat, the GLP-1 agonist semaglutide, and the TREM2 modulator AL-002, illustrate the tremendous challenges in this field.


The interest in tau and neuroinflammation is driven by the clinical observations that even with complete elimination of amyloid plaques, the clinical benefit is still modest and the benefit-risk unclear, with the increased risk of Amyloid-Related Imaging Abnormalities (ARIA), a form of microbleeds that in rare cases can have severe consequences.


Several key challenges in Alzheimer’s disease drug development for successful clinical development can be identified. In this blog, we outline five major challenges in Alzheimer’s disease and describe how mechanistic QSP modeling can support more informed development decisions.


## Challenge 1: Target engagement of therapeutic modalities


Based on the success of amyloid antibodies, many therapeutic modalities for tau and inflammation pathways use similar biologics, often combined with a brain shuttle to enhance brain penetration. Because of the complex blood-brain-barrier system (BBB), concentrations reach between 0.1 and 0.3% of plasma levels. The addition of a shuttle increases this by about 4-to 8-fold.[Physiology-Based Pharmacokinetic Modeling (PBPK) can help identify the best combination of half-life and shuttle selection for optimal brain penetration](https://pmc.ncbi.nlm.nih.gov/articles/PMC8604880/) ​1​.


This works quite well for amyloid antibodies that can bind to extracellular amyloid and amplify their response by engaging with microglia-dependent clearance pathways of extracellular amyloid. In contrast, for tau antibodies, the challenge is to reach sufficient levels inside the extremely small (30 nm) and crowded synaptic cleft to capture the seed-competent tau molecule that is released from the presynaptic nerve ending. A seed-competent tau molecule is a specific, misfolded form of the tau protein, which can induce other normal tau proteins to misfold and aggregate, propagating the spread of neurodegenerative diseases like Alzheimer’s. Spatial Monte-Carlo simulation ​2,3​ based on a realistic geometric and biologically relevant model of the synaptic cleft, can help identify the minimal concentration needed to achieve a sufficient level of interaction between the tau antibody and its substrate.


As the disease progresses with increasing demyelination taking place, seed-competent tau might also enter along the axonal projections as well. A spatially resolved mechanistic QSP model is available that includes various processes such as uptake, lysosomal breakthrough, aggregation with monomeric tau into soluble tau oligomers and neurofibrillary tangles, degradation via the autosome-lysosome and the proteasome pathway, transport along microtubule and secretion at the next presynaptic compartment. This model is calibrated using image-based Tau SUVR PET (Standardized Uptake Value Ratio (SUVR) used in Positron Emission Tomography) imaging readout.


A better solution would be to tackle tau and neuroinflammation targets with small molecules that can achieve adequate brain target engagement. Indeed, tau spends almost all its time inside neuronal cells, and blocking aggregation or enhancing degradation can achieve better outcomes over the long term. Similarly, a number of small molecules have been approved for peripheral inflammation that could serve as a template for addressing microglia- or astrocyte-relevant pathways.


## Challenge 2: Combination trials


As anti-amyloid therapy becomes part of the standard-of-care, the question arises about possible synergy with tau or neuroinflammation therapeutic modalities. Both preclinical and clinical data strongly suggest that soluble amyloid oligomers increase neuronal firing and, therefore, secretion of misfolded tau protein. To address this question,[a computational neuroscience and biophysically realistic QSP model was developed that links changes in amyloid oligomers to neuronal firing and integrates with the mechanistic tau propagation model described above](https://pmc.ncbi.nlm.nih.gov/articles/PMC5797372/) ​4​. The model can be used to identify unexpected non-linear interactions and optimize timing, duration, and patient selection of amyloid-tau combination therapies.


In a certain sense, combination therapy is already well established, as many Alzheimer’s patients take CNS active medications, such as standard-of-care acetylcholinesterase inhibitors, antidepressants, benzodiazepines, antipsychotics, and anti-epileptics. As an example,[serotonin dynamics can affect cognitive readout even with the same amount of amyloid reduction, possibly explaining the difference between aducanumab’s ENGAGE and EMERGE trial](https://pmc.ncbi.nlm.nih.gov/articles/PMC7983876/) ​5​. Using the properties of neuronal firing, this platform allows simulation of the effect of these symptomatic medications on functional cognitive readouts, such as CDR-SOB or ADAS-Cog, at the individual patient level ​6​. CDR-SOB is the Clinical Dementia Rating – Sum of Boxes measurement, a clinical endpoint to measure cognitive changes in the early stages of Alzheimer’s disease. ADAS-Cog, or the Alzheimer’s Disease Assessment Scale-Cognitive Subscale, is a neuropsychological test considered a “gold standard” in Alzheimer’s clinical trials.


## Challenge 3: Biomarkers do not tell the whole story


The pharmacodynamic interactions between comedications and disease-modifying interventions described above can lead to a large variability in functional readout, especially in small Phase 2 studies with a limited number of patients ​7​. These studies often show a beneficial effect on biomarkers, but the clinical signal on cognitive readout is blurred by the different effects of comedications. Using the concept of virtual twins by investigating the pharmacodynamic effects at the individual patient level allows for a better estimation of the functional effect of a novel therapeutic modality, possibly rescuing promising therapeutic approaches.


## Challenge 4: Selecting the right targets


The increased use of Non-Animal Methods (NAM), such as human Induced Pluripotent Stem Cells, brain organoids, or brain-on-a-chip, allows for focusing on human disease-specific pathways, circumventing the limited translatability of preclinical animal models. While this can certainly identify more relevant human targets, these models do not address the complex anatomical connectivity in the human brain that ultimately drives behavior and functional outcome. A possible solution consists of implementing findings from these models into complex computational neuroscience circuits based on human brain connectomics. As an example, using multi-electrode array readouts of electrophysiological activity in human neurons with the Fronto-Temporal Dementia V337M Tau mutations, a model was developed to predict the impact of tau pathology on a clinically relevant functional scale​8​.


Learn more about how Certara’s Non-Animal Navigator™ uses a combination of expert strategy and AI-enabled biosimulation solutions to reduce animal use in drug development.


[Learn more](https://www.certara.com/solutions/non-animal-navigator/)


## Challenge 5: The new frontier – addressing neuroinflammation


There is an increasing consensus that, besides amyloid and tau pathology, aberrant neuroinflammation plays a major role in the progression of Alzheimer’s disease. While we have a better understanding of how to address peripheral inflammation and immune disorders, the role of microglia and astrocytes in driving aberrant neuroinflammation in the human Alzheimer’s brain is incomplete, not in the least because of the lack of good biomarkers and the dynamic nature of the cellular phenotypes. One way to integrate the increasing body of knowledge is to develop mathematical models of microglia and astrocyte pathways of amyloid and tau clearance, release of anti- and proinflammatory cytokines, proliferation, and survival. We have developed a detailed QSP model of the TREM2, TLR4 and TNFa-R pathway (RIPK1) in microglia based on preclinical data but constrained with available clinical data with the objective of developing actionable predictions on the impact of specific therapeutic interventions, either as a stand-alone or in combination with other interventions.


Alzheimer’s disease remains one of the most complex therapeutic areas in drug development. By integrating mechanistic biology, clinical data, and computational neuroscience, Quantitative Systems Pharmacology provides a powerful framework for addressing the major challenges in Alzheimer’s disease, from target engagement and combination therapy to biomarker variability and neuroinflammation.


Continue to read how to build a comprehensive strategy for Alzheimer’s drug development by reading this complementary post on[building a robust clinical pharmacology and model-informed drug development strategy](https://www.certara.com/blog/building-a-robust-clinical-pharmacology-model-informed-drug-development-strategy-for-new-alzheimers-disease-drugs/) where we highlight how QSP modeling can address key translational and target engagement challenges from discovery through early clinical research.


To learn more about Certara’s Alzherimer’s Disease QSP model,contact us below .


## FAQs


### How does Quantitative Systems Pharmacology (QSP) improve Alzheimer’s drug development?


QSP modeling improves Alzheimer’s drug development by integrating biological mechanisms, pharmacokinetics, and clinical data into predictive models that can simulate target engagement, disease progression, and cognitive outcomes. This approach helps address key challenges such as limited brain penetration, biomarker–clinical disconnects, and patient variability, enabling better target selection, dose optimization, and early clinical decision-making.


### Why are biomarkers alone insufficient for predicting clinical outcomes in Alzheimer’s disease trials?


Biomarkers in Alzheimer’s disease trials often capture specific pathological processes, such as amyloid or tau burden, but do not fully reflect downstream functional outcomes like cognition. Comedications, disease heterogeneity, and non-linear biological interactions can obscure clinical signals, especially in small Phase 2 studies. AI-enabled QSP and virtual twin approaches help bridge this gap by linking biomarker changes to individual-level cognitive endpoints such as ADAS-Cog and CDR-SOB.


## Author


[Hugo Geerts, PhD](https://www.certara.com/teams/hugo-geerts-phd/) Head of Neuroscience Modelling, QSP


In addition to 18 years of mechanism-based QSP modeling in Neurology and Psychiatry as co-founder of In Silico Biosciences, Hugo has 20 years of experience in drug discovery and development as a Research Fellow at the Janssen Research Foundation laboratoria in Beerse, Belgium. At Certara, he leads a new Certara QSP consortium focused on neurodegenerative diseases.


## References


1 Bloomingdale P, Bakshi S, Maass C, et al. Minimal brain PBPK model to support the preclinical and clinical development of antibody therapeutics for CNS diseases. *J Pharmacokinet Pharmacodyn* . 2021;48(6):861-871. doi:10.1007/s10928-021-09776-7


2 Goff J, Khalifa M, Short SM, van der Graaf PH, Geerts H. Interactions of Therapeutic Antibodies With Presynaptically-Released Misfolded Proteins in Neurodegenerative Diseases. A Spatial Monte-Carlo Simulation Study. *CPT Pharmacometrics Syst Pharmacol* . 2025;14(7):1168-1178. doi:10.1002/psp4.70035


3 Geerts H, Bergeler S, Walker M, van der Graaf PH, Courade JP. Analysis of clinical failure of anti-tau and anti-synuclein antibodies in neurodegeneration using a quantitative systems pharmacology model. *Sci Rep* . 2023;13(1):14342. doi:10.1038/s41598-023-41382-0


4 Geerts H, Spiros A, Roberts P. Impact of amyloid-beta changes on cognitive outcomes in Alzheimer’s disease: Analysis of clinical trials using a quantitative systems pharmacology model. *Alzheimers Res Ther* . 2018;10(1). doi:10.1186/s13195-018-0343-5


5 Geerts H, Spiros A. Learning from amyloid trials in Alzheimer’s disease. A virtual patient analysis using a quantitative systems pharmacology approach. *Alzheimer’s and Dementia* . 2020;16(6). doi:10.1002/alz.12082


6 Geerts H, Spiros A. Simulating the Effects of Common Comedications and Genotypes on Alzheimer’s Cognitive Trajectory Using a Quantitative Systems Pharmacology Approach. *J Alzheimers Di* s. 2020;78(1):413-424. doi:10.3233/JAD-200688


7 Nicholas T DSLCRTIPRCCRRPSAGH. Systems pharmacology modeling in neuroscience: Prediction and outcome of PF-04995274, a 5-HT4 partial agonist, in a clinical scopolamine impairment trial. *Adv Alzheimer Dis* . 2014;2(3):83-96.


8 Diaz KSACCKKHMGH. Using an ADAS-Cog calibrated QSP neuronal network model to explore the impact if Tau V337M mutation on action potential propagation. *Alzheimer&Dementia* . 2019;15:S34-S35.


## Learn more about Certara’s Alzherimer’s Disease QSP model


## You May Also Like


AllModeling and Simulation - QSP


[Designing Confident IO Therapies: A Collection of QSP Projects eBook](https://www.certara.com/ebook/designing-confident-io-therapies-a-collection-of-qsp-projects/)


[Designing Confident IO Therapies: A Collection of QSP Projects eBook](https://www.certara.com/ebook/designing-confident-io-therapies-a-collection-of-qsp-projects/)[eBook](https://www.certara.com/category/ebook/)


### [Designing Confident IO Therapies: A Collection of QSP Projects eBook](https://www.certara.com/ebook/designing-confident-io-therapies-a-collection-of-qsp-projects/)


[Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)


[Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)[Publication](https://www.certara.com/category/publication/)


### [Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)[Poster](https://www.certara.com/category/poster/)


### [A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)
