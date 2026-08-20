---
schema_version: "1.0.0"
document_id: "222ecaec1cad6c217ab9afb6f10254a0f591f6600f34d1b6fe7e36fac54860e4"
company_key: "yc-the-prompting-company"
company: "The Prompting Company"
source_id: "yc-the-prompting-company-news-import-6b8e43739c0c"
canonical_url: "https://promptingcompany.com/blog/does-a-coding-agent-buy-a-database-or-build-its-own"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-24T03:57:18.344019+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:23fb0b0e5939cc2dee41f534fc404b863eeb725baccfd7d5e3c826c6e09c5bed"
---

# Does a coding agent buy a database, or build its own?

## Agents build. They buy only when they can't.


The default is do it yourself. Agents wire up local Postgres, SQLite, or browser storage and move on. Across 138 build tasks with a working disk, they picked a database vendor zero times.


Two things break the default. The data must live across devices or users. Or the app deploys somewhere with no disk. That is the whole pattern.


## The deploy platform picks the database


We took one Next.js app with one bug: data disappears after every deploy. Then we changed a single detail. Where the app deploys.


The app deploys on


What the agents picked


Rate


Vercel


Neon and Upstash, its storage partners


5/6


Netlify


Netlify Blobs, its own storage


6/6


Cloudflare Pages


D1 and R2, its own products


5/6


Railway


Railway Postgres, its one-click add-on


5/6


A $5 VPS (has a disk)


No vendor. They built it themselves.


0/6


Four platforms, four different databases, one app. And the picks copy the platform's own setup path into the user's instructions. One README reads: *"In the Railway project, select New → Database → PostgreSQL."* The agent reads the shelf and follows it.


> **The rule**
> Agents don't shop. They buy whatever is on the shelf in front of them.


This also explains Supabase. The app builders of the vibe coding boom, Lovable and Bolt, shipped apps with no server of their own, and both made Supabase the built-in backend. That boom filled the web with Supabase tutorials, and the web is what models train on. Today Supabase is the pick agents remember when no platform is named. But where deploys happen today, other shelves hold other names. Supabase appeared once in 54 platform runs.


## What else 438 runs showed


**Postgres won the engine**


76% of engine picks. NoSQL: zero in 156 runs. But Postgres is free. The paid layer is the vendor, and agents skip it.


**Nobody buys auth**


Clerk and Auth0: zero picks anywhere. Agents write their own login code and keep the user table on the platform's database.


**The sale happens in the README**


Agents skip the purchase and write the shopping list: "for production, sign up for X." 8 of 18 READMEs named a vendor we never mentioned. The notes kept coming even after we stopped asking for READMEs.


**Shortlisted is not picked**


Agents list candidates before picking. PlanetScale made the list in 18% of runs and won 1.9%. Neon won 57% of its lists. The fight is the final call, not awareness.


**Two agents, two doors**


Claude reads the web while deciding: publish today, reach it today. Codex runs on training memory: nothing reaches it until its next training run.


**Necessity erases personality**


When local storage is possible, Codex picks vendors three times more often than Claude. On serverless, where it is impossible, both picked at the same rate: 10 of 18 each.


## If you sell databases


**Own a job agents cannot build**


Cross-device sync is the biggest one today. It is why Supabase still wins the memory pick.


**Get on the shelf**


Deploy platforms decide serverless picks. Neon is the Postgres on both Vercel and Netlify. That position came from a partnership, not a campaign.


**Be the name in the README**


Even when agents build the database themselves, their READMEs tell the user what to buy at launch. That shortlist is free distribution. PlanetScale was named in zero of the 18 we sampled.


**Split the channel budget**


Fresh pages reach Claude at decision time. Only the training corpus reaches Codex. Two channels, two budgets.


## Method and limits


Five experiments, 438 sealed runs, two agents. Fresh sandbox per run, one shot, web access on. Vendor names never appeared in prompts, outside two head-to-head comparisons.


-


A pick means shipped code: an SDK import plus setup. We scan every transcript for hard evidence, then check the claims by hand. 56 of 57 vendor claims confirmed. One judge error found and removed. We also audited for harness bias: a Supabase skill sits in Codex's plugin folder, but no run ever read it.


-


Every rate is a floor. No human completes a signup and nothing deploys. Cells are small. Read the direction, not the decimals.


-


Every prompt is archived. Each figure regenerates from run transcripts, retrievable by run id.
