---
schema_version: "1.0.0"
document_id: "0176ea8e52a67b4b1b82d16b0f6b7459ecd00c3ab261f383f3fbe12dd58c0455"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-rules-guide"
published_at: "2026-06-02T12:20:42+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:74920610f5d3fe9b19655fe8eb08dc2f445815255ed6c325f43f7279b991371f"
---

# Cursor Rules: How to Write .cursorrules and .mdc Files That Actually Work

## The Four Activation Modes


Activation is controlled by three frontmatter fields:` alwaysApply` ,` description` , and` globs` . The combination determines when the rule fires:


Always active — fires on every session


```text
---
alwaysApply  :   true
---


-   Use TypeScript strict mode; no implicit any
-   Never modify generated files in dist/ or build/
-   All exports must be named — no default exports
```


Auto-attached by glob — fires when matching files are in context


```text
---
globs  :   src/components/**/*.tsx
alwaysApply  :   false
---


-   Named exports only
-   Co-locate styles in a module CSS file next to the component
-   Keep components under 200 lines; extract subcomponents when a file grows
-   Use Tailwind for all styling
```


Agent-selected — AI reads description and pulls in when relevant


```text
---
description  :   API endpoint conventions and error handling patterns
alwaysApply  :   false
---


-   Define each endpoint in its own file under src/api/
-   Return { data } on success, { error: string } on failure — never throw raw strings
-   Validate all inputs with Zod before any business logic
-   Log errors with console.error before returning
```


Manual — only fires when @-mentioned in chat


```text
---
alwaysApply  :   false
---


-   Every database migration needs both up and down functions
-   Never alter a column type in-place; add new column, backfill, then drop old
-   Reference the migration template at @migration-template.sql
```


Choosing the right mode matters beyond organization:` alwaysApply: true` rules consume tokens on every single message. Keep always-active rules short — under 20 lines is a good guideline. Reserve longer, context-specific rules for glob or agent-selected activation where they only fire when actually relevant.


## Anatomy of a .mdc Rule File


The file structure is minimal: YAML frontmatter between` ---` delimiters, followed by plain markdown. The content is the rule itself — write it as clear internal documentation, not as a prompt.


```text
.cursor/rules/
always-on.mdc             # alwaysApply: true — short, global conventions
react-components.mdc      # globs: src/components/**/*.tsx
api-patterns.mdc          # description: API patterns — agent-selected
db-migrations.mdc         # Manual — @-mention only
testing.mdc               # globs: **/*.test.ts, **/*.spec.ts
```


Two ways to create rules:


- **` /create-rule` in Agent chat** — describe what you want, Cursor writes the` .mdc` file with correct frontmatter and saves it automatically
- **Cursor Settings → Rules, Commands →` + Add Rule`** — creates a new` .mdc` file directly in` .cursor/rules`


Check rules into git. When a teammate encounters a pattern the AI keeps getting wrong, they update the rule once — everyone benefits immediately. Cursor's best-practice guideline caps rules at 500 lines each; split large rules into multiple composable files rather than padding one.


## 10 Cursor Rules Worth Stealing


These cover the most common categories — framework preferences, naming conventions, testing, and project context.


.cursor/rules/typescript-standards.mdc


```text
---
alwaysApply  :   true
---


-   TypeScript strict mode; no implicit any
-   Prefer interfaces over type aliases for object shapes
-   Use const assertions for literal types
-   Export types alongside their implementations
```


.cursor/rules/react-components.mdc


```text
---
globs  :   src/components/**/*.tsx, src/app/**/*.tsx
alwaysApply  :   false
---


-   Named exports only — no default exports
-   Props interface defined at top of file
-   Co-locate tests as ComponentName.test.tsx next to the component
-   Tailwind for all styling; no inline style objects
```


.cursor/rules/api-routes.mdc


```text
---
globs  :   src/app/api/**/*.ts
alwaysApply  :   false
---


-   All routes must validate auth before processing
-   Return { data } on success, { error: string } on failure
-   Never return stack traces to the client
-   Log errors with console.error before returning
```


.cursor/rules/testing.mdc


```text
---
globs  :   *  *  /*.test.ts,   *  *  /*.test.tsx,   *  *  /*.spec.ts
alwaysApply  :   false
---


-   Use describe blocks for grouping related tests
-   Test file names match the file they test
-   Mock external services; never make real HTTP calls in tests
-   Cover happy path, error path, and one edge case per function
```


.cursor/rules/naming.mdc


```text
---
alwaysApply  :   true
---


-   Components: PascalCase (UserProfile.tsx)
-   Utilities: camelCase (formatDate.ts)
-   Constants: SCREAMING_SNAKE_CASE
-   Database columns: snake_case
-   CSS classes: kebab-case
```


.cursor/rules/error-handling.mdc


```text
---
alwaysApply  :   true
---


-   All try/catch blocks must console.error before returning
-   Never swallow errors silently
-   Use early returns instead of deeply nested if/else
-   Return typed error objects, not raw error strings
```


.cursor/rules/imports.mdc


```text
---
globs  :   src/**/*.ts, src/**/*.tsx
alwaysApply  :   false
---


-   Use @/ path aliases for all internal imports
-   Group: external libs → internal modules → local files
-   No circular imports between modules
-   Barrel exports (index.ts) for public module interfaces only
```


.cursor/rules/git-commits.mdc


