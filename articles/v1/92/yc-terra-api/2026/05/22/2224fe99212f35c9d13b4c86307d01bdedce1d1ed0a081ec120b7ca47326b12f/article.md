---
schema_version: "1.0.0"
document_id: "2224fe99212f35c9d13b4c86307d01bdedce1d1ed0a081ec120b7ca47326b12f"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/parkrun-vs-vo2-max"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:1901ec03cceb17713832f655cab78f4855f94261ff51282bb8cf6b3e6d39c8aa"
---

# Is Parkrun a Better Measure of Fitness Than VO2 Max | Terra

## How Does It Compare to Other Training Metrics?


I tested 13 training metrics against Parkrun pace on the same matched set of 33 users, all computed within-user to strip out between-person confounding.


A note before the numbers: our CTL (Chronic Training Load) and ATL (Acute Training Load) are computed from session-average heart rate, not second-by-second data. A proper Bannister TRIMP integrates heart rate continuously across a session, weighting every second by how hard your heart is working. What we have is a single average HR per session, which means a steady 60-minute tempo run and a set of hard intervals with recoveries at the same average HR get identical TRIMP scores. Our CTL and ATL are approximations.


***Figure 3:** All metrics ranked by within-user median |r|. The larger r, the better. It shows the metric is “better” at predicting Parkrun pace. Note Frequency is hard to beat, mirroring earlier findings.* **Training metric** **Within-user median r** **How it is computed**


CTL (Banister fitness) −0.39 42-day exponentially weighted TRIMP average


Run frequency (sess/wk) −0.38 Number of running sessions per week


**Speed-HR fitness** **−0.38** **Rolling speed-HR curve shift vs baseline**


Total frequency (sess/wk) −0.38 All aerobic sessions per week


Run volume (km/wk) −0.35 Weekly running distance


Total volume (km/wk) −0.33 All-sport weekly distance


Duration (hrs/wk) −0.31 Total training hours


Longest run (km) −0.26 Longest single run that week


ATL (Banister fatigue) −0.21 7-day TRIMP average — but wrong sign for fatigue


% hard sessions −0.08 Near zero


TRIMP per minute −0.07 Near zero


Mean intensity +0.03 Essentially no signal


TSB (fitness minus fatigue) −0.02 The Banister composite — no signal


The speed-HR metric sits third in the table, tied with run frequency and just behind CTL. On a matched user set, it performs as well as any traditional training metric and crucially, it measures something fundamentally different.


## Two Independent Fitness Signals


This is the finding that I think is most interesting. CTL and speed-HR fitness are barely correlated within individuals, median within-user r = 0.11. They are measuring different things entirely:


- **CTL** captures how much training you have accumulated. It is a load metric. It knows how much work you have done, weighted by heart rate, but it does not know how your body has responded to that work.
- **Speed-HR fitness** captures the physiological adaptation. We can think of it as measuring whether your aerobic engine has actually improved, whether you can sustain a faster pace at the same cardiac cost. It does not know how much you trained, only whether you got fitter.


Speed-HR wins for 15 of 33 users. CTL wins for 18. Neither dominates. They attack fitness from orthogonal directions.


## The Wider Leaderboard


To put all of this in context, here is how the full suite of training metrics performed across the three analytical approaches I described in my previous work; within-user tracking, paired before-and-after comparisons, and heart rate efficiency, tested on the full park run cohort.


**Training metric** **Approach 1: Within-user** **Approach 2: Paired delta** **Approach 3: HR efficiency**


Run volume (km/wk) r = −0.43 r = −0.36 r = −0.30


Total volume (km/wk) r = −0.41 r = −0.47 r = −0.26


CTL (Banister) r = −0.40 r = −0.44 r = −0.26


Run frequency r = −0.37 r = −0.35 r = −0.28


Total frequency r = −0.37 r = −0.33 r = −0.28


Duration (hrs/wk) r = −0.30 r = −0.24 r = −0.25


ATL (Banister) r = −0.28 r = −0.30 r = −0.16


Mean intensity r = −0.12 r = −0.15 r = −0.21


Longest run (km) r = −0.09 r = −0.22 r = −0.23


% hard sessions r = −0.09 r = −0.01 r = −0.15


TRIMP per minute r = −0.05 r = −0.04 r = −0.16


TSB (CTL−ATL) r = 0.00 r = −0.01 r = −0.04


The pattern is consistent. Volume and frequency dominate (again!) Intensity (as measured in terms of session average) adds almost nothing. And the Bannister model's fitness-minus-fatigue composite (TSB) shows no signal at all, more on that below.


## A Note on the Bannister Model


The Bannister impulse-response model predicts that performance is fitness minus fatigue, CTL minus ATL. If that is true, TSB (the difference) should be a strong predictor: positive TSB means you are fit and rested, negative means you are carrying fatigue.


But TSB shows essentially zero correlation with park run pace across all three approaches (r = 0.00, −0.01, −0.04). The fitness-minus-fatigue composite does not predict 5 km performance in this dataset.


ATL on its own does correlate with pace, but in the wrong direction for a fatigue signal. Higher ATL (more recent training load) associates with faster park runs (r = −0.28), not slower ones. ATL is behaving as a second fitness signal, not a fatigue signal.


That could makes sense at this level: a week of high training load means you have been training hard recently, which on average means you are fitter. In a joint model with both CTL and ATL, only 5 of 15 highest-adherence users show the expected positive ATL coefficient (more fatigue = slower). For the majority, more recent load means faster running.


