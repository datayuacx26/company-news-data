---
schema_version: "1.0.0"
document_id: "af4a5d7afd455503906c8a99e266a7ea9d0acc67c8e97e5863c38121d6478f6b"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/housing-vlm-en"
published_at: null
first_seen_at: "2026-08-10T18:45:25.452283+00:00"
fetched_at: "2026-08-10T18:45:26.126556+00:00"
content_hash: "sha256:ca242a82fab786cf6ad032d64b0e5e96342e96ed9d15e35792dac09a60cf40b4"
---

# [Customer Success Story] How Korea Land & Housing Corporation Uses VLMs to Classify 18,000+ Housing Defect Combinations from a Single Image

Case Study


\[Customer Success Story\] How Korea Land & Housing Corporation Uses VLMs to Classify 18,000+ Housing Defect Combinations from a Single Image


Hyun Kim


Co-Founder & CEO | 2026/08/10 | 7 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


The era of handling housing defect reports through text forms and phone consultations is coming to an end.


This customer success story highlights how Korea Land & Housing Corporation (LH), Korea’s largest public housing provider and manager, worked with Superb AI to deploy a Vision-Language Model (VLM)-based automated defect classification system. The system handles a complex taxonomy of more than 18,000 combinations from a single image and enables 24/7 real-time defect intake. Here is how LH automated classification across five dimensions: space, component and material, defect type, construction type, and work type.


