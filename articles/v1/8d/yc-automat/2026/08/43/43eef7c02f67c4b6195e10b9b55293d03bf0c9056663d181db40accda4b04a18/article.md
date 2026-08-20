---
schema_version: "1.0.0"
document_id: "43eef7c02f67c4b6195e10b9b55293d03bf0c9056663d181db40accda4b04a18"
company_key: "yc-automat"
company: "Automat"
source_id: "yc-automat-news-import-800367705320"
canonical_url: "https://runautomat.com/blog/the-harness-is-the-product-agentic-first-development-at-scale"
published_at: "2026-08-11T21:51:48.979+00:00"
first_seen_at: "2026-08-07T03:24:31.231391+00:00"
fetched_at: "2026-08-07T03:24:33.196635+00:00"
content_hash: "sha256:998bcda7a92e4dc8c380d38cb81a05da830f9fa4a2866ef1b0c3738a2e059b81"
---

# The Harness Is the Product: Agentic First Development at Scale | Automat

Years ago I managed an engineer who was brilliant and exhausting. Fast, creative, allergic to process, and would refactor three modules because they noticed an antipattern on the way to fixing a one-line bug. If you've been in this industry long enough, you've met someone like this. My job wasn't to write the code. It was knowing when to let them run, when to pull them to a whiteboard for ten minutes, and when to be stern and say we're solving the wrong problem.


‍


Managing an agent rhymes with that. Same muscle, nearly the exact same job - except they weren't in any of the meetings, and they'll brief you like they were.


‍


Six months of running agent-first, greenfield, with a tiny team, has mostly been the same lesson with different quirks. The amnesia is new. The false familiarity is new. The (very) big invoice for context is new. The judgment calls aren't: should we do this, is it worth it, shortcut or pay down debt.


‍


Code is a byproduct; the harness is now the product. What follows is how we run the team, starting with who we work with.


‍


## **Your daily driver is a teammate, not the team**


‍


I've never built a good team by hiring five of the same engineer, you hire for gaps. You put the fast one next to the careful one, the skeptic next to the person who'll ship the first idea they come up with, because the argument in the middle is where the quality comes from.


‍


Models work the same way.


‍


Karpathy's “vibe coding” nails casual use, and for good reason. This stuff is technically opaque and when you're using it in production, it means spending dozens of hours a week typing at a shapeless entity. If the vibe is wrong for me, I'm miserable. I pick a primary model like a lead engineer: chemistry, taste, how it asks questions. I call that your **daily driver** .


‍


***Callout (single-model bias):*** *Much like that charismatic engineer, the model you like speaking with the most has failure modes you won't see (or more likely, ignore). Composition will always beat hunting for one perfect model.*


‍


Your daily driver can't be your entire bench though. If yours is too permissive (and permissive often feels fast), use a different model as principled **reviewer** . Different priors, different blind spots. Sometimes a third as a research-heavy **designer** , or a context-lacking fast agent for a **product-** level sanity check.


‍


Hire a cohesive team and you'll get cohesive results.


‍


## **The harness is a system**


‍


Every company I’ve worked at had the same onboarding problem in one of three shapes: The wiki didn't exist, or it was stale, or it had turned into a bloated graveyard everyone pretended to trust. Sure sometimes the team is genuinely lazy, but priorities and time constraints are the overwhelming culprits - documentation always competes with shipping, and shipping usually wins.


‍


Agents have the same problem, but nobody pulls them aside after standup and says "actually ignore the design doc, we wrote that when we were still on free tier Claude Code. Here's what we actually do." Whatever lands in context for that session is the whole of what they know.


‍


What's different is the mechanics are finally manageable. You can scope what loads when, promote knowledge as it proves itself, and put the non-negotiable stuff in tests when asking nicely isn't enough. Documentation was always the work; we just got away with deferring it because humans skim, infer, and patch the gaps in conversation. Agents don't . They execute what you wrote down literally. That's why the harness matters now.


‍


We run three tiers across three repos and several IDEs:


‍


- **Tier 3 - nice to have:** Reference depth you pull in when you need it.
- **Tier 2 - useful** : Scoped rules, about forty of them now for us, under two hundred lines each, attached when you touch matching files.
- **Tier 1 - necessary:** AGENTS.md, loaded every session, the invariants that have to survive amnesia.


‍ *‍*


