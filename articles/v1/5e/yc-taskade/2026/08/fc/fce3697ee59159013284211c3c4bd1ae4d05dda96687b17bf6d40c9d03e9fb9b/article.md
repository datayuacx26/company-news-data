---
schema_version: "1.0.0"
document_id: "fce3697ee59159013284211c3c4bd1ae4d05dda96687b17bf6d40c9d03e9fb9b"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/context-window-history"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-02T02:04:01.742194+00:00"
fetched_at: "2026-08-02T02:04:03.806964+00:00"
content_hash: "sha256:d7c8e9a691fae9a809b9557179a5aa0ea829462d6b5061cfedfe0dd422d05583"
---

# From 4K to 1M: The History of the Context Window (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


From 4K to 1M: The History of…


On this page (15)


Punched paper tape was the first context window. A reader head sat over a fixed span of the tape, and everything outside that span may as well not have existed. Move the head, and the old holes were gone.


Sixty years later the same constraint governs every large language model on the market. A model does not remember your last conversation. It reads a window of text, predicts the next token, and forgets. The only question that has ever mattered is: **how wide is the window?**


In June 2018 the answer was 512 tokens, about one printed page. In 2026 the answer is roughly one million tokens, about 2,500 pages, and the frontier providers have stopped charging a premium for it. This is the story of how that happened, what it cost, and why the biggest window is still not the best one.


