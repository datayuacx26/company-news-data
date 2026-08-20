---
schema_version: "1.0.0"
document_id: "765c1b120d132f0600f3fcefe1c4b75645ff9af92bb3d202dadb36d36b776a63"
company_key: "yc-frekil"
company: "Frekil"
source_id: "yc-frekil-rss-a6f9e192a2a2"
canonical_url: "https://www.frekil.com/blog/causal-inference-real-world-data-propensity-scores"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-25T05:42:05.860173+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:7b17f57812b8e562ebff13ecb47605aa0eddcb690e63a8e01eb766884db1334a"
---

# Causal Inference in Real-World Evidence: From DAGs to Target Trials

Real-world evidence is not just a data problem. It is a comparison problem. Causal inference is the discipline that helps us decide whether a real-world comparison is fair enough to support a decision.


Imagine a hospital system wants to evaluate trauma care. Some severely injured patients are treated at trauma centers. Others are treated at non-trauma centers. The outcome is emergency department mortality.


At first glance, the analysis sounds simple:


Compare mortality among patients treated at trauma centers versus non-trauma centers.


But that comparison is not automatically causal. Patients do not arrive at hospitals by random assignment. A patient may go to a trauma center because the injury is more severe, because an ambulance protocol routed them there, because the hospital is nearby, because the region has more resources, or because the patient has characteristics that affect triage.


Those same factors can also affect mortality.


That is the central difficulty in real-world evidence. The data may contain thousands or millions of patients, but the causal question still depends on whether the treated and comparison groups are exchangeable enough to interpret the outcome difference.


> The first job of causal inference is to stop us from confusing a difference in outcomes with the effect of a treatment.


## Why causal inference matters


Real-world studies often inform high-stakes decisions: where to invest in care delivery, whether a safety signal is real, whether an external control is credible, whether a label expansion is plausible, or whether a payer should believe an effectiveness claim.


The risk is not that RWE produces no answer. The risk is that it produces an answer that looks precise but answers the wrong question.


In the trauma-care example, suppose trauma-center patients have higher mortality. A naive reading might say trauma centers perform worse. But trauma centers may receive the most severe cases. The observed difference could reflect baseline severity, not care quality.


Now suppose trauma-center patients have lower mortality. A naive reading might say trauma centers save lives. That may be true, but it still needs to be earned. Trauma-center patients may differ in geography, transport time, prehospital care, documentation quality, or access to specialized services before the hospital encounter is even measured.


Both naive interpretations can be wrong for the same reason: the groups were different before treatment.


Causal inference gives the study team a disciplined way to ask:


- What exactly is the treatment or exposure?
- Who is the comparison group?
- When does follow-up begin?
- Which baseline differences must be adjusted for?
- Which variables should not be adjusted for?
- What assumptions are required for the estimate to mean what we say it means?


Without that discipline, RWE becomes a polished association study. With it, RWE can become decision-grade evidence.


## The question is counterfactual


The causal question is not:


Did trauma-center patients have lower mortality than non-trauma-center patients?


The causal question is:


What would have happened to the non-trauma-center patients if those same patients had instead been treated at trauma centers?


That second version is the counterfactual question. We observe the outcome under the care pathway the patient actually received. We do not observe the outcome under the pathway they could have received.


That missing outcome is what the study has to reconstruct.


For one patient, the idea can be written as:


outcome under trauma-center care - outcome under non-trauma-center care


The math is less important than the implication: a causal study is trying to compare the same kind of patient under two different treatment strategies. Because we cannot observe both paths for the same person, we need a comparison group that can stand in for the missing path.


That is why the comparator is the heart of the study.


## Start with a causal DAG


