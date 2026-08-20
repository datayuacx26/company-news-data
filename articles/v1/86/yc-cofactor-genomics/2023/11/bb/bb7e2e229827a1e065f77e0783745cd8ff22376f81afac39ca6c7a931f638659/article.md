---
schema_version: "1.0.0"
document_id: "bb7e2e229827a1e065f77e0783745cd8ff22376f81afac39ca6c7a931f638659"
company_key: "yc-cofactor-genomics"
company: "Cofactor Genomics"
source_id: "yc-cofactor-genomics-rss-8d01d31e56b4"
canonical_url: "https://link.springer.com/article/10.1007/s00432-023-05205-z"
published_at: "2023-11-20T22:10:29+00:00"
first_seen_at: "2026-07-20T23:20:23.761393+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:690da3f191df28c817ce79af3e037daf4c3cd1d994ff7515ac45c981aeb107e3"
---

# Multidimensional biomarker predicts disease control

## Background


Head and Neck Cancer (HNC) is the seventh most common cancer worldwide with 890,000 new cases and 450,000 deaths per year (Pulte and Brenner[2010](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR21) ; Chow[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR5) ). Despite advances in diagnosis and treatment in the curative setting, recurrent or metastatic disease (or both) develops in more than 65% of patients with squamous cell cancer of the head and neck (HNSCC; Argiris et al.[2008](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR1) ).


Based on a 2008 clinical trial, the standard of care for recurrent and metastatic squamous cell carcinoma of the head and neck (RM-HNSCC) was platinum-based chemotherapy along with 5-fluorouracil and the EGFR inhibitor cetuximab (EXTREME regimen; Vermorken et al.[2008](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR29) ; Sacco and Cohen[2015](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR23) ; Le et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR17) ). In 2016, the anti-PD-1 therapies pembrolizumab and nivolumab were approved for the treatment of platinum-pretreated RM-HNSCC, providing meaningful improvement in the standard of care for a subset of patients (Larkins et al.[2017](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR16) ; Ferris et al.[2016](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR10) ; Muro et al.[2016](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR19) ). In 2019, approval was extended to pembrolizumab as first-line treatment both as a monotherapy and in combination with platinum-based chemotherapy (combination therapy; Cohen et al.[2019a](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR6) ). Today, the standard of care for most patients is first line anti-PD-1 therapy for RM-HNSCC and second line anti-PD-1 therapy for platinum-pretreated RM-HNSCC (Pfister et al.[2023](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR20) ). Unfortunately, only 13–25% of patients’ tumors respond to anti-PD-1 monotherapy and 36% of patients respond to combination therapy (Le et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR17) ). Identifying those patients who will benefit from anti- PD-1 therapy has been an important goal, primarily utilizing the on-label companion diagnostic (CDx), a PD-L1 immunohistochemistry (IHC) assay.


PD-L1 IHC is a widely used biomarker for predicting clinical benefit in response to PD-1 inhibitors across many cancer types. Unfortunately, PD-L1 IHC testing in HNSCC, as in other cancer types, has relatively high sensitivity but poor specificity for predicting response and disease control (poor positive predictive value, PPV; Lu et al.[2019](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR18) ; Burtness et al.[2019](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR3) ; Brockstein and Vokes[2023](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR2) ). KEYNOTE-048 investigated whether pembrolizumab, as a monotherapy or in combination with chemotherapy, improved overall survival (OS) compared with cetuximab plus chemotherapy in participants with previously untreated RM-HNSCC (Burtness et al.[2019](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR3) ). In this study, PD- L1 IHC was used with combined positive score (CPS) thresholds of CPS ≥ 20 and CPS ≥ 1. Monotherapy anti-PD-1 patients with CPS ≥ 20 had a 53% Disease Control Rate (DCR), and patients with CPS ≥ 1 had a 47% DCR, compared to a 44% DCR in the total patient population, regardless of CPS. Thus, PD-L1 IHC had only a marginal benefit in predicting clinical benefit for pembrolizumab monotherapy.


