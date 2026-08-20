---
schema_version: "1.0.0"
document_id: "d789a1145f65abadac6686aeb3276c77d1ee56466ec757d009ee9604995c4520"
company_key: "yc-agilemd"
company: "AgileMD"
source_id: "yc-agilemd-news-import-11633bc91081"
canonical_url: "https://www.agilemd.com/post/why-prospective-validation-should-be-a-must-for-ai-early-warning"
published_at: "2024-09-24T00:00:00+00:00"
first_seen_at: "2026-07-26T21:58:40.811660+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:0b921376e72acfdfa2cce823d79ed8617f9533ac36d589dc664dabdbcb873209"
---

# Why Real-World Testing Should be a Must for AI Early Warning Devices

In their recent Comment in[Nature Medicine](https://www.nature.com/articles/s41591-024-03203-3) , Chouffani El Fassi and colleagues at UNC classified 521 FDA authorizations of medical AI devices between 1995 and 2022 based on validation (testing) data. They found that 43% of cleared devices lacked any validation data and another 28% included only retrospective data. This meant less than a third of the authorized devices demonstrated real-world prospective clinical testing. The authors proposed a new testing standard, pushing for prospective clinical validation to evaluate how models behave outside of a data lab. They argued that this would strengthen the legitimacy of FDA authorization, with the goal of building trust in AI models that have the potential to make significant, positive impact on care.


We couldn’t agree more. Predictive models behave differently in vivo and in vitro for lots of reasons. In a real hospital setting, for example, a nurse may jot down a patient’s vital signs on paper, entering them into the electronic health record (EHR) hours later and potentially with data entry errors, which then might be adjusted later. Also, a data outage or EHR downtime could interrupt the flow of important variables to a model, such as lab results. How does a real-time analytic and user interface behave when the inputs are imperfect?


We answered this question in our recent experience working with FDA to get[eCARTTM cleared as a medical device](https://www.agilemd.com/updates/ecart-fda-clearance) . A core concern about clinical AI devices is that they are not generalizable, meaning they do not perform consistently across variable patient populations (e.g. race, age, sex, clinical conditions), clinical settings (e.g. a small community hospital in the South versus a large academic health system in the Northeast) or time trends (e.g. pre- and post-pandemic). Much of the variability can be evaluated retrospectively with a sufficiently large and diverse dataset. However, in the case of early warning devices, retrospective testing involves calculating risk predictions from historic patient data after discharge. Ahead of testing, this data is cleaned and includes any corrections and updates made to the record during the hospitalization. Prospective validation, by contrast, evaluates the accuracy of the prediction as it is calculated in real time and presented to the clinical team, regardless of any gaps or delays in key data.


In our[FDA submission](https://www.accessdata.fda.gov/cdrh_docs/pdf23/K233253.pdf) we included retrospective validation data from nearly 1.8 million encounters across 21 hospitals and prospective validation data from just over 200 thousand encounters in the same hospitals. While the performance results were similar, they were not identical, and there were notable differences in the levels of missing data, as explored in this[Politico](https://www.politico.com/newsletters/digital-future-daily/2024/07/10/the-trouble-with-age-gating-the-internet-00167392) update. These prompted us to make changes in the device to monitor for variability in quantity and quality of input data so that we could more quickly resolve data issues and build feedback loops to frontline clinicians to improve data entry timing and accuracy.


We were able to demonstrate generalizability because of the size and diversity of our dataset, and the prospective validation ensured that the model behaved the same in real-life as it did in the laboratory. We are grateful to the FDA for pushing for such robust testing of eCART and hope that future analyses of AI medical devices show more consistent reliance on prospective validation.
