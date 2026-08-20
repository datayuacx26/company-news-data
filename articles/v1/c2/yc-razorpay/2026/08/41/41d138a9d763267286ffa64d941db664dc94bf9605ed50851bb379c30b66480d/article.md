---
schema_version: "1.0.0"
document_id: "41d138a9d763267286ffa64d941db664dc94bf9605ed50851bb379c30b66480d"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/the-winner-doesnt-take-it-all/"
published_at: "2026-08-04T06:34:17+00:00"
first_seen_at: "2026-08-04T07:08:41.517411+00:00"
fetched_at: "2026-08-04T09:43:50.004226+00:00"
content_hash: "sha256:da17bdac94a5776540a75903504a26c0b42d55699e9f2dda94fb448ded88de83"
---

# The Winner Doesn’t Take it All

*Evals built by Vinayak Kamath P*


We are burning more tokens than ever before, and we think that is a good thing. Token maxing is how you earn the right to make the harder calls that come after it. Where cost actually matters, which model fits which use case, and what durable AI platforms we will have to build as models are becoming commodity. A couple of months ago almost every AI workload at Razorpay ran on a single frontier model. Today we run a fleet of models, open source and frontier, each picked for a specific job.


[Slash](https://razorpay.com/blog/razorpay-engineers-built-slash-slash-builds-the-rest/) handles autonomous coding, pull requests, and integrations across dozens of internal systems. Slash uses open source models for a large category of use cases today. We did not want to guess our way there, so we built an eval framework to make these calls with evidence instead of opinion, and this post is about how it works.


Models are on their way to becoming a commodity, and betting on any one of them is a fragile choice. Having said that, though our token usage is at an all-time high, our AI cost is not. Here is what that means in practice:


- **Developer work is not one problem:** Planning a feature, building a feature, a code review, a stack trace summary, a ticket triage, and a doc lookup do not ask the same thing of a model, and no single model is best at all of them at once.


- **The gap between models is closing:** Open models now match or beat frontier models on plenty of real tasks, and which one leads changes month to month, so the interesting question stopped being “which model is best” and became “which model is best for this”.


- **Optionality is the asset:** If you can measure any model on your own work in an afternoon, you are never locked in. When something better ships you adopt it, and when a model regresses you move off it. That freedom is worth more than any single model choice.


- **Cost follows, it does not lead:** Spend went up as agents took on more of the SDLC. Picking the right model for each task keeps that in check, but the savings are a side effect. The goal is picking well.


So we modeled every query Slash gets, evaluated each one against a slate of models, and picked the model that suits the use case. We built eval frameworks for both coding and non-coding work, and this post covers the one we built for development.


## Why not just use public benchmarks


Before building any of this, we had to answer an obvious question. Why write our own evaluations at all, when public benchmarks already exist? Two reasons.


The first is contamination – public benchmarks leak into training data, and once a benchmark is in the training set, a high score stops telling you much. The corpora get polluted, the tests themselves become something models have effectively studied for, and the numbers drift away from real ability. If you want an honest signal, you need a set of problems the models have not already seen, which means one you built and kept to yourself.


The second is that benchmarks grade the model, and we do not run a model. We run an agentic system, and the model is only one part of it. The harness, the skill tree, the plugins, the workflows, the context layer, all of these are tunable knobs, and they behave differently with each model. A model that tops a public leaderboard can lose once you wire it into your harness, and a modest one can pull ahead with the right context and tools around it. No external benchmark can see any of that, because it is specific to how you have built your stack.


It also means the decision itself is not one number. Accuracy from the evals is only the start; the real call weighs cost, latency, how large a PR the model can handle, whether its changes clear the merge gate, how it treats security, how many real bugs it catches, and how often it waves through issues a human reviewer would have flagged. A generic benchmark can speak to a few of these. The rest are specific to our product and our workflows, which is exactly why building our own made sense. V1 of Slash SWE-Bench weighs all of it together, so the pick reflects the job the model actually has to do.


So for an AI-first organization, bespoke evaluations are not a luxury. They are the only way to measure the thing you actually ship.


### **From one model to a fleet**


Standardizing on a single frontier model was the right call to start, because it gave us one behaviour to reason about while we got the platform working. Once Slash was handling real volume, keeping one model for everything meant accepting a less optimal model for most of the work. So we stopped treating “which model” as one decision and started treating it as many. Each of the query types above rewards different strengths, which makes the right model a separate, empirical question whose answer changes as models improve.


Slash coding leverages open source models heavily because the evals said the open models were as good or better on the work that matters to us. That choice is not carved in stone, and it does not need to be. The value is not that we landed on open source, but that we can ask the question again the day a new model ships and trust the answer by the end of the day.


### **What “evaluate every query” actually means**


An evaluation, in our world, is a use case + a corpus of real examples from our own work + a way to score the output. We take a representative slate of queries for a use case, run every candidate model against the same items under the same conditions, and score the results the same way, so you get a ranking you can defend, with error bars, not a gut call.


It also means the model is only one of the levers we test. What actually answers a query is a whole workbench – the model, the agent, the harness, the metaharness above it, the skill tree, the MCPs. Every one of them can be tuned, and each change moves the result. Swapping the model is just the most common experiment; the same framework lets us hold the model fixed and change the harness, a skill, or a tool instead.


The hard part is not running a model once but running many models across many items, over and over as new models arrive, without the plumbing turning into a pile of one-off scripts. That is what the framework solves, and it is what keeps our optionality real rather than aspirational.


### **The eval framework**


V1 of Slash SWE-Bench’s core idea is a clean split – a domain-blind framework on one side and eval plugins on the other.


The framework owns everything that is not specific to a use case: model access through a single gateway, run orchestration, the judging machinery, corpus handling, statistics, and plugin assembly. A plugin owns only its domain, meaning what an item is, how the candidate model is exercised, and what the prompts say. Nothing below the assembly layer ever reaches into plugin code, so the framework stays stable no matter how many evaluations we stack on top.


The result is that authoring an evaluation is mostly config, not code. A working evaluation is two files, a manifest and a rubric prompt – you drop them under a folder, the plugin is discovered automatically, a validation pass checks everything without spending a token, and one command runs it.


We think of authoring as a ladder, and most evaluations never climb past the first rung:


- **Manifest only:** A stock corpus, a stock runner, and deterministic scoring, with zero code.


- **Add a rubric prompt:** A panel of judge models now scores each answer against your rubric.


- **Add hook functions:** A custom qualifier, normaliser, or runner, for when your domain needs one seam of real logic.


- **Add custom pipeline phases:** A whole stage the stock pipeline does not have, for the rare evaluation that needs it.


Most evaluations never need code at all. The first rung is pure configuration, and a golden-answer evaluation, where every item has a known correct answer, runs with no custom code and no judge model because the scoring is deterministic. Writing code is the exception, reserved for the higher rungs where a domain needs a step the stock components do not provide.


### **Making the numbers trustworthy**


A model ranking is only useful if you believe it. So most of the engineering in Slash SWE-Bench went into trust, not into calling the models.


Start with fairness. Item selection is seeded and deterministic, meaning the same corpus and seed always pick the same items in the same order. Every model sits the same exam, and any two runs can be compared item by item. Validation is spend-free, so a broken evaluation fails before it costs a single token.


Then, safe failure. Judging fails open and rules fail closed. If a judge call flakes, the item is marked unadjudicated instead of being guessed at. If a decision rule is unknown, the run raises an error instead of quietly picking one. A bad call never silently corrupts a result.


Raw judge votes are always stored, and every decision is derived from them at read time. Change the decision rule and you can re-score everything without re-running anything. Checkpointing is additive, so re-running a finished evaluation does nothing, and adding one new model runs only the new work, not the whole slate.


On the statistics, we do not report a single accuracy number and call it a day. Results carry clustered confidence intervals computed by item, so a model that got lucky does not get promoted over one that is actually better.


Who said there aren’t fun


*systems* problems in the world of AI?!?


### **What we do with the results**


The output of all this is a boring, repeatable decision. For this use case, this model, at this confidence. That decision is what lets us run a fleet, not one model stretched across every task, with coding work going to the open models that won their evaluations and other use cases going wherever the evidence points. When a new model lands we do not argue about it. We add it to the slate, run the delta, and read the number.


The results back this up in practice, not just in theory. Across our own production code-review evaluation, one model leads on precision, a different model is most trusted at the merge gate, an open model is the quietest reviewer, and a different open model is the cheapest. Four axes, four different winners.


That is the real payoff, and it is not a model or a cost number. Whatever changes next, a new model, a new harness, a new tool, we can ask “which workbench, for this” and trust the answer, and as the models keep converging, that advantage compounds. The winner doesn’t rule it all.


### **What’s next**


This post covered the framework we built for development workflows. We have also invested heavily for non-coding use-cases and leveraging the right models for sales, finance and other functions within the company. Additionally, we are doing a lot of interesting work on building a unified knowledge base that all our workflows rely on. We will get to all of these in the next few weeks.
