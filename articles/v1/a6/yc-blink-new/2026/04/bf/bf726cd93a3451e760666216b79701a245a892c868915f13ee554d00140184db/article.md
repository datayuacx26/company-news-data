---
schema_version: "1.0.0"
document_id: "bf726cd93a3451e760666216b79701a245a892c868915f13ee554d00140184db"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-md-context-engineering"
published_at: "2026-04-23T12:55:52+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:52:14.556861+00:00"
content_hash: "sha256:fef0b3aa97e21099fb497ef25380720b0a7a343048c7f52aa4377d8bd7f3fb85"
---

# CLAUDE.md Best Practices: The 10-Section Template That Makes Claude Code 10x Better

## Section 1: Project Overview (2-4 sentences)


What is this project? Who uses it? What's the main thing it does?


```text
## Project Overview
MyApp is a B2B SaaS tool for restaurant managers. It tracks inventory, generates purchase orders, and forecasts needs based on historical usage. About 200 paying restaurants use it daily. The codebase has been live since 2024.
```


This tells Claude Code: this is a production system with real users, not a toy project. It will be more conservative about changes.


## Section 2: Tech Stack (list format)


```text
## Tech Stack
-   Runtime: Node.js 20, TypeScript (strict mode)
-   Framework: Express 4
-   Database: PostgreSQL 15 via Prisma ORM
-   Auth: Clerk
-   Testing: Jest + Supertest
-   Package manager: npm (not yarn, not pnpm)
-   Deployment: Railway
-   Key libraries: zod (validation), dayjs (dates), winston (logging)
```


Explicit stack listing prevents the agent from suggesting React when you use Vue, or recommending a library you've already rejected.


## Section 3: Architecture Rules


This is the highest-impact section. Document the decisions you've already made so the agent respects them.


```text
## Architecture Rules
-   ALL database queries go in /services — never in /routes, /middleware, or /utils
-   ALL input validation goes in /middleware/validate.ts before routes run
-   /routes should contain ONLY: auth check, input forwarding, service call, response formatting
-   /utils contains ONLY stateless helper functions (no DB, no external APIs)
-   Error handling: throw from services, catch in routes, never swallow errors silently
-   Never use direct SQL strings — always use Prisma ORM methods
-   Database transactions: use prisma.$transaction() for multi-step writes
```


Without this, Claude Code will "helpfully" put a database query in a route because it's faster to write. With this, it knows that's a violation.


## Section 4: Coding Standards


```text
## Coding Standards
-   All public functions: JSDoc comments with @param, @returns, @throws
-   Error messages: user-facing only (no stack traces, no internal IDs in API responses)
-   Async: always async/await, never .then() chaining
-   Imports: absolute imports from src/ root, not relative (import { User } from 'models/user')
-   No console.log: use winston logger (logger.info, logger.warn, logger.error)
-   ESLint + Prettier auto-run on file save (configured in .eslintrc and .prettierrc — don't change these)
-   All new API endpoints must have rate limiting middleware
```


## Section 5: Testing Requirements


```text
## Testing Requirements
-   New service functions: unit tests required before PR
-   New API endpoints: integration tests required before PR
-   Test file location: /tests/unit/ for units, /tests/integration/ for routes
-   Test file naming: [  filename  ].test.ts matching source file name
-   Run tests:   `npm test`
-   Coverage minimum: 80% on new code (CI enforced)
-   Do NOT run   `npm run test:integration`   without a local DB running
```


This prevents the agent from writing tests in the wrong place, with the wrong naming, or from running a test command that would fail without a database.


## Section 6: Build and Run Commands


```text
## Build and Run Commands
-   Dev server:   `npm run dev`   (port 3000, hot reload)
-   Build:   `npm run build`   — ⚠️ DO NOT run this; it takes 4 minutes and breaks the dev server. Only CI runs builds.
-   Test:   `npm test`
-   Database migrations:   `npm run db:migrate`   (runs Prisma migrations)
-   Seed database:   `npm run db:seed`   (local only, never production)
-   Lint:   `npm run lint`
-   Type check:   `npm run typecheck`
```


The warning on` npm run build` is there because agents will run build commands without being asked if they're not told not to. This prevents a common annoying workflow interruption.


The 'What NOT to Do' section is the most protective part of CLAUDE.md — document your anti-patterns explicitly


Blink


## Section 7: External Services


```text
## External Services
-   Stripe: used for subscription billing. Webhooks in /routes/stripe-webhook.ts. Never modify this without testing in Stripe CLI first.
-   SendGrid: for transactional emails. Templates in /emails/. Don't hard-code email addresses.
-   Cloudflare R2: object storage for receipts. Helper at /lib/storage.ts. Use this helper, never call the S3 SDK directly.
-   Redis: session caching and rate limiting. Connection in /lib/redis.ts.


Environment variables: see .env.example. Local dev uses .env.local. Production uses Railway env vars.
```


