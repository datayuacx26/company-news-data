---
schema_version: "1.0.0"
document_id: "d1b2222ccda71d72131a70a2cc1f5233edcdb7369454128dae1dc3b58bab0193"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-5eeb9d1ed7ff"
canonical_url: "https://devblogs.microsoft.com/blog/models-dont-have-preferences-they-have-context/"
published_at: "2026-06-22T14:53:23+00:00"
first_seen_at: "2026-07-22T03:58:43.199661+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:4f807aea04686f33bc211a49b17f43a343840d9228da30db3a25bbdcc1867bd3"
---

# Models don’t have preferences, they have context

You open a fresh chat, type “What framework should I use for a web app?”, and the model says “React.” You screenshot it, share it, and write “Claude prefers React.” It gets engagement. People nod along. A few reply with their own results. And now we have a consensus: Claude prefers React. Except it doesn’t. The model doesn’t prefer anything. You’re reading the room, not the mind.


## The genre


There’s a whole genre of this. Run N prompts in a bare chat window, tabulate the answers, maybe build a heatmap, publish it as a blog post or a thread. “Which frameworks do LLMs prefer?” “What languages do models recommend most?” The format varies, but the method is the same: ask abstract questions in an empty context, count the answers, declare preferences.


It’s easy to see why this is appealing. It feels scientific: you have a sample size, you have a methodology, and it produces shareable results and charts. The problem isn’t effort or intent. The problem is that the entire setup is measuring something other than what it claims to measure.


## Context is everything


A language model’s output is shaped by everything in its context window: the system prompt, the conversation history, the files attached to the session, the OS metadata, the way the question is phrased. Strip all of that away, and you’re not getting the model’s “opinion.” You’re getting its default behavior in a vacuum, a reflection of its training data.


Try this: ask a model “What framework should I use?” in an empty chat. You’ll probably get React, because React dominates the training corpus. Now open a workspace that already contains Svelte files, attach the project context, and ask the same question. You’ll get Svelte. Same model, same question, different answer. The “preference” evaporated.


This isn’t a fringe observation. Gao and Kreiss[documented](https://arxiv.org/abs/2509.04373) how models shift behavior in what they call “testing mode,” producing systematically different outputs when they detect evaluation-like patterns. Anthropic[showed](https://www.anthropic.com/research/evaluating-ai-systems) that formatting changes alone (how you structure the prompt, what delimiters you use) can swing MMLU accuracy by roughly 5%. Five percent from *formatting* . If the shape of the question moves the needle that much, treating any single-context answer as a stable property of the model doesn’t hold up.


**The model isn’t expressing a preference. It’s completing a pattern. Change the pattern, change the completion.**


## The restaurant in a city you’ve never visited


Think of it this way. Asking a model “What framework should I use?” in an empty context is like asking someone “What’s your favorite restaurant?” in a city they’ve never visited. They’ll say something. It might even be a reasonable answer, pulled from reviews or general knowledge. But it tells you nothing about *their* taste, and it tells you nothing about where *you* should eat. It’s a default answer to an impossible question.


## What to do instead


If you actually want to know how a model performs with your technology, you need to test it the way developers actually use it. Set up a workspace. Include project files, configuration, dependencies. Write prompts that reflect real tasks: “refactor this component,” “add error handling to this endpoint.” Test with context that mirrors how your tool will be used in production.


*How would you even use “Claude prefers React” as actionable information?* You wouldn’t switch your tech stack because a model defaulted to it in an empty chat. Your team wouldn’t reorganize around it. What you actually want to know is: does this model write good code *in my stack, with my patterns, in my repo* ? That’s a question you can only answer with realistic context and realistic prompts.


We’ve been spending a lot of time thinking about how to properly evaluate AI-powered developer tools. The shortcut of testing in a vacuum is tempting, but it produces results that look precise and mean nothing.


## The model doesn’t have preferences


Models don’t have preferences. They have patterns shaped by context. What looks like an opinion is a reflection of whatever you put in front of them, and when you put nothing in front of them, you get the statistical average of the internet. That’s not insight, that’s noise.


If you strip the context and call the output a preference, you’re not measuring the model. **You’re measuring nothing.** Test with real context and real project structure. That’s where the signal is.
