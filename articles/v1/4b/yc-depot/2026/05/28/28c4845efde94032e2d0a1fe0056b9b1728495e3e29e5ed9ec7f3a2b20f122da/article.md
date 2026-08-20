---
schema_version: "1.0.0"
document_id: "28c4845efde94032e2d0a1fe0056b9b1728495e3e29e5ed9ec7f3a2b20f122da"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/context-isolation-in-coding-agent-loops"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:6bdfba1e2018c5cc4bdf263b79880ec9015a178302271d52fce15fe09537c489"
---

# Context isolation in coding agent loops

For a while, the way I used agents was basically this:


Paste in a Linear ticket. Add some repo context. Tell the model to be careful. Hope the diff didn't come back haunted.


Sometimes it worked. Sometimes it did the software equivalent of cleaning my garage by moving everything into the kitchen.


The annoying part was that the agents were useful. If they had been useless, I could have ignored them and gone back to typing everything myself like a respectable little code monkey.


But they were useful enough that my workflow around them started to matter.


## Context rot, basically


The first thing I noticed was that the longer I worked on a problem, the dumber the agent felt.


Not all at once. More like a subtle pile-of-poo feeling that built over time.


The agent would forget things we had talked about. It would stop applying rules I had set for the repo. It would ignore something I had explicitly told it three turns ago. The solutions got worse in this oddly familiar way, like watching someone get tired but still insist they were totally fine to drive.


The thread had context rot.


Ticket text, repo scans, logs, test output, half a plan, stale assumptions, review comments, more logs, some failed idea from 40 minutes ago, and then a cheerful little summary pretending this was all still coherent.


By the end, the context window was a junk drawer.


The agent still had all the words, technically. But the structure was gone. I could not tell what was load-bearing anymore. The model could not either.


The agent may have seemed dumber but it was *my* fault!


## Solving my AI problem with more AI


When my tool is an agent, every problem starts to look like a vibe.


So I built` /orc` .


` /orc` started from a pretty simple thought: writing code is only one hat an engineer wears.


Agents should not wear all the hats at once. They should take on one role at a time.


Before I write code, I make a plan. Then, because I am a feeble and imperfect human, that plan usually sucks. So I talk it through with other engineers.


They tell me where I'm being ambiguous. They point out edge cases. They ask why I'm doing the weird thing instead of the boring thing. They notice when I've quietly turned one feature into three features wearing a trench coat.


Usually, the better the plan gets, the better I execute.


Then I build the damn thing.


Then I push the sucker up for review.


Review has its own cast of characters:


- There is the "perfect is the enemy of the good" reviewer. The best user feature is a shipped feature. Let's go!
- There is the reviewer who can find the edge case hiding inside another edge case.
- There is the person less “LEEROY JENKINS” than I am, who prefers strictness and caution over velocity.
- And there is the excellent human who wants to leave the code better than we found it.


All of those people are useful. All of them are also annoying in their own special way. That is engineering.


At some point I realized I was asking one agent to do all of these jobs at once: plan the feature, challenge the plan, build the feature, review the feature, and decide whether the feature was good.


That is a weird job for a human. It is an even weirder job for a model sitting inside one increasingly polluted context window.


## The loop


So` /orc` became a loop:


Linear ticket ->[Planner](https://gist.github.com/121watts/9b433d3e64c64cf6104f0c7f1775f376#file-planner-md) ->[Clarifier](https://gist.github.com/121watts/9b433d3e64c64cf6104f0c7f1775f376#file-clarifier-md) ->[Human Gate](https://gist.github.com/121watts/9b433d3e64c64cf6104f0c7f1775f376#file-human-gate-md) ->[Builder](https://gist.github.com/121watts/9b433d3e64c64cf6104f0c7f1775f376#file-builder-md) ->[Reviewers](https://gist.github.com/121watts/9b433d3e64c64cf6104f0c7f1775f376#file-reviewers-md) ->[Reviewer Boss](https://gist.github.com/121watts/9b433d3e64c64cf6104f0c7f1775f376#file-reviewer-boss-md) .


I put a generic version of those role prompts[here](https://gist.github.com/121watts/9b433d3e64c64cf6104f0c7f1775f376) .


Each stage runs in its own subagent.


The main thread stays small because the main thread is not where all the work happens. I steer. The main agent delegates. The subagents do the messy work. The only things that come back are structured handoffs: a Build Packet, a pass/fail clarification, a builder handoff, reviewer verdicts.


No giant soup thread. No "here is every grep result I saw along the way." No context window slowly filling with sediment.


The planner gets the ticket and repo context, then produces a Build Packet. A tiny excerpt from a real one looked like this:


```text
# Local Action Manifest Build Packet


## Summary


`ci run --workflow`   sends fresh workflow YAML, but composite local actions
are expanded earlier from the server-side checkout, so local edits to
`.ci/actions/*/action.yml`   can be missed.


The narrow fix is to send manifest text directly and use it only for
inline compile.


## PR List


1.   `feat: accept local action manifests for inline compile`
2.   `feat: send local action manifests from ci run --workflow`
```


Boring on purpose.


The builder does not need the planner's whole wandering thought process. It needs the contract.


The planner cannot write code.


Then the clarifier gets the packet and tries to kill ambiguity before it turns into a diff. Are the acceptance criteria testable? Is there a red test? Are the commands real? Is this actually one PR? What assumptions are we about to accidentally bet the afternoon on?


If the plan is fake, the clarifier fails it.


Then` /orc` stops and asks me before building.


That pause is more important than it sounds. It is where I make myself not one-shot the thing. If the plan is mush, the builder will still build it. That is the problem. It will just build the mush very confidently.


After that, the builder gets the approved packet.


The builder's job is boring on purpose: write the failing test, prove it fails, make it pass, refactor only inside the touched area, run the real commands, then hand the result to review.


The biggest win, for me, is still context isolation. The planner does not need the builder's test output. The reviewers do not need the planner's repo spelunking transcript. I do not need every intermediate thought. I need the packet, the decision, the evidence, and the next move.


## My favorite part is still the review


It started because I was tired of push-wait-feedback.


Push the PR. Wait 10 or 15 minutes for automated reviewers to come back. Fix the thing. Push again. Wait again. Get a human comment that makes me realize the robot was not the embarrassing part.


So I wondered: what if I had something review the PR before I pushed?


The review sub-agents run independently, outside the main context window. They do not know the messy history of how the code got there. They do not share all the false starts and stale assumptions.


Fresh eyes, basically.


Annoying fresh eyes, which are the useful kind.


One real` /orc` run came back with` BLOCK` : local tests passed, but the claimed behavior was not actually exercised and one runtime path was still wrong.


I would rather learn that before I push.


The builder then added the missing behavior test, fixed the implementation, and ran review again. The second verdict was` SHIP WITH NITS` .


I do not have perfect data here. I am not going to pretend I ran a double-blind peer-reviewed study in my little engineer cave.


But it definitely made me feel less embarrassed before pushing, and it felt like there was less back and forth with review bots (and humans) afterward.


It also made me better at reviewing other people's PRs, because I could send in the little council before I opened my human mouth on GitHub.


## ` /orc` can be annoying


The review loop is also where` /orc` can get annoying.


For a typo, this is stupid. Please do not summon the full council because you misspelled` registry` .


For giant projects, it is not enough by itself. Arbitrary-size feature work needs more structure than this loop provides.


The sweet spot is somewhere around small, medium, and large. Not extra small. Not extra large.


The agents can also be pedantic. A clarifier can block on something obvious. A reviewer can mistake "different than I would do it" for "wrong." A boss can merge four mediocre complaints into one very official mediocre complaint. But these are also things I experience with actual humans. The difference is I can tell a subagent its feedback sucks.


And, I would rather have the annoyance up front than buried in a PR review cycle.


## Bias supported by research after the fact


I wish I could say I designed all of this from first principles because I am very wise.


Like any true engineer I built the loop first, found it pretty darn useful, and then found research to support my bias.


Anthropic has a good writeup on[how they built their multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) . Their setup uses a lead agent coordinating specialized subagents, each working in its own context window and returning compressed findings. They found that the multi-agent version beat a single-agent version by 90.2% on an internal research eval.


Research is not coding. Anthropic says that too. Research tasks split into independent directions more naturally than most coding tasks. Multi-agent systems also burn through tokens fast. Sometimes they are absolutely the wrong tool.


Cool. Bias supported, with footnotes and caveats. Moving on.


## Borrow the loop, steal the principle


If you want to steal the` /orc` loop, go ahead. But I'd rather you steal the principle that built it: break the work into roles, give each role its own context, and make the handoffs explicit.


That applies outside` /orc` , too. When I catch myself doing some annoying manual task now, I ask:


*Which parts require my judgment?*


*Which parts require fresh eyes?*


*Which parts are mostly structured checking?*


*Which parts would benefit from a separate context window?*


*Which parts could an agent do better than me, or at least help me do better?*


Then I try it.


Sometimes it works. Sometimes it produces a beautiful new form of nonsense. Still pretty fun.


` /orc` is just a loop.


The principle is the thing.


## FAQ


What causes a coding agent to feel dumber the longer the conversation goes?


Context rot. Ticket text, repo scans, logs, test output, half a plan, stale assumptions, review comments, more logs, and some failed idea from 40 minutes ago all pile up in one thread until the model can't tell what's load-bearing anymore. The agent still has all the words. The structure is gone. The fix isn't a smarter model, it's a less polluted context.


Why split coding work across multiple subagents instead of using one agent?


Writing code is one hat an engineer wears. Planning, challenging the plan, building, and reviewing are different jobs with different goals, and asking one agent to do all of them inside one growing context window is how you end up with very confident mush. Putting each role in its own subagent keeps the contexts clean and forces the handoffs to be explicit: a Build Packet, a pass/fail clarification, a reviewer verdict.


When is a multi-agent loop overkill for a coding task?


Minor changes, small docs updates, and so on would be overkill. For large feature work, a fixed loop also won't be enough on its own, you'll need more structure around it. The sweet spot is small, medium, and large tasks. Not extra small, not extra large.


How do you keep subagent reviewers from being pedantic or just wrong?


Same way you handle it with humans, you push back. The advantage with subagents is you can tell one its feedback sucks without burning a relationship. A clarifier might block on something obvious, a reviewer might confuse "different than I would do it" with "wrong," a boss might merge four mediocre complaints into one official mediocre complaint. I'd still rather get that annoyance up front than buried in a PR review cycle.


## Related posts


- [Using AI as my engineering copilot (not autopilot)](https://depot.dev/blog/using-ai-as-my-engineering-copilot-not-autopilot)
- [The end of push-wait-guess CI](https://depot.dev/blog/the-end-of-push-wait-guess-ci)
- [What we need from CI for agentic engineering](https://depot.dev/blog/ci-for-agentic-engineering)


Andrew "Watts" Watkins


Engineering Manager at Depot
