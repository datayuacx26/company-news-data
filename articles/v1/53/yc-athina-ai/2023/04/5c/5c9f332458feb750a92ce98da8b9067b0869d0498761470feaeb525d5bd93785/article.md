---
schema_version: "1.0.0"
document_id: "5c9f332458feb750a92ce98da8b9067b0869d0498761470feaeb525d5bd93785"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/a-study-on-prompt-design-advantages-and-limitations-of-chatgpt-for-deep-learning-program-repair"
published_at: "2023-04-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:1c8dbacd61f3dc84a2a4dc0d6a7f075800ad6f45bbb7cf498765d34fa0fc9995"
---

# A study on Prompt Design, Advantages and Limitations of ChatGPT for Deep Learning Program Repair

Do not index


Original Paper


[https://arxiv.org/abs/2304.0819](https://arxiv.org/abs/2304.08191)


Blog URL


[blog.athina.ai /a-study...m-repair](https://blog.athina.ai/a-study-on-prompt-design-advantages-and-limitations-of-chatgpt-for-deep-learning-program-repair)


**Original Paper:**[https://arxiv.org/abs/2304.08191](https://arxiv.org/abs/2304.08191)


**By:**[Jialun Cao](https://arxiv.org/search/cs?searchtype=author&query=Cao%2C%20J) ,[Meiziniu Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20M) ,[Ming Wen](https://arxiv.org/search/cs?searchtype=author&query=Wen%2C%20M) ,[Shing-chi Cheung](https://arxiv.org/search/cs?searchtype=author&query=Cheung%2C%20S)


**Abstract:**


> ChatGPT has revolutionized many research and industrial fields. ChatGPT has shown great potential in software engineering to boost various traditional tasks such as program repair, code understanding, and code generation. However, whether automatic program repair (APR) applies to deep learning (DL) programs is still unknown. DL programs, whose decision logic is not explicitly encoded in the source code, have posed unique challenges to APR. While to repair DL programs, an APR approach needs to not only parse the source code syntactically but also needs to understand the code intention. With the best prior work, the performance of fault localization is still far less than satisfactory (only about 30\\%). Therefore, in this paper, we explore ChatGPT's capability for DL program repair by asking three research questions. (1) Can ChatGPT debug DL programs effectively? (2) How can ChatGPT's repair performance be improved by prompting? (3) In which way can dialogue help facilitate the repair? On top of that, we categorize the common aspects useful for prompt design for DL program repair. Also, we propose various prompt templates to facilitate the performance and summarize the advantages and disadvantages of ChatGPT's abilities such as detecting bad code smell, code refactoring, and detecting API misuse/deprecation.


---


###


Summary Notes


##


Simplifying Deep Learning Program Repair with ChatGPT: Strategies for AI Engineers


Large language models like ChatGPT are making waves in the software engineering world, especially in code generation, understanding, and program repair.


This blog post focuses on ChatGPT's role in fixing deep learning (DL) programs, known for their intricate neural network-based decision processes.


We'll explore how AI engineers in big companies can use ChatGPT to improve DL program repair, tackle its challenges, and look forward to future possibilities.


###


Understanding the Challenge with DL Program Repair


DL programs are unique because they use neural networks, making it hard to identify and fix errors with traditional debugging tools.


This complexity calls for new methods, prompting researchers to investigate ChatGPT's potential in this specialized area.


###


Study Overview


The study we're discussing analyzed 58 buggy DL programs from StackOverflow and GitHub to evaluate ChatGPT against two benchmarks, AutoTrainer and DeepFD, in finding and fixing bugs.


This comparison aimed to understand ChatGPT's strengths and weaknesses in DL program repair.


###


Exploring Key Questions


Researchers focused on three main questions:


- **Can ChatGPT effectively find bugs in DL programs?**


- **Can better prompts improve ChatGPT's ability to fix these bugs?**


- **Does a dialogue with ChatGPT enhance the repair process?**


These questions helped assess ChatGPT's utility and efficiency in repairing DL programs.


###


Study Insights


- **Bug Detection:** ChatGPT was good at spotting basic and some complex bugs but fell short of DeepFD in recognizing deep learning-specific issues.


- **Bug Fixing:** Generic prompts led to subpar fixes, but detailed, context-rich prompts significantly boosted ChatGPT's repair accuracy.


- **Interactive Repair:** Allowing ChatGPT to ask for more information through a dialogue system further improved repair quality, showcasing the benefit of interactive models over static prompts.


###


Making the Most of ChatGPT


The study highlights the importance of prompt design and interactive dialogue in maximizing ChatGPT's usefulness for DL program repair. AI engineers can improve ChatGPT's debugging and repair ability by:


- Creating detailed prompts with rich context


- Using a dialogue-based system for ongoing clarification


These tactics optimize ChatGPT's performance, expanding its applicability in software engineering tasks.


###


Conclusions and Next Steps


ChatGPT, with the right prompts and an interactive system, proves to be a powerful asset for DL program repair.


The study encourages further exploration into advanced prompt generation and dialogue strategies, potentially broadening the scope beyond DL programs.


###


Future Research Directions


This study paves the way for several research opportunities:


- Developing automated systems for creating dynamic prompts


- Exploring various interactive models for AI-assisted debugging


- Applying these methods to a broader range of software engineering problems


For AI engineers at large companies, adopting these strategies could revolutionize DL program debugging and repair, setting new standards for efficiency and effectiveness in the field.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
