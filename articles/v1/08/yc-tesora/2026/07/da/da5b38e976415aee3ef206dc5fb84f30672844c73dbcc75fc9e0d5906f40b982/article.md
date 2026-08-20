---
schema_version: "1.0.0"
document_id: "da5b38e976415aee3ef206dc5fb84f30672844c73dbcc75fc9e0d5906f40b982"
company_key: "yc-tesora"
company: "Tesora"
source_id: "yc-tesora-rss-4d7d570f276c"
canonical_url: "https://tesoraai.substack.com/p/ai-benchmarks-are-up-why-arent-most"
published_at: "2026-07-14T15:15:08+00:00"
first_seen_at: "2026-08-10T03:58:35.856576+00:00"
fetched_at: "2026-08-10T03:58:36.939902+00:00"
content_hash: "sha256:27f443696f4b29da4076ffee282a1f14fa8d2b21ef0f676e67edf92d5a545a1b"
---

# AI benchmarks are up. Why aren’t most actuaries realizing these results?

# AI benchmarks are up. Why aren’t most actuaries realizing these results?


### How grades measuring AI over-represent its performance and hide its inability to scale to actuarial disciplines.


[AI for Actuaries @Tesora.ai](https://substack.com/@aiforactuariestesoraai)


and[Philo Bishay](https://substack.com/@philobishay)


Jul 14, 2026


AI labs have been telling us all year that their models are getting exponentially better. Across almost every capability that matters, whether reasoning, coding, tool use, or agentic behavior, scores climbed every quarter, and the actuarial consulting firms have clamored with excitement.


[WTW’s 2026 Advanced Analytics and AI Survey](https://www.wtwco.com/en-us/news/2026/03/insurers-using-advanced-analytics-and-ai-report-strong-returns-on-investment-and-premium-growth) reports that property-casualty carriers using sophisticated analytics ran combined ratios six points lower than slower adopters between 2022 and 2024 and grew faster to boot.


[Deloitte’s most recent industry survey](https://insurtechdigital.com/news/deloitte-insurers-deploy-gen-ai-across-76-of-organisations) reports that 76 percent of insurance organizations have deployed generative AI in one or more business functions. Given numbers pointed that consistently in one direction, one would reasonably expect actuaries to be reaping meaningful benefits by now.


As it turns out, they are not. Adoption is still piecemeal and benefits remain narrow.


[MIT’s State of AI in Business 2025 report](https://mlq.ai/media/quarterly_decks/v0.1_State_of_AI_in_Business_2025_Report.pdf) puts the failure rate for enterprise generative AI pilots at roughly 95 percent, and insurance-specific research from


[Datos Insights](https://datos-insights.com/ai-in-insurance-hub-research-insights/) finds that 53 percent of carriers remain stuck in pilot phases while only 5 percent have achieved widespread adoption. Nearly every carrier is investing, and almost none of them are running the tools at scale. This raises a question worth taking seriously: why is reality so far away from expectation?


Even pants require at least two numbers to approximate a fit, and yet with all the complexity of modern language and knowledge, we reduce agentic system scores to a single dimension


One reason is simply flawed grading. The “huge improvements” being published are single-score point estimates that assume underlying failures are random. For all the complexity built into agentic systems, nobody stops to ask how such a multi-faceted system can be summarized by a single score. Even pants require at least two numbers to approximate a fit, and yet with all the complexity of modern language and knowledge, we reduce agentic system scores to a single dimension. The dimensions excluded by the score hide a systematic failure in areas actuaries rely on.


If the failures were indeed random, users would expect to achieve the published results. But it turns out they aren’t random; they’re consistently deficient at specific tasks.


[Stephan Rabanser and colleagues at Princeton](https://arxiv.org/abs/2602.16666) ran fourteen leading agentic models many times against slight variations of the same problems, and found that a model rated eighty-five percent on a leaderboard did not solve eighty-five percent of instances reliably. It solved most consistently, and it failed on a stubborn minority in ways that repeated across runs. Because the failures were non-random, the model deficiency was structural and invisible to any measurement that reported only the mean.


Worse still, non-random failure is only part of the diagnosis. A separate study by


[Aaditya Khanal and colleagues at Northern Kentucky University](https://arxiv.org/abs/2603.29231) , working under the label “beyond pass@1,” ran ten models across roughly 23,400 episodes and showed that reliability decays with task duration. The longer the task, the worse the hidden failure gets, and the decay is faster than a random error rate would predict. This is not good for any actuarial applications extending past just the narrowest queries.


So how do we do a better job grading AI? Rabanser et al


[proposed](https://arxiv.org/abs/2602.16666) the missing measurements. Four axes, specifically:


An actuary reading that list has already been doing all four under different names. Consistency, getting the same answer on a rerun, is like back-testing. Robustness, whether the models hold through a range of inputs, is sensitivity testing. Predictability, whether errors cluster in ways one can anticipate, is a reasonableness range. Safety, in AI the bounded severity of the worst case, is a tail scenario or cat load. AI researchers arrived at reliability just as actuaries did, albeit through a different route and lexicon.


These new dimensions begin to explain why models fail when faced with actuarial complexity. Consistency and robustness are already thin, and the failures worsen as tasks gain more steps and nuance. A frontier model can help an actuary develop a single triangle, but give it a holistic task spanning data cleansing, decision making, and deployment, and the output falls well below a useful response. The


[Berkeley Function Calling Leaderboard](https://gorilla.cs.berkeley.edu/leaderboard.html) shows that models that ace one-shot single-tool tasks stumble sharply when they must remember context, manage multiple tools, or decide when not to act. Agentic scores drop well below single-turn scores across nearly every model on the leaderboard. “Comprehensive” AI assistants are measurably worse than narrower agents because complexity is where they fail.


Some actuaries may find the inflated promises of AI familiar. Consultants have oversold model performance before. When GLM vendors first pitched their software into pricing departments, lift curves were the metric consultants used to estimate model improvement and ROI. The pitch assumed the model was reflective of reality. Actual results never quite met that lift curve. Over time, actual results became the true compass, performance against a holdout, and the lift curve became a projection to be checked against experience rather than a promise to be believed.


As with GLMs, AI technology is clearly valuable, but the promise runs ahead of the delivery. The solution would be wrapping specialized agents into a harness. Individually, agents are faster and more accurate than humans at narrow tasks. But that specialization creates the next problem, which is keeping the agents honest and on task. Without a properly engineered harness, individual agents will hallucinate (in good faith), and no benchmark score today describes the failure patterns. For an actuary to see the operational gains that WTW and Deloitte are measuring, the investment will not be in the model, but in the harness around it: task decomposition, audit layers, and programmatic session recovery.


Early AI adopters all see promise, but the devil is always in the details. Long term winners and scalers will be those who invest in the overall harness, not just the piecemeal deployment which would otherwise limit the technology to the abilities of a few select super-users.


A guest post by


[Philo Bishay](https://substack.com/@philobishay?utm_campaign=guest_post_bio&utm_medium=web)


Head of Product, former chief actuary, MBA, lover of learning


[Subscribe to Philo](https://philobishay.substack.com/subscribe?)
