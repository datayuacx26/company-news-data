---
schema_version: "1.0.0"
document_id: "87be584d40ccd5a36f8d91e55fc8c23e62d186cc679682eda68f1c5bb52428dc"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/spec-driven-development-ai"
published_at: "2026-05-25T12:17:11+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:c9eee6cf54181831c7888a05efefa0050c888f9157a2e35d84e4726d8069c556"
---

# Spec-Driven Development: Write the Spec First, Let AI Build It

## The 7-Section Spec Template


Developer at whiteboard showing before-and-after: vague sticky note on left vs structured spec document with green checkmarks on right


Blink


*A vague brief leaves architecture decisions to the agent — a detailed 7-section spec leaves none*


Every reliable spec has seven sections. Each one removes a class of agent guessing.


**1. Problem Statement** — 2-3 sentences on the user pain this feature solves. "Users can't reset their password without contacting support, creating 3-4 support tickets per day." This keeps the spec grounded in real behavior, not technical abstraction.


**2. Solution Overview** — What you're building and what you are NOT building. "Email/password auth with password reset. Not OAuth. Not SSO. Not 2FA — future feature." The "not building" list is as important as what you are building.


**3. User Stories** — "As a \[user type\], I want to \[action\] so that \[outcome\]." Write 3-5 maximum. More than that and you're writing requirements docs, not a spec.


**4. Technical Requirements** — Data models with column types, API endpoints with request/response shapes, UI screens with actions and navigation. This is the longest section. It's also where most developers skip too many details.


**5. Edge Cases** — What happens when things fail. Expired tokens, duplicate emails, rate limit exceeded, empty states, network errors. Name them explicitly. Every unspecified edge case becomes an agent decision — usually a generic 500 error.


**6. Testing Requirements** — Which scenarios must have tests. Happy path, auth failure, rate limiting, token expiry, concurrent requests. Agents write significantly better test coverage when you name scenarios rather than leaving coverage to their judgment.


**7. Out of Scope** — An explicit list of what the agent should NOT implement. This section alone eliminates most scope creep. If something isn't in the spec but the agent thinks it "makes sense," the instruction is: stop and ask, don't build.


## How to Write a Spec in 20 Minutes


1


#### Write the problem statement first (3 min)


One paragraph. What user pain does this solve? What's the current workaround? Be specific: "Users manually copy data between two spreadsheets every Monday, which takes 45 minutes and introduces errors." This sentence anchors every decision in the spec that follows.


2


#### Define the out-of-scope list before the data model (5 min)


Before you write what you're building, write what you're not. This step surfaces scope assumptions before you've spent time speccing features you didn't want. List 3-5 things the agent might reasonably include but shouldn't: no OAuth, no mobile app, no email notifications, no audit log.


3


#### Write the data model (7 min)


List every table the feature touches. For each: table name, column names, column types, constraints, relationships. Example:` User: { id UUID PK, email VARCHAR(255) UNIQUE NOT NULL, password_hash VARCHAR NOT NULL, verified_at TIMESTAMP, created_at TIMESTAMP }` . If you can't write the data model, stop — you don't yet understand the feature well enough to build it.


4


#### List API endpoints or UI states (3 min)


For each endpoint: HTTP method, path, request body, response shape, auth requirement. For each UI screen: what it displays, what inputs exist, what each action does. Be specific —` POST /auth/login { email, password } → { access_token, expires_in }` , not "a login endpoint."


5


#### Add edge cases and test requirements (2 min)


Name the failure modes: invalid input, user doesn't exist, token expired. List which scenarios need tests. This step takes 2 minutes and prevents an entire class of post-build bugs.


## Using the Spec With Claude Code and Cursor


Save the spec as` SPEC_\[feature\].md` in your project root. One file per feature.


Open a new Claude Code session and reference it directly:


> "Read` SPEC_auth.md` and implement it. Start with the data model, then the API layer, then the UI. Do not deviate from the spec — if anything is ambiguous, ask before implementing."


