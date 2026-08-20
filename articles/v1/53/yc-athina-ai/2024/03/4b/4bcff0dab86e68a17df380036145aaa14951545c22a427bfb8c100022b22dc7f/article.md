---
schema_version: "1.0.0"
document_id: "4bcff0dab86e68a17df380036145aaa14951545c22a427bfb8c100022b22dc7f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/self-taught-optimizer-stop-recursively-self-improving-code-generation"
published_at: "2024-03-01T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:14.489943+00:00"
content_hash: "sha256:92ffeb4b4487d2273e5a52a52a31fa2bb1346fce7cae8fb4ffcc53ba02d6b6d0"
---

# Self-Taught Optimizer (STOP): Recursively Self-Improving Code Generation

Do not index


Original Paper


[https://arxiv.org/abs/2310.02304](https://arxiv.org/abs/2310.02304)


Blog URL


[blog.athina.ai /self-ta...neration](https://blog.athina.ai/self-taught-optimizer-stop-recursively-self-improving-code-generation)


**Original Paper:**[https://arxiv.org/abs/2310.02304](https://arxiv.org/abs/2310.02304)


**By:**[Eric Zelikman](https://arxiv.org/search/cs?searchtype=author&query=Zelikman%2C%20E) ,[Eliana Lorch](https://arxiv.org/search/cs?searchtype=author&query=Lorch%2C%20E) ,[Lester Mackey](https://arxiv.org/search/cs?searchtype=author&query=Mackey%2C%20L) ,[Adam Tauman Kalai](https://arxiv.org/search/cs?searchtype=author&query=Kalai%2C%20A%20T)


**Abstract:**


> Several recent advances in AI systems (e.g., Tree-of-Thoughts and Program-Aided Language Models) solve problems by providing a "scaffolding" program that structures multiple calls to language models to generate better outputs. A scaffolding program is written in a programming language such as Python. In this work, we use a language-model-infused scaffolding program to improve itself. We start with a seed "improver" that improves an input program according to a given utility function by querying a language model several times and returning the best solution. We then run this seed improver to improve itself. Across a small set of downstream tasks, the resulting improved improver generates programs with significantly better performance than its seed improver. A variety of self-improvement strategies are proposed by the language model, including beam search, genetic algorithms, and simulated annealing. Since the language models themselves are not altered, this is not full recursive self-improvement. Nonetheless, it demonstrates that a modern language model, GPT-4 in our experiments, is capable of writing code that can call itself to improve itself. We consider concerns around the development of self-improving technologies and evaluate the frequency with which the generated code bypasses a sandbox.


---


###


Summary Notes


##


Exploring the Self-Taught Optimizer (STOP): Advancing Recursive AI Improvement


The field of Artificial Intelligence (AI) is constantly advancing, with a growing focus on models that can improve their own learning processes.


The introduction of the Self-Taught Optimizer (STOP) is a significant step towards achieving recursive self-improvement in AI systems.


This blog post examines the STOP framework, its foundation, how it works, and its potential impact on AI development, especially for enterprise-level applications.


###


Introduction to STOP


The idea behind STOP originated from a simple question: Can we use a language model like GPT-4 not just to produce outputs but to optimize its own code?


Eric Zelikman and his team introduced STOP, a system that uses a language model to iteratively enhance itself, showing promising results in improving performance across different tasks.


This process represents a new method for boosting AI's problem-solving abilities.


###


Key Elements of STOP


STOP combines several modern techniques in language modeling and self-improvement. Here are its main components:


- **Algorithm** : It starts with a basic program, known as a 'seed improver,' which refines solutions to problems with guidance from a language model. This improver itself gets better over time.


- **Evaluation Strategies** : The success of STOP is gauged by its ability to surpass its initial problem-solving capabilities, with notable advancements in specific tasks.


- **Self-Improvement Techniques** : STOP uses strategies like beam search, genetic algorithms, and simulated annealing, all directed by the language model's suggestions.


- **Sandboxing** : An essential feature of STOP is its focus on safety, ensuring the self-improving code is contained and cannot perform harmful actions.


###


Experiments and Results


STOP was put to the test in various settings, demonstrating its effectiveness and adaptability:


- **Task-Specific Success** : Initially applied to learning parity with noise, STOP showed it could significantly self-improve, suggesting it can handle more complex issues.


- **Transferability** : The framework's ability to perform well in different tasks highlights its wide-ranging applicability.


- **Comparative Analysis** : When compared to other language models, STOP proved to be effective across several architectures.


###


Future Directions and Challenges


STOP paves the way for further exploration into recursive self-improvement in AI, showcasing the potential for language models to not only execute tasks but also refine how they carry out these tasks.


However, this innovation raises important safety and ethical questions, including issues around sandbox evasion and the impact of self-improving technologies.


###


The Future of AI Engineering


For AI engineers, especially those in enterprise settings, STOP is more than an algorithm; it represents a shift towards AI systems that can independently evolve and adapt. This could lead to more efficient, powerful, and resilient AI solutions capable of addressing new challenges.


####


Tips for Implementing STOP


- **Start Small** : Initially apply STOP to minor, non-critical tasks to gauge its impact and learn from the process.


- **Safety First** : Ensure the implementation of strong sandboxing and monitoring to keep the self-improvement process secure.


- **Iterate and Learn** : Continuously refine the STOP framework based on feedback from each iteration, customizing it to meet specific needs and challenges.


###


Conclusion


The development of the Self-Taught Optimizer is a landmark in the quest for self-improving AI systems.


By leveraging language models for recursive optimization, STOP introduces new opportunities for enhancing AI's problem-solving capabilities.


As we embark on this exciting new phase, the role of AI engineers and the future of AI development are set for a significant transformation.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
