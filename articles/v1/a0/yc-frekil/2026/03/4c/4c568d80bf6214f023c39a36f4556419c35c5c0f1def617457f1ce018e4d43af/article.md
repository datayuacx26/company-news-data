---
schema_version: "1.0.0"
document_id: "4c568d80bf6214f023c39a36f4556419c35c5c0f1def617457f1ce018e4d43af"
company_key: "yc-frekil"
company: "Frekil"
source_id: "yc-frekil-rss-a6f9e192a2a2"
canonical_url: "https://www.frekil.com/blog/why-rwe-studies-take-18-months"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-07-25T05:42:05.860173+00:00"
fetched_at: "2026-07-28T22:17:35.086279+00:00"
content_hash: "sha256:67907ddbeef470c46630abdd4e45176d528626619880c6130f50424738f98881"
---

# Why RWE Studies Still Take Months: Where the Time Goes

Ask a pharma, biotech, or CRO team why a retrospective RWE study still stretches for months, and the first answer usually sounds technical: messy EHRs, claims logic, endpoints, missing data, statistical review. All of that is real. But the deeper reason is simpler: every study has to rebuild the path from raw clinical data to decision-grade evidence.


The final statistical model is rarely the part that burns the calendar. The slow work is upstream: turning a clinical question into a protocol, proving the dataset is fit for the decision, extracting and standardizing source data, defining cohorts without ambiguity, validating every assumption, and packaging the result so medical, regulatory, HEOR, or payer teams can trust it. The FDA's RWE program uses the same language: real-world data only becomes useful real-world evidence when it is fit for purpose and can support the decision being made.[FDA, Real-World Evidence](https://www.fda.gov/science-research/science-and-research-special-topics/real-world-evidence)


## Where the months actually go


A typical retrospective study is not one workflow. It is a chain of handoffs. The research team frames the question. Epidemiology and biostatistics turn that question into a design. Data owners determine whether the right population, exposure, outcomes, and follow-up are observable. Engineers transform the data. Analysts implement the cohort and model. Stakeholders review the outputs. Any weak link sends the study backward.


> RWE timelines expand because every step has to answer the same question in a different form: can we trust this evidence enough to act on it?


