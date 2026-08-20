---
schema_version: "1.0.0"
document_id: "ddc635657270552805c15db5e1d6477500b52d16eafb494f1d77779c5f6a0023"
company_key: "yc-tesora"
company: "Tesora"
source_id: "yc-tesora-rss-4d7d570f276c"
canonical_url: "https://tesoraai.substack.com/p/jagged-intelligence-for-actuarial"
published_at: "2026-05-27T21:51:32+00:00"
first_seen_at: "2026-08-10T03:58:35.856576+00:00"
fetched_at: "2026-08-10T03:58:36.939902+00:00"
content_hash: "sha256:c19a0ce09af3a8b6875509fa9476d4602f233147113e5927b90687ab54752033"
---

# Jagged Intelligence for Actuarial AI

# Jagged Intelligence for Actuarial AI


### Why AI can be made reliable for complex actuarial tasks while it still can't count the R's in 'strawberry'


[Philo Bishay](https://substack.com/@philobishay)


,[Federico Reyes Gomez](https://substack.com/@federicoreyesgomez)


, and[AI for Actuaries @Tesora.ai](https://substack.com/@aiforactuariestesoraai)


May 27, 2026


AI may feel like a genius sometimes, yet other times, often in the same conversation, it acts moronically. You’ll watch a model walk through a Black-Scholes derivation and then, two prompts later, insist that there are


[two R’s in “strawberry.”](https://www.youtube.com/shorts/uFvZQDvw-S4) This isn’t a bug or a bad seed. It’s the actual shape of the technology.


In July 2024, Andrej Karpathy


[gave that shape a name](https://x.com/karpathy/status/1816531576228053133) : jagged intelligence. State-of-the-art models solve graduate-level math problems and then fail at things any middle schooler would breeze through. The capability surface isn’t a smooth gradient where harder questions are harder and easier ones are easier. It’s jagged terrain, with sharp peaks and surprising valleys, and the two can sit inches apart.


The general hope on the jaggedness is that the valleys will fill in eventually, you just have to wait for the next model release. We think that’s wrong, and the reason matters quite a lot if you’re an actuary trying to figure out where AI belongs in pricing, reserving, or anything a regulator looks at. The jaggedness isn’t a transient bug. It’s a property of how these models actually get trained, which means it isn’t going to be fixed by waiting.


Here’s roughly how it works. The first stage of training was imitation. We pointed models at most of the internet and had them predict the next word, over and over, until they were good at producing text that looks like the corpus. That worked well wherever the corpus was dense and self-consistent (Python code, English prose, popular math formats), but poorly wherever it was sparse, nuanced, or contradictory, which is most of the real world.


The real improvement in the post-2024 frontier came from a second stage called Reinforcement Learning from Verifiable Rewards, or RLVR, still used today. Instead of training the model on what the next word should be, one grades its full answer with an automated checker and rewards it when the answer is right. The catch is that the checker has to be cheap. We can’t pay humans to grade millions of attempts at math problems. But we can write a script that compares an answer to an answer key, and a unit test passes or fails on its own. So the labs trained extremely hard on the things that are cheaply graded, which is why frontier models have been racing up the competitive-math and software-engineering benchmarks for the past eighteen months. Where grading is cheap, training is extensive.


What we ended up with is a capability landscape that spikes wherever someone could build a grader and sags everywhere else. The model writes React components because GitHub has millions of them. It solves competition math because that math has answer keys. Now ask it whether a 2017 accident year at 18 months of development looks green for a commercial auto book whose mix shifted in 2019, and you’ll get an over-confident, fluent, plausible answer. Is it correct? Maybe, maybe not. Nothing in the answer will tell you which.


## Why actuarial work lands in a valley


Most actuarial work has the wrong shape for the training paradigm that’s been driving the recent frontier. There’s no automated grader for whether a development factor was correctly selected. There’s no script that can tell you whether your IBNR review caught what mattered. There’s no unit test for whether a rate-change decomposition will hold up to a regulator. The reward signals that made the model great at code don’t exist for actuarial work, and they aren’t on the way. Since what can’t be graded can’t be trained, actuaries will have to build their own AI tools, or wait a long time.


Take one task some actuaries do every week, reading a loss run. In theory it’s a record of claims activity. In practice it’s a semi-structured Excel file with the carrier’s logo eating the first eight rows, no consistent column headers, footnotes in merged cells, reserve and paid sometimes transposed, ALAE sometimes quietly bundled into indemnity, and (if you’re really unlucky) delivered as a scanned PDF of a printout of an Excel file because someone in 2003 decided that was the format and the workflow has not been revisited. To read it correctly, you need to know what this carrier tends to do, what the cedent has done historically, and what doesn’t look right. None of that is in any public corpus. There isn’t going to be a LossRunBench, because the data lives inside carriers who have every incentive to keep it where it is.


Even if some lab decided next quarter to point its optimizer at actuarial work, they’d hit a wall. Actuarial correctness isn’t local. Whether a development factor is right depends on the data behind it, the mix behind the data, the line of business behind the mix, and the carrier’s historical behavior behind the line. You can’t grade any of that from a single prompt and a single response. The reward signal you’d actually need is something more like “did this number survive three months of regulatory scrutiny without being changed,” and nobody has any idea how to turn that into something the model gets fed back during training.


There’s a feedback-loop problem on top of all this that makes the jaggedness more dangerous here than in other domains. When the model writes a bad function, tests fail and you fix it. When the model picks a 4.2% trend instead of 6.8%, nothing fails. The number flows through to the indication, the indication through to the filing, and three years later you find it again in the loss ratio. By then the analyst who made the call has moved firms and the file note reads “rolled forward from prior.”


## The coding precedent, a saving grace


Here’s the part that should actually be encouraging. AI for code looked exactly like this three years ago. In 2022 you’d ask GPT to write you a Python function and you’d get back something plausible that didn’t run, or ran but did the wrong thing, or solved a slightly different problem than the one you asked about. Nobody trusted these tools with anything that mattered. Production engineering teams treated AI like an over-eager intern who needed constant supervision. Three years later, the same engineers are shipping AI-written code through pull requests every day.


What changed isn’t really the underlying model. Frontier LLMs are smarter than they were in 2022, but they aren’t radically smarter, and the gap between “too unreliable for production” and “used by senior engineers daily” is bigger than two years of model improvement explains. What changed was the


*system* around the model. Look at Cursor, at Claude Code, at the agentic IDEs. These tools aren’t better LLMs. They’re better


*scaffolding* wrapped around an LLM.


The scaffolding does the things the model is bad at. The codebase is read by code, not by the model paraphrasing files into its working memory. Dependencies are resolved by the package manager, not by the model guessing at versions. Tests are run by the test runner. The model isn’t asked to remember what’s in scope; the IDE just tells it. And the model does the things it’s actually good at: writing the code, explaining the diff, proposing the next step, having a conversation with the developer about what they’re trying to do.


## The same path for actuarial AI


The same opportunity is open for actuarial work, and as far as we can tell (so far), nobody at a frontier lab is going to build it for you. The economics don’t justify it relative to coding and customer service. The reward signal is there for Reinforcement Learning (RL), but by the time the labs add actuarial tasks to their training set, your competitors’ actuaries using AI will already have beaten you in this market cycle. The problem isn’t really one a foundation model can solve unaided without insurance and actuarial expertise. What actuarial AI needs is its own scaffolding.


Let me be specific about what that scaffolding has to do. Start with the rating manual. It can’t be a text document the model reads and summarizes. It has to be a structured object the system can actually query, with factors, derivations, an as-of date, and a hash. When the model is asked to apply the manual, it calls the manual. When it’s asked to explain a rate, it walks the actual tree of factors that produced the rate. This matters most in the case every actuary has lived through, where a renewal is binding under a manual from two filings ago because the policy effective date predates the most recent revision, and the auditor would like a word.


The same logic applies to selections. Take a selected 0.85 development factor. By itself the number is useless. What’s useful is the package around it: why this factor was picked, what alternatives were considered, what last quarter’s selection was, who approved the change. That package is what the regulator asks about six months later, and what the analyst inheriting the book needs on day one. A generic LLM workflow throws all of it away the moment after it produces the number. A workflow built around the jaggedness keeps it as a first-class object. Jaya Gupta and Ashu Garg make the same point in their


[context graphs piece](https://foundationcapital.com/ideas/context-graphs-ais-trillion-dollar-opportunity) : the durable value isn’t in the model’s chain-of-thought, it’s in the persistent record of how a decision was made.


The hardest one to build, and the one that matters most in a regulated workflow, is honest abstention. A general LLM cannot tell when it’s in a valley. Its confidence reads exactly the same whether it’s on solid ground or making things up out of pure linguistic plausibility. A specialized system can know, because it knows what kinds of data it was tuned against and what falls outside that envelope. When the inputs drift outside, the system refuses and routes to a person. This has to be wired in as a core design principle, because the moment honest refusal becomes the embarrassing path, product pressure will erode it, and you’re back to confident wrong answers that nobody can defend.


None of this needs a frontier breakthrough. It needs an honest read of where the frontier is, and the engineering discipline to build a system that compensates for it. Cursor and Claude Code prove the playbook works. Actuarial AI is two or three years behind coding AI on this curve, and the companies that take the scaffolding seriously will be deploying reliable systems into regulated workflows years before the ones that are still waiting for GPT-7 to fix it for them.


A guest post by


[Philo Bishay](https://substack.com/@philobishay?utm_campaign=guest_post_bio&utm_medium=web)


Head of Product, former chief actuary, MBA, lover of learning


[Subscribe to Philo](https://philobishay.substack.com/subscribe?)


A guest post by


[Federico Reyes Gomez](https://substack.com/@federicoreyesgomez?utm_campaign=guest_post_bio&utm_medium=web)


CTO and Founder of Tesora


[Subscribe to Federico](https://federicoreyesgomez.substack.com/subscribe?)


A guest post by


[AI for Actuaries @Tesora.ai](https://substack.com/@aiforactuariestesoraai?utm_campaign=guest_post_bio&utm_medium=web)


Making AI useful and trustworthy for actuaries!


[Subscribe to AI](https://tesoraai.substack.com/subscribe?)
