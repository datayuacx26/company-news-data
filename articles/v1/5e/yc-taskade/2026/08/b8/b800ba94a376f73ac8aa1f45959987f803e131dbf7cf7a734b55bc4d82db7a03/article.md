---
schema_version: "1.0.0"
document_id: "b800ba94a376f73ac8aa1f45959987f803e131dbf7cf7a734b55bc4d82db7a03"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/ai-yield-explained"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T00:36:28.550226+00:00"
fetched_at: "2026-08-06T00:36:30.904396+00:00"
content_hash: "sha256:54901422eae986b2125133c5334ef2049a3119599552729ce943b2aad2a094d2"
---

# AI Yield: The Reliability Metric Almost Nobody Measures (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


AI Yield: The Reliability…


On this page (13)


Semiconductor manufacturing settled on its master metric decades ago. It is called **yield** : the fraction of chips on a wafer that come out working. Everything in a fab points at that one number, because you pay to process the whole wafer whether the dies are good or not, so cost per usable chip collapses the moment yield rises.


Artificial intelligence has no equivalent. We measure capability with enormous care and treat reliability as a feeling. Every leaderboard answers *what can the model do at its best?* Almost nothing answers *what fraction of what it makes is actually usable?*


The strange part is that the yield numbers already exist. They are scattered across engineering blogs, research papers, and conference talks, reported in Morris Chang's exact units by people who never use his word.


