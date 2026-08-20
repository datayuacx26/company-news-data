---
schema_version: "1.0.0"
document_id: "0ac022c60b54321c21b42e6cd84183e90921c2299b664956ea698f25f26262e4"
company_key: "yc-diversion"
company: "Diversion"
source_id: "yc-diversion-news-import-29544632d9e8"
canonical_url: "https://www.diversion.dev/blog/the-conversation-behind-the-commit"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-21T16:38:14.982062+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:c4095eb9c0672c5cb968d1dd414d4070e954c814d785d782331a27b39b07ccfb"
---

# The conversation behind the commit

Here's a scenario that comes up constantly in game development: you're three months into a project, you open a file, and a chunk of logic looks completely wrong. Or inexplicable. Or both. You go to the blame, you find the commit, and the message says something unhelpful like "fix animation state machine." Then you're stuck. The person who wrote it is heads-down on something else, left the studio, or honestly just doesn't remember.


That information gap is nothing new. Documentation was supposed to solve it and mostly never did.


What's changed is that more developers are now using AI assistants while they work. Not to hand off authorship. To move faster on build scripts, to debug a particularly nasty gameplay system, to work through a refactor without losing context. The AI is a tool, same as a profiler or a node graph. The developer is still driving.


But here's the problem those sessions create: the reasoning lives in the chat. What you ruled out and why. The constraint you were working around. The alternative approach you tried first. When the session closes, that's gone. The commit lands in the repo and the thinking behind it evaporates.


The Diversion plugin for Claude Code is built to stop that from happening.


‍


## How it works


Install the plugin and it runs quietly in the background. As you work in Claude Code, it checkpoints your conversation and links it to the commit it produces - automatically, with no new step to remember. Nothing about your workflow changes.


The payoff comes later. Capture starts the moment you install, so the sooner it's running, the more history you'll have to query later.


‍


## /diversion:ask


` /diversion:ask` answers plain-language questions about your codebase by combining three sources: the code itself,` dv` blame and history, and the captured conversations behind each commit.


An isolated analyst sub-agent does the work behind the scenes and returns a single synthesized answer. The raw transcripts never touch your active session - you get the context without the noise.


You can ask about a specific commit, a file, a feature, a line range, or a window of time:


` /diversion:ask What was the reasoning behind commit dv.commit.41508?


/diversion:ask Why did we change the damage calculation in Source/Combat/DamageSystem.cpp last month?


/diversion:ask Why does ApplyKnockback() ignore the mass parameter for ragdolls?


/diversion:ask What was the intent behind the asset streaming refactor in Content/Levels/ before I change it?


/diversion:ask Summarize the AI-assisted work on the build pipeline over the last two weeks.


/diversion:ask What conversation produced the change that introduced the physics jitter in the vehicle controller?


`


## Getting the most out of it


A few things that make` ask` more useful in practice:


- **Ask in the language of the problem, not the file.** "Why did we disable interpolation in the animation system" works better than "what changed in AnimationComponent.cpp."
- **Anchor when you can.** A commit ID, file path, or line range helps the analyst zero in fast.
- **Reach for it before refactoring AI-assisted code.** If you didn't write a chunk in a single focused session, there's a decent chance the important context is buried in a commit transcript.
- **Use it in code review.** Reviewers can ask why a change was structured the way it was, without interrupting the author mid-sprint.
- **Keep the thread going.** Follow-up questions build on each other and stay coherent within the same session.
‍


## Try it


` /plugin marketplace add DiversionCompany/diversion-claude
/reload-plugins
/plugin install diversion@diversion


`


[Setup instructions and source](https://github.com/DiversionCompany/diversion-claude) are on GitHub.


‍