***Callout (multi-IDE wiring):*** *Getting tiers to work across Cursor, Claude Code, and Codex took annoying frontmatter: Tier 2 gets \`globs\` + \`paths\`, no \`description\` (Cursor ignores globs when description is set); Tier 3 gets description only; Tier 2 symlinks from \`.cursor/rules/\` to \`.claude/rules/\`.*


‍


When something keeps mattering, enforcement moves up the ladder. A mistake becomes a pattern; a pattern becomes a problem. Anyone who's managed engineers knows that moment. The difference is instead of a tricky and tense conversation, you simply promote the fix to **programmatic** : contract-lock tests, coverage gates, deploy guards. Suggestions become laws.


‍


***Callout (context cost):*** *All of this context shows up on the inference bill. Bigger prompts, more tokens, pricier sessions. We notice. We also fight fewer bugs, re-explain less architecture, and let strong models run longer before a human has to step in. For us it's a time-versus-money trade, not a free win.*


‍


Context rot forced the split. An eight-hundred-fifty-line \`AGENTS.md\` was actively hurting us. We lead with “never do X” because[experiments say prohibitions stick better than suggestions.](https://www.arxiv.org/pdf/2604.11088)


Maintaining the tiers is the same ongoing work as onboarding docs and lint rules. The difference is it’s suddenly no longer a separate activity from delivering work.


‍


## **Every ship makes the next one cheaper**


‍


In a normal company you try to gain efficiency with retros, quarterly planning, postmortems, the tough conversation after someone merges the wrong thing. Important work. Also exhausting, easy to defer, and barely works. Let it linger a quarter and the lesson evaporates. Tribal knowledge lives in three people's heads until they leave.


‍


Agent-first makes these rituals impossible, and forces them to be front-loaded. The agent tab you're about to close can't have a retro with the three you're about to open. The harness has to get smarter as you ship, and every feature is a chance to encode what you learned for the next session: a state-machine feature added two scoped rules, and the next session that touched those files didn't rediscover the same landmines. Unlike a human team, the updated rules get followed literally on the next session.


‍


I don't babysit that process anymore. I'm not chasing people to update the wiki after a postmortem. Shipping the feature **is** the retro. If the lesson matters, it promotes up the ladder from the last section. The work still happens; it rides along with delivery instead of competing with it.


‍


Context lives at different depths on purpose. Linear holds past (and critically, future) plans that survive across sessions; Notion holds product intent humans still own; the codebase holds implementation scars in the tiered rules. You can't hand a new engineer the whole playbook on day one, but with agents you have no choice.


‍


## **Adversarial planning beats adversarial review**


‍


I've managed teams that reviewed every line and skipped the conversation that would have made those lines unnecessary. Skipping planning has always been a way of lying to yourself. You're borrowing speed against a bill you know is coming.


‍


The bill shows up the same way it always did: production bugs, PRs rotting in review because nobody agreed what we were building, V2 eating the quarter you promised sales. Agents don't change that; they just change how fast it gets out of control. A bad guess used to be one additional meeting, now it's forty files and one accidental migration to roll back. Good luck getting "actually, wait" to land in a GitHub comment after that.


‍


We still run Bugbot and multi-model reviews on PRs. Fine — even necessary. The argument that actually saves you happens earlier: plan time, skeptical second model, human in the loop when it's a product call, zero files open.


‍


**Lifecycle / state.** First pitch was if/else on status everywhere. Boring pushback, once someone said it out loud: what's the fifth state do to this? Capabilities table. New state is a row, not a repo-wide scavenger hunt.


‍


**Notifications / events.** The easy design for customer webhooks and in-app alerts: let people edit their notification settings like any other CRUD screen, which is fine until a webhook fails at 2am and nobody can see what was sent. The purist design: treat everything as an immutable event log, a great paper trail, but it would've meant rebuilding how we track workflow runs from the ground up. We needed both: settings you can change, plus a permanent record of each delivery. That's a product architecture call, not something you settle in drive-by comments on a three-thousand-line diff.


‍


*>* ***Callout (planning models are bad at time and sequencing):*** *They'll say six weeks for work that's an afternoon, or propose seven PRs when two would do - cautious in exactly the wrong places. Plan the implementation path alongside the design, not just the architecture diagram. Let two models fight about scope before anyone types, and then make the decision yourself; that's ten minutes. Let them fight once code exists; that's an afternoon for you.*


‍


## **Humans review the design, not the diff**


‍


I've trusted senior engineers with a review style that looks lazy on paper: read the PR description, scan the file list, ask if it serves the product, trust the diff because you trust the person and the process. That bargain took years of lunch conversations to earn, and agents don't give you those.


‍


Most teams have it backwards: agents review human code. We flip it. Humans review agent code for architecture, product fit, UX, roadmap fit. Did we end up somewhere we didn't mean to?


‍


***Callout (SOTA models are having product meetings without you):*** *This is especially true with newer models like Fable and the GPT-5.6 family, which is meant to find solutions at any cost without asking for help. Taste and product questions you didn't think about get asked and answered during that two-hour hands-free session you're bragging about at Zeitgeist with your software engineer friends.*


‍


That requires a skill a lot of senior ICs haven't practiced: **judging without typing,** and knowing what good looks like at the design level without reading every line. For some people the new workflow feels slower, harder to trust, less fun, because the job moved from "I wrote the solution" to "I decided whether the solution fits." Tech leads hit the same wall when they're no longer shipping more code than everyone else in the room. Some people take to it quickly. Some miss the old game. Both are normal.


‍


Line-level review still happens; it's just not the human's main job. Different models for writing vs reviewing. \`/deep-review\` runs five passes: documentation drift first, then bugs, regressions, edge cases, quality. \`BUGBOT.md\` rules from real bugs we've shipped.


‍


‍


That engineer from the opening never needed me to type faster than them. They needed someone in the room paying attention: when to stay out of the way, when to whiteboard for ten minutes, when to say we're solving the wrong problem before the sprint disappears into a refactor.


‍


At Automat we[re-started from scratch](https://runautomat.com/blog/a-builders-philosophy-why-were-rethinking-a-billion-dollar-question-and-rebuilding-it-from-scratch-now) in October, agent-first on purpose rather than retrofit, and since then the output has been ridiculous - on the order of a thousand commits and half a million lines touched across three repos (app, database schema, runtime), never crossing the five engineer mark - and the product is materially better for it. It's not an experiment. This is how we ship.


‍


The skeptics aren't wrong that there's slop out there; they're wrong about what counts as the work. They see the diff and miss the other half entirely. That's why it's never been easier to do rigorous engineering. You just need good engineering management.


‍


It was always the same deferred work: write it down, argue before the PR, review the design, encode the lesson. Humans patched the gaps in conversation; agents can't. So if you're a founder feeling behind because you're not reading every line of every diff, you're probably staring at the wrong artifact. Read the design. Pick your daily driver. Build the harness. Let the code be the byproduct.


‍
