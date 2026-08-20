---
schema_version: "1.0.0"
document_id: "1ce632385089b1fff00da15a8a1966a4c64d1358892ecc9f89cad951ab4aab0f"
company_key: "yc-frekil"
company: "Frekil"
source_id: "yc-frekil-rss-a6f9e192a2a2"
canonical_url: "https://www.frekil.com/blog/external-controls-rwe-clinical-development"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-25T05:42:05.860173+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:3208d9df1116715d992ce1339c8fff8cc45a55f55ff367186531099b281eab77"
---

# When the Control Arm Comes From the Real World

Picture a rare disease team late in development. The biology is compelling. The patients are few. The disease is serious. Everyone in the room knows what the cleanest study would look like on paper: randomize patients, give half the new therapy, give half the control, and let the evidence speak.


Then reality enters the meeting. There may be no approved standard treatment. A placebo arm may be ethically hard to defend. The available patient population may be too small to support a conventional phase 3 trial. Families and physicians may have waited years for a therapy, and asking patients to risk assignment to no active treatment can feel impossible.


This is where external controls become more than a statistical idea. They become a way to ask a fair question when a traditional randomized control arm is not feasible: what would have happened to similar patients without the new therapy?


## What an ECA actually is


An ECA, or external control arm, is the comparison group that comes from outside the current trial. FDA describes an externally controlled trial as one where outcomes in participants receiving the test treatment are compared with outcomes in people external to the trial who did not receive that same treatment. The control patients may come from an earlier time, which is a historical control, or from the same time period but another setting, which is a concurrent external control.[FDA external control guidance](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/considerations-design-and-conduct-externally-controlled-trials-drug-and-biological-products)


