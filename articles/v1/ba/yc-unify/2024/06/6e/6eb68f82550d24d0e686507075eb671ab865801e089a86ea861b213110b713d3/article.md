---
schema_version: "1.0.0"
document_id: "6eb68f82550d24d0e686507075eb671ab865801e089a86ea861b213110b713d3"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-28"
published_at: "2024-06-06T17:00:40+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:11a228a300678f0cc92bff4028d4fee3a6888720f697fbfee5c88c8f8d1a88e0"
---

# The Deep Dive Issue #28

# The Deep Dive Issue #28


### FlashRag, prompt optimization, and new processors.


[Nassim](https://substack.com/@nas07)


Jun 06, 2024


Here’s an unlikely scenario, you wake up tomorrow and find there’s no AI anymore, how would you feel about it? (Hint: Imagine having to learn again how to write basic code in your favorite language. Scared yet?).


You probably noticed how ChatGPT, Gemini, Claude, and Perplexity all went down simultaneously a couple days ago, giving us a short glimpse of an LLM-less world where you can’t solve your issues with a prompt. Prompting became so much of a norm that we can hardly envision a future without it now. Might be worth brushing up some extra skills on the sides though, just in case the tokens run dry again.


## Finding the golden retriever


Retrieval Augmented Generation (RAG) is a popular approach to improve the accuracy of LLM answers. However, comparing different RAG methods is difficult due to the lack of a standardized framework for building and testing them. FlashRAG provides this framework with a set of tools suited for RAG testing.


**Why would you care? -** Not all RAG methods work equally well depending on the use case. If your LLM application relies on accurate fact-retrieval, finding the appropriate method can help you boost your LLM’s performance.


*Overview of the FlashRAG ecosystem. Source: [Original Paper.](https://arxiv.org/pdf/2405.13576)*


**How does it work? -** FlashRAG is an open-source toolkit that provides complementary modules for RAG testing. Including:


-


**An Environment Module** to set up the necessary datasets, hyperparameters, and evaluation metrics for running experiments.


-


**Pre-implemented Components** for each stage of the RAG process. Namely, a


**Judger** which decides whether a query requires retrieval, a


**Retriever** that performs retrieval from a knowledge base, a


**Reranker** that


**** ranks retrieved documents to improve accuracy, a


**Refiner** to improve the input text, reducing token usage and noise, and a


**Generator** that produces the final answer.


-


**A**


**Pipeline Module** used to combine components to create different RAG processes depending on the use-case. Pipelines include the


**Sequential** pipeline that follows a linear execution path from the input query to the generator component, a


**Branching** pipeline that uses multiple parallel paths for a single query, a


**Conditional** pipeline that asks the Judger to direct queries based on certain conditions, and a


**Loop** pipeline where multiple cycles of retrieval and generation are carried.


The toolkit also includes a set of default benchmark datasets, pre-processed into a unified format and accessible through the Hugging Face platform to test each RAG pipeline on various evaluation metrics.


Check out the


[repository](https://github.com/RUC-NLPIR/FlashRAG) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-28?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Lab


#### What’s the magic word?


Guiding LLM outputs is often achieved using various prompting techniques. However, manually crafting effective prompts is time-consuming and domain-specific.


[PromptWizard](https://arxiv.org/pdf/2405.18369) proposes to automate this by using LLMs to create and refine prompts for specific tasks.


The process starts by refining the initial prompt instruction. A


*Mutate Agent* generates multiple variations of the instruction using different thinking styles, while a


*Scoring Agent* evaluates their effectiveness against training examples. Based on this evaluation, a


*Critic Agent* provides feedback, and a


*Synthesize Agent* uses this feedback to improve the instruction. The feedback incorporates assessments on failing prompts to refine the generation of synthetic examples to be more relevant and diverse.


This iterative process of optimizing both the prompt instruction and examples continues until an effective prompt is achieved. To further enhance the model's performance, a


*Reasoning Agent* generates detailed reasoning chains for the chosen examples, guiding the model's problem-solving process. Finally, PromptWizard integrates task intent and an expert persona into the prompts, making them more aligned with human understanding and reasoning.


PromptWizard consistently outperforms other prompt engineering methods, achieving an average of 5% improvement in accuracy across diverse tasks and datasets.


#### A+ for consistency


LLM performance greatly varies depending on the prompt used, which makes it difficult to reliably evaluate models. Assessing outputs is even trickier when the input prompts are erroneous.


To address the problem of inconsistent LLM evaluations due to prompt variations,


[PromptEval](https://arxiv.org/pdf/2405.17202) estimates an LLM's performance across numerous prompts without requiring a full evaluation for each prompt and example combination. It leverages a statistical model inspired by Item Response Theory (IRT), commonly used in educational testing. This model assumes that the correctness of an LLM's response depends on both the prompt's and the example's inherent difficulties.


By analyzing a small, strategically chosen subset of prompt-example pairs, PromptEval learns these difficulty parameters. This allows the method to predict the performance of unevaluated prompt-example combinations and estimate the overall performance distribution of the LLM across all prompts. To improve its accuracy, PromptEval can use additional information about the prompts, such as embeddings generated by pre-trained language models or hand-crafted features that capture specific formatting characteristics of the prompts.


PromptEval accurately estimates LLM performance across hundreds of prompts, achieving the same accuracy as evaluating all prompts with only 2-4 times the cost of a single-prompt evaluation.


#### Know what you don’t


One challenge with current RAG systems is how to balance the use of internal model knowledge with information retrieved from external sources. This indiscriminate retrieval can lead to inefficiencies, high computational costs, and even reduced accuracy when irrelevant or misleading information is incorporated.


[CTRLA](https://arxiv.org/pdf/2405.18727) is a novel approach that leverages the internal states of LLMs to make more informed decisions about when to retrieve external information. CTRLA accomplishes this through two key mechanisms: honesty control and confidence monitoring. Honesty control involves training a separate


*honesty*


*probe* , which learns to identify and amplify signals within the LLM's internal representations that correspond to truthful and accurate statements. This probe guides the LLM to be more truthful in its responses and acknowledge its limitations when it lacks sufficient knowledge, thus making it less likely to generate plausible-sounding but incorrect answers.


Further, a


*confidence probe* is trained to monitor the internal states of the LLM as it generates text, identifying instances where the model expresses low confidence in its own output. This probe is trained on a dataset designed to reflect different levels of confidence and helps identify when the LLM is uncertain about its response, triggering the retrieval of relevant information from external sources only when necessary. To further enhance retrieval effectiveness, CTRLA incorporates refined search query formulation strategies, ensuring the retrieved information aligns well with the user's question and the LLM's generated output.


CTRLA outperforms existing adaptive RAG methods on a range of question-answering tasks, achieving a better usage balance between internal knowledge and external information.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


#### The Pulse


**Stellar Performance -** Intel's announced its upcoming


[Lunar Lake](https://www.intel.com/content/www/us/en/newsroom/news/intels-lunar-lake-processors-arriving-q3-2024.html) laptop chip which marks a significant departure from its previous architecture, featuring a new system-on-chip design with a tripled AI accelerator enabling up to 48 TOPS for better handling of AI tasks, 14% faster CPU performance, 50% better graphics, and 60% improved battery life compared to its predecessor, Meteor Lake.


**Gotta build fast -** AMD unveiled the


[Ryzen 9000](https://www.amd.com/en/newsroom/press-releases/2024-6-2-amd-unveils-next-gen-zen-5-ryzen-processors-to-p.html) series, boasting ultra fast PC processors for content creation. These new chip lines, built on AMD's latest Zen 5 architecture, are scheduled to launch in July, joining AMD's recently announced AI-capable processors for laptops and desktops. AMD has also outlined its data center chip roadmap, with plans to release new Instinct accelerator series chips every year.


**Armed with a instructions -** Arm is now providing pre-designed chip blueprints optimized for AI applications. The


[Arm Compute Subsystems (CSS)](https://www.arm.com/products/compute-subsystems-for-client) , allow manufacturers to quickly integrate these designs with their own accelerators, resulting in significant performance gains for AI tasks in smartphones and PCs. These optimized blueprints, developed in collaboration with Samsung and TSMC, accelerate development time and allow for more efficient AI processing.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev team.
