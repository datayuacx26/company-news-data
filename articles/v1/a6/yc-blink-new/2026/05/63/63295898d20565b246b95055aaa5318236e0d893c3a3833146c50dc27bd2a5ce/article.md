---
schema_version: "1.0.0"
document_id: "63295898d20565b246b95055aaa5318236e0d893c3a3833146c50dc27bd2a5ce"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-we-build-at-blink"
published_at: "2026-05-09T02:54:59+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:12.047673+00:00"
content_hash: "sha256:ebaa472d40b6fcc642a51f8023cea894e14b47bbb7807f2b2a91c1c2f2562631"
---

# How We Build at Blink: Our AI-First Development Workflow

## Context engineering with CLAUDE.md


Every session starts with full project context.


If you're new to this style of development,[what is agentic coding](https://blink.new/blog/what-is-agentic-coding) covers the fundamentals — agents that execute entire development loops autonomously, not just autocomplete suggestions.


We have two files that make this work:` CLAUDE.md` (Claude Code's native memory file, read automatically on every session start) and` AGENTS.md` (the same, written for agents working across the whole repo).


` AGENTS.md` is the most important document in the codebase. It covers:


- The full architecture: which service owns what, where data lives, which systems are isolated from which
- The "never do this" rules that took production incidents to discover
- Links to deeper docs (` docs/lessons.md` ,` docs/blink-claw-deployment.md` , etc.) for the topics where one wrong move destroys a feature
- Environment variables and what each controls


Every agent working on Blink reads this at the start of every session. They don't need to explore the codebase to understand what Postgres cluster to write to, or why we can't` git add -A` , or what` npm run build` does to the dev server. It's already in the context.


The rule we follow: **if an agent makes a mistake twice, it goes in` AGENTS.md`** . Every lesson from production becomes a hard rule that every future agent reads before touching code.


[Cursor's agent best practices](https://www.cursor.com/blog/agent-best-practices) describe this as "Rules: static context for your project." We go further — our rules file is also a postmortem archive. Every rule has a story.


## What we learned the hard way


These are the rules that exist because we broke something.


**Never` git add -A` .**


One early agent batch-committed everything in the working tree, including a local` .env` override and a debug log file with internal API shapes. We caught it before push. Now our` AGENTS.md` has a hard rule: stage files explicitly by name, one by one.` git add -A` is blocked at the hook level.


**Read the whole file before editing.**


We had a rule: "read before you edit." We didn't enforce it mechanically. An agent rewrote a 200-line auth helper from scratch — correctly, functionally, but it removed a production workaround that existed for a very specific Fly.io timing issue we'd spent two days debugging. The workaround had no comment. The agent had no idea. Now our rules enforce read-whole-file before any edit.


**Never run` npm run build` during development.**


This one takes down the dev server. Completely. It sounds obvious, but agents seeing a TypeScript error will reach for the nearest build command. Blocked at the hook level now.


**Never create server functions for client-side logic.**


Server functions break when we ship new Next.js versions. We learned this with a particularly bad deployment. Now it's in` AGENTS.md` in bold: "Use API endpoints, not server functions." The agent doesn't need to understand why — the rule is there and it follows it.


**Small, focused commits.**


Every commit should do one thing. Not "add feature X" — "add API endpoint for feature X." Not "fix bugs" — "fix null check in session restore." Small commits mean reverting one specific thing is possible. With agents running fast, it's easy to end up with giant commits that can't be untangled. We now give agents explicit commit instructions in every spec.


The spec-first workflow: write what you want before AI touches code, then verify until PASS


Blink


## Always be deploying


We don't batch features and deploy weekly. We merge small and deploy constantly.


The codebase runs on Next.js 16 + React 19, TypeScript throughout, Turbopack for dev. Firebase handles auth. Postgres (PG2 on Railway) is the single source of truth for all app data. PostHog traces every LLM call. Tinybird handles analytics.


Agents don't need to configure any of this per deployment. They write code, run lint, commit, push. The Railway pipeline handles the rest.


We ARE the deployment target. We use Blink Cloud to ship the same product we build. When a subagent commits an infrastructure change, that change goes out on the same platform we tell users to ship on. This creates a very fast feedback loop: if something breaks for developers using Blink Cloud, we feel it immediately. If something is slower than it should be, we notice in our own workflow before it affects users.


The practical effect: we don't tolerate flaky deploys. Every friction point in the deployment pipeline is a friction point in our own development loop, so it gets fixed.


Deploy as you build — Blink Cloud handles infrastructure so the team focuses on product


Blink


## Build Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app using Blink and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


---


Both, for different things. Claude Code runs our longer autonomous loops — the ones where an agent needs to read 20+ files, make architectural decisions, and run for 10+ minutes without interruption. Cursor handles the interactive sessions where a developer wants to stay in the loop. The two workflows coexist. Our` AGENTS.md` context file works for both because it lives in the repo, not in a tool-specific config.


Two ways. First, the verifier loop catches most mistakes before they reach a commit — the verifier subagent checks the diff against the spec and returns FAIL with specific notes if something is wrong. Second, when something does reach production and break, we document it immediately in` docs/lessons.md` and add a hard rule to` AGENTS.md` . Every production incident becomes an agent rule. The rule list is long. That's the point.


` CLAUDE.md` is Claude Code's native memory file — it gets read automatically at the start of every Claude Code session.` AGENTS.md` is the broader context document for any agent (Claude Code, Cursor, cloud agents).` CLAUDE.md` points to` AGENTS.md` for the full rules.` AGENTS.md` covers architecture, invariants, "never do this" rules, and links to the deeper topic docs. If you're starting from zero, write` AGENTS.md` first and have` CLAUDE.md` reference it.


Spec decomposition first. If you can't split the spec into truly independent pieces, don't run parallel. When you can, each subagent gets its own git worktree — Cursor creates these automatically. Each agent has isolated file state and branches, so edits don't collide. We merge after all agents report done. If two agents touch the same file for different reasons, the spec wasn't decomposed cleanly enough. Rewrite the spec.


No. We started this on a team of two engineers. The spec-first + verifier loop pattern works better at small team size, not worse — there's no organizational overhead, and the context file is short enough that one person can maintain it. The main investment is writing` AGENTS.md` once and keeping it updated. After that, every agent inherits the accumulated knowledge of every mistake the team has made. That's a compounding advantage that scales with time, not headcount.
