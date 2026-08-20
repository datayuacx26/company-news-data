---
schema_version: "1.0.0"
document_id: "afa9f277fcd1e487965accdac304566b444f9299c9387e91abd88d234bef0412"
company_key: "monday-com-ltd-ordinary-shares"
company: "monday.com Ltd."
source_id: "monday-com-ltd-ordinary-shares-rss-78444ad48313"
canonical_url: "https://engineering.monday.com/how-we-released-a-major-version-of-our-design-system-in-1-week/"
published_at: "2026-05-06T10:06:29+00:00"
first_seen_at: "2026-07-20T23:22:14.841151+00:00"
fetched_at: "2026-07-28T21:14:13.187831+00:00"
content_hash: "sha256:5d2067a05257ab188e49e7d87e59ad49d51739f72e91c772d00fde46668e657e"
---

# How We Released a Major Version of Our Design System in 1 Week

If you’ve ever been part of a major version release, you probably remember how it feels.


Not one big moment, but a long sequence of small ones.


You pick a change. Open a PR. Realize something is missing. Go back and fix it. Then move to the next item.


Individually, nothing is especially complex. But together, it stretches out.


The work itself isn’t the problem, it’s the accumulation of all the surrounding steps.


In this article, we’ll walk through how we turned that process into something executable using AI and ended up building an entire major version in a week.


# **What This Process Usually Looks Like**


[The last time we did this](https://engineering.monday.com/how-to-ship-a-major-version-people-actually-upgrade-to/) , everything worked the way you’d expect.


We picked a change, implemented it, opened a PR, adjusted it, and merged it. Then we moved on to the next one.


Along the way, we wrote codemods when possible. Updated the changelog later. Eventually, we sat down to write a migration guide that tied everything together.


That flow repeated for every change.


Nothing about it felt wrong. It’s exactly how this kind of work is usually done. But when you look at it end to end, it’s a lot of steps, and you’re doing all of them for every single change.


End-to-end, that process took us five months.


# **Realizing a Breaking Change Is Actually a Workflow**


The shift started with a simple observation.


Every breaking change follows the same lifecycle:


define → implement → update usages → generate codemod → update docs → validate → ship


The order can vary slightly, but the structure is consistent.


Once we wrote it down, the pattern was obvious. We weren’t dealing with independent tasks. We were executing the same workflow repeatedly, by hand.


That made it a good candidate for automation – not at the level of individual steps, but as a complete system.


# **Turning Our monday Board Into Input**


We already had a board with all the breaking changes.


Each item was small enough to understand on its own. Rename a prop. Remove a behavior. Adjust an API that no longer made sense.


Normally, that board is a backlog. Work sits there until someone picks it up, understands it, and decides how to implement it.


This time, we treated it differently.


Each item was not something to interpret. It was something to pass through a predefined process.


# **Encoding the Process as a Skill**


Once the workflow was clear, the next step was ensuring it runs the same way every time.


We encoded it as a Claude Code skill:


/vibe-breaking-change


.


The goal wasn’t to assist with individual steps. It was to own the entire lifecycle of a breaking change.


Each run starts from a single item in the monday board and a clean worktree. From there, the skill treats that item as the source of truth and executes the full flow end-to-end.


The first part is the change itself. The skill applies the modification to the component code, but it doesn’t stop at the obvious surface-level update. It follows the impact of the change across the codebase, updating all relevant references so the system stays consistent. That includes usage patterns, types, and any internal dependencies that are affected by the change.


Once the code is updated, the migration layer is handled alongside it. When the change can be expressed deterministically, the skill generates a codemod as part of the same run. This ensures that the migration path is created at the same time as the breaking change, not reconstructed later.


In parallel, each run contributed to the documentation context. CHANGELOG.md and MIGRATION_GUIDE.md were incrementally updated with every change, capturing its intent and impact. By the end, those accumulated entries formed a complete, structured changelog and migration guide.


## **From implementation to delivery**


After the implementation phase, the skill validates the result. It runs the full set of checks: build, lint, and tests. If anything breaks, it fixes the issues within the same run until the change integrates cleanly with the rest of the system.


Only once everything passes does it move to delivery.


At that point, the skill opens a pull request. The PR is not a placeholder or a draft. It includes a full description of the change, the reasoning behind it, and a link back to the original monday item. By the time it’s opened, the change is already consistent, validated, and documented.


The final step is updating the board itself. The item is moved to “ready for code review,” reflecting the fact that the work is complete and ready for human validation.


# **What a Single Run Actually Looks Like**


The first time we ran the skill, we treated it as a controlled test.


We took a single item from the board, opened a fresh worktree, and ran


/vibe-breaking-change


.


What followed was not a sequence of manual decisions, but a continuous execution of the workflow.


It ran build, lint, and tests, fixing issues until everything passed.


Only then did it open a pull request.


By the time the PR appeared, the change was already complete and ready for review.


That was the moment the nature of the work changed.


Instead of thinking through each step of the process, we were watching a complete workflow execute from start to finish.


From there, scaling it was straightforward. Each item on the board went through the same path, with the same guarantees, producing the same kind of output every time.


# **What Actually Changed**


From the outside, it might look like we simply moved faster.


But the work itself didn’t change. We still made the same decisions, applied the same level of care to API changes, and treated each breaking change as something that affects real teams.


What changed wasn’t the scope. It wasn’t that the work got easier.


What changed is that we stopped thinking in terms of doing tasks and started thinking in terms of building repeatable systems.


A breaking change is not just a code modification. It is a sequence of operations that always includes implementation, codemod generation when applicable, documentation updates, and delivery through a reviewable pull request tied back to its source.


When those operations are executed manually, they are repeated with small variations every time. When they are captured as a workflow, they become something you can run.


# **Where This Leads**


By the end of the week, the version was complete.


Every change implemented. Every migration path defined. Documentation aligned with the code. The system held together as a single, coherent release.


The complexity didn’t disappear. The process did.


This is what AI-native development looks like in practice.


It’s not about writing code faster.


It’s about turning workflows into systems that can execute end-to-end.
