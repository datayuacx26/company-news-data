---
schema_version: "1.0.0"
document_id: "504d5a383d4b484b80e3f49ba455e44a5a581ae0806091ccd9161061ff155476"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/a-council-a-sword-and-a-fleet-of-agents-how-i-ship-code-now"
published_at: "2026-04-22T15:37:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:1be5850d42a5d13f32323f16a3845508c4e9ee4565feb5e6a3402c58f3527247"
---

# A council, a sword, and a fleet of agents: how I ship code now

I'm a senior software engineer, but roughly 95% of the code I ship now is written by AI and reviewed by dozens of agents.


Instead of writing code, lot of the work I do is providing context to the agents so I can, hopefully, come back to them having completed a bunch of tickets.


And then, I review that the code the agents produced is good and it does what it is supposed to do efficiently (although other agents perform all kind of tests, I still do manual checks).


I’ve been in this ‘journey’ for over a year.


And every week feels like an experiment. I keep improving my workflows. And along the way, I’ve rediscovered Git worktrees to work in several tickets in parallel, and that code is so cheap, that I can build custom tools just for myself if it makes my life easier.


In this blog post, I want to share how I ‘write code’ with AI, hoping more people find it useful and can share some tips on what they’re doing.


## I spend most of my time planning


When I say it’s always an experiment, I’m not only referring to the agents. We, devs, are also new to this. So I think we have to find a way to work that feels right for our human brain.


Personally, I like planning. I like having this thing I can review and work on separately. And for the agents, having discreet scopes can avoid polluting the implementation workflow context. Investing in planning pays off for both parts.


Good planning can take time, though. You first need to gather linear tickets, the Figma designs for UI you need to build, sketch how you want the feature to work, decide which libraries should be used.


So… I built an AI workflow that helps me plan. Behold, the Council of Elrond (shoutout to the LOTR fans out there).


## The Council of Elrond helps me plan


This AI workflow runs in five steps: classify, interview, knowledge capture, synthesize, publish.


