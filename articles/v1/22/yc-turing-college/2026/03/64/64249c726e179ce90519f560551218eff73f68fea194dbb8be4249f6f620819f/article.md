---
schema_version: "1.0.0"
document_id: "64249c726e179ce90519f560551218eff73f68fea194dbb8be4249f6f620819f"
company_key: "yc-turing-college"
company: "Turing College"
source_id: "yc-turing-college-news-import-c9a4b7bb10b2"
canonical_url: "https://www.turingcollege.com/blog/agentic-engineering-vs-vibe-coding"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:08.178356+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:d28842e4375139463b402a7e2547b43fbe4128b37ca755b2fb6f806f095f1ce7"
---

# Agentic Engineering vs. Vibe Coding

# Agentic Engineering vs. Vibe Coding


On February 4, 2026,[Andrej Karpathy posted a thread on X](https://x.com/karpathy/status/2019137879310836075) that reshuffled the vocabulary of an entire industry. One year earlier, he had tossed off a "shower thoughts throwaway tweet" — his words — coining "vibe coding." That phrase spread through every Slack channel, conference keynote, and VC pitch deck on earth. Karpathy himself admitted he still can't predict his tweet engagement "basically at all" after 17 years on the platform.


The anniversary thread carried weight. Karpathy wrote: "Many people have tried to come up with a better name for this to differentiate it from vibe coding, personally, my current favorite is **agentic engineering** ." He broke the term into two halves. "'Agentic' because the new default is that you are not writing the code directly 99% of the time, you are orchestrating agents who do and acting as oversight. 'Engineering' to emphasize that there is an art & science and expertise to it."


Those twelve words did what a year of community debate could not: they drew a clean line between the weekend hacker and the production-capable builder. But what does that line look like in practice? And why did we need it in the first place?


## What Is Agentic Engineering?


**Agentic engineering is a software development discipline where AI agents autonomously plan, write, test, and iterate on code with minimal human intervention. The human defines goals, constraints, and quality standards — then steps back. The agents handle implementation. The systematic approach reduces critical errors to a minimum, even when the human hasn't touched a single line of code.**


The word "agentic" means *agent-driven* . You're not pasting code from a chatbot into your editor. You're spinning up coding agents —[Claude Code](https://claude.com/product/claude-code) , Cursor, Windsurf — that can read your codebase, execute shell commands, run your test suite, and loop on failures until the tests pass. You scope the task. They build. You review the output — not every keystroke.


[Addy Osmani](https://addyosmani.com/blog/agentic-engineering/) , a software engineer at Google, captured the core tension in a February 2026 blog post. He noted that "vibe coding" had become a suitcase term — people used it to describe everything from a weekend hack to a disciplined agent-driven workflow. These are fundamentally different activities.[Simon Willison](https://simonwillison.net/guides/agentic-engineering-patterns/what-is-agentic-engineering/) proposed "vibe engineering" as a middle ground, but the word "vibe" carried too much baggage. "When you tell a CTO you're 'vibe engineering' their payment system," Osmani wrote, "you can see the concern on their face."


That concern leads to the obvious question: what went wrong with vibe coding?


## Why Did Vibe Coding Hit a Wall?


**Vibe coding fails when code moves from prototype to production. The technique skips design, skips review, and skips testing — which works for demos but collapses under the weight of real users, real security requirements, and real scale. The pattern repeats: it demos great, then reality arrives.**


Throughout 2025, the cycle played out on a loop. A developer would prompt an LLM, accept the output without reading it, paste error messages back in when something broke, and keep going. The demo looked great. The production deployment did not.


On the[Hacker News thread](https://news.ycombinator.com/item?id=47393908) discussing Simon Willison's agentic engineering guide — posted in March 2026 — one commenter put it bluntly: "After three months of seeing what agentic engineering produces first-hand, I think there's going to be a pretty big correction. Not saying that AI doesn't have a place... but there is a seriously delusional state in this industry right now."


Another pointed to a specific casualty. Amazon ordered a 90-day reset on its code deployment controls after a string of incidents in Q3 2025, at least one tied to Amazon's AI coding assistant Q. Dave Treadwell, Amazon's SVP of e-commerce services, described "high blast radius changes" in an internal document — software updates that propagated broadly because control planes lacked safeguards.


Vibe coding didn't cause the problem. Skipping the design thinking did.


Osmani catalogued where vibe coding still works — and the list is shorter than the hype suggests:


- **Greenfield prototypes and hackathon demos.** You need something running by Sunday. Quality is noise.
- **Personal scripts.** If it breaks, you regenerate it.
- **Learning and exploration.** Newcomers build things they couldn't otherwise.
- **Creative brainstorming.** Over-generate on purpose, throw everything away, build properly.


Beyond those four scenarios, the technique collapses. You try to modify the code, scale it, or secure it, and nobody — including the person who prompted it — understands what the code is doing. That gap between "works in a demo" and "works for real users" is precisely where agentic engineering steps in — as its own category, not as polished vibe coding.


## Vibe Coding vs. Agentic Engineering vs. AI Engineering: What's the Difference?


**Vibe coding lets AI write code with no human review. Agentic engineering gives agents more autonomy — they plan, execute, and iterate with minimal human intervention. AI engineering keeps the human close to the code at all times, designing multi-agent systems, reviewing agent decisions, and intervening at the implementation level when agents drift.**


The critical column is the middle one. Agentic engineering borrows the speed of vibe coding while adding enough structure to keep projects from derailing — without demanding the deep code-level involvement of AI engineering.


One commenter in the[HN discussion](https://news.ycombinator.com/item?id=47393908) captured the real unlock: "The real unlock with agents isn't single-agent capability — it's running multiple agents on independent tasks in parallel. One agent refactoring module A while another writes tests for module B. The constraint is making sure tasks are truly independent, which forces you to think about architecture more carefully upfront."


That last sentence is the whole game. And it explains why the benefits distribute differently depending on where you sit on the spectrum.


## The Skill Shift Across the Spectrum


**AI engineering disproportionately benefits experienced engineers who already understand system design, security patterns, and performance tradeoffs. They can intervene in agent decisions at the code level because they know what good code looks like. Agentic engineering, by contrast, opens a wider door — advanced non-engineers and engineers can both produce production-grade output by mastering task scoping, agent configuration, and systematic review.**


Osmani flagged an important nuance in[his February post](https://addyosmani.com/blog/agentic-engineering/) . He noted a "dangerous skill atrophy" risk: developers who lean on AI before building fundamentals can produce code without understanding it, ship features without learning why the patterns exist. Several engineering leaders have called this an emerging crisis — a generation of developers who can prompt but can't debug.


The Hacker News thread surfaced a subtler tension. One commenter wrote: "There is a qualitative difference between an engineer producing the code themselves and an engineer managing code generated by an LLM... Humans have to be accountable for that code in a lot of ways because ultimately accountability is something AI agents generally lack."


Another sharpened the point: "The main difference between layers of abstraction and agentic development is the 'fuzziness' of it. It's not deterministic. It's a lot more like managing a person."


That analogy lands. Agentic engineering demands management skills — the ability to scope tasks clearly, evaluate outputs critically, and spot when an agent has drifted. AI engineering demands those same skills *plus* the ability to read and rewrite the code itself.


Simon Willison himself weighed in on that thread: "It has been fascinating to watch how so many of the techniques associated with high-quality software engineering — automated tests and linting and clear documentation and CI and CD and cleanly factored code — turn out to help coding agents produce better results as well."


The takeaway: good engineering practices compound at every tier. But each tier rewards a different mix of skills — and the entry point for agentic engineering is wider than the industry has acknowledged.


## Where Each Approach Belongs


**Agentic engineering deserves its own category — separate from vibe coding, separate from AI engineering. The output is good enough for real users. The code quality, while imperfect, meets a bar that systematic agent workflows enforce. Critical errors drop to a minimum even when the human hasn't written a single line. This became production-viable in late 2025, when models like**[Anthropic's Opus](https://www.anthropic.com/claude/opus) **and OpenAI's Codex crossed the threshold for reliable agent-driven development.**


The mistake most people make is treating agentic engineering as a dressed-up version of vibe coding. Vibe coding produces throwaway artifacts — scripts, demos, proofs of concept that fall apart under real usage. Agentic engineering produces applications that your team and your users can rely on.


Here's what changed. Since November 2025, the model layer caught up with the workflow. Anthropic's Claude Opus 4.6 shipped with adaptive thinking, a 1-million-token context window, and agent team coordination. OpenAI's Codex models reached a level of code generation that loops reliably through test-fail-fix cycles. The agents stopped producing "almost right" code. They started producing "good enough to ship" code — and with features like[Anthropic's built-in code review](https://claude.com/solutions/agents) , the gap between "agent-generated" and "production-ready" shrank to a rounding error for a wide range of applications.


That's the unlock. A product manager who understands their domain can now spin up a[Claude Code](https://claude.com/product/claude-code) session, scope a task with clear constraints, let the agent build it, run it through code review, and ship an internal tool to their team. A founder without a technical co-founder can build and deploy a customer-facing mini-app. An engineer can run three agents in parallel across independent modules and ship in a day what used to take a week.


Are these outputs flawless? No. Will a senior AI engineer reviewing the same code find optimizations, edge cases, and architectural improvements? Every time. But "flawless" was never the bar. The bar is: does it work reliably for the people using it? Does it handle the common failure modes? Is the error surface small enough to manage?


For a growing category of applications — internal dashboards, workflow automations, data processing pipelines, customer-facing tools with bounded complexity — the answer is yes. Agentic engineering gets you there.


**Use vibe coding** when the code is disposable. A weekend hack. A one-off script. A brainstorm you'll delete on Monday.


**Use agentic engineering** when the output needs to work for real people — your team, your users, your customers — and you need it fast, with a systematic process that keeps quality above the production threshold. You don't need to be a senior engineer. You need to be a clear thinker who can scope tasks and evaluate results.


**Use AI engineering** when the system demands deep human oversight at the code level — mission-critical infrastructure, complex multi-agent architectures, applications where a silent mistake costs more than the time it takes to read every diff.


The lines between these three will blur as models improve. But right now, agentic engineering is the category with the most untapped surface area — and the one where the gap between "what's possible" and "what most teams are doing" is widest.
