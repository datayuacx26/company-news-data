---
schema_version: "1.0.0"
document_id: "1b887115d35d1c0f594591a21ce2cdc8e32bed13176f7ece9bbb003cd7bcc7ad"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/reasoning-tokens-and-memory-8x-cost-reduction-test"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T20:09:45.452514+00:00"
fetched_at: "2026-07-31T20:09:46.269261+00:00"
content_hash: "sha256:b56ccdb70a8166ce9ce441ed8238fcd2f073dc9a32f173e293679cd90c5bfcca"
---

# Reasoning Tokens and Memory: 8x Cost Reduction Test

Reasoning models are expensive; a GPT-5 mini at high reasoning effort could easily consume 5k to 7k tokens to answer just a single question. That's the token burden most developers complain about.


As developers, we want three things at once: the model should remember everything, the answers it produces should be accurate, and the cost should remain as low as possible. Normally, we can only achieve two. If we keep everything in context and answers remain accurate, the cost keeps rising with every turn. If we trim context to save on cost, then the model has to cut its context, leading to knowledge loss.


So, I took it upon myself to test if adding memory to reasoning models would actually help.


## **What "breaking the trilemma" would actually look like**


Let’s get this straight: we want to test whether memory only made things cheaper by making them dumber, or whether it yields the same high-quality results at much lower cost and with fewer tokens. The bar was:


1.


Reduce the cost and tokens, not by a rounding error


2.


The quality of the output should hold, and the model should recall the facts as they are.


3.


It should work on more than one model family, i.e, not vendor-specific.


For the setup, I chose two main reasoning models from different vendors and an LLM as a judge to verify the quality of the answers they produced.


-


**GPT-5 mini** from OpenAI with its reasoning depth controlled by` reasoning_effort` .


-


**Gemini 3.6 Flash** from Google, with its reasoning depth controlled by` reasoning.effort` .


-


**Claude-Haiku 4.5** from Anthropic, chosen as a neutral third vendor (our judge) with temperature set to 0 for reproducibility and to avoid self-preference bias.


Both models were pinned to “high” for reasoning effort, while each had different thinking implementations, different token accounting, and different API surfaces. If adding memory to both actually helps, then adding memory is worth it.


**Note:** The comparison that matters here is not GPT versus Gemini; it is memory versus no-memory, measured on each model independently.


## **The setup**


The setup has three parts: A dataset, two conditions, and a judge with a small set of rules that keep the comparison fair.


### **The dataset**


