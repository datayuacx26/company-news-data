---
schema_version: "1.0.0"
document_id: "81608b9984d744ef512a3aa736615e71a317ae6c43f6696eb6933c69b200bd3d"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-6fa26b21f70d"
canonical_url: "https://www.certara.com/blog/qsp-modeling-for-parkinsons-disease-solving-the-synthetic-control-challenge-in-clinical-trials/"
published_at: "2026-06-11T14:53:42+00:00"
first_seen_at: "2026-07-22T21:45:01.422888+00:00"
fetched_at: "2026-07-28T21:44:22.050751+00:00"
content_hash: "sha256:21cd0e4966c9340e7b793f18e97a921f7db9d5600ec6a94826975e018ac0957e"
---

# QSP Modeling for Parkinson’s Disease: Solving the Synthetic Control Challenge in Clinical Trials

ShareShareShare


Hugo Geerts, PhD Head of Neuroscience Modelling, QSP


June 11, 2026


Parkinson’s disease (PD) is the fastest-growing neurodegenerative disease, second only to dementia in prevalence. Its motor symptoms can be readily managed with standard-of-care dopaminergic therapy, such as L-Dopa or dopamine agonists. Yet the search for *disease-modifying* treatments has met with mixed success, and the reasons say a great deal about why **QSP modeling for Parkinson’s disease** has become such a valuable tool for designing, measuring, and interpreting trials.


Recent programs illustrate the point. Roche’s prasinezumab, an anti–alpha-synuclein antibody, did not reach statistical significance on its primary endpoint, but it did demonstrate a robust clinical signal in a subset of patients on specific comedications (Pagano et al., 2024); a new trial is now underway based on those findings. In parallel, the ongoing discussion around uniQure’s AMT-130 gene therapy for Huntington’s disease (Mwape et al., 2026), another neurodegenerative disease with a strong motor component, underscores the challenges companies face when they must rely on single-arm trials.


These programs share a common difficulty: in PD, demonstrating the benefit of a disease-modifying therapy is exceptionally hard. Below, we examine four challenges that complicate PD clinical development, and how quantitative systems pharmacology (QSP) modeling, delivered through a mechanism-based Virtual Twin® approach, can help sponsors detect a true functional signal and make better go/no-go decisions.


## Challenge 1: Optimized standard-of-care makes clinical signals hard to detect


The gold standard of care for PD is L-Dopa together with various dopamine agonists. Because these regimens can be optimized to deliver maximal symptom relief, detecting an incremental functional signal from a disease-modifying agent in an augmentation trial becomes a tall order. In the absence of a validated surrogate biomarker for PD pathology, sponsors must rely on clinical readouts alone to prove benefit, a high bar to clear.


Physicians also retain the prerogative to adjust standard-of-care during a trial to accommodate changes in their patients’ symptoms. Although these modifications can be well documented, they add considerable variability that further complicates the interpretation of clinical benefit.


## Challenge 2: A strong placebo response confounds the readout


A defining feature of PD clinical trials is the strong placebo response, driven by dopamine’s tight association with the brain’s reward circuitry. This response is even greater for more invasive interventions, such as gene and stem cell therapies, because patients are more deeply invested in the outcome.


These studies often face the added difficulty of enrolling a placebo-control arm. In small, single-arm Phase 2a studies, active-treatment outcomes are frequently compared against a synthetic control built from a historical observational study. But such longitudinal control lacks the strong placebo response that comes with the knowledge of being enrolled in a clinical trial—biasing the comparison.


## Talk through your PD trial with a QSP specialist


See how Certara’s expertise in QSP modeling for neurodegenerative indications can de-risk and support your PD program.


