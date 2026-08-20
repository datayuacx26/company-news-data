---
schema_version: "1.0.0"
document_id: "30e9f2e133f18f5b17ca7e3d800f75b645e158516b3c54d4043faf79394159dc"
company_key: "yc-agilemd"
company: "AgileMD"
source_id: "yc-agilemd-news-import-11633bc91081"
canonical_url: "https://www.agilemd.com/post/how-to-vet-early-warning-scores"
published_at: "2025-03-12T00:00:00+00:00"
first_seen_at: "2026-07-26T21:58:40.811660+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:546ed136718092220ef527c152ff1c5da805afcfa015eb1e1d8a02653ee0a8cd"
---

# How to Vet Early Warning Scores

‍


*Vetting early warning scores is not for the faint of heart. This post offers best practices for avoiding the common pitfalls.*


‍


As a health system leader, you’ve likely already recognized that your organization has an opportunity to better leverage predictive models to improve quality outcomes. If you’ve made it to the next step, you’ve begun to review the enormous landscape of options and, shortly thereafter, you’ve determined that proper vetting and selection of a tool is no small feat. There are the standard questions of ‘buy vs. build vs. use the EHR option.' Then, there's the question of whether to leverage a straightforward rules-based score versus a more complex and less transparent AI model, among many others.


The stakes for careful model selection are high. If an early warning score is not sensitive enough, critical patients will get missed. Conversely, if it is overly sensitive and therefore not specific, frontline staff will be inundated with false alarms and may miss true alerts as a result. And if it doesn’t fit into the workflow, it doesn’t matter how accurate it is: no one will use it.


Here’s our list of the key principles to keep in mind when vetting an early warning score for your health system:


**1. Look to existing literature for validations that may be relevant**


‍ **Why:** If there is a well-done, peer-reviewed validation available in a population similar to yours, it might provide the confidence you need and save you the effort of reinventing the wheel. But be careful: demographic distributions (e.g. age, race, and acuity), hospitals characteristics (e.g. academic medical centers vs. community hospitals), and care settings (e.g. emergency department, ward, or intensive care unit (ICU)) will all influence accuracy statistics and generalizability. **‍**


**Example:** We recently collaborated with researchers from Yale University, University of Wisconsin, and University of Chicago to publish a head-to-head[comparison of six commonly-used early warning scores](https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2824885) . Our results were meaningful for a field that lacks evidence on predictive scores and has a paucity of consistent testing approaches. This study reinforced that all predictive models need to be vetted thoroughly before being used in clinical practice.


**2. If you need to perform your own validation, use a robust dataset with sufficient variability**


‍ **Why:** Studies are powered based on the number of outcomes, not just the size of the database. This means that the rarer the outcome, the larger the dataset needs to be to ensure that the results you are seeing are not due to chance and that they will hold up in clinical practice. Among hospitalizations, sepsis accounts for approximately 10 percent of admissions and clinical deterioration for approximately 5 percent. Therefore, depending on the size and acuity of your health system, you will likely need at least one full year of data – and likely several years – to be adequately powered and to account for seasonality and multi-year fluctuations in acuity and disease.


‍ **Example:** Our study relied on data from 362,926 adult medical-surgical patient admissions to seven diverse hospitals, including a mix of academic and community sites of varying acuity, capturing nearly 17,000 deteriorations over the course of four and a half years.


**3. Assess model performance with a reliable comparator**


‍ **Why:** Looking at performance characteristics in a vacuum isn’t nearly as useful as comparing the tool to a default option. The most statistically sound comparison is a head-to-head validation (A vs. B) in the same data. When this isn’t possible, as in cases when you want to compare to a proprietary score you don’t have access to, consider conducting a two-way comparison to the same publicly available score that the propriety tool has been compared to in the literature (A vs. C plus B vs. C). In the published study, if the common score (C) has a similar scoring distribution and test statistics to your data, it is much more likely that the published results for the proprietary score (B) will be generalizable to your data. **‍**


**Example:** In our study we compared three proprietary AI scores to the National Early Warning Score (NEWS), NEWS2 and the Modified Early Warning Score (MEWS). Given the strong performance of NEWS, we argued for making it the default comparator for future analyses. In validating the performance of advanced AI models, we needed to see whether they were at least as predictive as the NEWS, lest we add cost and complexity without benefit.


**4. Pay attention to the outcome you choose**


‍ **Why:** One of the complicating factors of reviewing existing literature on the topic is that different studies use different outcomes with varying levels of consistency and incidence. Mortality is a very objective outcome with a relatively low incidence. As such, the area under the curve (AUCs) for mortality will generally be higher and positive predictive values (PPV) will be lower compared to ICU transfer, which is more common but also more subjective. Rapid response team (RRT) activations are even more subjective and variable across sites, making results that rely on that as an outcome less generalizable. Timeframe also impacts outcomes, so make sure you’re comparing apples to apples.


