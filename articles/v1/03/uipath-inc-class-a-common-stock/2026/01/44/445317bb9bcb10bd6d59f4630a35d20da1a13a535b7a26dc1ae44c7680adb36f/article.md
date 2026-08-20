---
schema_version: "1.0.0"
document_id: "445317bb9bcb10bd6d59f4630a35d20da1a13a535b7a26dc1ae44c7680adb36f"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/enterprise-case-classification-agent-support-ticket-routing-with-ai-de5c822d8153"
published_at: "2026-01-12T10:34:41+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T22:23:44.568277+00:00"
content_hash: "sha256:51fea5ff5bf25f9ee9eef1a9bbc5abf7a62d83e963b1abec725bc1f29260d9b3"
---

# Enterprise Case Classification Agent: Support Ticket Routing with AI

Rags


Llm Agent


Uipath


Customer Support


# Enterprise Case Classification Agent: Support Ticket Routing with AI


[Aayush Pratap Singh](https://medium.com/@singhpratapaayush07?source=post_page---byline--de5c822d8153---------------------------------------)


5 min read


·


Jan 12, 2026


--


## The problem: Misrouted support tickets


In enterprise support systems, metadata fields such as Product, Deployment Type, and Issue Type are critical for accurate ticket routing. However, selecting the correct values for these fields can be non-intuitive—especially when the issue spans multiple components or the available options lack clarity. This often results in incomplete or inaccurate metadata, causing ticket misrouting, delayed resolutions, and increased coordination overhead.


Our analysis showed that the probability of all three fields being correctly populated at the time of ticket creation was below 50%, underscoring the need for a more reliable solution.


To address this, we developed the Case Classification Agent, an AI-powered assistant that infers the correct values directly from the issue description. This enables accurate ticket routing from the outset, reducing manual intervention and improving resolution timelines.


## Real-world context: The support form


Below is the actual UI where users initiate a support request. The Case Classification Agent is integrated into this flow to enhance metadata accuracy from the very first step.


Press enter or click to view image in full size


*Figure 1: The “Describe Issue” step in the support ticket form. The AI assistant analyzes this input to infer metadata fields.*


## Objective: Metadata Inference


Integrate a generative AI assistant into the support case creation workflow to predict key metadata fields: Deployment Type, Product, and Issue Type.


**🛠️ Scope and Architecture**


- Input: Freeform issue description
- Output: Predicted deployment type, product, issue type, plus refined subject and description
- Data Sources: Solved SFDC tickets and official product documentation ([docs.uipath.com](https://docs.uipath.com/) )


## Sequence Flow Overview


Press enter or click to view image in full size


*Figure 2: Sequence diagram showing how the Case Classification Agent processes user input through ECS, SFDC Index, and LLM to generate predictions.*


## Iterative Development: From baseline to breakthrough


We followed a rigorous, experiment-driven approach to refine our solution.
We track combined accuracy using a` Product × DeploymentType` match as the baseline metric to optimize. IssueType, being more dynamic, is determined at runtime via the LLM rather than relying on historical ticket patterns.


Here’s how each iteration informed the next:


## 📘 Phase 1: Dual Index Retrieval (SFDC + Docs)


**Approach** : Use ECS Index to retrieve top documents from both SFDC and Docs Index. If the top document was from SFDC, extract the fields directly. If from Docs, use LLM to infer missing fields.


**Result** :


Press enter or click to view image in full size


Among them segregated accuracy in Index-wise:


Press enter or click to view image in full size


**Conclusion** : Docs Index underperformed significantly in terms of accuracy.


## 📙 Phase 2: SFDC-Only Index


**Change** : Dropped the Docs index due to poor accuracy performance and switched to using only the SFDC index. Now, all results are served exclusively by the SFDC index, which previously competed with and was partially served by the Docs index.


**Result** :


Press enter or click to view image in full size


## 📒 Phase 3: Focused Index (Subject + Description Only)


**Change** : Created a new SFDC index containing only the text fields: subject, description, and case number — to improve ECS focus. The previous index included ‌fields that were meant to be predicted.
Now, predicted fields are excluded from the index and will be fetched separately via an HTTP API call to SFDC.


**Result** :


Press enter or click to view image in full size


**Conclusion** : Accuracy declined. Removing context reduced the ECS’ effectiveness.


## 📗 Phase 4: BERT-Based Classification


**Change** : Classification Using ML Models — Supervised Learning


We implemented a supervised learning approach using Bidirectional Encoder Representations from Transformers ( **BERT** ), a deep neural network language model developed by Google in 2018.


Unlike traditional models that memorize keywords, BERT understands the **context** of words by converting each word into a dense vector (e.g., 768-dimensional), where the meaning adapts based on surrounding words.


## Get Aayush Pratap Singh’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


For example:


- “bank” in **“river bank”** vs. **“bank loan”** → different contextual vectors.


**Process Followed:**


- Trained the BERT model on 40,000 Salesforce tickets
- Exported the trained model (BERT training was done in Python. Our backend is in Node.js)
- Created a prediction script that takes user input (description) and returns score predictions for each field
- The system picks the top-scoring values and returns the best-matched **Product** and **Deployment Type**


**Result** :


Press enter or click to view image in full size


**Conclusion** : BERT model improved Product prediction but struggled with Deployment Type.


## 📓 Phase 5 (Final): RAG + Ontology Guardrails


**Change** : Integrated **ECS-based retrieval** with an **LLM-powered classification layer** using a retrieval augmented generation **(**[RAG](https://www.uipath.com/blog/product-and-updates/introducing-uipath-deeprag) **)** approach to improve both precision and reliability of the output.


### Key Enhancements Introduced:


- **Ontology-Based Prompt Engineering:**
Introduced structured ontology guidance by providing a predefined mapping of` Product × DeploymentType × IssueType` . This acts as a **guardrail** to constrain the LLM’s outputs to only valid and known combinations, reducing hallucinations and improving consistency.
- **Threshold Filtering:**
Implemented confidence score filtering based on ECS outputs. The LLM is invoked only when the ECS confidence is below a certain threshold, optimizing both accuracy and compute efficiency.
- **Issue type definitions from PSEs** :
Used curated IssueType definitions sourced from Product Support Engineers (PSEs) to guide the LLM’s interpretation and classification logic. These definitions help the model better understand how to phrase and differentiate IssueTypes in a domain-specific context.


**Result** :


Press enter or click to view image in full size


**Conclusion** : This final approach delivered robust, production-ready performance.


## 📊 Final Accuracy Snapshot


Press enter or click to view image in full size


## Key Learnings


- **Semantic search systems** can significantly enhance classification accuracy when paired with well-structured, high-quality historical data.
- **Knowledge sources vary in effectiveness** — domain-specific data typically outperforms general documentation for precise classification.
- **Supervised models like BERT** complement retrieval-based methods but require careful tuning and domain adaptation.
- **RAG** combined with **structured system** prompts **** delivers the best balance of precision, interpretability, and flexibility.


## UI integration and Feedback Loop


The Case Classification Agent is embedded directly into the support workflow. After analyzing the user’s input, it suggests metadata fields with high confidence, which users can review and edit.


Press enter or click to view image in full size


*Figure 3: The “Analyse Issue” step shows AI-suggested values for Deployment Type, Product, and Issue Type, which users can confirm or modify.*


### Feedback Loop


- Suggested fields are shown with confidence scores and are editable by users
- Overrides and feedback are logged for continuous improvement
- Metrics like **First Response Time (FRT)** and **acceptance rate** are tracked


## 📖 Appendix: Key Terms


- **SFDC (Salesforce):** customer relationship management platform that stores historical support tickets.
- **ECS:** UiPath context grounding and semantic retrieval system.
- **Retrieval Augmented Generation (RAG):** AI technique that combines document retrieval with language model reasoning.
- **Ontology Guardrails:** structured constraints ensuring AI outputs align with valid, predefined categories.
- **Product Support Engineer (PSE)** : domain experts who define issue-type mappings and validation logic.
