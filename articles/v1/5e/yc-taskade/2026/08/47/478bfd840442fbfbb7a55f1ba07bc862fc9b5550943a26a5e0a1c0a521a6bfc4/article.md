---
schema_version: "1.0.0"
document_id: "478bfd840442fbfbb7a55f1ba07bc862fc9b5550943a26a5e0a1c0a521a6bfc4"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/prompt-engineering-history"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T01:14:09.187087+00:00"
fetched_at: "2026-08-05T01:38:03.459815+00:00"
content_hash: "sha256:1bffea79df5f70e24dca8bb48e5d5042bd01caa48ba0e810348e8396f3f5235e"
---

# The History of Prompt Engineering: From Magic Words to Context Engineering (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


The History of Prompt…


On this page (16)


The machine in the photograph is a Teletype Model 33. In the 1960s and 1970s it was how most people talked to a computer: it printed a prompt, you typed a line, it printed a response. The word *prompt* meant the blinking invitation, not the thing you wrote.


Sixty years later the word flipped. The prompt is now the thing *you* write, and for a brief and strange period between 2020 and 2024 people were paid six figures to write it well.


This is the verified history of that period: where the techniques actually came from, which ones survived contact with better models, and what replaced the job.


> **TL;DR:** Prompt engineering began with the GPT-3 paper on[May 28, 2020](https://arxiv.org/abs/2005.14165) , peaked as a job title in 2023, and was largely absorbed into[context engineering](https://www.taskade.com/wiki/ai/context-engineering) by mid-2025. Most famous tricks are now obsolete. Few-shot examples, delimiters and schema-enforced outputs still work.[See how context lives in a workspace instead of a prompt box →](https://www.taskade.com/create)


---


## What Is Prompt Engineering, and When Did It Actually Start?


Prompt engineering is the practice of shaping a language model's input text to change its output, and it began the moment models became large enough to learn a task from examples in that input rather than from retraining. The canonical starting gun is[Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) by Tom B. Brown and colleagues, submitted May 28, 2020, which introduced a 175-billion-parameter model that could adapt to new tasks from a handful of examples typed into the prompt.


The idea did not arrive fully formed. Two earlier papers set it up:


- **[Language Models as Knowledge Bases?](https://arxiv.org/abs/1909.01066)** (Fabio Petroni et al., September 3, 2019) introduced the LAMA probe, which queried a pretrained model for facts using fill-in-the-blank cloze statements. No fine-tuning. Just carefully worded input.
- **[Exploiting Cloze Questions for Few Shot Text Classification](https://arxiv.org/abs/2001.07676)** (Timo Schick and Hinrich Schütze, January 21, 2020) reformulated classification tasks as cloze phrases so a model could understand them from a task description alone.


Both proved the same point before anyone had a name for it: **the input is a control surface** . When OpenAI opened the GPT-3 API in June 2020, thousands of people started discovering that control surface by trial and error, and a folk craft formed around it.


The name arrived with the literature.[Prompt Programming for Large Language Models](https://arxiv.org/abs/2102.07350) (Laria Reynolds and Kyle McDonell, February 15, 2021) argued that a well-designed zero-shot prompt could beat few-shot prompting, and treated prompts as programs rather than magic phrases. Five months later,[Pre-train, Prompt, and Predict](https://arxiv.org/abs/2107.13586) (Pengfei Liu et al., July 28, 2021) surveyed the field and named the paradigm shift: from *pre-train and fine-tune* to *pre-train, prompt, predict* .


Read the term definition in our dictionary:[prompt engineering](https://www.taskade.com/wiki/ai/prompt-engineering) ,[few-shot learning](https://www.taskade.com/wiki/ai/few-shot-learning) ,[zero-shot learning](https://www.taskade.com/wiki/ai/zero-shot-learning) .


---


## The Complete Prompt Engineering Timeline, 2019 to 2026


The single most useful artifact in this history is a dated list of what shipped and what it changed. Every entry below is anchored to a primary source.


Year What shipped What changed Why it mattered


**Sep 2019**[LAMA probe](https://arxiv.org/abs/1909.01066) Facts pulled from a frozen model with cloze sentences First proof the input alone could steer a model


**Jan 2020**[Pattern-Exploiting Training](https://arxiv.org/abs/2001.07676) Tasks rewritten as fill-in-the-blank phrases Task descriptions became a substitute for labels


**May 2020**[GPT-3 paper](https://arxiv.org/abs/2005.14165) In-context learning at 175B parameters The prompt became the interface


**Jun 2020** GPT-3 API private beta Thousands of non-researchers start prompting A folk craft forms outside the lab


**Feb 2021**[Prompt Programming](https://arxiv.org/abs/2102.07350) A good zero-shot prompt beats few-shot Prompts framed as programs, not spells


**Jul 2021**[Pre-train, Prompt, Predict](https://arxiv.org/abs/2107.13586) The paradigm gets a survey and a name Academic legitimacy


**Jan 2022**[Chain-of-thought prompting](https://arxiv.org/abs/2201.11903) Eight worked examples set a GSM8K record Reasoning becomes promptable


**Mar 2022**[InstructGPT](https://arxiv.org/abs/2203.02155) A 1.3B tuned model preferred over 175B GPT-3 Instruction tuning starts eating the tricks


**May 2022**[Let's think step by step](https://arxiv.org/abs/2205.11916) MultiArith 17.7% to 78.7% with one phrase The magic-words era peaks


**Jun 2022**[PromptBase](https://techcrunch.com/2022/07/29/a-startup-is-charging-1-99-for-strings-of-text-to-feed-to-dall-e-2/) Prompts sold from $1.99, platform took 20% Prompts briefly become a product


**Sep 2022**[Prompt injection named](https://simonwillison.net/2022/Sep/12/prompt-injection/) Instructions in data hijack instructions in prompts The security bill arrives


**Oct 2022**[ReAct](https://arxiv.org/abs/2210.03629) Reasoning interleaved with tool actions The prompt becomes a loop, not a string


**Nov 2022** ChatGPT launches Prompting goes mass-market Everyone becomes a prompt engineer


**2023** Prompt engineer job postings Salaries reported up to $335,000 Peak hype


**Oct 2023**[DSPy](https://arxiv.org/abs/2310.03714) Pipelines compiled instead of prompts hand-written Optimization replaces intuition


**Mar 2024**[IEEE Spectrum: prompt engineering is dead](https://spectrum.ieee.org/prompt-engineering-is-dead) Auto-generated prompts beat hand-tuned ones The job starts to dissolve


**Aug 2024**[Structured Outputs](https://openai.com/index/introducing-structured-outputs-in-the-api/) 100% schema match versus under 40% prompt-only Formatting moves out of the prompt


**Sep 2024** OpenAI o1 reasoning models Providers advise against chain-of-thought prompts The model absorbs the technique


**Nov 2024**[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) A standard way to attach tools and data Context becomes plumbing


**Jun 2025**[Context engineering named](https://simonwillison.net/2025/Jun/27/context-engineering/) Tobi Lütke and Andrej Karpathy adopt the term The discipline is renamed


**Jul 2025**[Context rot report](https://www.trychroma.com/research/context-rot) 18 models degrade as input grows More context is not better context


**Sep 2025**[Anthropic context engineering guide](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) Compaction, notes, sub-agents formalized Best practice published by a model lab


**2026**[AGENTS.md](https://agents.md/) at 60,000+ repos Instructions committed as files Context becomes durable infrastructure


---


## Era 1: The Magic Words Era, 2020 to 2022


Between GPT-3's release and ChatGPT's launch, prompting was almost entirely empirical, because nobody could see inside the model and nobody had published a theory of what worked. Practitioners traded phrasings the way anglers trade lures, and a real discovery was indistinguishable from a coincidence.


Then came the discovery that defined the era.[Large Language Models are Zero-Shot Reasoners](https://arxiv.org/abs/2205.11916) (Takeshi Kojima et al., May 24, 2022) found that prepending a single sentence, *Let's think step by step* , lifted MultiArith accuracy from **17.7 percent to 78.7 percent** and GSM8K from **10.4 percent to 40.7 percent** on the same model with no examples added.


That result was real, reproducible, and deeply misleading about the future. It taught a generation of users that the right words were worth more than the right information, and the folklore that followed was mostly wrong.


Era Signature technique Example prompt Why it stopped working


**2020 to 2021** Few-shot exemplars` Q: ... A: ...` repeated three times, then the real question Still works. Instruction tuning just made it optional for simple tasks


**2021 to 2022** Role framing *You are a world-class expert mathematician* Personas were shown not to improve factual accuracy


**2022** Trigger phrases *Let's think step by step* Reasoning models now do this internally by default


**2022 to 2023** Format begging *Respond ONLY with valid JSON, no other text* Replaced by schema enforcement at the API level


**2023** Incentives and pressure *I'll tip you $200* / *this is critical to my career* No significant effect on benchmark performance


**2023 to 2024** Mega-prompts A 4,000-word instruction block covering every edge case Accuracy degrades as input grows, well before the limit


The single best illustration of how fragile this era's knowledge was: Google DeepMind's[Large Language Models as Optimizers](https://arxiv.org/abs/2309.03409) (Chengrun Yang et al., September 7, 2023) let a model search for its own best instruction. When PaLM 2-L-IT did the searching, the top instruction it found for GSM8K was *Take a deep breath and work on this problem step-by-step* , scoring 80.2 percent. It spread across the internet as universal advice. It was an artifact of one search on one benchmark: swap in a different optimizer and the paper's own table returns a different winner, *Break this down* , at 79.9 percent.


---


## Era 2: Reasoning Moves Into the Prompt, 2022 to 2023


The second era replaced single phrases with prompt *structures* , and it produced the only techniques from this period that are still architecturally relevant in 2026. The pivot paper was[Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903) (Jason Wei et al., January 28, 2022), which showed that **eight** worked examples containing intermediate steps let a 540-billion-parameter model set a new state of the art on GSM8K.


What followed compounded quickly:


- **[Self-consistency](https://arxiv.org/abs/2203.11171)** (March 2022): sample several reasoning paths, take the majority answer. See[self-consistency](https://www.taskade.com/wiki/ai/self-consistency) .
- **[ReAct](https://arxiv.org/abs/2210.03629)** (Shunyu Yao et al., October 6, 2022): interleave reasoning traces with tool actions, reporting a 34-point absolute success-rate improvement on ALFWorld over imitation and reinforcement learning baselines while using only one or two in-context examples. See[the ReAct pattern](https://www.taskade.com/wiki/ai/react-pattern) .
- **[Tree of Thoughts](https://arxiv.org/abs/2305.10601)** (May 2023): branch, evaluate, backtrack. See[tree of thoughts](https://www.taskade.com/wiki/ai/tree-of-thoughts) .


ReAct is the hinge of the whole history. Before it, a prompt was a string you sent once. After it, a prompt was a **loop** : think, act, observe, think again. Everything we now call an agent descends from that shape, which is why the[agent loop](https://www.taskade.com/wiki/ai-agents/agent-loop) reads like ReAct with better tooling.


Read more on how the reasoning layer works in[How Do Large Language Models Work?](https://www.taskade.com/blog/how-do-llms-work) and[chain of thought](https://www.taskade.com/wiki/ai/chain-of-thought) .


---


## Era 3: The Gold Rush, 2022 to 2023


For roughly eighteen months, prompt engineering was treated as a profession rather than a skill, and the market briefly priced a text string as an asset.[PromptBase](https://techcrunch.com/2022/07/29/a-startup-is-charging-1-99-for-strings-of-text-to-feed-to-dall-e-2/) opened in June 2022 selling prompts for image and text models from $1.99 each, taking a 20 percent cut of every sale. Anthropic's *Prompt Engineer and Librarian* listing, reported by[Fortune in March 2023](https://fortune.com/2023/03/09/new-ai-jobs-chatgpt-like-assistants/) , carried a range reported up to **$335,000** , and the posting itself noted the field had existed for under two years.


The gold rush produced one durable contribution, and it was a warning rather than a technique. On[September 12, 2022](https://simonwillison.net/2022/Sep/12/prompt-injection/) , Simon Willison named **prompt injection** after Riley Goodside demonstrated that a line of text inside untrusted data could override the developer's instructions. Willison chose the name because the mechanics mirrored SQL injection.


Prompt injection is the one 2022-era phenomenon that has not been solved by better models. It is a structural consequence of mixing instructions and data in a single channel, and it is the reason[agent permissions](https://www.taskade.com/wiki/ai-agents/agent-permissions) and sandboxing matter more than clever wording.


---


## Era 4: The Model Ate the Prompt, 2023 to 2025


The techniques did not fail because they were wrong. They stopped mattering because model training absorbed them, which is the normal fate of a workaround. The turning point predates the hype peak:[Training language models to follow instructions with human feedback](https://arxiv.org/abs/2203.02155) (Long Ouyang et al., March 4, 2022) reported that outputs from a **1.3-billion-parameter** InstructGPT model were preferred over a **175-billion-parameter** GPT-3, despite 100x fewer parameters. Once models were tuned to follow instructions, elaborate framing bought less and less.


Four forces finished the job:


**1. Reasoning models internalized chain of thought.** OpenAI's own[reasoning guidance](https://developers.openai.com/api/docs/guides/reasoning-best-practices) tells developers to avoid chain-of-thought prompts, because "these models perform reasoning internally" and asking them to think step by step is unnecessary. See[reasoning models](https://www.taskade.com/wiki/ai/reasoning-models) .


**2. Formatting moved into the API.**[Structured Outputs](https://openai.com/index/introducing-structured-outputs-in-the-api/) (August 6, 2024) reached **100 percent** schema match in OpenAI's evaluations, versus under **40 percent** for the previous model without it. Asking nicely for JSON became obsolete the day you could declare a schema. See[structured outputs](https://www.taskade.com/wiki/ai/structured-outputs) and[function calling](https://www.taskade.com/wiki/ai/function-calling) .


**3. Optimizers outperformed humans.**[IEEE Spectrum](https://spectrum.ieee.org/prompt-engineering-is-dead) reported in March 2024 that VMware researchers found automatically generated prompts beat the best hand-tuned prompt "in almost every case."[DSPy](https://arxiv.org/abs/2310.03714) (October 5, 2023) replaced hand-written templates with compiled pipelines.[GEPA](https://arxiv.org/abs/2507.19457) (July 25, 2025) reported outperforming reinforcement learning by up to **20 percent** while using up to **35x fewer rollouts** . Anthropic shipped a prompt improver in its developer console in November 2024.


**4. Models became more literal.** OpenAI's[GPT-4.1 prompting guide](https://developers.openai.com/cookbook/examples/gpt4-1_prompting_guide) states the model "is trained to follow instructions more closely and more literally than its predecessors," and that getting the most from it "will require some prompt migration." Prompts tuned to coax intent out of a model that inferred liberally now over-steer a model that does exactly what it is told.


The job title thinned out accordingly. Reported hiring data through 2025 shows prompt engineer postings collapsing as the skill folded into AI engineering and product roles. Treat the specific percentages you see quoted as approximate: the underlying signal, that a standalone prompt-writing role stopped being a distinct hire, is consistent across sources.


---


## Era 5: Context Engineering, 2025 to 2026


Context engineering is prompt engineering's successor discipline, and it is defined by scope: instead of tuning one instruction, you curate everything the model can see. The label went mainstream in **June 2025** , when Shopify's Tobi Lütke posted that he preferred "context engineering" over "prompt engineering" and Andrej Karpathy amplified it, a moment[documented by Simon Willison on June 27, 2025](https://simonwillison.net/2025/Jun/27/context-engineering/) .


Anthropic gave it a working definition on[September 29, 2025](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) : prompt engineering covers "methods for writing and organizing LLM instructions," while context engineering covers "strategies for curating and maintaining the optimal set of tokens during LLM inference, including all the other information that may land there outside of the prompts."


The two disciplines answer different questions, which is why one replaced the other rather than competing with it:


Question Prompt engineering Context engineering


**What do you tune?** The wording of one instruction Everything in the window: tools, retrieval, memory, history


**What is the failure mode?** The model misreads your intent The model has the wrong information, or too much of it


**How do you improve it?** Rewrite and re-test by hand Change what gets retrieved, compacted or delegated


**What ships?** A string in a text box Files, tool definitions and a memory store


The reason the discipline had to change is measurable. Two findings broke the assumption that a bigger window is a better window:


- **[Lost in the Middle](https://arxiv.org/abs/2307.03172)** (Nelson F. Liu et al., July 6, 2023): performance is highest when relevant information sits at the beginning or end of the input and "significantly degrades" when the model must retrieve it from the middle.
- **[Context Rot](https://www.trychroma.com/research/context-rot)** (Chroma, July 14, 2025): across 18 frontier models, accuracy declines non-uniformly as input length grows, on tasks as simple as retrieval. See[context rot](https://www.taskade.com/wiki/ai/context-rot) .


Anthropic's published techniques for long-horizon work name the new toolkit directly: **compaction** (summarize history and restart with the compressed version), **structured note-taking** (persist memory outside the window), **sub-agent architectures** (delegate and return condensed summaries), and **just-in-time retrieval** (load data with tools instead of pre-stuffing it). Those map to[context compaction](https://www.taskade.com/wiki/ai-agents/context-compaction) ,[agent memory](https://www.taskade.com/wiki/ai-agents/agent-memory) ,[subagents](https://www.taskade.com/wiki/ai-agents/subagents) and[retrieval-augmented generation](https://www.taskade.com/wiki/ai/retrieval-augmented-generation) .


Two pieces of shared infrastructure finished the transition. The[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) , open-sourced November 25, 2024, standardized how tools and data attach to a model.[AGENTS.md](https://agents.md/) , now stewarded by the Agentic AI Foundation under the Linux Foundation, put durable project instructions in a committed file rather than a chat box, and is used by **more than 60,000** open-source repositories.


Here is the budget the modern practitioner is actually managing, and it is nothing like a text box:


```text
A 2026 AGENT TURN: WHERE THE TOKENS GO
======================================================================
LAYER                 TYPICAL SHARE   WHO CONTROLS IT   DECAYS?
--------------------  --------------  ----------------  ----------
System instructions     2 -  5 %      you, once         no
Tool definitions        5 - 20 %      your integrations no
Retrieved documents    20 - 50 %      your retriever    yes, fast
Memory / prior notes    5 - 20 %      your memory store yes
Message history        10 - 60 %      the conversation  yes, fast
The user prompt         1 -  3 %      the human         no
--------------------  --------------  ----------------  ----------
OLD PROMPT ENGINEERING OPERATED ON THE LAST ROW.
CONTEXT ENGINEERING OPERATES ON ALL SIX.
======================================================================


```


For a practical walkthrough, see our[context engineering field guide](https://www.taskade.com/blog/context-engineering) and[Productize Context Engineering](https://www.taskade.com/blog/productize-context-engineering) .


---


## Does This Old Prompt Trick Still Work in 2026?


The honest answer for most famous techniques is no, and the reason is almost always that a model update or an API feature absorbed the behavior. This table is the part of prompt engineering history nobody publishes, because it involves admitting which advice expired.


Trick Peak years Verdict in 2026 What the evidence says


**Few-shot examples** 2020 to now **Still works.** Highest-leverage move you have The original[GPT-3 result](https://arxiv.org/abs/2005.14165) ; still first-line in every provider guide


**Delimiters, XML tags, section headings** 2023 to now **Still works** OpenAI's[reasoning guidance](https://developers.openai.com/api/docs/guides/reasoning-best-practices) explicitly recommends markdown, XML tags and section titles


**Retrieving the right document** 2023 to now **Still works, and matters more each year** Just-in-time retrieval is a named Anthropic technique; see[RAG](https://www.taskade.com/wiki/ai/retrieval-augmented-generation)


**"Let's think step by step"** 2022 to 2023 **Mostly obsolete** on reasoning models Huge in[2022](https://arxiv.org/abs/2205.11916) ;[Wharton 2025](https://arxiv.org/abs/2506.07142) found negligible gains and 20 to 80% more latency on reasoning models


**"Act as a world-class expert"** 2023 **Obsolete for accuracy.** Fine for tone[162 roles, 2,410 questions, 4 model families](https://arxiv.org/abs/2311.10054) : no improvement, often slightly worse


**"Take a deep breath and work on this step-by-step"** 2023 **Obsolete.** Never generalize a per-model artifact[OPRO](https://arxiv.org/abs/2309.03409) found it only when PaLM 2-L-IT was the optimizer; a different optimizer found a different winner


**Tipping and threats** 2023 to 2024 **No effect**[Wharton Report 3](https://arxiv.org/abs/2508.00614) on GPQA Diamond and MMLU-Pro found no significant average effect


**Emotional stimuli** 2023 **Weak and unreliable**[EmotionPrompt](https://arxiv.org/abs/2307.11760) reported gains on 2023-era models; not established as a general rule since


**"Respond ONLY with valid JSON"** 2022 to 2023 **Superseded by an API parameter**[Structured Outputs](https://openai.com/index/introducing-structured-outputs-in-the-api/) : 100% schema match versus under 40% prompt-only


**"Do not hallucinate"** 2023 **Obsolete as an instruction** Grounding comes from retrieval and citations, not from a prohibition; see[hallucinations](https://www.taskade.com/wiki/ai/hallucinations)


**Mega-prompts that cover every case** 2023 to 2024 **Actively harmful**[Lost in the Middle](https://arxiv.org/abs/2307.03172) and[context rot](https://www.trychroma.com/research/context-rot)


**Hand-tuning wording by trial and error** 2022 to 2024 **Replaced by optimizers**[DSPy](https://arxiv.org/abs/2310.03714) ,[GEPA](https://arxiv.org/abs/2507.19457) , provider-side prompt improvers


**"Ignore previous instructions"** 2022 to now **Still works, and that is the problem**[Prompt injection](https://simonwillison.net/2022/Sep/12/prompt-injection/) remains structurally unsolved


The pattern is consistent. **Techniques that supply information survived. Techniques that supply pressure did not.**


---


## What Actually Still Works in 2026


Five habits carry almost all the remaining value, and none of them involve a magic phrase. They are the operational residue of six years of research.


1. **Show, do not describe.** Two or three real examples of the output you want outperform a paragraph describing it. This is the oldest technique in the field and the last one standing.
2. **Say the specific thing once, clearly.** OpenAI's GPT-4.1 guidance notes that "a single sentence firmly and unequivocally clarifying your desired behavior is almost always sufficient." Repetition and emphasis are not substitutes for specificity.
3. **Structure the input.** Headings, XML tags and explicit sections beat a wall of prose, because they let the model locate what you are referring to.
4. **Enforce format at the boundary, not in the prose.** Use a schema. See[structured outputs](https://www.taskade.com/wiki/ai/structured-outputs) .
5. **Curate before you add.** Every token you add competes for attention with every token already there. Retrieve narrowly, compact aggressively, and treat the window as a budget rather than a container.


A sixth, which is really a governance point: **write the durable parts down as files** . A committed instruction file such as[AGENTS.md](https://www.taskade.com/wiki/ai-agents/agents-md) or a workspace knowledge base survives the session. A clever prompt in a chat box does not.


---


## How This Plays Out Inside a Workspace


The practical consequence of the shift from prompts to context is that the interesting work moved from the text box to the system around it, which is exactly the layer a workspace owns.[Taskade Genesis](https://www.taskade.com/create) turns one prompt into a working app, and the app keeps its own projects, agents and automations, so the context that made the first output good is still there for the tenth.


Three parts do the work:


- **Memory.**[Taskade EVE](https://www.taskade.com/learn/genesis/eve-memory) holds project context across sessions, so you are not re-pasting background into every conversation. That is structured note-taking with a user interface.
- **Intelligence.**[AI agents](https://www.taskade.com/agents) run with a large built-in toolset, web search, file analysis, custom commands and persistent memory, across 15+ frontier models from OpenAI, Anthropic, Google and open-weight providers.
- **Execution.**[Automations](https://www.taskade.com/automate) connect 100+ bidirectional integrations, so a triggered event pulls real data into context and a completed step pushes results back out.


Work lands in 7 project views, List, Board, Calendar, Table, Mind Map, Gantt and Org Chart, so the output of an agent is something a team can actually operate rather than a block of chat text. If you want the starting points rather than the theory, the[prompt guide](https://www.taskade.com/learn/genesis/prompt-guide) ,[your first prompt](https://www.taskade.com/learn/genesis/first-prompt) and[agent prompts](https://www.taskade.com/learn/agents/agent-prompts) cover the practical patterns, and[Taskade Community](https://www.taskade.com/community) has working examples to clone.


Paid plans start at **$10 per month billed annually** .[Start building free →](https://www.taskade.com/create)


---


## Frequently Asked Questions


When did prompt engineering start?


The practice began with the[GPT-3 paper](https://arxiv.org/abs/2005.14165) on May 28, 2020, which showed a model could learn a task from examples in its input rather than from weight updates. It was foreshadowed by the[LAMA probe](https://arxiv.org/abs/1909.01066) in September 2019 and[Pattern-Exploiting Training](https://arxiv.org/abs/2001.07676) in January 2020. The label entered common use in 2021.


Who invented chain-of-thought prompting?


Jason Wei and colleagues at Google, in[Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903) , submitted January 28, 2022. Four months later Takeshi Kojima and colleagues showed a similar effect with zero examples using a single trigger sentence.


Does "Let's think step by step" still work in 2026?


Rarely. It produced enormous gains in 2022 and produces little on modern reasoning models, which reason internally. OpenAI's reasoning guidance advises against adding chain-of-thought instructions, and a[2025 Wharton study](https://arxiv.org/abs/2506.07142) measured 20 to 80 percent more response time for negligible accuracy benefit on those models.


What is context engineering and how is it different from prompt engineering?


Prompt engineering is writing and organizing instructions.[Context engineering](https://www.taskade.com/wiki/ai/context-engineering) is curating everything that reaches the model during inference: tools, retrieved documents, memory, message history and instructions together. Anthropic published that distinction on September 29, 2025.


Is prompt engineering dead?


The standalone job largely disappeared; the skill became a baseline.[IEEE Spectrum](https://spectrum.ieee.org/prompt-engineering-is-dead) ran that headline in March 2024 after researchers found automatically generated prompts beat hand-tuned ones in almost every case. The work moved into context engineering and AI engineering roles.


Do AI personas like "act as an expert" improve accuracy?


No. A study spanning[162 roles and 2,410 factual questions across four model families](https://arxiv.org/abs/2311.10054) found personas in system prompts do not improve accuracy and often slightly reduce it. They remain useful for tone and format.


Does offering to tip or threatening a model improve results?


No.[Wharton Report 3](https://arxiv.org/abs/2508.00614) tested tips and threats on GPQA Diamond and MMLU-Pro and found no significant effect on average performance, though individual questions varied.


What is context rot?


The measured decline in accuracy as input length grows, well before the advertised limit.[Chroma's July 2025 report](https://www.trychroma.com/research/context-rot) found it across 18 frontier models, including on simple retrieval tasks.


What prompting techniques still work in 2026?


Few-shot examples, clear delimiters, schema-enforced outputs, narrow retrieval and short specific instructions. Everything that supplies information survived. Everything that supplied social pressure did not.


What replaced hand-tuning prompts?


Automated optimizers and provider tooling.[DSPy](https://arxiv.org/abs/2310.03714) compiles pipelines instead of strings, and[GEPA](https://arxiv.org/abs/2507.19457) reported beating reinforcement learning by up to 20 percent with up to 35x fewer rollouts.


How does Taskade handle prompting and context?


[Taskade Genesis](https://www.taskade.com/create) turns a prompt into a working app whose projects, agents and automations persist, so context is reused rather than retyped. Taskade EVE keeps memory across sessions, agents connect to 100+ bidirectional integrations, and work is viewable in 7 project views. Paid plans start at $10 per month billed annually.


What is AGENTS.md and why does it matter?


An open Markdown format for project-specific agent instructions, stewarded by the Agentic AI Foundation under the Linux Foundation and used by more than 60,000 repositories. It is context engineering as a committed file rather than a retyped paragraph.


---


## The Prompt Was Never the Point


The Teletype printed a prompt because the machine needed to tell you it was ready. For four years we inverted that, and treated our side of the exchange as the thing that carried the intelligence. The research says otherwise: the phrases that felt powerful were mostly measuring the models' shortcomings, and each new model release quietly deleted another one.


What survived is unglamorous and durable. Give the model the right information, in a structure it can navigate, with tools it can call and memory it can keep. That is not a trick. That is a system, and systems are what a workspace is for.


Memory, intelligence, execution. **▲ ■ ●**


[Build your first Taskade Genesis app free →](https://www.taskade.com/create)


---


## Further Reading


### The Dictionary


- [Prompt Engineering](https://www.taskade.com/wiki/ai/prompt-engineering) and[Context Engineering](https://www.taskade.com/wiki/ai/context-engineering) side by side
- [Context Window](https://www.taskade.com/wiki/ai/context-window) ,[Token](https://www.taskade.com/wiki/ai/token) and[Knowledge Cutoff](https://www.taskade.com/wiki/ai/knowledge-cutoff)
- [System Prompt](https://www.taskade.com/wiki/ai/system-prompt) and[Prompt Chaining](https://www.taskade.com/wiki/ai/prompt-chaining)
- [Parametric Knowledge](https://www.taskade.com/wiki/ai/parametric-knowledge) and[Prompt Caching](https://www.taskade.com/wiki/ai/prompt-caching)
- [Non-Determinism](https://www.taskade.com/wiki/ai/non-determinism) and[Sycophancy](https://www.taskade.com/wiki/ai/sycophancy)
- [Model Context Protocol](https://www.taskade.com/wiki/ai/model-context-protocol) and[The Agent Harness](https://www.taskade.com/wiki/ai-agents/agent-harness)


### Related Histories and Deep Dives


- [The History of Workflow Automation](https://www.taskade.com/blog/automation-history) with the same timeline treatment
- [History of Mermaid.js](https://www.taskade.com/blog/history-of-mermaid) on diagrams as code
- [What Is Agentic Engineering?](https://www.taskade.com/blog/what-is-agentic-engineering)
- [The Agent Harness Explained](https://www.taskade.com/blog/agent-harness-explained) and[Agent Evals Explained](https://www.taskade.com/blog/agent-evals-explained)
- [Types of Memory in AI Agents](https://www.taskade.com/blog/ai-agent-memory)
- [AI Prompt Engineering: Human-Machine Interaction](https://www.taskade.com/blog/ai-prompt-engineering) , the 2023 primer this history replaces
- [They Generate Code, We Generate Runtime](https://www.taskade.com/blog/code-vs-runtime)


### Explore Taskade


- [Taskade Genesis](https://www.taskade.com/create) turns one prompt into a living app
- [AI Agents](https://www.taskade.com/agents) with tools, memory and multi-model collaboration
- [Automations](https://www.taskade.com/automate) across 100+ bidirectional integrations
- [Integrations](https://www.taskade.com/integrations) and[Community](https://www.taskade.com/community)