Because current biomarkers like PD-L1 IHC have a low PPV, clinicians are increasingly using combination therapy as the default treatment (Le et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR17) ). In KEYNOTE-048, the combination chemotherapy plus pembrolizumab cohort had a DCR of 64%, higher than pembrolizumab monotherapy regardless of PD-L1 status (Burtness et al.[2019](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR3) ). However, the risk of grade 3–5 adverse events is far higher with combination therapy or EXTREME when compared to monotherapy (Cohen et al.[2019b](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR7) ; Burtness et al.[2019](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR3) ). Therefore, to improve patient outcomes and better manage toxicity risks, it is important that a diagnostic accurately identify the subset of patients who will respond to monotherapy.


Because of the negative impact on patients and the increased healthcare burden, there is a strong need for more robust and effective methods of predicting disease control in response to PD-1 inhibitors. Building on our previous work (LaFranzo et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR15) ; Schillebeeckx et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR24) ,[2022](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR25) ), we developed an RNA-sequencing-based classifier called OncoPrism®-HNSCC to predict disease control with increased sensitivity and specificity compared to PD-L1 IHC in patients with RM-HNSCC treated with anti-PD-1 monotherapy. OncoPrism-HNSCC requires as little as two 10 μm formalin-fixed, paraffin-embedded HNSCC tumor tissue sections and classifies patients according to their likelihood of progressing on anti-PD-1 monotherapy with high sensitivity and specificity.


## Methods


### Study design and participants


Patients were recruited from eleven study sites across the USA: Washington University in St. Louis (St. Louis, MO), University of California San Diego (San Diego, CA), MultiCare Institute for Research and Innovation (Tacoma, WA), Ochsner Lafayette General Medical Center (Lafayette, LA), Dayton Physicians Network (Dayton, OH), Intermountain Healthcare (Salt Lake City, UT), Brooke Army Medical Center (Fort Sam Houston, Texas), Baylor College of Medicine (Houston, TX), Cancer Care Northwest (Spokane, WA), Mayo Clinic (Rochester, MN), and Northwest Oncology and Hematology (Hoffman Estates, IL).


Patients were enrolled in the retrospective, observational study following the inclusion and exclusion criteria outlined below. Eligible patients had recurrent or metastatic histologically or cytologically confirmed HNSCC and were treated with anti-PD-1 monotherapy as the first-line treatment for their recurrent or metastatic disease. Tissue analyzed in the study was from tumors biopsied prior to treatment and was formalin-fixed and paraffin embedded using standard protocols. De-identified, FFPE pre-treatment tumor biopsy specimens were provided to Cofactor Genomics for OncoPrism-HNSCC and PD-L1 IHC analysis. The 10% of samples with the longest time between biopsy and treatment (> 22.4 months) were removed from the study. Following anti-PD-1 treatment, patients must have had clinician-evaluated tumor response to immunotherapy using clinical assessment (RECIST, PERCIST, or other clinical criteria as appropriate in standard of care) to calculate disease control rate (DCR). Exclusion criteria included immunotherapy received in combination with any other therapy modality including radiation therapy, platinum-based chemotherapy, or taxane. Patients with insufficient tissue for analysis were also excluded from the study. Primary or metastatic tumor specimens were accepted, but metastatic tumors from bone or liver were not included.


The study protocol, “A Multicenter Cancer Biospecimen Collection Study” is registered as “NCT04510129—Predicting Immunotherapy Efficacy From Analysis of Pre-treatment Tumor Biopsies (PREDAPT)” on clinicaltrials.gov. The study protocol was approved by institutional review boards at either the study (WCG IRB) or site level, as appropriate. All patients provided signed, informed consent to participate, or consent was waived for deceased patients according to study protocol. Independent data monitoring was conducted by the study clinical research organization Curebase, Inc (San Francisco, CA).


