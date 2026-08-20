---
schema_version: "1.0.0"
document_id: "a4b03ad49ccb4d5addd2d978c6e5e883f478a54952866b59377e1f396450947e"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-6fa26b21f70d"
canonical_url: "https://www.certara.com/blog/minimizing-risk-in-hepatic-impairment-studies/"
published_at: "2026-07-13T12:22:29+00:00"
first_seen_at: "2026-07-22T21:45:01.422888+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:93051bd45776fefca47bbbc5e6139704b2e24a7109df9c9af5e22ff40d9cbc7f"
---

# When to conduct hepatic impairment studies and when modeling may be enough

ShareShareShare


Zoe Barter, PhD Senior Director, PBPK Consultancy, CertaraNolan Wood, PhD VP, Clinical Pharmacology Consulting


July 13, 2026


For drugs that undergo hepatic metabolism or biliary excretion, hepatic impairment (HI) studies are a critical


component


of the regulatory pathway. Yet with


guidances


that are now over twenty years old, a rapidly evolving modeling toolkit, and complex decisions around study design and timing, navigating HI requirements


remains


one of the more nuanced challenges in clinical pharmacology.


Here


we’ll


discuss


a practical framework for minimizing risk at every stage


:


from deciding whether a study is needed, to


leveraging


physiologically based pharmacokinetic


(


PBPK


)


modeling as a regulatory-accepted alternative.


### Why hepatic impairment studies matter


The core objective of any hepatic impairment evaluation is to inform prescribing information. Specifically, it helps physicians select an appropriate dose for patients with mild, moderate, or severe hepatic dysfunction. The consequences of getting this wrong are significant: an inadequate label may restrict patient access, trigger post-marketing commitments, or lead to unsafe dosing in a vulnerable population.


Both the


[FDA (2003)](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/pharmacokinetics-patients-impaired-hepatic-function-study-design-data-analysis-and-impact-dosing-and) and


