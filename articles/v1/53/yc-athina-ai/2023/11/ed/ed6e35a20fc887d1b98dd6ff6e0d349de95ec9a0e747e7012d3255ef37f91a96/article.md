---
schema_version: "1.0.0"
document_id: "ed6e35a20fc887d1b98dd6ff6e0d349de95ec9a0e747e7012d3255ef37f91a96"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/testing-llms-on-code-generation-with-varying-levels-of-prompt-specificity"
published_at: "2023-11-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:09.893530+00:00"
content_hash: "sha256:6ebc6761070704b92b1131a9080f8c40ba799a595b8293e2f4cb527d90c43cd3"
---

# Testing LLMs on Code Generation with Varying Levels of Prompt Specificity

Do not index


Original Paper


[https://arxiv.org/abs/2311.07599](https://arxiv.org/abs/2311.07599)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2311.07599](https://arxiv.org/abs/2311.07599)


**By:**[Lincoln Murr](https://arxiv.org/search/cs?searchtype=author&query=Murr%2C%20L) ,[Morgan Grainger](https://arxiv.org/search/cs?searchtype=author&query=Grainger%2C%20M) ,[David Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20D)


**Abstract:**


> Large language models (LLMs) have demonstrated unparalleled prowess in mimicking human-like text generation and processing. Among the myriad of applications that benefit from LLMs, automated code generation is increasingly promising. The potential to transform natural language prompts into executable code promises a major shift in software development practices and paves the way for significant reductions in manual coding efforts and the likelihood of human-induced errors. This paper reports the results of a study that evaluates the performance of various LLMs, such as Bard, ChatGPT-3.5, ChatGPT-4, and Claude-2, in generating Python for coding problems. We focus on how levels of prompt specificity impact the accuracy, time efficiency, and space efficiency of the generated code. A benchmark of 104 coding problems, each with four types of prompts with varying degrees of tests and specificity, was employed to examine these aspects comprehensively. Our results indicate significant variations in performance across different LLMs and prompt types, and its key contribution is to reveal the ideal prompting strategy for creating accurate Python functions. This study lays the groundwork for further research in LLM capabilities and suggests practical implications for utilizing LLMs in automated code generation tasks and test-driven development.


---


###


Summary Notes


####


The Effect of Prompt Specificity on Code Generation by Large Language Models


In the fast-paced realm of artificial intelligence (AI), Large Language Models (LLMs) such as GPT-3.5, GPT-4, and Claude-2 are revolutionizing how we approach software development.


Their ability to generate code has the potential to significantly speed up development tasks. However, the success of these models in producing accurate and efficient code largely depends on the prompts they receive.


This post explores the art of prompt engineering for LLMs in code generation, drawing on findings from a detailed study at Vanderbilt University.


####


Understanding Prompt Engineering


Prompt engineering is the practice of crafting inputs ("prompts") for LLMs to produce specific desired outputs. In code generation, the detail in these prompts is crucial.


A detailed prompt can lead the model to generate more accurate code, while a vague prompt might result in irrelevant or incorrect code.


####


Why Prompt Specificity Matters


The study "Testing LLMs on Code Generation with Varying Levels of Prompt Specificity" examined how different prompt types affect LLMs’ ability to generate Python code for various programming tasks. Researchers used four types of prompts:


- **Prompt Only:** Just the problem statement.


- **Prompt with Tests:** Problem statement plus example test cases.


- **Prompt Tests Only:** Only test cases, requiring the model to deduce the problem.


- **Prompt Generic Tests:** Test cases with unnamed functions, to test model flexibility.


####


Study Highlights


The study's analysis provided several key insights:


- **Accuracy Boost from Test Cases:** Including test cases in prompts significantly enhanced the code’s accuracy and reliability.


- **Handling Ambiguity:** LLMs varied in their ability to handle ambiguous prompts, especially noticeable in the "Prompt Generic Tests" category, where performance often dropped.


- **Differences Among Models:** GPT-3.5 and GPT-4, in particular, demonstrated strong adaptability and reasoning across different prompts.


####


Tips for AI Engineers


For AI engineers, these findings stress the importance of thoughtful prompt engineering. Some practical advice includes:


- **Include Test Cases:** Add example test cases to your prompts to steer the LLM towards accurate solutions.


- **Aim for Specificity:** Use clear and specific problem statements to minimize ambiguity.


- **Try Different Prompts:** Experiment with various prompt styles to find the most effective type for your coding challenge.


####


Looking Forward


This study paves the way for future research in prompt engineering, such as testing other LLMs, using different programming languages, and exploring more complex prompts. Including multi-modal inputs could also enhance LLMs’ code generation capabilities.


####


Conclusion


The research "Testing LLMs on Code Generation with Varying Levels of Prompt Specificity" sheds light on the crucial role of prompt specificity in optimizing LLMs for code generation.


Through strategic prompt engineering, AI engineers can greatly improve the code's accuracy and efficiency, leading to more advanced software development methods.


The study's findings and resources are shared on GitHub, inviting further exploration and contribution to the development of prompt engineering in AI.


This research not only demonstrates the potential of LLMs to improve software development but also highlights prompt engineering as an essential skill for AI engineers seeking to maximize these models' capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
