---
schema_version: "1.0.0"
document_id: "c262ebdbf4c4a88d76fa1ec0ec50a8e04538fc7183de8b0142dbbb830c782fe8"
company_key: "yc-aviator"
company: "Aviator"
source_id: "yc-aviator-rss-35144b27026a"
canonical_url: "https://www.aviator.co/blog/tackling-technical-debt-with-spec-driven-ai/"
published_at: "2026-04-09T10:11:00+00:00"
first_seen_at: "2026-07-20T23:23:35.507164+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:a3d85cf572e4f495a829abd38af26ec47afb58239886fc3c618775689499a059"
---

# Tackling Technical Debt with Spec-Driven AI

Technical debt has always been hard to measure, and even harder to eliminate. What’s changed is that AI has now become a part of the workflow. However, without guardrails, AI agents make things worse, not better.


They learn context from code around them, which means they inherit the debt too. A deprecated API present in enough files gets used again, and inconsistent patterns get replicated with confidence, at the speed of autocomplete.


The good news is that specs can fix this. They transform AI from a pattern replicator into a consistent, targeted contributor that moves your codebase toward a defined goal. This article explains how.


## What Is Spec-Driven AI Development?


Before an AI assistant even touches your codebase, you give it the necessary specifications to make sure it will operate within those specs. You define conventions, standards, constraints, and, just as importantly, what to avoid. This helps AI get a sense of what’s “right” and “wrong” as it builds context based on your code.


There are a few ways to do this. A standardized` agents.md` file is a popular one, and each assistant offers its own variation (` claude.md` ,` rules` ,` gemini.md` etc.). Regardless, they all serve the same purpose: to provide a persistent human written spec that gets included as context in every interaction.


The key lies in the definition of goals. Without specs, AI simply mirrors what it finds in the codebase, including the existing debt. But when you give it directions upfront (what’s right and what’s wrong), it starts building context around what a codebase is supposed to do rather than what it’s currently doing.


With a goal in mind, AI becomes a tool for reducing technical debt.


*The difference in workflows of development without specs versus spec driven development*


So where do specs fit? They have a place alongside other types of guardrails: linters, formatters, and CI checks. The difference is in timing: **specs** **change contributions before they get written** , instead of taking action after the fact.


