---
schema_version: "1.0.0"
document_id: "a45be2ce384c2deb1d7f9a9b444d71378ea3ea9f588d16d842f6b4ab14b36188"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/think-harder/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T08:23:50.502501+00:00"
fetched_at: "2026-08-06T08:23:51.680113+00:00"
content_hash: "sha256:c85961b7a63a026f7d47b0ce9756b6f84dc5dfbb3f75ca696e9a8e96f1bd21e2"
---

# Think harder: how prompts interact with reasoning options

Every provider has an effort knob now. Low, medium, high, xhigh, max, adaptive, ultra, and so on.


I thought that was the primary, if not only, control of reasoning effort. Surely that is much more powerful than just saying “think hard.” Prompt engineering was so 2025.


I’m here to say: prompt engineering is alive and well. Just saying “think hard” on low reasoning can do more than setting reasoning to high.


## TL;DR


Adding a “think hard” user message on Opus 5 at low reasoning increased total tokens by


**66%** , from 424 to 704. That was more than Opus 5 at high reasoning without the prompt, which used 662.


This applies to OpenAI models too. On Sol, low + “think hard” used 320 total tokens, more than the plain max baseline at 283.


Not satisfied with total tokens? It also bumped the provider-reported reasoning tokens directly. On Sol, low + “think hard” moved reasoning tokens from 60 to 114, just above the plain high baseline of 113.


It’s admittedly a slightly contrived experiment, but it’s very easy to reproduce.


## The setup


I used three “easy” math problems:


1. “How many distinct arrangements are there of the letters of the word MISSISSIPPI?”


*(34,650)*


2. “You flip a fair coin repeatedly. What is the expected number of flips until you first see the pattern HTH?”


*(10)*


3. “In how many ways can 8 identical balls be placed into 4 distinguishable boxes if no box may be empty?”


*(35)*


And two exact prompts:


> “Think hard about this problem. Be thorough and check your work carefully.”


> “Keep your reasoning brief.”


Then I tested five versions of every request:


1. No extra instruction


2. “Think hard” in the user prompt


3. “Think hard” in the system prompt


4. “Keep brief” in the user prompt


5. “Keep brief” in the system prompt


## Results


The provider setting matters. But the words in the prompt can often matter more.


“Think hard” usually pushed token use up. “Keep your reasoning brief” usually pushed it down. That kept happening even when effort was explicitly set to low, high, xhigh, or max.


Sol makes this really obvious. At low effort, user “think hard” moved total usage from 212 to 320 tokens. At max, it moved from 283 to 422.


## Prompt-Effort Matrix


Matrix of provider-reported reasoning tokens for GPT-5.6 Sol across reasoning effort and prompt placement.


On Sol, low + user “think hard” reached 114 reasoning tokens, just above the plain high baseline of 113. At max, it moved reasoning tokens from 144 to 210.


Here are Opus total tokens:


Matrix of mean total tokens for Claude Opus 5 across reasoning effort and prompt placement.


I used “mean total tokens” for Opus, because Opus especially seems to spill more of its reasoning into the actual output when prompted to think harder. At low effort, user “think hard” increased total tokens by 66%, from 424 to 704. That was more than high without the prompt at 662. Reasoning tokens still increased 28%, from 42 to 54.


Matrix of mean reasoning tokens for Claude Opus 5 across reasoning effort and prompt placement.


## User prompt vs. system prompt


The user prompt was more effective. User “think hard” used more total tokens in 23 of 26 model and effort pairs. User “keep brief” used fewer total tokens in 22 of 26.


Same for Claude. On Sonnet 4.6 at high effort, user “think hard” hit 1,383 tokens vs. 1,120 in the system prompt. User “keep brief” dropped to 552 vs. 805 in the system prompt.


## My point is....


If you care about cost, don’t count on reasoning effort to save the day. It changes the default, but users can override it.


## Full tables


### OpenAI


Full OpenAI total-token table with Sol first, followed by Terra and Luna.


### Claude


Full Claude total-token table with Opus 5 followed by Sonnet 4.6.
