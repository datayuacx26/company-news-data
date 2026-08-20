---
schema_version: "1.0.0"
document_id: "b43de67fcdca37d1348bf8c69d6efc2e67b48b30dffbd206294c8f50c42b7d27"
company_key: "yc-segmed"
company: "Segmed"
source_id: "yc-segmed-news-import-4f813eeed56d"
canonical_url: "https://www.segmed.ai/resources/blog/how-medical-imaging-generates-rwe-in-precision-oncology"
published_at: null
first_seen_at: "2026-07-22T12:58:23.262945+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:26f7c0e4cb1413076c9c94ec647c3ad2ad43cc20aeee1d9c867b586af0a98cf9"
---

# Imaging-Driven Real-World Evidence in Oncology: Advancing Precision Medicine Through Disease-Specific Research

Clinical trials have been the foundation of how we understand the field of oncology for many years. However there is one fundamental flaw in how we understand oncology through trials. Historically, the overall estimated patient participation rate to cancer treatment trials was 7.1%. At the community level, where the majority of patients are treated, only about 4% of adult cancer patients are treated in research trials.1


‍


This evidence gap is incredibly significant compared to the total number of oncology patients in the real-world. Most of the patients treated for cancer are getting treated in community-based programs and not included in any kind of formal research documentation.


‍


