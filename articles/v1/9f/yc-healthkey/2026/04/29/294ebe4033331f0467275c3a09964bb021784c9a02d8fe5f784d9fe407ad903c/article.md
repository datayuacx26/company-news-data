---
schema_version: "1.0.0"
document_id: "294ebe4033331f0467275c3a09964bb021784c9a02d8fe5f784d9fe407ad903c"
company_key: "yc-healthkey"
company: "HealthKey"
source_id: "yc-healthkey-news-import-1fd531f6f800"
canonical_url: "https://gethealthkey.com/blog/case-study-usup-cg-core-008"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-23T22:41:52.557718+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:0a29ba2737d5ce3766c0c6053177643e6a9d4863a688396da5d32cf44bd3eaeb"
---

# How HealthKey Screened Two Clinics in Parallel for a Phase 2 Bladder Cancer Trial.

Do not index


###


Quick tags


- Site:` US Urology Partners`


- Sponsor:` CG Oncology, Inc.`


- Indication:` High-Risk Non-Muscle-Invasive Bladder Cancer (NMIBC)`


- Specialty:` Urology / Oncology`


- Type:` Drug (Gene Therapy / Oncolytic Virus)`


- EHR:` ModMed` ,` iSalus`


###


Trial Description


The CORE-008 study ([NCT06567743](https://clinicaltrials.gov/study/NCT06567743) ) is a Phase 2, multi-arm, multi-cohort, open-label trial evaluating the safety and efficacy of *cretostimogene grenadenorepvec* in participants with high-risk non-muscle-invasive bladder cancer (NMIBC). CG Oncology sponsors the trial, which targets enrollment of 325 patients across 65 sites.


Cretostimogene is an oncolytic adenovirus administered directly into the bladder. Cohort B, the focus of this case study, enrolls patients with BCG-exposed high-risk high-grade NMIBC. Patients receive weekly induction treatments for 6 weeks, followed by maintenance cycles. The primary endpoints are complete response rate and high-grade event-free survival.


Source:[CG Oncology 10K](https://www.sec.gov/Archives/edgar/data/1991792/000095017024036143/cgon_10k_2023_1231.htm)


###


Clinic Description


US Urology Partners is a national network of urology practices that supports clinics with operations, technology, and a growing clinical research program. For CORE-008 Cohort B, USUP activated two of its brands as trial sites:


- **Central Ohio Urology Group (COUG)** in Gahanna, Ohio


- **Urology of Indiana** in Greenwood, Indiana


Sarah Faisal, VP of Research and Cancer Operations, directs USUP's clinical trial strategy across all brands, including the care navigation team responsible for tracking patient pathology and identifying clinical trial candidates.


###


Challenge


USUP's care navigators track bladder cancer patients across the network. They monitor daily cystoscopies, review pathology results, and flag patients who might qualify for clinical trials. At the locations they cover, this manual navigation workflow works well.


The problem is scale. USUP operates multiple urology practices across several states, and as the network grows, there are more locations than the care navigation team can reach.


💬


"We can't have our care navigators in all these places at once, so HealthKey helps add to their workflow."


**Sarah Faisal** , VP of Research and Cancer Operations, US Urology Partners


For CORE-008 Cohort B, the[eligibility criteria](https://clinicaltrials.gov/study/NCT06567743#participation-criteria) added complexity. Patients needed pathology-confirmed high-risk high-grade NMIBC with prior BCG exposure. All visible disease had to be resected within 90 days. Exclusions included muscle-invasive disease, significant immunodeficiency, and high-grade urothelial carcinoma in the upper urinary tract. In total, 24 distinct criteria had to be evaluated for each patient.


Finding these patients required reviewing unstructured pathology reports, cystoscopy notes, and treatment histories buried across hundreds of charts at each site. The volume was unsustainable for navigators to cover across multiple locations simultaneously.


###


Solution


USUP connected HealthKey to both[Central Ohio Urology Group](https://www.centralohiourology.com/) ’s ModMed EHR and[Urology of Indiana](https://www.urologyin.com/) ’s iSalus EHR. HealthKey ran CORE-008 pre-screening at both sites simultaneously, starting February 2026.


Across the two clinics, HealthKey ingested 859,015 medical records spanning 1,452 patients. This included over 215,000 unstructured clinical documents and scanned PDFs that were processed and analyzed alongside structured data like lab observations, encounter records, medication histories, procedures, conditions, and diagnostic reports. Patients averaged >800 clinical data points each.


HealthKey translated the trial's 24 inclusion and exclusion criteria into automated evaluation logic and ran 7,768 individual criteria evaluations across the combined patient population. The system pre-screened both sites in parallel -- something a single navigator physically cannot do.


💬


"I follow the cystoscopies at one site. So I'm seeing on the daily who's recurring. I obviously can't do that for every location."


**Care Navigator** , US Urology Partners


Patients who met all criteria were surfaced to coordinators with supporting evidence: the pathology report confirming high-grade NMIBC, documentation of prior BCG treatment, and cystoscopy notes showing disease status. Coordinators at each site reviewed the matches in HealthKey and moved candidates forward into screening.


###


The Results


####


1,452 patients and 859,015 medical records processed across 2 sites.


HealthKey screened the full bladder cancer populations at both Central Ohio Urology Group and Urology of Indiana against all 24 eligibility criteria. Manually reviewing this volume would have taken approximately 435 hours, or 54 full working days, at the standard rate of 20 minutes per patient chart. HealthKey reduced coordinator review time to approximately 2 minutes per patient.


####


5 patients met all 24 eligibility criteria. 2 enrolled.


Of 1,452 patients screened in parallel, 5 met every inclusion and exclusion criterion. 2 screen passed -- one at each site -- and both were enrolled into the trial. Enrollment was sponsor-capped at 1 patient per site, with the potential for additional slots opening in Q3/Q4 2026. HealthKey found both patients.


💬


"We had one that we were able to enroll at each site, thank you!"


**Sarah Faisal** , VP of Research and Cancer Operations, US Urology Partners


####


Two clinics screened simultaneously.


The core value was not just speed but parallel coverage. HealthKey screened both COUG (619 patients, 533,509 records) and Urology of Indiana (833 patients, 325,506 records) at the same time. The research team at each site reviewed patients while the system did the cross-site data processing that no single coordinator could match.


By mid-March, the care navigation team confirmed every site had their CORE patient enrolled.


###


Looking Ahead


With CORE-008 enrollment targets met at both sites, Sarah paused the trial in HealthKey to redirect resources to incoming studies. The decision was strategic, not a conclusion.


USUP is now expanding HealthKey to new trials across its network. For the care navigation team, the value is straightforward. HealthKey lets them extend their expertise to sites they cannot physically cover, without changing the workflow they've built at the locations they already manage.
