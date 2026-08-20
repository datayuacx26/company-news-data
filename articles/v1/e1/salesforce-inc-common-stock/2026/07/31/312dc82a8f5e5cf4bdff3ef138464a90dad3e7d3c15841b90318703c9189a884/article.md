---
schema_version: "1.0.0"
document_id: "312dc82a8f5e5cf4bdff3ef138464a90dad3e7d3c15841b90318703c9189a884"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-d41d574f701a"
canonical_url: "https://www.salesforce.com/news/stories/toward-self-improving-agents/"
published_at: "2026-07-23T16:19:18+00:00"
first_seen_at: "2026-07-23T17:19:23.197684+00:00"
fetched_at: "2026-08-19T08:35:07.822880+00:00"
content_hash: "sha256:1c7a22a2ed5a99eb66df3e2f7513d4e69dda16ebe6dbdb9897a116c634d7dd33"
---

# Toward Self-Improving Agents

Two teams ship an agent on the same Monday. Same model, same use case, same starting accuracy. One team activates a governed self-improvement loop around the model and its harness. The other ships the agent and moves on.


Three months later, one agent is dramatically better and noticeably cheaper to run. The other has been subjected to laborious manual patches that don’t evolve quickly enough to keep up with changing usage patterns, foundation model capabilities, and market expectations.


The foundation model both teams started with has been updated (or will be soon). But both teams got that upgrade for free; nothing about the model explains the gap between those two agents. The only thing that changed was how quickly the first agent learned from its contact with users, and how it improved at addressing the needs of those users.


Today, enterprise agents are often deployed like static applications. Improving them usually means employing human subject matter experts who spot a recurring problem, hypothesize possible fixes, and ship a patch while rolling it back if it didn’t work.


