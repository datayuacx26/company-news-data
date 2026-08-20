---
schema_version: "1.0.0"
document_id: "036dbc5d11699ad34f612c2c50b944fc7f143e41a4fc98d528b349e5700a73d5"
company_key: "yc-frekil"
company: "Frekil"
source_id: "yc-frekil-rss-a6f9e192a2a2"
canonical_url: "https://www.frekil.com/blog/propensity-score-methods-survival-outcomes-rwe"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-25T05:42:05.860173+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:ff7d66de9e9f4d051edc04a136c1a9262976d902bf6b83c20fd196727b99e536"
---

# Propensity Scores for Survival Outcomes: What RWE Teams Should Report

A propensity score is easiest to understand through the mistake it prevents. In real-world data, treated and untreated patients are rarely comparable at baseline. If we compare their outcomes directly, we may be measuring who they were before treatment, not what the treatment did.


Imagine a real-world study of patients discharged after an acute myocardial infarction. Some patients received a statin prescription at discharge. Others did not. The question sounds simple:


Did discharge statin prescribing reduce the risk of death over the next 8 years?


A naive analysis would compare 8-year mortality among patients who received statins with 8-year mortality among patients who did not. But that comparison is not fair by default. The patients who received statins may have been younger, healthier, better documented, more likely to receive guideline-based care, or different in many other baseline ways.


Those same baseline differences can affect survival.


That is the reason propensity scores exist. They help us build a comparison group where treated and untreated patients looked similar before treatment began. For survival outcomes, that design has to support more than a single Cox model. It should support survival curves, absolute risk differences at meaningful time points, hazard ratios when appropriate, and a clear statement of who the estimate applies to.


Peter C. Austin's tutorial on propensity score methods for survival outcomes is useful because it asks observational studies to report effects in a way that resembles randomized trial reporting: not just "the hazard ratio was lower," but "here is the survival experience under treatment and comparator strategies, here is the absolute difference, and here is the relative effect."[Austin, Statistics in Medicine](https://doi.org/10.1002/sim.5984)


> The job of the propensity score is not to produce the result. Its job is to make the comparison worth analyzing.


## The problem propensity scores solve


In a randomized trial, treatment assignment is randomized. That does not make every trial perfect, but it gives the study a major advantage: on average, the treatment and control groups should be similar at baseline.


Real-world evidence does not get that protection automatically.


Patients receive treatments for reasons. A physician prescribes a statin because of the patient's history, risk profile, contraindications, lab values, age, prior care, and clinical context. A patient may not receive a statin because they are frail, intolerant, near end of life, missing documentation, or treated in a setting with different prescribing behavior.


If we ignore those reasons, we can confuse treatment effect with treatment selection.


A propensity score is each patient's estimated probability of receiving the treatment, using only facts known before treatment.


propensity score = probability of treatment given baseline patient information


A simplified version might look like this:


**Patient A** 64 years old, moderate risk, received a statin, estimated statin probability 0.72.


**Patient B** 66 years old, moderate risk, did not receive a statin, estimated statin probability 0.70.


**Idea** These patients had similar baseline chances of receiving treatment, so they may be more credible comparators than two random patients from the database.


The score is not magic. It only balances measured baseline covariates. It does not fix unmeasured confounding, bad time-zero definitions, poor overlap, immortal time bias, post-treatment adjustment, missing endpoints, or informative censoring.


But when used carefully, it gives the study team a design tool: make treated and untreated groups comparable before looking at the outcome.


## Define the survival question first


Before estimating any score, the team should write down the trial it wishes it could run.


For the statin example, the target trial might be:


**Eligibility** Patients hospitalized for acute myocardial infarction who survived to discharge.


**Treatment** Statin prescription at hospital discharge.


**Comparator** No statin prescription at hospital discharge.


**Time zero** The discharge date, because that is when the treatment strategy begins.


**Outcome** Death during follow-up.


**Follow-up** From discharge until death, censoring, or the end of the study window.


This step is especially important for time-to-event outcomes. A survival study is not only asking whether an event happened. It is asking when the event happened, when follow-up began, how long patients were observed, and why some patients stopped being observed.


Censoring means that a patient's exact event time is not observed. For example, a patient may still be alive at the end of the study, or may leave the data source before the 8-year endpoint. Survival methods can handle censoring, but only under assumptions. If patients are censored for reasons related to their future risk, the analysis can be biased.


Time zero is equally important. If follow-up starts after treatment has already created a survival advantage, the study can accidentally give one group "immortal" time during which the event could not have been counted. Propensity scores cannot repair a broken time-zero definition.


## Decide whose effect you mean


Once the target trial is clear, the team has to decide whose treatment effect it wants.


The average treatment effect, or ATE, asks:


What would happen in the full eligible population if everyone received a statin versus if no one received a statin?


The average treatment effect for the treated, or ATT, asks:


Among the patients who actually received a statin, what would have happened if those same patients had not received one?


Those are different questions. Both can be legitimate. The right one depends on the decision.


