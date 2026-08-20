---
schema_version: "1.0.0"
document_id: "c3cdbfbf69b6498bc20e7635658ebd206c0900194a614eb94234a0dbf8b14fb1"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-db39e53ee409"
canonical_url: "https://www.cotool.ai/blog/evaluating-gpt-5-1-claude-opus-4-5-and-gemini-3-pro-ai-agents-in"
published_at: "2025-12-01T00:00:00+00:00"
first_seen_at: "2026-07-21T15:09:13.517815+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:571026055c72fc85e92a42ceff3ff61851493f7e4db0d7bb8f67a8e3944606b4"
---

# Evaluating AI Agents in Security Operations (December 2025)

# Evaluating AI Agents in Security Operations (December 2025)


Which frontier model should you use for SecOps automation? We added the latest cohort of frontier models to our benchmark to find out.


Research


December 1, 2025


Eddie Conk


Cofounder, CPO


##### *Find the latest results for this benchmark at our interactive research site:*[https://research.cotool.ai/benchmarks/botsv3](https://research.cotool.ai/benchmarks/botsv3)


*Note: This is an update to*[a previous blog post](https://cotool.ai/blog/evaluating-ai-agents-in-security-operations) *we published around benchmarking frontier AI models. In this post, we run the same benchmark on a set of newly released models, namely GPT 5.1, Claude Opus 4.5, and Gemini 3 Pro*


**TLDR;** We added **GPT-5.1** , **Claude Opus 4.5** , and **Gemini 3 Pro** to one of our internal benchmarks that evaluates models on real-world security operations (SecOps) tasks using Cotool’s agent harness and the Splunk BOTSv3 dataset. **GPT-5.1** and **Opus 4.5** achieve a modest improvement in SOTA on task accuracy (63%), while Gemini 3.0 Pro continues to lag behind the frontier. Notably, **Opus 4.5** completed tasks in half the wall-clock time of any other model, including **Haiku 4.5 (!)** , suggesting that reasoning efficiency can outweigh raw inference latency in long-horizon tasks. **** Finally, **GPT-5+** variant models maintain the performance-cost Pareto frontier. These results provide practical guidance for model selection in enterprise SecOps automation.


## The Eval


As a refresher, we reproduced the[Splunk BOTSv3](https://bots.splunk.com/) blue team Capture the Flag (CTF) environment. BOTSv3 comprises over **2.7M logs (spanning over 13 months) and 59 Question and Answer pairs** that test scenarios such as investigating cloud-based attacks (AWS, Azure) and simulated APT intrusions. For more information around motivation and methodology, checkout our previous blog post[Evaluating AI Agents in Security Operations](https://framer.com/projects/Cotool-Site--ZxGLlFNQn37vgVTiTQgF-6ak7V?node=gfT22mLYC) .


## Results


#### Accuracy:


**GPT-5.1** and **Claude Opus 4.5** tied for the highest overall accuracy at **65%** , representing a modest improvement over the previous SOTA of 63% ( **GPT-5** ). **Sonnet-4.5** remains close behind at 61%. **Gemini 3 Pro** achieved **51%** accuracy, which is a significant improvement over **Gemini 2.5 Pro** (25%), but still lagging behind the frontier models from **OpenAI** and **Anthropic** . The accuracy gap between the top performers and **Gemini** models remains substantial.


##### Cost Efficiency:


**GPT-5+** variants continue to define the performance-cost Pareto frontier, offering the best tradeoff between performance and dollar cost. **GPT-5.1** achieves top-tier accuracy at roughly **$1.67** per task. **Opus 4.5** , while matching **GPT-5.1** 's accuracy, costs ~ **3x** more per task ( **$5.14** ). While **GPT-5+** models are still the most cost efficient option, the gap between providers is closing.


**Gemini 3 Pro** sits in the mid-cost range ( **~$0.93** ) but doesn't deliver accuracy competitive with similarly-priced **GPT** models.


( **Note: cost estimates exclude prompt caching** )


##### Task Completion Rate:


**GPT-5.1** achieved 100% task completion, matching other OpenAI and most Anthropic models. Notably, **Opus 4.5** and Gemini 3 Pro had equal **92%** completion rate, this suggests **Opus 4.5** may struggle with long context tasks such as security log investigation without very tight tuning. We plan to investigate these failures in more detail.


##### Task Duration:


**Opus 4.5** was the fastest model of the newly tested cohort by a significant margin, completing tasks in just **122s** on average. This is notably roughly half the time of **Haiku-4.5** (240s), which was previously the fastest model in wall clock task duration and is presumably a much smaller model. This suggests that, despite being a larger model by size, **Opus 4.5** arrives at an answer more quickly producing more effective tool inputs and thus requiring fewer agentic turns. This is a significant update in how we think about the most cost effective models for long horizon tasks, as reasoning efficiency can overtake inference latency when operating over many steps.


**GPT-5.1** averaged **354s** , notably **25%** faster than GPT-5 ( **473s** ). **Gemini 3 Pro** averaged **500s** , placing it in the middle of the pack.


##### Tool Efficiency:


**Opus 4.5** averaged **16** tool calls per task, similar to **Sonnet 4.5** ( **16.7** ) and GPT-5 ( **18** ). GPT-5.1 showed improved efficiency with only **14.5** calls on average, suggesting better reasoning per tool invocation. **Gemini 3 Pro** had among the fewest average calls ( **9.3** ), though this didn't translate to better accuracy. Max tool calls for **Opus 4.5** and **Sonnet 4.5** reached **100** (indicating longer task convergence in some cases), while **GPT-5.1** showed more consistent behavior with a max around **60** calls. The efficiency of **Opus 4.5** over **Haiku 4.5** is evident in this metric as well, at ~ **42% fewer** tool calls. We plan to relax the tool call limits in future evals.


##### Token Efficiency:


**Opus 4.5** averaged **1.1M** tokens per task, while **GPT-5.1** consumed **1.2M** . Both are more token-efficient than **Sonnet 4.5** ( **3.1M** ) but consume more than GPT-5 ( **860k** ). **Gemini 3 Pro** averaged **505k** tokens, showing moderate efficiency. The **Anthropic** models tend to consume the highest token consumption, with Sonnet **4.5** 's max reaching nearly **34M** tokens on outlier runs. We again note the efficiency of **Opus 4.5** over **Haiku 4.5,** with the average run consuming **~42%** fewer tokens.


## Interpretation for Security Teams


For real-world SecOps agents considering the new models:


- **GPT-5.1** is now the recommended choice for most blue team investigation tasks. It matches **Opus 4.5** 's accuracy at roughly **1/3** the cost with better task completion reliability.
- **Opus 4.5** is ideal for time-critical investigations where SOTA accuracy is required and speed is a top priority
- **Gemini 3 Pro** shows meaningful improvement over previous Gemini models but still isn't competitive with OpenAI or Anthropic's best offerings for SecOps tasks. It may be worth revisiting as Google continues to iterate.


**Haiku 4.5** remains a strong choice for interactive triage or real-time alert enrichment, offering a good balance of speed ( **240s** ), accuracy ( **51%** ), and **100%** reliability at moderate cost.


### Call For Participation


See our[previous blog post](https://cotool.ai/blog/evaluating-ai-agents-in-security-operations) for more details on future work


Evals in security operations are an evergreen challenge. As agents take over more security operations tasks, benchmarking performance becomes increasingly critical. Our goal is to push the community forward with better metrics so that security teams can properly understand agent capabilities before handing over mission-critical tasks. This includes exploring where releasing open source projects would be beneficial.


**If you are:**


- Participating in or building blue-team CTF challenges or security training scenarios
- Working with production security datasets that could be anonymized for benchmarking
- Researching agent evaluation methodologies or prompt optimization techniques
- Running a security operations team interested in testing agents in controlled environments
- Building security-specific agents at your company and have insights on model effectiveness for different tasks


We'd love to hear from you. Reach out ateddie@cotool.ai or on X:[@cotoolai](https://x.com/cotoolai)
