---
schema_version: "1.0.0"
document_id: "516113400d3d33664d3f8bf4a523d5d5f191b7f048be40cbc51060576fcf78d2"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/safeguarding-crowdsourcing-surveys-from-chatgpt-with-prompt-injection"
published_at: "2023-06-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:6aba3f52f6ce65fb95730772b71d00440ae0f736e96d604f9352dfdfde4756dd"
---

# Safeguarding Crowdsourcing Surveys from ChatGPT with Prompt Injection

Do not index


Original Paper


[https://arxiv.org/abs/2306.08833](https://arxiv.org/abs/2306.08833)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2306.08833](https://arxiv.org/abs/2306.08833)


**By:**[Chaofan Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20C) ,[Samuel Kernan Freire](https://arxiv.org/search/cs?searchtype=author&query=Freire%2C%20S%20K) ,[Mo Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20M) ,[Jing Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20J) ,[Jorge Goncalves](https://arxiv.org/search/cs?searchtype=author&query=Goncalves%2C%20J) ,[Vassilis Kostakos](https://arxiv.org/search/cs?searchtype=author&query=Kostakos%2C%20V) ,[Zhanna Sarsenbayeva](https://arxiv.org/search/cs?searchtype=author&query=Sarsenbayeva%2C%20Z) ,[Christina Schneegass](https://arxiv.org/search/cs?searchtype=author&query=Schneegass%2C%20C) ,[Alessandro Bozzon](https://arxiv.org/search/cs?searchtype=author&query=Bozzon%2C%20A) ,[Evangelos Niforatos](https://arxiv.org/search/cs?searchtype=author&query=Niforatos%2C%20E)


**Abstract:**


> ChatGPT and other large language models (LLMs) have proven useful in crowdsourcing tasks, where they can effectively annotate machine learning training data. However, this means that they also have the potential for misuse, specifically to automatically answer surveys. LLMs can potentially circumvent quality assurance measures, thereby threatening the integrity of methodologies that rely on crowdsourcing surveys. In this paper, we propose a mechanism to detect LLM-generated responses to surveys. The mechanism uses "prompt injection", such as directions that can mislead LLMs into giving predictable responses. We evaluate our technique against a range of question scenarios, types, and positions, and find that it can reliably detect LLM-generated responses with more than 93% effectiveness. We also provide an open-source software to help survey designers use our technique to detect LLM responses. Our work is a step in ensuring that survey methodologies remain rigorous vis-a-vis LLMs.


---


###


Summary Notes


##


Protecting Crowdsourcing Surveys Against ChatGPT with Prompt Injection


In today's AI-driven age, Large Language Models (LLMs) like ChatGPT are transforming our interaction with technology by providing responses that closely mimic human conversation. However, this innovation brings challenges, particularly to the accuracy of crowdsourcing surveys. LLMs can produce answers that seem human, risking the integrity of survey data.


This blog post delves into the issue of LLMs in crowdsourcing and introduces "prompt injection," a method to detect and neutralize the influence of LLM-generated responses, ensuring data reliability.


###


The Issue with LLMs in Surveys


Crowdsourcing surveys are vital for gathering diverse insights affordably and efficiently. But the data's quality is crucial. The rise of LLMs that can create text indistinguishable from human writing threatens this quality.


These models can circumvent traditional checks, potentially contaminating the data pool and skewing research outcomes.


###


What is Prompt Injection?


Prompt injection is an innovative strategy to combat LLM interference in surveys. It involves embedding special cues within survey questions that prompt predictable LLM responses, thereby distinguishing human from machine-generated answers. This approach has proven over 93% effective in identifying LLM responses.


####


How It Works


- **Embedding Cues** : Incorporating unique cues into survey texts triggers specific responses from LLMs, leaving human responses unaffected.


- **Testing Across Conditions** : Its effectiveness has been validated across various survey types, confirming its reliability and adaptability.


###


Implementing Prompt Injection


An open-source tool has been developed to help survey creators use prompt injection. This tool eases the process of designing and testing custom prompts, offering features like:


- **Custom Prompt Design** : Enables the crafting of prompts tailored to specific survey needs.


- **Effectiveness Testing** : Allows for the evaluation of prompt success in spotting LLM-generated answers.


###


Ethical Considerations and Limitations


Despite its potential, prompt injection carries ethical concerns and limitations. There are questions about its impact on data quality and the possibility of misuse.


Moreover, the continuous evolution of LLMs means prompt injection must adapt to remain effective.


###


Conclusion


The emergence of LLMs like ChatGPT poses a real challenge to the validity of crowdsourcing survey data. Prompt injection offers a viable solution, ensuring data collected remains trustworthy.


As we advance, refining this technique and finding additional safeguards against AI's influence in data collection will be key.


Prompt injection marks a crucial step in ensuring AI advancements bolster rather than compromise the quality of crowdsourced information.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
