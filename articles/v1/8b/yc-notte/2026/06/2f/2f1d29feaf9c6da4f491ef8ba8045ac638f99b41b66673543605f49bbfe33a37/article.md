---
schema_version: "1.0.0"
document_id: "2f1d29feaf9c6da4f491ef8ba8045ac638f99b41b66673543605f49bbfe33a37"
company_key: "yc-notte"
company: "Notte"
source_id: "yc-notte-news-import-7238aba58520"
canonical_url: "https://www.notte.cc/blog/bringing-your-tools-into-anything-api"
published_at: "2026-06-04T12:00:00+00:00"
first_seen_at: "2026-07-22T06:23:42.732156+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:011a1d20b4a2696f45054ac258335493fa6fd0785392740a994a9696cae01b97"
---

# Bringing Your Tools into Anything API

[Anything API](https://www.notte.cc/blog/anything-api-turn-any-browser-workflow-into-a-production-ready-api) turns a sentence into a deployed function. Describe what you want, like "summarise my new Stripe charges" or "pull yesterday's signups," and it writes the function, runs it, and can put it on a schedule.


Today they can also write their results straight into the tools you already use.


## What you can do with them


The function already does the web work. Now it can drop the result where you want it, in the same sentence you were already writing:


- "Pull yesterday's signups from the dashboard and post the count to #growth."
- "Scrape a supplier's catalog page and add each product to an Airtable base."
- "Read a competitor's pricing page and drop the current numbers into a Notion database."


Each one is a browser task on one end and a write to your tools on the other, described in a single sentence.


## How you use it


1. **Ask.** Describe the function and where the result should go ("...and post it to Slack").
2. **Connect once.** The first time you mention a provider, you get a one-click connect step inside the chat. Authorise it once and you're done.
3. **Pick the destination.** If the function needs a specific target, like a Slack channel or a Notion database, you pick it from a searchable list.
4. **Ship.** The function deploys with the integration wired in.


## How does auth work


Calling a provider's API is the easy part. The work is in the credentials around it: OAuth flows, token storage, refresh, and rotation. We keep all of that out of the function we generate for you.


When you connect a provider, credentials are injected at runtime via our own proxy. The deployed function code never holds or sees the secret. If a provider isn't connected, you get a clear error telling you to reconnect instead of a silent failure.


## What's next


We'll keep adding providers. Because they all run through the same connection layer, each new one works the same way you already know: connect it once, then name it in a request. Tell us which one you want next.


Try it now:[https://anything.notte.cc/](https://anything.notte.cc/)


New to Anything API? Start with[Turn Any Browser Workflow into a Production-Ready API](https://www.notte.cc/blog/anything-api-turn-any-browser-workflow-into-a-production-ready-api) , then read the[Best Practices](https://www.notte.cc/blog/anything-api-best-practices) for writing reliable task descriptions.
