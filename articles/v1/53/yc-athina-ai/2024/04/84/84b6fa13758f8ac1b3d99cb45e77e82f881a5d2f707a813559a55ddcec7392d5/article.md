---
schema_version: "1.0.0"
document_id: "84b6fa13758f8ac1b3d99cb45e77e82f881a5d2f707a813559a55ddcec7392d5"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/wizardlm-empowering-large-language-models-to-follow-complex-instructions"
published_at: "2024-04-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:80f48af7f9d69b9661f6d42c7dd1be5b1721147c8f2ed7e9862d5f7885eb285e"
---

# WizardLM: Empowering Large Language Models to Follow Complex Instructions

Do not index


Original Paper


[https://arxiv.org/abs/2304.12244](https://arxiv.org/abs/2304.12244)


Blog URL


[blog.athina.ai /wizardl...ructions](https://blog.athina.ai/wizardlm-empowering-large-language-models-to-follow-complex-instructions)


**Original Paper:**[https://arxiv.org/abs/2304.12244](https://arxiv.org/abs/2304.12244)


**By:**[Can Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20C) ,[Qingfeng Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20Q) ,[Kai Zheng](https://arxiv.org/search/cs?searchtype=author&query=Zheng%2C%20K) ,[Xiubo Geng](https://arxiv.org/search/cs?searchtype=author&query=Geng%2C%20X) ,[Pu Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20P) ,[Jiazhan Feng](https://arxiv.org/search/cs?searchtype=author&query=Feng%2C%20J) ,[Chongyang Tao](https://arxiv.org/search/cs?searchtype=author&query=Tao%2C%20C) ,[Daxin Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20D)


**Abstract:**


> Training large language models (LLMs) with open-domain instruction following data brings colossal success. However, manually creating such instruction data is very time-consuming and labor-intensive. Moreover, humans may struggle to produce high-complexity instructions. In this paper, we show an avenue for creating large amounts of instruction data with varying levels of complexity using LLM instead of humans. Starting with an initial set of instructions, we use our proposed Evol-Instruct to rewrite them step by step into more complex instructions. Then, we mix all generated instruction data to fine-tune LLaMA. We call the resulting model WizardLM. Human evaluations on a complexity-balanced test bed and Vicuna's testset show that instructions from Evol-Instruct are superior to human-created ones. By analyzing the human evaluation results of the high complexity part, we demonstrate that outputs from our WizardLM are preferred to outputs from OpenAI ChatGPT. In GPT-4 automatic evaluation, WizardLM achieves more than 90\\% capacity of ChatGPT on 17 out of 29 skills. Even though WizardLM still lags behind ChatGPT in some aspects, our findings suggest that fine-tuning with AI-evolved instructions is a promising direction for enhancing LLMs. Our code and data are public at
>
>
> [this https URL](https://github.com/nlpxucan/WizardLM)


---


####


Summary


####


The Challenge of Following Complex Instructions


LLMs are great at creating coherent text but fall short when it comes to interpreting and executing detailed instructions. This has led to significant efforts to improve their capacity for handling more complex directions. Traditionally, models were trained on specific, hand-written instructions, restricting the range of tasks they could perform.


####


Advancements with Open-Domain Instruction Training


OpenAI's InstructGPT and ChatGPT models represent a leap forward, showing that LLMs can tackle a wider array of tasks more effectively. Yet, the search for a model that can effortlessly manage complex instructions is ongoing.


####


Introducing Evol-Instruct


Evol-Instruct is a new method aimed at boosting LLMs' ability to process complex instructions by:


- **Starting Simple** : It kicks off with basic instructions.


- **Increasing Complexity and Diversity** : Instructions evolve to become more complex and varied.


- **Selective Refinement** : Less effective instruction paths are weeded out, leaving a dataset rich with diverse and complex instructions for training LLMs.


This approach results in a dataset that's ideal for preparing LLMs to handle a wide range of tasks.


####


WizardLM: A New Benchmark


WizardLM, fine-tuned with Evol-Instruct generated data, outshines others, including ChatGPT, in understanding and executing complex instructions. This highlights the potential of evolving training methods to enhance LLM capabilities.


####


Methodology Summary


Developing WizardLM involved:


1. **Evolving Instruction Data** : Enhancing initial instructions to more complex forms.


1. **Automating Evolution** : Streamlining the process to improve instruction quality.


1. **Purposeful Fine-tuning** : Using evolved instructions as the basis for training the LLM to grasp a variety of instruction complexities.


####


Performance Evaluation


WizardLM surpasses other models in handling complex instructions and matches or nearly matches ChatGPT in several skills, marking a significant step forward in LLM evolution.


####


Looking Ahead


While WizardLM sets new standards, concerns about generating unethical content persist, pointing to the need for further research to ensure the model's ethical use. Despite these challenges, WizardLM's development through Evol-Instruct signals a major advance in equipping LLMs to interpret complex instructions, opening up new possibilities in natural language processing and AI.


####


Conclusion


WizardLM's creation via Evol-Instruct is a milestone in enabling LLMs to deal with complex instructions, offering AI engineers more capable and versatile tools. This progress promises to bridge the gap between human and machine understanding, heralding an era ripe with AI innovation.
