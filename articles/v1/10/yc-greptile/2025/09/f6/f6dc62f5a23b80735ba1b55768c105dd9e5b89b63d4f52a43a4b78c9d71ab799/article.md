---
schema_version: "1.0.0"
document_id: "f6dc62f5a23b80735ba1b55768c105dd9e5b89b63d4f52a43a4b78c9d71ab799"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/series-a"
published_at: "2025-09-23T00:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:c051dd3451921007089c93bcd174159582f9c9e3dd5e9384d984d66a7a867f9a"
---

# Series A and Greptile v3

We've raised a $25M Series A to Kill The Bug. The round is led by Benchmark Capital with continued support from Cory Levy, YC, and Initialized Capital.


## Building the independent, centralized validation layer for code


Increasingly, code is written collaboratively by humans and a variety of coding agents like Devin, Claude Code, and Cursor.


The result has been two-fold:


- We are writing more code than we have before
- Our systems for validating that code before it is deployed are failing to scale with that growth


At Greptile, we’re building the independent and universal code validation layer.


Today, that looks like an AI code review agent that checks PRs for bugs and enforced coding standards across software orgs.


This month alone, Greptile reviewed 500M+ lines of code for top companies like Brex, Substack, PostHog, Bilt, and even YC's internal software team, helping prevent 180,000+ bugs.


## Introducing Greptile v3


Alongside our Series A, we're introducing Greptile v3, a complete rewrite of our core agent architecture capable of catching 3x more critical bugs than Greptile v2.


Greptile v3 outperforms every other code reviewer we tested at catching bugs. Some of our customers including Brex have had early access to v3 for a few weeks and were surprised by how much better it is.


Here are the five new features we're most excited about. Some big, some small, hopefully all impactful.


## 1. Learning


Greptile now learns your team's best practices and coding standards by reading other engineer's comments in GitHub/GitLab.


Here are some real-world examples of things Greptile has learned from human comments. Note that learnings are isolated to organizations for privacy.


**Database & Transactions**


- Do not return NotFound errors before authorization checks, as this exposes information about resource existence to unauthorized users. Instead, perform authorization queries that return null/forbidden when the resource doesn't exist or user lacks access.
- Always let PostgreSQL handle ID creation when creating database records. Never generate IDs outside of the database.


**Logging & Error Handling**


- Do not use f-strings in log lines. Instead, attach dynamic values as log properties using keyword arguments to the logger methods.
- Before throwing ApiError objects in server API handlers, log meaningful context with a` logPrefix` to provide debugging context that will be lost once the error reaches` handleApiError` .


**Type Safety & Validation**


- Use discriminated unions in TypeScript and Zod for type-safe pattern matching when you have objects with different shapes based on a discriminator field.
- Prefer` @ts-expect-error` over` as any` when intentionally bypassing TypeScript type checking in test files.` @ts-expect-error` will produce a compiler error if the type issue is later resolved, while` as any` will silently remain.


**Security & Secrets**


- Use whitelist-only approaches for security validation instead of blacklists. Avoid maintaining exclude/blacklists for security-critical code as they require manual updates when new threats are introduced and can break automatic dependency updates.
- When querying polymorphic relationships (tables with both` resource` and` resource_id` columns), always filter on both the resource type and the resource ID to prevent incorrect results when different resource types share the same ID.


**Frontend & UI Patterns**


- Remove manual icon sizing classes (like` className="w-4 h-4"` ) when using RoundedButton component, as it handles icon sizing automatically through its variant system.
- Use theme tokens (like` bg-muted` ) instead of hard-coded hex colors (like` #F8F9FB` ) for hover backgrounds to maintain theme-awareness and support dark mode.


**Testing & Tooling**


- Break down comprehensive test methods into separate focused tests using pytest parametrization. Avoid single 'all or nothing' tests that cover multiple behaviors.
- Use freezegun when testing timestamp or datetime functionality to prevent flaky tests due to timing delays.


## 2. Greptile MCP Server


We're also rolling out the Greptile MCP server. There's a lot you can do with it but my two favorite things are:


1.


Asking your coding agent/IDE to automatically grab and resolve Greptile's comments.


2.


Letting your coding agent/IDE access all the team-specific rules and best practices that Greptile has learned from human PR comments.


## 3. Integrations with Jira, Notion and more


Greptile now integrates with your business apps like Jira and Notion to provide more context-aware feedback on your PRs.


For example, Greptile can automatically fetch the related JIRA ticket during a PR review and check if the PR meets its requirements.


## 4. Auto-detect CLAUDE.md, .cursorrules, etc.


Increasingly, teams are using` CLAUDE.md` ,` .cursor/rules` , etc. to document their best practices. Instead of pushing a new standard, Greptile auto-detects all the common file patterns and automatically pulls them into context to make its PR feedback more personalized.


## 5. Copy prompt for your coding agent


Every Greptile comment optionally comes with a copyable prompt for your coding agent with relevant context from the codebase so you can easily fix issues that Greptile raises in the PR.


You can also use the Greptile MCP to access these prompts within your coding agent, without ever leaving your environment.


Interested in trying Greptile? You can create a free account at[app.greptile.com](https://app.greptile.com/) or[reach out to us](https://www.greptile.com/contact) .
