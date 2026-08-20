---
schema_version: "1.0.0"
document_id: "09f5c7296603c0bd4a6991ae18fb3aeb15cb5b357eb9aa080b576621b28d705f"
company_key: "yc-rhizome-ai"
company: "Rhizome AI"
source_id: "yc-rhizome-ai-news-import-cebdb3b9da2f"
canonical_url: "https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data"
published_at: null
first_seen_at: "2026-08-13T04:11:53.154536+00:00"
fetched_at: "2026-08-13T04:11:56.647839+00:00"
content_hash: "sha256:1206c96c4c22de4db8e353313b8d14abc45d3ba2100f03877309b1960fd1c1d0"
---

# FDA Guidance on AI/ML Use in Clinical Trial Data Analysis

## What FDA has said about using AI/ML in clinical trial data analysis


### The short answer


FDA has not issued a prescriptive rulebook for running machine-learning models on trial data. Instead, in its January 2025 draft guidance it set out a **risk-based credibility assessment framework** : whenever an AI model produces information or data intended to support a regulatory decision about a drug's safety, effectiveness, or quality, the sponsor should establish that the model is credible enough for that specific use, with the rigor scaled to how much the model drives the decision and how bad it would be to get the decision wrong[7](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=7)[31](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=31)[46](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=46) . Analyzing clinical trial data is one of the applications FDA expressly contemplates[1](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=1) .


### The core framework: context of use and a 7-step process


FDA anchors everything to a **context of use (COU)** , the precise description of the AI model's role and scope in addressing a defined **question of interest**[46](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=46)[45](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=45) . Credibility is not a property of a model in the abstract; it is established for a model *within a stated COU*[31](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=31) .


The draft guidance lays out a seven-step credibility assessment process[29](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=29)[30](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=30) :


1. Define the question of interest.
2. Define the context of use.
3. Assess AI model risk.
4. Develop a plan to establish model credibility within the COU.
5. Execute the plan.
6. Document results and discuss any deviations from the plan.
7. Determine whether the model is adequate for the COU.


The plan should describe, as applicable, the model and its development process, the development data and why it is fit for the COU, model training, and the model evaluation process[32](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=32)[44](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=44)[34](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=34) . Results go into a **credibility assessment report** that may be submitted in a regulatory submission, included in a meeting package, or held and made available to FDA on request[30](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=30) . If credibility is not sufficiently established for the model's risk, FDA lists remedies such as downgrading the model's influence with additional evidence, increasing the rigor of the assessment, adding development data, establishing controls, changing the modeling approach, or rejecting or revising the COU[30](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=30) .


### How FDA defines "model risk"


Model risk drives how much evidence FDA expects. FDA derives it from two factors[86](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=86)[87](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=87)[88](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=88) :


- **Model influence** : the contribution or weight of the model's output in the decision relative to other evidence.
- **Decision consequence** : the significance of an adverse outcome if a decision based on the question of interest is wrong, considering both the severity and probability of that outcome.


Combining the two: if both are low, model risk is low; if both are high, model risk is high; where they differ, risk is driven by the more influential factor[86](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=86) . The credibility evaluation, planned activities, and level of documentation should all be **commensurate with model risk**[88](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=88)[92](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=92)[31](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=31)[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3) . In practice, a model whose output is one input among many into a low-stakes exploratory analysis sits at the light end; a model whose output substantially drives an efficacy or safety conclusion sits at the demanding end.


### What FDA says specifically about clinical trial data analysis


FDA's language on the trial-analysis use case is real but high-level. The agency states that AI/ML can process and analyze large data sets from clinical studies and other sources to help **develop clinical trial endpoints and assess outcomes** , and to **integrate data across trials and related sources** to improve understanding of disease features and progression[1](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=1) . The recurring condition is that the data used to develop the model must be **"fit for use"** , meaning relevant and representative of the target population or purpose[1](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=1) .


Across the broader development lifecycle, FDA points to AI/ML uses that touch trial data directly[1](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=1) :


- Integrating data from clinical studies, trials, natural history datasets, genetic databases, social media, and registries to characterize disease presentation, heterogeneity, progression, and subtypes.
- Processing large datasets from real-world data or digital health technologies (DHTs) to develop trial endpoints and assess outcomes.
- Predictive modeling for clinical pharmacokinetics and exposure-response.
- Identifying, evaluating, and processing postmarketing adverse drug experience information (pharmacovigilance).


On the trial-conduct side, FDA's related guidances describe DHT-captured endpoints (for example remote measurement of steps, memory-task performance, glucose levels, and hypoglycemic episodes) and note that DHT-measured endpoints may better capture meaningful changes in clinical function[105](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=105)[106](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=106) . One caveat from the source review: the guidance rows are clearer on endpoint development and DHT data capture than on AI-driven patient enrichment or site selection, which are discussed mainly in terms of broadening representative enrollment rather than as named AI applications[106](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=106) .


