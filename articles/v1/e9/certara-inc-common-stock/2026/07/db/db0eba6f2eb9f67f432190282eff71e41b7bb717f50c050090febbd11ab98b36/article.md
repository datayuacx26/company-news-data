---
schema_version: "1.0.0"
document_id: "db0eba6f2eb9f67f432190282eff71e41b7bb717f50c050090febbd11ab98b36"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-6fa26b21f70d"
canonical_url: "https://www.certara.com/blog/dosimetry-for-radiopharmaceutical-therapy-from-measurement-to-dose-justification/"
published_at: "2026-07-07T21:22:36+00:00"
first_seen_at: "2026-07-22T21:45:01.422888+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:e2c8dfb462b7305a6830aa342eeb9d15ddce710b07e6f02c239d8a56d1e03cdc"
---

# Dosimetry for radiopharmaceutical therapy: From measurement to dose justification

ShareShareShare


Hunter Stephens, PhD Associate Director, PharmacometricsJoshuaine Grant Senior Director, Quantitative Systems Pharmacology (QSP)Amandine Manon, PharmD Senior Director, Clinical Pharmacology and Translational MedicineMirjam Trame, PharmD, PhD Vice President, Certara Drug Development Solutions, Head of Pharmacometrics USA – Division II


July 7, 2026


Measuring absorbed dose is essential. But it cannot tell you whether a dosing regimen is optimal, whether an organ limit is biologically relevant, or whether the same regimen will perform consistently across patients.


Those questions sit at the heart of dose optimization in radiopharmaceutical therapy, and increasingly at the center of regulatory expectations. They are also why dosimetry for radiopharmaceutical therapy is now best understood not as a single measurement, but as one input into a broader quantitative evidence framework.


One welcome consequence of the FDA’s 2025 dosage optimization guidance is that dosimetry is no longer optional. Phase 1 studies of therapeutic radiopharmaceuticals are expected to generate the data needed to characterize absorbed dose and support dose optimization.1 As a result, more programs are advancing through development with meaningful dosimetry data in hand.


That progress is important. However, the availability of dosimetry data does not automatically translate into better dose-selection decisions.


For many years, radiopharmaceutical development has relied heavily on External Beam Radiation Therapy (EBRT) organ tolerances to guide escalation and assess safety. While absorbed dose measurements remain foundational, they cannot independently answer some of the most important questions facing development teams. Does a given tumor absorbed dose produce a clinically meaningful biological effect? What organ absorbed dose triggers late radiation induced adverse events? Are historical organ EBRT tolerances relevant for a specific radiopharmaceutical? How will exposure vary across patients, treatment cycles, and disease states? And ultimately, how can a selected dosing regimen be justified to regulators?


Addressing these questions requires moving beyond dosimetry as a measurement and toward dosimetry as part of a broader quantitative evidence framework.


## An absorbed dose is not a biological effect


Absorbed dose quantifies the cumulative amount of energy deposited in tissue. It does not directly measure the biological consequences of that exposure.


The same absorbed dose can produce very different biological effects depending on how rapidly the radiation is delivered. Because tissue repair occurs during prolonged exposure, the biological impact of a radioligand administered over days may differ substantially from that of external beam radiation delivered over minutes.


Radiobiology accounts for these differences through the concept of Biologically Effective Dose (BED), which incorporates dose rate, tissue repair kinetics, and tissue-specific radiobiological characteristics. As a result, identical absorbed-dose values do not necessarily correspond to identical biological outcomes.


This distinction has practical implications for dose optimization. In a population PK analysis of a ¹⁷⁷Lu-PSMA radioligand, our team evaluated BED across a range of injected activities and compared those values with the BED associated with an equivalent external-beam radiotherapy (EBRT) fraction. While the liver’s BED values were largely comparable due to the high dose rate, substantial differences emerged in the kidneys and salivary glands, who had substantially smaller dose rates, across all activity levels examined (p ≤ 0.0001). Absorbed doses also demonstrated evidence of saturation as injected activity increased, providing insight into the point at which further escalation may offer diminishing returns.


These findings illustrate that absorbed dose alone may not fully characterize the biological consequences of treatment. Understanding those consequences requires integrating radiobiology into the dose-selection process.


