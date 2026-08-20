---
schema_version: "1.0.0"
document_id: "480840b34909e025f79aea117c93f5a732c62823258b0f1654d640e559afee58"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/evidence-to-generate-e2g-a-single-agent-two-step-prompting-for-context-grounded-and-retrieval-augmented-reasoning"
published_at: "2024-01-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:c1f72788ce3925d748e9fa8e1d6a0f2ce76df5438000968cae8b6b87ff6f457c"
---

# Evidence to Generate (E2G): A Single-agent Two-step Prompting for Context Grounded and Retrieval Augmented Reasoning

Do not index


Original Paper


[https://arxiv.org/abs/2401.05787](https://arxiv.org/abs/2401.05787)


Blog URL


[blog.athina.ai /evidenc...easoning](https://blog.athina.ai/evidence-to-generate-e2g-a-single-agent-two-step-prompting-for-context-grounded-and-retrieval-augmented-reasoning)


**Original Paper:**[https://arxiv.org/abs/2401.05787](https://arxiv.org/abs/2401.05787)


**By:**[Md Rizwan Parvez](https://arxiv.org/search/cs?searchtype=author&query=Parvez%2C%20M%20R)


**Abstract:**


> While chain-of-thought (CoT) prompting has revolutionized how LLMs perform reasoning tasks, its current methods and variations (e.g, Self-consistency, ReACT, Reflexion, Tree-of-Thoughts (ToT), Cumulative Reasoning (CR)) suffer from limitations like slowness, limited context grounding, hallucination and inconsistent outputs. To overcome these challenges, we introduce Evidence to Generate (E2G), a novel single-agent, two-step prompting framework. Instead of unverified reasoning claims, this innovative approach leverages the power of "evidence for decision making" by first focusing exclusively on the thought sequences (the series of intermediate steps) explicitly mentioned in the context which then serve as extracted evidence, guiding the LLM's output generation process with greater precision and efficiency. This simple yet powerful approach unlocks the true potential of chain-of-thought like prompting, paving the way for faster, more reliable, and more contextually aware reasoning in LLMs. \\tool achieves remarkable results robustly across a wide range of knowledge-intensive reasoning and generation tasks, surpassing baseline approaches with state-of-the-art LLMs. For example, (i) on LogiQA benchmark using GPT-4 as backbone model, \\tool achieves a new state-of-the Accuracy of 53.8% exceeding CoT by 18%, ToT by 11%, CR by 9% (ii) a variant of E2G with PaLM2 outperforms the variable-shot performance of Gemini Ultra by 0.9 F1 points, reaching an F1 score of 83.3 on a subset of DROP.


---


###


Summary Notes


####


Revolutionizing Reasoning with Evidence to Generate (E2G) in AI


####


Introduction


The rise of Large Language Models (LLMs) has dramatically altered the AI landscape, showing great promise in handling complex tasks. However, their ability to reason over intricate, context-based information remains limited, affecting their usefulness in applications needing deep understanding and advanced reasoning.


The Evidence to Generate (E2G) framework emerges as a groundbreaking solution to boost LLMs' reasoning skills by grounding their process in relevant information, thereby reducing errors and cognitive strain.


####


Understanding the Challenges


- **Chain-of-Thought (CoT) Prompting Limitations:** While CoT prompting has made LLMs mimic the human-like step-by-step problem-solving approach, its effectiveness decreases with complex contexts requiring specific, relevant information.


- **Complexities of Context-based Reasoning:** Enhancing LLMs to better reason with context involves dealing with long, imperfect texts and ensuring the reasoning aligns with the given context, presenting a significant challenge.


####


The E2G Framework: A New Approach


E2G offers a streamlined, effective method to improve LLM reasoning through a single-agent, two-step strategy:


- **E-step (Evidence Extraction):** This step involves identifying and extracting key evidence from the context, ensuring the reasoning process is based on relevant information.


- **G-step (Generation):** With the evidence at hand, the model then generates answers or solutions, reducing cognitive load and increasing reasoning accuracy and efficiency.


This innovative approach significantly advances LLM prompting strategies.


####


E2G in Practice: Proven Success


E2G's effectiveness extends beyond theory. Tests on benchmarks like LogiQA and DROP have shown E2G outperforms existing methods, including CoT, in accuracy and efficiency, showcasing its potential to transform reasoning tasks in LLMs.


####


Looking Ahead: The Future of LLM Reasoning


E2G marks a crucial step forward in enhancing LLMs' reasoning capabilities, addressing the challenges of context-grounded reasoning and retrieval-augmented generation. It opens new doors for applying LLMs across various domains and tasks with a focus on evidence-based reasoning.


####


Future Research and Considerations


- **Expanding E2G Applications:** Future work will focus on refining E2G for specific domains, diverse reasoning tasks, and exploring context-reasoning datasets to further unlock LLM capabilities.


- **Ethical and Limitation Concerns:** It's important to consider potential limitations, especially in under-resourced domains or languages, and ethical issues related to data use and human evaluations to ensure responsible E2G implementation.


####


Conclusion


Evidence to Generate (E2G) introduces a novel chapter in advancing Large Language Models, providing a solid answer to the challenges of context-based reasoning and data retrieval.


As we continue to develop and refine this approach, the potential for LLMs to revolutionize industries and expand the boundaries of what's possible is increasingly exciting.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