[EMA (2005)](https://www.ema.europa.eu/en/evaluation-pharmacokinetics-medicinal-products-patients-impaired-hepatic-function-scientific-guideline) have issued guidance on when HI studies are required, and while the two share common ground, there are meaningful differences that teams must account for in their strategy.


### When is a hepatic impairment study required?


The FDA guidance sets a specific threshold: if hepatic metabolism or biliary excretion accounts for more than 20% of the absorbed dose (parent drug or metabolite), a hepatic impairment study is generally expected. The EMA guidance takes a similar approach in principle but does not specify a numeric threshold, instead providing recommendations based on likely use in hepatically impaired patients and the probable impact on exposure.


A study is typically not required when:


- The drug is gaseous or volatile (eliminated via the lungs)


- Less than 20% of the drug is hepatically metabolized


- The drug has a wide therapeutic index and no significant hepatic involvement


A practical decision tree for teams to work through, guiding sponsors whether a dedicated study in patients with hepatic impairment is required, is presented in Figure 1. Alternative strategies, including the use of modeling or a combination approach is discussed further below.


### Study design options: full vs. reduced


When a dedicated clinical HI study is required, two main designs are recommended based on current guidance:


#### Full design study


Includes patients with mild, moderate, and severe hepatic impairment, along with age- and gender-matched healthy controls. This design is considered appropriate when specific dosing recommendations are needed across the full spectrum of hepatic dysfunction. Enrollment is typically guided by the Child-Pugh classification, with approximately six subjects per group.


#### Reduced design study


Focuses on moderate hepatic impairment only, with matched controls. The FDA requires at least eight subjects per group for this design and specifies that any meaningful PK change seen in moderate impairment should also be reflected in labeling for mild patients. The EMA adopts a slightly different approach, recommending that sponsors may need to extend the study to severe or mild groups depending on the results in moderate hepatic impairment.


Single-dose studies are typically sufficient when PK is dose-proportional and there is no time-dependent behavior. Multiple-dose designs should be considered when induction, inhibition, or time-dependent PK changes are present.


Webinar


## Want to learn more about designing HI studies and applying PBPK modeling? View the full webinar.


[View webinar](https://www.bigmarker.com/ciiwebinars/minimize-risk-when-preparing-for-hepatic-impairment-studies)


### Alternative strategies: when a dedicated study is not the only answer


A dedicated study is not the only path to adequate HI labeling. There are several alternative approaches:


- **Population PK (PopPK** ): Patients with hepatic impairment can be included in early efficacy studies, and hepatic function assessed as a covariate in a pre-specified PopPK analysis. Both the FDA and EMA support this approach, which aligns with the recently issued ICH M15 guidance.


- **PBPK modeling:** Predict exposure in different levels of hepatic impairment, extrapolate from single to multiple dose, and supplement limited clinical data. While not explicitly mentioned in the FDA guidance, the EMA guidance references PBPK as a viable approach.


- **Combination approaches:** In many cases, the most efficient strategy combines early PopPK data (e.g., from patients with mild impairment enrolled in phase 2/3) with PBPK modeling to address the full spectrum of impairment.


- **No study:** If a sponsor decides not to conduct an HI study, labeling will reflect a lack of data. This will likely result in contraindication language for hepatically impaired patients and may also trigger a post-marketing commitment.


### PBPK modeling for hepatic impairment: a closer look


[Simcyp® Simulator](https://www.certara.com/software/simcyp-pbpk/) is a PBPK modeling software that separates drug data from systems data, allowing the software to model how physiological changes in a hepatically impaired population affect drug disposition. Key parameters built into the model include:


- Demographic distributions (age, weight, height)


- Organ weights and blood flows, including reduced liver volume and altered portal blood flow


- Plasma protein concentrations (albumin and alpha-1-acid glycoprotein), which decrease with increasing disease severity


- Enzyme and transporter expression levels across Child-Pugh A, B, and C categories


- Glomerular filtration rate and gastric residence time


Three cirrhosis population models are available in Simcyp Simulator, corresponding to Child-Pugh A, B, and C classifications.


#### Model performance


Across nine compounds and thirteen clinical studies, Simcyp Simulator’s hepatic impairment models predicted AUC ratios (impaired vs. healthy) within twofold across all Child-Pugh classifications, with average fold errors generally below 1.5.


#### What increases the chance of regulatory acceptance?


Based on our PBPK modeling consultancy experience, the following factors improve the likelihood that PBPK modeling will be accepted in lieu of a clinical HI study:


- A well-characterized ADME profile, including mass balance and enzyme phenotyping data


- Good understanding of any PK nonlinearities and single-to-multiple dose transitions


- Availability of drug-drug interaction (DDI) study data to refine fraction metabolized estimates


- A predicted exposure shift that is modest or clinically manageable


#### An evolving frontier: non-CYP enzymes and absorption


Most of our PBPK HI experience to date involves CYP-mediated clearance, particularly CYP3A4, where the modeling framework is well validated, but the field is evolving. Two key areas of active development include:


- **Non-CYP enzymes:** Changes in UGT and other enzyme expression across Child-Pugh categories are increasingly well characterized in the literature, but validated probe substrates for these enzymes are limited. For example, only two clinical studies are currently available to validate Simcyp’s UGT1A1 predictions (using raltegravir as a probe substrate).


- **Absorption impairment:** Work on avapritinib and other compounds is revealing that hepatic impairment affects not just elimination but also absorption through mechanisms including changes in gastric residence time, bile salt concentrations, and portal-systemic shunting. A robust mechanistic model for these absorption changes is still in development.


### Conclusion


Hepatic impairment studies sit at the intersection of regulatory obligation, patient safety, and drug development efficiency. With two regulatory guidances that predate many of today’s modeling capabilities, the field has evolved considerably beyond what those documents describe. The combination of well-designed clinical studies, pre-specified popPK analyses, and an EMA-qualified PBPK modeling platform like Simcyp Simulator now provides development teams a genuinely flexible toolkit for meeting HI labeling requirements while minimizing unnecessary burden.


The key is to start early, understand your compound’s elimination pathways, and work with experienced clinical pharmacologists who can help you design the most efficient path to adequate labeling.


## Learn more


Certara’s clinical pharmacology and PBPK


experts


support sponsors across all stages of drug development, including hepatic and renal impairment study design, modeling and simulation, regulatory strategy, and label negotiation.


[Explore our solutions](https://www.certara.com/solutions/)[Contact us to get in touch with an expert](https://www.certara.com/company/contact/)


## Authors


[Zoe Barter, PhD](https://www.certara.com/teams/zoe-barter-phd/) Senior Director, PBPK Consultancy, Certara


Dr. Barter has over 20 years of experience in drug metabolism, transport, complex interactions, and specific populations with a strong background in in vitro to in vivo extrapolation (IVIVE). She received her PhD in Drug Metabolism from the University of Sheffield in 2005. This was followed by a joint position as a Postdoctoral Researcher within the Unit of Clinical Pharmacology at the University of Sheffield and Research Scientist at Certara. In 2009, she joined Certara full-time where she has been involved in projects related to extrapolating clearance and extending compound and population databases within Simcyp Simulator and modeling of complex transporter and metabolism mediated drug-drug interactions and inter-ethnic differences in pharmacokinetics.


[Nolan Wood, PhD](https://www.certara.com/teams/nolan-wood-phd/) VP, Clinical Pharmacology Consulting


Dr Nolan Wood joined Certara in 2017 as a consultant in the clinical pharmacology and translational medicine group, providing clinical pharmacology leadership and stewardship to develop high quality, innovative clinical pharmacology programs supporting global drug development. Dr. Wood has more than 35 years of global drug development experience working in the pharmaceutical industry and as a consultant, covering all stages of development, from first-in-human studies through to regulatory submissions, including pediatric drug development. He has worked across a wide range of therapeutic areas involving both small molecules and biological products. Working as the global clinical pharmacology lead, he has taken responsibility for implementing model-informed drug development strategies resulting in successful global regulatory approvals for several compounds. He has contributed to the design and execution of studies in special populations including patients with renal and hepatic impairment and pediatric populations. Dr. Wood has extensive experience of interactions with the major health authorities and has written Clinical Pharmacology sections for regulatory submissions and requests for information.


Following a degree in Pharmacology, Dr Wood obtained a PhD in the Clinical Pharmacology Group at the University of Southampton. He has (co-) authored over 40 peer-reviewed publications and is a member of the British Pharmacological Society.


## FAQs


### When is a hepatic impairment study not required?


A dedicated hepatic impairment study is generally not required if the drug is gaseous or volatile (eliminated via the lungs), if hepatic metabolism or biliary excretion accounts for less than 20% of the absorbed dose, or if the drug has a wide therapeutic index with limited hepatic involvement. However, each case should be evaluated against the applicable regulatory guidance, and teams should be prepared for the possibility of a post-marketing commitment if a study is not conducted.


### What is the difference between a full design and a reduced design hepatic impairment study?


A full design study enrolls patients with mild, moderate, and severe hepatic impairment alongside matched healthy controls, providing comprehensive data across the entire spectrum of dysfunction. A reduced design study


includes only


moderate hepatic impairment and controls. The reduced design is a common first step


,


but if a meaningful PK change is


observed


, sponsors may need to extend to severe or mild groups, depending on the regulatory agency.


### Can PBPK modeling replace a dedicated hepatic impairment clinical study?


In some cases, yes. Both the FDA and EMA have accepted PBPK modeling in lieu of, or to supplement, clinical HI studies, particularly for extrapolating from single to multiple


dose


, from one Child-Pugh category to another, or to derive dose recommendations when clinical data is limited. However, acceptance is more likely when the drug’s ADME properties are well-characterized, clinical PK data is robust, and the predicted exposure shift is clinically manageable. Severe hepatic impairment is often held to a higher bar and may still require a post-marketing study.


### What is the Child-Pugh scale and why is it used in HI studies?


The Child-Pugh scale is a widely accepted clinical scoring system used to assess the degree of hepatic impairment. It incorporates three laboratory measures and two clinical measures to classify patients as Child-Pugh A (mild), B (moderate), or C (severe) impairment. Most regulatory guidance for HI studies references Child-Pugh classification for enrollment criteria and for defining the degree of impairment being studied. In oncology settings, the NCI Organ Dysfunction Working Group (NCIODAG) classification based on bilirubin and ALT may also be used.


### How early in development should hepatic impairment be considered?


Hepatic impairment planning should begin during nonclinical development, where early in vitro and in vivo ADME data can


indicate


the extent of hepatic metabolism. If significant hepatic elimination is


anticipated


, eligibility criteria for early phase 1 studies can be designed to include patients with mild HI. Building in pre-specified


PopPK


sampling from the outset maximizes the value of that data and may reduce the need for a dedicated standalone study later.


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