*Conceptual illustration. For equal absorbed dose, a protracted low-dose-rate radioligand exposure yields a lower Biologically Effective Dose (BED) than an acute external-beam fraction — so a constraint expressed in absorbed dose does not translate between the two settings.*


## The organ limit you inherited may not be your organ limit


Many of the dose constraints commonly used in radioligand therapy originated in EBRT, where they are supported by decades of clinical experience and validation.² The kidney threshold of 23 Gy is perhaps the most familiar example and has often been treated as a practical upper boundary for escalation.


However, EBRT constraints were established under radiobiological conditions that differ substantially from those encountered in radiopharmaceutical therapy. External Beam Radiation Therapy typically delivers dose rates that are orders of magnitude higher than those associated with radioligands. Consequently, tissue responses observed in one setting cannot necessarily be assumed to apply directly to the other.


Recognition of this issue has grown throughout the field. A 2024 editorial in the *Journal of Nuclear Medicine* highlighted the challenges associated with extrapolating EBRT constraints to radionuclide therapies.³ More recently, the FDA’s 2025 dosage optimization guidance acknowledged that previously accepted EBRT-derived limits may be exceeded when supported by appropriate scientific and clinical evidence.¹


The implication is not that historical constraints are inherently too conservative or too permissive. Rather, it is that they should not be accepted uncritically. Organ-specific limits should reflect the biology, isotope, dose rate, and treatment regimen being studied.


Determining those limits requires a quantitative framework that integrates radiation dosimetry, radiobiology, and exposure-response relationships. Such analyses may reveal that a traditional constraint unnecessarily restricts therapeutic activity, or conversely, that it provides less protection than originally assumed. In either case, the goal is not to challenge established limits for the sake of escalation, but to identify the most scientifically appropriate limit for the therapy under development.


### Move Beyond Dosimetry


Measuring absorbed dose is only the first step. Discover how Certara helps radiopharmaceutical sponsors integrate dosimetry, radiobiology, pharmacometrics, and model-informed drug development to support confident dose selection and regulatory decision-making.


