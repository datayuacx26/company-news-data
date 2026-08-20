---
schema_version: "1.0.0"
document_id: "fd226ef291798f9fefbcecce422ff669b9ce2fdb589726833df863917fab6c41"
company_key: "yc-melder"
company: "Melder"
source_id: "yc-melder-news-import-4912f91d0d40"
canonical_url: "https://melder.io/blog/problem-with-ai-benchmarks"
published_at: "2025-08-11T21:21:00+00:00"
first_seen_at: "2026-07-22T04:01:37.407515+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:82caa96db2b226ec39e420140627c8491f6d543da9bfc569fce961db1f719fd5"
---

# The problem with AI Benchmarks

With the release of GPT-5, I think it's time to discuss public benchmarks and why they are starting to matter less. Then, in the absence of benchmarks as your guide, how you should figure out what models to use.


In 2022/2023, benchmarks were a core talking point when it came to the best models. Some popular ones we re MMLU, AIME, and HumanEval. The basic structure of these benchmarks is the same – we give AI a set of hard problems then see how many the LLM can get correct out of the set. It's pretty similar to standardized tests for college/graduate school entrance exams.


They seem to make sense. If the AIs can do these tests well, they must be getting smart. And now the AIs do pretty well! Here's the latest scores per Vellum AI on some top benchmarks (I chose them for the pretty charts).


Source: https://www.vellum.ai/llm-leaderboard, August 11, 2025


You can see all the models are shooting up the rankings, and are tightly grouped around being pretty damn smart. In fact, so much so that the difference feels negligible between top models. Is a 1% point difference really meaningful enough that you can assert that one model vs. the next is better for average human tasks? The answer is definitively no. In fact, model providers are often barely eking out increases compared to the current best models when released – indicating a bit of gamesmanship involved to take over top spots.


So that's why we made harder evaluation sets. The International Math Olympiad (IMO) is a good set of hard problems. For those unfamiliar with IMO, it's the world's premiere math competition where literal genius 17-year-olds duke it out for best young math problem solver in the world. This year only 46 participants scored a Gold Medal. But both Google and OpenAI's models got gold too. So even super hard questions seem to be tough for evaluating best models.


There are other harder exams yet – like Humanities Last Exam that takes super hard questions from a bunch of world experts that are unlikely to be directly in LLM training data. And leads here are wider (current leader is GPT-5 at 42%, then Grok-4 at 25%). But these questions are very uncommon for everyday queries to look like:


Source: https://agi.safe.ai/


Do your ChatGPT messages look like the problems I showed?


No – in fact, almost none of the popular training sets look like real usage. Here are a few reasons why what we actually care about differs:


1. Most questions are not simply 'wrong' or 'right'
2. Most user problems are poorly defined
3. Agents are getting popular, and they pose a ton of interconnections of these problems


For #1, let me give you examples of my recent ChatGPT/Claude questions:


- Tell me the most important things when learning Spanish?
- What are great hidden gem desserts spot in Manhattan?
- Can men and women be friends? *I was watching When Harry Met Sally


None of these have 'right' answers, and it's unlikely that different users even consistently prefer the same ones. There's a lot of preference baked in.


For #2, let's look at a few more examples:


- What is a standard list of CS coursework?
- What are the consensus top 20 movies since 2020?
- Explain the most important SALT tax information and recent changes?


These probably trend a bit more 'right/wrong' in how I'd assess them – but not because I provided enough information to judge that. I'm more so evaluating the model's ability to gauge what I might want done in these situations. Judgement questions like good sources, what is/isn't relevant in a list, etc. This is an important skill for employees to understand, and the models need to be judged on this too. They mostly aren't in today's evals.


Regarding #3, there technically are agentic evals today – but they're still uncommon (you can see one in the chart from Vellum). Many of the most popular apps today are agents, needing to perform a ton of actions in sequence solving a bunch of ambiguous problems. The best example is Cursor (The AI Code Editor).


For example, let's say an engineer tells the Cursor Agent to build out an API integration for Gmail following the existing patterns in the code base. Cursor needs to search the codebase for existing code, pull in Gmail API docs, determine Auth needs, etc. It may also need to decide what features of the Gmail API are required and not. Some of these decisions Cursor could get right, others it could get wrong.


The question is open-ended enough that it could also just get different answers that are still valid. If the user had told the agent 'I need XYZ functions that pass these battery of tests', then of course we could provide evals. But ultimately, most users underspecify their agent requests and just see if the agent figures it out.


Given the breadth of these challenges, I think what really matters is user feedback. Over enough volume of testing, the best user experience will shine. Which means…


**You should figure out what models are best by talking to users.**


Of course, if you are the only user you care about this can be tough. That's why you should also ask users of similar products. The reason we use Anthropic models in the Melder Excel Agent today is because Anthropic has consistently been the best agent model from our code generation experience over the last 6 months, and many good engineers we know think similarly. When that changes, we will reconsider. Other versions of 'talk to users' include user interviews, A/B testing, and other metric tracking across model examples.


Another great way to evaluate new models is by building super realistic and good internal evals. But this is really hard and subject to constant changes to stay relevant. *There's a lot of low hanging fruit in just figuring out what people like by asking.*