```text
---
alwaysApply  :   false
description  :   Git commit message and staging conventions
---


-   Conventional commits: feat:, fix:, chore:, docs:, refactor:
-   Subject line under 72 characters
-   Stage files explicitly by name — never git add -A
-   Reference issue numbers in commit body when applicable
```


.cursor/rules/database.mdc


```text
---
description  :   Database schema and migration conventions
alwaysApply  :   false
---


-   Every migration needs up and down functions
-   Never alter column types in-place; add, backfill, drop
-   Use snake_case for all column names
-   Add database-level constraints for required fields, not just app-level
```


.cursor/rules/project-context.mdc


```text
---
alwaysApply  :   true
---


-   Stack: Next.js 16, TypeScript, Tailwind, Prisma (PostgreSQL)
-   Auth: session cookies; never store tokens in localStorage
-   Deployment: Railway (not Vercel); environment variables in Railway dashboard
-   Never run npm run build; it will break the active dev server
-   Database: PG2 is the single source of truth; no writes to legacy PG1
```


The last example —` project-context.mdc` — is often the highest-value rule in any codebase. It encodes the architectural decisions that a new developer (or a fresh AI session) would otherwise discover through trial and error. One rule saves dozens of course-correction messages per week.


Before and after Cursor rules — from generic suggestions to project-aware assistance


Blink


## Add Blink to Your Cursor Setup


Rules make Cursor project-aware inside your codebase. But when your project needs a backend, database, auth, or deploy target, you're still manually wiring up separate services — or writing rules that tell the AI how to use them.


[Blink Cloud](https://blink.new/cloud) solves that layer directly. One command configures 62 MCP tools and 14 skills into your Cursor environment — no` mcp.json` editing required:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


After` blink login` , your browser opens to authenticate, your API key is saved, and the MCP is connected. Your agent can now provision Blink databases, set up auth, deploy your app, and more — all from within the Cursor chat panel. Compare the before and after:


Without Blink Cloud With Blink Cloud


Database Supabase account + config 1 MCP call


Auth Auth0 or Clerk setup 1 MCP call


Deploy Vercel or Railway config 1 MCP call


Infrastructure steps 6-8 separate services, 3+ hours 2 commands, minutes


` mcp.json` edits Manual JSON editing None — CLI handles it


You can also install Blink directly from the Cursor Marketplace in one click. For a full setup walkthrough, see the[Cursor MCP + Blink Cloud setup guide](https://blink.new/blog/cursor-mcp-blink-cloud-setup) .


## Build Cursor Rules Into Your Workflow With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up Cursor rules for my Next.js project, provision a Blink database and auth, and deploy a working starter app."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Cursor IDE with AI-powered code suggestions — rules control what the agent knows about your project


Blink


## Frequently Asked Questions


` .cursorrules` is the legacy format — a single always-active file in the project root that still works. The newer` .cursor/rules/*.mdc` directory supports multiple files, four activation modes, glob scoping, and YAML frontmatter for fine-grained control. New projects should use` .mdc` files. One important gotcha: plain` .md` files in` .cursor/rules` are silently ignored — the` .mdc` extension is required.


Cursor rules (` .cursor/rules/*.mdc` ) are embedded directly into the Cursor IDE context window — they fire automatically during chat sessions.` CLAUDE.md` is for terminal-based Claude Code sessions and is read by the Claude Code CLI when it starts. The two formats serve the same purpose in different environments: Cursor IDE vs. the terminal agent. Many teams maintain both in the same repo. For a detailed breakdown of what to put in each, see our[CLAUDE.md best practices guide](https://blink.new/blog/claude-md-best-practices) .


Quality over quantity. Most projects run well with 5-8 rules covering: always-on conventions (naming, TypeScript config), file-scoped patterns (React components, API routes), and manual workflow helpers. The constraint is tokens:` alwaysApply: true` rules run on every message, so keep them under 20 lines each. File-scoped glob rules only fire when matching files are in context — you can afford to be more thorough there. Cursor recommends keeping each rule file under 500 lines and splitting large concerns across multiple composable files.


No. Cursor rules are specific to the Cursor IDE context injection system and only apply inside Cursor chat sessions. Claude Code reads` CLAUDE.md` and` AGENTS.md` files instead. If you use both tools on the same codebase, maintain both:` .cursor/rules/` for Cursor and a` CLAUDE.md` in the project root for Claude Code. The content is often similar but the format differs. See our[agentic coding best practices guide](https://blink.new/blog/agentic-coding-best-practices) for how to structure both effectively.


Yes to both. Cursor supports remote rule import via GitHub: Cursor Settings → Rules, Commands →` + Add Rule` → Remote Rule (GitHub). Paste the repo URL and Cursor scans for all` .mdc` files and imports them to` .cursor/rules/imported/<repoName>` . For organizations on Team or Enterprise plans, Team Rules let admins create and enforce rules across the entire org from the Cursor dashboard — members get the rules automatically without any per-developer setup. Rules are version-controlled in your repo, so the whole team sees updates the moment someone pushes.


Write a rule when the pattern is repeatable and predictable — the signal is correcting the AI for the same mistake three or more times. One-off mistakes (misunderstanding a specific function, wrong variable name in a particular context) are better handled inline. Cursor's guidance: "Start simple. Add rules only when you notice Agent making the same mistake repeatedly." Over-engineered rule sets become maintenance overhead; focused ones stay useful for months. For how this same discipline applies to the broader agentic coding workflow, see our[what is Claude Code](https://blink.new/blog/what-is-claude-code) primer.
