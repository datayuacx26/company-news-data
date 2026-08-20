---
schema_version: "1.0.0"
document_id: "54e0992e1388afc896c32f1dc9e482ec9437d1b14925724502e4998bc831b48d"
company_key: "yc-notte"
company: "Notte"
source_id: "yc-notte-news-import-7238aba58520"
canonical_url: "https://www.notte.cc/blog/claude-managed-agents-notte-browsers"
published_at: "2026-08-04T09:00:00+00:00"
first_seen_at: "2026-08-18T16:01:00.928611+00:00"
fetched_at: "2026-08-18T16:01:03.186610+00:00"
content_hash: "sha256:75f1a03af72cf05888ef84e982426d82cb42db27a02332d3231e94b979be9776"
---

# Claude Managed Agents x Notte: What the Sandbox Can't Reach

Claude Managed Agents can now drive a real browser. Point one at a Notte session and it can navigate, fill, and read the live web, on a schedule, without you hosting any of it.


## The browser Claude doesn't have


[Claude Managed Agents](https://platform.claude.com/docs/en/managed-agents/overview) is Anthropic's hosted runtime: they provision the container, the tool harness, and the agent loop, and you supply an environment, a prompt, and a session. The sandbox it ships with has bash and file tools, so an agent can install packages and call APIs.


That covers anything with an endpoint and a key, which is less of the web than it feels like from inside an IDE. Does the vendor portal still show invoices where the runbook says? What does our signup look like to someone not already logged in? Those only answer by loading the page. And a sandbox is a filesystem you own, deterministic and yours to reset, while a browser is an interface to systems you do not control, where failure is ambiguous by default. An assumption made wrongly in step three is still load bearing in step thirty.


The integration runs on Anthropic's credential vaults. Register` NOTTE_API_KEY` as an environment variable credential scoped to` api.notte.cc` , install the` notte` CLI into the environment, and the agent runs` notte sessions start` and drives` goto` ,` observe` , and` scrape` like any other shell tool. The sandbox only ever holds a placeholder. The real key is spliced in at the network boundary, so it never enters the model's context.


> "The agent never sees your key because the sandbox only holds a placeholder. The real key is attached at the network boundary, and only on requests to domains you allow, so it only goes where you've approved."
>
>
> Anthropic,[New in Claude Managed Agents](https://claude.com/blog/whats-new-in-claude-managed-agents)


Sessions come with stealth mode, CAPTCHA solving, proxy routing by country, and profiles that persist browser state between runs.


## What you can build


Launch a swarm of browsers in parallel, one page per subagent, and merge what comes back into a single report. We pointed one at our own site and it came back with a glossary of 56 terms and no search, a sign-in page with no route to sign up, and compliance links on the trust center that resolve to empty strings. None of that was on a roadmap.


Research a lead with a dedicated subagent per angle: pricing tiers, how usage is metered, named customers, what the open roles say about the stack.


Run a nightly agent that logs into a vendor portal on a customer's behalf and pushes what it finds to your system of record. Managed Agents brings the schedule. Vaults hold the credentials you own, Managed Auth holds the connection to a third-party service and re-authenticates it when it goes stale, and personas cover the case where no account exists yet, holding an inbox and a phone number on the same object so a signup that sends a text does not end the run.


## When the model leaves the loop


The runs worth keeping should stop costing tokens. Once a flow works,` notte functions create` freezes it into a serverless endpoint you can call over HTTP, hand a webhook, or put on a cron. The agent works the flow out once, interactively, and what it learned becomes a deployment with no model in the path the next morning. Managed Agents is where that gets discovered. A Function is where it goes to live.


## Where Notte actually fits


Most of what a cloud browser does is table stakes across the category. What we will say plainly is speed.[Browser Arena](https://browserarena.ai/) is our own benchmark, open source and reproducible, running every major provider through the same test on the same machines: create a session, connect over CDP, navigate, release. Notte connects in 353ms, the fastest of the seven on the board, at $0.05 an hour. We publish the harness because a benchmark nobody can rerun is just a claim, and publishing it means the bad days are visible too.


When an agent opens a browser on almost every turn, session start stops being a detail and becomes the loop.