One likely reason is that Parkrun happens weekly, and most people in this dataset are not “tapering” for it. They are just showing up on Saturday morning during normal training. In my opinion that is a core part of it’s value! In a dataset of athletes peaking for specific races after deliberate tapers, TSB might tell a different story.


## Disentangling Volume and CTL


Run volume and CTL are almost perfectly correlated in this dataset, r = 0.94 pooled, median r = 0.86 within individuals. They are measuring nearly the same thing. So which one is actually doing the work?


I ran partial correlations, controlling for run volume and asking whether CTL adds anything beyond simple distance.


**Metric (controlling for run volume)** **Strict (15 users) partial r** **Relaxed (38 users) partial r**


Cross-training volume −0.27*** −0.20***


Longest run −0.19*** −0.12**


Mean intensity −0.12* −0.13**


CTL (Banister) −0.07 (NS) −0.02 (NS)


Controlling for run volume, CTL drops to non-significance in both cohorts. But the reverse test tells a more nuanced story. Using residual analysis, stripping the shared variance and testing only the orthogonal components, the unique component of CTL (the part that run volume cannot explain) still correlates with pace: r = −0.18, p < 0.001. The unique component of volume does not: r = −0.06, p = 0.24. CTL subsumes volume, not the other way around. But the advantage is modest — ΔR² of about 0.03.


Coming back to the TRIMP limitation I flagged earlier. Our CTL and ATL are computed from session-average heart rate a 60-minute run at an average of 150 bpm could be a steady tempo effort or a set of hard intervals with recoveries, and our data treats them identically.


A proper Bannister TRIMP integrates HR second by second, weighting higher heart rates exponentially more, we will dive into this in the next analysis. That would almost certainly increase the CTL signal and might change the volume-vs-CTL story.


It would also improve the speed-HR metric, which currently bins session-average heart rate rather than using the full within-session distribution.


***Figure 4:** Individual plots of volume and pace. Volume as a strong predictor, is something we keep seeing in wearable data analysis.*


## Why These Correlations Are Stronger Than They Look


**Domain** **Typical R²** **What it measures**


Wearable sleep metrics → next-day performance 0.01–0.03 Sleep duration/HRV explaining workout quality


VO₂max estimates from wearables 0.10–0.20 Wrist-based estimates vs lab gold standard


Single blood biomarker → health outcome 0.02–0.08 One lab value predicting a clinical event


Training load → race performance (this study) 0.14–0.19 Single metric explaining within-user parkrun pace


Speed-HR cross-user validation (this study) 0.87 Training-only metric explaining race aerobic efficiency


In free-living wearable research, explaining 14–19% of performance variance from a single training metric is a strong result. Most wearable-derived metrics explain low single-digit percentages. The cross-user validation at R² = 0.87 is exceptional, it indicates that the training metric captures the same underlying signal as a standardized race.


## What This Means


Three things stand out from this analysis I think.


First, the speed-HR curve works. It may be simple but as lab-free fitness metric computed from session-average wearable data it predicts the same metric measured under standardized race conditions at r = 0.93.


We would expect this, we are looking at the same thing. Within individuals, it tracks fitness changes over time. It is not a noisy proxy, it looks like it’s capturing real physiological adaptation.


Second, volume predicts performance as well as or better than any sophisticated metric. Run more kilometers per week and you will run faster at park run. Not harder kilometers. Just more of them. The top of every leaderboard is dominated by volume and frequency metrics. This is a signal that keeps appearing from our data.


Third, fitness-from-training-load (CTL) and fitness-from-physiological-adaptation (speed-HR) are nearly independent signals. They correlate at r = 0.11 within individuals, yet both predict park run performance equally well. One measures the dose. The other measures the response. The fact that both independently predict the outcome, from different angles, is the strongest evidence that the underlying signal is real.


**References**


**References**


1. Banister, E. W. (1975). *A systems model of training for athletic performance.* Australian Journal of Sports Medicine, 7, 57–61. (See also: Clarke, D. C., & Skiba, P. F. (2013). *Rationale and resources for teaching the mathematical modeling of athletic training and performance.* American Journal of Physiology.)
2. Pobiruchin, M., et al. (2017). *Accuracy and Adoption of Wearable Technology Used by Active Citizens: A Marathon Event Field Study.* JMIR mHealth and uHealth, 5(2), e24.[https://mhealth.jmir.org/2017/2/e24/](https://mhealth.jmir.org/2017/2/e24/)
3. Neshitov, A., et al. (2023). *Estimation of cardiorespiratory fitness using heart rate and step count data collected from wearable devices.* Frontiers in Physiology.[https://pmc.ncbi.nlm.nih.gov/articles/PMC10517160/](https://pmc.ncbi.nlm.nih.gov/articles/PMC10517160/)
4. Vickers, A. J., & Vertosick, E. A. (2016). *An empirical study of race times in recreational endurance runners.* BMC Sports Science, Medicine and Rehabilitation, 8, 26.[https://bmcsportsscimedrehabil.biomedcentral.com/articles/10.1186/s13102-016-0052-y](https://bmcsportsscimedrehabil.biomedcentral.com/articles/10.1186/s13102-016-0052-y)
5. Haake, S. (2020). *The Role of Technology in Promoting Physical Activity: A Case-Study of parkrun.* Proceedings, 49(1), 80.[https://www.mdpi.com/2504-3900/49/1/80](https://www.mdpi.com/2504-3900/49/1/80)
