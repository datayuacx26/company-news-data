---
schema_version: "1.0.0"
document_id: "edf35791fd677258ece93f074ef778db66c4f1c7d65f1afb5577adacda583c43"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/thoughts-on-evals/"
published_at: "2025-09-05T00:00:00+00:00"
first_seen_at: "2026-07-22T10:48:02.200992+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:718c832c4ef8122331c82d74cc57f2cf4b858899e1c5feeae80fc035b2d69bae"
---

# Thoughts on Evals

Evals are an important part of building AI products. We know this first-hand.


*If you want a comprehensive, hands-on walkthrough of how teams are actually evaluating AI agents in 2026 - from offline evals to production monitoring to self-healing loops - read [How to Eval AI Agents - The 2026 Guide](https://www.howtoeval.com/) .*


[Raindrop](https://www.raindrop.ai/) uses AI to monitor the performance of AI agents. We generate billions of labels a month. We detect issues, generate reports, and automatically cluster intents. We are also constantly changing and optimizing how we detect issues. If we didn’t have evals, it would be impossible to make changes without breaking production


Internally, we use a custom eval platform inspired by evalite. Many of our customers use Langsmith and Braintrust.


We built Raindrop because evals just weren’t enough. Our customers pay for Raindrop alongside tools like Braintrust because Braintrust can’t tell them what they need to know.


I’m writing this because Ankur, the CEO of Braintrust, recently wrote a blog post directly dismissing A/B tests, and Raindrop specifically (without naming us). In the blog post, Ankur claims that evals are the future. He claims that they help you measure how good your product is, that they are key for rapid experimentation. He also claims that evals will become increasingly important as software becomes more personalized. I believe the opposite to be true for each of these claims.


---


> *"The recent acquisitions of [Statsig by OpenAI](https://www.statsig.com/blog/openai-acquisition) and [Eppo by Datadog](https://www.datadoghq.com/blog/datadog-acquires-eppo/) hint at the turning point: A/B testing is no longer sufficient for AI product optimization. The future is evals."*


*Side Note: For the sake of brevity, I’m going to avoid critiquing some of the stranger, more mind-bending parts of his blog post, like the above quote… which is like saying that Google’s acquisition of Windsurf is proof that coding agents are on the way out.*


---


## But first, what is an eval anyway?


Right now, it feels like everyone is reaching for a new word and calling it progress. “Offline Evals” “Online Evals” “LLM judges” “Scorers”. Fancy labels, familiar ideas.


When we blur definitions, we blur decisions. If you strip away the jargon, you have the two levers engineers have always used to understand change:


- testing changes before shipping (what Braintrust does best)


- measuring what


*actually happens* after shipping (what Raindrop does best)


Which one matters more?


**The answer is the same it has always been: the one that tells you the truth.** And in the age of agents and rapidly changing models, the truth increasingly lives in production.


Intentionally or not, the word "eval" has become increasingly vague. I've seen at least 6 distinct definitions of evals


\[eval-types\]


#### wtf is an eval?


I've seen people use the word eval to describe at least 6 different things:


1. A known input with no validator (e.g. a human, or team of humans, need to review/score the output. Sometimes there is an expected output, or at least criteria for what good looks like)


2. A known input, a known output, and ...


(Technically, A/B tests are just a type of "online evals") But when people talk about "evals" in the context of LLM's, they're usually tests. Sometimes smoke tests, other times unit tests. There's usually a test input, and an expected output. Like tests, evals are great at preventing regressions. They're useful sanity checks. They can make local iteration faster as well.


But our customers also want to know how their changes affect real world performance: which tool calls are causing issues, when agents make critical mistakes (deleting critical data, getting stuck in a loop). As agents get more capable, they become more unpredictable become and harder to test deterministically.


Monitoring is the solution. It shows you what you don’t know. Our customers can ship fast, then definitively answer the question: “Is this change (model, prompt, etc.) better or worse in the


*real world* ?” All they have to do is compare to a baseline (and this is what we mean by “A/B” test)


Now, I want to address each of Ankur’s claims individually.


## Claim 1: Evals are the future


**Reality: Test Driven Development is declining in favor of monitoring**


Thirty years ago, shipping software involved a


*lot* of testing. This was for good reason: at the time, software was shipped on CD-ROMs. If a bug was shipped, it could take years to fix.


Luckily for software engineers, things have changed since then. With the advent of the web, and the shift from client to server, bugs can be resolved in just a few minutes.


This shift significantly reduces what you need to test. Instead of meticulously testing every single environment, or debugging why a specific test is so flaky, teams can just test what matters and *


**ship faster.** *


Of course, you should still have tests for critical parts of your codebase. But for everything else, most people rely on monitoring tools like Sentry or Datadog. And, unlike testing, we’re seeing monitoring tools close that gap between bug -> fix faster than ever. Tools like Seer from Sentry can diagnose an error, pass that context to Cursor, and have a PR reviewer like Greptile approve the PR in under a minute.


In the real world, how fast you can ship and iterate does matter. When building an AI product, speed matters (this is more true than any industry in history). Eppo and Statsig were successful products, growing fast, used by some of the largest companies in history (including OpenAI and Anthropic).


Monitoring/experimentation makes


*even*


*more sense* for AI products vs. traditional software. LLM outputs are even less reproducible, even more stochastic. Underlying dependencies (the models) are deprecated faster than anything we’ve ever seen before in software engineering. There is no such thing as stable.


## Claim 2: Evals measure how good your product is


You wouldn’t brag about how many unit tests your product passes. Using evals to measure how good your product is equally silly. The real test for how good your product is how it performs in the real world.


When writing a new prompt, you often start with an idea of what “good” means. You can easily define a few test cases. But once you ship to production, the cracks start to appear. Users ask for things you never could have imagined. People on your team start to disagree what a “good” response means, for a given input.


That’s why the best AI products are defined and built iteratively. There’s a strong feedback loop between production and development. It’s some juggling act between the inherent limitations of models, which seem to change from week to week, and the needs of your users.


Funny enough, “not knowing the full scope of what you want ahead of time” is one of the most popular criticisms of Test Driven Development (which feels similar to the current wave of Eval Driven Development). And much like evals, you’re not supposed to complain about it.


Hackernews Discussion on TDD


Hackernews Discussion on TDD


Instead of evals being “the behavior you want”, in practice evals are often adversarially selected. That means, you (somehow) found a failure case, and then added it to your eval set. As a result, your evals just become a collection of issues you already know about.


How do you


*discover* issues you don’t know about? And when you fix an issue, how do you know whether or not your fix actually worked in the real-world? The answer is


**monitoring** .


Don’t take my word for it (I have an incentive for this to be true). Instead, Replit’s founding engineer wrote an excellent blog about how they think about A/B testing and evals building their coding agent:


[Read the blog here](https://giansegato.com/essays/probabilistic-era)


> ”Engineers need to keep the eval dataset up to date with … actual user behavior, but by definition \[that\] dataset will constantly lag behind. Since the input space is no longer limited, you can't simply write a few selected tests and follow test-driven development: you'll risk breaking critical features without even being aware of their existence.”


> “That’s … why testing in production, with traditional A/B tests, is also critical: … to stay as close as possible to the general population you’re serving, and have a higher chance to test a long-tail outcome.”


Agents are getting more and more capable. The tools they use are getting more and more open-ended. They’re able to run for longer and longer. If built correctly, they can perform tasks in ways you couldn’t have predicted or imagined. We no longer live in a world where you can just test a single input. Instead we have “agents” - autonomous AI’s that run for up to hours searching, running code, reasoning, and more.


The agents are too much to test. But not too much to monitor.


Traditional monitoring tools like Sentry track explicit errors (e.g. type errors, null pointers). But when AI agents fail, there’s usually no error code in sight.


It starts with what we call


**signals** . There are two types:


**semantic signals** and


**manual signals** .


Our customers create their own “semantic signals” (like, an agent getting stuck in a loop, forgetting the name of the user’s friend, or responding in the wrong language). We train custom, tiny models to look at millions of events every day and pluck these problematic events out.


Raindrop's Dashboard


We combine these semantic signals with “manual” signals sent from your app: thumbs up/down, deployment rates, model switching, and regeneration, for example.


The result is a comprehensive view of how “good” or “bad” your app is, which can be broken down by use case, model, and more.


This approach is essentially the same as what Hamel calls “error analysis.”


Hamel.dev


**Claim 3: Evals are good for rapid iteration**


Ankur claims that evals unlock rapid iteration. I found his logic hard to follow. I believe it to be false. But I’ll try to untangle it.


Ankur starts off by saying that A/B tests don’t work because it’s hard to create the variants to A/B.


> *“A/B testing assumes it's expensive to create variants.”*


> *“\[To A/B test\], you build each experience out, expose a fraction of your users to each … Each variant requires significant design and engineering work. You really can't explore 20 options at once.”*


But in the next paragraph, he describes exactly how you could use AI to automatically create variants.


> *“AI eliminates this constraint. When AI can automatically update your onboarding flow (or itself through prompt modifications) … You can now have 20 variants, or as many variants as you have users, or just one that updates automatically every 30 minutes based on real-user feedback.”*


He implies that this isn’t applicable to A/B testing. He doesn’t explain why.


In fact, it feels like a rebuttal to Eval Driven Development: a world of infinite variants implies infinite new types of issues to discover. “Good” becomes increasingly less defined.


It becomes more important to see how things changed in the real-world, not just how changes perform against a set of rigid, adversarially selected criteria.


> *“Instead of testing a handful of options and waiting weeks for results, teams can now test dozens of variations and see what works immediately.”*


“Weeks for results” is another straw man. A/B tests are much faster in practice: minutes to deploy, hours to get answers.


Imagine GPT-5 drops. With Raindrop, you can route 1% of your users to GPT-5 and instantly see how it impacts frustration, or any other signal you define. With evals, you’re stuck debugging/adapting old evals and trying to discover all of the new, unpredictable problems you haven’t yet made evals for.


The critical mistake Ankur is making here (and the one I am hesitant to point out to him) is that A/B tests on Braintrust ≠ A/B tests on Raindrop.


> *“…then A/B test how latency differences affect real users”*


That’s what Braintrust means by A/B testing. But it’s not what it means in Raindrop. Like we discussed earlier, semantic signals and intent tracking allows you to see exactly how behavior changes in a given experiment. And we’re only going to get better at this.


We answer the question: “How does X change how my agent behaves?” with ground truth-y signals from production. That’s what customers care about.


**Claim 4: Evals for Personalized Software**


Ankur implies that evals are an unlock for the future of personalized software. I believe the opposite: personalization is the most obvious rebuttal against the obsession with evals.


Imagine a future where models can truly adapt to each user: changing tone, brevity, and truly tuning itself to each user.


How could you possibly eval *


**that** *?


Imagine you have 1 million users. You’d need to somehow create evals for each of those users. You’d need to run those evals, for every single user, every single time to want to ship a change (which is, by the way, extremely expensive and slow).


Even if you could make all of these evals, it still wouldn’t be enough. Each separate tuning for each user could introduce its own new bugs. Bugs you never even imagined.


I think the right approach is obvious. Instead of millions of evals, you need just enough to perform a smoke-test. (Think ~100 cases to make sure you’re not causing known issues/taking down prod/etc)


And then? Well, you just ship. You can start with a small sample of users (in the future, potentially granular targeting based on what they use the product for)


You can then:


- Look for negative signals: anomalies, the user struggling, assistant failing to do something, negative feedback, etc, across millions of events.


- Cluster + find patterns in those negative signals (does it happen to a specific kind of user? specific scenarios? maybe specific languages/cohort/etc?)


- Understand number of events/users impacted.


Or you can write 10,000,000 evals. Your choice.


---


I don't have many bad things to say about Braintrust. Our customers that use Braintrust primarily use them 1. as a data warehouse and 2. for prompt management. I've heard they do these things pretty well. Most of our customers, however, haven't found their evals to be as useful as they thought. And they seem to be getting less useful


\[side-note-2\]


#### What about online evals? LLMs as a judge?


Technically, Raindrop is just a type of online evals. The problem is that "online evals" have become synonymous with just running offline-evals on a small sample of production data (usually with "LLMs as a judge") This isn't very useful in practice.


People love the idea of outsourcing the definition ...


.


## Further reading


For the no-bullshit, long-form version - offline evals, production monitoring, self-healing loops, and what actually works in 2026 - read


[How to Eval AI Agents - The 2026 Guide](https://www.howtoeval.com/) . It's the companion to this post.


###


Side Notes


\[eval-types\]


#### wtf is an eval?


I've seen people use the word eval to describe at least 6 different things:


2. A known input, a known output, and a deterministic validator (e.g. does the string contain X)


3. A known input, an expected output, and a LLM deciding whether the generated output is the same (e.g. does it generate the "correct" answer to a customer support question)


4. A known input and a human-aligned "LLM as a Judge" (e.g. score how funny this joke is)


5. Frontier model evals (which are a different thing all together, because they are designed to be aspirational)


6. Online evals, which is any of the definitions from 1-4, but run on a small sample of production data.


I think what I wrote applies to all, but it's worth flagging that we're not all talking about the same thing (e.g. I've seen 1 and 2 be the most useful in the real world)


6 is the most strikingly different, because people want evals as a PRD for future capabilities. With frontier model evals, you mainly want problems that the model is fundamentally not capable of solving. This is very different from the reality of building AI products.


LLMs as a judge can be useful in certain cases where it's very easy to align with human ratings.


\[side-note-2\]


#### What about online evals? LLMs as a judge?


People love the idea of outsourcing the definition of "good" to another LLM. In practice, there are issues.


**1) Calibration and reliability.** Aligning an LLM judge with human judgment is non-trivial and brittle. It requires the LLM to understand what "good" looks like perfectly. Many times, even individual team members disagree! To do it properly, you often need to build evals for your evals. Even then, teams often overfit their LLM judges on their evals. This manifests as flaky evals that fail for reasons unrelated to what they're testing (e.g. GPT-5 uses slightly different wording). It breaks the "write it once, automate forever" promise.


**2) Cost and coverage.** Running LLM judges on production outputs ("online evals") is prohibitively expensive. At meaningful coverage rates you'll spend a lot of money to evaluate a tiny slice of behavior. You can sample, but sampling reduces your ability to find failures, or understand how common they are.


**3) Pattern discovery vs single checks.** An LLM judge can score an output. It is incapable of discovering


*patterns* across millions of events: cohort-specific regressions, subtle distribution shifts, emergent failure modes of multi-step agents. Humans + analytics pipelines unlock that pattern detection; single-call judgments do not.


**4) Adversarial and reward-hacking behavior.** Where there's a measurable judge, there's an optimization target. The more distance between your target and the real world, the more Goodheart's Law applies.


LLM judges are useful as a


*narrow, complementary* tool. They are not a replacement for production monitoring, semantic signals, or controlled experiments.
