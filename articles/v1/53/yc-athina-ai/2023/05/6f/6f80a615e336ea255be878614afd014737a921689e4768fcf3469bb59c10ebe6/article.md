---
schema_version: "1.0.0"
document_id: "6f80a615e336ea255be878614afd014737a921689e4768fcf3469bb59c10ebe6"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/multitool-cot-gpt-3-can-use-multiple-external-tools-with-chain-of-thought-prompting"
published_at: "2023-05-26T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:bc595b615d484e5c22bd5cc9ad007d9c6eee34c70ec5182ba8f8fe99e7f33c12"
---

# MultiTool-CoT: GPT-3 Can Use Multiple External Tools with Chain of Thought Prompting

Do not index


Original Paper


[https://arxiv.org/abs/2305.16896](https://arxiv.org/abs/2305.16896)


Blog URL


[blog.athina.ai /multito...rompting](https://blog.athina.ai/multitool-cot-gpt-3-can-use-multiple-external-tools-with-chain-of-thought-prompting)


**Original Paper:**[https://arxiv.org/abs/2305.16896](https://arxiv.org/abs/2305.16896)


**By:**[Tatsuro Inaba](https://arxiv.org/search/cs?searchtype=author&query=Inaba%2C%20T) ,[Hirokazu Kiyomaru](https://arxiv.org/search/cs?searchtype=author&query=Kiyomaru%2C%20H) ,[Fei Cheng](https://arxiv.org/search/cs?searchtype=author&query=Cheng%2C%20F) ,[Sadao Kurohashi](https://arxiv.org/search/cs?searchtype=author&query=Kurohashi%2C%20S)


**Abstract:**


> Large language models (LLMs) have achieved impressive performance on various reasoning tasks. To further improve the performance, we propose MultiTool-CoT, a novel framework that leverages chain-of-thought (CoT) prompting to incorporate multiple external tools, such as a calculator and a knowledge retriever, during the reasoning process. We apply MultiTool-CoT to the Task 2 dataset of NumGLUE, which requires both numerical reasoning and domain-specific knowledge. The experiments show that our method significantly outperforms strong baselines and achieves state-of-the-art performance.


---


###


Summary Notes


####


Blog Post: Unlocking Advanced Reasoning in AI with MultiTool-CoT


Artificial intelligence (AI) is becoming increasingly sophisticated, especially in areas requiring complex reasoning.


This involves not just parsing language but also integrating real-world knowledge, performing arithmetic, and processing symbols.


Large Language Models (LLMs) have made significant strides, yet they often falter when faced with specialized knowledge or complex calculations.


This highlights a need for enhanced reasoning capabilities.


To tackle this issue, researchers have turned to integrating external tools with LLMs. Although this approach has shown promise, it's typically been limited to using one tool at a time.


This limitation raises an important question: How can we boost LLMs' reasoning abilities by utilizing multiple external tools at once? The answer lies in a revolutionary framework called MultiTool-CoT.


###


Introducing MultiTool-CoT


MultiTool-CoT emerged from the recognition of existing methods' shortcomings in fully leveraging multiple external tools to aid LLMs in reasoning tasks. This framework represents a significant leap forward.


####


Framework Highlights


MultiTool-CoT is an innovative framework that enhances LLMs by allowing them to use multiple external tools simultaneously for reasoning.


It's based on the Chain-of-Thought (CoT) prompting method, which is inspired by few-shot learning. This technique encourages LLMs to produce intermediate reasoning steps that include cues for invoking specific external tools, enriching the reasoning process.


####


Core Features:


- **Interactive Reasoning** : Enables dynamic use of various tools during the reasoning process.


- **CoT Prompting** : Guides LLMs through logical intermediate steps, making reasoning more transparent.


- **Integrated Outputs** : Seamlessly incorporates external tools' outputs into LLMs’ reasoning, improving result accuracy and depth.


###


Testing MultiTool-CoT's Effectiveness


MultiTool-CoT was rigorously evaluated using the Task 2 dataset of NumGLUE, focusing on numerical reasoning and domain-specific knowledge. It outperformed other methods, achieving an impressive 85.85% accuracy rate. This showcases its potential to revolutionize LLMs' approach to complex reasoning tasks.


####


Applications and Future Directions


MultiTool-CoT's adaptability makes it suitable for a wide array of applications, from improving decision-making in businesses to advancing scientific research. Its design allows for customization to specific needs by integrating various external tools.


####


Future Plans:


The journey doesn't stop with MultiTool-CoT's current success. Future efforts will aim to validate its effectiveness across more tasks and explore its use in complex real-world applications. This includes overcoming current limitations and enhancing its adaptability.


###


Conclusion


MultiTool-CoT represents a significant advancement in enhancing LLMs' reasoning capabilities. By enabling the integration of multiple external tools, it overcomes the limitations of previous approaches and sets the stage for more sophisticated reasoning processes.


Looking forward, MultiTool-CoT's continued development and refinement promise to significantly impact the AI landscape, offering a future where LLMs can tackle complex reasoning tasks with unprecedented accuracy and flexibility.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
