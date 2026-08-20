---
schema_version: "1.0.0"
document_id: "1787e86648fb9c26965da9ee634f92b02b2cffcab4902edc6aa51953eb17ff35"
company_key: "yc-healthkey"
company: "HealthKey"
source_id: "yc-healthkey-news-import-1fd531f6f800"
canonical_url: "https://gethealthkey.com/blog/case-study-access-cct-rise3"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-23T22:41:52.557718+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:4f356e94451daf24a42fe3578373a613da5355c30e22da8bb3b85a7c28584a28"
---

# How Access CCT Went from Two Enrolled Patients to a Pipeline of 39 on a Trial a Year Behind Schedule

Do not index


###


Quick tags:


- Sponsor:` Biohaven Therapeutics`


- Indication:` Refractory Focal Onset Epilepsy`


- Specialty:` Neurology`


- Type:` Drug (Small molecule)`


- EHR:` eClinicalWorks`


###


Research Site Description


Access CCT is a growing clinical research organization with neuroscience and dermatology sites in Louisiana, with expansion into California and partnerships with larger healthcare networks. Michelle Hurst leads research operations across sites, coordinating trial enrollment and site management. At the time of onboarding, Access CCT was managing multiple active trials and evaluating tools to scale their patient recruitment.


###


Trial Description