In plain English, an ECA is the best available answer to a counterfactual question: if the patients in the single-arm study had not received the investigational therapy, what would a clinically similar group have looked like over the same kind of follow-up? That group can be built from prior clinical trials, natural history studies, patient registries, electronic health records, claims, chart reviews, or other real-world data sources. FDA defines real-world data as patient health and care-delivery data routinely collected from sources such as EHRs, claims, registries, and digital health technologies.[FDA real-world evidence](https://www.fda.gov/science-research/science-and-research-special-topics/real-world-evidence)


Building an ECA is not the same as pulling a convenient historical table. A good team first defines the trial-like question: who would have been eligible, what counts as time zero, what treatment or non-treatment history matters, what endpoint will be measured, and how long patients must be followed. Then the team searches for patient-level data that can reproduce those rules. Only after that does the matching work begin.


The construction usually has four practical moves. Find a source population that resembles the treated population. Apply the same inclusion and exclusion logic as closely as the data allow. Balance the groups on baseline factors that influence prognosis and treatment choice, often through exact matching, propensity score matching, or weighting. Then stress test the result with balance checks, endpoint checks, missing-data checks, and sensitivity analyses. A synthetic control arm is a highly curated version of this idea. Synthetic does not mean fake patients. It means real patient histories are assembled and weighted to mimic the control arm the trial could not practically enroll.


> External control is not a shortcut around evidence. It is an attempt to make the missing control arm visible enough to support a decision.


## How RWE entered the development conversation


The modern regulatory opening for real-world evidence did not appear overnight. The[21st Century Cures Act](https://www.fda.gov/regulatory-information/selected-amendments-fdc-act/21st-century-cures-act) asked the FDA to evaluate how real-world evidence could support regulatory decisions, including new indications and post-approval requirements. FDA then released a[strategic framework for its RWE program](https://www.fda.gov/science-research/science-and-research-special-topics/real-world-evidence) and later issued guidance on submitting documents that include real-world data and real-world evidence.


For pharma and biotech teams, the practical message was clear. Randomized clinical trials remain the preferred foundation for most development programs. But there are places where the standard design strains against the disease: orphan indications, ultra-rare genetic conditions, certain oncology subtypes, and life-threatening settings where a concurrent placebo group may not be acceptable.


In those settings, real-world data,[natural history studies](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/rare-diseases-natural-history-studies-drug-development) , patient registries, and historical clinical trials can sometimes help build an external control arm. The goal is to compare the treated patients with patients who resemble them closely enough that the difference in outcomes can be interpreted. That sentence is easy to write and hard to earn.


## The Strensiq story


[Strensiq](https://www.accessdata.fda.gov/scripts/opdlisting/oopd/detailedIndex.cfm?cfgridkey=266608) offers a useful way to understand why external controls can matter. The drug was developed for hypophosphatasia, or HPP, a rare inherited disorder that affects bone and tooth development. Before Strensiq, there was no approved therapy for HPP in the United States. The relevant patients included infants and children with severe disease. A large, traditional randomized trial was not a natural fit for that world.


Instead of a conventional phase 3 trial with a concurrent control arm, the evidence package drew from[multiple clinical studies of patients treated with asfotase alfa](https://www.ncbi.nlm.nih.gov/books/NBK476044/) . Some of those studies were single-arm studies. The missing question was obvious: compared with what?


The answer came from a natural history study. Investigators selected patients from the natural history dataset who were similar to the treated population: patients with perinatal or infantile onset HPP. That external group became the reference point for understanding whether the treatment changed the course of disease. The same clinical review is a useful reminder that historical controls can bring baseline imbalances and uncertainty about retrospective data quality, even when they are the best feasible comparator.


The endpoints helped. Overall survival and invasive ventilator-free survival are not subtle exploratory signals. They are clinically direct, hard-to-ignore outcomes. In the Strensiq review, the treated patients showed much better survival and ventilator-free survival than the natural history controls. The effect size was large enough that the story did not depend on a delicate statistical interpretation.


> The stronger the endpoint and the larger the treatment effect, the less an external-control study has to ask reviewers to believe.


That does not make the evidence identical to a randomized trial. FDA statistical reviewers have historically viewed historical controls as a weaker level of evidence than randomized controls because unmeasured differences between groups can remain. P-values and hazard ratios can support the story, but they do not magically erase the design limitation. In a setting like Strensiq, the credibility came from the totality: rare disease context, serious unmet need, objective endpoints, careful patient selection, and a dramatic treatment effect.


## What Brineura and Bavencio teach


Not every external-control story is as clean.[Brineura](https://www.biomarin.com/news/press-releases/fda-approves-brineura-cerliponase-alfa-for-the-treatment-of-cln2-disease-a-form-of-batten-disease-and-ultra-rare-pediatric-brain-disorder-in-children/) shows how much work can sit inside a single word: endpoint. The approval was supported by a non-randomized, single-arm study compared with untreated patients from a natural history cohort. When the active study and historical data measure disease progression differently, the team has to prove that those measurements are comparable before the comparison can carry weight. In rare disease, that can mean deep clinical and statistical work just to show that two rating approaches are speaking the same language.


[Bavencio](https://www.fda.gov/drugs/approved-drugs/avelumab-bavencio) points to another pattern. In oncology, historical registry data and retrospective chart reviews can provide supportive context for single-arm studies. FDA's accelerated approval for metastatic Merkel cell carcinoma was based on the open-label, single-arm JAVELIN Merkel 200 trial, and later real-world chart review work helped describe outcomes in routine US academic practice.[Avelumab real-world chart review](https://pmc.ncbi.nlm.nih.gov/articles/PMC9394192/) That can be valuable when randomized evidence is difficult, but it also raises familiar questions. Were the patients treated at similar disease stages? Were prior therapies comparable? Were response, progression, and follow-up measured in the same way? Were the criteria used to assess tumors consistent across the datasets?


Ibrance is a useful label-expansion example. Pfizer announced that the 2019 expanded indication for men with HR-positive, HER2-negative advanced or metastatic breast cancer was based largely on electronic health records and post-marketing reports from IQVIA, Flatiron Health, and Pfizer's global safety database.[Pfizer Ibrance RWD approval](https://www.pfizer.com/news/press-release/press-release-detail/u_s_fda_approves_ibrance_palbociclib_for_the_treatment_of_men_with_hr_her2_metastatic_breast_cancer) It is not the same design problem as Strensiq, but it shows the same strategic pattern: when the subgroup is small and direct trial evidence is thin, well-structured real-world data can help close the evidence gap.


These examples are useful because they keep the conversation grounded. External controls are not one method. They are a family of designs that can use natural history studies, prior trials, registries, electronic health records, claims, chart review, and post-marketing data. The value depends less on the label placed on the data source and more on whether the source can answer the specific question.


## The control arm has to be believable


The central design problem is comparability. Patients in the external-control arm should resemble patients in the treated arm on the variables that matter: disease severity, duration of illness, prior therapies, age, baseline clinical status, line of therapy, biomarker profile, and any other factor that could influence both treatment choice and outcome.


The clinical evaluations and endpoints need to line up too. If one dataset captures progression through rigorous imaging review and another captures it through inconsistent routine care notes, the comparison may be biased before the model starts. If follow-up time differs sharply across studies, time-to-event endpoints can become hard to interpret. If subsequent therapies differ, overall survival can reflect more than the treatment being studied.


This is why study selection is the first real analysis. A sponsor may have access to a large amount of data and still not have the right data. The external source needs similar eligibility criteria, similar medical conditions, similar clinical assessments, and enough patient-level detail to adjust for differences. ISPE-endorsed recommendations on external controls make the same point: without randomization, credibility depends on careful, transparent planning to minimize bias and confounding.[ISPE external control considerations](https://pmc.ncbi.nlm.nih.gov/articles/PMC7687199/) In the big data era, the shortage is rarely rows. The shortage is fit-for-purpose evidence.


## Matching is useful, but it is not a rescue plan


[Propensity scores and matching methods](https://pmc.ncbi.nlm.nih.gov/articles/PMC9795588/) can help make external controls more comparable to treated patients. The basic idea is intuitive: use observed baseline characteristics to identify control patients who look like treated patients before treatment begins. If the groups are similar enough on the right variables, the outcome comparison becomes more credible.


But matching only works on what is measured. It cannot fix an endpoint that was not captured reliably. It cannot adjust for a clinical factor missing from the historical dataset. It cannot create statistical power when there are too few comparable patients. In ultra-rare disease, where sample sizes can be under 100, exact matching on a few critical variables may be more practical than a complicated model. That tradeoff has to be owned, not hidden.


Oncology brings a different version of the same challenge. There may be more patients, but the disease course and treatment pathway can be highly segmented. First line, second line, biomarker-positive, refractory, post-progression, prior exposure, response criteria, scan schedule - each detail changes who is being compared with whom.


> A synthetic control arm is only persuasive when clinicians can look at it and say: yes, these are the patients we would have expected to compare.


## Plan before looking at outcomes


The most important discipline is prospective planning. FDA's external-control draft guidance is written for sponsors and investigators considering externally controlled trials to provide evidence of safety and effectiveness, and that framing matters: this is study design, not a last-minute appendix. The analysis plan should define the population, endpoints, objectives, hypotheses, covariates, matching approach, sensitivity analyses, and rules for handling missing data before anyone starts tuning the design around outcomes. That is not bureaucracy. It is how the team protects the study from data dredging.


For studies using propensity score modeling, the baseline covariates and modeling rules should be defined before the outcome data are inspected. If there is randomness in the matching process, even the random seed should be recorded. If unexpected missingness makes the model impossible to run as planned, the revision should be documented clearly.


A practical model is to separate design from outcome analysis. An independent statistician can help build the external control, estimate propensity scores, and examine covariate balance while remaining blinded to outcomes. Once the treated population is enrolled and the external-control logic is locked, the outcome analysis can proceed with a cleaner firewall between design decisions and results.


## What good teams take away


External controls are most useful when they are designed early, not bolted onto a program after the pivotal evidence disappoints. They can help estimate effect size, choose endpoints, identify biomarkers, refine patient selection, reduce sample size, and make development more feasible in populations where conventional designs are strained. They can also support label expansion when cross-trial comparisons are clinically and methodologically defensible.


The lesson for pharma and biotech teams is not that real-world evidence can replace randomized evidence whenever randomization is inconvenient. The lesson is sharper: external controls can be powerful when the disease context justifies them, the data source is fit for purpose, the comparison is clinically credible, and the analysis is planned before the result is known.


In the best cases, the external control lets a development team tell a story that is both humane and rigorous. We could not ethically or practically randomize these patients in the usual way. So we found the closest credible picture of what would have happened without the therapy, tested the comparison hard, and asked whether the treatment changed the course of disease.


That is the promise of RWE in clinical development. Not more data for its own sake. Better evidence for the decisions where the old playbook is no longer enough.
