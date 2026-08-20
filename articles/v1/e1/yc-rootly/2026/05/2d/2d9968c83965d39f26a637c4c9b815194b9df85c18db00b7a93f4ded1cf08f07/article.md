---
schema_version: "1.0.0"
document_id: "2d9968c83965d39f26a637c4c9b815194b9df85c18db00b7a93f4ded1cf08f07"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/what-broke-when-engineering-went-fully-agent-based"
published_at: "2026-05-13T13:28:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:f67ad6759f0df0f2614d8daad924a68ab1447a4aea4a2706433d5e08f41a2894"
---

# What broke when engineering went fully agent-based

Last year, we went fully agent-based at Rootly. Cursor, Claude Code, Codex, all of it. The productivity gains were real. Something broke that I didn't expect though.


Each engineer wasn't running one agent. They were running several. Iain[ships code with a workflow he calls Anduril](https://rootly.com/blog/ai-changed-the-qa-job-the-skepticism-stayed) that combines Claude and Codex with adversarial review and automated UI testing. He runs five or more of these in parallel across different worktrees, with a local dashboard to keep track of them.


He's not unusual. Most of our engineers have built out their own version of this. John has one called Feta. Kate has[Playwright agent swarms](https://rootly.com/blog/ai-changed-the-qa-job-the-skepticism-stayed) . The patterns get shared in Slack threads and borrowed across the team through shared tooling.


This is great, and it's also where the fragmentation comes from. Every engineer has their own CLAUDE.md. Every engineer's swarm has its own set of conventions, picked up from whatever files it happened to read first and whatever workflow the engineer built around it. When we looked at PRs across the team, we saw the same patterns being implemented in slightly different ways. The agents were solving the same problems, but in a fragmented way.


This isn't a new problem. Conventions drift in any engineering team. What's different with agents is the speed. Ten engineers running multiple agents each can produce dozens of versions of "how we do things" inside a single quarter. The drift compounds faster than humans alone would produce it.


## The fix wasn't centralization


The instinct when something fragments is to centralize. Pick one agent, one CLAUDE.md one way of doing things. That doesn't work for us, and looking at what Iain and John and Kate have built, I wouldn't want it to. The patterns they've developed are too valuable to throw away. Telling them which agent harness to use is the same fight as telling people which text editor to use, and I don't want to have that fight.


What we could standardize was one level up. Not the agents. Not the harnesses. The substrate the agents ground in.


If every agent is pulling its context from somewhere, that somewhere can be shared. If every agent is following some set of conventions, those conventions can live in one place that everyone references. The agents themselves stay personal. The ground they stand on becomes common.


That's the principle we've been working from. It's the same principle we already use for linters and style guides. The tool is your choice. The rules are shared.


## The standards repo


The first thing we did was move our engineering standards out of Notion and into GitHub. They're markdown files now. Each one covers a specific area: Ruby, Rails migrations, feature flags, Postgres, observability, breaking changes. Some of these are well-developed. Others are still stubs that we're filling in. The consolidation is ongoing.


The reason for the move was practical. Agents can read markdown in GitHub fast. They can't read Notion. Version control gives us an edit history. PRs let us review changes to standards the same way we review changes to code. Notion was where standards went to be forgotten. GitHub keeps them in the path the engineering work already follows.


We're being honest with ourselves about the state. The README in that repo flags it as a work in progress, and that's accurate. Some areas have good coverage. Others have a placeholder file and a TODO. The point isn't that we've finished writing down everything we know. The point is that we have one place to put it, in a format the agents can use.


This repo is the ground. It's necessary but not sufficient. A document sitting in a repo doesn't change what gets shipped on its own. What changes what gets shipped is what the agents actually do when they're running. That's where the second piece comes in.


## The agent tools


The second repo we built is a Claude Code plugin called rootly-agent-tools. It's a marketplace of skills and commands. One install command, and every engineer on the team gets the same set. Updates push automatically. If we add a new skill, everyone has it the next time they pull.


The rule of thumb we use for what belongs in there is simple. If it would go in a repo's CLAUDE.md keep it there. If you'd want it everywhere, it belongs in the agent tools. That line is what keeps the marketplace from sprawling. It also makes the boundary between team-wide and repo-local explicit, which matters when more people start contributing.


## The pointer pattern


The skill that links the agent tools to the standards repo is one I want to talk about, because the design is more interesting than it looks.


The skill is called engineering-standards. It's 46 lines. It doesn't contain any standards. What it does is tell the agent that the standards repo is on disk, and to go check it before making an architectural decision. The agent reads the specific standard it needs, when it needs it. We don't pre-load the whole thing.


This was a deliberate choice. If we baked the standards into the skill, every agent invocation would pull the entire content whether the task needed it or not. By making the skill a pointer, the agent reads what's relevant to the decision in front of it. Migration questions pull the migration doc. Feature flag questions pull the feature flag doc. The cost stays small.


It's also less prescriptive. A skill that says "consult these standards" leaves room for the engineer to use judgment. A skill that pre-loads everything starts to feel like a checklist the agent is grading you against. We didn't want that.


## Process as skill


The piece of this that's doing the most work is a skill called submit-pr.


When an engineer asks an agent to ship a feature, submit-pr runs a six-phase pipeline. It writes E2E tests. It runs them against real data. It generates an architecture diagram. It builds a PR with verified evidence and a rollback plan. Every claim in the PR body has to be backed by actual execution output. Hand-written examples are not allowed.


This is in the agent tools, not the standards repo. We don't have a document called "how to write a good PR." We have a skill that produces good PRs. The skill encodes the behavior directly.


The rule about not fabricating evidence is the most important one in there. Agents are perfectly capable of producing plausible-sounding fiction. A description of a test that wasn't run. A console session that was hand-written. The skill is structured to prevent that. If the dev DB can't produce a real journey, the gap gets stated in the PR. We don't paper over it.


That's the pattern we're leaning into. Where we used to write a document and hope engineers read it, we now write a skill that produces the right thing by default. The standards repo still matters. But the skills are where the behavior actually lives.


## The thing I'm still figuring out


The thing I haven't solved is maintenance.


John built an internal Mac app called Soil over a weekend. It visualizes his agent workflows. Multiple worktrees, sub-agents, all of it. He shared it with the team. Other engineers started opening PRs to add features. One of our QA engineers, who doesn't write code day to day, added light mode.


The cost to build that kind of tool is cheaper than it has ever been. That's the part that's exciting. The part I'm still working through is what happens after the weekend.


Soil doesn't have an auto-updater. It doesn't have a settings menu. If John leaves Rootly tomorrow, who owns it? If macOS ships an update that breaks it next month, who fixes it?


The same question applies to the skills. Every rule in submit-pr is there because something went wrong at least once. Those rules are good now. Will they still be good in six months when the codebase has moved, when the agents have new capabilities, when our understanding of what good engineering looks like has shifted again? Probably some of them. Not all of them.


Building this stuff is cheap. Maintaining it is the same problem it has always been. We haven't solved it, and I don't think anyone has yet. That's the part of agent-based engineering I'm thinking about most right now.


‍
