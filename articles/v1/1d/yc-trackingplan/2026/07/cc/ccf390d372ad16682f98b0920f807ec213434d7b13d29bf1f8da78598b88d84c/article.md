---
schema_version: "1.0.0"
document_id: "ccf390d372ad16682f98b0920f807ec213434d7b13d29bf1f8da78598b88d84c"
company_key: "yc-trackingplan"
company: "Trackingplan"
source_id: "yc-trackingplan-news-import-6a56f7a9281f"
canonical_url: "https://www.trackingplan.com/blog/trackingplan-x-ai-an-engineering-note"
published_at: "2026-07-25T07:01:02.885+00:00"
first_seen_at: "2026-07-24T16:57:55.685518+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:2f717811af3b53a3907485415c7b6005c802e63b8aaf06e1965ab40731096b17"
---

# A third of the team. Four times the PRs each. Higher quality.

**` — THE CORE NUMBER`**


## A third of the team, four times the output each.


Each bar is pull requests merged per active engineer, per quarter; the label beneath it is how many engineers we had. As the team shrank from eight to three, output per person roughly quadrupled — and total output hit record highs.


Pull requests merged per engineer · by quarter


Bar = PRs per active engineer. Label = headcount that quarter.


50 PRs · 8 engineers


50


'24 Q3
**8**


43 PRs · 8 engineers


43


'24 Q4
**8**


52 PRs · 8 engineers


52


'25 Q1
**8**


48 PRs · 7 engineers


48


'25 Q2
**7**


91 PRs · 4 engineers


91


'25 Q3
**4**


80 PRs · 4 engineers


80


'25 Q4
**4**


175 PRs · 3 engineers


175


'26 Q1
**3**


189 PRs · 3 engineers ✦


189


'26 Q2
**3**


// Same codebase, a third of the people, roughly four times the pull requests each — while bug reports fell by half over the same period.


## Where the extra hours came from


