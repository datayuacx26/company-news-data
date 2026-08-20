---
schema_version: "1.0.0"
document_id: "d10c66a4f1c40867251eacd8d0bda6867c80d89f48681556149b91d1ba61dd34"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/chatgpt-model-changed-from-march-to-june-2023"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:129f7cb568adeacf59663a2244fabe4aefe69f3910832af0973fca313bf22cc7"
---

# How has the ChatGPT model changed from March to June?

[Joschka Braun](https://joschkabraun.com/) on Aug 3 2023


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting) .


A recent[paper](https://arxiv.org/pdf/2307.09009.pdf) proposing a drift in ChatGPT performance garnered a lot of attention recently. I did a deep dive and think the results are misinterpreted and some nuance lost. After reading it, I was left with more questions than answers, so I decided to investigate further.


## ​


The Setup


I downloaded the CSV of prime numbers from the paper for benchmarking. I realized that although the paper shared the prompt,` Is {{number}} a prime? Think step by step and then answer "\[Yes\]" or "\[No\].` , it didn’t state whether it was a system or user message. To cover my bases, I tried both. The paper used a temperature of 0.1 which seemed oddly specific, so to make the experiments more deterministic, I tested both 0.1 and 0.0. Alas, to save my wallet, I constrained myself to the GPT-3.5-turbo model.


## ​


Evaluating LLM BehaviorDrift


I used Parea’s platform to facilitate my testing. Using Parea’s Datasets tab, I uploaded the CSV as a test collection and built custom evaluation metrics. A common challenge with LLMs is coercing them to abide by strict output schemas. For example, the paper’s prompt says to answer “\[Yes\]” or “\[No\],” but their evaluation criteria only requires the model’s response to contain “yes.” Having read of the positive impact of relaxing parsing requirements on LLM performance for LeetCode questions, read more[here](https://twitter.com/si_boehm/status/1681801371656536068) , I decided to test two evaluation variants:


- **A Fuzzy version** - Give a score of 1 if “yes” is in the response (same as the paper)
- **A Strict version** - Give a score of 1 only if “\[yes\]” (with the brackets) is in the response


Also, I added an evaluation metric to determine whether or not the model followed the CoT strategy.


## ​


Observations


Using Parea’s benchmark feature, I could test all variants of my prompts against the different prime number test cases and with various evaluation metrics. Below is a summary table of the benchmarking exercise using a temperature of 0.1, like in the paper. The first two rows are metrics for GPT-3.5-turbo-0301 (March snapshot), and the last two are for GPT-3.5-turbo-0601 (June snapshot).


Benchmark overview comparing using user or system roll for the March (top 2 rows) and June snapshot (bottom 2 rows) for GPT-3.5-turbo.


**Insight 1:** No drift in accuracy if the prompt is assigned the “system” role (rows 2 and 4)


The paper reports a dramatic jump in accuracy from March to June. This holds when using the “user” role; however, the claim diminishes if I use the “system” role for the prompt. With a temperature of 0.1, the March model achieved 39% correct vs 51% for June. This delta dramatically shrinks when reducing the temperature to 0 to 44% for March and 48% for the June model.


**Insight 2:** Drift in likelihood to follow instructions flips depending on what role is assigned


When using the user role, the number of test cases for which the model followed CoT instructions increased from 9% to 70% from March to June. This also holds for the outputting the correct response in the specified format (strict vs. fuzzy parsing requirement). Surprisingly, the complete opposite trend surfaces by simply switching to using the “system” role. Adherence to CoT instructions drops from 25% to 7% from March to June. Once more, this flipping is more evident with a temperature of 0.


## ​


Conclusion


I agree with the paper that OpenAI’s fine-tuning process caused changes in model performance from March to June. However, the direction of that drift is unintuitive and inconsistent. With all prompt engineering, building intuition on what may drive model performance is helpful. Using multiple evaluation metrics is one way to build this intuition (e.g., measuring if CoT instructions are followed or if answers respect given formats). I open-sourced the results and evaluation metrics in this[GitHub repository](https://github.com/parea-ai/LLMDrift) . You can easily use any of these metrics on Parea or create your own and do experiments like this one. I’m excited to see what others will find when investigating model behavior!


## ​


Test Prompts with Parea


## Prompt Playground


Compare, version, test, and evaluate prompts
