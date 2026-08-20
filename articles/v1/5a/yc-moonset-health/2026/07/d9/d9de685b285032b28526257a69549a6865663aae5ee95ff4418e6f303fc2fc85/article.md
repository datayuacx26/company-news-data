---
schema_version: "1.0.0"
document_id: "d9de685b285032b28526257a69549a6865663aae5ee95ff4418e6f303fc2fc85"
company_key: "yc-moonset-health"
company: "Moonset Health"
source_id: "yc-moonset-health-news-import-59d86829977a"
canonical_url: "https://www.moonsethealth.com/blog/llms-101-for-nurses"
published_at: null
first_seen_at: "2026-07-24T11:43:38.030108+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:f8e43aa18fa454ed6eb5b4df639ee76a3d83d5215e2478fe065ce13a3d94db9c"
---

# LLMs 101 for Nurses: How Your AI Scribe "Understands" Clinical Notes

If you’ve ever wondered what happens “under the hood” when your AI scribe drafts a note or summarizes a patient’s status, the answer lies in **large language models** . This is the same technology powering modern chatbots and virtual assistants. Read on to learn what LLMs are, how they work, and why they’re ideally suited to help you spend less time charting and more time caring.


#### What Is a Large Language Model?


As an extremely simplified explanation, an LLM is a computer program trained to predict the next word in a sentence, over and over and over again, using massive amounts of text (think millions of clinical notes, medical journals, and textbooks). Through this process, it learns patterns of language: how words fit together, which phrases convey particular meanings, and how to generate fluent, context‑aware responses. But sophisticated LLMs (like the ones used by Moonset Health) go far beyond just that…


#### The Transformer Architecture


Most LLMs today are built on the **transformer** framework. Unlike older models that processed text one word at a time, transformers look at an entire sentence (or paragraph) simultaneously, using an attention mechanism to weigh which words matter most for understanding context. This means your scribe can grasp that “patient denies fever but reports increased pain” links “denies” to “fever” (a negative finding) while highlighting “increased” as a positive symptom.


#### Fine‑Tuning for Clinical Precision


A general LLM trained on the open web can write essays or chat about movies, but it doesn’t innately understand medical terminology. That’s where **fine‑tuning** comes in: the base model is re‑trained on healthcare‑specific texts like EHR notes and medical guidelines, so it masters the vocabulary and formats you use every day. The result? An AI scribe that knows to record “orthostatic hypotension” as a vital‑sign concern and to slot it under “Cardiovascular” in your note template.


#### Prompting and Context Windows


When you start dictating, your scribe frames your words as a **prompt** , which is the model’s starting point or custom "instructions." The model then generates the rest of the note within a “context window” that may span several thousand words, allowing it to reference earlier parts of the conversation (or even previous visits) as it writes. This continuity ensures your note feels cohesive, reflecting the patient’s history and the flow of today’s assessment.


#### Retrieval‑Augmented Generation (RAG)


To minimize errors and “hallucinations” (where the model makes up facts), many AI scribes use RAG: the model pulls in verified snippets from your EHR or static medical databases before drafting. For instance, if you mention “pain scale,” the scribe can fetch yesterday’s entry on “Pain: 3/10” and weave it seamlessly into today’s narrative, ensuring accuracy and consistency.


#### ln Conclusion


Large language models are the engines that power modern AI scribes, transforming raw speech into polished, accurate clinical documentation. By combining transformer‑based text generation, fine‑tuning on medical data, and retrieval‑augmented checks, these systems are tailor‑made to support hospice nurses, lightening documentation burdens and letting you focus on what you do best: compassionate patient care.


Feel free to reach out for a demo or pilot, and see firsthand how LLM‑driven scribing can transform your workflow.
