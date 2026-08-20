---
schema_version: "1.0.0"
document_id: "2dcaac146b3282ff8c130b05e90eb05af298f3dcf5ab63775b01091f8f1574d5"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/large-language-models-are-reasoners-with-self-verification"
published_at: "2022-12-19T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:58dd525b6a30aa37e142c690683c11474d91132e694d7a2a9892acba2f809e17"
---

# Large Language Models are reasoners with Self-Verification

Do not index


Original Paper


[https://arxiv.org/abs/2212.09561v1](https://arxiv.org/abs/2212.09561v1)


Blog URL


[blog.athina.ai /large-l...fication](https://blog.athina.ai/large-language-models-are-reasoners-with-self-verification)


**Original Paper:**[https://arxiv.org/abs/2212.09561v1](https://arxiv.org/abs/2212.09561v1)


**By:**[Yixuan Weng](https://arxiv.org/search/cs?searchtype=author&query=Weng%2C%20Y) ,[Minjun Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20M) ,[Shizhu He](https://arxiv.org/search/cs?searchtype=author&query=He%2C%20S) ,[Kang Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20K) ,[Jun Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20J)


**Abstract:**


> When a large language model (LLM) performs complex reasoning by chain of thought (CoT), it can be highly sensitive to individual mistakes. We have had to train verifiers to address this issue. As we all know, after human inferring a conclusion, they often check it by re-verifying it, which can avoid some mistakes. We propose a new method called self-verification that uses the conclusion of the CoT as a condition to build a new sample and asks the LLM to re-predict the original conditions which be masked. We calculate an explainable verification score based on the accuracy. This method can improve the accuracy of multiple arithmetics and logical reasoning datasets when using few-shot learning. we have demonstrated that LLMs can conduct explainable self-verification of their own conclusions and achieve competitive reasoning performance. Extensive experimentals have demonstrated that our method can help multiple large language models with self-verification can avoid interference from incorrect CoT. Code is available at \\url{
>
>
> [this https URL](https://github.com/WENGSYX/Self-Verification)


---


###


Summary Notes


##


Boosting LLM Reasoning with Self-Verification


In the dynamic world of artificial intelligence, Large Language Models (LLMs) like GPT-3 have advanced rapidly, showing human-like reasoning skills.


However, they still face challenges in complex reasoning tasks, leading to errors. To improve these AI systems, researchers are turning to **self-verification** , a promising method to make LLMs more accurate and dependable.


This blog post explores how self-verification boosts the reasoning abilities of LLMs, offering AI Engineers in enterprise companies a mix of technical insights and actionable advice on implementing this technique.


###


Understanding Self-Verification


Self-verification is a cutting-edge technique that allows LLMs to check their own answers.


Inspired by human behavior of double-checking our reasoning, this method uses Chain of Thought (CoT) prompts to generate multiple possible answers.


LLMs then evaluate each answer against the problem's conditions, choosing the most consistent one as the final answer.


####


How It Works


The self-verification process involves three main steps:


- **Generating Candidate Conclusions:** The model produces various reasoning paths using CoT prompts.


- **Verification Scoring:** It then assesses each conclusion by masking parts of the input conditions and checking consistency with the remaining information.


- **Implementation:** Researchers have applied this method to pre-trained models like GPT-3, testing it on arithmetic and logical reasoning tasks.


###


Experiments and Results


####


Setup


The method was tested with datasets such as GSM8K, SingleEq, MultiArith, and AQuA-RAT, encompassing arithmetic and commonsense reasoning tasks. Models like GPT-3 and Codex were used in these experiments.


####


Outcomes


Self-verification significantly increased reasoning accuracy in LLMs across all tasks and model setups, outperforming baseline models and previous methods. This shows its effectiveness in enhancing LLM reasoning, especially in complex tasks.


###


Key Takeaways


Self-verification marks a significant improvement over the standard CoT approach by adding a verification step, reducing errors from logical mistakes in initial conclusions. It mimics human cognitive processes, making LLMs more error-resistant.


###


Challenges and Next Steps


The success of self-verification partly depends on the quality of the initial reasoning paths. Inadequate initial reasoning can diminish the benefits of the verification step. Future efforts could focus on refining verification mechanisms and applying the method to more reasoning tasks.


###


Conclusion


Self-verification is a revolutionary method that noticeably boosts the reasoning abilities of LLMs.


By allowing models to critically assess their reasoning paths, it brings LLMs closer to human-like reasoning.


For AI Engineers, integrating self-verification into LLMs offers a chance to develop more reliable and precise AI systems, pushing AI innovation and application forward.


As we aim for AI that truly reflects human reasoning, self-verification shines as a key advancement. Continuing to develop and refine this method holds the promise of unlocking more advanced and dependable AI reasoning capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
