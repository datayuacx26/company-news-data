---
schema_version: "1.0.0"
document_id: "93d96631d6dc9f2ce76d64ccc2f9cd484a77a155089537693114e4f9c5f89bf9"
company_key: "yc-draftbit"
company: "Draftbit"
source_id: "yc-draftbit-news-import-aa5bd69b40d1"
canonical_url: "https://draftbit.com/whats-new/new-tasks-board-earn-free-credits-more/"
published_at: "2026-04-29T09:30:56.118+00:00"
first_seen_at: "2026-07-24T05:13:30.805139+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:07ff3d8fe4e479dee41f93a72dc24de32317f5d476062a91ec8dbc7229ddaed6"
---

# New Tasks Board, Earn Free Credits + more!

Hey everyone! Two big things we'd been teasing have now shipped to everyone, plus a round of polish across billing, Tasks, Claude Code rendering, and reliability.


**TL;DR:**[Tasks](https://help.draftbit.com/features/tasks/) are now generally available to everyone — track, organize, delegate, and start work from a shared task board inside the Builder. The ability to earn free credits is fully rolled out, with a new[Earn Credits tab](https://next.draftbit.com/earn) for submitting challenges and claiming rewards. GPT-5.5 is the new Codex default, Cloudflare and HighLevel are now MCP integrations, billing upgrades reset cycles cleanly with itemized credit breakdowns, and concurrent-edit saves are more reliable.


## Manage Work Better with Tasks


[Tasks](https://help.draftbit.com/features/tasks) — the project management view we previewed last update — is now available to everyone with editor access on your projects.


You can create tasks, set priorities, assign to teammates or hand-off to an AI agent, and more. Agents can also break down complex work into tasks and keep them updated automatically. Don't know how to describe what you want? Use **Draft with AI** to turn a rough idea into a clean task description before you start. And once you have a few tasks going, you can manage them from a Kanban-style board or a flat list view, see who (or which agent) is on what, and pick up wherever the team left off.


Read more in the[Tasks docs](https://help.draftbit.com/features/tasks) .


## Earn Free AI Usage Credits


[Earn Credits](https://help.draftbit.com/intro/account/credits/) is now rolled out to all users. There's a new **Earn** tab in the dashboard where you can browse challenges, submit your work, and — once a submission is reviewed — claim rewards directly into your org's credit balance.


See the[credits docs](https://help.draftbit.com/intro/account/credits/) for the full breakdown.


## Small But Notable


#### GPT-5.5 is the new Codex default


GPT-5.5 is now selectable as a Codex model and is the default for new Codex threads. If you've been using GPT-5.4 by default, expect a noticeable bump.


#### Cloudflare and HighLevel are now MCP integrations


Two new[MCP integrations](https://help.draftbit.com/features/integrations#mcp-integrations) are available: **Cloudflare** and **HighLevel** . Connect them up and your agent can work with them directly inside the builder.


#### Cleaner Claude Code thread rendering


Claude Code threads now render more like Codex threads: consecutive file reads collapse into a single "Read N files" row, the LS tool renders as "Listed files (path)" instead of a generic MCP call, reasoning sections flatten redundant single-event groups, and paths with spaces render cleanly. Less visual noise, easier to follow what the agent actually did.


#### Smoother billing upgrades


Paid subscription upgrades now start a fresh Stripe billing period immediately, with proration credited against the new term. The upgrade dialog has been redesigned with a clearer breakdown — current vs. new plan pricing, an itemized credit summary (new grant, overuse deduction, removed credits), and an explicit charge breakdown with proration credit and due-now total. The builder topbar's Manage Plan link now opens the internal[billing overview](https://next.draftbit.com/billing/overview) page, and the Stripe portal button there is renamed to **Invoices & Payments** so it's clearer what it does.


#### Better visibility into preview errors


Runtime errors from the web preview are now surfaced in the builder as toasts and reflected in the dev-server state, so you can catch issues without digging through the browser console.


#### More reliable saves


Saves from the builder are more resilient — if a save conflicts with a concurrent update from another collaborator or agent, Draftbit now pulls and retries automatically instead of failing.


#### GitHub export improvements


The[GitHub export](https://help.draftbit.com/features/publishing#github-export) config now accepts **org/repo** slugs (not just full URLs), validates the URL, branch, and token before saving, and adds a **Check Access** button so you can confirm the repo is reachable before you commit. When an export does fail, the actual git/GitHub error is shown instead of a generic "something went wrong."


#### Faster app delete and restart


Deleting and restarting apps is noticeably faster across the board.


#### Minor fixes worth calling out


- Published PWAs now serve the app's own favicon instead of the default Draftbit logo.
- The Save button in the builder top bar shows a tooltip when there are no unsaved changes, so it's clear why it's disabled.
- Also, the Xano proxy error message now mentions realtime resources, making it easier to diagnose configuration issues.
- And we fixed a sandbox error where bundle uploads were sometimes rejected with a payload hash mismatch, which could block the preview from starting.


## Coming Soon


#### Bring your own OpenRouter API key


Soon you'll be able to plug in your own[OpenRouter](https://openrouter.ai/) API key and use it for model calls inside Draftbit. That means access to OpenRouter's full model catalog, billing through your own OpenRouter account, and the freedom to run models that aren't part of our default lineup.


#### Supabase backend, built in


Still on the way: spin up and connect your app to a **Supabase** backend — database, APIs, cloud functions, realtime, and more — directly from inside the builder, with the[Supabase MCP connector](https://help.draftbit.com/features/integrations#supabase) pre-configured for you so your agent is ready to start building your backend the moment the project exists.


---


That's all for now — happy building!
