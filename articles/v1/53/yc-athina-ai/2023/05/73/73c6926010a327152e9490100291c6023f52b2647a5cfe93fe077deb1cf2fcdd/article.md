---
schema_version: "1.0.0"
document_id: "73c6926010a327152e9490100291c6023f52b2647a5cfe93fe077deb1cf2fcdd"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/making-large-language-models-better-reasoners-with-step-aware-verifier"
published_at: "2023-05-24T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:8283d6e067a28910b9064c19891b54d8a4ac745f03c3af74a309a8fa9fbce17e"
---

# Making Large Language Models Better Reasoners with Step-Aware Verifier

Do not index


Original Paper


[https://arxiv.org/abs/2206.02336](https://arxiv.org/abs/2206.02336)


Blog URL


[blog.athina.ai /making-...verifier](https://blog.athina.ai/making-large-language-models-better-reasoners-with-step-aware-verifier)


**Original Paper:**[https://arxiv.org/abs/2206.02336](https://arxiv.org/abs/2206.02336)


**By:**[Yifei Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20Y) ,[Zeqi Lin](https://arxiv.org/search/cs?searchtype=author&query=Lin%2C%20Z) ,[Shizhuo Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20S) ,[Qiang Fu](https://arxiv.org/search/cs?searchtype=author&query=Fu%2C%20Q) ,[Bei Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20B) ,[Jian-Guang Lou](https://arxiv.org/search/cs?searchtype=author&query=Lou%2C%20J) ,[Weizhu Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20W)


**Abstract:**


> Few-shot learning is a challenging task that requires language models to generalize from limited examples. Large language models like GPT-3 and PaLM have made impressive progress in this area, but they still face difficulties in reasoning tasks such as GSM8K, a benchmark for arithmetic problems. To improve their reasoning skills, previous work has proposed to guide the language model with prompts that elicit a series of reasoning steps before giving the final answer, achieving a significant improvement on GSM8K from 17.9% to 58.1% in problem-solving rate. In this paper, we present DIVERSE (Diverse Verifier on Reasoning Step), a novel approach that further enhances the reasoning capability of language models. DIVERSE has three main components: first, it generates diverse prompts to explore different reasoning paths for the same question; second, it uses a verifier to filter out incorrect answers based on a weighted voting scheme; and third, it verifies each reasoning step individually instead of the whole chain. We evaluate DIVERSE on the latest language model code-davinci-002 and show that it achieves new state-of-the-art results on six of eight reasoning benchmarks (e.g., GSM8K 74.4% to 83.2%).


---


###


Summary Notes


####


Improving Reasoning in Large Language Models with DIVERSE


The field of Artificial Intelligence (AI) has seen remarkable advancements with Large Pretrained Language Models (PLMs) like GPT-3 and PaLM, which excel in creating human-like text.


However, their ability to reason, especially through complex, multi-step tasks, remains limited. The introduction of a new method called DIVERSE (Diverse Verifier on Reasoning Step) aims to enhance these models' reasoning skills by generating varied prompts and utilizing a step-aware verifier.


####


How DIVERSE Works


DIVERSE enhances PLMs' reasoning through three key components:


- **Diverse Prompts:** It creates different reasoning paths for the same question, assuming the correct answer should be consistent across variations. This not only tests the model's reasoning robustness but also widens the exploration of solutions.


- **Verifier:** Incorporating a weighted voting scheme, this component evaluates the quality of the reasoning paths, playing a critical role in verifying the reasoning process's reliability.


- **Step-Aware Verifier:** This innovative component checks the accuracy of each reasoning step, allowing for a detailed analysis of potential reasoning errors and providing insights for improvement.


####


Testing DIVERSE


Experiments across various reasoning tasks, including arithmetic, commonsense, and inductive reasoning, were conducted to assess DIVERSE's effectiveness.


Using models like davinci and code-davinci-002 from OpenAI and benchmarks such as AsDiv and CommonsenseQA, significant improvements were observed, highlighting DIVERSE's potential to elevate PLM reasoning capabilities.


####


Results Highlights


DIVERSE consistently enhanced PLM performance across tasks and models, achieving state-of-the-art results on six benchmarks.


This method outperformed baseline approaches like Greedy Decode and Self-Consistency, indicating its effectiveness in improving reasoning.


####


In-Depth Case Study


Case studies revealed how DIVERSE's step-aware verifier pinpointed and corrected reasoning errors, showcasing its interpretative and diagnostic strengths.


Ablation studies further confirmed the benefits of diverse prompts and the step-aware verifier, leading to more varied reasoning paths and a stronger verification process.


####


DIVERSE vs. Previous Methods


Unlike earlier attempts to boost PLM reasoning with prompts, fine-tuning, or external knowledge, DIVERSE addresses these methods' limitations by innovatively employing diverse prompts, a voting verifier, and a step-aware verifier.


Future efforts will aim to broaden DIVERSE's task applicability and refine prompt generation.


####


Limitations and Future Directions


Despite its successes, DIVERSE faces challenges, including high computational demands and ensuring the accuracy of generated reasoning paths.


These issues underscore the necessity for continued research in this area.


####


Conclusion


DIVERSE marks a significant step forward in enhancing the reasoning capabilities of large language models. By leveraging diverse prompts and a step-aware verifier, it offers a comprehensive solution to a key AI challenge.


The ongoing development of methods like DIVERSE is crucial for realizing PLMs' full potential in complex reasoning tasks.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