### RNA extraction


RNA was extracted using RNAstorm (Cell Data Sciences, Fremont, CA) according to the manufacturer’s instructions. RNA quantity was assessed by the High Sensitivity RNA Qubit assay (Thermo Fisher Scientific, Waltham, MA). A predefined yield of 40 ng FFPE RNA was used as the minimum QC threshold. Quality of the RNA was assessed using a bioanalyzer (Agilent Technologies, Santa Clara, CA), and a DV200 of 20% or greater was used as the minimum threshold.


### Library preparation and sequencing


Libraries were prepared using the QuantSeq 3’ mRNA-Seq Library Prep Kit FWD for Illumina (Lexogen, Inc., Greenland, NH), following the manufacturer’s instructions with the protocol alterations noted as follows. RNA input into library preparation was 40 ng for all samples. UMI Second Strand Synthesis Module for QuantSeq FWD (Lexogen, Inc., Greenland, NH) replaced Second Strand Synthesis Mix 1 in the workflow. All samples were processed with an OncoPrism-HNSCC Positive Control, OncoPrism-HNSCC Negative Control, and a No Template Control. The Positive (high scoring) and Negative (low scoring) controls were RNA extracted from RM-HNSCC samples as described above. Final libraries were sequenced to a minimum depth of 10 million single-end 75 base pair reads on a NextSeq500 (Illumina, San Diego, CA), following the manufacturer’s protocols.


### Immunohistochemistry


PD-L1 staining was performed by Mosaic Labs (Lake Forest, CA) using the 22C3 pharmDx antibody (Agilent Technologies, Inc., Santa Clara, CA) or NeoGenomics Laboratories (Fort Myers, FL) using the PD-L1 22C3 FDA (KEYTRUDA®) for HNSCC Head and Neck stain. CPS assessment was performed by W.H.W. or by NeoGenomics. H&E staining was performed by NeoGenomics as part of the PD-L1 22C3 test or at Cofactor Genomics using xylene substitute Slide Brite (Newcomer Supply, Middleton, WI), as detailed by manufacturers. Samples were assessed for tumor purity by a board- certified pathologist (EJD), with a minimum 10% tumor cellularity required for inclusion.


### Processing of RNA sequencing data


FASTQ files were preprocessed with trim_galore/cutadapt version 0.4.1 to remove adapter sequences as well as reads with PHRED quality scores < 20 and reads that were < 20 bp. The trimmed reads were aligned to the human genome GRCh38 with STAR version 2.5.2a using the two-pass method as previously described (Schillebeeckx et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR24) ). Read counts were generated using htseq-count version 0.9.1 and annotation from Gencode version 22 (Schillebeeckx et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR24) ). Only samples with a minimum of 30% exonic alignment and at least 800,000 unique deduplicated counts were included in the study.


### Feature selection and biomarker training


103 patients were used to train the OncoPrism-HNSCC biomarker. As described previously, forward feature selection was performed with a logistic regression on 62 immunomodulatory features to generate candidate biomarkers (Fig.[1](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig1) C and Table S1; Schillebeeckx et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR24) ,[2022](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR25) ). Log2 values between 10–3 and 102 for the regularization parameter C were considered.