Before building a model, the team should draw the causal assumptions. A causal DAG, or directed acyclic graph, is a simple diagram that shows which variables are believed to cause which other variables. It is not decoration. It is a way to decide what must be adjusted for and what must be left alone.[Causal diagrams in epidemiologic research](https://journals.lww.com/epidem/abstract/1999/01000/causal_diagrams_for_epidemiologic_research.8.aspx)


For the trauma-care question, a simplified DAG might look like this:


This diagram makes the problem visible.


Injury severity is a confounder if it affects both where the patient is treated and whether the patient dies. If severe cases are routed to trauma centers and severe cases are more likely to die, severity must be adjusted for.


Distance and geography may also confound the comparison. They influence which hospital is reachable and may influence time to care, transfer patterns, and outcome.


Time to definitive care is trickier. If the question is the total effect of being treated at a trauma center, and trauma-center routing changes time to care, then time to care may be part of the causal pathway. Adjusting for it could remove part of the effect we are trying to estimate. If the question is instead the effect of hospital capability holding transport time fixed, then the design changes.


That is why DAGs matter. They force the team to name the estimand before choosing covariates.


> A covariate list is not a causal plan. A DAG is where the team writes down why each variable belongs in or out of the adjustment set.


## Then emulate the target trial


Once the causal structure is clear, the next step is target trial emulation. The idea is simple: write down the randomized trial you wish you could run, then emulate it as closely as possible using observational data. Hernan and colleagues describe this as specifying the trial protocol first, then mapping each part to real-world data.[Target trial emulation](https://jamanetwork.com/journals/jama/fullarticle/2799678)


For the trauma-care example, the target trial might be:


**Eligibility** Adults aged 18-64 with severe traumatic injury, such as injury severity score at least 25.


**Treatment strategies** Initial treatment at a trauma center versus initial treatment at a non-trauma center.


**Time zero** The moment the patient becomes eligible for the care pathway comparison, such as emergency department arrival or first qualifying trauma encounter.


**Outcome** Emergency department mortality.


**Causal contrast** The difference in mortality if the same eligible patients were treated under one hospital strategy versus the other.


**Analysis plan** Construct comparable groups, check balance, estimate the effect, and test sensitivity to hidden bias.


This step prevents many common RWE mistakes.


If time zero is defined after treatment begins, the study can introduce immortal time bias. If eligibility is defined differently across groups, the comparator is not fair. If the outcome window starts at different clinical moments, the study may compare unequal follow-up. If covariates are measured after treatment, the model may adjust away part of the effect or introduce new bias.


Target trial emulation turns "we have data" into "we have a study design."


## Use propensity scores as a design tool


After the target trial is specified, propensity scores can help construct the comparison group.


A propensity score is the probability that a patient receives the exposure given measured baseline covariates:


propensity score = probability of trauma-center treatment given baseline patient profile


In this example, the score might use pre-treatment variables such as age, injury severity, chronic conditions, insurance payer, geography, multiple injury, and sex. The purpose is not to predict hospital assignment for its own sake. The purpose is to find trauma-center and non-trauma-center patients who looked similar before the treatment strategy began.


There are several ways to use the score:


- Matching pairs each non-trauma-center patient with a similar trauma-center patient.
- Weighting creates a pseudo-population where measured baseline covariates are balanced.
- Stratification compares patients within bands of similar treatment probability.


In the trauma-care case this article is built around, the design used propensity score matching. Each non-trauma-center patient was matched to one trauma-center patient using baseline covariates, creating matched pairs for comparison.


The important point is that the propensity score is not the result. It is part of the design.


## Check whether the design worked


After matching or weighting, the team has to ask whether the groups are actually comparable.


This is where balance diagnostics matter. A good study reports whether measured baseline covariates are balanced after adjustment. The usual tool is the standardized mean difference. The exact formula is less important for a novice reader than the interpretation: it puts group differences on a common scale so the team can see whether imbalance remains.


For the trauma-care study, the key question is not "did the logistic regression converge?" It is:


After matching, do non-trauma-center and trauma-center patients have similar baseline injury severity, age, comorbidity, geography, payer, and other pre-treatment characteristics?


If the answer is no, the outcome comparison is still suspect.


If the answer is yes, the study has made progress. It has not proven causality, because unmeasured confounding may remain. But it has made the causal comparison more credible by showing that measured baseline differences no longer explain the result as easily.


Good diagnostics include:


- Propensity score overlap before and after adjustment.
- Covariate balance tables.
- Love plots showing standardized differences.
- Checks for extreme weights if weighting is used.
- Sensitivity analysis for hidden bias.


Balance is not a cosmetic appendix. It is the evidence that the design did what it promised.


## What the result can and cannot say


If the matched trauma-care analysis finds lower mortality for trauma-center care, the careful interpretation is not:


Trauma centers always cause lower mortality.


The careful interpretation is closer to:


Among patients comparable on the measured baseline variables used in the design, treatment at trauma centers was associated with lower emergency department mortality; under the assumptions of no unmeasured confounding, consistent treatment definition, and adequate overlap, this can be interpreted as a causal effect for the target population.


That sentence is longer because causal claims are conditional. They rest on assumptions.


The important assumptions are:


- Exchangeability: after adjustment, the groups are comparable on causes of treatment and outcome.
- Positivity: patients with similar profiles could plausibly appear in either treatment group.
- Consistency: "trauma-center treatment" means a sufficiently coherent care strategy.
- No interference: one patient's hospital assignment does not change another patient's outcome in a way that breaks the comparison.


A rigorous RWE study does not hide those assumptions. It makes them inspectable.


## The clean workflow


For a novice reader, the workflow should feel sequential:


1. Define the decision.


What decision will this evidence support? Trauma-system planning, payer coverage, safety review, label expansion, or clinical guideline development?


1. Write the counterfactual question.


For the patients who received one strategy, what would have happened under the alternative strategy?


1. Draw the DAG.


List the variables that cause treatment choice, outcome, or both. Identify confounders, mediators, and colliders before touching the model.


1. Specify the target trial.


Define eligibility, treatment strategies, time zero, follow-up, outcome, causal contrast, and analysis plan.


1. Build the comparison.


Use matching, weighting, stratification, or regression to make treatment groups comparable on measured pre-treatment covariates.


1. Prove balance.


Show overlap and covariate balance. If the groups are still different, revise the design or narrow the target population.


1. Estimate the effect.


Only after the design is credible should the team estimate the outcome difference.


1. Stress test the conclusion.


Run sensitivity analyses for hidden bias, alternative definitions, missingness, and modeling choices.


## The takeaway


Causal inference matters because real-world data is persuasive even when it is wrong. A large dataset can make a biased comparison look authoritative. A clean model can make a bad comparator look scientific. A tiny p-value can distract from a broken time zero.


The antidote is not more math. It is better design.


For real-world evidence, that design starts with the counterfactual question, becomes explicit in a causal DAG, gets operationalized through target trial emulation, and is defended through balance checks and sensitivity analysis.


That is how RWE earns the right to influence decisions. Not by claiming to be a randomized trial, and not by apologizing for being observational, but by making the comparison clear enough that others can inspect it, challenge it, and trust it.
