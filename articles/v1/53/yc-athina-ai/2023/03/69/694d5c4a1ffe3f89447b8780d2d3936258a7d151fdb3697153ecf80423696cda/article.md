---
schema_version: "1.0.0"
document_id: "694d5c4a1ffe3f89447b8780d2d3936258a7d151fdb3697153ecf80423696cda"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/cotever-chain-of-thought-prompting-annotation-toolkit-for-explanation-verification"
published_at: "2023-03-07T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:283c31b71adfdd444fe8aad74de38684f7af519c8175c79da6246dfc0fe9e47b"
---

# CoTEVer: Chain of Thought Prompting Annotation Toolkit for Explanation Verification

Do not index


Original Paper


[https://arxiv.org/abs/2303.03628](https://arxiv.org/abs/2303.03628)


Blog URL


[blog.athina.ai /cotever...fication](https://blog.athina.ai/cotever-chain-of-thought-prompting-annotation-toolkit-for-explanation-verification)


**Original Paper:**[https://arxiv.org/abs/2303.03628](https://arxiv.org/abs/2303.03628)


**By:**[Seungone Kim](https://arxiv.org/search/cs?searchtype=author&query=Kim%2C%20S) ,[Se June Joo](https://arxiv.org/search/cs?searchtype=author&query=Joo%2C%20S%20J) ,[Yul Jang](https://arxiv.org/search/cs?searchtype=author&query=Jang%2C%20Y) ,[Hyungjoo Chae](https://arxiv.org/search/cs?searchtype=author&query=Chae%2C%20H) ,[Jinyoung Yeo](https://arxiv.org/search/cs?searchtype=author&query=Yeo%2C%20J)


**Abstract:**


> Chain-of-thought (CoT) prompting enables large language models (LLMs) to solve complex reasoning tasks by generating an explanation before the final prediction. Despite it's promising ability, a critical downside of CoT prompting is that the performance is greatly affected by the factuality of the generated explanation. To improve the correctness of the explanations, fine-tuning language models with explanation data is needed. However, there exists only a few datasets that can be used for such approaches, and no data collection tool for building them. Thus, we introduce CoTEVer, a tool-kit for annotating the factual correctness of generated explanations and collecting revision data of wrong explanations. Furthermore, we suggest several use cases where the data collected with CoTEVer can be utilized for enhancing the faithfulness of explanations. Our toolkit is publicly available at
>
>
> [this https URL](https://github.com/SeungoneKim/CoTEVer)


---


###


Summary Notes


####


Enhancing AI Reasoning with CoTEVer: Simplifying Verification for Chain of Thought Prompting


The development of Artificial Intelligence (AI) is rapidly advancing, focusing on enabling large language models (LLMs) to reason and explain complex issues similarly to humans.


Chain of Thought (CoT) prompting is a cutting-edge method improving these models' reasoning abilities. Yet, ensuring these explanations are accurate remains a challenge.


This is where CoTEVer, a toolkit designed for verifying the accuracy of these machine-generated explanations, comes into play.


####


Introducing CoTEVer Toolkit


CoTEVer, developed by researchers from KAIST AI and Yonsei University, is tailored to enhance the dependability of explanations provided by LLMs. It's especially useful for AI engineers in businesses due to its unique features.


####


Key Features:


- **Evidence-Based Verification:** CoTEVer enables the comparison of AI explanations against evidence from the web, ensuring both logical and factual correctness.


- **Gathering Alternate Explanations:** It also helps collect alternative explanations when inaccuracies are found, aiding in the continuous improvement of LLMs.


- **Support for Various CoT Prompts:** The toolkit accommodates different CoT prompts, making it versatile for numerous reasoning tasks.


####


How CoTEVer Works


####


Generating and Verifying Explanations:


Using GPT-3, CoTEVer generates explanations for queries through a "Self Ask" method, breaking down complex answers into simpler sub-questions and answers. This method makes verifying explanations more efficient.


####


Finding and Using Evidence:


For explanation verification, CoTEVer finds and ranks relevant documents, presenting the most pertinent evidence to reviewers first. This streamlined approach aids in the quick and accurate revision of AI-generated explanations.


####


The Importance of CoTEVer


**For AI Engineers:** CoTEVer is a vital tool for enhancing the reasoning abilities of LLMs, providing a systematic way to ensure explanations are both coherent and evidence-backed.


**For the AI Community:** It's a rich resource for research, offering insights into improving explanation robustness and reliability in AI models, pushing towards more trustworthy AI decision-making.


####


Conclusion: Why CoTEVer Stands Out


CoTEVer bridges an essential gap in AI development, offering a reliable method for refining LLM-generated explanations.


Its structured, evidence-based approach marks a significant step towards more accurate AI reasoning.


The toolkit is open for use and further development, offering AI engineers a promising tool to enhance their models' reasoning capabilities.


We encourage you to explore CoTEVer and join in evolving it towards creating understandable and trustworthy AI.


Start with CoTEVer at[https://github.com/SeungoneKim/CoTEVer](https://github.com/SeungoneKim/CoTEVer) .


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
