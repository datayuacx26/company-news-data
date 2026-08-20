---
schema_version: "1.0.0"
document_id: "118ec41c81ed6846a1deb126710f412490ba7e31470d5589cb4c38bb7716ab28"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/ever-mitigating-hallucination-in-large-language-models-through-real-time-verification-and-rectification"
published_at: "2024-04-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:58.312202+00:00"
content_hash: "sha256:61b0a8000a6f396fd099032a66303be477575503c1cdffab50ef6ae06d1811b6"
---

# Ever: Mitigating Hallucination in Large Language Models through Real-Time Verification and Rectification

Do not index


Original Paper


[https://arxiv.org/html/2311.09114v2](https://arxiv.org/html/2311.09114v2)


Blog URL


[blog.athina.ai /ever-mi...fication](https://blog.athina.ai/ever-mitigating-hallucination-in-large-language-models-through-real-time-verification-and-rectification)


**Original Paper:**[https://arxiv.org/html/2311.09114v2](https://arxiv.org/html/2311.09114v2)


**By:**[Haoqiang Kang](https://arxiv.org/search/cs?searchtype=author&query=Kang%2C%20H) ,[Juntong Ni](https://arxiv.org/search/cs?searchtype=author&query=Ni%2C%20J) ,[Huaxiu Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao%2C%20H)


**Abstract:**


> Large Language Models (LLMs) have demonstrated remarkable proficiency in generating fluent text. However, they often encounter the challenge of generating inaccurate or hallucinated content. This issue is common in both non-retrieval-based generation and retrieval-augmented generation approaches, and existing post-hoc rectification methods may not address the accumulated hallucination errors that may be caused by the "snowballing" issue, especially in reasoning tasks. To tackle these challenges, we introduce a novel approach called Real-time Verification and Rectification (Ever). Instead of waiting until the end of the generation process to rectify hallucinations, Ever employs a real-time, step-wise generation and hallucination rectification strategy. The primary objective is to detect and rectify hallucinations as they occur during the text generation process. When compared to both retrieval-based and non-retrieval-based baselines, Ever demonstrates a significant improvement in generating trustworthy and factually accurate text across a diverse range of tasks, including short-form QA, biography generation, and multi-hop reasoning.


---


###


Summary Notes


As AI Engineers explore the potential of Large Language Models (LLMs) like GPT-3, ensuring the accuracy of generated content is crucial. Despite their advancements, LLMs sometimes produce inaccurate or "hallucinated" content, posing challenges for practical applications.


####


Understanding the Challenge


LLMs can generate content that is either completely fabricated or misleading, referred to as hallucinations. These inaccuracies fall into two categories:


- **Intrinsic Hallucinations** : Contradictions to known facts.


- **Extrinsic Hallucinations** : Statements that seem plausible but are unverifiable.


Traditional methods to address these inaccuracies, such as pre-generation and post-generation checks, are limited, especially in tasks involving complex reasoning.


####


Introducing the EVER Framework


The EVER (Real-Time Verification and Rectification) framework is designed to dynamically mitigate hallucinations during text generation by ensuring the accuracy and trustworthiness of each sentence before proceeding. The EVER process includes:


- **Initial Generation** : Starting with the creation of a sentence based on the user's prompt.


- **Concept-Level Validation** : Evaluating each sentence to identify potential hallucinations.


- **Rectification** : Correcting intrinsic hallucinations with factual evidence and clarifying extrinsic hallucinations.


- **Further Processing** : Revalidating information to ensure accuracy, with an option to omit misleading information.


####


Advantages of EVER


The EVER framework not only reduces hallucinations but also improves the trustworthiness of the text through real-time verification. It also aids in generating preference data pairs for tuning the model's accuracy over time.


####


Experiment Results


Tests on tasks like short-form QA and biography generation showed:


- A consistent reduction in hallucinations.


- Increased trustworthiness of generated content.


- Improved accuracy by incorporating specific information.


Despite these advancements, EVER does not replace traditional fact-checking but enhances LLMs by reducing hallucination instances.


####


Conclusion


The EVER framework significantly improves the reliability and accuracy of LLM-generated content, offering a real-time solution to the challenge of hallucinations. It enables AI Engineers to utilize LLMs more confidently across various industries.


####


Acknowledgement


The development of EVER was supported by the Center for AI Safety and the Google Cloud Research Credits program, contributing significantly to this project's advancement.


####


Further Reading


For those interested in deeper insights, the appendices detail EVER's application in specific tasks, the impact of multi-round rectification, and the experimental prompt templates, providing a comprehensive view of EVER's potential in enhancing LLM accuracy in enterprise settings.
