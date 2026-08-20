---
schema_version: "1.0.0"
document_id: "accf98e2c5bc79e18743cc15c9815ea06fe1900088f42534cb78af9a48e1a3f8"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-rss-c7049147c8d2"
canonical_url: "https://www.certara.com/blog/a-model-informed-approach-to-radioligand-therapy-dosing-under-project-optimus/"
published_at: "2026-08-13T17:01:34+00:00"
first_seen_at: "2026-08-13T17:33:09.673827+00:00"
fetched_at: "2026-08-13T17:33:13.024990+00:00"
content_hash: "sha256:299f0ee8b72a1cfe4e05a26144c1ab26ae1f53840c2fbac2540bed2c43315993"
---

# A model-informed approach to radioligand therapy dosing under Project Optimus

ShareShareShare


Hunter Stephens, PhD Associate Director, PharmacometricsJoshuaine Grant Senior Director, Quantitative Systems Pharmacology (QSP)Amandine Manon, PharmD Senior Director, Clinical Pharmacology and Translational MedicineMirjam Trame, PharmD, PhD Vice President, Certara Drug Development Solutions, Head of Pharmacometrics USA – Division IIDiane-Charlotte Imbs, PharmD, PhD Director Clinical Pharmacology, Drug Development Solutions


August 13, 2026


