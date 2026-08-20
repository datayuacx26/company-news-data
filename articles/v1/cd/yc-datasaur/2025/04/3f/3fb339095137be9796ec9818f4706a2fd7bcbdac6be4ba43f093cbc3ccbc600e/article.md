---
schema_version: "1.0.0"
document_id: "3fb339095137be9796ec9818f4706a2fd7bcbdac6be4ba43f093cbc3ccbc600e"
company_key: "yc-datasaur"
company: "Datasaur"
source_id: "yc-datasaur-news-import-f7082d4871d2"
canonical_url: "https://datasaur.ai/blog-posts/april-feature-updates-mixed-labeling-smarter-search-and-more-project-control"
published_at: "2025-04-28T00:00:00+00:00"
first_seen_at: "2026-07-25T01:27:49.393704+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:80d0127111646f55f7a7f279e722116dfc1eff5617d4931435eedb9970aefa5a"
---

# April Feature Updates: Mixed Labeling, Smarter Search, and More Project Control

# Data Studio


## Span + Line Labeling


We’ve introduced a new mixed labeling type that supports labeling spans within a line while also categorizing entire lines separately. This allows you to configure questions for each line and still perform regular span labeling. Perfect for use cases that require dual-level granularity like transcripts, contracts, or forms.


[Learn more](https://docs.datasaur.ai/nlp-projects/lets-get-labeling/span-based/span-+-line-labeling)


## Amazon Transcribe for Audio Labeling


You can now select Amazon Transcribe as an Automatic Speech Recognition (ASR) option when labeling audio files. This brings another powerful transcription engine to your toolkit, giving you more flexibility when comparing accuracy and usability between providers.


[Learn more](https://docs.datasaur.ai/api/custom-asr)


## Bottom-Level Label Selection in Span Labeling


To reduce accidental selections and ensure more precise annotations, we’ve added an option to restrict span labels to only bottom-level labels in your hierarchy. You can now prevent the selection of parent labels/categories when more specific labels are intended.


[Learn more](https://docs.datasaur.ai/nlp-projects/lets-get-labeling/label-sets#limit-selection-to-bottom-level-labels-only)


## Bulk Applying Project Tag


You can now apply tags to multiple projects in bulk directly from the Projects table. Whether you're organizing by client, task type, or deadline, bulk tagging makes streamlines project management.


## Search and Show Only Matching Lines


A new setting in the Search extension allows you to filter and display **only matching lines** . This helps reviewers and labelers focus on the most relevant sections, speeding up review and annotation.


[Learn more](https://docs.datasaur.ai/advanced/extensions/search#search-result)


# LLM Labs


## Direct Access LLMs Expansion


We've significantly expanded our Direct Access LLMs lineup, giving you instant access to the latest state-of-the-art models without complex API setup or configuration. Our newest additions include:


- Gemma 3 27B, available from Google AI and Hugging Face
- Deepseek R1, now available as an alternative option through Amazon Bedrock
- GPT 4.1, GPT 4.1. mini, and GPT 4.1. nano, available from both OpenAI and Azure OpenAI
- GPT o3 and GPT 4 mini, also available from both OpenAI and Azure OpenAI
- Grok 3 from xAI


## **Automatic Sync for External Object Storage**


Managing your knowledge base just got easier with automatic synchronization for External Object Storage. This new feature allows you to schedule periodic syncs between your external storage (AWS S3, Google Cloud Storage, Azure Blob) and LLM Labs. This automation saves valuable time and ensures your RAG applications always have access to the latest data without requiring manual intervention.


## Conversational Prompting in Sandbox


Our Sandbox environment now supports conversational prompting, allowing you to replicate multi-turn dialogues and historical conversations within your testing environment. This feature is invaluable for prompt engineers and developers who need to troubleshoot or fine-tune conversational AI applications.


By simulating realistic conversation flows, you can better understand how your models respond to context-aware queries and make more informed adjustments to your prompting strategies. This brings the Sandbox experience closer to real-world chat applications, helping you develop more natural and effective AI interactions.


We’re excited to bring these features to your workflow. As always, your feedback helps shape Datasaur. Let us know what you'd like to see next!
