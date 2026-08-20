---
schema_version: "1.0.0"
document_id: "a0ad176a60317eecc417732b0d000ccc0ee2c12bd5a4c711270a21464d86e917"
company_key: "yc-agilemd"
company: "AgileMD"
source_id: "yc-agilemd-news-import-11633bc91081"
canonical_url: "https://www.agilemd.com/post/sepsis-alliance-webinar-recap-sepsis-prediction-models-yales-ecart-case-study"
published_at: "2024-11-19T00:00:00+00:00"
first_seen_at: "2026-07-26T21:58:40.811660+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:bc0786f8a540a2eee90c2f11189784772399f3c5b9ca5f137eba3a2fc04eca79"
---

# Sepsis Alliance Webinar Recap: Sepsis Prediction Models + Yale’s eCART Case Study

## Webinar Recap: Evaluation and Implementation of AI Powered Solutions for Sepsis


If you’re reading this, you’re likely painfully familiar with these three truths: 1) sepsis accounts for 1 in 3 hospital deaths, 2) every minute counts for septic patients, and 3) hospital care teams are underwater with alerts.


At a recent Sepsis Alliance webinar, three experts weighed in on why sepsis prediction models are failing us and how we might rethink the current approach.


Below is a summary of the key insights with embedded video sections.


#### The False Alarm Problem and Two Different Lassos


‍


Dr. Dana Edelson, Executive Medical Director for Rescue Care and Resiliency at the University of Chicago and AgileMD’s Co-Founder and Chief Medical Officer, opened the webinar by framing the problem: most sepsis alerts in hospitals today generate too many false alarms or arrive too late to drive meaningful action. She likened the situation to the “boy who cried wolf,” wherein overburdened clinicians learn to ignore alerts.


Edelson explained that since sepsis is the confluence of infection and organ dysfunction there are two primary AI approaches to finding these patients:


- **The Standard Approach - Predict infection:** Most AI based sepsis tools are screening primarily for signs of infection —such as fever or elevated white blood cell count — regardless of the life threat or organ disfunction. The result? A wide lasso which creates unnecessary false alarms (grey), fails to distinguish the infected patients (blue) from those who are likely to become septic (purple), and misses many life-threatening clinical deteriorations (red), nearly half of which are septic.


- **The Alternative Approach - Predict clinical deterioration:** Using a general early warning score that heavily weights signs of critical illness – like respiratory rate and blood pressure – allows for a smaller lasso, which picks up the clinical deteriorations due to sepsis (purple) and those due to other causes (red) with fewer false alarms (grey) and without triggering on the low-risk infections (blue) who are unlikely to become septic.


#### Sepsis Prediction Models Are Inherently Flawed


‍


Dr. Patrick Lyons, an intensivist and healthcare delivery scientist at Oregon Health & Science University, highlighted the fundamental challenges with current strategies and proposed some solutions:


1. **There is no gold standard for identifying sepsis:** Various proposed options, such as Sep-1, Sep-3 and CDC ASE, produce wildly different sepsis incidence and outcome rates and all exclude septic patients who were never recognized to be septic. Since models trained on imprecise outcomes are inherently unreliable, sepsis model accuracy suffers.
2. **Time Zero is unknown:** Because we can’t identify when sepsis actually began, we settle for proxies of Time Zero that rely on clinical interventions to indicate recognition of sepsis. However, in 85% of cases, sepsis interventions are started before formal sepsis criteria are met, resulting in models that predict *recognition* of sepsis better than sepsis itself. When superfluous alerts that fire after clinician recognition are excluded, the accuracy falls even further, as highlighted in a recent study1 from the University of Michigan where the Epic Sepsis Model was no better than a coin toss in predicting untreated sepsis.
3. **Detecting all cause deterioration is less flawed AND catches sepsis:** General early warning scores trained on discreet deterioration outcomes, such as ICU transfer and death, don’t suffer from the same definitional flaws of sepsis models. These models are trained on a discrete deterioration event, of which 2 in 5 are due to sepsis. When combined with a workflow, use of general early warning scores have been associated with decreased hospital mortality and time to ICU transfer as well as an increase in lactate ordering.
‍


Area under the receiver operator characteristics curve (AUROC) for the Epic Sepsis Model prior to various sepsis interventions. Adapted from a 2024 study by Kamran and colleagues at the University of Michigan (DOI: 10.1056/AIoa2300032).


#### Real-World Evidence: Yale New Haven Health’s Journey


‍


Jennifer Johnson, Critical Care Nurse Practitioner at Yale New Haven Health (YNHHS), shared how her health system tackled alert fatigue and improved patient outcomes by adopting eCART. Yale focused on:


- **Data-driven tool selection:** To decide which model to use, Yale compared six general early warning scores in a large cohort study2 of over 360,000 patients. eCART emerged as the clear leader, offering significantly higher predictive accuracy and nearly 48,000 fewer alerts compared to Epic DI, which emerged as one of the worst performing.


- **EHR integrated clinical workflows:** In addition to decreasing false alarms, Yale was motivated to switch to eCART because of the embedded clinical workflows. They customized several pathways, which clinicians access from the eCART user interface. Johnson noted that about 40% of the patients with elevated eCART scores are in fact septic so all the pathways include screening and/or treatment of sepsis as a primary focus. In addition, the pathway for front line nurses focuses on increasing vital sign frequency for patients with elevated eCART scores, with a particular emphasis on accurate respiratory rate assessment and documentation of nurse worry. The provider pathway includes a goals of care assessment. Yale also has a rapid response nurse pathway for proactive rounding on high-risk patients.
- **Driving adoption with performance metrics:** Yale tracks eCART workflow compliance and outcomes with a focus on timely acknowledgement of eCART elevations and pathway completion. Johnson noted that “for the very first time in our 10+ year journey, \[we\] have universal adoption of a clinical deterioration tool.” Specifically, she showed that nurses enter the pathway and complete the sepsis screen and/or worry assessment in over 90% of patients with eCART elevations. In contrast, the Epic Sepsis Model had fired across the system over 140,000 times in the first 10 months of 2023, with only 13% of alerts being acknowledged:


#### Key Takeaways for Clinical Leaders


The insights from Dr. Edelson, Dr. Lyons, and Johnson offer a roadmap for clinical leadership seeking to improve sepsis outcomes:


1. **Break down silos** : Align sepsis and deterioration teams under a unified framework to improve response coordination.


1. **Reevaluate sepsis prediction** : Consider shifting from infection-driven alerts to deterioration-driven models like eCART.


1. **Prioritize workflow integration** : Ensure that alerts trigger clear, actionable protocols to reduce variability and prevent clinician burnout.


1. **Leverage reporting for change management** : Use metrics like alert response times and reassessment compliance to identify gaps and drive accountability.


‍


##### References:


1. Kamran F, Tjandra D, Heiler, A, et al. Evaluation of Sepsis Prediction Models before Onset of Treatment. NEJM AI. 2024;1(3). doi: 10.1056/AIoa2300032.[https://ai.nejm.org/doi/abs/10.1056/AIoa2300032](https://ai.nejm.org/doi/abs/10.1056/AIoa2300032)


1. Edelson DP, Churpek MM, Carey KA, et al. Early Warning Scores With and Without Artificial Intelligence \[published correction appears in JAMA Netw Open. 2024 Nov 4;7(11):e2448969. doi: 10.1001/jamanetworkopen.2024.48969\]. JAMA Netw Open. 2024;7(10):e2438986. Published 2024 Oct 1. doi:10.1001/jamanetworkopen.2024.38986.https://pubmed.ncbi.nlm.nih.gov/39405061/


‍