## Section 8: What NOT to Do


Document the anti-patterns you've already fought and fixed. This is the most underutilized section.


```text
## What NOT to Do


-   Do NOT use setTimeout for retry logic — use the   `retryWithBackoff`   helper in /utils/retry.ts
-   Do NOT add console.log for debugging — use the logger and remove debug logs before committing
-   Do NOT use Prisma's   `raw`   queries — use the ORM. The one exception is /scripts/analytics.ts (documented there)
-   Do NOT modify /middleware/errorHandler.ts without reading error-handling.md in /docs first
-   Do NOT create new Stripe Price IDs in code — update via Stripe dashboard, reference by env var
-   Do NOT store user PII in logs (emails, phone numbers, payment data)
-   Do NOT run   `git push --force`   on main
```


Every item in this list was learned the hard way by your team. Document it so the agent (and new team members) don't make the same mistake.


## Section 9: Context Shortcuts


Define project-specific terms the agent should know:


```text
## Context Shortcuts
-   "the client" = Acme Corp (our biggest enterprise customer)
-   "the new flow" = the redesigned onboarding flow (in /features/onboarding-v2)
-   "legacy auth" = the old JWT-based auth in /auth-legacy (being phased out, do not add to it)
-   "the metrics endpoint" = GET /api/internal/metrics (internal only, not exposed externally)
-   "the dashboard" = /pages/dashboard.tsx (user-facing, high traffic, performance-sensitive)
```


This prevents "I need to fix the dashboard" from being ambiguous.


## Section 10: Ongoing Work


```text
## Ongoing Work (as of April 2026)
-   Active refactor: moving from JWT auth to Clerk (see MIGRATION.md for details)
-   Old auth in /auth-legacy — do not add new endpoints to this
-   New auth in /middleware/clerk.ts — use this for all new endpoints
-   Known issue: the inventory forecast job sometimes times out on large datasets (>10k items). This is tracked in issue #1247. Don't try to fix it unless specifically asked.
-   Upcoming: Stripe API v3 migration Q3 2026 — don't write new Stripe code against v2 API
```


This prevents the agent from "helpfully" extending the old auth system you're migrating away from.


## CLAUDE.md Maintenance


A CLAUDE.md file that's never updated loses its value fast.


**Update it when:**


- You add a new external service
- You establish a new architectural pattern
- The agent makes a mistake you don't want repeated
- A team member asks "why do we do X?" (that question = undocumented pattern)


The best practice: at the end of any Claude Code session where the agent needed repeated correction, add the correction pattern to CLAUDE.md's "What NOT to Do" section. That instruction won't need to be given again.


## Build Your Project With Claude Code and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask Claude Code with a good CLAUDE.md in place:


> "Build me a full-stack app following the architecture in CLAUDE.md and host it on Blink."


Blink Cloud handles database, auth, backend, and hosting automatically.[Learn more about Blink Cloud →](https://blink.new/cloud)


Long enough to prevent repeated mistakes, short enough to scan in 2 minutes. A typical CLAUDE.md for a mature project is 200-400 lines. Below 50 lines, you're probably missing something critical. Above 500 lines, the agent may not absorb all of it effectively. The 10 sections above produce a file in the 200-350 line range for most projects.


Most of it should be committed. The exception: personal preferences that only apply to you (your local setup, paths specific to your machine). If you have personal preferences that differ from team standards, put them in CLAUDE.md.local and add that file to .gitignore. The main CLAUDE.md stays in version control and benefits the whole team.


CLAUDE.md is the Claude Code-specific instruction file — it's read by Anthropic's Claude Code at session start. AGENTS.md is the multi-agent instruction pattern used by OpenAI, Cursor, and other tools that support it. The structure is similar; the audience differs. If you use both Claude Code and other agents on the same project, maintain both files. Blink's own codebase uses AGENTS.md for Cursor agents and a separate CLAUDE.md for Claude Code sessions.


Yes. The Claude Code VS Code extension reads CLAUDE.md just like the CLI does. Both surfaces respect the same instruction file. You don't need separate CLAUDE.md files for different surfaces — one file, all surfaces.


Prompt engineering is writing better individual prompts. Context engineering is structuring the persistent context (CLAUDE.md, project structure, naming conventions) so every prompt starts with better inputs. Context engineering produces more consistent results because the improvement is systemic, not per-session. Anthropic's 2026 report calls it the most important shift for developers using AI — the difference between an agent that knows your project and one you have to re-educate every session.