**Fig. 1**[Full size image](https://link.springer.com/article/10.1007/s00432-023-05205-z/figures/1)


RNA-seq data was used to build the biomarker. **A** A multidimensional biomarker was trained using tumor specimen RNA-seq gene expression data and clinical response data from 103 HNSCC patients. Logistic regression and forward feature selection were used to integrate the data (“Predictive Immune Modeling”) and produce a multidimensional biomarker. **B** Patients were segregated by outcome and evaluated for progression free survival (PFS). CR, complete response; PR, partial response; SD, stable disease, PD, progressive disease. CR, PR, and SD together constitute “non-progressors”. PD patients are “progressors”. **C** Candidate feature relative abundance is shown for each patient sample. Features are gene expression data or gene signatures associated with specific immune cell types. Patient samples are sorted by outcome label (true outcome), then by OncoPrism Score and PD-L1 IHC prediction


Candidate biomarkers were considered for their analytical reproducibility (variance) and clinical performance (area under the curve, AUC), and the final model was chosen to balance the two. Briefly, nine replicates from 6 patients (54 samples) were used to measure the variance of scores of candidate biomarkers. OncoPrism Scores for each replicate in the analytic variability dataset were generated and mean centered for each biological sample. A single measure of variability for each candidate model, defined as 4 times the standard deviation of the mean centered OncoPrism Scores (4*std), was used to evaluate variance. The final model was chosen to maximize clinical performance (AUC ≥ 0.76) while minimizing analytical variability (4*std ≤ 20).


Training and cross validation was performed using observational FFPE samples from patients treated with monotherapy pembrolizumab or nivolumab. The clinical cut-offs for the test system were established using the two endpoints identified for the clinical study: clinical outcome (CR, PR, SD, and PD, defined by RECIST criteria; Eisenhauer et al.[2009](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR9) ) and OS (median months survived from the start of therapy to death or last contact). CR, PR, and SD patients were considered “non-progressors” while PD patients were considered “progressors”.


Using a bootstrapping approach, scores were generated for each patient to evaluate biomarker performance and choose a threshold. Considering *n* unique patient samples, patients are chosen *n* times with replacement and used for training a model. Using this trained model, a score (“out-of-bag score”) is generated for the remainder patients. This process was done 1000 times, and the out-of-bag score of each patient was averaged to generate the final OncoPrism Score. This method minimizes bias and therefore best approximates future performance in an independent patient cohort.


### OncoPrism scores and prediction


The OncoPrism-HNSCC biomarker generates an OncoPrism Score from 0 to 100 that correlates with disease control and OS of HNSCC patients treated with anti-PD-1 monotherapy treatment. Higher OncoPrism Scores represent higher confidence by the model that the patient will be a non-progressor. The median score (46) was used to set the threshold between predicted progressor (scores less than the median) and predicted non-progressors (scores greater than the median).


### Statistics


Statistical analysis was performed using R (R Core Team[2021](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR22) ). Heatmap was made using the “ComplexHeatMap” and “circlize” packages (Gu et al.[2014](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR11) ,[2016](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR12) ). Survival figures and analysis were done using the “survminer” and “survival” packages, and significance was determined using log rank methods (Therneau and Grambsch[2000](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR28) ; Therneau[2023](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR27) ). DCR across quartiles, biomarker predictions and other groups, overall response rate (ORR), sensitivity, and specificity were compared using Fisher’s exact test.


## Results


### Patient selection


Pre-treatment FFPE tumor samples were collected from the 172 patients with RM-HNSCC enrolled under the internal review board-approved PREDAPT protocol. Thirty patients were removed from the study for pre-analytical reasons such as incomplete data or unacceptable tissue type. All remaining patients received at least one dose of anti-PD-1 monotherapy (following sample collection) and had documented anti-PD-1 outcome data. Following tumor cellularity estimation, RNA extraction, library preparation, and sequencing, 103 samples from 11 clinical sites met all inclusion/exclusion and quality control criteria (Table S2). Table[1](https://link.springer.com/article/10.1007/s00432-023-05205-z#Tab1) compares the patient population for this study (“OncoPrism”) with two other large ICI studies in RM-HNSCC (Hanna et al.[2018](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR13) ; Burtness et al.[2019](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR3) ).


**Table 1 Patient data (n=103)**[Full size table](https://link.springer.com/article/10.1007/s00432-023-05205-z/tables/1)


### Biomarker development


These 103 samples served as the training set for the development of the biomarker. RNA-seq data were analyzed with a proprietary analysis pipeline to integrate gene expression signatures associated with eight immune cell types (Schillebeeckx et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR24) ) and five additional T cell states (Schillebeeckx et al.[2022](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR25) ), as well as other immune related gene expression data in each sample (Fig.[1](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig1) A). Patients were grouped according to disease control status as determined by clinical assessment at 8–10 weeks (Eisenhauer et al.[2009](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR9) ). Non-progressors were patients with complete response (CR), partial response (PR), or stable disease (SD). Progressors were patients with progressive disease (PD). Median progression free survival (PFS) for CR, PR, SD, and PD was undetermined, 8.4, 6.8, and 1.8 months, respectively (Fig.[1](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig1) B). The gene expression data and disease control label served as input to a machine-learning based approach we term Predictive Immune Modeling to generate a multidimensional biomarker that predicts disease control for anti-PD-1 monotherapy in RM-HNSCC. Sixty-two candidate feature genes were pre-selected (Fig.[1](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig1) C and Table S1; Schillebeeckx et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR24) ,[2022](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR25) ) to minimize the risk of over-fitting, and a forward-feature selection method was used to select the features in the final model (see methods).


### Performance of biomarker


To minimize inaccuracies and biased performance associated with the more traditional approach of segregating a single training and a single validation cohort in a patient pool of 103 samples, bootstrapping cross validation was used to best estimate future performance (Efron[1979](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR8) ). By maximizing the size of the training set, we better capture the diversity of the patient population. Considering *n* unique patient samples, patients are chosen *n* times with replacement and used for training a model. Using this trained model, a score (“out-of-bag score”) is generated for the remainder of patients. This process was done 1000 times, and the out-of-bag score of each patient was averaged to generate the final OncoPrism Score. The OncoPrism Score, a value from 0 to 100, indicates the likelihood of non-progression with anti-PD-1 monotherapy treatment.


A receiver operating characteristic (ROC) curve was generated to assess the performance of the biomarker across all thresholds (Fig.[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig2) A, orange line). The area under the curve (AUC) was 0.76. An AUC of 1 represents perfect performance while an AUC of 0.5 represents performance equal to random chance. In contrast, the traditional, single analyte on-label diagnostic, PD-L1 IHC, had an AUC of 0.65 (n = 100; Fig.[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig2) A, gray line). Next we looked at the correlation between patient OncoPrism Score and disease control (Fig.[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig2) B). The likelihood of non-progression increased as the OncoPrism Score increased. This correlation between OncoPrism Score and disease control demonstrates the power of the biomarker to predict disease control with anti-PD-1 monotherapy in a continuous manner. To quantitate this relationship, we divided OncoPrism Scores into quartiles and measured the Disease Control Rate (DCR) in each quartile (Fig.[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig2) C). The overall DCR for all quartiles was 41%. DCR was 12% in the lowest quartile (OncoPrism Scores 0–35), 23% in the second quartile (OncoPrism Scores 36–46), 64% in the third quartile (OncoPrism Scores 47–62), and 65% in the highest quartile (OncoPrism Scores 63–100). Relative to the DCR for all patients, non-progressors were significantly underrepresented in the first quartile and significantly enriched in the third and fourth quartiles (Fisher’s exact test, p < 0.01).


**Fig. 2**[Full size image](https://link.springer.com/article/10.1007/s00432-023-05205-z/figures/2)


Higher OncoPrism Scores are associated with disease control with anti-PD-1 therapy **A** ROC curve showing the performance of OncoPrism-HNSCC compared to PD-L1 IHC across all thresholds. An out-of-bag (OOB) method was used to assess performance. OncoPrism-HNSCC (orange) has an area under the curve (AUC) of 0.76, while PD-L1 IHC (grey) has an AUC of 0.65. The dashed line (red) is performance equivalent to chance. **B** Patients were ranked along the x-axis according to their OncoPrism Score (y-axis) and colored according to their actual clinical outcome (progressors = grey, non-progressors = orange) **C** Samples were ranked according to their OncoPrism Score and divided into quartiles for assessment. The disease control rate (DCR) for all patients is represented by the solid line. The blue bars represent the actual DCR for each quartile not a mean. Significant differences among quartiles were determined using Fisher’s Exact test (*p < 0.05, **p < 0.01, ***p < 0.001)


### Development of the OncoPrism-HNSCC test utilizing biomarker


RM-HNSCC patients are commonly treated with anti-PD-1 plus platinum-based chemotherapy (combination therapy) by default (Le et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR17) ). We sought to develop a test to identify patients who will benefit from anti-PD-1 monotherapy. The biomarker and OncoPrism Score described above provide the foundation for OncoPrism-HNSCC, a test to guide this treatment decision. Because the upper two quartiles of patients had significantly higher DCR than the lower two quartiles (p < 0.01; Fig.[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig2) C), we used the median score (46) as the threshold for predicting disease control. The first and second quartiles combined constituted predicted progressors, while the third and fourth quartiles combined represented predicted non-progressors. This median threshold balances the predictive power of the test with the size of the patient population for whom the test would improve outcomes relative to existing biomarkers and standards of care.


Using this categorical labeling, the DCR for predicted non-progressors was 65%, significantly higher than the DCR of 17% for predicted progressors (Fisher’s exact test, p < 0.001; Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) A). The relative risk of progression for predicted progressors relative to non-progressors was 2.34 (p < 0.001). Although we designed the model to predict disease control, Overall Response Rate (ORR) was also significantly higher in predicted non-progressors (41%) compared to predicted progressors (13%; p = 0.002; Fig. S1). Importantly, predicted non-progressors also had significantly longer overall survival (OS; median = 13.7 months) than predicted progressors (median = 7.9 months; Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) B; p = 0.004).