One of the ways to address this problem is with[real-world evidence (RWE)](https://www.segmed.ai/resources/blog/real-world-imaging-datasets-to-enhance-precision-medicine) . Real-world evidence is documented clinical evidence that comes from routine healthcare and not clinical research settings. Real-world evidence provides a clear understanding of how real patients receive treatment in real hospitals. When we think of how the use of medical imaging relates to this shift; it is at the very heart of what is going to be the next big change.


‍


Each CT, MRI and PET/CT scan obtained for patients throughout their treatment for cancer contains an abundance of biological data. The biological data contained in each imaging study includes tumor size, tumor characteristics, metabolic activity, response to treatment, etc. for each individual patient. Therefore, if that biological data is properly de-identified and aggregated once it has been placed into an electronic format, it can be utilized as a great source of RWE.2


‍


This article discusses how this process works, what has already been accomplished with it, and where it is headed.


##
Which Imaging Modalities Drive Real-World Evidence in Oncology?


Three modalities form the backbone of oncology real-world imaging data (RWiD): CT, MRI, and PET (combined with CT or MRI). Together, they create a longitudinal picture of disease progression and treatment response. Each captures a different dimension of tumor biology.


‍


‍


#### CT scan **:** most scalable real-world imaging source


In the United States, about 93 million CT examinations were performed during the year 2023 alone making CT the largest source of[real-world imaging data](https://www.segmed.ai/solutions/for-medical-devices-and-ai-rd) available to oncology researchers.3 CT scans occur at diagnosis, during treatment, and after treatment. They provide a continuous, longitudinal record of tumor evolution across the care continuum.


‍


#### PET/ MRI: matching PET/ CT with added clinical value


For years, PET/ CT was the gold standard for oncology staging. PET/ MRI has now demonstrated equivalent diagnostic accuracy with meaningful advantages. A prospective study of 1,003 oncologic examinations in 918 patients found PET/ MRI and PET/ CT had comparable lesion detection capabilities.4 PET/ MRI also delivered superior soft-tissue contrast and significantly lower radiation exposure. A subsequent meta-analysis confirmed PET/ MRI is superior to PET/ CT for detecting liver, brain, and bone metastases.5 These are the sites that most often drive treatment decisions in advanced solid tumors. For real-world staging and response data, this makes PET/ MRI an increasingly important RWiD source.


‍


##
How Is Imaging RWE Applied in Precision Oncology?


Radiomics is perhaps the most exciting application for the use of RWiD in precision oncology. It allows for the extraction of quantitative, high-dimensional features from medical images. These quantitative features describe texture, shape, intensity patterns and tumor biology that are typically not able to be observed by the human eye.


Based upon AI models trained on these extracted features, the outcome can be predicted in terms of molecular status, treatment response and prognosis from the routine clinical care system's scans that have previously been obtained.
‍


###
Case Study: Predicting RAS Mutation Status in Colorectal Cancer


Metastatic colorectal cancer (mCRC) treatment hinges on RAS mutation status. RAS status determines eligibility for anti-EGFR therapies like cetuximab and panitumumab. Standard practice requires tissue biopsy which is invasive, prone to sampling bias, and logistically difficult. Imaging RWE offers a non-invasive alternative.


*Granata et al. (2024)* developed a CT and MRI radiomics model to predict RAS status in colorectal liver metastases (CRLMs). The model extracted 851 radiomic features per lesion from routine pre-surgical scans. It reliably predicted RAS mutational status without requiring additional invasive procedures.6


A separate multicenter study of 185 CRLM patients went further. A deep learning model using arterial and venous phase CT achieved an AUC of 0.98 for RAS prediction. That outperformed radiomics-only models (AUC 0.90) and single-phase deep learning approaches. It also showed prognostic value for progression-free and overall survival.7


RWE from imaging also plays a vital role beyond biomarker prediction. Precision oncology trials enroll highly selected, biomarker-defined patient populations. Real-world imaging data shows what happens when those treatments reach broader clinical practice. That kind of comparative effectiveness evidence is increasingly required by regulators and health technology assessment bodies.


‍


## What Are the Challenges in Using Real-World Imaging Data?


The opportunity is real, so are the obstacles. Three challenges consistently limit the scale and reliability of imaging RWE in oncology:


‍


#### **Challenge 1: Variation in Protocols Across Institutions**


One hospital's CT radiomic features may not be reproduced by another CT Scanner in another hospital implying that scanner manufacturer, slice thickness and contrast timing add to the differences. The Image Biomarkers Standardization Initiative (IBSI) attempts to provide a standard set of guidelines; However, adoption is not consistent across research institutions.


‍


#### **Challenge 2: Frequency of CT Scans is Not Clinical Trial Like**


In Real-World Practice, follow-up CT deliveries are on average every 6-9 Weeks. These timeframes are not dictated by an institutional protocol but rather based on clinician’s judgement. Hence, longitudinal data availability varies across centers and geographies.


‍


#### **Challenge 3: Lack of Data Availability and Privacy:**


Patient medical imaging archives are generally managed in isolated systems and subject to stringent federal and state regulations regarding health information privacy (HIPAA & GDPR). Because of this, access to patient data for multi-centre research is restricted. Therefore, in order for an RWiD platform to be scalable and available for use, DICOM will need to have de-identified (removed Protected Health Information) patient data.


‍


> **Research note: How scarce is annotated imaging data?**


> A 2023 systematic review surveyed four major cancer imaging archives. Researchers found only 29 open-access datasets with paired segmentations and clinical annotations. That covered just 10,354 patients and 49,515 scans across all cancer types globally. This scarcity is the primary structural bottleneck separating imaging science from clinical translation.12


‍


## Where Is Imaging RWE in Oncology Headed?


‍


###
1. Healthcare Foundation Models Trained on Real-World Imaging


Large AI models pre-trained on massive datasets are transforming medical AI. In oncology, they require diverse, high-volume, annotated imaging data to train effectively. No single institution can provide that at the required scale. The Sybil lung cancer risk model illustrates what is possible when data access is solved. Sybil was validated on 27,383 low-dose chest CT scans across three international cohorts. It predicts a patient's 6-year lung cancer risk from a single scan. No clinical data or radiologist annotations required. This is only possible because large-scale real-world imaging datasets exist.8


###
2. Genomics Integration: Building Holistic Precision Profiles


Imaging enables visualizing tumors while genomics enables us to understand what is causing the tumor. Together they are transforming and emerging as Radiogenomics. Radiogenomics is changing how researchers develop new treatments as it can identify mutations in tumors and predict treatment outcomes through imaging studies. More importantly, it's providing an evidence-based approach for predicting whether chemotherapy or other therapies will work in specific patients by combining imaging and genomic profiling.


According to a review published in MedComm in 2024, substantial evidence has been generated using radiogenomic applications across multiple cancer types. These include breast, lung, glioma, and colorectal cancers, as well as other solid tumors. Researchers have validated the ability of imaging data to predict genetic mutations. Imaging has also been shown to help characterize the tumor immune microenvironment. Additionally, it can be used to predict treatment outcomes. Furthermore, radiogenomic approaches enable effective prognosis classification. These applications are currently being addressed with either invasive testing or by making these answers impossible under standard of care. By leveraging existing clinical imaging data, researchers can generate evidence supporting these applications without introducing additional procedures. This approach maintains the same cost structure as routine imaging and standard diagnostic workflows.9


‍


### 3. Lifecycle RWE: From Trial to Post-Market Surveillance


Data in oncology outside of clinical trials are primarily sourced from electronic health records (EHRs) and claims databases. While these sources are valuable, they only capture diagnostic information, prescriptions, and notes from healthcare professionals. They do not capture imaging data; without imaging data, tumor response cannot be accurately confirmed using RECIST. In addition, the physician-reported tumor response rates are higher than the actual tumor response. Furthermore, when using external control arms developed only from EHR data, the treatment effects are systematically underestimated. This creates a distorted view of the evidence that could have a significant influence on regulatory decisions. This is a critical issue related to the true purpose of real-world evidence (RWE) in oncology.10


‍


RECIST is considered the worldwide standard for assessing tumor responses and is based only on images. The FDA uses two primary endpoints (objective response rate \[ORR\] and progression-free survival \[PFS\]) to measure the success of an oncology drug, which are both based on imaging scans. This has occurred for the approval of all oncology drug applications for every solid tumor indication, including lung and gastrointestinal cancers. No other evidence can be substituted into this standard process.11 When imaging data can be structured, de-identified and made available to analyze in real world cohorts, there is a dramatic change in the ability of RWE to deliver on its potential. Oncology evidence will now transition from relying on physician memory (recall) and billing codes to utilizing the same objective (i.e., scan confirmed) standard used in clinical trials. It is this alignment that provides regulatory support for imaging-derived RWE and truly allows it to be used appropriately throughout drug development life-cycle.


‍


## Closing Thoughts


While randomized controlled trials remain essential, real-world evidence (RWE) offers exciting opportunities to evaluate cancer in the real world. More people with cancer are excluded from trials than included, and imaging data that is produced during normal course of care contain important evidence that trials are unable to capture. There is no greater source of real-world data for oncology than in the imaging generated from patients- CT and MRI are two of the largest and easiest to access forms of the type of real-world data that would be available if we had multi-institutional, curated, de-identified imaging data of research quality.


The current limitation facing precision oncology is the infrastructure necessary to compile these multi-institutional data. This is exactly what Segmed was designed to accomplish.


‍


## Are you ready to start building your RWE oncology imaging cohort? ****


Explore Segmed's oncology imaging datasets for radiomics, artificial intelligence and RWE research.


Request consultation regarding one of[Segmed's datasets](https://www.segmed.ai/contact-us) to assist with your precision medicine research query.


‍


---


‍


## Frequented Asked Questions - F.A.Q.


### **What is real-world imaging data in oncology?**


Real world oncological imaging data pertains to radiological images (CT, MRI, PET) taken in standard clinical practice (not controlled clinical trial). The patient population for these images can span a wide range of different demographic characteristics (including age, sex), disease severity and treatment regimen reflecting the overall practice of medicine in oncology as a profession. Imaging data (longitudinal scans), imaging reports and associated metadata are available which allow investigators to evaluate the patient's tumor over time, how the patient’s tumor responds to therapy and ultimately what the treatment outcome will be. Additionally, real world oncological imaging data can be leveraged to generate evidence at scale, increase the knowledge of cancer biology outside controlled research settings and may provide valuable insight for drug development, research and precision oncology approaches.


‍


### **How does medical imaging generate real-world evidence?**


Using medical imaging to produce real‐world evidence (RWE) allows for longitudinal evaluation of disease progression over time through the use of routine care data to identify patient response to treatment and clinical outcomes of care based on utilizing standard methodologies. Imaging biomarkers from CT, MRI, and PET scans provide quantitative measures of tumor burden, as well as measurement of tumor heterogeneity and therapeutic response. When imaging data is combined with electronic health record (EHR) data, it produces contextual information regarding treatment patterns and survival outcomes for patients in the specified cohort from whom the imaging and EHR data were collected. Advanced analytics (including AI and radiomics) can provide additional quantitative feature extraction from images, allowing them to be transformed into structured data formats and enabling researchers to conduct observational studies, utilize external control arms and utilize regulatory‐grade evidence in support of research and development of oncology therapies.


‍


### **What imaging modalities are used in oncology research?**


Tumor evaluation and assessment of response to treatment can be performed through multiple imaging types in the field of oncology research. The most common tools used are CT scans (show anatomical detail), MRI (show soft tissue detail), and PET scans (show metabolic activity). Two examples of hybrid imaging modalities that provide both functional and structural data about a tumor are PET-CT and PET-MRI. Ultrasound can be used to image certain types of cancer as it provides real-time imaging of the tumor, while digital pathology can be completed at the cellular level to assist in generating radiology based evidence during cancer research. Each imaging type has its own unique contributions to understanding the patients' tumors, therefore creating a full assessment of the patients' tumor and majority of studies conducted regarding cancer are created with multi-modal real world evidence.


‍


### **What is radiomics in oncology?**


In oncology, radiomics is the extraction of quantitative high dimensional feature sets from medical imaging data for characterizing the phenotype (cellular and morphological) of tumors as well as their microenvironments. Examples of these features include those that describe texture, shape & intensity of the region of interest within an image. These features can be analyzed using statistical & machine learning methods in order to identify relationships or patterns that may not be predicted or discerned by visual inspection alone. By converting standard images into "data that can be mined," radiomics creates the raw material to allow researchers and clinicians to make predictions about an individual patient regarding their diagnosis, prognosis and response to treatment. Therefore, radiomic information is important for precision oncology: through supporting biomarker discovery and integrating radiomic data with genomic & clinical data, it helps enable more targeted/individualized treatment options for patients with cancer.


‍


### **Why is real-world evidence important in oncology?**


Oncology relies heavily on real-world evidence (RWE). RWE provides evidence of how well treatments work in different kinds of patients and how they work in everyday medicine (as opposed to controlled clinical trials). There is more variety in terms of demographics, comorbidities, and adherence to treatment amongst patients than there is in clinical trials. RWE is used for regulatory purposes, post-market surveillance, and comparative effectiveness research. RWE helps researchers understand the use of treatment for rare types of cancer more quickly than clinical trials can, and it helps researchers understand the long-term impact of cancer treatment. Researchers can make better choices due to the use of RWE, which is combined with real-world imaging data and clinical/genomic data, and therefore speed up drug development and establish personalized treatment strategies for patients with cancer.


‍


---


‍


# References


1. Unger JM, Shulman LN, Facktor MA, Nelson H, Fleury ME. National Estimates of the Participation of Patients With Cancer in Clinical Research Studies Based on Commission on Cancer Accreditation Data. Journal of clinical oncology \[Internet\]. 2024 Apr 2;42(18). Available from:[https://doi.org/10.1200/JCO.23.01030](https://doi.org/10.1200/JCO.23.01030)
2. FDA. Real-World Evidence \[Internet\]. U.S. Food and Drug Administration. 2019. Available from:[https://www.fda.gov/science-research/science-and-research-special-topics/real-world-evidence](https://www.fda.gov/science-research/science-and-research-special-topics/real-world-evidence)
3. Smith-Bindman R, Chu PW, Azman Firdaus H, Stewart C, Malekhedayat M, Alber S, et al. Projected Lifetime Cancer Risks From Current Computed Tomography Imaging. JAMA Internal Medicine \[Internet\]. 2025 Apr 14;185(6). Available from:[https://jamanetwork.com/journals/jamainternalmedicine/fullarticle/2832778](https://jamanetwork.com/journals/jamainternalmedicine/fullarticle/2832778)
4. Martin O, Schaarschmidt BM, Kirchner J, Suntharalingam S, Grueneisen J, Demircioglu A, et al. PET/MRI Versus PET/CT for Whole-Body Staging: Results from a Single-Center Observational Study on 1,003 Sequential Examinations. Journal of Nuclear Medicine \[Internet\]. 2019 Dec 5;61(8):1131–6. Available from:[https://jnm.snmjournals.org/content/61/8/1131](https://jnm.snmjournals.org/content/61/8/1131)
5. Zhang C, Liang Z, Liu W, Zeng X, Mo Y. Comparison of whole-body 18F-FDG PET/CT and PET/MRI for distant metastases in patients with malignant tumors: a meta-analysis. BMC Cancer \[Internet\]. 2023 Jan 10;23(1). Available from:[https://pmc.ncbi.nlm.nih.gov/articles/PMC9830828/](https://pmc.ncbi.nlm.nih.gov/articles/PMC9830828/)
6. Granata V, Fusco R, Setola SV, Brunese MC, Di Mauro A, Avallone A, et al. Machine learning and radiomics analysis by computed tomography in colorectal liver metastases patients for RAS mutational status prediction. La radiologia medica \[Internet\]. 2024 May 18 \[cited 2026 Mar 17\];129(7):957–66. Available from:[https://doi.org/10.1007/s11547-024-01828-5](https://doi.org/10.1007/s11547-024-01828-5)
7. Zhang B, Wang K, Xu T, Zhu H, Wang K, Wang J, et al. Deep learning model for predicting the RAS oncogene status in colorectal cancer liver metastases. Journal of Cancer Research and Therapeutics \[Internet\]. 2025;21(2). Available from:[https://journals.lww.com/cancerjournal/fulltext/2025/05000/deep_learning_model_for_predicting_the_ras.5.aspx](https://journals.lww.com/cancerjournal/fulltext/2025/05000/deep_learning_model_for_predicting_the_ras.5.aspx)
8. Mikhael PG, Wohlwend J, Yala A, Karstens L, Xiang J, Takigami AK, et al. Sybil: A Validated Deep Learning Model to Predict Future Lung Cancer Risk From a Single Low-Dose Chest Computed Tomography. Journal of Clinical Oncology \[Internet\]. 2023 Jan 12;41(12). Available from: https://doi.org/10.1200/JCO.22.01345
9. He W, Huang W, Zhang L, Wu X, Zhang S, Zhang B. Radiogenomics: bridging the gap between imaging and genomics for precision oncology. MedComm \[Internet\]. 2024 Sep 1;5(9). Available from:[https://pmc.ncbi.nlm.nih.gov/articles/PMC11381657/](https://pmc.ncbi.nlm.nih.gov/articles/PMC11381657/)
10. Lerro CC, Bradley MC, Forshee RA, Rivera DR. The Bar Is High: Evaluating Fit-for-Use Oncology Real-World Data for Regulatory Decision Making. JCO Clinical Cancer Informatics \[Internet\]. 2024 Mar \[cited 2025 May 10\];(8). Available from:[https://ascopubs.org/doi/10.1200/CCI.23.00261](https://ascopubs.org/doi/10.1200/CCI.23.00261)
11. Eisenhauer EA, Therasse P, Bogaerts J, Schwartz LH, Sargent D, Ford R, et al. New response evaluation criteria in solid tumours: Revised RECIST guideline (version 1.1). European Journal of Cancer \[Internet\]. 2009;45(2):228–47. Available from:[https://pubmed.ncbi.nlm.nih.gov/19097774/](https://pubmed.ncbi.nlm.nih.gov/19097774/)
12. Woznicki P, Fabian Christopher Laqua, Al-Haj A, Bley T, Baeßler B. Addressing challenges in radiomics research: systematic review and repository of open-access cancer imaging datasets. Insights into imaging \[Internet\]. 2023 Dec 12 \[cited 2024 May 13\];14(1). Available from:[https://pmc.ncbi.nlm.nih.gov/articles/PMC10716101/](https://pmc.ncbi.nlm.nih.gov/articles/PMC10716101/)
