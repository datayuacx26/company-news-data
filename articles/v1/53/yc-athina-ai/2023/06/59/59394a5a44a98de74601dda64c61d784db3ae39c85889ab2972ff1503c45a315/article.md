---
schema_version: "1.0.0"
document_id: "59394a5a44a98de74601dda64c61d784db3ae39c85889ab2972ff1503c45a315"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/ai-chain-on-large-language-model-for-unsupervised-control-flow-graph-generation-for-statically-typed-partial-code"
published_at: "2023-06-01T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:ba6071548916d619a00ed209a8809efecfaf15d93461656aee5831e72a79993f"
---

# AI Chain on Large Language Model for Unsupervised Control Flow Graph Generation for Statically-Typed Partial Code

Do not index


Original Paper


[https://arxiv.org/abs/2306.00757](https://arxiv.org/abs/2306.00757)


Blog URL


[blog.athina.ai /ai-chai...ial-code](https://blog.athina.ai/ai-chain-on-large-language-model-for-unsupervised-control-flow-graph-generation-for-statically-typed-partial-code)


**Original Paper:**[https://arxiv.org/abs/2306.00757](https://arxiv.org/abs/2306.00757)


**By:**[Qing Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20Q) ,[Zhou Zou](https://arxiv.org/search/cs?searchtype=author&query=Zou%2C%20Z) ,[Zhenchang Xing](https://arxiv.org/search/cs?searchtype=author&query=Xing%2C%20Z) ,[Zhenkang Zuo](https://arxiv.org/search/cs?searchtype=author&query=Zuo%2C%20Z) ,[Xiwei Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20X) ,[Qinghua Lu](https://arxiv.org/search/cs?searchtype=author&query=Lu%2C%20Q)


**Abstract:**


> Control Flow Graphs (CFGs) are essential for visualizing, understanding and analyzing program behavior. For statically-typed programming language like Java, developers obtain CFGs by using bytecode-based methods for compilable code and Abstract Syntax Tree (AST)-based methods for partially uncompilable code. However, explicit syntax errors during AST construction and implicit semantic errors caused by bad coding practices can lead to behavioral loss and deviation of this http URL address the issue, we propose a novel approach that leverages the error-tolerant and understanding ability of pre-trained Large Language Models (LLMs) to generate CFGs. Our approach involves a Chain of Thought (CoT) with four steps: structure hierarchy extraction, nested code block extraction, CFG generation of nested code blocks, and fusion of all nested code blocks' CFGs. To address the limitations of the original CoT's single-prompt approach (i.e., completing all steps in a single generative pass), which can result in an \`\`epic'' prompt with hard-to-control behavior and error accumulation, we break down the CoT into an AI chain with explicit sub-steps. Each sub-step corresponds to a separate AI-unit, with an effective prompt assigned to each unit for interacting with LLMs to accomplish a specific purpose.Our experiments confirmed that our method outperforms existing CFG tools in terms of node and edge coverage, especially for incomplete or erroneous code. We also conducted an ablation experiment and confirmed the effectiveness of AI chain design principles: Hierarchical Task Breakdown, Unit Composition, and Mix of AI Units and Non-AI Units.Our work opens up new possibilities for building foundational software engineering tools based on LLMs, as opposed to traditional program analysis methods.


---


###


Summary Notes


##


Simplifying Control Flow Graph Creation with Large Language Models


In the fast-paced world of software engineering, getting a clear picture of how applications run is crucial, especially with big and complex systems. Control Flow Graphs (CFGs) are essential for this, as they visually map out all the possible paths an application might take during execution.


However, creating accurate CFGs, especially from incomplete or flawed code, is challenging and often requires advanced solutions. This blog explores a groundbreaking approach that uses Large Language Models (LLMs) for generating CFGs without supervision, opening new doors for AI engineers in big companies.


###


Traditional CFG Generation: The Challenges


Creating CFGs is not new, but traditional techniques struggle with incomplete or incorrect code. These methods typically analyze bytecode or use Abstract Syntax Trees (AST), but they fall short when the code has errors or is not yet finalized. This limitation affects the accuracy of CFGs and their usefulness in real-world situations where codebases are often imperfect.


###


A Fresh Approach: AI Chain on LLMs


Thanks to recent advancements, there's a fresh strategy that employs pre-trained Large Language Models (LLMs) for generating CFGs. This method, known as the AI Chain, divides the CFG generation into smaller tasks, each handled by a specific AI unit. The AI Chain includes:


- **Extracting Structure Hierarchy:** Identifying the code's nested structures.


- **Extracting Nested Code Blocks:** Pulling out individual code blocks.


- **Generating CFGs for Nested Blocks:** Creating CFGs for each block.


- **Fusing Graphs:** Combining all CFGs into one comprehensive graph.


Using LLMs like GPT-3.5 and CodeX, this process more effectively manages incomplete or erroneous code than traditional methods.


###


Inside the AI Chain


####


Extracting Structure Hierarchy


The first step is to understand the code's overall structure. LLMs, with their training on extensive datasets, are great at spotting patterns and hierarchies, making them perfect for this job.


####


Extracting Nested Code Blocks


After mapping out the structure, the next task is to extract individual code blocks. This detail is key to creating accurate CFGs for complex applications.


####


Generating CFGs for Nested Blocks


A CFG is then generated for each block. This step is critical as it turns the code into a visual map that shows possible execution paths.


####


Fusing Graphs


The last step is to merge these individual CFGs into one all-encompassing graph. This comprehensive view is crucial for understanding how an application behaves.


###


Testing and Results: A Comparison


The AI Chain method outperforms traditional CFG tools, especially with code that has errors or is incomplete. An in-depth study confirms the effectiveness of this innovative approach, highlighting its potential to significantly enhance CFG generation.


###


Benefits for Software Engineering


This methodology offers multiple advantages:


- **Better Debugging and Maintenance:** Accurate CFGs can greatly reduce the time and effort needed for debugging and maintaining complex software.


- **Enhanced Code Understanding:** A clear visualization of all execution paths helps engineers understand application behavior, leading to better decision-making.


- **Efficiency with Flawed Code:** The ability of LLMs to handle incomplete or incorrect code opens new possibilities for software development and analysis.


###


Conclusion: Revolutionizing CFG Generation


The AI Chain method represents a major breakthrough in software engineering. By leveraging Large Language Models, it not only addresses the shortcomings of traditional methods but also sets the stage for more advanced and efficient software analysis tools.


Looking ahead, the potential for further improvements and the application of this method in other areas of software engineering is immense and exciting.


In an era where understanding and debugging complex software systems is increasingly important, innovative solutions like the AI Chain are indispensable. It's not just a step but a giant leap towards a more efficient, tolerant, and understandable approach to software engineering.


###


Index Terms


- **CFG Generation**


- **AI Chain**


- **In-Context Learning**


- **Software Engineering Tools**


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