**Fig. 3**[Full size image](https://link.springer.com/article/10.1007/s00432-023-05205-z/figures/3)


OncoPrism-HNSCC predicts disease control and overall survival (OS) better than PD-L1 IHC. **A** DCR is significantly higher for OncoPrism-HNSCC predicted non- progressors than for OncoPrism-HNSCC predicted progressors (p < 0.001). DCR for PD- L1 CPS ≥ 20 (“non-progressor”) is significantly higher than CPS < 20 (“progressor”; p = 0.02). Bars represent the actual DCRs not a mean. Significant differences among quartiles were determined using Fisher’s Exact test. **B** OncoPrism-HNSCC predicted non-progressors have significantly longer OS than predicted progressors (p = 0.004; n = 103). OS is measured from the time of first anti-PD-1 treatment. **C** PD-L1 IHC CPS ≥ 20 patients (corresponding to a PD-L1 predicted non-progressor) do not have longer OS than CPS < 20 (p = 0.7; n = 100). OS is measured from the time of first anti-PD-1 treatment


### OncoPrism-HNSCC is more predictive than PD-L1 IHC


OncoPrism-HNSCC performed well compared to the existing anti-PD-1 CDx, PD-L1 IHC. PD-L1 IHC and quantitation was performed on 100 samples and used to evaluate PD-L1 IHC performance. Pembrolizumab is approved as a first-line therapy for patients with a PD-L1 IHC combined positive score greater than or equal to one (CPS ≥ 1). Samples with a PD-L1 IHC CPS ≥ 1 had lower DCR (44%) than OncoPrism-HNSCC predicted non-progressors (65%; p = 0.02; compare Table S3 to Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) A). In practice, CPS ≥ 20 is a more common threshold than CPS > 1 for monotherapy treatment decisions (Pfister et al.[2023](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR20) ). Even with this higher PD-L1 threshold (CPS ≥ 20), the DCR for PD-L1 IHC increases only marginally to 51%, compared to 27% for CPS < 20 (p = 0.02; Table S3).