If a guideline team is asking what should happen for all eligible post-MI patients, the ATE may be more natural. If the study is asking what benefit was associated with treatment among patients who actually received statins, the ATT may be more natural.


The method has to match the question.


**ATE** Population-level effect in all eligible patients.


**ATT** Effect among patients who actually received treatment.


**Matching** Conventional pair matching usually targets the ATT, or the ATT among treated patients who can be matched.


**Weighting** IPTW can target the ATE or ATT depending on the weights.


This is why "estimand" matters, even if the word is ugly. It means the exact treatment effect the study is trying to estimate. If the estimand is unclear, the paper may report a precise number while making a vague claim.


## Use only facts known before treatment


The propensity score model should use baseline variables: information available before the treatment decision.


In the statin example, reasonable baseline variables might include age, sex, comorbidities, prior cardiac history, lab values, vital signs, disease severity markers, presenting symptoms, prior medications, hospital characteristics, calendar time, and other factors known before discharge that could influence both prescribing and survival.


The practical question is:


If this were a randomized trial, which baseline differences would worry us if they were imbalanced between arms?


Those are the variables the design should try to balance.


The covariate list should come from clinical and causal reasoning, not a quick screen of p-values. A variable can be important even if it is not statistically significant in a baseline table. The goal is not to predict treatment as cleverly as possible. The goal is to balance the variables that could make the treated and untreated groups unfairly different.


Post-treatment variables should not go into the propensity score. If a lab result, hospitalization, adherence measure, or complication happens after discharge, adjusting for it may remove part of the treatment effect or introduce new bias.


## Matching makes the comparison tangible


Propensity score matching is the easiest method to picture.


For each treated patient, find an untreated patient with a similar propensity score. Then compare outcomes in the matched sample.


In the statin study, that means comparing statin patients with non-statin patients who had similar baseline probabilities of receiving a statin. Instead of comparing all treated patients with all untreated patients, the design asks for a fairer comparison:


Among patients who looked similarly likely to receive a statin at discharge, did the statin group survive longer?


Matching is attractive because it is tangible. A reviewer can understand the comparison. A clinical team can inspect matched groups and ask whether the patients really look alike.


But matching has a cost. Some treated patients may not have a good untreated match. In Austin's case study, caliper matching retained about 80% of treated patients and excluded about 20% because no suitable untreated match was available.


That matters because the target population changes. The result no longer cleanly describes every treated patient. It describes the treated patients who could be matched, and those may be less severe, more typical, or otherwise different from the unmatched treated patients.


Matching also changes the statistical structure. Matched treated and untreated patients should not be treated as two unrelated samples. For survival curves, the comparison should respect the matched sets. For a marginal hazard ratio, a treatment-only Cox model in the matched sample can be used with robust variance estimation that accounts for clustering within matched pairs.


The report should say exactly how matching was done: nearest neighbor or optimal matching, caliper width, whether replacement was allowed, the score scale used for matching, the number of treated patients retained, and the number excluded.


## Weighting keeps more patients, but can become unstable


Inverse probability of treatment weighting, or IPTW, takes a different approach.


Instead of dropping patients who are hard to match, weighting keeps patients and assigns them weights based on how surprising their treatment was.


If a patient looked unlikely to receive a statin but did receive one, that patient may receive a large weight. If a patient looked very likely to receive a statin but did not receive one, that patient may also receive a large weight.


The goal is to create a weighted pseudo-population where measured baseline covariates are balanced between treatment groups. In plain English, weighting tries to make the treated and untreated groups look as if treatment assignment was less tied to baseline risk.


That only works approximately, and only under assumptions: the propensity model must be reasonable, the groups must have enough overlap, and extreme weights must not let a few unusual patients dominate the estimate.


This is why IPTW reports should include weight diagnostics. The team should show whether weights are stable, whether trimming was used, and whether trimming changed the target population. Stabilized or trimmed weights can reduce instability, but they are design choices, not cosmetic cleanup.


The advantage of weighting is flexibility. With the right weights, IPTW can estimate either the ATE or the ATT. It also connects to marginal structural models, which are useful when treatment or confounding changes over time.


The disadvantage is that the comparison is less visible than matching. A weighted pseudo-population can be valid, but the study has to work harder to make it inspectable.


## Report the result like a survival study


After the matched or weighted population is built, the outcome analysis should answer a survival question, not just produce a familiar model coefficient.


A hazard ratio compares instantaneous event hazards among people still at risk at each time point. It is useful, but it is not a risk ratio, and it does not directly tell the reader the probability of death by year 1, year 5, or year 8.


That is why Austin emphasizes marginal survival curves. They show estimated survival over time in the treated and untreated strategies. From those curves, the study can report absolute differences at clinically meaningful time points.


For example, in Austin's myocardial infarction case study, the absolute reduction in 8-year mortality associated with discharge statin prescribing varied across propensity score designs:


**Caliper matching** Absolute 8-year mortality reduction of 0.039, corresponding to a number needed to treat of 26.