The chart understates what shrinking the team gave back, because eight people don't just cost eight salaries — they cost the machinery that keeps eight people aligned. We ran daily reviews and a weekly in-depth sync. We planned in two-week and eight-week cycles — our adaptation of Basecamp's[Shape Up](https://basecamp.com/shapeup) methodology — with all the ceremonies it takes to get there: Josele — our CEO, who is also our CPO — turned the roadmap into product specs and feature proposals; Julia Vivas, our product designer, did everything it takes to turn those into something buildable — with consistent Figma designs as the visible output, sequenced with a plan in mind so several engineers could build in parallel without colliding. And I was the technical half of that machine: large, deep code reviews as tech lead, architectural planning and discussion, coordinating dependencies across people, and being the interrupt handler whenever a question or an issue came up.


None of that was waste — it's what coordinating eight people honestly costs. It just isn't building.


Three people with full shared context, plus agents carrying the review load, dissolve nearly all of it. The syncs shrank to conversations. The planning horizon collapsed — Josele went from writing specs for that machine to shipping code in it. Deep review became the agents' first pass, so I read the diffs that matter instead of all of them. Design stopped having to choreograph parallel workstreams months ahead. The hours that used to go into keeping people aligned now go into the product itself — coding or, these days, prompting.


## Three threads, moving in parallel


It helps to keep three things apart that are easy to jumble together: how we *write* code, how we *ship and operate* it, and what we actually *build* for customers. Each moved on its own track.


### Thread 1 · How we write code


#### From autocomplete to agent-written, in two years


The single biggest change wasn't a tool — it was watching the way our code gets written turn over completely: from finishing our lines, to drafting our functions, to opening its own pull requests. The moments that got us there:


THE BEFORE


**Autocomplete.** The editor finished the line you were already typing. You still wrote everything else.


2024–25


**LLM-assisted.** Chat moved into the editor and drafted whole functions — but a human drove every decision, keystroke by keystroke.


AUG 2025


**Agents enter code review.** The first agent to do real, unattended work: reviewing every pull request we open. Still our highest-leverage bet.


JAN 2026


**Autonomous routines.** Agents begin working unsupervised — triaging new issues, patching dependencies, running security passes and opening their own PRs.


FEB–MAR 2026


**The skills library.** Its heart lands — two dozen named workflows in two months. Shipping an integration or an audit becomes a single command.


APR 2026


**The rulebook.** The bulk of our agent rules — a manual for every part of the codebase — plus a second, independent reviewer. Now agents are reliable everywhere.


That arc has an endpoint, and it's where we work now: **effectively every line is agent-written** — drafted by an LLM in Cursor or Claude, with a growing share opened by agents running on their own. The question stopped being "did AI help write this" — it did — and became "how much of it ran with no human in the loop at all."


### Thread 2 · How we ship & operate


Writing code faster is pointless if it jams at review and release. So the path from a finished change to a merged one is a set of named steps an engineer — or an agent — runs by name:


- ` /create-pr` opens a draft pull request: it syncs with the main branch and writes a scoped title, description and linked issues.
- ` /autoreview` then babysits it — watching CI, fixing failures, and answering review comments (human *and* bot) until everything is green.
- ` /contextual-review` catches the class of problem a diff-only bot can't: a change that passes every check but quietly does the wrong thing. It needs the context of the working session to see it.
- ` /wrap-up` is the end-of-session cleanup pass — fresh-eyes reviewers simplify the code without changing behavior, capture any new repo rules, and update the design docs before anything merges. It's our equivalent of Claude's["dreaming"](https://claudefa.st/blog/guide/mechanics/auto-dream) : the session's lessons get consolidated instead of lost.


On top of those, a **merge ladder** lets you choose how much review a change gets — and who hits merge. Three of the four review the code; only the last skips it:


low


/ship-it-lite


Reviewed and CI-checked, minimal ceremony — then **you** merge.


standard


/ship-it


Full review and cleanup — then **you** merge.


approved


/lgtm


Reviewed; once you approve, it self-merges.


trivial


/yolo


No review — straight to a squash-merge. Only for changes you know cold.


#### Review is three independent opinions, not one


Nothing merges without review, and it isn't a single reviewer — every pull request is read independently by both **Cursor and Claude** , including a dedicated security pass, kept deliberately separate so we get honest disagreement instead of an echo chamber. They don't anchor on each other's comments. This is the single change that let three people hold the quality bar while shipping far more.


#### Agents stand our watch, too


The same idea runs our operations. Keeping the service healthy — what we call the "Sheriff" rotation — used to mean a person watching dashboards. Now agent routines take the first pass: triaging production alerts, sweeping our dependencies for known vulnerabilities, and keeping our data integrations healthy — each one opening its own fix PR for a human to approve. And the loop can close on its own: when we file a bug, one agent triages and labels it, then another agent drafts a candidate fix and opens the PR — before an engineer has even looked at it.


What makes all of this possible is that agents hold the same instruments we do. We gave them a command-line interface to Trackingplan itself and MCP connections into our systems and data, so an agent can pull a customer's tracking plan configuration, query live traffic, or measure a query's cost without a human driving. That unblocked whole classes of work beyond writing code — debugging, auditing, benchmarking, optimizing — that used to need a person at a dashboard.


### Thread 3 · What we actually shipped


Leverage would be hollow if it only produced process. It didn't — the last twelve months were our most productive ever on the product itself:


24


New integrations


Eighty-three[integrations](https://www.trackingplan.com/integrations) supported in all — two dozen added this year, each turning a marketing or analytics tool's traffic into structured data.


11


New deep audits


An entire data-quality audit framework — checks that flag broken tracking — built from nothing this year.


12


Consent integrations


A consent-monitoring subsystem covering a dozen consent platforms, from zero a year ago.


~5×


Faster new-event detection


A newly-appearing event now surfaces within minutes — up to 31× faster on some plans.


2


Major analyst features


Configurable attribution views and a saved-reports API — both shipped and generally available.


soon


A native Trackingplan agent


Our own AI agent, answering questions straight from your raw data — in development now, leveraging everything we've learned building *with* AI to build the AI product itself.


The most telling part isn't the counts — it's *who* ships them. The people delivering this work aren't all engineers: a whole integration or a new data-quality audit is one skill away (` /new-parser` ,` /new-audit` ), and the rails are safe enough that **David** — our swiss-army knife, who does a bit of everything — has shipped eight integrations end-to-end this year, while **Ángel** , our head of customer success, ships customer-specific configurations or custom reports directly. The three engineers built the rails; the rest of the company drives on them.


#### **Sales feels it, too**


This isn't only an engineering story — the same speed shows up in how we sell:


- **Trials no longer stall on missing integrations.** When a prospect uses a destination we don't support yet, we can now add it much faster than before — even mid-trial — which directly unblocks conversions.
- **Value on day one.** Our new deep audits surface tracking issues after a single day of data, so a prospect sees real findings in the very first presentation, not after weeks of monitoring.
- **The most-valued feature of the last two years is itself AI:** the Warning Analysis in our[AI-assisted debugger](https://www.trackingplan.com/solutions/ai-assisted-debugger) — the single thing customers have praised most in this whole period.


## We did get faster — where it counted


The pull requests that mattered — the large, meaty changes that needed real review — used to sit for **days** waiting on a human. That was the real bottleneck, and it's what automated review fixed: the slowest changes fell from roughly **five days to under two** (90th percentile), and a typical reviewed PR from about a day to a few hours. The small stuff always merged in minutes — it was never the problem; the big changes are what stopped getting stuck.


## Machines propose, humans decide


We no longer really measure "AI-written versus hand-written," because there's almost no hand-written code left to compare against. The line that matters now is how much work arrives *already drafted* . Dependency updates have come in as bot pull requests for years. What's new is that, since early 2026, our own coding agents open pull requests too — dozens a week and climbing — and every bug we file gets a candidate fix drafted automatically before an engineer opens the editor. The human job shifted from writing the first draft to deciding on it.


**` — THE RECEIPTS`**


## What actually moved


Everything here comes from our own git history, the GitHub API, and our CI, over roughly two years. Windows are calendar quarters unless noted.


Metric Before Now Change


Active engineersmerging code each quarter 8 3 keep-it-simple


PRs merged per engineerper quarter ~50 ~189 ×3.8


Pull requests mergedtrailing 12 months — 2,061 record months


Code written with an LLMassisted or autonomous ~0% ~100% since early '26


Slow-tail merge time90th percentile ~5 days < 2 days much faster


Bug reportslate 2024 → late 2025 274 133 −51%


New integrationslast 12 months — 24 83 total


New deep auditslast 12 months 0 11 new framework


Agent-opened pull requestsdrafted for review 0 dozens a week · climbing


Read those together and the shape is clear. We didn't add engineers — we went the other way and put the difference into the rest of the company. Three people now do what eight used to, the big reviewed PRs that used to sit for days clear in hours, and quality went *up* . AI is the only reason that arithmetic holds.


Active engineers


8


→


3


keep-it-simple


PRs / engineer / quarter


50


→


189


×3.8


Slow-tail merge (90th pct)


~5 d


→


<2 d


faster


Bug reports · late '24 → '25


274


→


133


−51%


## The team behind the numbers


Three of us carry the repository day to day — **Jose** (our lead engineer), **Josele** (our CEO and CPO), and me. There's no management layer between us and the code; even our CEO still ships it. And we build on a foundation a larger engineering team laid down. **Oleg Kozynenko** , **Daniel Sangorrin** , **Jordi Cantó Gálvez** and **Manuel Miranda** were among the most prolific engineers this codebase has ever had; a great deal of the ingestion, parsing and platform work our agents build on every day is theirs. And **Julia Vivas** , our product designer: holding the whole picture together — digesting every plan and designing it with the consistency and foresight that let a team that size build in parallel — was demanding, essential work, and she carried it with Josele on the product side and me on the technical one.


Keeping engineering small was a deliberate decision about where a growing company invests its people, not a verdict on anyone who moved on. I'm grateful to all of them; none of this would exist without their work.


## What I'd tell another team


- **If you're already fast, "ship more PRs" is the wrong goal.** Measure what AI actually frees up. For us it was people — *and the ceremonies it takes to coordinate them* . We reinvested both in the rest of the company.
- **Automate review before anything else.** It's the single change that let three engineers keep the quality bar while shipping far more.
- **Make the workflow a set of named, repeatable skills.** Once shipping an integration or an audit is a single agent skill, people who aren't engineers can ship product too.
- **Keep a cleanup ratchet.** Speed without an enforced end-of-session review just produces faster mess.
- **Spend on the best models.** Compute is the cheapest thing in the building.


We're not finished — our own agent is still in the oven, and the fully-autonomous share is still small. But the direction is set: a small, senior team, and a codebase where adding an agent is as ordinary as adding a teammate.


**— Alexandros Andre Chaaraoui, CTO & Cofounder, Trackingplan**


*Figures come from our own git history, the GitHub API, and our CI, over roughly two years; windows are calendar quarters unless noted. "Active engineer" means a person who merged at least eight pull requests in a quarter; time-to-merge is measured from a pull request opening to it merging. Individual figures are approximate and rounded.*