Additionally, OncoPrism-HNSCC outperformed PD-L1 CPS ≥ 20 across multiple metrics, including higher sensitivity (0.79 vs 0.64, p = 0.005) and specificity (0.70 vs 0.61, p = 0.009; Table[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Tab2) ). While the OncoPrism-HNSCC and PD-L1 predictions were concordant for most samples (73/100), for samples where the tests disagreed, PD-L1 IHC had more false positives (12) and false negatives (7) than OncoPrism-HNSCC (6 and 2, respectively; Table S4).


**Table 2 Performance metrics**[Full size table](https://link.springer.com/article/10.1007/s00432-023-05205-z/tables/2)


Unlike with OncoPrism-HNSCC, patients with PD-L1 CPS ≥ 20 did not have longer OS (Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) C). We used the Cox proportional hazards model to assess the relationships between OncoPrism-HNSCC prediction, PD-L1 IHC CPS category, and survival in the 100 samples with OncoPrism-HNSCC, PD-L1, and survival data (Fig.[4](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig4) ). Relative to OncoPrism-HNSCC predicted progressors, predicted non-progressors had a hazard ratio (HR) of 0.51 (p < 0.01), indicating a significant positive association with survival. For PD-L1 IHC, relative to CPS < 20 the HR for CPS ≥ 20 was 1.18 (p = 0.51), indicating no association with survival.