### Data quality, bias, and transparency


FDA's most concrete expectations on data quality, bias, and transparency currently live in the **AI-enabled device software** guidance rather than the drug guidance[8](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=8)[55](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=55)[56](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=56) :


- **Representativeness and data drift** : address representativeness in data collection for development, testing, and monitoring throughout the lifecycle, and evaluate performance across intended-use subgroups; AI systems are sensitive to differences in input data, and such drift can degrade performance[55](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=55) .
- **Bias** : FDA describes AI bias as a tendency to produce incorrect results in a systematic, sometimes unforeseeable way, affecting safety and effectiveness across some or all of the intended-use population; bias control includes representative data collection and subgroup performance evaluation across the lifecycle[55](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=55) .
- **Transparency and explainability** : transparency should be designed in from the earliest design stage through decommissioning; explainability tools and visualizations can help, but if poorly designed or unvalidated they can mislead users[55](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=55)[56](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=56) .


For drug submissions, the parallel expectation is that development data be demonstrably fit for the COU, with the model development process documented in the credibility assessment plan[44](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=44)[34](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=34) .


### Lifecycle maintenance and monitoring


FDA treats an AI model as something to maintain, not a one-time deliverable. For drugs and biologics, sponsors should define detailed **lifecycle maintenance plans** : model performance metrics, a risk-based monitoring frequency, and triggers for retesting, with the level of detail commensurate with model risk[110](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=110)[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3) . Model changes, whether inherent drift or intentional modifications, should run through the pharmaceutical quality system and change-management process, with parts of the credibility assessment re-executed (including retraining and retesting) as needed[110](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=110)[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3) . The maintenance plan should be available for review within the manufacturing site's quality system, with a summary in the marketing application for product- or process-specific models[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3) . On the device side, FDA encourages a **predetermined change control plan (PCCP)** to pre-specify and obtain authorization for intended model modifications, addressed in dedicated guidance[9](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=9)[55](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=55) .


### Engaging with FDA


FDA strongly encourages early engagement to align on the question of interest, COU, model risk, and planned credibility activities[32](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=32)[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3) . The mechanism depends on stage and intended use[59](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=59)[60](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=60)[61](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=61)[62](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=62)[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3) :


- **INTERACT meetings** for early development, generally before an IND or pre-IND meeting, for novel programs with unique early-development challenges.
- **Pre-IND meetings** for feedback on the development program before IND submission, including nonclinical work, initial clinical design, and CMC controls.
- **Center for Clinical Trial Innovation (C3TI)** for discussing AI in clinical trial designs before formal IND submission.
- **Complex Innovative Trial Design (CID) meeting program** for AI in novel trial designs.
- **Drug development tool / ISTAND-related engagement** for qualifying an AI-based drug development tool.


### Scope: what the drug guidance does and does not cover


The draft guidance applies to AI models used to produce information or data supporting regulatory decisions about a drug's safety, effectiveness, or quality across the nonclinical, clinical, postmarketing, and manufacturing phases[31](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=31)[46](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=46) . It explicitly does **not** address[31](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=31)[46](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=46) :


- Use of AI in **drug discovery** (excluded from the "drug product life cycle" for this guidance).
- Use of AI for **operational efficiencies** (internal workflows, resource allocation, or drafting a submission) where those uses do not affect patient safety, drug quality, or the reliability of nonclinical or clinical study results.


AI/ML embedded in medical devices is handled under the separate device software guidances[8](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=8)[9](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=9) , and computational-model credibility concepts also appear in the medical device modeling and simulation guidance[11](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=11) and the model-informed drug development guidance M15[10](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=10) .


### Practical takeaways for RA teams


- Treat each analytical use of AI on trial data as a distinct COU and write the question of interest and COU down before building the credibility argument[46](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=46)[45](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=45) .
- Grade your evidence to model risk, and be explicit about model influence and decision consequence, since that is the lever FDA uses to set expectations[86](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=86)[88](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=88) .
- Show your data are fit for use and representative, and document the development and evaluation process in a credibility assessment report[1](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=1)[44](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=44)[30](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=30) .
- Plan for the model's whole life: monitoring metrics, retesting triggers, and change control[110](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=110)[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3) .
- Engage early through the meeting pathway that fits your stage; the framework rewards alignment before the analysis is locked[3](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=3)[59](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=59) .


Because the central drug guidance remains a draft, expect refinement after the comment period. Deeper follow-ups worth asking Rhizome directly: how the framework maps onto a specific model type (for example an ML-based endpoint adjudication tool), how the device PCCP mechanics work for adaptive algorithms[9](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=9) , and how M15 model-informed drug development principles interact with the AI credibility framework[10](https://rhizomeai.com/articles/fda-guidance-on-aiml-use-in-clinical-trial-data?showSourceNumber=10) .
