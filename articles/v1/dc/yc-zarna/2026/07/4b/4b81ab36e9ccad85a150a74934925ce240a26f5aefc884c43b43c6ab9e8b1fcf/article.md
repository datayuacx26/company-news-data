---
schema_version: "1.0.0"
document_id: "4b81ab36e9ccad85a150a74934925ce240a26f5aefc884c43b43c6ab9e8b1fcf"
company_key: "yc-zarna"
company: "Zarna"
source_id: "yc-zarna-news-import-0c4a0266984e"
canonical_url: "https://www.zarnaai.com/blog/dont-marry-an-ai"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-24T07:55:10.581126+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:0904851f42eba43e6a93276828dd8d828d8411727455a9073adc50b18166a12f"
---

# Don't Marry an AI

The question I get most from partners isn't really about Zarna, at least not at first. They want to know which AI they should buy.


I spend a lot of my week talking to partners and principals who know they need some kind of AI strategy because everyone around them keeps saying they do, but nobody has given them a straight answer on what that means, so at some point in the conversation, sometimes during the first meeting and sometimes in a follow-up after they've tried a few tools themselves, they ask the question they feel like they should already know the answer to.


"So which one should we actually be using? Is it ChatGPT? Claude? That Gemini thing?"


I get why they ask. There are too many products, the model names keep getting worse, and every provider has a benchmark chart proving that its model is the smart choice, so picking one feels like the responsible thing to do. I just think the choice matters much less than the industry wants you to believe, and I've watched smart people burn weeks comparing models as if the answer will still hold six months from now.


Claude, GPT, and Gemini are all strong in different ways, but my recommendation will probably change before the contract, integration, or workflow you build around it does, and that is the part I would actually worry about.


## The trapdoor


The older history still matters because it shows this isn't a strange month that we can ignore. GPT-4 launched in March 2023 and for about a year there wasn't much of a debate, so if you had asked me then I probably would have told you to use GPT, stop turning it into a procurement project, and get on with the actual work.