At scale, specs become a fundamental unit of programming, and you can read all about it in this[deep dive on spec driven development](https://www.aviator.co/blog/spec-driven-development-the-key-to-scalable-ai-agents/) .


## Specs are the Missing Link in AI-Assisted Debt Reduction


Without specs, there’s as much (or as little, to be precise) consistency as in any codebase that has a history. A model could prioritize updating an error-handling pattern in one place and completely ignore it in the next. It might also extract a utility function that already exists somewhere else, not because it lacks capacity but because it lacks a goal.


Specs change the output by changing the input. When an AI assistant works within well-written specs, it has a very clear goal, constraints, and conventions. Output becomes repeatable and more aligned with your domain. By defining a target state within specs, every contribution moves you closer to that target, gradually reducing tech debt.


If you’re a platform engineer, this matters. Specs aren’t just a developer tool. They are infrastructure. A well-written spec enforces conventions and prevents deprecated patterns from re-emerging. It’s doing the same job as a linter rule, but at an even earlier stage.


Keep in mind that when specs span an organization, you should be ready to address ownership, versioning, and how to verify that they actually work. All this is covered in detail in[The Platform Engineer’s Guide to Enterprise AI Coding Tools](https://www.aviator.co/blog/the-platform-engineers-guide-to-enterprise-ai-coding-tools/) .


## Creating Effective Specs for Debt Reduction


A good spec consists of:


- well-defined target state
- clear scope
- explicit constraints
- a list of patterns to avoid


These components give an AI assistant enough context to make consistent and predictable decisions. An explicit example (before/after state) often guides the AI better than abstract rules alone.


Where do you start in debt reduction? You can make decisions based on the impact of each known debt: how often it appears in the codebase, how much friction it adds to day-to-day workflow, and similar. The debt that scores high across all dimensions is the best starting point.


Note that these dimensions also make the process measurable since you have identified the current state and can now track the improvements.


*A decision matrix to determine what parts of tech debt to focus on first, offsetting impact agains frequency.*


When writing specs that should hold up over time, it’s good practice to focus on the patterns rather than individual files or function levels. Version your specs alongside the code they govern, so that code and specs ship together.


And remember: **the spec is part of the code.**


## Integrating Spec-Driven AI Into Your Existing Development Workflow


First of all, the goal isn’t to overhaul how a team currently works. It’s to introduce specs at points in the workflow where they add value with the least friction. In practice, this usually means at local development, on PRs, and in CI/CD pipelines. Local agents tap into specs before making suggestions or autocompletes. In addition, specs define the standard during PR processes, and CI/CD checks validate changes before and after they are integrated with the main branch. The process doesn’t change, it just introduces new standards.


Individual developers (or teams) take ownership of local specs, like the` agents.md` type of files. Collaboration is possible, but these are not the organization-wide specs. Those that apply on a shared level are owned by platform engineers to ensure consistency across teams.


For storing and sharing specs across teams,[Aviator Runbooks](https://www.aviator.co/runbooks) are a natural fit. Rather than having specs as static` agents.md` files across repos, Runbooks become the shared space to collaborate and iterate on specs, with clear versioning, audit trails of decisions, and reusability across your organization.


A Runbook that targets a specific debt (say,[migrating from a deprecated software package](https://www.aviator.co/blog/migrating-from-node-js-14-to-18-a-vibe-coding-vs-spec-driven-walkthrough/) or using LTS standards), can be authored once and executed consistently across every affected repository. Each step generates a reviewable PR. This level of collaboration and control is what makes[spec driven development](https://thenewstack.io/spec-driven-development-the-key-to-scalable-ai-agents/) viable at organizational scale.


*A Runbook targeting a Node.js LTS migration: authored once, executable across every affected repository. Each step generates a reviewable PR*


## How to Measure Technical Debt Reduction with Spec-Driven AI


Tech debt has always been hard to measure. It’s subjective and tends to show up indirectly as slower velocity, time spent in review, or even a growing reluctance to touch parts of the codebase (a major red flag!). You can track these markers and watch how they change over time.


Specs help trace tech debt because they directly address patterns and, more importantly, anti-patterns. If a spec targets a specific anti-pattern, it becomes easy to see how many times it pops up and where. Even fuzzy patterns are not hidden from AI assisted tools. **When a spec defines a target state, you can measure your distance from it.**


Beyond pattern tracking, it’s worth mentioning the other signals that can give a good indication of a debt reduction journey together. You can measure things like time-to-refactor per module, PR review cycles, or overall velocity and even compare areas of the codebase with specs applied versus those without.


Let these measurements guide you. If results vary too much, the specs are probably too broad. If they produce consistent refactors, they are probably too narrow (or outdated). Making an effort to create visibility on your tech debt is the best tool for reducing it over time.


*Specs improve through usage and evaluation. Don’t expect them to be perfect at first write. Measuring tells you which direction to refine*


## Avoiding Common Spec-Driven Development Pitfalls


Specs are only useful if they are accurate, properly scoped, and well-maintained. Most teams that struggle with spec-driven development (AI-assisted or not) run into similar problems.


- **Specs that are too broad or too narrow:** Broad specs are easy to maintain but give an AI plenty of room for interpretation. Unsurprisingly, the opposite is true for specs that are too narrow. The sweet spot is defining intent plus constraints. Make specs clear enough to become predictable and flexible enough so that they don’t need a rewrite, change, or polish with every contribution.
- **Outdated or stale specs:** An outdated spec is worse than no spec at all. It actively steers your AI assistant in the wrong direction at every turn. Specs are living documents and need to be treated as such. If a refactor or migration makes a rule obsolete, the corresponding spec should be updated or closed in the same PR.
- **Skipping the review process:** Spec-driven development doesn’t mean you get to take the hands off the steering wheel. All contributed code should pass a human review. The goal is not to replace humans but to have them spend their time on where it matters. The same applies to any change in specs.
- **Treating specs as one-time documents:** This might be the most common pitfall: a spec that gets written but never revisited over time. As a result, it starts to drift from the codebase. The AI will not forget about it and will keep applying it, although the standard has already moved past it at this point. Specs need an ownership model, just like any other part of code and infrastructure. They need a review rhythm and assessment on value.


## Specs as the Missing Guard Rails in AI Development


Spec-driven development doesn’t represent a technical shift only. It’s also a change in mindset. Specs should be treated as infrastructure, not documentation only. In addition, technical debt should be measured as a gap between the current state and your defined target. You can let AI handle the repetitive work of closing that gap consistently across every file it touches.


Platform engineers need to verson, own, and maintain specs, just like any other critical system. Their impact also needs to be measured, and the insights should be fed back into the system. As for individual developers, they need to reach for a spec before writing a prompt.


The tools and practices are still evolving, but the foundation is solid. Aviator’s[Spec-Driven Development: The Key to Scalable AI Agents](https://www.aviator.co/blog/spec-driven-development-the-key-to-scalable-ai-agents/) and their[Runbook Library](https://www.aviator.co/runbooks) are pretty good first stops. They contain and reference ready-made templates for common debt categories, and Aviator supports a platform built around making spec-driven development a collaborative process.


From there, the patterns in this article give you everything you need to start.


## Frequently Asked Questions


### What is spec-driven AI development?


Spec-driven AI development means giving an AI coding assistant explicit specifications (conventions, standards, constraints, and patterns to avoid) before it touches your codebase. This ensures the AI operates toward a defined target state rather than replicating existing patterns, including technical debt.


### How does AI make technical debt worse without specs?


Without specs, AI coding assistants build context from the surrounding code, which means they inherit existing debt. Deprecated APIs present in enough files get reused. Inconsistent patterns get replicated at the speed of autocomplete. Specs prevent this by defining what “right” and “wrong” looks like before any code is written.


### What is an agents.md file and how does it reduce technical debt?


An` agents.md` file is a persistent, human-written spec that gets included as context with every AI interaction. Variations include` claude.md` ,` gemini.md` , and assistant-specific rules files. They all serve the same purpose: giving the AI a consistent goal so every contribution moves the codebase closer to a defined target state rather than reinforcing existing debt.


### How do specs differ from linters and CI checks for managing technical debt?


Linters, formatters, and CI checks act after code is written. Specs change contributions before they get written. This shifts the cost to the left in the development workflow, reducing the number of iterations and context switches needed to reach a clean result.


### What makes a spec effective for technical debt reduction?


An effective spec includes a well-defined target state, clear scope, explicit constraints, and a list of patterns to avoid. Before-and-after examples give an AI better guidance than abstract guidelines. Specs should be written at the pattern level, not the file or function level, and versioned alongside the code they govern.


### How do you measure technical debt reduction with spec-driven AI?


Because specs target specific patterns and anti-patterns, you can directly track how often those patterns appear across the codebase over time. Broader signals include time-to-refactor per module, PR review cycles, and overall development velocity. By comparing parts of the codebase with an applied spec versus those without it, you get a concrete view of progress