[Connect with our neuroscience QSP experts](https://www.certara.com/qsp-modeling-platforms-for-neuroscience/#Contact-us-CTA)


## Challenge 3: There is no validated biomarker for alpha-synuclein pathology


Unlike Alzheimer’s disease, where well-validated fluid and imaging biomarkers exist for amyloid and tau pathology, PD currently lacks reliable surrogate biomarkers. Efforts are underway to validate seed amplification assays (SAA) and to develop imaging biomarkers for alpha-synuclein, the core deposit in Lewy bodies, but functional clinical outcomes such as the Unified Parkinson’s Disease Rating Scale (UPDRS) remain the gold standard.


Regulatory agencies are increasingly open to more patient-centric readouts—for example, daily Good ON-time, the duration over which symptoms are well controlled, potentially captured through wearables (Rose et al., 2022). However, ON-time is largely driven by the pharmacokinetics and pharmacodynamics of L-Dopa formulations and dopamine agonists, introducing high variability between individual patient readouts.


## Challenge 4: Motor symptoms range from rigidity to dyskinesia


Long-term L-Dopa treatment can trigger the onset of troublesome dyskinesia—the jittery, involuntary movement known as L-Dopa–induced dyskinesia (LID)—which affects both Good ON-time and the patient experience. LID is often managed with the NMDA modulator amantadine, which can further complicate the pharmacodynamic interaction with an investigational therapeutic.


## The solution: QSP modeling for Parkinson’s disease with individualized Virtual Twin® controls


Certara offers a mechanism-based Virtual Twin® Quantitative Systems Pharmacology (QSP) model that serves as an individualized synthetic control for each real patient on their actual treatment. The model is built on the anatomical and functional connections of the cortico-striatal-thalamocortical motor circuit (Roberts et al., 2016). Informed by insights from deep-brain recording studies, its readout is extensively calibrated against clinical data for both UPDRS and Good ON-time.


Each Virtual Twin® carries the same baseline standard-of-care comedications and disease state as its real-world counterpart in the trial. Its clinical trajectory on motor symptoms, whether UPDRS or OFF-time, is then simulated in an untreated condition, with the placebo effect included. This enables an evaluation of clinical effect on a per-patient basis, significantly reducing the variability inherent in group averages. In effect, the approach delivers the statistical power of a crossover study, where each patient serves as their own placebo control, within the practicality of a traditional parallel-group design.


The result: sponsors can identify a real functional clinical signal in single-arm trials and small proof-of-concept Phase 2 studies for disease-modifying therapies—building confidence in the therapeutic molecule and supporting optimal clinical trial design.


## Ready to de-risk your next neurodegenerative disease program?


Connect with Certara’s neuroscience QSP team to learn how Virtual Twin® technology can strengthen your trial design and accelerate confident decision-making.


References


Mwape, C., Qureshi, A. A., Saeed, M. Z., Fatima, A., Jamil, A., Mahmood, H., Batool, A., & Khan, A. M. (2026). AMT-130 gene therapy: a promising disease-modifying approach for Huntington’s disease. *Annals of Medicine and Surgery* , 88(1), 1144–1145.[https://doi.org/10.1097/MS9.0000000000004574](https://doi.org/10.1097/MS9.0000000000004574)


Pagano, G., Taylor, K. I., Anzures-Cabrera, J., Simuni, T., Marek, K., Postuma, R. B., Pavese, N., Stocchi, F., Brockmann, K., Svoboda, H., Trundell, D., Monnet, A., Doody, R., Fontoura, P., Kerchner, G. A., Brundin, P., Nikolcheva, T., Bonni, A., PASADENA Investigators, & Prasinezumab Study Group. (2024). Prasinezumab slows motor progression in rapidly progressing early-stage Parkinson’s disease. *Nature Medicine* , 30(4), 1096–1103.[https://doi.org/10.1038/s41591-024-02886-y](https://doi.org/10.1038/s41591-024-02886-y)


Roberts, P., Spiros, A., & Geerts, H. (2016). A humanized clinically calibrated quantitative systems pharmacology model for hypokinetic motor symptoms in Parkinson’s disease. *Frontiers in Pharmacology* , 7, 6.[https://doi.org/10.3389/fphar.2016.00006](https://doi.org/10.3389/fphar.2016.00006)


Rose, R., Mitchell, E., Van Der Graaf, P., Takaichi, D., Hosogi, J., & Geerts, H. (2022). A quantitative systems pharmacology model for simulating OFF-time in augmentation trials for Parkinson’s disease: application to preladenant. *Journal of Pharmacokinetics and Pharmacodynamics* , 49(6).[https://doi.org/10.1007/s10928-022-09825-9](https://doi.org/10.1007/s10928-022-09825-9)


## Author


[Hugo Geerts, PhD](https://www.certara.com/teams/hugo-geerts-phd/) Head of Neuroscience Modelling, QSP


In addition to 18 years of mechanism-based QSP modeling in Neurology and Psychiatry as co-founder of In Silico Biosciences, Hugo has 20 years of experience in drug discovery and development as a Research Fellow at the Janssen Research Foundation laboratoria in Beerse, Belgium. At Certara, he leads a new Certara QSP consortium focused on neurodegenerative diseases.


## FAQs


### Why is it so hard to show a disease-modifying benefit in Parkinson's trials?


Standard-of-care therapy is already optimized for symptom relief, so detecting an added functional signal is difficult. On top of that, there’s a strong placebo response, no validated biomarker for the underlying pathology, and wide variability in motor symptoms, all of which make a true clinical effect harder to read.


### What is a synthetic control arm, and why does the placebo response complicate it?


A synthetic control is a comparison group built from data rather than a concurrent placebo arm, often used in small single-arm Phase 2 studies. The problem in Parkinson’s is that controls drawn from historical observational data lack the strong placebo response that comes with knowing you’re in a trial, which biases the comparison.


### How does a Virtual Twin® serve as a control for each patient?


Each Virtual Twin® mirrors a real patient’s baseline disease state and standard-of-care comedications, then simulates their untreated trajectory with the placebo effect included. Comparing the patient against their own twin evaluates effect on a per-patient basis, giving the statistical power of a cross-over study within a standard parallel-group design.


### Why is there no validated biomarker for Parkinson's, and what's used instead?


Unlike Alzheimer’s, Parkinson’s currently lacks reliable fluid or imaging biomarkers for alpha-synuclein pathology, though seed amplification assays and imaging approaches are in development. Until those mature, functional clinical outcomes like the UPDRS and Good ON-time remain the gold standard.


## Speak to a QSP expert


Discover how Certara’s QSP modeling platforms for neuroscience can accelerate your therapies.


Tailored solutions for Alzheimer’s, Parkinson’s, and other neuro indications


Advanced simulation tools for patient-centric outcomes


Expertise in addressing complex neurodegenerative challenges


## You May Also Like


AllCentral Nervous System (CNS)


[A Quantitative Systems Pharmacology Comparison Of Tau-targeted Therapeutic Strategies In Alzheimer’s Disease](https://www.certara.com/poster/a-quantitative-systems-pharmacology-comparison-of-tau-targeted-therapeutic-strategies-in-alzheimers-disease/)


[A Quantitative Systems Pharmacology Comparison Of Tau-targeted Therapeutic Strategies In Alzheimer’s Disease](https://www.certara.com/poster/a-quantitative-systems-pharmacology-comparison-of-tau-targeted-therapeutic-strategies-in-alzheimers-disease/)[Poster](https://www.certara.com/category/poster/)


### [A Quantitative Systems Pharmacology Comparison Of Tau-targeted Therapeutic Strategies In Alzheimer’s Disease](https://www.certara.com/poster/a-quantitative-systems-pharmacology-comparison-of-tau-targeted-therapeutic-strategies-in-alzheimers-disease/)


[Oxford Academic, Neuro-Oncology Pediatrics – BRAF pub: Growth recovery in patients with BRAF-altered pediatric low-grade glioma following treatment with tovorafenib](https://www.certara.com/publication/growth-recovery-in-patients-with-braf-altered-pediatric-low-grade-glioma-following-treatment-with-tovorafenib/)


[Oxford Academic, Neuro-Oncology Pediatrics – BRAF pub: Growth recovery in patients with BRAF-altered pediatric low-grade glioma following treatment with tovorafenib](https://www.certara.com/publication/growth-recovery-in-patients-with-braf-altered-pediatric-low-grade-glioma-following-treatment-with-tovorafenib/)[Publication](https://www.certara.com/category/publication/)


### [Oxford Academic, Neuro-Oncology Pediatrics – BRAF pub: Growth recovery in patients with BRAF-altered pediatric low-grade glioma following treatment with tovorafenib](https://www.certara.com/publication/growth-recovery-in-patients-with-braf-altered-pediatric-low-grade-glioma-following-treatment-with-tovorafenib/)


[How quantitative systems pharmacology addresses key challenges in Alzheimer’s drug development](https://www.certara.com/blog/how-quantitative-systems-pharmacology-addresses-key-challenges-in-alzheimers-drug-development/)


[How quantitative systems pharmacology addresses key challenges in Alzheimer’s drug development](https://www.certara.com/blog/how-quantitative-systems-pharmacology-addresses-key-challenges-in-alzheimers-drug-development/)[Blog](https://www.certara.com/category/blog/)


### [How quantitative systems pharmacology addresses key challenges in Alzheimer’s drug development](https://www.certara.com/blog/how-quantitative-systems-pharmacology-addresses-key-challenges-in-alzheimers-drug-development/)