> **TL;DR:** The LLM context window grew roughly 2,000x in eight years, from GPT-1's 512 tokens in 2018 to the 1M-token default of 2026. But accuracy did not scale with it:[Chroma's 2025 Context Rot study](https://research.trychroma.com/context-rot) found all 18 models tested got less reliable as input grew. Bigger windows made[context engineering](https://www.taskade.com/wiki/ai/context-engineering) the job, not obsolete.


Five-hole and eight-hole punched paper tape. Photo: Wikimedia Commons / TedColes / Public domain.


---


## What Is a Context Window?


A[context window](https://www.taskade.com/wiki/ai/context-window) is the total span of text a language model can reference while producing a single response, counted in[tokens](https://www.taskade.com/wiki/ai/token) . One token is approximately 0.75 English words, so a 200,000-token window holds roughly 150,000 words, or about 500 printed pages. Everything the model can "see" for that request lives inside it, and everything outside it is invisible.


The window is working memory, not knowledge. What the model absorbed during[training](https://www.taskade.com/wiki/ai/model-parameters) is[parametric knowledge](https://www.taskade.com/wiki/ai/parametric-knowledge) , frozen at a[knowledge cutoff](https://www.taskade.com/wiki/ai/knowledge-cutoff) . The window is the only place fresh information can enter. That is why the number matters commercially: it sets the ceiling on how much of your codebase, your contract, or your customer history a model can consider at once.


It is also why models feel forgetful. A model is[stateless](https://www.taskade.com/wiki/ai/stateless-vs-stateful) between requests. Every "memory" you have ever seen an assistant display was re-sent to it as text inside the window.


---


## The Master Timeline: Every Context Window Milestone, 2017 to 2026


The context window grew from 512 to 10,000,000 tokens in under seven years, a roughly 20,000x expansion in advertised capacity. The table below is the complete milestone record, with each window converted to printed pages using the providers' own arithmetic: 1 token is about 0.75 words, and a printed page holds about 300 words, so **1 page is about 400 tokens** . That rule reproduces both OpenAI's "over 300 pages" claim for 128K and Anthropic's "over 500 pages" claim for 200K.


Year Model or release Window (tokens) Pages The first that mattered


Jun 2017[Transformer paper](https://arxiv.org/abs/1706.03762) n/a n/a Attention costs scale with the square of sequence length


Jun 2018[GPT-1](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) 512 ~1 First generative pretrained transformer


Oct 2018[BERT](https://arxiv.org/abs/1810.04805) 512 ~1 512 becomes the industry-wide ceiling


Feb 2019[GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) 1,024 ~3 The first doubling


May 2020[GPT-3](https://arxiv.org/abs/2005.14165) 2,048 ~5 175B parameters, still a five-page window


Nov 2022 ChatGPT (GPT-3.5) 4,096 ~10 Context becomes a consumer-visible limit


Mar 2023 GPT-4 8,192 ~20 Window size becomes a pricing tier


Mar 2023 GPT-4-32k 32,768 ~82 First window that fits a small book


Mar 2023 Claude 9,000 ~23 Anthropic's starting point


May 2023[Claude 100K](https://www.anthropic.com/news/100k-context-windows) 100,000 ~250 First six-figure window


Jun 2023 gpt-3.5-turbo-16k 16,385 ~41 Long context reaches the cheap tier


Nov 2023 GPT-4 Turbo 128,000 ~320 128K becomes the default expectation


Nov 2023[Claude 2.1](https://www.anthropic.com/news/claude-2-1) 200,000 ~500 200K standard, and the recall debate starts


Feb 2024[Gemini 1.5 Pro](https://blog.google/technology/ai/google-gemini-next-generation-model-february-2024/) 1,000,000 (limited) ~2,500 First million-token window shipped to anyone


Mar 2024[Claude 3](https://www.anthropic.com/news/claude-3-family) 200,000 ~500 Capable of 1M+, gated to select customers


May 2024[Gemini 1.5 Pro](https://blog.google/technology/ai/google-io-2024-100-announcements/) 2,000,000 (waitlist) ~5,000 First 2M window


Aug 2024[Magic LTM-2-mini](https://magic.dev/blog/100m-token-context-windows) 100,000,000 (research) ~250,000 100M claimed, not a general model


Apr 2025[GPT-4.1](https://developers.openai.com/api/docs/models/gpt-4.1) 1,047,576 ~2,600 OpenAI joins the million-token tier


Apr 2025[Llama 4 Scout](https://ai.meta.com/blog/llama-4-multimodal-intelligence/) 10,000,000 ~25,000 Largest advertised window, open weights


Aug 2025[Claude Sonnet 4](https://claude.com/blog/1m-context) 1,000,000 (beta) ~2,500 1M arrives, with a premium above 200K


2026 Frontier default ~1,000,000 ~2,500 1M standard, and the premium disappears


Two things stand out. First, the entire jump from "one page" to "one book" happened inside a single year, 2023. Second, the 2026 entry is not a bigger number. It is the same number without a surcharge, which is a more meaningful shift than any of the raw increases before it.


---


## Why the Window Was Stuck at 512 Tokens for Two Years


The 512-token ceiling was an economics problem, not an ambition problem. The[attention mechanism](https://www.taskade.com/wiki/ai/attention-mechanism) introduced in[Attention Is All You Need](https://arxiv.org/abs/1706.03762) (Vaswani et al., June 12, 2017) compares every token to every other token, so compute and memory grow with the **square** of sequence length. Doubling the window quadruples the cost.[GPT-1](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) trained on "contiguous sequences of 512 tokens," and[BERT](https://arxiv.org/abs/1810.04805) sampled inputs "such that the combined length is ≤ 512 tokens." Neither team was short of text. They were short of memory.


That quadratic wall explains the strangest fact in the table above:[GPT-3](https://arxiv.org/abs/2005.14165) had 175 billion parameters in May 2020 and a context window of exactly 2,048 tokens, five printed pages. The paper states it plainly: "All models use a context window of` nctx` = 2048 tokens." The model got 1,500x larger than GPT-1. The window got 4x larger.


---


## The Engineering That Broke the Quadratic Wall


Seven research results, published between 2019 and 2023, turned the context window from a fixed architectural constant into a product decision. Each attacked a different bottleneck, and the million-token window required all of them.


Year Technique The bottleneck it attacked What it unlocked


Apr 2019[Sparse Transformers](https://arxiv.org/abs/1904.10509) Every token attending to every token Attention reduced to O(n√n), sequences "tens of thousands" long


Apr 2020[Longformer](https://arxiv.org/abs/2004.05150) Quadratic attention on long documents Attention that "scales linearly with sequence length"


Apr 2021[RoPE](https://arxiv.org/abs/2104.09864) Fixed absolute position embeddings Position encoding with "flexibility of sequence length"


Aug 2021[ALiBi](https://arxiv.org/abs/2108.12409) Models failing outside their trained length Train on 1,024 tokens, run at 2,048, same perplexity


May 2022[FlashAttention](https://arxiv.org/abs/2205.14135) GPU memory traffic, not raw arithmetic Exact attention at 16K then 64K sequence lengths


Jun 2023[Position Interpolation](https://arxiv.org/abs/2306.15595) Extending a model already trained LLaMA to 32,768 tokens "within 1000 steps" of fine-tuning


Oct 2023[Ring Attention](https://arxiv.org/abs/2310.01889) The memory ceiling of one device Context "up to device count times longer," millions of tokens


The insight in[FlashAttention](https://arxiv.org/abs/2205.14135) (Dao et al., May 27, 2022) is the one that changed the shipping schedule. Tri Dao's team showed the bottleneck was not floating-point math but data movement between levels of GPU memory. Restructuring attention to be aware of that hierarchy produced a 3x speedup on GPT-2 at 1K sequence length and made 64K-token sequences tractable. Nothing about the mathematics changed. The plumbing did.


[Ring Attention](https://arxiv.org/abs/2310.01889) (Liu, Zaharia, and Abbeel, October 3, 2023) removed the last hard ceiling by distributing a single sequence across many devices, making context length scale with the size of your cluster rather than the memory of one chip. It was published four months before Google announced a million-token window.


---


## 2023: The Year the Window Grew 50x


The context window went from 4,096 tokens to 200,000 tokens in eleven months, a 48x expansion inside a single calendar year. Set against the rest of the[history of computing](https://www.taskade.com/blog/history-of-computing) , that is an unusually steep capability curve, and it was driven by direct answer-and-counter between two vendors.


The sequence was a direct exchange of moves. ChatGPT launched on GPT-3.5 with 4,096 tokens in November 2022 and made "the conversation got too long" a household complaint.[GPT-4 arrived in March 2023](https://www.taskade.com/blog/openai-chatgpt-history) with 8,192 tokens and a 32,768-token variant sold separately, the first time a window size was itself a product SKU. Two months later, on May 11, 2023, Anthropic[expanded Claude from 9,000 to 100,000 tokens](https://www.anthropic.com/news/100k-context-windows) , roughly 75,000 words, and demonstrated it by loading The Great Gatsby. In November, OpenAI answered at DevDay with GPT-4 Turbo at 128,000 tokens, and[Anthropic answered fifteen days later](https://www.anthropic.com/news/claude-2-1) with Claude 2.1 at 200,000 tokens, "roughly 150,000 words, or over 500 pages."


The same month, the counter-argument arrived. Greg Kamradt ran a "needle in a haystack" pressure test, hiding a single out-of-place sentence at varying depths inside long documents and asking the model to find it. The results were uncomfortable for both vendors: recall was near perfect at the very top and very bottom of a document, and unreliable in between. The industry had spent a year buying window size and had not checked whether the middle of the window worked.


It did not. Four months earlier,[Liu et al. had already published the reason](https://arxiv.org/abs/2307.03172) .


---


## Lost in the Middle: Why Bigger Windows Stopped Meaning Better Answers


Model accuracy is not uniform across the context window. The[Lost in the Middle](https://arxiv.org/abs/2307.03172) paper (Nelson F. Liu et al., July 6, 2023) found that "performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts, even for explicitly long-context models." The shape of that curve has held up across three years of models.


```text
ACCURACY BY POSITION OF THE ANSWER WITHIN THE CONTEXT
(shape reported by Liu et al., "Lost in the Middle", 2023)   high  |*                                                    *         | *                                                  *         |   *                                              *         |     *                                          *         |        *                                    *         |            *                            *         |                 *                  *         |                       *      *    low  |                          **         +-----------------------------------------------------          start        <--  position in context  -->       end            ^                       ^                       ^        recalled                the dead zone            recalled        reliably              (accuracy floor)           reliably
```


Two later benchmarks turned that observation into a purchasing criterion.


Benchmark Year What it tested What it found


[Lost in the Middle](https://arxiv.org/abs/2307.03172) 2023 Position of the answer within the input Accuracy peaks at the edges, collapses in the middle


Needle in a Haystack 2023 Single-fact recall by depth and length Recall degrades well before the advertised limit


[RULER](https://arxiv.org/abs/2404.06654) 2024 Multi-needle, multi-hop, aggregation "Only half" of models claiming 32K+ held up at 32K


[Context Rot](https://research.trychroma.com/context-rot) 2025 18 models, accuracy vs input length Performance "grows increasingly unreliable as input length grows"


Chroma's[Context Rot](https://research.trychroma.com/context-rot) report (Kelly Hong, Anton Troynikov, and Jeff Huber, July 14, 2025) is the most damaging of the four, because it tested tasks that should be trivial. Models failed to reliably reproduce a simple repeated-word sequence as the input grew. A single distractor sentence measurably reduced accuracy. Semantically distant questions degraded far faster than obvious ones. None of that is a retrieval problem. It is the window itself getting less trustworthy the more you put in it.


[Context rot](https://www.taskade.com/wiki/ai/context-rot) has since moved from a research finding into vendor documentation, where it is now cited as a reason to curate what enters the window rather than fill it. That is a remarkable reversal for a capability the same vendors spent 2023 advertising by the token.


---


## What Actually Fills a Context Window


Almost everything in a modern request competes for window space, and the user's prompt is usually the smallest part of it. On a typical agentic turn, the system prompt, tool definitions, conversation history, tool results, attached files, retrieved documents, and the model's own reasoning tokens all consume the same budget as the question you typed.


Here is where the tokens realistically go on a long coding turn. The allocation below is illustrative rather than a vendor specification, but the proportions match what teams building on[agent harnesses](https://www.taskade.com/wiki/ai-agents/agent-harness) consistently report.


```text
A 1,000,000-TOKEN AGENT TURN: WHERE THE TOKENS ACTUALLY GO
(illustrative allocation for a coding agent, not a vendor spec) component                      tokens    share
system prompt and persona       2,000     0.2%  |    tool definitions               12,000     1.2%  |#    project knowledge              40,000     4.0%  |##    retrieved documents           120,000    12.0%  |######    conversation history          250,000    25.0%  |############    tool results and file reads   400,000    40.0%  |####################    reasoning tokens and reply     60,000     6.0%  |###
in context                    884,000    88.4%    headroom remaining            116,000    11.6%
```


Two details catch teams out repeatedly.


**Cached tokens still occupy the window.**[Prompt caching](https://www.taskade.com/wiki/ai/prompt-caching) changes what you pay for a repeated prefix, not whether it takes up space. A cached 300,000-token codebase costs a fraction of full price and still consumes 300,000 tokens of a 1,000,000-token budget.


**Reasoning tokens count too.** On models that produce visible reasoning before answering, those tokens are billed as output and, on several current models, are carried forward into the next turn as input. A long chain of reasoning is a long-context cost, not a free extra.


---


## What It Costs to Fill a Window


Filling a one-million-token window once costs between $0.30 and $10.00 at 2026 list prices, depending on the model tier. That is cheap. Filling it fifty times in one agent session is not, and that is the cost nobody quotes.


Input price per 1M tokens Fill a 128K window Fill a 1M window 1M window, read from cache


$0.30 $0.04 $0.30 $0.03


$1.00 $0.13 $1.00 $0.10


$2.00 $0.26 $2.00 $0.20


$3.00 $0.38 $3.00 $0.30


$5.00 $0.64 $5.00 $0.50


$10.00 $1.28 $10.00 $1.00


The cached column assumes the common industry multiplier of 10 percent of the base input price for a cache hit, which is what Anthropic publishes. Prompt caching arrived on August 14, 2024,[claiming cost reductions of up to 90 percent and latency reductions of up to 85 percent](https://www.anthropic.com/news/prompt-caching) for long prompts. It is the single largest reason long-context agents became economically viable.


The pricing history of the window is its own three-act story:


1. **Window size as a SKU (2023).** GPT-4-32k was sold as a separate, more expensive model than GPT-4.
2. **The long-context surcharge (2024 to 2025).**[Claude Sonnet 4's 1M beta](https://claude.com/blog/1m-context) charged $6 per million input tokens and $22.50 per million output tokens above 200K, against $3 and $15 below it. Google's current Gemini Pro pricing still doubles input from $2.00 to $4.00 per million above 200K.
3. **The surcharge disappears (2026).** Anthropic's current documentation states that its 1M-token models bill "at standard pricing," and that "a 900k-token request is billed at the same per-token rate as a 9k-token request."


That third act is the real 2026 headline. Long context stopped being a premium feature and became the baseline, which shifts the entire optimization problem from "can I afford this window" to "should I be filling it at all."


---


## Advertised Window Versus Effective Window


An advertised context window is a hard limit, not a performance guarantee, and the two numbers diverge sharply.[RULER](https://arxiv.org/abs/2404.06654) (Hsieh et al., April 9, 2024) put it bluntly: "While these models all claim context sizes of 32K tokens or greater, only half of them can maintain satisfactory performance at the length of 32K."


There is a second comparability problem almost nobody accounts for. **A token is defined by each model's tokenizer** , so identical text produces different counts on different models. Anthropic notes that its newer tokenizer "produces approximately 30% more tokens for the same text" than its previous one. A 200,000-token window on one tokenizer holds meaningfully less prose than a 200,000-token window on another. Comparing window numbers across vendors is comparing two rulers with different centimetres.


Practical reading rules for any published context number:


- **Treat the advertised figure as a ceiling, not a working range.** Assume useful recall well below it and test on your own documents with your own questions.
- **Check where the price tier changes.** A 200K price break tells you where the vendor's own economics get harder.
- **Test the middle, not the edges.** Edge recall is near perfect on every model and tells you nothing.
- **Test with distractors present.** Chroma found single distractors measurably reduce accuracy, and clean synthetic tests hide that.
- **Re-verify after every model update.** Window behaviour changes between versions even when the advertised number does not.


---


## From Bigger Windows to Better Context


The discipline that replaced "get a bigger window" is[context engineering](https://www.taskade.com/wiki/ai/context-engineering) : deciding what enters the window, in what order, and what leaves as a session grows. It exists because the two obvious strategies both fail. Filling a million-token window on every turn is expensive and, per every benchmark above, less accurate. Truncating history loses the thread.


The working answer in 2026 is a layered one, and each layer maps to a different mechanism.


Choosing between them is a routing decision, not a preference:


Strategy Best when Cost profile What it does not solve


[Retrieval](https://www.taskade.com/wiki/ai/retrieval-augmented-generation) The corpus is far larger than any window Cheapest per answer Retrieval quality becomes the bottleneck


[Prompt caching](https://www.taskade.com/wiki/ai/prompt-caching) A large prefix repeats across many calls About 10% of base input price on hits Space: cached tokens still fill the window


[Compaction](https://www.taskade.com/wiki/ai-agents/context-compaction) A session outlives the window Cheap, lossy by design Detail lost in summarization is gone


[Persistent memory](https://www.taskade.com/wiki/ai-agents/agent-memory) Facts must survive the[session](https://www.taskade.com/wiki/ai-agents/agent-session) Storage, not tokens Needs a decision about what is worth keeping


Long context The document genuinely must be read whole Highest per call Accuracy still degrades with length


If you are choosing between these for a production system, the deeper comparison lives in[fine-tuning vs RAG vs prompting](https://www.taskade.com/blog/fine-tuning-vs-rag) and the practical playbook in our[context engineering field guide](https://www.taskade.com/blog/context-engineering) . For the retrieval half specifically, see[vector databases and semantic search explained](https://www.taskade.com/blog/vector-databases) .


---


## How Taskade Approaches Context


Taskade treats durable context as something that belongs in the workspace, not in a chat window. Projects, uploaded knowledge, and agent memory persist across sessions, so an agent starting fresh recovers what it needs from the workspace instead of depending on one enormous prompt surviving intact.


That structure is what Workspace DNA describes: **Memory** is the context that persists, **Intelligence** is the reasoning applied to it, and **Execution** is the work that results. In practice it means:


- **[Agents](https://www.taskade.com/agents)** draw on workspace knowledge and[persistent memory](https://www.taskade.com/wiki/ai/persistent-memory) rather than re-reading the whole history each turn. Details in[/learn/agents/knowledge](https://www.taskade.com/learn/agents/knowledge) .
- **[Taskade Genesis](https://www.taskade.com/create)** builds apps whose context lives in structured project data across 7 project views, so the app carries its own state. See[/learn/genesis/app-context](https://www.taskade.com/learn/genesis/app-context) .
- **[Automations](https://www.taskade.com/automate)** move information between the workspace and 100+ bidirectional[integrations](https://www.taskade.com/integrations) , keeping context current without a human pasting it into a prompt.
- **Model routing** spans 15+ frontier models from OpenAI, Anthropic, Google, and open-weight providers, so the window available adapts to the task instead of the task adapting to one model.


Taskade EVE coordinates that work across projects. Paid plans start at $10 per month billed annually, and you can see what teams have built with this pattern in the[community gallery](https://www.taskade.com/community) .


---


## Frequently Asked Questions


What is a context window in an LLM?


A context window is the total amount of text a language model can reference while generating a single response, measured in tokens. It includes the system prompt, conversation history, tool definitions, tool results, attached files, retrieved documents, and the reply being written. One token is roughly 0.75 English words, so 200,000 tokens holds about 150,000 words or 500 printed pages.


What was the first LLM context window and how big was it?


[GPT-1](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) , released in June 2018, trained on contiguous sequences of 512 tokens, roughly one printed page.[BERT](https://arxiv.org/abs/1810.04805) used the same 512-token ceiling in October 2018. GPT-2 doubled it to 1,024 tokens in February 2019.


When did the first 100,000-token context window ship?


Anthropic shipped it on[May 11, 2023](https://www.anthropic.com/news/100k-context-windows) , expanding Claude from 9,000 to 100,000 tokens, described as roughly 75,000 words. Anthropic doubled that to 200,000 tokens with[Claude 2.1](https://www.anthropic.com/news/claude-2-1) on November 21, 2023.


Which model had the first 1 million token context window?


Google announced[Gemini 1.5 Pro on February 15, 2024](https://blog.google/technology/ai/google-gemini-next-generation-model-february-2024/) with a standard 128,000-token window and 1 million tokens for a limited group of developers, and reported testing up to 10 million tokens in research. It reached 2 million tokens for waitlisted developers in May 2024. The full arc is covered in our[Google Gemini history](https://www.taskade.com/blog/google-gemini-history) and[Anthropic and Claude history](https://www.taskade.com/blog/anthropic-claude-history) posts.


What is the largest context window available in 2026?


[Llama 4 Scout](https://ai.meta.com/blog/llama-4-multimodal-intelligence/) advertises 10 million tokens with open weights, the largest published figure. Among hosted frontier models the practical ceiling is around 1 million:[GPT-4.1](https://developers.openai.com/api/docs/models/gpt-4.1) shipped 1,047,576 tokens, and current Claude models make 1 million the default at standard pricing. See also our roundup of[open-source LLMs](https://www.taskade.com/blog/open-source-llms) .


Does a bigger context window mean better answers?


No.[Lost in the Middle](https://arxiv.org/abs/2307.03172) (2023) found accuracy peaks at the start and end of the input and drops in the middle.[RULER](https://arxiv.org/abs/2404.06654) (2024) found only half of models claiming 32K or more held up at 32K.[Context Rot](https://research.trychroma.com/context-rot) (2025) tested 18 models and found all grew less reliable as input length increased, even on trivial tasks.[Evals](https://www.taskade.com/wiki/ai/evals) on your own data are the only reliable check.


What is context rot?


Context rot is the measured decline in accuracy and recall as input tokens increase, even when the task stays simple. The term was popularized by Chroma's July 2025 report and now appears in vendor documentation as a reason to curate a window rather than fill it. See[context rot](https://www.taskade.com/wiki/ai/context-rot) for the full definition, and[hallucinations](https://www.taskade.com/wiki/ai/hallucinations) for how it interacts with fabricated output.


How much does it cost to fill a 1 million token context window?


At $3 per million input tokens, once, about $3.00. Read from cache at the common 10 percent multiplier, about $0.30. The real driver is repetition: an agent that re-sends a near-full window on each of 50 turns pays 50 times, which is why[prompt caching](https://www.taskade.com/wiki/ai/prompt-caching) and[compaction](https://www.taskade.com/wiki/ai-agents/context-compaction) matter more than raw window size.


Do cached tokens still count against the context window?


Yes. Caching changes what you pay for repeated tokens, not whether they occupy space. A cached prefix consumes exactly the share of the window it always did. Caching is a cost lever, not a capacity lever, and confusing the two is the most common long-context budgeting mistake.


Are context window sizes comparable across models?


Not directly. A token is defined by each model's tokenizer, so identical text produces different counts on different models. Anthropic notes its newer tokenizer produces approximately 30 percent more tokens for the same text than its previous one. Compare windows by how much of your actual content fits, not by the headline number.


What is context engineering and why did it replace prompt engineering?


[Context engineering](https://www.taskade.com/wiki/ai/context-engineering) is the practice of deciding what enters a window, in what order, and what leaves as a session grows.[Prompt engineering](https://www.taskade.com/wiki/ai/prompt-engineering) optimizes the instruction; context engineering optimizes retrieval, tool definitions, memory, compaction, and caching around it. It became dominant once windows grew past the point where filling them was accurate or affordable. Start with the[field guide](https://www.taskade.com/blog/context-engineering) and the[agent harness explainer](https://www.taskade.com/blog/agent-harness-explained) .


How does Taskade handle context across long-running work?


Taskade keeps durable context in the workspace instead of the chat window, so projects, knowledge sources, and[agent memory](https://www.taskade.com/blog/ai-agent-memory) persist across sessions and an agent recovers state on the next run. Work routes across 15+ frontier models from OpenAI, Anthropic, Google, and open-weight providers, and paid plans start at $10 per month billed annually. See[pricing](https://www.taskade.com/pricing) or start on[Taskade Genesis](https://www.taskade.com/create) .


---


## The Window Was Never the Point


Eight years of context window history follow one pattern. Every time the window got bigger, the constraint moved rather than disappeared. At 512 tokens the constraint was memory. At 200,000 it was recall. At 1,000,000 it is cost and curation, and by 2026 even the cost premium is gone, leaving curation alone.


Which means the winning move in 2026 is the opposite of the one that won in 2023. Then, you bought the biggest window you could. Now, you put the smallest correct thing in it, and you keep everything else somewhere durable: a workspace, a knowledge base, a memory store. The paper tape got infinitely long. The reading head still has to be pointed at the right part of it.


Build on structured context instead of a bigger prompt with[Taskade Genesis](https://www.taskade.com/create) , and see what the pattern looks like in production across the[community gallery](https://www.taskade.com/community) .


▲ ■ ●
