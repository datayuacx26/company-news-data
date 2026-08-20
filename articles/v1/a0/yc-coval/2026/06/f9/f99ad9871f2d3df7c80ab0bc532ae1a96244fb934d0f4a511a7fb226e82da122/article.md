---
schema_version: "1.0.0"
document_id: "f99ad9871f2d3df7c80ab0bc532ae1a96244fb934d0f4a511a7fb226e82da122"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/our-vision-for-voice-ai/"
published_at: null
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:01dc7f89f98f725660a78ab79cbd8d061dbd3bf3689f02aecd59062e9240628a"
---

# We raised $28M. Here's what's next for us.

At Coval, we talk a lot about the future of voice AI and agentic self-improvement loops. Last week we announced our[$28M Series A](https://www.coval.ai/blog/coval-series-a) , and today I want to share the future we’re seeing take shape.


Within three years, every business, from enterprise to mom-and-pop, is going to have voice agents regularly serving customers and clients. The way a modern business needs a website, a trustworthy 24/7 voice agent will become a necessity of relevance. These agents will support a broad array of integrations and reliable behaviors. Many of them will be genuinely useful, and some will be mind-bogglingly terrible.


The keystone word there is *trust* . Trust has to be earned, from executives and customers alike, through consistent agent improvement loops.


We’ve seen this problem before. The reason we let autonomous vehicles onto public roads is that the evaluation infrastructure finally got rigorous enough: billions of simulated miles before a single real one. Voice needs that same foundation, and almost no one has built it yet.


Today, Coval tests and reports. You simulate thousands of conversations to regression test before launch, evaluate every call in production, and sharpen your non-deterministic metrics with human review. That’s only the foundation of our next leap forward.


We’re turning Coval from a tool you open into a cross-platform agent optimizer that runs the recursive self-improvement loop where you prefer to work. We believe that’s the shortest path from *“I have an agent”* to *“my agent’s performance improved an average of 18% every month for the last six months.”*


## The improvement loop


Internally, we draw it as a loop. Every production agent rides the same cycle: define agent behaviors, create test cases, find what’s breaking, ship a fix, prove it didn’t break something else, and do it again.


Many teams start with manual testing, and that’s a good idea,[until it’s not](https://www.coval.ai/blog/manual-qa-voice-ai) . Every organization eventually hits a tipping point of volume or complexity where QA by hand is no longer plausible. An effective agent improvement loop has multiple stages. Tap a stage to see what it does, or play the loop to watch it run, with your metrics at the core.


## The agent story


We’re building Coval agents that work the loop with you, in three places.


#### Setup: agents that create your evals


Setting up an eval suite is the highest-friction part of using Coval today. It shouldn’t be. Coval reads everything it knows about your agent and proposes the suite; you’re the reviewer.


- **Recommended evals, with receipts.** Each recommendation cites its source, like a` successful_handoff` metric drawn from the 14 calls where your agent transferred to a human.
- **Metrics from a sentence.** Type “Did the agent verify identity before discussing account details?” and Coval drafts the rubric, runs it on prior calls, and shows you where it disagrees with a human grader.
- **Personas from real conversations.** Generate and update personas from the calls you’re already monitoring, instead of writing them by hand.
- **Test cases from your context.** Build them from past live calls, or from your prompts, documents, and workflows.


#### Insight: understand your conversations at scale


Monitoring becomes a search bar on any work surface. @mention the same questions you’d ask a teammate in Slack, Linear, or GitHub, and use what comes back, whether it’s you or your coding agents, to improve agent performance.


- **Find anything in plain language.** “Show me calls over five minutes where the agent didn’t escalate a fraud issue.”
- **Occurrence rate on demand.** Quantify how often something is actually happening, the moment you wonder.
- **Trends you weren’t tracking.** Coval, not your customers, is the first to flag a new failure cluster.
- **A daily Slack digest.** A per-agent morning summary that reads like a narrative, not a notification firehose.


#### Optimize: close the loop with self-healing agents


This is the shift from a measurement product to a workflow product. Today, when an eval surfaces a regression, you leave Coval to do anything about it. We want the team’s rhythm to become *“Coval identified a bug, found what broke, proposed the fix, helped implement it, and quantified the improvement for our team and leadership.”*


- **A coding agent that ships fixes.** It proposes a prompt, tool, or guardrail change, runs it against your eval suite, and opens a PR with the evidence: pass rates, failing calls now passing, no new regressions. You approve.
- **Prompt optimization.** A lighter path when all you need to tune is the prompt.
- **Tag @coval to investigate.** Mention it in any tool you use, and it pulls the conversations, runs a hypothesis, and posts findings where you already work.
- **Review queues that teach.** Coval suggests the calls worth reviewing, and your disagreements with a metric become proposed rubric edits.


**Building at the frontier of voice and agents?** Come trade notes. I'm hosting a live AMA on where this is all heading and what self-healing agent infrastructure takes to build.


[Reserve your spot](https://www.coval.ai/future-of-agentic-voice)


## The principles behind it


It’s easy to build an agent that suggests irrelevant noise. It’s hard to build one that suggests exactly what you need, exactly when you need it. That’s the bar, and it’s why we’re holding ourselves to a few rules.


1. **You stay in control.** Every automated decision is inspectable, editable, and reversible. The agents move faster; you stay the arbiter of judgment.
2. **Evidence over slop.** Every recommendation and flagged regression shows its work: which conversations, which signals, what changed, and why.
3. **Built for humans and agents alike.** Every capability ships with an interface for people and a CLI + API for the coding agents your team already runs, like Claude Code and Codex.
4. **Every insight earns its place.** A dashboard tells you something is wrong. Coval’s job is to do something about it, and to help your agent get measurably better over time.


In short, we see a near future where affordable voice agents perform extremely well on quality, consistency, and compliance. The road to get there is paved with billions of simulations and trillions of live calls feeding agentic improvement loops.


It’s a future I’m excited to build with my team. (We’re[hiring](https://www.coval.ai/careers) , if you want to build it with us.)


## Ask us anything


We’re part of a broader community building at the frontier of agents and voice AI, and the best thinking here is collective. I’d love to hear what you’re working on and wrestling with. Join me for a live AMA on the agentic roadmap, the principles behind it, and what self-healing agent infrastructure actually takes to build.


July 16, 2026 at 11:00 am


[Reserve your spot](https://www.coval.ai/future-of-agentic-voice)
