---
schema_version: "1.0.0"
document_id: "aa1108dadfe5052104912f76cb753a74de87ef3e6ec9d1760fe9d7d0e8ba712d"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/enhancing-large-language-models-against-inductive-instructions-with-dual-critique-prompting"
published_at: "2023-05-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:b85851aafcff4bf075552a49072a050851fbfc57d4e0a7e71f1c3817b69c12cb"
---

# Enhancing Large Language Models Against Inductive Instructions with Dual-critique Prompting

Do not index


Original Paper


[https://arxiv.org/abs/2305.13733](https://arxiv.org/abs/2305.13733)


Blog URL


[blog.athina.ai /enhanci...rompting](https://blog.athina.ai/enhancing-large-language-models-against-inductive-instructions-with-dual-critique-prompting)


**Original Paper:**[https://arxiv.org/abs/2305.13733](https://arxiv.org/abs/2305.13733)


**By:**[Rui Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20R) ,[Hongru Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20H) ,[Fei Mi](https://arxiv.org/search/cs?searchtype=author&query=Mi%2C%20F) ,[Yi Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20Y) ,[Boyang Xue](https://arxiv.org/search/cs?searchtype=author&query=Xue%2C%20B) ,[Kam-Fai Wong](https://arxiv.org/search/cs?searchtype=author&query=Wong%2C%20K) ,[Ruifeng Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20R)


**Abstract:**


> Numerous works are proposed to align large language models (LLMs) with human intents to better fulfill instructions, ensuring they are trustful and helpful. Nevertheless, some human instructions are often malicious or misleading and following them will lead to untruthful and unsafe responses. Previous work rarely focused on understanding how LLMs manage instructions based on counterfactual premises, referred to here as \\textit{inductive instructions}, which may stem from users' false beliefs or malicious intents. In this paper, we aim to reveal the behaviors of LLMs towards \\textit{inductive instructions} and enhance their truthfulness and helpfulness accordingly. Specifically, we first introduce a benchmark of \\underline{\\textbf{Indu}}ctive {In\\underline{\\textbf{st}}ruct}ions (\\textsc{\\textbf{INDust}}), where the false knowledge is incorporated into instructions in multiple different styles. After extensive human and automatic evaluations, we uncovered a universal vulnerability among LLMs in processing inductive instructions. Additionally, we identified that different inductive styles affect the models' ability to identify the same underlying errors, and the complexity of the underlying assumptions also influences the model's performance. Motivated by these results, we propose \\textsc{Dual-critique} prompting to improve LLM robustness against inductive instructions. Our experiments demonstrate that \\textsc{Dual-critique} prompting significantly bolsters the robustness of a diverse array of LLMs, even when confronted with varying degrees of inductive instruction complexity and differing inductive styles.


---


###


Summary Notes


####


Enhancing Large Language Models Against Misleading Prompts


In the rapidly advancing field of AI, Large Language Models (LLMs) stand out as key drivers of innovation. Yet, their vulnerability to misleading or incorrect prompts—known as inductive instructions—poses a significant challenge. These issues can cause LLMs to generate harmful or false content, impacting their reliability. This post explores strategies to improve LLM robustness against such challenges, focusing on a novel solution to enhance their reliability.


###


Understanding the Challenge


Inductive instructions, driven by user misunderstandings or malicious intent, undermine LLM output integrity. Despite advancements in AI, addressing these misleading instructions has remained a hurdle. A new benchmark, INDust, and a solution strategy, Dual-critique prompting, are introduced to combat this issue effectively.


###


Introducing the INDust Benchmark


INDust categorizes misleading instructions into three types:


- Fact-Checking Instructions (FCI)


- Questions based on False Premises (QFP)


- Creative Instructions based on False Premises (CIFP)


This benchmark, building on existing datasets, measures how well LLMs handle misinformation and complex prompts, crucial for enhancing LLM robustness.


###


Key Performance Metrics


- **Truthfulness** : Evaluates LLMs' ability to correct or identify false premises.


- **Helpfulness** : Measures the capacity of LLMs to provide constructive feedback on user misconceptions.


###


Insights from INDust


Evaluation highlights a universal LLM vulnerability to complex, misleading instructions, underscoring the need for sophisticated solutions.


###


Dual-critique Prompting Solution


This method involves two critique components:


- **User-critique** : Identifies misinformation in user prompts.


- **Self-critique** : Ensures LLM responses are accurate and safe.


####


Features:


- Shows significant robustness improvements.


- Includes simpler Single-step Dual-critique (SDual-critique) and more comprehensive Multi-step Dual-critique (MDual-critique), with a preference for the former due to its straightforwardness.


###


Experimental Findings


Dual-critique prompting consistently boosts LLM truthfulness and helpfulness, favoring SDual-critique for its simplicity and effectiveness.


###


Potential and Challenges


This approach, requiring no extra training, significantly enhances LLM responses to misleading instructions. However, its ability to fully mimic real-world complexities and the risk of misuse remain concerns.


###


Ethical Framework


The development prioritizes creating safer, more truthful LLMs, with ethical considerations in annotation work to ensure fairness and responsibility.


###


Wrap-up


LLMs' susceptibility to misleading instructions is a major barrier to their potential. The INDust benchmark and Dual-critique prompting present a significant leap towards more dependable LLMs. This advancement is crucial for AI engineers aiming to deploy ethical AI technologies that resonate with societal values. Although the journey to robust LLMs is ongoing, tools like Dual-critique prompting equip us to better address these challenges.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