The final checkpoint that determines the quality of residential services is how defects are handled. According to Korea’s Ministry of Land, Infrastructure and Transport, the Defect Review and Dispute Mediation Committee has processed[an annual average of roughly 4,600 defect dispute cases over the past five years](https://www.kairnews.com/news/477362) , reaching 4,761 cases in 2025. The most common defect types—functional failures, lifting and detachment, cracks, condensation, and water leakage—show that defects are a core resident concern directly tied to everyday life at home.


One technology drawing attention as a way to address this challenge is the Vision-Language Model (VLM), which understands images and language at the same time. According to market research firm[Precedence Research](https://www.precedenceresearch.com/vision-language-models-market) , the global VLM market is projected to grow from approximately USD 3.74 billion in 2025 to roughly USD 35.96 billion by 2035, representing a compound annual growth rate of 25.41%. A VLM’s ability to interpret a situation from a single photo and a short complaint text is especially well suited to residential service environments, where large volumes of unstructured resident data are generated.


To move beyond the limits of a text- and phone-based consultation process handling hundreds of thousands of defect reports each month, LH partnered with Superb AI on a VLM-based automated defect classification project.


# **Challenge: 300,000 Images per Month, 18,000 Combinations, and Uneven Experience Across Agents**


The challenge LH faced was not simply a matter of workload. It was a structural problem in which the complexity of the classification taxonomy, the workforce, and the nature of the data were all intertwined.


## **1. Uneven defect interpretation across agents with different levels of experience**


Defect classification requires experience.


Even for the same defect, an agent’s ability to understand and classify the issue could vary depending on their years of experience. As a result, maintaining consistent classification standards across the organization was not easy.


## **2. Four million unstructured images per year**


The defect photos and text submitted by residents are unstructured data. Shooting angles, lighting conditions, and ways of describing the issue all vary widely.


Manually reviewing 300,000 images per month, or approximately four million images per year, to assign the correct maintenance category required a significant amount of time. Translating everyday resident expressions such as “there’s a draft coming through the window frame” into precise technical terminology also fell to the agents.


## **3. A long-tail taxonomy with more than 18,000 combinations**


LH’s defect classification system is built around five dimensions: space, component and material, defect type, construction trade, and work type. Together, these dimensions create more than 18,000 possible combinations.


The difficulty is that most of these combinations occur at very low frequency, forming a long-tail distribution. Conventional approaches trained primarily on frequently occurring categories struggle to classify rare combinations accurately.


## **4. Operational costs caused by misclassification**


When a defect is classified incorrectly, a repair contractor that is not suited to the actual issue may be assigned. As the first-visit resolution rate decreases, operational costs increase.


The existing intake and classification process was caught in a three-way bind: uneven agent experience, unstructured data, and a complex classification taxonomy.


# **Solution: An End-to-End Classification Pipeline Built on a Single VLM, Without a Detection Model**


The core of Superb AI’s proposed solution was simplicity.


Instead of stacking object detection and classification models in layers, Superb AI built the entire pipeline around a single **VLM-based automated defect classification system** that understands images and language together.


## **1. Five-dimensional classification from a single image**


When a resident submits a defect photo, the VLM classifies the space, component and material, defect type, construction trade, and work type simultaneously from that single image.


Because the process is handled by one model without a separate detection step, the pipeline stays simple and the maintenance burden is reduced. The system operates in real time, 24 hours a day, returning classification results immediately after submission, regardless of agent working hours.


## **2. Conditional inference and natural-language explanations**


The system precisely detects key defect types such as cracks, water leakage, damage, mold, and staining. For defects that are difficult to determine from a still image alone, such as the cause of a leak, it reasons probabilistically based on learned patterns.


Going a step further, the VLM explains each defect situation in natural language. This helps agents immediately understand the basis for the classification result and provide clearer guidance to residents.


## **3. An operating framework with explicit performance targets**


The system was designed with clear performance targets: detection accuracy of 90% or higher, false positives and false negatives below 10%, and single-image processing time of less than one second. It was also built to maintain these targets through periodic performance monitoring and validation. This is not a “deploy and done” arrangement, but an operating model for continuous quality management.


This diagram shows the end-to-end pipeline in which submitted defect images pass through the VLM and are converted into five-dimensional classification results and natural-language explanations.


# **Benefit: From Individual Experience to a Standardized System**


By introducing VLMs, LH shifted its defect intake and classification process from a workflow dependent on individual experience to one grounded in a standardized system.


## **Quantitative Results**


- **Automated classification at the scale of four million images per year:** AI classifies up to 300,000 defect images per month in real time, 24 hours a day, minimizing manual intake and classification work for service agents.
- **Detection accuracy maintained at 90% or higher:** By managing performance against a standard of false positives and false negatives below 10%, LH secured consistent classification quality without variability caused by differences in agent experience.
- **Single-image processing in less than one second:** With classification completed immediately after submission, the system fundamentally improved the speed of defect handling.


## **Conventional Process vs. VLM-Based System**


## **Qualitative Impact**


- **Fewer delays caused by misclassification:** By accurately identifying the defect type and affected component or material at the intake stage, AI reduces the likelihood that an incorrect initial classification will delay repair work.
- **Reduced workload fatigue for service agents:** AI absorbs simple, repetitive intake and classification tasks, allowing agents to focus on resident complaints that require human judgment.
- **Turning data into an asset:** As every defect record accumulates in a structured format, LH now has a database that can support rigorous analysis of defect patterns by housing complex, material, and years since construction.


# **Expansion Potential: From Defect Classification to Defect Prevention**


The true value of this project lies not only in intake automation, but in the data generated through the process.


## **Proactive preventive maintenance**


LH can now analyze patterns such as which materials repeatedly cause which defects, in which housing complexes, and after how many years since construction. This creates the foundation for shifting from reactive handling after a defect is reported to proactive preventive maintenance that addresses recurring problem areas in advance.


## **Data-driven long-term repair planning**


Structured defect history data enables long-term repair planning based on evidence rather than intuition. This can support better prioritization of repair budgets and optimization of asset management.


## **Application across public residential services**


The VLM approach, which understands a situation from a single image and complaint text, is not limited to defect classification. The same structure can be extended across public service areas where large volumes of unstructured image-based complaints are generated, including facility inspections and complaint image analysis.


# **Setting a New Standard for AI in Public Services**


Four million defect images per year. More than 18,000 classification combinations. This is a problem too large to manage through human expertise alone. LH addressed it with a single-model VLM approach.


This case offers a practical path forward for public institutions and large-scale facility management organizations that have hesitated to adopt AI because of complex taxonomies and unstructured complaint data.


Superb AI supports the full journey of AI adoption for real-world operations, from building datasets to running models and validating performance.


If, like LH, you are exploring VLM-based automation tailored to your organization’s classification taxonomy and workflows, leave your information below. A Superb AI specialist will contact you shortly.


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Case Study \[Customer Success Story\] The Future of Precision Diet Management, Powered by Domain-Aware Vision AI Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/ai-food-recognition-en)[Case Study \[Customer Success Story\] Building an AI-Powered Monitoring System to Prevent EV Fires Hyun Kim Co-Founder & CEO | 5 min read](https://superb-ai.com/en/resources/blog/ev-fire-monitoring-ai-en)[Case Study \[Customer Success Story\] Developing a Smartphone-Based Cavity Diagnosis AI Model in Just Three Weeks Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/aju-univ-hospital-medical-ai-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