Then[Claude 3 Opus](https://www.anthropic.com/news/claude-3-family) arrived in March 2024 and credibly beat GPT-4 across the headline benchmarks,[Gemini 1.5 Pro](https://blog.google/technology/ai/google-gemini-next-generation-model-february-2024/) took the context-window record two months later,[Claude 3.5 Sonnet](https://www.anthropic.com/news/claude-3-5-sonnet) moved ahead of GPT-4o on most of the published benchmarks in June, and by the end of the year OpenAI had released[o1](https://openai.com/index/introducing-openai-o1-preview/) and changed the conversation again around a kind of reasoning buyers hadn't even been comparing when the year began.


You don't even need to look that far back now. Anthropic released Claude Fable 5 on June 9, 2026, and three days later the US government issued an export-control directive citing national security concerns and requiring Anthropic to cut off foreign nationals, including its own foreign-national employees. Anthropic couldn't verify nationality in real time, so it[switched the model off for everyone](https://www.anthropic.com/news/fable-mythos-access) , which meant the model users had only just started testing disappeared almost as soon as it arrived.


The controls were lifted at the end of June and Anthropic[brought Fable back on July 1](https://www.anthropic.com/news/redeploying-fable-5) , but the return came with another moving answer: paid subscribers would get it within 50% of their weekly limits through July 7, then it would move to usage credits. July 7 became July 12, and July 12 became[July 19](https://x.com/claudeai/status/2076351401006154204) , so users kept reaching the date when the model was meant to leave their plan and finding out at the last minute that they had another few days.


Right in the middle of that, OpenAI[released GPT-5.6](https://openai.com/index/gpt-5-6/) across ChatGPT, Codex, and its API on July 9, and the[early results people started posting on X](https://www.axios.com/2026/07/09/ai-openai-gpt-release) were strong enough that Claude users were openly talking about whether to move their subscriptions. Anthropic has not said GPT-5.6 caused the Fable extensions, and I don't know that it did, but from a customer's side the sequence is hard to miss: one model comes back under a short deadline, a competitor launches something people are excited about, and the deadline keeps moving while everyone tries to work out which subscription still makes sense.


Look at everything a buyer had to track in one month: the model changed, access vanished because of a government order, the provider brought it back under different safeguards and pricing, and a new release from somebody else made the whole decision feel stale before most teams had finished testing the first one.


Whenever someone asks me which AI to buy, they're asking for a snapshot of a race where both the capability and the terms of access keep moving while contracts, integrations, approvals, and training all move at normal enterprise speed. I can tell you what I like today and I can give you reasons, but I can't pretend the answer will survive until your next annual planning cycle, or even your next IC offsite.


The better decision is choosing the layer built on top of the models, sometimes called a harness, because that is where your firm's workflow, permissions, data, and judgment actually live, and it is the part that should remain useful through the next model release and whatever changes again after that.


I think about the model a bit like electricity, which is an imperfect analogy but close enough: you need it, the quality matters, and the economics matter, but you would never rebuild the office around one power company's current product and call the wiring your competitive advantage. The useful thing is what you plugged in, and you want the power source to stay replaceable.


The companies furthest into this are already behaving that way. When Menlo Ventures[surveyed enterprise buyers](https://menlovc.com/2024-the-state-of-generative-ai-in-the-enterprise/) in late 2024, it found them running an average of three foundation models and moving work between them depending on the task, while the market share underneath those choices kept shifting: over three years the 2023 front-runner fell from 50% to 34% to[27%](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/) , while the runner-up moved from 12% to 40% and took the lead. They didn't discover one perfect model that the rest of us missed; they built their systems so being wrong about a model wasn't a crisis.


A long commitment to one provider gets expensive in ways that don't show up in the first proposal, because the price of running the same level of capability keeps falling, as much as 40x a year by[Epoch AI's count](https://epoch.ai/data-insights/llm-inference-price-trends) , while the models keep improving, and once you've built everything around one vendor you only receive those gains when the vendor gives them to you or when you're willing to renegotiate and rebuild.


The part I worry about more is the data, because every custom GPT, uploaded document, screening preference, old memo, and bit of feedback starts collecting behind one company's login, under terms and pricing you don't control, and after a while leaving the provider doesn't feel like changing software anymore because some of your firm's memory has become tangled up with the product you're trying to leave.


Most firms would never let a single fund administrator or data provider get that kind of grip on their core records, but AI often slips through with less scrutiny because it arrives as a helpful productivity tool, one person starts using it, then a team does, and by the time anyone calls it infrastructure the dependency is already there.


So I would ask a vendor two fairly boring questions before spending much time on model comparisons: if a better model ships next month, can you switch the model underneath my workflows without tearing them apart, and if we leave you a year from now, does our firm's knowledge leave with us in a form we can actually use?


The answers don't need to come with a big model-agnostic speech, but they do need to be clear, because if switching models requires a rebuild or getting your own knowledge back requires a negotiation, then the product is already asking you to rent something that should belong to your firm.


## The bet we made


I should say the obvious part out loud: this is the thesis behind Zarna, and I have a stake in it, so I'm not pretending I arrived at the conclusion as a neutral observer after writing most of an essay. We built the company this way because the alternative looked fragile to us.


Zarna is model-agnostic on purpose, which in practice means that when a better model becomes the right choice for a task, a customer shouldn't have to migrate their workflows or retrain the team just to use it. The AI associate they have already set up to read an inbound teaser, log the deal in the CRM, and draft the pass email should keep doing that job while the model underneath gets replaced or improved.


The firm's knowledge also needs to stay separate from any one model: the deals, memos, screening logic, and judgment should live in a layer the firm controls and the model can read from when it works, rather than slowly disappearing into a vendor account that becomes harder to leave every month. We have spent years building the machinery underneath that, and the whole point is that a customer shouldn't need an internal team chasing every model release to keep up.


We didn't make those choices because model independence sounds principled on a slide. We made them after watching the best model change several times in one year and imagining what it would feel like to explain to a customer that the system we sold them six months ago was now stuck on the wrong side of that change.


So when a partner asks me which AI they should buy, I usually tell them which models I like, because that part is still useful, and then I tell them not to build the firm's future around my answer. My answer will change. Their workflows and their knowledge should not have to change with it.


[All articles](https://www.zarnaai.com/blog) Share