**Optimal matching** Absolute 8-year mortality reduction of 0.023, corresponding to a number needed to treat of 45.


**ATE weighting** Absolute 8-year mortality reduction of 0.055, corresponding to a number needed to treat of 18.


**ATT weighting** Absolute 8-year mortality reduction of 0.018, corresponding to a number needed to treat of 57.


Those differences are the point. The method changes the population and the claim. A single hazard ratio would hide too much of the decision-relevant story.


A strong survival RWE report should include:


- Survival curves for the treated and comparator strategies.
- Absolute event-probability differences at meaningful time points.
- A marginal hazard ratio when proportional hazards is a reasonable summary.
- A check of whether proportional hazards is plausible.
- Alternative summaries when needed, such as restricted mean survival time or time-specific risks.


For nonfatal outcomes, one more caution matters. If death can prevent the outcome from occurring, the study may have a competing-risk problem. In those settings, cumulative incidence methods may be more appropriate than treating` 1 - survival` as the event probability.


## Check whether the groups really became comparable


A propensity score model is not good because it predicts treatment well. It is good if it balances the measured baseline variables needed for the comparison.


The C-statistic is not the main test. A high C-statistic can even be a warning sign that treated and untreated patients are very different. What matters is whether the matched or weighted groups look similar after adjustment.


The standard diagnostic is the standardized mean difference. It puts baseline differences on a common scale so the team can compare imbalance before and after matching or weighting. Many researchers use an absolute standardized mean difference above 0.10 as a sign that imbalance may still matter, but judgment still matters.


Balance checks should include:


- Baseline balance before adjustment.
- Baseline balance after matching or weighting.
- Propensity score overlap between groups.
- The percentage of treated patients retained after matching.
- Weight distributions and effective sample size for IPTW.
- Important interactions or nonlinear terms when they matter clinically.
- Any covariates that remain meaningfully imbalanced.


Balance should be inspected before the outcome is interpreted. If the design does not balance the measured confounders, the outcome model should not be relied on to rescue the claim.


## Say what assumptions the result needs


Propensity score analyses are not assumption-free. They are only credible when the assumptions are visible.


The most important assumptions are:


**Exchangeability** After adjustment, treated and untreated patients are comparable on the causes of treatment and outcome.


**Measured confounding** The important confounders are measured well enough to use in the design.


**Overlap** Patients with similar baseline profiles could plausibly receive either treatment strategy.


**Consistency** The treatment strategy is defined clearly enough that "statin at discharge" means a coherent intervention.


**Censoring** Loss of follow-up is handled in a way that does not distort the survival comparison.


The report should also discuss what could still be missing: frailty, contraindications, physician judgment, adherence, socioeconomic status, disease severity, care quality, undocumented intolerance, or any other factor that could influence both treatment and survival.


Sensitivity analysis asks how strong a hidden bias would need to be to change the conclusion. Austin's case study is a useful warning: a small p-value did not mean the result was immune to unmeasured confounding.


For survival RWE, sensitivity checks should go beyond hidden confounding. They should also test time-zero definitions, exposure grace periods, censoring assumptions, missing data, proportional hazards violations, competing risks, caliper choices, trimming choices, and alternative outcome definitions.


The goal is not to make observational evidence sound weak. The goal is to make the strength of the evidence inspectable.


## The practical workflow


For a novice reader, the workflow should be simple:


1.


State the treatment question in trial-like language.


2.


Define eligibility, treatment, comparator, time zero, outcome, and follow-up.


3.


Decide whether the target is the full eligible population, the treated population, or both.


4.


Build a propensity score using only baseline variables.


5.


Use matching or weighting to construct a comparable population.


6.


Prove that measured baseline covariates are balanced.


7.


Estimate survival curves in the matched or weighted population.


8.


Report absolute event-probability differences at meaningful time points.


9.


Report a hazard ratio only with the right interpretation and diagnostics.


10.


Stress test the result against hidden bias and survival-specific design choices.


That sequence keeps the reader from treating propensity scores as a modeling trick. They are part of the study design.


## The takeaway


The simplest way to misuse propensity scores is to treat them as a button before a Cox model: estimate the score, match patients, report a hazard ratio, move on.


That is not enough for time-to-event RWE.


A better analysis starts with a trial-like question, uses the propensity score to build a credible comparison, checks whether the design worked, and reports survival results in terms that decision-makers can understand.


For the statin example, the final answer should not be only:


The hazard ratio was lower after propensity score adjustment.


It should be closer to:


In this matched or weighted population, statin prescribing at discharge was associated with better survival over 8 years; the report shows the survival curves, the absolute mortality difference, the marginal hazard ratio, the balance diagnostics, the patients included, and the sensitivity of the result to hidden bias and design choices.


That longer sentence is more useful because it says what was compared, who the comparison applies to, how large the effect was, and what assumptions the reader has to believe.


That is how a propensity score survival analysis becomes decision-grade RWE instead of a polished observational association.
