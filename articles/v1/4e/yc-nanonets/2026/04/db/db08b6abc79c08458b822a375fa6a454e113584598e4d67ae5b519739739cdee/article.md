---
schema_version: "1.0.0"
document_id: "db08b6abc79c08458b822a375fa6a454e113584598e4d67ae5b519739739cdee"
company_key: "yc-nanonets"
company: "NanoNets"
source_id: "yc-nanonets-rss-fcb6878f0099"
canonical_url: "https://nanonets.com/blog/vibe-coding-best-practices-claude-code/"
published_at: "2026-04-16T11:50:46+00:00"
first_seen_at: "2026-07-20T23:24:03.815131+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:c22f357b9913932255d7a67ceb1d73d875b10e2bdec50a50c9e5e5da8d0e83d3"
---

# Vibe Coding Best Practices: 5 Claude Code Habits for Better Agentic Coding

Vibe coding went from Andrej Karpathy's tweet to Collins Dictionary's Word of the Year in under twelve months. In Y Combinator's Winter 2025 batch, 25% of startups had codebases that were 95% or more AI-generated. GitHub has reported that Copilot was responsible for an average of 46% of code being written across programming languages, and 61% in Java.


So yes, it has become the new normal and everyone's doing it but unfortunately, most people are doing it badly. The tools like Claude Code and Cursor are amazing but most vibe coders use them like autocomplete on steroids, like a genie: just prompt randomly and wait for it to cook. But trust me the output looks crazy at first glance until the codebase is a mess the agent itself can't navigate, lol.So in this guide, we cover 5 things which can make you as good as a developer who went to school for this. Maybe better.


---


## **1. Use CLAUDE.md and Rules as Persistent Context**


Every Claude Code or Cursor session starts with the agent having seen nothing about your project before. It reads whatever files you point it at, infers what it can, and guesses the rest. For small isolated tasks that is fine but for anything heavy it is not, because those guesses keep compounding.


Let’s say you are three weeks into building a SaaS billing system. You open a new session and ask the agent to add a usage based pricing tier. It does not know you already have a BillingService class in /services/billing.py. It does not know you standardized on Stripe's price_id format for all pricing objects. So it creates a new PricingService, picks its own format, and builds something parallel to your existing architecture. Four sessions later you have two billing systems and neither is complete.


A CLAUDE.md file at the root of your project gets read at the start of every session. Here is what a real one looks like for a SaaS project:


```text
# Project: Acme SaaS


## Stack
- Node.js + Express backend
- PostgreSQL with Prisma ORM
- React + TypeScript frontend
- Stripe for billing (price IDs follow format: price_[plan]_[interval])


## Key services
- /services/billing.py — all Stripe logic lives here, do not create parallel billing code
- /services/auth.py — JWT + refresh token pattern, see existing implementation before touching auth
- /lib/db.ts — single Prisma client instance, import from here


## Conventions
- All API responses: { data, error, meta } shape
- Errors always use AppError class, never plain Error
- Every DB query needs explicit field selection, no select *


## Do not touch
- /legacy/payments/ — deprecated, being removed in Q3
- /auth/oauth.py — frozen until SSO ships
```


Cursor now documents Rules and AGENTS.md for persistent instructions. GitHub Copilot supports repository-wide instruction files like .github/copilot-instructions.md, and some Copilot agent surfaces also read AGENTS.md, CLAUDE.md, and GEMINI.md.


When you add a new service or establish a new convention, update the file immediately. It becomes the agent's memory between sessions.


