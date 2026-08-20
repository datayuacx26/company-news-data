---
schema_version: "1.0.0"
document_id: "a31a9449970148ad5eb393448c10fad3e51bd0edd87ace75487067301814de3d"
company_key: "yc-ritivel"
company: "Ritivel"
source_id: "yc-ritivel-news-import-f2b2dd915304"
canonical_url: "https://www.ritivel.com/blog/ema-fda-guiding-principles-ai-drug-development"
published_at: "2026-01-14T00:00:00+00:00"
first_seen_at: "2026-07-23T23:10:20.911041+00:00"
fetched_at: "2026-07-28T22:23:40.402133+00:00"
content_hash: "sha256:e9afa83763e89f013c69dc343efab9e8c96c923d73b388425b85a5902b184b3f"
---

# The EMA-FDA Guiding Principles for AI in Drug Development: A Reference Guide

In January 2026, the **European Medicines Agency (EMA)** and the **U.S. Food and Drug Administration (FDA)** jointly published[“Guiding Principles of Good AI Practice in Drug Development.”](https://www.fda.gov/about-fda/artificial-intelligence-drug-development/guiding-principles-good-ai-practice-drug-development)


This joint publication represents the first formal alignment between US and EU regulators on expectations for AI in drug development. The 10 principles establish a shared vocabulary and framework for how AI systems should be built, validated, and maintained across the product lifecycle.


Below, we examine each principle, define the technical terms, and discuss the practical implications. A glossary of key terms is included at the end.


01


Human-Centric by Design


Ethics


02


Risk-Based Approach


Risk


03


Adherence to Standards


Compliance


04


Clear Context of Use


Scope


05


Multidisciplinary Expertise


Governance


06


Data Governance


Data


07


Model Design Practices


Development


08


Performance Assessment


Validation


09


Life Cycle Management


Operations


10


Clear Information


Transparency


## The 10 Guiding Principles


01


### Human-Centric by Design


“The development and use of AI technologies align with ethical and human-centric values.”


This principle establishes the foundational philosophy: AI in drug development exists to support human decision-making, not replace it. The emphasis is on augmentation rather than automation of judgment.


💡


**In practice:** Expect regulatory reviewers to ask how human oversight is maintained in AI-assisted workflows. Systems where humans cannot review, override, or understand AI outputs will face scrutiny.


02


### Risk-Based Approach


“The development and use of AI technologies follow a risk-based approach with proportionate validation, risk mitigation, and oversight based on the context of use and determined model risk.”


Not all AI applications carry the same risk. An AI tool that suggests document formatting has different implications than one that informs dosing decisions. This principle requires validation rigor to scale with potential impact.


💡


**Note:** The principle doesn't define specific risk tiers. Organizations will need to develop their own risk classification frameworks, likely drawing on existing approaches like ICH Q9 for quality risk management.


03


### Adherence to Standards


“AI technologies adhere to relevant legal, ethical, technical, scientific, cybersecurity, and regulatory standards, including Good Practices (GxP).”


This principle anchors AI within the existing pharmaceutical quality framework. **GxP** refers to the family of “Good Practice” quality guidelines:


- **GMP** (Good Manufacturing Practice) — manufacturing quality
- **GLP** (Good Laboratory Practice) — laboratory study quality
- **GCP** (Good Clinical Practice) — clinical trial conduct
- **GDP** (Good Distribution Practice) — distribution and storage


AI systems used in regulated activities must meet the same quality standards as any other tool or process in drug development.


04


### Clear Context of Use


“AI technologies have a well-defined context of use (role and scope for why it is being used).”


**Context of use** is a regulatory concept that defines:


- What specific problem the AI is solving
- What data it will be applied to
- What decisions it will inform
- Who the intended users are
- What the boundaries and limitations are


💡


**Note:** Validation is tied to context. A model validated for summarizing clinical trial data cannot be assumed valid for generating safety narratives without separate validation for that use case.


05


### Multidisciplinary Expertise


“Multidisciplinary expertise covering both the AI technology and its context of use are integrated throughout the technology's life cycle.”


Building AI for drug development requires more than data scientists. This principle mandates collaboration across:


- **Data science / ML engineering** — model development
- **Domain experts** — regulatory affairs, clinical research, pharmacology
- **Quality assurance** — validation and compliance
- **Ethics / legal** — responsible AI considerations
- **End users** — the people who will actually use the system


No single discipline has all the knowledge needed to build trustworthy AI for regulated environments.


06


### Data Governance and Documentation


“Data source provenance, processing steps, and analytical decisions are documented in a detailed, traceable, and verifiable manner, in line with GxP requirements.”


This principle introduces several key concepts:


- **Data provenance** — the documented trail of where data came from and how it has been transformed
- **Traceability** — the ability to trace any output back to its source data and processing steps
- **Data governance** — policies and processes for managing data quality, security, and privacy


💡


**For regulatory submissions:** If an AI system generates or contributes to content in a regulatory document, auditors and reviewers will likely ask how outputs can be traced to source data. The ability to answer this question is a practical requirement.


07


### Model Design and Development Practices


“The development of AI technologies follows best practices in model and system design and software engineering and leverages data that is fit-for-use, considering interpretability, explainability, and predictive performance.”


This principle packs in several important concepts:


- **Fit-for-use data** — data that is appropriate for the intended purpose, considering accuracy, completeness, and relevance
- **Interpretability** — how understandable the model's internal logic is (some models like decision trees are inherently interpretable)
- **Explainability** — the ability to explain how a model arrived at a specific output (can be applied even to “black box” models)
- **Robustness** — resistance to errors, noise, and edge cases
- **Generalisability** — performance on new data not seen during training


The goals are clear: transparency, reliability, and performance that supports patient safety.


08


### Risk-Based Performance Assessment


“Risk-based performance assessments evaluate the complete system including human-AI interactions, using fit-for-use data and metrics appropriate for the intended context of use.”


Note the emphasis on **human-AI interactions** . Validation isn't just about testing the model in isolation—it's about testing how the complete system performs when humans are using it.


💡


**Consider:** A model that performs well in technical benchmarks may still fail in practice if users misinterpret outputs or if the interface design leads to errors. User testing is part of validation.


09


### Life Cycle Management


“Risk-based quality management systems are implemented throughout the AI technologies' life cycles, including to support capturing, assessing, and addressing issues. The AI technologies undergo scheduled monitoring and periodic re-evaluation to ensure adequate performance (e.g., to address data drift).”


AI systems are not “set and forget.” This principle mandates ongoing:


- **Monitoring** — continuous tracking of system performance
- **Re-evaluation** — periodic validation that the system still works
- **Issue management** — processes for identifying and addressing problems


The principle specifically calls out **data drift** —the phenomenon where model performance degrades over time because the data it encounters in production differs from training data. This is a common challenge in real-world AI deployment.


10


### Clear, Essential Information


“Plain language is used to present clear, accessible, and contextually relevant information to the intended audience, including users and patients, regarding the AI technology's context of use, performance, limitations, underlying data, updates, and interpretability or explainability.”


Transparency is the theme. Stakeholders should be able to understand:


- What the AI does and doesn't do
- How well it performs (with relevant metrics)
- What its limitations are
- What data it was trained on
- How to interpret its outputs


💡


**Note on “plain language”:** The intended audience varies. Documentation for regulators can be technical; information for patients must be accessible. The principle is about appropriateness, not simplification.


## Practical Implications


The principles don't prescribe specific technical solutions. Instead, they establish expectations that organizations will need to interpret based on their specific context of use. A few observations:


**Documentation requirements will increase.** Principles 6, 7, and 10 emphasize traceability and transparency. Organizations using AI in submissions should expect to provide detailed documentation of data sources, model development, validation approaches, and ongoing monitoring.


**The “context of use” concept is central.** Validation requirements are tied to intended use and risk level. A model used for exploratory analysis has different requirements than one informing dosing decisions.


**Lifecycle management is not optional.** Principle 9 makes clear that deploying an AI system is the beginning, not the end. Ongoing monitoring for issues like data drift is expected.


For regulatory affairs teams evaluating AI tools, these principles provide useful criteria. Key questions to ask vendors:


- How is data provenance tracked and documented?
- What validation has been performed, and for what context of use?
- How are outputs traced back to source data?
- What monitoring is in place for model performance over time?
- How is human oversight maintained in the workflow?


## Glossary of Key Terms


Artificial Intelligence (AI)A machine-based system that can, for a given set of human-defined objectives, make predictions, recommendations, or decisions influencing real or virtual environments. Source:[FDA CDER](https://www.fda.gov/about-fda/center-drug-evaluation-and-research-cder/artificial-intelligence-drug-development)


Machine Learning (ML)A subset of AI that involves training algorithms to improve performance at a task based on data, enabling systems to learn from experience without being explicitly programmed. Source:[FDA CDER](https://www.fda.gov/about-fda/center-drug-evaluation-and-research-cder/artificial-intelligence-drug-development)


GxP (Good Practice)A collection of quality guidelines and regulations including Good Manufacturing Practice (GMP), Good Laboratory Practice (GLP), Good Clinical Practice (GCP), and Good Distribution Practice (GDP). These ensure products are consistently produced and controlled according to quality standards. Source: FDA/EMA


Good Clinical Practice (GCP)An international ethical and scientific quality standard for designing, conducting, recording, and reporting trials that involve human subjects. Compliance provides public assurance that the rights, safety, and well-being of trial subjects are protected. Source:[ICH E6(R2)](https://www.ich.org/page/efficacy-guidelines)


Good Manufacturing Practice (GMP)Regulations that require manufacturers to take proactive steps to ensure their products are safe, pure, and effective. GMP regulations require a quality approach to manufacturing. Source: FDA 21 CFR Parts 210/211


Data ProvenanceThe documented trail that accounts for the origin of data, where it has moved, and how it has been altered. Provenance provides a historical record of the data and its origins. Source: FDA/EMA Guidelines


Data DriftA change in the statistical properties of input data over time that can cause model performance to degrade. Also called "covariate shift," it occurs when the data the model encounters in production differs from the training data. Source: ML Engineering Best Practices


Model GeneralisabilityThe ability of a machine learning model to perform well on new, unseen data that was not part of the training set. A generalisable model captures underlying patterns rather than memorizing specific examples. Source: ML Engineering Best Practices


Model RobustnessThe property of a model to maintain its performance when faced with perturbations in the input data, including noise, missing values, or adversarial examples. Robust models are less sensitive to small changes in input. Source: ML Engineering Best Practices


InterpretabilityThe degree to which a human can understand the cause of a decision made by an AI system. Interpretable models (like linear regression or decision trees) are inherently understandable. Source: DARPA XAI Program


ExplainabilityThe ability to explain, in human-understandable terms, how an AI system arrived at a particular output. Unlike interpretability, explainability can be applied post-hoc to complex "black box" models. Source: DARPA XAI Program


Context of UseThe specific setting in which an AI tool will be deployed, including the intended purpose, the population or data it will be applied to, the decisions it will inform, and the regulatory framework it operates within. Source: FDA/EMA Guidelines


Fit-for-Use DataData that is appropriate and adequate for its intended purpose. This includes considerations of accuracy, completeness, consistency, timeliness, and relevance to the specific application. Source: FDA/EMA Guidelines


Life Cycle ManagementThe ongoing process of monitoring, maintaining, updating, and eventually retiring an AI system throughout its operational life. Includes scheduled re-validation, performance monitoring, and adaptation to changing requirements. Source: FDA/EMA Guidelines


Model ValidationThe process of assessing whether an AI model's outputs are accurate, reliable, and appropriate for its intended use. Validation demonstrates that the model performs well on data it hasn't seen during training. Source: FDA/EMA Guidelines


Human-AI InteractionThe interface and relationship between human users and AI systems, including how humans provide input, interpret outputs, and maintain oversight. Good human-AI interaction design ensures AI augments rather than replaces human judgment. Source: FDA/EMA Guidelines


---


#### 📚 Reference


European Medicines Agency & U.S. Food and Drug Administration. “Guiding Principles of Good AI Practice in Drug Development.” *Joint EMA-FDA Publication* (2026).


[Read the full paper →](https://www.fda.gov/about-fda/artificial-intelligence-drug-development/guiding-principles-good-ai-practice-drug-development)


Tags


AI


FDA


EMA


Guidelines


GxP


Drug Development


Compliance


Written by


### Pavan Kalyan


Founder & CEO


Building AI-native regulatory automation at Ritivel. Passionate about accelerating life-saving therapies through technology.
