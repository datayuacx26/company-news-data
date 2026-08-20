---
schema_version: "1.0.0"
document_id: "a18f688a2b5c8dc1c58e476e63a04348eee5116e3a30679baa8326beec4b71d5"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/ai-coding-a-sober-review"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:4debbce8465fb0c96747e092e54a81029cfc4cd9eff4e531936e0a670b6289ca"
---

# AI Coding: A Sober Review

## **AI Coding: A Sober Review**


September 17, 2025 · 4 min read


Shikhar Bhardwaj


Software Engineer


*Disclaimer*


*I would like to make it clear that the findings in this blog post are all my own personal observations. This is not an objective comparison with repeatable tests. I used these tools in my daily work and noticed their strengths and weaknesses. Someone else might experience a very different outcome. I work on Ubicloud, which is written in Ruby and follows a design pattern uncommon in the industry.*


### TL;DR


AI Dev Tools are useful now, especially for writing tests, prototyping, and repetitive tasks. They are not magic, but excellent helpers. For complex code or debugging, human input is still better. Providing better context, scoping the task well, and reusing past information are key to getting the most out of them. Context management, persistence, and large language models keep improving. Soon, they may be essential.


What tools do I use right now? For day-to-day tasks, Windsurf is a winner for me at the moment. For creating new, complex things from scratch, Claude Code works well.


### Introduction


Over the last 6–7 months, I tried various AI coding tools for daily tasks, side projects, and testing ideas. I was looking for the right balance of performance, cost, and flexibility between self‑hosted and cloud options. After hearing about Qwen 2.5's strong programming skills, I tested self‑hosted code assistants with Continue.dev on an RTX 3090. The setup worked well but still felt short of GitHub Copilot. I found that cloud APIs from Anthropic and OpenAI offered better price-performance. As a result, I switched to OpenRouter with Continue.dev to test models from OpenAI, Anthropic, and DeepSeek. However, after encountering limitations, I expanded my usage to include tools like Cursor and Windsurf.


### Tools I Used


The following table gives very basic information about what tools I used.


Editor/Tool Cost Open Source BYOK Default Models


Continue.dev


$


Claude 4 Sonnet, Codestral


Cursor


$20/m


Claude 4 Sonnet, et al*


Windsurf


$15/m


SWE-1


Cline


$$$


Claude Sonnet 4


Claude Code


$$$+


Claude Opus 4


This table consists of the information you can find anywhere. However, I decided to compare them in these 4 areas because they mattered the most to me.


- **Cost** : Getting the exact cost for using these tools is not straightforward, as each of them has a different take on usage based pricing. Some tools offer a free tier with various limitations, which you hit if you’re doing any serious work. Some of them also offer PAYG (Pay as you go) but as you can see, it becomes difficult to compare them. I chose to compare them according to my own usage metrics and how much they cost me, definitely not an objective comparison.
- **Open Source** : This is something I care about. Open‑source tools are flexible, hackable, and make it easier to integrate different systems.
- **Bring Your Own Key (BYOK)** : This feature lets me switch between providers such as OpenAI, Anthropic, and OpenRouter. OpenRouter stands out. It lets me change models and providers without tying credits to one.
- **Default Models** : This lists the default or suggested models that these tools use at the time of writing. This changes with time but at the moment, it is dominated by different versions of Claude from Anthropic. Windsurf stands out in this regard by having their own SWE-1 model, which is free for a limited time.


### How They Performed


I compare the performance of these tools in three general areas: building new features, writing tests, and debugging. This is definitely less scientific and more subjective because I didn’t test each of these tools in the same time frame on the same task. Most often, I changed the tool I was using once I started to feel like it could do better in certain tasks.


**Editor-based tools:** **Continue.dev vs Cursor vs Windsurf**


**Continue.dev** is a VS Code extension; it provides an autocomplete and chat interface to interact with an LLM to generate or edit code. I used it around Q4 2024 and Q1 2025, starting with self-hosted Qwen 2.5 Coder and later on with OpenRouter-based Claude, DeepSeek, and Gemini models.