I built a[LoCoMo benchmark](https://www.emergentmind.com/topics/locomo) -inspired 5-conversation dataset for this experiment. The original LoCoMo dataset consists of 50 conversations, up to 35 sessions each, around 300 turns per conversation, and roughly 200 question-answer pairs, spanning single-hop, multi-hop, temporal, and open-domain questions.


The dataset is small, but deliberately content-rich, and every conversation covers a different memory-eval category. Each conversation carries an anchor turn with the fact, three filler turns that are plausible but irrelevant, and a recall turn that asks the question. The filler exists to make the naive condition pay for context it does not need, and to give retrieval a chance to pick the wrong thing.


Here is a glimpse of the dataset I created:


**Note:** Five conversations are illustrative, not a benchmark.


```text
{
"id"  :    "loco_1_single_hop_port"  ,
"category"  :    "single_hop_recall_with_distractors"  ,
"anchor_turn"  :    {
"role"  :    "user"  ,
"content"  :    "Quick infra note: the production billing service listens on
port   8443  ,    behind   the   internal   load   balancer  .  Remember    8443    is
specifically   the   billing   service    in    prod  . "
}  ,
"filler_turns"  :    [
{    "role"  :    "user"  ,    "content"  :    "Separately, the auth service runs on port 9000
and   the   metrics   collector   is   on   3001.    What  's a clean way to
document   a   service  - to  - port   mapping  ? " },
{    "role"  :    "user"  ,    "content"  :    "We expose a health-check endpoint on 8080 across
every   service.  Share   one port convention ,    or   per- service  ? " },
{    "role"  :    "user"  ,    "content"  :    "The staging billing service is on a different
port   than    prod.  How   to keep staging and prod ports from colliding? " }
] ,
"recall_turn"  :    {
"role"  :    "user"  ,
"content"  :    "I'm writing the prod load-balancer rule for billing. Which port
does the production billing service  listen   on? "
}  ,
"expected_answer"  :    "8443"  ,
"wrong_answer_signal"  :    "9000"
}
```


```text
{
"id"  :    "loco_1_single_hop_port"  ,
"category"  :    "single_hop_recall_with_distractors"  ,
"anchor_turn"  :    {
"role"  :    "user"  ,
"content"  :    "Quick infra note: the production billing service listens on
specifically   the   billing   service    in    prod  . "
}  ,
"filler_turns"  :    [
and   the   metrics   collector   is   on   3001.    What  's a clean way to
document   a   service  - to  - port   mapping  ? " },
every   service.  Share   one port convention ,    or   per- service  ? " },
port   than    prod.  How   to keep staging and prod ports from colliding? " }
] ,
"recall_turn"  :    {
"role"  :    "user"  ,
"content"  :    "I'm writing the prod load-balancer rule for billing. Which port
does the production billing service  listen   on? "
}  ,
"expected_answer"  :    "8443"  ,
"wrong_answer_signal"  :    "9000"
}
```


```text
{
"id"  :    "loco_1_single_hop_port"  ,
"category"  :    "single_hop_recall_with_distractors"  ,
"anchor_turn"  :    {
"role"  :    "user"  ,
"content"  :    "Quick infra note: the production billing service listens on
specifically   the   billing   service    in    prod  . "
}  ,
"filler_turns"  :    [
and   the   metrics   collector   is   on   3001.    What  's a clean way to
document   a   service  - to  - port   mapping  ? " },
every   service.  Share   one port convention ,    or   per- service  ? " },
port   than    prod.  How   to keep staging and prod ports from colliding? " }
] ,
"recall_turn"  :    {
"role"  :    "user"  ,
"content"  :    "I'm writing the prod load-balancer rule for billing. Which port
does the production billing service  listen   on? "
}  ,
"expected_answer"  :    "8443"  ,
"wrong_answer_signal"  :    "9000"
}
```


The` anchor_turn` holds the fact (8443), the three` filler_turns` are plausible-but-irrelevant and seed distractor ports (9000, 3001, 8080) to bait retrieval, and the` recall_turn` asks the question. The other four conversations follow the same shape for multi-hop, temporal-update, preference, and numeric-recall categories.


### **The two conditions**


Every test conversation runs through each model twice under the following two conditions:


#### *Fig: Condition A and Condition B*


#### **Condition A (Naive full history)**


Under this condition, at the final question, we resend the entire conversation, including the anchor turn that holds the fact, all filler turns, and the recall question. This acts as the baseline for the experiment.


#### **Condition B (Memory supported via Mem0)**


For this condition, we store the pre-recall turns in[Mem0](https://mem0.ai/) with` infer=True.` It runs through an LLM that extracts the facts, decisions, and preferences worth keeping. At recall time, we search Mem0 for the current question and inject the model context only with what comes back, along with the question.


The difference between these two conditions makes up the result for this experiment. Condition A gives the model everything, while condition B gives it a short, extracted memory from the retrieval.


### **LLM as a judge**


The first version of my experiment used a substring check, which meant that if it didn’t find the right word, then it would say that the response was wrong. It took me one run to see how broken that is. An answer that said "use pytest, not unittest" got marked wrong because the string "unittest" appeared, even though the answer was correct.


So, I replaced it with an LLM judge (` anthropic/claude-haiku-4.5` ) from a different vendor, with temperature set to 0 for reproducibility. Its only job was to decide whether the answer endorses the expected fact as its recommendation, not whether a word appears or not. This changed the difference between a quality column that measures recall and one that measures coincidence.


### **Rules**


Three rules held across every run:


1.


**Fresh memory per cell:** Before each conversation's memory got populated, the store for that model and conversation was wiped, so a rerun never read what a previous run left behind.


2.


**Matched reasoning tier:** Both models run at` "high"` effort. The reasoning knob is the one control both APIs share.


3.


**Different contexts:** For a given model and conversation, the naive run and the memory run differ only in what context the model receives.


The only thing that changes between condition A and condition B is where the context comes from: the full transcript or a searched memory.


## **Results**


Before we move to the results of this experiment, here is a point worth noting. Mem0's hosted` add()` operation with` infer=True` is asynchronous, which means it returns immediately with` {"status": "PENDING"}` and does the extraction server-side. This is actually the right design for a write path you do not want to block on. If you search at this point when Mem0 is actually writing, you will get empty results.


A simple fix to this is batch-populate, i.e, fire up all the` add()` calls up front, then poll` search()` once, in parallel, until each store returns something, and only then run the queries. This won’t cost you any tokens, and your model will never see an empty memory response.


Now, with everything working, I ran five full conversations with two models under both conditions and scored their responses with a judge. Here's what I got:


**Model**


**Condition**


**Avg input tok**


**Avg output tok**


**Avg cost**


GPT-5 mini


naive full history


186


9,651


$0.0193


GPT-5 mini


mem0


113


1,136


$0.0023


Gemini 3.6 Flash


naive full history


181


3,228


$0.0245


Gemini 3.6 Flash


mem0


146


660


$0.0052


The input tokens do drop, from 186 to 113 on GPT-5 mini, and from 181 to 146 on Gemini, because the retrieved memory is a short extracted fact rather than the full transcript. But the significant number is the output token column. GPT-5 mini's average output tokens fell from 9,651 to 1,136 tokens. That is the reasoning collapsing.


In the naive condition, the model receives a five-turn transcript full of distractor questions, and it reasons over all of it. In the memory condition, it receives one clean fact and one question, and it reasons briefly and answers. For a reasoning model, keeping it on task is worth more than shrinking the prompt because thinking tokens are the expensive ones.


***Fig: Cost per question, naive vs mem0, on both models.***


On GPT-5 mini, memory made the average question 8.4 times cheaper, and on Gemini 3.6 Flash, it was 4.7 times cheaper, all while maintaining quality. Across all ten memory cells, the judge scored nine correct, while the naive condition scored ten out of ten. Thus, memory provided a significant cost reduction at the expense of a single miss.


Upon investigating this single miss, I found that one session timeout was set to 30 minutes, then changed to 15 minutes later in the same conversation. When asked for the current value, Gemini said 15 and got it right, while GPT-5 mini said 30 and got it wrong. Here's the interesting part: Mem0 kept both values as separate memories and flagged the new one as overriding the old one. Gemini picked up on the override cue and used it, while GPT-5 mini focused on other details in the memory (who signed off, that it was for the first release) and stuck with the old number instead. So this wasn't memory failing; rather, it was two models reading the same well-annotated memory differently.


> **Note on accounting:** There are two places you pay tokens: writing to memory and answering a question. When storing a turn with` infer=True` , you use some tokens because Mem0 runs a small LLM to pull out the facts. But you pay that once, when the turn happens. Every question after that just retrieves a short memory. The naive approach skips the write step, but it pays the full cost of re-reading and re-reasoning over the entire history on every single question. So you store once, then save on every question that follows.


## **Conclusion**


The cost-quality trilemma is real for reasoning models because they carry the whole context from the history and pay the price twice: once to feed it and once to reason over it.


From this experiment, we learned that memory can bend this trilemma by not cutting the context but by handing a distilled version of the data your model needs. In this run, we saved not only on tokens, but our cost fell by roughly 5 to 8 times. A memory layer decides what the model can see; it does not decide how the model reasons over what it sees. You need both to be good, and you should test the second one on the model you deploy.


If you are running reasoning models with full historical context, then adding memory could save your reasoning tokens while maintaining the quality of the full context.


> [Mem0](https://app.mem0.ai/?utm_source=mem0_blog&utm_medium=kw_blog&utm_campaign=understanding_memory_benchmark&utm_content=understanding_memory_benchmark) is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.*
>
>
> Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/?utm_source=mem0_blog&utm_medium=kw_blog&utm_campaign=understanding_memory_benchmark&utm_content=understanding_memory_benchmark)
>
>
> Or self-host mem0 from our[open-source GitHub repository](https://github.com/mem0ai/mem0)


## **Frequently asked questions**


### Q. Is this a comparison of GPT-5 mini versus Gemini 3.6 Flash?


No. The comparison that matters is memory versus no-memory, measured on each model on its own. I ran two models from two different labs on purpose, so that if memory helps both, the result is about the mechanism and not about one vendor's stack. Which model is "better" is a different question I was not asking.


### Q. Where do the token savings actually come from?


Mostly from the output side. On GPT-5 mini, average output tokens fell from 9,651 to 1,136. In the naive condition, the model reasons over a full transcript full of distractor turns; with memory, it gets one clean fact and reasons briefly. Input tokens drop too, but for a reasoning model, the thinking tokens are the expensive ones, and keeping the model on task is what saves the most.


### Q. Is five conversations enough to trust the multiples?


The 8.4x and 4.7x figures are real for this run and repeat across two vendors, but the exact magnitude will shift with your conversations, reasoning tier, and how much filler your histories carry.


### Q. Does memory only help reasoning models?


No. Any model fed a growing conversation benefits from sending less of it. Reasoning models just benefit the most, because their thinking tokens ride on top of the context you resend, so trimming the context compounds. In this test, the saving showed up clearly on both models I tried, from two different labs.
