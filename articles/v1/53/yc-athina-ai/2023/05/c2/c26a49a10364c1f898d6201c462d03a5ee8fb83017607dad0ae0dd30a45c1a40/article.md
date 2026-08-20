---
schema_version: "1.0.0"
document_id: "c26a49a10364c1f898d6201c462d03a5ee8fb83017607dad0ae0dd30a45c1a40"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/tree-of-thoughts-deliberate-problem-solving-with-large-language-models"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:f554049b667cdf5a2fefef1d6535e71ad7f4fde6934d5a285851fd322f03daa8"
---

# Tree of Thoughts: Deliberate Problem Solving with Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2305.10601](https://arxiv.org/abs/2305.10601)


Blog URL


[blog.athina.ai /tree-of...e-models](https://blog.athina.ai/tree-of-thoughts-deliberate-problem-solving-with-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2305.10601](https://arxiv.org/abs/2305.10601)


**By:**[Shunyu Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao%2C%20S) ,[Dian Yu](https://arxiv.org/search/cs?searchtype=author&query=Yu%2C%20D) ,[Jeffrey Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20J) ,[Izhak Shafran](https://arxiv.org/search/cs?searchtype=author&query=Shafran%2C%20I) ,[Thomas L. Griffiths](https://arxiv.org/search/cs?searchtype=author&query=Griffiths%2C%20T%20L) ,[Yuan Cao](https://arxiv.org/search/cs?searchtype=author&query=Cao%2C%20Y) ,[Karthik Narasimhan](https://arxiv.org/search/cs?searchtype=author&query=Narasimhan%2C%20K)


**Abstract:**


> Language models are increasingly being deployed for general problem solving across a wide range of tasks, but are still confined to token-level, left-to-right decision-making processes during inference. This means they can fall short in tasks that require exploration, strategic lookahead, or where initial decisions play a pivotal role. To surmount these challenges, we introduce a new framework for language model inference, Tree of Thoughts (ToT), which generalizes over the popular Chain of Thought approach to prompting language models, and enables exploration over coherent units of text (thoughts) that serve as intermediate steps toward problem solving. ToT allows LMs to perform deliberate decision making by considering multiple different reasoning paths and self-evaluating choices to decide the next course of action, as well as looking ahead or backtracking when necessary to make global choices. Our experiments show that ToT significantly enhances language models' problem-solving abilities on three novel tasks requiring non-trivial planning or search: Game of 24, Creative Writing, and Mini Crosswords. For instance, in Game of 24, while GPT-4 with chain-of-thought prompting only solved 4% of tasks, our method achieved a success rate of 74%. Code repo with all prompts:
>
>
> [this https URL](https://github.com/princeton-nlp/tree-of-thought-llm)


---


###


Summary Notes


####


Enhancing Language Models with the Tree of Thoughts Framework


The quest to develop artificial intelligence that not only understands language but can also solve complex problems is at the forefront of AI research. Language models like GPT-4 have shown impressive text generation abilities, but their problem-solving skills are still evolving.


The introduction of the Tree of Thoughts (ToT) Framework marks a significant step towards improving these abilities by enhancing systematic problem-solving beyond basic decision-making. This post explores the ToT Framework and its potential to revolutionize problem-solving in AI, particularly for AI Engineers in enterprise settings.


####


Understanding the Tree of Thoughts Framework


The ToT Framework is a cutting-edge tool for language models, designed to tackle tasks requiring advanced reasoning and decision-making. It builds on the "Chain of Thought" method, allowing for the exploration of various reasoning paths and enabling models to assess their own decision-making process.


With a tree-like structure where each node represents a "thought" or a coherent text unit, the framework aids in breaking down complex problems into manageable steps. It employs exploration techniques such as breadth-first search (BFS) or depth-first search (DFS), proving effective in various tasks from mathematical problems to creative writing.


####


Key Features and Achievements


- The ToT Framework introduces a new approach for language model inference, emphasizing the evaluation of multiple reasoning paths.


- It significantly outperforms previous models in tasks like the Game of 24, with a success rate of 74% compared to GPT-4's 4% using traditional prompting.


- Its adaptability across different tasks highlights the framework's potential to enhance language models' problem-solving capabilities without extra training.


####


Experimental Successes


The effectiveness of the ToT Framework is showcased through its application in different domains:


- **Game of 24:** It generates and evaluates intermediate steps, significantly improving success rates.


- **Creative Writing:** Guides language models to produce coherent and creative text.


- **Mini Crosswords:** Enhances understanding and reasoning, leading to better crossword solutions.


These examples underline the framework's ability to improve language models' performance in tasks that require in-depth reasoning and planning.


####


Implications and Future Directions


The ToT Framework's ability to integrate systematic planning and search capabilities into language models signals a major advancement in AI problem-solving. Its modular design promises wide applicability, suggesting significant improvements in real-world AI applications. Future research will focus on its potential for more complex applications and the possibility of integrating ToT-style decision-making directly into language model training for even greater capabilities.


####


Challenges and Considerations


While promising, the ToT Framework demands more computational resources than simpler methods and has been tested on relatively straightforward tasks. Further exploration is needed to assess its suitability for complex real-world problems. Moreover, the broader impact and ethical implications of more autonomous and intelligent decision-making by language models require careful consideration, emphasizing the need for safeguards against misuse.


####


Conclusion


The Tree of Thoughts Framework offers a remarkable advancement in enhancing language models' problem-solving abilities. By supporting the systematic exploration of reasoning paths and fostering more deliberate decision-making, ToT sets the stage for transformative applications of language models in complex problem-solving. Its development and potential impact highlight an exciting direction for future AI research, with significant implications for enterprise applications and beyond. Special thanks to Oracle Collaborative Research, the National Science Foundation, and other contributors for their support of this innovative project.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
