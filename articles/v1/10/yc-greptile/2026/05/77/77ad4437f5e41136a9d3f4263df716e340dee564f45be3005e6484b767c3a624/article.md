---
schema_version: "1.0.0"
document_id: "77ad4437f5e41136a9d3f4263df716e340dee564f45be3005e6484b767c3a624"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-7ca8d0432254"
canonical_url: "https://www.greptile.com/content-library/code-review-checklist"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-08-12T03:54:42.000307+00:00"
fetched_at: "2026-08-12T03:54:43.717460+00:00"
content_hash: "sha256:9e054d4bb7098bca1a764ea6332e35d422cdfb734c29956181e22c3e2fd0ec0f"
---

# How to ACTUALLY Think About Code Reviews: The 3-Layer Checklist

Before you approve that next PR, use this checklist. It's organized around three layers that separate what machines should catch from what requires human judgment. It's a code review checklist any team can run on every PR to improve code quality without slowing reviews down.


Layer 1: Mechanical


These should never require human review time. If you're checking these manually, your tooling has failed.


Code Formatting


-
-


Testing (Automated)


-
-
-


Documentation (Basic)


-


Set up pre-commit hooks, linters, and CI/CD to catch all of these before opening a PR.


Layer 2: Structural


These questions require understanding the codebase, architectural patterns, and long-term consequences.


Architecture & Design


-
-
-
-


Performance


-


Best Practices


-
-
-
-
-
-


Maintainability


-
-
-


Testing (Structural)


-
-


Layer 3: Narrative


Without this layer, reviewers waste time reconstructing context that should have been provided upfront.


Requirements & Context


-
-


Documentation & Reasoning


-
-


Testing (Context)


-


## Code Review Examples


Greptile reviews code in production across major open-source projects. Here's what it catches:


**Storybook** (88k stars) → memory leak from an unclosed readline interface.[View PR](https://github.com/storybookjs/storybook/pull/30070#discussion_r1886403614)


**PostHog** (30k stars) → graph traversal logic error causing nodes to be skipped.[View PR](https://github.com/PostHog/posthog/pull/30972#discussion_r2034194672)


**NVIDIA PhysicsNeMo** → incorrect precision calculations in active-learning example.[View PR](https://github.com/NVIDIA/physicsnemo/pull/1174#discussion_r2457571950)


**Mastra** (17k stars) → multi-slash model ID parsing extracting the wrong provider.[View PR](https://github.com/mastra-ai/mastra/pull/8235#discussion_r2392311989)


**Raycast** (6.7k stars) → unhandled` writeFileSync` crash.[View PR](https://github.com/raycast/extensions/pull/20521#discussion_r2216451131)


These are structural issues that traditional linters can't catch. See more examples at[greptile.com/examples](https://www.greptile.com/examples) .


## Why The Checklist Is Organized This Way


You receive a PR. Eighty-six files changed. You know you're supposed to review this thoroughly (that's your job!), but you have four meetings today, two bugs to fix, and your own deadline looming.


So you skim. You leave a few comments on the variable names, the unnecessary whitespace you hate, and recommend a few lines to log more effectively. The PR merges and everyone moves on, but something nags at you. Did you actually review the code? How long are you supposed to read it?


Heck, what even counts as a "good" code review? How can you do that?


Well, the problem is: *your team has never agreed on what a code review is actually for.*


This is not a process problem; this is a clarity problem, and clarity begins by understanding what you're looking at.


When you open a pull request, you're not looking at a blob of code to review; you're looking at three layers stacked on top of one another. We'll give you a mental model to identify these layers, deal with them correctly, and take your code reviews to the next level.


## The 3-Layer Mental Model


### Layer 1: Mechanical — what machines should catch


This is formatting. Linting. Whether tests pass. Code style. Import order. It's everything a computer can verify without understanding what your code *does* .


Here's the thing: *every minute a human spends reviewing this layer is a waste.*


If someone leaves a comment saying "can you add a space here" or "this should be const, not let," that's not code review.


### Layer 2: Structural — what determines long-term changeability


This is architecture. You review how the pieces fit together, whether this new function tangles dependencies or keeps them clean, whether the abstraction makes sense, and whether naming reveals intent or obscures it.


This is what should take up 80% of your time during a code review.


This is *hard* . It requires understanding both the code and the system it lives in. It requires taste, experience, and the ability to imagine futures — "if we do it this way, what happens when we need to add X?"


A junior engineer can catch mechanical issues. Only senior engineers who've lived through architectural pain can catch structural ones.


### Layer 3: Narrative — what the code change means for the project


This is the *why* . Why this approach? Why now? What alternatives were considered? What's the migration path? What edge cases exist? What's the plan if this doesn't work?


Most of this doesn't live in the code itself — it lives in the PR description, in Slack threads, and in the reasoning behind decisions.


This is where you communicate your intent for the changes made, and this is where you get to make decisions that live forever, since this is where you set your standards.


A PR with no narrative context adds to the burden of reviewing. The reviewer has to reconstruct the reasoning from scratch. They ask questions that should have been answered upfront, and approve changes they don't fully understand because they're exhausted from detective work.


A strong PR narrative defines your engineering team's communications culture.


Once you see these three layers, the rest of the code review finally makes sense. Let's make this practical.


## Code Review Best Practices


Code review best practices aren't just a list of rules — they're tactics for improving code quality at each layer. Every best practice you've heard is solving a problem in one of the three.


**Layer 1 (Mechanical)**


1. Automate style checks → removes mechanical noise from human review, saving time.
2. Use pre-commit hooks → catches mechanical issues before PR creation.


**Layer 2 (Structural)**


1. Keep PRs small → makes structural impact easier to reason about.
2. Review architecture first → catches structural issues before implementation details.
3. Senior engineers should review critical paths → matches expertise to structural complexity.


**Layer 3 (Narrative)**


1. Write detailed PR descriptions → provides narrative context upfront, saves reasoning time.
2. Explain the *why* , not just the *what* → strengthens the narrative layer for the entire team.
3. Link to related issues / docs → enriches the context for engineers to explore.


See the pattern? When you know which layer you're operating in, best practices stop being random advice and start being strategic tools.


## Secure Code Review Lives in Layer 2


Most teams treat secure code review as a separate process. That's a category error. It's a Layer 2 question you ask on every PR.


Vulnerabilities live in the structural layer. They show up in how functions relate, what they trust about their callers, and what state they leave behind. Linters don't catch them (Layer 1). PR descriptions don't explain them (Layer 3). They sit exactly where you should already be reviewing.


Five secure code review questions worth asking on every Layer 2 review:


- Does this change widen the attack surface? (new endpoints, external inputs, file or shell access)
- Are inputs validated *and* sanitized at the boundary they cross, not assumed-clean later?
- Are auth and permission checks running at the layer that owns the rule, not duplicated three places?
- Could this change leak sensitive data into logs, error messages, or response bodies?
- If this code panics or throws, what state does it leave behind?


A senior reviewer doing structural review is doing secure code review. Splitting them into two passes wastes time and creates the gaps where vulnerabilities slip through.


But here's where it gets interesting: some structural review is actually automatable now with[AI code review](https://www.greptile.com/what-is-ai-code-review) .


## The Greptile Advantage


Traditional tools can't help with structural review because they analyze files in isolation. But Greptile builds a complete graph of your codebase — every function, every dependency, every call site.


When you change a function, Greptile instantly knows:


- What patterns exist elsewhere in your codebase
- Where this function is called and what impact changes will have
- Whether you're breaking consistency with similar functions


Greptile catches the coupling issues, pattern inconsistencies, and architectural drift that usually require a senior engineer to spot, freeing them to focus on the truly nuanced judgment calls.


And the best part is that Greptile understands the 3-layer framework introduced above. Greptile categorizes its findings into three types that map directly to the layers you should be focusing on:


1. **Syntax** — catches syntax errors and code that won't compile or run.
2. **Logic** — focuses on faulty logic, architectural inconsistencies, and pattern violations that cause unexpected behavior or crashes.
3. **Style** — highlights suggestions for improvement and team best practices.


You can control which comment types Greptile leaves in your dashboard.


This checklist works alongside the linters, type checkers, and code quality tools your team already uses. It's the human layer they can't replace.


The right code quality metrics aren't lines of code reviewed or comments left. They're issues caught before merge, time saved on Layer 1 noise, and how often Layer 2 catches result in real architectural change. Greptile gives you those metrics directly.[Try Greptile →](https://www.greptile.com/)
