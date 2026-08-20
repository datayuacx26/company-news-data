---
schema_version: "1.0.0"
document_id: "f3926ed7217b6e34cf0e0af1bdaa26bdfe31f0e553ce45a17fc5ec7312cdc7d8"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/llm-coding-benchmarks"
published_at: "2025-07-11T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:84965992d2bac1fa8eb732a2a773f833f12cd0db770565c8279e10fb30ea286e"
---

# 15 LLM coding benchmarks

LLMs are rapidly becoming more advanced at coding tasks, driving the development of real-world applications, from coding co-pilots to developer productivity tools to automated code reviewers. But as model capabilities grow, so does the need to measure their performance.


This blog highlights 15 LLM coding benchmarks designed to evaluate and compare how different models perform on various coding tasks, including code completion, snippet generation, debugging, and more.


**Want more examples of LLM benchmarks?** We put together[database](https://www.evidentlyai.com/llm-evaluation-benchmarks-datasets) of 250+ LLM benchmarks and datasets you can use to evaluate the performance of language models.


## HumanEval


[HumanEval](https://github.com/openai/human-eval) measures how well LLMs can generate code. It tests their ability to understand programming tasks and produce syntactically correct and functionally accurate pieces of code based on given prompts.


The dataset includes 164 programming tasks and unit tests that automatically check the model-generated code against expected results, simulating how a human developer would validate their work. A model’s solution must pass all provided test cases for a given problem to be considered correct.


> Paper:[Evaluating Large Language Models Trained on Code](https://arxiv.org/abs/2107.03374) by Chen et al. (2021)
> Dataset:[HumanEval dataset](https://github.com/openai/human-eval)


Example problems from the HumanEval dataset. Credit:[Evaluating Large Language Models Trained on Code](https://arxiv.org/abs/2107.03374)


## MBPP (Mostly Basic Programming Problems)


[Mostly Basic Programming Problems (MBPP)](https://huggingface.co/datasets/google-research-datasets/mbpp) evaluates how well LLMs can generate short Python programs from natural language descriptions. It includes 974 entry-level tasks covering common programming concepts like list manipulation, string operations, loops, conditionals, and basic algorithms. Each task provides a clear description, an example solution, and a set of test cases to validate the LLM's output.


> Paper:[Program Synthesis with Large Language Models](https://arxiv.org/abs/2108.07732) by Austin et al. (2021)
> Dataset:[MBPP dataset](https://huggingface.co/datasets/google-research-datasets/mbpp)


Example problems and generated solutions from the MBPP dataset.Credit:[Program Synthesis with Large Language Models](https://arxiv.org/abs/2108.07732)


## SWE-bench


[SWE-bench (Software Engineering Benchmark)](https://www.swebench.com/) assesses the ability of LLMs to tackle real-world software issues sourced from GitHub. It includes more than 2200 issues and their corresponding pull requests from 12 widely used Python repositories. The benchmark challenges models to generate patches that fix the issues based on the provided codebase and issue description. Unlike simpler code generation tasks, SWE-bench requires models to handle long contexts, perform complex reasoning, and operate within execution environments.


> Paper:[SWE-bench: Can Language Models Resolve Real-World GitHub Issues?](https://arxiv.org/abs/2310.06770) by Jimenez et al. (2023)
> Dataset:[SWE-bench dataset](https://github.com/princeton-nlp/SWE-bench)


How SWE-bench works. Credit:[SWE-bench: Can Language Models Resolve Real-World GitHub Issues?](https://arxiv.org/abs/2310.06770)


## CodeXGLUE


[CodeXGLUE](https://github.com/microsoft/CodeXGLUE) is a benchmark dataset for program understanding and code generation. It includes 14 datasets, 10 diversified programming tasks, and a platform for model evaluation and comparison. The tasks include clone detection, defect detection, cloze test, code completion, code translation, code search, code repair, text-to-code generation, code summarization, and documentation translation.


The benchmark's creators also provide three baseline models: a BERT-style model for program understanding problems, a GPT-style model for completion and generation problems, and an Encoder-Decoder framework that tackles sequence-to-sequence generation problems.


> Paper:[CodeXGLUE: A Machine Learning Benchmark Dataset for Code Understanding and Generation](https://arxiv.org/abs/2102.04664) by Shuai Lu et al. (2021)
> Dataset:[CodeXGLUE dataset](https://huggingface.co/datasets?search=code_x_glue)


CodeXGLUE’s datasets and tasks description. Source:[CodeXGLUE: A Machine Learning Benchmark Dataset for Code Understanding and Generation](https://arxiv.org/abs/2102.04664)


## DS-1000


[DS-1000](https://ds1000-code-gen.github.io/) is a code generation benchmark that focuses on data science problems. It contains 1000 coding challenges sourced from 451 StackOverflow questions. The tasks span seven popular Python libraries, including NumPy, Pandas, TensorFlow, PyTorch, and scikit‑learn.


Example tasks include realistic operations like data manipulation (e.g., Pandas DataFrame transforms) and machine learning tasks (e.g., training a model with scikit‑learn or PyTorch). Each completed task is run against test cases to check functional correctness and against constraints on API usage to make sure the generated code uses intended library functions.


> Paper:[DS-1000: A Natural and Reliable Benchmark for Data Science Code Generation](https://arxiv.org/abs/2211.11501) by Lai et al. (2022)
> Dataset:[DS-1000 dataset](https://huggingface.co/datasets/xlangai/DS-1000)


DS-1000 evaluation flow. Source:[DS-1000: A Natural and Reliable Benchmark for Data Science Code Generation](https://arxiv.org/abs/2211.11501)


## APPS (Automated Programming Progress Standard)


[APPS](https://github.com/hendrycks/apps) is a code generation benchmark that measures the ability of LLMs to generate “satisfactory Python code” based on an arbitrary natural language specification. The benchmark includes 10,000 problems, collected from open-access coding websites like Codeforces or Kattis. The task difficulty ranges from one-line solutions to substantial algorithmic challenges. Each problem is accompanied by test cases and ground-truth solutions to evaluate the generated code.


> Paper:[Measuring Coding Challenge Competence With APPS](https://arxiv.org/abs/2105.09938) by Hendrycks et al. (2021)
> Dataset:[APPS dataset](https://huggingface.co/datasets/codeparrot/apps)


An example problem from APPS with possible generated code and example test cases. Source:[Measuring Coding Challenge Competence With APPS](https://arxiv.org/abs/2105.09938)


## EvalPlus


[EvalPlus](https://github.com/evalplus/evalplus) is an evaluation framework that assesses the functional correctness of LLM-synthesized code. It extends the test cases of the popular HumanEval benchmark by 80x and the MBPP benchmark by 35x.


EvalPlus augments the evaluation dataset with large amounts of test cases produced by an automatic test input generator. It uses ChatGPT to generate a set of seed inputs for later mutation. The generator randomly selects a seed from a seed pool of ChatGPT-generated inputs and mutates it to create a new input. If the new input meets the requirements, it is added to the seed pool, and the process repeats.


> Paper:[Is Your Code Generated by ChatGPT Really Correct? Rigorous Evaluation of Large Language Models for Code Generation](https://arxiv.org/abs/2305.01210) by Liu et al. (2023)
> Dataset:[EvalPlus dataset](https://github.com/evalplus/evalplus/tree/master/evalplus/data)


Overview of EvalPlus. Source:[Is Your Code Generated by ChatGPT Really Correct? Rigorous Evaluation of Large Language Models for Code Generation](https://arxiv.org/abs/2305.01210)


## CrossCodeEval


[CrossCodeEval](https://crosscodeeval.github.io/) is a multilingual benchmark that tests LLMs' ability to perform cross-file code completion. Unlike popular benchmarks like HumnEval or MBPP, it evaluates models on completing code not just within a single file but across a project — capturing dependencies and modularity of real-world coding tasks.


The dataset is built on a set of GitHub repositories in four popular programming languages: Python, Java, TypeScript, and C#. The benchmark authors employ static analysis to extract code completion tasks that specifically require cross-file context to solve accurately.


> Paper:[CrossCodeEval: A Diverse and Multilingual Benchmark for Cross-File Code Completion](https://arxiv.org/abs/2310.11248) by Ding et al. (2023)
> Dataset:[CrossCodeEval dataset](https://github.com/amazon-science/cceval/tree/main/data)


How the CrossCodeEval works. Source:[CrossCodeEval: A Diverse and Multilingual Benchmark for Cross-File Code Completion](https://arxiv.org/abs/2310.11248)


## Repobench


[RepoBench](https://github.com/Leolty/repobench) evaluates LLMs on repository-level code auto-completion tasks. It consists of three interconnected evaluation tasks:


- **RepoBench-R (Retrieval)** assesses the model's ability to retrieve relevant code snippets from other files to provide necessary context for code completion.
- **RepoBench-C (Code Completion)** evaluates the model's capability to predict the next line of code using both in-file and cross-file contexts.
- **RepoBench-P (Pipeline)** combines retrieval and code completion tasks to test the model's performance in handling complex scenarios that require both retrieving context and generating appropriate code.


The tasks are derived from GitHub repositories and reflect real-world programming challenges where understanding and integrating information across multiple files is essential. The benchmark supports both Python and Java.


> Paper:[RepoBench: Benchmarking Repository-Level Code Auto-Completion Systems](https://arxiv.org/abs/2306.03091) by Liu et al. (2023)
> Dataset:[RepoBench-R](https://huggingface.co/datasets/tianyang/repobench-r) ,[RepoBench-C](https://huggingface.co/datasets/tianyang/repobench-c) ,[RepoBench-P](https://huggingface.co/datasets/tianyang/repobench-p)


Construction of a prompt for repository-level cross-file code completion. Source:[RepoBench: Benchmarking Repository-Level Code Auto-Completion Systems](https://arxiv.org/abs/2306.03091)


## Code Lingua


[Code Lingua](https://github.com/codetlingua/codetlingua) evaluates LLMs in programming language translation. It compares models' abilities to understand what the code implements in the source language and translate the same semantics into the target language. For example, converting a function from Java to Python or from C++ to Go. The benchmark also tracks bugs introduced or fixed during translation, assessing semantic fidelity and robustness .


Code Lingua incorporates some commonly used datasets like CodeNet and Avatar and consists of 1700 code samples in five languages – C, C++, Go, Java, and Python – with more than 10,000 tests, over 43,000 translations, 1748 bug labels, and 1365 bug-fix pairs.


> Paper:[Lost in Translation: A Study of Bugs Introduced by Large Language Models while Translating Code](https://arxiv.org/abs/2308.03109) by Pan et al. (2023)
> Dataset:[Code Lingua dataset](https://huggingface.co/iidai)


Taxonomy of bugs introduced while translating code using LLM. Source:[Lost in Translation: A Study of Bugs Introduced by Large Language Models while Translating Code](https://arxiv.org/abs/2308.03109)


## ClassEval


[ClassEval](https://github.com/FudanSELab/ClassEval) is a manually constructed benchmark that measures how well LLMs can generate full classes of code. It consists of 100 class-level Python coding tasks covering over 400 methods. The tasks are designed to have dependencies such as library dependencies, field dependencies, or method dependencies – reflecting real-world software engineering scenarios where code isn’t isolated functions but classes.


Each coding task consists of an input description for the target class, a test suite for verifying the correctness of the generated code, and a canonical solution that acts as a reference implementation of the target class.


> Paper:[ClassEval: A Manually-Crafted Benchmark for Evaluating LLMs on Class-level Code Generation](https://arxiv.org/abs/2308.01861) by Du et al. (2023)
> Dataset:[ClassEval dataset](https://huggingface.co/datasets/FudanSELab/ClassEval)


*Task topics in the ClassEval benchmark. Credit:*[ClassEval: A Manually-Crafted Benchmark for Evaluating LLMs on Class-level Code Generation](https://arxiv.org/abs/2308.01861)


## LiveCodeBench


[LiveCodeBench](https://livecodebench.github.io/) is a benchmark that evaluates the coding abilities of LLMs on 400 problems from three competition platforms: LeetCode, AtCoder, and CodeForces. The coding problems are updated over time to reduce the risk of data contamination.


Beyond code generation, the benchmark also focuses on a broader range tasks, such as self-repair, code execution, and test output prediction. Each problem is annotated with a release date, so one can evaluate the model’s performance on tasks released after the model’s training cutoff to see how well the model generalizes to unseen problems.


> Paper:[LiveCodeBench: Holistic and Contamination Free Evaluation of Large Language Models for Code](https://arxiv.org/abs/2403.07974) by Jain et al. (2024)
> Dataset:[LiveCodeBench dataset](https://huggingface.co/livecodebench)


*Overview of the different scenarios present in LiveCodeBench. Credit:*[LiveCodeBench: Holistic and Contamination Free Evaluation of Large Language Models for Code](https://arxiv.org/abs/2403.07974)


## CodeElo


[CodeElo](https://github.com/QwenLM/CodeElo) evaluates how well LLMs can generate code in competition-style programming tasks. Problems are sourced from the Codeforces platform, one of the major competitive programming sites, and vary in problem difficulty and algorithms needed to solve the task.


The benchmark uses an Elo rating system common in chess and competitive games to evaluate model performance on a scale similar to that of human contestants.


> Paper:[CodeElo: Benchmarking Competition-level Code Generation of LLMs with Human-comparable Elo Ratings](https://arxiv.org/abs/2501.01257) by Quan et al. (2025)
> Dataset:[CodeElo dataset](https://huggingface.co/datasets/Qwen/CodeElo)


*The Elo rating leaderboard. Credit:*[CodeElo: Benchmarking Competition-level Code Generation of LLMs with Human-comparable Elo Ratings](https://arxiv.org/abs/2501.01257)


## ResearchCodeBench


[ResearchCodeBench](https://researchcodebench.github.io/index.html) focuses on evaluating LLMs’ ability to implement code from recent machine learning research papers. The benchmark consists of 212 coding challenges derived from top 2024–2025 papers, and LLMs are tasked to translate the conceptual contribution of each paper into executable code.


Here’s how it works: a model is given a research paper, a target code snippet, and context code; and is prompted to fill in the missing code. The generated code is then evaluated against curated tests for functional correctness.


Unlike other code generation benchmarks that test on well‐known tasks, ResearchCodeBench aims to stress how well LLMs perform on new research ideas and whether they are able to support research and innovation.


> Paper:[ResearchCodeBench: Benchmarking LLMs on Implementing Novel Machine Learning Research Code](https://arxiv.org/html/2506.02314v1) by Hua et al. (2025)
> Dataset:[ResearchCodeBench dataset](https://researchcodebench.github.io/leaderboard/index.html)


*Overview of the ResearchCodeBench task setup. Credit:*[ResearchCodeBench: Benchmarking LLMs on Implementing Novel Machine Learning Research Code](https://arxiv.org/html/2506.02314v1) **


## SciCode


[SciCode](https://scicode-bench.github.io/) is a research coding benchmark curated by scientists. It tests LLMs on their ability to generate code to solve real scientific problems. The dataset consists of 80 main problems decomposed into 338 subproblems in 6 natural science domains: mathematics, physics, chemistry, biology, materials science, and a computational domain.


Each task contains optional scientific background descriptions, gold-standard solutions and test cases to check the correctness of the generated code. To solve the tasks, an LLM must demonstrate its abilities in knowledge recall, reasoning, and code synthesis.


> Paper:[SciCode: A Research Coding Benchmark Curated by Scientists](https://arxiv.org/abs/2407.13168) by Tian et al. (2024)
> Dataset:[SciCode dataset](https://huggingface.co/datasets/SciCode1/SciCode)


‍


*A SciCode main problem is decomposed into multiple smaller and easier subproblems. Credit:*[SciCode: A Research Coding Benchmark Curated by Scientists](https://arxiv.org/abs/2407.13168)


## Test your AI system with Evidently


While benchmarks help compare models, they rarely reflect the specifics of your AI application. To better fit into the scope of your use case – be it a coding copilot or developer productivity app – you need[custom evaluations](https://www.evidentlyai.com/llm-guide/llm-evaluation) .


That’s why we built[Evidently](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) . Our open-source library, with over 25 million downloads, simplifies testing and evaluating LLM-powered applications with built-in evaluation templates and metrics.


For teams working on complex, mission-critical AI systems,[Evidently Cloud](https://www.evidentlyai.com/register) provides a platform to collaboratively test and monitor AI quality. You can generate synthetic data, create evaluation scenarios, run tests, and track performance — all in one place.


Ready to test your AI system?[Sign up for free](https://www.evidentlyai.com/register) or[schedule a demo](https://www.evidentlyai.com/get-demo) to see Evidently Cloud in action. We're here to help you build with confidence!