**Fig. 4**[Full size image](https://link.springer.com/article/10.1007/s00432-023-05205-z/figures/4)


Cox proportional hazards model of overall survival (OS). Patients with complete OncoPrism-HNSCC and PD-L1 data were included (n = 100). OncoPrism-HNSCC predicted non-progressors have a Hazard Ratio of 0.51 relative to predicted progressors (p = 0.008). There is no significant difference in OS between PD-L1 < 20 and PD-L1 ≥ 20


### OncoPrism-HNSCC performance is similar across RM-HNSCC subtypes


Although the study was not powered to statistically evaluate sub-groups, OncoPrism- HNSCC performance was qualitatively similar across clinical variables including primary tumor site and HPV status. While the overall DCR varied across primary tumor sites, in each site the predicted non-progressors had a higher DCR than predicted progressors (Table S5). Likewise, while the DCR in HPV-positive (p16 +) patients was higher than in HPV-negative (p16-) patients, as expected (Weinberger et al.[2006](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR30) ; Singhi and Westra[2010](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR26) ; Cai et al.[2014](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR4) ; Pfister et al.[2023](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR20) ), OncoPrism-HNSCC performance was similar regardless of p16 status (Table S6). Additionally, we used Cox proportional hazards model to explore clinical factors associated with OS (Fig. S2). OncoPrism-HNSCC predicted non-progressors were positively associated with survival. Eastern Cooperative Oncology Group (ECOG) score of 2 and a positive or unknown p16 status were negatively associated with survival. PD-L1 status, overall stage, smoker status, and primary tumor site were not correlated with survival.


## Discussion


OncoPrism-HNSCC predicts disease control and OS in response to anti-PD-1 monotherapy in a 103 patient dataset from 11 clinical sites using out-of-bag methods (Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) A–B). In addition, although OncoPrism-HNSCC was trained to predict disease control, it also significantly predicts overall response (Fig. S1). While PD-L1 IHC was also moderately predictive of DCR, PD-L1 did not predict overall response or OS in our study population (Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) A and C; Fig. S1). OncoPrism-HNSCC also had significantly higher sensitivity and specificity than PD-L1 IHC (Table[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Tab2) ).