Protocol development is the first place time accumulates. In a trial, many design choices are enforced prospectively by randomization and protocol-controlled follow-up. In RWE, those choices have to be specified after care has already happened: index date, washout period, exposure window, comparator definition, outcome ascertainment, confounders, censoring rules, and sensitivity analyses. The[STaRT-RWE template](https://www.bmj.com/content/372/bmj.m4856) exists because small ambiguities in prose can become large differences in implementation.


Data access is the next constraint. Claims, EHR, registry, and mortality data can each answer different parts of the question, but each source comes with its own contracting, governance, privacy, refresh-cycle, linkage, and data-use constraints. Even internal data is rarely “ready” simply because it exists. A researcher still needs permission, a reproducible extraction path, a de-identification process, and a way to understand what was captured versus what happened outside the system.


## The data preparation bottleneck


Once access is secured, the practical work begins. Claims data is structured but optimized for payment, not clinical nuance. EHR data is clinically rich but fragmented across notes, orders, labs, flowsheets, medication lists, and local coding practices. Registries may be curated but narrow. The FDA's 2024 guidance on EHR and claims data tells sponsors to assess whether those sources are relevant and reliable for the specific regulatory question, including data accrual, provenance, missingness, linkage, and validation.[FDA, EHR and Claims RWD Guidance](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/real-world-data-assessing-electronic-health-records-and-medical-claims-data-support-regulatory)


> The bottleneck is not data volume. It is data fitness: relevance, reliability, traceability, and enough clinical context to support the inference.


That is why harmonization takes so long. Variable names need to be normalized. Local codes need to be mapped. Units need to be reconciled. Eligibility criteria need to be translated into computable definitions. Dates need to be aligned into clean patient timelines. Missing values need to be characterized before anyone decides whether to impute, exclude, or redesign the endpoint. Duke-Margolis describes reliability checks in terms of completeness, conformance, plausibility, consistency, and provenance for exactly this reason.[Duke-Margolis, Fitness for Use and Reliability](https://healthpolicy.duke.edu/publications/determining-real-world-datas-fitness-use-and-role-reliability)


Common data models help, but they do not remove the work. OMOP CDM is valuable because it standardizes the structure and content of observational data so analyses can be run more consistently across sources.[OHDSI, OMOP Common Data Model](https://ohdsi.github.io/CommonDataModel/) EMA's DARWIN EU network uses the same idea: data partners standardize data into OMOP so it can be analyzed faster for regulatory use.[EMA, DARWIN EU](https://www.ema.europa.eu/en/about-us/how-we-work/data-regulation-big-data-other-sources/real-world-evidence/data-analysis-real-world-interrogation-network-darwin-eu) But every conversion still has to preserve clinical meaning. A mapped field is not the same thing as a validated endpoint.


## Cohort logic is where ambiguity becomes expensive


The most deceptively slow part of many RWE studies is cohort construction. “Patients with metastatic breast cancer who initiated second-line therapy after progression” sounds clear in a meeting. In data, it becomes a set of code lists, medication rules, washout windows, staging proxies, line-of-therapy logic, observation requirements, and exclusions. Each definition affects who enters the study and who disappears.


This is why transparent reporting standards matter. The[RECORD statement](https://journals.plos.org/plosmedicine/article?id=10.1371%2Fjournal.pmed.1001885) was created because routinely collected health data raises reporting issues that generic observational-study guidance does not fully cover, including code lists, linkage, and how the study population was identified from the source database. Without that transparency, reviewers cannot tell whether a result reflects biology, clinical practice, coding behavior, or an artifact of the extract.


## The analysis is visible, but it is not the whole study


Once the cohort is locked and the dataset is analysis-ready, the statistical work usually moves faster. That does not mean it is easy. The team still has to handle confounding, missingness, competing risks, censoring, subgroup definitions, negative controls, and sensitivity analyses. But those tasks are at least legible. They live in the SAP, the code, the tables, and the review comments.


The hidden drag is all the rework around the analysis: a medical reviewer questions a code list, the data team finds a site-specific lab unit problem, a comparator definition changes, follow-up is shorter than expected, or the endpoint turns out to be under-captured in the available source. Each issue is scientifically legitimate. Each one can add another loop.


## What changes when the plumbing is automated


This is the part Frekil is built around. The opportunity is not to replace epidemiologists or biostatisticians. It is to move the repetitive execution work into an agentic evidence layer, so the experts spend their time on the decisions that actually require judgment: whether the causal question is well-formed, whether the comparator is clinically credible, whether the outcome is observable, and whether the assumptions behind the analysis are defensible.


In practice, that means agents can draft the cohort definition, generate candidate code lists, map eligibility criteria to the available data model, run data-quality checks, and show exactly which patients enter or leave the cohort as definitions change. The epidemiologist is not hand-writing every query from scratch. They are reviewing the phenotype, approving or rejecting the logic, and asking better follow-up questions.


The same pattern applies to causal inference. Frekil can turn a research question into a proposed target-trial-style specification: eligibility, treatment strategies, time zero, follow-up, outcomes, causal contrast, estimand, and sensitivity analyses. Target trial emulation is useful because it forces observational studies to specify the trial they are trying to emulate before the analysis begins.[JAMA, Target Trial Emulation](https://jamanetwork.com/journals/jama/fullarticle/2799678)


Frekil can also help draft causal DAGs for review. That matters because DAGs make the team's assumptions explicit: which variables are confounders, which are mediators, which are colliders, and which should or should not be adjusted for. Causal diagrams have been used in epidemiology to identify variables that must be measured and controlled to estimate effects without confounding.[Greenland, Pearl, and Robins](https://journals.lww.com/epidem/Abstract/1999/01000/Causal_Diagrams_for_Epidemiologic_Research.8.aspx)


Once those assumptions are approved, agents can generate the analysis code, execute it in a controlled environment, produce the tables and figures, and preserve lineage from the final estimate back to the cohort definition, code lists, data checks, and protocol choices. If the team wants to test a new hypothesis, they do not restart a six-month process. They edit the question, review the changed assumptions, re-run the evidence pipeline, and compare the result.


When that infrastructure exists, the operating model changes. Instead of asking how many bespoke studies the team can afford this year, organizations can ask which decisions need evidence now: a safety signal, an access question, a label-expansion hypothesis, a trial feasibility readout, a comparator strategy, a subgroup where the pivotal trial was thin. The bottleneck moves from data plumbing back to scientific prioritization. That is how RWE can shrink from months to minutes for repeatable analyses on connected, analysis-ready data: not by skipping rigor, but by making rigor executable.


That is the real cost of slow RWE. It is not only budget. It is the number of questions an organization never asks because the workflow makes each question too expensive to test. The data already exists. The next generation of RWE infrastructure has to make the evidence arrive while the decision still matters.
