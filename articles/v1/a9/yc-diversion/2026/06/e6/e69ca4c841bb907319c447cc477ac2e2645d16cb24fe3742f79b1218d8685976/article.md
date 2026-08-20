---
schema_version: "1.0.0"
document_id: "e69ca4c841bb907319c447cc477ac2e2645d16cb24fe3742f79b1218d8685976"
company_key: "yc-diversion"
company: "Diversion"
source_id: "yc-diversion-news-import-29544632d9e8"
canonical_url: "https://www.diversion.dev/blog/your-ai-sessions-are-doing-work-your-repo-cant-explain"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-21T16:38:14.982062+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:901f640aee32785e449fc918111db2744e395b7342b4b897937f9617b4727ea2"
---

# Your AI sessions are doing work your repo can't explain

‍


More of the code being committed today was written with an AI assistant in the loop. That's not a prediction anymore. If you're using Claude Code or any similar tool with any regularity, a meaningful chunk of your recent commits have a conversation behind them - a session where you worked through an approach, ruled something out, hit a constraint, decided on a direction.


That session closed. The reasoning is gone. The commit is in the repo, the diff is reviewable, but the thinking that produced it is nowhere.
‍


## How it works


This is a new version of an old problem. We've always lost context between the moment someone made a decision and the moment someone else needed to understand it. What's different now is the volume and the speed. Teams are shipping more code with AI assistance, the sessions that produce it are ephemeral by design, and the gap between "what changed" and "why it changed" keeps widening.


The Diversion plugin for Claude Code is a direct answer to that gap.


Install the plugin and it runs in the background as you work in Claude Code. It checkpoints your conversation and links it to the commit it produces - automatically, with no new step and no habit to build. You don't trigger anything; the plugin watches the work happen and does the linking for you.


Capture starts the moment you install, so the sooner it's running, the more history you'll have to query later.
‍


## /diversion:ask


` /diversion:ask` answers questions about your codebase in plain language, using three sources: the code itself,` dv` blame and history, and the captured conversations behind each commit.


An isolated analyst sub-agent does the work behind the scenes and returns a single synthesized answer. The raw transcripts never enter your active session - you get the context without flooding your context window.


Ask about a specific commit, a file, a feature, a line range, or a window of time:


` /diversion:ask What was the reasoning behind commit dv.commit.41508?


/diversion:ask Why did we change the retry logic in core/src/sync/engine.py?


/diversion:ask What was the reasoning behind how parseConfig() handles missing fields?


/diversion:ask What was the intent behind the caching layer in payments/ before I refactor it?


/diversion:ask Summarize last week's AI-assisted work on the auth subsystem.


/diversion:ask What conversation produced the change that broke checkout?


`


## Getting the most out of it


A few things that make` ask` more useful in practice:


- **Ask in the language of the problem, not the file.** "Why did we switch from optimistic to pessimistic locking in the session handler" beats "what changed in[session.py](http://session.py/) ."
- **Anchor when you can.** A commit ID, file path, or line range helps the analyst focus and speeds up the answer.
- **Reach for it before refactoring AI-generated code.** The transcript behind the original implementation often contains exactly the constraints the refactor needs to respect.
- **Make it part of your review process.** Reviewers can ask what produced a change — without pulling the author out of flow to explain it.
- **Keep the thread going.** Follow-up questions stay coherent within the same session. You can drill down without losing context.
‍


## You don't have to migrate


On GitHub? Install Diversion alongside your existing setup - your remotes, PRs, and CI don't change. It's purely additive: the work you do from today becomes queryable, and the sooner you start, the more reasoning you'll have to draw on later.


‍


## Try it


` /plugin marketplace add DiversionCompany/diversion-claude
/reload-plugins
/plugin install diversion@diversion


`


[Setup instructions and source](https://github.com/DiversionCompany/diversion-claude) are on GitHub.


‍
