---
schema_version: "1.0.0"
document_id: "b789918c1711de2d5d0f097d7ad83b2fd114325fedd17ccd77c93daeb3c8ecac"
company_key: "yc-continue"
company: "Continue"
source_id: "yc-continue-rss-8ad3fe8c275c"
canonical_url: "https://blog.continue.dev/spaghetti-software-factory"
published_at: "2026-03-06T20:00:00+00:00"
first_seen_at: "2026-07-24T23:23:23.996128+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:d6263134fc044c6b3c2615016ea132b4624499346e279bc1f185d8ae8c722bb9"
---

# Check on Your Spaghetti Software Factory

There's a[Factorio Steam review](https://steamcommunity.com/id/site19/recommended/427520) that became scripture in the community. A numbered list about factory chaos escalates into a darkly poetic monologue: "A hundred furnaces belch smoke and the black blood of the earth is torn from its cradle to fuel the fires of industry." The narrator becomes a cyborg overseeing a dead planet. The closing refrain, "The factory grows," became a mantra. 305 comments, almost every one just people chanting it years later.


It works because it mirrors what the game does to you. You start optimizing a small thing and before you know it you've consumed a planet and can't stop. The reviewer wrote it at 94 hours. He went on to play 235.


Everyone building with AI agents is speedrunning this experience.


## You're Already Building a Factory


You start with one Claude Code session. Then two. Then four in parallel, each in its own worktree. You add a CLAUDE.md. You write skills. You connect MCPs. You didn't set out to build a factory, but you built one. And like any factory built without a plan, things tangle fast. That's fine. Spaghetti factories are still factories.


The problem is what they produce. Shriram Krishnamurthi, a Brown CS professor,[published a teardown](https://gist.github.com/shriram/064756c61ca98774c2a509aa3893d941) of code Claude Code generated for a bookstore app. Floating-point arithmetic for money. Custom CSV parsers instead of standard libraries. Functions named` filterByTitle` that actually implement search. Each one minor. Together, a codebase that teaches the next agent session to write worse code. Slop breeds slop.


Joseph Ruscio named this trajectory:[write-only code](https://www.heavybit.com/library/article/write-only-code) . "A large and growing fraction of production code is never read by a human." The industry response has been faster horses: AI review bots, slightly quicker review tools. Review is the new bottleneck.


We solved this problem before. Not the AI part, but the structural part. The pattern that worked was tests and CI.


## Writing Things Down


The first instinct is right: markdown. AGENTS.md files, specs, skills, plans. You're encoding your standards. The security practices you care about. The naming conventions that exist in your head. The architecture boundaries that agents keep crossing.


Encoding is not enforcing. You can write the best skill in the world and nobody has to use it. Not your teammate, not the agent, not the background process that opened a PR at 3am.


## Running Checks Locally


The real shift is when you write checks and run them with your coding agent. Markdown files, plain English, each one describing a single standard. Each one runs as a full AI agent, not just scanning a diff but reading files, running commands, exercising judgment. If it finds something, it fails with a suggested fix. If everything's clean, it passes silently.


This is not conceptually new. It's tests. Semantic tests for things linters can't express: "every new endpoint needs auth middleware," "don't add dependencies without justification," "if this PR touches the metrics pipeline, verify the dashboard isn't corrupted."


We had a telemetry integrity check that was silent for weeks. Then an agent changed an event name that powered our core dashboard. The check caught it, flagged the impact, suggested a fix. A convention check stopped hardcoded colors from leaking into our design system. These aren't hypotheticals.


## Running Checks Without You


Tests started local too. Developers ran them on their machines and trusted each other to do the same. That stopped working for every reason that applies to checks now: "works on my machine" divergence (your local agent has a different prompt construction, different context window, different tool configuration than your teammate's), the honor system (people forget, skip, rush), and silent drift. If a bad convention slips in three PRs before anyone catches it, the agent reads that code as the standard. It writes more like it. The codebase degrades from the inside.


Martin Fowler[named this](https://martinfowler.com/articles/continuousIntegration.html) in his original CI article. CI's deeper insight: the result had to be visible to the whole team, immediately. A red build meant everyone knew. Checks need the same visibility, on the PR, before merge, before the problem compounds.


## Building It Yourself


The next step is putting these checks in CI so they run on every PR. GitHub Actions is the obvious choice: you write a workflow that spins up Claude Code, points it at your` .continue/checks/` directory, and has it evaluate the diff. AI is good enough now that you believe the agent can build this infrastructure too. It can. 80 lines of YAML. Works on Monday.


By month one: you're managing background processes for parallel execution, routing stdout to separate files, serializing API calls to avoid rate limits. The agent doesn't always produce valid JSON, so you add fallback parsing. A check fails and you want to understand why, but the session is a terminated process on a recycled VM. By month two: someone builds a dashboard because nobody wants to dig through Actions logs. Then it needs auth, a diff viewer, an "accept fix" button, loop detection, historical storage. You built a SaaS product to avoid buying one. By month three: the checks have drifted. No metrics on which catch real issues vs. which cry wolf. No feedback loop. The checks fossilize or produce noise until someone asks to turn them off.


Some problems don't have YAML solutions. You can't follow up with the agent that made a judgment call. You can't track flakiness, enforce org-wide policies, or start from community checks instead of writing every one from scratch. The system that prevents spaghetti becomes its own spaghetti factory.


As your factory produces more code faster, the reliability of the checking system becomes load-bearing infrastructure. A quality control system that gates every PR across your org is not a side project. It's infrastructure you'll be on call for. You don't build your own CI for the same reason: not because you can't, but because reliability is the product, and you'd rather it be someone's entire job.


## Adding Mission Control


Or you skip the previous section.


Same markdown files in` .continue/checks/` . Full agent execution on every PR. Native GitHub status checks. No YAML. No dashboard to build. It presents as simple because the thorny problems (non-determinism, verdict parsing, concurrent execution, credential isolation, loop detection) are solved underneath. You don't know what you don't know until month three.


On month one, a check fails and you reject it with feedback. "Line 42 is intentional, don't flag that pattern." The feedback refines the check. The check gets better because you used it.


On month two,[intervention rate metrics](https://blog.continue.dev/intervention-rates-are-the-new-build-times) show which checks humans override, where false positives cluster. That data drives check evolution instead of fossilization. You start from community checks, beginning at someone else's iteration 15.


On month three, you realize PR-open was just the first trigger. You want scans that sweep the whole codebase when a new standard is introduced. Flags that route attention to sensitive changes (auth, migrations, billing) without blocking. Checks on PR merge, not just PR open. Triggers from Sentry alerts and Snyk findings. In Mission Control, these are agents you can set up. In YAML, each one is another workflow to maintain.


## The Factory Must Grow


We went on this journey ourselves. Then we started reading about the[Toyota Production System](https://continue.dev/blueprint) and realized the wheel was invented decades ago. Jidoka: automation with a human touch. Kaizen: continuous improvement. Andon: the alert cord that stops the line. A check that catches a corrupted dashboard is an andon cord. A check that evolves from the last 20 PRs is kaizen. The whole system (standards encoded by humans, enforced by AI, decided by humans) is jidoka.


The progression we see with every team: they start with one check for their most painful standard. They see it catch something real. They write three more. Within weeks,` .continue/checks/` becomes the encoding of their engineering taste.


[The Factorio review](https://steamcommunity.com/id/site19/recommended/427520) ended with a cyborg overseeing a dead planet. Software factories don't have to end that way. The factory must grow, yes. It grows better when you check on it.