Classify figures out the scope, it usually guesses it from my original prompt. Is this a project, like the[collaborative retrospective editor](https://rootly.com/blog/how-we-used-crdts-to-build-real-time-collaborative-retrospectives) ? A feature inside it? Or just a bug fix that needs a Linear ticket fleshed out? Everything after scales accordingly.


Interview is where it gets a bit weird (I feel like I’m talking to myself). The agent interviews me based on parameters I set before. I sketch what I want, roughly: here's what I'm using, here's a pattern I want to follow, here's how I want it to behave.


Then the workflow asks me what I'm missing. What about this edge case? What about that user flow? Sometimes I go "oh, great point, I didn't think of that." Sometimes it suggests something back and I go "actually, that way is better." We go back and forth until the plan seems good enough.


Knowledge capture pulls in whatever's useful from past decisions, patterns we've already agreed on.


Synthesize turns the whole thing into a clean doc.


Publish drops it into Linear, Notion, or a markdown file, depending on where it needs to live.


And that's it. I come out the other side with a fleshed-out plan I can hand to another engineer, or pass straight to my implementation workflow.


With a solid plan, the implementation agents can run without stopping to ask me questions halfway through.


## Agents implement tickets in up to 10 steps


Implementation is when the real fun begins. My implementation agents decide, based on the plan provided, which steps are needed. They can perform more research, refine the plan, split into more specialized agents for implementation.


I have 40+ specialized agents that can do AWS, CI/CD, Cloudflare, e2e tests, and other tasks.


After implementation, other agents review the work done by the implementation agents:


- Playwright UI testing agent swarms
- Agents that connect to the Figma MCP to compare the final output with the intended design
- Agents that compare the code quality with Rootly’s engineering standards.


Most of the implementation agents are Claude-based. But a lot of the reviewing agents are Codex-based, to introduce more entropy and improve the likeliness to catching issues.


If the reviewers catch an issue, they attempt to re-prompt the implementation agents. Worst case scenario, they let me know and I have to review the original plan and tune the workflow.


If it’s all good, the agents open a PR which I can review before passing it on to another human reviewer on the team. Of course, there’s another org-wide PR reviewing agent too.


## Behold, my personal command center


You may have noticed the screenshots above don’t really seem familiar to popular tools. That’s because I created this implementation workflow and UI for it for myself and my very specific way of working.


Nobody at Rootly uses it. I built this for myself. It has exactly every label and button and color exactly where I want them.


From the same guy who brought you The Council of Elrond (me lol), behold, Andúril (yes, more LOTR).


### Andúril implements in parallel


One Andúril run takes 30 - 60 minutes. That sounds slow until you remember I have five or six of them going at once.


Each ticket gets its own git worktree with isolated infra: its own server, database, ports, the lot. If one agent is working on a UI ticket, I can kick off another on a totally different ticket and the two never touch each other.


Spinning one up is a single command:


` rootly-worktree-up \[branch-name\] \[--light\]`


It checks out a fresh worktree, starts an isolated local infra on its own ports, clones from my main dev DB so all my test data comes along, and opens Claude Code inside.` --light` skips the Sidekiq workers, which I rarely need for UI work.


[Git worktrees](https://git-scm.com/docs/git-worktree) have been in git for over a decade, and very few people used them. As a human engineer, you only ever really to work on one branch at a time, switch, commit, push. Now that five agents are running at once, worktrees suddenly matter a lot.


The dashboard shows the whole fleet: what's running, what's off, what's waiting for me. I can walk away, come back, and know exactly where every session is at.


## The things we keep coming back to


Most engineers at Rootly have their own versions of this. So we talk about it a lot. These are things I keep leaning on.


**Preserve context.** Every design choice I make comes back to this. The implementation workflow shouldn't be wading through planning context it doesn't need. The reviewer shouldn't be reading the executor's reasoning. A session that's seen too much isn't a cleaner session, it's a worse one. When in doubt, keep the context windows small and the handoffs explicit.


**Adversarial agents, not agreeable ones.** The reviewers I actually trust are blank, zero-context Claude and Codex agents that have never seen the planning or the execution, just the diff and the ticket. Different model families, different training, different failure modes. You want them to disagree with each other and with the main session. More entropy in the review is the point, not a side effect.


**Orchestrate workflows, don't write one.** Andúril does implementation. Council of Elrond does planning. Twenty-step mega-workflows sound tidy on paper and suck in practice. Small composable building blocks that hand off to each other is how you end up with something you can actually reason about, and how you end up building new ones without ripping the old ones apart.


**Git worktrees are back.** Nobody really used them for a decade because no human works on five branches at once. Agents do. One command and I've got a fresh branch on a new worktree, a fresh app on its own infra, a fresh database cloned from my development DB, and Claude Code open in the new directory. Cheap, disposable, isolated.


**Stop optimizing for single-workflow speed.** I don't really care how long one workflow takes. If it grinds for 30 minutes, fine, I've got five or six running at the same time. Start it, walk away, come back, validate. Optimize the fleet for throughput, not any individual session for latency.


**Code is so cheap, build your own tools.** A year ago you bought developer tools. Now you build them for your company. Now I'm building them for the project I'm on this month. The Andúril dashboard is heavily skewed toward UI tickets because that's what I'm shipping right now. When I switch to a higher percentage of backend work, I'll make a new one. The bar for "worth building yourself" has dropped through the floor.


**Close the loop.** Every ticket feeds back into the knowledge base so the next one is a little easier. Without that, you're running the same workflow against a cold start every time. With it, the system gets a little smarter each ticket. It's the part people skip and the part that compounds.


## Every week feels like an experiment


I don't think I've nailed this. Every week I tweak something: add a new agent, rewrite a workflow, rip out a step that wasn’t really doing much. The setup I described here will probably look different by the time you're reading this.


What I miss is flow state. A couple of years ago, a gnarly bug would absorb me for an hour. Dial in, hit Stack Overflow, iterate until it clicked. Now my day is planning, waiting, reviewing, and context-switching between sessions. More chess, fewer puzzles. Some days I miss the puzzles.


But I'm shipping more work, with more reviewers, better tested, in less elapsed time.


The north star, for me: start the workflow, walk away, come back, validate. Don't really care what happened in the middle. Most of the time, I don't.


If you're building your own version of this, I'd love to hear what you've figured out. Especially the stuff that isn't working, that's where the interesting experiments are.


‍
