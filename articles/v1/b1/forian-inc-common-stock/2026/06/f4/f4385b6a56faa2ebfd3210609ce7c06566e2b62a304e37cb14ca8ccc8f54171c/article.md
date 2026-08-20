---
schema_version: "1.0.0"
document_id: "f4385b6a56faa2ebfd3210609ce7c06566e2b62a304e37cb14ca8ccc8f54171c"
company_key: "forian-inc-common-stock"
company: "Forian Inc."
source_id: "forian-inc-common-stock-rss-1c01cf5ce14b"
canonical_url: "https://blog.forian.com/same-closed-claims-more-complete-picture-the-power-of-hybrid-claims-data"
published_at: "2026-06-30T15:00:02+00:00"
first_seen_at: "2026-07-20T23:19:46.527638+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:e6c083c3be0b62adc41d99356bda0d1e49cebafc96f8dbeaad08a4c9df4d4b7a"
---

# Same Closed Claims, More Complete Picture: The Power of Hybrid Claims Data

It is well established across HEOR, RWE, and Commercial Analytics that closed claims provide a complete view into a patient’s healthcare resource utilization while enrolled in their insurance plan. Sourced directly from payers, researchers also find immense value in the monthly enrollment files closed claims offer, as this data ensures that patients in a study are active. The claims themselves are fully adjudicated as well, meaning researchers trust the codes on claim have been validated by the payer.


However, for studies focused on describing the journey and outcomes of a patient cohort, it is imperative to remember claims are validated by the payer for


**reimbursement**


**to the provider.** Put simply, they are scrutinized such that the healthcare payer believes the CPT/HCPCS codes correctly represent the service provided to the patient, and the ICD-10 codes accurately described a condition(s) which necessitated said service. The goal is not necessarily to ensure that the claim includes all pertinent information for an HEOR analysis. Therefore, it’s worth asking whether any valuable data is lost in the process. To explore this question, we compared how the exact same encounters are represented in open versus closed claims, with the goal of determining which source provides the most complete and useful information for research.


