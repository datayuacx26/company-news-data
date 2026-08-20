---
schema_version: "1.0.0"
document_id: "23adb9f3390835d1369578361406c2deb307b817272650f74facb7c367e85980"
company_key: "yc-datasaur"
company: "Datasaur"
source_id: "yc-datasaur-news-import-f7082d4871d2"
canonical_url: "https://datasaur.ai/blog-posts/we-tested-4-top-llms-for-labeling-one-suprised-us"
published_at: "2025-07-15T00:00:00+00:00"
first_seen_at: "2026-07-25T01:27:49.393704+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:b9de157d2b49f47f618124909ea1e23ba27dc38911b9e36b1a6d4c4d0c8ad6d9"
---

# We Tested 4 Top LLMs for Labeling. One Surprised Us.

## **Introduction**


We tested four of the most popular LLMs on an entity labeling task to see which performs best out-of-the-box. This benchmark used Datasaur’s Labeling Agent feature to apply each model in zero-shot conditions. Our goal was to discover:


Who is the best labeler?


GPT-4o, Claude 3.7 Sonnet, Gemini 2.5 Pro, or Llama 3.3 70B


---


## **Method**


We used 500 lines of text from the public[WNUT-17 dataset](https://huggingface.co/datasets/leondz/wnut_17) , which contains user-generated content such as tweets. These are messy, short-form sentences, which makes them a good challenge for real-world entity recognition. The dataset is full of lingo and cultural references that challenge labelers with ambiguity.


The task involved labeling 6 entity types within the document: people, organizations, locations, products and creative works. We used a zero-shot prompt — no examples were provided — asking the model to extract entities from a given text into a structured JSON format.


---


## **What is the Labeling Agent?**


[Labeling Agent](https://docs.datasaur.ai/agent/labeling-agent-beta) enables teams to assign multiple LLMs as labelers to automatically label a project. It provides structured prompts and consistent formatting, and tracks metrics like accuracy, conflict rate, and time-to-label.


### ‍ **Use Your Own Model with Custom Logic**


You decide how the model should behave by writing a prompt. That prompt can follow specific labeling instructions, reflect project guidelines, or even mirror how your team thinks about annotation. You can even contextualize the model with a golden set for reference.


Once it’s set up, you can call the model from Data Studio whenever needed. It labels all unannotated data in the document, giving you results consistent with your prompt and ready for review.


### **Models Substituting Labelers**


The Labeling Agent treats your model(s) as a project contributor. That means:


- You assign it directly to a project.
- It labels within the app interface.
- Its contributions are visible, trackable, and reviewable.


There’s no need to log in as the model or manage it separately. It works alongside your team, and its output is handled just like human-generated labels.


This empowers you to compare multiple models on the same dataset and quickly visualize agreement levels across annotations. For this test case, we used:


- GPT-4o
- Claude 3.7 Sonnet
- Gemini 2.5 Pro
- LLaMA 3.3 70B


---


## **Results**


We evaluated each model based on two factors: accuracy and coverage. Accuracy was measured by inter-annotator agreement (IAA) between the model and a human reviewer. Coverage was calculated using the formula: **‍**


- Accuracy = Accepted out of Predicted Labels
- Coverage = Accepted labels / 744 total labels
- Final Score = 70% Accuracy + 30% Coverage


Each label required agreement with at least 2 models for Datasaur to auto-accept that label: a consensus of 2 out of 4. The remaining labels were reviewed and finalized by a human reviewer.


*(red labels indicate conflict between models for a reviewer to resolve; grey labels indicate consensus among Labeling Agents)*


Using accuracy and coverage, and despite not being the leading LLM for current benchmarks, ChatGPT-4o performed the best of all models.


Model Accepted Total Predicted Missed Accuracy % Coverage % Time Spent Weighted Score


GPT-4o 465 502 251 92.63 64.94 14 m 5 s 84.32


Claude 3.7 527 625 177 84.32 74.86 19 m 5 s 81.48


Gemini 2.5 Pro 546 637 175 85.71 75.73 1 h 28 m 82.72


LLaMA 3.3 70B 483 531 237 90.96 67.08 8 m 48 s 83.8


🔹 **Note** : Gemini took significantly more time (1h 28m) compared to the other models (8–12 minutes). This may be a tradeoff to consider when optimizing for throughput.


## **Discussion**


These results show that no single model dominates across every metric. ChatGPT achieved the highest overall score: marking an impressive 92% accuracy with zero-shot prompting. Claude offered a balance between speed and quality, where Llama was significantly more efficient with coverage. Gemini provided the best coverage of the dataset but took the longest time to label compared to the group. These insights are important as your team weighs priorities.


If you're experimenting with LLMs for labeling tasks, Datasaur's Labeling Agent makes it easy to compare them side by side. Choose a model, configure your prompt, enjoy hands-free labeling, and start evaluating.[Sign up today for access](https://datasaur.ai/watch-demo) to your Labeling Agent.
