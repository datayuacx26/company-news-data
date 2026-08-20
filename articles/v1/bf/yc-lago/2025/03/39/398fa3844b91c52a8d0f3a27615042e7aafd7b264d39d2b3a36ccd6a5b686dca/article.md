---
schema_version: "1.0.0"
document_id: "398fa3844b91c52a8d0f3a27615042e7aafd7b264d39d2b3a36ccd6a5b686dca"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/billing-shouldnt-suck"
published_at: "2025-03-31T00:00:00+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:875544cdab40c5c64c71f5cd65a9afda4a668133c1cb4c3621fb3d93f91df5e8"
---

# Lago Manifesto: Building billing shouldn't suck (for neither business nor eng)

Getting paid should be the easy part after you’ve done the hard part of building something people want.


It’s not a problem that goes viral on social media, but it’s ubiquitous in tech companies: Pricing and monetization take forever to ship (if you manage to at all). We seem to think that’s just how things are.


Multiple full-time engineers building and maintaining a billing system? That’s just how things are.


Feature releases delayed because it takes a month to translate the new pricing spreadsheet into reality? That’s just how things are.


Need to build everything in-house even though you’re paying for billing software? That’s just how things are.


Missed quarterly results because price adjustments are blocked on 3 edge cases? That’s just how things are.


Spending hours manually issuing invoices and responding to billing questions? That’s just how things are.


An engineer is crying at their desk because they’ve spent 2 days on the logic of converting purchased credits into a usage allowance? That’s just how things are.


You have to pay a couples’ therapist to attend the engineering <> revenue team sync every week? That’s just how things are.


It’s 8pm and you’re fixing the logic for invoice formatting to comply with Guatemalan legislation, daydreaming about a return to simpler times so you can be a medieval blacksmith and get paid in bushels of wheat? That’s just how things are.


**That’s how things are, but they shouldn’t be.**


Not too long ago, racking a basement full of servers and flying in a technician to upgrade from Oracle v2.483 to Oracle v2.484 was “just the way things are”. Billing is still in the basement server era. It’s due for an update more than ever before.[Lago](https://getlago.com/docs/faq/about-lago) is that update—a new type of monetization engine.


## Software monetization is changing


The first era of buying B2B software was simple: You bought a license to the current version of the software and owned it. The second era was cloud-hosted SaaS where you pay a subscription (usually per month and seat), but don’t need to maintain anything.


But subscription fatigue, shrinking head counts and expensive AI APIs are putting seat-based pricing in decline.


**Meanwhile, monetization matters more than ever. User growth is out, revenue is in.**


So companies are adopting usage-based pricing to counteract high AI costs and keep customers from churning from subscription fatigue.


Sometimes that means pay-as-you-go pricing. Other times it means consumption credits. Other times it means a subscription and overages. In some companies, it means some backend engineer called Jerry notices a usage spike and decides to rate-limit you.


Now that monetization is so important, companies are discovering they need a billing system to ship more complex pricing models.


And those billing systems often become monsters they no longer control: Each individual part of billing is simple enough, but the arising system becomes too complex to untangle.


Stripe— *the* fintech company—doesn’t even use its own Stripe Billing product for that reason!


It’s not like we invented billing SaaS. Providers have been around forever. But most of them don’t really solve the problem. Instead, teams often buy a system that can’t accommodate their monetization, so they build computation and logic in-house and only send the final data to their billing provider.


**This degrades the billing system to a PDF generator that sends emails.**


Doesn’t help, does it? Whether you’re building a clunky homegrown billing system you can never replace or buying a system that makes you do all the work, you still have all the billing headaches.


We don’t know this from “market analysis” or user interviews. We know it from being on the founding team of a $5b fintech, and many a sigh, eyerolls and “URGHHHH, WHY WHY WHY DOES BILLING HAVE TO BE SO COMPLICATED????”


The billing team was the biggest internal tools team and the homegrown system was a mess we couldn’t replace. At the time, we thought that’s just how things are. Speaking of internal tools, billing was also a frequent source of tension: Business, finance and leadership teams would wonder why shipping “simple” pricing updates took so long.


But we scratched our heads and wondered why nobody was solving the problem for real. So we did. And solving it *for real* meant building something different than what’s out there.


## We’re open source (because billing needs to be)


Conventional business logic advises keeping your IP secret. But[Lago](https://getlago.com/docs/faq/about-lago) is open source.


That’s because most billing vendors don’t build for the people who actually implement the new billing. They’re built to appeal to revenue teams by making things easy for *them.*


But engineers often hate those systems because advanced billing requires deep integration and customization, which often means workarounds or needing to build around the billing system, not with it. Open source makes it easy to mold[Lago](https://getlago.com/docs/faq/about-lago) into the system you need. not the system you were sold and now need to contort into something that kinda-sorta does what you intended.


Of course, Engineers ultimately need to implement these billing systems, so they deserve a seat at the table. This is a bigger trend in the software industry: Think of PostHog, which built growth tools like analytics with engineers in mind.


And as pricing gets more complex, eng’s seat at the table will become more important because any new pricing needs to be technically feasible and implemented regularly.


Billing systems that don’t serve engineers create delays, frustration and lost revenue.


## We make billing modular (because it gives you flexibility)


There’s often a tension in buying software: The all-in-one system that’s more rigid but does more of the work for you vs. the open system that’s more flexible but requires more input from you.


But billing anything more complex than simple subscriptions always requires customization and deep integration, so the effortless end-to-end system doesn’t exist. That’s why we believe more openness is better.


Being modular gives our customers the best of both worlds: An end-to-end, API-based system that still talks to arbitrary other systems.


Because billing is the backend of pricing and monetization. Like most other backends, it’s a collection of microservices that (hopefully) talk to each other and (hopefully) don’t become so entangled they stop talking to each other.


Events, billable metrics, usage calculation, invoicing, wallets, etc. are all their own API, makes[Lago](https://getlago.com/docs/faq/about-lago) more composable. Our customers can use or not use any individual part of this and have the flexibility they need.


This also makes billing less of an island: Because[Lago](https://getlago.com/docs/faq/about-lago) can receive from and send data to many, many tools, which means users can keep their preferred tools as their source of truth.


## Billing in the future: Say goodbye to the "basement server" era


Billing looked like a solved problem when everything cost $10/mo/user. But the business of software changed while billing didn’t.


AI costs money. Usage varies. Pricing gets complex. You need to move fast. And billing has become critical infrastructure.


We’re building[Lago](https://getlago.com/docs/faq/about-lago) so billing becomes infrastructure you *don’t have to think about* . Like CI/CD. Like logging. Messy, homegrown systems should look like racking a basement full of servers that host an Oracle database.


The next wave of software companies won’t build brittle homegrown billing systems or duct-tape around a vendor that was designed for 2014 SaaS. They’ll build with an open system.


That’s what better billing looks like.
