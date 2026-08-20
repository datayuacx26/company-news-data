---
schema_version: "1.0.0"
document_id: "d90465c6241510379a1f44589bee2ef9a6b10a0bb2389b7aa8ee7433427b43bc"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/active-prompting-with-chain-of-thought-for-large-language-models"
published_at: "2023-02-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:971650f73b56db6cb2048f6815f05cede31251c3879cec755198f7b839f5acd3"
---

# Active Prompting with Chain-of-Thought for Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2302.12246](https://arxiv.org/abs/2302.12246)


Blog URL


[blog.athina.ai /active-...e-models](https://blog.athina.ai/active-prompting-with-chain-of-thought-for-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2302.12246](https://arxiv.org/abs/2302.12246)


**By:**[Shizhe Diao](https://arxiv.org/search/cs?searchtype=author&query=Diao%2C%20S) ,[Pengcheng Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20P) ,[Yong Lin](https://arxiv.org/search/cs?searchtype=author&query=Lin%2C%20Y) ,[Tong Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20T)


**Abstract:**


> The increasing scale of large language models (LLMs) brings emergent abilities to various complex tasks requiring reasoning, such as arithmetic and commonsense reasoning. It is known that the effective design of task-specific prompts is critical for LLMs' ability to produce high-quality answers. In particular, an effective approach for complex question-and-answer tasks is example-based prompting with chain-of-thought (CoT) reasoning, which significantly improves the performance of LLMs. However, current CoT methods rely on a fixed set of human-annotated exemplars, which are not necessarily the most effective examples for different tasks. This paper proposes a new method, Active-Prompt, to adapt LLMs to different tasks with task-specific example prompts (annotated with human-designed CoT reasoning). For this purpose, we propose a solution to the key problem of determining which questions are the most important and helpful ones to annotate from a pool of task-specific queries. By borrowing ideas from the related problem of uncertainty-based active learning, we introduce several metrics to characterize the uncertainty so as to select the most uncertain questions for annotation. Experimental results demonstrate the superiority of our proposed method, achieving state-of-the-art on eight complex reasoning tasks. Further analyses of different uncertainty metrics, pool sizes, zero-shot learning, and accuracy-uncertainty relationship demonstrate the effectiveness of our method. Our code will be available at
>
>
> [this https URL](https://github.com/shizhediao/active-prompt)


---


###


Summary Notes


####


Boosting LLM Reasoning with Active-Prompt: A Step Forward


###


Introduction


Large Language Models (LLMs) have been making waves in AI, tackling everything from language processing to solving complex issues.


Yet, they often hit a snag with deep reasoning tasks, critical for advanced AI in business. Enter Active-Prompt, a fresh approach combining Chain-of-Thought (CoT) prompting with active learning to refine how LLMs learn to reason.


This post explores Active-Prompt's method, tests, findings, and its potential to enhance LLM reasoning significantly.


###


How It Works


####


Estimating Uncertainty


Active-Prompt starts by figuring out which questions stump the LLM the most. It does this by asking the model the same question multiple times and checking how much the answers vary, using measures like disagreement and entropy.


This step ensures the focus is on improving areas where the LLM struggles the most.


####


Choosing and Teaching


The method then zeroes in on these tough questions. Human annotators provide detailed CoTs for these, making sure the effort goes where it's needed most.


This not only makes better use of human time but also raises the quality of training prompts.


####


Learning


With these high-quality, hand-picked examples, the LLM gets better at tackling similar reasoning tasks. This approach promises a smarter and more efficient way to boost LLM reasoning skills.


###


Testing the Approach


Active-Prompt was put through its paces with tests on arithmetic, commonsense, and symbolic reasoning. It was measured against other prompting methods using accuracy and how well it could predict its own uncertainty. This thorough testing aimed to solidly assess Active-Prompt's effectiveness.


###


Results


Active-Prompt clearly outperformed traditional methods, especially in complex reasoning areas. Its success stems from focusing on the most uncertain and thus informative questions, significantly boosting reasoning skills.


###


Key Insights


- **Uncertainty Metrics Matter** : Not all ways of measuring uncertainty are equal. Disagreement and entropy were the most effective, pointing the way to fine-tune Active-Prompt.


- **Quality Over Quantity** : The study highlighted that well-placed human input could greatly improve LLMs. It's not about how much help, but how relevant it is.


- **Wide Application** : Active-Prompt isn't limited by LLM types or specific tasks, making it a versatile tool for enhancing AI across various enterprise applications.


###


Conclusion


Active-Prompt marks a notable advance in empowering LLMs with better reasoning skills. By smartly combining CoT prompting with active learning, it not only elevates LLM performance but also uses human effort wisely.


As AI grows, methods like Active-Prompt will be key in developing smarter, more efficient AI systems for business use.


Future research will likely explore new uncertainty measures, dive deeper into what makes annotations effective, and apply these lessons to even tougher reasoning challenges, broadening AI's capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