One more thing: context rot is real. A[2025 Chroma study](https://trychroma.com/interview-series/context-rot) of 18 models found measurable accuracy drops as conversations grew longer, even on simple tasks. A 40-message session covering three features is slower and less accurate than three separate 15-message sessions. Open a new conversation for each distinct task. Pin only the files relevant to that task.


### Build your own zero-code agent for free


[Try here](https://agents.nanonets.com/login)


---


## **2. Make the Agent Plan Before It Builds**


The default behavior of every agentic tool is to start writing code the moment you describe something. For a self-contained task like "add a field to this form" that is fine but for anything with real scope it will create problems you do not find until you are deep into the implementation.


Here is a concrete example. You are building a team invitation system: a user enters an email, the system sends an invite, the recipient clicks a link, creates an account, and gets added to the team. Sounds simple but that feature touches your users table, your teams table, a new invitations table, your email service, your auth flow, and your JWT generation. If the agent misunderstands how your auth flow works and builds the invitation acceptance logic against a different assumption, you will not find out until the feature is mostly done.


Before any feature with scope, send this first:


```text
Before writing any code: analyze the codebase, then give me a step-by-step plan
for building the team invitation system. List every file you will modify, every
file you will create, every DB migration needed, and any assumptions you are
making about the existing code. Do not write code yet.


```


A good plan output looks like this:


```text
Files to modify:
- /routes/teams.ts — add POST /teams/:id/invite and POST /teams/accept-invite
- /services/email.ts — add sendTeamInvite() using existing Resend client
- /prisma/schema.prisma — add Invitation model


Files to create:
- /services/invitations.ts — token generation, validation, expiry logic


DB migration:
- invitations table: id, team_id, email, token (unique), expires_at, accepted_at


Assumptions:
- Invite tokens expire after 48 hours
- Inviting an already-registered email still goes through the invite flow
- No invite limit per team currently
```


Read that a couple of times and make sure: Is the 48-hour expiry right? Did it miss the rate limiting you need? Is it using the email service correctly? Fix the plan before a single line of code gets written.


The other side of this is prompt specificity. The more precisely you describe what you want, the less the agent has to infer.


Vague Specific


"Add payments" Integrate Stripe Checkout for the Pro plan ($29/month). On success, set user.plan = 'pro' and user.stripe_customer_id. On cancellation redirect to /pricing. Use existing BillingService in /services/billing.ts.


"Build an API" REST endpoint POST /api/reports. Accepts { start_date, end_date, metric } in request body. Validates dates with Zod. Queries the events table grouped by day. Returns { data: \[{ date, count }\], total }.


"Fix the slow query" The GET /api/users endpoint takes 4 seconds. The users table has 800k rows. Add a database index on created_at and rewrite the query to use pagination (limit 50, cursor-based). Do not change the response shape.


##
**3. Use a Separate Review Agent for Security and Logic**


Coding agents are optimized to complete tasks, not to understand why every guardrail exists. Columbia DAPLab has documented recurring failure patterns across major coding agents, including security issues, data management mistakes, and weak codebase awareness. That makes blind trust dangerous: the same agent that fixes a bug can also remove the check that was preventing a worse one.


The clearest real example of this: in the Replit agent incident of 2025, the autonomous agent deleted a project's primary production database because it decided the database needed cleanup. It was following its optimization objective. It was also violating an explicit instruction not to modify production data. And sadly, no human reviewed what it was about to do.


The agent that wrote your code is not in a good position to catch its own mistakes. Claude Code supports subagents: separate agents that run in completely isolated contexts with no memory of what the first agent built. You define them in .claude/agents/:


```text
---
name: security-reviewer
description: Reviews code for security issues after implementation is complete
tools: Read, Grep, Glob
model: opus
---


You are a senior security engineer doing a pre-ship review.


For every route added or modified, check:
- Is authentication enforced? Can an unauthenticated request reach this?
- Is the user authorized? Can user A access user B's data?
- Is input validated before it hits the database?
- Are there any hardcoded secrets, API keys, or credentials?


Report: file name, line number, specific issue, suggested fix.
Do not summarize. Report every issue you find.


```


After your main agent finishes building the invitation system:


```text
Use the security-reviewer subagent on all the files we just created or modified.


```


Here is what a real reviewer output looks like:


```text
/routes/teams.ts line 47
Issue: POST /teams/accept-invite does not verify the token belongs to the
email address of the logged-in user. Any authenticated user who knows a valid
token can accept any invite.
Fix: Add check that invitation.email === req.user.email before accepting.


/services/invitations.ts line 23
Issue: Token generated with Math.random() — not cryptographically secure.
Fix: Replace with crypto.randomBytes(32).toString('hex').


```


Neither of those would have been caught by the building agent. Both would have made it to prod.


[Escape.tech's scan](https://escape.tech/) of 5,600 vibe-coded apps found over 400 exposed secrets and 175 instances of PII exposed through endpoints. Most of it is exactly this category of issue, authorization logic that works functionally but has holes.


### Build your own zero-code agent for free


[Try here](https://agents.nanonets.com/login)


---


## **4. Prompt in Layers, Not in One Giant Spec**


**Role assignment changes what the agent prioritizes.** "Build this feature" and "Act as a senior engineer who has been burned by poorly tested payment code before. Build this feature." produce different outputs. The second one will add edge case handling, write more defensive validation, and flag assumptions it is not sure about. The model responds to framing.


**Build features in layers, not all at once.** The standard mistake when building something like a Stripe integration is to ask for the whole thing in one prompt. You get code that compiles but has the billing logic, webhook handling, and database updates tangled together. Instead:


Prompt 1:


```text
Set up the Stripe Checkout session creation only.
Endpoint: POST /api/subscribe
Accepts: { price_id, user_id }
Returns: { checkout_url }
Do not handle webhooks yet. Do not update the database yet. Just the session creation.


```


Review that. Make sure the Stripe client is initialized correctly, the right price_id is being passed, the success and cancel URLs point to the right places.


Prompt 2:


```text
Now add the Stripe webhook handler.
Endpoint: POST /api/webhooks/stripe
Handle these events only: checkout.session.completed, customer.subscription.deleted
On checkout.session.completed: set user.plan = 'pro', user.stripe_customer_id = customer id from event
On customer.subscription.deleted: set user.plan = 'free'
Verify the webhook signature using STRIPE_WEBHOOK_SECRET from env.


```


Review that separately, check the signature verification, also that the user lookup is correct.


Each layer is reviewable and has a clear scope. If something is wrong you know exactly where.


**Use pseudo-code when you know the logic but not the implementation:**


```text
Build a rate limiter for the /api/send-invite endpoint.
Logic:
- Key: user_id + current hour (e.g. "user_123_2026041514")
- Limit: 10 invites per hour per user
- On limit exceeded: return 429 with { error: "Rate limit exceeded", retry_after: seconds until next hour }
- Use Redis if available in the project, otherwise in-memory Map is fine


```


This is more accurate than "add rate limiting to the invite endpoint" because you have specified the key structure, the limit, the error response shape, and the storage preference. There is almost nothing left to guess.


---


## **5. Review Diffs, Restrict Access, and Test Immediately**


The majority of developers shipping AI generated code spend moderate to significant time correcting it. Only around 10% ship it close to as is. Those are mostly experienced Claude Code users with tight CLAUDE.md files and structured build sessions.


**Read every diff before committing.** git diff before every commit. When the agent has modified a file you did not ask it to touch, either the prompt left room for interpretation or the agent overreached. Both are worth understanding before the code goes anywhere.


**Restrict what the agent can access.** The permissions.deny block in ~/.claude/settings.json prevents the agent from reading or writing specific paths. A .cursorignore file does the same in Cursor.


```text
{
"permissions": {
"deny": [
"/auth/oauth.py",
"/.env",
"/.env.production",
"/legacy/**",
"/migrations/**"
]
}
}


```


Oh, migrations deserve special mention. An agent that can write its own migration files can silently alter your database schema. Keep migrations out of reach and write them yourself after reviewing what the agent built.


**Test immediately after every feature.** Not as a separate task later, right after. "Now write unit tests for the invitation service we just built. Cover: token expiry, duplicate invite to same email, accept with wrong user, accept with expired token." The agent that just built the feature knows the edge cases. Ask for tests while that context is live.


### Build your own zero-code agent for free


[Try here](https://agents.nanonets.com/login)


---


That's it. Share with whoever needs it. Happy prompting!