In recent years, radioligand therapy programs have entered the clinic following a familiar path: borrowing organ dose limits from external beam radiotherapy (EBRT), and escalating activity levels until reaching or slightly exceeding these EBRT thresholds in organs such as kidneys. In August 2025, the FDA signaled that these approaches alone are no longer sufficient. Its draft guidance on dosage optimization formally brought therapeutic radiopharmaceuticals under[Project Optimus](https://www.certara.com/project-optimus/) .¹


Project Optimus is often interpreted as a push toward dose optimization rather than using maximum tolerated dose. In practice, it raises the bar for how dose and regimen decisions are justified. Sponsors are expected to support those decisions using the totality of available evidence, including dosimetry, pharmacokinetics, biodistribution, imaging, efficacy, and safety, rather than relying on tolerability alone.²


For radioligand therapies, the question is no longer simply what activity to administer. It is whether the selected activity and specific activity can be supported by a clear understanding of exposure, response, and patient variability. The strongest scientific rationale comes from understanding the biological and pharmacological factors that drive clinical outcomes. Increasingly, that requires mechanistic modeling such as quantitative systems pharmacology (QSP).


## Beyond maximum tolerated dose (MTD) and traditional dose finding


The maximum tolerated dose paradigm was developed for cytotoxic therapies and does not translate cleanly to radioligand therapies. Efficacy can plateau well below the tolerated ceiling, and the limiting factor is often not systemic tolerability, but the balance between radiation delivered to the tumor and radiation delivered to organs at risk. That is a therapeutic-index problem, which is exactly why Project Optimus moves from MTD toward exposure–response and benefit–risk.


Radiopharmaceutical developers are being asked to answer a different question. Rather than identifying a single activity level, they must understand how efficacy and safety change across patients, target expression levels, and disease states. A single average answer can obscure the very factors that influence efficacy and safety. Addressing those questions requires incorporating the underlying biology directly into the QSP model.


## QSP: Modeling the biology behind the therapeutic window


QSP provides a way to connect the biological drivers of response with the measurements used to guide development decisions. QSP models integrate target expression, binding, internalization, biodistribution, and radiation dosimetry to characterize tumor and organ exposure, therapeutic index, and dose response. They can also evaluate these relationships across diverse patient populations and varying levels of target expression, rather than relying on a single representative patient.


A QSP analysis of ¹⁷⁷Lu-PSMA-617 by Minucci and colleagues at Certara illustrates this approach.⁹ The model incorporated blood, tumor, kidney, and salivary gland compartments, along with PSMA binding, internalization, and radioactive decay. After scaling from mouse to human, it successfully reproduced observed blood, tumor, and kidney exposure. More importantly, it challenged a long-held assumption: salivary gland uptake could not be fully explained by target expression alone. Even a hundredfold increase in modeled PSMA expression failed to account for the observed uptake. Whether the underlying mechanism is truly non-PSMA mediated remains an active area of investigation. Clinicopathologic studies showing reduced uptake in PSMA-/- mice suggest it may be a PSMA-mediated mechanism, while more recent analyses failed to establish a relationship between PSMA expression and salivary gland uptake.6,7 Regardless of the outcome, the model’s value lies in its ability to test hypotheses against data rather than rely on assumptions. This is particularly important when the interplay between the target and dose-limiting off-target organs influences the therapeutic window.


Because QSP captures the underlying targeting mechanisms, it can also evaluate how the therapeutic index changes as target expression varies between patients and evolves throughout treatment. This has important implications for radioligand therapy. As tumor burden decreases and target expression on tumor cells diminishes over successive treatment cycles, the tumor sink effect may diminish, increasing uptake in dose-limiting organs such as the salivary glands. Efforts to reduce exposure to those organs can, in turn, reduce tumor exposure in the patients who are responding to therapy.8 This effect has been demonstrated with QSP modeling, highlighting a key risk of increasing mass dose to reduce off-tumor uptake.9 A fixed dose limit cannot account for these dynamics. Characterizing them is essential for dose optimization.


*QSP characterizes how tumor and dose-limiting organ exposure rise with injected activity and how the resulting therapeutic window varies across patients and target expression levels.*


## Looking beyond dosimetry


Project Optimus requires more than absorbed dose calculations. In our upcoming webinar, Certara experts show how clinical pharmacology, pharmacometrics, QSP, PBPK, dosimetry, and regulatory strategy work together to help sponsors optimize dose selection and build stronger evidence packages for therapeutic radiopharmaceutical development.


[Register for the webinar: From dosimetry to dose decisions](https://www.certara.com/webinar/from-dosimetry-to-dose-decisions-how-clinical-pharmacology-and-model-informed-approaches-are-shaping-trt-development/)


## Connecting biology to clinical outcomes


A mechanistic model is only part of the evidence.[Population pharmacokinetic (PK) analyses](https://www.certara.com/white-paper/introducing-population-pharmacokinetic-analysis-into-your-early-drug-development-efforts/) that incorporate blood PK and time-activity-curves in organs and tumor from imaging are equally important.


One of the most common assumptions in radioligand therapies development is the use of[organ dose limits derived from EBRT](https://www.certara.com/blog/dose-optimization-considerations-for-targeted-radiation-therapies/) . The challenge is that dose rate matters. EBRT delivers a high dose over a short period of time, whereas radioligand therapies deliver radiation gradually over days, allowing healthy tissue additional time for repair and cell cycles to redistribute. As a result, the same absorbed dose may not produce the same biologically effective dose (BED).


This distinction has practical implications. A population PK analysis of a ¹⁷⁷Lu-PSMA radioligand demonstrated that kidney and salivary gland BED estimates differed substantially from values predicted using EBRT assumptions.³,⁴ The magnitude and direction of these deviations differed across organs, reflecting their distinct uptake, retention, and clearance kinetics, so a single correction factor applied uniformly across tissues would not capture the true biologically effective dose.


The complexity increases further with alpha-emitting therapies such as ²²⁵Ac. In these cases, absorbed dose estimates depend not only on the kinetics of the radioligand, but also on how radioactive decay daughters redistribute after potential liberation from the ligand.⁵ Mechanistic and QSP models can address this directly by representing each decay daughter as a distinct species with its own kinetics, making assumptions about daughter redistribution explicit and testable against data rather than leaving them implicit in a single absorbed-dose estimate.


Those exposure estimates only matter once they are linked to outcomes. On the efficacy side, that means relating tumor-absorbed dose or biologically effective dose (BED) to measures of response such as PSA decline, radiographic response, or progression-free survival; on the safety side, relating absorbed dose or BED in organs at risk to the toxicities that limit therapy, such as bone marrow absorbed dose or BED to hematotoxicity or salivary glands absorbed dose or BED to xerostomia. Because population PK supplies individual exposure estimates across a heterogeneous population, these exposure-response relationships can be characterized patient by patient rather than as a single average, which is exactly what a benefit-risk assessment under Project Optimus requires.


The key point is that QSP and population PK are complementary. Understanding where radiation is delivered is essential, but so is understanding how that exposure translates into efficacy and safety. Project Optimus expects these approaches to be integrated into a single exposure-response framework rather than treated as separate analyses.


## Dose optimization starts before the clinic


Many of the most consequential dose optimization decisions are made long before a patient enters a clinical trial. Mechanistic models can help evaluate target feasibility, determine whether target expression and accessibility are sufficient to support a viable therapeutic index, prioritize or inform candidate selection criteria, and inform first-in-human dose selection.


These models can also help compare how different constructs balance tumor uptake against off-target exposure, allowing teams to make more informed portfolio decisions earlier in development.


Identifying an unfavorable therapeutic window before committing radionuclide supply, manufacturing resources, and clinical trial capacity can reduce development risk, prevent costly late-stage decisions, and improve candidate selection.


## The defensible dose framework


### Six questions the evidence should answer


1. Absorbed dose and biologically effective dose, informed by radiobiology rather than gray alone.
2. Organ constraints derived for the molecule, isotope, and dose rate, not extrapolated from external beam radiotherapy.
3. The uptake and biodistribution mechanism tested against data, not assumed from target expression.
4. Therapeutic index and dose–response characterized across patient heterogeneity and variable target expression.
5. For α-emitters, explicit and justified assumptions for how decay daughters redistribute.
6. Imaging, PK, dosimetry, and mechanism integrated into one exposure–response narrative.


## From data to decisions


For radioligand therapy developers, the implications of Project Optimus extend beyond dose optimization. The guidance reflects a broader shift in regulatory expectations. Regulators want to understand not only what dosing regimen was selected, but why it was selected and how that decision is supported by the available evidence.


Radioligand therapies present unique challenges because efficacy, toxicity, target expression, biodistribution, and patient variability are closely interconnected. Addressing those challenges requires more than dosimetry alone. It requires an integrated understanding of how biology influences exposure and response.


Many of the decisions that shape a successful development program are made long before a pivotal study begins. Bringing together dosimetry, clinical pharmacology, pharmacometrics, QSP, and regulatory science early in development gives sponsors a stronger scientific foundation for dose selection and a clearer rationale to support regulatory interactions.


Dose selection will always remain a clinical decision. Demonstrating why that dose is appropriate for a particular therapy, and patient population increasingly depends on quantitative evidence. For radiopharmaceutical developers, the goal is not simply to identify the right dose. It is to build a clear, scientifically defensible rationale for the decisions that guide development from the earliest stages through registration.


## Key takeaways


- Project Optimus requires a stronger scientific rationale for dose selection.
- Integrating dosimetry, QSP, and population PK provides a more complete understanding of exposure and response.
- Patient variability should be incorporated into dose optimization, not treated as an afterthought.
- Model-informed drug development (MIDD) approaches help answer critical development questions before pivotal studies begin.
- A well integrated quantitative strategy strengthens both development decisions and regulatory interactions.


## Authors


[Hunter Stephens, PhD](https://www.certara.com/teams/hunter-stephens-phd/) Associate Director, Pharmacometrics


Hunter Stephens, PhD, is an Associate Director in the Pharmacometrics group at Certara. He specializes in applying mathematical and computational models to understand the pharmacokinetics and pharmacodynamics of drugs, especially radiopharmaceuticals. He has a PhD in Medical Physics from Duke University. He also holds an MS in Physics from North Carolina State University and a BS in Mathematics from Tennessee Tech University. His work in TRT has focused on building semi-mechanistic population PK models to simulate and predict absorbed and biologically effective doses to inform dose-range finding and questions of safety and efficacy. In addition, he has extensive experience in radiation dosimetry from external and internal sources.


[Joshuaine Grant](https://www.certara.com/teams/joshuaine-grant/) Senior Director, Quantitative Systems Pharmacology (QSP)


Joshuaine Grant is a Senior Director in Quantitative Systems Pharmacology at Certara with more than 25 years of experience integrating biophysics, disease biology, and quantitative modeling to advance drug development. She leads collaborative projects that use mechanistic and translational QSP modeling to inform key decisions from discovery through the clinic. Her broad experience across biologics and complex therapeutics has supported Certara’s growing leadership in radioligand and targeted radiotherapies.


[Amandine Manon, PharmD](https://www.certara.com/teams/amandine-manon/) Senior Director, Clinical Pharmacology and Translational Medicine


Amandine joined Certara in 2020. She served as a Clinical pharmacologist in several pharmaceutical companies for 15 years. She has a proven track record in preclinical and clinical PK, clinical pharmacology with a special focus on oncology, drug development from early stages to Phase 3, and regulatory experience. Amandine graduated as a PharmD from Paris University, France and she also holds a Master’s degree in Pharmacokinetics.


[Mirjam Trame, PharmD, PhD](https://www.certara.com/teams/mirjam-trame/) Vice President, Certara Drug Development Solutions, Head of Pharmacometrics USA – Division II


Mirjam is an expert in pharmacometrics and oncology drug development, serving as Head of Pharmacometrics USA – Division II at Certara Drug Development Solutions. With expertise spanning complex biologics, she supports exposure-response analysis, dose and study optimization, and regulatory strategy, with a special focus on radiotherapeutics and cell and gene therapies.


[Diane-Charlotte Imbs, PharmD, PhD](https://www.certara.com/teams/dr-diane-charlotte-imbs/) Director Clinical Pharmacology, Drug Development Solutions


Dr. Diane-Charlotte Imbs is a Director in Clinical Pharmacology at Certara. She joined Certara in 2020 and has been supporting several projects (mAbs, radiopharmaceuticals, small molecules, ADC, fixed-dose combinations) from Phase I to Phase III in rare diseases, hematology, oncology, and post-marketing in cardiology. Before joining Certara, she spent 3 years at Ipsen as a clinical pharmacology project manager where she supported several Phase I projects in oncology (radiopharmaceuticals, small molecules). Diane-Charlotte is a pharmacist by training with a master’s degree in pharmacology and a PhD in Clinical Pharmacokinetics (University of Toulouse, France).


## Frequently asked questions


### Does Project Optimus apply to radiopharmaceuticals and targeted radionuclide therapy?


Yes. In August 2025, the FDA’s draft guidance on dosage optimization formally brought therapeutic radiopharmaceuticals under[Project Optimus](https://www.certara.com/project-optimus/) . In practice, this means sponsors are expected to justify dose and regimen decisions using the totality of available evidence—dosimetry, pharmacokinetics, biodistribution, imaging, efficacy, and safety—rather than relying on tolerability or maximum tolerated dose (MTD) alone.


### Why can’t organ dose limits from external beam radiotherapy (EBRT) be applied to radioligand therapy?


Because dose rate matters. EBRT delivers a high dose over a short period, whereas radioligand therapies deliver radiation gradually over days, giving healthy tissue additional time to repair. As a result, the same absorbed dose can produce a different biologically effective dose (BED), and the deviations vary by organ—so a single EBRT-derived correction factor does not hold across tissues.


### What is the difference between absorbed dose and biologically effective dose (BED) in TRT?


Absorbed dose measures the energy deposited in tissue, while BED accounts for how that dose is delivered over time and the biological repair that occurs between and during exposures. For radioligand therapies delivered slowly over days, BED often provides a more meaningful basis for setting organ constraints and relating exposure to efficacy and toxicity than absorbed dose alone.


### How does QSP modeling support dose optimization for radioligand therapies?


Quantitative systems pharmacology (QSP) integrates target expression, binding, internalization, biodistribution, and radiation dosimetry to characterize tumor and organ exposure, therapeutic index, and dose response across diverse patients. Because it captures the underlying targeting biology,[QSP modeling](https://www.certara.com/services/quantitative-systems-pharmacology/) can test mechanistic hypotheses against data and show how the therapeutic window shifts as target expression changes between patients and over successive treatment cycles.


### Do alpha-emitting therapies such as ²²⁵Ac require different dosimetry considerations?


Yes. For alpha-emitters, absorbed dose depends not only on the kinetics of the radioligand but also on how radioactive decay daughters redistribute after they are liberated from the ligand. Mechanistic and QSP models can represent each decay daughter as a distinct species with its own kinetics, making assumptions about daughter redistribution explicit and testable rather than hidden inside a single absorbed-dose estimate.


## Ready to build a stronger dose optimization strategy?


Whether you’re planning a first-in-human study, optimizing a dosing regimen, or preparing for regulatory interactions under Project Optimus, Certara’s integrated team of experts can help you build a scientifically defensible evidence package.


[Talk to a TRT expert](https://www.certara.com/certara-complex-biologics/targeted-radionuclide-therapy-trt-and-theranostics/#Contact-us-CTA)


## Sources


1. FDA. Oncology Therapeutic Radiopharmaceuticals: Dosage Optimization During Clinical Development. Draft Guidance for Industry, August 2025.


2. Commentary on the FDA radiopharmaceutical dosing guidance — dose justification and integration of dosimetry and PK/PD data (Targeted Oncology, 2025); Response to the FDA Dosage Optimization Draft Guidance for Radiopharmaceutical Therapies, J Nucl Med (2026).


3. Wahl RL, et al. Normal-Tissue Tolerance to Radiopharmaceutical Therapies, the Knowns and the Unknowns. J Nucl Med. 2021;62(Suppl 3):23S.


4. Hope TA, Hofman MS, et al. Rethinking Dosimetry: The Perils of Extrapolated EBRT Constraints to Radionuclide Therapy (editorial), J Nucl Med (2024).


5. Liubchenko G, et al. Image-based dosimetry for \[225Ac\]Ac-PSMA-I&T therapy and the effect of daughter-specific pharmacokinetics. Eur J Nucl Med Mol Imaging (2024). doi:10.1007/s00259-024-06681-2.


6. Rupp NJ, Umbricht CA, Pizzuto DA, et al. First Clinicopathologic Evidence of a Non–PSMA-Related Uptake Mechanism for 68Ga-PSMA-11 in Salivary Glands. J Nucl Med. 2019;60(9):1270–1276.


7. Julian W, et al. Searching for Protein Off-Targets of Prostate-Specific Membrane Antigen-Targeting Radioligands in the Salivary Glands. Cancer Biother Radiopharm. 2024. doi:10.1089/cbr.2024.0066.


8. Salivary Gland Toxicity of PSMA Radioligand Therapy: Relevance and Preventive Strategies. J Nucl Med (2018);59(8):1172.


9. Source posters (Certara, SNMMI): “Translating Targeted Radiotherapies from Mouse to Human Using QSP: A Pluvicto Case Study” (S. Minucci, B. Koirala, B. Lang, J. Nosbisch, J. Grant); “Population Pharmacokinetic Modeling to Predict Absorbed and Biologically Effective Dose for Radioligand Therapies” (H. Stephens, A. Manon, M. N. Trame). (Certara, ASCPT 2026) “Quantitative systems pharmacology model of a targeted radiotherapy to inform a target candidate profile and aid in early decision-making” (S. Minucci, B. Koirala, B. Lang, J. Nosbisch, J. Grant)


## You May Also Like


AllOncology/Hematology


[Population Pharmacokinetic Modeling of Niraparib to Assess Different Absorption Models](https://www.certara.com/publication/population-pharmacokinetic-modeling-of-niraparib-to-assess-different-absorption-models-jcp/)


[Population Pharmacokinetic Modeling of Niraparib to Assess Different Absorption Models](https://www.certara.com/publication/population-pharmacokinetic-modeling-of-niraparib-to-assess-different-absorption-models-jcp/)[Publication](https://www.certara.com/category/publication/)


### [Population Pharmacokinetic Modeling of Niraparib to Assess Different Absorption Models](https://www.certara.com/publication/population-pharmacokinetic-modeling-of-niraparib-to-assess-different-absorption-models-jcp/)


[Beyond approval: Using model-informed strategies to maximize the value of ADCs](https://www.certara.com/blog/beyond-approval-using-model-informed-strategies-to-maximize-the-value-of-adcs/)


[Beyond approval: Using model-informed strategies to maximize the value of ADCs](https://www.certara.com/blog/beyond-approval-using-model-informed-strategies-to-maximize-the-value-of-adcs/)[Blog](https://www.certara.com/category/blog/)


### [Beyond approval: Using model-informed strategies to maximize the value of ADCs](https://www.certara.com/blog/beyond-approval-using-model-informed-strategies-to-maximize-the-value-of-adcs/)


[ADC dose optimization: Applying model-informed strategies in development](https://www.certara.com/blog/adc-dose-optimization-applying-model-informed-strategies-in-development/)


[ADC dose optimization: Applying model-informed strategies in development](https://www.certara.com/blog/adc-dose-optimization-applying-model-informed-strategies-in-development/)[Blog](https://www.certara.com/category/blog/)


### [ADC dose optimization: Applying model-informed strategies in development](https://www.certara.com/blog/adc-dose-optimization-applying-model-informed-strategies-in-development/)