- **Building features** : For me, it worked fine for simple things. It didn’t have features for planning tasks. So, for larger projects or existing code, users need to do more work. It often applied changes incorrectly (diffs applied to wrong lines, duplicate content, applying the change got stuck etc.). Managing context was another challenge. It's mainly up to the user to provide the right context with the prompt.
- **Writing tests** : **** Setting up new tests and assertions was a challenge for most of the tools here. All these tools could write basic tests well, but they needed oversight to ensure the right behaviors were asserted. Another area that needs attention is test naming. Sometimes, the generated names are poor. They can be too long or focus on the wrong aspect of the behaviour being tested. Repeating existing patterns to cover all cases was easy. For example, testing different Enum values or generating cases with invalid inputs had close to a 100% success rate.


PS: While writing this post, I tried using Continue.dev again. I was happy to see many recent updates that make the experience smoother in several areas. Continue.dev now offers deterministic diff apply, fast apply, and a better diff review experience. There are also several improvements in Agent mode.


‍ **Cursor** is a closed-source VS Code fork. It has AI features like autocomplete and an "agent" mode. This mode can generate, edit, run code, and fix errors. While you can use the editor itself without signing up, using any AI features needs an account. I started using Cursor in March 2025 after struggling with edits in Continue.dev.


- **Building features** : Cursor improved in many ways from Continue.dev. The most significant being "Fast Apply" for edits to existing code. The autocomplete features were strong. They suggested multi-line edits and matched patterns often. For larger changes, Cursor handled context and planning well. However, results improved a lot with more manual context and a clear task breakdown at the start. With that said, you cannot build complex features or major changes automatically. They need careful review and intervention. Reviewing the proposed changes was easier. You could accept or reject changes at different levels: each line, file, or all at once.
- **Writing tests** : As mentioned earlier, setting up new tests or assertions was hit or miss. Cursor performed better when similar tests were present in other parts of the codebase. For other testing-related tasks, performance was pretty good.
- **Debugging** : It wasn't great overall, but Cursor often debugged its own code well. Most of the time, it delivered a working version, though it needed some prompts and made questionable choices at times. Performance was weak for issues that involved many systems or code execution.


**Windsurf** is available both as a VS Code plugin and a standalone closed-source VS Code fork. I decided to use the editor to get the full experience. It has a core feature set similar to **Cursor** , but it emphasises team workflows, context sharing, and reproducibility.


- **Building features** : I had the best experience with Windsurf compared to the other two tools while building new features. It handled large tasks better, planned changes well, and applied them correctly. The autocomplete also had a better hit rate for me here as compared to Cursor, especially for multi-line edits. With that said, building complex features still needed careful attention, intervention, and review.
- **Writing Tests** : A pretty similar experience to Cursor. Thanks to the better autocomplete feature, I could repeat cases using only autocomplete. I didn’t need to prompt the agent at all.
- **Debugging** : Not much different from Cursor. Debugging performance on anything involving problems of moderate complexity requires enhancement.


**Agentic Tools: Claude Code and Cline ‍**
Although all the tools above have agentic workflows, some tools, like **Claude Code** and **Cline** , focus on being fully agent-driven. They need little to no intervention, even for long and complex tasks.These tools work like assistants that can use tools to:


- Read/write files.
- Run shell commands.
- Search the web.
- Plan and make changes step by step.


With these capabilities, these tools can run the entire development loop (Edit → Build → Test → Repeat). Paired along with large context windows, they can autonomously manage a given task. Claude Code is a terminal-only CLI. Cline lives inside VS Code as a plugin.


Example: I asked Claude Code to build a fuzz test framework for our managed PostgreSQL service using an OpenAPI spec. In 30 minutes and $3 of token cost, it got something running. It wasn’t perfect but good enough to build on.


Both tools are great for creating new features. They can handle long, moderately complex tasks with large contexts. In my experience, Claude Code edges out Cline overall in areas that need a more complex understanding or reasoning quality.


**Problems**


- Very slow for big tasks.
- Can cost $10–20 easily per feature if using PAYG.
- Not good for running many jobs at once.


### Final Thoughts


Windsurf handles my everyday tasks. Claude Code helps with complex features and prototypes.


Self-hosting isn’t worth it right now unless you already have the infrastructure. Big players in the market are currently subsidising inference costs. For individual developers, investing heavily in LLM inference infrastructure doesn’t make sense. Also, open-source models have improved a lot lately, but the top coding models are still closed-source.


Improvements to Continue.dev highlight the rapid pace of development. Open-source tools are closing the gap fast. I am tempted to give Continue.dev another spin, with the latest Qwen3 coder models. I guess I'll need to go all around the circle once again.