> **TL;DR:** AI yield is the share of AI attempts that become usable output, borrowed from chipmaking's master metric. Public 2026 figures range from 2.5% (AlphaProof on formalized Erdős problems) to ~80% (Spotify's judged coding agent). Serial math is brutal: 30 steps at 95% each yields 21%. Fix the process, not the vibe.[Build a checked loop in a workspace →](https://www.taskade.com/create)


---


## 🏭 What Is Yield, and Why Did One Man Build an Industry on It?


Yield is the share of manufactured units that come out usable, and it is the number the entire semiconductor business turns on. **Morris Chang** made it his life's work: hired by Texas Instruments in 1958 and handed the hardest problem in the building, he took a near-zero line to roughly 20–30% and built a reputation for understanding manufacturing at the level of physics rather than management.


Yield is driven by **defect density** and falls exponentially as die size grows. That is the entire economics of a fab in one sentence. You already paid to process the wafer; the only variable that matters is how many good dies come off it.


Chang got a second yield lesson in 1981, and it is the one that pointed him at Asia. TI's Miho fab in Japan was running memory yields of **40–50%** , roughly double its Houston plant. Sent to find out why, he found about **2% employee turnover in Japan against 10–25% in Houston** , plus technically educated shift leaders and equipment engineers on the Japanese line. His conclusion: advanced manufacturing's future was not in the United States.


In February 1987 he founded **TSMC** on a promise no chip company had made before — *we will build your chips, and we will never design our own* . He was chairman from founding (Philips supplied the first CEO, James E. Dykes); Chang held the CEO title from March 1998 to June 2005, again from June 2009 to November 2013, then stayed chairman until retiring in June 2018. Intel refused to invest, then sent Andy Grove to inspect the fab in late 1987, qualified it, and became TSMC's first American customer for legacy 1.5-micron parts.


By most estimates TSMC now manufactures around **90% of the world's leading-edge logic** (5nm and below) and roughly 70% of the whole foundry market including mature nodes. As of July 2026 it is worth about **$2.16 trillion — the sixth most valuable company on earth.**


> **Worth knowing before you cite a node name:** "3nm" and "2nm" are marketing names for process generations, not measurements. Nothing on a 3nm chip is 3nm wide; transistor pitch on N3 is roughly 48nm. The names sell; the yield number is what the business actually runs on.


An entire industry organized itself around one honest number. That is the move AI has not made yet.


---


## 📊 Why Does AI Measure Capability but Not Yield?


AI measures capability because capability is easy to score and flattering to publish. A benchmark takes a curated test set, gives the model one attempt per item, and reports a percentage. It answers *how good is this model on a good day?* Yield answers a completely different question: *of everything this system produced this week, what fraction was usable?*


The two diverge fast, because a benchmark run is a single step and real work is a chain. The[history of AI benchmarks](https://www.taskade.com/blog/ai-benchmarks-history) is a history of measuring the model in isolation, which is exactly the condition production never provides. Meanwhile[non-determinism](https://www.taskade.com/wiki/ai/non-determinism) means the same prompt can produce different output on different runs, so a single-attempt score is a sample, not a guarantee.


Three things get measured today. Only one of them is yield.


What teams measure What it answers Is it yield?


Benchmark score Peak capability on curated items No — single-step, best case


Adoption / usage Did people try it? No — that is demand, not quality


Conversion Did the user pay? No — that is commerce, not correctness


**Accepted yield** **What share of attempts shipped unedited?** **Yes**


That third row is the trap most product analytics fall into. A "success rate" that counts whether a user *started* something measures attempts, not output. A fab reporting "15% of customers began a tapeout" as its yield figure would not have customers for long.


---


## 🔢 What Are the Real AI Yield Numbers in 2026?


The numbers exist and they are wildly spread — from 2.5% to about 80% — which is itself the point. Nobody publishes them under a common name, so nobody compares them, so nobody optimizes them as a single quantity the way a fab optimizes its wafer line.


System Reported yield What one "unit" is Source type


AlphaProof Nexus (DeepMind) **9 of 353 = 2.5%** A formalized Erdős problem solved and machine-checked[Published research](https://deepmind.google/)


Overnight auto-research loop **20 of 700 = 2.9%** A genuine improvement to training code Publicly reported, March 2026


Background coding agent, before judge **~20–30%** A pull request accepted Engineering blog


Background coding agent, after judge **~80%** A pull request accepted Engineering blog


Independent-reviewer pipeline **63% of ~1,000 changes** had a mistake caught A code change reviewed by a different model Self-reported by the engineer


A few notes so these are cited honestly:


**AlphaProof's 2.5%** comes from a formalized subset: 353 of Erdős's 1,200-plus open problems, chosen entirely by what the open-source community had already formalized in Lean. The successes cluster in combinatorics, convex optimization, and number theory. The nine solved include problems open since 1970 and 1996. That is *good dies per wafer* , plus a cost-per-good-die in the range of tens to a few hundred dollars per solved problem — Chang's two favourite numbers, computed for mathematics.


**The 20-of-700 figure** comes from an overnight auto-research loop pointed at a training codebase that an expert had already tuned for months. Two days, 700 experiments, 20 genuine improvements, an 11% cut in training time, and a bug in the author's own attention implementation that he had missed. Treat it as reported rather than peer-reviewed, but note that **2.9% lands within a point of AlphaProof's 2.5%** — two completely unrelated systems, hunting in completely unrelated search spaces, landing on the same order of yield.


**The 20–30% → 80% ramp** is the most encouraging number on the list, because it is a *ramp* . Adding an[LLM judge](https://www.taskade.com/wiki/ai/llm-as-a-judge) roughly tripled the pull-request success rate of a background coding agent. Then, once models and the surrounding[agent harness](https://www.taskade.com/wiki/ai-agents/agent-harness) improved, the team **removed the judge** — and did not remove verification. They replaced a probabilistic judge with stronger deterministic verification: real CI builds and strengthened test automation. That ordering is the whole lesson, and we come back to it below.


**The 63% catch rate** is a defect-detection number rather than a pass rate, which makes it the closest thing on the list to in-line metrology. A different model reviewing the builder's work caught a mistake in roughly 63% of about 1,000 changes across 59 repositories, with stale documentation as a top defect class — the agent changed the code and did not update the README. It is self-reported, and an earlier snapshot from the same engineer reported 68% across 267 changes and 15 repos, so treat the exact figure as a range.


---


## ⛓️ Why Does a 99% Agent Fail a 30-Step Job?


Because serial yield multiplies. When every step in a run must succeed for the run to succeed, total yield is the product of the per-step rates, not the average. Thirty steps at 99% each is 0.99³⁰ ≈ **74%** . Thirty steps at 95% each is 0.95³⁰ ≈ **21%** . Per-step benchmarks never surface this, which is precisely why long-horizon[agent loops](https://www.taskade.com/wiki/ai-agents/agent-loop) disappoint teams that only measured single steps.


*X-axis values are per-step reliability in percent. Drop from 99% to 95% per step and a 30-step run goes from mostly working to mostly scrap.*


Written out, the cliff is steeper than intuition allows:


Steps in the run 99.9% per step 99% per step 95% per step 90% per step


5 99.5% 95.1% 77.4% 59.0%


10 99.0% 90.4% 59.9% 34.9%


20 98.0% 81.8% 35.8% 12.2%


**30** **97.0%** **74.0%** **21.5%** **4.2%**


50 95.1% 60.5% 7.7% 0.5%


Two consequences follow immediately, and they are the same two a fab lives by.


**First: shorten the chain.** Every step you delete is a multiplication you no longer perform. Halving a 30-step run to 15 steps at 95% takes yield from 21% to 46% with no model change at all. This is the quantitative case for narrow, well-scoped agents over sprawling ones, and it is why teams that cut tool menus often see reliability climb.


**Second: raise each step, do not inspect at the end.** Chang's doctrine in five words: *encode quality into the process.* Inspection at final test tells you a unit is scrap; it does not make the unit good. A checker bolted onto the end of a 30-step run at 95% is sorting a batch that was already 79% scrap when it arrived.


There is a happier version of the same algebra, and it is worth holding alongside the grim one. Independent checks **multiply in your favour** . A generator that is right 90% of the time, paired with a supervisor that is right 90% of the time, misses only when both miss — roughly 1% of the time, for about 99% effective reliability. That is the redundancy case. Serial yield is the production case. Real systems run both, which is why the shape of your pipeline matters more than the identity of your model. There is a fuller treatment in[how AI agents actually stay reliable](https://www.taskade.com/blog/ai-agent-reliability) .


---


## 🪜 What Actually Raises Yield: The Fix Ladder in Order


The ladder works in one order and fails in the others: **deterministic gate first, then an independent judge for what determinism cannot cover, and never the model that wrote the work.** Teams that start with an LLM judge and stop there plateau, because a probabilistic checker inherits the failure modes of the thing it is checking.


### Rung 1: A compiler cannot lie


A deterministic gate is a check written in code that returns the same verdict every time: a type checker, a linter, a build, a test suite, a schema validator, a formal proof checker. It cannot be flattered, hurried, or argued with. AlphaProof's entire 2.5% is credible *only* because Lean verified every proof — the generator was free to hallucinate as much as it liked, because the gate did not care about confidence.


For software work you already own most of this.[Spec-driven development](https://www.taskade.com/wiki/ai/spec-driven-development) is the practice of turning a fuzzy requirement into the kind of checkable rule a gate can enforce, and structured output plus schema validation does the same job for data.


### Rung 2: A judge for what code cannot check


Some output resists a hard rule. Does this UI match the blueprint? Is this summary faithful? Did the flow work end to end? That is where an LLM judge earns its keep, and the 20–30% → 80% ramp shows how much it can be worth. But hold it as a **bridge, not a destination** — as each fuzzy check hardens into a rule, graduate it down to Rung 1.


Property Deterministic gate LLM judge


What it is Code: tests, schema, compiler Another model's opinion


Verdict stability Identical every run Varies run to run


Can it be fooled? No Yes, by tone or confidence


Best for Anything expressible as a rule Open-ended, hard-to-code output


Cost profile Write once, runs free Recurring tokens per check


Role in the ladder **Destination** **Bridge**


### Rung 3: Never let the generator certify itself


The model that produced the work must never be the one that signs it off. This is final-test independence, stated for software. A self-check carries the same reasoning that caused the error, and models exhibit[sycophancy](https://www.taskade.com/wiki/ai/sycophancy) — they agree with themselves, defend committed answers, and hand out passing grades. Fresh context is what makes a second opinion an actual second opinion.


The sensitivity dial matters too, exactly as it does on a wafer probe. Published[AI code review](https://www.taskade.com/wiki/ai/ai-code-review) comparisons put one reviewer at roughly 82% recall with about 11 false positives per run, and another deliberately tuned to 6–18% recall at under 3% false positives. Neither is wrong. They are two different settings of the same instrument, and which you want depends on whether a missed defect or a wasted human minute costs you more.


### Rung 4: Turn rejects into defect classes


A rejected unit is data. The stale-documentation finding above is a defect *class* , not a one-off: agent changes code, agent does not update the README, reviewer catches it 63% of the time. Once you can name a class you can design it out of the process — a checklist item, a gate rule, a template — which is the difference between screening and yield engineering.


Two failure modes are worth calling out because they masquerade as gates:


- **The cheap proxy that passes broken units.** An HTTP 200 says a thing was manufactured. A file-size check says a screenshot exists. Neither says the artifact works — a "Preview Not Available" shell can easily weigh more than 9 KB and sail through a byte check. This is a large share of[why AI-generated apps break](https://www.taskade.com/blog/why-ai-generated-apps-break) after a green build.
- **The die that routes around the test.** A proof that technically compiles because the hard lemma was stubbed out is the software equivalent of a chip that passes probe by bypassing the defect. If your gate can be satisfied without doing the work, it will be.


---


## 🧮 How Do You Compute Your Own AI Yield This Quarter?


Define yield as a three-stage funnel and count it for one week on one repeating task. You do not need new tooling. You need three counters and the discipline to look at the third one.


```text
ATTEMPTS                    1,000   ← every time the system tried
|
v
GATE PASSES                   712   ← cleared an automated check you already own
|                                 (tests, build, schema, lint)
v                                 gate yield = 71.2%
JUDGE PASSES                  503   ← cleared an independent checker
|                                 verified yield = 50.3%
v
CLEAN ACCEPTS                 388   ← a human shipped it without editing
ACCEPTED YIELD = 38.8%   <-- the number     (illustrative figures — the point is the shape of the funnel, not these values)
```


Three rules keep the number honest:


1. **Count attempts, not sessions.** A user who retried five times produced five units, not one. Hiding retries inflates yield exactly the way counting only good wafers would.
2. **"Accepted unchanged" is the real gate.** Output a human had to fix is a reworked unit, not a good one. Track rework separately; it is your single best signal about which step is weakest.
3. **Bin by defect class, not by severity.** Severity is a judgment call. "Stale docs", "wrong schema", "silent no-op" are classes you can design out.


Once you have the funnel, the operating question stops being *which model should we use?* and becomes *which step has the lowest pass rate, and what would raise it?* That is a manufacturing question, and manufacturing has a hundred years of answers to it. It is also the question[AI agents in production](https://www.taskade.com/blog/ai-agents-in-production) turns out to be about, and the one that drives real[agent evaluation](https://www.taskade.com/wiki/ai-agents/agent-evaluation) rather than leaderboard chasing.


Yield stage Formula What a low number tells you


Gate yield gate passes ÷ attempts The generator is producing structurally broken work


Verified yield judge passes ÷ attempts It compiles but does not do what was asked


Accepted yield clean accepts ÷ attempts Taste, spec, or context is missing


Rework rate edited accepts ÷ accepts The step just before the edit is your weak link


Pair this with[agent observability](https://www.taskade.com/wiki/ai-agents/agent-observability) so you can see *where* in a run the yield is being lost, and with[evals](https://www.taskade.com/wiki/ai/evals) so a change you make is measurable rather than anecdotal.


---


## 🧬 What Does Yield Look Like Inside a Workspace?


Taskade already measures execution yield — for automations, and only there. Every automation in the listing shows a **health badge computed from recent run history** : green means leave it alone, yellow means inspect, red means it needs attention, with counts refreshed every five minutes. Every run gets a **Run Details tab with a full step-by-step execution log** , inputs and outputs visible per step.


That is a real yield instrument. It answers *what fraction of recent runs were good, and exactly which step lost it* — the two questions a fab asks all day. The[Automations Execution guide](https://www.taskade.com/learn/automation/automations-execution) walks through reading one.


To be straight about the boundary: **this is execution yield, not generation yield.** Taskade does not publish a yield figure for how many[generated AI apps](https://www.taskade.com/ai/apps) come out working, and this article is not claiming one. The honest framing is that the instrument exists and has been pointed at one layer of the stack. Pointing it at the others is the interesting work.


What the workspace does give you is the *shape* of the loop in one place, which is what makes yield measurable at all:


- **Memory** —[Projects](https://www.taskade.com/) hold the source of truth across 7 project views (List, Board, Calendar, Table, Mind Map, Gantt, Org Chart), so a rejected unit and its defect class live next to the work rather than in a spreadsheet nobody opens.
- **Intelligence** —[AI agents](https://www.taskade.com/agents) with **15+ frontier models from OpenAI, Anthropic, Google, and open-weight providers** act as the generator, and a *second* agent with a different model and a fresh brief acts as the independent judge.[Custom agents](https://www.taskade.com/learn/agents/custom-agents) let you set them up with different instructions on purpose.
- **Execution** —[Automations](https://www.taskade.com/automate) run the loop, and **100+ bidirectional integrations** (triggers pull events in, actions push data out) mean the gate can call the CI system, the schema validator, or the test suite you already trust.
- **The human rung** — a[human-in-the-loop](https://www.taskade.com/wiki/ai-agents/human-in-the-loop) approval step in front of anything expensive, so "accepted unchanged" is a recorded event and not a vibe.


That loop is[Workspace DNA](https://www.taskade.com/wiki/genesis/workspace-dna) : Memory feeds Intelligence, Intelligence triggers Execution, Execution creates Memory. Yield is simply the number that falls out of it once you agree to count.


You can build the whole loop on the Free plan and keep it there while you find out whether your yield is 20% or 80%. Paid plans start at **Pro, $10/month billed annually** ($120/year), with Business at **$25/month billed annually** ($300/year) when you want the loop running across a team. Full details on the[pricing page](https://www.taskade.com/pricing) .


---


## ❓ Frequently Asked Questions About AI Yield


Is AI yield the same thing as accuracy?


No. Accuracy is a per-item property of a model on a test set. Yield is a per-batch property of a whole process, including retries, rejects, and rework. A model with 95% accuracy inside a 30-step chain gives you a **21%** process yield, and only the second number predicts what your team experiences on a Tuesday.


Which yield stage should I optimize first?


The one with the steepest drop. If gate yield is low, the generator is producing structurally broken work and the fix is a tighter spec or fewer steps. If gate yield is high but accepted yield is low, the output compiles and still is not what anyone wanted, and the fix lives in[context engineering](https://www.taskade.com/wiki/ai/context-engineering) or the brief.


Does a bigger model raise yield?


Sometimes, and less than teams expect. A stronger model raises each per-step rate a little; shortening the chain, adding a gate, and separating the checker from the generator raise the product a lot. The 20–30% → 80% ramp came from adding a judge, not from swapping the model.


Can I use one agent as both generator and judge?


You can, and it is the most common mistake in the category. A self-check inherits the reasoning that produced the error, and models tend to ratify their own output. Use a different model, a fresh context, and a brief the generator never saw. See[agent governance](https://www.taskade.com/blog/ai-agent-governance) for how to formalize the separation.


What if my task has no deterministic gate?


Look harder before concluding it does not. Schema validity, required fields, link resolution, numeric bounds, idempotence, and "did the side effect actually happen" are all checkable in code for tasks that feel unstructured. Whatever genuinely resists a rule goes to the judge, and gets revisited every quarter.


How many steps is too many for an agent run?


There is no fixed number, only the arithmetic. Decide your acceptable end-to-end yield, measure your honest per-step rate, and solve for the length. At 95% per step, an 80% target allows about four steps. At 99%, about twenty. Anything longer needs checkpoints that let a run resume rather than restart.


Does yield thinking apply outside code?


Yes, and it is often easier there. Support replies, research briefs, data extraction, and content drafts all have a clean definition of a good unit: shipped without edits. Count attempts, count clean accepts, divide. The[agent harness explained](https://www.taskade.com/blog/agent-harness-explained) piece covers the surrounding machinery for non-code work.


Why do two teams using the same model report such different yields?


Because they are not really using the same system. Chain length, tool count, context quality, gate strength, and checker independence differ, and each of those multiplies. This is the same reason two products on an identical model feel like different products — the agent harness is doing most of the work.


What is the fastest yield win for most teams?


Delete steps. It requires no new tooling, no model change, and no budget. Every step removed is one fewer multiplication, and most agent workflows carry two or three steps that exist because someone was being thorough rather than because the output needs them.


Where should the human sit in the loop?


At the accept gate, and at anything irreversible. Humans are expensive checkers and excellent arbiters of taste, so spend them on "is this actually what we wanted" and let code handle "does this compile". The[error-fixing guide](https://www.taskade.com/learn/genesis/error-fixing) is a good example of the split: the system proposes a fix, and a person decides whether to apply it.


---


## 🎯 The Metric Is Sitting Right There


Morris Chang did not invent yield. He made it the number everyone reported, compared, and organized around — and an industry with a shared honest metric compounds in a way an industry with a shared vibe does not.


AI has the numbers already. **2.5%** on formalized Erdős problems. **2.9%** on overnight research experiments. **20–30% climbing to 80%** on judged pull requests. **63%** of changes with a mistake caught by an independent reviewer. Nobody calls any of it yield, so nobody compares it, so nobody optimizes it as one quantity.


Start with one task, three counters, and a week. Put a deterministic gate under it, an independent judge beside it, and a human at the accept line. Then look at the number you have been guessing at all year.


▲ Memory holds the defect classes. ■ Intelligence generates and independently checks. ● Execution runs the loop and logs every step.[Start building the loop free →](https://www.taskade.com/create) — or browse working apps other people shipped in the[Community Gallery](https://www.taskade.com/community) and see what a good unit looks like before you define your own.
