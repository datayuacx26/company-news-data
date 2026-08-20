---
schema_version: "1.0.0"
document_id: "2d914238f12cf201918fcad8f3ac9f8c0797ea2272eff6adc9d906c1c6ed979f"
company_key: "yc-doe"
company: "Doe"
source_id: "yc-doe-news-import-a3f9325ff0ec"
canonical_url: "https://doe.so/blog/do-not-bet-on-the-model"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-21T16:44:00.145230+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:78fc67e89e4514330bb925b64b3cfbdeae0eb916bddd384b68975d242968a948"
---

# Do Not Bet on the Model

Every few months, the default model changes.


First everyone builds around GPT. Then Claude looks better for code or long documents. Then Gemini gets cheaper or faster for a different class of work. Then an OSS model clears cleanup at a fraction of the cost.


There is no single intelligence layer to buy once and forget.


Inside a company, that becomes subscriptions, prompts, uploads, and context scattered across teams. The bill goes up before the work gets easier to move.


Uber shows what happens when the tools are useful.[Its CTO said](https://www.indiatoday.in/technology/story/uber-cto-says-ai-spending-plans-fall-short-as-tools-like-claude-code-drive-costs-up-2896621-2026-04-15) AI coding tools had already exceeded the budget he thought the company needed. Uber did not have an adoption problem. The tools worked, people used them, and usage outran the plan.


The answer is to keep the work stable while the intelligence behind it changes, not to freeze usage or crown one model.


A contract clause may deserve frontier prices today; admin cleanup probably does not. Some work needs speed, some needs a private route, and some can move to OSS once task-specific evals show the same acceptance rate.


The route changes because the market changes.[METR](https://metr.org/time-horizons/) tracks longer task horizons.[Epoch](https://epoch.ai/trends/) tracks falling fixed-performance inference prices.[OpenRouter](https://openrouter.ai/state-of-ai) shows how quickly OSS usage can move.


Model share keeps moving. The company should benefit from that movement instead of rebuilding its process around it.


That movement only matters if the company can switch models without rebuilding the work.


That is the investment: not a model prediction, but the ability to keep choosing the best cost and intelligence tradeoff as the market changes.


score=P(accepted)×work value−model cost−review cost−delay cost−failure risk−switch cost\\text{score} = P(\\text{accepted}) \\times \\text{work value} - \\text{model cost} - \\text{review cost} - \\text{delay cost} - \\text{failure risk} - \\text{switch cost}


score


=


P


(


accepted


)


×


work value


−


model cost


−


review cost


−


delay cost


−


failure risk


−


switch cost


Run each task where that score is highest. The winner changes when prices fall, smaller models close the gap, privacy needs a private route, or the task becomes routine.


The right model for a task is not a brand preference; it is a route decision.


## Do Not Make People Remember the Tradeoff


Today the route is folklore: people remember Claude for one kind of writing, ChatGPT for another, a coding agent for engineering, a private model for sensitive data, and a cheap model for cleanup. That works until price changes, quality moves, or a review failure shows up.


That is manual switching, not adaptability.


The system should hold the context, permissions, task history, reviewed examples, tools, and final destination. Then the company can change the model behind the work without asking every team to move its prompts, files, approvals, and habits again.


That is what makes a moving model market useful. A new frontier model can improve hard work. A cheaper model can lower cost on routine work. OSS or a private route can handle work where data control matters. The company gets better options without making people rebuild the workflow each time.


That is why Doe is built around the work itself. A task keeps its source trail, permissions, reviewer, decision, writeback, and correction for next time. If Claude is the right route today and Gemini or an OSS model is the right route tomorrow, the company should get the cheaper or better path without moving the work again.


Do not bet the company on a model. Bet on the system that lets the company keep changing models while the work gets cheaper, better, or safer.
