---
schema_version: "1.0.0"
document_id: "4b3da96df2e4d2682d14d9a51836f6783a554d89ac258b24e5e414c90b2003d2"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/self-refine-iterative-refinement-with-self-feedback"
published_at: "2023-03-30T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:de82323dfc0405efa1b1a04e4e83d60e850c97c1dbae33b53168e09cd6a925fd"
---

# Self-Refine: Iterative Refinement with Self-Feedback

Do not index


Original Paper


[https://arxiv.org/abs/2303.17651v1](https://arxiv.org/abs/2303.17651v1)


Blog URL


[blog.athina.ai /self-re...feedback](https://blog.athina.ai/self-refine-iterative-refinement-with-self-feedback)


**Original Paper:**[https://arxiv.org/abs/2303.17651v1](https://arxiv.org/abs/2303.17651v1)


**By:**[Aman Madaan](https://arxiv.org/search/cs?searchtype=author&query=Madaan%2C%20A) ,[Niket Tandon](https://arxiv.org/search/cs?searchtype=author&query=Tandon%2C%20N) ,[Prakhar Gupta](https://arxiv.org/search/cs?searchtype=author&query=Gupta%2C%20P) ,[Skyler Hallinan](https://arxiv.org/search/cs?searchtype=author&query=Hallinan%2C%20S) ,[Luyu Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20L) ,[Sarah Wiegreffe](https://arxiv.org/search/cs?searchtype=author&query=Wiegreffe%2C%20S) ,[Uri Alon](https://arxiv.org/search/cs?searchtype=author&query=Alon%2C%20U) ,[Nouha Dziri](https://arxiv.org/search/cs?searchtype=author&query=Dziri%2C%20N) ,[Shrimai Prabhumoye](https://arxiv.org/search/cs?searchtype=author&query=Prabhumoye%2C%20S) ,[Yiming Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20Y) ,[Sean Welleck](https://arxiv.org/search/cs?searchtype=author&query=Welleck%2C%20S) ,[Bodhisattwa Prasad Majumder](https://arxiv.org/search/cs?searchtype=author&query=Majumder%2C%20B%20P) ,[Shashank Gupta](https://arxiv.org/search/cs?searchtype=author&query=Gupta%2C%20S) ,[Amir Yazdanbakhsh](https://arxiv.org/search/cs?searchtype=author&query=Yazdanbakhsh%2C%20A) ,[Peter Clark](https://arxiv.org/search/cs?searchtype=author&query=Clark%2C%20P)


**Abstract:**


> Like people, LLMs do not always generate the best text for a given generation problem on their first try (e.g., summaries, answers, explanations). Just as people then refine their text, we introduce SELF-REFINE, a framework for similarly improving initial outputs from LLMs through iterative feedback and refinement. The main idea is to generate an output using an LLM, then allow the same model to provide multi-aspect feedback for its own output; finally, the same model refines its previously generated output given its own feedback. Unlike earlier work, our iterative refinement framework does not require supervised training data or reinforcement learning, and works with a single LLM. We experiment with 7 diverse tasks, ranging from review rewriting to math reasoning, demonstrating that our approach outperforms direct generation. In all tasks, outputs generated with SELF-REFINE are preferred by humans and by automated metrics over those generated directly with GPT-3.5 and GPT-4, improving on average by absolute 20% across tasks.


---


###


Summary Notes


##


SELF-REFINE: Transforming LLM Output for AI Engineers


In the ever-progressing world of artificial intelligence, the quest to enhance large language models (LLMs) is ongoing.


SELF-REFINE emerges as a cutting-edge method that significantly boosts LLM outputs through iterative refinement and self-feedback, marking a notable advance for AI engineers.


###


Key Concepts of Iterative Refinement in LLMs


Iterative refinement is a technique used across various fields, involving the improvement of initial outputs through repeated feedback and adjustments.


SELF-REFINE applies this principle to LLMs, allowing them to self-enhance by utilizing their own feedback, bypassing the need for extra training data or complex algorithms.


This process, akin to human iterative improvement, is executed with AI's unparalleled speed and scale.


###


How SELF-REFINE Works


SELF-REFINE's process is straightforward yet profoundly effective, involving:


- Creating an initial LLM output.


- Generating feedback on this output using the same LLM.


- Refining the output based on this feedback.


- Repeating the cycle until the output meets the desired quality.


This self-sufficient loop allows LLMs to optimize their outputs using existing capabilities.


###


Practical Applications


SELF-REFINE has been successfully tested in tasks such as:


- Rewriting reviews


- Generating acronyms


- Crafting stories


- Rewriting code


In each scenario, it has shown significant enhancements over traditional direct generation methods by models like GPT-3.5 and GPT-4, proving its broad utility and efficacy.


###


Methodology Behind SELF-REFINE


The framework's strength lies in its simplicity, employing a few-shot prompting technique. This method guides the model in generating both the initial output and the subsequent feedback, keeping outputs relevant and focused.


###


Results and Comparisons


SELF-REFINE consistently surpasses traditional methods, delivering major improvements in output quality across various tasks.


This represents not just an incremental improvement but a major advancement, eclipsing both traditional reinforcement learning and other LLM approaches.


###


Advancements in LLM Applications


SELF-REFINE enhances AI in two significant ways:


1. It offers a novel, efficient way to iteratively refine LLM outputs without extra data or training.


1. It capitalizes on the inherent strengths of existing LLMs, making it a scalable solution for a wide range of applications.


###


The Future of SELF-REFINE


Looking forward, the potential for SELF-REFINE is vast. Exploring its application in more complex, multi-task environments and deeper integration with LLMs could elevate AI systems to new efficiencies and capabilities.


###


Conclusion: Impact on AI Engineering


For AI engineers, especially those in enterprise settings, SELF-REFINE presents a robust tool for perfecting LLM outputs.


Its self-feedback mechanism offers a scalable way to enhance LLM applications, making it an essential part of the AI development toolkit.


As we delve deeper into LLM capabilities, innovations like SELF-REFINE will be pivotal in achieving the highest quality outputs, signaling a promising direction for the future of AI.


####


Acknowledgements


This research received support from the Air Force Research Laboratory, underscoring the collaborative effort to advance AI technology.


####


Key Takeaways


SELF-REFINE offers a significant breakthrough in improving LLM outputs through iterative refinement, eliminating the need for additional training or complex algorithms.


It stands as a practical, scalable, and highly effective method for AI engineers to refine and elevate their models, pushing the boundaries of AI technology further.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