**Example:** We chose deterioration, defined as mortality or ICU transfer within 24 hours of a score, as the primary outcome in our study. In addition, we presented deterioration results for 12 and 48 hours as well as mortality alone for all three time points. The best and worst performers were consistent across outcome, but the results themselves varied considerably, with AUCs being highest when the outcome was mortality within 12 hours and lowest when the outcome was deterioration within 48 hours. Therefore, when you compare results make sure the outcome is consistently defined and avoid the inclusion of RRT calls as an outcome, if possible.


**5. Ensure that the model is tested in the way it will be used**


**Why:** Probably the most common mistake we see made in the comparative analyses of risk scores is the oversimplification of the dataset. An individual patient can generate hundreds of risk scores (i.e. observations) during a hospitalization (i.e. encounter). It’s tempting to want to use the highest score for each patient and then run all the statistics using only that single observation. Unfortunately, in clinical practice you don’t know the highest score until after discharge and the highest one may occur long after the deterioration was identified by the team. If you want to mimic how the tool will perform in practice, you need to consider the longitudinal nature of the data.


**Example:** In our recent validation for FDA clearance as well as in the recent head-to-head, the large dataset enabled us to perform a bootstrap analysis in which we reran the analysis one hundred times, randomly sampling one observation per encounter each time. This method allowed us to account for the longitudinal nature of the data without introducing bias from long encounters or ones with a higher frequency of vital sign collection.


**6. Understand the tradeoff between sensitivity and positive predictive value (PPV)**


**Why:** The perfect predictive model alerts well in advance for all the patients who go on to deteriorate (i.e. 100% sensitive) and alerts only on the patients who actually go on to deteriorate (i.e. 100% PPV). Unfortunately, perfect predictors are like mermaids – they don’t actually exist. In reality, all tools are trading off sensitivity and PPV to varying extents. The key is to understand how much of a tradeoff your frontline staff can tolerate. The general rule of thumb for early warning scores is that the moderate risk threshold needs to identify at least half of the deteriorations (i.e. 50% sensitivity) and be actionable at least one out of every ten times (i.e. 10% PPV). Therefore, in addition to comparing the area under the receiver operator characteristics curves (AUROCs), compare PPVs at the 50% sensitivity threshold to get a sense of how your teams will experience the tool when you put it into practice.


**Example:** In our study, eCART had a PPV of 17.3%, nearly double that of NEWS (the second best, with a PPV of 9.5%) and nearly three times that of Epic's Deterioration Index (which had the lowest PPV at 6.3%) at comparable sensitivities around 50%. This would have translated into tens of thousands more false alarms for both NEWS and Epic DI, compared to eCART, without any improvement in identifying deteriorations.


**7. Don’t forget about workflow**


**Why:** Having vetted and selected a tool which minimizes false alarms, it’s tempting to rely on interruptive electronic health record (EHR) alerts to catch the attention of frontline staff. Unfortunately, these alerts are overplayed, and your staff have already learned to ignore them. Rather than interruptive alerts, consider passive alerting with a clearly defined workflow and success metrics so you can measure and drive adoption more collaboratively.


**Example:** Yale New Haven Health System implemented eCART in their EHR with embedded clinical pathways. They met weekly with nursing unit leadership to share utilization metrics and were able to achieve 90% pathway screening compliance without a single interruptive alert. In contrast, staff ignored 87% of the interruptive Epic Sepsis alerts.


This is undoubtedly hard, but the effort that you put in now to choose wisely will pay off in frontline buy-in, adoption, and ultimately patient lives.


Do you have any more thoughts on key vetting principles? We’d love to[hear from you](https://www.agilemd.com/contact-us) .


###### ***About AgileMD*** **


*At AgileMD, we are driven to improve patient outcomes by making evidence-based care universally accessible. This blog is dedicated to topics that keep us up at night, on which we have deep interest or expertise. Our clinical decision support software products have been used by over 135,000 providers in over 250 U.S. hospitals in the care of more than 4 million patient encounters. Since our founding at the University of Chicago, our work has been evaluated in 80 peer-reviewed publications, and we have received nearly $3 million in federal funding from the U.S. Department of Health & Human Services (HHS).*


[eCART™](https://www.agilemd.com/#Solution-eCART) ****** *guides care teams to the highest-risk hospitalized patients using industry-leading, FDA-cleared AI combined with actionable, embedded decision support for all-cause clinical deterioration.*[Clinical Pathways](https://www.agilemd.com/#Solution-Pathways) *give care teams immediate access to the most updated protocols, streamlining order entry and documentation directly in their EHR workflows.*


‍


‍


‍