Current biomarkers in RM-HNSCC are only moderately predictive, causing clinicians to treat more aggressively with combination therapy (anti-PD-1 therapy combined with platinum-chemotherapy) than would be required with better biomarkers (Chow[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR5) ; Le et al.[2020](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR17) ). In KEYNOTE-048, patients with PD-L1 IHC CPS ≥ 20 had only moderately higher disease control with pembrolizumab monotherapy than all monotherapy patients regardless of PD-L1 status (DCR of 53% vs 44%, respectively; Burtness et al.[2019](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR3) ). Likewise in our study, the 51% DCR in monotherapy-treated patients with CPS ≥ 20 was only moderately better than the 39% DCR in the total monotherapy-treated patient population (Table S3). Using the FDA approved monotherapy threshold of CPS ≥ 1 reduced the predictive power of PD-L1 IHC even more. Importantly, in KEYNOTE-048, the 64% DCR in all combination therapy patients was higher than the DCR to anti-PD-1 monotherapy even in patients with CPS ≥ 20 (DCR = 53%). Because the DCR for combination-treated patients (regardless of PD-L1 status) is higher than monotherapy-treated patients even with high PD-L1, many clinicians default to the more aggressive combination therapy. However, treatment with chemotherapy—with or without anti-PD-1—causes significant patient quality of life issues, increased serious adverse events, and higher healthcare costs (Ferris et al.[2016](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR10) ; Harrington et al.[2017](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR14) ; Brockstein and Vokes[2023](https://link.springer.com/article/10.1007/s00432-023-05205-z#ref-CR2) ).


With any biomarker used to make patient treatment decisions, it is important to avoid withholding an effective treatment from patients who would benefit. OncoPrism-HNSCC has a higher sensitivity than PD-L1 IHC (0.79 vs. 0.64, p = 0.005; Table[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Tab2) ), corresponding to a lower number of false negatives wherein the test incorrectly predicts progression. In this study PD-L1 IHC had 14 false negatives compared to just 9 for OncoPrism-HNSCC (Table S4). Seven of these false negatives were common to both tests, with just two false negatives unique to OncoPrism-HNSCC. In addition, seven patients who are PD-L1 IHC false negatives were correctly predicted as non-progressors by OncoPrism-HNSCC. While there remain individual patients who would not receive the optimal therapy with OncoPrism-HNSCC, there are fewer such patients when following the OncoPrism-HNSCC prediction compared to the existing PD-L1 IHC prediction. Future work will focus on further reducing false negatives.


OncoPrism-HNSCC predicts with high specificity patients who will have disease control in response to monotherapy, potentially enabling clinicians to avoid overtreatment in a subset of patients. In our dataset, monotherapy-treated OncoPrism-HNSCC-predicted non-progressors had a DCR of 65% compared to just 51% for CPS ≥ 20 (Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) A). Future studies should compare monotherapy and combination therapy outcomes among predicted progressors and non-progressors. In contrast to OncoPrism-HNSCC predictions, PD-L1 expression was only moderately correlated with monotherapy DCR in our study (Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) A and Table S3), and not at all correlated with OS (Fig.[3](https://link.springer.com/article/10.1007/s00432-023-05205-z#Fig3) C). Further, the data from this study indicate that relative to CPS ≥ 20, OncoPrism-HNSCC provides superior sensitivity (0.79 vs 0.64, p = 0.005) and specificity (0.70 vs 0.61, p = 0.009) in monotherapy patients relative to PD-L1 IHC (Table[2](https://link.springer.com/article/10.1007/s00432-023-05205-z#Tab2) ). While these results are limited by the lack of an independent validation set, additional studies are ongoing to validate these results in additional independent cohorts of patients.


## Conclusions


OncoPrism-HNSCC is a novel multidimensional RNA-seq biomarker-based clinical test that predicts DCR, ORR, and OS in monotherapy anti-PD-1-treated RM-HNSCC patients. OncoPrism-HNSCC fills a clinical need in the treatment of HNSCC by identifying with high sensitivity and specificity patients who may benefit from monotherapy. This ability to better predict patient disease control to anti-PD-1 monotherapy provides an opportunity to give patients the most effective treatment option and avoid unnecessary chemotherapy.
