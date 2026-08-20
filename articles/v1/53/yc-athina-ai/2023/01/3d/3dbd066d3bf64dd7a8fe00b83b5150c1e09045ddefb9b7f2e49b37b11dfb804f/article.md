---
schema_version: "1.0.0"
document_id: "3dbd066d3bf64dd7a8fe00b83b5150c1e09045ddefb9b7f2e49b37b11dfb804f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/language-models-are-greedy-reasoners-a-systematic-formal-analysis-of-chain-of-thought"
published_at: "2023-01-26T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:ea6e8afa7eeb8a7fce778abd768cd3381b7576722f9342c7dcd1a7e9c223c596"
---

# Language Models Are Greedy Reasoners: A Systematic Formal Analysis of Chain-of-Thought

Do not index


Original Paper


[https://arxiv.org/abs/2210.01240v3](https://arxiv.org/abs/2210.01240v3)


Blog URL


[blog.athina.ai /languag...-thought](https://blog.athina.ai/language-models-are-greedy-reasoners-a-systematic-formal-analysis-of-chain-of-thought)


**Original Paper:**[https://arxiv.org/abs/2210.01240v3](https://arxiv.org/abs/2210.01240v3)


**By:**[Abulhair Saparov](https://arxiv.org/search/cs?searchtype=author&query=Saparov%2C%20A) ,[He He](https://arxiv.org/search/cs?searchtype=author&query=He%2C%20H)


**Abstract:**


> Large language models (LLMs) have shown remarkable reasoning capabilities given chain-of-thought prompts (examples with intermediate reasoning steps). Existing benchmarks measure reasoning ability indirectly, by evaluating accuracy on downstream tasks such as mathematical reasoning. However, it is unclear how these models obtain the answers and whether they rely on simple heuristics rather than the generated chain-of-thought. To enable systematic exploration of the reasoning ability of LLMs, we present a new synthetic question-answering dataset called PrOntoQA, where each example is generated from a synthetic world model represented in first-order logic. This allows us to parse the generated chain-of-thought into symbolic proofs for formal analysis. Our analysis on InstructGPT and GPT-3 shows that LLMs are quite capable of making correct individual deduction steps, and so are generally capable of reasoning, even in fictional contexts. However, they have difficulty with proof planning: When multiple valid deduction steps are available, they are not able to systematically explore the different options.


---


###


Summary Notes


####


Unpacking the Reasoning Skills of Language Models: A Closer Look


Exploring the reasoning abilities of Artificial Intelligence (AI), especially in Large Language Models (LLMs), is crucial for making AI smarter.


The introduction of Chain-of-Thought (CoT) prompting has revolutionized how we understand AI's thought process. This blog post examines a thorough study on LLMs' reasoning abilities, focusing on the PRONTO QA dataset and its role in AI reasoning.


####


Introduction: Why Reasoning Matters in AI


Reasoning is a fundamental aspect of both human and artificial intelligence. For AI, reasoning is tested through complex question-answering tasks.


The latest breakthrough, the Chain-of-Thought (CoT) prompting, encourages LLMs to outline their thought process step by step, improving their problem-solving skills and giving us insight into their reasoning.


####


Methodology: Analyzing AI Reasoning Using PRONTO QA


The centerpiece of this study is the PRONTO QA dataset, crafted to break down CoT into symbolic proofs for a deeper look into AI's reasoning steps. Here's how it was done:


- **Creating the PRONTO QA Dataset:**


- **Formal Analysis:**


####


Evaluation and Discoveries


The study put models like INSTRUCT GPT and GPT-3 under the microscope, assessing their ability to generate logical answers and reasoning paths. Key discoveries include:


- **Reasoning Steps:** While LLMs can generate accurate reasoning steps individually, they struggle with planning out complex proofs.


- **Ontology Type:** The model's performance changes based on the ontology, hinting at a dependency on the data it was trained on.


- **Generalization:** The same issues were observed across different setups, pointing to a broader problem in AI reasoning.


####


Identifying Hurdles in AI Reasoning


The study pinpoints proof planning as a major challenge, especially when multiple reasoning paths are possible.


This highlights a current limitation of LLMs in tackling intricate reasoning tasks that involve navigating through several logical steps.


####


Building on Previous Research


This research builds upon and expands previous studies, comparing its results with other datasets like PROOF WRITER and FOLIO.


Future suggestions include improving proof planning and exploring reasoning across various logical frameworks.


####


Contributions of the Study


This analysis offers significant insights into AI reasoning:


- **Innovative Dataset and Method:** The PRONTO QA dataset provides a new way to examine LLMs' reasoning abilities.


- **Highlighting Weaknesses:** It identifies key challenges, especially in proof planning and step selection.


- **Laying Groundwork for Future Exploration:** The findings serve as a basis for further research into boosting AI's reasoning skills.


####


Conclusion: Advancing AI Reasoning


This study meticulously explores LLMs' reasoning capabilities, showcasing both strengths and areas for improvement. The PRONTO QA dataset stands out as an essential tool for future research aimed at unlocking AI's full reasoning potential. As we strive to enhance AI's cognitive abilities, focusing on improving its reasoning is vital.


This thorough analysis not only deepens our understanding of AI's reasoning processes but also paves the way for significant advancements in AI capabilities.


By tackling identified challenges and exploring new methods, we can heighten AI's intelligence and adaptability, leading to more sophisticated and dependable AI applications in the future.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
