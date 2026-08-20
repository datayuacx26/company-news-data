---
schema_version: "1.0.0"
document_id: "7e587afffdc6ed7b5d7806efb39680cff7d081e64322c1e177c0b9cb1069dba0"
company_key: "yc-continue"
company: "Continue"
source_id: "yc-continue-rss-8ad3fe8c275c"
canonical_url: "https://blog.continue.dev/283-net-lines-and-a-passport"
published_at: "2026-02-25T04:00:00+00:00"
first_seen_at: "2026-07-24T23:23:23.996128+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:98d0fb604b74aa9f4ef671e581a7e811394c071f464df3dcf027321a513a7fd5"
---

# 283 Net Lines and a Passport

Yesterday I shipped 29 PRs, renewed my passport, and had two conversations that landed on opposite ends of the same question.


## 30 Tickets


The day started with a list. 30 tickets in a Linear project called "The Last Palooza," the kind of accumulated product debt that lives in the gap between what I built and what I meant to build. Sidebar jank. Confusing buttons. Pages that take two seconds to load when they should feel instant. None of it urgent. All of it the difference between software that works and software that feels right.


The approach: 8 waves of 4 parallel Claude Code sessions, each in its own git worktree, each planning before building. A meta-plan to orchestrate each plan and each implementation. A ship script that emerged mid-session because the mechanical overhead of linking PRs to Linear tickets to[Threader](https://threader.sh/) sessions was eating time. By the end: **+3,917 lines added, -3,634 deleted, +283 net.** I reshaped the product more than I grew it.


One ticket got canceled at the planning stage. "There's still a lot of stuff on the agent page" was too vague to act on. That's the plan-first workflow doing its job. Better to catch ambiguity before writing code than after.


The rhythm settled quickly. Four tabs is the sweet spot: enough to keep momentum, few enough to stay aware of what each is doing. I'd approve a plan in one tab, flip another to YOLO, check back on a third that was running through checks. The moments that mattered were the judgment calls. Canceling the vague ticket. Noticing when a plan didn't account for a dependency. Knowing which tickets could run in parallel and which needed to wait. The machines handled the rest.


Between waves, I renewed my passport. My turn to meet the requirements.


## The Same Question


Two conversations bookended the work.


**The first was with a team that's scaling.** Big round, lots of SDKs. Documentation that started as optional and gradually became load-bearing. The question underneath every other question: *how do we enforce quality when everything moves this fast?*


**The second was with someone building at full speed without a safety net.** Agents running in parallel across VMs, writing their own skills, self-healing through traces of previous runs. YOLO mode, always. But the agents are overeager: one had pushed a PR on its own, decided to use the` gh` CLI without being told to. He wants the speed. He also wants guarantees. The problem is every task in software is a one-off, and the ground is shifting under everyone.


Two different scales, same problem. One team needs their existing knowledge enforced consistently. The other needs guardrails that don't exist yet.


Every PR I shipped today ran through 27 checks: code conventions, security, mobile layout, accessibility, database migrations. Human taste encoded as markdown files in the repo, running as AI agents on every PR, passing silently or failing with a fix attached. I set the standards. The checks enforce them. I decide what ships. The whole session is visible in Threader: every plan, every judgment call, every YOLO moment.


Chad wrote about this pattern earlier this week in the context of[Vercel's 57 React performance rules](https://blog.continue.dev/standards-as-code-vercel-react-rules/) . Most teams will drop rules like those in a wiki or paste them into an agent prompt. Both fail for the same reason: they depend on humans remembering. Standards-as-checks is the alternative.


## Meet the Requirements


That's what yesterday was. 29 PRs was not 29 acts of automation. It was me setting taste (canceling the vague ticket, approving plans, knowing when to let it run and when to hold back) while machines handled enforcement and execution. The scaling team is at the stage where they're writing the rules down. The fast team is at the stage where agents write the rules themselves. I'm in the middle, building the bridge: human knowledge encoded as standards, running on every PR, decided by humans.


But a day like this also produces a map. Every intervention (canceling a ticket, catching a missing dependency, rejecting a plan) is signal. Every non-intervention (YOLO mode running clean through 27 checks) is signal too. As Chad wrote last summer,[intervention rates are the new build times](https://blog.continue.dev/intervention-rates-are-the-new-build-times) . CI/CD measured how fast you ship. The next thing to measure is how rarely you override.


The checks are why 29 PRs shipped in a day without cutting corners. The next step is evolving the process so people are only at the points that require judgment. Not less human involvement, more precise human involvement.


The passport renewal wasn't so different. You fill out the form, attach the photo, provide the documents. If anything is wrong, it gets kicked back. Nobody at the passport office reads your life story. They check that you met the requirements. Standards-as-checks works the same way.


Tickets shipped 29


Tickets canceled 1


Lines added 3,917


Lines deleted 3,634


Net lines +283


PRs opened 29


Waves 8


Max concurrent agents 4


Checks run per PR 27


Tools built during session 1


Midjourney prompts 1


Passports renewed 1


Conversations with users 2


To set up checks on your own repo, paste this into your favorite coding agent:


```text
Create AI checks for my codebase: https://continue.dev/walkthrough


```