The goal of **recursive self-improvement (RSI)** , however, is to use the AI agent to automate that cycle at scale: detect what’s failing, diagnose root causes, test multiple improvements[using simulations](https://www.salesforce.com/blog/synthetic-data-for-training) , and learn from the combinations that increase performance on both technical and business KPIs.


At Salesforce,[more than 11 million Agentforce calls run each day](https://engineering.salesforce.com/how-agentforce-runs-secure-ai-agents-at-11-million-calls-per-day/) , and no two sessions are the same. The system can’t learn on its own, no matter how many calls you have, and no team can manually tune every recurring failure at any kind of reasonable cost. This is where RSI can help convert that universe of tasks, contexts, and outcomes into trustworthy upgrades to agent performance.


But if a better frontier model arrives for free every few months, why invest in a loop at all? Because the model is the one part of your agent you don’t own, and it’s depreciating fast. Stanford’s[AI Index 2025](https://hai.stanford.edu/assets/files/hai_ai_index_report_2025.pdf) found that the inference cost of leading GPT-class models fell more than 280 times in about 18 months (from $20 to $0.07 per million tokens) while the gap between the best open and closed models kept closing.


The durable value, then, lives in what compounds on top of cheap compute: searching over agent designs, learning from accumulated experience. And for an enterprise, that “experience” is your own production traffic.


The upshot is this: **A model is something you rent. A**[loop](https://www.salesforce.com/news/stories/loop-engineering/) **belongs to you, and it’s something that compounds.**


## The self-improvement flywheel


To make an agent better, you first must define what “better” means. You set success metrics like accuracy, speed, cost, and business KPIs, as well as guardrails around things like policy violations and[regressions](https://www.salesforce.com/blog/ai-agent-evaluation/) . Those definitions are business and product decisions, so owners need to be able to inspect or revise them.


> **Recent work on agents as iterative optimizers has found that greedy hill climbing often matches these more elaborate methods while requiring fewer separate evaluations, so it’s often fine to start simple.**


With definitions of what “better” looks like ingested by the agent, an automated search process begins: Measure the current performance, identify issues and root causes, evolve parts of the configuration, evaluate the candidate(s) with simulations, and keep only the changes that improve outcomes without violating constraints. Then repeat the whole process. Computer scientists sometimes call this **hill climbing** : iteratively searching for better configurations.


The default method for hill climbing is called[greedy search](https://www.sciencedirect.com/topics/computer-science/greedy-search-strategy) , where you accept the first improvement found. It’s the simplest approach in a broad optimization toolkit that also includes[evolutionary methods](https://www.sciencedirect.com/topics/computer-science/evolutionary-algorithms) ,[multi-armed bandits](https://towardsdatascience.com/the-multi-armed-bandit-problem-a-beginner-friendly-guide-2293ce7d8da8/) , and[reinforcement learning](https://www.salesforce.com/agentforce/reinforcement-learning/) . Recent work on[agents as iterative optimizers](https://arxiv.org/abs/2603.27415) has found that greedy hill climbing often matches these more elaborate methods while requiring fewer separate evaluations, so it’s often fine to start simple.


Whichever optimization method you use, the resulting changes are usually larger than a prompt but smaller than an entire product: the system around the model, not the model itself. An optimization engine watches what the agent does; simulates discrete, testable changes; and learns from the combinations that work to repeat the cycle.


Here’s what that flywheel can look like in practice when you optimize the system around the model instead of the model weights themselves:


Open Image Modal *The frozen-weight self-improvement loop is a flywheel in which the agent powered by a fixed frontier model continuously improves through interconnected cycles of autonomous observation, diagnosis, simulation, and human input.*


# Image Modal


*The frozen-weight self-improvement loop is a flywheel in which the agent powered by a fixed frontier model continuously improves through interconnected cycles of autonomous observation, diagnosis, simulation, and human input.*


This is a set of loops, with feedback and[feedforward](https://en.wikipedia.org/wiki/Feed_forward_(control)) loops inside them. For instance, observation and diagnosis often iterate together. What you see changes the diagnostics, and a new diagnosis changes what you look for. Generation and simulation can iterate the same way. When a failed evaluation doesn’t advance, it produces new hypotheses to test. A promoted change produces new production evidence, starting the whole cycle again.


> **If the base model is frozen, how much can an agent really improve? The answer is: more than you’d expect.**


“Learning,” or compounding, is what makes this a[flywheel](https://www.linkedin.com/posts/deryaisler_agentflywheel-aiproductleadership-activity-7465776744399032320-1-F9/) . Every validated fix becomes a durable regression test, every new skill joins a shared library, every routing improvement gets versioned into configuration, and lessons distilled by the optimization engine improve its issue detection and resolution accuracy. Gains persist – they don’t evaporate between cycles. **Each spin of the wheel makes the next one faster.**


Much like permission modes in Claude Code or other coding agents, any enterprise can configure this cycle to require explicit human input for review, governance, or gates at any stage. It simply depends on how fast they want the flywheel to spin and how autonomously, which is usually a question of scale: At one to two agents, manual gating can work well. At 10–100 agents, governed autonomy becomes a must.


Of course, some diagnoses point outside the agent’s prompts altogether, to data sources like a knowledge base, tools, policies, business logic, or[other A2A agents](https://www.salesforce.com/agentforce/multi-agent-orchestration/?d=cta-body-promo-8) that can all be evolved as well.


## Frozen weights are an aggressive starting point


Today, most enterprises don’t retrain a frontier model’s core weights themselves. That’s usually the provider’s domain, though managed fine-tuning options are expanding. A natural question is: If the base model is frozen, how much can an agent really improve?


The answer is: more than you’d expect. Frozen weights constrain what you can change – you’re not retraining the model – but they don’t limit how you improve. Prompts, tool configurations, knowledge in vector stores, workflow sequencing, retrieval strategies, evaluators, permission sets… all of that, and more, is open territory. It is even still possible to run full reinforcement learning (RL) on an agent whose model *and* prompt are frozen. This works by rewarding optimizations to the system *around* the model, such as the agent’s memories and harness, instead of the model itself.


Salesforce AI Research was among the first to demonstrate RL-like techniques for optimizing a frozen-weight agent, with 2023’s[Retroformer](https://arxiv.org/abs/2308.02151) , a model which learned to tune the frozen agent’s prompts in response to new environments. Better results, no weight updates. And we’ve come a long way since.


That’s why freezing the weights is the aggressive move, not the timid one. When the learning lives “outside” the model, every change is something you can see, test, and undo. A failed but automated experiment costs only seconds, not a dayslong retraining run. **The system actually evolves faster** ***because*** **the model holds still.**


## Where adaptation happens


The question isn’t whether an agent can self-improve, but how deep into its own design you let that improvement reach, and whether those changes persist. For example:


- **Context: Change what the agent “knows.”** This is the narrowest but most common example of adaptation. The agent reflects on what went wrong and carries that forward.[Reflexion](https://arxiv.org/abs/2303.11366) does this in plain language. The agent writes itself a note after a failure and uses it to steer the next attempt. No training, no weight change. The improvement lives in memory.
- **Routing: Change which “path” it takes.** Instead of forcing every task through one configuration, the system builds specialized branches – different prompts, skills, tools, and so on – and directs incoming work to the best fit. Our colleagues at Amazon demonstrated this with[Adaptive Auto-Harness](https://arxiv.org/abs/2606.01770) : build branches, then route.
- **Workflow or harness: Change how the agent itself is structured.** This is the deepest reach. You’re not changing what the agent knows or which path it takes; you’re changing how it’s built.[AFlow](https://arxiv.org/abs/2410.10762) , for example, treats the agent’s workflow as code and searches for better arrangements of where generation, review, and iteration can occur. DeepMind’s[AlphaEvolve](https://deepmind.google/discover/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/) famously pairs proposed changes with automated evaluators. Sakana AI’s[Darwin Gödel Machine](https://arxiv.org/abs/2505.22954) rewrote a coding agent’s own structure while keeping model weights fixed, doubling its success rate on a standardized software engineering benchmark.


These are select examples, and they aren’t equally mature. Context adaptation – learning what to retrieve and remember – is production-ready today. Routing adaptation has strong research evidence but less deployment history. And workflow rewriting, often with meta-harnesses, remains active research.


What can this all look like in practice? DoorDash offers one production example of these layers reinforcing each other. In its[agentic food metadata engine](https://careersatdoordash.com/blog/building-food-metadata-with-llm-juries-context-optimization-multimodal-ai/) , an AI-based evaluation layer improved annotation accuracy by roughly 20% over typical human reviewers. Those evaluation signals then fed back into what the system “knew”: an automated loop they claim accelerated development by 10 times without updating the serving model’s weights. The improved labels then enabled smaller, cheaper models to rival frontier quality at about 10% of the claimed inference cost. Each layer of the loop made the others better.


## This flywheel can spin backward


The hard part of self-improvement isn’t only producing candidate changes. Indeed, modern vanilla LLMs can generate many plausible edits to prompts, tool descriptions, plans, skills, and code. One of the more difficult problems is forecasting and then measuring whether a change will actually improve the agent across the scenarios that matter.


A misconfigured self-improving system can make a bad change persistent. If the evaluator measures the wrong target – say, cases closed rather than cases actually resolved – the agent might learn to close cases faster by giving shorter, less helpful answers. The score goes up. The customer experience gets worse.


We call this “reward hacking,” and it isn’t hypothetical. During a[sandboxed Darwin Gödel Machine experiment](https://sakana.ai/dgm/) , the same system that doubled a coding agent’s success rate was told to raise a “no-hallucination” score. One self-modified variant stopped logging the markers used to detect hallucinations, earning a perfect score… without fixing the behavior. Archived lineage made it at least visible, but the incident showed that even a strong proposal mechanism will still exploit a weak evaluator. A 2024[Nature study on model collapse](https://www.nature.com/articles/s41586-024-07566-y) found similar degradation when models were recursively trained on their own generated data.


The lesson for enterprise agents: Generated output should rarely become persistent memory, technically called policy, without verification in an external environment. The fix is structural. A subagent that proposes a change to optimize for one goal should be checked by subagents focused on other goals. Accepted changes should depend on multiple forms of evidence, including simulation results, regression suites, adversarial cases, and often, human judgment. The evaluators themselves also need calibration and drift monitoring.


The company that can verify useful changes fastest is the company whose agents improve fastest. **Verification speed becomes capability acceleration.**


## An advantage that appreciates


So, picture those two teams again, three months on. The model upgrades arrived for both (probably for free). One agent is just a bit better than the day it shipped. The other has agentically turned production experience into hundreds of incremental optimization and improvements producing capabilities no competitor can download – cheaper to run, harder to beat, compounding quietly in the background – with every gain recorded and auditable.


Enterprise agents won’t be judged by how clever they were on day one. **They’ll be judged by how much they compounded by day 100, and whether the loop that did the compounding was one you could trust with your proprietary data.**


The differentiator isn’t just which model your agent runs. It’s whether your agent is wired to learn from its own production traffic, safely, legibly, with every gain something you can inspect and undo. The model is a rental every competitor can buy. The self-improvement loop is an asset that appreciates.


At Salesforce AI, we’re dogfooding these automations and testing them in enterprise settings with a variety of flagship design partners – including agents that optimize other agents using similar “governed autonomy” loops.


The thesis is universal: The agents that win aren’t the ones that start smartest. They’re the ones that keep learning on their own.


*The information in this article is provided for informational purposes only. Salesforce makes no guarantee about its accuracy or completeness.*