→[Explore our Targeted Radionuclide Therapy & Theranostics solutions](https://www.certara.com/certara-complex-biologics/targeted-radionuclide-therapy-trt-and-theranostics/)


## For α-emitters, even the absorbed dose is a model output


Alpha-emitting radiopharmaceuticals introduce additional complexity because absorbed-dose estimates themselves can be highly dependent on modeling assumptions.


Following decay of ²²⁵Ac, daughter radionuclides may detach from the parent molecule and redistribute before undergoing subsequent decay.⁴ The extent and pattern of this redistribution can substantially influence where radiation is ultimately delivered and which tissues receive the greatest burden.


In our analyses of a particular scenario, alternative assumptions regarding daughter radionuclide behavior produced meaningful differences in estimated absorbed dose to blood and liver, with smaller but still notable differences observed in kidneys and salivary glands. These findings illustrate the sensitivity of dosimetry calculations to assumptions that may not be directly observable in clinical studies.


The challenge is compounded by the practical difficulties associated with imaging alpha emitters. Low administered activities often limit direct quantitative imaging, prompting the use of surrogate imaging agents and serial blood sampling to support dosimetry assessments.⁵ As a result, absorbed-dose estimates frequently depend on a combination of measured data and scientifically justified assumptions regarding pharmacokinetics, imaging surrogates, and daughter radionuclide fate.


For alpha-emitting therapies, absorbed dose is therefore not simply a measured quantity. It is the output of a quantitative framework whose assumptions must be explicitly stated, evaluated, and defended.


### From dosimetry to dose optimization


Dosimetry data is inherently sparse. Most studies collect a limited number of imaging time points from patients who differ substantially in tumor burden, target expression, organ function, and disease characteristics. This is a defining challenge of dosimetry for radiopharmaceutical therapy: the measurements are precious, but on their own they are incomplete.


Population PK approaches provide a means of translating these fragmented observations into a more comprehensive understanding of exposure. By leveraging information across patients, population PK modeling can estimate exposure from limited sampling schedules, quantify between-patient variability, evaluate alternative imaging strategies, and characterize cumulative exposure across repeated treatment cycles.


More importantly, population modeling creates a bridge between dosimetry and exposure-response. Rather than simply describing where radiation was delivered, these approaches help quantify how exposure relates to efficacy, safety, and treatment outcomes across diverse patient populations.


This distinction becomes increasingly important as programs advance from early clinical development into dose optimization, registration, and ultimately clinical practice. Regulators are increasingly interested not only in where radiation is distributed, but also in whether the selected dose and regimen are supported by evidence demonstrating an appropriate balance of benefit and risk.


### From a measurement to a justification


The radiopharmaceutical field has made tremendous progress in its ability to measure radiation exposure. Dosimetry for radiopharmaceuticals is now more accurate and more routine than ever, which has strengthened development programs and improved confidence in dosimetry assessments. However, the next challenge extends beyond measurement.


Dose optimization ultimately depends on understanding how radiation exposure translates into biological effect across tissues, patients, and treatment cycles. Absorbed dose remains an essential input, but it is only one component of the evidence needed to support development decisions.


As regulatory expectations continue to evolve, particularly under the principles outlined in Project Optimus, sponsors will increasingly be expected to demonstrate not only how much radiation was delivered, but why a particular dose and regimen represent the most appropriate choice for patients.


Meeting that standard requires integrating dosimetry for radiopharmaceuticals with radiobiology, population PK, BED, and exposure-response analyses into a coherent scientific framework. The opportunity is not simply to generate more dosimetry data, but to extract more insight from the data already being collected.


For radiopharmaceutical developers, that distinction may prove increasingly important. Programs that can translate dosimetry into a robust, evidence-based justification for dose selection will be better positioned to optimize benefit-risk, support regulatory interactions, and ultimately improve outcomes for patients.


*This is the technical companion to our piece on the Project Optimus standard of evidence for radiopharmaceuticals. More on Certara’s model-informed approach to TRT and theranostics:*[Radiopharmaceutical Development for TRT and Theranostics](https://www.certara.com/certara-complex-biologics/targeted-radionuclide-therapy-trt-and-theranostics/) *.*


## Advance Dose Optimization with Model-Informed Approaches


Successful radiopharmaceutical development requires more than measuring absorbed dose. Discover how Certara helps sponsors integrate dosimetry, radiobiology, pharmacometrics, and exposure-response modeling to support dose optimization and regulatory decision-making.


[Learn how we support radiopharmaceutical dose optimization](https://www.certara.com/certara-complex-biologics/targeted-radionuclide-therapy-trt-and-theranostics/)


## Sources


1. FDA. Oncology Therapeutic Radiopharmaceuticals: Dosage Optimization During Clinical Development. Draft Guidance for Industry, August 2025.


2. Dose-volume constraints for organs at risk in radioligand therapy were largely derived from external beam radiotherapy; see EBRT-derived-constraint literature in radionuclide therapy.


3. Hope TA, Hofman MS, et al. Rethinking Dosimetry: The Perils of Extrapolated EBRT Constraints to Radionuclide Therapy (editorial), J Nucl Med (2024).


4. Liubchenko G, et al. Image-based dosimetry for \[225Ac\]Ac-PSMA-I&T therapy and the effect of daughter-specific pharmacokinetics. Eur J Nucl Med Mol Imaging (2024). doi:10.1007/s00259-024-06681-2.


5. Response to the FDA Dosage Optimization Draft Guidance for Radiopharmaceutical Therapies, J Nucl Med (2026) — on α-emitter imaging challenges, surrogate imaging agents, and serial blood sampling for dosimetry.


—. Source poster (Certara, SNMMI): “Population Pharmacokinetic Modeling to Predict Absorbed and Biologically Effective Dose for Radioligand Therapies” (H. Stephens, A. Manon, M. N. Trame).


## Authors


[Hunter Stephens, PhD](https://www.certara.com/teams/hunter-stephens-phd/) Associate Director, Pharmacometrics


Hunter Stephens, PhD, is an Associate Director in the Pharmacometrics group at Certara. He specializes in applying mathematical and computational models to understand the pharmacokinetics and pharmacodynamics of drugs, especially radiopharmaceuticals. He has a PhD in Medical Physics from Duke University. He also holds an MS in Physics from North Carolina State University and a BS in Mathematics from Tennessee Tech University. His work in TRT has focused on building semi-mechanistic population PK models to simulate and predict absorbed and biologically effective doses to inform dose-range finding and questions of safety and efficacy. In addition, he has extensive experience in radiation dosimetry from external and internal sources.


[Joshuaine Grant](https://www.certara.com/teams/joshuaine-grant/) Senior Director, Quantitative Systems Pharmacology (QSP)


Joshuaine Grant is a Senior Director in Quantitative Systems Pharmacology at Certara with more than 25 years of experience integrating biophysics, disease biology, and quantitative modeling to advance drug development. She leads collaborative projects that use mechanistic and translational QSP modeling to inform key decisions from discovery through the clinic. Her broad experience across biologics and complex therapeutics has supported Certara’s growing leadership in radioligand and targeted radiotherapies.


[Amandine Manon, PharmD](https://www.certara.com/teams/amandine-manon/) Senior Director, Clinical Pharmacology and Translational Medicine


Amandine joined Certara in 2020. She served as a Clinical pharmacologist in several pharmaceutical companies for 15 years. She has a proven track record in preclinical and clinical PK, clinical pharmacology with a special focus on oncology, drug development from early stages to Phase 3, and regulatory experience. Amandine graduated as a PharmD from Paris University, France and she also holds a Master’s degree in Pharmacokinetics.


[Mirjam Trame, PharmD, PhD](https://www.certara.com/teams/mirjam-trame/) Vice President, Certara Drug Development Solutions, Head of Pharmacometrics USA – Division II


Mirjam is an expert in pharmacometrics and oncology drug development, serving as Head of Pharmacometrics USA – Division II at Certara Drug Development Solutions. With expertise spanning complex biologics, she supports exposure-response analysis, dose and study optimization, and regulatory strategy, with a special focus on radiotherapeutics and cell and gene therapies.


## Advance your radiopharmaceutical development program with confidence


Whether defining your first-in-human dose, evaluating biodistribution and dosimetry, optimizing treatment strategy, or preparing for regulatory interactions, Certara helps targeted radionuclide therapy (TRT) and theranostics teams reduce uncertainty and make confident, model-informed development decisions.
We help teams


Support translational dose selection and escalation strategies


Optimize dose, regimen, fractionation, and therapeutic window


Characterize biodistribution, dosimetry, and exposure–response relationships


Apply PBPK, QSP, and pharmacometrics modeling to targeted radiopharmaceutical development


Build submission-ready evidence aligned with evolving FDA and EMA expectations


## You May Also Like


AllModel Informed Drug Development


[Assessing Cross-Version Simcyp Simulator Prediction Performance Using a Bioequivalence Framework to Support EMA Qualification](https://www.certara.com/poster/assessing-cross-version-simcyp-simulator-prediction-performance-using-a-bioequivalence-framework-to-support-ema-qualification/)


[Assessing Cross-Version Simcyp Simulator Prediction Performance Using a Bioequivalence Framework to Support EMA Qualification](https://www.certara.com/poster/assessing-cross-version-simcyp-simulator-prediction-performance-using-a-bioequivalence-framework-to-support-ema-qualification/)[Poster](https://www.certara.com/category/poster/)


### [Assessing Cross-Version Simcyp Simulator Prediction Performance Using a Bioequivalence Framework to Support EMA Qualification](https://www.certara.com/poster/assessing-cross-version-simcyp-simulator-prediction-performance-using-a-bioequivalence-framework-to-support-ema-qualification/)


[Important drug development questions that model-based meta-analysis answers](https://www.certara.com/blog/important-drug-development-questions-that-model-based-meta-analysis-answers/)


[Important drug development questions that model-based meta-analysis answers](https://www.certara.com/blog/important-drug-development-questions-that-model-based-meta-analysis-answers/)[Blog](https://www.certara.com/category/blog/)


### [Important drug development questions that model-based meta-analysis answers](https://www.certara.com/blog/important-drug-development-questions-that-model-based-meta-analysis-answers/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)[Poster](https://www.certara.com/category/poster/)


### [A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)