*This blog is part of a series on hybrid claims data and real-world evidence. Read the first post*[here](https://blog.forian.com/hybrid-claims-data-essential-strategy-rwe-heor-studies) *.*


### **What Closed Claims Misses, Hybrid Recovers**


To ensure our findings were broadly applicable, we analyzed patients across three care categories (Acute, Chronic, Mental) spanning nine total indications.


Acute


-


Type 1 Diabetic Ketoacidosis (T1DKA)


-


Orbital Cellulitis


-


Pancreatitis


Chronic


- Amyotrophic Lateral Sclerosis (ALS)


- Chronic Kidney Disease (CKD) Stages 4-5


- Chronic Obstructive Pulmonary Disease (COPD)


Mental


- Obsessive Compulsive Disorder (OCD)


- Borderline Personality Disorder (BPD)


- Major Depressive Disorder treated with Electroconvulsive Therapy (MDD w/ ECT)


**Cohort Selection**


- **Chronic and mental health conditions** required at least three occurrences of the applicable ICD-10 codes, with a minimum of six months between the first and last.


- **Acute conditions** required at least two qualifying claims, without the longitudinal requirement.


- For all conditions, patients were required to have a


**t least one year of lookback and one year of follow up** , either through open claims activity or continuous enrollment in closed claims (except for MDD, where we did not require the lookback).


- Cohorts were limited to patients


**whose index dates matched exactly** in both the open and closed claims datasets.


Category


#


Patients


Inclusion


Criteria


Exclusion Criteria


Acute


T1DKA


234


ICD-10 E10.1X


ICD-10 E11.XX


Orbital Cellulitis


792


ICD-10 H05.01X


-


Pancreatitis


17,076


ICD-10 K85.XX


-


Chronic


ALS


257


ICD-10 G12.21


ICD-10 G10, ICD-10 G20X


CKD Stages 4-5


14,339


ICD-10 N18.4, N18.5, N18.6


-


COPD


37,974


ICD-10 J44.XX


-


Mental


OCD


6,693


ICD-10 F42.X


ICD-10 F60.5


BPD


4,338


ICD-10 F60.3


ICD-10 F60.2


MDD w/ ECT


401


ICD-10 F33 + CPT 90870


-


Following cohort selection, we examined a 365-day period after each patient’s index date and identified claims that were present in both open and closed claims. A “claim” was defined as a unique combination of patient identifier, service/statement from and to dates, and claim type (institutional or professional).


Below is the total number of overlapping claims found per indication.


Indication


# of Open Claims


# of Closed Claims


# of Overlapping Claims


Acute


T1DKA


3,341


4,272


2,839


Orbital Cellulitis


16,667


26,909


14,052


Pancreatitis


441,932


676,018


357,855


Chronic


ALS


8,640


12,205


6,529


CKD Stages 4-5


555,177


956,240


466,783


COPD


1,105,897


1,798,713


947,446


Mental


OCD


162,350


210,977


134,270


BPD


142,297


181,948


117,177


MDD w/ ECT


14,926


20,587


12,138


On each of these overlapping claims, we looked at the differences in variables that are most commonly used to describe cohorts and define outcomes:


- Diagnosis Codes (ICD10)


- Procedure Codes (CPT/HCPCS)


- Place of Service Codes


- Type of Bill Codes


- Provider NPI


##


## **Diagnosis Codes Are Largely Consistent Across Claims Sources**


*(72%-85% unchanged depending on indication category)*


Across all indications, changes in diagnosis codes were relatively uncommon when translating an encounter from open claims to closed claims. When differences occurred, closed claims were most often a superset of open claims, meaning the sequence of diagnoses contained all codes present on the open claim plus additional codes.


In acute and chronic conditions, this occurs around twice as often as the inverse (Codes Removed + Completely Obscured) where open claims are a superset of closed (16% vs 9% and 14% vs 7% respectively). However, the difference is less pronounced in Mental Conditions (7% vs 6%). The most common codes which were changed between the claim sources are shown below for patients in the CKD, MDD w/ ECT, and Pancreatitis cohorts sorted by number of encounters where closed claims were additive.


**Patients with Late Stage CKD**


**# of Encounters**


**Code**


**Description**


**Added by Closed**


**Removed from Open**


R69


Illness, Unspecified


21,923


0


I10


Essential (Primary) Hypertension


9,360


4,971


N179


Acute Kidney Failure, Unspecified


6,588


2,327


Z6841


Body Mass Index \[Bmi\] 40.0-44.9, Adult


4,797


0


N184


Chronic Kidney Disease, Stage 4 (Severe)


4,632


2,969


**Patients w/ MDD+ECT**


**# of Encounters**


**Code**


**Description**


**Added by Closed**


**Removed from Open**


F332


Major Depressive Disorder, Recurrent Severe Without Psychotic Features


156


79


R45851


Suicidal Ideations


104


25


I10


Essential (Primary) Hypertension


86


48


F329


Major Depressive Disorder, Single Episode, Unspecified


81


69


F411


Generalized Anxiety Disorder


76


124


**Patients w/ Pancreatitis**


**# of Encounters**


**Code**


**Description**


**Added by Closed**


**Removed from Open**


R69


Illness, Unspecified


11,044


0


I10


Essential (Primary) Hypertension


6,793


3,665


K8590


Acute Pancreatitis Without Necrosis Or Infection, Unspecified


5,567


2,229


R109


Unspecified Abdominal Pain


5,099


2,530


Z6841


Body Mass Index \[BMI\] 40.0-44.9, Adult


3,699


0


In both CKD and Pancreatitis, ICD10 R69 (Illness, Unspecified) is the most commonly additive code in closed claims, followed by BMI 40.0-44.9 which appears to replace less specific codes such as morbid obesity and BMI greater than 40 (removed from ~6K open claims in the COPD cohort and ~4K in the pancreatitis cohort). However, neither code provides significant value in the context of cohorts and outcomes studies. Illness, Unspecified speaks for itself and provides limited clinical insight. While the BMI code may appear to be an improvement over the broader obesity-related codes, it still represents a range rather than a precise measurement of weight change, as would be available through an EMR source.


**Therefore, while closed claims can certainly be additive in terms of number of codes, it is important to consider the clinical relevance and utility of the codes being added.**


Based on these results, there does not appear to be a clear indication that one source is inherently “better” than the other for diagnosis codes. In the majority of cases, the diagnosis codes represented in both sources are identical. When differences do occur, the most commonly added codes do not provide meaningful additional insight. Although less frequently observed, we also see meaningful diagnosis codes being removed from open claims. For each indication evaluated, at least one


**identifying** diagnosis code was removed from open claims at a rate of more than 40% of the closed claims additions, with removals exceeding 50% in CKD and MDD (see highlighted rows), diminishing the true additive rate of closed claims.


A study which makes use of both data sources likely stands the best chance at seeing “everything” with respect to diagnosis codes.


## **Procedure Codes Show Similar Patterns with More Variation by Indication**


In both acute and mental conditions, the


p


rocedure


c


ode breakdown


looks similar to the


d


iagnosis


c


odes.


M


ost


of the time


,


the codes are unchanged, and


there is


a similar ratio of additive v


ersu


s removal for both conditions.


C


hronic conditions look different


. While still unchanged


more than


70% of the time,


encounters


w


h


ere the procedure codes on the


o


pen


c


laim are a superset of th


ose on the


c


losed


is


actually more


common


than the opposite (15% vs 12%).


This is driven


mainly by


COPD,


where the most


frequently


removed code


is a PA-Medicaid specific HCPCS code, W1793,


Attendant care


services;


per 15 minutes.


Code


Description


# of Claims where Code was present in Open but not Closed


W1793


Attendant care services; per 15 minutes


108,069


W1895


Personal Emergency Response System (Monthly Maintenance)


9,203


W1760


Home Delivered Meals-Frozen Entrée


7,991


36415


Collection Of Venous Blood By Venipuncture


2,374


G8427


Eligible Clinician Attests To Documenting In The Medical Record They Obtained, Updated, Or Reviewed The Patient's Current Medications


2,348


**Facility and Place of Service Details Frequently Unavailable in Closed Claims**


On institutional claims, Type of Bill was completely removed about 60% of the time in closed claims, in each of the indications. This introduces an additional complexity in determining place of service of the claim.


# of Encounters where Code was Present in Open but not Closed Claims


Code


Description


BPD


COPD


T1DKA


111


HOSP/INP(INCL MDCR A)/ADM/DISCH CLM


35,950


203,411


688


021


SNF/INP(INCL MDCR A)/ADM/DISCH CLM


12,384


11,585


62


231


SNF/OP/ADM/DISCH CLM


6,641


37,714


198


121


HOSP/INP(MDCR B ONLY)/ADM/DISCH CLM


5,785


220,667


542


211


SNF/INP(INCL MDCR A)/ADM/DISCH CLM


5,257


77,894


448


221


SNF/INP(MDCR B ONLY)/ADM/DISCH CLM


5,008


70,770


169


811


SPECFAC/HOSPICE(NONHOSP BASED)/ADM/DISCH CLM


3,425


32,149


53


117


HOSP/INP(INCL MDCR A)/REPLC OF PRIOR CLM


0


53,22


21


131


HOSP/OP/ADM/DISCH CLM


0


7,091


0


In the BPD, COPD, and T1DKA cohorts, the most frequently omitted Type of Bill codes corresponded to inpatient hospitals and skilled nursing facilities. This is a clear win for open claims. However, the practical implications may be less significant than they initially appear. Even without these codes, a researcher can often rely on applicable CPT/HCPCS, revenue, or DRG codes to identify the place of service. This may require some extra effort, but most HEOR teams already have a standard hierarchy of codes to use in place when determining place of service.


While these results imply open claims are unequivocally superior to closed claims for Type of Bill data, that does not necessarily diminish the value of closed claims. Researchers have access to other mechanisms for identifying place of service on institutional claims, which can help mitigate the impact of missing Type of Bill information.


**Missing Provider NPIs Can Impact Specialty-Based Research**


The final code type we analyzed was Provider NPI. Across all three indication categories, at least one Provider NPI present on the open claim was missing from the corresponding closed claim more than 50% of the time. This finding has important implications for research that relies on provider specialty information. For example, studies seeking to quantify specialist visit rates following treatment initiation, or those requiring diagnoses or procedures to be attributed to specific provider specialties, may be particularly affected.


To better understand the potential impact, we examined the specialty types associated with missing NPIs in the Orbital Cellulitis, ALS, and OCD cohorts. The five most frequently affected specialties for each indication are shown below.


Orbital Cellulitis


ALS


OCD


Specialty


# Encounters where NPI Removed


Specialty


# Encounters where NPI Removed


Specialty


# Encounters where NPI Removed


Internal Medicine


861


Neurology


478


Family Medicine


4,444


Emergency Medicine


658


Family Medicine


369


Internal Medicine


3,310


Family Medicine


654


Internal Medicine


309


Social Worker - Clinical


2,617


Pediatrics


538


Pulmonary Disease


133


Emergency Medicine


2,533


Family


214


Nurse Practitioner


86


Pediatrics


2,472


Open claims beats closed on provider NPI. Without this detail, there is no way to know the specialty of treating physicians. Imagine a study designed to quantify specialist visit rates following initiation of a new treatment, or one that requires confirmation that a diagnosis was made by a specific provider specialty. Missing NPIs could present clear challenges in either scenario, such as skewed specialist visit rates, or a lower volume of patients when requiring a diagnosis or procedure from a specific specialist type.


**The Path Forward: Leveraging Both Open and Closed Claims**


So, which source is better at representing the exact same encounter, open claims or closed claims? As is often the case in healthcare data, the answer is: it depends.


For Type of Bill, Place of Service, and Provider NPI, open claims clearly contain more information. For diagnosis and procedure codes, however, there is little evidence that one source is consistently more informative than the other. Across all indications evaluated, both sources contained the exact same sequence of codes more than 70% of the time. When differences did occur, closed claims generally added more codes, although many appeared to have limited clinical relevance. At the same time, it is worth noting that clinically meaningful diagnosis codes were also removed from open claims at a nontrivial rate.


Ultimately, each source has distinct strengths and limitations. Open claims provide clear advantages for identifying place of service, Type of Bill, and provider specialty, but lack enrollment data and do not guarantee complete capture of all patient interactions. Closed claims offer enrollment information and a more complete adjudicated record, but frequently omit provider and facility-level details. Rather than relying exclusively on one source and potentially missing important context, researchers should utilize a hybrid claims approach that layers open claims onto closed claims to capture the most complete view of patient care.