The Phase 2/3 RISE 3 study ([NCT06309966](https://clinicaltrials.gov/study/NCT06309966) ) is a multicenter, randomized, double-blind, placebo-controlled trial evaluating BHV-7000 as adjunctive therapy in adults with refractory focal onset epilepsy. The study is sponsored by Biohaven Therapeutics and aims to enroll 390 patients across 174 sites worldwide.


BHV-7000 targets treatment-resistant epilepsy, a condition where patients continue to have seizures despite trying multiple anti-seizure medications. The primary endpoint measures the change in 28-day average seizure frequency from baseline, comparing BHV-7000 to placebo over an 8-week treatment phase. Secondary endpoints include the proportion of patients achieving 50% or greater seizure reduction and seizure freedom during the treatment period.


Patients must have drug-resistant focal epilepsy (failed two or more anti-seizure medication regimens), be on 1-3 current medications, and be able to maintain accurate seizure diaries. The eligibility criteria span 32 distinct requirements covering seizure history, medication use, prior surgeries, and clinical documentation.


Source:[Biohaven Clinical Trials](https://www.biohavenclinicaltrials.com/study-resources/foe-study-medication/)


###


Research Site Description


###


Challenge


Access CCT was participating as an investigative site for the Biohaven RISE 3 trial. The study had been recruiting for over a year and was only halfway to enrollment, well behind the sponsor's timeline. Despite having an enrollment target of 5 patients at their site, they had enrolled just 2 patients through traditional screening methods.


The trial's eligibility criteria made screening exceptionally difficult. Patients needed a documented diagnosis of focal onset epilepsy (not generalized epilepsy) for at least one year, drug-resistant disease confirmed by failure of two or more anti-seizure medications, and a minimum seizure frequency of 4 or more per month. They could not have had status epilepticus within 6 months, neurosurgery within 4 months, or be on prohibited medications including strong CYP3A4 inducers.


The clinical nuance compounded the difficulty. Seizures documented as "generalized" in the chart might actually be focal seizures that generalized out, a critical distinction that required reading deep into neurology notes. Medication "failures" had to mean two different medications, not dosage adjustments of the same drug. Seizure frequency required manual verification against patient-reported diaries, not just chart documentation.


Access CCT had already deployed another recruitment platform to help with this trial. That platform was free to the site (sponsor-funded), but after a month of use, the results were underwhelming. EHR integrations were slow to connect, and support was limited to a single contact reachable only by email.


💬


"It's an epilepsy trial that's real, real tough to recruit for. It's been going on for over a year and they're only halfway to enrollment even though they were supposed to be finished by now."


**Michelle Hurst** , Chief Executive Officer, Access CCT


Source:[LinkedIn](https://www.linkedin.com/in/michelle-hurst-29546325/)


###


Solution


Access CCT added HealthKey to run alongside their existing platform, giving them a head-to-head comparison on the same trial with the same patient population.


HealthKey connected to Access CCT's eClinicalWorks EHR and began processing patient records within weeks. The platform ingested 996,044 medical records across 5,608 patients, including clinical notes, medication histories, lab results, and diagnostic reports.


HealthKey's AI translated all 32 inclusion and exclusion criteria into logic that could be applied across both structured fields and unstructured clinical documentation. The system performed 35,679 individual criteria evaluations, surfacing 39 patients who met all eligibility requirements. Each match included links to the supporting evidence: seizure frequency documented in neurology notes, medication histories confirming failed regimens, and documentation ruling out exclusion criteria.


The platform's criteria flexibility proved critical. When Access CCT's neurologists explained that many patients charted with "generalized seizures" actually had focal onset seizures that generalized, HealthKey adjusted the criteria in real time to include these patients as candidates rather than hard-excluding them. The clinical team could then verify each case.


💬


"Some of these patients in their charts, it'll say that they have generalized seizures. Well, that's an exclusion for this study. But what we learned from our neurologists is that patients can have a focal onset seizure that can generalize out. And then they'll call it a generalized seizure, but it's not truly a generalized seizure. So we wanted to pull all those patients in just in case that they really did have focal seizures, and so far all of them have. So that was a big catch."


**Michelle Hurst** , Chief Executive Officer, Access CCT


HealthKey's customer success team went beyond the platform. When criteria changes surfaced additional candidates, HealthKey's team proactively reviewed the new patients' charts and flagged the strongest ones to the coordinator, saving hours of manual chart review.


💬


"I want to give a shout out to Adrian while y'all are all here on this call. Whenever we change some criteria to kind of open it up for more patients, there were a few additional patients that were identified that might qualify. Adrian literally went in there, looked at them for me, and told me, Hey, you need to look at these. They're looking good. So he has been a tremendous help. That's not something that you get from other platforms."


**Michelle Hurst** , Chief Executive Officer, Access CCT


###


The Results


####


5,608 patients screened. 996,044 medical records processed.


HealthKey screened the entire patient population across Access CCT's eClinicalWorks system. Manually reviewing this volume would have required approximately 1,682 hours, or over 210 full working days. HealthKey completed the analysis and surfaced eligible patients within weeks.


####


39 patients identified. The one brought in for screening was missed by the other platform.


After running both HealthKey and their existing recruitment platform on the same trial, the results were clear. HealthKey identified 39 patients meeting all 32 eligibility criteria. One patient who was brought in for screening via HealthKey was missed by the other platform.


💬


"Their system did not pull this patient at all, the one that's qualifying. Don't know why, but just wanna let you know that. So congratulations."


**Michelle Hurst** , Chief Executive Officer, Access CCT


The difference came down to clinical nuance. The 32 eligibility criteria for RISE 3 required interpreting unstructured neurology notes, distinguishing between medication failures and dosage changes, and verifying seizure frequency across multiple data sources. HealthKey's AI processed all of this across nearly a million medical records. A platform that could not read into the clinical documentation missed the patient who was right there in the data.


####


**1,682 hours of manual chart review replaced.**


Screening 5,608 patients against 32 eligibility criteria by hand would have taken a single coordinator over 210 full working days. Manual screening would not have finished before the trial ended. HealthKey completed the full population screen and surfaced eligible patients within weeks, giving the team time to focus on outreach and enrollment.


💬


"I'd rather look with my own eyes than take a chance on missing one."


**Michelle Hurst** , Chief Executive Officer, Access CCT


###


Looking Ahead


Within weeks of seeing HealthKey's results on RISE 3, Access CCT began expanding. Michelle Hurst requested feasibility analysis for a myasthenia gravis trial at a second site, and her California team started planning to onboard additional studies onto the platform.


💬


"Y'all are awesome. This has been a great experience."


**Michelle Hurst** , Chief Executive Officer, Access CCT
