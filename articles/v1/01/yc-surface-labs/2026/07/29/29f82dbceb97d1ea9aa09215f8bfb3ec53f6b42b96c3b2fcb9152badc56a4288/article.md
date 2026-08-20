---
schema_version: "1.0.0"
document_id: "29f82dbceb97d1ea9aa09215f8bfb3ec53f6b42b96c3b2fcb9152badc56a4288"
company_key: "yc-surface-labs"
company: "Surface Labs"
source_id: "yc-surface-labs-news-import-ffea4c1e1d4e"
canonical_url: "https://withsurface.com/blog/why-we-moved-our-docs-off-mintlify"
published_at: "2026-07-25T11:52:18.919+00:00"
first_seen_at: "2026-07-25T16:43:49.879087+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:a3bd76bc2a6f8c7c8d67a2ade2f9dda0be2e3b013f8a74773f7e5d7535d19f8e"
---

# Why We Moved Our Docs Off Mintlify

Last night I (Meer Tarbani - Growth & Engineering @ Surface Labs) rebuilt our[documentation site](https://docs.withsurface.com/) from scratch and shipped it before I went to bed.


Two hours. One person. 108 pages.


---


## Nobody could see a change before it went live


Including our CEO.


Every other product we ship gets a preview link on every pull request: click it, look at it, approve it. Our docs didn't. You edited, you shipped, you found out. When our CEO wanted to check a page before customers saw it, there was no way to show him.


That's not a missing nice-to-have. That's shipping blind on the page people land on when they're already confused.


Preview deployments are a Pro feature. **$540/month.**


## The docs lived somewhere else


Different repo, different deploy, different review.


So every API change was two jobs, and the second one was always the one that slipped. Docs don't go stale because people are lazy. They go stale because updating them is a separate errand.


## The readers changed and the pricing didn't


Our docs get read by AI agents as much as by people now.


Every agent-facing improvement pushed us further up a credit meter — **10,000 credits** a month, then a penny each. We were being metered on exactly the direction we most wanted to grow.


---


## Just pay for Pro?


We almost did. Here's the math we were looking at:


Plan Price


Starter $0/mo


**Pro** **$540/mo** ($450 billed annually)


Enterprise Contact us


Starter is genuinely good — custom domain, editor, API playground, a real product. The trap isn't the free tier. It's that the first thing you want after the free tier is a preview link, and that's the $540 door. Sixty-five hundred dollars a year, for a docs site whose content we were already writing ourselves.


But the price wasn't what stopped us. This was: **none of the things we wanted are hard.**


A preview link isn't hard. A machine-readable version of a page isn't hard. These aren't feats of engineering being generously amortized across customers — they're switches, and someone had put them behind a tier. Paying **$6,500** a year to have a switch flipped is a worse feeling than paying **$6,500** a year for something difficult.


## Rewrites are supposed to be a bad idea


They are. So we didn't do one.


The move was to[Fumadocs](https://www.fumadocs.dev/) — open source, and built on the exact stack the rest of our product already runs on. That's the whole reason this took an evening instead of a quarter. It wasn't a new system to learn. It was one more app next to the ones we already ship.


And we didn't rewrite a single page. Rather than edit 108 documents to fit a new framework, I taught the new framework to speak the old one's language. The content moved untouched. The URLs stayed identical.


What we got back, for nothing:


- **Preview deploys on every change** — the thing we started this for
- **Docs in the same repo as the code** , so a change and its documentation land in the same review
- **Search** , self-hosted
- **Machine-readable versions of every page** for AI agents — the metered feature
- **Twenty minutes later** , structured data and SEO metadata, because now it's just an app and we can


The link checker I wrote along the way immediately found two pages that had been quietly 404ing in production.


We had been paying someone to host those.


---


## "But your non-technical people can't edit git"


This was the strongest argument for a hosted platform. It expired.


The person who wants a docs change now describes it in plain English to a coding agent, and the agent writes it and opens the pull request. Nobody has to learn git. They have to know what they want the page to say, which they always did.


The visual editor was a moat around a problem that agents drained.


## What we actually gave up


**We own it now.** Uptime, upgrades, the broken build at 2am. Ours.


**We rebuilt things that already worked.** Each one was small. Small times ten is not small.


**Mintlify's API playground is still better than ours.** We didn't need it enough to pay for everything around it.


---


## The switches are cheap now


A year ago, this migration is a two-week project, which means it's a meeting, which means it doesn't happen and we pay the $540.


That's the part worth generalizing. The build cost of leaving a platform has collapsed, and a lot of SaaS pricing is still quoted against the old number.


So it's worth asking, about whatever you're paying for: how much of this is genuinely hard, and how much of it is a switch someone put behind a tier?


---


*Pricing read from[mintlify.com/pricing](https://www.mintlify.com/pricing) , July 25, 2026.*