The agent reads the entire spec, flags gaps, then builds. Review the output against the spec — not against your memory of what you wanted. The spec is the contract.


For Cursor, add` SPEC_auth.md` to your workspace root and reference it in Composer:


> "@SPEC_auth.md implement this feature using plan mode first — show me the proposed changes before executing."


When requirements change mid-build, update the spec file first. Describe changes in the updated file, not in chat. Chat context clears. The spec persists.


When the agent proposes something not in the spec, you have two choices: update the spec to include the change (if the approach is genuinely better), or reject the change. Never let undocumented deviations accumulate — they compound into architectural drift.


This is the feature-level equivalent of[CLAUDE.md](https://blink.new/blog/claude-md-best-practices) . The spec governs what gets built for one feature; CLAUDE.md governs how the agent works across the project. Both belong in version control.


## Spec-Driven Development at Scale: Multiple Features


As your project grows, one spec becomes many.


Create a` /specs` directory at the project root. Each feature gets its own file:` SPEC_auth.md` ,` SPEC_billing.md` ,` SPEC_notifications.md` . Add a` SPECS.md` index with a one-line summary per feature.


Developer at futuristic desk with floating /specs/ directory tree, AI agent panels each picking up a different spec and building features in parallel


Blink


*Each feature gets its own SPEC.md — parallel agents work from independent, complete specifications*


When you start a new agent session: "Read` SPECS.md` for context, then read` SPEC_billing.md` and implement it."


When a feature is complete, the spec becomes living documentation. Future agents read it to understand what was built and why. Future developers read it when something breaks. The spec captures decisions made before the code existed — the reasoning that never appears in git history.


When a spec grows past 4 pages, split it.` SPEC_auth.md` becomes` SPEC_auth_signup.md` and` SPEC_auth_permissions.md` . Smaller specs are easier to review, easier to execute, and easier to keep accurate.


Start with one spec for your current feature. Once you've run the workflow twice, verbal instructions will feel like working without tests.


For a broader view of how specs fit into the full[agentic coding workflow](https://blink.new/blog/agentic-coding-best-practices) , the best practices guide covers the complete picture — context engineering, tool use, and verification rituals that work alongside spec-driven execution.


## Build Spec-Driven Apps With Claude Code on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Implement the feature in SPEC.md and host the app on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Long enough that the agent has no architecture decisions left to make. A simple CRUD feature: 1-2 pages. A full auth system: 2-4 pages. The test: read the spec and see if you can determine every column type, every endpoint's response shape, and every validation rule. If you can't, the spec isn't done.


Not for single-file edits or obvious bug fixes. Write a spec when the feature touches more than two files, requires a data model, or involves multiple API endpoints. The break-even point is roughly 30-45 minutes of implementation time — if it would take that long to build, a 15-minute spec saves more time than it costs.


A prompt is a request. A spec is a contract. A prompt says "add user authentication." A spec defines the data model, exact endpoints, validation rules, error messages, test scenarios, and an explicit list of what not to build. Prompts leave decisions to the agent. Specs don't. The[agentic coding guide](https://blink.new/blog/what-is-agentic-coding) covers the full mental model shift.


They work at different scopes. CLAUDE.md is project-level context: your stack, conventions, how to run tests, general preferences. A spec is feature-level: what this specific feature does, its data model, its API contract, its edge cases. Both are read by the agent. Both belong in version control.


Update the spec file first. Always. Start a new agent session referencing the updated spec. Never describe changes only in chat — that context disappears when the session ends. Version-controlled specs mean you can diff what changed, understand why it changed, and track when decisions were made.


Yes. Add your spec to the workspace and reference it with` @SPEC_\[feature\].md` in Composer. Use plan mode to review proposed changes before execution. Pin the spec to context for every follow-up conversation. The workflow is identical in principle to Claude Code — the spec is just attached differently.
