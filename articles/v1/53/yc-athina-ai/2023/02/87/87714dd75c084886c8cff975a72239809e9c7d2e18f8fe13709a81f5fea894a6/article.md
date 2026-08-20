---
schema_version: "1.0.0"
document_id: "87714dd75c084886c8cff975a72239809e9c7d2e18f8fe13709a81f5fea894a6"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/evaluating-the-robustness-of-discrete-prompts"
published_at: "2023-02-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:3009c35416dba47609031b1f9bd02d2a45cb0bae08b60fdd9187f32f7e64939a"
---

# Evaluating the Robustness of Discrete Prompts

Do not index


Original Paper


[https://arxiv.org/abs/2302.05619](https://arxiv.org/abs/2302.05619)


Blog URL


[blog.athina.ai /evaluat...-prompts](https://blog.athina.ai/evaluating-the-robustness-of-discrete-prompts)


**Original Paper:**[https://arxiv.org/abs/2302.05619](https://arxiv.org/abs/2302.05619)


**By:**[Yoichi Ishibashi](https://arxiv.org/search/cs?searchtype=author&query=Ishibashi%2C%20Y) ,[Danushka Bollegala](https://arxiv.org/search/cs?searchtype=author&query=Bollegala%2C%20D) ,[Katsuhito Sudoh](https://arxiv.org/search/cs?searchtype=author&query=Sudoh%2C%20K) ,[Satoshi Nakamura](https://arxiv.org/search/cs?searchtype=author&query=Nakamura%2C%20S)


**Abstract:**


> Discrete prompts have been used for fine-tuning Pre-trained Language Models for diverse NLP tasks. In particular, automatic methods that generate discrete prompts from a small set of training instances have reported superior performance. However, a closer look at the learnt prompts reveals that they contain noisy and counter-intuitive lexical constructs that would not be encountered in manually-written prompts. This raises an important yet understudied question regarding the robustness of automatically learnt discrete prompts when used in downstream tasks. To address this question, we conduct a systematic study of the robustness of discrete prompts by applying carefully designed perturbations into an application using AutoPrompt and then measure their performance in two Natural Language Inference (NLI) datasets. Our experimental results show that although the discrete prompt-based method remains relatively robust against perturbations to NLI inputs, they are highly sensitive to other types of perturbations such as shuffling and deletion of prompt tokens. Moreover, they generalize poorly across different NLI datasets. We hope our findings will inspire future work on robust discrete prompt learning.


---


###


Summary Notes


####


Evaluating the Strength of Discrete Prompts in NLP


Discrete prompts have become essential in enhancing pre-trained language models (PLMs) for specific tasks within Natural Language Processing (NLP).


These prompts, whether crafted by hand or generated automatically, are key in customizing PLMs. Yet, questions about their reliability and adaptability, especially for automatically generated prompts, are growing.


This blog explores a detailed study on how well discrete prompts hold up under various conditions, providing critical insights for AI Engineers in enterprise settings.


####


Study Approach


The study investigates several discrete prompt learning techniques:


- **AutoPrompt (AP)**


- **Manually-written Prompts (MP)**


- **Head-based Fine-Tuning (HFT)**


It uses CommitmentBank (CB) and MultiNLI (MNLI) datasets and tests the prompts' strength against changes like shuffling, deletion of tokens, and adversarial changes.


####


Experimental Details


The experiments are based on the RoBERTa-large model, known for its excellence in NLP tasks. The study measures prompt effectiveness through precision (P@1) and introduces the Rate of Degradation (RoD) to gauge performance drops under different conditions.


####


Main Discoveries


- **Performance Sensitivity** : MPs typically outperform APs when there are many training examples available. APs are sensitive to the arrangement and selection of tokens, suggesting they may latch onto dataset-specific oddities rather than learn broad patterns.


- **Generalization Challenges** : When prompts trained on one dataset are tested on another, there's a significant drop in performance, highlighting issues with prompt transferability.


- **Handling Perturbations** : APs and MPs show some resilience to minor perturbations but struggle significantly with adversarial changes and when tokens are shuffled or removed. APs are more negatively impacted.


####


Key Takeaways


This study highlights the difficulties in using automatically learned discrete prompts for robust NLP applications. It points to the need for more resilient prompt learning methods that can handle adversarial situations and work well across different datasets.


####


Advice for AI Engineers


- **Expand Training Data** : Use a wide variety of training examples to improve prompt versatility.


- **Test Prompt Strength** : Regularly challenge your prompts with perturbations and adversarial scenarios to pinpoint vulnerabilities.


- **Refine Prompt Design** : Though automatic prompts are convenient, manually created prompts, with a thorough understanding of the specific task and dataset, might offer superior performance and reliability.


####


Considerations and Ethical Aspects


The study focuses on English, meaning its findings may not apply to other languages. It also examines a limited number of prompt learning methods, suggesting the need for further research. Additionally, it acknowledges the biases in PLMs like RoBERTa, emphasizing the importance of considering these biases in future NLP model development and evaluations.


####


Acknowledgments


This research was supported by the JSPS KAKENHI, Grant Number JP22H03654, highlighting the collaborative effort behind this extensive study.


####


Final Thoughts


The exploration of discrete prompts in fine-tuning PLMs underscores both challenges and opportunities for AI Engineers.


By understanding the strengths and limitations of different prompt learning methods, professionals can better tackle NLP tasks, leading to more dependable and efficient AI solutions.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
