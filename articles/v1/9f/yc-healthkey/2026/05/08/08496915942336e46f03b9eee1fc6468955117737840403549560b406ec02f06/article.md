---
schema_version: "1.0.0"
document_id: "08496915942336e46f03b9eee1fc6468955117737840403549560b406ec02f06"
company_key: "yc-healthkey"
company: "HealthKey"
source_id: "yc-healthkey-news-import-1fd531f6f800"
canonical_url: "https://gethealthkey.com/blog/case-study-flourish-enlighten-6"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-23T22:41:52.557718+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:38c4bb77471a383bfe495f763d56fa4bd10fd3f8e56dfba9a2b4249e2737788d"
---

# How Flourish Research Screened 12,004 Patients Across 6 Sites for a Phase 3 Obesity Trial Targeting GLP-1 Add-On Therapy.

Do not index


###


Quick tags:


- Sponsor:` Eli Lilly and Company`


- Indication:` Persistent Obesity / Overweight`


- Specialty:` Endocrinology / Metabolic`


- Type:` Drug (Amylin Receptor Agonist)`


- EHR:` athenahealth, Practice Fusion`


###


Trial Description


The Phase 3 ENLIGHTEN-6 study ([NCT07392190](https://clinicaltrials.gov/study/NCT07392190) ) is a randomized, double-blind, placebo-controlled trial evaluating eloralintide (LY3841136) in adults with persistent obesity or overweight who are already being treated with a weekly incretin. Sponsored by Eli Lilly and Company, the study aims to enroll 900 patients globally and will run approximately 80 weeks per participant.


Eloralintide is a subcutaneously administered amylin receptor agonist designed to provide additional weight loss when added to existing GLP-1 receptor agonist therapy. Unlike standalone obesity treatments, ENLIGHTEN-6 specifically targets patients whose weight loss has plateaued or remained insufficient despite treatment with medications like semaglutide (Ozempic/Wegovy) or tirzepatide (Mounjaro/Zepbound).


Source:[Nature](https://www.nature.com/articles/s41574-025-01125-9)


The primary endpoint is percent change from baseline in body weight at Week 64. Secondary endpoints include changes in waist circumference, triglycerides, systolic blood pressure, BMI, fasting glucose in patients with type 2 diabetes, and patient-reported outcomes including quality of life (SF-36, EQ-5D-5L) and control of eating (CoEQ).


###


Challenge


Flourish Research needed to identify patients across six physician practices who met a very specific eligibility profile: adults already on stable weekly incretin therapy (only one of these four GLP-1 agonists: Ozempic, Wegovy, Mounjaro, or Zepbound) who still had persistent obesity or overweight with at least one obesity-related complication.


The[inclusion criteria](https://clinicaltrials.gov/study/NCT07392190#participation-criteria) required confirming that each patient was on a stable incretin dose at screening, had a BMI of 30 or higher (or 27+ with a qualifying comorbidity such as hypertension, dyslipidemia, obstructive sleep apnea, cardiovascular disease, or type 2 diabetes), and had maintained stable body weight with less than 5% change at screening.


Then came the exclusions. Patients were disqualified for prior or planned bariatric surgery, type 1 diabetes, use of DPP-4 inhibitors, amylin analogs, or insulin within 90 days, or any major cardiovascular event (heart attack, stroke, coronary revascularization, unstable angina, or CHF hospitalization) within the prior 90 days.


Verifying these criteria manually meant cross-referencing medication lists to confirm GLP-1 prescriptions and dosing stability, reviewing BMI trends over time, checking for qualifying comorbidities across problem lists and encounter notes, and scanning surgical and procedure histories for bariatric interventions. Across six sites with different EHR systems, this was not a task that could be done efficiently by hand.


The timeline added urgency. With insurance formulary changes hitting January 1st, patients on GLP-1 medications risked being switched to different drugs or doses, potentially disqualifying them from the trial. Flourish needed to identify eligible patients and notify them before those changes took effect.


💬


"The insurance is changed after January 1st. So we need people that are on stable doses of Ozempic or Mounjaro, so we need to give them heads up not to switch."


**Ludmila Fonariuc** , Clinical Team Manager, Flourish Research


Ludmila Fonariuc


###


Solution


Flourish Research connected HealthKey to multiple EHR systems across their six participating sites, including athenahealth, eClinicalWorks, and Practice Fusion. For each site, setup was straightforward: physicians authorized EHR access, and HealthKey's white-glove onboarding service translated all of ENLIGHTEN-6's inclusion and exclusion criteria into logic the AI could apply across both structured fields and unstructured clinical notes.


HealthKey screened 12,004 patients and processed 4,650,421 medical records across the six sites, performing 62,020 individual criteria evaluations. Each evaluation checked medication histories for stable GLP-1 prescriptions, BMI measurements and weight trends, comorbidity documentation, surgical histories, recent cardiovascular events, and lab values including HbA1c.


The criteria were extensive. HealthKey evaluated 63 distinct eligibility criteria. Across all six sites, 23 patients met every criterion. Each match included the supporting clinical evidence: the medication list confirming incretin therapy and dosing stability, weight measurements showing BMI qualification, problem list entries documenting obesity-related comorbidities, and procedure histories confirming no prior bariatric surgery.


For patients with borderline eligibility, such as those with A1C values near the diabetes threshold, HealthKey flagged them for coordinator review with all relevant evidence pre-assembled rather than automatically excluding them.


###


The Results


####


12,004 patients screened and 4,650,421 medical records processed across 6 clinics


HealthKey evaluated every patient across six Flourish clinics against the full ENLIGHTEN-6 eligibility profile. The largest clinic contributed 5,834 patients and over 2.1 million medical records alone. Across the network, HealthKey processed an average of 387 medical records per patient, spanning observations, medication requests, conditions, encounters, diagnostic reports, clinical documents including unstructured PDF documents.


💬


"When we ran YDAL at our site, it produced one perfect patient in the first week. And this provider doesn't write very many GLP ones. And that perfect patient is randomizing."


**Christopher Zomerfeld** , Executive Director - Detroit, Flourish Research


[Christopher Zomerfeld](https://www.linkedin.com/in/christopher-zomerfeld-16731b205/)


####


3,601 hours of manual chart review eliminated


At a baseline of 20 minutes per manual chart review, screening 12,004 patients would have taken a single coordinator over 450 working days. HealthKey completed the work across all six sites and surfaced 23 fully qualified candidates, giving each site's team months back to focus on enrollment and patient care rather than chart review.


💬


"You could have a regular person go into it, sit in an office for eight hours and scrub an EMR. You're gonna get one one-hundredth through that EMR, probably one one-thousandth. And the depth you're gonna go into each patient to actually determine eligibility unless you know every part of the inclusion exclusion offhand is probably one-fiftieth as deep."


**Christopher Zomerfeld** , Executive Director - Detroit, Flourish Research


####


62,020 criteria evaluations across 63 eligibility requirements


Every patient was evaluated against the full complexity of the ENLIGHTEN-6 protocol: GLP-1 medication verification, dosing stability checks, BMI threshold calculations stratified by comorbidity status, surgical history exclusions, recent cardiovascular event screening, and lab value checks.


💬


"This tool just saves so much time for us. I keep telling people that anyone who doesn't have HealthKey in the future in clinical research is just going to be chasing everyone who does."


**Christopher Zomerfeld** , Executive Director - Detroit, Flourish Research


###


Looking Ahead


ENLIGHTEN-6 represents the first obesity and metabolic study in the HealthKey-Flourish partnership. Flourish enrolled one patient in the first two weeks HealthKey at the first site, with five sites still actively screening. Across their organization, Flourish Research now uses HealthKey on 20+ active studies spanning cardiology, neuroscience, metabolic, and other therapeutic areas.


The multi-site deployment model proved that HealthKey can scale across a research network: connect multiple EHR systems, translate one protocol into AI-readable criteria, and surface eligible patients at every site simultaneously. For a trial like ENLIGHTEN-6, where the eligibility profile requires verifying medication history, weight trends, and comorbidities across unstructured clinical notes, the alternative was months of manual chart review across six disconnected systems.
