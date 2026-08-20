---
schema_version: "1.0.0"
document_id: "cbd8f65e3d7d0bbc01552a2b9c6a5ecd65b4064460959a9d261bfee621198f34"
company_key: "yc-onyx"
company: "Onyx"
source_id: "yc-onyx-news-import-2a3a6e22cda1"
canonical_url: "https://onyx.app/blog/building-internet-search"
published_at: "2025-09-18T00:00:00+00:00"
first_seen_at: "2026-07-25T17:58:53.878122+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:887ac7ad3e5178cc8604d3c2fdd16389bb5a66ff68cb0a1e0a3045bc7bce0750"
---

# Building an Internet Search to rival OpenAI

We looked at OpenAI, Anthropic, Grok, and Gemini and cracked how they all do internet search. Surprisingly, all of them do it the same way.


Here's the quick spoiler-You give the LLM 2 actions (tools) that it can use when it decides it's necessary:


1.


**web.search** to find the most relevant links and snippets.


2.


**web.open_url** to read the contents of the page in full.


The key insight here-let the LLM choose what it wants to read, just like a human would (but in parallel and faster).


## How we cracked the code


### What is available to the LLM?


It turns out, if you prompt these chat systems in a clever way, it's pretty easy to uncover their underlying mechanisms. Different providers have different levels of obfuscation but when it comes to these more advanced functionalities, the protection from prompt hacks is fairly non-existent.


Instructions from ChatGPT


In fact you can just directly ask it for the names of the tools and it will just tell you. Anthropic even publishes their prompts so it makes it even easier (read them *[here](https://docs.anthropic.com/en/release-notes/system-prompts#august-5-2025)* ).


We tried this across several users for each platform and every time the tool names and descriptions came back the same. It also aligns with *[leaked System Prompts](https://www.reddit.com/r/PromptEngineering/comments/1j5mca4/i_made_chatgpt_45_leak_its_system_prompt/)* that were published not long ago which provides additional certainty that the LLM is not just hallucinating responses.


We also did negative testing-if you ask the LLM if it has access to the “open_url” tool, it says it does, but if you ask it something similar like “follow_link”, it will correct it and say it has access to “open_url” instead.


### How does it get used?


Similar to how we uncovered the tools available to the model, we can ask it to share its search process with us as it goes along. As the steps get run and the calls/results are populated in the LLM's context, it is also able to explain back to the user what is going on.


Anthropic Web Search


> **Note:** A fun update as I just tried this again today with OpenAI and instead of running 1 query, it ran 4 in parallel and consolidated the results from them before choosing which ones to open.


## How it works in Onyx


Behind the scenes of these two functionalities (search and read), there are two sets of technologies. The first is a Web Search API like Google's (we decided to support *[Google](https://programmablesearchengine.google.com/about/)* , *[Serper](https://serper.dev/)* , and *[Exa](https://exa.ai/)* ). You can find a comparison of them *[here](https://docs.onyx.app/overview/core_features/web_search)* . These APIs just give back a snippet and some metadata like the URL. To get the full text we have to rely on a web scraper. For this we have one built in house and also offer *[Firecrawl](https://www.firecrawl.dev/)* .


Onyx Reasoning


In addition to this we've found that when dealing with a lot of additional context from web pages, that reasoning models tend to do quite well. To ensure the best quality and low hallucination rate regardless of model choice, we've also introduced reasoning through chain-of-thought for typically non-reasoning models.


If you\`re interested in testing it out, you can either *[set up Onyx locally](https://docs.onyx.app/deployment/getting_started/quickstart)* or *[sign up](https://cloud.onyx.app/auth/signup)* for a free trial on Onyx Cloud.
