---
schema_version: "1.0.0"
document_id: "045363608eeab0996c16c01a9ad42d35b6557575f0603d3dc3da4ce2feb89de7"
company_key: "mastech-digital-inc-common-stock"
company: "Mastech Digital Inc"
source_id: "mastech-digital-inc-common-stock-rss-2af4b8fac33f"
canonical_url: "https://www.mastechdigital.com/blogs/shifting-snowflake-cortex-analyst-production-accuracy"
published_at: "2026-06-02T11:51:02+00:00"
first_seen_at: "2026-07-20T23:19:47.752827+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:e38d7e9ee8122d5dfe358d9905d4351ba678bb3723a037a57d7c95c2eb325c05"
---

# Shifting Snowflake Cortex Analyst to Production Accuracy

## Problem we faced


We have been using CoCo to build solutions and it provides a lot of skills one of which is to build semantic views. Sematic views are important for tools like Cortex Analyst when building Cortex Agent. We built an agent for[Value Based Care solution](https://www.mastechdigital.com/blogs/architecting-ai-driven-value-based-care-on-snowflake) specifically for Care manager persona and when testing this agent, we found out that the agent was returning recommendations that didn’t align with ground truth for some patients. The recommendation seemed logically correct but missed vital details.


### Analysis and Findings


When we deep-dived into the generated code by CoCo, the underlying reasons for the baseline model's failures became clear.


Crucially, this wasn't an issue of access, both the[CoCo automated assistant](https://www.mastechdigital.com/snowflake-summit-2026) and the Custom Domain pipeline were supplied with the exact same comprehensive set of 10 ecosystem tables in the prompt instructions. Yet, when tasked with constructing the semantic schema, the automated baseline option buckled under the relational complexity.


The baseline view treated data transformation as an afterthought, relying on flat runtime joins over 4 basic tables. This design left major analytical blind spots across the clinical data ecosystem,leaving the LLM entirely unaware of critical attributes for patient records.


-


4-Table Clinical Blind Spot:


The automated baseline view completely ignored four foundational tables: Medications, HEDIS Care Gaps, Lab Diagnostics, and Social Determinants of Health. If a user asked about medication adherence or open care gap lists, the LLM had no reference point and was forced to hallucinate or fail.


-


Unmaterialized Join Tax:


The baseline view relied heavily on complex, unmaterialized logic at runtime. The Custom Domain Skill instead built an engineered[Snowflake](https://www.mastechdigital.com/snowflake-summit-2026) Dynamic Table.


-


Lack of Synonyms:


Users rarely use strict column names in text queries. The baseline view contained zero synonym maps, while the Custom Domain Skill embedded rich vocabulary arrays mapping terms like 'member', 'subscriber', and 'patient' interchangeably, and linking 'doctor' or 'provider' directly to \`ASSIGNED_PHYSICIAN\`.


In healthcare and other[highly regulated domains](https://www.mastechdigital.com/blogs/ai-governance-healthcare-databricks-hipaa-clinical-compliance) , critical information is often distributed across multiple interconnected tables containing medications, care gaps, laboratory diagnostics, social determinants of health, provider assignments, and risk stratification data.


When the semantic layer fails to expose these relationships correctly, the AI is forced to infer answers from incomplete context. Even with stringent constraints and rules in instructions it will never be enough


It creates a concerning scenario where:


- Queries execute successfully.


- SQL appears syntactically correct.


- Dashboards display answers confidently.


- The underlying answer is factually wrong.


The result is a system that looks operationally healthy while silently generating misinformation or missing information.


## The Solution: Building a Custom Domain Skill


Instead of asking the AI to dynamically discover relationships across fragmented healthcare tables at runtime, we moved the complexity into the data engineering layer. The Custom Domain Skill was designed to create a unified Patient 360 semantic view.


Key design principles included:


- Pre-materializing business metrics.


- Flattening complex relational structures.


- Extracting important values from semi-structured data.


- Embedding domain-specific synonyms.


- Adding explicit AI SQL generation guidance.


- Defining categorization boundaries for out-of-scope requests.


The resulting architecture transformed a fragmented healthcare ecosystem into a single AI-ready analytical surface.


**Architectural Feature**


**CoCo Baseline View**


**Domain Skill Dynamic Table (Patient-360)**


Object Typology


Standard Logical View (Unmaterialized)


Dynamic Table (Materialized, Target Lag = 1 Hr)


Underlying Base Tables


4 Tables (Patients, State, Users ×2, Risk Scores)


8 Tables (Consolidated across entire VBC ecosystem)


Medication Metrics


None


Total Count, Active Count, Active List, Prescribed Drug Classes, Avg Adherence %


HEDIS Care Gaps


None


Total Gaps, Open/Closed Counts, Open Gap List, Closure Rate %


Clinical Lab Diagnostics


None


Total Lab Results, Abnormal Lab Count, Last Lab Timestamp


SDOH Integration


None


Raw Profile JSON, Semi-structured extraction of ADI Score and ADI Level


Synonym Mapping


No


Yes (Extensive multi-synonym arrays for dimensions & metrics)


AI Text-to-SQL Guardrails


Absent


Explicitly embedded via ai_sql_generation & categorization rules


## The Setup and Evaluation


###


### Setup


The evaluation consisted of the following:


#### 1. Direct CoCo-Generated View (Baseline)


In the first approach, we directly prompted Cortex CoCo with the available healthcare ecosystem tables and asked it to generate the required analytical assets automatically.


The workflow was intentionally simple:


1. Provide CoCo with the list of source tables.


2. Allow CoCo to analyze relationships and column metadata.


3. Generate a logical analytical view.


4. Generate the corresponding semantic view for Cortex Analyst.


5. Execute natural language healthcare queries against the generated semantic layer.


The goal of this setup was to represent the fastest path an organization might take when onboarding Cortex Analyst by leveraging automation to create a semantic layer directly from raw source tables with minimal engineering effort.


This generated artifact is referred to throughout the evaluation as the **CoCo Baseline View** .


#### 2. Custom Domain Skill (Patient-360 Builder)


In the second approach, instead of asking CoCo to directly generate the final analytical assets, we built a dedicated Custom Domain Skill called **Patient-360 Custom Domain Skill**


\`patient-360-builder-v2\`


The purpose of the skill was to act as a domain-aware data engineering layer before semantic model generation.


Rather than exposing the LLM directly to fragmented operational tables, the Custom Domain Skill creates a curated analytical surface specifically optimized for Text-to-SQL generation and healthcare reasoning.


We curated a evaluation dataset consisting of 30 complex, real-world population health questions ranging from:


- Specific medication counts


- Medication adherence metrics


- HEDIS care gap analysis


- Laboratory result summaries


- Area Deprivation Index (ADI) lookups


###


### Evaluation Metrics


When evaluating the performance of Text-to-SQL systems and Generative AI agents especially in high-stakes environments like healthcare traditional software testing methods fall short. You cannot simply check if the code runs; you must evaluate the **validity of the logic** and the **truth of the output** .


To do this objectively, evaluation frameworks rely on two key primary metric layers: **Answer Correctness (AC)** and **Logical Consistency (LC)** . The benefits include the following:


- It Isolates "Smart" AI from "Bad" Data Architecture


- It Catches "Silent Hallucinations"


- It Measures Production-Readiness


#### Answer Correctness (AC)


Answer Correctness measures whether the final answer delivered to the end-user matches the absolute ground-truth reality of the database. It asks a simple question: *Is the final number or text factually right?*


AC is typically scored on a scale from


0.00


to


1.00


:


- **1.00 (Perfect):** The AI answers "9 medications" when the patient actually has 9 medications.


- **0.00 (Factual Hallucination):** The AI answers "3 medications" when the patient actually has 9 medications (as seen in the Martha Jones scenario).


Why it matters:


In clinical settings, AC is the literal line between operational safety and dangerous misinformation. If an executive or care manager makes a decision based on an AI response, an AC score below 1.00 means the system is actively injecting false information into the workflow.


#### Logical Consistency (LC)


What it is:


Logical Consistency measures the structural integrity of the AI’s reasoning and the generated SQL query itself. It ignores whether the data *inside* the database is correct and focuses entirely on the code's construction. It asks: *Did the AI write valid, syntactically sound SQL that maps cleanly to the user’s intent?*


How it is calculated:


* High LC (~1.00):


The generated SQL has perfect syntax, joins tables on correct primary keys, and correctly uses filters (like


WHERE status = 'active'


).


- Low LC (<0.50): The SQL breaks due to syntax errors, references non-existent columns, or joins completely unrelated tables together blindly.


Why it matters:


LC tells you how stable your AI engine is. A system can have high Logical Consistency but low Answer Correctness if it writes beautiful, flawless SQL code against the *wrong* tables.


Using either metric alone creates a massive evaluation blind spot. Pitting them against each other is the only way to expose structural flaws in a GenAI semantic layer.


For a system to be enterprise-grade, it needs a tight matrix pairing:


- **High LC + Low AC:** Means your data engineering is poor (the AI is guessing because tables are messy or missing).


- **Low LC + High AC:** Means your system is unstable (the AI is getting lucky, but the queries will break tomorrow).


- **High LC + High AC:** The holy grail of production-readiness. The code is structurally deterministic, and the data architecture ensures the output is always true.


###


### Evaluation Results


The performance numbers from the evaluation run highlights a definitive gap in execution quality:


**Evaluation Run Name**


**Total Records**


**Avg Answer Correctness**


**Avg Logical Consistency**


**Total Input Tokens**


**Total LLM Calls**


Eval_CoCo_Generated_Run_1


30 Questions


63.0%


98.0%


1,003,295


108


Eval_Domain_Skill_Generated_Run_1


30 Questions


77.0%


100.0%


926,750


98


Deep-Diving Into Specific Queries:


Under the CoCo baseline evaluation, a medication related query scored a poor 0.33. The model had no visibility into medication data as the baseline view does not have that data


and was forced to generate guesses based on limited peripheral rows. Under the Custom Domain Skill, it scored a perfect 1.00 because the entire list of active medications was pre-compiled directly into the patient-360 table. Similarly, queries looking at average medication adherence percentages or Area Deprivation Index (ADI) levels scored a flat 0.00 under CoCo but hit a flawless 1.00 under the Custom Domain Skill.


Crucially, we noticed a major difference in out-of-scope query behavior. When we tested standard non-clinical prompts (e.g., 'Write me a Python script to scrape Amazon prices'), both systems correctly rejected the requests with a 1.00 score. However, the Custom Domain Skill achieved this instantly at the semantic boundary thanks to its built-in \`ai_question_categorization\` configuration. The baseline view, lacking these guardrails, passed the prompt upstream to the core LLM, which wasted processing steps scanning unrelated tables before giving up. This structural difference explains why the Custom Domain Skill consumed significantly fewer total input tokens (926,750 vs. 1,003,295) and minimized total infrastructure costs.


## Evaluation Summary


Single iteration of Custom Domain Skill delivered measurable improvements:


- Answer Correctness increased from 63.0% to 77.0%.


- Logical Consistency improved from 98.0% to 100.0%.


- Input token consumption decreased by 76,545 tokens.


- Total LLM calls decreased from 108 to 98.


- Critical clinical queries improved from failing scores (0.00–0.33) to perfect scores (1.00).


- Out-of-scope requests were rejected more efficiently.


- Infrastructure costs were reduced without sacrificing correctness.


## Key Takeaway


The standard automation tool (CoCo Baseline) failed because it only looked at 4 basic tables and ignored the complex clinical data. It forced the AI to guess, resulting in poor accuracy scores (like 0.33 or 0.00).


By building a Custom Domain Skill, those hidden clinical tables were pre-cleaned and flattened into a single, unified view. This eliminated the guesswork, gave the AI clear rules to follow, and instantly boosted accuracy scores to perfection (1.00) on critical healthcare questions. The broader lesson extends beyond healthcare.[Enterprise AI](https://www.mastechdigital.com/enterprise-ai) systems do not fail because large language models are incapable.


They fail because semantic context is incomplete. Organizations will suffer from underinvesting in semantic architecture, domain modeling, and data engineering. This evaluation also demonstrates that context engineering is often more impactful along with model engineering to make AI real for business in regulated industries like Healthcare and Life Sciences.
